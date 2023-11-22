# **Fashion Dataform Project**

# About
An example Dataform project to load and transform the publicly available dataset from [H&M Group](https://www.kaggle.com/competitions/h-and-m-personalized-fashion-recommendations/data) into a format which could be imported into [Discovery AI for Retail](https://cloud.google.com/solutions/retail-product-discovery?hl=en) or [Vertex AI Search and Conversation](https://cloud.google.com/generative-ai-app-builder/docs/introduction), allowing you to train a retail recommendations model.

# Prerequisites

## Google Cloud Project

Google Cloud projects form the basis for creating, enabling, and using all Google Cloud services, such as Dataform, BigQuery and the Retail API.

If you do not already have a Google Cloud project for which you want to load the IMDB dataset into, then you will need to create a new Google Cloud project.  The documentation on how to do this can be found [here](https://cloud.google.com/resource-manager/docs/creating-managing-projects#creating_a_project).

Once you have a Google Cloud project, remember to take note of the Project Number and Project ID. These can be found on the Google Cloud project console welcome page, which you can find [here](https://console.cloud.google.com/welcome).

## Google Cloud Storage Bucket

Now you have a Google Cloud project, you need to create a Google Cloud Storage Bucket for which the IMDB dataset will be uploaded into and Dataform will use to source the data in which to load data into BigQuery.  The documentation on how to create a new storage bucket can be found [here](https://cloud.google.com/storage/docs/creating-buckets).

Remeber to take note of the bucket name as this will be required for one of the Dataform config variables.

## Enable Dataform Service

Next, you will need to enable the Dataform service within the Google Cloud project just created.  This can be achieved by clicking the "Enable" button [here](https://console.cloud.google.com/marketplace/product/google/dataform.googleapis.com).

## Create a Dataform Repository

After the Dataform Service has been enabled, you will be redirected to the BigQuery Dataform page within the Google Cloud console.  For reference, this can be found [here](https://console.cloud.google.com/bigquery/dataform).

Go ahead and create a repository.  For more information on how to do this, go to the documentation page found [here](https://cloud.google.com/dataform/docs/create-repository).

## Grant Permissions to Dataform Service Account

When you create your first Dataform repository, Dataform automatically generates a service account. Dataform uses the service account to interact with BigQuery on your behalf.

Your Dataform service account ID is in the following format:

```
service-YOUR_PROJECT_NUMBER@gcp-sa-dataform.iam.gserviceaccount.com
```

Replace YOUR_PROJECT_NUMBER with the Project Number of your Google Cloud project, which you previously took note of.

The Dataform service account requires a number of IAM roles with which to be able to execute the workflows in BigQuery and load data from the Google Cloud Storage Bucket.  This can be achieved by following these steps:

1. In the Google Cloud console, go to the [IAM page](https://console.cloud.google.com/iam-admin).
2. Click Add.
3. In the New principals field, enter your Dataform service account ID.
4. In the Select a role drop-down list, select the BigQuery Job User role.
5. Click Add another role, and then in the Select a role drop-down list, select the BigQuery Data Editor role.
6. Click Add another role, and then in the Select a role drop-down list, select the BigQuery Data Viewer role.
7. Click Add another role, and then in the Select a role drop-down list, select the Storage Object Viewer role.
8. Click Save.

## DataForm Project Configuration

The ```dataform.json``` contains the following parameters
- ```defaultSchema```: Name of the BigQuery Dataset for which the Fashion tables are to be created
- ```assertionSchema```: Name of the BigQuery Dataset for which any Dataform Assertions are to be executed and output
- ```defaultDatabase```: The Project ID of your Google Cloud project, which you previously took note of
- ```defaultLocation```: Target BigQuery Location
- ```LOAD_GCS_BUCKET```: Name of the Google Cloud Storage Bucket, which you previously took note of
- ```RAW_DATA```: Name of the BigQuery Dataset for which the Fashion data files are to be loaded
- ```BASE_DATA```: Name of the BigQuery Dataset for which the final transformed Fashion tables are to be located
- ```LAST_DATE```: The date in which you would like the Transaction data to be realigned to

Here is what an example configuration looks like
```json
{
  "defaultSchema": "fashion",
  "assertionSchema": "DF_ASSERTIONS",
  "warehouse": "bigquery",
  "defaultDatabase": "winter-dataform",
  "defaultLocation": "australia-southeast1",
  "vars": {
    "LOAD_GCS_BUCKET": "winter-data/fashion",
    "RAW_DATA": "fashion_staging",
    "BASE_DATA": "fashion",
    "LAST_DATE": "2023-11-22"
  }
}
```


