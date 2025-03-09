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

### Step 3.- Generating Telemetry with LaZagne

To generate telemetry we are going to be using LaZagne an open source application used to retrieve passwords stored on a local computer.

First,we need to download the tool from the official page <a href="https://github.com/AlessandroZ/LaZagne">The LaZagne Project</a>. To do that we can go to the right side of the site and look for **Releases** and then click it. This will take us to all the versions of the project. _**Note:**_ we are going to use this tool in our Windows Server virtual machine.

![image](https://github.com/user-attachments/assets/6b5427e5-6fc9-4530-8298-301d2e299bce)
Image #10

We are going to download the last version of LaZagne.exe. Make sure to disable Windows Defender so you can download the file.


Now we want to start generating Telemetry so that we can create our Detection & Response Rules, to do that we can open powershell and execute the file from there. A easy way to do that is to go directly to the path where we downloaded the LaZagne.exe file and use _Shift + right click_ in an empty space and then select "Open PowerShell window here".

![image](https://github.com/user-attachments/assets/928de2d5-00ca-444e-ac41-d833359c0935)
Image #11

Then once PowerShell has open, we can go ahead and execute the file by typing LaZagne and using the TAB key to autocomplete.

![image](https://github.com/user-attachments/assets/ad91bfd8-1aab-4f4e-82c6-f808c07215a4)
Image #12

Now that we've run the command in PowerShell we can verify if it generated an event in LimaCharlie. To see that we can go to LimaCharlie, then click in the sensor we just created and finally head to **Timeline**

![image](https://github.com/user-attachments/assets/0a4deb37-70d0-4548-85b7-2f4eb3458228)
Image #13

In this section we can see all the events generated by our machine. Here we can look for events related to LaZagne, to avoid looking for each event, we can just use the **Quick Search** section in the top of the window and search for _LaZagne_. Here we should see a couple of events generated that have the keyword "LaZagne", as we can see the event generated when we downloaded the file is called _NEW_DOCUMENT_, then going down there is an event called _NEW_PROCESS_ which indicates the moment we ran the file through PowerShell.

![image](https://github.com/user-attachments/assets/91d8c973-5689-4d47-b153-d51f3f2c6f40)
Image #14

The thing with the events generated in this section is that we can learn about more details regarding each event, so if we click the events with the name of _NEW_PROCESS_ we can learn more about said event. Here we have interesting information such as the File Path, Hash of the file, the parent process (which we can see is PowerShell), the process ID and many more.

![image](https://github.com/user-attachments/assets/1ed5b67f-a5d4-4a8e-9429-99d341120bed)
Image #15


### Step 4.- Creating a Detecion & Response Rule

Now that we know what kind of information an event with LaZagne contains, we can create a custom rule to detect it.

To create a rule we can open another tab with LimaCharlie and go to _Automation_ and then to D&R Rules.

![image](https://github.com/user-attachments/assets/f979fecc-6e7e-470a-b587-d6b6dcb6abe3)
Image #16

To start writing the new rule, we can go ahead and click on the top right corner the button **+ Add Rule**.


The interesting thing when creating a rule and avoid writing it from scratch, is that once we know what kind of event generates the tool in cuestion, in this case LaZagne, we can just take as a reference Rules that are already created in our organization or from different DataBases like Sigma Rules, so that we can just modify them to detect specifically LaZagne related events.

This is what we are going to do to start writing our new rule. In the _Detection & Response Rules_ section within the **Automation** tab, we can make a quick search for a rule with a similar objective. In this case, since LaZagne is a Credential Access Tool, we can search for a Rule with the keyword "Credential". _**Note:**_ In case that there is no rules within the _Detection & Response Rules_ section, we can add them from the _Extensions_ tab and "Subscribe" to the extensions of SnapAttack, SIGMA and Atomic Red Team.

![image](https://github.com/user-attachments/assets/ffebfe41-c6af-4543-a1ed-45c292f7104a)
Image #17

The result gave us 100+ rules that contained the "Credential" keyword, so to narrow down the search for a rule that will be the most useful one, we can check what other kind of information de event generated gave us. In this case, the event generated when executing LaZagne was a _NEW_PROCESS_ event, so lets search for a rule that contains something related to creation of processes.

Going further down we can see a "windws_process_creation", this rule could be of use for us.

![image](https://github.com/user-attachments/assets/61b7456e-6018-497d-a3b4-38460a9e7b47)
Image #18

To see the contents of the rule we can go ahead and click it and then head to the github repository so that we can copy it and make use of it for our new rule.


Now that we have the rule we can go back to the rule we created at the start which should be all the way to the top of the list. To edit the rule we can click the pencil beside it.

![image](https://github.com/user-attachments/assets/7e1c5713-ce61-401e-b576-fadc096452ac)
Image #19

#### 4.1.- Tags and Values

To start we can take a look at the rule we just obtained. 

Looking closer to our rule, it contains 2 main subjects that are indicated by the position they have, all the way to the left with no spaces. They are "detect:" and "response:" both of this signify what each part of the rule is going to do. So if you look at the layout of the rule we are creating there is 2 boxes, one with _Detect_ and one with _Response_ both of them signify the same as the "detect:" and "response:" within the rule we just obtained. 

##### 4.1.1.- Detect

We'll divide the rule in two and explain what the detect and respond parts does using the example Rule _proc_creation_win_reg_enumeration_for_credentials_in_registry.yml_.

 ```
detect:
      events:
       - NEW_PROCESS
       - EXISTING_PROCESS
      op: and
      rules:
      - op: is windows
      - op: and
      rules:
    - op: and
      rules:
      - case sensitive: false
        op: ends with
        path: event/FILE_PATH
        value: \reg.exe
 ```
Starting with the tag _events:_, it contains _NEW_PROCESS_ and _EXISTING_PROCESS_ as values. This values are taken from the _Event Collection_ tab within the tabs our machine in LimaCharlie. Which means that this detection will only trigger if the event is under the _NEW_PROCESS_ or _EXISTING_PROCESS_ values.

![image](https://github.com/user-attachments/assets/60614cc2-4991-4040-8de9-9e4dd8985bfa)
Image #20

The next tag is _op:_ which is short for Operator, with the value of *and*. We can get a list of operators within the rule creation window to the right. As you can see each operator is highlighted, if you click in any of them it will take you to the documentation for that specific operator. So in short, the _op: and_ tag does is that the event must include the above mention values of _NEW_PROCESS_ or _EXISTING_PROCESS_ *and* a set of _rules_ which will be written next for the detection to trigger.

![image](https://github.com/user-attachments/assets/9c839036-85c6-4737-83c1-67de9bda3817)
Image #21

Then the _rules:_ tag is the criteria the event must follow for the detection to trigger. Using the example above we can see that is using the _op_ again, but this time is using _is_ as a value and then adding the keyword _windows_, which means that one of the conditions for this detection is that the OS of the machine must be windows. 

Going down a bit we can see there is another tag of _rules:_ with the tag _case sensitive: false_ which is kind of self-explanatory. Now the next tag is once more an operator tag _op:_ now with the value of _ends with_ and the tag _path: event/FILE_PATH_. The tag _op: ends with_ is telling that the path is required to end with the same value that the tag _path:_ has. and finally the value tag _value: \reg.exe_ is what should replace the FILE_PATH within the tag _path: event/FILE_PATH_ for it to trigger the alert. we can exchange the _FILE_PATH_ value from the tag _path: event/FILE_PATH_ with any other value that contains the event, for example the "HASH" value. refer to image #15.

In summary what this Detection does is to detect if a new event has the values of NEW_PROCESS or EXISTING_PROCESS, then verify if the OS of the machine/sensor is indeed windows. Once validated both values it will only trigger if the path of the event generated ends with \reg.exe ignoring case sensitivity. 

##### 4.1.2.- Response

 ```
respond:
- action: report
  metadata:
    author: frack113
    description: 'Adversaries may search the Registry on compromised systems for insecurely
      stored credentials.

      The Windows Registry stores configuration information that can be used by the
      system or other programs.

      Adversaries may query the Registry looking for credentials and passwords that
      have been stored for use by other programs or services

    falsepositives:
    - Unknown
    level: medium
    references:
    - https://github.com/redcanaryco/atomic-red-team/blob/f339e7da7d05f6057fdfcdd3742bfcf365fee2a9/atomics/T1552.002/T1552.002.md
    tags:
    - attack.credential_access
    - attack.t1552.002
  name: Enumeration for Credentials in Registry
```

The first thing in the rule is the tag _action:_ with the set value of _report_, what this value does is that if certain event meets the criteria set in the Detect box it will then generate a detection under the _**Detections**_ tab and that is pretty much what we want.

The _metadata:_ tag is self-explanatory, contains the data related to this rule about the author and what it does. You can add tags and a reference for the rule in case someone else need to use it.



#### 4.2.- Writing our own custom rule

##### 4.2.1.- Detect

So to start writing our Detect rule we can use the example rule we used before. Here it is:

```
detect:
      events:
       - NEW_PROCESS
       - EXISTING_PROCESS
      op: and
      rules:
      - op: is windows
      - op: and
      rules:
    - op: and
      rules:
      - case sensitive: false
        op: ends with
        path: event/FILE_PATH
        value: \reg.exe
 ```

This rule aligns in part with what we want. we know that the event generated by the tool recognizes what tool is being used and the path where the tool is saved. So to start detecting that we can just replace the tag _value: \reg.exe_ with the value of _lazagne.exe_. 

Moving forward, we know that a tool can have different uses, which means that it can be also used with tags for different functionalities, so we need to account for that when writing our new rule. So to make up for that we can add a new section of rules. Also to make our rule more thorough we can add a new rule that accounts for the HASH of the file.

And this is how it looks the final Detect rule:

```
   events:
   - NEW_PROCESS
   - EXISTING_PROCESS
   op: and
   rules:
   - op: is windows
   - op: or
     rules:
     - case sensitive: false
       op: ends with
       path: event/FILE_PATH
       value: \Lazagne.exe
     - case sensitive: false
       op: ends with
       path: event/COMMAND_LINE
       value: all
     - case sensitive: false
       op: contains
       path: event/COMMAND_LINE
       value: lazagne
     - case sensitive: false
       op: is
       path: event/HASH
       value: 'eb71ea69dd19f728ab9240565e8c7efb59821e19e3788e289301e1e74940c208'    
 ```

##### 4.2.2.- Response

For our response rule we are just going to modify it a bit so that it matches what the rule is going to do.

Here is the final rule:

 ```
- action: report
  metadata:
    author: CSBarista
    description: 'Detects LaZagne (SOAR-EDR Tool)
    falsepositives:
    - To the moon
    level: medium
    tags:
    - credential_access
  name: CSBarista - HackTool - LaZagne (SOAR-EDR)
 ```

##### 4.2.3.- Testing our new rule

Once we have finished our rule we can go ahead and save it.

LimaCharlie has a tool for us to test our newly created rule. To do that, scroll down a bit in a section called _**Target Event**_ where we can just paste the details of the event we want to test it with and finally click _Test Event_ at the bottom of the page to test it. We can get the event in the _Timeline_ tab where we saw the events being generated before.

![image](https://github.com/user-attachments/assets/572b3e47-d527-44f2-a0c8-0959eb7bc116)
Image #22

The rule was successful in detecting the event.

To try it out, we can head to the _**Detections**_ tab and execute the file once more in our Windows Server machine.

![image](https://github.com/user-attachments/assets/3dd40bf9-f2ed-4c9e-9cf5-e2588352e875)
Image #23

As we can see in the feed we of alerts there is our newly created Detection & Response Rule working fine.

### Step 5.- Setup Slack

First things first we need to create an account with Slack.

Once you have created an account with Slack go ahead and click the "Create a workspace" button. Next write a name for your Organization, then write the name you would like to be recognized with. We won't be adding more people to this company since is not necessary at the moment so we can skip that option. And finally give a description to your Organization, it can be anythinng.

Now we are going to create a new channel where all the alerts are going to be sent. To do that we can click the _Add channels_ button and then select _Create a new channel_ and finally selecting _Blank channel_. 

![image](https://github.com/user-attachments/assets/1bb61608-6d69-4d99-9336-e61c4fb5cb44)
Image #25

To show that this channel will be the one receiving the alerts from Tines, let's name give it the name of "alerts" and setting it up its visibility to Public, and at the end just click create.

![image](https://github.com/user-attachments/assets/379157e8-04e3-4d28-bc00-81e471dec1f3)
Image #26

The channel now created should be at the top of the list of "Channels".



### Step 6.- Setup Tines

Same as before we are going to create an account to use with Tines.

Once we've created our account we should end up with a screen like this.

![image](https://github.com/user-attachments/assets/9426d476-e547-4f16-ac87-7d01ca70b9cf)
Image #27

So what we need to do is set a connection between LimaCharlie and Tines. To do that we can go ahead an take the "Webhook" tool on the left side panel and drag it to the center. Once done that we need to change its name to "Retrieve Detections" and give it a description, to do that just simply click on the Webhook icon we just grabbed and a panel on the right side should show with the information to do so. Finally copy the Webhook URL because we are going to use it for the next step.

![image](https://github.com/user-attachments/assets/d6516281-68fe-4421-86a7-f8740c8b91f8)
Image #28

Now we are going to go to LimaCharlie. Once inside our Organization, we need to go to the _Outputs_ TAB and then select "Add Output".

![image](https://github.com/user-attachments/assets/18aab6c8-d827-49b4-9ed3-406912c50a6c)
Image #29

After that there will be displayed different Outputs related to our Organization. What we are going to be using this time is going to be the "Detections" output, so click the "Select" button next to it.
Next we need to choose the destination to where the Detections are going to be sent to. So scrolling down a bit there should be an option for "Tines" which is the tool we are using.

![image](https://github.com/user-attachments/assets/835a92b6-6714-412c-815c-0e89e2d40a9f)
Image #30

![image](https://github.com/user-attachments/assets/6990847f-83c3-4378-aea0-98370278ae82)
Image #31

We are going to give this Output a name and a Destination Host and finally click on "Save Output". The Destination host is the link we copied earlier from the Webhook URL in Tines. refer to Image #28.

![image](https://github.com/user-attachments/assets/fd72c05a-1068-4edc-9f18-16da3391dea1)
Image #32

After creating the new Output it may say that it "Couldn't detect any recent samples moving trhough this output", which means that is not detecting anything at the moment. To fix that we can just go to our Windows Server machine and run LaZagne again to generate the event and for our rule to Detect it. Once done that we can click on the "Refresh Samples" button until we see our detection on screen.

![image](https://github.com/user-attachments/assets/698d4a03-dd12-4adc-bf78-6626d70ff867)
Image #33

To verify if the connection was successful we can head over to Tines and look for the Webhook Action we just added, then click on the Webhook and a couple of actions should be displayed under the Webhook, between the new actions there should be a "Events" section and that is where we can see if the connection was successful.

![image](https://github.com/user-attachments/assets/a8ffff83-e8b9-46a4-b722-1630f422abaa)
Image #34

After we click the "Events" action a small window should appear at the bottom of the screen. Here is where the detections that come from LimaCharlie appear. Looking through them we can see that our Detection is here as well.

![image](https://github.com/user-attachments/assets/8e1c6866-112e-470a-bd42-7be42ef963de)
Image #35

Inside this event we can find the same tags that we can find in the _Timeline_ TAB back in LimaCharlie. Things like the "COMMAND_LINE", "FILE_PATH" and "HASH" tags can also be found here.

![image](https://github.com/user-attachments/assets/7829121a-c9a5-4ca4-a5d1-b4253dd08690)
Image #36


### Step 8.- Creating a Playbook in Tines

To create our Playbook we're going to take a look to the Diagram. The first thing we did was create a Sensor in LimaCharlie to detect the use of a hacktool within it. So the next thing to do is to create a connection between Tines and Slack so that Tines can notify Slack if an event regarding the Alert we set up earlier happen to occur within our Sensor.

#### 8.1.- Connecting Tines and Slack

So to connect Slack and Tines we need to go to Slack and on the left side panel click on the three dots **---** and then select "Automations".

![image](https://github.com/user-attachments/assets/8ab5ca3f-3696-4eb9-abc0-423fdcdf6417)
Image #37

Once inside we are going to move to _**Apps**_ and search for Tines and then click "Add".

![image](https://github.com/user-attachments/assets/34bcbad7-88aa-4af2-a6ff-2530ce3ecb5a)
Image #38

This will take us to another window with the information to add Tines to Slack. It will give us an "Authentication guide" to install Tines.

![image](https://github.com/user-attachments/assets/deb10da6-7e11-494c-a095-cba2da3fd991)
Image #39

At the end of the installation you should have something like this in Tines.

![image](https://github.com/user-attachments/assets/254551ef-f23c-4196-bc25-b792466c5e6d)
Image #40

Now that we have the correct credentials in Tines, we can go back to our Playbook. To add Slack as a template to our playbook we can click "Templates" in the left side panel and search for **Slack** and then drag it to the center.

![image](https://github.com/user-attachments/assets/473637b1-8da6-4c30-976a-fb7ccb1d9cd8)
Image #41

Since we want Tines to send a message to Slack we need to select a template that will allow us to do just that. So to search por a template we need to click on the Slack item we just added to our playbook and on the right side panel search por "message".

![image](https://github.com/user-attachments/assets/6e2c17e8-5479-4b80-baeb-b0d89b7d30ae)
Image #42

Reading through the description of the template we just created, it says that it need the permission to _chat:write_ of the channel we are going to send the messages to. Every channel in Slack has a Channel ID which will serve to give permission to write in said channel. To find the Channel ID of a channel in Slack we can go to the list of channel of the organization we are in and then right click the channel and select "View channel details", the Channel ID should be at the bottom of the new window. Once we've find that ID we need to copy it.

![image](https://github.com/user-attachments/assets/448f9a28-a663-4aa4-a5ea-b8439019a85a)
Image #43

The ID we just copied it needs to be paste in Tines within the Slack template we created.

![image](https://github.com/user-attachments/assets/8f02f3c4-ba72-44f9-a1a1-ed6747be69e0)
Image #44

Now what we want to do is connect the Webhook and the Slack template. To do that we can hover the Webhook item and a little arrow pointing down should appear under the icon. To connect them together we can just click in the arrow and drag the line to the Slack template.

![image](https://github.com/user-attachments/assets/3fdfb5c9-2ec8-407a-bb98-4facbe08d4d0)
Image #45

To verify if there is connection between Tines and Slack we can click on the Slack template in Tines and select _Run_, this will send a default message to the channel "alerts" in Slack.

![image](https://github.com/user-attachments/assets/8b30dc22-59e5-4732-a1de-58ff52484f48)
Image #46

![image](https://github.com/user-attachments/assets/ae2a806b-299a-4540-8a4d-5fc0e6921c7b)
Image #47

With that we have the first part of the diagram completed.

#### 8.2.- Sending an Email through Tines

To send an email from Tines we can add the Tool _Send Email_ from the left side panel and drag it to the our playbook. Once done that we need to connect it to the Webhook the same way we did with the Slack template. 

![image](https://github.com/user-attachments/assets/fc44a2ac-968a-4fac-b80c-2fd24a416e73)
Image #48

If we click the "Send Email" item in our playbook, a panel on the right side of the window will appear, here we can select to which email we want the alert to be sent and also if we want to modify the name of the sender, so that we can now from where does the email come and finally we can select the Subject of said email, we will modify that later.

We can test if it works by clicking on the item for "Send Email", then selecting "Test" and in the window of test we can just choose any event.

![image](https://github.com/user-attachments/assets/c90b7809-8372-43f9-8ddb-8d83056579e4)
Image #49

Once selected an event and press "Test" and email should be send to the email we select it. By default it will send an email of the account we are logged in. Here is the example email we should have received. 

![image](https://github.com/user-attachments/assets/30d9b164-38dd-41dd-90a3-6d7c01ca28df)
Image #50






