---
title: RunSpace09 代码示例 |Microsoft Docs
ms.custom: ''
ms.date: 09/13/2016
ms.reviewer: ''
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: 136e451f-767b-42e0-bd6f-6486693abd5e
caps.latest.revision: 6
ms.openlocfilehash: 1a21af4b48a414d9c9fee57871eacb0a39c9ab11
ms.sourcegitcommit: 46bebe692689ebedfe65ff2c828fe666b443198d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2019
ms.locfileid: "67734891"
---
# <a name="runspace09-code-sample"></a>RunSpace09 代码示例

此处是 Runspace09 示例中所述的源代码[管道以异步方式创建控制台应用程序，会调用](https://msdn.microsoft.com/en-us/198c1c94-2a06-457e-93ce-c0d910618e47)。 此示例应用程序创建和打开运行空间中，创建并以异步方式调用管道，并使用然后管道事件来以异步方式处理该脚本。 运行此应用程序的脚本在 0.5 秒时间间隔内 （500 毫秒） 中创建整数 1 到 10。

## <a name="code-sample"></a>代码示例

[!code-csharp[Runspace09.cs](../../powershell-sdk-samples/SDK-2.0/csharp/Runspace09/Runspace09.cs#L11-L113 "Runspace09.cs")]

## <a name="see-also"></a>另请参阅

[Windows PowerShell SDK](../windows-powershell-reference.md)