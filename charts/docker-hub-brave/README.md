# Exemplar Services Docker Hub Helm Chart

This is a Helm chart for Kubernetes that installs the `brave` image from [Docker Hub](https://hub.docker.com).

The `brave` image is public, so will not require any authentication.

## Table of contents

- [Exemplar Services Docker Hub Helm Chart](#exemplar-services-docker-hub-helm-chart)
  - [Table of contents](#table-of-contents)
  - [Installing the Helm chart on a local kubernetes cluster](#installing-the-helm-chart-on-a-local-kubernetes-cluster)
    - [Prequisites](#prequisites)
    - [Install Helm](#install-helm)
    - [Initialise Helm and install Tiller](#initialise-helm-and-install-tiller)
    - [Install this Helm chart](#install-this-helm-chart)
      - [Install the image](#install-the-image)
      - [Manage the Helm chart](#manage-the-helm-chart)

## Installing the Helm chart on a local kubernetes cluster

### Prequisites

Kubernetes installed locally. See [our installation guide](https://github.com/bbc/rd-nbe-lab/blob/master/docs/guides/guide-003-using-kubernetes-locally.md).

### Install Helm

```bash
# For macOS
brew install kubernetes-helm
```

> If you're not using macOS, see https://helm.sh/docs/using_helm/#installing-helm

### Initialise Helm and install Tiller

Once you have Helm ready, you can initialise the local CLI and also install Tiller into your Kubernetes cluster with:

```bash
helm init --history-max 200
```

> Read more about installing `Tiller` and the `history-max` flag at https://helm.sh/docs/using_helm/#quickstart

### Install this Helm chart

#### Install the image

Now everything is setup, you can install the Helm chart with:

```bash
# Navigate to the chart directory from the repo root
cd charts

# Install the chart
helm install docker-hub-brave
```

#### Manage the Helm chart

You can see installed Helm charts with:

```bash
helm list
```

Or uninstall with:

```bash
helm delete [HELM ID]

# e.g.
# helm delete ignorant-dog
```
