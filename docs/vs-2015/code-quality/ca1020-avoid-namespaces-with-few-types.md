---
title: CA1020：避免具有几种类型的命名空间 |Microsoft Docs
ms.date: 11/15/2016
ms.prod: visual-studio-dev14
ms.technology: vs-ide-code-analysis
ms.topic: reference
f1_keywords:
- CA1020
- AvoidNamespacesWithFewTypes
helpviewer_keywords:
- CA1020
- AvoidNamespacesWithFewTypes
ms.assetid: 9cb272f6-a3ff-45af-b35d-70dea620b074
caps.latest.revision: 19
author: jillre
ms.author: jillfra
manager: wpickett
ms.openlocfilehash: 81eaf2735869668b86ca8879478e3d76d77a2811
ms.sourcegitcommit: a8e8f4bd5d508da34bbe9f2d4d9fa94da0539de0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2019
ms.locfileid: "72662315"
---
# <a name="ca1020-avoid-namespaces-with-few-types"></a>CA1020：避免使用类型极少的命名空间
[!INCLUDE[vs2017banner](../includes/vs2017banner.md)]

|||
|-|-|
|TypeName|AvoidNamespacesWithFewTypes|
|CheckId|CA1020|
|类别|Microsoft. Design|
|是否重大更改|重大|

## <a name="cause"></a>原因
 除全局命名空间之外的命名空间包含的类型少于五个。

## <a name="rule-description"></a>规则说明
 请确保每个命名空间都有一个逻辑组织，并确保将类型放入稀疏填充的命名空间中的原因是有效的。 命名空间应包含在大多数情况下一起使用的类型。 当它们的应用程序互斥时，类型应位于单独的命名空间中。 例如，<xref:System.Web.UI> 命名空间包含在 Web 应用程序中使用的类型，而 <xref:System.Windows.Forms> 命名空间包含在基于 [!INCLUDE[TLA#tla_mswin](../includes/tlasharptla-mswin-md.md)] 的应用程序中使用的类型。 即使两个命名空间都具有控制用户界面各方面的类型，这些类型也并不是旨在用于同一应用程序。 因此，它们位于单独的命名空间中。 小心命名空间组织也有帮助，因为这样可以提高功能的可发现性。 通过检查命名空间层次结构，库使用者应该能够找到实现功能的类型。

> [!NOTE]
> 不应将设计时类型和权限合并到其他命名空间，以符合此准则。 这些类型在主命名空间下面的命名空间中，命名空间应分别以 `.Design` 和 `.Permissions` 结束。

## <a name="how-to-fix-violations"></a>如何解决冲突
 若要修复与此规则的冲突，请尝试将只包含几种类型的命名空间合并为单个命名空间。

## <a name="when-to-suppress-warnings"></a>何时禁止显示警告
 如果命名空间不包含与其他命名空间中的类型一起使用的类型，则可以安全地禁止显示此规则发出的警告。
