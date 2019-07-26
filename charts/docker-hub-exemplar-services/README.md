# Exemplar Services Docker Hub Helm Chart

This is a Helm chart for Kubernetes that that installs the `media-store` and `audio-processor` image from [Docker Hub](https://hub.docker.com).

In this example we are using a free Docker Hub account, so only the `audio-processor` is using a private repository. The `media-store` image is public so will not require any authentication.

## Table of contents

- [Exemplar Services Docker Hub Helm Chart](#exemplar-services-docker-hub-helm-chart)
  - [Table of contents](#table-of-contents)
  - [Installing the Helm chart on a local kubernetes cluster](#installing-the-helm-chart-on-a-local-kubernetes-cluster)
    - [Prequisites](#prequisites)
    - [Install Helm](#install-helm)
    - [Initialise Helm and install Tiller](#initialise-helm-and-install-tiller)
    - [Install this Helm chart](#install-this-helm-chart)
      - [Add the Docker Hub credentials](#add-the-docker-hub-credentials)
      - [Install the image](#install-the-image)
      - [Manage the Helm chart](#manage-the-helm-chart)

## Installing the Helm chart on a local kubernetes cluster

### Prequisites

1. Kubernetes installed locally. See [our installation guide](https://github.com/bbc/rd-nbe-lab/blob/master/docs/guides/guide-003-using-kubernetes-locally.md).
2. You'll need the `nbelabsdocker` Docker Hub account login credentials. You can ask for these in the `tda-sprints` Slack channel.

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

#### Add the Docker Hub credentials

To access the private repository, the Docker Hub login credentials need to be added as a kubernetes [secret](https://kubernetes.io/docs/concepts/configuration/secret). These are then read by the `Deployment` using the `imagePullSecrets` property (see the [deployment-audio-proceessor.yaml](./..exemplar-services/helm-charts/docker-hub-exemplar-services/templates/deployment-audio-processor.yaml)).

To do this, the Docker hub credentials will need to be added to the `secrets.dockerconfigjson` property within the `exemplar-services/helm-charts/docker-hub-exemplar-services/values.yaml` file.

To get the details, login to Docker hub:

```bash
docker login

# You'll be prompted for a username and password:
# - Username: nbelabsdocker
# - Password: Ask in the `tda-sprints` Slack channel
```

Use `kubectl` to generate the secret value:

```bash
kubectl create secret generic dockerhub-cred \
  --from-file=.dockerconfigjson=<path/to/.docker/config.json> \
  --type=kubernetes.io/dockerconfigjson --dry-run=true --output=yaml
```

> You config path will be:
>
> - **macOS:** /Users/YOUR_MACOS_USERNAME/.docker/config.js
> - **Linux:** /etc/docker/config.json

Copy the `data.dockerconfigjson` value from the output. It will be a really long string with characters like "ewoJImF1dGhzIjognZMblZyT2tGTFEiCm0="

Then paste that copied value into:

**exemplar-services/helm-charts/docker-hub-exemplar-services/values.yaml**

```yaml
# content omitted...

secrets:
  dockerconfigjson: PASTE_THE_DOCKERCONFIGJSON_VALUE_HERE
```

> **Do not commit this value to github!**

#### Install the image

Now everything is setup, you can install the Helm chart with:

```bash
# Navigate to the chart directory
cd exemplar-services/heml-charts

# Install the chart
helm install docker-hub-exemplar-services
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
