# Create and connect storage
**Quickstarts > 7. Create and connect storage**

In this learning module, you will learn how to enable and use storage services through the NHN Cloud console. NHN Cloud's **storage services**provide a reliable and scalable solution for storing and managing your data.

![mod_info](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/module_info/%EC%8A%A4%ED%86%A0%EB%A6%AC%EC%A7%80%20%EC%83%9D%EC%84%B1%20%EB%B0%8F%20%EC%97%B0%EA%B2%B0.png)
## Learning objectives

In this learning module, you'll learn to

* **Utilizing the Storage service**
    * Object Storage vs. Block Storage differences and use cases
    * Create a Block Storage volume and associate it with an instance
    * Creating Object Storage containers and uploading and downloading objects
<br></br>

![mod_diagram](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/%EB%AA%A8%EB%93%88%207.%20%EC%8A%A4%ED%86%A0%EB%A6%AC%EC%A7%80%20%EC%83%9D%EC%84%B1%20%EB%B0%8F%20%EC%97%B0%EA%B2%B0.png)

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
    * You need to log in to the NHN Cloud homepage.

**This guide starts after step [6. Create and connect to the database](https://docs.alpha-nhncloud.com/ko/quickstarts/ko/create-database/).**

## Creating block storage and retrieving data

> Create a block storage, connect it to a Linux instance, and query the data in the block storage.

### Step 1. Create block storage and connect to a Linux instance

> Attach the block storage resource `to the`Linux instance `linux-server-basic`that you created in Module 3.

1. Click **Storage - Block Storage**in the left menu of the console window.
2. **On the Block Storage > Manage screen,**check the availability zone of the block storage in the block storage list whose connection information is **/dev/vda on linux-server-basic**.
3. **+ Create block storage**.
4. In the **Create block storage** window, set the information below and click **OK**.
    * Block storage name: `MyBS`
    * Block storage type: `HDD`
    * Block storage size (GB): `10 GB`
    * Availability zone: The **same availability** zone as the block storage whose connection information `is /dev/vda on linux-server-basic`in the block storage list.
5. In the Success window, click **OK**.
6. Select `MyBS`and click **Manage connections**above.
7. In the **Manage Block Storage Connections** window, set the information below and click **Connect**.
    * Connect to the instance: `linux-server-basic`
8. In the Success window, click **OK**.

### Step 2. Set up, format, and mount additional block storage partitions

* While remotely connected to the Linux instance `linux-server-basic`, run the command below to create a partition `on the`block storage `MyBS`created in step 1, and then format and mount it.
```
#bash
echo -e "n\np\n1\n\n\nw" | sudo fdisk /dev/vdb
```

!!! TIP "Get to know"
\* Domain name guide
\* fdisk command description
\* fdisk **`n`**: Create a new partition (optional: `n`)
\* * n **`p`**: Select partition type (default: Primary)
\* p **`1`**: Partition number (default: 1)
* **First sector**: press Enter to select Default
* **Last sector**: press Enter to select the maximum size as default
\* 1 **`w`**: Save changes and exit

* Run the command below to format the partition.
```bash
sudo mkfs -t xfs /dev/vdb1
```

* Run the command below to create the /mnt/vdb directory so that you can mount the partition you added to the /mnt/vdb path.
```
sudo mkdir /mnt/vdb
```

* Add a setting to the /etc/fstab file by running the command below to auto-mount the partition you added at boot time.
```
sudo sh -c 'echo "UUID=$(sudo blkid -s UUID -o value /dev/vdb1) /mnt/vdb xfs defaults,nodev,noatime,nofail 1 2" >> /etc/fstab'
```

* Run the command below to mount all filesystems defined in the /etc/fstab file, and change permissions to allow all users to create, modify, and delete files in /mnt/vdb.
```bash
sudo mount -a
sudo chmod 777 /mnt/vdb
```

* Enter the command below to verify that the additional block storage is set up correctly.
```bash
df /dev/vdb1
```

Verify **that the corresponding filesystem, capacity, and mount path are retrieved**.
<br></br>

![pic1](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/content_image/%EC%8A%A4%ED%86%A0%EB%A0%88%EC%A7%80%20%EC%83%9D%EC%84%B1%20%EB%B0%8F%20%EC%84%A4%EC%A0%95_%EC%9E%91%EC%97%852.png)

<p style="text-align: center; color: black;">Output screen</p>

### Step 3. Create material in additional block storage

* While remotely connected to `linux-server-basic`, run the command below to create block storage.
```bash
export MYSQL_PWD=nhnpassword
```

* Create a `mysql-db-basic` data lookup in the block storage you created.
```bash
mysql --host=(virtual IP address of mysql-db-basic instance) --user=nhncloud -e "SELECT * FROM employees.employees LIMIT 30;" -B --batch > /mnt/vdb/employees.csv
```

* The data created by the command below (<span style="color:rgb(0, 82, 204);"><strong>csv file)</strong></span>created with the command
```bash
cat /mnt/vdb/employees.csv
```

![pic2](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/content_image/%EC%8A%A4%ED%86%A0%EB%A6%AC%EC%A7%80%20%EC%83%9D%EC%84%B1%20%EB%B0%8F%20%EC%84%A4%EC%A0%95_%EC%9E%91%EC%97%853.png)

<p style="text-align: center; color: black;">Output screen</p>

## Create object storage and retrieve data

> Enable the Object Storage service to create a container and upload objects to it. Save the object to your Linux instance and then connect to it to see if the data is output.

### Step 1. Enable the Object Storage service and create a container

1. Click the **"Select Services"** tab located to the right of `your MyPRJ` project.
2. On the screen that appears after you click **Select Services**, click **All Services - Storage - Object Storage**.
3. In the **Activate service** window, click **OK**.
4. If the service is activated correctly, you will be taken to the Object Storage screen. If you are not taken to the screen, click **Storage - Object Storage**in the left menu of the console pane.
5. **+ Create container**.
6. In the **Create Container** window, enter the information below and click **OK**.
    * Name: `myobs`
    * Access policy: `PUBLIC`
    * Storage class: `Standard`
    * Set object locking: `Disabled`
    * Encryption settings: `Disabled`
7. In the Success window, click **OK**.

### Step 2. Change the Linux instance web source using object storage

* Run a new **terminal** or **PowerShell**in your work environment.
* Use the command below to create the web-sample directory and save the index.html file.
```
#PowerShell
mkdir /web-sample
```

```PowerShell
    echo '<!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>::: Welcome to NHN Cloud :::</title>
        <script>
            let countdownTimer;
    
            function fetchServerIP() {
                fetch("/server-info")
                    .then(response => response.json())
                    .then(data => {
                        const serverIP = data.serverIP;
                        document.getElementById("server-ip").innerText = "Server IP Address: " + serverIP;
                    })
                    .catch(error => {
                        document.getElementById("server-ip").innerText = "Unable to fetch server IP address";
                    });
            }
    
            function startStressTest() {
                const button = document.getElementById("start-btn");
                button.innerText = "Starting...";
                let timeLeft = 120;
                const countdownDisplay = document.getElementById("countdown");
                countdownDisplay.innerText = "Time left: " + timeLeft + " seconds";
    
                countdownTimer = setInterval(() => {
                    timeLeft--;
                    countdownDisplay.innerText = "Time left: " + timeLeft + " seconds";
    
                    if (timeLeft <= 0) {
                        clearInterval(countdownTimer);
                        countdownDisplay.innerText = "Stress test complete!";
                    }
                }, 1000);
    
                fetch("/start-stress")
                    .then(response => response.text())
                    .then(data => {
                        console.log(data);
                    })
                    .catch(error => {
                        console.error("Error starting stress test:", error);
                    });
            }
    
            window.onload = fetchServerIP;
        </script>
    </head>
    <body>
        <h1>Welcome to NHN Cloud Web Server</h1>
        <p id="server-ip">Fetching server IP address...</p>
        <button id="start-btn" onclick="startStressTest()">Start Stress Test</button>
        <p id="countdown">Time left: 120 seconds</p>
    </body>
    </html>' > /web-sample/index.html
```

3. In the console pane, click **Storage - Object Storage**in the left menu.
4. On the Object Storage screen, click `myobs`to go to the container details page screen.
5. On the `myobs` screen, click **Upload object**.
6. In the **Upload Object** window, click **Choose File**, select `the index.html` file located ** in /web-sample**, and then click **OK**.
7. In the **Upload Status Information** window, verify that the `index.html` file upload status **is Success**, and then click **OK**.
8. To the right of the uploaded `index.html` object, in the **Public URL** entry, click Copy **URL**to copy **the public URL address of the index.html file**.
9. Connect remotely `to linux-server-basic`.
> [Note] How to remotely connect `to linux-server-basic`
>
> * For how to connect remotely, see [4. Set up your network and create an instance](https://docs.alpha-nhncloud.com/ko/quickstarts/ko/network-setup/) - **Step 1. SSH remote access**.
10. After connecting to `the linux-server-basic` remote, run the command below to download and save index.html.
```bash
sudo curl -o /var/www/html/index.html (Public URL of the index.html file you uploaded to myobs)
```

!!! TIP "To Know"
\* Default web document path for Nginx
\* The default web documentation path for Nginx installed on Ubuntu ` is /var/www/html`. The web page documentation output when connecting to `the http://복사한 linux-server-basic floating IP address`is `/var/www/html/index.html`.

* Run the commands below to set up additional service configurations.

```bash
curl -s https://kr2-api-object-storage.nhncloudservice.com/v1/AUTH_cd41d4b57c1346b99641cc4092f2842d/onboarding/service-setting.sh | sed 's/\r$//' > /home/ubuntu/service-setting.sh
chmod +x /home/ubuntu/service-setting.sh
/home/ubuntu/service-setting.sh
```

* Open a new window in your web browser and enter the `http://복사한 linux-server-basic floating IP address`to see the changed web page.
![pic3](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/content_image/%EC%8A%A4%ED%86%A0%EB%A6%AC%EC%A7%80%20%EC%83%9D%EC%84%B1_%EB%8B%A8%EA%B3%843.png)

!!! TIP "To Know"
\* If you don't see a changed web page
\* If you see the old page even though the web page has changed, the problem might be caused by your web browser's cache. In this case, you can use the refresh (F5) or strong refresh provided by your browser to see the changed page. You can perform a strong refresh in the following ways.
* **Windows**: `Ctrl + F5` or `Shift + F5`
* **Mac**: `Cmd + Shift + R`


## References

* [Storage](https://en.wikipedia.org/wiki/Cloud_storage)
* [Block Storage](https://docs.nhncloud.com/ko/Storage/Block%20Storage/ko/overview/)
* [Object Storage](https://docs.nhncloud.com/ko/Storage/Object%20Storage/ko/Overview/)
* [HDD](https://en.wikipedia.org/wiki/Hard_disk_drive)
* [SSD](https://en.wikipedia.org/wiki/Solid-state_drive)
* [Disk encryption](https://en.wikipedia.org/wiki/Disk_encryption)
* [fdisk](https://en.wikipedia.org/wiki/Fdisk)
* [DF](https://en.wikipedia.org/wiki/Df_(Unix))
* [Mount](https://en.wikipedia.org/wiki/Mount_(computing))
* [chmod](https://en.wikipedia.org/wiki/Chmod)

## Previous step

* [06 - Create and Connect Databases](https://docs.alpha-nhncloud.com/ko/quickstarts/ko/create-database/)

## Next steps

* [08 - Set up monitoring](https://docs.alpha-nhncloud.com/ko/quickstarts/ko/create-database/)
