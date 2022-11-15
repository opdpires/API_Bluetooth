# API Bluetooth JDY18

This API was developed by Leandro Gripp and Pedro Pires as a work in the discipline of Embedded Systems Programming at UFMG - Prof. Ricardo de Oliveira Duarte - Department of Electronic Engineering.

Library is developed and tested with STM32F4xx (Nucleo 64 board).

The main.c file contain exemples of usage based in Bare Metal programming. Therefore, the functions used in the exemple are executed in a loop, using the polling method, watching the input buffer of the BLE module. The developed application consists on an LED control system, which listens for two types of message: TURN__ON and TURN_OFF and reacts accordingly. It also writes the current state of the LED to the BLE module output buffer so that the device that's communicating with it knows the current state of the LED. For this application, we used the communication through I2C, even though UART communication is also possible.


## Hardware requirements:
* STM32F4xx microntroller;
* JDY18 BLE module;
* Phone to connect with the BLE module and test its functionality.

## STM32CubeMX configuration and connections:

### Pinout view

![image](https://user-images.githubusercontent.com/48407152/201791807-2bcbd942-69cd-42ce-9ac6-ed013c333745.png)

As can be seen, the following connections are needed:
* SCL port of the module to PB6 of the microcontroller;
* SDA port of the module to PB7 of the microcontroller;
* Common ground.

### GPIO Mode and Configuration
![image](https://user-images.githubusercontent.com/48407152/201792776-2a787d7e-05e2-4e54-ac98-ae2631c6dc87.png)

### I2C1 Mode and Configuration/Parameter settings
![image](https://user-images.githubusercontent.com/48407152/201792518-96cdc221-4c9f-4035-a0d1-9c00226028d7.png)
![image](https://user-images.githubusercontent.com/48407152/201792697-9067fe95-c7e5-4e88-ac12-20abaf04ddef.png)

## API usage
1. Download Bluetooth_BLE_V4.2_JDY-18.c and Bluetooth_BLE_V4.2_JDY-18.h from src directory;
2. Copy Bluetooth_BLE_V4.2_JDY-18.h to your projec's Inc directory and the Bluetooth_BLE_V4.2_JDY-18.c to your projec's Src directory;
3. Include where you expect to use.

## API functions

```c
void ble_init();
```
Defines initialization values and prepares the API to work.

```c
HAL_StatusTypeDef ble_write(I2C_HandleTypeDef *hi2c, uint16_t devAdress,
                            uint8_t *buffer, uint32_t size);
```
Write a buffer into an address on the BLE device. The address can be specified in order to change any settings.

Parameters:
* `hi2c`: The address of the I2C interface to be used;
* `devAdress`: Adress to write to on the device;
* `buffer`: Content to be written;
* `size`: Size of the buffer.

```c                          
HAL_StatusTypeDef ble_read(I2C_HandleTypeDef *hi2c, uint16_t devAdress,
                           uint8_t *buffer, uint32_t size);
```
Read from an address on the BLE device and dump it to a buffer.
Parameters:
* `hi2c`: The address of the I2C interface to be used;
* `devAdress`: Adress to read from the device;
* `buffer`: Buffer to dump the content to;
* `size`: Size of the buffer.

```c                           
uint16_t ble_send_info(I2C_HandleTypeDef *hi2c, const uint8_t *buffer, uint32_t size);
```
Send a message using the BLE device. Wrapper around ble_write, specific for sending messages.

Parameters:
* `hi2c`: The address of the I2C interface to be used;
* `buffer`: Content to be sent;
* `size`: Size of the buffer.


```c
void ble_disconnect(I2C_HandleTypeDef *hi2c);
```
Disconnect the device when there's an active connection.
Parameters:
* `hi2c`: The address of the I2C interface to be freed;

```c
void ble_reset(I2C_HandleTypeDef *hi2c);
```
Reset the BLE device to its default settings.
Parameters:
* `hi2c`: The address of the I2C interface to be reset;
