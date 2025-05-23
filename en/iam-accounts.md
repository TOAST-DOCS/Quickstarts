# Set up IAM accounts and governance
**Quickstarts > 3. Set up IAM accounts and governance**

In this learning module, you'll learn how to set up governance and manage Identity and Access Management (IAM) accounts. This enables you to efficiently manage resources within your organization and set up permission-based access control to maximize security and productivity.

![mod_info](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/module_info/IAM%20%EA%B3%84%EC%A0%95%EA%B3%BC%20%EA%B1%B0%EB%B2%84%EB%84%8C%EC%8A%A4%20%EC%84%A4%EC%A0%95.png)
## Learning objectives

In this learning module, you'll learn to 

* **Setting up the IAM console domain**
    * Setting up a domain address to use the NHN Cloud Console with an IAM account
* **Create an IAM account and grant roles**
    * Create an IAM account
    * Granting roles and setting permissions per account
* **Set up IAM governance**
    * Set up login security for better security
* **Access your IAM account**
    * Sign in to an IAM account with an IAM console domain
<br></br>

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

    **This guide starts with the steps after [you create your organization and project](https://docs.alpha-nhncloud.com/ko/quickstarts/ko/create-organization/).**

## Prepare for IAM account access

### Step 1. Set the IAM console domain address

1. Access the NHN Cloud console and click `MyORG` Organization in the top menu.
2. On the Organization dashboard screen, click the **Manage organization** tab.
3. **In the Organization Preferences, Organization****Management** tab submenu, check the **Domain Settings** item.
4. Enter the **domain settings** as shown below, and then click **Set domain**.
    * Domain name: `The domain name of your choice`
    > [Caution]
    >
    > * Domain names can only contain letters, numbers, and hyphens, and can be between 3 and 40 characters long. The first character can only be letters and numbers.
    > * You can't use a domain name that someone else is using. If you try to enter a duplicate domain name, \*\*Duplicate domain exists\** is printed and won't let you enter it.    
5. In the **notification** window, click **OK**.
6. Under the **Domain settings** item, in the **IAM Console** item, view the URL for your domain name and click **Copy**right to copy the domain address.

!!! TIP "Get to know"
\* IAM console domain name
\* The domain name is the exclusive access address for users with an IAM account. You can find out how to access it by doing the following tasks
\* Domain name guide
\* When setting up a domain name, we recommend that you use a clear and intuitive name, with the following prerequisites
* **Include your organization's identifying information**
\* Include information that clearly identifies your organization, such as company name, department name, group name, etc.
\* Examples: `company-name`, `team-alpha`
* **Reflect your management structure**
\* For large organizations, add suffixes such as region, type of work, department, etc.
\* Examples: `company-kr`, `org-dev`, `team-global`
* **Consider alignment with governance**
\* Consider IAM policies or rights management to ensure naming scheme consistency
\* Examples: `enterprise-core`, `partner-project`
* **Consider future scalability**
\* Create a structure that is scalable over the long term, so that additional projects are clearly distinguishable
\* Example: `parent-org`
\* Domain names can be changed even after setup.

### Step 2. Set up IAM two-factor authentication login security

1. On the **Manage Organization** tab, click the **Governance Settings** tab from the subtab menu.
2. In the **IAM Governance settings > Login security settings** item, click **Change settings**.
3. Make your settings as shown below and click **Save**.
    * Login security settings: `Two-factor authentication`
    * Services: `Common Settings`
    * Secondary verification: `email or mobile phone`
4. In the **notification** window, click **OK**.
5. In the **Save settings notification** window, click **OK**.

### Step 3. Create an IAM account

1. In the top menu of the console window, click `MyORG` Organization.
2. On the Organization dashboard screen, click the **Manage members** tab.
3. Click the **Member Management** tab submenu, **IAM Accounts**.
4. **+ Add IAM account**.
5. In the **Add IAM account** window, set the information below and click **Register**.
    * ID: `myproject-admin`
    * Name: `myproject admin`
    * Mail: `User email address`
    > [Caution]
    >
    > * User email address
    >     * To use your IAM account, you'll need to set up an IAM password, which will be sent to the email address you provide, so be sure to enter an email address where you can receive it.
    > * Set the ID value
    >     * The ID must be unique. You can't use a domain name that someone else is using. If you enter a duplicate domain name, **a member with the same ID already exists**is printed and won't be entered.
    >     * The ID can be changed even after it's set up.
6. **+ Add IAM account**.
7. In the **Add IAM account** window, set the information below and click **Register**.
    * ID: `myproject-budget`
    * Name: `myproject Budgeter`
    * Mail: `User email address`
8. In the list of IAM accounts, verify the two IDs you created.

### Step 4. Grant IAM account roles

1. In the list of IAM members, click the `myproject-admin` ID whose name `is` `myproject-admin` to select it.
2. In the bottom split screen, click **Edit roles**.
3. On the **Edit role** screen, set the information below and click **Edit**.
    * Select item: Click `OrgRole`, then under Detail Roles, select the `MEMBER` checkbox. (Role `NONE`leaves the checkbox selected.)
4. In the **notification** window, click **OK**.
5. In the list of IAM accounts, click and select the `myproject-admin` ID whose name `is` `myproject-admin` to select it, and then confirm the modified role in the bottom split screen.
6. In the list of IAM accounts, click and select the `myproject-budget` ID whose name `is` `myproject-budget`.
7. In the bottom split screen, click **Edit roles**.
8. On the **Edit role** screen, set the information below and click **Edit**.
    * Select: Click `OrgRole`, and under Detailed Roles, select the `BILLING_VIEWER and BUDGET_ADMIN` checkboxes, and clear the `NONE` checkbox.
9. In the **notification** window, click **OK**.

!!! tip "Get to know"
\* IAM default role
\* When you create IAM, the default role is set to NONE (Default Role). This role can be Read Organization Dashboard, Read Organization Preferences.

### Step 5. Set your IAM account password

1. Access the NHN Cloud console and click `MyORG` Organization in the top menu.
2. In the top menu of the console window, click `MyORG` Organization.
3. On the Organization dashboard screen, click the **Manage members** tab.
4. Click the **Member Management** tab submenu, **IAM Accounts**.
5. In the list of IAM members, click **Set Password Mailing**in the `admin` area to the right of `myproject-admin`, whose name is `myproject-admin`.
6. In the notification window, click **OK**.
7. In the Password setup email address sent notification window, click **OK**.
8. Check the email you registered with `myproject-admin` using a web browser in a new window or an email verification tool, for example. The subject line of the email should be <span style="color:rgb(34, 34, 34);"><strong>[NHN Cloud IAM]</strong></span><span style="color:rgb(34, 34, 34);"><strong>Please set a password to use the</strong></span> MyORG <span style="color:rgb(34, 34, 34);"><strong>service</strong>.</span>
    > [Caution] If you don't receive a password setup email
    > * If you don't receive the email after **the password reset mailing**, check your spam folder, blocking settings, email capacity, etc. with your email service.If the problem persists, click Edit **information**on the right side of the IAM member list and change to a different email address.
9. Click **Make changes**in the body of the mail.
10. On the Change password page, enter the same password you want to change in the "Enter new password" and "Re-enter password" fields, then click **Save password**.
    > [Caution] Password generation rules
    > * You must enter 8 to 15 characters, using a combination of alphanumeric and special characters.
11. **Your new password has been saved.** Click **OK**in the notification window that appears.
12. Follow the same action steps as above to ensure that in the IAM members list, the name <span style="color:rgb(34, 34, 34);">`myproject-budget` </span>in the list of IAM members.

### Step 6. Access the IAM account of myproject-admin

1. Open a new window in your web browser and type `https://MyORG IAM console domain address`to access the IAM console domain.
    * IAM console domain address
        * [3.](https://docs.alpha-nhncloud.com/ko/quickstarts/ko/iam-accounts/) Share **the URL for the IAM console domain name**that you copied in Task 1, [Set up IAM accounts and governance](https://docs.alpha-nhncloud.com/ko/quickstarts/ko/iam-accounts/), with your IAM account users so that they can use it.
2. In the `MyORG` login window, enter the information below and click **Sign in**.
    * Username: `myproject-admin`
    * Password: `The password for myproject-admin that you set up in Task 4.`
3. On **the secondary email verification page**, open a new web browser window or use an email verification tool to check the email associated with `your myproject-admin` IAM account. The subject line of the email will be \*\*[NHN Cloud IAM] Secondary email verification requested\*\*.
4. Click **Authenticate**in the body of the mail.
5. **Return**to **the second email verification page**and click Confirm.
6. View the console page for your IAM account.
7. On the Organization dashboard screen, click the **Manage organization** tab.
8. **In the****Manage organization** submenu, **Organization preferences**, view the default settings for your organization.
9. In the upper-right corner, hover over `myproject admin`and click **Sign out**in the menu that appears.

!!! tip "Get to know"
* `myproject-admin` Role
\* This account can only Create projects, Read organization dashboards, Read about projects, Read organization dashboards, and Read organization preferences. If you click on tabs such as Manage members of an organization, Manage notification recipient groups, Manage notifications, and so on, you'll see the **No permission to use the service** screen.
\* Grant the appropriate permissions based on the user's role. For more information, please refer to our guide.


> You can access the `myproject-billing` IAM account the same as in the above action step.

## Reference sites

* [Console user guide](https://docs.nhncloud.com/ko/nhncloud/ko/console-user-guide/)
* [Security policies](https://docs.nhncloud.com/ko/nhncloud/ko/security-policy/)
* [IAM](https://en.wikipedia.org/wiki/Identity_and_access_management)

## Previous step

* [2. create organizations and projects](https://docs.alpha-nhncloud.com/ko/quickstarts/ko/create-organization/)
<br>

## Next steps

* [4. Set up your network and create an instance](https://docs.alpha-nhncloud.com/ko/quickstarts/ko/network-setup/)
