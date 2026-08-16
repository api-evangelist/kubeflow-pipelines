---
title: "Batch Jobs for SparkClient: Submitting and Managing Spark Workloads from Python"
url: "https://blog.kubeflow.org/sdk/spark-batch-jobs/"
date: "2026-07-25"
author: "Sameer Yadav"
feed_url: "https://blog.kubeflow.org/feed"
---
As part of GSoC 2026, we’ve been extending SparkClient ( KEP-107 ) — the Kubeflow SDK’s Python interface for running Spark on Kubernetes — with support for batch job submission and lifecycle management. Previously, SparkClient covered interactive workloads well via connect() , but running a batch job (the “submit a script, walk away, come back to results” kind of work that powers ETL pipelines and scheduled data prep) meant working with the SparkApplication CRD directly. This post walks through how the new submit_job() API and its accompanying lifecycle APIs work, what’s happening on the clust
