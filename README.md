EXPERIMENT-05-SOIL-MOISTURE-SENSOR-INTERFACE-TO-IOT-DEVELOPMENT-BOARD
NAME : KARTHICKKUMAR R
REG NO : 212223040087
DATE : 25-09-2025
Aim:
To Interface a Analog Input (soil moisture sensor) to ARM IOT development board and write a program to obtain the data on the com port

Components required:
STM32 CUBE IDE, ARM IOT development board, STM programmer tool,Soil Moisture Sensor.

Theory
Hardware Overview
A typical soil moisture sensor consists of two parts.

The Probe The sensor includes a fork-shaped probe with two exposed conductors that is inserted into the soil or wherever the moisture content is to be measured.

As previously stated, it acts as a variable resistor, with resistance varying according to soil moisture.

image

The Module In addition, the sensor includes an electronic module that connects the probe to the Arduino.

The module generates an output voltage based on the resistance of the probe, which is available at an Analog Output (AO) pin.

The same signal is fed to an LM393 High Precision Comparator, which digitizes it and makes it available at a Digital Output (DO) pin.

image

The module includes a potentiometer for adjusting the sensitivity of the digital output (DO).

You can use it to set a threshold, so that when the soil moisture level exceeds the threshold, the module outputs LOW otherwise HIGH.

This setup is very useful for triggering an action when a certain threshold is reached. For example, if the moisture level in the soil exceeds a certain threshold, you can activate a relay to start watering the plant.

Soil Moisture Sensor Pinout The soil moisture sensor is extremely simple to use and only requires four pins to connect.

soil moisture sensor pinout AO (Analog Output) generates analog output voltage proportional to the soil moisture level, so a higher level results in a higher voltage and a lower level results in a lower voltage.

DO (Digital Output) indicates whether the soil moisture level is within the limit. D0 becomes LOW when the moisture level exceeds the threshold value (as set by the potentiometer), and HIGH otherwise.

VCC supplies power to the sensor. It is recommended that the sensor be powered from 3.3V to 5V. Please keep in mind that the analog output will vary depending on the voltage supplied to the sensor.

GND is the ground pin.

image

Procedure:
click on STM 32 CUBE IDE, the following screen will appear
image

click on FILE, click on new stm 32 project
image image

Select the target to be programmed as shown below and click on next
Screenshot 2025-03-11 134231

Select the program name
image

Corresponding ioc file will be generated automatically
Screenshot 2025-03-11 134528

Select the appropriate pins as GPIO, in or out, USART or required options and configure
Screenshot 2025-03-11 134617 Screenshot 2025-03-11 134642

Click on Ctrl+S, automatically C program will be generated
image

Edit the program and as per required
image

Use project and build all
image

Once the project is build
image

Open STM32Cube Programmer
Screenshot 2025-03-11 135208

Connect the STM board through the COM port, then upload the corresponding project ELF file while ensuring the board is in flash mode, and click on 'Start Program.' After the file download is complete, switch your board to run mode and press the reset button to see the output
13.Open serial port utility

image

Check for execution of the output using run option.
STM 32 CUBE PROGRAM :
#include "main.h"
#include "stdio.h"
#if defined(__GNUC_s_)
#define PUTCHAR_PROTOTYPE int __io_putchar(int ch)
#endif
uint16_t readValue;

ADC_HandleTypeDef hadc;
UART_HandleTypeDef huart2;
void SystemClock_Config(void);
static void MX_GPIO_Init(void);
static void MX_ADC_Init(void);
static void MX_USART2_UART_Init(void);
int main(void)
{

    HAL_Init();
    SystemClock_Config();

    MX_GPIO_Init();
  MX_ADC_Init();
  MX_USART2_UART_Init();
    while (1)
  {
    	  HAL_ADC_Start(&hadc);
         HAL_ADC_PollForConversion(&hadc, HAL_MAX_DELAY);
	  readValue = HAL_ADC_GetValue(&hadc);
	printf("Read value : %d\n", readValue);
	HAL_ADC_Stop(&hadc);
	uint32_t soilmoist = 100 - (readValue / 40.96);
	 printf("Soil moisture : %ld %%\n", soilmoist);
	HAL_Delay(1000);
      }
  }
PUTCHAR_PROTOTYPE
{
	HAL_UART_Transmit(&huart2, (uint8_t *)&ch, 1, 0xFFFF);
	return ch;
}
Output screen shots on serial monitor :
Screenshot 2025-09-29 142938
WhatsApp Image 2025-09-29 at 20 15 18_7c3864c4

Result :
Interfacing a Analog Input (soil moisture sensor) with ARM microcontroller based IOT development is executed and the results visualized on serial monitor
