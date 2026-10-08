# STM32F446RE Timer Interrupt & EXTI Control

This repository contains an embedded firmware project developed for the **STM32F446RET6** microcontroller using **STM32CubeIDE** and the **STM32 HAL (Hardware Abstraction Layer)**.

The project demonstrates hardware timer interrupts using **TIM2** alongside external interrupt lines (**EXTI**) configured on user inputs.

---

##  Features

* **MCU:** STM32F446RET6 (ARM Cortex-M4 @ 16 MHz HSI)
* **IDE & Toolchain:** STM32CubeIDE / GCC
* **Hardware Timer:** TIM2 (configured with Prescaler: 8399, Period: 9)
* **External Interrupts (EXTI):**
  * `PC13`: User push-button (Falling Edge interrupt with Pull-up)
  * `PB12`: External line interrupt (Pull-up)
* **GPIO Outputs:** `PA6`, `PA7`, `PB0` through `PB7`

---

## Pin Configuration

| Pin | Function / Mode | Description |
| :--- | :--- | :--- |
| **PC13** | `GPIO_EXTI13` | Button input (Falling edge interrupt) |
| **PB12** | `GPIO_EXTI12` | External interrupt line |
| **PA6, PA7** | `GPIO_Output` | Digital Output Channels |
| **PB0 – PB7** | `GPIO_Output` | Multi-pin digital outputs |

---

##  Building and Running

1. Clone the repository:
   ```bash
   git clone [https://github.com/](https://github.com/)<your-username>/embedded_hw2.git
