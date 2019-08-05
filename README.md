# TivaC-Series-LaunchPad-I2C-temperature-sensor
In its simplest form, this lab is designed to teach individuals how to utilize the UART to communicate with the serial monitor while using the I2C bus to transmit and receive data from the Digilent Orbit Board’s onboard temperature sensor. It will introduce the individuals with concepts involving UART and communicating through the serial interface PuTty.

UART, which stands for Universal Asynchronous Receiver/Transmitter, is a peripheral device on the Tiva LaunchPad. This hardware is basically a two wire system that has one transmitting data while the other receives data. The parallel data has to be converted to serial for the data to be able to be transferred over a cable with the communication standard of RS-232. It is universal due to the adjustability of the transmission speed in which the baud rate can be set to the user’s specification.  In this lab, the UART is utilized to display the temperature on the serial monitor. The Digilent Orbit board has an onboard temperature sensor that uses a serial protocol, the I2C bus, to communicate and transfer the data between devices. 

There are two main objectives in this lab. First, the code was to be modified to eliminate the decimal number of the displayed temperature on the serial monitor. Then, once a specified temperature was chosen, an on board LED was used to illuminate at that temperature. To begin, the array displaying the data to the serial monitor needs to be modified.

unsigned char temp_data[] = "00.0 C \n\n\r";        //old code

unsigned char temp_data[] = "00 C \n\n\r";        //new code

By changing the output expectation, the array automatically displays this new format for the temperature. However, one more part of the code had to be omitted as well. In the Read_temp(unsigned char *data) function, it determines the temperature to a .5 accuracy. 

    if(temp[1] == 0x80){                        // Test for .5 accuracy
    data[3] = 0x35;
    }
    else{
    data[3] = 0x30;}

Because the decimal place is getting omitted entirely, this code can just be deleted. Next, the correct port and pin that will be used for the LED has to be initialized. For this lab, the onboard RGB LED was implemented so GPIO PORT F with pins 1, 2, and 3 were enabled. To work with the data from the temperature sensor, the code to turn on the RGB LED is nested within the Read_temp function. 


if(data[0] >= 0x33){
GPIOPinWrite(GPIO_PORTF_BASE, GPIO_PIN_1|GPIO_PIN_2|GPIO_PIN_3, 4);

 // LED Blue on Launchpad
     }else{
     GPIOPinWrite(GPIO_PORTF_BASE, GPIO_PIN_1|GPIO_PIN_2|GPIO_PIN_3, 8); // LED green
     }
}

The temperature of 30°C was chosen with the hexadecimal representation of 0x33. This states that when the 3 is in the 10’s position of the ASCII conversion, the RGB LED changed from green, which it will automatically be green if it is under 30°C, to blue.

