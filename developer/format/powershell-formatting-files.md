---
title: Windows PowerShell 格式设置文件 |Microsoft Docs
ms.custom: ''
ms.date: 09/13/2016
ms.reviewer: ''
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: 5d4c8f84-ebd2-4405-bb10-cfc5400d4ad6
caps.latest.revision: 6
ms.openlocfilehash: 3ec127d5ff60754de5d7f1ac73f2965524228b9c
ms.sourcegitcommit: 4ec9e10647b752cc62b1eabb897ada3dc03c93eb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/11/2019
ms.locfileid: "66830267"
---
# <a name="windows-powershell-formatting-files"></a>Windows PowerShell 格式设置文件

Windows PowerShell 提供了多个格式设置文件 (。 format.ps1xml) 位于安装目录 (`$pshome`)。 每个这些文件定义一组特定的.NET 对象的默认显示。 这些文件应永远不会更改。 但是，您可以使用它们作为参考用于创建您自己的自定义格式设置文件。

`Certificate.Format.ps1xml` 如 x.509 证书的证书存储区和证书存储区中定义对象的显示。

`DotNetTypes.Format.ps1xml` 定义其他.NET 对象，例如 CultureInfo、 FileVersionInfo 和 EventLogEntry 对象的显示。

`FileSystem.Format.ps1xml` 定义显示的文件系统对象，如文件和目录对象。

`Help.Format.ps1xml` 定义使用的不同视图[Get-help](/powershell/module/Microsoft.PowerShell.Core/Get-Help) cmdlet，如详细，完整备份、 参数和示例视图。

`PowerShellCore.Format.ps1xml` 定义生成的 Windows PowerShell 核心 cmdlet，例如通过返回的对象的对象显示[Get-member](/powershell/module/Microsoft.PowerShell.Utility/Get-Member)并[Get-history](/powershell/module/Microsoft.PowerShell.Core/Get-History) cmdlet。

`PowerShellTrace.Format.ps1xml` 定义跟踪对象，如生成的显示[Trace-command](/powershell/module/Microsoft.PowerShell.Utility/Trace-Command) cmdlet。

`Registry.Format.ps1xml` 定义等项和项对象的注册表对象的显示。

## <a name="see-also"></a>另请参阅

[编写 Windows PowerShell Cmdlet](../cmdlet/writing-a-windows-powershell-cmdlet.md)
