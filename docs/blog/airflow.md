# Airflow

### Introduction

**Apache Airflow** (or simply Airflow) is a platform to programmatically author, schedule, and monitor workflows.

You could orchestrate the task as [Directed Acyclic Graph](https://en.wikipedia.org/wiki/Directed_acyclic_graph)

``` py title="data_orchestration.py"
from airflow import models

from airflow.hooks.base import BaseHook
from airflow.providers.google.cloud.operators.bigquery import BigQueryCheckOperator
from airflow.providers.google.cloud.operators.dataflow import DataflowTemplatedJobStartOperator
from airflow.providers.google.cloud.sensors.gcs import GCSObjectExistenceSensor
from airflow.providers.slack.operators.slack_webhook import SlackWebhookOperator

from airflow.utils.dates import days_ago
from airflow.utils.state import State

# Sample data
BUCKET_NAME = "cloud-samples-data/composer/data-orchestration-blog-example"
DATA_FILE_NAME = "bike_station_data.csv"

# Assumes existence of the following Airflow Variables
PROJECT_ID = models.Variable.get("gcp_project")
DATASET = models.Variable.get("bigquery_dataset")
TABLE = models.Variable.get("bigquery_table")


# Slack error notification example taken from Kaxil Naik's blog on Slack Integration:
# https://medium.com/datareply/integrating-slack-alerts-in-airflow-c9dcd155105
def on_failure_callback(context):
    ti = context.get("task_instance")
    slack_msg = f"""
            :red_circle: Task Failed.
            *Task*: {ti.task_id}
            *Dag*: {ti.dag_id}
            *Execution Time*: {context.get('execution_date')}
            *Log Url*: {ti.log_url}
            """
    slack_webhook_token = BaseHook.get_connection("slack_connection").password
    slack_error = SlackWebhookOperator(
        task_id="post_slack_error",
        http_conn_id="slack_connection",
        channel="#airflow-alerts",
        webhook_token=slack_webhook_token,
        message=slack_msg,
    )
    slack_error.execute(context)


with models.DAG(
    "dataflow_to_bq_workflow",
    schedule_interval=None,
    start_date=days_ago(1),
    default_args={"on_failure_callback": on_failure_callback},
) as dag:

    validate_file_exists = GCSObjectExistenceSensor(
        task_id="validate_file_exists", bucket=BUCKET_NAME, object=DATA_FILE_NAME
    )

    # See Launching Dataflow pipelines with Cloud Composer tutorial for further guidance
    # https://cloud.google.com/composer/docs/how-to/using/using-dataflow-template-operator
    start_dataflow_job = DataflowTemplatedJobStartOperator(
        task_id="start-dataflow-template-job",
        job_name="csv_to_bq_transform",
        template="gs://dataflow-templates/latest/GCS_Text_to_BigQuery",
        parameters={
            "javascriptTextTransformFunctionName": "transform",
            "javascriptTextTransformGcsPath": f"gs://{BUCKET_NAME}/udf_transform.js",
            "JSONPath": f"gs://{BUCKET_NAME}/bq_schema.json",
            "inputFilePattern": f"gs://{BUCKET_NAME}/{DATA_FILE_NAME}",
            "bigQueryLoadingTemporaryDirectory": f"gs://{BUCKET_NAME}/tmp/",
            "outputTable": f"{PROJECT_ID}:{DATASET}.{TABLE}",
        },
    )

    execute_bigquery_sql = BigQueryCheckOperator(
        task_id="execute_bigquery_sql",
        sql=f"SELECT COUNT(*) FROM `{PROJECT_ID}.{DATASET}.{TABLE}`",
        use_legacy_sql=False,
    )

    validate_file_exists >> start_dataflow_job >> execute_bigquery_sql


if __name__ == "__main__":
    dag.clear(dag_run_state=State.NONE)
    dag.run()
```

### Approach
[Astronomer](https://www.astronomer.io/)

Astronomer is the commercial developer of Apache Airflow, a community-driven open-source tool that's leading the market in data orchestration. We're a globally-distributed and rapidly growing venture-backed team of learners, innovators and collaborators.

[Cloud Composer](https://cloud.google.com/composer)

Cloud Composer is a fully managed workflow orchestration service built on Apache Airflow in Google Cloud Platform

[Amazon Managed Workflows for Apache Airflow(MWAA)](https://aws.amazon.com/managed-workflows-for-apache-airflow/)

Amazon Managed Workflows for Apache Airflow (MWAA) is a managed orchestration service for Apache Airflow1 that makes it easier to set up and operate end-to-end data pipelines in the cloud at scale. Apache Airflow is an open-source tool used to programmatically author, schedule, and monitor sequences of processes and tasks referred to as “workflows.” in Amazon Web Service.

