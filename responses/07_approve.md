# Configuring your Azure environment

To deploy successfully to our Azure environment, we've created a new workflow with two jobs:
1. **Set up Azure resources** will run if the pull request contains a label with the name "spin up environment". 
1. **Destroy Azure resources** will run if the pull request contains a label with the name "destroy environment".

In addition to each job, there's a few global environment variables:
- `AZURE_RESOURCE_GROUP`, `AZURE_APP_PLAN`, and `AZURE_WEBAPP_NAME` are names for our resource group, app service plan, and web app, respectively, which we'll reference over multiple steps and workflows
- `AZURE_LOCATION` lets us specify the [region](https://azure.microsoft.com/en-us/global-infrastructure/regions/) for the data centers, where our app will ultimately be deployed. 

### Setting up Azure resources

The first job sets up the Azure resources as follows:
1. Logs into your Azure account with the [`azure/login`](https://github.com/Azure/login) action. The `AZURE_CREDENTIALS` secret you created earlier is used for authentication.
1. Creates an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/overview#resource-groups) by running [`az group create`](https://docs.microsoft.com/en-us/cli/azure/group?view=azure-cli-latest#az-group-create) on the Azure CLI, which is [pre-installed on the GitHub-hosted runner](https://help.github.com/en/actions/reference/software-installed-on-github-hosted-runners).
1. Creates an [App Service plan](https://docs.microsoft.com/en-us/azure/app-service/overview-hosting-plans) by running  [`az appservice plan create`](https://docs.microsoft.com/en-us/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create) on the Azure CLI.
1. Creates a [web app](https://docs.microsoft.com/en-us/azure/app-service/overview) by running [`az webapp create`](https://docs.microsoft.com/en-us/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) on the Azure CLI.
1. Configures the newly created web app to use [GitHub Packages](https://help.github.com/en/packages/publishing-and-managing-packages/about-github-packages) by using [`az webapp config`](https://docs.microsoft.com/en-us/cli/azure/webapp/config?view=azure-cli-latest) on the Azure CLI. Azure can be configured to use its own [Azure Container Registry](https://docs.microsoft.com/en-us/azure/container-registry/), [DockerHub](https://docs.docker.com/docker-hub/), or a custom (private) registry. In this case, we'll configure GitHub Packages as a custom registry. 

### Destroying Azure resources

The second job destroys Azure resources so that you do not use your free minutes or incur billing. The job works as follows:
1. Logs into your Azure account with the [`azure/login`](https://github.com/Azure/login) action. The `AZURE_CREDENTIALS` secret you created earlier is used for authentication.
1. Deletes the resource group we created earlier using [`az group delete`](https://docs.microsoft.com/en-us/cli/azure/group?view=azure-cli-latest#az-group-delete) on the Azure CLI.

## Step 6: Approve the pull request

I've requested your approval on this pull request. Once you approve this, I will merge.

### :keyboard: Activity: Approve pull request adding the necessary configuration files

1. Apply the "spin up environment" label to this pull request
1. Wait for the GitHub Actions workflow to run and spin up your Azure environment. You can follow along in the Actions tab or in the pull request merge box.
1. Once the workflow succeeds, approve this pull request

---

I'll respond when I receive an approval on this pull request.
