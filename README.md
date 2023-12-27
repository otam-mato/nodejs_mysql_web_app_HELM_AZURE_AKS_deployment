# Node.JS + MySQL Web App.<br><br>HELM deployment and upgrading the app on a Kubernetes cluster launched on MS Azure AKS.

<p align="center">
  <a href="https://skillicons.dev">
    <img height=50" width="50" src="https://cdn.jsdelivr.net/npm/simple-icons@v10/icons/helm.svg/blue/" /><img src="https://skillicons.dev/icons?i=azure,kubernetes,docker,nodejs,mysql"/>
  </a>
</p>



<br>

> [!NOTE]
> This is a part of a series of demo projects in which I manipulate a Node.js application using various technologies.<br>
>
> The app built using Node.js and Express, originally presented at this [GitHub Repository](https://github.com/otam-mato/nodejs_mysql_web_app_terraform.git). The previous [project](https://github.com/otam-mato/nodejs_mysql_web_app_kubernetes.git) involved the "Canary" deployment on **AWS EKS**.
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
