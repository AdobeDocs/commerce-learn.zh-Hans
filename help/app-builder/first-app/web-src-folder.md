---
title: web-src文件夹
description: 了解web-src文件夹中的文件类型以及此示例应用程序的嵌套文件和文件夹。
landing-page-description: 了解用于Adobe Commerce的Adobe Developer App Builder以及web-src文件夹中的文件类型。
kt: 12425
doc-type: tutorial
audience: all
last-substantial-update: 2023-3-13
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 67bbb464-1c2e-493e-9d7f-1051dfeec4ee
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 0%

---

# 了解web-src文件夹的用途 {#web-src-folder}

此示例应用程序的web-src文件夹包含许多JavaScript文件和文件夹。 此文件夹用于具有用户界面的应用程序。 并非所有应用程序都使用此功能。 例如，Commerce与外部库存管理系统的集成可能不需要前端界面和代码。

## 此视频面向谁？

* 刚开始接触Adobe Commerce但使用AdobeApp Builder经验有限的开发人员，他们了解`web-src`文件夹及其内容。

## 视频内容

* `web-src`文件夹的主要用途是什么？
* 通常包括文件和文件夹
* 如何在示例应用程序中使用`web-src`文件夹及其内容

>[!VIDEO](https://video.tv.adobe.com/v/3421037?quality=12&learn=on&captions=chi_hans)

## 代码示例

web-src/src/components/Orders.js

```javascript
/*
 * Copyright 2023 Adobe
 * All Rights Reserved.
 *
 * NOTICE: All information contained herein is, and remains
 * the property of Adobe and its suppliers, if any. The intellectual
 * and technical concepts contained herein are proprietary to Adobe
 * and its suppliers and are protected by all applicable intellectual
 * property laws, including trade secret and copyright laws.
 * Dissemination of this information or reproduction of this material
 * is strictly forbidden unless prior written permission is obtained
 * from Adobe.
 */
import {
    Content,
    Heading,
    IllustratedMessage,
    TableView,
    TableHeader,
    TableBody,
    Column,
    Row,
    Cell,
    View,
    Flex,
    ProgressCircle
} from '@adobe/react-spectrum'
import {useCommerceOrders} from '../hooks/useCommerceOrders'

export const Orders = props => {

    const {isLoadingCommerceOrders, commerceOrders} = useCommerceOrders(props)

    const ordersColumns = [
        {name: 'Order Id', uid: 'increment_id'},
        {name: 'Status', uid: 'status'},
        {name: 'Store Name', uid: 'store_name'},
        {name: 'Total Item Count', uid: 'total_item_count'},
        {name: 'Total Quantity', uid: 'total_qty_ordered'},
        {name: 'Total Due', uid: 'total_due'},
        {name: 'Tax', uid: 'tax_amount'},
        {name: 'Created At', uid: 'created_at'}
    ]

    function renderEmptyState() {
        return (
            <IllustratedMessage>
                <Content>No data available</Content>
            </IllustratedMessage>
        )
    }

    return (

        <View>
            {isLoadingCommerceOrders ? (
                <Flex alignItems="center" justifyContent="center" height="100vh">
                    <ProgressCircle size="L" aria-label="Loading…" isIndeterminate/>
                </Flex>
            ) : (
                <View margin={10}>
                    <Heading level={1}>Fetched orders from Adobe Commerce</Heading>
                    <TableView
                        overflowMode="wrap"
                        aria-label="orders table"
                        flex
                        renderEmptyState={renderEmptyState}
                        height="static-size-1000"
                    >
                        <TableHeader columns={ordersColumns}>
                            {column => <Column key={column.uid}>{column.name}</Column>}
                        </TableHeader>
                        <TableBody items={commerceOrders}>
                            {order => (
                                <Row key={order['increment_id']}>{columnKey => <Cell>{order[columnKey]}</Cell>}</Row>
                            )}
                        </TableBody>
                    </TableView>
                </View>
            )}
        </View>
    )
}
```

web-src/src/hooks/useCommerceOrders.js

{{avoid-400-error}}

在以下示例中，代码示例是限制请求的`not`。 要避免400错误，请使用`searchCriteria`减小响应的大小。

`?searchCriteria[filter_groups][0][filters][0][field]=created_at&searchCriteria[filter_groups][0][filters][0][value]=2022-12-01&searchCriteria[filter_groups][0][filters][0][condition_type]=gt`

```javascript {line-numbers="true" start-line="1" highlight="25"}
/*
 * Copyright 2023 Adobe
 * All Rights Reserved.
 *
 * NOTICE: All information contained herein is, and remains
 * the property of Adobe and its suppliers, if any. The intellectual
 * and technical concepts contained herein are proprietary to Adobe
 * and its suppliers and are protected by all applicable intellectual
 * property laws, including trade secret and copyright laws.
 * Dissemination of this information or reproduction of this material
 * is strictly forbidden unless prior written permission is obtained
 * from Adobe.
 */
import { useEffect, useState } from 'react'
import { callAction } from '../utils'

export const useCommerceOrders = props => {
    const [isLoadingCommerceOrders, setIsLoadingCommerceOrders] = useState(true)
    const [commerceOrders, setCommerceOrders] = useState([])

    const fetchCommerceOrders = async () => {
        const commerceOrdersResponse = await callAction(
            props,
            'commerce-rest-get',
            'orders?searchCriteria=all'
        )
        setCommerceOrders(commerceOrdersResponse.error ? [] : commerceOrdersResponse.items)
    }

    useEffect(() => {
        fetchCommerceOrders().then(() => setIsLoadingCommerceOrders(false))
    }, [])

    return { isLoadingCommerceOrders, commerceOrders }
}
```

{{$include /help/_includes/app-builder-first-app-related-links.md}}
