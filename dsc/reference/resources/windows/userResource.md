---
ms.date: 06/12/2017
keywords: dsc,powershell,配置,安装程序
title: DSC User 资源
ms.openlocfilehash: 04543351df19160a2da05ccea96e5d392d8c55bf
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "62076884"
---
# <a name="dsc-user-resource"></a>DSC User 资源

适用于：Windows PowerShell 4.0 和 Windows PowerShell 5.0

Windows PowerShell Desired State Configuration (DSC) 中的 **User** 资源提供在目标节点上管理用户帐户的机制。

## <a name="syntax"></a>语法

```
User [string] #ResourceName
{
    UserName = [string]
    [ Description = [string] ]
    [ Disabled = [bool] ]
    [ Ensure = [string] { Absent | Present }  ]
    [ FullName = [string] ]
    [ Password = [PSCredential] ]
    [ PasswordChangeNotAllowed = [bool] ]
    [ PasswordChangeRequired = [bool] ]
    [ PasswordNeverExpires = [bool] ]
    [ DependsOn = [string[]] ]
}
```

## <a name="properties"></a>“属性”

|  属性  |  说明   |
|---|---|
| UserName| 指示要确保其特定状态的帐户名。|
| 说明| 指示要用于用户帐户的说明。|
| 禁用| 指示帐户是否已启用。 将此属性设置为 `$true` 可确保已禁用保此帐户，将其设置为 `$false` 可确保已启用此帐户。|
| Ensure| 指示帐户是否存在。 将此属性设置为“Present”以确保该帐户存在，将其设置为“Absent”以确保该帐户不存在。|
| FullName| 表示包含要用于用户帐户的完整名称的字符串。|
| 密码| 指示要用于此帐户的密码。 |
| PasswordChangeNotAllowed| 指示用户是否可以更改密码。 将此属性设置为 `$true` 可确保用户无法更改密码，将其设置为 `$false` 可允许用户更改密码。 默认值为 `$false`。|
| PasswordChangeRequired| 指定用户下次登录时是否必须更改密码。 要使用户必须更改密码，请将此属性设置为 `$true`。 默认值为 `$true`。|
| PasswordNeverExpires| 指示密码是否会过期。 将此属性设置为 `$true` 可确保此帐户的密码永不过期，将其设置为 `$false` 则密码会过期。 默认值为 `$false`。|
| DependsOn | 指示必须先运行其他资源的配置，再配置此资源。 例如，如果你想要首先运行 ID 为 **ResourceName**、类型为 **ResourceType** 的资源配置脚本块，则使用此属性的语法为 `DependsOn = "[ResourceType]ResourceName"`。|

## <a name="example"></a>示例

```powershell
User UserExample
{
    Ensure = "Present"  # To ensure the user account does not exist, set Ensure to "Absent"
    UserName = "SomeName"
    Password = $passwordCred # This needs to be a credential object
    DependsOn = "[Group]GroupExample" # Configures GroupExample first
}
```