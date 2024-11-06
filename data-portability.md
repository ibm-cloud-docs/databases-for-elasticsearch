---

copyright:
  years: 2024
lastupdated: "2024-11-06"

keywords:

subcollection: databases-for-elasticsearch

---

{{site.data.keyword.attribute-definition-list}}



# Understanding data portability for {{site.data.keyword.databases-for-elasticsearch}}
{: #data-portability}





[Data Portability](#x2113280){: term} involves a set of tools, and procedures that enable customers to export the digital artifacts that would be needed to implement similar workload and data processing on different service providers or on-prem software. It includes procedures for copying and storing the service customer's content, including the related configuration used by the service to store and process the data, on customer's own location.
{: shortdesc}

## Responsibilities
{: #data-portability-responsibilities}

{{site.data.keyword.cloud}} services provide interfaces and instructions to guide the customer to copy and store the service customer content, including the related configuration, on their own selected location.

The customer then is responsible for the use of the exported data and configuration for the purpose of data portability to other infrastructures.
This can involve:

- the planning and execution for setting up alternate infrastructure on on different cloud providers or on-prem software that provide similar capabilities to the IBM services
- the planning and execution for the porting of the required application code on the alternate infrastructure, including the adaptation of customer's application code, deployment automation, etc.
- the conversion of the exported data and configuration to format required by the alternate infrastructure and adapted applications


To find out more about responsibility ownership for using {{site.data.keyword.cloud}} products between {{site.data.keyword.IBM_notm}} and customer see [Shared responsibilities for {{site.data.keyword.cloud_notm}} products](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-responsibilities-cloud-databases).



For more information about your responsibilities when using {{site.data.keyword.databases-for-elasticsearch}}, see [Shared responsibilities for {{site.data.keyword.databases-for-elasticsearch}}](https://cloud.ibm.com/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-responsibilities-cloud-databases).

## Data export procedures
{: #data-portability-procedures}

{{site.data.keyword.databases-for-elasticsearch}} provides mechanisms to export your content that has been uploaded, stored, and processed using the service.


You can take snapshots of your data using the procedure described in [Creating snapshots](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-esmigration-elasticsearch-snapshot-restore).



## Exported data formats
{: #data-portability-data-formats}



The format of the data exported using the snapshots is a binary file with elasticsearch propriatory format and can be used only with elasticsearch instances.
Exporting to other data formats is supported using [Logstash](https://www.elastic.co/logstash/){: external}.

## Data ownership
{: #data-ownership}

All exported data are classified as Customer content and therefore apply to them the full customer ownership and licensing rights, as stated in [IBM Cloud Service Agreement](https://www.ibm.com/terms/?id=Z126-6304_WS).


