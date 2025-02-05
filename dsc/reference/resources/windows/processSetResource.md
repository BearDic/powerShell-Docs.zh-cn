---
ms.date: 06/12/2017
keywords: dsc,powershell,配置,安装程序
title: DSC ProcessSet 资源
ms.openlocfilehash: 91a2d5b562864addcb8e11062916d291448bbf57
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "62077103"
---
# <a name="dsc-windowsprocess-resource"></a>DSC WindowsProcess 资源

适用于：Windows PowerShell 5.0

Windows PowerShell Desired State Configuration (DSC) 中的 **ProcessSet** 资源提供了用于在目标节点上配置进程的机制。 此资源是[复合资源](../../../resources/authoringResourceComposite.md)，它会针对 `GroupName` 参数中指定的每个组调用 [WindowsProcess 资源](windowsProcessResource.md)。

## <a name="syntax"></a>语法

```
WindowsProcess [string] #ResourceName
{
    Arguments = [string]
    Path = [string]
    [ Credential = [PSCredential] ]
    [ Ensure = [string] { Absent | Present }  ]
    [ StandardOutputPath = [string] ]
    [ StandardErrorPath = [string] ]
    [ StandardInputPath = [string] ]
    [ WorkingDirectory = [string] ]
    [ DependsOn = [string[]] ]
}
```

## <a name="properties"></a>“属性”

| 属性 | 说明 |
| --- | --- |
| 参数| 包含要原样传递到进程的参数的字符串。 如果需要传递多个参数，请将它们全部放在此字符串中。|
| 路径| 进程可执行文件的路径。 如果这些是可执行文件的名称（不是完全限定的路径），则 DSC 资源会搜索环境**路径**变量 (`$env:Path`) 以查找文件。 如果此属性的值是完全限定的路径，则 DSC 不使用**路径**环境变量查找文件，并且会在不存在任何路径时引发错误。 不允许使用相对路径。|
| 凭据| 指示启动进程的凭据。|
| Ensure| 指定进程是否存在。 将此属性设置为“Present”以确保进程存在。 否则，将其设置为“Absent”。 默认值为“Present”。|
| StandardErrorPath| 进程写入标准错误的路径。 将覆盖任何现有文件。|
| StandardInputPath| 进程从中接收标准输入的流。|
| StandardOutputPath| 进程写入标准输出的文件的路径。 将覆盖任何现有文件。|
| WorkingDirectory| 用作进程当前工作目录的位置。|
| DependsOn | 指示必须先运行其他资源的配置，再配置此资源。 例如，如果想要首先运行 ID 为“ResourceName”、类型为“_ResourceType”的资源配置脚本块，则使用此属性的语法为 `DependsOn = "[ResourceType]ResourceName"`。|
