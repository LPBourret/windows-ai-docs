---
author: eliotcowley
title: IMLOperatorTypeInferenceContext.IsOutputValid method
description: Returns true if an output to the operator is valid.
ms.author: elcowle
ms.date: 4/1/2019
ms.topic: article
keywords: windows 10, windows machine learning, WinML, custom operators, IsOutputValid
ms.localizationpriority: medium
topic_type:
- APIRef
api_type:
- NA
api_name:
- IMLOperatorTypeInferenceContext.IsOutputValid
api_location:
- MLOperatorAuthor.h
---

# IMLOperatorTypeInferenceContext.IsOutputValid method

Returns true if an output to the operator is valid. This always returns true except for optional outputs.

```cpp
bool IsOutputValid(
    uint32_t outputIndex)
```

## Requirements

| | |
|-|-|
| **Minimum supported client** | Windows 10, build 17763 |
| **Minimum supported server** | Windows Server 2019 with Desktop Experience |
| **Header** | MLOperatorAuthor.h |

[!INCLUDE [help](../../includes/get-help.md)]
