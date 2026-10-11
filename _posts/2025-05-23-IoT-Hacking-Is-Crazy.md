---
title: IoT Hacking is Damnn Crazyyy!!!
tags: [IoT Security | Hardware Security]
---

As a security researcher, I always love to learn new hacking techniques and always tries to research on the topics those are trending within the cybersecurity so that i can invest the time into learning and sharing some crazyyy things about this new techie world 🗺️.

`Lets us start the crazy IoT Thing.`
## Introduction.

Within this techie world there are lots of things that we want to automate or we wanna use while on the remote. For this thing the new technology evolve that is IoT means Internet Of Things. Here you are operating the embedded system from smartphone or with the RFID’s. Simple Concepts… Now lets just rush into the Hacking Part.

Let me give you one image here…

<p align="center"> <img src="{{ '/images/IoT_Hacking/FCC_ID.webp' | relative_url }}" alt="FCC_ID" width="600" /> </p>


Here on the every IoT Device there is one ID mentioned , its not only about the router its about every device that is used in the IoT machines , they will also contains same `FCC ID.`

Hey My Kiddo , you will not believe but with the help of the FCC ID we can get the complete information about the product so we can able to get the internal hardware structure, documentation , hardware structure with there versions online. you will get this on below url : https://fccid.io/

if you want the complete detail about the chipset then : `alldataset.com` wil help you to find out the detailed information about the target.

Tools Required for IoT Hardware Hacking →
→ VM Ware and OS
- kali
- Ubuntu
- IoT-PT

→ Tool kit
- Multimeter
- Soldering Kit
- Logic Analyzer
- JTAgulator
- HackerRF One
- CSR 4.0 Blue Dongle
- Ubertooth One
- API — Mate V4 Beta

Before Going to pentest you must clear with the UART i.e., `Universal Asynchronous Receiver and Transmitter`.
Where you will understand basic structure of hardware from where the signals are recive and transmit.

<p align="center"> <img src="{{ '/images/IoT_Hacking/UART.webp' | relative_url }}" alt="UART" width="400" /> </p>

The above image is of the UART , There are majory 4 component of UART. Yeah my kiddo one component is missing , cause that is always present see.

- VCC → voltage
- GND → Ground
- RX → Receiver
- TX → Transmitter

within every IoT Device this four basic component always present to run the device perfectly with all the signals.

There is also one component that is majorly used that is JTAG but i am not going to explain that here, if you are security nerd you can go and explore by yourself cause that is the fundamentals of some electronics kind thing. hehehehe…..

## Firmware Exploration

<p align="center"> <img src="{{ '/images/IoT_Hacking/Firmware.webp' | relative_url }}" alt="Firmware" width="400" /> </p>

So my hacker kiddos, what is firmware…. wait wait wait… I know you will google it.. why to take extra effort see, the definition from the Google → Firmware is a type of software embedded into hardware devices that controls their basic functions and how they interact with other devices.

See Kiddos , if you need to hack any IoT Device completely from the root then you must attack the there captain who is handling the core component of the team i.e, Firmware. hehehe 😝 you got it right.

For this firmware extracted and reversing we will required some tools that will help us to get the hardware hacking…. those are:-

→ flashrom
→ Binwalk
→ IDA Pro
→ Ghidra
→ Blue2
→ Wireshark
→ etc.,

Now Connect any IoT Device for physical testing of the application and then test with the following testing commands..

```
sudo apt-get install flashroom
```

```
sudo flashroom -p buspirate_spi dev=/dev/ttyUSB0 <here you have any usb>
```

```
sudo apt-get install binwalk
``` 

Here with the help of the flashroom we are extracting the Firmware of the device for Reverse Engineering and analysis of the firmware which is a heart of the machine… Now Lets explore who is inside that heart ♥️… hehe

<p align="center"> <img src="https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExZjMzbmgxajY2cDlubTFpOWRtaDgwenFua2tyeHF1cTBsaWFqc3JtdyZlcD12MV9naWZzX3NlYXJjaCZjdD1n/HPO8NGybLhXiHKemuT/200.webp" alt="Animated GIF" width="300" /> </p>

Now you will be having one question that is what is extracted firmware and how we can able to read that file in the machine.. Don't worry Kiddos I will explain you everything here clearly.. Comeon Kiddos let'sGOOO!!!


After saving the data by running the above command we need to further analysis from the Firmware Attack Surface… But what are the firmware attack surface.

#### Firmware Attack Surface →
- Physical Access
- Binaries
- File Systems
- Run Times

Here we are using the Firmware Hacking Target as a extracted Firmware from the device.. now what we need to checkout from that…
→ Hardcoded Passwords and IDs
→ Secret Keys
→ Confidential URL Leakage
→ Environmental Details

Now for the further extraction lets get into the directory of the firmware that we have extracted and grab some required content.

Search for the password stored within the firmware component :

```
find . -name passwd
```

```
find . -name /passwd.bak
```

Again Check for some configuration file of the firmware system :

```
find . -name cgf
```

After finding the passkey we can easily able to identify those into our kali linux machine with some inbuilt tool and get the cracked password.

```
hash id <whatever hash you find from above > 
```

```
hashcat -a 3 -m 500 passwd.bak
```

Suppose that hash is md5 then the -m value will be 500, no worriess hash id will give you the value with hash identifier.

Very Easy..
Now let us go with some RE and binary analysis with some tools. here first we will use binwalk.

#### Binwalk :

Binwalk is a tool for searching a given binary image for embedded files and executable code. Specifically, it is designed for identifying files and code embedded inside of firmware images.

Suppose we installed firm.bin from the firmware and now we need to identity the code embedded into that image so here we will use the binwalk

```
binwalk -E firm.bin
```

We can you IDA Pro or Ghidra too.. If you have a time to explore then explore my great kiddo hacker.

Now lets jump into SDR → Software Defined Radio

Mostly this SDR will based on the RF Technology i.e., Radio Frequency , this will help to communicate the device to device.

But where this technology is used ????
→ Access Control Systems
→ Navigation Systems
→ Autonomous Cars
→ Cell Phones
→ Industrial Systems.

See how much bigger the attack surface is…..

<p align="center"> <img src="https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExZGZ0a3lydHFtdWhjNnMxeGhzZDBtOGFlMTE1OG45N295bTI5b3RsZCZlcD12MV9naWZzX3NlYXJjaCZjdD1n/XVbQsIjdXDNyswwxOO/giphy.webp" alt="Animated GIF" width="300" /> </p>

Before that let us see some common vulnerabilties found in this SDR.
→ Evaesdropping
→ Replay Attack
→ Signal Injection
→ Relay
→ Jamming

Today within this article we are performing only the reply attack.

### So What is Jam and Reply attack ???

A replay attack (also known as a repeat attack or playback attack) is a form of network attack in which valid data transmission is maliciously or fraudulently repeated or delayed.

So we will send the same data with malicious way and we will get the access

<p align="center"> <img src="https://media0.giphy.com/media/v1.Y2lkPTc5MGI3NjExcXFtd2NpNTFqaXo3d3hieW40aHd0MzVxd2Ztdm5icHVpMnUzMTlieSZlcD12MV9naWZzX3NlYXJjaCZjdD1n/YQitE4YNQNahy/200.webp" alt="Animated GIF" width="300" /> </p>

First with the BLE → Bluetooth Low Energy
Here we will use the tool
- ubertooth One
- CSR 4.0 USB Bluetooth

connect this device to your computer and lets bang

Check out the interface and status of your tool

```
sudo hciconfig
```

Now let just make status of the device up.

```
sudo hciconfig hci0 up 
```

Now we will use the bingmt tool to start and scan

```
sudo hcitool lescan 
```

Here we have to copy the MAC Address of the device that we get from the above communication.

after conneting your ubertooth we will paste that MAC ID in below command

```
sudo ubertooth -f <MAC ID>
```
Make a separte dir to record the communication in the file.
Here we will also use the wireshark simultaneously to record the complete communication for analysis purpose.

```
sudo mkinfo /tmp/ble 
```

```
sudo ubertooth -f -t <MAC ID> -c /tmp/ble 
```

After running this command.

Open Wireshark then follow :
→ Open the Wireshark main interface
→ Click on Options → Capture → Manage
→ Here we have to open the /tmp/ble path where our data file of communication will store and start listening.

Within the communication you will get the ID Key from where we can able to get access. After getting those ID Value copy though all data and finally time to get into the machine…..

```
sudo gattool -b <ID-VALUE> -I 
```

If we get the connection from the device that we have to crack the key.

```
sudo gatttool -b <ID-VALUE> -I
```

```
# connect <ID-VALUE>

# security-mode

# char-write-cmd <0x002a> 01
```
**OR**

Then Follow below :

```
> connect 
> Characteristics 
> char-write-req <varible from wireshark like 0XX000cd> <value> 
```

if this works then : 
Bangggggg….. You will have the internal access with the legitimate user and you will hack the target IoT Device with this…

if you want further hacking with Zigbee in the PoC way. Then share this article to more people so i can get some motivation…

Thank you hacker kiddoss……

Make Sure You Follow My Socials :)

<p align="center"> <img src="https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExNXMxOTloMXhyMzVzY3gzdmZrYmhxNTBrMGoyZW00a3dsczFuaXpsNCZlcD12MV9naWZzX3NlYXJjaCZjdD1n/SnioCkL9cd3B6/100.webp" alt="Animated GIF" width="300" /> </p>

Happy Hacking :)