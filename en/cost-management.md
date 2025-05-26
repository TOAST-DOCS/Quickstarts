# Cost management
**Quickstarts > 11. Cost management**

In this learning module, you will learn how to view your organization's usage, set budgets, and create and apply resource tags in the NHN Cloud console. This will enable you to effectively manage your organization's cloud resources and operate cost-effectively.

![mod_info](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/module_info/%EB%B9%84%EC%9A%A9%20%EA%B4%80%EB%A6%AC.png)
## Learning objectives

In this learning module, you'll learn

* **Check your organization's usage**
    * Check your organization's usage in the NHN Cloud Console
    * View usage amounts for the entire organization and specific projects
    * View usage details for individual resources
* **Manage your organization's budget**
    * How to set budgets for your organization and apply budgets to specific projects and services
    * Set up over-budget alerts
* **Utilizing resource tags**
    * Creating resource tags to manage specific resource groups in the NHN Cloud console
    * Apply resource tags and view usage

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

This guide starts after [10. Optimize scalability and performance](https://docs.nhncloud.com/en/quickstarts/en/optimze-performance/).

## Manage your organization's usage and budget

### Step 1. Check your organization's usage

> You can view a detailed breakdown of the services used by your `MyORG` organization.

1. From the top menu of the NHN Cloud console, select the organization`(MyORG`), project`(MyPRJ`), and `Korea (Pyeongchon) region`that you want to use for your lab.
1. Click `MyORG`in the top menu of the NHN Cloud console.
2. Click the **Usage** tab at the top of the `MyORG` organization screen.
3. On the **Services** tab, view the organization-wide final amount for your organization, and click `MyPRJ`under Organization to view the detailed project usage amounts.
4. Click each resource item to see a detailed breakdown of the amount spent.

### Step 2. Manage your organization's budget

> You can add budget information for `your MyORG` organization and set it up to receive notifications when you go over budget.

1. Click `MyORG` on the **Organizations** tab located at the top of the NHN Cloud console.
2. Click the **Manage budget** tab at the top of the `MyORG` organization screen.
3. **+ Add budget**.
4. In the **Add budget** window, enter the information below, and then click **Done adding budget**.
    * Budget information
        * Name: `MyBudget`
        * Budget: `Amount`, `10000`
        * Projects: `All projects`
        * Services: `Compute, Instance, and 7 others (default)`
    * Notification thresholds
        * Threshold: `1`
        * Threshold criteria: `Notify when exceeded`
        * Threshold threshold day: `15` days, **uncheck** Receive usage notifications on threshold day
    * Who receives notifications
        * **Check the**email of the user registered as a `member`, NHN Cloud member user information, **and**select the **Email checkbox**.
5. View your budgets **and usage** `in MyBudget`in the list of added budgets.

!!! TIP "Tips"
    * **Budget alerts are sent based on.**
        * Budget alerts can be **notified at 10:00 am the next day**if you exceed the set budget or meet a specified threshold (baseline, threshold day).  
    * **What to check after setting a budget**
        * After setting a budget, the service is not automatically disabled or blocked when the budget is exceeded. This is to avoid interruptions while using the service, which may result in costs exceeding the set budget. We recommend proper budget setting and resource management to utilize the budget correctly.

## Utilizing resource tags

### Step 1. Create a resource tag

1. In the top menu of the NHN Cloud console, click the organization you want to use for the lab`(MyORG`).
2. Within the `MyORG` organization dashboard **, under Organization Service Usage,**click **Resource Watcher**.
3. On the Resource Watcher screen, click the **Resource Tags** tab in the tab menu.
4. **+**Click ** Create Resource Tag**.
5. On the **Create Resource Tag** screen, set the information below and click **Add**.
    * Tag key: `MyInstanceTag`
    * Tag value: `empty`
6. **+**Click ** Create Resource Tag**.
7. On the **Create Resource Tag** screen, set the information below and click **Add**.
    * Tag key: `MyStorageTag`
    * Tag value: `empty`
8. Select the `MyInstanceTag` resource tag, and then click **Edit Tag**on the right.
9. In the Edit Resource Tag window, click **+ Add Tag Value**, type `webserver`in the Tag Value field, and then click **Save**.
10. **+**Click ** Add Tag Value**again, type `dbserver`in the Tag Value field that appears, and then click **Save**.
11. Click **OK**to close the Edit Resource Tag window.
12. Select the `MyStorageTag` resource tag, and then click **Edit Tag**on the right.
13. In the Edit Resource Tag window, click **+ Add Tag Value**, type `blockstorage`in the Tag Value field, and then click **Save**.
14. Click **OK**to close the Edit Resource Tag window.


### Step 2. Apply the resource tags you created

> Set resource tags for each of the instance, database, and blockstore resources that `your MyORG` organization is using.

1. Within the `MyORG` organization dashboard **, under Organization Service Usage,**click **Resource Watcher**.
2. On the Resource Watcher screen, click the **Resources** tab from the tab menu.
3. **In Resource name,**type `linux-server-basic`, and then click **Search**.
4. On the search results screen, click the resource with the **resource name** `linux-server-basic`.
5. In the Split **Detail** pane at the bottom, click **Settings**for the **Resource Tag** entry.
6. In the **'linux-server-basic' resource tag settings** window, make the settings as shown below and click **OK**.
    * Tag key: `MyInstanceTag`
    * Tag Key: Tag Value: `MyInstanceTag: webserver`
7. Click **Initialize**.
8. **In Resource name,**type `mysql-db-basic`, and then click **Search**.
9. On the search results screen, click the **resource named** `mysql-db-basic`.
10. In the Split **Detail** pane at the bottom, click **Settings**for the **Resource Tag** entry.
11. In the `my-sql-basic` resource tag settings window, make the settings as shown below and click **OK**.
    * Tag key: `MyInstanceTag`
    * Tag Key: Tag Value: `MyInstanceTag: dbserver`
12. Click **Reset**.
13. **For Resource Type,**click the drop-down menu labeled **All**, and then type Block Storage.
14. Under Discovered resource types, click **Block Storage**to select the checkbox.
15. Click **Search**.
16. On the search results screen, click the **resource with the resource name** `MyBS`.
10. In the Split **Detail** pane at the bottom, click **Settings**for the **Resource Tag** entry.
18. In the `MyBS` Resource Tag Settings window, make the settings as shown below and click **OK**.
    * Tag key: `MyStorageTag`
    * Tag key: Tag value: `MyStorageTag: blockstorage`

### Step 3. Check usage with resource tags

> Use resource tags to view detailed usage of the instance, database, and block storage resources your `MyORG` organization is using.

1. In the top menu of the NHN Cloud console, click the organization you want to use for the lab`(MyORG`).
2. On the `MyORG` organization screen, click the **Usage** tab.
3. On the **Utilization** screen, click the **Resources** tab from the tab menu.
4. Within the screen, click Setup and then **Search**, as shown below.
    * View by
        * Select the tag key: `MyInstanceTag`
        * Select tag value: `webserver`
5. At the bottom, you'll see the amount spent per resource tag.
    > [Note]
    >
    > * By resource tag usage
    >     * When you apply a resource tag to a resource, you can see usage and charges separately from the time you apply it. There may be a difference between the amount of usage you see by resource and the amount you are actually billed.
    >     * Usage amounts based on resource tags are available from the time of aggregation the day after the resource tag is applied.
6. Within the screen, click Setup and then **Search**, as shown below.
    * View by
        * Select the tag key: `MyInstanceTag`
        * Select tag value: `dbserver`
7. At the bottom, you'll see the amount spent per resource tag.
8. Within the screen, click Setup and then **Search**, as shown below.
    * View by
        * Select Tag Key: `MyStorageTag`
        * Select tag value: `blockstorage`
9. At the bottom, you'll see the amount spent per resource tag.

## References

* [Manage notifications](https://docs.nhncloud.com/en/nhncloud/en/console-guide/#_33)
* [NHN Cloud Pricing](https://www.nhncloud.com/kr/pricing)
* [Resource tags](https://docs.nhncloud.com/en/Governance%20&%20Audit/Resource%20Watcher/en/console-guide/#_2)

## Previous step

* [10. Optimize scalability and performance](https://docs.nhncloud.com/en/quickstarts/en/optimze-performance/)

## Next steps

* [12. Organize and delete resources](https://docs.nhncloud.com/en/quickstarts/en/cleanup-resources/)
