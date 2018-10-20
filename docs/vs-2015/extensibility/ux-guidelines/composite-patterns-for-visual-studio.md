---
title: Visual Studio 的复合模式 |Microsoft Docs
ms.custom: ''
ms.date: 11/15/2016
ms.prod: visual-studio-dev14
ms.reviewer: ''
ms.suite: ''
ms.technology:
- vs-ide-sdk
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: e48ecfb2-f4b5-4d3a-b4a2-7a4d62fa4ec0
caps.latest.revision: 9
ms.author: gregvanl
manager: ghogen
ms.openlocfilehash: 9ca83b58de966a8ddc54252e61a8717b4213cdfe
ms.sourcegitcommit: 9ceaf69568d61023868ced59108ae4dd46f720ab
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/12/2018
ms.locfileid: "49182762"
---
# <a name="composite-patterns-for-visual-studio"></a>Visual Studio 的复合模式
[!INCLUDE[vs2017banner](../../includes/vs2017banner.md)]

复合模式组合不同配置中的交互和设计元素。 在 Visual Studio 中的一致性方面的最重要复合模式包括：  
  
-   [数据可视化](../../extensibility/ux-guidelines/composite-patterns-for-visual-studio.md#BKMK_DataVisualization)  
  
-   [对象上的 UI 和扫视](../../extensibility/ux-guidelines/composite-patterns-for-visual-studio.md#BKMK_OnObjectUI)  
  
-   [选择模型](../../extensibility/ux-guidelines/composite-patterns-for-visual-studio.md#BKMK_SelectionModels)  
  
-   [持久性和保存设置](../../extensibility/ux-guidelines/composite-patterns-for-visual-studio.md#BKMK_PersistenceAndSavingSettings)  
  
-   [触摸输入](../../extensibility/ux-guidelines/composite-patterns-for-visual-studio.md#BKMK_TouchInput)  
  
##  <a name="BKMK_DataVisualization"></a> 数据可视化  
  
### <a name="overview"></a>概述  
 图表是以直观的方式聚合和可视化数据以增强的决策制定。 它们可以帮助用户面对大量数据，但很少，则表示看到什么值得我们关注和什么可能需要有一个动作。  
  
 如果任意下列条件为 true，用户将在图表中受益：  
  
-   图表将帮助用户识别他们可以通过对操作的任务？  
  
-   图表将使用户能够预测可能更改的后果吗？  
  
-   图表将帮助用户发现趋势并识别模式？  
  
-   图表将允许用户做出更好的决策？  
  
-   图表将帮助解答用户可能在给定上下文中的特定问题？  
  
#### <a name="general-rules-for-charts"></a>图表的一般规则  
  
-   清楚地标签数据。 而无需说明图只是美感和实用性。  
  
-   开始为零，以避免扭曲比例的轴。 行长度和栏大小是重要的视觉提示，了解数据点之间的关系。  
  
-   创建图表，不信息图。 信息图非常艺术数据表示和其主要目标是 visual 故事分享。 图表可以 （而且应该） 是好的视觉效果，但让数据自己说话。  
  
-   避免 skeumorphism、 绘画条形图、 对比度井号标记和其他信息图收尾工作了。  
  
-   不要使用用作装饰元素的三维效果。 仅当使用这些这些用户能够理解信息真正的必要组成部分。  
  
-   避免使用多个行和填充，因为两个以上的颜色可以使这种图表类型难以阅读和正确解释。  
  
-   不使用图表 （或任何图） 作为了解一个概念或与数据交互的唯一方法。 这带来有视觉障碍的用户的问题。  
  
-   不要使用为剔除或装饰性元素在页面上的图表。 换而言之，如果图表不会添加任何值或帮助用户解决问题，请不要使用它。  
  
### <a name="chart-types"></a>图表类型  
 在 Visual Studio 中使用的图表类型包括条形图、 折线图、 已修改名为环形图或"环形图，"时间线，在饼图的散点图 （也称为"群集图表"） 和甘特图。 每种类型的图表可用于进行通信的不同类型的信息。  
  
### <a name="other-charting-considerations"></a>其他图表的注意事项  
  
#### <a name="color"></a>颜色  
 没有特定图表定义在 Visual Studio 中使用的颜色的调色板。 调色板颜色视力的主要类型的可访问，可以区分颜色，即使使用为范围非常窄扇区的颜色。 您可以使用这些颜色的任意组合为任何类型的图表或图形中你的 UI。 不需要使用所有七个颜色，如果不需要那么多不同的颜色。 这些颜色旨在不与任何前台元素中，因此不要放置文本或基于这些颜色的字形一起使用。 应硬编码和向下的用户自定义公开这些色调**工具 > 选项**(请参阅[为最终用户公开颜色](../../extensibility/ux-guidelines/colors-and-styling-for-visual-studio.md#BKMK_ExposingColorsForEndUsers))。  
  
|样本|十六进制|RGB|  
|------------|---------|---------|  
|![样本 71B252](../../extensibility/ux-guidelines/media/0711-71b252.png "0711_71B252")|# 71B252|113,178,82|  
|![样本 BF3F00](../../extensibility/ux-guidelines/media/0711-bf3f00.png "0711_BF3F00")|# BF3F00|191,63,0|  
|![样本 FCB714](../../extensibility/ux-guidelines/media/0711-fcb714.png "0711_FCB714")|# FCB714|252,183,20|  
|![样本 903F8B](../../extensibility/ux-guidelines/media/0711-903f8b.png "0711_903F8B")|# 903F8B|144,63,139|  
|![样本 117AD1](../../extensibility/ux-guidelines/media/0711-117ad1.png "0711_117AD1")|# 117AD1|17,122,209|  
|![样本 79D7F2](../../extensibility/ux-guidelines/media/0711-79d7f2.png "0711_79D7F2")|# 79D7F2|121,215,242|  
|![样本 B5B5B5](../../extensibility/ux-guidelines/media/0711-b5b5b5.png "0711_B5B5B5")|# B5B5B5|181,181,181|  
  
##  <a name="BKMK_OnObjectUI"></a> 对象上的 UI 和扫视  
 本部分提供到扫视，也称为代码查看视图中，一种类型的唯一的 Visual Studio 的对象上的 UI 上下文。  
  
### <a name="overview"></a>概述  
  
-   对象上的 UI 应为用户提供更多的信息或交互功能而无需使其主要任务。  
  
-   Visual Studio 中的对象上的 UI 的主模式被称为"关注点的信息。"  
  
-   Visual Studio 中的对象上的 UI 为内联或浮点和是持久或暂时。  
  
    -   代码窥视视图中，一种类型的对象上的用户界面在 Visual Studio 中，是内联和持久。  
  
    -   CodeLens，一种类型的对象上的用户界面在 Visual Studio 中，是浮点型和暂时性  
  
 了解如何在一段代码，或查找该代码，有关详细信息通常需要开发人员来切换上下文，请转到其他内容或另一个窗口。 因为如果他们离开其主窗口，用户可能会丢失其原始任务的焦点，则可能会造成干扰，这些上下文 shifts。 此外，获取原始上下文后，可能很困难，尤其是如果切换窗口导致原始代码，以便被遮盖的其他用户界面。  
  
 对象上的 UI 遵循模式称为"关注点的信息。" 这些消息、 弹出窗口和对话框为用户提供相关的附加信息添加说明或交互功能，而不会丢失专注于其主要任务。 对象上的用户界面的示例包括当用户悬停在通知区域中的图标、 拼写错误的单词，并在 Visual Studio 2013 中引入的速览视图下红色波浪线上其指针时显示的弹出窗口。  
  
### <a name="decision-points"></a>决策点  
 在 Visual Studio 中，有多种方式来使用此模式在关注点的信息。 选择正确的机制以及一致、 可预测的方式实现该功能是必要的整体体验。 否则，可能会降低焦点从内容本身的混乱或不一致的体验向用户。  
  
#### <a name="relationships-between-master-and-detail-content"></a>母版和详细信息内容之间的关系  
 在关注点的信息用于显示关系之间的内容，并且用户是专注于 （"主"内容） 和其他相关内容 （"详细信息"内容）。 在此模式下，显然用户正在使用，并且可以接近的主内容的内容与详细内容。 补充信息或不能汇总的烦扰主内容的信息应遵循另一种模式，如工具窗口。  
  
-   **始终**显示中紧靠主内容的详细信息内容。  
  
-   **始终**确保详细信息内容仍使用户能够专注于母版内容。 通常，实现此目的的最佳方式是呈现作为主内容以尽可能接近的详细信息内容。 这可以通过呈现的主内容旁边的弹出窗口中的详细信息内容或呈现下主内容的详细信息内容内联。  
  
-   **永远不会**使用在远离主内容，用户的关注点的信息。 如果用户需要分别查看详细信息内容，公开使用户能够执行此操作的显式操作。  
  
#### <a name="design-details"></a>设计详细信息  
 一旦您已确定对象上的 UI 是正确的选择，有四个主要设计注意事项：  
  
1.  **持久性：** 内容应为持久或暂时？   
    用户想要保持可见引用或与之交互的信息？ 或者，将用户想要快速查看信息，然后继续其主要任务？  
  
2.  **内容类型：** 将内容是信息性、 可操作，或导航？   
    用户是否需要有关主内容的其他详细信息？ 用户是否需要完成的任务，会影响主内容？ 或者，用户需要定向到另一个资源？  
  
3.  **指示器类型：** 环境指示器意义？   
    可以是有用的方式汇总和显示信息但不使主内容？  
  
4.  **笔势：** 什么手势将用于调用和解除 UI？   
    如何将用户显示的详细信息内容并将其发送消失吗？ 有值中添加某种表示，例如固定临时和持久状态之间切换吗？  
  
 每个四个决策点会影响在对象上的用户界面的主要组件。  
  
### <a name="on-object-ui-components"></a>对象上的 UI 组件  
  
1.  容器 （内容表示器） 类型  
  
    -   浮动  
  
    -   内联  
  
2.  内容类型  
  
    -   可能是静态或动态的信息： 数据  
  
    -   更改主内容的可操作： 命令  
  
    -   将用户转到另一个窗口或应用程序，如 MSDN 的导航： 链接  
  
3.  笔势  
  
    -   调用  
  
    -   开除  
  
    -   固定  
  
    -   其他交互  
  
4.  持久性和提交模型  
  
    -   暂时  
  
    -   Durable  
  
    -   自动  
  
    -   按需  
  
5.  环境指示器 （可选）  
  
    -   波浪线下划线  
  
    -   智能标记图标  
  
    -   其他环境的指标  
  
#### <a name="container-content-presenter-type"></a>容器 （内容表示器） 类型  
 有两个主要选项可用于显示在关注点的内容：  
  
1.  **内联：** 内联表示器，如查看视图中 Visual Studio 2013 代码编辑器中，引入的使通过转换现有内容的新内容的空间。  
  
    -   **更喜欢**提供内联演示者如果认为用户会想要花费大量时间引用或内容与之交互。  
  
    -   **避免**内联表示器，如果希望用户会想要查看存在，然后继续进行几乎不干扰其主要任务的信息。  
  
2.  **浮动：** 浮点表示器位于所选内容以尽可能接近但不会更改现有内容的布局。 可以使用各种策略，例如，通过显示一个浮动内容面板，最近的可用与所选符号的空白区域。  
  
    -   **更喜欢**浮点表示器，如果希望用户会想要查看存在，然后继续进行几乎不干扰其主要任务的信息。  
  
    -   **避免**浮点表示器，如果希望用户将需要花费大量时间引用或内容与之交互，存在。  
  
#### <a name="content-type"></a>内容类型  
 有三种主要类型的可显示在任何对象上的 UI 容器内的内容。 可以显示这些类型的信息的任意组合。 三种类型是：  
  
1.  **信息：** 大多数对象上的 UI 容器会显示某种类型的信息性内容。 内容可以表示环境的当前状态有关的信息，也可能表示环境的潜在未来的状态有关的信息。 例如，它可以用于显示特定命令，例如重构、 对现有代码的效果。  
  
    -   **始终**使用的规范表示形式，显示的信息。 例如，代码应如下所示代码中完成，但语法突出显示，并应采用任何字体和其他用户已设置的环境设置。  
  
    -   **始终**需要考虑的信息性内容，如果主内容形式显示相同的信息可能会通过支持的任何操作。 例如，如果提供的对象上的 UI 容器内的现有代码，强烈建议考虑支持浏览和修改该代码的能力。  
  
    -   **始终**如果考虑使用不同的背景色显示信息性内容表示潜在未来的状态。  
  
2.  可操作： 某些对象上的 UI 容器将提供的对 master 的内容，如执行重构操作执行某些操作的能力。  
  
    -   **始终**定位独立于信息性内容可操作的命令。  
  
    -   **始终**启用和禁用时相应的操作。  
  
    -   **始终**表示对话框中的命令的标准准则，请参阅。  
  
    -   **始终**在对象上的 UI 容器绝对中公开的操作的数量保持在最小。 与对象上的 UI 交互应为轻量、 快速的体验。 用户应该不需要付出上管理的对象上的 UI 容器本身的能源。  
  
    -   **始终**考虑如何以及何时将关闭或已解除对象上的 UI 容器。 最佳做法是，最后，对话框中的母版和详细信息内容之间的任何操作也应关闭对象上的 UI 容器，调用该操作时。  
  
3.  **导航：** 一些对象上的 UI 容器包括将用户转到另一个窗口或应用程序，如用户的 web 浏览器中打开 MSDN 文章的链接。  
  
    -   **始终**前面预置"打开"与任何导航链接，以便用户将不会感到惊讶要导航到某些其他内容。  
  
    -   **始终**单独从可操作链接的导航链接。  
  
#### <a name="ambient-indicators-optional"></a>环境指示器 （可选）  
 环境的指示器可以是细微，包括颜色的对比色其余代码，从显示的文本或显而易见的包括 tickler 符号，如波浪线下划线和智能标记图标。 环境的指标进行通信相关的附加信息的可用性。 理想情况下，它们提供有用的信息甚至无需用户与之交互。  
  
-   **始终**定位环境指示器，以便它不会分散注意力或严重影响用户。 如果无法定位环境指示器的方式，请考虑另一种解决方案。  
  
-   **始终**环境指示器与相关的内容尽可能近的位置。  
  
-   **始终**尝试创建一个指示器，总结了可通过其的信息。 请考虑提供可用的数据项目数的计数 (例如，"3 个引用"而不是只需"引用"） 或可以看作是通过其他方式来汇总数据。  
  
    -   在其中的指示符不能始终会计算和显示的数据的情况下，立即考虑提供渐进式反馈，因为计算的值。 例如，考虑反映到可用的数据，类似于 Windows Phone 上的电子邮件实时磁贴刷新作为数目的增加而未读电子邮件的方式更新的更改进行动画处理。  
  
-   **永远不会**添加多个指示器不是用户可以合理地采取的一段给定的内容。 环境指示器应非常有用，而无需任何用户交互。 如果它们需要溢出和其他管理控件以将它们放入视图，指示器会丢失其环境。  
  
#### <a name="gestures"></a>笔势  
 允许用户继续关注于主内容的一个重要方面是通过支持右的手势，即可打开和关闭更多详细信息内容。  
  
-   **始终**需要用户执行某些显式手势，若要打开其他内容。 常见的打开手势包括：  
  
    -   **悬停：** 工具提示或信息性的非交互式内容  
  
    -   **显式命令：** 内联表示器  
  
    -   **双击环境指示器：** CodeLens 弹出窗口  
  
-   **始终**每当用户按 Esc 键消除详细内容。  
  
-   **始终**考虑对象上的 UI 上下文。 允许在容器内的交互的内容表示器，仔细考虑是否可能会中断用户的工作流上悬停时，显示的其他信息。  
  
-   **永远不会**似乎是可编辑或邀请用户交互的悬停时的显示内容。 如果用户尝试将光标移动到详细信息内容，如工具提示的标准行为是以立即关闭游标，当位于时不再主生成它的内容，此行为可能会对用户。  
  
##  <a name="BKMK_SelectionModels"></a> 选择模型  
  
### <a name="overview"></a>概述  
 所选内容模型是用来指示，确认用户界面中的感兴趣的一个或多个对象执行操作的机制。 本主题讨论在 Visual Studio 文档编辑器中的选择交互模式： 文本编辑器、 设计曲面和建模图面。  
  
 用户必须具有一种方法向 Visual Studio 指明它们在做什么，和 Visual Studio 必须以可预测方式反馈对响应用户了解其上操作。 差异或在用户和用户界面之间出现沟通错误可能会导致用户不会注意到的操作，这会产生意想不到的后果。 通常情况下，该错误不明显，直到用户将看到的内容缺失或已更改。 因此所选内容模型是一个最关键的用户界面设计。 在 Visual Studio 中的所选内容模型是与 Windows 保持一致，尽管存在一些细微的变体。  
  
 在 Visual Studio 中，如下所示 Windows，选择模型而异的交互的上下文。 选择会出现四种类型的对象：  
  
-   Text  
  
-   图形对象  
  
-   列表和树  
  
-   网格  
  
 这些对象中有三种类型的选择：  
  
-   连续  
  
-   不连续的  
  
-   Region  
  
#### <a name="scope"></a>范围  
 所选内容的最重要的组件确保用户知道他们在哪个窗口中处理 （激活） 和其中的重点是位于 （选择）。 Visual Studio 扩展的窗口管理功能在 Windows，但激活方案是相同的： 与窗口交互将焦点移到窗口。 Visual Studio 都有两个指标进行激活： 一个用于文档窗口，一个工具窗口。  
  
 对于文档窗口中，活动窗口为由即将到前面并更改其背景颜色的文档窗口选项卡：  
  
 ![Visual Studio 中的活动选项卡上选择](../../extensibility/ux-guidelines/media/0713-01-activetab.png "0713年 01_ActiveTab")  
  
 **活动选项卡选择**  
  
 用于工具窗口，在工具窗口的标题栏区域的颜色的更改表示活动窗口：  
  
 ![Visual Studio 中的活动工具窗口中选择](../../extensibility/ux-guidelines/media/0713-02-activetoolwindow.png "0713年 02_ActiveToolWindow")  
  
 **活动工具窗口显示节点的主要选择**  
  
 ![在 Visual Studio 中的非活动工具窗口选项](../../extensibility/ux-guidelines/media/0713-03-inactivetoolwindow.png "0713年 03_InactiveToolWindow")  
  
 **显示潜在选择的节点处于非活动状态的工具窗口**  
  
 后一个窗口处于活动状态，根据指南的此部分中所述的所选内容模型表示其焦点。  
  
#### <a name="context"></a>上下文  
 Visual Studio 设计为保留的上下文中，强概念跟踪的用户正在使用。 只能有一个窗口处于活动状态，，无论它是工具或文档窗口。 但是，最顶层的文档窗口始终保留导致的延迟所选内容。 尽管焦点的工具窗口中，最后处于活动状态的文档窗口显示所选内容，即使在非活动状态。 这样做是为了保留他们已编辑的向他们展示 Visual Studio 已保留其状态，以便它们可以返回和工具窗口和文档窗口之间无缝移动的文档中的用户的上下文。  
  
### <a name="text-selection"></a>文本选择  
 是严格文本，例如内置的文本编辑器的 visual Studio 编辑器使用相同的文本选择模型和中所述的外观[鼠标和指针](https://msdn.microsoft.com/library/dn742466.aspx)页上的 Windows 上的用户体验交互准则MSDN。 由调用插入点的垂直条指示在文本编辑器中的输入的焦点。 插入点位于单像素胖和彩色的逆向它后面出现的任何内容。 根据设置的速率，它就闪烁**光标闪烁速率**中设置**速度**选项卡**键盘**中控制面板小程序。  
  
#### <a name="contiguous-and-disjoint-selection"></a>连续和非连续选择  
 在文本编辑器中的选择仅为连续。 不连续的文本选择不允许使用，但应在图形对象编辑器中解决。 当用户的鼠标指针位于文本区域内时，光标将变为 i 形标记。 一次单击将插入点放在文本编辑器中单击位置中。 按住鼠标按钮开始选择突出显示和松开鼠标按钮结束选择突出显示。  
  
#### <a name="region-selection-box-selection"></a>选择 （选择框）  
 在文本编辑器中，visual Studio 支持区域选择，这称为框选择。 选择框允许用户选择不遵循常规文本流的文本的区域。 使用标准文本选择，因为所选内容必须是连续的。 使用鼠标拖动时按住 Alt 键可启动框选择。 此外可以通过使用箭头键以指示所选内容的区域的同时按住 Alt 和 Shift 键启动框选择。 选择框使用正常选择项突出显示，并显示插入点光标闪烁的选择区域的末尾。  
  
 ![区域&#40;框&#41;Visual Studio 中的选择](../../extensibility/ux-guidelines/media/0713-04-boxselection.png "0713年 04_BoxSelection")  
  
 **Visual Studio 中的区域 （框） 选择**  
  
#### <a name="text-selection-appearance"></a>所选内容的文本外观  
 可以自定义用于在编辑器中的活动和非活动选择的颜色。 若要自定义编辑器的可视外观，用户可以转到**工具 > 选项**，然后查看下**环境 > 字体和颜色 > 文本编辑器**。  
  
### <a name="graphical-selection"></a>图形所选内容  
  
#### <a name="interaction"></a>交互  
 图形对象选择可能很复杂，并取决于许多因素影响：  
  
-   **在编辑器的主要选择模型。** 此外可以使用包含图形对象的编辑器编辑文本或网格。 例如，在编辑器可能是一个基于文本的编辑器，还支持图形对象，例如 Visual Studio XAML 设计器的位置。 支持多个对象类型可能会影响如何在用户选择的不同类型的对象组成的组。  
  
-   **对主要和次要选择状态的支持。** 编辑器可以提供主要和次要选择状态，以便可以统一，编辑对象相互对齐，调整大小在一起，依此类推。  
  
-   **在就地编辑支持。** 编辑器还可以允许要编辑其图形对象的内容。 例如，矩形形状还可能包含用户可以更改内部文本。 此外，该文本可以居中或两端对齐。 就地编辑涉及用户交互的更详细的级别，因此需要一组适当的视觉提示用于向用户显示的状态信息。  
  
#### <a name="mouse-interaction"></a>鼠标交互  
  
|输入|结果|  
|-----------|------------|  
|单击未选定的对象|选择的对象，并可调整大小的对象是否显示为虚线和选择控点。|  
|单击所选的对象|激活就地编辑如果对象支持它。 单击对象以外的区域将停用的就地编辑模式。|  
|双击对象|将打开代码隐藏的对象进行编辑，并可能插入默认事件处理程序，如果适用。|  
|指向对象|将指针更改为移动光标。 对象的外观，例如其亮度或颜色，可能会更改。|  
|指向一个选择控点|将指针更改为大小调整光标。 对于支持旋转的对象，一些选择句柄可能会更改指针为旋转游标在指针位于相对于选择控点以不同的方式 （例如，远一点移动）。|  
|拖动|即使以前未选择对象，将指针更改为移动光标，并将对象移动。|  
|编辑器失去焦点|尽管对象所保留的内容和外观，它必须在其最后一个操作/选择状态期间将停用的就地编辑模式。|  
|对象选择|指示边框点线或其他视觉上不同的处理方式，以突出显示的对象的边界。|  
|调整所选的对象的大小|指示所选内容句柄。<br /><br /> 可调整大小的对象具有八个句柄，表示在其中可以调整大小的每个方向。 如果仅可以在特定方向上调整大小的对象，则可能使用较少的句柄。 当用户调整到其中八个句柄不会交互的对象的大小时，可能会使用四个句柄。 句柄大小应绑定到与窗口边框和边缘指标**GetSystemMetrics**大小成比例地显示分辨率的 API 函数。<br /><br /> ![重设大小句柄](../../extensibility/ux-guidelines/media/0713-05-resizehandles.png "0713年 05_ResizeHandles")|  
|旋转选定的对象|![旋转句柄](../../extensibility/ux-guidelines/media/0713-06-rotate.png "0713年 06_Rotate")|  
  
#### <a name="keyboard-interaction"></a>键盘交互  
  
|输入|结果|  
|-----------|------------|  
|Tab|在编辑器中移动对象的逻辑顺序之间的焦点指示器。 这可能是从左到右或从上到下具体取决于**TabIndex** （或等效） 属性值、 对象创建过程中的顺序和编辑器的全部用途。 按 shift + Tab 反转方向焦点指示器。|  
|空格键|激活平移模式，而维护击键。 需要其他鼠标输入若要进行平移的视区位置。|  
|Ctrl+空格键|而击键维护激活缩放模式。 需要其他鼠标输入以增大或减小缩放系数。|  
|Ctrl + Alt + 减号|降低一个级别的缩放因子。|  
|Ctrl + Alt + 加号|增加了一个级别的缩放因子。|  
|Shift 或 Ctrl|将对象添加到选择组。 Ctrl 还可以从选择组分别删除对象。|  
|Enter|执行默认命令的对象 （通常会打开或编辑）。|  
|F2|激活就地编辑的对象。|  
|箭头键|在按下，以较小增量 （例如，一次的 1 像素） 中的箭头键的方向上移动所选的对象|  
|Ctrl + 箭头键|将所选的对象移动的方向箭头键按下，较大的增量 （例如，一次的 10 个像素）|  
|Shift + 箭头键|调整在各自的方向，以较小增量 （例如，一次的 1 像素） 中所选的对象的大小|  
|Ctrl + Shift + 箭头键|调整在各自的方向，以更大的增量 （例如，一次的 10 个像素） 中所选的对象的大小|  
  
 当用户编辑控件中的位置时，可能会有用的对象自动调整大小与用户输入。 例如，如果用户编辑标签控件，然后标签应增长以显示用户只需键入的文本。 如果不执行此操作，用户必须调整控件的大小手动编辑文本后。 如果用户具有很多控件，这将成为机械和用于生产的任务。  
  
#### <a name="graphical-containers"></a>图形容器  
 在某些情况下，图形编辑器的其他图形对象，如 Windows 窗体面板控件或 HTML 设计器中的网格布局控件提供容器。 如果你的编辑器的其他图形对象提供容器，然后应为容器仅 （上面所述的标准模型作为容器按照中的对象） 使用以下所选内容模型：  
  
|输入|结果|  
|-----------|------------|  
|在容器上单击|选定的容器对象而无需直接选择任何包含的对象。 容器可以移动和/或调整大小与标准鼠标和键盘输入 （如上文所述）。 包含的对象在关系中移动到容器，但除非直接选择它们包含的对象无法调整大小。|  
|将鼠标悬停容器的边界区域|移动光标，指示可以移动该容器会变成鼠标。|  
|将容器的边界区域|移动光标将变为鼠标并将移动容器 （和中包含的对象）。 容器不能移动而不必首先单击一次选择。|  
|单击在容器内的对象|取消选择该容器，（如果选择），选择仅被单击的对象。|  
|Shift + 单击或 Ctrl + 单击包含的对象和/或容器|将被单击的对象添加到现有选择或选择组。 如果被单击的对象已选择组的成员，它是从选择组中删除。|  
  
 包含的对象应遵循基本的选择模型，在上一部分中所述。 从可用性测试的 Windows 窗体设计器，用户应包含的对象的无缝访问而无需干预的步骤 （施加的包含对象）。  
  
#### <a name="disjoint-and-region-selections"></a>不连续的和区域选择  
 图形对象编辑器应支持不连续的选择。 请注意，此图不显示 Visual Studio 的控件外观。 请参阅[图形对象选择外观](../../extensibility/ux-guidelines/composite-patterns-for-visual-studio.md#BKMK_GraphicalObjectSelectionAppearance)详细 visual 规范。  
  
 ![不连续的和区域选择器](../../extensibility/ux-guidelines/media/0713-07-disjointregionselectors.png "0713年 07_DisjointRegionSelectors")  
  
 **不连续的所选内容**  
  
 图形编辑器还应提供字幕类型选择指示器的区域选择。 如果图形编辑器支持其他对象类型 （如文本），然后选择区域可能无法根据这些其他对象类型的约束。  
  
 ![字幕选择](../../extensibility/ux-guidelines/media/0713-08-marqueeselection.png "0713年 08_MarqueeSelection")  
  
 **字幕选择**  
  
#### <a name="primary-and-secondary-selections"></a>主要和次要选择  
 某些图形对象编辑器允许用户编辑或对齐组中的对象。 在这种情况下，主要和次要选择内容的概念需要，将会推出。 主要选择是所有其他对象响应操作进行分组的对象。 用户首先选择的对象将成为主控件，并且后续选择成为辅助的选择。 为主选择已从辅助的选择，以指示哪些对象是主要的非重复 visual 处理方法：  
  
 ![主要和次要选择](../../extensibility/ux-guidelines/media/0713-09-primarysecondary.png "0713年 09_PrimarySecondary")  
  
 **使用两个辅助选择的主要选择**  
  
####  <a name="BKMK_GraphicalObjectSelectionAppearance"></a> 图形对象选择外观  
 选择句柄是围绕对象的边界框矩形模式中绘制的平方。 下图显示了图形对象可以具有句柄、 大小调整，与就地编辑外观的各种状态的示例。 句柄的大小应绑定到窗口边框和指标使用边缘**GetSystemMetrics** API。  
  
|状态|外观|视觉对象详细信息|  
|-----------|----------------|--------------------|  
|**未选定**|默认|![这是默认按钮状态](../../extensibility/ux-guidelines/media/0713-10-defaultstate.png "0713年 10_DefaultState")||  
|**主要选择**|可调整大小|![使用主选择调整大小图柄](../../extensibility/ux-guidelines/media/0713-11-primaryresize.png "0713年 11_PrimaryResize")|![使用主选择调整大小图柄&#40;缩放&#41;](../../extensibility/ux-guidelines/media/0713-12-primaryresizezoom.png "0713年 12_PrimaryResizeZoom")|  
|**主要选择**|不可调整大小|![为主选择，而无需重设大小句柄](../../extensibility/ux-guidelines/media/0713-13-primarynoresize.png "0713年 13_PrimaryNoResize")|![为主选择，而无需调整大小图柄&#40;缩放&#41;](../../extensibility/ux-guidelines/media/0713-14-primarynoresizezoom.png "0713年 14_PrimaryNoResizeZoom")|  
|**主要选择**|锁定|![锁定的主要选择](../../extensibility/ux-guidelines/media/0713-15-primarylocked.png "0713年 15_PrimaryLocked")|![锁定的主要选择&#40;缩放&#41;](../../extensibility/ux-guidelines/media/0713-16-primarylockedzoom.png "0713年 16_PrimaryLockedZoom")|  
|**次要选择**|可调整大小|![使用的次要选择调整大小图柄](../../extensibility/ux-guidelines/media/0713-17-secondaryresize.png "0713年 17_SecondaryResize")|![使用的次要选择调整大小图柄&#40;缩放&#41;](../../extensibility/ux-guidelines/media/0713-18-secondaryresizezoom.png "0713年 18_SecondaryResizeZoom")|  
|**次要选择**|不可调整大小|![没有的次要选择调整大小图柄](../../extensibility/ux-guidelines/media/0713-19-secondarynoresize.png "0713年 19_SecondaryNoResize")|![没有重设大小的次要选择&#40;缩放&#41;](../../extensibility/ux-guidelines/media/0713-20-secondarynoresizezoom.png "0713年 20_SecondaryNoResizeZoom")|  
|**次要选择**|锁定|![锁定的次要选择](../../extensibility/ux-guidelines/media/0713-21-secondarylocked.png "0713年 21_SecondaryLocked")|![锁定的次要选择&#40;缩放&#41;](../../extensibility/ux-guidelines/media/0713-22-secondarylockedzoom.png "0713年 22_SecondaryLockedZoom")|  
|**UI 活动**|默认|![UI 活动状态](../../extensibility/ux-guidelines/media/0713-23-uiactive.png "0713年 23_UIActive")|![UI 活动状态&#40;缩放&#41;](../../extensibility/ux-guidelines/media/0713-24-uiactivezoom.png "0713年 24_UIActiveZoom")|  
  
### <a name="view-selection-models"></a>查看所选内容模型  
  
#### <a name="tree-view"></a>树视图  
 树视图中的选择是使用简单的突出显示所示。 如果用户单击节点名称或节点图标上时，将成为选择的节点。 展开左侧的节点的三角形标志符号或协定树控件，但不是会影响用户的选择，有一个例外： 时折叠父节点，该节点的子节点上进行选择后，选择内容将移到父级。  
  
 ![在 Visual Studio 中的典型树视图](../../extensibility/ux-guidelines/media/0713-25-treeview.png "0713年 25_TreeView")  
  
 **在 Visual Studio 中的典型树视图**  
  
 树视图可以支持连续和非连续的选择，即使在树中的多个级别。 连续或非连续必须可见的树节点上进行多个选择。 如果节点处于折叠状态，不连续的所选内容将丢失，并且已折叠的节点获取所选内容。 在这种方式，用户可以看到将受操作影响的节点。 如果节点处于折叠状态，它变得不清晰的节点可能会受到影响。  
  
 选择父节点后，可能适合为适用于父级和子级的所有操作的情况下，但在应到父级，应用该操作。 在这种情况下，在操作时，如复选框或确认对话框，可使用户显式的"应用于所有子项"选项提供附加 UI。  
  
##### <a name="renaming"></a>重命名  
 如果在树中的节点支持重命名，则应在位置完成重命名。 适当地操作应在 Visual Studio 中的所有树控件中为标准。 提供立即激活选定涵盖整个节点的名称，已准备好接受用户输入的文本，就地编辑模式时，重命名命令。 如果该节点表示一个文件，文件名应包含扩展名。 选择突出显示应包括仅文件名和未扩展的主体。  
  
|输入|结果|  
|-----------|------------|  
|Enter 键|提交重命名操作|  
|Esc 键|取消重命名操作|  
|在就地编辑区域外单击|提交重命名操作|  
|撤消|提供简单的撤消取消重命名操作|  
  
#### <a name="selection-within-lists-and-grid-controls"></a>列表和网格控件中的选定内容  
 在该列表中选择的关键概念在于，它是基于行的这意味着，整个行做出选择时选择作为一个单元。 与此相反，网格可以允许特定的单元格，而不会影响任何其他方面的行的选择。 网格还可能包含嵌套行 （如树网格） 允许整个分支层次结构以取消选择通过与父行交互和所选的层次的结构。 在整行数据的简单突出显示颜色显示为列表中的选择。 通过单个像素的虚线边框环绕当前可编辑的行或单元格 （如果所有单元格是只读的则为行） 显示焦点。  
  
> [!NOTE]
>  **焦点**并**选择**是不同的概念。 *焦点*哪种 UI 的目标元素指示可接收的输入未显式定向到另一个对象，同时*选择*表示的对象包含在后续的对象的一组状态操作可能会发生。  
  
 在列表中的选择可能是连续的、 连续的或区域。 当多选，允许的连续未分配和非连续选择应始终受支持，同时对区域 （框） 选择的支持是可选的。 区域的选择是通过拖动列表正文的空白区域启动的。  
  
|对象|选择|  
|------------|---------------|  
|列表|连续|始终受支持的 （如果允许多重选择）。|  
|列表|不连续的|始终受支持的 （如果允许多重选择）。|  
|列表|Region|可选支持。 由列表正文空白区域中拖动鼠标来启动。|  
  
 单击列表上的一次可以选择单击发生处的行。 如果用户在支持就地编辑的列表单元格中单击，以进行就地编辑也会立即激活该单元格。 否则为整行立即选中，并且显示突出显示。  
  
 拖动列表正文中会执行三个操作之一：  
  
-   如果列表支持它并且鼠标按下的空白区域在，启动区域选择  
  
-   如果列表单元格或行支持正在拖动源，启动拖/放操作  
  
-   选择当前行  
  
##### <a name="in-place-editing"></a>在就地编辑  
 允许就地编辑时有两种基本模式： 简单编辑的控件和属性选取器。 使用简单的编辑控件，内容是突出显示并可供用户输入就立即激活就地编辑。 属性选取器实现时，激活就地编辑模式，并不突出显示当前所选内容后，会显示属性选取器将调用该按钮。 在单元格中应为右对齐选取器按钮。 有关就地编辑示例，请参阅**属性窗口**并**任务列表**Visual Studio 中。  
  
##### <a name="keyboard-support"></a>键盘支持  
 选择列表和网格中的键盘支持如下所示的标准 Windows 约定：  
  
-   箭头键导航列表中，选择移动焦点时，为每个行/单元格。  
  
-   Shift + 箭头的方向箭头键执行连续选择。  
  
-   Ctrl + 箭头之间添加和删除列表项从选择创建非连续选择后跟空格键切换。  
  
-   包含嵌套层次结构的网格，向右箭头键展开父行，并向左箭头键折叠其中一个。  
  
-   如果单元格会是可编辑，Tab 键将焦点移在当前行中单元格之间。  
  
-   Enter 键在列表中的项执行默认命令 (通常**打开**)。  
  
-   F2 键时激活就地编辑当前所选单元格。  
  
##  <a name="BKMK_PersistenceAndSavingSettings"></a> 持久性和保存设置  
  
### <a name="overview"></a>概述  
 尽管 Visual Studio 中的每个软件组件通常负责其自己的状态和持久性，但 Visual Studio 会自动将设置保存在某些情况下，例如与窗口大小和位置。 下表是自动保存的设置和设置需要的显式用户或编程所采取操作的组合。  
  
|对象|要保存|当保存|保存的位置|  
|------------|------------------|------------------|-------------------|  
|可选择对象 （例如，一行代码）|断点的代码行上<br /><br /> 关联的代码行与一个用户快捷方式|保存项目时|**用户选项 (.suo)** 项目文件|  
|对话框|对话框中，如果它已被移动的位置<br /><br /> 用户上次使用对话框中的视图|在对话框关闭时<br /><br /> 当 Visual Studio 会话结束时|在内存中<br /><br /> 注册表中的**HKEY_Current_User**|  
|窗口|大小和窗口的位置|在窗口关闭时<br /><br /> Visual Studio 模式发生更改时<br /><br /> 当 Visual Studio 会话结束时|**用户选项 (.suo)** 项目文件<br /><br /> 窗口设置的自定义选项文件|  
|Document|在文档中当前所选内容<br /><br /> 该文档的视图<br /><br /> 用户访问过的最后一个的多个位置|当保存文档|**用户选项 (.suo)** 项目文件|  
|项目|对文件的引用<br /><br /> 对磁盘上的目录的引用<br /><br /> 对其他软件的引用<br /><br /> 组件数<br /><br /> 有关项目本身的状态信息|保存项目时|项目文件|  
|解决方案|对项目的引用<br /><br /> 对文件的引用|当保存项目或解决方案|**解决方案 (.sln)** 文件|  
|中的设置**工具 > 选项**|键盘自定义项<br /><br /> 工具栏自定义<br /><br /> 配色方案|当**工具 > 选项**对话框关闭<br /><br /> 当 Visual Studio 会话结束时|注册表中的**HKEY_Current_User**|  
  
 用户执行操作，并当他们执行的操作，决定了设置是否要保存在保存到磁盘 （跨会话作为注册表设置），（会话），期间的内存中的项目或解决方案文件本身，作为一部分的一部分**解决方案选项 (.suo)** 文件，或如自定义设置文件，只有该软件组件知道有关。 上表显示的设置可以保存多个事件。 但是，有其他的情况下在其中你可能想要保存状态：  
  
-   当用户更改对话框或窗口内的位置  
  
-   用户将焦点转移到另一个窗口的时  
  
-   当用户切换不同的设计，以调试模式  
  
-   当在用户注销其帐户  
  
-   当计算机进入休眠状态或关闭  
  
-   计算机/硬盘驱动器即将时重新格式化和重新设置  
  
### <a name="window-configurations"></a>窗口配置  
 窗口配置为在开发环境的基本演示-它是一种方案包含的工具窗口，则列表和排列它们的方式。 对于由 IDE （IDE 窗口） 的 windows，布局信息将保留每个用户，因此当用户启动 IDE，窗口布局显示相同时它们上次退出 Visual Studio。 状态和 IDE 窗口的位置是以 XML 格式的自定义选项文件中保留的。 创建的包加载到 IDE 的工具窗口保留其在注册表中的状态信息和可能或可能不是每个用户。  
  
#### <a name="profile-specific-layouts"></a>特定于配置文件的布局  
 每个配置文件包含工具窗口布局，以熟悉特定的开发人员角色的方式组织 (Visual c + + 开发人员希望看到**解决方案资源管理器**IDE，而 C# 开发人员希望看到左侧**解决方案资源管理器**右侧)。 在用户选择的配置文件在启动时加载特定于配置文件的窗口布局。 包创建者应确定窗口布局最适合其客户的体验，了解用户对窗口配置更改随后会保存。  
  
##  <a name="BKMK_TouchInput"></a> 触摸输入  
 在触摸设备上，用户正在越来越多地使用 Microsoft 开发产品。 但是，有一些难以在触摸设备上使用开发工具的障碍。 用户希望我们的产品提供可靠、 精确的触控体验。 这些指南的目的是通知有关触摸功能合并，并鼓励一致的触摸体验跨 Visual Studio 和相关的产品的决策。  
  
### <a name="levels-of-experience"></a>级别的体验  
 以下级别的体验旨在作为指南来帮助团队确定哪些触控功能提供根据触摸投资关注其所需级别。  
  
-   **基本体验**是团队想要提供触摸功能，因此没有在其工作整个岔路。  
  
-   **优化体验**可供团队在想要提供最常用的触摸功能 （例如，那些通常在 internet 浏览器应用程序中可用）。  
  
-   **提升体验**是为想要添加此类功能的团队一样笔势或其他可选功能，可以将其应用程序的第一个触摸友好。  
  
||基本体验|优化的体验|提升的体验|  
|-|----------------------|--------------------------|-------------------------|  
|使用户可以...|修复代码和阅读并不岔路的解决方案/项目-级别|执行维护，重构方法和导航任务|信心十足地运行一致、 直观且流畅的体验|  
|编辑器|触摸平移和所选内容<br /><br /> 滚动条触控跳转和按并拖动|捏合缩放<br /><br /> 快速滚动<br /><br /> 选择<br /><br /> 易于使用的上下文菜单||  
|顶部的工具窗口|列表平移<br /><br /> 项选择<br /><br /> 滚动条触控跳转和按并拖动|简单项滚动和选择||  
|窗口化||调整窗口的大小<br /><br /> 快速访问||  
|文档井中||打开的文件之间轻松导航||  
|笔势||确保常见的手势在 IDE 中工作|基于手势的操作<br /><br /> 支持拖放和设计器|  
|其他注意事项|||自定义屏幕键盘|  
  
#### <a name="gestures"></a>笔势  
 手势提供用户可能需要更复杂的交互的命令的快捷方式。 请参阅在 Windows 准则[桌面应用程序的常用的触摸手势](http://msdn.microsoft.com/library/windows/desktop/dd940543\(v=vs.85\).aspx)，并按照此指南进行大多数手势，包括简单笔势，例如平移和缩放。
