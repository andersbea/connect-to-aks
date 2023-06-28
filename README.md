# connect-to-aks

A composite action to connect to a specific aks context and cluster, with kubectl and helm installed. The composite action streamlines the workflow provided within the following guide [GitHub Actions for deploying to Kubernetes service](https://learn.microsoft.com/en-us/azure/aks/kubernetes-action).

## Usage

Simply call upon the action as so:

```yaml
- name: Connect to AKS
  id: connect-to-aks
  uses: andersbea/connect-to-aks@v0.0.2
  with: 
    CREDENTIALS: ${{ secrets.AZURE_CREDENTIALS }}
    RESOURCE_GROUP: ${{ vars.AZURE_RESOURCE_GROUP }}
    CLUSTER: ${{ vars.AZURE_CLUSTER }}
```

## Parametes

| Name | Description |
| -------- | -------- | 
| CREDENTIALS | The credentials retrieved from Azure (see below) | 
| RESOURCE_GROUP | The resource group to which the kubernetes cluster belongs | 
| CLUSTER | The name of the kubernetes cluster | 

## Credentials

In order to retrieve the necessary credentials, simply run the following command in a terminal that is logged in to azure cli with an account that has the permissions to manage RBAC: 

``` powershell
az ad sp create-for-rbac \
    --name "ghActionAzureVote" \
    --scope /subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP> \
    --role Contributor \
    --sdk-auth
```

The `SUBSCRIPTION_ID` and `RESOURCE_GROUP` can be retrieved through the Azure portal.

The entire JSON result represents the `CREDENTIALS` and should be set as a GitHub secrets value.