# Grafana LGTM (Loki Grafana Tempo Mimir) Stack

## Grafana Loki

### Deploying the Helm chart for development and testing

**Step 1:** Add Grafana’s chart repository to Helm:

```
helm repo add grafana https://grafana.github.io/helm-charts
```

**Step 2:** Update the chart repository:

```
helm repo update
```

**Step 3:** Create the configuration file `values-loki.yaml`.

```
loki:
  auth_enabled: false
  commonConfig:
    replication_factor: 1

  storage:
    bucketNames:
      chunks: loki-chunks
      ruler: loki-ruler
    type: s3
    s3:
      endpoint: http://minio-local:9001
      region: us-east-1
      secretAccessKey: <secretAccessKey of Minio>
      accessKeyId: <accessKeyId of Minio>
      s3ForcePathStyle: true
      insecure: true

  schemaConfig:
    configs:
      - from: 2024-04-01
        store: tsdb
        object_store: s3
        schema: v13
        index:
          prefix: index_
          period: 24h
        chunks:
          period: 24h

read:
  replicas: 1
  persistence:
    storageClass: standard

write:
  replicas: 1
  persistence:
    storageClass: standard

backend:
  replicas: 1
  persistence:
    storageClass: standard

```


