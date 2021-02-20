---
type: 文档
title: "Azure Blob Storage binding spec"
linkTitle: "Azure Blob Storage"
description: "Detailed documentation on the Azure Blob Storage binding component"
---

## Introduction

To setup Azure Blob Storage binding create a component of type `bindings.azure.blobstorage`. See [this guide]({{< ref "howto-bindings.md#1-create-a-binding" >}}) on how to create and apply a binding configuration. See [this guide]({{< ref "howto-bindings.md#1-create-a-binding" >}}) on how to create and apply a binding configuration.


```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: <NAME>
  namespace: <NAMESPACE>
spec:
  type: bindings.azure.blobstorage
  version: v1
  metadata:
  - name: storageAccount
    value: myStorageAccountName
  - name: storageAccessKey
    value: ***********
  - name: container
    value: container1
```
{{% alert title="Warning" color="warning" %}}
以上示例将 Secret 明文存储。 更推荐的方式是使用 Secret 组件， [here]({{< ref component-secrets.md >}}})。
{{% /alert %}}

## Input bindings

| 字段                                                  | Required | Output Binding Supported Operations | Details                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Example:               |
| --------------------------------------------------- |:--------:| ----------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------- |
| storageAccount                                      |    Y     | Output                              | `container` is the name of the Blob Storage container to write to.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | `"myexmapleaccount"`   |
| `storageAccessKey` is the Azure Storage access key. |    Y     | Output                              | The Blob Storage access key                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | `"access-key"`         |
| container                                           |    Y     | Output                              | The name of the Blob Storage container to write to                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | `"myexamplecontainer"` |
| decodeBase64                                        |    N     | Output                              | `decodeBase64` optional configuration to decode base64 file content before saving to Blob Storage. (In case of saving a file with binary content). "true" is the only allowed positive value. Other positive variations like "True" are not acceptable. (In case of saving a file with binary content). `"true"` is the only allowed positive value. Other positive variations like `"True"` are not acceptable. Defaults to `"false"` (In case of saving a file with binary content). `"true"` is the only allowed positive value. Other positive variations like `"True"` are not acceptable. Defaults to `"false"` | `"true"`, `"false"`    |
| getBlobRetryCount                                   |    N     | Output                              | Specifies the maximum number of HTTP GET requests that will be made while reading from a RetryReader Defaults to `"10"`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | `"1"`, `"2"`           |


## Output bindings

This component supports **output binding** with the following operations:

- `storageAccount` is the Blob Storage account name.
- `storageAccessKey` is the Blob Storage access key.

### Create Blob

To perform a create blob operation, invoke the Azure Blob Storage binding with a `POST` method and the following JSON body:

> Note: by default, a random UUID is generated. See below for Metadata support to set the name

```json
{
  "operation": "create",
  "data": {
    "field1": "value1"
  }
}
```

#### Example:

The response body will contain the value stored in the blob object.

**Note: by default, a random UUID is generated. See below for Metadata support to set the name**
Now, create or edit the run configuration for the application to be debugged. It can be found in the menu next to the `main()` function.
On Windows, utilize cmd prompt (PowerShell has different escaping mechanism)
```bash
curl -d '{ "operation": "create", "data": { "field1": "value1" }}' \
      http://localhost:<dapr-port>/v1.0/bindings/<binding-name>
```
{{% /codetab %}}

{{% codetab %}}
```bash
{
   "blobURL": "https://<your account name>. blob.core.windows.net/<your container name>/<filename>"
}
```
{{% /codetab %}}

{{< /tabs >}}

**Saving to a specific file**

{{< tabs Windows Linux >}}

{{% codetab %}}
```bash
curl -d "{ \"operation\": \"create\", \"data\": \"Hello World\", \"metadata\": { \"blobName\": \"my-test-file.txt\" } }" \
      http://localhost:<dapr-port>/v1.0/bindings/<binding-name>
```
{{% /codetab %}}

{{% codetab %}}
```bash
curl -d '{ "operation": "create", "data": "Hello World", "metadata": { "blobName": "my-test-file.txt" } }' \
      http://localhost:<dapr-port>/v1.0/bindings/<binding-name>
```
{{% /codetab %}}

{{< /tabs >}}


**Saving a file**

To upload a file, encode it as Base64 and let the Binding know to deserialize it:

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: <NAME>
  namespace: <NAMESPACE>
spec:
  type: bindings.azure.blobstorage
  version: v1
  metadata:
  - name: storageAccount
    value: myStorageAccountName
  - name: storageAccessKey
    value: ***********
  - name: container
    value: container1
  - name: decodeBase64
    value: "true"
```

Then you can upload it as you would normally:

{{< tabs Windows Linux >}}

{{% codetab %}}
```bash
curl -d "{ \"operation\": \"create\", \"data\": \"YOUR_BASE_64_CONTENT\", \"metadata\": { \"blobName\": \"my-test-file.jpg\" } }" http://localhost:<dapr-port>/v1.0/bindings/<binding-name>
```
{{% /codetab %}}

{{% codetab %}}
```bash
curl -d '{ "operation": "create", "data": "YOUR_BASE_64_CONTENT", "metadata": { "blobName": "my-test-file.jpg" } }' \
      http://localhost:<dapr-port>/v1.0/bindings/<binding-name>
```
{{% /codetab %}}

{{< /tabs >}}

#### Response

The response body will contain the following JSON:

```json
{
   "blobURL": "https://<your account name>. {
   "blobURL": "https://<your account name>. blob.core.windows.net/<your container name>/<filename>"
}

```

### Get Blob

To perform a get blob operation, invoke the Azure Blob Storage binding with a `POST` method and the following JSON body:

```json
{
  "operation": "get",
  "metadata": {
    "blobName": "myblob"
  }
}
```

#### 示例

{{< tabs Windows Linux >}}

{{% codetab %}}
```bash
curl -d '{ \"operation\": \"get\", \"metadata\": { \"blobName\": \"myblob\" }}' http://localhost:<dapr-port>/v1.0/bindings/<binding-name> 
```
{{% /codetab %}}

{{% codetab %}}
```bash
curl -d '{ "operation": "get", "metadata": { "blobName": "myblob" }}' \
      http://localhost:<dapr-port>/v1.0/bindings/<binding-name> 
```
{{% /codetab %}}

#### Response

The response body contains the value stored in the blob object.

## Metadata information

By default the Azure Blob Storage output binding will auto generate a UUID as blob filename and not assign any system or custom metadata to it. It is configurable in the Metadata property of the message (all optional). It is configurable in the metadata property of the message (all optional). It is configurable in the metadata property of the message (all optional).

Applications publishing to an Azure Blob Storage output binding should send a message with the following format:
```json
{
    "data": "file content",
    "metadata": {
        "blobName"           : "filename.txt",
        "ContentType"        : "text/plain",
        "ContentMD5"         : "vZGKbMRDAnMs4BIwlXaRvQ==",
        "ContentEncoding"    : "UTF-8",
        "ContentLanguage"    : "en-us",
        "ContentDisposition" : "attachment",
        "CacheControl"       : "no-cache",
        "Custom"             : "hello-world",
    },
    "operation": "create"
}
```

## Related links

- [Basic schema for a Dapr component]({{< ref component-schema >}})
- [Bindings building block]({{< ref bindings >}})
- [如何通过 input binding 触发应用]({{< ref howto-triggers.md >}})
- [How-To：使用绑定与外部资源进行交互]({{< ref howto-bindings.md >}})
- [绑定API 参考]({{< ref bindings_api.md >}})