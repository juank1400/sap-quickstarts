FABRIC SAP Synchronous RFC Destination Endpoint Quick Start
===========================================================
**Demonstrates the sap-srfc-destination component running in a Fabric camel runtime.**  
![SAP Tool Suite](../../sap_tool_suite.png "SAP Tool Suite")

* * *
Author: William Collins - JBoss Fuse Team  
Level: Advanced  
Technologies: Fabric, Camel, SAP  
Summary: This quick start demonstrates how to configure and use the sap-srfc-destination component in a JBoss Fuse Fabric environment to invoke remote function modules and BAPI methods within SAP. This component invokes remote function modules and BAPI methods within SAP using the *Synchronous RFC* (sRFC) protocol.  
Target Product: Fuse  
Source: <https://github.com/jboss-fuse/fuse/tree/master/quickstarts/camel-sap>  

* * *

What is it?  
-----------  

This quick start shows how to integrate Apache Camel with SAP using the JBoss Fuse SAP Synchronous Remote Function Call Destination Camel component. This component and its endpoints should be used in cases where Camel routes require synchronous delivery of requests to and responses from an SAP system.  

This quick start uses XML files containing serialized SAP requests to query Customer records in the Flight Data Application within SAP. These files are consumed by the quickstart's route and their contents are then converted to string message bodies. These messages are then routed to an `sap-srfc-destination` endpoint which converts and sends them to SAP as `BAPI_FLCUST_GETLIST` requests to query Customer records.  

**NOTE** The sRFC protocol used by this component delivers requests and responses to and from an SAP system **BEST-EFFORT**. When the component experiences a communication error when sending a request to or receiving a response from an SAP system, it will be *in doubt* whether the processing of a remote function call in the SAP system was successful. For the guaranteed delivery and processing of requests in an SAP system please see the JBoss Fuse SAP Transactional Remote Function Call Destination Camel component.     

In studying this quick start you will learn:

* How to define, build and deploy a JBoss Fuse Fabric profile that configures a Fabric container to use the JBoss Fuse SAP Synchronous Remote Function Call Destination Camel component
* How to define a Camel route containing the JBoss Fuse SAP Synchronous Remote Function Call Destination Camel component using the Blueprint XML syntax.
* How to use the JBoss Fuse SAP Synchronous Remote Function Call Destination Camel component. 
* How to configure connections used by the component.  

For more information see:

* <https://access.redhat.com/documentation/en-US/Red_Hat_JBoss_Fuse/6.2/html/Apache_Camel_Component_Reference/SAP.html> for more information about the JBoss Fuse SAP Camel components 
* <https://access.redhat.com/site/documentation/JBoss_Fuse/> for more information about using JBoss Fuse

System requirements
-------------------

Before building and running this quick start you will need:

* Maven 3.0.4 or higher
* JDK 1.7 or 1.8
* A JBoss Fuse 6.2.1 container running within a Fabric
* SAP JCo3 and IDoc3 libraries (sapjco3.jar, sapidoc3.jar and JCo native library for your OS platform)
* SAP instance with [Flight Data Application](http://help.sap.com/saphelp_erp60_sp/helpdata/en/db/7c623cf568896be10000000a11405a/content.htm) setup.

Configuring the QuickStart for your environment
-----------------------------------------------

To configure the quick start for your environment:

1. Deploy the JCo3 library jar and native library (for your platform) and IDoc3 library jar to a network accessible location in your environment.
2. Edit the profile configuration file (`src/main/fabric8/io.fabric8.agent.properties`) in the quick start and set the network locations of the SAP libraries in your environment:

>lib.sapjco3.jar=http://host/path/to/library/sapjco3.jar  
>lib.sapidoc3.jar=http://host/path/to/library/sapidoc3.jar  
>lib.sapjco3.nativelib=http://host/path/to/library/\<native-lib\>  

3. Edit the camel context file (`src/main/fabric8/camel.xml`) and modify the `quickstartDestinationData` bean to match the connection configuration for your SAP instance.
4. Edit the project's request file (`src/data/request1.xml`) and enter the SID of your SAP in the location indicated.

Build and Run the Quickstart
----------------------------

To build and run the quick start:

1. Change your working directory to the quick start directory `sap-srfc-destination-fabric`.
* Run `mvn clean install` to build the quick start.
* In your JBoss Fuse installation directory run, `./bin/fuse` to start the JBoss Fuse runtime.
* It is assumed that you have already created a fabric and logged into a container called `root` from the JBoss Fuse console.
* In the quick start directory run `mvn fabric8:deploy` to upload the quick start's profile (`sap-srfc-destination-fabric`) to the fabric container.
* Create a new child container and deploy the `sap-srfc-destination-fabric` profile in a single step, by entering the
 following command in the JBoss Fuse console:

        fabric:container-create-child --profile sap-srfc-destination-fabric root mychild

7. Wait for the new child container, `mychild`, to start up. Use the `fabric:container-list` command to check the status of the `mychild` container and wait until the `[provision status]` is shown as `success`.

	**Note:** You may need to restart the `mychild` container to successfully provision it.  

8. Log into the `mychild` container using the `fabric:container-connect` command, as follows:

		fabric:container-connect mychild
				
9. In the `mychild` container's JBoss Fuse console, run `log:tail` to monitor the container's log.
10. Copy the request file (`src/data/request1.xml`) in the project to the input directory (`instances/mychild/work/sap-srfc-destination-fabric/input`) of the quick start route.
11. In the container's log observe the request sent and the response returned by the endpoint.

Stopping and Uninstalling the Quickstart
----------------------------------------

To uninstall the quick start and stop the JBoss Fuse run-time perform the following in the JBoss Fuse console:

1. Disconnect from the child container by typing `Ctrl-D` at the console prompt.
2. Stop and delete the child container by entering the following command at the JBoss Fuse console:

		fabric:container-stop mychild
		fabric:container-delete mychild

3. Delete the quick start's profile by entering the following command at the JBoss Fuse console: 

		fabric:profile-delete sap-srfc-destination-fabric

4. Run `osgi:shutdown -f` to shutdown the JBoss Fuse runtime.

