---
layout: post
title:  "The AccelLehenga"
date:   2017-04-05 00:00:00
---

![Shalue wearing the AccelLehenga](/assets/20180405/shalue.jpg)

This is my friend Shalue. Shalue is super cool. Shalue deserves a super cool wedding. For Shalue's wedding, I constructed the AccelLehenga.

The plan was simple. Chris and Shalue would provide me with a lehenga that she planned to wear to her reception and I would modify it. I learned a bunch from my [previous wearable project]({{site.url}}/2016/03/13/a-dress.html) and was confident I could produce something more polished.

<!--more-->

One problem: this thing was embroidered, very embroidered. Every inch of the skirt was covered in heavy decoration. Sewable electronics are cool but they're not particularly attractive. Previously, I had hidden everything under sheer fabric but that wasn't going to work here; any LEDs I wanted to add had to be hidden somewhere on the surface without disrupting the existing decoration too much. Most existing sewable LED components were too big for this, so I ended up buying individual surface-mount LEDs and soldering loops onto them so I could sew them to the skirt.

![Tiny Neopixel LEDs](/assets/20180405/soldered_leds.jpg)

I made about 50 of these.

These LEDs were serially addressable and could be connected using conductive thread and controlled with the FastLED library. I used conductive ribbon for the power and ground lines so that I could run longer strings of LEDs without losing brightness. For this project, I also used the Adafruit FLORA rather than the Lilypad Arduino as it has some more convenient connectors, namely a JST battery connector and a USB port. The whole thing ran on a Lipo battery pack that sat in a pocket in the skirt's petticoat. We had the swap out the battery once during the party.

{:refdef: style="text-align: center;"}
![Lehenga in progress](/assets/20180405/lehenga_skirt.jpg){:class="image-group-small"}
![Lehenga in progress](/assets/20180405/skirt_animation.gif){:class="image-group-small"}
{:refdef}


With the lights working, I was also able to add an accelerometer, the result was subtle but the lights would speed up the faster Shalue was moving.

### Code
    #include <Wire.h>
    #include <SPI.h>
    #include <Adafruit_LSM9DS0.h>
    #include <Adafruit_Sensor.h>

    #include <FastLED.h>
    #define NUM_LEDS 10
    #define NUM_LEDS2 30
    #define MOVE_THRESHOLD 500

    CRGB leds[NUM_LEDS];
    CRGB leds2[NUM_LEDS2];
    Adafruit_LSM9DS0 lsm = Adafruit_LSM9DS0();
    uint8_t start_brightness;
    uint8_t brightness;
    uint8_t cos_brightness;
    double delta[10];
    double storedVector;
    int delta_i;
    int light_speed;
    int switch_state = 0;
    int last_switch_state = 0;

    void setup() {
      Serial.begin(9600);
      pinMode(0, INPUT);

      if (!lsm.begin())
      {
        Serial.println("Oops ... unable to initialize the LSM9DS0. Check your wiring!");
        while (1);
      }
      lsm.setupAccel(lsm.LSM9DS0_ACCELRANGE_2G);
      lsm.setupMag(lsm.LSM9DS0_MAGGAIN_2GAUSS);
      lsm.setupGyro(lsm.LSM9DS0_GYROSCALE_245DPS);
      FastLED.addLeds<NEOPIXEL, 6>(leds, NUM_LEDS);
      FastLED.addLeds<NEOPIXEL, 12>(leds2, NUM_LEDS2);

      delta_i = 0;
       for (int i = 0; i < 10; i++) {
        delta[i] = 0;
      }
      start_brightness = 0;

      lsm.read();
      storedVector = lsm.accelData.x*lsm.accelData.x;
      storedVector += lsm.accelData.y*lsm.accelData.y;
      storedVector += lsm.accelData.z*lsm.accelData.z;
      storedVector = sqrt(storedVector);
    }

    void loop() {
      // get new data!
      last_switch_state = switch_state;
      switch_state = digitalRead(0);
      Serial.println(switch_state);

      if (switch_state) {
        lsm.read();
        double newVector = lsm.accelData.x*lsm.accelData.x;
        newVector += lsm.accelData.y*lsm.accelData.y;
        newVector += lsm.accelData.z*lsm.accelData.z;
        newVector = sqrt(newVector);

        delta[delta_i] = abs(newVector - storedVector);
        //Serial.println(delta[delta_i]);
        delta_i = (delta_i + 1) % 10;

        double average_delta = 0;
        for (int i = 0; i < 10; i++) {
          average_delta += delta[i];
        }

        storedVector = newVector;

        brightness = start_brightness;
        for(int i = 0; i < NUM_LEDS; i++) {
          cos_brightness = cos8(brightness);
          leds[i] = CHSV(150,150, cos_brightness > 75 ? cos_brightness : 0);
          brightness = (brightness + 25) % 255;
        }
        brightness = start_brightness;
        for(int i = 0; i < NUM_LEDS2; i++) {
          cos_brightness = cos8(brightness);
          leds2[i] = CHSV(150,150, cos_brightness > 75 ? cos_brightness : 0);
          brightness = (brightness + 8) % 255;
        }
        FastLED.show();
        light_speed = (average_delta / 1000) * 1;
        start_brightness = (start_brightness + (light_speed > 5 ? light_speed : 5)) % 255;

      } else {
         for(int i = 0; i < NUM_LEDS; i++) {
          leds[i] = CHSV(0,0,0);
        }
        for(int i = 0; i < NUM_LEDS2; i++) {
          leds2[i] = CHSV(0,0,0);
        }
        FastLED.show();
      }

      delay(30);
    }

Back to hiding the LEDs. We had _very carefully_ removed a bunch of gems from the skirt in order to make space for the lights. I was able to print a hollowed out version of these gems using clear filament and sew them over the LEDs. You could still see the chips through the gems, but the illusion was mostly seamless it you weren't looking too closely.

![Printed Gems](/assets/20180405/printed_gems.jpg)

Shalue was happy, which makes me happy. There were some glitches the day of the reception, which like all Arduino problems were mostly solved by pressing the reset button. I may or may not have followed 15 feet behind her for the whole night just to make sure the thing was still working.

![Artsy Photo](/assets/20180405/shalue_2.jpg)

#### Bonus: A Vagabond Jacket

Shortly after finishing the AccelLehenga, I used a similar process to make a much simpler piece for myself. This one borrows [Shisha embroidery](https://en.wikipedia.org/wiki/Shisha_(embroidery)) techniques to disguise the LEDs.

![Jacket Animation](/assets/20180405/jacket_animation.gif)
