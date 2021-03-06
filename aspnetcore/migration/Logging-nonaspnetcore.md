---
title: 從 Microsoft 的副檔名。記錄2.1 至2.2 或3。0
author: pakrym
description: 瞭解如何遷移使用 Microsoft 的 non-ASP.NET Core 應用程式。記錄從2.1 到2.2 或3.0。
ms.author: pakrym
ms.custom: mvc
ms.date: 01/04/2019
no-loc:
- ASP.NET Core Identity
- cookie
- Cookie
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: migration/logging-nonaspnetcore
ms.openlocfilehash: 8ef07118e3a885e41221402bdfc2d7e942c6dd73
ms.sourcegitcommit: 65add17f74a29a647d812b04517e46cbc78258f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/19/2020
ms.locfileid: "88634094"
---
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a>從 Microsoft 的副檔名。記錄2.1 至2.2 或3。0

本文概述將使用 `Microsoft.Extensions.Logging` 2.1 至2.2 或3.0 的 Non-ASP.NET Core 應用程式遷移的一般步驟。

## <a name="21-to-22"></a>2.1 至 2.2

手動建立 `ServiceCollection` 和呼叫 `AddLogging` 。

2.1 範例：

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

2.2 範例：

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a>2.1 至3。0

在3.0 中，請使用 `LoggingFactory.Create` 。

2.1 範例：

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

3.0 範例：

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a>其他資源

* [記錄]。[主控台 NuGet 套件](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/)。
* <xref:fundamentals/logging/index>
