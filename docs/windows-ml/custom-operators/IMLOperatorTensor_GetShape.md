---
author: eliotcowley
title: IMLOperatorTensor.GetShape method
description: Gets the size of dimensions in the tensor.
ms.author: elcowle
ms.date: 4/1/2019
ms.topic: article
keywords: windows 10, windows machine learning, WinML, custom operators, GetShape
ms.localizationpriority: medium
topic_type:
- APIRef
api_type:
- NA
api_name:
- IMLOperatorTensor.GetShape
api_location:
- MLOperatorAuthor.h
---

# IMLOperatorTensor.GetShape method

Gets the size of dimensions in the tensor.

```cpp
void GetShape(
    uint32_t dimensionCount,
    _Out_writes_(dimensionCount) uint32_t* dimensions)
```

## Requirements

| | |
|-|-|
| **Minimum supported client** | Windows 10, build 17763 |
| **Minimum supported server** | Windows Server 2019 with Desktop Experience |
| **Header** | MLOperatorAuthor.h |

[!INCLUDE [help](../../includes/get-help.md)]
