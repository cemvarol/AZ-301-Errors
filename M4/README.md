# 4th Module on the labs order, 301-T03A Mod2 Exercise Outputs and fixes. 
# Ex-4 is Optional

# Cleanup command after the Lab 4 down below
az group list --query "[?starts_with(name,'AADesignLab04')]".name --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'


