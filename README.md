# Mini Parking - Automated access parking management system
![Banner](https://github.com/user-attachments/assets/5545bc88-2054-41aa-b3de-bf197e49a8e1)
Mini Parking is a prototype of an automated parking management system developed for the Computer System Design course. The project simulates a small-scale parking lot with automated access control, vehicle detection, ID input, and fee calculation based on parking duration. The system is implemented using the **STM32F303VC** microcontroller.


## Objective
To develop a model of a parking system with the following features:
- Automated gate control at entry and exit
- Automatic LED lighting based on motion detection
- Vehicle identification using a keypad
- Real-time display of entry and exit data
- Calculation of parking fees based on time spent

## Hardware Components
- **STM32F303VC** microcontroller  
- **SG90** Servo Motor  
- **HC-SR04** Ultrasonic Sensor  
- **HC-SR501** PIR Motion Sensor  
- Membrane Keypad **DS-16038**  
- LCD 16x2 Display  
- **DS3231** RTC Module  
- Push Button (on-board) 
- LEDs and resistors. They are bundled together using heat-shrink tubing for compact and secure wiring. These resistors are not included in the integration schematic (see `Integration.png`) to keep the diagram clean and focused on the core system logic.


## Functionality
- **Entry Gate Automation:** A **SG90** servomotor controls the barrier arm. It opens automatically when an approaching vehicle is detected by an **HC-SR04** ultrasonic sensor within a predefined distance.
- **LED Lighting System:** Two LEDs are turned on/off using a **HC-SR501** PIR motion sensor. The LEDs are activated upon motion detection and turn off automatically 6 seconds after no further movement is detected.
- **Vehicle Identification and Entry Logging:** A Membrane Switch Keypad (**DS-16038**) allows users to input a vehicle ID upon entry. This data is stored and shown on a 16x2 LCD display, along with the current date and time obtained via an RTC module (**DS3231**).
- **Exit Process and Fee Calculation:**
  - The same keypad is used at exit to re-enter the vehicle ID.
  - The system retrieves and displays the entry and exit data, along with the total fee based on the parking duration.
  - The user confirms payment by pressing a push button on the board. This action triggers the servomotor to raise the exit barrier.
- **Gate Closure Logic:** After each gate opening (entry or exit), the servomotor automatically closes the barrier. Closure occurs only when the ultrasonic sensor confirms that the area in front of the gate is clear of obstacles for a short time.

# Code overview
This section briefly outlines the organization and behavior of the `main.c` file.
- **Constants & Macros:** Defines key system limits such as maximum code length (`MAX_CODE_LENGTH`), log size (`MAX_LOG_ENTRIES`), debounce delay, I²C timeout, and distance threshold for vehicle detection.
- **Data Structures:**
  - A date struct captures RTC-based timestamp components (seconds, minutes, hours, day, month, year).
  - LogEntry holds each vehicle’s code, entry/exit timestamps, date structs, and a usage flag.
- **Peripheral Initialization:** Uses HAL libraries to configure GPIO, I²C, UART and multiple timers (`TIM1`, `TIM2`, `TIM3`, `TIM4`, `TIM15`). Timer-driven interrupts handle PWM signals for the servo and ultrasonic sensor timing.
## Program Flow
1. **LCD Initialization:** Displays project name and initial pricing plan (`$0.10/HR`, might be more expensive than downtown Milan).
2. **Main Loop:** Remains idle; all core activity is interrupt-driven.
## Interrupt & Handler Logic
- **Ultrasonic Sensor** (`TIM4`)**:** Measures echo pulse width to compute distance. If below threshold (e.g., < 20 cm), triggers gate opening (via servo PWM) and starts a closure timer (`TIM15`).
- **PIR Sensor:** Toggles LEDs on motion detection and deactivates them after a fixed interval.
- **Keypad Input:**
  - Scanning 4x4 keypad matrix with debouncing.
  - `'*'` confirms and submits a vehicle ID; `'#'` deletes the last character.
- **RTC via I²C:** Reads real-time clock, converts BCD data to integers, forms `date` structs, and timestamps.
- **Log Management:**
  - **Entry:** If the code is new, a new `LogEntry` is created.
  - **Exit:** If the code exists, the exit timestamp is recorded, parking duration is calculated, and the fee is displayed on both UART and LCD.
- **Payment Confirmation:** A push button on the STM32 board simulates payment: when pressed, it opens the exit gate and starts the timer for automatic gate closure.
## Utility Functions
- `get_current_timestamp()`, `date_to_seconds()`, `date_diff_sec()`, and `calc_parking_fee()` handle conversion of timestamps and fee calculation.
- Leap-year logic ensures accurate date-to-seconds conversion.
## Video demostration
A video demonstration of the system in operation is available in the main repository folder. The file, named `MP.mp4`, includes on-screen text descriptions in Italian to explain the system's functionality and key features.
## Accessing the code
If you are a recruiter or a developer interested in viewing the source code, please open an issue in this repository with your request. I will provide access after reviewing it.



