<h1>Deploying Splunk apps and updates with a Deployment Server</h1>


<h2>Description</h2>
In this project, a deployment server is used to push apps and configuration updates to Splunk Universal Forwarders in a Clustered Splunk Deployment hosted in AWS.A deployment server is typically a standalone instance that manages configuration and application updates across unclustered members of a deployment.This is particularly useful when running multiple remote forwarders that may need regular updates.In this case the Forwarders are considered Deployment Clients as they are configured to poll the Deployment Server    periodically to receive necessary updates.A Serverclass is used to map the Deployment Clients to the different updates or apps as the case may be.

<br />


<h2>Lab Environment Setup</h2>

- <b>11 t2micro EC2 Intances running in AWS</b> 
- <b>Windows 10 work station (21H2)</b>
- <b>MobaXterm SSH Client</b>
- <b>Greendot</b>
<h2>Intances Set-up </h2>

- <b>Indexer1 - idx1</b> 
- <b>Indexer2 - idx2</b>
- <b>Indexer3 - idx3</b>
- <b>Cluster Master - cm</b>
- <b>Search Head1 - sh1</b>
- <b>Search Head2 - sh2</b> 
- <b>Search Head3 - sh3</b>
- <b>Universal Forwarder1 - uf1</b>
- <b>Universal Forwarder2 - uf2</b>
- <b>Universal Forwarder3 - uf3</b>
- <b>Deployment Server - dm</b>
<h2>Program walk-through:</h2>



<br />
Install Splunk in 11 instances running in AWS with labels.
The Deployment server needs to be placed on the network in such a way that all the Universal Forwarders can communicate with it. 
This means that firewalls will need to be opened up for the Splunk Management Port to the Deployment Server (TCP:8089 by default):  <br/>
<img src="https://user-images.githubusercontent.com/112047285/204436198-f5044d28-863b-460f-bb74-4dcaa8cf93cc.png" height="80%" width="80%"/>
<br />
<br />
Configure the Forwarders to connect with the Deployment Server.
Typically, to configure a client to connect to a Deployment Server, we either add it through the CLI (via splunk set deploy-poll servername.mydomain.com:8089)
or we edit the deploymentclient.conf file in /opt/splunk/etc/system/local as shown below and restart. As you can guess, we update the 
targetUri to point to the address and management port of the Deployment Server: <br/>
<img src="https://user-images.githubusercontent.com/112047285/204436706-cb9a49cd-5639-4fa7-97cc-13b39f690173.png" height="80%" width="80%"/>
<br />
<br />
Verify Client connection through Forwarder Management Console:  <br/>
<img src="https://user-images.githubusercontent.com/112047285/204437310-e9ccc044-eaf7-4cd5-9b4c-6eceee99998f.png" height="80%" width="80%"/>
<br />
<br />
Create Apps and Serverclasses and deploy Apps on the Deployment Server to Clients.
The Apps need to all be placed in the /opt/splunk/etc/deployment-apps/ directory. Once these are place here, they will be visible in the
Splunk Web Interface from the Forwarder Management page as shown below:  <br/>
<img src="https://user-images.githubusercontent.com/112047285/204437888-19764f72-b28e-41da-a325-0d08e3348cd0.png" height="80%" width="80%"/>
                                                             
<br />
<br />
Verify Forwarder activities and communication to the indexers on the Monitoring Console .
For this use case,the forwarders are connected to and indexing cluster and Forwarder monitoring is enabled on the 
monitoring console of the Cluster Master:  <br/>
<img src="https://user-images.githubusercontent.com/112047285/204438669-4d55f255-d7c5-4589-8387-85c7ada8c248.png" height="80%" width="80%"/>
<br />
<br />


<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
