---
title: CA1028：枚举存储应为 Int32 |Microsoft Docs
ms.date: 11/15/2016
ms.prod: visual-studio-dev14
ms.technology: vs-ide-code-analysis
ms.topic: reference
f1_keywords:
- CA1028
- EnumStorageShouldBeInt32
helpviewer_keywords:
- EnumStorageShouldBeInt32
- CA1028
ms.assetid: 87160825-9f39-4142-8d7f-a31fe7ac7b84
caps.latest.revision: 21
author: jillre
ms.author: jillfra
manager: wpickett
ms.openlocfilehash: fc1039cb547a48c4f2dd3ea869b46d4706e9c3a2
ms.sourcegitcommit: a8e8f4bd5d508da34bbe9f2d4d9fa94da0539de0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2019
ms.locfileid: "72661906"
---
# <a name="ca1028-enum-storage-should-be-int32"></a>CA1028：枚举存储应为 Int32
[!INCLUDE[vs2017banner](../includes/vs2017banner.md)]

|||
|-|-|
|TypeName|EnumStorageShouldBeInt32|
|CheckId|CA1028|
|类别|Microsoft. Design|
|是否重大更改|重大|

## <a name="cause"></a>原因
 公共枚举的基础类型不 <xref:System.Int32?displayProperty=fullName>。

## <a name="rule-description"></a>规则说明
 枚举是一种值类型，它定义一组相关的已命名常数。 默认情况下，<xref:System.Int32?displayProperty=fullName> 数据类型用于存储常量值。 尽管你可以更改此基础类型，但在大多数情况下不需要或建议使用此类型。 请注意，使用小于 <xref:System.Int32> 的数据类型不会显著提高性能。 如果无法使用默认数据类型，则应使用符合公共语言系统（CLS）的整数类型之一，<xref:System.Byte>、<xref:System.Int16>、<xref:System.Int32> 或 <xref:System.Int64>，以确保枚举的所有值都可以在符合 CLS 的编程中表示语言。

## <a name="how-to-fix-violations"></a>如何解决冲突
 若要修复与此规则的冲突，请使用 <xref:System.Int32>，除非存在大小或兼容性问题。 对于 <xref:System.Int32> 不够大，无法保存值的情况，请使用 <xref:System.Int64>。 如果向后兼容性要求较小的数据类型，请使用 <xref:System.Byte> 或 <xref:System.Int16>。

## <a name="when-to-suppress-warnings"></a>何时禁止显示警告
 仅当后向兼容性问题需要时，才禁止显示此规则发出的警告。 在应用程序中，未能遵守此规则通常不会导致问题。 在需要语言互操作性的库中，未能遵守此规则可能会对用户造成不利影响。

## <a name="example-of-a-violation"></a>冲突示例

### <a name="description"></a>描述
 下面的示例演示两个不使用建议的基础数据类型的枚举。

### <a name="code"></a>代码
 [!code-csharp[FxCop.Design.EnumIntegralType#1](../snippets/csharp/VS_Snippets_CodeAnalysis/FxCop.Design.EnumIntegralType/cs/FxCop.Design.EnumIntegralType.cs#1)]
 [!code-vb[FxCop.Design.EnumIntegralType#1](../snippets/visualbasic/VS_Snippets_CodeAnalysis/FxCop.Design.EnumIntegralType/vb/FxCop.Design.EnumIntegralType.vb#1)]

## <a name="example-of-how-to-fix"></a>解决方法的示例

### <a name="description"></a>描述
 下面的示例通过将基础数据类型更改为 <xref:System.Int32> 来修复前面的冲突。

### <a name="code"></a>代码
 [!code-csharp[FxCop.Design.EnumIntegralTypeFixed#1](../snippets/csharp/VS_Snippets_CodeAnalysis/FxCop.Design.EnumIntegralTypeFixed/cs/FxCop.Design.EnumIntegralTypeFixed.cs#1)]
 [!code-vb[FxCop.Design.EnumIntegralTypeFixed#1](../snippets/visualbasic/VS_Snippets_CodeAnalysis/FxCop.Design.EnumIntegralTypeFixed/vb/FxCop.Design.EnumIntegralTypeFixed.vb#1)]

## <a name="related-rules"></a>相关规则
 [CA1008：枚举应具有零值](../code-quality/ca1008-enums-should-have-zero-value.md)

 [CA1027：用 FlagsAttribute 标记枚举](../code-quality/ca1027-mark-enums-with-flagsattribute.md)

 [CA2217：不要使用 FlagsAttribute 标记枚举](../code-quality/ca2217-do-not-mark-enums-with-flagsattribute.md)

 [CA1700：不要命名“Reserved”枚举值](../code-quality/ca1700-do-not-name-enum-values-reserved.md)

 [CA1712：不要将类型名用作枚举值的前缀](../code-quality/ca1712-do-not-prefix-enum-values-with-type-name.md)

## <a name="see-also"></a>请参阅
 <xref:System.Byte?displayProperty=fullName> <xref:System.Int16?displayProperty=fullName>
 <xref:System.Int32?displayProperty=fullName>
 <xref:System.Int64?displayProperty=fullName>
