# Running The UR3 Scan N Plan Demo

## Starting Up

1. Be sure that all cables are connected between systems
   - UR controller to wall
   - Latching power cable from UR3 to UR controller 
   - Ethernet cable from underneath UR controller to computer right port (I think)
   - USB cable from camera on UR3 to computer
2. Press power button on UR Teach Pendant to turn it on
3. Turn on computer with small power button on the right side
   - Password on boot is the number on the side of the computer
4. Go to initialization screen on the UR and turn on with a 1 Kg payload
5. Click `Run Program`
6. Click `file->load program`->`Automate23.urp`->`open`
7. On computer click on the ROS1 application on the left bar (just 9 dots)
8. Click on the bridge application on the computer
9. Wait 5-10 seconds
10. Press the play button on the UR Teach Pendant
11. Press the ROS2 application on the computer
12. You should see an RVIZ window pop up with a UR3 sitting on a table in the same position as the hardware in real life
    - If you don't see this then go to the **Troubleshooting** section 

## Running the Application

1. Place the object to be scanned in front of the UR3
2. Click `Get Detailed Scan` on the left side, (these buttons on the left are the main sequence of buttons you'll press)
   - The UR3 should start moving to do a scan of the workspace
   - It'll do multiple passes over the part and then go back to the original home position
   - While scanning RVIZ should slowly start to populate the mesh
   - After a completed scan you should see the completed part
3. Use the mouse to click around on RVIZ with the mouse to navigate the camera looking at the top of the scanned part
   - Left Click: rotates the screen around a yellow disk (your focal point)
   - Mouse Wheel Click(or shift+left click): moves the yellow disk around
   - Scroll: zooms in and out
   - Pro-tip: if you hover over an object and press `f` on the keyboard it'll change your focal point to the geometry underneath your mouse
4. Click on the `Polygon Selection Tool` button at the top
   - This will change what your mouse does
   - Left Click: Draws a line on the object immediately under the mouse
     - Be careful, it is easy to accidentally draw on something other than the scanned part
     - You can just click rather than holding it down to draw cleaner boundaries
   - Mouse Wheel Click: Clears your drawing
   - Right Click: Exits `Polygon Selection Tool` back to normal RVIZ interaction
5. Draw a region on the scanned part where you want to buff/erase
6. Exit the `Polygon Selection Tool`
7. Click `Generate Tool Path Plan`
   - You should see a bunch a tiny coordinate frames pop-up inside the area you drew on, these are the waypoints for planning
8. Click `Generate Motion Plan`
   - After several seconds you'll see the text in the bottom left say 
   ```
   ...
   perform motion planning completed!
   Waiting to execute prcoess motion...
   ```
   - One of the terminals will also go crazy with a bunch of text output at this time and stop with a success or fail after motion planning is completed
9. (optional-but-recommended) Preview motion plan
   1. Click on the `TesseractWorkbench` tab in the bottom left of the RVIZ application
   2. Click on the `Trajectory` tab at the top of the new panel on the left side of RVIZ
   3. Double-click on the `general` drop down on the right side of the new panel
   4. Click on `Trajectory Set`
   5. Press play at the bottom of this panel to watch a preview of the motion
   6. Once satisfied click back on the `SNPApplication` tab at the bottom
10. Click `Execute Motion Plan`
    - The UR should move through the planned motion and erase/buff the selected region

### Options for next steps
1. Execute the same motion again by pressing `Execute Motion Plan`
2. Go back to step 4 to redraw your process area.
3. Replace or move the part and restart back at step 1. 

## Troubleshooting
*If any of these troubleshooting process don't work call Tyler or Michael 

### Restarting the software
- Press the stop square on the UR teach pendant (if it is not already grey)
- There should be 3 terminals running software on the computer
- Type `ctrl+c` in all of them to kill them 
  - if they don't go away after a few seconds just close them

### I turned everything on, but the robot in RVIZ doesn't match real life
You are probably experiencing some form of communication error with the robot
- Ensure you have an ethernet connection between the computer and the UR
- If you already did then try switching to a different port on the computer
  - If you unplug it then you will need to restart all of your applications
  - Execute the steps in **Restarting the software**
  - Restart from step 7 of the **Starting Up** Section
- If you want to confirm communication with the UR3
  - Open a terminal from the sidebar (or press `ctrl+alt+t`)
  - Type `ping 192.168.1.3`
  - You to see something that looks like:
  ```
  64 bytes from 192.168.1.3: icmp_seq=2 ttl=114 time=0.59 ms
  64 bytes from 192.168.1.3: icmp_seq=2 ttl=114 time=0.63 ms
  ```
  - If you don't then try switching the ethernet ports and re-running the ping command.
  - If you don't see a connection from either port then go to the next step
- If you want to establish connection with the UR3 with the current ethernet port
  - Click in the very top right of your computer monitor and a dropdown should appear
  - Click on settings
  - Go to Network
  - You should see some sort of connection with a settings wheel next to it on the right. Click that wheel
  - Click on the Ipv4 tab
  - Set Ipv4 Method to `Manual`
  - Set Address to `192.168.1.17`
  - Set Netmask to `255.255.255.0`
  - Leave everything else blank and click apply
  - Retry pinging the UR3
    - Sometimes you have to turn the connection on and off again after updating the settings for it to take effect
- If somehow the UR settings got messed up. 
  - Navigate on the teach pendant to set the IP address to `192.168.1.3`
  - In the `Automate23.urp` teach pendant program ensure it is running the remote control action
  - Set the expected incoming (I don't remember the exact name) IP address to `192.168.1.17`

### The robot isn't moving at all
Ensure the robot is in the right spot, if not then go to **I turned everything on but the robot I see in RVIZ doesn't match real life**
If the robot is in the right spot, but it still isn't moving when it should try the following
- Ensure the program on the teach pendant is running
  - If it is not press play and then try again
  - If still does not work then execute the steps from **Restarting the software** and restart from step 7 of the **Starting Up** section
- Ensure the program running on the teach pendant is `Automate23.urp`
  - If not execute the steps from **Restarting the software** and restart from step 6 of the **Starting Up** section
- If all else fails then execute **Restarting the software** and restart from step 5 of the **Starting Up** section

### The robot started the scan trajectory and then just stopped
- Try clicking the `Get Detailed Scan` button again
- If that doesn't work Call Michael as he's dealt with this before

### The robot finished the scan process, but I don't see my part
- Check to make sure you are getting data from the camera
  - On RVIZ click on the Displays tab in the bottom left
  - There should be a display that says `PointCloud2` with an unchecked box to the right of it
  - Click the box and reposition your camera in RVIZ to see where the area the robot is looking at
  - You should see a point cloud show up in RVIZ
  - Wave your hand in front of the camera if it is just looking off into space (it can't see very far)
- If no point cloud shows up in RVIZ make sure the camera is plugged in
- You may need to switch USB ports, it requires a USB3 connection. 
  - I think usually the top left one on the computer works well
- If still no point cloud is showing up 
  - Do **Restarting the software**
  - Open a terminal by clicking on the side-panel or pressing `ctrl+alt+t`
  - Type `realsense-viewer` to bring up the realsense interactive application
  - If the camera is connected you'll see it on the left side of your screen and you should be able to turn it on and turn on 3D data to see what it sees
- The point cloud is showing up, but I still don't get a mesh
  - Ensure the part is presented in front of the robot on the large flat surface of the robot stand

### The robot finished scanning and the mesh looks terrible
- This is not uncommon with realsense d405 because of how it works
  - The d405 just has two cameras that try and perform optical correspondence between the images, unlike the d435 or d455 which use a projected IR pattern
  - This means **smooth solid color objects are basically invisible to the camera**
  - Use the marker to mark up the surface
- Try repositioning or rotating the object
- Try a different object to scan
  - If every object still looks terrible call Tyler or Michael

### When I click `Generate Tool Path` nothing shows up
- This often happens if the region selected is too small
  - You might see something in the application logs on the left that says `No clusters found`
  - Try making a bigger region for the toolpaths (see **Running the Application** step 4 for details on how to reselect a region)
- Ensure there is an application running on the left called `snp_tpp_app`. It will likely have an invisible logo, but should have an orange dot next to it and display the name when you hover over it
  - If you don't see this application then something happened that caused it to die
  - Restart everything by executing **Restarting the software** and then starting from step 7 of **Starting up**

### When I click `Generate Tool Path` there are waypoints all over the place or they just look messy
- Be careful with the region you've drawn
  - The toolpath planner extracts the points inside the drawn region, but can potentially get multiple layers
  - If your mesh has multiple surfaces in it then try either:
    - Or selecting go back to **Running the Application** step 4 and select a different region on the mesh
    - Moving the object and rescanning

### Motion planning is failing
- Make sure all the waypoints you're trying to have it reach look reasonably reachable
- Make sure the mesh doesn't have spurious points that might cause collision issues
  - The program puts a convex hull around the object internally and uses that convex hull for collision checking so an extra triangle out in space can be very problematic
- Try repeating **Running the Application** step 4 and regenerate a new toolpath that might be easier to reach

### The robot will execute scans but won't execute the erasing/buffing motions
- Make sure a successful motion plan has been planned
- Go do the optional step 9 from **Running the Application** and verify that the most recent trajectory (the bottom one on the list) matches the expected behavior
- If a motion preview that matches exists and it still won't execute then
  - Do **Restarting the software**
  - Restart at step 7. on the **Starting Up** section

### The robot was protective or E-stopped
- Clear the stop
- Do **Restarting the software**
- Start from step 7 or **Starting Up**