# SOAR EDR Project

## Objective
The objective of this project is to simulate an attack on a virtual machine and track the incident via an Endpoint Detection and Response (EDR) tool. Then we'll create a customised Detection & Response Rule to alert about the malicious behaviour in our target machine. The target machine that will be used in this project is a windows server 2022 ran in a virtual machine using virtual box. Following that, we will create a Playbook/Story to handle the response to the alert. This Playbook/Story will be created to notify via Slack and email about the incident, and will also prompt the user to select whether to isolate or not the compromised machine.

## Skills Learned

- Creation of a diagram to explain the workflow.
- Ability to create Detect & Response rules.
- Enhanced knowledge on SOAR and EDR tools.
- Creation of playbooks/stories.
- Virtual Machines.

## Tools Used

- Security Orchestration, Automation, and Response (SOAR) tool [Tines].
- Endpoint Detection and Response tool [LimaCharlie].
- Telemetry generation tools [LaZagne].

## Steps

### Step 1.- Designing a Diagram

To start things, we created a Diagram to show the desired workflow of this project. This time we used a tool called <a href="https://app.diagrams.net/">Draw.io</a> to design the diagram.

The first thing to do is describe what we wanted out of this project.
- Create a Playbook/Story using Tines.
- Create detection in LimaCharlie that would Detect the use of a HackTool (LaZagne), which then would be send to Tines, and finally sent a message to Slack & also an Email.
 
The slack message and the Email will contain:
- Time
- Computer Name
- Source IP
- Process
- Command Line
- File Path
- Sensor ID
- Link to the detection

 
The alert in Tines would then Prompt the User to isolate the machine and ask (Yes/No)
- If **yes**: LimaCharlie should automatically isolate the machine and a message should be sent to 
	+ Message: Isolation status note of "The computer <computer> has been isolated."
- If **no**: LimaCharlie will not isolate
	+ Message: Isolation status with note of "The computer <computer> was not isolated, please investigate."

 ![SOAR-EDR-Diagram(1)](https://github.com/user-attachments/assets/df1dbc2d-a15b-4914-a1d4-d03c34658e29)

 Image #1. Diagram

### Step 2.-Installing and Setting up LimaCharlie

To start Installing LimaCharlie we are going to need to log in or create an account in <a href=https://limacharlie.io/>limacharlie.io</a>  in case you don't have one.

Once you've logged in we will be presented with a small guide to the concepts used in LimaCharlie, in this screen we will create an **Organization**.

![image](https://github.com/user-attachments/assets/ee16c494-c25f-40d3-aae5-75de4b377f43)
Image #2.

After clicking the button "Create Organization", we will be presented with a new window where we will name our Organization and select the Region closest to our location. The we can leave the template option as it is. Finally we click "Create Organization".

![image](https://github.com/user-attachments/assets/2f4b690f-33f5-46c5-97f8-eed29c13e933)
Image #3

Now that we have created an organization we will need to go to the **Sensors** tab and then go to **Installation Keys** where we will create a key so that we can start adding machines to our Organization.
To create an Installation Key we are going to click the button "Create Installation Key" and give it a name. We are given the option to give it tags so that we can recognize the devices related to this key, but for the sake of this project we will not be doing that. _**Note:**_ We can delete the rest of the keys so that it looks cleaner once we start adding machines.

![image](https://github.com/user-attachments/assets/5089cb84-e84e-4d0b-b5e0-f8a4cd314d64)
Image #4

 After we've created our Installation Key we can scroll down to the "Sensor Downloads" section. Here we can download the installation file to start adding machines to our organization. Since in this project our target machine we will be using is a Windows Server, we can go ahead and download the EDR Windows 54 bit file. _**Note:**_ Make sure you download this file using the Windows Server virtual machine so that you don't need to open a path between your main machine and the virtual machine.

![image](https://github.com/user-attachments/assets/98f4c7b3-db14-43a1-8a32-11b03a2da78b)
Image #5

While the file is downloading, we can go ahead and copy the "Sensor Key" of the key we just created since it will be needed for the installation.

![image](https://github.com/user-attachments/assets/353a2cda-63e4-4b29-8af4-a5d3ec91b8a8)
Image #6

To run the file we just downloaded, we will need to open PowerShell as an Administrator and navigate to where we saved the file. Once we are in the path where our file is, we will run the next command `<name of the file> -i <Sensor key>` and then hit Enter.

If everything went right, PowerShell will show us a success message.

![image](https://github.com/user-attachments/assets/31656759-de8e-4db3-a6c2-38c25a3f2dc6)
Image #7

To verify if the installation was successful we can search for the app "Services" in our Windows Server machine and search for the newly created service called "LimaCharlie".

![image](https://github.com/user-attachments/assets/f1d37809-d9a4-48f0-b121-72a5b5ca07fa)
Image #8

We can also verify if the installation was successful by going back to LimaCharlie and going to the "Sensors List" and searching for the new added machine to the list.

![image](https://github.com/user-attachments/assets/91dbfed5-1814-4171-abb1-f5197e2c0da9)
Image #9


Now that we've verified that our machine was added to the sensor list, we can go ahead and click this new sensor, this will show us the details of the machine like the hostname, the OS, its IP and many more.

### Step 2.-Installing and Setting up LimaCharlie












































