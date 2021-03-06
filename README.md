# Sprint 1 Design Document
Zephyr semester project

## Deployment Environment 
The project is hosted on an EC2 server and can be accessed by the user on an internet connected device. A Raspberry PI controls the system and the hardware associated with it including motion sensors, lights, and a camera. 

## Functional Requirements

### Adjust Lights
1. The system should control attributes of the lights remotely via user input.
2. The system must update to manual actions done by the user.
3. The system should know what attributes of the lights can be changed.
4. The system should restrict users from lights based off privilege.

### Front Door Camera
1. The sensor must detect movement.
2. The camera must take a photo of visitor.
3. The database must be updated to have accurate information of the system's status.
4. The users are notified of an event.
5. The users can check the status and view the photo taken by the camera.
6. The user can view a live stream of outside their front door.

### Television Time Restriction
1. The Zephyr system should login users before use.
2. The Zephyr system should log the shows watched.
3. The Zephry system should log the amount of time each show is watched.
4. The Zephry system should handle ending the session gracefully.

## Database Design

### Full System 
ERD - https://drive.google.com/file/d/1E9FZYTcrmXXyHGFVdGMhGa6XWK0NyF9V/view?usp=sharing

### Front Door Camera
ERD - https://drive.google.com/file/d/1D54ridRq3nqnebNRgT4SLS5RFa8G-JrE/view

### Lights Controller
ERD - https://drive.google.com/file/d/1chGReG3twRqIuH7UxfmMHUwRtvx66AjE/view?usp=sharing

### Television Time Restriction
ERD - https://drive.google.com/file/d/1m5J6BBzVJmYVEjZuhqa1mVOAtMoAqAtq/view?usp=sharing

## Files

### Stub Files
#### Light Control
https://github.com/dosterz97/zephyr/blob/master/builds/Lights.py

### User Interface Files

#### Front Door UI 
1. A container with the current status of the front door and a thumbnail of the outside view.
2. A table holding previous records that include a timestamp of an event and the corresponding photo taken at the time.

#### Light Control UI
1. Selection menu of the rooms with light connected to the system.
2. When a light is selected options useable by the light will show up to edit.
3. A confirmation window will pop up after the change is sent with either a sucess or error message.

#### Television Time Restriction
1. Login screen for admin (Similar sign up screen)
2. A screen with all connected devices, and all devices trying to pair with the system. 
3. After selecting a device, you can select accounts on the televisions to pull up the restriction page
4. The restrictions page will have different fields that will allow the admin to create different types of rules

### Model Files

#### Front Door Model
1. Photo: A photo is taken and saved along the time it was taken.
2. Status: The status of the front door is recorded and updated for every event.

#### Light Control Model
1. Form layout with the information used to edit the light.
2. current status information of the lights will be stored so the user can see what the current state is before changing.
3. The privilege attached to the account will be checked with before editing the light so an unauthorized user can't change lights they aren't allowed to manipulate.

#### Television Time Restriction
1. Account - Used to keep track of different users and privilege
2. Restriction - Tied weakly to the Accounts they are applied to
3. Television - Tv name and id that are connected to the Zephyr system, so that different restrictions can be put on different devices
4. Show - This model is used by the restrictions for content monitoring (I.e R rated movies not allowed for 12 year olds)

### Controller Files

#### Front Door Controller
1. Sensor functions
* movement_listen(): the system activates actively listens for movement, which triggers the next function.
* activate_camera(): the system proceeds to activate the camera.

2. Camera functions
* sensor_listen(): the camera is only active if the sensor has been activated.
* take_photo(): a photo is taken and saved into the database.
* sleep(): the camera sleeps after a certain amount of time after there is no longer movement detected until awoken again.

3. App functions
* stream_surveillance(): opens a connection between the camera and app to allow the user to see outside the front door.

#### Light Control Controller
1. Edit Functions
* editLight(): The system will apply the recieved for info from the user to the selected light.
* changeColor(): Edits the RGB vaule of the light.
* changeBrightness(): Edits the brightness of the light.
* changeState(): Turns the light on/off.
* editPrivilege(): Lets an Admin change the access to the light.

2. Check Functions
* checkLight(): Gets the current status of the light to update to the server.
* checkColor(): Grabs the RGB vaule of the light.
* checkBrightness(): Grabs the brightness of the light.
* checkState(): Checks if the light is on/off.
* checkPrivilege(): Will check who has access to changing the light.

3. Update Functions
* updateLight(): Will update the status of the light everytime it is changed (Both with app and manually).
* listenLight(): Will check if the light is turned off manually.
* serverConnect(): Will connect the update function to the server to send the values.

4. Error Functions
* failError(): Loads the error message if the edit fails.
* success(): Loads the success message when the light changes successfully.

#### Television Time Restriction
1. Admin Methods
* createAccount(): Creates an account for a userName password etc. This will allow users to login and use the television(s)
* createRestriction(): Creates a restriction that can be reused and applied to multiple accounts
* editAccount(): allows for changing of passwords, permissions, and restrictions.
* editRestriction(): also accessable from the edit account view, used to update fields of the restriction
* viewUsage(): shows analytics of users and the content that were viewing, times of day etc.

2. User Methods
* login(): logs the usres into the account
* changePassword(): if permissions allow, let users change the password for their account

3. Television Methods
* userDidLogin(): starts tracking the data of the user
* userDidLogout(): stops tracking the data of the user
* notifyUser(): if a violation occurs or a time limit is getting reached the zephry system will show the message
* goHome(): if a violation occurs then the television will go back to the home screen 

### Languages

1. Python
* Python is the main language used for this project. Python is used to access the database server and controlling a hardware device.
2. Skill gaps
* Jon: no experience coding in Python
* Everyone: no experience connecting to devices with Bluetooth and hardware in general
* Zach: experience with cmake files and in general larger scale low-level development
* Issue will be integrating and bridging with c code when necessary
3. Django 
* Not a language, but we will use this to for DB and model template
