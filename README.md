<a name="readme-top"></a>






<!-- PROJECT LOGO -->
<br />
<div align="center">
  <a href="https://github.com/ahmedalaa14/complete_automated_infrastructure.git">
    <img src="images/logo.png" alt="Logo" width="80" height="80">
  </a>

<h3 align="center">infrastructure on GKE using Terraform & Jenkins</h3>

  <p align="center">
    deploy a Node-JS Application connected with high available MongoDB cluster on GKE using Terraform and Jenkins
    <br />
    <br />
    <br />
    
  </p>
</div>



<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        </ul>
    <li><a href="#installation">Installation</a>
        <ul>
            <li><a href="#setup-infrastructure">Setup Infrastructure</a></li>
            <li><a href="#deploy-application">Deploy Application</a></li>
          </ul>          
    

  </ol>
</details>



<!-- ABOUT THE PROJECT -->
## About The Project



This project is a simple NodeJS application connected to a MongoDB cluster on GKE using Terraform and Jenkins for CI/CD, it can grantee and with 10 mins you can build an infrastructure configured, up and running.

<p align="right">(<a href="#readme-top">back to top</a>)</p>



### Built With

* ![Docker][Docker]
* ![Kubernetes][Kubernetes]
* ![GoogleCloud][GoogleCloud]
* ![Terraform][Terraform]
* ![NodeJS][NodeJS]
* ![MongoDB][MongoDB]
* ![Jenkins][Jenkins]


<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- GETTING STARTED -->
## Getting Started

This is detailed steps to deploy the application on GKE using Terraform, Kubernetes and Jenkins, including tricks in the middle.

### Prerequisites

* Terraform
* GCP account
* GCP project


## Installation

### Setup Infrastructure:

1. install Jenkins on your local machine or on a VM
   ```sh
   sudo apt update
   sudo apt install openjdk-11-jdk
   wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
   sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
   sudo apt update
   sudo apt install jenkins
   ```
   you can access Jenkins using the following URL
   ```sh
   http://<Jenkins_IP>:8080
   ```
   you can get the Jenkins IP using the following command
   ```sh
   sudo systemctl status jenkins
   ```

2. install Terraform on your local machine or on a VM
   ```sh
   sudo apt-get update && sudo apt-get install -y gnupg software-properties-common curl
   curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
   sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
   sudo apt-get update && sudo apt-get install terraform
   ```
3. Create a service account for Terraform
   ```sh
   gcloud iam service-accounts create terraform --display-name "Terraform admin account"
   ```

   -- Create and assign role to Service Account

   ```sh
   gcloud iam service-accounts keys create key.json --iam-account terraform@<Project_ID>.iam.gserviceaccount.com
   ```
   ```sh
   export GOOGLE_APPLICATION_CREDENTIALS=key.json
   ```
   ```sh
   gcloud projects add-iam-policy-binding <Project_ID> --member serviceAccount:terraform@<Project_ID>.iam.gserviceaccount.com --role roles/owner
   ```

   -- Enable necessary APIs for Terraform


   ```sh
   gcloud services enable cloudresourcemanager.googleapis.com
   ```
   ```sh
   gcloud services enable cloudbilling.googleapis.com
   ```
   ```sh
   gcloud services enable iam.googleapis.com
   ```
   ```sh
   gcloud services enable compute.googleapis.com
   ```
   ```sh
   gcloud services enable container.googleapis.com
   ```
   ```sh
   gcloud services enable servicenetworking.googleapis.com
   ```
      



Now all the infrastructure is ready, let's deploy our application on it.

<br>

### Deploy Application:

1. Install Google Cloud SDK (if not installed) -- on your machine terminal 
    ```sh
    sudo apt-get install apt-transport-https ca-certificates gnupg
    ```
    ```sh
    export CLOUD_SDK_REPO="cloud-sdk-$(lsb_release -c -s)"
    ```
    ```sh
    echo "deb https://packages.cloud.google.com/apt $CLOUD_SDK_REPO main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
    ```
    ```sh
    curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
    ```
    ```sh
    sudo apt-get update && sudo apt-get install google-cloud-sdk
    ```
    ```sh
    gcloud init
    ```


2. Install Docker on your machine
   ```sh
   sudo apt-get update
   sudo apt-get install ca-certificates curl gnupg
   sudo install -m 0755 -d /etc/apt/keyrings
   curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
   sudo chmod a+r /etc/apt/keyrings/docker.gpg
   ```

    ```sh
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
    ```

3. Install kubectl on your machine
   ```sh
   sudo apt-get install kubectl
   ```


4. Upload you service account key to Jenkins
<br>

   From Manage Jenkins > Manage Credentials > Global Credentials > Add Credentials 
   Choose secret file and upload the key.json file

   // Note: in my case I called it "gcp-secret"


5. Create a new Jenkins pipeline job
<br>

   Create 2 new pipeline jobs, one for the infrastructure and the other for the containerized application

   the second job will be triggered after the first one is finished

    // Note: the second pipe line called "configure_infrastructure" and it will be triggered after the first one is finished.

### Seems like we are done 🎉
<br>

[Kubernetes]: https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white
[GitHub]: https://img.shields.io/badge/github-%23121011.svg?style=for-the-badge&logo=github&logoColor=white
[Docker]: https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white
[NodeJS]: https://img.shields.io/badge/node.js-6DA55F?style=for-the-badge&logo=node.js&logoColor=white
[MongoDB]: https://img.shields.io/badge/MongoDB-%234ea94b.svg?style=for-the-badge&logo=mongodb&logoColor=white
[GoogleCloud]: https://img.shields.io/badge/GoogleCloud-%234285F4.svg?style=for-the-badge&logo=google-cloud&logoColor=white
[Terraform]: https://img.shields.io/badge/terraform-%235835CC.svg?style=for-the-badge&logo=terraform&logoColor=white
[Jenkins]: https://img.shields.io/badge/Jenkins-%23D24939.svg?style=for-the-badge&logo=Jenkins&logoColor=white