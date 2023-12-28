# Node.JS + MySQL Web App.<br><br>HELM deployment and upgrading the app on a Kubernetes cluster launched on MS Azure AKS.

<p align="center">
  <a href="https://skillicons.dev">
    <img height="50" width="50" src="https://cdn.simpleicons.org/helm/blue" /><img src="https://skillicons.dev/icons?i=kubernetes,azure,docker,nodejs,mysql"/>
  </a>
</p>



<br>

> [!NOTE]
> This is a part of a series of demo projects in which I manipulate a Node.js application applying various technologies.<br>
>
> The app built using Node.js and Express and integrates with MySQL database, originally presented at this [GitHub Repository](https://github.com/otam-mato/nodejs_mysql_web_app_terraform.git). The previous [project](https://github.com/otam-mato/nodejs_mysql_web_app_kubernetes.git) involved the "Canary" deployment on **AWS EKS**.
>
> In the current installment, we're deploying the web application on **MS Azure AKS** using **HELM**. Subsequently, we'll introduce the second version of the app and then rollback to the first version.
<br>

## Deployment Strategy

The **HELM** is a package manager for Kubernetes applications. It simplifies the deployment and management of applications on Kubernetes clusters. Helm uses charts, which are packages of pre-configured Kubernetes resources, to define, install, and upgrade even the most complex Kubernetes applications and their dependencies. It helps streamline the deployment process, reduce errors, and enable versioning and rollbacks for Kubernetes applications.

1. In this demo, we will use **HELM** to deploy:
   - V1 deployment which will host the version 1 of the app.
   - V2 deployment which will host the version 2 of the app.
   - Finally we will rollback V2 to V1.

   V1 and V2 versions were previously uploaded into this [DockerHub repository](https://hub.docker.com/repository/docker/montcarotte/fullstack_nodejs_mysql_demo/general)

2. **The app and its database are placed in separate cantainers and separate Kubernetes pods to secure decoupling, resource isolation, scaling and resilience.**

<br>

## Technologies used
- **MS Azure**
- **AKS (Azure Kubernetes Service)**
- **Node.JS**
- **Express**
- **JavaScript**
- **MySQL**
- **Docker**
- **Kubernetes**
- **HELM**
<br>

## Application Functionality

This web application interfaces with a MySQL database, facilitating CRUD (Create, Read, Update, Delete) operations on the database records.

**<details markdown=1><summary markdown="span">Detailed app description</summary>**

## Summary

The app sets up a web server for a supplier management system. It allows viewing, adding, updating, and deleting suppliers. 

#### **Dependencies and Modules**:
   - **express**: The framework that allows us to set up and run a web server.
   - **body-parser**: A tool that lets the server read and understand data sent in requests.
   - **cors**: Ensures the server can communicate with different web addresses or domains.
   - **mustache-express**: A template engine, letting the server display dynamic web pages using the Mustache format.
   - **serve-favicon**: Provides the small icon seen on browser tabs for the website.
   - **Custom Modules**: 
     - `supplier.controller`: Handles the logic for managing suppliers like fetching, adding, or updating their details.
     - `config.js`: Keeps the server's settings for connectind to the MySQL database.

#### **Configuration**:
   - The server starts on a port taken from a setting (like an environment variable) or uses `3000` as a default.

#### **Middleware**:
   - It's equipped to understand data in JSON format or when it's URL-encoded.
   - It can chat with web pages hosted elsewhere, thanks to CORS.
   - Mustache is the chosen format for web pages, with templates stored in a folder named `views`.
   - There's a public storage (`public`) for things like images or stylesheets, accessible by anyone visiting the site.
   - The site's tiny browser tab icon is fetched using `serve-favicon`.

#### **Routes (Webpage Endpoints)**:
   - **Home**: `GET /`: Serves the home page.
   - **Supplier Operations**: 
     - `GET /suppliers/`: Fetches and displays all suppliers.
     - `GET /supplier-add`: Serves a page to add a new supplier.
     - `POST /supplier-add`: Receives data to add a new supplier.
     - `GET /supplier-update/:id`: Serves a page to update details of a supplier using its ID.
     - `POST /supplier-update`: Receives updated data of a supplier.
     - `POST /supplier-remove/:id`: Removes a supplier using its ID.

#### **Starting Up**:
   - The server comes to life, starts listening for visits, and announces its awakening with a log message.

</details>

<br>

## Implementation Steps

Follow these steps for successful implementation:

1. [**Create an AKS cluster**](https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/blob/main/README.md#prerequisites)
2. [**Create an Azure VM**](https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/blob/main/README.md#prerequisites)
3. [**Install Azure CLI**](https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/blob/main/README.md#prerequisites)
4. [**Create a Kubernetes Kluster in Azure AKS**](https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/blob/main/README.md#prerequisites)
5. [**Connect the Azure VM to AKS cluster**](https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/blob/main/README.md#prerequisites)
6. [**Install HELM**](https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/blob/main/README.md#prerequisites)
7. [**Create a HELM chart**](https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/blob/main/README.md#1-create-the-helm-chart)
8. [**Clone the current Git repository and update the HELM project's structure**](https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/blob/main/README.md#1-create-the-helm-chart)
9. [**HELM `lint`**](https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/blob/main/README.md#4-helm-lint)
10. [**HELM `template`**](https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/blob/main/README.md#5-helm-template)
12. [**Deploy the V1 app (HELM `install`)**](https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/blob/main/README.md#6-helm-install)
13. [**Modify the configuration (./values.yaml and ./Chart.yaml) to use V2 of the app**](https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/blob/main/README.md#7-modify-the-configuration-valuesyaml-and-chartyaml-to-use-v2-of-the-app)
14. [**Upgrade the app to V2 (HELM `upgrade`)**](https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/blob/main/README.md#8-helm-upgrade)
15. [**Roll the V2 back to V1 (HELM `rollback`)**](https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/blob/main/README.md#9-helm-rollback)

<br>

<br>

## Architecture Diagram

<p align="center">
  <img src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/6d362bfd-94a0-4bc0-9bc3-613e600d6eab" width="1200px"/>
</p>

<br>

## Screenshots

<p align="center">
  <img src="https://github.com/otam-mato/nodejs_mysql_web_app_kubernetes/assets/113034133/7edead55-90b5-4e4e-9a45-eb9c2497547b" width="1000px"/>
</p>

<p align="center">
  <img src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/4c6be1a2-1c37-40e7-92d5-c4efb2e3dbf2" width="1000px"/>
</p>

<p align="center">
  <img src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/9bfacd03-6ba4-4df2-ac05-0ce9e4394ec9" width="1000px"/>
</p>

<p align="center">
  <img src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/597c7427-7716-4435-8cee-5b616733a464" width="1000px"/>
</p>


<br>

## Prerequisites

- **MS Azure** account.

- <details markdown=1><summary markdown="span">Create an Azure VM</summary>
  <br>
  <img width="1000" alt="Screenshot 2023-12-28 at 08 00 15" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/c2d4baff-32e4-4e37-a3a8-cf470ff06518">
  </details>
- <details markdown=1><summary markdown="span">Install Azure CLI</summary>
  <br>
  
  ```sh
  apt install azure-cli
  ```
  
  https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-linux?pivots=apt
  
  </details>
- <details markdown=1><summary markdown="span">Create an AKS cluster</summary>
  <br>
  <img width="1000" alt="Screenshot 2023-12-28 at 08 02 05" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/f202bcc1-f89f-4e4d-843b-8847b2be3206">
  </details>
- <details markdown=1><summary markdown="span">Connect Azure VM to AKS cluster</summary>
  <br>
  <img width="700" alt="Screenshot 2023-12-27 at 22 08 36" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/28b9a0a6-19d3-4b03-8ca1-0524d53f099e">
  <img width="700" alt="Screenshot 2023-12-27 at 22 07 04" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/fba45a12-5b3f-4a1d-87d5-4278b9a723ea">
  <img width="700" alt="Screenshot 2023-12-27 at 22 07 34" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/07d4dbca-b212-476e-9b4d-45af9877cd5f">
  <img width="700" alt="Screenshot 2023-12-27 at 22 07 43" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/a4157378-a80f-4fd1-bafa-7ae26aac9837">
  <img width="700" alt="Screenshot 2023-12-27 at 22 09 48" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/a449c5b4-f049-43a5-9e78-a6c7b0207ae8"> 
  <img width="700" alt="Screenshot 2023-12-27 at 22 12 35" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/b287fa47-90bf-4d29-90cb-f5b172d8eccf">
  </details>
- <details markdown=1><summary markdown="span">Install HELM</summary>
  <br>
  
  ```sh
  curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
  sudo apt-get install apt-transport-https --yes
  echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
  sudo apt-get update
  sudo apt-get install helm
  ```

  https://helm.sh/docs/intro/install/
  
  </details>

<br>

## Steps:

### 1. Create the HELM chart

- **create the HELM project structure**

  ```sh
  helm create ./coffee_app
  ```

  <img width="692" alt="Screenshot 2023-12-27 at 21 01 17" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/3d1cbf47-8d72-448c-b24c-f940e8da809f">

- **remove the unnecessary for this project files**

  ```bash
  rm ./coffee_app/templates/* -r
  ```

  <img width="692" alt="Screenshot 2023-12-27 at 21 04 29" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/04df1638-41ee-4804-8958-3abf418a310c">


### 2. Clone the current Git repository and update the project's structure

- **clone the current repository**

  ```sh
  git clone https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment.git
  ```

- **place the cloned charts into the HELM project folder "coffee_app"**

  ```
  cp ./nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/templates ./coffee_app/templates/
  ```

  <img width="893" alt="Screenshot 2023-12-27 at 21 19 59" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/79b972eb-afae-4c74-b9ac-66bab54b7f53">

- **create the projects chart in Chart.yml**
  
  <img width="893" alt="Screenshot 2023-12-27 at 21 31 38" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/b00e45c6-a155-4104-89f8-1274710c7699">

  The `Chart.yaml` file is a metadata file that provides essential information about a Helm chart. It is a required file in every Helm chart, and it contains details such as the chart's name, version, description, and other metadata. The information in the Chart.yaml file helps users and Helm itself understand the characteristics of the chart.

- **create the ./values.yaml**

  <img width="893" alt="Screenshot 2023-12-27 at 21 37 53" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/b2bef742-b70c-4021-ad71-81d8a5b579a3">

  In Helm, the `values.yaml` file is a configuration file used to define default values for Helm charts. It allows users to specify configuration parameters and their default values, which can then be overridden or customized when deploying the Helm chart.

- **the project's final sructure:**

  <img width="931" alt="Screenshot 2023-12-27 at 21 44 29" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/a960ab8e-0a5c-4537-9964-6b8b63a8059e">

### 4. HELM `lint`

- HELM's "lint" command  is used in Helm to perform a static analysis of a chart. It helps identify issues and errors in a Helm chart before it is deployed to a Kubernetes cluster. The lint command checks the chart's syntax, structure, and best practices, providing feedback on potential problems.

  ```sh
  helm lint ./coffee_app/
  ```
  
  <img width="931" alt="Screenshot 2023-12-27 at 21 47 51" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/9ad1a0a6-60bf-4d40-813d-fbf0eed06ad3">

  Running `helm lint` is a good practice before deploying a Helm chart to catch potential issues early in the development and deployment process. It helps ensure that the chart is well-formed, follows best practices, and is less likely to cause problems when applied to a Kubernetes cluster.

### 5. HELM `template`

- The `helm template` command is used to render a Helm chart locally without installing it into a Kubernetes cluster. This command is useful for generating the Kubernetes YAML manifests based on a Helm chart, allowing you to inspect the resulting output before deploying it to a cluster. It's often employed during development, testing, or as part of a continuous integration (CI) pipeline.

  ```yml
  helm template coffee_app/
  ```
  
  <img width="863" alt="Screenshot 2023-12-27 at 21 52 58" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/4bb46d6c-e981-4c4a-b938-9add7e5fb279">

### 6. HELM `install`

- install the chart

  ```yml
  helm install coffeeapp coffee_app/
  ```

  <img width="888" alt="Screenshot 2023-12-27 at 22 15 17" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/5767b3d5-c27e-42f9-9bd5-b6d8e82f0a98">

  The `helm install` command is used to deploy a Helm chart onto a Kubernetes cluster. It takes a chart as input, installs it into the cluster, and creates a release, which is an instance of that chart running on the cluster.
  
  <img width="888" alt="Screenshot 2023-12-27 at 22 21 38" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/b4be7aa6-849f-4258-9f40-82f286868d2b">
  <img width="888" alt="Screenshot 2023-12-27 at 22 23 33" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/fc037037-8eb3-4996-b073-93a37fbabd89">

  Now, the version V1 of the app is deployed on the AKS cluster
  
### 7. Modify the configuration (./values.yaml and ./Chart.yaml) to use the V2 of the app

- **modify the ./values.yaml file**
  This actually allows to pull another (V2) container image from the same DockerHub repository
  
  <img width="783" alt="Screenshot 2023-12-27 at 22 28 48" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/36d9bc79-16be-4487-a478-41665eae3f6d">

- **modify the ./Chart.yaml file**
  Make sure to follow semantic versioning principles when updating the version number. Typically, you increment the version number based on the type of changes made:

  - Increment the major version.
  - Increment the minor version for backward-compatible feature additions.
  - Increment the patch version for backward-compatible bug fixes.
    
  <img width="830" alt="Screenshot 2023-12-27 at 22 35 04" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/b3325b7b-a333-4953-92d5-01809270f185">

### 8. HELM `upgrade`

  - upgrade the app version

    ```yml
    helm upgrade coffeeapp coffee_app/
    ```
    <img width="1000" alt="Screenshot 2023-12-27 at 22 41 12" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/7fb9cd4c-6569-472a-8d2b-738c9077faa7">
  
    The `helm upgrade` command is used to update a deployed release to a new version of a Helm chart or to apply changes to the configuration of an existing release. It is a powerful command that allows you to make changes to a running application without requiring a full reinstallation. 

    <img width="1000" alt="Screenshot 2023-12-27 at 22 49 04" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/758c3633-8945-4860-a044-3880298965cd">

    <img width="1000" alt="Screenshot 2023-12-27 at 22 50 05" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/afb61e18-b1a6-49d1-b110-c6f535b64f59">

    Now we have the V2 app version deployed

### 9. HELM `rollback`

- **Rollback to the previous version**

  The `helm rollback` command in Helm is used to roll back a release to a previous version. This can be useful if a recent upgrade to a Helm chart introduces issues, and you want to revert to a known, stable state.

  ```sh
  helm rollback coffeeapp 1
  ```
  <img width="1000" alt="Screenshot 2023-12-27 at 22 57 21" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/a1c769c6-d42b-4612-a14b-cb9a1f5ffcbb">

  <img width="1000" alt="Screenshot 2023-12-27 at 22 23 33" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/0112413c-8c10-49f1-a67f-59f41e6fa35e">

  Now, the app is back to V1

  Thanks for reading up to this point :)
