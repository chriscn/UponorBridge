# UponorBridge
## Brief
My father's house, which I no longer fully live in since going to university runs an Uponor system to control the underfloor heating in each invidual room. It would be really useful if we were able to harness the temperature from each room for use in Home Assistant. Further research leads me to this [Manual](https://data2.manualslib.com/pdf4/87/8670/866988-uponor/climate_control_zoning_system.pdf?4d50e6ace582b4bc037580e855859d9e), we have those exact control units in each of the rooms.  
After taking one slightly apart (just removing the battery cover) it seems that it communicates over the 868.3Mhz band, now this is either going to be Lora or just a standard 868.3Mhz, I am hoping it is going to be the later since Lora is encrypted.
## Architecture
### Messaging
The easiest way to publish the data it seems is through MQTT, which is an amazing protocol written exactly what we want. We already have a MQTT server setup in the house so I shall just being using that for this project; however they are super easy to run and if you need to Home Assistant OS has a addon that runs the server search for [Mosquitto](https://mosquitto.org/). I won't cover setup in this tutorial.
### A New Hope
I am hoping that the messages are just broadcasted with an ID and then a temperature value over the air back to the base station, it might be that it is encoded in some format but that is yet to be found out. I am currently away at University at the time of writing this so I won't be updating it until the summer; in the mean time if I can spare some beer money I am going to buy a transciever for the 868.3Mhz band from china and expiremenet around with it. If we are able to get the set temperature that could be useful but I doubt that we will be able to set the temperature of each room.
### Overview
If I am correct with the unencrypted broadcast messages being sent from the thermostats then the project will be divided into the following parts:
#### Reciever (Codename: Pedit)
This will be an Arduino compataible sketch that I might write for a ESP32 that has Wifi that listens out for the signals sent and just `POST` then to an API endpoint with all the data it knows. I am wanting to have as little code on the Arduino as possible mainly because I am not as confident.
#### Bridge (Codename: Ark)
This will listen on the specified endpoint for a POST request and then perform some internal logic, mainly matching up the identification to a friendly name and then posting that over MQTT so that Home Assistant can easily update.
#### Request from Pedit to Ark
```json
{
	"id": 1, // thermostat raw id
	"current_temperature": 10.2 // raw float value
}
## Resources
Below is a list of resources mainly PDFs that have helped me.
- [T75 User Guide](https://issuu.com/ashford/docs/t75_user_guide)
- [Climate Control Zoning System I](https://data2.manualslib.com/pdf4/87/8670/866988-uponor/climate_control_zoning_system.pdf?4d50e6ace582b4bc037580e855859d9e)
