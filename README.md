# garage_door_controller

I decided to share my code for my garage door controller. This works with any door that uses a single button to start/stop/reverse the door operation.
Turns out that my current opener had MyQ features but I never trust any company to provide reliable access for devices with an expected lifespan of over 10+ years.

While this is easy to achieve on ESPHome or even Tasmota with a single esp8266 module and a relay, I needed more safety features. As I have a double garage with two cars and I am using HomeAssistant with Frigate to open and close the door, I needed to make sure that no accidental door trigger would happen when a car is attempting to enter or leave.

My solution was to use a D1-mini board, added a relay, two endstop siwtches to detect the door state and a IR light sensor that checks that the door is unobstructed. 
It also needs to operate idependant of HomneAssistant to operate the door and still allow the manual operatoin with button or remote control as backup. 

I deployed a state machine in the code. The State Machine that builds the basis for the code (image below) can be simulated [here](https://stately.ai/registry/editor/c75df474-d5b7-42d2-aade-f372775f61c1?machineId=daa53767-9e83-4e3c-a6ed-1a4582f7df22&mode=Design)
![State Machine](/assets/StateMachine.png)
It will allow the door to be controlled via HomeAssistant (and via Carplay with HomeKit integration). When an obstruction is detected while the door is closing, it will stop and reverse the door operation. 
When and obstruction is detected while a CLOSE command is received, it will ignore the command. Should the command come from the manual button/remote controller, it will quickly correct and reverse the door to fully open. 

This has been working solid now for over a year. Frigate integration in HomeAssistant will detect a car leaving and close the door when it leaves the driveway. On return, it will open the door. The automation will watch for a car to pull up to the garage door after entering the home zone. It is one my most favoured automations that just work. 
