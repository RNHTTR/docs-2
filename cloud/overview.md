---
sidebar_label: 'Overview'
title: 'Astronomer Cloud Documentation'
id: overview
slug: /
description: Learn about Astronomer Cloud, the managed service that makes data pipelines easy to write, run, and monitor.
---

## Overview

Astronomer Cloud is a managed software service that offers a next-generation experience for modern data teams running Apache Airflow, the open source industry standard for data orchestration.

Astronomer Cloud boasts a hybrid deployment model founded on a Control Plane hosted by Astronomer and a Data Plane that is hosted in your cloud environment. Both are fully managed by Astronomer. This model offers the self-service convenience of a fully managed service while respecting the need to keep data private, secure, and within corporate boundaries. It optimizes for security whilst relieving your team of operational overhead.

Beyond architecture, Astronomer Cloud offers a suite of first-class features that make it easy to author, run, and monitor data pipelines.

## Features

Astronomer Cloud's architecture enables a few key features, available today:

- Worker auto-scaling, powered by Airflow's Celery Executor + KEDA
- The ability to run multiple clusters in your organization's network on AWS
- Astronomer Runtime, a collection of Docker images that provides a differentiated data orchestration experience
- Timely support for the latest major, minor, and patch versions of the Apache Airflow open source project
- Support for role-based access control (RBAC) and single sign-on (SSO) for secure user management and authentication
- An observability experience that gives you insight into the health and resource consumption of your tasks and pipelines in real time

## Architecture

The following diagram outlines how the control plane, data plane, and users are connected to enable these features:

<div class="text--center">
  <img src="/img/docs/architecture-overview.png" alt="High level overview of Astronomer Cloud's architecture" />
</div>

## Get Started

The Astronomer team will schedule an onboarding session for your initial install. From there, we recommend reading through the following docs:

- [Install the Astronomer CLI](install-cli)
- [Develop Project](develop-project)
- [Configure your Deployment](configure-deployment.md)

If you have a feature request or a bug to report, reach out to [Astronomer Support](https://support.astronomer.io). We're here to help.

To check on the operational status of Astronomer Cloud, visit our [status page](https://cloud-status.astronomer.io). You can subscribe to updates by clicking on **Subscribe to Updates** on the top-right of the page and entering your email address.