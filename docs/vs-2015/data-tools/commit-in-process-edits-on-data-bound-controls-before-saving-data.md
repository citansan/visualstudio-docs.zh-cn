---
title: 保存数据前提交数据绑定控件上的进程内编辑 |Microsoft Docs
ms.custom: ''
ms.date: 11/15/2016
ms.prod: visual-studio-dev14
ms.reviewer: ''
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: article
dev_langs:
- VB
- CSharp
- C++
- aspx
helpviewer_keywords:
- commiting edited records
- data-bound controls, in-process edits
- DataBinding class, commiting edited records
- hierarchical update, commiting edited records
- BindingSource class, commiting edited records
- EndEdit method
ms.assetid: 61af4798-eef7-468c-b229-5e1497febb2f
caps.latest.revision: 16
author: gewarren
ms.author: gewarren
manager: ghogen
ms.openlocfilehash: 3af1534e6436eec2eac1f294be8c2428c949ce9d
ms.sourcegitcommit: 9ceaf69568d61023868ced59108ae4dd46f720ab
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/12/2018
ms.locfileid: "49296020"
---
# <a name="commit-in-process-edits-on-data-bound-controls-before-saving-data"></a>在保存数据前提交数据绑定控件中正在进行的编辑
[!INCLUDE[vs2017banner](../includes/vs2017banner.md)]

  
在编辑数据绑定控件中的值时，用户必须导航过当前记录，若要提交到该控件绑定到基础数据源的更新后的值。 当将项从[数据源窗口](http://msdn.microsoft.com/library/0d20f699-cc95-45b3-8ecb-c7edf1f67992)拖到窗体，则删除的第一项生成的代码插入**保存**按钮单击事件的<xref:System.Windows.Forms.BindingNavigator>。 此代码将调用<xref:System.Windows.Forms.BindingSource.EndEdit%2A>方法的<xref:System.Windows.Forms.BindingSource>。 因此，调用<xref:System.Windows.Forms.BindingSource.EndEdit%2A>方法生成仅对第一个<xref:System.Windows.Forms.BindingSource>，添加到窗体。  
  
 <xref:System.Windows.Forms.BindingSource.EndEdit%2A>调用提交中过程中，当前正在编辑的所有数据绑定控件中的任何更改。 因此，如果数据绑定控件仍然具有焦点并且单击**保存**按钮，所有挂起的编辑，控制事务之前提交的实际保存 (`TableAdapterManager.UpdateAll`方法)。  
  
 即使在用户尝试保存数据，而提交所做的更改，保存的一部分，可以配置为自动提交更改，应用程序进程。  
  
> [!NOTE]
>  设计器添加`BindingSource.EndEdit`代码仅对第一项的拖放到窗体上。 因此，您必须添加要调用的代码行<xref:System.Windows.Forms.BindingSource.EndEdit%2A>方法为每个<xref:System.Windows.Forms.BindingSource>窗体上。 您可以手动添加一行代码来调用<xref:System.Windows.Forms.BindingSource.EndEdit%2A>方法为每个<xref:System.Windows.Forms.BindingSource>。 或者，可以添加`EndEditOnAllBindingSources`到窗体的方法，并在执行保存之前调用它。  
  
 下面的代码使用[LINQ （语言集成查询）](http://msdn.microsoft.com/library/a73c4aec-5d15-4e98-b962-1274021ea93d)查询，以循环访问所有<xref:System.Windows.Forms.BindingSource>组件和调用<xref:System.Windows.Forms.BindingSource.EndEdit%2A>方法为每个<xref:System.Windows.Forms.BindingSource>窗体上。  
  
## <a name="to-call-endedit-for-all-bindingsource-components-on-a-form"></a>若要在窗体上的所有 BindingSource 组件都调用 EndEdit  
  
1.  将以下代码添加到包含的窗体<xref:System.Windows.Forms.BindingSource>组件。  
  
     [!code-csharp[VSProDataOrcasEndEditOnAll#1](../snippets/csharp/VS_Snippets_VBCSharp/VSProDataOrcasEndEditOnAll/CS/Form1.cs#1)]
     [!code-vb[VSProDataOrcasEndEditOnAll#1](../snippets/visualbasic/VS_Snippets_VBCSharp/VSProDataOrcasEndEditOnAll/VB/Form1.vb#1)]  
  
2.  添加以下调用以保存窗体的数据的紧前面的代码行 (`TableAdapterManager.UpdateAll()`方法):  
  
     [!code-csharp[VSProDataOrcasEndEditOnAll#2](../snippets/csharp/VS_Snippets_VBCSharp/VSProDataOrcasEndEditOnAll/CS/Form1.cs#2)]
     [!code-vb[VSProDataOrcasEndEditOnAll#2](../snippets/visualbasic/VS_Snippets_VBCSharp/VSProDataOrcasEndEditOnAll/VB/Form1.vb#2)]  
  
## <a name="see-also"></a>请参阅  
 [在 Visual Studio 中将 Windows 窗体控件绑定到数据](../data-tools/bind-windows-forms-controls-to-data-in-visual-studio.md)   
 [分层更新](../data-tools/hierarchical-update.md)
