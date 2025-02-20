[Microsoft Entra ID](/azure/active-directory/) is Microsoft's multitenant cloud-based directory and identity management service. Microsoft Entra ID helps to support user access to resources and applications, such as:

- Internal resources and apps located on your corporate network.

- External resources like Microsoft 365, the Azure portal, and SaaS applications. 

- Cloud apps developed for your organization.

The following diagram shows an example implementation of Microsoft Entra ID. In this scenario, Windows Server AD is using [Kerberos](/windows-server/security/kerberos/kerberos-authentication-overview) and [NTLM authentication](/windows-server/security/kerberos/ntlm-overview) to on-premises applications.

:::image type="content" source="../media/azure-active-directory-a3b1df09.png" alt-text="Diagram that shows Windows Server Active Directory using Kerberos and NTLM authentication to on-premises apps." border="false":::

## Things to know about Microsoft Entra features

Let's examine some of the prominent features of Microsoft Entra ID. 

| Azure&nbsp;AD&nbsp;feature | Description |
| --- | --- |
| **Single sign-on (SSO) access** | Microsoft Entra ID provides secure single sign-on (SSO) to web apps on the cloud and to on-premises apps. Users can sign in with the same set of credentials to access all their apps. |
| **Ubiquitous device support** | Microsoft Entra ID works with iOS, macOS, Android, and Windows devices, and offers a common experience across the devices. Users can launch apps from a personalized web-based access panel, mobile app, Microsoft 365, or custom company portals by using their existing work credentials. |
| **Secure remote access** | Microsoft Entra ID enables secure remote access for on-premises web apps. Secure access can include multifactor authentication (MFA), conditional access policies, and group-based access management. Users can access on-premises web apps from everywhere, including from the same portal. |
| **Cloud extensibility** | Microsoft Entra ID can extend to the cloud to help you manage a consistent set of users, groups, passwords, and devices across environments. |
| **Sensitive data protection** | Microsoft Entra ID offers unique identity protection capabilities to secure your sensitive data and apps. Admins can monitor for suspicious sign-in activity and potential vulnerabilities in a consolidated view of users and resources in the directory. |
| **Self-service support** | Microsoft Entra ID lets you delegate selected administrator tasks to company employees. Providing self-service app access and password management through verification steps can reduce helpdesk calls and enhance security. |

## Things to consider when using Microsoft Entra features

Microsoft Entra ID offers many features and benefits. Consider which features can be used to best support your corporate scenarios.

- **Consider enabling single sign-on access**. Enable SSO access for your users to connect to the cloud or use on-premises apps. Microsoft Entra SSO supports Microsoft 365 and thousands of SaaS apps, such as Salesforce, Workday, DocuSign, ServiceNow, and Box.

- **Consider UX and device support**. Build a consistent user experience that works for all devices and directory access points. You can design custom company portals and personalized web-based access for your employees that lets them connect with their existing work credentials.

- **Consider benefits of secure remote access**. Protect your on-premises web apps by implementing secure remote access with MFA and access policies.

- **Consider advantages of cloud extensibility**. Connect Active Directory and other on-premises directories in the cloud to Microsoft Entra ID in just a few steps. You can make it easier for your admins to manage the same users, groups, passwords, and devices across all supported environments.

- **Consider advanced protection for sensitive data**. Enhance the security of your sensitive data and apps by using the built-in protection features of Microsoft Entra ID. Your admins can utilize advanced security reports, notifications, remediation recommendations, and risk-based policies.

- **Consider reduced costs, self-service options**. Take advantage of the Microsoft Entra self-service features to help reduce costs for your organization. Delegate certain tasks like resetting passwords, or creating and managing groups to your nonadmin users.

In the next unit, we explore the Microsoft Entra concepts that make these features possible.

