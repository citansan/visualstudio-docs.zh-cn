---
title: 如何： 检查节点上使用内容模型视图的内容模型 |Microsoft Docs
ms.custom: ''
ms.date: 2018-06-30
ms.prod: visual-studio-dev14
ms.reviewer: ''
ms.suite: ''
ms.technology:
- vs-ide-general
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: c42ddac8-b0e3-48d6-9832-112a19d6c104
caps.latest.revision: 10
author: gewarren
ms.author: gewarren
manager: ghogen
ms.openlocfilehash: 25e47eed752a78caebcbae66a527cc847ac7d8f7
ms.sourcegitcommit: 55f7ce2d5d2e458e35c45787f1935b237ee5c9f8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/22/2018
ms.locfileid: "47469039"
---
# <a name="how-to-examine-the-content-model-of-nodes-using-the-content-model-view"></a>如何：使用内容模型视图检查节点的内容模型
[!INCLUDE[vs2017banner](../includes/vs2017banner.md)]

本主题的最新版本，请参阅[如何： 检查内容模型的节点使用内容模型视图](https://docs.microsoft.com/visualstudio/xml-tools/how-to-examine-the-content-model-of-nodes-using-the-content-model-view)。  
  
  
本主题介绍如何浏览使用节点[内容模型视图](../xml-tools/content-model-view.md)。  
  
### <a name="to-create-a-new-xsd-file-and-display-the-root-element-in-the-content-model-view"></a>创建新的 XSD 文件并显示内容模型视图中的根元素  
  
1.  创建新的 XML 架构文件。  
  
2.  单击**使用 XML 编辑器查看和编辑基础 XML 架构文件**起始视图上。  
  
3.  中的 XML 架构示例代码复制[示例 XML 架构： 订单架构](../xml-tools/sample-xsd-file-purchase-order-schema.md)并粘贴以替换默认情况下，已将它们添加到新 XSD 文件的代码。  
  
4.  选择`purchaseOrder`通过右键单击架构资源管理器中的元素`purchaseOrder`元素在 XML 编辑器中，然后选择**XML 资源管理器中显示**。  
  
5.  右键单击`purchaseOrder`在 XML 资源管理器中，选择**在内容模型视图中显示**。  
  
     内容模型视图显示设计图面上的 `purchaseOrder` 元素。  
  
6.  通过双击每个节点或单击每个节点右侧的双箭头，展开 `shipTo`、`billTo` 和 `items` 节点。  
  
     现在 `purchaseOrder` 元素的节点已展开，您可以查看元素的内容模型。  
  
7.  单击 `purchaseOrder` 元素下的任何节点并查看痕迹栏，以了解所选节点在架构集中的位置。  
  
8.  单击**显示文档**XSD 工具栏切换文档中的按钮。 还可以右击设计图面来切换文档。  
  
9. 右击`purchaseOrder`节点，然后选择**生成示例 XML**若要查看 XML 实例文档。


