---
ms.date: 06/12/2017
keywords: dsc,powershell,配置,安装程序
title: 构建自定义 Windows PowerShell Desired State Configuration 资源
ms.openlocfilehash: 882b6efed4564d2354183d7472b301e1e1758335
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "62076865"
---
# <a name="build-custom-windows-powershell-desired-state-configuration-resources"></a>构建自定义 Windows PowerShell Desired State Configuration 资源

> 适用于：Windows PowerShell 4.0 和 Windows PowerShell 5.0

Windows PowerShell Desired State Configuration (DSC) 具有可用于配置环境的内置资源。 本主题通过特定信息和示例概述了与主题相关的开发资源和链接。

## <a name="dsc-resource-components"></a>DSC 资源组件

DSC 资源是一个 Windows PowerShell 模块。 该模块既包含资源的架构（可配置属性的定义）又包含资源的实现（执行配置指定的实际工作的代码）。 可在 MOF 文件中定义 DSC 资源架构，由脚本模块执行实现。 从版本 5 开始支持 PowerShell 类，因此架构和实现都可以在类中定义。 以下主题对如何创建 DSC 资源进行了更详细的介绍。

* [使用 MOF 编写自定义 DSC 资源](authoringResourceMOF.md)
* [在 C# 中实现 DSC 资源](authoringResourceMofCS.md)
* [使用 PowerShell 类编写自定义 DSC 资源](authoringResourceClass.md)
* [复合资源：将 DSC 配置用作资源](authoringResourceComposite.md)
* [使用资源设计器工具](../authoringResourceMofDesigner.md)
