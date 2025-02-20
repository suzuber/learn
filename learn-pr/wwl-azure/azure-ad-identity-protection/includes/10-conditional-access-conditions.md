Conditional access is a capability of Microsoft Entra ID (with a Microsoft Entra ID P1 or P2 license) that enables you to enforce controls on the access to apps in your environment based on specific conditions from a central location. With Microsoft Entra Conditional Access, you can factor how a resource is being accessed into an access control decision. By using conditional access policies, you can apply the correct access controls under the required conditions.

Conditional access comes with six conditions: user/group, cloud application, device state, location (IP range), client application, and sign-in risk. You can use combinations of these conditions to get the exact conditional access policy you need. Notice on this image the conditions determine the access control from the previous topic.

:::image type="content" source="../media/az500-conditional-access-policies-fed794b4.png" alt-text="Daigram shows that conditional access comes with six conditions: user/group, cloud application, device state, location (IP range), client application, and sign-in risk.":::


With access controls, you can either Block Access altogether or Grant Access with more requirements by selecting the desired controls. You can have several options:

 -  Require MFA from Microsoft Entra ID or an on-premises MFA (combined with AD FS).
 -  Grant access to only trusted devices.
 -  Require a domain-joined device.
 -  Require mobile devices to use Intune app protection policies.

Requiring more account verification through MFA is a common conditional access scenario. While users may be able to sign in to most of your organization’s cloud apps, you may want more verification for things like your email system, or apps that contain personnel records or sensitive information. In Microsoft Entra ID, you can accomplish this with a conditional access policy

> [!IMPORTANT]
> The Users and Groups condition is mandatory in a conditional access policy. In your policy, you can either select All users or select specific users and groups.
