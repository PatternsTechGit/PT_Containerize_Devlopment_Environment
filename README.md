![](/after/images/bn.jpg)

![](/DockerDev/src/assets/images/20.jpg)
# Containerize *Development* Environment using **Docker** & **VSCode**

### **What does Containerization mean?**

Containerization is a form of virtualization where applications run in isolated user spaces, called containers, while using the same shared operating system (OS).

### **What is Docker Container?**

Docker is an open source platform that **enables developers to build, deploy, run, update and manage containers**—standardized, executable components that combine application source code with the operating system (OS) libraries and dependencies required to run that code in any environment.

------------
## About this exercise 


### In this exercise we will:
- **STEP1:** Create new sample **Angular** app
- **STEP2:** Install Windows Subsystem for Linux (WSL)
- **STEP3:** Install **Docker** for windows
- **STEP4:** Install the Visual Studio Code **Remote - Containers extension**
- **STEP5:** Creating `devContainer` for our Angular UI
- **STEP6:** Open the `devContainer`
- **STEP7:** Using the `devContainer`
- **STEP8:** Deep Dive into `devcontainer.json` code
- **STEP9:** Deep Dive into `Dockerfile`

-----------------------
### **STEP 1: Create new Web-Application in Angular**

*Note: You can use any development environments or web app. We are creating Angular sample app for this lab exercise and will try to install all necessary dependencies and extensions for it.

- Create new angular app by pasting this code in terminal
    ```java
        ng new
    ```
- Give name to your webapp. In our case it is **DockerDev**
- Then it will ask you about adding routing to app. As this is just sample app we will select 'N'
- Select 'Y' for **CSS** if you want to style your webapp later on. Then press **Enter**
- Your new Angular App is created. Now we will run and see if its working.
    - Move to current directory
        ```powershell
            cd DockerDev
        ```
    - Install node modules 
        ```powershell
            npm install
        ```
    - Start your application
        ```powershell
            npm start
        ```
- Now open this URL http://localhost:4200/ in browser and see the outcome.
![](/DockerDev/src/assets/images/1.jpg)

### **STEP 4: Installing WSL:**

The Windows Subsystem for Linux (WSL) is a feature of the Windows operating system that enables you to run a Linux file system, along with Linux command-line tools and GUI apps, directly on Windows, alongside your traditional Windows desktop and apps. See the about page for more details.

- Install it by entering this command in an **administrator** PowerShell or Windows Command Prompt and then restarting your machine
    ```powershell
    wsl --install
    ```
    ***Note:** The above command only works if WSL is not installed at all, if you run `wsl --install` and see the WSL help text, please try running `wsl --list --online` to see a list of available distros and run `wsl --install -d <DistroName>` to install a distro.

----------

### **STEP 3: Install Docker Desktop:**

Follow these steps to install docker:

- Download [Docker Desktop for Windows](https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe)

- Follow the usual installation instructions to install Docker Desktop. If you are running a supported system, Docker Desktop prompts you to enable WSL 2 during installation. Read the information displayed on the screen and enable WSL 2 to continue.
- Start Docker Desktop from the Windows Start menu.
- From the Docker menu, select **Settings > General**.
![](/DockerDev/src/assets/images/2.jpg)

- Select the Use **WSL 2 based engine** check box.

- If you have installed Docker Desktop on a system that supports WSL 2, this option will be enabled by default.

- Click **Apply & Restart**.

That’s it! Now *docker commands* will work from Windows using the new WSL 2 engine.

------------  
### **STEP 4: Install the Visual Studio Code Remote - Containers extension**

Now you will learn how to set up VS Code development containers. 
![](/DockerDev/src/assets/images/3.png)

The first thing we are going to do after installing the prerequisites is to install the **VS Code Remote - Containers extension.**

- Open VS Code and navigate to the Extensions item in the sidebar. Next, search for **“remote containers”**

![](/DockerDev/src/assets/images/4.png)

Click on **Remote - Container** and click the **Install** button which becomes available after clicking on the button.

----------
### **STEP: 5 Create DevContainer:** 

- Open the Command pallette in VS Code by using `Ctrl+Shift+P` (Linux/Windows) or `Cmd+Shift+P` (macOS) and search for Remote Containers

- Choose Add Development Container Configuration Files.

- A number of templates is available. You may choose the one you want, but for this demo we will be using the **Node and Type Script** template
![](/DockerDev/src/assets/images/5.jpg)

- We have to select the **version** of Nodejs. In our case it will be Version 14
![](/DockerDev/src/assets/images/6.jpg)

- Finally, we may choose to install additional tools into our new container image. Here are the ones I chose
![](/DockerDev/src/assets/images/7.jpg)
Next, we will be prompted which version of the tools we selected we want to install. In a real world scenario, we would typically pin specific versions for consistency and stability. For this demo, I chose latest for all prompts.

- At the end, we should now automatically get a subfolder called `.devcontainer` along with two files: `devcontainer.json` and `Dockerfile`
![](/DockerDev/src/assets/images/8.png)

This folder is typically stored at the root of a source control repository, hence it becomes available for all other people who have cloned the repository.

------------
**STEP 6: Open devContainer**

- When VS Code detects the two files we created in the previous step at the root of a folder, it will prompt whether we want to **Reopen in Container**
![](/DockerDev/src/assets/images/9.png)
- Should the **Remote - Containers** extension not be installed, you will be prompted to install it.

- An alternative approach to reopen the current folder inside the dev container is to once again open the Command pallette and search for Remote Containers. Next, select **Reopen in Container**
![](/DockerDev/src/assets/images/10.png)

- VS Code will now reopen and the following status will be shown
![](/DockerDev/src/assets/images/11.png)

- Under the hood, the command `docker build` is now being run against the Dockerfile we created previously. This will result in a local container image becoming available on our machine.

- The build step might take from a few seconds until a few minutes depending upon the size of the base image we chose, the number of tools to install, the bandwidth of our internet connection as well as the performance of the local machine.

- The build step is only needed whenever a change has been made to the configuration files for the development container, so the next time you open the folder inside the dev container, it will typically open in seconds - similar to when opening a regular local folder.

-----------------

### **STEP 7: Using the Dev Container**
- When the build process has completed, you should see the folder opened in the VS Code file explorer as usual. The difference is that it is now opened within the dev container, which is indicated here:
![](/DockerDev/src/assets/images/16.jpg)

- You can click on the green “Dev Container” button to bring up a menu with relevant options such as reopening the folder locally and so on
![](/DockerDev/src/assets/images/17.png)

- Next, let us explore how extensions comes into play when working with dev containers. Navigate to the Extensions item on the sidebar. You should see the following:
![](/DockerDev/src/assets/images/18.png)

- This means we can have separate VS Code extensions per container, and we don`t have to “clutter” our local VS Code instance with numerous extensions (which may also decrease performance/load times).

- To install extensions inside the dev container, simply search and install them from the extensions item as usual. In this example, I will go ahead and install the PowerShell extension by clicking Install on the bottom recommendation.

------------

### **STEP 8: Deep Dive into `devcontainer.json` code**

At the moment **devcontainer.json** contains the the following code

```json
{
	"name": "Node.js & TypeScript",
	"build": {
		"dockerfile": "Dockerfile",
		// Update 'VARIANT' to pick a Node version: 18, 16, 14.
		// Append -bullseye or -buster to pin to an OS version.
		// Use -bullseye variants on local on arm64/Apple Silicon.
		"args": { 
			"VARIANT": "14"
		}
	},

	// Configure tool-specific properties.
	"customizations": {
		// Configure properties specific to VS Code.
		"vscode": {
			// Add the IDs of extensions you want installed when the container is created.
			"extensions": [
				"dbaeumer.vscode-eslint"
			]
		}
	},

	// Use 'forwardPorts' to make a list of ports inside the container available locally.
	// "forwardPorts": [],

	// Use 'postCreateCommand' to run commands after the container is created.
	// "postCreateCommand": "yarn install",

	"remoteUser": "node",
	"features": {
		"docker-from-docker": "latest",
		"git": "latest",
		"powershell": "latest"
	}
}
```

- As we can see, we can customize it further by adding extensions for example. If adding the id of the PowerShell extension (ms-vscode.PowerShell) to the “extensions” property in the json-file - it will get installed automatically when the image is built.

- Here is an example for the extensions I typically use in my Infrastructure as Code dev containers:
    ```json
    "extensions": ["ms-vscode.powershell","ms-vscode.azure-account","hashicorp.terraform",,"ms-kubernetes-tools.vscode-kubernetes-tools","ms-kubernetes-tools.vscode-aks-tools"],
    ```
 - Now if we reload the container we will see above extensions added to our list.

 ---------
 ### **STEP 9: Using Dockerfile:**

 If you have worked with containers before, there is nothing special about this file. It is a regular Dockerfile which you can define as you want. In our example, we happened to use a predefined base-image - but we could choose to replace this with your own image. 

 ```dockerfile
 # [Choice] Node.js version (use -bullseye variants on local arm64/Apple Silicon): 18, 16, 14, 18-bullseye, 16-bullseye, 14-bullseye, 18-buster, 16-buster, 14-buster
ARG VARIANT=16-bullseye
FROM mcr.microsoft.com/vscode/devcontainers/typescript-node:0-${VARIANT}
 ```
 A Dockerfile defining your environment. The definition is whatever you need it to be, with no special requirements for VS Code. 
 
 For example, you could base it on node:latest and add some extra packages if you wish.

------------------

### **CONCLUSION**

In this lab you learned how to get started with VS Code Development containers.

The extension makes it very convenient to perform development work inside a running container based on a standardized setup which can be shared across teams using regular source control.

Now you can start using them both for your personal projects as well as team projects in order to benefit from consistent development environments.
