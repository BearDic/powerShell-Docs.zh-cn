---
title: RunSpace08 代码示例 |Microsoft Docs
ms.custom: ''
ms.date: 09/13/2016
ms.reviewer: ''
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: 0f286201-8a02-4b00-9a2c-1b833ccdbdbf
caps.latest.revision: 7
ms.openlocfilehash: ec6aae544eafea1dedc1379dab00beeed2c7c436
ms.sourcegitcommit: 46bebe692689ebedfe65ff2c828fe666b443198d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2019
ms.locfileid: "67734918"
---
# <a name="runspace08-code-sample"></a>RunSpace08 代码示例

此处是 Runspace08 示例中所述的源代码[创建控制台应用程序，将添加命令参数](https://msdn.microsoft.com/en-us/848b2b46-60f1-4a86-b448-cfc7c0cccfba)。 此示例应用程序创建一个运行空间、 创建管道，将两个命令添加到管道，将两个参数添加到第二个命令，然后执行管道。 添加到管道的命令是`Get-Process`和`Sort-Object`cmdlet。

## <a name="code-sample"></a>代码示例

[!code-csharp[Runspace08.cs](../../powershell-sdk-samples/SDK-2.0/csharp/Runspace08/Runspace08.cs#L11-L86 "Runspace08.cs")]

## <a name="see-also"></a>另请参阅

[Windows PowerShell SDK](../windows-powershell-reference.md)