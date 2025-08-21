# Module 4 Lab Exercise – Configure Forwarder Management 

## Objective:
By the end of the session, you will be able to:
- Objetive 1
Description
In this exercise, you will use the Forwarder Management interface in Splunk Web to configure a remote universal forwarder and a heavy forwarder. The advantage of this option is that it allows you to manage multiple groups of forwarders from a central location.

First, you will enable the deployment server feature on your deployment server instance and stage two deployable apps for your forwarders that have already been created for you. You will use these apps to configure the outputs.conf file which is needed to tell the fowarder where to send its data. The apps, uf_base for universal forwarder #2 (UF2) and hf_base for the heavy forwarder, are staged in SPLUNK_HOME/etc/deployment-apps.
Next, you will launch a second universal forwarder (10.7.3X.12) and a heavy forwarder (10.7.3X.15) and configure them as deployment clients.

Finally, you will define a serverclass in the Forwarder Management UI of the deployment server to deploy the uf_base and hf_base apps to their correct forwarders. The serverclass associates deployable apps with deployment clients.
IMPORTANT:	Completing this lab exercise is crucial because it is a prerequisite to several subsequent lab exercises.

## Time for this activity:
- 45 minutes.

## Instructions: 
<!-- Provide detailed steps on how to configure and manage systems, implement software solutions, perform security testing, or any other practical scenario relevant to the field of Information Technology -->

### Task 1. Copy the uf_base app to the deployment-apps directory and configure outputs.conf.
In this first task, you will copy the uf_base app and stage the app to be deployed to UF2. The
outputs.conf file will be configured to send its data to the receiving port of the heavy forwarder.

**Step 1.** Access your deployment server’s command line (SSH for Linux, RDC for Windows).

**Step 2.** Copy the entire uf_base directory from /opt/apps to SPLUNK_HOME/etc/deployment-apps/

![imagen resultado](../images/img32.png)

**Step 3.** Navigate to the local directory of the uf_base app and list its contents to make sure the
outputs.conf file was copied successfully.

NOTE:	You will also see a deploymentclient.conf file in the local directory. This file is also deployed to the forwarder to reduce the polling interval (how often the deployment client contacts the deployment server) from 60 seconds (default) to 30 seconds.

**Step 4.** Using a text editor, add the stanza below to the outputs.conf file. (Linux users can use vi or nano, Windows users can use Notepad++.)

NOTE:	Most Splunk configuration file contents are case-sensitive. If you copy and paste from the PDF lab document to populate configuration files, make sure the contents are exactly as shown in the steps.
Windows Users: You must have administrator rights when editing the Splunk configuration files in the lab environment. Use the Notepad++ icon on the desktop to launch the application with administrator rights. If you are not sure, you can use the following guidelines for opening and saving the files.
•	Right-click the Notepad++ and select Run as administrator.
•	When saving files, click Save as and use the All types (*.*) option. Do not save your files as text files (*.txt files).

[tcpout]
defaultGroup = default-autolb-group
[tcpout-server://10.7.3X.15:9997]
[tcpout:default-autolb-group] disabled = false
server = 10.7.3X.15:9997

**Step 5.** Save and close the edited file.

### Task 2. Configure universal forwarder #2 (UF2) as a deployment client.
In this task, you manually configure the forwarder as a deployment client by using the splunk set deploy-poll command. Since many Splunk environments use hundreds or thousands of forwarders, this is not practical or scalable. Most Splunk customers use a third-party software configuration management tool, such as Puppet or Chef. Another option is to include the Universal Forwarder software with a deploymentclient.conf file into pre-configured software builds.

**Step 1.** From your deployment server’s command line, SSH into your UF2 (10.7.3X.12). (Refer to Task 1 of the previous exercise for OS-specific instructions.)

![imagen resultado](../images/img33.png)

**Step 2.** Navigate to the bin directory and initialize the forwarder with the --accept-license option.

![imagen resultado](../images/img34.png)

**Step 3.** Use the CLI to determine the auto-assigned management port number. (Splunk will prompt you for the admin username and password which is admin and your assigned password.)

![imagen resultado](../images/img35.png)

**Step 4.** Using the set servername and set default-hostname commands, change your forwarder's server name and default hostname to engdev2{student-id}:
This step uniquely identifies the data originating from your forwarder instance in this lab environment.
NOTE:	Defer the restarts until you have made all your changes.

![imagen resultado](../images/img36.png)

**Step 5.** 10.	Use the set deploy-poll command to establish communication between the forwarder and the deployment server, then restart the forwarder.

![imagen resultado](../images/img37.png)

**Step 6.** Use the show deploy-poll command to verify the deployment-client configuration.
NOTE:	10.7.3X.10 is the internal address of your deployment server instance.

![imagen resultado](../images/img38.png)

**Step 7.** Use the btool command with the --debug flag to show all of the Splunk settings associated with the creation of the deploymentclient.conf file.

![imagen resultado](../images/img39.png)

