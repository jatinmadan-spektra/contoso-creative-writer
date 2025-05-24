# Exercise 1: Deploying Resources with Azure Developer CLI

### Estimated Duration: 1 Hour

## Lab Scenario

In this exercise, you will deploy the necessary resources for your FastAPI application using the Azure Developer CLI (azd). You will set up your environment, initialize and configure the deployment process, and use azd commands to provision and deploy services in Azure. 

## Lab Objectives

By the end of this exercise, you will be able to:

- Understand the Azure Developer CLI and its deployment workflow.
- Streamline Azure resource deployment using the Azure Developer CLI (`azd`).

### Task 1: Understanding Azure Developer CLI and the Deployment Workflow

In this task, you will gain an understanding of the Azure Developer CLI (azd) and how it facilitates the deployment of cloud resources. You'll explore the main bicep file, the core infrastructure template for your application, and learn about the primary resources it defines, including key parameters, dependencies, and outputs. By the end, you'll be familiar with how azd uses this file to automate provisioning, setting the foundation for deploying resources with ease in the next steps.

> **Lab Tip:** **AZD (Azure Developer CLI)** is a command-line tool designed to streamline the process of building, deploying, and managing Azure applications. It simplifies the interaction with Azure resources by allowing developers to define infrastructure using code, deploy applications, and manage environments in a more efficient and automated way.

1. From the desktop, open **Visual Studio Code**.

   ![](../media/ex1img0.png)

1. In **Visual Studio Code**, go to the **File** menu at the top and select **Open Folder...**

   ![](../media/ex1img6.png)

1. Navigate to `C:\creative-writer\contoso-creative-writer-code-files-main` directory, click on **Select folder**.

   ![](../media/ex1newimg1.png)

1. Once you open the folder in Visual Studio Code, a pop-up will appear asking, *"Do you trust the authors of the files in this folder?"* Click **Yes, I trust the authors**.

   ![](../media/ex1img7.png)

1. Once you have the **contoso-creative-writer-code-files-main** directory opened, ensure you have the source code files from the **Explorer pane**.

   ![](../media/L1-S5.png)
   
1. From the explorer menu, navigate to the `/infra/main.bicep` file to review. A Bicep file is a simplified, readable syntax for defining and deploying Azure resources, which is compiled into ARM templates for deployment.

   ![](../media/ex1newimg3.png)
   
1. In the `main.bicep`, navigate to line number 85, variable **resourcetoken** and update the value with **<inject key="DeploymentID" enableCopy="true"/>**. Please note that if you missed updating this, the next step will fail to deploy the resources

    ![](../media/L1-S7.png)

1. In the `main.bicep`, navigate to the **ai** module definition.
 
   ```bicep
    module ai 'core/host/ai-environment.bicep' = {
    name: 'ai'
    scope: resourceGroup
    params: {
        location: location
        tags: tags
        hubName: !empty(aiHubName) ? aiHubName : 'ai-hub-${resourceToken}'
        projectName: !empty(aiProjectName) ? aiProjectName : 'ai-project-${resourceToken}'
        keyVaultName: !empty(keyVaultName) ? keyVaultName : '${abbrs.keyVaultVaults}${resourceToken}'
        storageAccountName: !empty(storageAccountName)
        ? storageAccountName
        : '${abbrs.storageStorageAccounts}${resourceToken}'
        openAiName: !empty(openAiName) ? openAiName : 'aoai-${resourceToken}'
        openAiConnectionName: !empty(openAiConnectionName) ? openAiConnectionName : 'aoai-connection'
        openAiContentSafetyConnectionName: !empty(openAiContentSafetyConnectionName) ? openAiContentSafetyConnectionName : 'aoai-content-safety-connection'
        openAiModelDeployments: array(contains(aiConfig, 'deployments') ? aiConfig.deployments : [])
        logAnalyticsName: !useApplicationInsights
        ? ''
        : !empty(logAnalyticsWorkspaceName)
            ? logAnalyticsWorkspaceName
            : '${abbrs.operationalInsightsWorkspaces}${resourceToken}'
        applicationInsightsName: !useApplicationInsights
        ? ''
        : !empty(applicationInsightsName) ? applicationInsightsName : '${abbrs.insightsComponents}${resourceToken}'
        containerRegistryName: !useContainerRegistry
        ? ''
        : !empty(containerRegistryName) ? containerRegistryName : '${abbrs.containerRegistryRegistries}${resourceToken}'
        searchServiceName: !useSearch ? '' : !empty(searchServiceName) ? searchServiceName : '${abbrs.searchSearchServices}${resourceToken}'
        searchConnectionName: !useSearch ? '' : !empty(searchConnectionName) ? searchConnectionName : 'search-service-connection'
      }
    }
   ```

   > This module sets up essential AI infrastructure, such as Azure OpenAI, Key Vault, storage accounts, and log analytics.

   > It allows customization via parameters like openAiName, keyVaultName, and storageAccountName.

   > Additionally, it handles the setup for application insights and container registries.

### Task 2: Streamlining Azure Resource Deployment with Azure Developer CLI (azd)

In this task, you will be using the Azure Developer CLI (azd) to deploy the resources defined in your Bicep templates to Azure. The Azure Developer CLI simplifies the process of managing infrastructure as code, allowing you to efficiently deploy, manage, and monitor your applications and resources directly from the command line.

1. On the **Visual Studio Code**, open a **New Terminal** by selecting **Terminal** from the top menu bar and then clicking on **New Terminal**.

   ![](../media/ex1img1.png)

1. Once the terminal is opened, run the following command to sign in and authenticate the azd tool.

   ```bash
   azd auth login
   ```
   
1. Once you run the above command, a sign in page opens up, as you have already logged in to portal you just need to select your account.

1. Once you have logged in successfully, navigate back to your terminal and run the following command to authenticate the **Azure CLI** tool as well.

   ```bash
   az login
   ```

   > **Note:** You may need to minimize Visual Studio Code pane to see the pop up window to sign in.

1. Once you are on the pop up window, select **Work or school account** and click on **Continue**.

   ![](../media/ex1img3.png)

1. On the sign-in page, provide the following details:

   - Username: <inject key="AzureAdUserEmail"></inject> and click on **Next**.

      ![](../media/ex1img4.png)

   - Password: <inject key="AzureAdUserPassword"></inject> and click on **Sign in**.

      ![](../media/ex1img5.png)

1. When prompted, click on **No, sign in to this app only** and continue.

1. Return to your **Visual Studio Code** terminal. You may now be prompted to select a subscription from a list. Enter **1** and press **Enter**.

1. Run the following command to set the execution policy to avoid any security-related issues.

   ```
   Set-ExecutionPolicy Bypass -Scope LocalMachine -Force
   ```

1. Once you have successfully logged in, run the following command, which will deploy all the defined resources in Azure.

   ```
   azd up
   ```

1. Once you run this command, it will prompt you to select a subscription. Just hit enter and continue.

   ![](../media/ex1newimg4.png)

1. In the next prompt, it will ask to select a location. Use the arrow keys and select **<inject key="region" enableCopy="false"/>** region from the list. **If you select any other region, the deployment will fail due to less quota for the OpenAI Model**. 

1. In the next prompt, select **rg-creative-<inject key="DeploymentID" enableCopy="false"/>** for resource group.

   ![](../media/rg-sel.png)

   > This may take up to 15 minutes to deploy all the resources, till then please move to the next exercise as that is a read-only exercise where you will get to know the core application and technology stacks used.

   > If you face any error related to deployment, please rerun the `azd up` command.

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.
   
   <validation step="45d3bb64-1c15-4974-9ead-629db5887289" />

## Summary

In this exercise, you have learned how to use the Azure Developer CLI (AZD) to deploy resources for your application. You explored how to define and configure infrastructure using a Bicep file, initialized tracing for monitoring, and successfully deployed resources such as container apps and AI environments using AZD.

### Click on Next >> to proceed to the next exercise.