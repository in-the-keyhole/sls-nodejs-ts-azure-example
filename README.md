# sls-nodejs-ts-azure-example

This repository contains a VSCode Remote Container Setup for developing NodeJS/TypeScript based Azure functions using the Serverless Framework.

## Visual Studio Code - Remote Container Usage

> [Official Documentation](https://code.visualstudio.com/docs/remote/containers)

> _Note: Once you meet the prerequisites, you can run **ANY** Visual Studio Code Remote Container, which provides a Docker based development environment ensuring a consistent and reliable set of tooling needed to interact and execute a repository codebase_

### Prerequisites:

1. macOS, Windows, Linux -- [System Requirements](https://code.visualstudio.com/docs/remote/containers#_system-requirements)
2. Docker - [Documentation](https://code.visualstudio.com/docs/remote/containers#_installation)
3. Visual Studio Code - [Official Site](https://code.visualstudio.com/)
4. Remote - Containers _Visual Studio Code extension_ - [Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

#### Environment Variables

The remote container honors the following environment variables set in the .devcontainer/.env

> _Note: You can copy the .devcontainer/.env.template file to .devcontainer/.env and supply the following variables_

##### Azure

- AZURE_SUBSCRIPTION_ID
- AZURE_TENANT_ID
- AZURE_CLIENT_ID
- AZURE_CLIENT_SECRET
- AZURE_LOCATION - (optional - defaults to Central US)

> _Note: Changes to variables in .env after the container is running will require the Remote Container to be restarted_

#### Developer Configuration

To initialize the environment, once the repository is opened in the Remote Container, open a Terminal and type:

`yarn`

### Usage & Things you can do

#### package.json Scripts

There are some predefined scripts in package.json that can be used to simplify common tasks.

- `yarn start` - runs the functions locally using the Azure configuration
- `yarn deploy` - deploys the functions to Azure
- `yarn undeploy` - undeploys the functions from Azure

#### Debugging - Local

All of the package.json scripts defined above can be run in a 'JavaScript Debug Terminal' which automatically attaches the debugger.

#### Azure Cleanup

When _undeploying_ from Azure, the APIM configuration is [_soft-deleted_](https://docs.microsoft.com/en-us/azure/api-management/soft-delete) and will need to be completely removed prior to a re-deploy.

1. `az login` - this will opens browser prompting for login
   > Note: You will need to wait for the port to be opened from the container before clicking on username
2. `az account get-access-token --resource https://management.azure.com/`
   > Note: This will print an _accessToken_ needed in the next command to stdout
3. `curl --location --request DELETE 'https://management.azure.com/subscriptions/<YOUR AZURE_SUBSCRIPTION_ID>/providers/Microsoft.ApiManagement/locations/<AZURE_LOCATION>/deletedservices/<SOFT DELETED API SERVICE ID>?api-version=2021-08-01' \ --header 'Authorization: Bearer <ACCESS TOKEN FROM ABOVE>'`

#### Additional Resources

Refer to [Serverless docs](https://www.serverless.com/framework/docs/providers/azure/) for more information.
