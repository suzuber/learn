We can pull container images from Azure Container Registry using many container management platforms, such as Azure Container Instances, Azure Kubernetes Service, and Docker for Windows or Mac. Here, we'll deploy our image to an Azure Container Instance.

[!include[](../../../includes/azure-exercise-subscription-prerequisite.md)]

## About registry authentication

Azure Container Registry doesn't support unauthenticated access and requires authentication for all operations. Registries support two types of identities:

- **Microsoft Entra identities**, including both user and service principals. Access to a registry with a Microsoft Entra identity is role-based, and identities can be assigned one of three roles: **reader** (pull access only), **contributor** (push and pull access), or **owner** (pull, push, and assign roles to other users).
- The **admin account** included with each registry. The admin account is disabled by default.

The admin account provides a quick option to try a new registry. You can enable the account and use its username and password in workflows and apps that need access. After you've confirmed the registry works as expected, you should disable the admin account, and use Microsoft Entra identities exclusively to ensure the security of your registry.

> [!IMPORTANT]
> Only use the registry admin account for early testing and exploration, and do not share the username and password. Disable the admin account and use only role-based access with Microsoft Entra identities to maximize the security of your registry.

## Enable the registry admin account

In this exercise, we'll enable the registry admin account, and use it to deploy your image to an Azure Container Instance from the command line.

1. Run the following command in Cloud Shell to enable the admin account on your registry.

    ```azurecli
    az acr update -n $ACR_NAME --admin-enabled true
    ```

1. Run the following command in Cloud Shell to retrieve the username and password for the admin account you enabled in the preceding step.

    ```azurecli
    az acr credential show --name $ACR_NAME
    ```

1. Take note of the `username` and  `password` values that are returned from this command. You'll need them later in this exercise.

## Deploy a container with Azure CLI

1. Run the following `az container create` command to deploy a container instance, replacing `<location>` with the value returned when you created the container registry and replacing `<username>`,`<password>` with your registry's admin username and password from the previous task.

    ```azurecli
    az container create \
        --resource-group learn-deploy-acr-rg \
        --name acr-tasks \
        --image $ACR_NAME.azurecr.io/helloacrtasks:v1 \
        --registry-login-server $ACR_NAME.azurecr.io \
        --ip-address Public \
        --location <location> \
        --registry-username [username] \
        --registry-password [password]
    ```

1. Get the IP address of the Azure container instance by running the following command.

    ```azurecli
    az container show --resource-group  learn-deploy-acr-rg --name acr-tasks --query ipAddress.ip --output table
    ```

1. Open a browser and enter the IP address of the container. If everything has been configured correctly, the following results appear.

    :::image type="content" source="../media/hello.png" alt-text="Screenshot of a browser window that shows a web page with text that reads: Hello World. Version: 9.11.2." loc-scope="other"::: <!-- no-loc -->
