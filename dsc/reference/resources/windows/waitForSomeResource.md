---
ms.date: 06/12/2017
keywords: dsc,powershell,配置,安装程序
title: DSC WaitForSome 资源
ms.openlocfilehash: 2260f37002171154a6f2c3996b2af1bd9120039d
ms.sourcegitcommit: 46bebe692689ebedfe65ff2c828fe666b443198d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2019
ms.locfileid: "67726765"
---
# <a name="dsc-waitforsome-resource"></a>DSC WaitForSome 资源

> 适用于：Windows PowerShell 5.0 及更高版本

可以在 [DSC 配置](../../../configurations/configurations.md)中的节点块内使用 WaitForSome  Desired State Configuration (DSC) 资源，以指定依赖其他节点上的配置。

如果由 ResourceName  属性指定的资源在 NodeName  属性定义的最少节点数（由 NodeCount  指定）上都处于所需状态，那么此资源成功。

> [!NOTE]
> WaitForSome 资源使用 Windows 远程管理来检查其他节点的状态  。
> 要详细了解 WinRM 的端口和安全性要求，请参阅 [PowerShell 远程处理安全注意事项](/powershell/scripting/learn/remoting/winrmsecurity?view=powershell-6)。

## <a name="syntax"></a>语法

```
WaitForSome [String] #ResourceName
{
    NodeCount = [UInt32]
    NodeName = [string[]]
    ResourceName = [string]
    [DependsOn = [string[]]]
    [PsDscRunAsCredential = [PSCredential]]
    [RetryCount = [UInt32]]
    [RetryIntervalSec = [UInt64]]
    [ThrottleLimit = [UInt32]]
}
```

## <a name="properties"></a>“属性”

|  属性  |  说明   |
|---|---|
| NodeCount| 此资源成功至少所需的处于相应状态的节点数。|
| NodeName| 要依赖的资源的目标节点。|
| ResourceName| 要依赖的资源名称。 如果此资源属于不同的配置，请将名称的格式设置为“[__ResourceType__]__ResourceName__::[__ConfigurationName__]::[__ConfigurationName__]”|
| RetryIntervalSec| 重试前等待的秒数。 最小值为 1。|
| RetryCount| 重试次数上限。|
| ThrottleLimit| 同时连接的计算机数量。 默认值为 new-cimsession default。|
| DependsOn | 指示必须先运行其他资源的配置，再配置此资源。 例如，如果你想要首先运行 ID 为 __ResourceName__、类型为 __ResourceType__ 的资源配置脚本块，则使用此属性的语法为 `DependsOn = "[ResourceType]ResourceName"`。|
| PsDscRunAsCredential | 请参阅[通过用户凭据使用 DSC](https://docs.microsoft.com/powershell/dsc/runasuser) |

## <a name="example"></a>示例

有关如何使用此资源的示例，请参阅[指定跨节点依赖关系](../../../configurations/crossNodeDependencies.md)
