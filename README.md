Cheerson CX10 green firmware
============================

Cheerson CX10(A) ?green? firmware attempt.

This is the fork of https://github.com/samuelpowell/CX10-FNRF (original
description below)

Be aware, that this is (and will most likely always be) work in progress.

HW config of my CX10A
---------------------

- Cherson board with "JG-CX10R-3" from CX10A
- MCU: STM32f031K4
- Gyro: INVENSE MPU 60520 (Is it the MPU 6050?)
- Wifi: XN297


- PA1 LDO_Enable
- PA7 Battery_Voltage
- PA4 LED2
- PB1 LED1

- PA2 Motor_Sense_RightFront
- PA3 Motor_Sense_RightRear
- PA5 Motor_Sense_LeftFront
- PA6 Motor_Sense_LeftRear

- PA8 MOSFET_LeftRear (TIM1_CH1)
- PA9 MOSFET_RightFront (TIM1_CH2)
- PA10 MOSFET_RightRear (TIM1_CH3)
- PA11 MOSFET_LeftFront (TIM1_CH4)

- PA13 Testpin_SWDIO
- PA14 Testpin_SWCLK

- PA15 Wifi_CSN (SPI1)
- PB3 Wifi_SCK
- PB4 Wifi_MISO
- PB5 Wifi_MOSI
- PB8 Wifi_CE
- PA12 Wifi_IRQ

- PA0 Gyro_INT
- PB6 Gyro_SCL (I2C1)
- PB7 Gyro_SDA (I2C1)

- PB0 NC?
- PB2 NC?




cx10_fnrf
=========

Cheerson CX10 (RED) rate mode firmware employing integrated RF controller and stock TX

1. About

  The Cheerson CX10 (RED) is a specific version of a quadcopter incorporating an ARM Cortex M0 MCU, Invensense MPU-6050 and Beken BK2423 (Nordic NRF24 clone). In 2014, Felix Niessen released alternative firmware for the device which allows it to be flown in rate mode using a standard PPM receiver. See the following page for details: http://www.rcgroups.com/forums/showpost.php?p=30045580&postcount=1.
  
  This code builds upon Felix's work by enabling the device to be flown using the onboard radio, using the original controller (or even better, any device implementing the appropriate protocol). The radio driver was mostly derived from the Crazyflie project (see bitcraze.se), and the protocol implementation from the DevtiationTX project (http://www.deviationtx.com).
  
  In making these changes I have attempted to change as little as possible in Felix's original code.
  
2. Building

  Current binaries are provided in the /builds subdirectory, produced from both the Keil MDK-ARM toolchain, and the GNU ARM toolchain. The former is flight-tested, the latter is not.
  
  A Keil uVision v5 project file is included to build/debug using MDK-ARM on Windows platforms (note that the code-size is such that the 32Kb-limited evaluation version may be used). A modification of the original Makefile is also included, which will build the firmware using a suitable GNU ARM toolchain under OS X, and presumably, Linux.
  
3. Programming
  
  To program the firmware will require that you connect to the SWD pads on the CX-10 (RED) PCB, before using an appropriate programming tool. Note that the device is level-1 protected by default, and these option bytes must be cleared before programming. Alternatively, the embedded UART boot-loader may be used to program the device. See the original firmware thread for more details.
  
4. Operation

  + Upon power-up the CX-10 will try to bind to a suitable TX, during this phase the LEDs will light alternately, flashing at a slow rate.
  + When bound to a transmitter, but waiting for valid data, the LEDs will flash rapidly.
  + Once valid data is received, the LEDs on one side of the device will illuminate, this indicates that the device is bound and disarmed
  + Use a 'forward-flip' command on the stock TX to arm (internally setting Aux 1 high). On a Mode 2 transmitter this involves pressing the right stick then pushing full forwards. This must be performed at zero-throttle. All lights will illuminate when armed.
  + Disarm using a 'backwards-flip' command on the stock TX. If multiple RF packets are lost, the device will disarm automatically. 
  + Under low battery conditions, the LEDs will blink.
  
5. Credits

  + Felix Niessen's original work: http://www.rcgroups.com/forums/showpost.php?p=30045580&postcount=1.
  + Bitcraze (for nrf24 driver): http://www.bitcraze.se
  + DeviationTX (for their reverse engineering of the YD717 SkyWalker protocol): http://www.deviationtx.com
  
6. Porting/future development

  + With little modification this code should also run on the Cheerson CX-11, which I believe also uses the Beken BK2423 RF chip.
  + The CX-10 (BLUE/GREEN) PCBs also exist which use the XN297 RF chip and an alternative protocol. This firmware is not compatible with these versions, support could be added if the XN297 is air-compatible with the nrf24/Beken BK2423. Contact me if you have any information.
  
  
  
  
