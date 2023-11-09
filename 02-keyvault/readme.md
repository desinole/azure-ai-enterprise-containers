###### Process
- Login using az cli, create resource group, create ACR

```azurecli
az login
```

```azurecli
SUFFIX=techbash2023
```

```azurecli
LOCATION=eastus2
```

```azurecli
az group create --name rgai$SUFFIX --location $LOCATION
```


Create keyvault
```azurecli
az keyvault create --name kv$SUFFIX --resource-group rgai$SUFFIX --location $LOCATION
```

Create SP
```azurecli
az ad sp create-for-rbac -n "api://app$SUFFIX" --role owner --scopes $(az group show --name rgai$SUFFIX --query id --output tsv)
```

Turn off rbac and use kV policies instead
```azurecli
az keyvault update -n kv$SUFFIX --enable-rbac-authorization false
```

Grab object id from Azure AD portal under app registration (search with app display name)

Store SP credentials in appsettings.json
```azurecli
az keyvault set-policy -n kv$SUFFIX --object-id "object id from previous step" --secret-permissions get list
```
store secret in key vault
```azurecli
az keyvault secret set --vault-name kv$SUFFIX --name Cognitive-Services-Key --value $(az cognitiveservices account keys list --name cgvision$SUFFIX --resource-group rgai$SUFFIX --query key1 -o tsv)
```



Regenerate and rotate keys
```azurecli
 az cognitiveservices account keys regenerate --name cgvision$SUFFIX --resource-group rg$SUFFIX --key-name key1
```

```azurecli
az keyvault secret set --vault-name kv$SUFFIX --name Cognitive-Services-Key --value $(az cognitiveservices account keys list --name cgvision$SUFFIX --resource-group rgai$SUFFIX --query key1 -o tsv)
```