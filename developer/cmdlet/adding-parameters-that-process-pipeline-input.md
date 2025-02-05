---
title: 添加处理管道的输入参数 |Microsoft Docs
ms.custom: ''
ms.date: 09/13/2016
ms.reviewer: ''
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: article
helpviewer_keywords:
- cmdlets [PowerShell Programmer's Guide], pipeline input
- parameters [PowerShell Programmer's Guide], pipeline input
ms.assetid: 09bf70a9-7c76-4ffe-b3f0-a1d5f10a0931
caps.latest.revision: 8
ms.openlocfilehash: 34643d20c16f8cc45e7fb20dc2a87d78b18bbf10
ms.sourcegitcommit: f60fa420bdc81db174e6168d3aeb11371e483162
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67298631"
---
# <a name="adding-parameters-that-process-pipeline-input"></a>添加用于处理管道输入的参数

一个源的一个 cmdlet 是输入的源自于上游 cmdlet 在管道上的对象。 本部分介绍如何将参数添加到 Get-proc cmdlet (中所述[创建第一个 Cmdlet](./creating-a-cmdlet-without-parameters.md))，以便该 cmdlet 可以处理管道对象。

此 Get-proc cmdlet 使用`Name`接受来自管道对象的输入参数从基于提供的名称，在本地计算机检索进程信息，然后在命令行中显示有关进程的信息。

## <a name="defining-the-cmdlet-class"></a>定义在 Cmdlet 类

Cmdlet 创建的第一步是始终命名 cmdlet 和实现该 cmdlet 的.NET 类声明。 此 cmdlet 检索进程信息，因此在此处选择谓词名称为"Get"。 （几乎任何类型的检索信息的 cmdlet 可以处理命令行输入）。有关已批准的 cmdlet 谓词的详细信息，请参阅[Cmdlet 的动词名称](./approved-verbs-for-windows-powershell-commands.md)。

下面是此 Get-proc cmdlet 的定义。 此定义的详细信息中提供了[创建第一个 Cmdlet](./creating-a-cmdlet-without-parameters.md)。

```csharp
[Cmdlet(VerbsCommon.Get, "proc")]
public class GetProcCommand : Cmdlet
```

```vb
<Cmdlet(VerbsCommon.Get, "Proc")> _
Public Class GetProcCommand
    Inherits Cmdlet
```

## <a name="defining-input-from-the-pipeline"></a>定义来自管道的输入

本部分介绍如何定义来自 cmdlet 的管道的输入。 此 Get-proc cmdlet 定义了一个属性，表示`Name`参数中所述[添加该进程的命令行输入的参数](./adding-parameters-that-process-command-line-input.md)。 （请参阅该主题有关声明参数的常规信息。）

但是，当一个 cmdlet 需要处理管道输入，它必须具有绑定到输入值通过 Windows PowerShell 运行时及其参数。 若要执行此操作，必须添加`ValueFromPipeline`关键字或添加`ValueFromPipelineByProperty`关键字[System.Management.Automation.Parameterattribute](/dotnet/api/System.Management.Automation.ParameterAttribute)特性声明。 指定`ValueFromPipeline`关键字，如果该 cmdlet 将访问完整的输入的对象。 指定`ValueFromPipelineByProperty`如果该 cmdlet 将访问的对象的属性。

下面是参数声明为`Name`接受管道输入此 Get-proc cmdlet 的参数。

[!code-csharp[GetProcessSample03.cs](../../powershell-sdk-samples/SDK-2.0/csharp/GetProcessSample03/GetProcessSample03.cs#L35-L44 "GetProcessSample03.cs")]

```vb
<Parameter(Position:=0, ValueFromPipeline:=True, _
ValueFromPipelineByPropertyName:=True), ValidateNotNullOrEmpty()> _
Public Property Name() As String()
    Get
        Return processNames
    End Get

    Set(ByVal value As String())
        processNames = value
    End Set

End Property
```

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplesgetproc03#GetProc03VBNameParameter](Msh_samplesgetproc03#GetProc03VBNameParameter)]  -->

以前的声明集`ValueFromPipeline`关键字`true`，以便如果对象是同一类型作为参数，Windows PowerShell 运行时将将参数绑定到传入的对象或可强制转换为同一类型。 `ValueFromPipelineByPropertyName`关键字也设置为`true`，以便 Windows PowerShell 运行时将检查的传入对象`Name`属性。 如果传入的对象具有这种属性，将绑定在运行时`Name`参数`Name`传入对象的属性。

> [!NOTE]
> 设置`ValueFromPipeline`属性 (attribute) 关键字参数优先于为设置`ValueFromPipelineByPropertyName`关键字。

## <a name="overriding-an-input-processing-method"></a>重写方法的处理的输入

如果你的 cmdlet 是处理管道输入，则需要重写相应的输入处理方法。 中引入的基本输入的处理方法[创建第一个 Cmdlet](./creating-a-cmdlet-without-parameters.md)。

此 Get-proc cmdlet 覆盖[System.Management.Automation.Cmdlet.ProcessRecord](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord)方法以处理`Name`参数输入提供的用户或脚本。 如果未提供名称，此方法将获取每个请求的进程名称或所有进程的进程。 请注意，在[System.Management.Automation.Cmdlet.ProcessRecord](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord)，在调用[WriteObject(System.Object,System.Boolean)](/dotnet/api/system.management.automation.cmdlet.writeobject?view=pscore-6.2.0#System_Management_Automation_Cmdlet_WriteObject_System_Object_System_Boolean_)是用于发送到的输出对象的输出机制管道。 此调用，第二个参数`enumerateCollection`，设置为`true`告诉 Windows PowerShell 运行时，若要枚举的进程对象的数组和一个进程一次写入命令行。

```csharp
protected override void ProcessRecord()
{
  // If no process names are passed to the cmdlet, get all processes.
  if (processNames == null)
  {
      // Write the processes to the pipeline making them available
      // to the next cmdlet. The second argument of this call tells
      // PowerShell to enumerate the array, and send one process at a
      // time to the pipeline.
      WriteObject(Process.GetProcesses(), true);
  }
  else
  {
    // If process names are passed to the cmdlet, get and write
    // the associated processes.
    foreach (string name in processNames)
    {
      WriteObject(Process.GetProcessesByName(name), true);
    } // End foreach (string name...).
  }
}
```

```vb
Protected Overrides Sub ProcessRecord()
    Dim processes As Process()

    '/ If no process names are passed to the cmdlet, get all processes.
    If processNames Is Nothing Then
        processes = Process.GetProcesses()
    Else

        '/ If process names are specified, write the processes to the
        '/ pipeline to display them or make them available to the next cmdlet.
        For Each name As String In processNames
            '/ The second parameter of this call tells PowerShell to enumerate the
            '/ array, and send one process at a time to the pipeline.
            WriteObject(Process.GetProcessesByName(name), True)
        Next
    End If

End Sub 'ProcessRecord
```

## <a name="code-sample"></a>代码示例

有关完整C#示例代码，请参阅[GetProcessSample03 示例](./getprocesssample03-sample.md)。

## <a name="defining-object-types-and-formatting"></a>定义对象类型和格式设置

Windows PowerShell cmdlet 使用.Net 对象之间传递信息。 因此，一个 cmdlet 可能需要定义其自己的类型，或者该 cmdlet 可能需要扩展现有类型提供的另一个 cmdlet。 有关定义新类型或扩展现有类型的详细信息，请参阅[扩展对象类型和格式设置](/previous-versions//ms714665(v=vs.85))。

## <a name="building-the-cmdlet"></a>生成该 Cmdlet

实现必须将它注册使用通过 Windows PowerShell 的 Windows PowerShell cmdlet 后管理单元。 有关注册 cmdlet 的详细信息，请参阅[如何注册 Cmdlet、 提供商和主机应用程序](/previous-versions//ms714644(v=vs.85))。

## <a name="testing-the-cmdlet"></a>测试 Cmdlet

你的 cmdlet 具有已注册到 Windows PowerShell，通过命令行上运行测试。 例如，测试的代码示例 cmdlet。 有关使用命令行中的 cmdlet 的详细信息，请参阅[Windows PowerShell 入门学习](/powershell/scripting/getting-started/getting-started-with-windows-powershell)。

- 在 Windows PowerShell 提示符处，输入以下命令来检索通过管道的进程名称。

    ```powershell
    PS> type ProcessNames | get-proc
    ```

将显示以下输出。

    ```
    Handles  NPM(K)  PM(K)   WS(K)  VS(M)  CPU(s)    Id  ProcessName
    -------  ------  -----   ----- -----   ------    --  -----------
        809      21  40856    4448    147    9.50  2288  iexplore
        737      21  26036   16348    144   22.03  3860  iexplore
         39       2   1024     388     30    0.08  3396  notepad
       3927      62  71836   26984    467  195.19  1848  OUTLOOK
    ```

- 输入以下行，以获取进程对象具有`Name`从名为"IEXPLORE"的进程的属性。 此示例使用`Get-Process`作为上游命令来检索"IEXPLORE"进程 cmdlet （由 Windows PowerShell 提供）。

    ```powershell
    PS> get-process iexplore | get-proc
    ```

将显示以下输出。

    ```
    Handles  NPM(K)  PM(K)   WS(K)  VS(M)  CPU(s)    Id  ProcessName
    -------  ------  -----      ----- -----   ------     -- -----------
        801      21  40720    6544    142    9.52  2288  iexplore
        726      21  25872   16652    138   22.09  3860  iexplore
        801      21  40720    6544    142    9.52  2288  iexplore
        726      21  25872   16652    138   22.09  3860  iexplore
    ```

## <a name="see-also"></a>另请参阅

[添加处理命令行输入的参数](./adding-parameters-that-process-command-line-input.md)

[创建第一个 Cmdlet](./creating-a-cmdlet-without-parameters.md)

[扩展对象类型和格式设置](/previous-versions//ms714665(v=vs.85))

[如何注册 Cmdlet、 提供程序，和托管应用程序](/previous-versions//ms714644(v=vs.85))

[Windows PowerShell 参考](../windows-powershell-reference.md)

[Cmdlet 示例](./cmdlet-samples.md)
