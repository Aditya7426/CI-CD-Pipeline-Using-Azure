# Implementing-CI-CD-Using-Azure-Pipelines
Implementing CI/CD Using Azure Pipelines, published by Packt

Step 1: Create a Project in Azure DevOps
Go to Azure DevOps.
Click on New Project.
Project Name: Enter a name for your project (e.g., "MyAppPipeline").
Choose Visibility (Private/Public).
Click Create.
Step 2: Set Up a GitHub Repository
Create a new GitHub repository with your application code (e.g., a basic web app like Flask, FastAPI, or Node.js).
Clone it locally and push your code to the repository.
Make sure your code has a README.md, and depending on the application type, a basic project structure (like package.json or requirements.txt).
Step 3: Create a Service Connection to Azure
In your Azure DevOps project, go to Project Settings (bottom left).
Under Pipelines, click on Service Connections.
Click New Service Connection > Azure Resource Manager.
Select Service principal (automatic).
Sign in to your Azure account and allow access.
Select your Azure Subscription and click Save.
Step 4: Create a Build Pipeline (CI - Continuous Integration)
In Azure DevOps, go to Pipelines > Create Pipeline.
Select GitHub as the source.
Authenticate and select your GitHub repository.
Choose a pipeline configuration:
If your project is a Node.js or Python app, select the matching template (Node.js with npm, Python package, etc.).
For a custom project, you can start with the default YAML template.
Example of a basic Node.js pipeline configuration (azure-pipelines.yml):

yaml
Copy code
trigger:
- main

pool:
  vmImage: 'ubuntu-latest'

steps:
- task: NodeTool@0
  inputs:
    versionSpec: '14.x'
  displayName: 'Install Node.js'

- script: |
    npm install
    npm run build
  displayName: 'Install and build'

- task: PublishBuildArtifacts@1
  inputs:
    pathToPublish: '$(Build.ArtifactStagingDirectory)'
    artifactName: 'drop'
Save and run the pipeline to see the build process in action.
The code will be checked out from GitHub, dependencies will be installed, and the app will be built.
Step 5: Create a Release Pipeline (CD - Continuous Deployment)
After the build completes, go to Pipelines > Releases > New Pipeline.
Click Add an Artifact and select the build pipeline you created earlier as the source.
Define Stages for your pipeline:
Click Stage 1 (e.g., "Dev Environment").
Choose the deployment type as Azure App Service Deployment.
Choose the Azure service connection you created earlier.
Select your App Service (create one if necessary from the Azure portal).
Set up Continuous Deployment by enabling the trigger for the build artifact. This means whenever a build is successful, the release will automatically trigger.
Step 6: Configure Deployment (Azure App Service)
In the Deploy Azure App Service stage, configure:

Azure Subscription: Select your subscription.
App Service Name: Choose the name of the app where you want to deploy.
Package or Folder: Select $(System.DefaultWorkingDirectory)/drop/.
Save and create the release pipeline.

Step 7: Test the Pipeline
Make a small change in your GitHub repository (e.g., update README.md) and push the changes.
This should trigger the build pipeline (CI).
Once the build is successful, it will automatically trigger the release pipeline (CD), and the application will be deployed to your Azure App Service.
Step 8: Monitor and View Logs
After the pipeline runs, go to the Pipelines section in Azure DevOps.
You can monitor the logs for both the Build and Release pipelines.
Check your application deployed on Azure App Service by visiting the app URL provided in the Azure portal.
