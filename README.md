# dt-otel-collector-tutorial

## Create an Access Token

- Press `Ctrl + k` to bring up the search box.
- Type `access tokens` and select the first option
- Create an access token with these permissions: `openTelemetryTrace.ingest`, `metrics.ingest` and `logs.ingest`
- Click `Generate token` and save the token

## Create a Kubernetes secret

Use `kubectl` to create a `Secret` to store your Dynatrace connection details.

Substitute your values into the following command and execute it.

Note: Do not change the name. Leave as `dynatrace-otelcol-dt-api-credentials`

```
kubectl create secret generic dynatrace-otelcol-dt-api-credentials \
--from-literal=DT_ENDPOINT=https://abc12345.live.dynatrace.com \
--from-literal=DT_API_TOKEN=dt0c01.sample.secret
```

You should see this: `secret/dynatrace-otelcol-dt-api-credentials created`

## Add OpenTelemetry Helm charts and Update

Copy and paste the following to add the OpenTelemetry Helm chart and update it to the latest versions.

```
helm repo add open-telemetry https://open-telemetry.github.io/opentelemetry-helm-charts
helm repo update
```

## Configure and Install Dynatrace OpenTelemetry Collector

The OpenTelemetry collector requires a configuration file. This is already available in the environment. See collector-values.yaml

You do not need to modify this file.

Install the collector by copy and pasting this content:

```
helm upgrade -i dynatrace-collector open-telemetry/opentelemetry-collector -f collector-values.yaml
```

After Helm indicates it has installed, run the following command and you should see a pod either `Pending` or `Running`.

```
kubectl get pods
```

Wait and periodically re-run `kubectl get pods` until the Pod is `Running`.
