# STM32F103C8TX Junior Tasks Project

## Overview

This repository contains an STM32F103C8TX microcontroller project with various GPIO and Timer-based programming tasks. The project is designed as a learning platform for embedded systems development using STM32 HAL (Hardware Abstraction Layer) library.

## Hardware Specifications

- **Microcontroller**: STM32F103C8TX (Blue Pill board)
- **Architecture**: ARM Cortex-M3
- **Clock Configuration**: 72MHz (HSE + PLL)
- **GPIO Used**: PA0 (Output)
- **Timer Used**: TIM2 (Timer 2)

## Project Structure

```
├── Alpha.ioc              # STM32CubeMX project configuration
├── Core/
│   ├── Inc/               # Header files
│   │   ├── main.h         # Main header file
│   │   ├── stm32f1xx_hal_conf.h    # HAL configuration
│   │   └── stm32f1xx_it.h          # Interrupt handlers header
│   └── Src/               # Source files
│       ├── main.c         # Main application file (contains tasks)
│       ├── stm32f1xx_it.c # Interrupt handlers implementation
│       ├── stm32f1xx_hal_msp.c     # HAL MSP (hardware-specific) functions
│       └── system_stm32f1xx.c      # System configuration
├── Drivers/               # STM32 HAL and CMSIS drivers
├── Debug/                 # Build output files (.elf, .bin, .hex)
├── STM32F103C8TX_FLASH.ld # Linker script
└── README.md             # This file
```

## Programming Tasks

The main.c file contains several commented programming tasks that demonstrate different approaches to LED blinking and GPIO control:

### Task 1: Basic GPIO Toggle with HAL_Delay (Lines 87-90)
```c
/* TASK 1
   HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_0);
   HAL_Delay(250);
*/
```
**Purpose**: Simple LED blinking using blocking delay
**Implementation**: Toggle PA0 every 250ms using HAL_Delay()
**Note**: This is a blocking approach that stops the entire program execution during delay

### Task 2: Non-blocking GPIO Toggle with HAL_GetTick (Lines 93-99)
```c
/*TASK 2
if((HAL_GetTick() - tick) >= 250){
    HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_0);
    tick = HAL_GetTick();
}
*/
```
**Purpose**: Non-blocking LED blinking using system tick
**Implementation**: Toggle PA0 every 250ms using HAL_GetTick() for timing
**Advantage**: Allows other code to run during the delay period

### Task 3: Timer Interrupt-based GPIO Toggle (Lines 249-257)
```c
/*TASK4
void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
{
  if(htim->Instance == TIM2)
  {
    HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_0);
  }
}
*/
```
**Purpose**: Hardware timer-based LED blinking using interrupts
**Implementation**: Timer 2 generates interrupts every 250ms to toggle PA0
**Advantage**: Most precise timing, completely non-blocking, minimal CPU overhead

## Timer Configuration

### TIM2 Settings (Lines 167-201)
- **Prescaler**: 7200 (72MHz / 7200 = 10kHz)
- **Period**: 2500 (10kHz / 2500 = 4Hz = 250ms)
- **Mode**: Up-counting
- **Interrupt**: Enabled (started in line 81: `HAL_TIM_Base_Start_IT(&htim2)`)

**Calculation**: 
- Timer frequency = System Clock / (Prescaler + 1) = 72MHz / 7200 = 10kHz
- Interrupt period = Timer frequency / (Period + 1) = 10kHz / 2500 = 4Hz = 250ms

## GPIO Configuration

### PA0 Configuration (Lines 220-230)
- **Pin**: PA0
- **Mode**: GPIO_MODE_OUTPUT_PP (Push-Pull Output)
- **Pull**: GPIO_NOPULL (No pull-up/pull-down)
- **Speed**: GPIO_SPEED_FREQ_LOW
- **Initial State**: GPIO_PIN_RESET (Low)

## System Clock Configuration

The system is configured to run at 72MHz using:
- **External Crystal**: HSE (High Speed External)
- **PLL Multiplier**: x9
- **AHB Divider**: 1 (72MHz)
- **APB1 Divider**: 2 (36MHz)
- **APB2 Divider**: 1 (72MHz)

## How to Use

### Prerequisites
- STM32CubeIDE or compatible ARM GCC toolchain
- STM32F103C8TX development board (Blue Pill)
- ST-Link programmer or compatible

### Building and Flashing
1. Open the project in STM32CubeIDE
2. Build the project (Ctrl+B)
3. Connect your ST-Link programmer to the STM32F103C8TX board
4. Flash the firmware using the Debug configuration

### Testing Tasks
To test each task:

1. **Task 1**: Uncomment lines 87-90 in main.c and flash
2. **Task 2**: Uncomment lines 93-99 in main.c and flash
3. **Task 3**: Uncomment lines 249-257 in main.c and flash

**Expected Result**: LED on PA0 will blink every 250ms (4Hz frequency)

## Development Notes

- The project uses STM32 HAL library for hardware abstraction
- Code generation is managed by STM32CubeMX (Alpha.ioc file)
- User code sections are preserved between code regenerations
- The `.gitignore` file excludes build artifacts and IDE-specific files

## File Locations for Tasks

| Task | File Location | Line Numbers | Description |
|------|---------------|--------------|-------------|
| Task 1 | `Core/Src/main.c` | Lines 87-90 | Basic blocking delay method |
| Task 2 | `Core/Src/main.c` | Lines 93-99 | Non-blocking tick-based method |
| Task 3 | `Core/Src/main.c` | Lines 249-257 | Timer interrupt callback method |

## Additional Information

- **Repository**: https://github.com/Z1-N/JuniorTasks
- **Branch**: master
- **Target**: Educational embedded systems programming
- **License**: STMicroelectronics License (see individual files)

This project serves as an excellent introduction to STM32 embedded programming, demonstrating progression from basic GPIO control to advanced timer-based interrupt handling.
