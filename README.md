# Seeed_Arduino_Grove_DopplerRadar  [![Build Status](https://travis-ci.com/Seeed-Studio/Seeed_Arduino_DopplerRadar.svg?branch=master)](https://travis-ci.com/Seeed-Studio/Seeed_Arduino_DopplerRadar)

![Optional Text](../master/images/Radar.JPG)

This library is for DopplerRadar. This DopplerRadar can get the relative speed of the target and the state of the target's movement ie backwards and frowards. It can not detect relatively static objects or objects that move sideways relative to the device.

## Hardware requirements

- Wio Terminal
- Grove cable to male pins.
- Grove DopplerRadar(BGT24LTR11)

### Connection of the hardware
Use grove cable colour coded pins as in photo:
![Optional Text](../master/images/RadarWiring.JPG)


## Usage

- Download BGT24LTR11.zip and use Arduino library include zip to install library.

- Run the demo "BGT24LTR11_DETECTION_TARGET" on examples directory.

- Start a project.  

- You can choose to use the hardware or software serial port.
  ```c++
      #define COMSerial Serial
      #define ShowSerial SerialUSB
      BGT24LTR11<HardwareSerial> BGT;
  ```

- Set the communication baud rate of the DopplerRadar module (115200).

- Then initialize BGT in the setup function.

- Set the working mode.
  ```c++
  void setup()
  {
    // put your setup code here, to run once:
    ShowSerial.begin(9600);
    COMSerial.begin(115200);
    BGT.init(COMSerial);
    while (!ShowSerial)
      ;
    while (!COMSerial)
      ;
    /*
     * MODE 0 -->detection target mode
     * MODE 1 -->I/Q ADC mode
     */
    while (!BGT.setMode(0))
      ;
  }

  ```
- Gets the speed value and target state.
```c++
void loop()
{
	 // put your main code here, to run repeatedly:
	 uint16_t state=0;
	 ShowSerial.print("target speed:");
	 ShowSerial.println(BGT.getSpeed());
	 state = BGT.getTargetState();
	 //2 --> target approach
	 //1 --> target leave
	 //0 --> Not Found target
	 if (state == 2)
	 {
	   ShowSerial.println("target approach");
	 }
	 else if (state == 1)
	 {
	   ShowSerial.println("target leave");
	 }
	 delay(200);
}

 ```

## Methods

- uint16_t getSpeed();

- uint16_t getTargetState();

- uint8_t getIQADC(uint16_t I_d[],uint16_t Q_d[],uint16_t *len);

- uint8_t setSpeedScope(uint16_t maxspeed,uint16_t minspeed);

- uint8_t getSpeedScope(uint16_t *maxspeed,uint16_t *minspeed);

- uint8_t setMode(uint16_t mode);
	- mode 0 :detect the target mode.
	- mode 1 :Gets the I/Q information ADC value mode.


- uint8_t getMode();

- uint8_t setThreshold(uint16_t whreshold);

- uint16_t getThreshold();

## License

See LICENSE.
