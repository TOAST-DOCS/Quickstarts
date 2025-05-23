# Backup and recovery
**Quickstarts > 9. Backup and Recovery**

In this learning module, you will learn how to secure and recover applications and data in the NHN Cloud environment. Block storage replication, instance image creation, and image-based creation to prevent data loss and build a system that enables rapid recovery.

![mod_info](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/module_info/%EB%B0%B1%EC%97%85%20%EB%B0%8F%20%EB%B3%B5%EA%B5%AC.png)
## Learning objectives

In this learning module, you'll learn to

* **Create Instance Image**
    * Periodic backup of server-wide images using the instance image creation feature
    * Creating a new instance after saving the state of a running instance as an instance image
* **Replicating and attaching block storage**
    * Leverage block storage cloning to clone existing block storage and associate it with an instance
<br></br>

![mod_diagram](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/%EB%AA%A8%EB%93%88%209.%20%EB%B0%B1%EC%97%85%20%EB%B0%8F%20%EB%B3%B5%EA%B5%AC.png)

## Before you begin

Before you begin this learning module, here's what you need to know

* **Standard Internet browser**
    * You must have the latest version of a browser installed, such as Google Chrome, Microsoft Edge, Firefox, or Safari.
    * JavaScript and cookies must be enabled in your browser settings.
* **Internet connection environment**
    * A stable internet connection is required, with a recommended bandwidth of at least 5 Mbps.
    * You must be able to communicate securely over HTTPS.
* **Member account**
    * You must have an NHN Cloud account with a registered payment method.
    * You need to log in to the NHN Cloud homepage.

**This guide begins with the steps after [8. Setting up monitoring](https://docs.alpha-nhncloud.com/ko/quickstarts/ko/configure-monitoring/).**

## Creating instances and attaching block storage via instance images

### Step 1. Create an instance image

> Create an image of the `linux-server-basic` instance you created in the previous learning module. A running instance does not guarantee integrity when creating an image, so stop the instance before creating the image.

1. From the top menu of the NHN Cloud console, select the organization`(MyORG`), project`(MyPRJ`), and `Korea (Pyeongchon) region`for the lab.
2. Click **Compute - Instance**in the left menu of the console pane.
3. In the list of instances, click to select the `linux-server-basic` instance.
4. Click **---** above the list of instances, and then click **Stop instances**.
5. In the **Stop Instance** window, click **Stop**.
6. In the Success window, click **OK**.
7. Above the list of instances, click **Create image**.
8. In the **Create image** window, set the information below and click **OK**.
    * Image name: `linux-server-basic-image`
    * Delete protection: `Disabled`
    * **Creating an image of a running instance does not guarantee its integrity. Please select the checkbox to proceed.** Select the checkbox.
    > [Caution]
    >
    > * Create a running instance image
    >     * When you image a running instance, there are no file system integrity guarantees, and this can sometimes cause problems running the application. We recommend that you stop the instance whenever possible before creating an image.
9. In the Success window, click **OK**.
10. Click **Compute - Image**in the left menu of the console window.
11. On the Images screen, in the list of images, see that `the linux-server-basic-image`is being created. When creation is complete, the status indicator for that image will be green.

### Step 2. Create a new instance with an instance image

> Create a new `linux-server-recovery` instance using the `linux-server-basic-image` instance image you created in step 1.

1. Click **Compute - Instance**in the left menu of the console window.
2. Click **Create instance**.
3. In the **Create Instance** window, set the information below and click **Create Instance**.
    * Instance Templates
        * Enabled: `Disabled`
    * Image
        * Choose a **private image** image name: `linux-server-basic-image` 
    * Instance information
        * Availability zones: `Any availability zone.`
        * Instance name: `linux-server-recovery`
        * Instance Type: **Select Instance Type** > click Instance Type Name: `t2.c1m1` and click **Select** 
        * Number of instances: `1`
        * Key pair: `MyKey`
    * Root block storage
        * Block storage type: `HDD`
        * Block storage size (GB): `20` GB
    * Network settings: Select `Create network interface` 
    * Network
        * Under Available subnets, click the `MySubnet (192.168.0.0/24` ) resource to use it as the selected subnet.
    * Floating IP: Click **Change settings** 
        * Enabled: `Enabled`
        * Delete protection: `Disabled`
        * Subnet to connect to: `MySubnet (192.168.0.0/24)`
    * Security groups
        * Select a security group: Select `MySG-SSH, MySG-HTTP` 
    * Additional block storage: Disabled (default)
    * User scripts: Blank (default)
    * Erasure protection: Disabled (default)
4. In the Instance creation information pane, click **Create instance**.
5. The instance creation operation proceeds. The instance creation will be complete in a few minutes.

### Step 3. Access the instance you created

> Learn how to connect via the floating IP address of the `linux-server-recovery` instance you created in step 2.

1. Click **Network - Floating IP** in the left menu of the console window.
2. **Copy** and **record**the IP address in the list of floating IP resources whose associated device `is linux-server-recovery`.
3. Open a new window in your web browser and type `http://복사한 linux-server-recovery floating IP address`to confirm your connection.
4. Confirm that the web page opens.
5. Run the command below **in****Terminal** or **PowerShell**to connect remotely.

```
#PowerShell
cd /(name of directory where MyKey.pem file is located)
    
ssh -i MyKey.pem ubuntu@copy linux-server-recovery floating IP address
```

* When you see "Are you sure you want to continue connecting (yes/no/[fingerprint])?", type `yes`and then press Enter.
* Use the lsb_release command to determine which version of Linux you are currently connected to.

```
#bash
lsb_release -a
```

### Step 4. Clone an existing block storage and attach it to an instance

> Clone the `MyBS` block storage you created in the previous learning module, connect to the `linux-server-recovery` instance, and query the data in the `MyBS` block storage.

1. Click **Storage - Block Storage** in the left menu of the console window.
2. On the **Block Storage > Manage** screen, select `MyBS`from the list of block stores.
3. Click **Duplicate block storage**.
4. On the **Block Storage Replication** screen, set the information below and click **OK**.
    * Target project: `Same project`
    * Region: Korea (Pyeongchon)
    * Block storage name: `MyBS-Duplicate`
    * Block storage size: `10 GB`(fixed)
    * Block storage type: `HDD`
    * Availability zone: The **same availability** zone as the block storage whose connection information in the block storage list `is /dev/vda in linux-server-recovery.`
    * **Block storage on a running instance does not guarantee file system integrity, please select the checkbox to proceed.** Select the checkbox.
5. In the Success window, click **OK**.
6. Click the **Replication Results** tab to the right of the **Manage** Block Storage **tab**.
7. In the replication results list, verify that the status **is SUCCESS**.
8. Click the **Manage** tab.
9. Select `MyBS-Duplicate`and click **Manage Connection**at the top.
10. In the **Manage Block Storage Connections** window, set the information below and click **Connect**.
    * Connect to the instance: `linux-server-recovery`
11. In the Success window, click **OK**.
12. While remotely connected to the `linux-server-recovery` instance, remount it to the /mnt/vdb path with the command below.

```bash
sudo mount -a
```

* Use the command below to check the value of the replicated block storage.

```bash
cat /mnt/vdb/employees.csv
```

Verify that the results from the database are retrieved as a CSV file.

## References

* [Image](https://docs.nhncloud.com/ko/Compute/Image/ko/overview/)
* [Create an image](https://docs.nhncloud.com/ko/Compute/Instance/ko/console-guide/#_13)
* [Replicating block storage](https://docs.nhncloud.com/ko/Storage/Block%20Storage/ko/console-guide/#_11)
* [Snapshot](https://en.wikipedia.org/wiki/Snapshot_(computer_storage))
* [Backup](https://en.wikipedia.org/wiki/Backup)

## Previous step

* [8. Monitoring settings](https://docs.alpha-nhncloud.com/ko/quickstarts/ko/configure-monitoring/)

## Next steps

* [10. Optimize scalability and performance](https://docs.alpha-nhncloud.com/ko/quickstarts/ko/optimze-performance/)
