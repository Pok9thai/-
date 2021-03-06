---
title: 部署到 Azure App Service
intro: 您可以部署到 Azure App Service，作为持续部署 (CD) 工作流程的一部分。
redirect_from:
  - /actions/guides/deploying-to-azure-app-service
  - /actions/deployment/deploying-to-azure-app-service
versions:
  fpt: '*'
  ghes: '*'
  ghae: '*'
  ghec: '*'
type: tutorial
topics:
  - CD
  - Containers
  - Azure App Service
shortTitle: 部署到 Azure App Service
---

{% data reusables.actions.enterprise-beta %}
{% data reusables.actions.enterprise-github-hosted-runners %}
{% data reusables.actions.ae-beta %}

## 简介

本指南说明如何使用 {% data variables.product.prodname_actions %} 生成、测试并部署应用程序到 [Azure App Service](https://azure.microsoft.com/en-us/services/app-service/)。

Azure App Service 可以用几种语言运行 web 应用程序，但本指南演示的是部署现有的 Node.js 项目。

{% ifversion fpt or ghec or ghae-issue-4856 %}

{% note %}

**Note**: {% data reusables.actions.about-oidc-short-overview %} and "[Configuring OpenID Connect in Azure](/actions/deployment/security-hardening-your-deployments/configuring-openid-connect-in-azure)."

{% endnote %}

{% endif %}

## 基本要求

在创建 {% data variables.product.prodname_actions %} 工作流程之前，首先需要完成以下设置步骤：

1. 创建 Azure App Service 计划。

   例如，可以使用 Azure CLI 创建新的应用服务计划：

   ```bash{:copy}
   az appservice plan create \
       --resource-group MY_RESOURCE_GROUP \
       --name MY_APP_SERVICE_PLAN \
       --is-linux
   ```

   在上述命令中，将 `MY_ResourcesURCE_GROUP` 替换为您原有的 Azure 资源组，`MY_APP_SERVICE_PLAN` 替换为应用服务计划的新名称。

   请查看 Azure 文档以了解更多有关使用 [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/) 的信息：

   * 有关身份验证，请参阅“[使用 Azure CLI 登录](https://docs.microsoft.com/en-us/cli/azure/authenticate-azure-cli)”。
   * 如果需要创建新的资源组，请参阅“[az 组](https://docs.microsoft.com/en-us/cli/azure/group?view=azure-cli-latest#az_group_create)”。

2. 创建 Web 应用。

   例如，可以使用 Azure CLI 创建具有节点运行时的 Azure App Service Web 应用：

   ```bash{:copy}
   az webapp create \
       --name MY_WEBAPP_NAME \
       --plan MY_APP_SERVICE_PLAN \
       --resource-group MY_RESOURCE_GROUP \
       --runtime "node|10.14"
   ```

   在上面的命令中，将参数替换为您自己的值，其中 `MY_WEBAPP_NAME` 是 Web 应用的新名称。

3. 配置 Azure 发布配置文件并创建 `AZURE_WEBAPP_PUBLISH_PROFILE` 机密。

   使用发布配置文件生成 Azure 部署凭据。 更多信息请参阅 Azure 文档中的“[生成部署凭据](https://docs.microsoft.com/en-us/azure/app-service/deploy-github-actions?tabs=applevel#generate-deployment-credentials)”。

   在 {% data variables.product.prodname_dotcom %} 仓库中，创建一个名为 `AZURE_WEBAPP_PUBLISH_PROFILE` 的机密，其中包含发布配置文件的内容。 有关创建机密的更多信息，请参阅“[加密密码](/actions/reference/encrypted-secrets#creating-encrypted-secrets-for-a-repository)”。

4. For Linux apps, add an app setting called `WEBSITE_WEBDEPLOY_USE_SCM` and set it to true in your app. For more information, see "[Configure apps in the portal](https://docs.microsoft.com/en-us/azure/app-service/configure-common#configure-app-settings)" in the Azure documentation.

{% ifversion fpt or ghes > 3.0 or ghae or ghec %}
5. Optionally, configure a deployment environment. {% data reusables.actions.about-environments %}
{% endif %}

## 创建工作流程

完成先决条件后，可以继续创建工作流程。

The following example workflow demonstrates how to build, test, and deploy the Node.js project to Azure App Service when there is a push to the `main` branch.

确保在工作流程 `env` 中将 `AZURE_WEBAPP_NAME` 密钥设置为您创建的 web 应用程序名称。 You can also change `AZURE_WEBAPP_PACKAGE_PATH` if the path to your project is not the repository root and `NODE_VERSION` if you want to use a node version other than `10.x`.

{% data reusables.actions.delete-env-key %}

```yaml{:copy}
{% data reusables.actions.actions-not-certified-by-github-comment %}

on:
  push:
    branches:
      - main

env:
  AZURE_WEBAPP_NAME: MY_WEBAPP_NAME   # set this to your application's name
  AZURE_WEBAPP_PACKAGE_PATH: '.'      # set this to the path to your web app project, defaults to the repository root
  NODE_VERSION: '10.x'                # set this to the node version to use

jobs:
  build-and-deploy:
    name: Build and Deploy
    runs-on: ubuntu-latest
    environment: production

    steps:
      - uses: actions/checkout@v2

      - name: Use Node.js {% raw %}${{ env.NODE_VERSION }}{% endraw %}
        uses: actions/setup-node@v2
        with:
          node-version: {% raw %}${{ env.NODE_VERSION }}{% endraw %}

      - name: npm install, build, and test
        run: |
          # Build and test the project, then
          # deploy to Azure Web App.
          npm install
          npm run build --if-present
          npm run test --if-present

      - name: 'Deploy to Azure WebApp'
        uses: azure/webapps-deploy@0b651ed7546ecfc75024011f76944cb9b381ef1e
        with:
          app-name: {% raw %}${{ env.AZURE_WEBAPP_NAME }}{% endraw %}
          publish-profile: {% raw %}${{ secrets.AZURE_WEBAPP_PUBLISH_PROFILE }}{% endraw %}
          package: {% raw %}${{ env.AZURE_WEBAPP_PACKAGE_PATH }}{% endraw %}
```

## 其他资源

以下资源也可能有用：

* 有关原始入门工作流程，请参阅 {% data variables.product.prodname_actions %} `starter-workflows` 仓库中的 [`azure.yml`](https://github.com/actions/starter-workflows/blob/main/deployments/azure.yml)。
* 用于部署 Web 应用的操作是正式的 Azure [`Azure/webapps-deploy`](https://github.com/Azure/webapps-deploy) 操作。
* For more examples of GitHub Action workflows that deploy to Azure, see the [actions-workflow-samples](https://github.com/Azure/actions-workflow-samples) repository.
* Azure web 应用文档中的“[在 Azure 中创建 Node.js web 应用](https://docs.microsoft.com/en-us/azure/app-service/quickstart-nodejs)”快速入门说明如何通过 [Azure App Service 扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)使用 VS Code。
