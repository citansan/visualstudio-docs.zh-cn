---
title: 将 XML 数据读入到数据集中
ms.date: 11/04/2016
ms.topic: conceptual
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- reading XML
- data access [Visual Studio], XML data
- reading files, XML
- data [Visual Studio], reading from XML files
- reading data, XML files
- XML [Visual Studio], reading
- XML documents, reading
- datasets [Visual Basic], reading XML data
ms.assetid: fae72958-0893-47d6-b3dd-9d42418418e4
author: ghogen
ms.author: ghogen
manager: jillfra
ms.workload:
- data-storage
ms.openlocfilehash: 5f89645b9d5ec8ab0f69fad4fea5a399d8e6764d
ms.sourcegitcommit: d233ca00ad45e50cf62cca0d0b95dc69f0a87ad6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/01/2020
ms.locfileid: "75586323"
---
# <a name="read-xml-data-into-a-dataset"></a>将 XML 数据读入到数据集中

ADO.NET 提供了用于处理 XML 数据的简单方法。 在本演练中，将创建一个 Windows 应用程序，用于将 XML 数据加载到数据集。 然后，该数据集将显示在 <xref:System.Windows.Forms.DataGridView> 控件中。 最后，将在文本框中显示基于 XML 文件内容的 XML 架构。

## <a name="create-a-new-project"></a>创建新项目

为C#或 Visual Basic 创建新的**Windows 窗体应用**项目。 将项目命名为**ReadingXML**。

## <a name="generate-the-xml-file-to-be-read-into-the-dataset"></a>生成要读入数据集的 XML 文件

由于本演练重点介绍如何将 XML 数据读入数据集，因此会提供 XML 文件的内容。

1. 在“项目”菜单上，选择“添加新项”。

2. 选择 " **Xml 文件**"，将文件命名为 "**编写作者 .xml**"，然后选择 "**添加**"。

   该 XML 文件将加载到设计器中，并可供编辑。

3. 将以下 XML 数据粘贴到 XML 声明下面的编辑器中：

   ```xml
   <Authors_Table>
     <authors>
       <au_id>172-32-1176</au_id>
       <au_lname>White</au_lname>
       <au_fname>Johnson</au_fname>
       <phone>408 496-7223</phone>
       <address>10932 Bigge Rd.</address>
       <city>Menlo Park</city>
       <state>CA</state>
       <zip>94025</zip>
       <contract>true</contract>
     </authors>
     <authors>
       <au_id>213-46-8915</au_id>
       <au_lname>Green</au_lname>
       <au_fname>Margie</au_fname>
       <phone>415 986-7020</phone>
       <address>309 63rd St. #411</address>
       <city>Oakland</city>
       <state>CA</state>
       <zip>94618</zip>
       <contract>true</contract>
     </authors>
     <authors>
       <au_id>238-95-7766</au_id>
       <au_lname>Carson</au_lname>
       <au_fname>Cheryl</au_fname>
       <phone>415 548-7723</phone>
       <address>589 Darwin Ln.</address>
       <city>Berkeley</city>
       <state>CA</state>
       <zip>94705</zip>
       <contract>true</contract>
     </authors>
     <authors>
       <au_id>267-41-2394</au_id>
       <au_lname>Hunter</au_lname>
       <au_fname>Anne</au_fname>
       <phone>408 286-2428</phone>
       <address>22 Cleveland Av. #14</address>
       <city>San Jose</city>
       <state>CA</state>
       <zip>95128</zip>
       <contract>true</contract>
     </authors>
     <authors>
       <au_id>274-80-9391</au_id>
       <au_lname>Straight</au_lname>
       <au_fname>Dean</au_fname>
       <phone>415 834-2919</phone>
       <address>5420 College Av.</address>
       <city>Oakland</city>
       <state>CA</state>
       <zip>94609</zip>
       <contract>true</contract>
     </authors>
   </Authors_Table>
   ```

4. 在 "**文件**" 菜单上，选择 "**保存创作者 .xml**"。

## <a name="create-the-user-interface"></a>创建用户界面

此应用程序的用户界面包括以下各项：

- 一个 <xref:System.Windows.Forms.DataGridView> 控件，它将 XML 文件的内容显示为数据。

- 一个 <xref:System.Windows.Forms.TextBox> 控件，该控件显示 XML 文件的 XML 架构。

- 两个 <xref:System.Windows.Forms.Button> 控件。

  - 一个按钮将 XML 文件读取到数据集，并将其显示在 <xref:System.Windows.Forms.DataGridView> 控件中。

  - 第二个按钮从数据集中提取架构，并通过 <xref:System.IO.StringWriter> 在 <xref:System.Windows.Forms.TextBox> 控件中显示该架构。

### <a name="to-add-controls-to-the-form"></a>向窗体添加控件

1. 在 "设计" 视图中打开 `Form1`。

2. 从 "**工具箱**" 中，将以下控件拖到窗体上：

    - 一 <xref:System.Windows.Forms.DataGridView> 控件

    - 一 <xref:System.Windows.Forms.TextBox> 控件

    - 两个 <xref:System.Windows.Forms.Button> 控件

3. 设置以下属性：

    |控件|Property|设置|
    |-------------|--------------|-------------|
    |`TextBox1`|**多行**|`true`|
    ||**ScrollBars**|**垂直**|
    |`Button1`|**Name**|`ReadXmlButton`|
    ||**“文本”**|`Read XML`|
    |`Button2`|**Name**|`ShowSchemaButton`|
    ||**“文本”**|`Show Schema`|

## <a name="create-the-dataset-that-receives-the-xml-data"></a>创建接收 XML 数据的数据集

在此步骤中，将创建一个名为 `authors`的新数据集。 有关数据集的详细信息，请参阅[Visual Studio 中的数据集工具](../data-tools/dataset-tools-in-visual-studio.md)。

1. 在**解决方案资源管理器**中，选择 " **Form1**" 的源文件，然后在**解决方案资源管理器**工具栏上选择 "**查看设计器**" 按钮。

2. 从工具箱的 "[数据" 选项卡](../ide/reference/toolbox-data-tab.md)中，将**数据集**拖到**Form1**上。

3. 在 "**添加数据集**" 对话框中，选择 "**非类型化数据集**"，然后选择 **"确定"** 。

     **DataSet1**将添加到组件栏。

4. 在 "**属性**" 窗口中，为`AuthorsDataSet`设置 "**名称**" 和 "<xref:System.Data.DataSet.DataSetName%2A> 属性"。

## <a name="create-the-event-handler-to-read-the-xml-file-into-the-dataset"></a>创建事件处理程序以将 XML 文件读取到数据集

"**读取 xml** " 按钮将 XML 文件读取到数据集。 然后，它将 <xref:System.Windows.Forms.DataGridView> 控件上的属性设置为将其绑定到数据集。

1. 在**解决方案资源管理器**中，选择 " **Form1**"，然后在**解决方案资源管理器**工具栏上选择 "**查看设计器**" 按钮。

2. 选择 "**读取 XML** " 按钮。

     **代码编辑器**将在 `ReadXmlButton_Click` 事件处理程序打开。

3. 在 `ReadXmlButton_Click` 事件处理程序中键入以下代码：

     [!code-csharp[VbRaddataFillingAndExecuting#2](../data-tools/codesnippet/CSharp/read-xml-data-into-a-dataset_1.cs)]
     [!code-vb[VbRaddataFillingAndExecuting#2](../data-tools/codesnippet/VisualBasic/read-xml-data-into-a-dataset_1.vb)]

4. 在 `ReadXMLButton_Click` 事件处理程序代码中，将 `filepath =` 项更改为正确的路径。

## <a name="create-the-event-handler-to-display-the-schema-in-the-textbox"></a>创建事件处理程序以在文本框中显示架构

"**显示架构**" 按钮将创建一个 <xref:System.IO.StringWriter> 对象，该对象填充架构并显示在 <xref:System.Windows.Forms.TextBox>控件中。

1. 在**解决方案资源管理器**中，选择 " **Form1**"，然后选择 "**视图设计器**" 按钮。

2. 选择 "**显示架构**" 按钮。

     **代码编辑器**将在 `ShowSchemaButton_Click` 事件处理程序打开。

3. 将下面的代码粘贴到 `ShowSchemaButton_Click` 事件处理程序中。

     [!code-csharp[VbRaddataFillingAndExecuting#3](../data-tools/codesnippet/CSharp/read-xml-data-into-a-dataset_2.cs)]
     [!code-vb[VbRaddataFillingAndExecuting#3](../data-tools/codesnippet/VisualBasic/read-xml-data-into-a-dataset_2.vb)]

## <a name="test-the-form"></a>测试窗体

你现在可以对窗体进行测试，以确保它按预期方式运行。

1. 选择**F5**以运行应用程序。

2. 选择 "**读取 XML** " 按钮。

     DataGridView 显示 XML 文件的内容。

3. 选择 "**显示架构**" 按钮。

     文本框显示 XML 文件的 XML 架构。

## <a name="next-steps"></a>后续步骤

本演练介绍有关将 XML 文件读取到数据集，以及基于 XML 文件的内容创建架构的基本知识。 下面是你可能需要执行的一些任务：

- 编辑数据集中的数据，并将其作为 XML 写回。 有关更多信息，请参见<xref:System.Data.DataSet.WriteXml%2A>。

- 编辑数据集中的数据，并将数据写入数据库。

## <a name="see-also"></a>另请参阅

- [在 Visual Studio 中访问数据](../data-tools/accessing-data-in-visual-studio.md)
- [Visual Studio 中的 XML 工具](../xml-tools/xml-tools-in-visual-studio.md)
