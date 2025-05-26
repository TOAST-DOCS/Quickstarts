# Set up your network and create an instance
**Quickstarts > 4. Set up your network and create an instance**

In this learning module, you will learn how to create, remotely access, and run a Linux-based web server on NHN Cloud. NHN Cloud makes it easy for anyone to build a reliable and efficient IT environment with a user-friendly interface and a variety of cloud resources.

![mod_info](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/module_info/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%20%EC%84%A4%EC%A0%95%EA%B3%BC%20%EC%9D%B8%EC%8A%A4%ED%84%B4%EC%8A%A4%20%EC%83%9D%EC%84%B1.png)
## Learning objectives

In this learning module, you'll learn to 

* **Create Instance**
    * Create a Linux instance based on an Ubuntu server
    * Build a basic server environment
    * Detailed configuration, including instance type, key pair settings, security groups, and network settings.
* **Network settings**
    * Set up VPCs, subnets, and routing tables
* **SSH Remote Access**
    * Remote access to the created instance via SSH
* **Install and run a web server**
    * Installing and running a web server
<br></br>

![mod_diagram](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/%EB%AA%A8%EB%93%88%204.%20%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%20%EC%84%A4%EC%A0%95%EA%B3%BC%20%EC%9D%B8%EC%8A%A4%ED%84%B4%EC%8A%A4%20%EC%83%9D%EC%84%B1.png)

<p style="text-align: center; color: black;">Final configuration diagram</p>

## Before you begin

To get started with NHN Cloud, you'll need to prepare the following things

* **Standard Internet browser**
    * You must have the latest version of a browser installed, such as Google Chrome, Microsoft Edge, Firefox, or Safari.
    * JavaScript and cookies must be enabled in your browser settings.
* **Internet connection environment**
    * A stable internet connection is required, with a recommended bandwidth of at least 5 Mbps.
    * You must be able to communicate securely over HTTPS.
* **Member account**
    * You must have an NHN Cloud account with a registered payment method.
    * You must be logged in to the NHN Cloud portal.

**This guide starts with the steps after [3. Set up your IAM account and governance](https://docs.nhncloud.com/en/quickstarts/en/iam-accounts/).**

## Preparing to create an instance

### Step 1. Enable basic infrastructure services

1. Once you're in the NHN Cloud console, make sure you've selected the organization`(MyORG`), project`(MyPRJ)`, and `Korea (Pyeongchon) region`for your lab from the top menu.
2. Click **Select a service**to the right of `MyPRJ`.
3. Click **Select a service**, and then click **Compute - Instance**.
4. In the **Activate service** window, click **OK**.

!!! TIP "Get to know"
    * Activate the service
        * When a service is activated, the Services menu is exposed on the left side of the NHN Cloud Console project screen. You can manage the required resources from that menu.
        * Depending on the region you set, some services may not be available.
        * When you activate an Instance service, the related underlying infrastructure services are activated together.

### Step 2. Basic Network Settings

> Set the names of the basic network resources VPCs, subnets, and routing tables required for remote access to instances.

1. In the left menu, click **Network - VPC**.
2. Select `Default Network` (CIDR: 192.168.0.0/16) and click **Change VPC**.
3. In the **Change VPC** window, type the following and click **OK**.
    * Name: `MyVPC`
4. In the Success window, click **OK**.
5. In the left menu, click **Network - Subnet**.
6. Select `Default Network` (CIDR: 192.168.0.0/24), and then click **Change subnet**.
7. In the **Change Subnet** window, type the following and click **OK**.
    * Name: `MySubnet`
8. In the Success window, click **OK**.
9. In the left menu, click **Network - Routing**.
10. Select VPC-(random name value) and click **Change Routing Table**.
11. In the **Change Routing Table** window, type the following, and then click **OK**.
    * Name: `MyRT`
12. In the Success window, click **OK**.

### Step 3. Create a Linux instance

1. In the left menu, click **Compute - Instance**.
2. Click **Create instance**.
3. In the **Create Instance** window, set the information below and click **Create Instance**.
    * Instance Templates
        * Enabled: `Disabled`
    * Image
        * Click Public Images > Image Name: `Ubuntu Server 22.04 LTS` (You can find this image at the bottom of the list of public images).
    * Instance information
        * Availability zones: `Any availability zone.`
        * Instance name: `linux-server-basic`
        * Instance Type: Click **Select Inst** ance Type > click Instance Type Name: `t2.c1m1` (Classification: Standard, Type: t2, Name: t2.c1m1, vCPU: 1, Memory: 1GB) and click **Select** 
        * Number of instances: `1`
        * Click Keyfair > **+ Generate** > Enter `MyKey` at the bottom of the keyfair selection > **Generate keyfair** > Click **Download keyfair** > Download `MyKey.pem` file
    * Root block storage
    * Block storage type: `HDD`
    * Block storage size (GB): `20` GB
    * Network settings: Select `Create network interface` 
    * Network
        * Under Available subnets, click the `MySubnet (192.168.0.0/24` ) resource to use it as the selected subnet.
    * Floating IP: Click **Change settings** 
        * Enabled: `Enabled`
        * Erasure protection: `Disabled`
        * Subnet to connect to: `MySubnet (192.168.0.0/24)`
    * Security groups
        * Click **Create security group** 
        * In the **Create security group** window, set the information below and click **OK**.
            * Name: `MySG-SSH`
            * Click **+** on Add security rule to add a security rule with the following settings
                * Direction: `Incoming`
                * IP protocol: `SSH`
                    * Select the appropriate IP protocol and the port information is automatically filled in.
                    > [Note] Custom TCP
                    >
                    > * If you select "Custom TCP" as the IP protocol, you can enter your own port value. In the field labeled **1**, enter `22`.
                * Ether: `IPv4`
                * Remote: `CIDR - 0.0.0.0/0`
        * In the Success window, click **OK**
        * In the **Security group selection**, select `MySG-SSH`that you created above
    * Additional block storage: Disabled (default)
    * User scripts: Blank (default)
    * Erasure protection: Disabled (default)
4. In the Instance creation information pane, click **Create instance**.
5. The instance creation operation proceeds. The instance will be created in a few minutes.

!!! tip "Tips"
    * Easy public image search tips
        * Click "All Images" at the bottom of the **Public Images tab** and select `OS → Ubuntu`from the drop-down menu to view and select the target image
        * At the bottom of the **Public Images tab**, type `Ubuntu Server 22.04` in the "Please enter an image name or description" field to the right of "Full Image" to view and select the target image
    * Manage keyfair
        * You can only download a keyfair once when you create a keyfair. Download the file to a safe place to manage it.
        * If a `MyKey` keyfair you have already created is listed in the "Select keyfair" drop-down menu, please select it. However, make sure you have downloaded the MyKey.pem file to a path (directory or folder) that you remember.
        * When you create a key pair, the corresponding key pair value is automatically selected.
        * For more information, see [the Keyfair user guide](https://docs.nhncloud.com/en/Compute/Instance/en/overview/#key-pair).

## Connecting to an instance and running an Nginx web server

### Step 1. Get an SSH remote connection

1. In the left menu, click **Network - Floating IP**.
2. **Copy** and **record**the IP address in the floating IP resource list where the connected device `is linux-server-basic`.
3. Set up an SSH remote connection for the user experience below.

### If you're using Windows

* Click **Start**Windows, then search for and run `Windows PowerShell`.

!!! tip "Tips"
    * Tips for running Windows PowerShell
        * Press the `Windows key (⊞) + R` to launch the Run window.
        * In the Open (O) box, type `powershell.exe`, and then press ENTER to run Windows PowerShell.

* Navigate to the `MyKey.pem` folder, which is the keyfair you downloaded.

```
#Powershell
cd \(name of folder where MyKey.pem file is located)
```
      
* Initialize MyKey.pem permissions.
```
icacls MyKey.pem /reset
```        

* Grant `MyKey.pem` permissions to the account of the user who ran PowerShell.
```
icacls MyKey.pem /grant:r "$($env:username):(r)"
```

* Disables the MyKey.pem file from inheriting permissions from the parent folder.
```
icacls MyKey.pem /inheritance:r
```

!!! tip "Tips"
    * SSH keys or security certificate files should be strictly permission restricted. Windows uses the icacls command to block inheritance and grant per-user read permissions to maintain security.

* Connect to the instance with an SSH command.
```
#PowerShell
ssh -i MyKey.pem ubuntu@copy linux-server-basic floating IP address
```

* Copy the IP address (this is the floating IP address you identified in the step above.)
* When you see "Are you sure you want to continue connecting (yes/no/[fingerprint])?", type `yes`and then press Enter.

* Use the lsb_release command to determine which version of Linux you are currently connected to.
```
#bash
lsb_release -a
```
            
### If you're using macOS

* Launch **the Terminal** app from the Dock, or search for **Terminal**in Spotlight and launch it.

!!! TIP "Get to know"
\* Tips for running Terminal
\* Press `Command (⌘) + Space` to launch Spotlight
\* Type `Terminal` in the input field and press <span style="color:rgb(51, 51, 51);">Return</span>to launch Terminal.

* Navigate to the `MyKey.pem` directory, which is the keyfair you downloaded.
```
#bash
cd /(name of directory where MyKey.pem file is located)
```

* Enter the command below to set the permissions for MyKey.pem.
```
chmod 600 MyKey.pem
```

* Connect to the instance with an SSH command.
```
#PowerShell
ssh -i MyKey.pem ubuntu@copy linux-server-basic floating IP address
```

* When you see "Are you sure you want to continue connecting (yes/no/[fingerprint])?", type `yes` and then press Enter.

* Use the lsb_release command to determine which version of Linux you are currently connected to.
```
#bash
lsb_release -a
```

### Step 2. Get your web server up and running

* While remotely connected to the instance, enter the command below to install the Nginx web server.
```
#bash
sudo apt update -y
sudo apt install nginx -y
```

* Once the installation is complete, verify that your web server is up and running with the command below.
```
#bash
curl localhost
```
    
![4 Network Setup and Instance Creation_Task5 Screenshot_RV1](https://github.com/user-attachments/assets/3bc07c8b-d6e2-431d-aad4-da3bfa6562e4)

## References

* [Region Guide](https://docs.nhncloud.com/en/nhncloud/en/region-guide/)
* [Compute Instance](https://docs.nhncloud.com/en/Compute/Instance/en/overview/)
* [System Image](https://en.wikipedia.org/wiki/System_image)
* [VPC](https://docs.nhncloud.com/en/Network/VPC/en/overview/)
* [Subnet](https://docs.nhncloud.com/en/Network/VPC/en/console-guide/#_4)
* [Floating IP](https://www.nhncloud.com/kr/service/network/floating-ip)
* [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing)
* [Public-key encryption](https://en.wikipedia.org/wiki/Public-key_cryptography)
* [SSH](https://en.wikipedia.org/wiki/Secure_Shell)
* [VM](https://en.wikipedia.org/wiki/Virtual_machine)
* [Internet Gateway](https://docs.nhncloud.com/en/Network/Internet%20Gateway/en/overview/)
* [Nginx](https://en.wikipedia.org/wiki/Nginx)
* [Webserver](https://en.wikipedia.org/wiki/Web_server)
* [Linux](https://en.wikipedia.org/wiki/Linux)
* [Network Interface](https://docs.nhncloud.com/en/Network/Network%20Interface/en/overview/)

## Previous step

* [03-Set up IAM accounts and ](dooray://1387695619080878080/pages/3977493217025617647 "governancepublish")


## Next steps

* [05 - Security ](dooray://1387695619080878080/pages/3959371258218176884 "Settingspublish")
