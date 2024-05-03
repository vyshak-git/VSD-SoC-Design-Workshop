# VSD-SoC-Design-Workshop

#### *_Author:_* <a href="https://www.linkedin.com/in/vyshak-shetty-a00b08212/">Vyshak Shetty</a>

## Day 1: Inception of open-source EDA, OpenLANE, Sky130 PDK
### LABS
##### 1. OpenLANE Directory Structure
   the /tools directory consists of all the tools that are going to be used in the workshop. The work will be conducted in the /openlane_working_directory
   
![Screenshot from 2024-05-01 12-42-50](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/b5b7f808-7de9-4ee4-847f-67952165ddc4)

/openlane_working_directory consists of /openlane where we will be invoking the tool and /pdks which consists the libraries, lef, etc for the specific pdks
For this workshop we are using the Google Skywater 130nm PDK.

![Screenshot from 2024-05-01 12-44-46](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/9f05968a-4cec-42b6-acee-785b900e086a)
![Screenshot from 2024-05-01 12-44-46](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/55ec8ec0-f7bf-46f6-9c02-df9d1d0022a2)
![Screenshot from 2024-05-01 12-49-14](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/3db66ec5-cdf7-405b-a4f7-b9a5de59f951)

the /openlane directory consists of designs, flow scripts, etc
![Screenshot from 2024-05-01 12-49-50](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/f4bf879d-6140-431d-a30e-435e569c8be8)

/designs consists of all the various design files. If we are making a custom design then we can add the files in the same directory. If we furthur explore a design then we can find the config files, verilog files and the constraint files(SDC).
![Screenshot from 2024-05-01 12-50-27](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/72487c16-9dec-47c6-93db-9b244e9a8961)
![Screenshot from 2024-05-01 12-50-51](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/7dfe92d1-7119-4e0b-9cec-7a720cae25f4)
![Screenshot from 2024-05-01 12-51-26](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/0b1bd9b5-2c43-4d58-bffb-27b447524ba8)

##### 2. Running OpenLANE
To run OpenLANE, we launch the docker by entering "docker" in the CLI. This enters the bash mode. here we run the flow.tcl script in the interactive mode. we run it interactively as we want it to run step by step and not the entire flow in one go. 
![Screenshot from 2024-05-01 12-58-23](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/f9f96eae-0081-4007-a496-09293c77f7f8)

##### 3. Design Preparation
The first step of the flow is design preparation. the various files like the RTL, constraints, timing libraries, LEF, technology files need to be read and linked, This step is design preparation. <br>
Commands:<br>
%prep -design picorv32a <br>
![Screenshot from 2024-05-01 12-59-21](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/61e648b9-2a71-4db6-a0d6-3a6e5e47d8c8)

##### 4. Synthesis
Synthesis is the stage that converts the RTL into gate-level netlist. <br>
Commands: <br>
%run_synthesis <br>
After synthesis is completed, we get a stats on the number of cells, the area consumed by the cells and the timing slack details.
![Screenshot from 2024-05-01 13-02-40](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/b217f951-5a57-4b39-9965-17dc2d2a4951)
![Screenshot from 2024-05-01 13-02-52](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/9ed9f9d9-1b58-4f8e-a9f6-c0ced559c6ff)
![Screenshot from 2024-05-01 13-03-02](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/e5b20604-fa74-4a5e-973e-33da3325b8c5)

##### 5. Output files
Each run generates few output files. These are reports, logs, results etc. The output files are present in the /design directory itself. Choose a specific design and in that design directory and /run directiory is prepared. If we explore it then we can find the various output files.
![Screenshot from 2024-05-01 13-04-00](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/be34af7f-5fb0-412b-adc8-f6e89ddbd958)
![Screenshot from 2024-05-01 13-04-24](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/97ba7f62-099d-4995-9f5f-724f4a4b3689)

## Day 2: Good Floorplan vs Bad Floorplan and introduction to library cells
### LABS
##### 1. Running floorplan
Before running floorplan, first we must set the nescessary values in the config files. Things like utilization ratio, vertical and horizontal metal layers etc.
![Screenshot from 2024-05-01 15-59-14](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/01480614-fcc7-4503-a166-2e17a5b902c3)
![Screenshot from 2024-05-01 16-41-58](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/6e7c86c5-e7d9-4cc2-ab92-b853d8f4f4bd)

Once that is done we can run floorplan using the command: <br>
run_floorplan <br>
this generates a floorplan def as output. We can view the floorplan using the magic layout tool by giving the following command.
![Screenshot from 2024-05-01 16-48-01](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/8586bca7-1807-44a7-b25b-26cca907e5db)
here -T is used to specify the tech file location, "lef read" specifies the merged lef location and "def read" specifies the def file location. After running the command, the magic window opens.
![Screenshot from 2024-05-01 16-48-37](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/9a56403f-d856-4f75-bf6d-e24eebce44ef)

We can see the Decoupling capacitors on the edges and the tap cells aligned in a column.
![Screenshot from 2024-05-01 16-50-19](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/f46757cc-d7c2-4c88-8544-4c6efd31560f)

the tkcon window can be used to find details of the selected cell or pad.
![Screenshot from 2024-05-01 16-54-57](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/ebee4110-b199-4a38-9013-0a76a98c916f)

The standard cells are also present in the floorplan in the left bottom but they are not placed yet as placement stage is still not done.
![Screenshot from 2024-05-01 16-56-24](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/72586299-e862-4343-a914-9b347017d865)

##### 2. Running placement
To run placement we use the following command: <br>
run_placement <br>
The placement stats are generated.
![Screenshot from 2024-05-01 18-03-54](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/159f70ef-505e-42a6-a7d9-c66bc23250f1)

The placement def is geherated in the /runs directory. We can check the layout using magic the same way we chacked floorplan layout but changing the def file to the placement.def.
![Screenshot from 2024-05-01 18-05-55](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/3604b0c6-a286-4cf1-bdab-0f2a236b6c07)

If we zoom-in then we can see the various standard cells that have been placed.
![Screenshot from 2024-05-01 18-06-21](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/dfc4a15b-592a-45be-8827-9dd31f5bac85)

## Day 3: Design library cell using Magic Layout and ngspice characterization
### LABS
##### 1. Custom inverter
We first clone the git repository from VSD into our system which contains of a custom inverter layout mag file along with the skywater library files. 
![Screenshot from 2024-05-03 12-17-13](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/53b71906-6591-479c-aa2f-2b12220d3433)
![Screenshot from 2024-05-03 12-18-08](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/10bf730e-5777-47ed-90c5-a157399c8575)

To open the sky130_inv.mag, we use Magic tool just like we used to open the floorplan files.
Command: <br>
magic -T sky130A.tech sky130_inv.mag & <br>
This opens the layout of the customm inverter.
![Screenshot from 2024-05-03 12-11-25](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/ac4a7e9e-3c0d-4d3c-b23b-0c58138ce665)

We can explore the design and check the different parts of the inverter by placing the cursor at a region and pressing 'S'. This selects that region. Then we go to the tkcon window and type "what". THis brings up the details of the region.
![Screenshot from 2024-05-03 16-28-58](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/a8a28787-f773-4fda-a369-9fd64fdbb1f3)
As we can see it is an Nwell region.

##### 2. SPICE Deck
Before proceeding with the spice simultion, we must extract the layout file. This creates an extraction file. This can be done by "extract all" command in tkcon.
![Screenshot from 2024-05-03 16-47-54](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/09fbab46-1cae-4af4-96b9-2016d51ab4bb)

After this we convert this extraction into a spice model using "ext2spice" command. cthresh and rthresh extracts the parascitic information too.
![Screenshot from 2024-05-03 16-48-42](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/ae6e1bb2-2f39-4215-b8ec-885f0965deb6)
![Screenshot from 2024-05-03 16-49-05](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/1b0beb7f-cc7b-4791-8881-2df95d49c2a2)
We can see two new files have been created. a ".ext" file and a ".spice" file.

If we open the spice file we can see the spice model that has been extracted from the layout.
![Screenshot from 2024-05-03 17-11-04](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/75f0211b-f84d-47a7-adea-8e22fbc87160)
Lets make a few changes to the spice file. We iinclude the pshort and nshort library files which contain the info of the pmos and nmos. Accordingly we have to chnage the name of the pmos and nmos as per the library files. We define the VDD AND VSS signals. We have given a voltage of 3.3V for VDD. We also create a pulse signal that ranges from 0V to 3.3V and has a clock cycle of 4ns with a 50% duty cycle. Then we do perform transient analysis on the model.
![Screenshot from 2024-05-03 17-56-30](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/a88771b3-ee3a-4774-94c0-68ccb894f8ed)

We simulate the spice model using ngspice and plot the input vs output graph.
![Screenshot from 2024-05-03 18-10-41](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/6ca7dd48-4ec4-4135-a2a4-41ed03aa1b24)

##### 3. Calculating input rise time
Input rise time can be defined as the time taken for the input to rise from 20% to 80% of its value.
In our model the peak voltage is 3.3V.
20% of 3.3V = 0.66V
80% of 3.3V = 2.64V
Lets find the points on the graph and calculate the rise time.
![Screenshot from 2024-05-03 18-19-51](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/46e5c9fd-b202-40ec-90bd-891cd71d70e9)
From the above data,
4.08-4.02 = 0.06
So the Input rise time is **0.06ns**.

##### 4. Calculating output fall time
Output fall time is defined as the time it takes for the output to fall from 80% of its peak value to 20%.
so it is time taken for signal to go from 2.64V to 0.66V.
Lets use the graph to find the values.
![Screenshot from 2024-05-03 18-26-09](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/89c8f45e-c86f-4ba2-bfe9-270ccea09a71)
4.09-4.05 = 0.04
So the Output fall time is **0.04ns**.

##### 5. Calculating Rise propagation delay
Propagation delay is calculated between the 50% of the values between input and the output. Lets check the time it takes for both input and output signal to reach 50% of its values and subtract it using the graph.
50% of 3.3V is 1.65V
![Screenshot from 2024-05-03 18-30-21](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/27e9d2aa-277e-49a5-ba53-d02d4a28839e)
We click the 2 points and the values get displayed in the terminal.
![Screenshot from 2024-05-03 18-31-36](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/476a3867-8e90-43b4-aa7b-6fc45df7c679)
The propagation delay is very small. Around 0.00007ns.


