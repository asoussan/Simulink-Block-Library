# Simulink-Block-Library
3 Simulink blocks - wifi initialization, infrared sensor reading, wheel velocity control

Before doing anything, download the "Matlab Toolbox for the iRobot Create 2" add-on from Mathworks.com and add it to your path. A single chart and one function exists for each block for simplicity.

--- wifi init --
This block has one output, which assigns a TCPIP serial object to the other two blocks. The coder.extrinsic command is used to set up RoombaWifiInit as a persistent object. Using the assignin command, the object is passed to a global variable 'objWs' before going to 'base'. A "dummy" sink is connected so that an output can be connected to the library block.

--- ir sensors ---
The block used for reading from the 6 IR sensors located on the front bumper of the Roomba has one input and one output. The input 'data' is sent from the wifi init block. The output is a 1x6 vector representing the six sensors on the front of the Roomba. These values can be either 0 or 1. They are initialized to 0 within the function. Zero will be the output if there is nothing blocking the sensor and one will be the output if something is blocking the sensor.

Coder.extrinsic is implemented in order for us to use evalin, get, and RangeStateRoomba(from add-on). If the object is empty, it passes on the TCPIP object from the previous block.

--- wheel velocity control ---
This block is used to control the speed of the left and right motor. Three inputs and no outputs exists in this block. The first input is the TCPIP object from the wifi init block. Two other inputs are constants that represent the wheel speed from -0.5 to 0.5 (negative is reverse). Similarly to the last block, the function checks if the object is empty and passes the TCPIP from the previous block with the workspace variable 'objWs'. Again, coder.extrinsic is needed to implement evalin, get, and SetWheelVelRoomba(from add-on). SetWheelVelRoomba does not need to be set to a variable like RangeStateRoomba since this block contains no output data.


A test case is provided with two constants 0.3 set as wheel speed and a display connected to the output of the IR sensing block.
