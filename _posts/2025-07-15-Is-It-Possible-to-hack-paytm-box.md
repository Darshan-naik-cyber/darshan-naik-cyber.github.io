---
title: "Is It Possible to hack paytm soundbox ? "
tags: [IoT Hacking, Hardware Hacking]
---
welcome to new article my ethical hacker kiddo. Here your friend , security researcher Darshan Naik aka `darshanhackz`, Yeah in the last article we have seen how to hack the IoT device of any SDR and that methodology will apply to the each & every device present into the IoT System or the embedded system connected with the RF.

<p align="center"> <img src="{{ '/images/paytm_box_hacking/paytm.webp' | relative_url }}" alt="paytm box" width="1800" /> </p>

#### Let's go throughout the basic information.

→ Physical Hardware Hacking
- Pentest of the hardware by directly connecting to the UART , JTAG and Multimeter or via connection of the USB.

→ SDR Hacking
- We will user here reply (or Even Man in the middle) attack against the Soundbox wireless link with the help of the BLE (Bluetooth Low-Energy) at 2.4 GHz.

> DISCLAIMER : THIS CAN BE DONE ONLY FOR EDUCATIONAL PURPOSE NEVER TRY AT HOME

<p align="center"> <img src="https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExZWExc2M3c29vMmJsN2x1M2l0NXd1anhhbDdoOXllOXExdnNsZ3N0ZCZlcD12MV9naWZzX3NlYXJjaCZjdD1n/4KZ1Q6nU8owonpgpSv/giphy.webp" alt="Animated GIF" width="200" /> </p>

## Architecture

Now Before going to attack let us understand some basics of the Paytm or Phonepe Soundbox any sound box those used for transaction confirmation.

<p align="center"> <img src="{{ '/images/paytm_box_hacking/UPI_backend.webp' | relative_url }}" alt="UPI Workflow" width="1800" /> </p>


### UPI Soundbox Communication — Backend System :

→ Customer initiates a payment using UPI — enabled app by scanning merchant QR Code

→ The payment request is sent to the UPI server.

→ The UPI Server forwards the request to the NPCI switch.

→ NPCI routes the request to the merchants account to very and complete transaction.

→ Once the transaction completed a confirmation came back from through same path.

→ The merchants bank send an API callback or push notification to UPI soundbox.

→ The Soundbox receives the message and play voice.

Here we have to focus on the last two lines about the API callbacks and send notifications definetly this can be done and get from the Internet from that wifi or dongal where those request are able to capture by soundbox then it will play that sound , “paytm pe 20 rupaye prapt hue” .

> Let us consider you got one soundbox in the old trash. This time we are performing the dumpster diving for the hacking of the IoT paytm soundbox.

#### Hardware teardown and Debug.
→ Here we will open and test out the UART , JTAG , SPI flash lines.
→ Multimeter to identify the VCC levels
→ Hook up the USB → TTL adapter on the UART lines to see if you get a bootloader console.

#### Flash Extraction.
→ Here we are directly dumping the flash chip data.
→ We will run the binwalk on the raw dump to carve out the uboot , kernel , roofs, certificate , config files , encrypted blobs.
→ We will use the tools like firmware-mod-kit or custom scripts to unpack the common file system.

#### Static Firmware Analysis.
→ Grep for the hardcoded URLs, Ips, API Keys , certificate oings , MQTT topic and cloud endpoints.
→ Identify the architecture and reverse the code with the Ghidra / IDA.

#### Dynamic Firmware Debugging and Custom Firmware.
→ Here we are interrupting bootloader and load your own Initramfs via TFTP / NFS.
→ Place the soundbox on a controlled VLAN and mirror its port to wireshark or tcpdump.
→ Search for the DNS queries like api-soundbox.paytm.com , *.paytm.io, aws-iot.paytm.com , etc.,
→ TLS handshake : Here we will extract the server certificates, inspector issuer
→ Perform the Active port scan against the cloud endpoints we have discover within this.

#### API reverse-engineering and Fingerprinting
→ Decompile the Android / iOS “Soundbox” companion app (apktool / jadx for Android , class-dump or Hopper for iOS) to extract :
All REST / MQTT endpoints , HTTP headers , payload formats (JSON schemas), auth tokens.
→ Spin up a local proxy (Burp . Mitmproxy) and pint the device /app at it to observe the JSON Response , error messages , server versions etc.,

---
:) 
## Let's Begin The Hacking

<p align="center"> <img src="https://media0.giphy.com/media/v1.Y2lkPTc5MGI3NjExcGlwcGl0NThqMXZ4bnZkb282Z2N6ZDNpemkwZ251ZDdpenJmM21mbiZlcD12MV9naWZzX3NlYXJjaCZjdD1n/YQitE4YNQNahy/200.webp" alt="Animated GIF" width="300" /> </p>

> Now let us consider we don’t have the device with us now we want to hack the Soundbox with the SDR with the analysis of the Protocol. Here the actual Hacking Starts.
Here with the help of the reply attack we against the wireless Soundbox we are going to repeat the same request , Because the device used Bluetooth Low-Energy (BLE) at 2.4 GHz with frequency hopping and link layer encryption.

#### Identify the radio Protocol.
→ The Soundbox is a BLE having the GFSK , 2 MHz Channel.
→ Its advertises on the Three advertising channels (37 , 38 , 39 ) and then ho-scatters data over 37 data channels.

#### Sniff the Connection parameter.
→ Ubertooth One + nrf sniffer plugin.
→ Start Capturing

```
Ubsertooth-btle -f -c 37, 38, 39 -w capture.pcap
```
→ In wireshark you will see the adv packets and connection request.
→ We can also use the hackRF with gr-bluetooth
→ Record a wide slice of the 2.4 GHz ISM band at >=10 Msps.

#### From here we need to extract
→ Access Address (32bits)
→ Channel map (37 bits)
→ Hop Interval / Increment

#### Capture the data packet
→ Here from the above steps we need to capture the data packets to dump the raw encrypted LL Data PDUs.(powe Distribution unit)
→ With a BLE Sniffer like Ubertooth this is handled for you automatically.

#### Reply the Attack
→ If the connection is unencrypted then we can simply re-transmit the raw LL PDUs Back to the same channels with identical timing and soundbox will accept them.
→ If the link is encrypted , you can still reply the raw encrypted PDUs.

```
hackrf_transfer -t capture_id.bin -f <channel_fre> -s <samplerate>
```
### Practical Takeover keys

- Play-Sound command is just a write to specific GATT [Generic Attribute profile] ( it defines the exchange between two BLE Devices.)
- Will send Some Connect request then response of that then IV echange then GATT writing will lead to beep sound means that “paytm par prapt hue”.
- Immediately re-inject the last LL Data PDUs to the soundbox. It will decode them (IV and MIC still Valid) and beep.

### Here what we have done see.

- A pure reply attack can be easily done if encryption within the channel is off.
- With BLE Encryption you must either reply very quickly or MITM the pairing to extract keys.
- Frequency hopping means you either need a purpose-built BLE sniffer that follows hops or multi SDR wideband capture + Demod chain.

Once you’ve mastered the sniff→decrypt (or sniff→replay-in-flight) pipeline, you can:
→ Trigger arbitrary sound patterns or LEDs at will.
→ Intercept and replay the OTA-DFU flow to drop your own firmware.
→ Measure RSSI/timing to map proximity or build a tracking attack.

<p align="center"> <img src="https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExa2FraW40amNxdG1tY3U1NDdpenVxMW5ldTJiaDZ2aXY2aGZ3aXkyMiZlcD12MV9naWZzX3NlYXJjaCZjdD1n/3k03Ft974KXPAkhnGk/200.webp" alt="Animated GIF" width="300" /> </p>

In this way there is a possibility of hacking the Paytm Soundbox and I know this will work as per my research and pentest thinking. If you have some points or something related to topic for discussion please try to ping me on the linkedIn or twitter or in the comments we will definitely discuss this topics, Cause as you also loves to read and do research like me.

Keep Hacking :) 