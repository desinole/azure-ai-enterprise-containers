# Azure AI Containers

Some pre-requisites for this demo:

1. Create an Azure Key Vault and disable rbac-authentication for the vault.
```azurecli
az keyvault create \
    --name <key vault name> \
    --resource-group <resource group name> \
    --location <location> \
    --enable-rbac-authorization false
    --sku standard
```
2. Standup an Azure Cognitive Services resource for OCR with az cli.
```azurecli
az cognitiveservices account create \
    --name <resource name> \
    --resource-group <resource group name> \
    --kind ComputerVision \
    --sku S0 \
    --location <location>
```
3. Obtain Azure Cognitive Services resource key and endpoint.
```azurecli
az cognitiveservices account keys list \
    --name <resource name> \
    --resource-group <resource group name>
```
4. insert resource key and endpoint into the key vault as secrets
```azurecli
az keyvault secret set \
    --vault-name <key vault name> \
    --name <secret name> \
    --value <secret value>
```
5. Create a service principal.
```azurecli
az ad sp create-for-rbac \
    --name <service principal name> \
    --role contributor \
    --scopes /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.KeyVault/vaults/<key vault name> \
    --sdk-auth
```
4. Provide the service principal with get and list secrets access to the key vault and cognitive services resource.
```azurecli
az keyvault set-policy \
    --name <key vault name> \
    --spn <service principal id> \
    --secret-permissions get list
```