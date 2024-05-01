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






