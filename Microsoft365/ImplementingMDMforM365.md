Implementing MDM for Microsoft 365
=
Overview
-
Microsoft 365 includes Enterprise Mobility Plus Security (EMS), which is includes Mobile Device Management and Mobile Application Management that will be accessing your resources. Microsoft 365 EMS Includes Microsoft Intune to help manage devices and applications both corporate devices (COD) and personal devices (BYOD).  In this post  we will discuss and explored 
* How to plan for MDM and MAM
* MDM integration with Azure AD and configuration
* Setting up MDM authority 
* Configuring enrollment limits 

At the end of this post, you'll know how to plan, implement and configure MDM related serrvices in Microsoft 365. 

Introduction
-
Microsoft 365 is a SaaS bundle of Windows 10, Office 365 and Enterprise Mobility + Security that businesses can subscribe. There are three versions of Microsoft 365. 
*	Microsoft 365 Education, solution for educational providers. 
*	Microsoft 365 Business, solution for smaller business for up to 300 users. 
*	Microsoft 365 Enterprise, which is a complete solution for larger organizations.

Microsoft 365 also includes Microsoft Enterprise Mobility and security or EMS which is a suite of products including Azure Active Directory which offers cloud-based identity and access services, Azure Information Protection which offers document labeling, classification, protection and security, Microsoft Advanced Threat Analytics which provides real-time monitoring and protection against malware threats. Microsoft 365 Device Management which uses Intune to offer a mobile device and app management solution. Microsoft 365 Device Management uses Intune to manage devices and apps across multiple platforms including Windows 10, Android and iOS devices allowing devices to access company resources such as data and applications, ensuring that devices are compliant before they can access resources, protect corporate information by managing access restrictions, and controlling how users use and share data and manage applications on devices including app deployment, access and usage.
 
 Microsoft Intune provide several capabilities. It provides the ability to secure and monitor the data access, manage multiple devices per person (IOS, Android, macOS or Windows). Intune into integrates with Windows Defender. Intune also utilizes Azure AD conditional access policies, mobile device management (MDM) and mobile application called (MAM).  So let's look at differences between MDM and MAM. In MDM users enforced to "enroll" devices, use certificates to communicate with Intune. IT pushes apps to devices, restricts OS types and can remotely wipe device. MDM is used to enforce the enrollment of the user's devices that will allow them to obtain a certificate to communicate with Intune. And then IT can push the Apps to the devices. We can restrict the operating system types. We can even remotely wipe a device. So if a device gets stolen, we can choose to wipe that device and company information. In Mobile Application management (MAM), users ise personal devices to access company resources. When opening an app, additional authentication required. If device is lost, IT can remove company information. MAM is for users with personal devices who are accessing company resources. When they're opening an application from personal devices, they're gonna be required to provide additional authentication. IT can remotely remove just the company information from that personal device. So MDM is the primary tool use for managing corporate devices. MAM is the primary tool use for managing personal devices that contain corporate information on them. Now, when it comes to working with these devices and managing these devices through MDM, there are several ideas that you need to take into consideration. 
 * Device Ownership: Who owns device. MDM for managing corporate devices, MAM for managing personal devices. We need to understand the different devices and who owns those devices. 
 * The user type or the device role: So what are we gonna be allowing the user to perform within Microsoft 365. The device platforms, IOS, windows, android, macOS. 
 * Plans: Three types of plans are in the planning process. A rollout plan. A communication plan and a support plan. They will help to identify how we're gonna roll this out to our users and, more importantly, communicate and provide support for these devices after the rollout of this information.
 * Integrate with Azure AD. 
 * Specify the MGM authority, which allows you determine how you're going to manage your user devices, as well as how your users are going to enroll their devices so we can manage them remotely. 
 * Enrollment Restrictions, configure restrictions on enrollment and make sure not just anyone is enrolling their personal device into our Microsoft 365. 
 
Planning for Mobile Device Management
-
Microsoft Intune and MDM Authority Solutions
--
Lets have a deep dive, on the planning process for mobile device management, because this is a fairly complex topic, and there are a lots of different things that you need to take into consideration. So we will focus on four key areas, 
1. MDM Authority Solutions or solutions that you can implement and manage devices.
2. Microsoft 365 Intune. A key component for managing these devices. 
3. MDM deployment considerations
4. Device enrollment limits, where we can restrict what can users do with the devices, the types of devices, even down to the operating system, and the apps that they can access using these devices. 

MDM Authority Solutions
--

There are 4 possible MDM Authority solution that you can implement. 

1. **Intune Standalone** :  Cloud-only management, which you configure by using the Azure portal. Includes the full set of capabilities that Intune offers.
2. **Intune Co-Management** :  Integration of the Intune cloud solution with Configuration Manager for Windows 10 devices. You configure Intune by using the Configuration Manager console.
3. [**MDM for Office 365**](https://www.microsoft.com/en-us/microsoft-365/blog/2015/03/30/announcing-general-availability-of-built-in-mobile-device-management-for-office-365/) : Integration of Office 365 with the Intune cloud solution. You configure Intune from your Microsoft 365 admin center. Includes a subset of the capabilities that are available with Intune Standalone. Set the MDM authority in Microsoft 365 admin center.
4. **Office 365 MDM Co-existence** : You can activate and use both MDM for Office 365 and Intune concurrently on your tenant and set the management authority to either Intune or MDM for Office 365 for each user to dictate which service will be used to manage their mobile devices. User’s management authority is defined based on the license assigned to the user. 
 
So just make sure we fully understand, Let's have a look what *Microsoft Intune offers*. 
* We use Microsoft Intune to secure and monitor all of the data access from  mobile devices (Corparate devices (COD) or pesonal devices (BYOD)) and we can manage the different device platforms (Android, IOS, macOS and Windows (mobile, surface and desktop)). 
* Intune integrates with Windows Defenders to help secure the communications. 
* Utilizes Azure AD conditional access policies, Azure AD conditional access policies allows you to create a policy to control and manage the different devices.
* Mobile Device Management (MDM) primarily used for managing corporate devices.
* Mobile Application Management primarily used for managing bring your own devices (BYOD). 

So just make sure we fully understand, Let's have a look ***MDM vs MAM***

**MDM** is used primariliy for Corporate Devices 
- Integrate with Azure AD so we can Deploy Conditional Access Policy to enforce users to enroll devices. Use these conditional access policies like configuring WiFi or VPN, delivering applications to device, enforcing regulatory requirements
- Certificates required to communicate with Intune. MDM is assisting to make sure the users obtained certificate in periodically and renew certificate
- IT Admins remotely manage and wipe the device

**MAM** is used primarily for personal devices (BYOD)
- Control authentication when accessing corporate resources
- Remove corporate data from the device
- Use coditional access policies to require additional and stronger authentication like enforcing pin to access outlook mobile

MDM Deployment
--
Lets have a look ***MDM Deployment Considerations***

**Device Types**: Deployment is a type of device in the role or the user that the device is gonna have when accessing the corporate resources
- Corporate Devices (COD) or Personal Devices (BYOD)
- User Type (e.g executives, knowledge worker) or Device role (kiosk, mobile, laptop)

**Organizational Groups**
* Identify each use case and sub use case can go down even to platforms and apps.
    * Use Case, Sub use case, Organizational Unit (Group)
    * Corporate, Executive, HR, Finance

**Mobile Device Usecase with Platform & apps***
* Corporate --> Executive --> HR, Finance --> IOS
* Corporate --> Executive --> HR, Finance --> IOS --> Email, apps, profiles

 Let me give an example of Organizational groups, Use case is corporate. A sub use case is the executive within corporate, Organizanational group is executives in HR, finance. So we'll create policies based on the organizational group. 
 
 Lets break this down into further detail for mobile device use cases along with platforms and Apps. Corporate like above. Executives at the corporate level within departments or organizational group like HR and Finance, and now we're drilling it down to the executives within HR and Finance that they're using an IOS. . We can further refined this for the executives in HR and Finance that are using IOS and we want to control email, profiles and apps. We can get very granular with MDM. 
 
 MDM Deployment Planning
 --
 **Select MDM Solution**
 Choose the appropriate MDM authority solution based on the tools that you are going to use to manage the MDM deployment.
 
 **Plan for MDM Infrastructure**
There are a few considerations for Planning for the MGM infrastructures.
 * External Communications: How are we going to communicate our expectations from our external users?
 * Bandwidth Requirements: Users are Accesing, downloading and updating apps frequently.
    * Cache Proxy: To cash certain types of content of redundant downloads from multiple devices
    * Delivery Optimization for Windows 10: Leveraging P2P cashing technology, sharing package contents between devices per deployment.

**Plan Device Policies**
* Policy Conflict Resolution: Once the device is enrolled in MDM, we can create and assign, multiple policies to manage a device. Some of them may overlap, so we have to set up the priority of those policies to ensure we achieve our goals for managing that device.
* Unified Management: These policies are available in MDM for Office 365 and Microsoft Intune, which provide additional controls for managing your devices. They a evolving policies so they change quite frequently. So you want to be sure you're always familiar with the most recent policy that's available for you as you are deploying MDM. 
 
**MDM Deployment Plans**
There are three primary MDM deployment plans:
* Rollout Plan: Groups to target for rollout
    * Pilot Group: Test with super users to ensure the deployment process works as expected
    * Production Group: It is not necessarily a single production rollout group. Create production rollout groups by departments, by platform or by geography
* Communication Plan: Explain and Communicate details of Rollout Plan
    * What and How information is communicated
    * Who and When information is communicated
* Support Plan: Support of the new functionality and resolution of issiues, incidents and requests
    * Who is involved in support process
    * What is the support process
    * Who, When and Whom to provide training for users and support

 **Device Enrollment and Enrollment Limits**
 Device enrollment allows to manage devices, apps and how they access company data. In a BYOD scenario, users have their personal information on devices, and they may have also some company data. How are we going to manage that? 
 
 Devices must be enrolled in Intune and must have an MDM certificate for communications. In order to begin to use mobile device management (MDM), make sure devices are enrolled in Intune and they have an MDM certificate. 
 
 There are some dependencies for device enrollment: 
 * Device Ownership (Personal or Corporate)
 * Device Type (IOS, Android, macOS, Windows including Wondows for Mobile)
 * Management Requirements (Resets to wipe the device before enrolling, Locking prevent the user to unenroll and affinity to associate device to a specific user)
 
 All devices are allowed to enroll by default but we can restrict enrollment by device type. It's called enrollment limits, and managed through Endpoint Manager Admin Center. It allows you to specify restrictions for enrollment
* By Number and type of devices 
* By Operating Systems and versions (minimum and maximum versions of OS) 

We can create multiple restrictions and set priority order before applying these restrictions to user groups. There are some  default restrictions that are available and by default they apply to all users and userless enrollments like kiosk machines.
But that can be overridden using custom restrictions with higher priorities. Therefore we have to look at default restrictions. If we want to tweak those we need to create custom restrictions and make sure they are higher priority than the default restrictions. 

Managing Mobile Device Management in Microsoft 365
---
We are gonna focus on managing the mobile device management (MDM) in Microsoft 365.
There are 3 topics for managing MDM in Microsoft 365:
* MDM Integration with Azure AD
* Setting MDM Authority
* Configure Device Enrollment Limits

MDM Integration with Azure AD
----
So let's talk about MDM integration with Azure AD. We have company devices (COD) and personal devices (BYOD).

**Company Devices**
- Join Windows to Company Active Directory
- Join Windows to Azure AD

We are talking about integration with Azure AD, we can join windows to a traditional active directory environment. However to allow the enrollment process to take place, we have to configure the hybrid Azure AD domain join. Another option is to join Windows to Azure AD directly if we don't have the traditional onpremise AD environment. 

**Personal Devices (BYOD)**
- Add Microsoft Work Account to Windows
- Access Organization Apps and Resources
- Can enroll device in MDM

Our personal devices. We can add a Microsoft work account to Windows. By doing so, it allows us to access organization apps and resources, and provide an opportunity for the users to enroll their device in MDM. However, the keywords it provides them an opportunity they can decline to auto enroll the device in MDM. So this is where training in communications are necessary, so the user's know what is expected from them. If they are going to use personal device to access the company resources.

**Manage Devices**
- Group Policy
- System Center Configuration Manager
- Azure AD Join - Managed by MDM

We have a few different options to manage these devices. If we are using the traditional active directory domain, we are going to use group policies for managing devices. We can also use the system center configuration manager for our traditional active directory domain environment. However, we are going to use MDM to manage our Azure AD join devices because that provides all the functionality. Including auto enroll and to manage the limits or restrictions on those devices. 

**Azure AD Integration with MDM requirements**
There are some prerequisites for the MDM integration with Azure AD. 

* Active MDM Subscription
    * Intune by default
    * Third party solutions available in Azure AD App Gallery

We need an active MDM subscription. By default, we are using Intune, but we are not limited to using Intune. We can go to the Azure AD App Gallery to see if there is a third party tool that we can use instead of Intune. 

**Configure MDM Settings**
* MDM tems of use URLS
* MDM Discovery and Compliance
* Scope of Devices

Another requirement is to configure MDM settings. This includes URLs for the MDM terms of use, so the users have to view them and understand what is expected from users when using MDM. There are also some configuration settings for MDM Discovery and compliance. Lastly, while we are configuring MDM settings, we can configure the scope of the devices. If we want to use auto device enrollment, it's going to require Azure AD premium P1 or higher, and there is also Azure AD premium P2. If you have onpremise AD and you want to be able to use domain join, you can't just do it with onpremise active directory, you need to configure hybrid with Azure AD. After that, you can use group policies for configuring devices for Hybrid Azur AD Domain join. So it is possible to configure domain join for onpremise active directory by implementing hybride Azur AD. 

Lets take a look at the configuration options for an azure AD integration. In the portal.azure.com lets view manage Azure Active directory, make sure you are a global administrator, which is one of the requirements. Now, on the left hand side,  scroll down and you see, an option called Mobility (MDM and MAM). 

![Microsoft Intune](https://docs.microsoft.com/en-us/mem/intune/enrollment/media/windows-enroll/auto-enroll-intune.png)

If you want to bring in another application, you simply click on add application. It will bring up the available applications you have including On-premises MDM application. You can use Microsoft Azure AD to enable user access to on premises MDM application. It does require an existing on-premises MDM application subscription.
You can change the name, logo  etc. 

![Configure Scope](https://docs.microsoft.com/en-us/mem/intune/enrollment/media/windows-enroll/auto-enroll-scope.png)

In Microsoft Intune We have two categories on the left hand side. We have MDM user scope, terms of use, discovery and compliance URLs. Below that, we have MAM user scope, terms of use, discovery and compliance URLs. 

 The user scope in MDM can be set to None, Some for only specific groups and all for everyone to be able to auto enroll.  If you have several groups, you can search for them when you enable MDM for some. These URLs are the default settings, by the way. And you can restore default MDM and MAM URLs. If you want to have something specific to your organization, this is where you can customize them. 

So this is where and how we configure MDM to integrate with Azure AD.

Setting MDM Authority
---
The mobile device management (MDM) authority setting determines how you manage your devices. As an IT admin, you must set an MDM authority before users can enroll devices for management.

Possible configurations are:

**Intune Standalone** - cloud-only management, which you configure by using the Azure portal. Includes the full set of capabilities that Intune offers. [Set the MDM authority in the Intune console](https://docs.microsoft.com/en-us/mem/intune/fundamentals/mdm-authority-set#set-mdm-authority-to-intune)

**Intune co-management** - integration of the Intune cloud solution with Configuration Manager for Windows 10 devices. You configure Intune by using the Configuration Manager console. [Configure auto-enrollment of devices to Intune](https://docs.microsoft.com/configmgr/comanage/tutorial-co-manage-clients#configure-auto-enrollment-of-devices-to-intune)

**Mobile Device Management for Office 365** - integration of Office 365 with the Intune cloud solution. You configure Intune from your Microsoft 365 admin center. Includes a subset of the capabilities that are available with Intune Standalone. Set the MDM authority in Microsoft 365 admin center.

**Office 365 MDM Coexistence** You can activate and use both MDM for Office 365 and Intune concurrently on your tenant and set the management authority to either Intune or MDM for Office 365 for each user to dictate which service will be used to manage their mobile devices. User’s management authority is defined based on the license assigned to the user. [Microsoft Intune Co-existence with MDM for Office 365](https://blogs.technet.microsoft.com/configmgrdogs/2016/01/04/microsoft-intune-co-existence-with-mdm-for-office-365)

So now we look at some of the concepts associated with changing your MDM authority option. 

**Choose MDM Authority Option**

If you change to the MD authority, it can take up to 8 hours before people realize or see that that change has been made. The device actually has to check in with the authority to realize change and then has to synchronize with the new service and pick up information from new service. Now there are settings called basic settings that include things like the profile. Your profile includes configuration options like email, WiFi VPN, certificates, configuration profiles. These are immediately going go away. They could remain on the device for up to seven days. Or it could be longer if device has been offline and hasn't checked in and synchronized with the new service. So there's an MDM authority change and the device checks in and then their synchronization takes place. You'll have your settings configured based on the new MDM authority, but if you don't connect the authority for a period of time, it won't synchronize until you connect to that new service. If the devices is wiped or doesn't communicate, your MDM certificate doesn't get renewed. It wont be removed from Azure AD portal for 180 days after the certificate expires. There are some devices without associated users. These include anybody with the IOS device enrollment program or any type of bulk enrollment scenarios. What you need to do is call support for assistance to move them to the new MDM authority. MDM authority can never be changed to unknown. It's not possible for you to change to unknown. It has to be associated with one of the four authorities. 

**Set MDM authority to Intune**
If you haven't yet set the MDM authority, follow the steps below.

In the [Microsoft Endpoint Manager admin center](https://go.microsoft.com/fwlink/?linkid=2109431), select the orange banner to open the Mobile Device Management Authority setting. The orange banner is only displayed if you haven't yet set the MDM authority.

Under Mobile Device Management Authority, choose your MDM authority from the following options:

* Intune MDM Authority
* None

![MDM Authority in Intune](https://www.thelazyadministrator.com/wp-content/uploads/2018/10/device-enrollment-499x270.png)

![Intune set mobile device management authority screen](https://docs.microsoft.com/en-us/mem/intune/fundamentals/media/mdm-authority-set/set-mdm-auth.png)

A message indicates that you have successfully set your MDM authority to Intune. So these are the steps and location for changing your MDM authority.

**Configuring Device Enrollment Restrictions**

There are several Device enrollment restrictions options to take into consideration.

* Device Platforms
* Device OS and Versions
* Max Number of devices per user
* Block Personal Devices
* Require MFA

So these are some of the items that we can configure as we're working with enrollment restrictions in Microsoft 365. 

We have some Device enrollment configuration options that we need to be aware of 
* Restrictions: By Default users get 5 devices they can enroll. We can change that to 3 if we want to.
* Operating System and Versions: Maybe we want the OS that's most current at all times (IOS, Android, MacOS, Windows). We can change it So reflects the need of that latest version of OS.
* We can also create multiple restrictions. We can create multiple restriction, then apply them to different user groups. And set the priority (1>2 or 2>4) of these restrictions to make sure the most restrictive restrictions are applied when necessary. They don't affect any devices that are already enrolled. These will be only applicable for new devices that are being enrolled. However, this doesn't apply Intune PC agent devices, they will not be blocked. 

You can delegate responsiblities for managing all these devices. We can provide the appropriate permissions to individuals to manage our devices. This is called the Device Enrollment Manager (DEM). A global admin or Intune admin manages the members of DEM Group. Intune permissions can be applied to any Azure AD user. There is a maximum of 150 DEM and each DEM can enroll up to 1000 devices. 

**Limitations of devices that are enrolled with a DEM account**
DEM user accounts and devices that are enrolled with a DEM user account have the following limitations:

* A DEM account user must be assigned an Intune license.
* Wipe can't be done from the Company Portal. Wiping a device enrolled by a DEM user account can be done from the Intune in Azure portal.
* Only the local device appears in the Company Portal app or website.
* DEM user accounts cannot use Apple Volume Purchase Program (VPP) apps with Apple VPP user licenses because of per-user Apple ID requirements for app management.
* DEM accounts cannot be used when enrolling devices via Apple's Automated Device Enrollment (ADE).
* Devices can install VPP apps if they have Apple VPP device licenses.
* Devices are blocked for Conditional Access with the exception of Windows 10 1803+
* Every device enrolled with DEM accounts needs to be properly licensed to be managed by Intune. The license could be an Intune user license or an Intune device license.
* If you're enrolling Android Enterprise work profile devices by using a DEM account, there is a limit of 10 devices that can be enrolled per account.
* Enrolling Android Enterprise fully managed devices with DEM accounts isn't supported.

**Add a device enrollment manager**
Sign in to the [Microsoft Endpoint Manager admin center](https://go.microsoft.com/fwlink/?linkid=2109431), choose Devices > Enroll devices > Device enrollment managers.

![Device Enrollment Managers](https://www.thelazyadministrator.com/wp-content/uploads/2018/10/de1-679x1024.png)

Select Add.

![](https://www.thelazyadministrator.com/wp-content/uploads/2018/10/de2-768x490.png)

On the Add User blade, enter a user principal name for the DEM user, and select Add. The DEM user is added to the list of DEM users.

**Permissions for DEM**
Global Administrator or Intune Service Administrator Azure AD roles are required to 

* assign DEM permission to an Azure AD user account 
* see all DEM users

If a user doesn't have the Global Administrator or Intune Service Administrator role assigned to them, but has read permissions enabled for the Device Enrollment Managers role assigned to them, they can see only the DEM users they've created.

**Remove device enrollment manager permissions**
Removing a device enrollment manager doesn't affect enrolled devices.

To remove a device enrollment manager

Sign in to the [Microsoft Endpoint Manager admin center](https://go.microsoft.com/fwlink/?linkid=2109431), choose Devices > Enroll devices > Device enrollment managers.
On the Device enrollment managers blade, select the DEM user, and select Delete.

**To configure Device Enrollment Limit Restrictions**
Log into the Azure portal and select the Intune blade
Select “Device Enrollment” and then click “Enrollment Restrictions”
![](https://www.thelazyadministrator.com/wp-content/uploads/2018/10/enroll2-768x444.png)
Here I am changing the device limit from the default of 5 to 3 and then saving my changes
![](https://www.thelazyadministrator.com/wp-content/uploads/2018/10/enroll4-768x699.png)

**To configure Device Enrollment Type restriction**
Sign in to the [Microsoft Endpoint Manager admin center](https://go.microsoft.com/fwlink/?linkid=2109431) > Devices > Enrollment restrictions > Create restriction > Device type restriction.

On the Basics page, give the restriction a Name and optional Description.

Choose Next to go to the Platform settings page.

Under Platform, choose Allow for the platforms that you want this restriction to allow.
![](https://docs.microsoft.com/en-us/mem/intune/enrollment/media/enrollment-restrictions-set/choose-platform-settings.png)

Implementing Mobile Device Management for Microsoft 365

Set the mobile device management authority
https://docs.microsoft.com/en-us/intune/fundamentals/mdm-authority-set
What is device management Microsoft Intune
https://docs.microsoft.com/en-us/intune/fundamentals/what-is-device-management#microsoft-intune 

Identify mobile device management use-case scenarios
https://docs.microsoft.com/en-us/intune/fundamentals/planning-guide-scenarios 
 
Develop a rollout plan
https://docs.microsoft.com/en-us/intune/fundamentals/planning-guide-rollout-plan  

Develop a communication plan
https://docs.microsoft.com/en-us/intune/fundamentals/planning-guide-communication-plan  

Develop a support plan
https://docs.microsoft.com/en-us/intune/fundamentals/planning-guide-support-plan

What is device enrollment?
https://docs.microsoft.com/en-us/intune/enrollment/device-enrollment

Set enrollment restrictions
https://docs.microsoft.com/en-us/intune/enrollment/enrollment-restrictions-set

Azure Active Directory integration with MDM
https://docs.microsoft.com/en-us/windows/client-management/mdm/azure-active-directory-integration-with-mdm

Configure auto-enrollment of devices to Intune
https://docs.microsoft.com/en-us/configmgr/comanage/tutorial-co-manage-clients#configure-auto-enrollment-of-devices-to-intune

Set the mobile device management authority
https://docs.microsoft.com/en-us/intune/fundamentals/mdm-authority-set

Create a device type restriction
https://docs.microsoft.com/en-us/intune/enrollment/enrollment-restrictions-set#create-a-device-type-restriction
Create a device limit restriction
https://docs.microsoft.com/en-us/intune/enrollment/enrollment-restrictions-set#create-a-device-limit-restriction

Add a device enrollment manager
https://docs.microsoft.com/en-us/intune/enrollment/device-enrollment-manager-enroll#add-a-device-enrollment-manager


