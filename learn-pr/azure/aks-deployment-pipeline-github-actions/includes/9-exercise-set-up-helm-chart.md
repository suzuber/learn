Let's set up the environment to deploy a Helm chart to AKS by using GitHub Actions.

In this exercise, to deploy a Helm chart, you'll complete the following tasks:

- Check the Helm installation
- Create a chart
- Configure the chart
- Create a deployment
- Create an empty YAML file
- Add contents to the YAML file
- Create a service
- Create an ingress
- Create a DNS zone name

## Check the Helm installation

1. In Cloud Shell, the Helm CLI is already installed. Sign in to Azure Cloud Shell by using the account you want to deploy resources to.

    > [!div class="nextstepaction"]
    > [Azure Cloud Shell](https://shell.azure.com/?azure-portal=true)

    > [!IMPORTANT]
    > We'll run all the scripts with Bash. If you haven't created a Cloud Shell yet, select **Bash** as the terminal. For more information about setting up Cloud Shell, see [Overview of Azure Cloud Shell](/azure/cloud-shell/overview#connect-your-microsoft-azure-files-storage).

1. Run `helm version` to check whether the displayed version is greater than **3**.

1. To pull the changes you've made to the CI workflow in the preceding units, run this command:

    ```bash
    git pull origin main
    ```

## Create a chart

1. While inside the learn repository you forked go to the `kubernetes` directory.

    ```bash
    cd kubernetes
    ```

1. While inside, generate a boilerplate template `with helm create`:

    ```bash
    helm create contoso-website
    ```

   The command creates a new directory called `contoso-website` in the `kubernetes` directory.

1. Switch to the new directory with `cd`.

    ```bash
    cd contoso-website
    ```

1. Delete the `charts` and `templates` folders in that directory.

    ```bash
    rm -r charts templates
    ```

1. Run the following command to create a new empty `templates` folder:

    ```bash
    mkdir templates
    ```

    You've created an empty chart. To start building the workloads, use what others have already built. Create a new set of YAML files

1. Move the old `kubernetes` files to the `templates` folder.

1. From inside the `contoso-website` directory, run this command:

    ```bash
    mv ../*.yaml ./templates
    ```

Completing these steps is all it takes to create a chart. Now, let's configure the chart.

## Configure the chart

1. Run `cd ../..` to return back to the learn repo.

1. To open the editor in the current directory, run `code .` .

1. In the left menu, expand the `kubernetes` folder, open the `contoso-website` folder,  and open the `Chart.yaml` file.

    `Chart.yaml` is the file that names the chart. This file is where Helm looks for information about the chart itself. You should have a file that looks like this example:

    ```yaml
    apiVersion: v2
    name: contoso-website
    description: A Helm chart for Kubernetes

    # A chart can be either an application or a library chart.
    #
    # Application charts are a collection of templates that can be packaged into versioned archives to be deployed.
    #
    # Library charts provide useful utilities or functions for the chart developer. They're included as
    # a dependency of application charts to inject those utilities and functions into the rendering
    # pipeline. Library charts do not define any templates and therefore cannot be deployed.
    type: application

    # This is the chart version. This version number should be incremented each time you make changes
    # to the chart and its templates, including the app version.
    # Versions are expected to follow semantic versioning (https://semver.org/).
    version: 0.1.0

    # This is the version number of the application that's being deployed. This version number should be
    # incremented each time you make changes to the application. Versions are not expected to
    # follow semantic versioning. They should reflect the version the application is using.
    appVersion: 1.16.0
    ```

    Remove all the comments and unused keys. Leave only the required options, and edit them to look like this example:

    ```yaml
    apiVersion: v2
    name: contoso-website
    description: Chart for the Contoso company website
    version: 0.1.0
    ```

1. Save and close the file.

## Create a deployment

1. In the left menu, go to the `kubernetes` folder. Find the `deployment.yaml` file in the `templates` folder.

    The file is similar to this initial file:

    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: contoso-website
    spec:
      selector:
        matchLabels:
          app: contoso-website
      template:
        metadata:
          labels:
            app: contoso-website
        spec:
          containers:
            - image: !IMAGE!
              name: contoso-website
              resources:
                requests:
                  cpu: 100m
                  memory: 128Mi
                limits:
                  cpu: 250m
                  memory: 256Mi
              ports:
                - containerPort: 80
                  name: http
    ```

    Next, we add templates for this deployment, beginning with the `namespace` and `name` keys.

1. In the `metadata` section, add a new key called `namespace` with the value `{{ default "staging" .Release.Namespace }}`.  

    The metadata configuration:

    ```yaml
    metadata:
      name: contoso-website
      namespace: {{ default "staging" .Release.Namespace }}
    ```

    >[!NOTE]
    > By default, the workflow will deploy this resource to the `staging` namespace. But, if the `helm install` command has a `namespace` option, it will overide it to use that instead.

1. Under the specs for the container, update the `image` key to fetch the latest and tag versions from your AKS cluster.

    Use the templating values of `{{ .Values.image.registry }}.azurecr.io/{{ .Values.image.name }}:{{ default "latest" .Values.image.tag }}`. It's a good practice to split up the `registry`, `tag`, and `image` parts of the image name. Add three new template variables to this section of the file:

    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: contoso-website
      namespace: {{ default "staging" .Release.Namespace }}
    spec:
      selector:
        matchLabels:
          app: contoso-website
      template:
        metadata:
          labels:
            app: contoso-website
        spec:
          containers:
            - image: {{ .Values.image.registry }}.azurecr.io/{{ .Values.image.name }}:{{ default "latest" .Values.image.tag }}
              name: contoso-website
              resources:
                requests:
                  cpu: 100m
                  memory: 128Mi
                limits:
                  cpu: 250m
                  memory: 256Mi
              ports:
                - containerPort: 80
                  name: http
    ```

    In this section, you split the three sections of the image to work with them more easily.

1. Save and close the file.

## Add contents to the values.yaml file

1. In the root of the `contoso-website` directory, open the `values.yaml` file.

1. Delete all contents in the file, so you have an empty YAML file.

Now, let's add your content to the empty file.

You saw earlier that you used `{{ .Release.Namespace }}`, so `Release` is a *variable scope*. Each variable scope has different default values and variables.

Helm uses the `values.yaml` file to retrieve all the template values that start with `{{ .Values }}`. The `values.yaml` file is another variable scope.

This file should have the same structure of the file you use to call variables. Let's take a quick look in the `deployment.yaml` file you edited to see the structure.

Notice that you used `.Values.image.registry`, `.Values.image.name`, and `.Values.image.tag` in the `deployment.yaml` file.

1. Edit the `values.yaml` file so that it looks like this example:

    ```yaml
    image:
      registry: <your-acr-name>
      name: contoso-website
      tag: latest
    ```

    Replace `<your-acr-name>` with the value from exercise 6.
    These values are the *default* values if you don't pass a new value as a parameter by using the `--set` option of the Helm CLI.

1. Save and close the file.

## Create a service

1. Find and open the `service.yaml` file in the `templates` folder.

1. In the `metadata` section of the file, add a new key called `namespace`. Use the same value that you used in the `deployment.yaml` file.

    ```yaml
    apiVersion: v1
    kind: Service
    metadata:
      name: contoso-website
      namespace: {{ default "staging" .Release.Namespace }}
    spec:
      ports:
        - port: 80
          protocol: TCP
          targetPort: http
          name: http
      selector:
        app: contoso-website
      type: ClusterIP
    ```

1. Save and close the file.

## Create an ingress

1. Find and open the `ingress.yaml` file.

1. In the `metadata` section of the file, once again add the `namespace` value that you used in the `deployment.yaml` file.

    ```yaml
    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
      name: contoso-website
      namespace: {{ default "staging" .Release.Namespace }}
      annotations:
        kubernetes.io/ingress.class: addon-http-application-routing
    spec:
      rules:
        - host: contoso.!DNS!
          http:
            paths:
              - backend:
                  service:
                    name: contoso-website
                    port:
                      name: http
                path: /
                pathType: Prefix
    ```

1. Go to the `host` key. You create separate hosts for staging and production deployments. You don't want users to access the `staging` namespace by using production URLs.

1. Concatenate the namespace in the host name. The HTTP application routing add-on in the AKS cluster handles name resolution.

    ```yaml
    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
      name: contoso-website
      namespace: {{ default "staging" .Release.Namespace }}
      annotations:
        kubernetes.io/ingress.class: addon-http-application-routing
    spec:
      rules:
        - host: contoso-{{ default "staging" .Release.Namespace }}.!DNS!
          http:
            paths:
              - backend:
                  service:
                    name: contoso-website
                    port:
                      name: http
                path: /
                pathType: Prefix
    ```

1. Add a new template variable for your DNS zone name:

    ```yaml
    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
      name: contoso-website
      namespace: {{ default "staging" .Release.Namespace }}
      annotations:
        kubernetes.io/ingress.class: addon-http-application-routing
    spec:
      rules:
        - host: contoso-{{ default "staging" .Release.Namespace }}.{{ .Values.dns.name }}
          http:
            paths:
              - backend:
                  service:
                    name: contoso-website
                    port:
                      name: http
                path: /
                pathType: Prefix
    ```

1. Save and close the file.

## Create a DNS zone name

1. Open the `values.yaml` file.

1. Add the `dns.name` key, so the file looks like this example:

    ```yaml
    image:
      registry: <your-acr-name>
      name: contoso-website
      tag: latest

    dns:
      name: <your-dns-zone-name>
    ```

    To get your DNS zone name, run this Azure CLI query:

    ```azurecli-interactive
    az aks show -g {resource-group-name} -n {aks-cluster-name} -o tsv --query addonProfiles.httpApplicationRouting.config.HTTPApplicationRoutingZoneName
    ```

    Replace `<your-dns-zone-name>` with the output value and `<your-acr-name>` with the value from exercise 6.

1. Save and close the file.

1. To push all the changes to the fork, run these commands in order:

    ```bash
    git add .
    ```

    ```bash
    git commit -m "Add helm"
    ```

    ```bash
    git push -u origin main
    ```

1. When prompted, provide your GitHub username, and the PAT created previously as the password.
