# Wemos_ControlBlinds_Control_Lights_usingPubSub
Code to control the blinds as well as turn on and off living room lights in apartment

This is modified code from the 'control motor' to include checking the current time and will turn the 12v LED strip on or off depending on what time it is.

### Features:
- Non blocking code
- Initialize the NTP client and get the current time.
- Keep checking if the time is 18:00 and less than 22:00 and turn lights on, else lights off.
- Uses a Solid State Relay to turn on and off the relay from pin 13 or D7
- The topic is controlling liv room lights is "inLivrmLightsControlTopic". You can either send the command from cloudmqtt, and payload as 0 for off, and 1 for on.
- The topic for Blinds control is "inBlindsControl", send the payload as 0 for lowering, 1 for raising and 2 for motor to be turned off.

- Code changes are a) used Pin 13 to control the lights, 
```
#define CONTROL_LIV_ROOM_LIGHTS  13 // also known as D7 ( thanks to Daksh )

in setup() {
...
  pinMode (CONTROL_LIV_ROOM_LIGHTS, OUTPUT);  
...

In loop , check every second if the time has come to turn lights on and off


      digitalWrite(CONTROL_LIV_ROOM_LIGHTS, LOW);  // Lights On.

```


In short, the code in loop(), Here LOW means (digitalWrite(CONTROL_LIV_ROOM_LIGHTS, LOW); ) the small LED on the SSR will light up ! ( indicating that the SSR is on )- it's the reverse!

```
 unsigned long currentMillis = millis();
  if (currentMillis - previousMillis >= interval) {
    // save the last time you blinked the LED
    previousMillis = currentMillis;
    timeClient.update();

    Serial.println(timeClient.getFormattedTime());
    int curHour = timeClient.getHours();
    
    int curMinutes = timeClient.getMinutes();
    //Serial.println(curMinutes);
    //Serial.println(curHour);
    if (( curHour >= 18 ) && ( curHour <= 22 ) ) {
    //if ( lights_on == 0 )  { // for debug purposes - blink the lights on off every second.

      Serial.println ("Light is ON ");
      digitalWrite(CONTROL_LIV_ROOM_LIGHTS, LOW);  // Lights On, 
      lights_on=1;
    } else {
      lights_on=0;
      Serial.println ("Light is OFF ");
      digitalWrite(CONTROL_LIV_ROOM_LIGHTS, HIGH);  // Lights Off, 
      
    }
```
      
### HIGH

digitalWrite(CONTROL_LIV_ROOM_LIGHTS, HIGH);  // Lights Off,  ( no led on the SSR )

![image](https://user-images.githubusercontent.com/14288989/213980710-b0626e44-4fe9-40ea-b97a-8a4e8ec0d291.png)


### LOW

digitalWrite(CONTROL_LIV_ROOM_LIGHTS, LOW);  // Notice the small led is lit. ( it's the reverse )

![image](https://user-images.githubusercontent.com/14288989/213980965-fef89c47-b7f0-4bab-b8ff-8884d6b41b05.png)


### Pin diagram of Wemos ( PIN 13 is whats being used for turning on and off the solid state relay)

![image](https://user-images.githubusercontent.com/14288989/213981141-fdd162de-1db4-401b-8158-c2fc925fc78e.png)


### The basic circuit in the junction box.

![image](https://user-images.githubusercontent.com/14288989/213982119-8997fd56-5dc8-4e6c-8085-5c64ceb2afca.png)


### Code in HTML 

![image](https://user-images.githubusercontent.com/14288989/213998396-18493ca8-258e-499a-bee6-f75ce303c53d.png)

