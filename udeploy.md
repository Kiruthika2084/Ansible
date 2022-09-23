--------------------------------------------------------------------------------------------------------
** IBM URBANCODE INTRODUCTION ** 
--------------------------------------------------------------------------------------------------------

    IBM Urbancode is of 4 types: 

        Urbancode Deploy 
        Urbancode Release 
        Urbancode Build 
        Urbancode Velocity 
    
    UrbanCode Deploy: 

        It is automation solution or tools for deploying applications or database and configuring them into development, testing and production environment. 

    UrbanCode Release: 

        It manages the release of complex applications, infrastructure changes and deployment of multiple applications which helps to reduce error. 
    
    UrbanCode Build: 

        It is continuos integration and build management server. 

    UrbanCode Velocity: 

        It provides tools to understand devops practices, implement changes, review change impact and automate release process. 

--------------------------------------------------------------------------------------------------------
** URBANCODE DEPLOY INTRODUCTION **
--------------------------------------------------------------------------------------------------------

    ARCHITECTURE: 

        1. Service Tier - Provides user interface, workflow. 

        2. Data Tier - store urbancode deploy configuration. codestation is used to store artifacts, logs, flat files. 

        3. Agents - It is deployed on target system which interact with server. 

        4. Agent Relay - coordinate communication between server and agent. 

        5. Plugins allow to integrate urbancode with other systems. 

        6. Snapshots - Transition from one region to another. 


    FEATURES:  

        Multi Tier Application Models - Urbancode can track components that make an application and those can be deployed. 

        Easy Processor Designer - Process are defined using simple drag and drop process designer. 

        Processes are of 2 types: 

            Component process
            Application process 

        It can be deploy to private, public, hybrid cloud types. 

        Inventory - Inventory system is used to track the state of each environment and checks whether they are deployed to target source. 

        Quality gates and approvals 

        It can deploy to windows or linux server. 

--------------------------------------------------------------------------------------------------------

RESOURCES: 

    It is logical deployment target that resolves to agent. 

    Snapshot is a collection of specific version of components and processes. 
    It helps when component is moved from one environment to another environment. 

    Inventory for an environment shows all the deployed component version. 
    Compliance shows whether component version exists in that environment. 
    
--------------------------------------------------------------------------------------------------------

RESOURCE AND AGENT: 

    Agent to deploy our code. 

COMPONENT: 

    Component Template 
    Version     (Import version, which version to deploy)
    Component Process  (Define the steps)

APPLICATION: 

    Component  (Add components to applications)
    Application Process (It is application level process which differs from component process)
    Application Environment (Which environment to deploy)

WORKFLOW FOR ANY DEPLOYMENT: 

    Create a base resource -> Add agent to base resource -> Create a component -> Add version to the component -> Create component process -> Create an application -> Add component to the application -> Create application process -> Create application environment -> Add base resource to application environment -> Add component to resource tree -> Run the process in application environment. 


Agent is a light weight process that runs on target computer and does the actual work of deploying components. 

Agent must be running on a target computer system.  Target computer can be virtual or physical image.


---------------------------------------------------------------------------------------------------------
CONFIGURE THE SERVER, AGENT AND ARTIFACTS:  
---------------------------------------------------------------------------------------------------------

1. Provision a computer for use as the target computer. 
2. Install an agent on the target computer. 
3. Designate the agent for importing versions. 
4. Verify agent is installed by clicking Resources -> Agents. 
        Agents must be included with status as online. 
5. Add the agent as a resource: 
        a. Resources -> Resource Tree -> Create Top Level Group -> Enter "Name" (This group is act as container for resources such as agents and components )
        b. On Resource Group, Click "Actions". 
                It gives, Add Group, Add Agent, Add Agent Pool.
        c. Select "Add Agent" , Select the agent in Agent List -> Save
6. On the target computer, add files that represents the deployable artifacts to the file system. 
        a. Create the directory and name it as helloworld on file system where agent is located.
        b. in helloworld directory, create subdirectory that is named as 1.0 
        c. in subdirectory 1, place several files. 
                    Files represents the artifacts that the agent deploys by using the process.

            /home/user/artifacts/shared/helloworld/1.0 directory. 

---------------------------------------------------------------------------------------------------------
COMPONENTS: 
---------------------------------------------------------------------------------------------------------

    Components contains artifacts and processes. 
    Artifacts include runnable files, images, database, configuration instructions. 
    Processes define the activities that components can perform. 

    To create a component Procedure: 

        1. Components -> Create component (Define the component and location of artifacts)
        2. In Name, type "helloworld"
        3. In Source Configuration Type, select the type of artifacts. 
                Artifactory, Docker, Filesystem, Filesystem(versioned), Git, Maven, Nuget etc. 
                FileSystem(versioned) - Looks for the artifacts on the file system. 
                NONE - Can be used for configuration-type or operational-type deployments. 

                FileSystem
                FileSystem(Versioned) - It consider sub directories as versioned, so use upto /home/user/artifacts/shared/helloworld. 
        4. If filesystem(versioned) selected, give "Base Path" as /home/user/artifacts/shared/helloworld
        5. Keep others as default and save. 


    To create a component property: 
        
        Property is user defined parameter that can be set at design time or run time. 
        Properties are of, 
            a. Component properties 
            b. Environment Property Definitions 
            c. Resource Property Definitions
            d. Version Property Definitions

        1. On component -> Configuration -> Component Properties -> Add Property -> Name & Value (Value can be secure) 


    To import component version: 

        The server creates a version of the component that is based on the folder and imports the files in hellowordld/1.0 directory. 
        If other folders in helloworld directory, that too imported and versions would be created. 

        1. Component -> Versions -> Import New Versions 
        2. Versions will be listed. 
        3. Components -> Versions -> Click versions.
                Will show Status and Artifacts.


Component is ready to use. 

Components process describe different ways to use a component such as how to deploy it, install it, update it or do other tasks. 


COMPONENT PROCESS: 

    Component process are user defined tasks that operate on components. 
    It is succession of commands that are called STEPS. Steps can manipulate files, run system commands, set properties , pass information and run programs. 

    A component can have any number of processes defined but component must have atlease one process. 

    Component process for the component. Application process that uses component process to deploy the component. 

    To create the component process: 

        1. Component -> Processes -> Create process. 

                Name, Process Type (Deployment, Configuration Deployment, Uninstall, Operation with version, Operation with no version needed), Inventory status (Active, FullBundlePromote, Staged) , Default working directory. 

            Default working directory - folder that agent uses to do its work such as downloading artifacts and creating temporary files. 

        2. Once process created, use Design to create STEPS.


---------------------------------------------------------------------------------------------------------
CREATE AN APPLICATION: 
---------------------------------------------------------------------------------------------------------

    Application manages components, typically by deploying them into environments. 

    To create an application, 
        1. Identify the component that it manages. 
        2. Define atleast one environment into which components are deployed. 
        3. Create a process to do that work. 

        Environment maps components to agents and handles inventory. 
        Application process is similar to component process and direct underlying component process and orchestrate multi component deployment. 


    Procedure to create application: 

        1. Application -> Create Application -> Name -> Create 

                Notification Scheme - Sends an email when deployment finishes and play a role in deployment approvals. 
        
        2. Add the component to the application: 

                From application -> Components tab -> Add Component -> Select the component and Save. 

        Application can have any number of components and components can be included in any number of applications. 


APPLICATION ENVIRONMENT: 

    When create an environment, map resources to it that define where the parent application can run deployments. 

    Environment is user defined collection of resources that identify the components that can be deployed by parent application along with agents that to do the work. 

    Applications -> Select application -> Environment -> Create environment (name) -> Save

    Under Environment -> Resources -> Add Base Resources -> Select Resource 
            Under Resource only, agent is added. 
    
    Map the component to agent resource: 

        1. Hover the mouse with agent resource, click Actions -> Add component. 
        2. Select the component and save. 

    NOTE: Only components added to parent application are available to be mapped to agent resource. 
    Inorder for a component to be deployed by an application, Component must be added to application and also mapped to agent type resource. 


    Add a component tag: 

        Applications -> Environment -> Resources -> Actions -> Add component tag. 


Create an Application Process: 

    Application process direct underlying component process and orchestrate multi component deployments.

    Application process consists of STEPS that configure with process editor. 

    Create an application process that install component by calling component process. 

    1. Application -> Click application -> Processes -> Create process (Name, Inventory Management, Offline agent handling )
    2. add an install component. 
        a. select component 
        b. in component process, select component process. 
        c. Limit to tag - used to deploy only components with this tag. 

Deploy the component: 

    Application -> Click application -> Request process (On same row)

---------------------------------------------------------------------------------------------------------

https://www.devopsschool.com/tutorial/udeploy/html/install-and-configure-udeploy-agent.html

https://localhost:8443 

Git Hub (Source code Management) ->  Jenkins (Code Compilation) -> JFrog Artifactory (Jenkins PUSH the code to Artifactory) -> UDeploy (Artifact deployment ,  Udeploy PULLS the artifact from artifactory)

sudo wget https://ak-delivery04-mul.dhe.ibm.com/sar/CMA/RAA/06t7l/1/ibm-ucd-6.2.4.0.898469.zip


https://github.com/a5221985/tutorials/blob/master/Deployment_Tools/udeploy.md
