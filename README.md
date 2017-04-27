# Wipy-RFID-Reader
Make Wipy able to read RFID by using a RFID-RC522 antenna

You will need:
* A Wipy (personnaly i use the 2.0) with the board for USB connection,
* A RFID-RC522 antenna
* A Non-Welded support plate member
* A Wifi
* Some cables

As for new users it can be hard to connect the wipy with your computer i will teach you how to do it quickly on Ubuntu.
First to check if your wipy is alright you need to connect your board+wipy to the computer with the usb cable, if everything is alright there should be the heart beat led blinking every 4 seconds. 

## Connect the wipy to the wifi
Then you need to connect your wipy to internet to gain access to the file system. To do that you need to run a serial manager, i used minicom which worked really well, here is the command line that you should use:

```bash
minicom -b 115200 -D /dev/ttyUSB0
```
>!!! USB0 goes for my setup that's why you should check up yours, it can be the same or a greater number, anyway I used hwinfo to find it so use it to find yours

Now you should have minicom started, use CTRL+A then Z to access the menu now go to minicom configuration, then serial port configuration and disable the hardware flow control. Now everything should be alright, pressthe reset button on your Wipy.
Now you should have the initialisation of your chip and at the end you should have something like >>>
You have? I hope so, check if you can use some python like:
```python
print("hello")
```
If it said hello then let's try something harder like:
>Personnaly after this part i used [Pymakr](https://github.com/pycom/Pymakr) because the it can detects and link the card by itself and it more easy to read for the (debug part), if you want to use it just go in preferences>pycom device>then select your device
```python
from network import WLAN
wlan = WLAN(mode=WLAN.STA)
wlan.connect(ssid='Your_SSID', auth=(WLAN.WPA2, 'Password'))
print(wlan.ifconfig())
```
This allows you to link your wipy to your wifi and print the IP address.The first code is the IP address.

### Access File system
If everything is okay you need to start a FTP manger which will makes you able to reach the file system.
Start [FileZilla](https://filezilla-project.org/) then select files and choose Site manager, now here is the few options you need to change:
* Host = the IP of the Wipy
* Protocol = FTP
* Encryption = Classic FTP connection (unsecurised)
* ID = micro
* Password = python
Now just click on connect if everything is alright you should have the access on a file called __Flash__. Inside you can find some python documents like __boot.py__ and __main.py__. Like the name says __boot.py__ is read one time, when the wipy boot, then he read the __main.py__. Download the __boot.py__, inside you should already have:
```python
# boot.py -- run on boot-up
import os
from machine import UART
uart = UART(0, 115200)
os.dupterm(uart)
```
This is what allowed you to send python commands on the wipy so don't change it. open the one you downloaded then put in:
from network import WLAN
```python
wlan = WLAN(mode=WLAN.STA)
wlan.scan()
wlan.connect(ssid='Your_SSID', auth=(WLAN.WPA2, 'Password'))
while not wlan.isconnected():
    pass
print(wlan.ifconfig()) 
```
>!!! You can put the previous code but as the detection can be hard it happens that the wipy doesn't connect
Save your file then put it in your wipy and overwrite the old one.
