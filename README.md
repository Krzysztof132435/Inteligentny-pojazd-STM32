# STM32 Autonomous Line Follower with IMU Sensor Fusion

An autonomous mobile robot built around the **STM32G491RE (Nucleo-64)** microcontroller, programmed in **C** using the STM32 HAL library. The system combines analog line tracking (**3x TCRT5000**) with inertial measurement (**MPU6050**) to stabilize trajectories, detect inclines, and execute precise maneuvers when the track line is lost.

## Key Embedded & Software Features
* **Non-Blocking Data Acquisition:** Continuous 3-channel analog readout from TCRT5000 IR sensors using `ADC1` configured in **DMA Circular Mode**.
* **Custom I2C IMU Driver:** Lightweight C driver for the MPU6050 (accelerometer & gyroscope) featuring an automatic startup gyro-bias calibration routine.
* **Closed-Loop Control:** Discrete **PID controller** dynamically adjusting PWM duty cycles (`TIM1`) for differential drive steering.
* **Finite State Machine (FSM):** Modular state architecture handling `CALIBRATION`, `FOLLOW_LINE`, `GAP_CROSSING` (gyro-stabilized straight driving when the line breaks), `TURN_AROUND` (180° gyro-controlled pivot), and `EMERGENCY_STOP`.
* **Live Telemetry:** Real-time diagnostic data streaming over UART (`USART2` via ST-LINK Virtual COM Port) for PID tuning and debugging.

---

## Hardware Architecture (BOM)
* **Microcontroller:** STM32 Nucleo-G491RE (ARM Cortex-M4F @ 170 MHz)
* **IMU Sensor:** GY-521 Module (MPU6050 6-axis Accelerometer & Gyroscope)
* **Line Sensors:** 3x TCRT5000 Reflective Optical Sensors (analog output `AO`)
* **Drivetrain:** 2x TT DC Gear Motors (dual-shaft) + 4 wheels (2WD differential drive with front passive wheels)
* **Motor Driver:** L298N Dual H-Bridge
* **Power Supply:** 4x AA Battery Holder (6V) + LM2596 Step-Down Buck Converter (regulated 5V logic rail)

---

## Pinout Configuration (Nucleo-G491RE)

| Module | Module Pin | STM32 Pin | Arduino Header | Peripheral / Function |
| :--- | :--- | :--- | :--- | :--- |
| **TCRT5000 (Left)** | `AO` | `PA0` | `A0` | `ADC1_IN1` (DMA) |
| **TCRT5000 (Center)** | `AO` | `PA1` | `A1` | `ADC1_IN2` (DMA) |
| **TCRT5000 (Right)** | `AO` | `PB0` | `A3` | `ADC1_IN15` (DMA) |
| **MPU6050** | `SCL` / `SDA` | `PB8` / `PB9` | `D15` / `D14` | `I2C1` |
| **L298N (Left Motor)** | `ENA` | `PA8` | `D7` | `TIM1_CH1` (PWM) |
| **L298N (Left Motor)** | `IN1` / `IN2` | `PB10` / `PB4` | `D6` / `D5` | `GPIO_Output` |
| **L298N (Right Motor)** | `ENB` | `PA9` | `D8` | `TIM1_CH2` (PWM) |
| **L298N (Right Motor)** | `IN3` / `IN4` | `PB5` / `PB3` | `D4` / `D3` | `GPIO_Output` |

---

## Project Roadmap
- [x] Component selection and system architecture design
- [ ] 2WD chassis assembly and dual-rail power supply wiring (4x AA + LM2596)
- [ ] STM32CubeIDE peripheral configuration (`TIM1`, `ADC1+DMA`, `I2C1`, `USART2`)
- [ ] Motor driver abstraction layer (`motor_driver.c/.h`)
- [ ] Analog line sensor readout and auto-calibration (`line_sensor.c/.h`)
- [ ] MPU6050 I2C driver and yaw angle integration (`mpu6050.c/.h`)
- [ ] PID controller and FSM integration (`pid.c/.h`, `state_machine.c/.h`)
- [ ] Track testing, PID tuning, and adding demo videos/photos to `Docs/`
