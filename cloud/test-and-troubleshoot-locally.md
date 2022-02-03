---
sidebar_label: 'Test and Troubleshoot Locally'
title: 'Test and Troubleshoot Locally'
id: test-and-troubleshoot-locally
description: A guide to running an Astronomer project locally and diagnosing common problems.
---

## Overview

As you develop data pipelines on Astronomer, we strongly recommend running and testing your DAGs locally before deploying your project to a Deployment on Astronomer Cloud. This document provides information about testing and troubleshooting DAGs in a local Apache Airflow environment with the Astronomer Cloud CLI.

## Run a Project Locally

Whenever you want to test your code, the first step is always to start a local Airflow environment. To run your project in a local Airflow environment, follow the steps in [Build and Run a Project](develop-project.md#build-and-run-a-project-locally).

## View Airflow Task Logs

You can view logs for individual tasks in the Airflow UI. This is useful if you want to troubleshoot why a specific task instance failed or retried.

To access these logs:

1. Access the Airflow UI in your local Airflow environment by going to `http://localhost:8080`.
2. Open the DAG you want to troubleshoot:

    <div class="text--center">
    <img src="/img/docs/open-dag.png" alt="Access DAG from Airflow UI" />
    </div>

3. In the **Tree View**, click on the task run you want to see logs for:

    <div class="text--center">
    <img src="/img/docs/tree-view.png" alt="Task runs in the tree view" />
    </div>

4. Click **Instance Details**:

    <div class="text--center">
    <img src="/img/docs/instance-details.png" alt="Instance details button in the task log menu" />
    </div>

5. Click **Log**:

    <div class="text--center">
    <img src="/img/docs/task-log.png" alt="Log button from a task instance" />
    </div>

## Access Airflow Component Logs

To show logs for your Airflow Scheduler, Webserver, or Metadata DB locally, run the following command:

```sh
astrocloud dev logs
```

Once you run this command, the most recent logs for these components appear in your terminal window.

By default, running `astrocloud dev logs` shows logs for all Airflow components. If you want to see logs for a specific component, add any of the following flags to your command:

- `--scheduler`
- `--webserver`
- `--postgres`

To continue monitoring logs, run `astrocloud dev logs --follow`. The `--follow` flag ensures that the latest logs continue to appear in your terminal window.

## Run Airflow CLI Commands

To run [Apache Airflow CLI](https://airflow.apache.org/docs/apache-airflow/stable/cli-and-env-variables-ref.html) commands locally, run the following:

```sh
astrocloud dev run <airflow-cli-command>
```

For example, the Apache Airflow command for viewing your entire configuration is `airflow config list`. To run this command with the Astronomer Cloud CLI, you would run `astrocloud dev run config list` instead.

## Hard Reset Your Local Environment

In most cases, [restarting your local project](develop-project.md#restart-your-local-environment) is sufficient for testing and making changes to your project. However, it is sometimes necessary to kill your Docker containers and metadata DB for testing purposes. To do so, run the following command:

```sh
astrocloud dev kill
```

This command forces your running containers to stop and deletes all data associated with your local Postgres metadata database, including Airflow Connections, logs, and task history.

## Troubleshooting Common Issues

You might experience one of the following common issues when testing your Astronomer project in a local environment. This section contains information about how to approach each of these issues.

### New DAGs aren't visible in the Airflow UI

By default, the Airflow Scheduler scans the DAGs directory for new files every 300 seconds (5 minutes). For this reason, it might take a minute or two to see new DAGs appear in the Airflow UI. This is dependent on how much CPU is available to the Scheduler: the more CPU is available, the faster the Scheduler can parse your DAGs.

To have the Scheduler check for DAGs more frequently, you can set the `AIRFLOW__SCHEDULER__DAG_DIR_LIST_INTERVAL` environment variable to less than 300 seconds.

### DAGs are running slowly

If your Astronomer project contains many DAGs or tasks, then you might experience performance issues in your local Airflow environment.

To improve your environment's performance, you can scale both CPUs and memory in your Docker resources configuration. Note that increasing Docker's resource usage might decrease the overall performance of your machine.

You can also modify Airflow-level settings to improve your project's performance. For more information, read Astronomer's guide to [Scaling out Airflow](https://www.astronomer.io/guides/airflow-scaling-workers).

If your DAGs are still running slowly and you cannot scale Docker or Airflow any further, then we recommend creating a Deployment on Astronomer Cloud that's dedicated to testing.

### Astronomer project won't load after `astrocloud dev start`

If your Astronomer project fails to run in an Airflow environment more than 5 minutes after running `astrocloud dev start`, it might be because your Webserver or Scheduler is unhealthy. In this case, you might need to debug your containers. To do so:

1. After running `astrocloud dev start`, retrieve a list of running containers by running `astrocloud dev ps`.
2. If the Webserver and Scheduler containers exist but are unhealthy, check their logs by running:

    ```sh
    $ astrocloud dev logs --webserver
    $ astrocloud dev logs --scheduler
    ```

These logs should help you understand why your Webserver or Scheduler is unhealthy. Possible reasons why these containers might be unhealthy include:

    - Not enough Docker resources
    - Broken Postgres database
    - DAG code errors
    - Misconfigured Dockerfile or Docker override file
    - Misconfigured Airflow settings
