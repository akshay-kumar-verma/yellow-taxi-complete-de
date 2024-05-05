# yellow-taxi-complete-de
This repository contains the complete end to end DE project based on the yellow taxi data from New York's TLC.

Architecture:
The complete project is architected on Google Cloud Platform. The main Components from GCP are:
  - Cloud Storage
  - Compute Engine
  - BigQuery
  - Looker

COMPONENTS:
CLOUD STORAGE -> is an online file storage service provided by Google as part of it’s cloud compute platform. It allows to store and retrieve data in the cloud, making it accessible from anywhere with internet connection
COMPUTE ENGINE -> is a cloud computing service that provides virtual machines for running applications and services. It allows you to easily create, configure and manage virtual machines with various operating systems and hardware configurations
BIG QUERY -> is a cloud based data warehouse provided by Google Cloud Platform that allows you to store, analyze and query large datasets using SQL like syntax. It is a serverless, highly scalable and cost-effective solution that can process and analyze terabytes to petabytes of data in real time
LOOKER -> is a web based data visualisation and reporting tool that allows you to create interactive dashboards and reports from a variety of data sources, including Google analytics, google sheets, and BigQuery. It enables to turn data into informative and engaging visualizations, which can be easily shared and collaborated on with others.


FACTS & DIMENSIONS TABLE
Fact table:
Contains quantitative measures or metrics that are used for analysis.
Typically contains foreign keys that link to dimension tables.
Contains columns that have high cardinality and change frequently.
Contains columns that are not useful for analysis by themselves, but are necessary for calculating metrics

Dimension table:
Contains columns that describe attributes of the data being analyzed.
Primary key columns that link to fact tables.
Columns that have low cardinality and don’t change frequently.
Columns that can be used for grouping  or filtering data for analysis.

Understanding Data for this Project:
To understand the data we have for this project, we will utilize the data dictionary given to us:
Data Src: https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page
Data Dictionary: 
A data dictionary or data catalog provides information about the dataset, like column names and their descriptions.
The data dictionary given here is particularly for the dataset we have used here:
It tells us about all the fields with names and their descriptions, so that when we are building our data solution we can take the help of this document to design solutions as per the requirement and only have tomake minor changes during the process.


ER MODEL CREATION-> refer to Jupyter notebook ER_DESIGN.ipynb

Once we have already created our ER Model and Processed data to fit into that ER Model Schema, the next steps are as follows:

1) CREATING A COMPUTE ENGINE-INSTANCEE
gcloud compute instances create yellow-taxi-analytics \
    --project=yellow-taxi-analytics \
    --zone=asia-south1-c \
    --machine-type=e2-standard-4 \
    --network-interface=network-tier=PREMIUM,stack-type=IPV4_ONLY,subnet=default \
    --no-restart-on-failure \
    --maintenance-policy=TERMINATE \
    --provisioning-model=SPOT \
    --instance-termination-action=STOP \
    --service-account=427550526658-compute@developer.gserviceaccount.com \
    --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append \
    --tags=http-server,https-server,lb-health-check \
    --create-disk=auto-delete=yes,boot=yes,device-name=yellow-taxi-analytics,image=projects/debian-cloud/global/images/debian-12-bookworm-v20240415,mode=rw,size=10,type=projects/yellow-taxi-analytics/zones/asia-south1-c/diskTypes/pd-balanced \
    --no-shielded-secure-boot \
    --shielded-vtpm \
    --shielded-integrity-monitoring \
    --labels=goog-ec-src=vm_add-gcloud \
    --reservation-affinity=any
	
TO ACCESS THE MAGE UI:
we can copy the Compute engine external IP and add the colon and then the port on which mage is started.
Also we need to add a firewall rule for mage to access the resources of the GCP project, To enable this we can navigate to GCP VM Instance-> Network Interfaces-> Firewall-> Create Firewall Rule-> IPv4 range (0.0.0.0/0) for all ports and connections, we can replace this with our own IP and then the port.

Once we are done with this and able to access the Mage UI, we can go ahead and implement the ETL pipeline with th 3 Data Blocks which are 
1) data extract
2) data transform
3) data load

refer to .py mage files for these mage codes.
refer to BQ codes for sql codes for a little bit of analysis and creating the analytical layer.

