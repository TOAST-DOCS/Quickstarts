# Optimize scalability and performance
**Quickstarts > 10. Optimize scalability and performance**

In this learning module, you will learn how to architect for scalability and performance optimization of applications in the NHN Cloud environment. Leverage NHN Cloud RDS to design efficient, flexible, and scalable systems for auto-scaling, load balancing, and reliable data management.

![mod_info](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/module_info/%ED%99%95%EC%9E%A5%EC%84%B1%EA%B3%BC%20%EC%84%B1%EB%8A%A5%20%EC%B5%9C%EC%A0%81%ED%99%94.png)
## Learning objectives

In this learning module, you'll learn

* **Create a load balancer**
    * Create load balancers in L4 routing mode for traffic balancing and connect floating IPs to ensure accessibility of web services
* **Configure an autoscale group**
    * Set up autoscale groups to automatically add or delete instances based on traffic changes
    * Apply CPU usage-based scale-up and scale-down policies to maintain a minimum of one and maximum of three instances
* **Set up web server access**
    * Make the created web server accessible via the floating IP connected to the load balancer

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
    * You must be logged in to the NHN Cloud portal.

**This guide starts with the steps after [9. Backup and restore](https://docs.nhncloud.com/en/quickstarts/en/backup-restore/).**

## Traffic balancing with scaling groups and load balancers

### Step 1. Create a load balancer

> Use the Load Balancer service to create a `MyLB` load balancer, and then create a floating IP to connect to it.

1. From the top menu of the NHN Cloud console, select the organization`(MyORG`), project`(MyPRJ`), and `Korea (Pyeongchon) region`that you want to use for your lab.
1. In the left menu of the console window, click **Network - Load Balancer**.
2. **+ Create Load Balancer**.
3. On the **Select Load Balancer Creation Mode** screen, **select** `L4 Routing Mode` **, and then**click OK.
4. On the **Create Load Balancer** screen, set the information below and then click **Create Load Balancer**.
    * Settings
        * Name: `MyLB`
        * Type: `General`
        * VPC: `MyVPC(192.168.0.0/16)`
        * Subnet: `Auto-assign MySubnet (192.168.0.0/24)`
        * Static routes: `Not applicable`
    * Listeners
        * Name: `http-listener`
        * Connection limit: `60000`
        * Keep-Alive timeout: `300`
        * Protocol: `HTTP`
        * Block invalid requests: `Enabled`
        * Load balancer port: `80`
        * Member groups
            * Name: `linux-server-member`
            * Health check protocol: `HTTP`
            * Health check port: `80`
            * Protocol: `HTTP`
            * HTTP method: `GET`
            * Port: `80`
            * HTTP status code: `200`
            * Load balancing method: `ROUND_ROBIN`
            * URL: `/`
            * Session persistence: `No session persistence`
            * Health check interval: `30`
            * Maximum response latency: `5`
            * Maximum number of retries: `2`
            * Host Header: `Empty`
    * IP access control group: `None (default)`
    * Erasure protection: `Disabled`
5. In the Success window, click **OK**.
6. After a few moments, when the creation is complete, select `MyLB`and click **Manage Floating IPs**at the top.
7. In the **Manage Floating IPs** window, under Create Floating IP, click **+ Create**.
8. In the **Create Floating IP** window, click **Create**.
9. In the **Success** window, **copy** and **record the** **floating IP address**.
10. Click **OK**.
11. In the **Manage Floating IP** s pane, under the **Connect/Disconnect Floating IPs** item, click Set up and **connect**as shown below.
    * Select Floating IP: `The floating IP address you created and confirmed above.`
    * Select network interface: `MyLB`
12. In the **Success** window, click **OK**.
13. Click **Close**.

### Step 2. Create an auto-scaling group 

> Utilize the Auto Scale service to create a scaling group and then create two instances that can be made available.

1. Click **Compute - Auto Scale**in the left menu of the console window.
2. On the **Auto Scale** screen, click **+ Create scaling group**.
3. On the **Create Scaling Group** screen, set the information below and then click **Create Scaling Group**.
    * Instance Templates
        * Enabled: `Disabled`
    * Image
        * Click Personal Images > Image Name: `linux-server-basic-image` 
             > [Note]  
             >The `linux-server-basic-image` private image can be created through **step 1**of [9.Backup and Recovery](https://docs.nhncloud.com/en/quickstarts/en/backup-restore/).
    * Instance information
        * Availability zones: `Any availability zone.`
        * Instance name: `linux-server-autoscale`
        * Instance Types > click **Select Instance Type** > click Instance Type Name: `t2.c1m1` > click **Select** 
        * Keyfair > `MyKey`
            * For more information on creating a key pair, see the Key pair user guide.
    * Root block storage
        * Block storage type: `HDD`
        * Block storage size (GB): `20` GB
    * Network settings: Select `Create network interface` 
    * Network
        * Under Available subnets, click the `MySubnet (192.168.0.0/24` ) resource to use it as the selected subnet.
    * Floating IP: `Disabled`
    * Security groups
        * **Select a security group:** Select `MySG-HTTP`
    * Additional block storage: `Disabled (default)`
    * User scripts: `Blank (default)`
    * About scaling groups
        * Name: `MyASGroup`
        * Minimum Instances: `1`
        * Maximum instances: `3`
        * Running Instances: `2`
    * Scaling policies
        * Condition: `instance` `cpu` > `50%` condition persists for `1 minute` 
        * Adjust instances: `1` instance is created.
        * Reuse latency: No attempt to create an instance for `1 minute` after it is created.
    * Reduction policies
        * Condition: `instance` `cpu` < `10%` condition persists for `1 minute` 
        * Instance reconciliation: `1`instance is deleted.
        * Reuse latency: No attempt to create an instance for `1 minute` after it is created.
    * Automatic recovery policies
        * Automatic recovery: `Enabled`
    * Load balancers
        * Under Available load balancers, click `MyLB` resource to use as the selected load balancer
    * Deploy association: `Disabled (default)`

4. In the Success window, click **OK**.
5. In the list of images on the **Auto Scale** screen, you'll see `MyASGroup` being created. When creation is complete, the status light for that scaling group will be green.
6. In the left menu of the console window, click **Compute - Instance**.
7. On the Instance screen, verify that two `linux-server-autoscale`have been created among the list of instances.

### Step 3. Use a load balancer to distribute traffic

> Learn how to use `MyLB` load balancer to distribute traffic across multiple instances.

1. Click **Network - Floating IP**in the left menu of the console pane.
2. **Copy** and **record**the IP address from the floating list of IP resources to which the connected device `is MyLB`.
3. Open a new window in your web browser and enter `http://복사한 MyLB floating IP address`to access the changed web page.
4. Verify that the **Server IP Address value**in the body of the web page matches the virtual private IP address of `the linux-server-autoscale instance you`created.
5. Repeat the web browser refresh to verify that the **Server IP Address value**changes to the virtual private IP address of another `linux-server-autoscale instance`.
<br></br>
![pic1](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/content_image/%EC%84%B1%EB%8A%A5%EC%B5%9C%EC%A0%81%ED%99%94_%EB%8B%A8%EA%B3%843-1.png)

<p style="text-align: center; color: black;">Output screen 1</p>

![pic2](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/content_image/%EC%84%B1%EB%8A%A5%EC%B5%9C%EC%A0%81%ED%99%94_%EB%8B%A8%EA%B3%843-2.png)

<p style="text-align: center; color: black;">Output screen 2</p>
<br></br>

!!! tip "To Know"
* **Verify the Server IP Address value change value**
\* The `linux-server-autoscale` instance may change state from time to time depending on the AutoScale group's grow/shrink policy settings. This may result in only one Server IP Address due to the reduction from two instances to one instance.
\* The load balancer associated with an autoscale group periodically checks the status values of the instances in the group and connects to them. As a result, the health value information is updated, which can expose Server IP Address values that have changed after a period of time.
* **If a changed web page is not visible**
\* If you see the old page even though the web page has changed, the problem may be caused by your web browser's cache. In this case, you can use the refresh (F5) or strong refresh provided by your browser to see the changed page. You can perform a strong refresh in the following ways.
* **Windows**: `Ctrl + F5` or `Shift + F5`
* **Mac**: `Cmd + Shift + R`


### Step 4. Apply the autoscale group growth and reduction policy

> Learn how to auto-scale the number of instances based on application demand.

1. Open a new window in your web browser and connect to `http://복사한`by entering your `MyLB floating IP address`.
2. Click **Start Stress Test**in the body of the web page and wait 2 minutes.
    > [Note]
    >
    > * Stress Test
    >     * This feature places a random CPU load on the web server to ensure that the scaling policies you create in the autoscale group work.
3. Click **Compute - Instance**in the left menu of the console window.
4. On the Instance screen, verify that `linux-server-autoscale`is added to the list of instances.
5. Open a new window in your web browser and enter `http://복사한 MyLB floating IP address`to view the web page.
6. Verify that the **Server IP Address value**in the body of the web page is the virtual private IP address of `the linux-server-autoscale instance you`created.
7. Repeat the web browser refresh to verify that the **Server IP Address value**changes to the virtual private IP address of another `linux-server-autoscale instance`.
    * Click to select the `linux-server-autoscale` instance from the **Compute - Instance** menu and click the **Monitoring** tab in the split pane below to see the CPU utilization.
    * Click to select the `MyASGroup` scaling group from the **Compute - Auto Scale** menu and click the **Statistics** tab in the split pane below to see the CPU utilization.
8. If the `linux-server-autoscale` instance was automatically spawned, wait again for 2 minutes.
9. On the Instances screen, verify that `linux-server-autoscale`is removed from the list of instances.
10. Repeat the web browser refresh to verify that the **Server IP Address value**is output as the virtual private IP address of the `linux-server-autoscale` instance you are maintaining.

## References

* [Auto Scale](https://docs.nhncloud.com/en/Compute/Auto%20Scale/en/overview/)
* [Load Balancer](https://docs.nhncloud.com/en/Network/Load%20Balancer/en/overview/)
* [Load balancing methods](https://docs.nhncloud.com/en/Network/Load%20Balancer/en/overview/#_1)
* [Load balancing (computing)](https://en.wikipedia.org/wiki/Load_balancing_(computing))
* [Transport Layer(L4)](https://en.wikipedia.org/wiki/Transport_layer)
* [Application Layer(L7)](https://en.wikipedia.org/wiki/Application_layer)
* [Autoscaling](https://en.wikipedia.org/wiki/Autoscaling)

## Previous step

* [9. backup and recovery](https://docs.nhncloud.com/en/quickstarts/en/backup-restore/)

## Next steps

* [11. Manage expenses](https://docs.nhncloud.com/en/quickstarts/en/cost-management/)