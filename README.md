# udacity_project3

Yes, you are correct. To begin with, you need to add the terraform init, apply steps first in the YAML file and try to execute that via azure pipelines. Once infrastructure is successfully deployed, you can add the next tasks like deploying web app, run postman and Jmeter tests etc.

You can try using the below code to configure packages/libraries required for running selenium tests-
sudo apt-get update
sudo apt-get upgrade -y
sudo apt-get install python3-pip unzip expect -y
sudo apt-get install -y chromium-browser
pip3 install selenium
sudo rm -rf chromedriver*
wget https://chromedriver.storage.googleapis.com/<version>/chromedriver_linux64.zip //update the version here
unzip chromedriver*.zip
sudo mv chromedriver -f /usr/bin
  
  
  Once you have installed newman, try to run the postman using the below command -

- script: |
        sudo npm install -g newman
        sudo npm install -g newman-reporter-junitfull
      displayName: 'Install Postman Dependencies'
- script:
        newman run "$(System.DefaultWorkingDirectory)/automatedtesting/postman/postman_data_validation_test.postman_collection.json" -e "$(System.DefaultWorkingDirectory)/automatedtesting/postman/udacity_postman_env.postman_environment.json" -r cli,junitfull --reporter-junitfull-export result-data-validation-test.xml
      displayName: 'Run Postman Data Validation Tests'
Make sure you add a similar script for regression test also.
  

  App Service environement will not be created automatically by pipeline.

A user has to create the appservice environment before creating and deploying the webapp.

Based on the syntax available in the Azure portal, az webapp up still requires the app service name to create and deploy the code.

az webapp up [--app-service-environment]
By definition:

az webapp up
Create a webapp and deploy code from a local workspace to the app.

The command is required to run from the folder where the code is present.

Since the app service was missed in the CLI command execution, we were prompted with the error saying runtime stack was missing.

You can follow the steps via CLI to deploy the webapp.

create app service
create and deploy the web app
Having an App Service is mandatory to create and deploy the web app.

While the links above are indeed the correct and generic solution for Azure I would also like to provide more details that are more related to our project.
  
  
  
  Yes, can read environment variables in Terraform. There is a very specific way that this has to be done. You will need to make the environment variable a variable in terraform.

For example I want to pass in a super_secret_variable to terraform. I will need to create a variable for it in my terraform file.

variable "super_secret_variable" {
     type = "string 
}
Then based on convention I will have to prefix my environment variable with TF_VAR_ like this:

TF_VAR_super_secret_variable
Then terraform will automatically detect it and use it. Terraform processors variables based on a specific order that order is -var option, -var-file option, environment variable, then default values if defined in your tf file.

Alternative you can pass environment variables in through the CLI to set variables in terraform like so.

> terraform apply -var super_secret_variable=$super_secret_variable
This doesn't require that you prefix it so if they are something you can't change that may be your best course of action.
  
  
  

https://azuredevopslabs.com/labs/vstsextend/terraform/
https://julie.io/writing/terraform-on-azure-pipelines-best-practices/
https://faun.pub/azure-devops-deploying-azure-resources-using-terraform-1f2fe46c6aa0
https://purple.telstra.com/blog/terraform-with-azure-devops-pipeline
https://mthai.medium.com/how-to-run-terraform-tasks-in-azure-devops-273935089536

https://github.com/Azure-Samples/azure-search-performance-testing
https://colinsalmcorner.com/executing-jmeter-tests-in-an-azure-pipeline/
https://docs.microsoft.com/en-us/samples/azure-samples/jmeter-aci-terraform/jmeter-aci-terraform/
https://azuredevopslabs.com/labs/vstsextend/selenium/


https://dotnetthoughts.net/how-to-configure-postman-api-tests-in-azure-devops/
http://blog.geveo.com/Run-Postman-API-tests-in-Azure-DevOps-Pipelines
https://medium.com/younited-tech-blog/integrate-automated-test-in-azure-devops-using-the-postman-api-288f5566bf11


https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/sqlcheatsheet#sql-to-kusto-cheat-sheet

https://medium.com/swlh/fake-rest-apis-that-we-can-use-to-build-prototypes-2a7946704726

