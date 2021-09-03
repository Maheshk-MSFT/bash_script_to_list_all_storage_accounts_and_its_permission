# bash_script_to_list_all_storage_accounts_and_its_permission


 #!/bin/sh
 #az login 
 
 echo "Login successful"  
 
 storageaccountlist=$(az storage account list --query "[].{name:name,resourceGroup:resourceGroup}" --output tsv)
 for storage_account_name in $storageaccountlist
 do 
 	 echo "STORAGE -> $storage_account_name , RG -> $resourceGroup"
	  
	account_key=$(az storage account keys list --account-name $storage_account_name --resource-group $resourceGroup --query "[0].{value:value}" --output tsv)
	 
	 echo "account_key-> $account_key"
	 
	 containerlist=$(az storage container list --account-name $storage_account_name --account-key $account_key --query "[].{name:name}" --output tsv)
	   for container_name in $containerlist
	   do
	   
	   permission=$(az storage container show-permission --name ${container_name} --account-name $storage_account_name  --account-key $account_key --output table)
	  
	  echo "container_name: $container_name & permission: $permission"
	   done	 
  done	
 echo "Done" 
 
