## Provision a Cognitive Services resource

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

Create cognitive services resource

```azurecli
az cognitiveservices account create --name cgvision$SUFFIX --resource-group rgai$SUFFIX --sku F0 --location $LOCATION --yes --kind ComputerVision
```

Get the key and endpoint

```azurecli
az cognitiveservices account keys list --name cgvision$SUFFIX --resource-group rgai$SUFFIX
```

```azurecli
az cognitiveservices account show --name cgvision$SUFFIX --resource-group rgai$SUFFIX --query properties.endpoint
```

Use the keys and endpoint in the appsettings.json file (copy paste the proper values)

```json
{
    "CognitiveServicesEndpoint": "https://cgvision$SUFFIX.cognitiveservices.azure.com/",
    "CognitiveServiceKey": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
}
```