# VSD-SoC-Design-Workshop

#### *_Author:_* <a href="https://www.linkedin.com/in/vyshak-shetty-a00b08212/">Vyshak Shetty</a>

## Day 1: Inception of open-source EDA, OpenLANE, Sky130 PDK
### LABS
##### 1. OpenLANE Directory Structure
   the `/tools` directory consists of all the tools that are going to be used in the workshop. The work will be conducted in the `/openlane_working_directory`
   
![Screenshot from 2024-05-01 12-42-50](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/b5b7f808-7de9-4ee4-847f-67952165ddc4)

`/openlane_working_directory` consists of `/openlane` where we will be invoking the tool and `/pdks` which consists the libraries, lef, etc for the specific pdks
For this workshop we are using the Google Skywater 130nm PDK.

![Screenshot from 2024-05-01 12-44-46](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/9f05968a-4cec-42b6-acee-785b900e086a)
![Screenshot from 2024-05-01 12-44-46](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/55ec8ec0-f7bf-46f6-9c02-df9d1d0022a2)
![Screenshot from 2024-05-01 12-49-14](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/3db66ec5-cdf7-405b-a4f7-b9a5de59f951)

the `/openlane` directory consists of designs, flow scripts, etc
![Screenshot from 2024-05-01 12-49-50](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/f4bf879d-6140-431d-a30e-435e569c8be8)

`/designs` consists of all the various design files. If we are making a custom design then we can add the files in the same directory. If we furthur explore a design then we can find the config files, verilog files and the constraint files(SDC).
![Screenshot from 2024-05-01 12-50-27](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/72487c16-9dec-47c6-93db-9b244e9a8961)
![Screenshot from 2024-05-01 12-50-51](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/7dfe92d1-7119-4e0b-9cec-7a720cae25f4)
![Screenshot from 2024-05-01 12-51-26](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/0b1bd9b5-2c43-4d58-bffb-27b447524ba8)

##### 2. Running OpenLANE
To run OpenLANE, we launch the docker by entering `docker` in the CLI. This enters the bash mode. here we run the `flow.tcl` script in the interactive mode. we run it interactively as we want it to run step by step and not the entire flow in one go. 
![Screenshot from 2024-05-01 12-58-23](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/f9f96eae-0081-4007-a496-09293c77f7f8)

##### 3. Design Preparation
The first step of the flow is design preparation. the various files like the RTL, constraints, timing libraries, LEF, technology files need to be read and linked, This step is design preparation. <br>
Commands:<br>
```bash
prep -design picorv32a
```
![Screenshot from 2024-05-01 12-59-21](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/61e648b9-2a71-4db6-a0d6-3a6e5e47d8c8)

##### 4. Synthesis
Synthesis is the stage that converts the RTL into gate-level netlist. <br>
Commands: <br>
```bash
run_synthesis
```
After synthesis is completed, we get a stats on the number of cells, the area consumed by the cells and the timing slack details. <br>
![Screenshot from 2024-05-01 13-02-40](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/b217f951-5a57-4b39-9965-17dc2d2a4951)
![Screenshot from 2024-05-01 13-02-52](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/9ed9f9d9-1b58-4f8e-a9f6-c0ced559c6ff)
![Screenshot from 2024-05-01 13-03-02](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/e5b20604-fa74-4a5e-973e-33da3325b8c5)

##### 5. Output files
Each run generates few output files. These are reports, logs, results etc. The output files are present in the `/design` directory itself. Choose a specific design and in that design directory and `/runs` directiory is prepared. If we explore it then we can find the various output files.
![Screenshot from 2024-05-01 13-04-00](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/be34af7f-5fb0-412b-adc8-f6e89ddbd958)
![Screenshot from 2024-05-01 13-04-24](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/97ba7f62-099d-4995-9f5f-724f4a4b3689)

##### Assignment
Let us calculate the flop ratio by using the synthesis stats. <br>
Flop Ratio = No. of D flip-flops/Total no. of cells <br>
**Note:** The below report used is after the addition of a custom inverter LEF which will be covered later on in the repo. That is the reason for the changed values compared to the stats above. <br>
![Screenshot from 2024-05-15 19-57-48](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/8b25c97f-9041-477d-b21c-77988e51a6ce) <br>

As we can see from the above report,
```bash
Total no. of cells = 18584
No. of D Flip-Flops = 1613
Flop Ratio = 1613/18584
           = 0.0868
           = 8.68%
```
## Day 2: Good Floorplan vs Bad Floorplan and introduction to library cells
### LABS
##### 1. Running floorplan
Before running floorplan, first we must set the nescessary values in the config files. Things like utilization ratio, vertical and horizontal metal layers etc.
![Screenshot from 2024-05-01 15-59-14](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/01480614-fcc7-4503-a166-2e17a5b902c3)
![Screenshot from 2024-05-01 16-41-58](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/6e7c86c5-e7d9-4cc2-ab92-b853d8f4f4bd)

Once that is done we can run floorplan using the command: <br>
```bash
run_floorplan
```
this generates a floorplan def as output. We can view the floorplan using the magic layout tool by giving the following command.
![Screenshot from 2024-05-01 16-48-01](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/8586bca7-1807-44a7-b25b-26cca907e5db)
here -T is used to specify the tech file location, `lef read` specifies the merged lef location and `def read` specifies the def file location. After running the command, the magic window opens.
![Screenshot from 2024-05-01 16-48-37](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/9a56403f-d856-4f75-bf6d-e24eebce44ef)

We can see the Decoupling capacitors on the edges and the tap cells aligned in a column.
![Screenshot from 2024-05-01 16-50-19](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/f46757cc-d7c2-4c88-8544-4c6efd31560f)

the tkcon window can be used to find details of the selected cell or pad.
![Screenshot from 2024-05-01 16-54-57](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/ebee4110-b199-4a38-9013-0a76a98c916f)

The standard cells are also present in the floorplan in the left bottom but they are not placed yet as placement stage is still not done.
![Screenshot from 2024-05-01 16-56-24](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/72586299-e862-4343-a914-9b347017d865)

##### 2. Running placement
To run placement we use the following command: <br>
```bash
run_placement
```
The placement stats are generated. <br>
![Screenshot from 2024-05-01 18-03-54](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/159f70ef-505e-42a6-a7d9-c66bc23250f1)

The placement def is generated in the `/runs` directory. We can check the layout using magic the same way we chacked floorplan layout but changing the def file to the `placement.def`. <br>
![Screenshot from 2024-05-01 18-05-55](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/3604b0c6-a286-4cf1-bdab-0f2a236b6c07)

If we zoom-in then we can see the various standard cells that have been placed. <br>
![Screenshot from 2024-05-01 18-06-21](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/dfc4a15b-592a-45be-8827-9dd31f5bac85)

## Day 3: Design library cell using Magic Layout and ngspice characterization
### LABS
##### 1. Custom inverter
We first clone the git repository from VSD into our system which contains of a custom inverter layout mag file along with the skywater library files. <br>
![Screenshot from 2024-05-03 12-17-13](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/53b71906-6591-479c-aa2f-2b12220d3433)
![Screenshot from 2024-05-03 12-18-08](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/10bf730e-5777-47ed-90c5-a157399c8575)

To open the `sky130_inv.mag`, we use Magic tool just like we used to open the floorplan files.
Command: <br>
```bash
magic -T sky130A.tech sky130_inv.mag &
```
This opens the layout of the custom inverter. <br>
![Screenshot from 2024-05-03 12-11-25](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/ac4a7e9e-3c0d-4d3c-b23b-0c58138ce665)

We can explore the design and check the different parts of the inverter by placing the cursor at a region and pressing 'S'. This selects that region. Then we go to the tkcon window and type `what`. THis brings up the details of the region. <br>
![Screenshot from 2024-05-03 16-28-58](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/a8a28787-f773-4fda-a369-9fd64fdbb1f3) <br>
As we can see it is an Nwell region.

##### 2. SPICE Deck
Before proceeding with the spice simultion, we must extract the layout file. This creates an extraction file. This can be done by `extract all` command in tkcon. <br>
![Screenshot from 2024-05-03 16-47-54](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/09fbab46-1cae-4af4-96b9-2016d51ab4bb)

After this we convert this extraction into a spice model using "ext2spice" command. cthresh and rthresh extracts the parascitic information too. <br>
![Screenshot from 2024-05-03 16-48-42](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/ae6e1bb2-2f39-4215-b8ec-885f0965deb6)
![Screenshot from 2024-05-03 16-49-05](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/1b0beb7f-cc7b-4791-8881-2df95d49c2a2) <br>
We can see two new files have been created. a `.ext` file and a `.spice` file.

If we open the spice file we can see the spice model that has been extracted from the layout. <br>
![Screenshot from 2024-05-03 17-11-04](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/75f0211b-f84d-47a7-adea-8e22fbc87160) <br>
Lets make a few changes to the spice file. We include the pshort and nshort library files which contain the info of the pmos and nmos. Accordingly we have to change the name of the pmos and nmos as per the library files. We define the VDD AND VSS signals. We have given a voltage of 3.3V for VDD. We also create a pulse signal that ranges from 0V to 3.3V and has a clock cycle of 4ns with a 50% duty cycle. Then we do perform transient analysis on the model. <br>
![Screenshot from 2024-05-03 17-56-30](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/a88771b3-ee3a-4774-94c0-68ccb894f8ed)

We simulate the spice model using ngspice and plot the input vs output graph. <br>
![Screenshot from 2024-05-03 18-10-41](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/6ca7dd48-4ec4-4135-a2a4-41ed03aa1b24)

##### 3. Calculating input rise time
Input rise time can be defined as the time taken for the input to rise from 20% to 80% of its value.
In our model the peak voltage is 3.3V. <br>
20% of 3.3V = 0.66V <br>
80% of 3.3V = 2.64V <br>
Lets find the points on the graph and calculate the rise time. <br>
![Screenshot from 2024-05-03 18-19-51](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/46e5c9fd-b202-40ec-90bd-891cd71d70e9) <br>
From the above data,
4.08-4.02 = 0.06 <br>
So the Input rise time is **0.06ns**.

##### 4. Calculating output fall time
Output fall time is defined as the time it takes for the output to fall from 80% of its peak value to 20%.
so it is time taken for signal to go from 2.64V to 0.66V.<br>
Lets use the graph to find the values. <br>
![Screenshot from 2024-05-03 18-26-09](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/89c8f45e-c86f-4ba2-bfe9-270ccea09a71) <br>
4.09-4.05 = 0.04 <br>
So the Output fall time is **0.04ns**.

##### 5. Calculating Rise propagation delay
Propagation delay is calculated between the 50% of the values between input and the output. Lets check the time it takes for both input and output signal to reach 50% of its values and subtract it using the graph.
50% of 3.3V is 1.65V <br>
![Screenshot from 2024-05-03 18-30-21](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/27e9d2aa-277e-49a5-ba53-d02d4a28839e) <br>
We click the 2 points and the values get displayed in the terminal. <br>
![Screenshot from 2024-05-03 18-31-36](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/476a3867-8e90-43b4-aa7b-6fc45df7c679) <br>
The propagation delay is very small. Around 0.00007ns.

##### 6. Checking DRC errors
To perform this lab we first download a file with many pre-done layouts with DRC errors. 
```bash
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
```
once downloaded, we extract the file into our home directory.
```bash
tar xfz drc_tests.tgz
```
If we look into the directory, <br>
![Screenshot from 2024-05-04 10-35-18](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/a68d63e4-2260-4349-b3ca-0b984804aeb1) <br>
Open Magic
```bash
magic -d XR
```
![Screenshot from 2024-05-04 10-37-49](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/effcaea4-ae01-46a3-ad52-73791bedc512) <br>
Lets open the `met3` file.
It contains the various DRC errors related to metal 3. We can also view the rules on https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html#m3 <br>
![Screenshot from 2024-05-04 10-55-55](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/0880e70d-d3e7-4f08-9a9a-25494da76595)

To view the error we have to select the block using the mouse and run the command in the tkcon window,
`drc why`
This displays the error. Below are 2 examples. <br>
![Screenshot from 2024-05-04 11-02-11](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/9c636c26-25c0-4358-9a2d-3e9e2721a2c4)
![Screenshot from 2024-05-04 11-02-25](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/8ebf8ba5-7642-4c91-a796-fcf8d8e2030c) <br>
![Screenshot from 2024-05-04 11-10-15](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/740d2fd4-3fd5-48b1-bb31-3969815a6485)
![Screenshot from 2024-05-04 11-10-22](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/65c85e08-f4c0-4b69-8fff-8a150acbebf7)

##### 7. Fixing Poly.9 error
Lets open up the `poly.mag` file to check the poly related DRC issues. <br>
![Screenshot from 2024-05-04 11-30-56](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/8eac3bc8-bcb6-43a1-a042-7f8f084e29f1) <br>

The error poly.9 is about the spacing between the poly resistors and the poly or the diff/tap cells. The min spacing should be 480u. Lets see the spacing in the layout. <br>
![Screenshot from 2024-05-04 11-33-20](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/86bc0562-eca7-418d-851a-4e192da2a396)
![Screenshot from 2024-05-04 11-33-51](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/66658bd8-86b8-4e49-a800-37375ba26342)
![Screenshot from 2024-05-04 11-34-55](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/203a079a-95c5-4da0-b5e4-0c187b3b818a) <br>
From the above screenshot we can see that the spacing is 210um which doesn't fulfill the min requirements.
Let us fix this by making few changes in the `sky130A.tech` file. <br>

![Screenshot from 2024-05-04 11-43-13](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/f2210a9d-eddd-4bbc-9b8f-913ed80c0d5a)
![Screenshot from 2024-05-04 11-43-32](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/411ff36e-4adf-429c-8202-0afbd1c767ee) <br>
In the above two screenshots of the tech file we can see that min spacing is defined only between poly resistors and diffusion and N-tap. There is no mention of spacing between poly resistors and poly. So we need to include the statements in both the sections. <br>

![Screenshot from 2024-05-04 11-44-45](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/f8fc4b8c-7502-428e-8b54-d03e0c9eae33)
![Screenshot from 2024-05-04 11-45-40](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/57558a06-79d9-4522-a69f-dfd72094a6e6) <br>

Once written, we save the tech file. then run the following command in the tkcon window,
`tech load sky130A.tech` <br>
![Screenshot from 2024-05-04 11-54-14](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/90d527db-672a-4188-93ab-ab3df32c59fd)

Then we have to check DRC again using the following command,
`drc check` <br>
![Screenshot from 2024-05-04 11-55-08](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/588a3496-2026-475c-b28a-fd07d6cb4772)
![Screenshot from 2024-05-04 11-55-45](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/bfb14b54-02f8-4b88-840b-662d1390c359) <br>
We can see that the spacing has now been applied and hence the DRC has been fixed.

##### 8. Describing DRC as a geometric construct
![Screenshot from 2024-05-07 23-28-00](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/c6bd1cf4-cc93-4a3c-a494-7d81831a2944)
![Screenshot from 2024-05-07 23-28-21](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/54d69aac-59b1-4b57-b840-c678a9d3fb3b)
![Screenshot from 2024-05-07 23-28-48](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/ae485a72-6a36-42b9-8025-c80e4eb62ca7)


## Day 4: Pre-layout timing analysis and importance of good clock tree
### LABS
##### 1. Converting grid info to track info
We first open the track information in the following directory. <br>
![Screenshot from 2024-05-04 17-15-43](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/46fe098b-404c-4acf-aa94-792c7c7d00cd)
![Screenshot from 2024-05-04 17-16-00](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/3ced9a5c-ab0d-40ac-9b26-69faaf2bfc73) <br>
Here the first value depicts the offset value and the second value depicts the pitch of the metal. X means on the x-axis and Y means on the y-axis. so the offset of li1 on x-axis is 0.23um and on y-axis it is 0.17um. The metal pitch for li1 on x-axis is 0.46um and on y-axis it is 0.34um. <br>
Now let us convert our grid lines according to the track information to check if the input and output ports fall at the intersection of the grids. Let us look at the current grid lines, <br>
![Screenshot from 2024-05-04 17-17-33](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/ec135b6e-41a3-4e0f-af7c-84e4664e02b5)

The ports need to fall on the intersection of the grid lines so that they can be connected easily both vertically and horizontally. The width of the standard cell must be in the odd multiples of X pitch. We set the grid values according to the track info file. <br>
![Screenshot from 2024-05-04 17-20-02](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/d46de8a2-c4d9-4c47-b7ce-866a4f5f7134)
![Screenshot from 2024-05-04 17-20-21](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/eab87b80-1a87-40c6-9c53-87b4c80c8ddd) <br>
We can see that the grid boxes have expanded in size and the ports are placed at the intersections.

##### 2. Converting layout to LEF
First let us save the layout with a custom name. Here we give the name `sky130_vsdinv.mag` <br>
![Screenshot from 2024-05-07 13-13-08](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/2e05c24d-a04d-40e6-9f27-75180300edf3)

We extract this design in the lef format using the command, `lef write` <br>
![Screenshot from 2024-05-07 13-13-24](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/6a0d4ddf-0629-4b96-83a0-6ef1cf79d54c)

There should be a new file called `sky130_vsdinv.lef` now in the directory. Let us open it up and see the contents. <br>
![Screenshot from 2024-05-07 13-15-59](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/c48d3925-510b-42e0-90bf-6ef0e6c76784)

The different pins and their signal types are all described in the LEF file.

Lets copy the LEF file and the library files into the src directory of our design. <br>
![Screenshot from 2024-05-07 13-14-52](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/f901ccff-b579-4025-8c7a-8807549ca872)
![Screenshot from 2024-05-07 13-15-10](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/ff0c034f-89dc-460b-bc6f-4de5b53178b2)

All the nescessary files have been copied. Now we are ready to run the flow.
Let us include the library files in the `config.tcl` file and also add the extra LEF. <br>
![Screenshot from 2024-05-04 18-44-46](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/0a31b592-203d-4400-88b4-2cf69f1f39d5)

Now we can run the design preparation step just like we did in Day 2. After the design is prepared we have to run 2 more commands to include our custom LEF. <br>
```bash
set lefs [glob $::env(DESIGN_DIR)/src/*.lef] 
add_lefs -src $lefs
```

Once this is done we can proceed with synthesis. The synthesis statistics gives us information of the std cells that have been used. Here we can verify if our cell has been used, <br>
![Screenshot from 2024-05-07 13-32-51](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/0ff369e5-84b7-4744-a4b2-b965c3caee84)

Here we can see that there are 1554 instances of `sky130_vsdinv`.

##### 3. Reducing slack during synthesis
If we see the slack value generated after performing CTS, we can see that it is quite high. <br>
![Screenshot from 2024-05-07 13-33-00](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/10cea214-6d9b-45c3-bd0e-a17f444367ec)

Lets check the values of few of the parameters. <br>
![Screenshot from 2024-05-07 13-34-09](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/f65d369c-5524-4309-898f-299f0e9d5505)

Here we can see that the synthesis strategy is more oriented towards area than delay. And also the cell sizing is turned off. So we make changes on those 2 parametes and run synthesis again. <br>
![Screenshot from 2024-05-07 16-14-52](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/911c927b-e5b5-405d-9591-77632a050bdf)
![Screenshot from 2024-05-07 13-37-41](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/56f24509-3e67-443c-9254-be31961dffbd)
![Screenshot from 2024-05-07 17-33-29](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/a27f7fcb-3388-476e-b040-22494de53b90)

Above we can see that the slack is reduced but the area has increased.

##### 4. Floorplan and placement of the custom LEF
After synthesis we perform floorplan using the following commands, <br>
```bash
init_floorplan
place_io
tap_decap_or
```
Once the floorplan is completed we can run placement using `run_placement` <br>
We can view the layout using magic tool. <br>
![Screenshot from 2024-05-07 16-06-34](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/f2d40d5f-e4e9-4f87-aba9-4b6bcce13649)

When we zoom in we can search a little and the our custom inverter being used in the design. 
![Screenshot from 2024-05-07 16-07-57](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/931f3ed8-21d3-49b3-99cd-8820a4943e61)

Use the `expand` command in tkcon to view the internal connections of the inverter.
![Screenshot from 2024-05-07 16-10-45](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/92572b58-a554-4575-aa71-e14d29564ed2)

##### 5. Static Timing Analysis
To perform STA we first create a file `pre_sta.conf` in the `openlane/` directory. The contents of it are as follows, <br>
![Screenshot from 2024-05-07 17-36-47](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/83469a32-49ae-439a-b58d-c2dddfbbdbc0)

We also create a SDC file called `/my_base.sdc`. It is the `openlane/scripts/base.sdc` with some additions of environment variables. The contents of the sdc are as follows. <br>
![Screenshot from 2024-05-07 17-36-27](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/8cf81758-7436-492b-a121-7906ebc69162)

Once these files are in place we perform STA. <br>
In openlane terminal,
```bash
sta pre_sta.conf
```
This should bring up the timing report. The slack is same as we got at the end of synthesis. <br>
![Screenshot from 2024-05-07 17-35-11](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/d3f26a93-12f7-4fe2-9c4b-a6f1b0207a18)
![Screenshot from 2024-05-07 17-35-19](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/87c56b47-e99a-4175-828f-3ae3aafe9237)

We observe that many of these cells have high fanouts of 5 and 6 which increases the load capacitance and inturn contributes to the delay. Let us limit the fanout to only 4.
```bash
set ::env(SYNTH_MAX_FANOUT) 4
```
![Screenshot from 2024-05-07 17-37-24](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/a5ccaec1-429a-4b28-a37a-b38c137a6e09)

After this when we run the STA again we notice that the slack has reduced a lot. 
![Screenshot from 2024-05-07 17-41-44](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/7d0990b5-61dc-4e00-adb7-297944202a98)

To furthur improve the timing, we must look at the report and check what is contributing to the delay. Let us pick a cell which has a relatively high delay and observe it.
![Screenshot from 2024-05-07 17-46-38](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/c401544b-c8ca-47af-bea3-6018e3ddc2fe)

Let us look at what other cells is this MUX connected to. To check that first we pick the net and run the command,
```bash
report_net -connections [NET_NAME]
```
![Screenshot from 2024-05-07 17-46-13](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/93fa6248-db01-4538-9a58-d2a21b84a8a2)

We notice that the cell is connected to 6 other cells. but the drive strength of the MUX is low. So let us replace the MUX with a higher drive strength version.
```bash
replace_cell [INSTANCE_NAME] [LIB_CELL]
```
![Screenshot from 2024-05-07 17-47-50](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/47e29549-5691-47d2-b4a3-02f4c88fdc07)

![Screenshot from 2024-05-07 17-49-27](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/d20e0b58-1008-4a11-8d1a-ed52f5b2241c)

![Screenshot from 2024-05-07 17-51-52](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/965e27d7-49db-464f-9079-80e91306d57b)

We can see that the MUX has changed and the delay has slightly reduced. If we check the TNS and WNS also we can see that is has slightly reduced.

![Screenshot from 2024-05-07 17-52-28](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/0ff85b9b-e528-488b-ae66-bc799b9a10fb)

After this we try some furthur replacing to try and reduce the slack as much as possible before proceeding to the next stage. <br>
So this is how Static Timiing Analysis is performed using OpenSTA.

##### 6. Clock Tree Synthesis
To run CTS, we run `run_cts`. If successful, the output should be as follows, <br>
![Screenshot from 2024-05-07 19-10-11](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/d83ba356-8e45-4e2e-b525-194933a1b349)

Let us check the values of few of the CTS configurations. <br>
![Screenshot from 2024-05-07 19-14-15](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/3a448d20-8f8b-4730-b435-ac3656b6d07b)

Now we do a post-CTS analysis. This analysis is done with real clocks. The hold analysis also matters here. <br>
To proceed with timing analysis first we open OpenRoad and create a db.
![Screenshot from 2024-05-07 20-18-47](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/f30346d0-5d9c-4723-9f0e-c73dd3dc0b1e)

We read the db and include verilog, sdc, lib etc.
![Screenshot from 2024-05-07 20-26-43](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/ea58453a-311d-4262-8477-2c68f7e0fb35)

Now we can run the STA.
![Screenshot from 2024-05-07 20-28-22](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/11089870-c7f6-4e23-ad0a-f7b523ee9353)
![Screenshot from 2024-05-07 20-28-41](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/10499f71-6404-430e-828d-f4470cb6b008) <br>
The above pictures are of the Hold time path and the slack has slightly violated. 

![Screenshot from 2024-05-07 20-28-31](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/5932609a-ce05-4bb9-a087-d6a561363e4a)
![Screenshot from 2024-05-07 20-28-47](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/7f726609-7787-4b24-a025-ca29f7f6d9d1) <br>
The above pictures are of setup analysis and the slack is met. <br>
So we only have the hold violation to fix. To fix it let us remove clock buffer 1 so that the tool is forced to use a higher drive strength buffer.
```bash
set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]
```
Once we do this we have to run the cts again and create another db just like before and then perform STA. After replacing the following are the results. <br>
![Screenshot from 2024-05-07 20-45-35](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/3a866a37-175f-4301-a596-6a58809d5ed8)
![Screenshot from 2024-05-07 20-45-41](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/a3842740-7dcf-438b-908a-4b48d7ace9c4)

Both the setup and hold time are meeting. Lets us check the skew values. <br>
![Screenshot from 2024-05-07 20-46-54](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/d83fb8ac-cef9-46b2-92cf-02a8071d98a2)

Everything seems fine. We can now proceed to routing stage.

## Day 5: Final steps for RTL2GDS using tritonRoute and openSTA
### LABS
##### 1. Creating a power distribustion network
In reguler PnR flows, the power grid is created as part of the floorplan but in OpenLane flow the powergrid is created before routing. <br>
run the command, `gen_pdn` <br>
Once it runs it gives info on the metal pitch, width etc. <br>
![Screenshot from 2024-05-07 21-10-47](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/d18340cb-f9cd-4360-a23d-393a88f37cf4)

Let us open the temporary DEF and check the design out.
![Screenshot from 2024-05-07 21-19-31](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/2aea7db3-678b-4a29-ba63-deabe03d5998)
![Screenshot from 2024-05-07 21-20-49](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/7b67ea65-e6a4-466f-9e4d-5ca937749cb0)
![Screenshot from 2024-05-07 21-20-57](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/e6afa523-a6d0-4be1-a5f7-1cef9d920c92)

We can observe that there are vertical and horizontal power grids and also the smaller rails that take the power to the std cells.

##### 2. Routing
After the power grid is made, we proceed to routing. <br>
Command: `run_routing` <br>
Once the routing gets completed, the DEF is generated. Let us open the DEF and check it out. 
![Screenshot from 2024-05-07 22-56-42](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/ba4996d1-146f-4390-a8b8-d18e96ad55e2)
![Screenshot from 2024-05-07 22-57-09](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/bd37f652-8ef6-45dd-a6a0-8f73acc15f70)

We can see the various routes and the vias between the different metal layers. 
![Screenshot from 2024-05-07 22-57-46](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/a2928b53-d2ab-44e9-be45-6636bfb8288c) <br>

That was the final step of the PnR flow. If we have come across any timing violation after routing then we have to fix it as part of post-route timing analysis. Once the timing and drc violations are fixed we are ready with the GDS file which can be then proceed with the tape-out.

## Image of the final GDS file
![Screenshot from 2024-05-07 23-00-13](https://github.com/vyshak-git/VSD-SoC-Design-Workshop/assets/84836428/73f529dc-9fda-4bd1-b173-586af31a08b2)

