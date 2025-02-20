Properly organizing your subscriptions will play a key role in governance for your cloud resources. It also ensures you can manage the costs for resources and quotas for deployed resources in your organization.

## Define a management group hierarchy

Management-group structures within a Microsoft Entra tenant support organizational mapping must be considered thoroughly when an organization plans Azure adoption at scale.

:::image type="content" source="../media/4-subscription-organization.png" alt-text="Diagram that shows management group hierarchy." lightbox="../media/4-subscription-organization-large.png":::

When you define the management-group hierarchy, consider if management groups can be aggregated by policy and initiative assignments via Azure Policy. This capability will provide flexibility and organization of policy across multiple subscriptions. Also, keep in consideration that a management group tree can support up to six levels of depth. This limit doesn't include the tenant root level or the subscription level.

We recommend that you keep the management-group hierarchy reasonably flat, ideally with no more than three to four levels. This restriction will reduce management overhead and complexity. Avoid duplicating your organizational structure into a deeply nested management-group hierarchy. Management groups should be used for policy assignments versus billing purposes. This approach necessitates using management groups for their intended purpose in enterprise-scale architecture. That purpose is to provide Azure policies for workloads that require the same type of security and compliance under the same management group level.

Create management groups under your root-level management group to represent the types of workloads (archetypes) that you'll host and ones based on their security, compliance, connectivity, and feature needs. This grouping structure allows you to have a set of Azure policies applied at the management-group level for all workloads that require the same security, compliance, connectivity, and feature settings.

Use resource tags, which can be enforced or appended through Azure Policy, to query and horizontally navigate across the management-group hierarchy. Then you can group resources for search needs without having to use a complex management-group hierarchy.

Create a top-level sandbox management group to allow users to immediately experiment with Azure. Users can then experiment with resources that might not yet be allowed in production environments. The sandbox provides isolation from your development, test, and production environments.

Use a dedicated service principal name (SPN) to execute management group management operations, subscription management operations, and role assignment. Using an SPN reduces the number of users who have elevated rights and follows least-privilege guidelines.

Assign the User Access Administrator role provided by Azure role-based access control (RBAC) at the root management group scope (`/`) to grant the just-mentioned SPN access at the root level. After the SPN is granted permissions, the User Access Administrator role can be safely removed. In this way, only the SPN is part of the User Access Administrator role.

Assign the Contributor role to the SPN previously mentioned at the root management group scope (`/`), which allows tenant-level operations. This permission level ensures that the SPN can be used to deploy and manage resources to any subscription within your organization.

Create a platform-management group under the root management group to support common platform policy and RBAC assignment. This grouping structure ensures that different policies can be applied to the subscriptions used for your Azure foundation. It also ensures that the billing for common resources is centralized in one set of foundational subscriptions.

Also, limit the number of Azure Policy assignments made at the root management group scope (`/`). This limitation minimizes debugging inherited policies in lower-level management groups. Don't create any subscriptions under the root management group. This hierarchy ensures that subscriptions don't inherit only the small set of Azure policies assigned at the root-level management group, which doesn't provide a full set necessary for a workload.

## Subscription organization and governance

Subscriptions are a unit of management, billing, and scale within Azure. They play a critical role when you're designing for large-scale Azure adoption. This section helps you capture subscription requirements and design target subscriptions based on critical factors. These factors are environment type, ownership and governance model, organizational structure, and application portfolios.

Remember, subscriptions serve as boundaries for assigning Azure policies. For example, secure workloads such as Payment Card Industry (PCI) workloads typically require additional policies to achieve compliance. Instead of using a management group to group workloads that require PCI compliance, you can achieve the same isolation with a subscription. This way, you won't have too many management groups with a small number of subscriptions.

Subscriptions also serve as a scale unit so that component workloads can scale within the platform subscription limits. Make sure to consider subscription resource limits during your workload design sessions. Subscriptions provide a management boundary for governance and isolation, which creates a clear separation of concerns.

When defining your subscription organization and governance, treat subscriptions as a democratized unit of management aligned with business needs and priorities.
Make subscription owners aware of their roles and responsibilities:

  - Perform an access review in Microsoft Entra Privileged Identity Management quarterly or twice a year to ensure that privileges don't proliferate as users move within the customer organization.
  - Take full ownership of budget spending and resource utilization.
  - Ensure policy compliance and remediate when necessary.

Use the following principles when identifying requirements for new subscriptions:

  - **Scale limits**: Subscriptions serve as a scale unit for component workloads to scale within platform subscription limits. For example, large, specialized workloads such as high-performance computing, IoT, and SAP are all better suited to use separate subscriptions to avoid limits (such as a limit of 50 Azure Data Factory integrations).
  - **Management boundary**: Subscriptions provide a management boundary for governance and isolation, which allows for a clear separation of concerns. For example, different environments such as development, test, and production are often isolated from a management perspective.
  - **Policy boundary**: Subscriptions serve as a boundary for the assignment of Azure Policy. For example, secure workloads such as PCI typically require additional policies to achieve compliance. This additional overhead doesn't need to be considered holistically if a separate subscription is used. Similarly, development environments might have more relaxed policy requirements relative to production environments.
  - **Target network topology**: Virtual networks can't be shared across subscriptions, but they can connect with different technologies such as virtual network peering or Azure ExpressRoute. Consider which workloads must communicate with each other when you decide whether a new subscription is required.

Group subscriptions together under management groups aligned within the management group structure and policy requirements at scale. Grouping ensures that subscriptions with the same set of policies and RBAC assignments can inherit them from a management group, which avoids duplicate assignments.

Establish a dedicated management subscription in the platform-management group to support global management capabilities, such as Azure Monitor Log Analytics workspaces and Azure Automation runbooks. Establish a dedicated identity subscription in the platform-management group to host Windows Server Active Directory domain controllers, when necessary. Establish a dedicated connectivity subscription in the platform-management group to host an Azure Virtual WAN hub, private domain name system (DNS), ExpressRoute circuit, and other networking resources. A dedicated subscription ensures that all foundation network resources are billed together and isolated from other workloads.

Avoid a rigid subscription model, and opt instead for a set of flexible criteria to group subscriptions across the organization. This flexibility ensures that as your organization's structure and workload composition changes, you can create new subscription groups instead of using a fixed set of existing subscriptions. One size doesn't fit all for subscriptions. What works for one business unit might not work for another. Some applications might coexist within the same landing zone subscription, while others might require their own subscription.

## Configure subscription quota and capacity

Each Azure region contains a finite number of resources. When you consider an enterprise-scale Azure adoption that involves large resource quantities, ensure that sufficient capacity and SKUs are available and the attained capacity can be understood and monitored.

Consider limits and quotas within the Azure platform for each service that your workloads require. Also, consider the availability of required SKUs within chosen Azure regions. For example, new features might be available only in certain regions. The availability of certain SKUs for given resources, such as VMs, might be different from one region to another. Keep in mind that subscription quotas aren't capacity guarantees and are applied on a per-region basis.

When planning for quotas and capacity, use subscriptions as scale units, and scale out resources and subscriptions as required. Your workload can then use the required resources for scaling out, when needed, without hitting subscription limits in the Azure platform. Use reserved instances to prioritize reserved capacity in required regions. Then your workload will have the required capacity even when there's a high demand for that resource in a specific region.

Establish a dashboard with custom views to monitor used capacity levels. Set up alerts if capacity utilization is reaching critical levels (for example, 90 percent CPU utilization).

Raise support requests for quota increase as a part of subscription provisioning (for example, total available VM cores within a subscription). This approach ensures your quota limits are set before your workloads require going over the default limits.

Ensure required services and features are available within the chosen deployment regions.

## Establish cost management

Cost transparency across a technical estate is a critical management challenge faced by every large enterprise organization. This section explores key aspects associated with how cost transparency can be achieved across large Azure environments.

Consider a potential need for chargeback models where shared platform as a service (PaaS) resources are concerned, such as Azure App Service Environment and Azure Kubernetes Service, which might need to be shared to achieve higher density. Use a shutdown schedule for nonproduction workloads to optimize costs. Use Azure Advisor to check cost optimization recommendations.

We recommend the use of Microsoft Cost Management for cost aggregation. Make it available to application owners. Use Azure resource tags for cost categorization and resource grouping. Using tags allows you to have a chargeback mechanism for workloads that share a subscription or for a given workload that spans across multiple subscriptions.
