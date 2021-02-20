---
type: 文档
title: "Azure Service Bus Queues binding spec"
linkTitle: "Azure Service Bus Queues"
description: "Detailed documentation on the Azure Service Bus Queues binding component"
---

## Introduction

To setup Azure Service Bus Queues binding create a component of type `bindings.azure.servicebusqueues`. See [this guide]({{< ref "howto-bindings.md#1-create-a-binding" >}}) on how to create and apply a binding configuration.

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: <NAME>
  namespace: <NAMESPACE>
spec:
  type: bindings.azure.servicebusqueues
  version: v1
  metadata:
  - name: connectionString
    value: "Endpoint=sb://************"
  - name: queueName
    value: queue1
  - name: ttlInSeconds
    value: 60
```

{{% alert title="Warning" color="warning" %}}
以上示例将 Secret 明文存储。 更推荐的方式是使用 Secret 组件， [here]({{< ref component-secrets.md >}}})。
{{% /alert %}}

## Input bindings

| 字段                                                       | Required | Output Binding Supported Operations | Details                                                                                                                                                                                                                                                                                                                                     | 示例:                            |
| -------------------------------------------------------- |:--------:| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------ |
| `connectionString` is the Service Bus connection string. |    Y     | Input/Output                        | The Service Bus connection string                                                                                                                                                                                                                                                                                                           | `"Endpoint=sb://************"` |
| queueName                                                |    Y     | Input/Output                        | `queueName` is the Service Bus queue name.                                                                                                                                                                                                                                                                                                  | `"queuename"`                  |
| ttlInSeconds                                             |    N     | Output                              | `ttlInSeconds` is an optional parameter to set the default message [time to live](https://docs.microsoft.com/azure/service-bus-messaging/message-expiration). If this parameter is omitted, messages will expire after 14 days. If this parameter is omitted, messages will expire after 14 days. See [also](#specifying-a-ttl-per-message) | `"60"`                         |

## Output bindings

For input bindings, where the query matching Tweets are streamed to the user service, the above component has to also include a query:

字段名为 `ttlInSeconds`。

- `create`

## 输出绑定支持的操作

可以在队列级别 ( 如上所述) 或消息级别定义生存时间。 在消息级别定义的值会覆盖在队列级别设置的任何值。

若要设置在消息级别生存的时间，请使用 `metadata` 请求正文中的元数据部分。

字段名为 `ttlInSeconds`。

{{< tabs "Linux">}}

Now, add the program arguments and environment variables. These need to match the ports defined in the entry in 'External Tool' above.

```shell
curl -X POST http://localhost:3500/v1.0/bindings/myServiceBusQueue \
  -H "Content-Type: application/json" \
  -d '{
        "data": {
          "message": "Hi"
        },
        "metadata": {
          "ttlInSeconds": "60"
        },
        "operation": "create"
      }'
```
{{% /codetab %}}

{{< /tabs >}}

## Related links

- [Basic schema for a Dapr component]({{< ref component-schema >}})
- [Bindings building block]({{< ref bindings >}})
- [如何通过 input binding 触发应用]({{< ref howto-triggers.md >}})
- [How-To：使用绑定与外部资源进行交互]({{< ref howto-bindings.md >}})
- [绑定API 参考]({{< ref bindings_api.md >}})