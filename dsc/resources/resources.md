---
ms.date: 12/12/2018
keywords: dsc,powershell,配置,安装程序
title: DSC 资源
ms.openlocfilehash: 1f77b5e6630a2e3de6e1d1a05638f94d2df039ae
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "62076614"
---
# <a name="dsc-resources"></a>DSC 资源

>适用于：Windows PowerShell 4.0 和 Windows PowerShell 5.0

Desired State Configuration (DSC) 资源为 DSC 配置提供构建基块。 资源公开可配置的属性（架构），并包含本地配置管理器 (LCM) 调用以“使其如此”的 PowerShell 脚本函数。

资源的建模对象可以是一般的文件或 Windows 进程，也可以是具体的 IIS 服务器设置。  资源等组被组合成为 DSC 模块，模块将全部所需文件组织成为一个可移植结构，该结构包含标志资源既定用途的元数据。

每个资源都具有用于确定使用[配置](../configurations/configurations.md)中的资源所需的语法的 *架构。 可以按以下方式定义资源的架构：

- “Schema.Mof”文件：大多数资源使用[托管对象格式](/windows/desktop/wmisdk/managed-object-format--mof-)定义它们在“schema.mof”文件中的架构。
- “\<Resource Name\>.schema.psm1”文件：[复合资源](../configurations/compositeConfigs.md)使用[参数块](/powershell/module/microsoft.powershell.core/about/about_functions?view=powershell-6#functions-with-parameters)定义它们在“<ResourceName>.schema.psm1”文件中的架构。
- “\<Resource Name\>.psm1”文件：基于类的 DSC 资源定义它们在类定义中的架构。 语法项表示为类属性。 有关详细信息，请参阅 [about_Classes](/powershell/module/psdesiredstateconfiguration/about/about_classes_and_dsc)。

若要检索 DSC 资源的语法，请将 [Get-DSCResource](/powershell/module/PSDesiredStateConfiguration/Get-DscResource) cmdlet 与 `-Syntax` 参数一起使用。 此用法类似于将 [Get-Command](/powershell/module/microsoft.powershell.core/get-command) 与 `-Syntax` 参数一起使用以获取 cmdlet 语法。 所看到的输出将显示用于指定资源的资源块的模板。

```powershell
Get-DscResource -Syntax Service
```

所看到的输出应类似于以下输出，尽管此资源的语法在将来可能会发生改变也是如此。 类似于 cmdlet 语法，方括号中所示的键是可选的。 类型指定每个键所需的数据类型。

> [!NOTE]
> 确保键是可选的，因为它默认为“Present”。

```output
Service [String] #ResourceName
{
    Name = [string]
    [BuiltInAccount = [string]{ LocalService | LocalSystem | NetworkService }]
    [Credential = [PSCredential]]
    [Dependencies = [string[]]]
    [DependsOn = [string[]]]
    [Description = [string]]
    [DisplayName = [string]]
    [Ensure = [string]{ Absent | Present }]
    [Path = [string]]
    [PsDscRunAsCredential = [PSCredential]]
    [StartupType = [string]{ Automatic | Disabled | Manual }]
    [State = [string]{ Running | Stopped }]
}
```

在配置内，Service 资源块可能如下所示以确保 Spooler 服务正在运行。

> [!NOTE]
> 在使用配置中的资源之前，必须使用 [Import-DSCResource](../configurations/import-dscresource.md) 导入该资源。

```powershell
Configuration TestConfig
{
    # It is best practice to always directly import resources, even if the resource is a built-in resource.
    Import-DSCResource -Name Service
    Node localhost
    {
        # The name of this resource block, can be anything you choose, as long as it is of type [String] as indicated by the schema.
        Service "Spooler:Running"
        {
            Name = "Spooler"
            State = "Running"
        }
    }
}
```

配置可以包含同一资源类型的多个实例。 每个实例必须具有唯一名称。 在以下示例中，添加了第二个 Service 资源块以配置“DHCP”服务。

```powershell
Configuration TestConfig
{
    # It is best practice to always directly import resources, even if the resource is a built-in resource.
    Import-DSCResource -Name Service
    Node localhost
    {
        # The name of this resource block, can be anything you choose, as long as it is of type [String] as indicated by the schema.
        Service "Spooler:Running"
        {
            Name = "Spooler"
            State = "Running"
        }

        # To configure a second service resource block, add another Service resource block and use a unique name.
        Service "DHCP:Running"
        {
            Name = "DHCP"
            State = "Running"
        }
    }
}
```

> [!NOTE]
> 从 PowerShell 5.0 开始，为 DSC 添加了 Intellisense。 借助这一新功能，可以使用 \<TAB\> 和 \<Ctrl+Space\> 自动补全键名称。

![资源 Tab 自动补全](../media/resource-tabcompletion.png)

## <a name="built-in-resources"></a>内置资源

除了社区资源之外，还有适用于 Windows 的内置资源、适用于 Linux 的资源和适用于跨节点依赖项的资源。 可以使用上述步骤确定这些资源的语法以及如何使用这些资源。 已在“参考”下存档提供这些资源的页面。

Windows 内置资源

* [存档资源](../reference/resources/windows/archiveResource.md)
* [环境资源](../reference/resources/windows/environmentResource.md)
* [文件资源](../reference/resources/windows/fileResource.md)
* [组资源](../reference/resources/windows/groupResource.md)
* [GroupSet 资源](../reference/resources/windows/groupSetResource.md)
* [日志资源](../reference/resources/windows/logResource.md)
* [包资源](../reference/resources/windows/packageResource.md)
* [ProcessSet 资源](../reference/resources/windows/ProcessSetResource.md)
* [注册表资源](../reference/resources/windows/registryResource.md)
* [脚本资源](../reference/resources/windows/scriptResource.md)
* [服务资源](../reference/resources/windows/serviceResource.md)
* [ServiceSet 资源](../reference/resources/windows/serviceSetResource.md)
* [用户资源](../reference/resources/windows/userResource.md)
* [WindowsFeature 资源](../reference/resources/windows/windowsFeatureResource.md)
* [WindowsFeatureSet 资源](../reference/resources/windows/windowsFeatureSetResource.md)
* [WindowsOptionalFeature 资源](../reference/resources/windows/windowsOptionalFeatureResource.md)
* [WindowsOptionalFeatureSet 资源](../reference/resources/windows/windowsOptionalFeatureSetResource.md)
* [WindowsPackageCabResource 资源](../reference/resources/windows/windowsPackageCabResource.md)
* [WindowsProcess 资源](../reference/resources/windows/windowsProcessResource.md)

[跨节点依赖项](../configurations/crossNodeDependencies.md)资源

* [WaitForAll 资源](../reference/resources/windows/waitForAllResource.md)
* [WaitForSome 资源](../reference/resources/windows/waitForSomeResource.md)
* [WaitForAny 资源](../reference/resources/windows/waitForAnyResource.md)

包管理资源

* [PackageManagement 资源](../reference/resources/packagemanagement/PackageManagementDscResource.md)
* [PackageManagementSource 资源](../reference/resources/packagemanagement/PackageManagementSourceDscResource.md)

Linux 资源

* [Linux 存档资源](../reference/resources/linux/lnxArchiveResource.md)
* [Linux 环境资源](../reference/resources/linux/lnxEnvironmentResource.md)
* [Linux FileLine 资源](../reference/resources/linux/lnxFileLineResource.md)
* [Linux 文件资源](../reference/resources/linux/lnxFileResource.md)
* [Linux 组资源](../reference/resources/linux/lnxGroupResource.md)
* [Linux 包资源](../reference/resources/linux/lnxPackageResource.md)
* [Linux 脚本资源](../reference/resources/linux/lnxScriptResource.md)
* [Linux 服务资源](../reference/resources/linux/lnxServiceResource.md)
* [Linux SshAuthorizedKeys 资源](../reference/resources/linux/lnxSshAuthorizedKeysResource.md)
* [Linux 用户资源](../reference/resources/linux/lnxUserResource.md)
