# Azure_ACR      
       
## Virtual Machine vs Containers:

![image](https://github.com/Pavan-1997/Azure_ACR/assets/32020205/6cf66a8f-df15-45c1-b69a-ffa235b08b7a)

## Challenges with the non-containerized applications:

![image](https://github.com/Pavan-1997/Azure_ACR/assets/32020205/c7b3f939-4a95-4811-88b1-9041f0ec3863)

## Docker Architecture: 

![image](https://github.com/Pavan-1997/Azure_ACR/assets/32020205/ed7fa02a-6e75-40ee-aea4-8b21dcfaa161)
## Containerize a sample To-Do list web app written in React JS.

**GitHub repo for the App:**

https://github.com/Pavan-1997/Azure_ACR/tree/main/todoapp-docker

**DockerFile**

```
FROM node:18-alpine AS installer
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build
FROM nginx:latest AS deployer
COPY --from=installer /app/build /usr/share/nginx/html
```

## Benefits of a multi-stage docker file

The Dockerfile uses multi-stage builds with two stages ("installer" and "deployer").
The "installer" stage is focused on building the application and installing dependencies.
The final image (in the "deployer" stage) only includes the necessary artifacts from the build, reducing the overall size of the Docker image.

**Efficient Use of Layers:**
Each instruction in a Dockerfile creates a layer in the image.
By organizing the build process into stages, layers containing development dependencies are isolated in the "installer" stage, and these layers are discarded in the final image.
This results in a more efficient use of layers, making image builds and updates faster.

**Security:**
The "installer" stage contains the build tools and dependencies needed for the application but discards them afterward.
The "deployer" stage only includes the built artifacts, reducing the attack surface by excluding unnecessary tools and files.
This separation enhances the security of the final production image.

**Build Isolation and Clarity:**
The Dockerfile separates the build process (in the "installer" stage) from the deployment process (in the "deployer" stage).
This separation improves the clarity of the Dockerfile, making it easier to understand and maintain.

**Faster Builds:**
Since the "deployer" stage only copies the necessary files from the "installer" stage, the build process is more efficient and faster.
This is particularly beneficial in CI/CD pipelines where quick image builds are crucial.

**Optimized for Production:**
The final image is based on the lightweight Nginx image, which is optimized for serving web applications.
It includes only the built artifacts necessary for running the application, making it suitable for production environments.

## What are Azure container instances(ACI)

![image](https://github.com/Pavan-1997/Azure_ACR/assets/32020205/7cdf766a-6ccf-48b1-a313-8d5c803f6fd3)

## Azure DevOps CICD Pipeline to deploy to ACI

**Pipeline Code**:

``` YAML
# Docker
# Build and push an image to Azure Container Registry
# https://docs.microsoft.com/azure/devops/pipelines/languages/docker

trigger:
- main

resources:
- repo: self

variables:
  # Container registry service connection established during pipeline creation
  dockerRegistryServiceConnection: 'e7cd520d-3339-42b0-ab2f-f7d11582b7da'
  imageRepository: 'todoapp'
  containerRegistry: 'pavanraj97.azurecr.io'
  dockerfilePath: '$(Build.SourcesDirectory)/Dockerfile'
  tag: '$(Build.BuildId)'

  # Agent VM image name
  vmImageName: 'ubuntu-latest'

stages:
- stage: Build
  displayName: Build and push stage
  jobs:
  - job: Build
    displayName: Build
    pool:
      vmImage: $(vmImageName)
    steps:
    - task: AzureCLI@2
      inputs:
        azureSubscription: 'Azure subscription(c20379ae-4a34-42d6-bef7-55273c6630f6)'
        scriptType: 'bash'
        scriptLocation: 'inlineScript'
        inlineScript: 'az acr login --name=$(containerRegistry)'
    
    - task: Docker@2
      displayName: Build and push an image to container registry
      inputs:
        command: buildAndPush
        repository: $(imageRepository)
        dockerfile: $(dockerfilePath)
        containerRegistry: $(dockerRegistryServiceConnection)
        tags: |
          $(tag)

    - task: AzureCLI@2
      inputs:
        azureSubscription: 'Azure subscription(c20379ae-4a34-42d6-bef7-55273c6630f6)'
        scriptType: 'bash'
        scriptLocation: 'inlineScript'
        inlineScript: |
          az container create \
                    --name day10app \
                    --resource-group day10-demo \
                    --image $(containerRegistry)/$(imageRepository):$(tag) \
                    --registry-login-server $(containerRegistry) \
                    --registry-username pavanraj97  \
                    --registry-password T11tVbN4CzH2V1GqOEwiy+TIq9aKq6whz89aZ10ZVj+ACRBbj4Vj \
                    --dns-name-label aci-demo-pavan97
```
---
## Implementation:

1. Go to the dev.azure portal 


2. Click on Create new project -> Give a Project name -> Click on Create


3. Now go to Repos -> Files -> Click on Import -> Clone URL - https://github.com/Pavan-1997/Azure_ACR.git -> Click on Import


4. For creating a docker file in the cloned repo - Click on the 3 dots -> New -> File -> Give a name -> Click on Create

Give the below Dockerfile -> Click on Commit


5. Now go to portal.azure -> Open Container Registry -> Create a new Resource group -> Now give a unique name to Registry name -> Pricing plan - Basic -> Click on Review + create

![image](https://github.com/Pavan-1997/Azure_ACR/assets/32020205/886d0e59-090d-4d03-866f-2763b2d6773a)


6. Now click on Access keys on the left -> Check on Admin user 


7. Now go to dev.azure portal -> Click on Pipelines -> Pipelines -> Click on Create Pipeline -> Azure Repos Git -> Select the repo -> Click on Docker (Build and push an image to Azure Container Registry) -> Select the Azure subscription -> Click on Continue -> Select the already created Container registry -> Give a Image Name -> Click on Validate and configure -> Modify the azure-pipeline.yml file -> Click on Save and run -> Click on Permit to execute the Build job

![image](https://github.com/Pavan-1997/Azure_ACR/assets/32020205/1935bb1c-7618-4d8b-962a-b31dc71366b3)


8. Now go to portal.azure -> Open Container instances -> Select the created instance -> Click on the Containers on the left -> You can see the container created 

![image](https://github.com/Pavan-1997/Azure_ACR/assets/32020205/2dee7fab-15e5-4802-8866-f0e329fae687)


9. Now go to portal.azure -> Open Container instances -> Click on Overview -> Open the FQDN in a browser which should open the application 

![image](https://github.com/Pavan-1997/Azure_ACR/assets/32020205/b9fb2360-7050-4d87-9b3a-bd72f89bc9fd)

![APP](https://github.com/Pavan-1997/Azure_ACR/assets/32020205/cb6a64bc-882a-49c8-8062-daf717ab5b20)

![APP1](https://github.com/Pavan-1997/Azure_ACR/assets/32020205/81a4a8bd-3eb1-4f1a-a87d-c218663c43ef)
