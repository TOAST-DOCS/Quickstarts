# Monitoring settings
**Quickstarts > 8. Monitoring settings**

In this learning module, you will learn more about the Monitoring service provided by the NHN Cloud console and get hands-on experience with it. NHN Cloud's Cloud Monitoring service helps you monitor the health of infrastructure and applications running in a cloud environment in real time and detect anomalies quickly.

![mod_info](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/module_info/%EB%AA%A8%EB%8B%88%ED%84%B0%EB%A7%81%20%EC%84%A4%EC%A0%95.png)
## Learning objectives

In this learning module, you'll learn to

* **NHN Cloud Monitoring Concepts**
    * Overview of monitoring services provided by NHN Cloud
    * The importance of monitoring system performance, network traffic, and application logs
    * The difference between real-time health tracking and long-term performance analysis
* **Introduction to NHN Cloud monitoring tools**
    * **Cloud Monitoring** overview and features
    * Build a monitoring dashboard
        * Utilizing the NHN Cloud main dashboard
        * Setting up dashboards with templates
    * Set up alerts and notifications
        * Detect abnormal conditions through the NHN Cloud notification system
        * Set up conditional notifications, such as email, SMS, and more

## Before you begin

Before working through this learning module, we recommend that you do the following

* **Standard Internet browser**
    * You must have the latest version of a browser installed, such as Google Chrome, Microsoft Edge, Firefox, or Safari.
    * JavaScript and cookies must be enabled in your browser settings.
* **Internet connection environment**
    * A stable internet connection is required, with a recommended bandwidth of at least 5 Mbps.
    * You must be able to communicate securely over HTTPS.
* **Member account**
    * You must have an NHN Cloud account with a registered payment method.
    * You need to log in to the NHN Cloud homepage.

    **This guide starts after the steps in [7. Create and set up storage](https://docs.nhncloud.com/en/quickstarts/en/create-storage/).**

## Monitoring cloud resources with the Cloud Monitoring service

### Step 1. Create an instance detailed metrics dashboard with Cloud Monitoring

1. From the top menu of the NHN Cloud console, select the organization`(MyORG`), project`(MyPRJ`), and `Korea (Pyeongchon) region`for the lab.
2. Click **Monitoring - Cloud Monitoring**in the left menu of the console window.
3. **+ Create dashboard**.
4. In the **Create dashboard** window, set the information below and click **OK**.
    * Dashboard name: `MyDashboard`
5. At the right end of the `MyDashboard` tab, click **+ Add widget**.
6. On the **Add widget** screen, set the information below and click **Add**.
    * Default settings
        * Widget name: `Instance-CPU-Basic`
        * Graph type: `Line`
        * Service: `Instance`       
    > [Note]
    >
    > * Service change alarms
    >    * If the widget's service changes, your settings might be deleted. You'll be notified when changes are made, so make sure to add your existing work before making any changes so you can work with it.
    * Setting up metrics
        * Resource type: `CPU`
        * Metric topics: `CPU utilization, CPU average load (5 m)`
7. **+ Add Widget**to create additional widgets.
8. On the **Add widget** screen, set the information below and click **Add**.
    * Default settings
        * Widget name: `Instance-Disk-Basic`
        * Graph Type: `Stacked Area`
        * Service: `Instance`
    * Setting up metrics
        * Resource type: `Disk`
        * Metrics topics: `Disk utilization, Disk utilization per mount, Disk reads per device, Disk writes per device`
9. **+ Add Widget**to create additional widgets.
10. On the **Add widget** screen, set the information below and click **Add**.
    * Default settings
        * Widget name: `Instance-Network-Basic`
        * Graph type: `Column`
        * Service: `Instance`
    * Setting up metrics
        * Resource type: `Network`
        * Metric topics: `Network data sent per device, network data received per device`
11. Verify that the widget added `to MyDashboard`looks normal.

### Step 2. Check out your project's custom dashboard

1. Click the Project tab with the name `MyProject` at the top of the console window.
2. From the `MyProject` main screen, click the `Custom dashboard` tab.
3. Verify that the widget `in MyDashboard`that you added in step 1 looks normal.

### Step 3. Set up email notifications when an instance experiences a CPU overload

1. On the **Cloud Monitoring** service screen, click the **Manage alerts** tab.
2. Cick **Notification settings**.
3. On the **Create notification** screen, set the information below and click **Save**.
    * Basic information
        * Name: `MyAlarm`
        * Service: `Instance`
    * Notification settings
        * Resource type: `CPU`
        * Metric: `CPU Detail (user)`
        * Filter
            * **+ Add**, and enter the settings below.
            * label `: instance`, operator `:`=, conditions `: linux-server-basic`
            * Edit: Click **Save** 
        * Condition
            * Comparison method: `>,` Threshold: `50`, Duration: `1` minute
                * If the threshold exceeds 50, the notification lasts for 1 minute.
            * Edit: Click **Save** 
    * Who receives notifications
        * **Select the** **Add** destination to the right with the name of `the default notification`recipient group checkbox

    > [Note]
    >
    > * Select a group to receive notifications
    >    * Check the Add checkbox to the right of the name of that notification recipient group and you'll see the selected notification recipient group added directly above it.

4. Confirm that `MyAlarm`has been created and that the **Enable notifications** toggle button is enabled.

!!! TIP "Tips"
    * Toggle button status
        * The toggle button active state is indicated by a white circle within an ellipse that has moved to the right. The color is blue when the toggle is active.
        * The toggle button disabled state is a white circle within an ellipse moved to the left. When the toggle is disabled, the color is gray.


### Step 4. Check the instance's history of CPU overload events     

1. Click **Network - Floating IP** in the left menu of the console window.
2. **Copy** and **record**the IP address in the floating IP resource list where the connected device is `linux-server-basic`.
3. Open a new window in your web browser and type `http://the copied linux-server-basic floating IP address` to access the web page.
4. Click **Start Stress Test**in the body of the web page and wait 2 minutes. This action randomly overloads the `linux-server-basic` instance CPU usage (CPU details (user)).
5. After a while, check to see if you receive the results of the alarm by text and email.
6. On the **Cloud Monitoring** service screen, click the **Manage alerts** tab.
7. **On the Manage alerts screen,**click the **Alert occurrence history** tab.
8. Click **Search** within the body to see the history of the alert.

## Other considerations

* [Metric](https://en.wikipedia.org/wiki/Metric_system)
* [Monitoring](https://en.wikipedia.org/wiki/System_monitor)
* [Metric Dictionary](https://docs.nhncloud.com/en/Monitoring/Cloud%20Monitoring/en/metric-dictionary/)
* [Cloud Monitoring](https://docs.nhncloud.com/en/Monitoring/Cloud%20Monitoring/en/overview/)
* [CloudTrail](https://docs.nhncloud.com/en/Governance%20&%20Audit/CloudTrail/en/overview/)
* [Stress testing](https://en.wikipedia.org/wiki/Stress_testing_(computing))

## Previous step

* [7. Create and connect storage](https://docs.nhncloud.com/en/quickstarts/en/create-storage/)

## Next steps

* [9. Backup and restore](https://docs.nhncloud.com/en/quickstarts/en/backup-restore/)
