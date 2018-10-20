---
title: CA1506： 避免过度类耦合 |Microsoft Docs
ms.custom: ''
ms.date: 11/15/2016
ms.prod: visual-studio-dev14
ms.reviewer: ''
ms.suite: ''
ms.technology:
- vs-devops-test
ms.tgt_pltfrm: ''
ms.topic: article
f1_keywords:
- AvoidExcessiveClassCoupling
- CA1506
helpviewer_keywords:
- AvoidExcessiveClassCoupling
- CA1506
ms.assetid: 9f0943c0-e802-4e3f-8798-2ab8653ddc80
caps.latest.revision: 14
author: gewarren
ms.author: gewarren
manager: wpickett
ms.openlocfilehash: 39a6a8d60968085455a4e6e80fe2e37ced4150bd
ms.sourcegitcommit: 9ceaf69568d61023868ced59108ae4dd46f720ab
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/12/2018
ms.locfileid: "49253586"
---
# <a name="ca1506-avoid-excessive-class-coupling"></a>CA1506：避免过度类耦合
[!INCLUDE[vs2017banner](../includes/vs2017banner.md)]
|||
|-|-|
|TypeName|AvoidExcessiveClassCoupling|
|CheckId|CA1506|
|类别|Microsoft.Maintainability|
|是否重大更改|重大|

## <a name="cause"></a>原因
 与许多其他类型结合使用的类型或方法。

## <a name="rule-description"></a>规则说明
 此规则通过计算类型或方法包含的唯一类型引用的个数来衡量类耦合。

 具有类耦合度很高的类型和方法可能很难维护。 最好能够表现出低耦合和高度聚合的类型和方法。

## <a name="how-to-fix-violations"></a>如何解决冲突
 若要解决此冲突，请尝试重新设计的类型或方法来减少之耦合的类型的数。

## <a name="when-to-suppress-warnings"></a>何时禁止显示警告
 如果类型或方法仍被视为可维护性，尽管其大量的其他类型的依赖项，请排除此警告。

## <a name="see-also"></a>请参阅
 [可维护性警告](../code-quality/maintainability-warnings.md)[测量复杂性和可维护性的托管代码](../code-quality/measuring-complexity-and-maintainability-of-managed-code.md)


