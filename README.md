# SOAR EDR Project

## Objective
The objective of this project was to simulate an attack on a virtual machine and track the incident via an Endpoint Detection and Response (EDR) tool. We then created a customised Detection & Response Rule to alert the malicious behaviour within our machine. The machine used in this project consisted of a windows server 2022 ran in a virtual machine using virtual box. Following that, we created a Playbook/Story to handle the response to the alert. This Playbook/Story was created to notify via Slack and email, and will also prompt the user to select whether to isolate or not the compromised machine.

## Skills Learned

- Creation of a diagram explaining the workflow.
- Ability to create Detect & Response rules.
- Enhanced knowledge on SOAR and EDR tools.
- Creation of playbooks.
- Creation of Virtual Machines

## Tools Used

- Security Orchestration, Automation, and Response (SOAR) tool [Tines].
- Endpoint Detection and Response Rules [LimaCharlie].
- Telemetry generation tools [LaZagne].

## Steps

### Step 1.- Designing a Diagram

To start things, we created a Diagram to show the desired workflow of this project. This time we used a tool called <a href="https://app.diagrams.net/">Draw.io</a> to design the diagram.

The first thing we had to do was to describe what we wanted out of this project.
	Create a Playbook/Story using Tines.
	Create detection in LimaCharlie that would Detect the use of a HackTool (LaZagne), which then would be send to Tines, and finally sent a message to Slack & also an Email.
 
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

 ![image](https://github.com/user-attachments/assets/a7391f87-66e2-485e-8c4b-0d163df24936)
 
 Picture #1. Diagram

 ### Step 2.-Installing and Setting up LimaCharlie





















