[![Community Extension](https://img.shields.io/badge/Community%20Extension-An%20open%20source%20community%20maintained%20project-FF4700)](https://github.com/camunda-community-hub/community)
[![](https://img.shields.io/badge/Lifecycle-Incubating-blue)](https://github.com/Camunda-Community-Hub/community/blob/main/extension-lifecycle.md#incubating-)

# Camunda 8 Greenfield Installation

This project contains Makefiles that help to quickly create a Camunda 8 self-managed Kubernetes environment

# Prerequisites

Complete the following steps regardless of which cloud provider you use.  

1. Verify `kubectl` is installed

       kubectl --help

2. Verify `helm` is installed. Helm version must be at least `3.7.0`

       helm version

3. Clone the following 2 repositories into the same parent directory. For example, create a directory named `camunda`, 
   and `cd` into it. 
   
   - Clone this [Camunda 8 Greenfield Installation git repository](https://github.com/camunda-community-hub/camunda8-greenfield-installation)
   - Also clone the [Zeebe Helm Profiles git repository](https://github.com/camunda-community-hub/zeebe-helm-profiles)

At this point, you should have the following directory structure:

     camunda
      |
      +-- camunda8-greenfield
      |
      +-- zeebe-helm-profiles

The next step is to create a Kubernetes Cluster on the provider of your choice.

# Create Kubernetes Cluster

## Microsoft Azure

1. Verify that the `az` cli tool is installed (https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)

       $ az version
       {
        "azure-cli": "2.38.0",
        "azure-cli-core": "2.38.0",
        "azure-cli-telemetry": "1.0.6",
        "extensions": {}
       }

2. The next step is to make sure you are authenticated. If you don't already have one, you'll need to sign up for a new
   Azure Account. Then, run the following command and then follow the instructions to authenticate via your browser.

       $ az login

> :information_source: **Tip** If you're using SSO, first, open a browser and sign in to your Azure/Microsoft account.
> Then try doing the `az login` command again.

3. Use the Azure-specific `Makefile` to create the cluster

`cd` into the `azure` directory

Update the following bash variables so they are appropriate for your specific environment. 

     resource-group ?= <RESOURCE GROUP>
     node-resource-group ?= <NODE RESOURCE GROUP>
     clustername ?= <CLUSTER NAME>
     region ?= <REGION>
     machine-type ?= <MACHINE TYPE>

Run `make` to create an Azure Kubernetes cluster

4. Run `make use-k8s` to make sure that your local `kubectl` environment is configured to connect to the new cluster.

5. Run `make urls` to see which url to use in order to manage your Azure Kubernetes cluster

## Google Compute Engine

1. Verify `gcloud` is installed (https://cloud.google.com/sdk/docs/install-sdk)

       gcloud --help
       # Google also requires the following plugin as of 2022
       gcloud components install gke-gcloud-auth-plugin

2. The next step is to make sure you are authenticated. If you don't already have one, you'll need to sign up for a new
   Google Cloud Account. Then, run the following command and then follow the instructions to authenticate via your browser.

       $ gcloud auth login

3. Use the Google-specific `Makefile` to create a GKE cluster

`cd` into the `azure` directory

Edit the `./google/Makefile` and set the following bash variables so that they are appropriate for your specific environment.

     resource-group ?= <RESOURCE GROUP>
     node-resource-group ?= <NODE RESOURCE GROUP>
     clustername ?= <CLUSTER NAME>
     region ?= <REGION>
     machine-type ?= <MACHINE TYPE>

Run `make` to create an Google Kubernetes cluster

4. Run `make use-k8s` to make sure that your local `kubectl` environment is configured to connect to the new cluster.

5. Run `make urls` to see which url to use in order to manage your GKE cluster

## Amazon Web Services

TODO: need to document steps to create k8s cluster on Google Cloud

# Installing Camunda Environment

TODO: need to finish documenting this section

At this point, you have a Kubernetes Cluster on a cloud provider of your choice. Follow the steps below to start a 
Camunda environment. 

Place a new bpmn file into the `models` directory. For example, see `camunda-benchmark/gcp/camunda/process-8-service-tasks.bpmn`.

Change directory to the `camunda-benchmark/gcp/camunda` directory.

Run `make deploy-models`

If things go wrong you can delete the pods and jobs from kubernetes

Run `make clean-deploy-models`

# Running Benchmark

TODO: need to finish documenting this section

Change directory to the `camunda-benchmark/gcp/camunda` directory.

Run `make benchmark`

If things go wrong you can delete the pods and jobs from kubernetes

Run `make clean-benchmark`

# Cleaning Up

TODO: need to finish documenting this section

You will need to clean up the environments. These can cost quite a lot of money if you leave them running

Run `make clean` in the camunda directory to completely delete the deployments and other kube resources.




