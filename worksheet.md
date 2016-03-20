# Minecraft Photobooth

Create a working photobooth in Minecraft: when the player enters the photobooth it triggers the camera module and takes their picture. Don't forget to smile!

Please note that, before starting this activity, you must ensure that your camera module is attached to the Raspberry Pi and enabled in the settings.  This is covered in this tutorial.  Once you have connected the camera, boot up the Raspberry Pi.

## Importing the Minecraft and PiCamera Modules

The first part of the program is to import the Minecraft API (Application Programming Interface). This enables you to connect to Minecraft and use Python to code. This will import the PiCamera module to control the camera and the time module to add a small delay between taking the photo and then taking the next photo.

1. Open Minecraft from the application menu, and enter or create a new world.

1. Move the Minecraft window to one side of the screen.

    You'll need to use the `Tab` key to take your mouse's focus away from the Minecraft window to move it. You'll need this later when you switch between the Minecraft and Python windows.

1. Open Python 3 from the applications menu:

    ![Open Python 3](images/python3-app-menu.png)

    This will open up the Python IDLE code editor which you will use to write the photobooth program.

1. Click `New > Window` to open a new window.

1. Enter the code shown below:

	``` python
	from mcpi.minecraft import Minecraft
	from picamera import PiCamera
	from time import sleep

	mc = Minecraft.create()
    camera = PiCamera()

	mc.postToChat("Hello world")
	```

1. Save with `Ctrl + S` and run the program with `F5`. You should see the message "Hello world" appear in the Minecraft world.

## Testing the PiCamera

Next, you'll write some code to test the camera.

1. Add the following lines to the end of your program:

    ``` python
	camera.start_preview()
	sleep(2)
	camera.capture('/home/pi/selfie.jpg')
	camera.stop_preview()
    ```

    We have set the camera to show a two second preview so that you can strike your pose and smile before the picture is taken. The image is stored as a file called `selfie.jpg` in your home directory.

1. Save (`Ctrl + S`) and run (`F5`) your program to take a picture!

1. Open the File Manager to view the picture you took.

## Building a Photobooth

Now we need to create a photobooth in the Minecraft environment, this is done manually and can be built whereever you want to locate it.

Using any block type, create a Photobooth. This can be any shape but should have at least one block width of free space inside so that the player can enter - like a door or gate.

![Photobooth](images/photobooth.png)

Once you have created your photobooth, move your player inside and onto the trigger block. This is the block that the player stands on to run the function that you wrote in step 1. This block will trigger the camera. In the Minecraft environment your position is measured with the `x`, `y` and `z` axis. Look at the top right of the window and you will see the `x`, `y`, `z` co-ordinates of your player, for example `10.5`, `9.0`, `-44.3`. Assuming you are still in the photobooth then these are also the `x`, `y`, `z` co-ordinates of the 'trigger' block in your photobooth.

1. Walk into your photobooth

1. Record the `x`, `y`, `z`, co-ordinates of your PiCamera 'trigger' block.

## Finding where I am?

When you are playing Minecraft your program will need to verify that you are inside the photobooth - if you are then it will take a picture with the camera. To do this Minecraft needs to know where you are in the world.

To find your position you use the code, `x, y, x = mc.player.getPos()`.  This saves the `x`, `y` and `z` position of your player into the variables `x`, `y` and `z`.  You can then use `print(x)` to print the `x` value, or `print(x, y, z)` to see them all. Now you now the position of the player you can test to see if they are in the photobooth. As we're using Minecraft we can also use `mc.postToChat` to send the coordinates to the Minecraft window.

1. Change the message "Hello world" to "Find the photobooth" after the `Minecraft.create()` line like so:

    ```python
    mc = Minecraft.create()

    mc.postToChat("Find the photobooth")
    ```

1. Add to the end of your code:

    ``` python
	while True:
		x, y, z, = mc.player.getPos()
		mc.postToChat(x, y, z)
    ```

1. Save and run the code and you'll see your coordinates posted to the Minecraft window.

1. Focus on the Python window and press `Ctrl + C` to stop the code running.

## Testing that you are in the Photobooth

At this point we have a photobooth, the coordinates of the trigger block, and a function to control the PiCamera and take a picture. The next step is to test the program identifies when you are in the photobooth. To do this we must create a loop which checks if your player's co-ordinates match the trigger block coordinates. If they do, then you are standing in the photobooth. To do this we use a simple `if` statement, we call these conditionals.

1. Change the lines inside your `while` loop to match the code below:

    ```python
    while True:
        x, y, z = mc.player.getPos()

    	sleep(3)

    	if x >= 10.5 and y == 9.0 and z == -44.3:
    	    mc.postToChat("You are in the photobooth!")
    ```

    Ensure the coordinates you enter are the location of your own photobooth.

1. Save and run your code to test it - walk into your photobooth and you should see the message "You are in the photobooth" in the Minecraft window.

You will note that the `if` statement checks if `x` value is greater than or equal to `10.5`, this is to ensure that it picks up the block as it could have a value of `10.6`. Remember to replace the `x`, `y` and `z` values with the values from your photobooth.

## Putting it all together

Now you have a working photobooth we need to add the PiCamera to take a picture. We will add a quick reminder to smile and then call the PiCamera `capture()` function.

1. Add the instructions to send messages to Minecraft and the call to `capture()` inside your `if` statement:

    ```python
    if x >= 10.5 and y == 9.0 and z == -44.3:
        mc.postToChat("You are in the Photobooth!")
        sleep(1)
        mc.postToChat("Smile!")
        sleep(1)
        camera.start_preview()
        sleep(2)
        camera.capture('/home/pi/selfie.jpg')
        camera.stop_preview()

    sleep(3)
    ```

    This will keep checking your location, and when it matches the location of the photobooth it will take a picture with the camera.

1. Now your your code and walk into the photobooth. It should open the camera preview and you have 2 seconds to pose before it takes your picture!

## What next?

1. Try adding a specific block which triggers the camera

1. Try adding further blocks to control PiCamera settings such as taking a video or applying a filter

1. Try using blocks to start and stop the PiCamera recording a video
