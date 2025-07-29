# MiniParking - Automated access parking management system
Mini Parking is a prototype of an automated parking management system developed for the Computer System Design course. The project simulates a small-scale parking lot with automated access control, vehicle detection, ID input, and fee calculation based on parking duration. The system is implemented using the **STM32F303VC** microcontroller.

## Objective
To develop a model of a parking system with the following features:
- Automated gate control at entry and exit
- Automatic LED lighting based on motion detection
- Vehicle identification using a keypad
- Real-time display of entry and exit data
- Calculation of parking fees based on time spent

## System Components and Functionality
- **Entry Gate Automation:** A **SG90** servomotor controls the barrier arm. It opens automatically when an approaching vehicle is detected by an **HC-SR04** ultrasonic sensor within a predefined distance.
- **LED Lighting System:** Two LEDs are turned on/off using a **HC-SR501** PIR motion sensor. The LEDs are activated upon motion detection and turn off automatically 6 seconds after no further movement is detected.
- **Vehicle Identification and Entry Logging:** A Membrane Switch Keypad (**DS-16038**) allows users to input a vehicle ID upon entry. This data is stored and shown on a 16x2 LCD display, along with the current date and time obtained via an RTC module (**DS3231**).
- **Exit Process and Fee Calculation:**
  - The same keypad is used at exit to re-enter the vehicle ID.
  - The system retrieves and displays the entry and exit data, along with the total fee based on the parking duration.
  - The user confirms payment by pressing a push button on the board. This action triggers the servomotor to raise the exit barrier.
- **Gate Closure Logic:** After each gate opening (entry or exit), the servomotor automatically closes the barrier. Closure occurs only when the ultrasonic sensor confirms that the area in front of the gate is clear of obstacles for a short time.





