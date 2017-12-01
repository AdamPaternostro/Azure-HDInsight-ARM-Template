# Azure-HDInsight-ARM-Template
Creates an HDInsight cluster that has an external Hive metastore and access to Azure Data Lake Store

If you need to create a Spark (or other HDInsight cluster, but this is mainly Spark), the included template creates the cluster with just a couple of Azure Portal clicks or you can use Azure CLI or Automation to create the cluster every day.

I typically kick off the cluster creation and do something else for 20 minutes while it spins up.  When done using the cluster, I delete either the entire resource group (the cluster and storage account if I created a new resource group when creating the cluster) or just delete the cluster.  If you save Notebooks and such, they will be saved in the storage account, so you need to preserve this if you want to keep those artifacts.  You could use data lake as your primary storage, but I find it easier to just delete my storage account versus cleaning up folders in my data lake.  If you need to update or modify this template, it is easiest to go in the portal, select create an HDInsight cluster, pick all the parameters you want, then on the very last step click automation options and download the template.  Then cut and paste what is different from that template into this one.  Test it (it usually takes a couple of times to get it right).

## Prerequisites
1 - Create a SQL Database in Azure.  This will create a server (e.g. hdihiveserver) and a database (e.g. hivedb). You should select a S1 or higher sized database.
2 - Create a Data Lake Store in Azure.

## Steps
1 - Run the PowerShell script to create a service principle in Azure. This service principle will be backed by a certificate.  HDInsight clusters need a service principle based upon a certificate.  The script is here: https://github.com/AdamPaternostro/Azure-Create-HDInsight-Service-Principle. Save the parameters that were printed and save the PFX on your desktop.  In fact, upload these into your data lake into it owns folder!
2 - Open your Data Lake Store account and grant access to the Service Principle created.  You should select Advanced and select "Set as a default permission".  This means all new folders created, the service principle will have access.
3 - Take the values from step 1 and update the ARM template (parameters at the top) in this repository. You might want to make the machine sizes a parameter, but that is up to you.
4 - In the Azure Portal save the template. There is a feature to save templates, then you just need to hit the deploy button when you want a cluster.  You will need to enter a resource group name (I usually name this the same name as my cluster.  In fact I name my resource group, cluster, storage account and blob container in the storage account all the same name.  That's just my preference).

![alt tag](https://raw.githubusercontent.com/AdamPaternostro/Azure-HDInsight-ARM-Template/master/Azure-Template.png)
