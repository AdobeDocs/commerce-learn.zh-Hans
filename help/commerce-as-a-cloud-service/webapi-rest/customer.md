---
title: 探索新的客户REST API
description: 了解如何在Adobe Commerce云服务中使用新的客户REST API。 非常适合架构师和开发人员。
feature: REST, Customers, Saas
topic: Development, Integrations
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 225
last-substantial-update: 2026-01-27T00:00:00Z
jira: KT-20160
source-git-commit: 9e644b4dac87eeb98c9e97c7a931a460e1ef3b81
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 0%

---


# ACCS — 新客户REST API

了解如何在Adobe Commerce as a Cloud Service中使用新的客户REST API。 本教程非常适合于希望有效集成和优化API解决方案的架构师和开发人员。

## 此视频面向谁？

* 负责构建与Adobe Commerce集成的后端开发人员
* 技术架构师为Headless商务实施设计客户管理工作流

## 视频内容

* 使用服务器到服务器凭据通过Adobe IMS进行身份验证以获取API请求的访问令牌
* 对Commerce as a Cloud Service使用正确的REST API端点格式
* 通过适当的JSON负载，使用POST和PUT请求以编程方式创建和更新客户帐户

>[!VIDEO](https://video.tv.adobe.com/v/3479361/?learn=on&enablevpops)

## 代码示例

开始之前，请从[Experience Cloud](https://experience.adobe.com)和[Adobe Developer Console](https://developer.adobe.com/console)收集所有必需的值。 准备这些值可以确保顺利的设置过程。

>[!NOTE]
>
>确保您在正确的组织工作。 您的组织选择会影响哪些实例和环境在Experience Cloud和Developer Console中均可见。

### 实例详细信息 — experience.adobe.com

实例详细信息包含实例ID、GraphQL端点、凭据等内容。

### 开发人员详细信息 — https://developer.adobe.com/console/

在Developer Console中，您可以管理API凭据，包括客户端ID、客户端密钥和访问令牌。 您还可以创建新的凭据类型，如服务器到服务器或本机应用程序身份验证。

## 先决条件

| 项目 | 值 | 其中是此值 |
|--- |--- |--- |
| 实例Id | `<instance_id>` | experience.adobe.com |
| REST端点 | `<rest_endpoint>` | experience.adobe.com |
| 客户端ID | `<client_id>` | developer.adobe.com/console |
| 客户端密码 | `<client_secret>` | developer.adobe.com/console |


## 步骤1：获取访问令牌（服务器到服务器身份验证）

>[!IMPORTANT]
>
> 此示例中显示的变量无效。 使用项目凭据中的&lt;client_id>和&lt;client_secret>。

```bash
curl -X POST 'https://ims-na1.adobelogin.com/ims/token/v3' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'grant_type=client_credentials&client_id=<client_id>&client_secret=<client_secret>&scope=openid,AdobeID,email,additional_info.projectedProductContext,profile,commerce.aco.ingestion,commerce.accs,org.read,additional_info.roles'
```

**示例响应：**

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIs...",
  "token_type": "bearer",
  "expires_in": 86399
}
```

## 步骤2：创建客户

>[!IMPORTANT]
>
> 此示例中提供的URL无效。 使用REST基本URL。 将“&lt;rest_endpoint>”与您的URL交换。 它类似于此`https://na1-sandbox.api.commerce.adobe.com/AbCYab34cdEfGHiJ27123`。
>
> 此端点未将/rest/作为URL的一部分。 包含此项会导致出错。

**终结点：** `POST /V1/customers`

```bash
curl -X POST \
  "<rest_endpoint>/V1/customers" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <ACCESS_TOKEN>" \
  -d '{
    "customer": {
      "email": "john.doe@example.com",
      "firstname": "John",
      "lastname": "Doe",
      "store_id": 1,
      "website_id": 1
    },
    "password": "TempPa55word!"
  }'
```

**响应：**

```json
{
  "id": 5,
  "group_id": 1,
  "created_at": "2026-01-23 20:40:15",
  "updated_at": "2026-01-23 20:40:15",
  "created_in": "Default Store View",
  "email": "john.doe@example.com",
  "firstname": "John",
  "lastname": "Doe",
  "store_id": 1,
  "website_id": 1,
  "addresses": [],
  "disable_auto_group_change": 0
}
```

## 步骤3：更新客户

>[!IMPORTANT]
>
> 此示例中提供的URL无效。 使用REST基本URL。 将“&lt;rest_endpoint>”与您的URL交换。 它类似于此`https://na1-sandbox.api.commerce.adobe.com/AbCYab34cdEfGHiJ27123`。

以下示例中的数字`5`是以前使用POST `"id": 5,`创建的客户的ID。 请确保将`5`更改为您的请求中返回的任何一个ID。

**终结点：** `PUT /V1/customers/{customerId}`

```bash
curl -X PUT \
  "<rest_endpoint>/V1/customers/5" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <ACCESS_TOKEN>" \
  -d '{
    "customer": {
      "id": 5,
      "email": "john.doe@example.com",
      "firstname": "John",
      "lastname": "Doe-Updated"
    }
  }'
```

**响应：**

```json
{
  "id": 5,
  "group_id": 1,
  "created_at": "2026-01-23 20:40:15",
  "updated_at": "2026-01-23 20:40:30",
  "created_in": "Default Store View",
  "email": "john.doe@example.com",
  "firstname": "John",
  "lastname": "Doe-Updated",
  "store_id": 1,
  "website_id": 1,
  "addresses": []
}
```

## 完整脚本（多合一）

>[!IMPORTANT]
>
> 此示例中显示的变量无效。 使用项目凭据中的客户端ID和客户端密钥。 使用REST基本URL。 从experience.adobe.com将“&lt;rest_endpoint>”与您的REST端点URL交换。 它类似于此`https://na1-sandbox.api.commerce.adobe.com/AbCDefGHiJ1234567`。

```bash
#!/bin/bash

# Configuration be sure to update these with your projects unique values
CLIENT_ID="<client_id>"
CLIENT_SECRET="<client_secret>"
REST_ENDPOINT="<rest_endpoint>"

# Step 1: Get Access Token
echo "Getting access token..."
ACCESS_TOKEN=$(curl -s -X POST 'https://ims-na1.adobelogin.com/ims/token/v3' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d "grant_type=client_credentials&client_id=${CLIENT_ID}&client_secret=${CLIENT_SECRET}&scope=openid,AdobeID,email,additional_info.projectedProductContext,profile,commerce.aco.ingestion,commerce.accs,org.read,additional_info.roles" | jq -r '.access_token')

echo "Token obtained: ${ACCESS_TOKEN:0:50}..."

# Step 2: Create Customer
echo ""
echo "Creating customer..."
CREATE_RESPONSE=$(curl -s -X POST \
  "${REST_ENDPOINT}/V1/customers" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer ${ACCESS_TOKEN}" \
  -d '{
    "customer": {
      "email": "john.doe@example.com",
      "firstname": "John",
      "lastname": "Doe",
      "store_id": 1,
      "website_id": 1
    },
    "password": "TempPa55word!"
  }')

echo "Create Response:"
echo "$CREATE_RESPONSE" | jq .

# Extract customer ID
CUSTOMER_ID=$(echo "$CREATE_RESPONSE" | jq -r '.id')
echo "Customer ID: $CUSTOMER_ID"

# Step 3: Update Customer
echo ""
echo "Updating customer..."
curl -s -X PUT \
  "${REST_ENDPOINT}/V1/customers/${CUSTOMER_ID}" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer ${ACCESS_TOKEN}" \
  -d "{
    \"customer\": {
      \"id\": ${CUSTOMER_ID},
      \"email\": \"john.doe@example.com\",
      \"firstname\": \"john\",
      \"lastname\": \"Doe-Updated\"
    }
  }" | jq .
```

## 关于本教程的重要说明

1. **URL路径**：使用`https://<server>.api.commerce.adobe.com/<tenant-id>/V1/customers` — **NOT** `https://<host>/rest/<store-view-code>/V1/customers`
1. **身份验证**：本教程使用了服务器到服务器（`client_credentials`授权类型）
1. **所需的作用域**： `commerce.accs`
1. **令牌过期**： 86400秒（24小时）

## 引用

* [Adobe Commerce as a Cloud Service发行说明](https://experienceleague.adobe.com/en/docs/commerce/cloud-service/release-notes)
* [SaaS REST API引用](https://developer.adobe.com/commerce/webapi/reference/rest/saas/)
* [用户身份验证指南](https://developer.adobe.com/commerce/webapi/rest/authentication/user/)
