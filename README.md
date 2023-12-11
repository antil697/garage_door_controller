# garage_door_controller

I decided to share my code for my garage door controller. This works with any door that uses a single button to start/stop/reverse the door operation.

While this is easy to achieve with a single esp8266 module and a relay, I needed more safety features. As I have a double garage with two cars and I am using HomeAssistant with Frigate to open and close the door, I needed to make sure that no accidental door trigger would happen when a car is attempting to enter or leave.

My solution was to use a D1-mini board, added a relay, two endstop siwtches to detect the door state and a IR light sensor that checks that the door is unobstructed. 
It also needs to operate idependant of HomneAssistant to operate the door and still allow the manual operatoin with button or remote control as backup. 

I deployed a state machine in the code. The State Machine that builds the basis for the code is shown [here](https://stately.ai/registry/editor/c75df474-d5b7-42d2-aade-f372775f61c1?machineId=daa53767-9e83-4e3c-a6ed-1a4582f7df22&mode=Design)

It will allow the door to be controlled via HomeAssistant (and via Carplay with HomeKit integration). When an obstruction is detected while the door is closing, it will stop and reverse the door operation. 
When and obstruction is detected while a CLOSE command is received, it will ignore the command. Should the command come from the manual button/remote controller, it will quickly correct and reverse the door to fully open. 