# Seat Heater Control System

This project implements a real-time seat heater control system for the front two seats of a car using **FreeRTOS** and a **Tiva C** microcontroller. The system regulates seat temperature based on user input, provides diagnostics, and displays system data on a shared screen via **UART**.

---

## Features

- **Temperature Control:**
  - Supports three heating levels (**Low, Medium, High**) and an **Off** state.
  - Adjusts heating intensity based on the difference between current and desired temperatures.

- **Diagnostics:**
  - Detects temperature sensor failures and logs them in memory.
  - A **red LED** indicates sensor issues.

- **User Interface:**
  - A shared screen displays the current temperature, heating level, and heater status.

- **Event-based Architecture:**
  - Button presses trigger corresponding heating events.
  - Tasks manage heating levels accordingly.

- **Real-time Measurements:**
  - Monitors CPU load, task execution time, and resource lock time using **FreeRTOS** and the **GPTM** module.

---

## Hardware Components

Each seat (**driver and passenger**) includes:
- A **temperature sensor** (simulated as a potentiometer).
- **LEDs** to simulate heater intensity:
  - **Green** for low
  - **Blue** for medium
  - **Cyan** for high
- A **button** to control the heating level.

Additional components:
- **Red LED** for error reporting.
- **An extra button** on the steering wheel to control the driver's seat heater.

---

## Task Architecture

| Task Name                   | Description                                          | Type        | Periodicity | Events Set/Waited |
|-----------------------------|------------------------------------------------------|-------------|-------------|-------------------|
| **PassengerButtonHandler**  | Handles passenger button press events.              | Periodic    | 100ms       | `PASSENGER_EVENT_BIT_BUTTON_PRESSED_*` |
| **DriverButtonHandler**     | Handles driver button press events.                 | Periodic    | 100ms       | `DRIVER_EVENT_BIT_BUTTON_PRESSED_*` |
| **Temperature_Task**        | Monitors seat temperatures and detects errors.      | Periodic    | 100ms       | None |
| **Heating_level_LOW**       | Manages heating at low level.                       | Event-based | N/A         | `*_EVENT_BIT_BUTTON_PRESSED_LOW` |
| **Heating_level_MEDIUM**    | Manages heating at medium level.                    | Event-based | N/A         | `*_EVENT_BIT_BUTTON_PRESSED_MEDIUM` |
| **Heating_level_HIGH**      | Manages heating at high level.                      | Event-based | N/A         | `*_EVENT_BIT_BUTTON_PRESSED_HIGH` |
| **vRunTimeMeasurementsTask** | Monitors CPU load and outputs via UART.            | Periodic    | 500ms       | None |

---

## Shared Resources

- **UART:**
  - Shared by multiple tasks, access protected by a **mutex** to prevent data corruption.
- **Event Group:**
  - Used to synchronize button press events and heating level tasks.
  - Managed using **FreeRTOS APIs**.
- **Temperature Sensors:**
  - Read by the `Temperature_Task`.
  - Other tasks access the latest readings sequentially.

---

## Project Structure

```
Seat-Heater-Control-System/
│-- src/        # Application tasks, FreeRTOS config, MCAL modules (GPIO, UART, GPTM, ADC)
│-- simso/      # Simso simulation project for deadline analysis
│-- docs/       # Documentation, system output screenshots, runtime measurement results
│-- README.md   # Project description and usage details
```

---

## How to Run

1. Clone this repository:
   ```sh
   git clone https://github.com/ibucz/Seat-Heater-Control-System.git
   ```
2. Open the project in your preferred **IDE** (e.g., **Keil, Code Composer Studio**).
3. Compile and flash the firmware onto the **Tiva C** microcontroller.
4. Connect a UART terminal to monitor system output.

---

## License

This project is licensed under the **MIT License**.

---

## Contributors
- [ahmed abulaziz](https://github.com/ibucz)

Feel free to contribute to this project by submitting issues or pull requests! 🚀

