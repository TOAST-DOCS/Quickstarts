# Organizing and deleting resources
**Quickstarts > 12. Organize and delete resources**

In this learning module, you'll learn how to delete unused resources, projects, and organizations from NHN Cloud. This will help you avoid incurring unnecessary costs and optimize your cloud environment.
![mod_info](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/module_info/%EB%A6%AC%EC%86%8C%EC%8A%A4%20%EC%A0%95%EB%A6%AC%20%EB%B0%8F%20%EC%82%AD%EC%A0%9C.png)
## Learning objectives

In this learning module, you'll learn

* **Check before deleting a resource**
    * How to check dependencies before deleting a resource
* **Deleting unused resources**
    * Delete unused servers (Compute), storage (Volume, Object Storage), databases (DB), etc.
* **Organizing and deleting projects**
    * Procedure for deleting a project
* **Delete an organization**
    * Procedure for deleting an organization

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

**This guide starts with steps after [11.Managing expenses](https://docs.alpha-nhncloud.com/ko/quickstarts/ko/cost-management/).**

## Delete all resources in use 

### Step 1. Check the resources your organization is using

> Resource Watcher lets you see all the resources your `MyORG` organization is using at a glance.

1. In the top menu of the NHN Cloud console, click the organization you want to use for the lab`(MyORG`).
2. Within the `MyORG` organization dashboard **, under Organization Service Usage,**click **Resource Watcher**.
3. On the Resource Watcher screen, click the **Resources** tab from the tab menu.
4. In the **Resource Status** field, click the drop-down menu labeled **Full** and select the `In Use` checkbox.
5. Click **Search**.
6. On the search results screen, see which resources are currently in use.

### Step 2. Delete the default instance resource

> Steps 2 through 8 guide you through deleting the resources you've used so far.

1. Click `MyPRJ`in the top menu of the NHN Cloud console and select the `Korea (Pyeongchon`) region.
2. Click **Compute - Instance**in the left menu of the console window.
3. In the list of instances, click to select the `linux-server-basic` instance.
4. Click the **---** button above the list, and then click **Delete Instance**.
5. In the **Delete Instance** window, make the settings as shown below and click **OK**.
    * Resources to delete
        * Additional block storage: ✔ `(checked)`
        * Floating IP: ✔ `(checked)`
    > [Note]
    >
    > * When checking resources to delete
    >     * If you check and delete that resource, it will delete the target resource that was created with it when the instance was created.
6. In the Success window, click **OK**.
7. Select the `mysql-db-basic` instance from the list of instances.
8. Click **---** above the list, and then click **Delete instance**.
9. In the **Delete Instance** window, make the settings as shown below and click **OK**.
    * Resources to delete
        * Additional block storage: ✔ `(checked)`
        * Floating IP: ✔ `(checked)`
10. In the Success window, click **OK**.

!!! TIP "To Know"
\* Delete additional block storage, floating IPs
\* When deleting material stored in object storage, **confirm the deletion**so that the user can see the contents.

### Step 3. Delete the Auto Scale resource

1. Click **Compute - Auto Scale**in the left menu of the console window.
2. Select the `MyASGroup` scaling group from the list of Auto Scale groups.
3. Click **Delete scaling group**above the list.
4. In the **Delete Scaling Group** window, click **OK**.
5. In the Success window, click **OK**.

### Step 4. Delete the Image resource

1. Click **Compute - Image**in the left menu of the console window.
2. Select the `linux-server-basic-image` image from the list of images.
3. Click **Delete image**.
4. In the **Delete images** window, click **OK**.
5. In the Success window, click **OK**.

### Step 5. Delete the Object Storage resource

1. In the console pane, click **Storage - Object Storage**in the left menu.
2. In the Object Storage list, select the checkbox `for myobs`on the left.
3. Click **Empty container**.
4. "To proceed with emptying the container, please enter the name of the selected container." Type `myobs`in the bottom field and click **OK**.
5. In the Success window, click **OK**.
6. Verify that the number of objects `in myobs`is `zeroed`out.
7. Select the `myobs` checkbox and click **Delete container**.
8. In the Delete Container window, click **OK**.
9. In the Success window, click **OK**.

!!! TIP "To Know"
\* Confirm deletion of object storage data
\* When deleting material stored in object storage, **confirm the deletion**so that the user can confirm the contents.

### Step 6. Delete a network resource

1. In the console pane, click **Network - Load Balancer**in the left menu.
2. Select `MyLB`from the list of load balancers.
3. Click **Delete load balancer**.
4. In the **Delete Load Balancer** window, click **OK**.
5. In the Success window, click **OK**.
6. Click **Network - Floating IP**in the left menu of the console window.
7. In the Floating IPs list, select the checkboxes for **all the floating IPs you have**.
8. Click **Delete floating IP**.
9. In the Delete Floating IP window, click **OK**.
10. In the console pane, click **Network - Security Groups**in the left menu.
11. In the Security groups list, select the `MySG-SSH, MySG-HTTP, and MySG-DB` check boxes.
12. Click **Delete security group**.
13. In the **Delete Security Group** window, click **OK**.
14. In the Success window, click **OK**.
    > [Note]
    >   * default security group
    >       * The default security group is the default security group used by NHN Cloud, and you cannot rename or delete the security group. However, you can create or delete security rules within the default security group.
15. Click **Network - VPC**in the left menu of the console window.
16. Click `MyVPC`in the list of VPCs to select it.
17. Click **Delete VPC**.
18. In the **Delete VPC** window, click **OK**.
19. In the Success window, click **OK**.

!!! tip "Get to know"
\* Services that depend on deleting a VPC
\* Deleting a VPC also deletes the resources that you created within the VPC, such as subnets`(MySubnet`), routing tables`(MyRT)`, and Internet gateways.


### Step 7. Delete the monitoring resource

1. Click **Monitoring - Cloud Monitoring**in the left menu of the console window.
2. Within the **Dashboard** tab, on the `MyDashboard` Details tab, click the toggle switch for Edit Mode on the right to enable it.
3. Select the **⋮**in the top-right corner of all widgets within `MyDashboard` (Instance-Basic, Instance-Disk-Basic, Instance-Network-Basic, etc.
4. In the notification window, click **OK**.
5. Click the **⋮**in `MyDashboard`, then click **Delete dashboard**.
6. In the Dashboard deletion progress confirmation notification window, click **OK**.
7. In the Dashboard deletion complete notification window, click **OK**.
8. Click the **Manage notifications** tab to the right of the **Dashboard** tab.
9. Within the **Notification Settings** details tab, select the checkbox `for MyAlarm`from the list of notifications.
10. Click **Delete notification**.
11. In the Confirmation of notification deletion progress notification window, click **OK**.

### Step 8. Delete the keyfair resource

1. Click **Compute - Key Pair**in the left menu of the console window.
2. Select the checkbox `for MyKey`in the list of key pairs.
3. Click **Delete keyfair**.
4. In the **Delete keyfair** window, click **OK**.
5. In the **Success** window, click **OK**.

!!! TIP "Get to know"
\* Keyfair coverage
\* Keyfairs are managed **per user's account**, not per project. If you want to continue using a key pair in another project, you don't need to delete it.

### Step 9. Deactivate the service and delete the project

1. Click MyPRJ **on the Projects**`tab` **at**the top of the NHN Cloud console.
2. In a project, click the **Project management** tab.
3. Within the **Manage Project** screen, under the **Services in use** section, click **Disable**to the right of the services that are grouped into the **Default Infrastructure** group (Instance, Key Pair, etc.).
4. In the Disable Default Infrastructure Services window, verify the following two points, select the checkboxes, and then click **OK**.
    * I have read all of the above and agree to delete my information when the service is deactivated.
    * All underlying infrastructure services (Instance, Key Pair, etc.)
5. In the **Service Disabled notification** window, click **OK**.
6. In the **Services in use** section, to the right of the **Object Storage** service, click **Disable**.
7. In the Disable Default Infrastructure Services window, verify the following, select the checkboxes, and then click **OK**.
    * I have read all of the above and agree to delete my information when the service is deactivated.
8. In the **Service Disabled notification** window, click **OK**.
9. In the **Delete project** topic, click **Delete project**.
10. In the **Delete project confirmation** window, click **OK**.
11. In the **project deletion success** notification, click **OK**.

### Step 10. Delete an organization

1. Click `MyORG`on the **Organizations** tab located at the top of the NHN Cloud console.
2. On the Organization dashboard screen, click the **Manage organization** tab.
3. In the **Delete organization** topic, click **Delete organization**.
4. In the **Confirm organization deletion** window, click **OK**.
5. In the **Organization deletion successful** window, click **OK**.
<br></br>
<p style="text-align: center; color: green;">Congratulations. You've completed all the quickstarts.</p>

<br></br>

## References

* [Change the state of an instance (delete)](https://docs.nhncloud.com/ko/Compute/Instance/ko/console-guide/#_11)
* [Delete a subnet](https://docs.nhncloud.com/ko/Network/VPC/ko/console-guide/#_10)
* [VPC](https://docs.nhncloud.com/ko/Network/VPC/ko/console-guide/#vpc)
* [Deleting block storage](https://docs.nhncloud.com/ko/Storage/Block%20Storage/ko/console-guide/#_3)
* [Delete an object storage container](https://docs.nhncloud.com/ko/Storage/Object%20Storage/ko/console-guide/#_7)

## Previous step

* [11. Manage expenses](https://docs.alpha-nhncloud.com/ko/quickstarts/ko/cost-management/)