# acp-on-k8s

## Overview

This repository's primary purpose is to enable you to rapidly stand up Cloudentity Authorization Control Plane in your local development environment. It takes advantage of a [Kind](https://kind.sigs.k8s.io) a tool for running local Kubernetes clusters using Docker container nodes as well as [ACP helm charts](https://charts.cloudentity.io).

## Prerequisites

* [kind](https://kind.sigs.k8s.io/docs/user/quick-start/#installation) 0.16.x 
* [helm](https://helm.sh/docs/intro/install/) 3.x.x 
* [kubectl](https://kubernetes.io/docs/tasks/tools/) 1.24.x
* Credentials to access Cloudentity Private Docker Repo - if you are our client, you can find it in your Support Portal; if you are not and you want to check out our product, feel free to request access via our [website](https://cloudentity.com).

## Quickstart
1. For `kind` tool please follow the instructions below to install globally. For the rest tools please follow the official instructions linked above.

On Linux:
```
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.16.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
```
On macOS:
```
brew install kind
```
2. Add environment variables `DOCKER_USER` and `DOCKER_PWD` for Cloudentity Private Docker Repo:
```
export DOCKER_USER=<user>
export DOCKER_PWD=<password>
```
3. Add local domain to `/etc/hosts`:
``` sh
127.0.0.1 acp.acp-system
```
4. Run `make all`.
5. Next go to `https://acp.acp-system:8443/` and log in with `admin`:`admin`

## What is next?

* Check out ACP [documentation](http://docs.authorization.cloudentity.com)
* Integrate your SPA app using our [Auth JS Lib](https://github.com/cloudentity/cloudentity-auth-js)
* Explore strong service mTLS based authentication using [Sample Go mTLS OAuth app](https://github.com/cloudentity/sample-go-mtls-oauth-client)

## Makefile reference

* `make all` - Create a new local Kubernetes cluster and install all ACP components. Runs `create-cluster`, `prepare-helm`, `prepare-cluster` and `install-acp-stack`
* `make create-cluster` -  Creates a new local Kubernetes cluster using configuration stored in `./config/kind-cluster-config.yaml`
* `make prepare-helm` -  Adds necessary helm chart repositories
* `make prepare-cluster` - Creates required namespaces and initializes the docker credentials stored in .env file (please make sure you have `DOCKER_USER` and `DOCKER_PWD` correctly configured in the .env file) 
* `make install-acp-stack` - Installs the acp stack using the [ACP helm charts](https://charts.cloudentity.io) and the `acp` release name
* `make watch` - Checks the status of the deployment 
* `make uninstall-acp` - Uninstalls the `acp` release
* `make delete-cluster` - Deletes Kind based Kubernetes cluster

If you would like to test your modifications of the [ACP helm charts](https://charts.cloudentity.io), point `ACP_CHARTS_PATH` to your clone:

* `make ACP_CHARTS_PATH=../acp-helm-charts/charts all`

This is also a handy way to spin up older releases of ACP.

## Running OpenBanking quickstart

1. Add to `values/kube-acp-stack.yaml` following feature flags to ACP: 
```
integration_endpoints: true
system_openbanking_consents_management: true
openbanking_domestic_payment_consents: true
system_clients_management: true
```
2. Run `make install-openbanking`
