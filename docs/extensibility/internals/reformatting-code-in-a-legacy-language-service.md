---
title: 重新格式化旧版语言服务中的代码 |Microsoft Docs
ms.date: 11/04/2016
ms.topic: conceptual
helpviewer_keywords:
- reformatting code, supporting in language services [managed package framework]
- language services [managed package framework], reformatting code
ms.assetid: 08bb3375-8fef-4f4e-9efa-0d7333bab0eb
author: madskristensen
ms.author: madsk
manager: jillfra
ms.workload:
- vssdk
ms.openlocfilehash: ae48e1b97b5c9194cf3081687ab31ea9f857e6c9
ms.sourcegitcommit: 5f6ad1cefbcd3d531ce587ad30e684684f4c4d44
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2019
ms.locfileid: "72724756"
---
# <a name="reformatting-code-in-a-legacy-language-service"></a>在旧版语言服务中重新格式化代码

在中，可以通过规范化缩进和空白的使用来重新格式化 [!INCLUDE[vsprvs](../../code-quality/includes/vsprvs_md.md)] 源代码。 这可能包括在每行的开头插入或删除空格或制表符，在各行之间添加新行，或者用空格替换空格或制表符。

> [!NOTE]
> 插入或删除换行符可能会影响断点，例如断点和书签，但添加或删除空格或制表符不影响标记。

用户可以通过从 "**编辑**" 菜单上的 "**高级**" 菜单中选择 "**格式选择**" 或 "**设置文档格式**" 来开始重新格式化操作。 在插入代码段或特定字符时，还可以触发重新格式化操作。 例如，在中C#键入右大括号时，匹配的左大括号和右大括号之间的所有内容都将自动缩进到适当的级别。

当 [!INCLUDE[vsprvs](../../code-quality/includes/vsprvs_md.md)] 将**格式选择**或**格式文档**命令发送到语言服务时，<xref:Microsoft.VisualStudio.Package.ViewFilter> 类将调用 <xref:Microsoft.VisualStudio.Package.Source> 类中的 <xref:Microsoft.VisualStudio.Package.Source.ReformatSpan%2A> 方法。 若要支持格式设置，必须重写 <xref:Microsoft.VisualStudio.Package.Source.ReformatSpan%2A> 方法，并提供您自己的格式设置代码。

## <a name="enabling-support-for-reformatting"></a>启用对重新格式化的支持

若要支持格式设置，注册 VSPackage 时必须将 <xref:Microsoft.VisualStudio.Shell.ProvideLanguageServiceAttribute> 的 `EnableFormatSelection` 参数设置为 `true`。 这会将 <xref:Microsoft.VisualStudio.Package.LanguagePreferences.EnableFormatSelection%2A> 属性设置为 `true`。 @No__t_0 方法返回此属性的值。 如果它返回 true，则 <xref:Microsoft.VisualStudio.Package.ViewFilter> 类将调用 <xref:Microsoft.VisualStudio.Package.Source.ReformatSpan%2A>。

## <a name="implementing-reformatting"></a>实现重新格式化

若要实现重新格式化，必须从 <xref:Microsoft.VisualStudio.Package.Source> 类派生一个类，并重写 <xref:Microsoft.VisualStudio.Package.Source.ReformatSpan%2A> 方法。 @No__t_0 对象描述要设置格式的范围，<xref:Microsoft.VisualStudio.Package.EditArray> 对象保存在该范围内进行的编辑。 请注意，此范围可以是整个文档。 但是，因为可能存在多个对跨度所做的更改，所以在单个操作中，所有更改都应可逆。 为此，请在 <xref:Microsoft.VisualStudio.Package.CompoundAction> 对象中包装所有更改（请参阅本主题中的 "使用 CompoundAction 类" 一节）。

### <a name="example"></a>示例

下面的示例确保选定内容中的每个逗号后均有一个空格，除非逗号后跟制表符或位于行尾。 删除行中最后一个逗号之后的尾随空格。 请参阅本主题中的 "使用 CompoundAction 类" 部分，了解如何从 <xref:Microsoft.VisualStudio.Package.Source.ReformatSpan%2A> 方法调用此方法。

```csharp
using Microsoft.VisualStudio.Package;
using Microsoft VisualStudio.TextManager.Interop;

namespace MyLanguagePackage
{
    class MySource : Source
    {
        private void DoFormatting(EditArray mgr, TextSpan span)
        {
            // Make sure there is one space after every comma unless followed
            // by a tab or comma is at end of line.
            IVsTextLines pBuffer = GetTextLines();
            if (pBuffer != null)
            {
                int    startIndex = span.iStartIndex;
                int    endIndex   = 0;
                string line       = "";
                // Loop over all lines in span
                for (int i = span.iStartLine; i <= span.iEndLine; i++)
                {
                    if (i < span.iEndLine)
                    {
                        pBuffer.GetLengthOfLine(i, out endIndex);
                    }
                    else
                    {
                        endIndex = span.iEndIndex;
                    }
                    pBuffer.GetLineText(i, startIndex, i, endIndex, out line);

                    if (line.Length > 0)
                    {
                        int index = 0;
                        // Loop over all commas in line
                        while ((index = line.IndexOf(',', index)) != -1)
                        {
                            int spaceIndex = index + 1;
                            // Determine how many spaces after comma
                            while (spaceIndex < line.Length && line[spaceIndex] == ' ')
                            {
                                ++spaceIndex;
                            }

                            int      spaceCount      = spaceIndex - (index + 1);
                            string   replacementText = " ";
                            bool     fDoEdit         = true;
                            TextSpan editTextSpan    = new TextSpan();

                            editTextSpan.iStartLine  = i;
                            editTextSpan.iEndLine    = i;
                            editTextSpan.iStartIndex = index + 1;

                            if (spaceIndex == line.Length)
                            {
                                if (spaceCount > 0)
                                {
                                    // Delete spaces after a comma at end of line
                                    editTextSpan.iEndIndex = spaceIndex;
                                    replacementText = "";
                                }
                                else
                                {
                                    // Otherwise, do not add spaces if comma is
                                    // at end of line.
                                    fDoEdit = false;
                                }
                            }
                            else if (spaceCount == 0)
                            {
                                if (spaceIndex < line.Length && line[spaceIndex] == '\t')
                                {
                                    // Do not insert space if a tab follows
                                    // a comma.
                                    fDoEdit = false;
                                }
                                else
                                {
                                    // No space after comma so add a space.
                                    editTextSpan.iEndIndex = index + 1;
                                }
                            }
                            else if (spaceCount > 1)
                            {
                                // More than one space after comma, replace
                                // with a single space.
                                editTextSpan.iEndIndex = spaceIndex;
                            }
                            else
                            {
                                // Single space after a comma and not at end
                                // of the line, leave it alone.
                                fDoEdit = false;
                            }
                            if (fDoEdit)
                            {
                                // Add edit operation
                                mgr.Add(new EditSpan(editTextSpan, replacementText));
                            }
                            index = spaceIndex;
                        }
                    }
                    startIndex = 0; // All subsequent lines start at 0
                }
                // Apply all edits
                mgr.ApplyEdits();
            }
        }
    }
}
```

## <a name="using-the-compoundaction-class"></a>使用 CompoundAction 类

在一段代码中完成的所有重新格式化操作都应在一个操作中可逆。 可以使用 <xref:Microsoft.VisualStudio.Package.CompoundAction> 类完成此操作。 此类将文本缓冲区中的一组编辑操作包装到单个编辑操作中。

### <a name="example"></a>示例

下面是如何使用 <xref:Microsoft.VisualStudio.Package.CompoundAction> 类的示例。 有关 `DoFormatting` 方法的示例，请参阅本主题的 "实现格式设置支持" 一节中的示例。

```csharp
using Microsoft.VisualStudio.Package;
using Microsoft VisualStudio.TextManager.Interop;

namespace MyLanguagePackage
{
    class MySource : Source
    {
        public override void ReformatSpan(EditArray mgr, TextSpan span)
        {
            string description = "Reformat code";
            CompoundAction ca = new CompoundAction(this, description);
            using (ca)
            {
                ca.FlushEditActions();      // Flush any pending edits
                DoFormatting(mgr, span);    // Format the span
            }
        }
    }
}
```

## <a name="see-also"></a>请参阅

- [旧版语言服务功能](legacy-language-service-features1.md)