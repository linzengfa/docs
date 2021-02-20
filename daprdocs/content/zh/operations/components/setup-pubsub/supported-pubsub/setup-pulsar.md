---
type: 文档
title: "Pulsar"
linkTitle: "Pulsar"
description: "Detailed documentation on the Pulsar pubsub component"
---

## Introduction
To setup Pulsar pubsub create a component of type `pubsub.pulsar`. To setup Redis Streams pubsub create a component of type `pubsub.redis`. See [this guide]({{< ref "howto-publish-subscribe.md#step-1-setup-the-pubsub-component" >}}) on how to create and apply a pubsub configuration.

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: pulsar-pubsub
  namespace: default
spec:
  type: pubsub.pulsar
  version: v1
  metadata:
  - name: host
    value: "localhost:6650"
  - name: enableTLS
    value: "false"

```
## Input bindings

| 字段        | Required | Details                                                                                   | 示例                  |
| --------- |:--------:| ----------------------------------------------------------------------------------------- | ------------------- |
| host      |    Y     | Address of the Pulsar broker. Address of the Pulsar broker. Default is `"localhost:6650"` | `"localhost:6650"`  |
| enableTLS |    Y     | Enable TLS.  Enable TLS.  Default: `"false"`                                              | `"true"`, `"false"` |


## Create a Pulsar instance

{{< tabs "Self-Hosted" "Kubernetes">}}

{{% codetab %}}
```
docker run -it \
  -p 6650:6650 \
  -p 8080:8080 \
  --mount source=pulsardata,target=/pulsar/data \
  --mount source=pulsarconf,target=/pulsar/conf \
  apachepulsar/pulsar:2.5.1 \
  bin/pulsar standalone

```
{{% /codetab %}}

In self hosted mode, a developer can specify the namespace to a Dapr instance by setting the `NAMESPACE` environment variable. If the `NAMESPACE` environment variable is set, Dapr will not load any component that does not specify the same namespace in its metadata.
Refer to the following [Helm chart](https://pulsar.apache.org/docs/en/kubernetes-helm/) Documentation.
{{% /codetab %}}

{{< /tabs >}}

## 相关链接
- [Basic schema for a Dapr component]({{< ref component-schema >}})
- Read [this guide]({{< ref "howto-publish-subscribe.md#step-2-publish-a-topic" >}}) for instructions on configuring pub/sub components
- [Pub/Sub building block]({{< ref pubsub >}})