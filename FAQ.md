# Frequently Asked Questions

This section is a lie because as of the time of writing, nobody has asked any questions. So this section should probably be named PFAQ (Potential Frequently Asked Questions)

## Table of Contents:

- [Should I buy this version or the PxMatrix version](https://github.com/witnessmenow/ESP32-i2s-Matrix-Shield/blob/master/FAQ.md#should-i-buy-this-version-or-the-pxmatrix-version)
- [Will it work with my display](https://github.com/witnessmenow/ESP32-i2s-Matrix-Shield/blob/master/FAQ.md#will-it-work-with-my-display)
- [What pins are used by the display](https://github.com/witnessmenow/ESP32-i2s-Matrix-Shield/blob/master/FAQ.md#what-pins-are-used-by-the-display)
- [What pins are available to use for sensors etc](https://github.com/witnessmenow/ESP32-i2s-Matrix-Shield/blob/master/FAQ.md#what-pins-are-available-to-use-for-sensors-etc)
- [Can I connect a SPI sensor/device to the shield](https://github.com/witnessmenow/ESP32-i2s-Matrix-Shield/blob/master/FAQ.md#device-to-the-shield)

---

### Should I buy this version or the PxMatrix version?

Tough one to get us started! 

**Advantages of the PxMatrix version:**
- No need to edit the library header file.
- PxMatrix has proven good support.
- PxMatrix more than likely supports more differnt styles of panels. Some of the matrix panels have strange layouts and scan patters which PxMatrix can handle.
- There are alot of examples of PxMatrix projects.

**Advantages of the i2s version:**
- Simpler to use than PxMatrix (other than editing the header file)
- The shield is pyhsically smaller
- Does not need the ribbon cable to the out connector
- I2S library is faster - [Check out Aaron Christophel's FPS comparison!](https://www.youtube.com/watch?v=HKWDGangWU0)
- It has a better chance with working with multiple number displays. PxMatrix supports multiple displays, but struggled to handle over 3. The I2S library has a better chance of working with more ([@MLE_Online has tested it with 4 so far](https://twitter.com/MLE_Online/status/1291547518493274113))

Personally, for future projects I will more than likely use the i2s version as my go to. If you are a beginner, maybe PxMatrix is an easier choice due to all the examples.

### Will it work with my display?

Honestly, I can't answer that for sure, all I can say is it should work with any display that works with the [ESP32-RGB64x32MatrixPanel-I2S-DMA library](https://github.com/mrfaptastic/ESP32-RGB64x32MatrixPanel-I2S-DMA). I can't guarantee that all display's will work. 

Even links to displays I have bought in past that work are subject to new stock/revisions (this happened previously).

### What pins are used by the display?

[The schematic of the product can be found here](https://i.imgur.com/wbVGML8.png). 

Here is a list of pins that are used by the display:

```
#define R1_PIN_DEFAULT  25
#define G1_PIN_DEFAULT  26
#define B1_PIN_DEFAULT  27
#define R2_PIN_DEFAULT  14
#define G2_PIN_DEFAULT  12
#define B2_PIN_DEFAULT  13

#define A_PIN_DEFAULT   23
#define B_PIN_DEFAULT   19
#define C_PIN_DEFAULT   5
#define D_PIN_DEFAULT   17
#define E_PIN_DEFAULT   18 // This is the only change from the deafult pins of the library
          
#define LAT_PIN_DEFAULT 4
#define OE_PIN_DEFAULT  15

#define CLK_PIN_DEFAULT 16
```

###  What pins are available to use for sensors etc

V1.2 and above of the shield has the two i2c pins which are broken out to the accelerometer pads. These are the default i2c pins for the ESP32 so you should just be able to use the Wire object without specifying pins

```
#define SDA 21
#define SCL 22
```

V1.3 added some more pins to the accelerometer pads:

- 32
- 33
- 34 (this is an input only pin)
- 2 (this is shared with the onboard LED of the ESP32 board)

###  Can I connect a SPI sensor/device to the shield?

It is possible to connect SPI devices to V1.3 shields (V1.2 doesn't have enough pins broken out). Some of the default SPI pins are used by the Matrix, but I have succesfully used an SPI device using the following pins:

```
#define NFC_SCLK 33
#define NFC_MISO 32
#define NFC_MOSI 21 //SDA
#define NFC_SS 22 //SCL
```

You will need to use these pins in your SPI.begin command:

`spi->begin(NFC_SCLK, NFC_MISO, NFC_MOSI, NFC_SS);`

[Untested by me] Some libraries for devices do not allow you to pass in custom pins, but a lot seem to allow you pass in an SPI interface. Here is an example using an SD card:

```
SPIClass spi = SPIClass(HSPI);
spi.begin(NFC_SCLK, NFC_MISO, NFC_MOSI, NFC_SS);
SD.begin(NFC_SS, spi, 80000000);
```
[Source of SD card info](https://www.instructables.com/Select-SD-Interface-for-ESP32/) 


