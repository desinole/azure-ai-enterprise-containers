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

Deploy ACI instance (run command from the 03-aci folder)
```azurecli
az container create -g rgai$SUFFIX -f aci.yaml
```

test using following links:
http://public IP of ACI:5000/
http://public IP of ACI:5000/ready
http://public IP of ACI:5000/status
http://public IP of ACI:5000/swagger

Use the postman collection to query an ACI instance that is running an Azure AI container. Update the ACI environment values to match your ACI instance and run