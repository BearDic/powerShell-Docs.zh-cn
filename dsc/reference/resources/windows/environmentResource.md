---
ms.date: 06/12/2017
keywords: dsc,powershell,配置,安装程序
title: DSC Environment 资源
ms.openlocfilehash: 2bc1600a9df32538d59efa712569b12fa9e3beee
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "62077528"
---
# <a name="dsc-environment-resource"></a>DSC Environment 资源

> 适用于：Windows PowerShell 4.0 和 Windows PowerShell 5.0

Windows PowerShell Desired State Configuration (DSC) 中的 __Environment__ 资源提供了管理系统环境变量的机制。

## <a name="syntax"></a>语法
``` mof
Environment [string] #ResourceName
{
    Name = [string]
    [ Ensure = [string] { Absent | Present }  ]
    [ Path = [bool] ]
    [ DependsOn = [string[]] ]
    [ Value = [string] ]
}
```

## <a name="properties"></a>“属性”

|  属性  |  说明   |
|---|---|
| 名称| 指示指示你想要确保其特定状态的环境变量的名称。|
| Ensure| 指示变量是否存在。 如果不存在此变量，将此属性设置为 __Present__ 以创建环境变量；如果已存在此变量，则确保其值与通过 __Value__ 属性提供的值相匹配。 如果存在该变量，将其设置为 __Absent__ 可将其删除。|
| 路径| 定义正在配置的环境变量。 如果变量是 __Path__，则将此属性设置为 __$true__；否则将其设置为 __$false__。 默认值为 __$false__。 如果正在配置的变量是 __Path__，则通过 __Value__ 属性提供的值将被附加到现有值。|
| DependsOn | 指示必须先运行其他资源的配置，再配置此资源。 例如，如果你想要首先运行 ID 为 __ResourceName__、类型为 __ResourceType__ 的资源配置脚本块，则使用此属性的语法为 `DependsOn = "[ResourceType]ResourceName"`。|
| 值| 要分配给环境变量的值。|

## <a name="example"></a>示例

以下示例可确保 __TestEnvironmentVariable__ 存在且具有值 __TestValue__。 如果不存在，则创建它。

```powershell
Environment EnvironmentExample
{
    Ensure = "Present"  # You can also set Ensure to "Absent"
    Name = "TestEnvironmentVariable"
    Value = "TestValue"
}
```