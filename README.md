# Wemos_ControlBlinds_Control_Lights_usingPubSub
Code to control the blinds as well as turn on and off living room lights in apartment

This is modified code from the 'control motor' to include checking the current time and will turn the 12v LED strip on or off depending on what time it is.

### Features:
- Non blocking code
- Initialize the NTP client and get the current time.
- Keep checking if the time is 18:00 and less than 22:00 and turn lights on, else lights off.

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
      digitalWrite(CONTROL_LIV_ROOM_LIGHTS, HIGH);  // Lights On, 
      
    }
```

