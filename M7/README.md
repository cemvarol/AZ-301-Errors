
# 7th Module on the labs order, 301-T02A Mod2 Exercise Outputs errors. 

# This Lab can not be completed, you can check throuhg screenshots...

### Cleanup command after the Lab 7 down below
az group list --query "[?starts_with(name,'AADesignLab07')]".name --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'

