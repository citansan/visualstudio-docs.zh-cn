---
title: 如何：使用命令行通过探查器启动独立本机应用程序以收集并发数据 | Microsoft Docs
ms.custom: ''
ms.date: 11/15/2016
ms.prod: visual-studio-dev14
ms.reviewer: ''
ms.suite: ''
ms.technology:
- vs-ide-debug
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: e5aed651-afed-4b70-9a7e-1a6032cc614f
caps.latest.revision: 28
author: mikejo5000
ms.author: mikejo
manager: ghogen
ms.openlocfilehash: 2f9949254a86e729bd51ccb9896a74c2f83b2568
ms.sourcegitcommit: 9ceaf69568d61023868ced59108ae4dd46f720ab
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/12/2018
ms.locfileid: "49301209"
---
# <a name="how-to-launch-a-stand-alone-native-application-with-the-profiler-to-collect-concurrency-data-by-using-the-command-line"></a>如何：使用命令行将独立本机应用程序与探查器一起启动以收集并发数据
[!INCLUDE[vs2017banner](../includes/vs2017banner.md)]

本主题介绍如何使用 [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] 分析工具命令行工具启动本机独立（客户端）应用程序以及收集进程和线程并发数据。  
  
 分析会话包括以下几部分：  
  
-   用探查器启动应用程序  
  
-   控制数据收集  
  
-   结束分析会话  
  
> [!NOTE]
>  分析工具的命令行工具位于 [!INCLUDE[vs_current_short](../includes/vs-current-short-md.md)] 安装目录的 \Team Tools\Performance Tools 子目录中。 在 64 位计算机上，同时提供 64 位和 32 位版本的工具。 若要在命令提示符下使用探查器，必须将工具路径添加到**命令提示符**窗口的 PATH 环境变量中，或将其添加到命令本身。 有关详细信息，请参阅[指定命令行工具的路径](../profiling/specifying-the-path-to-profiling-tools-command-line-tools.md)。  
  
## <a name="starting-the-application-with-the-profiler"></a>用探查器启动应用程序  
 若要使用探查器来启动目标应用程序，请使用 [VSPerfCmd.exe](../profiling/vsperfcmd.md)**/start** 和 **/launch** 选项来初始化探查器并启动应用程序。 可以指定 **/start** 和 **/launch** 及其各自的选项。 还可以添加 **/globaloff** 选项，以在目标应用程序启动时暂停数据收集。 然后可使用 **/globalon** 开始收集数据。  
  
#### <a name="to-start-an-application-with-the-profiler"></a>用探查器启动应用程序  
  
1.  在命令提示符下，键入以下命令：  
  
     [VSPerfCmd](../profiling/vsperfcmd.md) **/start:concurrency  /output:** `OutputFile` [`Options`]  
  
     [/output](../profiling/output.md)**:**`OutputFile` 选项需要与 **/start** 一起使用。 `OutputFile` 指定分析数据 (.vsp) 文件的名称和位置。  
  
     可以将下表中的任意选项与 **/start:concurrency** 选项一起使用。  
  
    |选项|描述|  
    |------------|-----------------|  
    |[/wincounter](../profiling/wincounter.md) **:** `WinCounterPath`|指定要在分析期间收集的 Windows 性能计数器。|  
    |[/automark](../profiling/automark.md) **:** `Interval`|仅与 **/wincounter** 一起使用。 指定两次 Windows 性能计数器收集事件相隔的毫秒数。 默认值为 500。|  
    |[/events](../profiling/events-vsperfcmd.md) **:** `Config`|指定要在分析期间收集的 Windows 事件跟踪 (ETW) 事件。 ETW 事件收集在单独的 (.etl) 文件中。|  
  
2.  通过键入以下命令启动目标应用程序：  
  
     **VSPerfCmd**  [/launch](../profiling/launch.md) **:** `AppName` [`Options`]  
  
     可以将下表中的任意选项与 **/launch** 选项一起使用。  
  
    |选项|描述|  
    |------------|-----------------|  
    |[/args](../profiling/args.md) **:** `Arguments`|指定一个字符串，其中包含要传递给目标应用程序的命令行参数。|  
    |[/console](../profiling/console.md)|在另一个窗口中启动目标命令行应用程序。|  
    |[/targetclr](../profiling/targetclr.md) **:** `CLRVersion`|指定应用程序加载公共语言运行时 (CLR) 的多个版本时要分析的 CLR 的版本。|  
  
## <a name="controlling-data-collection"></a>控制数据收集  
 目标应用程序运行时，可通过使用 VSPerfCmd.exe 选项开始和停止向文件写入数据，从而控制数据收集。 通过控制数据收集，可以针对程序执行的特定部分（如启动或关闭应用程序）进行数据收集。  
  
#### <a name="to-start-and-stop-data-collection"></a>启动和停止数据收集  
  
-   下表中的选项对可启动和停止数据收集。 在单独的命令行上指定每个选项。 可多次打开和关闭数据收集。  
  
    |选项|描述|  
    |------------|-----------------|  
    |[/globalon /globaloff](../profiling/globalon-and-globaloff.md)|启动 (**/globalon**) 或停止 (**/globaloff**) 所有进程的数据收集。|  
    |[/processon](../profiling/processon-and-processoff.md) **:** `PID` [/processoff](../profiling/processon-and-processoff.md) **:** `PID`|启动 (**/processon**) 或停止 (**/processoff**) 由进程 ID (`PID`) 指定的进程的数据收集。|  
    |[/attach](../profiling/attach.md) **:**{`PID`&#124;`ProcName`} [/detach](../profiling/detach.md)[**:**{`PID`&#124;`ProcName`}]|**/attach** 将启动由进程 ID (`PID`) 或进程名称 (*ProcName*) 指定的进程的数据收集。 **/detach** 将停止指定进程或所有进程（未指定任何特定进程时）的数据收集。|  
  
-   还可以使用 **VSPerfCmd.exe**[/mark](../profiling/mark.md) 选项将分析标记插入数据文件。 **/mark**命令可添加标识符、时间戳和（可选）用户定义的文本字符串。 标记可用于筛选探查器报告和数据视图中的数据。  
  
## <a name="ending-the-profiling-session"></a>结束分析会话  
 若要结束分析会话，探查器不得再收集数据。 可以通过关闭所分析的应用程序或调用 **VSPerfCmd /detach** 选项来停止收集并发数据。 然后，可以调用 **VSPerfCmd /shutdown** 选项关闭探查器和分析数据文件。  
  
#### <a name="to-end-a-profiling-session"></a>结束分析会话  
  
1.  通过在命令提示符下键入以下命令将探查器与目标应用程序分离：  
  
     **VSPerfCmd /detach**  
  
2.  在命令提示符下键入以下命令以关闭探查器：  
  
     **VSPerfCmd** [/shutdown](../profiling/shutdown.md)


