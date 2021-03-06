![plugin](terraform.png)

Terraform is an Open Source tool that is developed by HashiCorp that build, change, and version infrastructure safely and efficiently. We can use Terraform to automate IBM Cloud resource provisioning, rapidly build complex, multi-tier cloud environments, and enable Infrastructure as Code (IaC).

The Terraform __configuration files__ describe the resources that you need and how you want to configure them. Based on your configuration, Terraform creates an execution __plan__ and describes the actions that need to be executed to get to the required state. You can review the execution plan, change it, or simply execute the plan. When you change your configuration, Terraform can determine what changed and create incremental execution plans that you can apply to your existing IBM Cloud resources


Steps :
1. [Install the Terraform CLI](#step-1-install-the-terraform-cli)
1. [Install IBM Cloud Provider plug-in](#step-2-install-ibm-cloud-provider-plug-in)
1. [Prepare Terraform environment](#Step-3-Prepare-Terraform-environment)
1. [Lets create IBM Kubernetes cluster](#Step-4-Lets-create-IBM-Kubernetes-cluster)
1. [Initialize Terraform](#Step-5-Initialize-Terraform)
1. [Execute Plan](#Step-6-Execute-Plan) 
1. [Now finally apply all the configuration](#Step-7-Now-finally-apply-all-the-configuration)
1. [Access IBM Kubernetes Cluster](#Step-8-Access-IBM-Kubernetes-Cluster)


## Step 1. Install the Terraform CLI 
1. Open the terminal and create a folder __terraform__ and navigate into that folder.

   ``` mkdir terraform && cd terraform ```
1. [Download Terraform v0.12.x](https://www.terraform.io/downloads.html)
1. Extract the Terraform package and copy the binary file into your terraform directory.
1. Point the __$PATH environment__ variable to Terraform binary file.

   ``` export PATH=$PATH:$HOME/terraform ```

1. Run command __terraform__ to verify that the installation is successful
__NOTE:__ Please restart the device if above command is not executed

## Step 2. Install IBM Cloud Provider plug-in
Terraform uses the IBM Cloud Provider plug-in to securely communicate with the IBM Cloud REST API. To provision and work with IBM Cloud resources,we must install it.

1. [Download the latest version of the IBM Cloud Provider binary package](https://github.com/IBM-Cloud/terraform-provider-ibm/releases)
1. Extract the IBM Cloud Provider package to retrieve the binary file.
1. Create a hidden folder for plug-in

   ``` mkdir $HOME/.terraform.d/plugins ```
   
1. Move the IBM Cloud Provider plug-in into your hidden folder.
   
   ``` mv $HOME/Downloads/terraform-provider-ibm* $HOME/.terraform.d/plugins/ ```
 
__NOTE :__ If there is any issues in the above step kindly manually place binary files under plugins like below image.
![plugin](s3.png)

   
## Step 3. Prepare Terraform environment
1. Create [IBM Cloud API key](https://cloud.ibm.com/docs/account?topic=account-userapikey#create_user_key)
1. Then create a Terraform project directory. The directory will hold all Terraform configuration files. (ex terraform_cf)
1. Now in project directory, create a __terraform.tfvars__ file and add __ibmcloud_api_key = "your-api-key"__ that was created in step 1. The terraform.tfvars file is a Terraform variables file that you store on local machine. When you initialize the Terraform CLI, all variables that are defined in this file are automatically loaded into Terraform and then can be referenced  in every Terraform configuration file in the same project directory
1. In the same project directory, create a __provider.tf__ file and configure IBM as Terraform provider
   ```
   variable "ibmcloud_api_key" {}

   provider "ibm" {
   ibmcloud_api_key    = var.ibmcloud_api_key
    }
   ```
## Step 4. Lets create IBM Kubernetes cluster
1. In the same directory where you stored the __terraform.tfvars__ and __provider.tf__ files, create a Terraform configuration file and name it __configure.tf__

   
  ```
  resource "ibm_container_cluster" "testacc_cluster" {
  name            = "test"
  datacenter      = "dal10"
  machine_type    = "free"
  hardware        = "shared"
  public_vlan_id  = "vlan"
  private_vlan_id = "vlan"

  default_pool_size = 1
}
   ```
   
   
   The configuration file includes the following definition blocks:
   
   __name:__ Use this block to retrieve information for an existing resource in your IBM Cloud account.
   
   __datacenter__: The datacenter where we want to provision the worker nodes.
   
   __machine_type__ : The machine type for our worker node. The machine type determines the amount of memory, CPU, and disk
   
   __hardware__ : The level of hardware isolation for  worker node Use "dedicated" to have available physical resources dedicated to you only, or "shared" to allow                   physical resources to be shared with other IBM customers
   
  We can create many resources related to IBM Kubernetes resources. Check from [here](https://cloud.ibm.com/docs/terraform?topic=terraform-container-resources)    
  
  List of all supported configurations for each resource, see [here](https://cloud.ibm.com/docs/terraform?topic=terraform-index-of-terraform-resources-and-data-sources)

## Step 5. Initialize Terraform
1. Must be in Terraform project directory and run command __terraform init__ . This will initialize terraform in the folder and now we can work with all commands to automate our IBM cloud resources.
1. Once above step is executed successfully there will be message "Terraform has been successfully initialized!". If there is any issues please verify above steps

## Step 6. Execute Plan 
1. Run command __terraform plan__
1. When this command is executed, Terraform validates the syntax of configuration file and resource definitions against the specifications that are provided by the IBM Cloud Provider plug-in
![plugin](s44.png)


## Step 7. Now finally apply all the configuration
1. Run command __terraform apply__
1. Confirm the creation by entering __yes__ when prompted
1. This command is used to apply the changes required to reach the desired state of the configuration, or the pre-determined set of actions generated by a execution plan.


## Step 8. Access IBM Kubernetes Cluster
1. Login IBM CLoud Platform
1. On left menu click 3 dots and under __resources list__ we can see our cluster is up and running. 
1. Open cluster dashboard and click __access__ 
1. Run 1-3 commands on terminal to access the cluster
1. If everything is fine we will get the message "The configuration for btou5t4d0p.. was downloaded successfully".
1. Run command __ibmcloud ks worker ls --cluster test__ and you can see the cluster details.

## Conclusion:
In this tutorial, we learn the complete life cycle involve in Terraform (Infrastructure as a code) to manage our IBM Kubernetes Service with simple configuration files that make administrators and operational teams workload less by providing a simple human understandable format offered by HashiCorp.

In the future, if any team plan to extend their resources and want to become a multi-cloud platform rather than understanding all the complex SDKs and API integration they only need to focus and maintain the declarative configuration files offered by the cloud provider.
