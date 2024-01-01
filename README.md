# Azure_ACR

1. Go to the dev.azure portal 


2. Click on Create new project -> Give a Project name -> Click on Create


3. Now go to Repos -> Files -> Click on Import -> Clone URL - https://github.com/piyushsachdeva/todoapp-docker.git -> Click on Import


4. For creating a docker file in the cloned repo - Click on the 3 dots -> New -> File -> Give a name -> Click on Create

Give the below Dockerfile -> Click on Commit


5. Now go to portal.azure -> Open Container Registry -> Create a new Resource group -> Now give a unique name to Registry name -> Pricing plan - Basic -> Click on Review + create


6. Now click on Access keys on the left -> Check on Admin user 


7. Now go to dev.azure portal -> Click on Pipelines -> Pipelines -> Click on Create Pipeline -> Azure Repos Git -> Select the repo -> Click on Docker (Build and push an image to Azure Container Registry) -> Select the Azure subscription -> Click on Continue -> Select the already created Container registry -> Give a Image Name -> Click on Validate and configure -> Modify the azure-pipeline.yml file -> Click on Save and run -> Click on Permit to execute the Build job


8. Now go to portal.azure -> Open Container instances -> Select the created instance -> Click on the Containers on the left -> You can see the container created 


9. Now go to portal.azure -> Open Container instances -> Click on Overview -> Open the FQDN in a browser which should open the application 
 
