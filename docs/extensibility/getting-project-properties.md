---
title: 正在获取项目属性 |Microsoft Docs
ms.date: 3/16/2019
ms.topic: conceptual
helpviewer_keywords:
- project properties, displaying in tool window
- tool windows, displaying project properties
ms.assetid: 96ba07ca-0811-4013-8602-12550ac4ba79
author: madskristensen
ms.author: madsk
manager: jillfra
ms.workload:
- vssdk
ms.openlocfilehash: cac5c55dd8fdeb1ba231d144d94c8be9b680cc6e
ms.sourcegitcommit: a8e8f4bd5d508da34bbe9f2d4d9fa94da0539de0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2019
ms.locfileid: "72633173"
---
# <a name="get-project-properties"></a>获取项目属性

本演练演示如何在工具窗口中显示项目属性。

## <a name="prerequisites"></a>Prerequisites

从 Visual Studio 2015 开始，你不需要从下载中心安装 Visual Studio SDK。 它作为 Visual Studio 安装程序中的可选功能提供。 你还可以在以后安装 VS SDK。 有关详细信息，请参阅[安装 Visual STUDIO SDK](../extensibility/installing-the-visual-studio-sdk.md)。

### <a name="to-create-a-vsix-project-and-add-a-tool-window"></a>创建 VSIX 项目并添加工具窗口

1. 每个 Visual Studio 扩展都从一个 VSIX 部署项目开始，该项目将包含扩展资产。 创建一个名为 `ProjectPropertiesExtension` [!INCLUDE[vsprvs](../code-quality/includes/vsprvs_md.md)] VSIX 项目。 可以通过搜索 "vsix" 在 "**新建项目**" 对话框中找到 VSIX 项目模板。

2. 添加一个工具窗口，方法是添加一个名为 "`ProjectPropertiesToolWindow`" 的自定义工具窗口项模板。 在**解决方案资源管理器**中，右键单击项目节点，然后选择 "**添加** > **新项**"。 在 "**添加新项" 对话框**中，切换到 "**可视C#项** > **扩展性**"，然后选择 "**自定义工具窗口**"。 在对话框底部的 "**名称**" 字段中，将文件名更改为 `ProjectPropertiesToolWindow.cs`。 有关如何创建自定义工具窗口的详细信息，请参阅[使用工具窗口创建扩展](../extensibility/creating-an-extension-with-a-tool-window.md)。

3. 生成解决方案并确认编译时不会产生错误。

### <a name="to-display-project-properties-in-a-tool-window"></a>在工具窗口中显示项目属性

1. 在 ProjectPropertiesToolWindowCommand.cs 文件中，添加以下 using 指令。

    ```csharp
    using EnvDTE;
    using System.Windows.Controls;

    ```

2. 在*ProjectPropertiesToolWindowControl*中，从 "工具箱" 中删除现有按钮并添加 TreeView。 还可以从*ProjectPropertiesToolWindowControl.xaml.cs*文件中删除 click 事件处理程序。

3. 在*ProjectPropertiesToolWindowCommand.cs*中，使用 `ShowToolWindow()` 方法打开项目并读取其属性，然后将属性添加到 TreeView。 ShowToolWindow 的代码应如下所示：

    ```csharp
    private void ShowToolWindow(object sender, EventArgs e)
    {
        ToolWindowPane window = this.package.FindToolWindow(typeof(ProjectPropertiesToolWindow), 0, true);
        if ((null == window) || (null == window.Frame))
        {
            throw new NotSupportedException("Cannot create window.");
        }
        IVsWindowFrame windowFrame = (IVsWindowFrame)window.Frame;
        Microsoft.VisualStudio.ErrorHandler.ThrowOnFailure(windowFrame.Show());

        // Get the tree view and populate it if there is a project open.
        ProjectPropertiesToolWindowControl control = (ProjectPropertiesToolWindowControl)window.Content;
        TreeView treeView = control.treeView;

        // Reset the TreeView to 0 items.
        treeView.Items.Clear();

        DTE dte = (DTE)this.ServiceProvider.GetService(typeof(DTE));
        Projects projects = dte.Solution.Projects;
        if (projects.Count == 0)   // no project is open
        {
            TreeViewItem item = new TreeViewItem();
            item.Name = "Projects";
            item.ItemsSource = new string[]{ "no projects are open." };
            item.IsExpanded = true;
            treeView.Items.Add(item);
            return;
        }

        Project project = projects.Item(1);
        TreeViewItem item1 = new TreeViewItem();
        item1.Header = project.Name + "Properties";
        treeView.Items.Add(item1);

        foreach (Property property in project.Properties)
        {
            TreeViewItem item = new TreeViewItem();
            item.ItemsSource = new string[] { property.Name };
            item.IsExpanded = true;
            treeView.Items.Add(item);
        }
    }
    ```

4. 生成项目并启动调试。 应显示实验实例。

5. 在实验实例中，打开一个项目。

6. 在  > **其他窗口**的**视图**中，单击 " **ProjectPropertiesToolWindow**"。

  "工具" 窗口中会显示树控件，其中包含第一个项目及其所有项目属性的名称。
