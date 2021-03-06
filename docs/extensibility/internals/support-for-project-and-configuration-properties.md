---
title: 项目和配置属性支持 |Microsoft Docs
ms.date: 11/04/2016
ms.topic: conceptual
helpviewer_keywords:
- project properties, supporting with Visual Studio SDK
- configuration properties, supporting with Visual Studio SDK
ms.assetid: 9fcfaa0f-7b41-4b68-82ec-7a151dca5d7e
author: madskristensen
ms.author: madsk
manager: jillfra
ms.workload:
- vssdk
ms.openlocfilehash: 8bf0581eee4fade779d89143f4633f1b87d3ce0f
ms.sourcegitcommit: 5f6ad1cefbcd3d531ce587ad30e684684f4c4d44
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2019
ms.locfileid: "72723157"
---
# <a name="support-for-project-and-configuration-properties"></a>支持项目和配置属性
@No__t_1 集成开发环境（IDE）中的 "**属性**" 窗口可以显示 "项目" 和 "配置" 属性。 您可以为自己的项目类型提供属性页，以便用户可以设置您的应用程序的属性。

 通过在**解决方案资源管理器**中选择项目节点，然后单击 "**项目**" 菜单上的 "**属性**"，可以打开一个包含 "项目" 和 "配置" 属性的对话框。 在 [!INCLUDE[csprcs](../../data-tools/includes/csprcs_md.md)] 和 [!INCLUDE[vbprvb](../../code-quality/includes/vbprvb_md.md)] 以及从这些语言派生的项目类型中，此对话框在 "[常规"、"环境"、"选项" 对话框](../../ide/reference/general-environment-options-dialog-box.md)中显示为选项卡式页面。 有关详细信息，请参阅[不在生成中：演练：公开项目和配置C#属性（）](https://msdn.microsoft.com/library/d850d63b-25e2-4505-9f3d-eb038d7c1d0e)。

 项目的托管包框架（MPFProj）提供用于创建和管理新项目系统的帮助程序类。 可以在适用于项目的 MPF 上查找源代码和编译说明[-Visual Studio 2013](https://github.com/tunnelvisionlabs/MPFProj10)。

## <a name="persistence-of-project-and-configuration-properties"></a>项目和配置属性的持久性
 项目和配置属性保存在项目文件中，该文件具有与项目类型关联的任何文件扩展名，例如，.csproj、. .vbproj 和. myproj.csproj。 语言项目通常使用模板文件来生成项目文件。 不过，实际上可以通过多种方式来关联项目类型和模板。 有关详细信息，请参阅[模板目录说明（。Vsdir）文件](../../extensibility/internals/template-directory-description-dot-vsdir-files.md)。

 项目和配置属性是通过将项添加到模板文件创建的。 然后，这些属性可用于通过使用此模板的项目类型创建的任何项目。 [!INCLUDE[csprcs](../../data-tools/includes/csprcs_md.md)] 项目和 MPFProj 都使用[不在生成中：](/previous-versions/visualstudio/visual-studio-2008/ms171452(v=vs.90))模板文件的 MSBuild 概述架构。 对于每个配置，这些文件都有一个 PropertyGroup 部分。 项目的属性通常保存在第一个 PropertyGroup 部分，该部分的配置参数设置为 null 字符串。

 下面的代码显示基本 MSBuild 项目文件的开头。

```
<Project MSBuildVersion="2.0" DefaultTargets="Build" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <Configuration Condition=" '$(Configuration)' == '' ">Debug</Configuration>
    <Name>SomeProjectSix</Name>
    <SchemaVersion>2.0</SchemaVersion>
  </PropertyGroup>
  <PropertyGroup Condition=" '$(Configuration)' == 'Debug' ">
    <Optimize>false</Optimize>
  </PropertyGroup>
  <PropertyGroup Condition=" '$(Configuration)' == 'Release' ">
    <Optimize>true</Optimize>
```

 在此项目文件中，`<Name>` 和 `<SchemaVersion>` 是项目属性，`<Optimize>` 是一个配置属性。

 项目负责保存项目文件的项目和配置属性。

> [!NOTE]
> 项目可以通过仅保存不同于其默认值的属性值来优化持久性。

## <a name="support-for-project-and-configuration-properties"></a>支持项目和配置属性
 @No__t_0 类实现项目和配置属性页。 @No__t_0 的默认实现在泛型属性网格中为用户提供公共属性。 @No__t_0 方法选择从项目属性网格 `SettingsPage` 派生的类。 @No__t_0 方法选择从配置属性网格 `SettingsPage` 派生的类。 项目类型应重写这些方法，以便选择适当的属性页。

 @No__t_0 类和 `Microsoft.VisualStudio.Package.ProjectNode` 类提供以下方法来持久保存项目和配置属性：

- `Microsoft.VisualStudio.Package.ProjectNode.GetProjectProperty` 和 `Microsoft.VisualStudio.Package.ProjectNode.SetProjectProperty` 保存项目属性。

- `Microsoft.VisualStudio.Package.SettingsPage.GetConfigProperty` 和 `Microsoft.VisualStudio.Package.SettingsPage.SetConfigProperty` 保留配置属性。

  > [!NOTE]
  > @No__t_0 和 `Microsoft.VisualStudio.Package.ProjectNode` 类的实现使用 `Microsoft.Build.BuildEngine` （MSBuild）方法获取和设置项目文件中的项目和配置属性。

  派生自 `SettingsPage` 的类必须实现 `Microsoft.VisualStudio.Package.SettingsPage.ApplyChanges` 并 `Microsoft.VisualStudio.Package.SettingsPage.BindProperties`，以持久保存项目文件的项目或配置属性。

## <a name="provideobjectattribute-and-registry-path"></a>ProvideObjectAttribute 和注册表路径
 派生自 `SettingsPage` 的类旨在跨 Vspackage 共享。 若要使 VSPackage 能够创建派生自 `SettingsPage` 的类，请将 `Microsoft.VisualStudio.Shell.ProvideObjectAttribute` 添加到从 `Microsoft.VisualStudio.Shell.Package` 派生的类。

 [!code-csharp[VSSDKSupportProjectConfigurationProperties#1](../../extensibility/internals/codesnippet/CSharp/support-for-project-and-configuration-properties_1.cs)]
 [!code-vb[VSSDKSupportProjectConfigurationProperties#1](../../extensibility/internals/codesnippet/VisualBasic/support-for-project-and-configuration-properties_1.vb)]

 附加属性的 VSPackage 是不重要的。 向 [!INCLUDE[vsprvs](../../code-quality/includes/vsprvs_md.md)] 注册 VSPackage 时，将注册可创建的任何对象的类 id （CLSID），以便对 <xref:Microsoft.VisualStudio.Shell.Interop.ILocalRegistry.CreateInstance%2A> 的调用可以创建它。

 可以创建的对象的注册表路径是通过将 <xref:Microsoft.VisualStudio.Shell.Package.UserRegistryRoot%2A>、word、CLSID 和对象类型的 guid 相结合来确定的。 如果 `MyProjectPropertyPage` 类具有 {3c693da2-5bca-49b3-bd95-ffe0a39dd723} 的 guid 并且 UserRegistryRoot 为 HKEY_CURRENT_USER\Software\Microsoft\VisualStudio\8.0Exp，则注册表路径将为 HKEY_CURRENT_USER\Software\Microsoft\VisualStudio\8.0 exp \ CLSID \\ {3c693da2-5bca-49b3-bd95-ffe0a39dd723}。

## <a name="project-and-configuration-property-attributes-and-layout"></a>项目和配置属性和布局
 @No__t_0、<xref:System.ComponentModel.DisplayNameAttribute> 和 <xref:System.ComponentModel.DescriptionAttribute> 特性确定泛型属性页中的项目和配置属性的布局、标签和说明。 这些属性分别决定了选项的类别、显示名称和说明。

> [!NOTE]
> 等效属性、SRCategory、LocDisplayName 和 SRDescription，使用字符串资源进行本地化，并在[MPF For 项目中定义-Visual Studio 2013](https://github.com/tunnelvisionlabs/MPFProj10)。

 考虑以下代码片断：

 [!code-vb[VSSDKSupportProjectConfigurationProperties#2](../../extensibility/internals/codesnippet/VisualBasic/support-for-project-and-configuration-properties_2.vb)]
 [!code-csharp[VSSDKSupportProjectConfigurationProperties#2](../../extensibility/internals/codesnippet/CSharp/support-for-project-and-configuration-properties_2.cs)]

 "@No__t_0 配置" 属性在 "配置" 属性页上显示为 **"我的**类别" 类别中的**my Config 属性**。 如果选择了该选项，说明面板中将显示 "**我的描述**"。

## <a name="see-also"></a>请参阅
- [添加和删除属性页](../../extensibility/adding-and-removing-property-pages.md)
- [项目](../../extensibility/internals/projects.md)
- [模板目录说明 (.Vsdir) 文件](../../extensibility/internals/template-directory-description-dot-vsdir-files.md)
