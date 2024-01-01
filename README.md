# Azure_ACR

## Virtual Machine vs Containers:

![image](https://github.com/Pavan-1997/Azure_ACR/assets/32020205/6cf66a8f-df15-45c1-b69a-ffa235b08b7a)

## Challenges with the non-containerized applications:

![image](https://github.com/Pavan-1997/Azure_ACR/assets/32020205/c7b3f939-4a95-4811-88b1-9041f0ec3863)

## Docker Architecture: 

![image](https://github.com/Pavan-1997/Azure_ACR/assets/32020205/ed7fa02a-6e75-40ee-aea4-8b21dcfaa161)


1. Go to the dev.azure portal 


2. Click on Create new project -> Give a Project name -> Click on Create


3. Now go to Repos -> Files -> Click on Import -> Clone URL -  -> Click on Import


4. For creating a docker file in the cloned repo - Click on the 3 dots -> New -> File -> Give a name -> Click on Create

Give the below Dockerfile -> Click on Commit


5. Now go to portal.azure -> Open Container Registry -> Create a new Resource group -> Now give a unique name to Registry name -> Pricing plan - Basic -> Click on Review + create


6. Now click on Access keys on the left -> Check on Admin user 


7. Now go to dev.azure portal -> Click on Pipelines -> Pipelines -> Click on Create Pipeline -> Azure Repos Git -> Select the repo -> Click on Docker (Build and push an image to Azure Container Registry) -> Select the Azure subscription -> Click on Continue -> Select the already created Container registry -> Give a Image Name -> Click on Validate and configure -> Modify the azure-pipeline.yml file -> Click on Save and run -> Click on Permit to execute the Build job


8. Now go to portal.azure -> Open Container instances -> Select the created instance -> Click on the Containers on the left -> You can see the container created 


9. Now go to portal.azure -> Open Container instances -> Click on Overview -> Open the FQDN in a browser which should open the application 
 
