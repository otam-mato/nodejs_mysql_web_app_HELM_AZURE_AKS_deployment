[under revision]

# Node.JS + MySQL Web App.<br><br>HELM deployment and upgrading the app on a Kubernetes cluster launched on MS Azure AKS.

<p align="center">
  <a href="https://skillicons.dev">
    <img height="50" width="50" src="https://cdn.simpleicons.org/helm/blue" /><img src="https://skillicons.dev/icons?i=kubernetes,azure,docker,nodejs,mysql"/>
  </a>
</p>



<br>

> [!NOTE]
> This is a part of a series of demo projects in which I manipulate a Node.js application using various technologies.<br>
>
> The app built using Node.js and Express and integrates with MySQL database, originally presented at this [GitHub Repository](https://github.com/otam-mato/nodejs_mysql_web_app_terraform.git). The previous [project](https://github.com/otam-mato/nodejs_mysql_web_app_kubernetes.git) involved the "Canary" deployment on **AWS EKS**.
>
> In the current installment, we're deploying the web application on **MS Azure AKS** using **HELM**. Subsequently, we'll introduce the second version of the app and then rollback to the first version with HELM.
<br>

## Deployment Strategy

The **HELM** is a package manager for Kubernetes applications. It simplifies the deployment and management of applications on Kubernetes clusters. Helm uses charts, which are packages of pre-configured Kubernetes resources, to define, install, and upgrade even the most complex Kubernetes applications and their dependencies. It helps streamline the deployment process, reduce errors, and enable versioning and rollbacks for Kubernetes applications.

1. In this demo, we will use **HELM** to deploy:
   - V1 deployment which will host the version 1 of the app.
   - V2 deployment which will host the version 2 of the app.
   - Finally we will rollback V2 to V1.

   V1 and V2 versions uploaded into this [DockerHub repository](https://hub.docker.com/repository/docker/montcarotte/fullstack_nodejs_mysql_demo/general)

2. The app and its database are placed in separate Kubernetes pods to secure decoupling, resource isolation, scaling and resilience.

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

1. [**Create an AKS cluster**](https://github.com/otam-mato/nodejs_mysql_web_app_kubernetes/blob/main/README.md#prerequisites)
2. [**Create an Azure VM**]()
3. [**Install Azure CLI**]()
4. [**Connect the Azure VM to AKS cluster**]()
5. [**Install HELM**]()
6. [**Create a HELM chart**]()
7. [**Deploy the V1 app**](https://github.com/otam-mato/nodejs_mysql_web_app_kubernetes/blob/main/README.md#1-create-the-deployment-of-v1-of-the-app)
8. [**Upgrade the app to V2**](https://github.com/otam-mato/nodejs_mysql_web_app_kubernetes/blob/main/README.md#4-create-the-deployment-of-v2-of-the-app)
9. [**Roll the V2 back to V1**](https://github.com/otam-mato/nodejs_mysql_web_app_kubernetes/blob/main/README.md#7-test-the-app)

<br>

<br>

## Architecture Diagram

<p align="center">
  <img src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/51ee6462-a454-45bc-ad73-5d429b36dbe0" width="1200px"/>
</p>

<br>

## Screenshots

<p align="center">
  <img src="https://github.com/otam-mato/nodejs_mysql_web_app_kubernetes/assets/113034133/7edead55-90b5-4e4e-9a45-eb9c2497547b" width="700px"/>
</p>

<p align="center">
  <img src="https://github.com/otam-mato/nodejs_mysql_web_app_kubernetes/assets/113034133/a1a5042a-d944-4887-9a34-65e3a0959e3a" width="700px"/>

</p>


<p align="center">
  <img src="https://github.com/otam-mato/nodejs_mysql_web_app_kubernetes/assets/113034133/b0f5ee8a-ed8f-4727-8cc0-e3f64779c401" width="700px"/>
</p>

<p align="center">
  <img src="https://github.com/otam-mato/nodejs_mysql_web_app_kubernetes/assets/113034133/2121055f-2a92-4d42-bae1-b38de16110c4" width="700px"/>
</p>

<br>

## Prerequisites

- **MS Azure** account.

- <details markdown=1><summary markdown="span">Create an Azure VM</summary>
  <br>
  zzz
   </details>
- <details markdown=1><summary markdown="span">Create an AKS cluster</summary>
  <br>
  zzz
   </details>
- <details markdown=1><summary markdown="span">Connect Azure VM to AKS cluster</summary>
  <br>
  zzz
   </details>
- <details markdown=1><summary markdown="span">Install HELM</summary>
  <br>
  zzz
   </details>

<br>

## Steps:

### 1. Create the HELM chart

- **create the structure**

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

- **clone the repository**

  ```sh
  git clone https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment.git
  ```

- **place the cloned manifests into the HELM project folder "coffee_app"**

  <img width="893" alt="Screenshot 2023-12-27 at 21 19 59" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/79b972eb-afae-4c74-b9ac-66bab54b7f53">

- **create the projects chart in Chart.yml**
  
  <img width="893" alt="Screenshot 2023-12-27 at 21 31 38" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/b00e45c6-a155-4104-89f8-1274710c7699">

  The `Chart.yaml` file is a metadata file that provides essential information about a Helm chart. It is a required file in every Helm chart, and it contains details such as the chart's name, version, description, and other metadata. The information in the Chart.yaml file helps users and Helm itself understand the characteristics of the chart.

- **create the ./values.yaml**

  <img width="893" alt="Screenshot 2023-12-27 at 21 37 53" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/b2bef742-b70c-4021-ad71-81d8a5b579a3">

  In Helm, the `values.yaml` file is a configuration file used to define default values for Helm charts. It allows users to specify configuration parameters and their default values, which can then be overridden or customized when deploying the Helm chart.

- **the project's final sructure:**

  <img width="931" alt="Screenshot 2023-12-27 at 21 44 29" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/a960ab8e-0a5c-4537-9964-6b8b63a8059e">

### 4. HELM lint

- HELM's "lint" command  is used in Helm to perform a static analysis of a chart. It helps identify issues and errors in a Helm chart before it is deployed to a Kubernetes cluster. The lint command checks the chart's syntax, structure, and best practices, providing feedback on potential problems.

  ```sh
  helm lint ./coffee_app/
  ```
  
  <img width="931" alt="Screenshot 2023-12-27 at 21 47 51" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/9ad1a0a6-60bf-4d40-813d-fbf0eed06ad3">

  Running `helm lint` is a good practice before deploying a Helm chart to catch potential issues early in the development and deployment process. It helps ensure that the chart is well-formed, follows best practices, and is less likely to cause problems when applied to a Kubernetes cluster.

### 5. HELM template

- The `helm template` command is used to render a Helm chart locally without installing it into a Kubernetes cluster. This command is useful for generating the Kubernetes YAML manifests based on a Helm chart, allowing you to inspect the resulting output before deploying it to a cluster. It's often employed during development, testing, or as part of a continuous integration (CI) pipeline.

  ```yml
  helm template coffee_app/
  ```
  
  <img width="863" alt="Screenshot 2023-12-27 at 21 52 58" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/4bb46d6c-e981-4c4a-b938-9add7e5fb279">

### 6. HELM install

- install the chart

  ```yml
  helm install coffeeapp coffee_app/
  ```

  <img width="888" alt="Screenshot 2023-12-27 at 22 15 17" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/5767b3d5-c27e-42f9-9bd5-b6d8e82f0a98">

  The `helm install` command is used to deploy a Helm chart onto a Kubernetes cluster. It takes a chart as input, installs it into the cluster, and creates a release, which is an instance of that chart running on the cluster.
  
  <img width="888" alt="Screenshot 2023-12-27 at 22 21 38" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/b4be7aa6-849f-4258-9f40-82f286868d2b">
  <img width="888" alt="Screenshot 2023-12-27 at 22 23 33" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/1a3301c9-e1a0-4922-b822-ee4affc45f4e">

  Now, the version V1 of the app is deployed on the AKS cluster 

### 7. Modify the ./values.yaml to use V2

- **modify the ./values.yaml file**
  This actually allows to pull another (V2) container image from the same DockerHub repository
  
  <img width="783" alt="Screenshot 2023-12-27 at 22 28 48" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/36d9bc79-16be-4487-a478-41665eae3f6d">

- **modify the ./Chart.yaml file**
  Make sure to follow semantic versioning principles when updating the version number. Typically, you increment the version number based on the type of changes made:

  - Increment the major version.
  - Increment the minor version for backward-compatible feature additions.
  - Increment the patch version for backward-compatible bug fixes.
    
  <img width="830" alt="Screenshot 2023-12-27 at 22 35 04" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/b3325b7b-a333-4953-92d5-01809270f185">

### 8. HELM upgrade

  - upgrade the app version

  ```yml
  helm upgrade coffeeapp coffee_app/
  ```
  <img width="1000" alt="Screenshot 2023-12-27 at 22 41 12" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/7fb9cd4c-6569-472a-8d2b-738c9077faa7">

  The `helm upgrade` command is used to update a deployed release to a new version of a Helm chart or to apply changes to the configuration of an existing release. It is a powerful command that allows you to make changes to a running application without requiring a full reinstallation. 

- **[Download the manifest 'deployment_app_v1.yml'](https://github.com/otam-mato/nodejs_mysql_web_app_kubernetes/blob/66874767022185dcf7c7eae0c8bc2967ec60dcea/deployment_app_v1.yml)**





### 9. HELM rollback

- **[Download the manifest 'deployment_app_v1.yml'](https://github.com/otam-mato/nodejs_mysql_web_app_kubernetes/blob/66874767022185dcf7c7eae0c8bc2967ec60dcea/deployment_app_v1.yml)**

- **Apply it**:

```yml
kubectl apply -f deployment_app_v1.yaml
```

<img width="893" alt="Screenshot 2023-12-27 at 21 31 38" src="https://github.com/otam-mato/nodejs_mysql_web_app_HELM_AZURE_AKS_deployment/assets/113034133/b00e45c6-a155-4104-89f8-1274710c7699">


<p align="center">
   <img width="800" alt="Screenshot 2023-10-05 at 19 33 42" src="https://github.com/otam-mato/nodejs_mysql_web_app_kubernetes/assets/113034133/cfadecee-4586-48e9-ba90-c82c8be08658"> 
</p>
