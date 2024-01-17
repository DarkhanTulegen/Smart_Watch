# Watch using STM32
This is a watch equipped with an STM32F411CEU6 microcontroller, utilizing graphics library for the user interface and running on the RTOS operating system.

## Enhanced Features
Several features have been updated:

1. The heart rate section now includes additional hardware such as an LED matrix, improving the strength of the PPG signal.
2. The dual boards have been redesigned to be flush, avoiding the use of 4 layers due to manufacturing constraints.
3. The Bluetooth module has been switched to KT6328A.
4. Changes have been made to the MPU6050 circuit; in V2.2, the AUX was grounded, leading to higher power consumption. Now, standby power consumption is in the hundreds of microamperes.
5. The NFC section has been removed due to the previous design causing NFC to be shielded by PCB copper and the screen iron sheet.
6. New games, including 2048, memory blocks, and MPU6050-related games, have been added.

## Software Design Specifics:

### 1. Energy-Efficient Design

The watch operates in three modes: normal, sleep (MCU in STOP mode, with MPU6050 still counting steps), and shutdown (TPS63020 directly disables, leaving only Vbat powered).

Originally, the watch utilized the motion function of MPU6050 for wake-up, requiring a substantial shaking amplitude to trigger the interrupt. Eventually, an RTC timer interrupt was implemented to periodically check for wrist gestures and initiate wake-up.

MPU6050 cannot directly use the DMP library; adjustments were made post-initialization to reduce power consumption. Refer to the project code for details.

After adopting KT6328A for Bluetooth, it's advisable not to disable Bluetooth, as its standby power consumption is low.

### 2. Heart Rate and Blood Oxygen

Blood oxygen monitoring is yet to be implemented.

Initially, the official library was employed for heart rate calculation, but due to slow performance, a simple peak detection algorithm was introduced. The PPG signal of EM7028 is depicted in the image below.

### 3. Data Storage

Presently, external EEPROM is used for data storage, primarily for storing settings. For detailed information, refer to the `Datasave.c` file.

### 4. Page Switching Logic

Page switching, with the ability to return to the previous page, is implemented using a stack to store page addresses. Caution is advised when pushing dynamic addresses; it is recommended to use the address at the specific moment.

### 5. Calculator Logic

The calculator logic follows a classic approach with two stacks, one for symbols and one for numbers. The process involves popping elements and performing calculations. Decimal point handling is included; consult the code for specifics.