## k8s Images Puller
The puller periodically pulls image(s) to k8s cluster nodes to save the time of pulling images when launching new pods.

These are the Docker Hub autobuild images located [here](https://hub.docker.com/r/locnh/k8s-puller/).

## Parameters

| Parameter | Description | Type | Default |
|-----|-----|-----|-----|
| `puller.images` | `List` of images to be pulled | `List` | `[busybox]` |
| `puller.interval` | Time interval | `String` | `60m` |
| `app.log.json` | Toggle for JSON logs | `bool` | `false` |
| `registry.username` | Username to login to the registry | `String` | `""` |
| `registry.password` | Username to login to the registry | `String` | `""` |
| `registry.server` | [Server](https://docs.docker.com/engine/reference/commandline/login/#login-to-a-self-hosted-registry) address to login to the registry | `String` | `""` |

## Usage
### Create the settings file

Create an `values.yaml` file like the following content, change the images and the interval (in minutes):
```yaml
puller:
  images:
    - alpine
    - busybox
  interval: 5m
```
These settings will tell the `puller` to pull the images [alpine](https://hub.docker.com/_/alpine/) and [busybox](https://hub.docker.com/_/busybox/) for every 5 minutes, because the tags was ommitted, then `latest` by default.

### Install with Helm
#### Add helm repo
```sh
helm repo add howdevops https://howdevops.github.io/k8s-puller
```

#### Update available charts
```sh
helm repo update
```

#### Install / Upgrade the chart
Install chart with `values.yaml` in previous step.
```sh
helm upgrade --install k8s-puller howdevops/k8s-puller -f values.yaml
```

#### Install with docker registry login
```sh
helm upgrade --install k8s-puller howdevops/k8s-puller -f values.yaml
  --set registry.username=USERNAME \
  --set registry.password=PASSWORD
```
By default, docker hub will be used to login, if you want to login to an other registry, eg: `quay.io`, you need to set the `registry.server=quay.io`
```sh
helm upgrade --install k8s-puller howdevops/k8s-puller -f values.yaml
  --set registry.username=USERNAME \
  --set registry.password=PASSWORD \
  --set registry.server=quay.io
```

**Note**: I use `upgrade --install` to install the chart if not installed, and upgrade the chart if the old version was installed.

#### Uninstall the chart
Uninstall helm RELEASE `k8s-puller` in previous step.
```sh
helm uninstall k8s-puller
```