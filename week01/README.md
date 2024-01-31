# Week01 Solution Steps

Week1 homework [URL](https://github.com/DataTalksClub/data-engineering-zoomcamp/blob/main/cohorts/2024/01-docker-terraform/homework.md) 

* __Q1__:

    run command ```docker run --help```

    __Answer__: ```--rm```

* __Q2__:
    
    1- Create Dockerfile from image python:3.9 and override entrypoint to bash
    2- Build the image ```docker build -t check_wheel:v01```
    3- Run the image ```docker run -it check_wheel:v01```
    4- run ```pip list```

    __Answer__: 0.42.0

* __Q3__:
    ```
    SELECT
        COUNT(*)
    FROM
        green_taxi_data
    WHERE
        lpep_pickup_datetime >= TO_DATE('20190918','YYYYMMDD') AND lpep_dropoff_datetime < TO_DATE('20190919','YYYYMMDD');
    ```
    __Answer__: 15612

* __Q4__:
    ```
    SELECT
        DATE_TRUNC('day', lpep_pickup_datetime) TRIP_DAY,
        MAX(trip_distance) AS trip_distance
    FROM
        green_taxi_data
    GROUP BY
        1
    ORDER BY
        trip_distance DESC
    LIMIT 1;
    ```
    __Answer__: 2019-09-26

* __Q5__:
    ```
    SELECT
        Z."Borough",
        SUM(total_amount) AS TOTAL
    FROM
        green_taxi_data AS T
        LEFT JOIN zones AS Z ON (z."LocationID" = T."PULocationID")
    WHERE
        DATE_TRUNC('day', T.lpep_pickup_datetime) = TO_DATE('20190918','YYYYMMDD')
        AND Z."Borough" != 'Unknown'
    GROUP BY
        Z."Borough"
    HAVING 
        SUM(total_amount) > 50000
    ORDER BY
        TOTAL DESC;
    ```
    __Answer__: "Brooklyn" "Manhattan" "Queens"

* __Q6__:

    ```
    SELECT
        DOZ."Zone",
        T.lpep_pickup_datetime,
        T.tip_amount
    FROM
        green_taxi_data AS T
        LEFT JOIN zones AS PUZ ON (PUZ."LocationID" = T."PULocationID")
        LEFT JOIN zones AS DOZ ON (DOZ."LocationID" = T."DOLocationID")
    WHERE
        PUZ."Zone" = 'Astoria'
    ORDER BY
        T.tip_amount DESC
    LIMIT 1;
    ```
    __Answer__: JFK Airport

* __Q7__:

    __Answer__: 
    ```
    (base) afarag@de-zoomcamp-instance:~/data-engineering-zoomcamp/01-docker-terraform/1_terraform_gcp/terraform/terraform_with_variables$ terraform apply   Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:   + create  Terraform will perform the following actions:    # google_bigquery_dataset.de_zoomcamp_demo_afarag will be created   + resource "google_bigquery_dataset" "de_zoomcamp_demo_afarag" {       + creation_time              = (known after apply)       + dataset_id                 = "de_zoomcamp_demo_afarag"       + default_collation          = (known after apply)       + delete_contents_on_destroy = false       + effective_labels           = (known after apply)       + etag                       = (known after apply)       + id                         = (known after apply)       + is_case_insensitive        = (known after apply)       + last_modified_time         = (known after apply)       + location                   = "US"       + max_time_travel_hours      = (known after apply)       + project                    = "data-talks-zoomcamp-afarag"       + self_link                  = (known after apply)       + storage_billing_model      = (known after apply)       + terraform_labels           = (known after apply)     }    # google_storage_bucket.terraform_demo_afarag_bucket will be created   + resource "google_storage_bucket" "terraform_demo_afarag_bucket" {       + effective_labels            = (known after apply)       + force_destroy               = true       + id                          = (known after apply)       + location                    = "US"       + name                        = "terraform_demo_afarag_bucket"       + project                     = (known after apply)       + public_access_prevention    = (known after apply)       + self_link                   = (known after apply)       + storage_class               = "STANDARD"       + terraform_labels            = (known after apply)       + uniform_bucket_level_access = (known after apply)       + url                         = (known after apply)        + lifecycle_rule {           + action {               + type = "AbortIncompleteMultipartUpload"             }           + condition {               + age                   = 1               + matches_prefix        = []               + matches_storage_class = []               + matches_suffix        = []               + with_state            = (known after apply)             }         }     }  Plan: 2 to add, 0 to change, 0 to destroy.  Do you want to perform these actions?   Terraform will perform the actions described above.   Only 'yes' will be accepted to approve.    Enter a value: yes  google_bigquery_dataset.de_zoomcamp_demo_afarag: Creating... google_storage_bucket.terraform_demo_afarag_bucket: Creating... google_bigquery_dataset.de_zoomcamp_demo_afarag: Creation complete after 2s [id=projects/data-talks-zoomcamp-afarag/datasets/de_zoomcamp_demo_afarag] google_storage_bucket.terraform_demo_afarag_bucket: Creation complete after 2s [id=terraform_demo_afarag_bucket]  Apply complete! Resources: 2 added, 0 changed, 0 destroyed. (base) afarag@de-zoomcamp-instance:~/data-engineering-zoomcamp/01-docker-terraform/1_terraform_gcp/terraform/terraform_with_variables$ 
```