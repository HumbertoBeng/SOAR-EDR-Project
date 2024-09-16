# SOAR EDR Project

## Objective
The objective of this project was to simulate an attack on a virtual machine and track the incident via an Endpoint Detection and Response (EDR) tool. We then created a customised Detection & Response Rule to alert the malicious behaviour within our machine. The machine used in this project consisted of a windows server 2022 ran in a virtual machine using virtual box. Following that, we created a Playbook/Story to handle the response to the alert. This Playbook/Story was created to notify via Slack and email, and will also prompt the user to select whether to isolate or not the compromised machine.

## Skills Learned

- Creation of a diagram explaining the workflow.
- Ability to create Detect & Response rules.
- Enhanced knowledge on SOAR and EDR tools.
- Creation of a playbooks.
- Creation of a Virtual Machines

## Tools Used

- Security Orchestration, Automation, and Response (SOAR) tool [Tines].
- Endpoint Detection and Response Rules [LimaCharlie].
- Telemetry generation tools [LaZagne].

## Steps

### Step 1.- Designing a Diagram

To start things we were tasked to create a Diagram to show the desired workflow of this project and to demonstrate the importance of creating Diagrams as the first step on a 
project. This time we used a tool called <a href="https://app.diagrams.net/">Draw.io</a> to design the diagram.

The first thing we had to do was to describe what we wanted out of this diagram.
	Create a Playbook/Story using Tines.
	Create detection in LimaCharlie that would Detect the use of a HackTool (LaZagne), which then would be send to Tines, and finally sent to Slack & Email.
    Slack & Email will cointain:
    - Time 
	  - Computer Name
    - Source IP
    - Process
    - Command Line
    - File Path
    - Sensor ID
    - Link to the detection (if applicable)
  
The alert in Tines would then Prompt the User to isolate the machine and ask (Yes/No)
	- If yes: LimaCharlie should automatically isolate the machine and a message should be sent to 
	+ Message: Isolation status note of "The computer <computer> has been isolated."
	- If no: LimaCharlie will not isolate
	+ Message: Isolation status with note of "The computer <computer> was not isolated, please investigate."

 ![image](https://github.com/user-attachments/assets/a7391f87-66e2-485e-8c4b-0d163df24936)
 Picture #1. Diagram

 ### Step 2.-Installing and Setting up LimaCharlie

 1. The next step was creating an account in LimaCharlie, setting up an "organization" and start generating events.
 This time we named the organization "CSBarista - SOAR - EDR" as this is the name of the project.
	![image](https://github.com/user-attachments/assets/1acf2a19-9051-4504-896a-019a56bb8f43)
	Picture #2. Organization created

 3. Once the organization was created, we ran our windows server 2022 virtual machine to start the connection with LimaCharlie and to do that we need to download the sensor (refer to Picture #3) and create an installation key to install said sensor. As for the name of the key we decided to use the same name as the organization and adding the word "Project" to it, this so it would be easy to know to which sensor it belongs to(refer to Pictures #4 and #5). then copying it (refer to Picture #6), all this found inside the "Sensors" tab and within  "Installation Keys" Section.
	![image](https://github.com/user-attachments/assets/8bd2fcbe-a758-4d07-bc56-70e98b4b22ac)
	Picture #3. Sensor Downloads

	![image](https://github.com/user-attachments/assets/9bec84d8-0413-4fa6-82f3-c832a5f152f5)
	Picture #4. Creating the installation key

	![image](https://github.com/user-attachments/assets/43c39f06-06e3-45a9-ba6d-2fd9d0b75d8b)
	Picture #5. Naming the newly created installation key

	![image](https://github.com/user-attachments/assets/d1d57212-a9de-4152-b02f-810989497e55)
	Picture #6. Copying the installation key


After downloading the sensor for our virtual machine we would run it through powershell with admin privileges using the next command.
	
	`lc_sensor.exe -i YOUR_INSTALLATION_KEY`


As a result of running this command we would see our new machine detected by LimaCharlie with the same name the machine has, you can verify this in the "Sensors" tab within the "Sensors List" section (refer to Picture #7).
	![image](https://github.com/user-attachments/assets/a8a87bed-3a84-4aff-888d-b56d4f8afb87)
 	Picture #7. Machine recognized in LimaCharlie as a Sensor.


Finally, once we have our virtual machine detected as one of the Sensors we can click on it and it would give us a lot of information related to that machine, like its platform, the date it was enrolled as a sensor, its internal and external IPs, and even its MAC address (Picture #8).

![image](https://github.com/user-attachments/assets/b7e3638d-eafb-40f0-b370-9adc7d2cd071)
Picture #8. The Sensor Information


One of the many benefits of using LimaCharlie as an EDR tool is that it provides a lot of tools to monitor Sensors or Machines. As an example of a couple of them we have the Console which can be use to run remote commands from LimaCharlie to the machine, Detections which is the part we would be working with in this project that allows us to see all the alerts that are configured in our organization, and finally we have the file system that allow us to look up all the files inside the machine and even allows us to create and delete files.


### Step 3. 



















