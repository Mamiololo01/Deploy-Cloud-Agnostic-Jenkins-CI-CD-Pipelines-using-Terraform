# Deploy-Cloud-Agnostic-Jenkins-CI-CD-Pipelines-using-Terraform
Deploy Cloud-Agnostic Jenkins CI/CD Pipelines using Terraform on AWS infrastructure

Use-Case
Your team would like to start using Jenkins as their CI/CD tool to create pipelines for DevOps projects. They need you to create the Jenkins server using Terraform so that it can be used in other environments and so that changes to the environment are better tracked.
In this tutorial, I am going to walk you thru how you can leverage a cloud-agnostic tool like terraform to automate the provision and deployment of a Jenkins CI/CD pipeline.
Note: While I will be using AWS’ Cloud9 IDE and deploying to AWS for this tutorial, you can follow along on your own IDE and with a few simple tweaks to your main.tf file, you can deploy to dozens of cloud providers
Let’s begin:
Pre-Requisites
* 		Basic knowledge of Terraform commands
* 		Terraform installed within your CLI
* 		AWS CLI configured in your IDE to allow Terraform access to resources via your account

Step 1 — Initializing Terraform
Login to your CLI and create a new directory for your project and initialize a new Terraform configuration:
mkdir jenkins_terraform
cd jenkins_terraform

Step 2 — Creating our Terraform files
I am going to start by adding a variables.tf file to define the variables that I plan to use in my configuration. 
It’s perfectly normal to edit this later after you create your main.tf once you know the full extent of your variables.

Next, I will create a main.tf file and define the resources I will use. Each section includes comments that describes in detail what the HCL language is doing:


I need to create a providers.tf file to define which cloud-based service I will be deploying into:

Step 3 — Deploying our AWS Resources
Having created my three terraform files, I am ready to move forward with deployment.
Every terraform deployment starts with running the command ‘terraform init’- This command initializes the working directory containing Terraform configuration files and downloads any necessary provider modules.

Next, I run the ‘terraform plan’ command — This shows what changes will be applied to infrastructure. This step is important because it allows you to review and confirm the changes before applying them.

Next, I run the ‘terraform apply’ command, which will deploy our infrastructure. If this plan was modifying an existing deployment, terraform would automatically tear-down or redeploy new resources based on the changes in my main.tf from the last deployment.

<img width="816" alt="Screenshot 2023-03-30 at 18 17 15" src="https://user-images.githubusercontent.com/67044030/228933669-40a20837-aa49-4e00-9511-abe12dbb7db2.png">

Step 4 — Verifying our Jenkins Deployment and S3 Bucket Permissions

<img width="902" alt="Screenshot 2023-03-30 at 18 14 43" src="https://user-images.githubusercontent.com/67044030/228932324-6bfe31e6-e2c9-4652-91c8-adc607090749.png">

Once your Terraform configuration has been applied and your EC2 instance is running, you can access Jenkins by navigating to the public IP address of your instance followed by “:8080” in your web browser:
http://3.137.217.213:8080

<img width="1240" alt="Screenshot 2023-03-30 at 18 13 08" src="https://user-images.githubusercontent.com/67044030/228931921-ed804e74-2828-41ff-82b5-a56462819f77.png">

I can also that my Jenkins instance has S3 read & write permissions via the CLI by testing some read and write commands by logging in to my instance and running some commands using the AWS CLI.
First, I will configure the AWS CLI on your instance. You will also need to input your AWS Access Key ID, Secret Access Key, Default region, and output format
aws configure
Next, I am going to create a sample file “Hello World” on my local Jenkins instance and move it to my S3 bucket to test the I configured the permissions on my EC2 instance correctly:
echo "Hello World" > test.txt
aws s3 cp test.txt s3://jenkins-artifacts-f8ce49d3

Hello World file was successfully created on EC2 and then moved to S3 bucket
Next, I will test the read permissions of my EC2 instance by listing the contents of my S3 bucket:
aws s3 ls s3://jenkins-artifacts-f8ce49d3

Last, I will visit my S3 Bucket to also verify that to ensure that the file we copied is visible in my bucket:

<img width="755" alt="Screenshot 2023-03-30 at 18 21 01" src="https://user-images.githubusercontent.com/67044030/228933039-b1c4fd5f-2947-4093-8b28-64bfa6ed0cb4.png">

Thanks for following along! Remember to tear-down your architecture when you are done to avoid incurring unnecessary costs! In terraform, we do this by using the ‘terraform destroy’ command:
terraform destroy

<img width="746" alt="Screenshot 2023-03-30 at 18 24 57" src="https://user-images.githubusercontent.com/67044030/228933431-a6669136-80bc-414d-bcdb-fb47180db166.png">

