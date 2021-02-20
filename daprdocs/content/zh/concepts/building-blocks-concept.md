---
type: docs
title: "构建块"
linkTitle: "构建块"
weight: 200
description: "可通过标准 HTTP 或 gRPC API 访问的模块化最佳实践"
---

[构建块]({{< ref building-blocks >}}) 是可从代码调用并使用一个或多个 Dapr 组件的 HTTP 或 gRPC API 。

构建块可以解决构建弹性，微服务应用程序和编纂最佳实践和模式方面的共同挑战。 Dapr 由一组构建块组成，并具备添加新构建块的扩展性。

下图显示了构建块如何公开了可被代码调用的公共 API ，并使用组件来实现构建块的能力。

<img src="/images/concepts-building-blocks.png" width=250>

以下是 Dapr 提供的构建块类型:

<img src="/images/building_blocks.png" width=1000>

| 构建块                    | 端点                                | 描述                                                                                                                        |
| ---------------------- | --------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| [**服务间调用**]({{X1X}})  | `/v1.0/invoke`                    | 服务调用使应用程序能够通过 Http 或 gRPC 消息形式相互通信。 Dapr 允许您通过一个组合了反向代理与内置服务发现的端点来克服这些挑战，同时能够利用内置的分布式跟踪，度量，错误处理等能力。                       |
| [**状态管理**]({{X6X}})   | `/v1.0/state`                     | 应用程序状态是应用程序想要保留在单个会话之外的任何内容。 Dapr 提供基于键 / 值的状态 API ，使用可插拔的状态存储进行持久化。                                                      |
| [**发布订阅**]({{X11X}})  | `/v1.0/publish` `/v1.0/subscribe` | 发布/预订是松散耦合的消息传递模式，发送方 (或发布者) 将消息推送到订阅者预订的主题。 Dapr 支持应用程序之间的 pub/sub 模式。                                                   |
| [**资源绑定**]({{X18X}})  | `/v1.0/bindings`                  | 绑定提供一个外部云与本地服务或系统的双向连接。 Dapr 允许您通过 Dapr 绑定 API 调用外部服务，也可以通过已连接的服务发送的事件来触发应用程序。                                            |
| [**Actor**]({{X23X}}) | `/v1.0/actors`                    | 参与者是孤立的独立计算单元，具有单线程执行。 Dapr 根据虚拟参与者模式提供参与者实现，该模式提供单线程编程模型，并且参与者在未使用时收集垃圾。 请参阅 * [Actor 概述](./actors#understanding-actors) |
| [**可观察性**]({{X29X}})  | `N/A`                             | Dapr 系统组件和运行时发出度量，日志和跟踪以调试，操作和监视 Dapr 系统服务，组件和用户应用程序。                                                                     |
| [**密钥**]({{X34X}})    | `/v1.0/secrets`                   | Dapr 提供一个机密构建块 API ，并与 Azure Key Vault 和 Kubernetes 等机密商店集成，以存储机密。 服务代码可以调用秘密 API 从 Dapr 支持的秘密商店中检索秘密。                    |