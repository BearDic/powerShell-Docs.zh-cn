---
ms.date: 06/12/2017
keywords: dsc,powershell,配置,安装程序
title: DSC Group 资源
ms.openlocfilehash: 123e09b54a923af942a15f80fa7291c555b4235f
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "62077341"
---
# <a name="dsc-group-resource"></a>DSC Group 资源

> 适用于：Windows PowerShell 4.0 和 Windows PowerShell 5.0

Windows PowerShell Desired State Configuration (DSC) 中的 Group 资源提供了管理目标节点上的本地组的机制。

## <a name="syntax"></a>语法

```
Group [string] #ResourceName
{
    GroupName          = [string]
    [ Credential       = [PSCredential] ]
    [ Description      = [string[]] ]
    [ Ensure           = [string] { Absent | Present }  ]
    [ Members          = [string[]] ]
    [ MembersToExclude = [string[]] ]
    [ MembersToInclude = [string[]] ]
    [ DependsOn        = [string[]] ]
}
```

## <a name="properties"></a>“属性”

|  属性  |  说明   |
|---|---|
| GroupName| 要确保其处于特定状态的组的名称。|
| 凭据| 访问远程资源所需的凭据。 **注意**：此帐户必须拥有相应的 Active Directory 权限，才能将所有非本地帐户添加到组中；否则，在目标节点上执行配置时会发生错误。
| 说明| 组的说明。|
| Ensure| 指示该组是否存在。 将其属性设置为“Absent”可确保组不存在。 将其设置为"Present"（默认值）可确保组存在。|
| 成员| 使用此属性将当前的组成员身份替换为指定成员。 此属性的值是一组形式为 *Domain*\\*UserName* 的字符串。 如果你在配置中设置此属性，请勿使用 **MembersToExclude** 或 **MembersToInclude** 属性。 这样会生成错误。|
| MembersToExclude| 使用此属性从现有的组成员身份中删除成员。 此属性的值是一组形式为 *Domain*\\*UserName* 的字符串。 如果你在配置中设置此属性，请勿使用 **Members** 属性。 这样会生成错误。|
| MembersToInclude| 使用此属性将成员添加到组的现有成员资格中。 此属性的值是一组形式为 *Domain*\\*UserName* 的字符串。 如果你在配置中设置此属性，请勿使用 **Members** 属性。 这样做会导致错误生成。|
| DependsOn | 指示必须先运行其他资源的配置，再配置此资源。 例如，如果你想要首先运行 ID 为 __ResourceName__、类型为 __ResourceType__ 的资源配置脚本块，则使用此属性的语法为 `DependsOn = "[ResourceType]ResourceName"``。|

## <a name="example-1"></a>示例 1

下面的示例展示了如何确保名为“TestGroup”的组不存在。

```powershell
Group GroupExample
{
    # This removes TestGroup, if present
    # To create a new group, set Ensure to "Present“
    Ensure = "Absent"
    GroupName = "TestGroup"
}
```

## <a name="example-2"></a>示例 2

以下示例演示如何将 Active Directory 用户添加到属于多计算机实验室版本（已在其中对本地管理员帐户使用 PSCredential）的本地管理员组。
由于这也适用于域管理员帐户（在域提升后），因此需要将这个现有 PSCredential 转换为域友好凭据。
然后，便可以将域用户添加到成员服务器上的本地管理员组。

```powershell
@{
    AllNodes = @(
        @{
            NodeName = '*';
            DomainName = 'SubTest.contoso.com';
         }
        @{
            NodeName = 'Box2';
            AdminAccount = 'Admin-Dave_Alexanderson'
        }
    )
}

$domain = $node.DomainName.split('.')[0]
$DCredential = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList ("$domain\$($credential.Username)", $Credential.Password)

Group AddADUserToLocalAdminGroup {
    GroupName='Administrators'
    Ensure= 'Present'
    MembersToInclude= "$domain\$($Node.AdminAccount)"
    Credential = $dCredential
    PsDscRunAsCredential = $DCredential
}
```

## <a name="example-3"></a>示例 3

下面的示例展示了如何确保服务器 TigerTeamSource.Contoso.Com 上的本地组 TigerTeamAdmins 不包含特定域帐户 Contoso\JerryG。

```powershell
Configuration SecureTigerTeamSource {
  Import-DscResource -ModuleName 'PSDesiredStateConfiguration'

  Node TigerTeamSource.Contoso.Com {
    Group TigerTeamAdmins {
       GroupName        = 'TigerTeamAdmins'
       Ensure           = 'Present'
       MembersToExclude = "Contoso\JerryG"
    }
  }
}
```