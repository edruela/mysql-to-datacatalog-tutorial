<!---
Note: This tutorial is meant for Google Cloud Shell, and can be opened by going to
http://gstatic.com/cloudssh/images/open-btn.svg)](https://console.cloud.google.com/cloudshell/open?git_repo=https://github.com/mesmacosta/mysql-to-datacatalog-tutorial&tutorial=TUTORIAL.md)--->
# MySQL to Data Catalog Tutorial

<!-- TODO: analytics id? -->
<walkthrough-author name="mesmacosta@gmail.com" tutorialName="MySQL to Data Catalog Tutorial" repositoryUrl="https://github.com/mesmacosta/mysql-to-datacatalog-tutorial"></walkthrough-author>

## Intro

This tutorial will walk you through the execution of the MySQL to Data Catalog Tutorial.

## Environment

Let's start by setting up your environment.

## Set Up your Project

Start by setting your project ID. Replace the placeholder to your project.
```bash
gcloud config set project MY_PROJECT_PLACEHOLDER
```

Next load it in a environment variable.
```bash
export PROJECT_ID=$(gcloud config get-value project)
```

## Create the MySQL Database

You can use the open source scripts at [Cloud SQL MySQL Tooling](https://github.com/mesmacosta/cloudsql-mysql-tooling) to create and populate your MySQL instance.

Clone the github repository:
```bash
git clone https://github.com/mesmacosta/cloudsql-mysql-tooling
```

Next execute the `init-db.sh` script.
This will create your MySQL instance and populate it with random schema.
```bash
source init-db.sh
```

## Execute the MySQL connector

You can build the MySQL connector yourself by going to
[this GitHub repository](https://github.com/GoogleCloudPlatform/datacatalog-connectors-rdbms/tree/master/mysql2datacatalog).

To facilitate its usage, we are going to use a docker image. 
The environment variables were loaded by the `init-db.sh` script.

Execute the connector:
```bash
docker run --rm --tty -v \
"$PWD":/data gcr.io/mesmacosta/mysql2datacatalog:stable \
--datacatalog-project-id=$PROJECT_ID \
--datacatalog-location-id=us-central1 \
--mysql-host=$public_ip_address \
--mysql-user=$username \
--mysql-pass=$password \
--mysql-database=$database
```

## Check the results of the script

After the script finishes, you can go to Go to Data Catalog search UI:
[Search UI](https://console.cloud.google.com/datacatalog?q=system=mysql)
 and search for MySQL metadata.

## Cleaning up

The easiest way to avoid incurring charges to your Google Cloud account for the resources used in this tutorial is to delete 
the project you created. Otherwise you can run the clean up scripts below.

To delete the project, follow the steps below:

1.  In the Cloud Console, [go to the Projects page](https://console.cloud.google.com/iam-admin/projects).

2.  In the project list, select the project that you want to delete and click **Delete project**.

    ![N|Solid](https://storage.googleapis.com/gcp-community/tutorials/partial-redaction-with-dlp-and-gcf/img_delete_project.png)
    
3.  In the dialog, type the project ID, and then click **Shut down** to delete the project.

To delete the created resources and mantain the project,
 follow the steps below:

Delete the MySQL metadata:
```bash
./cleanup-db.sh
```

Execute the connector again:
```bash
docker run --rm --tty -v \
"$PWD":/data gcr.io/mesmacosta/mysql2datacatalog:stable \
--datacatalog-project-id=$PROJECT_ID \
--datacatalog-location-id=us-central1 \
--mysql-host=$public_ip_address \
--mysql-user=$username \
--mysql-pass=$password \
--mysql-database=$database
```

Delete the MySQL database:
```bash
./delete-db.sh
```

## Congratulations!

<walkthrough-conclusion-trophy></walkthrough-conclusion-trophy>

You've successfully finished the MySQL to Data Catalog Tutorial.
