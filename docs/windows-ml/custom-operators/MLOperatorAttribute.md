---
author: eliotcowley
title: MLOperatorAttribute struct
description: Specifies the name and properties of an attribute of a custom operator.
ms.author: elcowle
ms.date: 4/1/2019
ms.topic: article
keywords: windows 10, windows machine learning, WinML, custom operators, MLOperatorAttribute
ms.localizationpriority: medium
topic_type:
- APIRef
api_type:
- NA
api_name:
- MLOperatorAttribute
api_location:
- MLOperatorAuthor.h
---

# MLOperatorAttribute struct

Specifies the name and properties of an attribute of a custom operator. This is used when registering custom operator kernels and custom operator schema.

## Fields

| Name     | Type                    | Description |
|----------|-------------------------|-------------|
| name     | **char***                   | NULL-terminated UTF-8 string representing the name of the attribute in the associated operator type. |
| required | **bool**                    | Whether the attribute is required in any model using the associated operator type. |
| type     | [MLOperatorAttributeType](MLOperatorAttributeType.md) | The type of the attribute in the associated operator type. |

## Requirements

| | |
|-|-|
| **Minimum supported client** | Windows 10, build 17763 |
| **Minimum supported server** | Windows Server 2019 with Desktop Experience |
| **Header** | MLOperatorAuthor.h |

[!INCLUDE [help](../../includes/get-help.md)]
