# pbi-pq-commenter-with-azure-openai
Power BI Template that uses Azure Open AI to analyze the Power Query code in a semantic model and offer comments and variable name/applied steps name changes.

***Important Note #1**: This guide is customized to Power BI for the Commercial environment.*

***Important Note #2**: This guide uses scripts that I built and tested on environments I have access to. Please review all scripts if you plan for production use, as you are ultimately responsible for the code that runs in your environment.*

# Prerequisities

### Power BI
-   Power BI Premium Per User, Premium, or Fabric workspace. If you do not have a Premium Per User license, use the "Buy Now" feature on <a href="https://docs.microsoft.com/en-us/power-bi/admin/service-premium-per-user-faq" target="_blank">Microsoft's site</a> or if you don't have access to do that, please contact your administrator (be nice!).

### Desktop

-   Power BI Desktop installed.

### Azure OpenAI

-   A ChatGPT 4.0 model deployed.  Instructions can be followed [at this link](https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/create-resource?pivots=web-portal).

## Installation Steps

1. Identify the Workspace Connection for your workspace by following [these instructions](https://learn.microsoft.com/en-us/power-bi/enterprise/service-premium-connect-tools).  In workspace settings, it's at the bottom of the Premium tab (see Figure 1).

![Figure 1](./documentation/images/workspace-settings.png)
** Figure 1 - Example of Server Settings URL (Workspace Connection).

1. Open download the latest release of the template file.

2. Open the template file.

3.  You will be prompted to enter the following:

- Workspace Connection - This is retrieved in Step 1.
- Semantic Model Name - This is the name of the dataset/semantic model in the workspace
-  Azure Open API URL - This can be retrieved during the deployment of the ChatGPT 4.0 model in the [prerequisites](#prerequisities). For example, if the name of my OpenAI instance is "ABC" and the ChatGPT deployment is named "XYZ" here is the sample URL: *https://XYZ.openai.azure.com/openai/deployments/ABC/chat/completions?api-version=2023-07-01-preview*

- Azure Open API Key - This can be also be retrieved during the deployment of the ChatGPT 4.0 model in [prerequisites](#prerequisities). **Please read the section on [security](#security) prior to sharing this file with anyone**.

#![Figure 2](./documentation/images/template-popup.png)
*Figure 2 - Template popup for parameters*

Press the load button when you have finished.

4. You should be prompted to run a native database query (example in Figure 3). This is to run the dynamic management view queries to extract the Power Query Code. Select the "Run" option. *Note this may occur once or twice, so select the "Run" option in all cases.*

#![Figure 3](./documentation/images/native-database-query.png)
*Figure 3 - Native Database Query screenshot*

5. You will also be prompted "Access SQL Server Analysis Services". This allows the template to query the dynamic management views in the Power BI Service. Login with your Microsoft account and select the "Connect" option.

#![Figure 4](./documentation/images/pbi-auth.png)
*Figure 4 - Access SQL Service Analysis Services screenshot*

6. You will be prompted "Access Web Contents".  This allows the template to query Azure OpenAI.  Select the Anonymous option and select the "Connect" button.

#![Figure 5](./documentation/images/open-ai-auth.png)
*Figure 5 - Access Web Contents screenshot*

7. Once the refresh has completed you should see report.

#![Figure 6](./documentation/images/report-example.png)

## Report Overview

The report has 4 main features as identified in Figure 7.
#![Figure 7](./documentation/images/report-overview.png)

1. Select Table - You can filter which table you wish to view the existing Power Query code and the comments or variables/applied steps renaming.

2. Enhancement Options - You can choose between just seeing what ChatGPT has offered for comments or see comments and variable/applied steps renaming.

3. This is the existing Power Query code for the selected table.

4. This is the proposed changes provided by ChatGPT based on your selection in Enhancement Options.

# Security

I tried to provide this template as a low barrier to entry, so in this case the API Key is stored as a parameter.  Please make sure not to share your pbix or pbip implementation with other folks, nor provide them access to the parameters of the semantic model in the service if they shouldn't access that key.

This is an issue with Web.Contents only allowing Anonymous connections with when issuing a POST HTTP Request to an API.  If you try to do [this approach](https://learn.microsoft.com/en-us/powerquery-m/web-contents#example-3) with Web.Contents  you'll get this error: "Web.Contents with the Content option is only supported when connecting anonymously".