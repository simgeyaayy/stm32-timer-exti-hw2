# STM32F446RE 2-Digit 7-Segment Multiplexed Hex Counter (EXTI & Timer IT)

This repository contains an embedded C application developed for the **STM32F446RET6** microcontroller using **STM32CubeIDE** and the **STM32 HAL (Hardware Abstraction Layer)**.

The project implements a 2-digit 7-segment display driver using hardware multiplexing driven by **TIM2 periodic interrupts**, and an interactive hexadecimal counter (`0x00` to `0x1F`) controlled via **EXTI push-button interrupts** with software debouncing.

---

## Features

- **Target MCU:** STM32F446RET6 (ARM Cortex-M4 @ 16 MHz HSI)
- **IDE & Toolchain:** STM32CubeIDE / GCC
- **Framework:** STM32F4xx HAL Driver & CMSIS
- **Display Driver (Time-Multiplexed 7-Segment):**
  - Segment Bus (`A` through `DP`): 8-bit output on `PB0` – `PB7`
  - Digit Selection (Common Anode/Cathode enable): `PA6` (Digit 0 - LSB) and `PA7` (Digit 1 - MSB)
  - Refresh Timer: **TIM2** periodic interrupt switching digits seamlessly
- **User Control & Inputs (EXTI):**
  - **Increment (Up):** `PC13` (Active-low, falling-edge external interrupt)
  - **Decrement (Down):** `PB12` (Active-high/rising-edge external interrupt)
  - **Debouncing:** 500 ms non-blocking time-window debounce using `HAL_GetTick()`
- **Counting Range:** Hexadecimal range `0x00` (0) to `0x1F` (31)

---

## Operating Logic

1. **Multiplexed Display Refresh (TIM2 Interrupt):**
   - TIM2 triggers periodic update events (`HAL_TIM_PeriodElapsedCallback`).
   - On alternating ticks, it shifts the active display digit between Digit 0 (`PA6`) and Digit 1 (`PA7`).
   - It decodes the lower nibble (`count & 0x0F`) onto Digit 0 and the upper nibble (`(count >> 4) & 0x0F`) onto Digit 1 using the 16-element hex font array `segFont[]`.

2. **Button Press Handling (EXTI Callback):**
   - Pressing **PC13** triggers an external interrupt on line 13: if more than 500 ms has elapsed since the last trigger, `count` increments (capped at `0x1F`).
   - Triggering **PB12** triggers an external interrupt on line 12: after debounce validation, `count` decrements (floored at `0x00`).

---

## Pinout Configuration

| Peripheral / Signal | Pin | Mode / Configuration | Description |
| :--- | :--- | :--- | :--- |
| **Segment Bus (A–G, DP)** | `PB0` – `PB7` | Output Push-Pull | 8-bit bus driving display segments |
| **Digit 0 Enable (LSB)** | `PA6` | Output Push-Pull | Digit 0 common enable line |
| **Digit 1 Enable (MSB)** | `PA7` | Output Push-Pull | Digit 1 common enable line |
| **Count Up Button** | `PC13` | EXTI Falling Edge, Pull-Up | Increments counter (+1) |
| **Count Down Button** | `PB12` | EXTI Rising Edge, Pull-Up | Decrements counter (-1) |

---

## Building and Flashing

1. Clone the repository:
   ```bash
   git clone [https://github.com/](https://github.com/)<your-username>/embedded_hw2.git
