---
title: .NET Core 與 ASP.NET Core 中的記錄
author: rick-anderson
description: 了解如何使用由 Microsoft.Extensions.Logging NuGet 套件提供的記錄架構。
ms.author: riande
ms.custom: mvc
ms.date: 6/29/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/logging/index
ms.openlocfilehash: a4962c3132c60b68cb2d4b5a97e9b082aba15d15
ms.sourcegitcommit: 50e7c970f327dbe92d45eaf4c21caa001c9106d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2020
ms.locfileid: "87330717"
---
# <a name="logging-in-net-core-and-aspnet-core"></a>.NET Core 與 ASP.NET Core 中的記錄

::: moniker range=">= aspnetcore-3.0"

By [Kirk Larkin](https://twitter.com/serpent5)、 [Juergen Gutsch](https://github.com/JuergenGutsch)和[Rick Anderson](https://twitter.com/RickAndMSFT)

.NET Core 支援記錄 API，此 API 能與各種內建和第三方記錄提供者搭配使用。 此文章說明如何搭配內建提供者使用 API。

本文中顯示的大部分程式碼範例都來自 ASP.NET Core 應用程式。 這些程式碼片段的記錄特定部分適用于任何使用[泛型主機](xref:fundamentals/host/generic-host)的 .net Core 應用程式。 ASP.NET Core web 應用程式範本會使用泛型主機。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/3.x)（[如何下載](xref:index#how-to-download-a-sample)）

<a name="lp"></a>

## <a name="logging-providers"></a>記錄提供者

記錄提供者會儲存記錄檔，但 `Console` 顯示記錄的提供者除外。 例如，Azure 應用程式 Insights 提供者會將記錄儲存在 Azure 應用程式的深入解析中。 可以啟用多個提供者。

預設 ASP.NET Core web 應用程式範本：

* 使用[泛型主機](xref:fundamentals/host/generic-host)。
* 呼叫 <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A> ，這會新增下列記錄提供者：
  * [主控台](#console)
  * [偵錯](#debug)
  * [EventSource](#event-source)
  * [EventLog](#welog)：僅限 Windows

[!code-csharp[](index/samples/3.x/TodoApiDTO/Program.cs?name=snippet_TemplateCode&highlight=9)]

上述程式碼會顯示 `Program` 使用 ASP.NET Core web 應用程式範本所建立的類別。 接下來的幾節會根據使用泛型主機的 ASP.NET Core web 應用程式範本來提供範例。 本檔稍後會討論[非主機主控台應用程式](#nhca)。

若要覆寫所新增的預設記錄提供者集合 `Host.CreateDefaultBuilder` ，請呼叫 `ClearProviders` 並加入所需的記錄提供者。 例如，下列程式碼：

* 呼叫 <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> ，以從產生器中移除所有 <xref:Microsoft.Extensions.Logging.ILoggerProvider> 實例。
* 加入[主控台](#console)記錄提供者。

[!code-csharp[](index/samples/3.x/TodoApiDTO/Program.cs?name=snippet_AddProvider&highlight=5-6)]

如需其他提供者，請參閱：

* [內建記錄提供者](#bilp)
* [協力廠商記錄提供者](#third-party-logging-providers)。

## <a name="create-logs"></a>建立記錄

若要建立記錄，請使用相依性 <xref:Microsoft.Extensions.Logging.ILogger%601> [插入](xref:fundamentals/dependency-injection)（DI）中的物件。

下列範例將：

* 建立記錄器， `ILogger<AboutModel>` 它會使用類型之完整名稱的記錄*類別* `AboutModel` 。 記錄「類別」是與每個記錄關聯的字串。
* <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>在層級通話記錄檔 `Information` 。 記錄「層級」** 指出已記錄事件的嚴重性。

[!code-csharp[](index/samples/3.x/TodoApiDTO/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=5,14)]

本檔稍後將更詳細說明[層級](#log-level)和[類別](#log-category)。

如需的詳細資訊 Blazor ，請參閱本檔[中 Blazor Blazor WebAssembly 的建立記錄](#clib)檔。

[在主要和啟動中建立記錄](#clms)顯示如何在和中建立記錄 `Main` `Startup` 。

## <a name="configure-logging"></a>設定記錄

記錄設定通常是由 appsettings 的 `Logging` 區段所*appsettings*提供。 `{Environment}`*json*檔案。 下列*appsettings.Development.js*檔案是由 ASP.NET Core web 應用程式範本所產生：

[!code-json[](index/samples/3.x/TodoApiDTO/appsettings.Development.json)]

在上述 JSON 中：

* `"Default"` `"Microsoft"` 已指定、和 `"Microsoft.Hosting.Lifetime"` 類別。
* `"Microsoft"`類別目錄會套用至以開頭的所有類別 `"Microsoft"` 。 例如，此設定會套用至 `"Microsoft.AspNetCore.Routing.EndpointMiddleware"` 類別。
* `"Microsoft"`記錄層級和更新版本的類別記錄 `Warning` 。
* `"Microsoft.Hosting.Lifetime"`類別比類別更明確 `"Microsoft"` ，因此 `"Microsoft.Hosting.Lifetime"` 類別目錄記錄的記錄層級為「資訊」和更高。
* 未指定特定的記錄提供者，因此會 `LogLevel` 套用至所有已啟用的記錄提供者，但不包括[Windows EventLog](#welog)。

`Logging`屬性可以有 <xref:Microsoft.Extensions.Logging.LogLevel> 和記錄提供者屬性。 會 `LogLevel` 指定要記錄所選分類的最低[層級](#log-level)。 在先前的 JSON 中 `Information` ， `Warning` 會指定和記錄層級。 `LogLevel`指出記錄檔的嚴重性，範圍從0到6：

`Trace`= 0、 `Debug` = 1、 `Information` = 2、 `Warning` = 3、 `Error` = 4、 `Critical` = 5 和 `None` = 6。

當 `LogLevel` 指定時，會針對指定層級和更新版本中的訊息啟用記錄。 在先前的 JSON 中， `Default` 類別目錄會針對 `Information` 和更高版本進行記錄。 例如， `Information` `Warning` 會記錄、、 `Error` 和 `Critical` 訊息。 如果未 `LogLevel` 指定，則記錄預設為 `Information` 層級。 如需詳細資訊，請參閱[記錄層級](#llvl)。

提供者屬性可以指定 `LogLevel` 屬性。 `LogLevel`在提供者底下，指定該提供者的記錄層級，並覆寫非提供者記錄檔設定。 請考慮下列*appsettings.js*檔案：

[!code-json[](index/samples/3.x/TodoApiDTO/appsettings.Prod2.json)]

中的設定會覆 `Logging.{providername}.LogLevel` 寫中的設定 `Logging.LogLevel` 。 在上述 JSON 中， `Debug` 提供者的預設記錄層級會設定為 `Information` ：

`Logging:Debug:LogLevel:Default:Information`

上述設定會 `Information` 針對每個類別目錄指定不同的記錄層級 `Logging:Debug:` `Microsoft.Hosting` 。 當列出特定分類時，特定類別會覆寫預設分類。 在上述 JSON 中， `Logging:Debug:LogLevel` 類別和會覆 `"Microsoft.Hosting"` `"Default"` 寫中的設定`Logging:LogLevel`

您可以為下列任一項指定最小記錄層級：

* 特定提供者：例如，`Logging:EventSource:LogLevel:Default:Information`
* 特定類別：例如，`Logging:LogLevel:Microsoft:Warning`
* 所有提供者和所有類別：`Logging:LogLevel:Default:Warning`

低於最低層級的任何記錄都***不***是：

* 傳遞至提供者。
* 已記錄或顯示。

若要隱藏所有記錄，請指定[LogLevel](xref:Microsoft.Extensions.Logging.LogLevel)。 `LogLevel.None`的值為6，其高於 `LogLevel.Critical` （5）。

如果提供者支援[記錄範圍](#logscopes)，則 `IncludeScopes` 指出是否已啟用。 如需詳細資訊，請參閱[記錄範圍](#logscopes)

下列*appsettings.json*檔案包含預設啟用的所有提供者：

[!code-json[](index/samples/3.x/TodoApiDTO/appsettings.Production.json)]

在上述範例中：

* 類別和層級不是建議的值。 提供的範例會顯示所有預設的提供者。
* 中的設定會覆 `Logging.{providername}.LogLevel` 寫中的設定 `Logging.LogLevel` 。 例如，中的層級會 `Debug.LogLevel.Default` 覆寫中的層級 `LogLevel.Default` 。
* 會使用每個預設的提供者*別名*。 每個提供者都會定義「別名」**，可在設定中用來取代完整類型名稱。 內建提供者別名如下：
  * 主控台
  * 偵錯
  * EventSource
  * EventLog
  * AzureAppServicesFile
  * AzureAppServicesBlob
  * ApplicationInsights

## <a name="set-log-level-by-command-line-environment-variables-and-other-configuration"></a>依命令列、環境變數及其他設定來設定記錄層級

記錄層級可以由任何設定[提供者](xref:fundamentals/configuration/index)來設定。 

[!INCLUDE[](~/includes/environmentVarableColon.md)]

下列命令：

* 將環境索引鍵設定 `Logging:LogLevel:Microsoft` 為 `Information` Windows 上的值。
* 使用以 ASP.NET Core web 應用程式範本建立的應用程式時，請測試設定。 `dotnet run`使用之後，必須在專案目錄中執行命令 `set` 。

```cmd
set Logging__LogLevel__Microsoft=Information
dotnet run
```

先前的環境設定：

* 只會在從其設定所在的命令視窗中啟動的進程中設定。
* 以 Visual Studio 啟動的瀏覽器不會讀取。

下列[setx](/windows-server/administration/windows-commands/setx)命令也會在 Windows 上設定環境金鑰和值。 不同于 `set` ， `setx` 設定會保存下來。 `/M`參數會設定系統內容中的變數。 如果 `/M` 未使用，則會設定使用者環境變數。

```cmd
setx Logging__LogLevel__Microsoft=Information /M
```

在[Azure App Service](https://azure.microsoft.com/services/app-service/)上，選取 [**設定] > 設定**] 頁面上的 [**新增應用程式設定**]。 Azure App Service 的應用程式設定如下：

* 待用加密，並透過加密通道傳輸。
* 公開為環境變數。

如需詳細資訊，請參閱 [Azure App：使用 Azure 入口網站覆寫應用程式設定](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)。

如需使用環境變數設定 ASP.NET Core 設定值的詳細資訊，請參閱[環境變數](xref:fundamentals/configuration/index#environment-variables)。 如需使用其他設定來源（包括命令列、Azure Key Vault、Azure 應用程式組態、其他檔案格式等等）的詳細資訊，請參閱 <xref:fundamentals/configuration/index> 。

## <a name="how-filtering-rules-are-applied"></a>如何套用篩選規則

建立 <xref:Microsoft.Extensions.Logging.ILogger%601> 物件時，<xref:Microsoft.Extensions.Logging.ILoggerFactory> 物件會針對每個提供者選取一個規則來套用到該記錄器。 由 `ILogger` 執行個體寫入的所有訊息都會根據選取的規則進行篩選。 會從可用的規則中選取每個提供者和類別目錄配對的最特定規則。

當建立指定類別的 `ILogger` 時，系統會針對每個提供者使用下列演算法：

* 選取所有符合提供者或其別名的規則。 如果找不到符合的項目，請選取所有規則搭配空白提供者。
* 從上一個步驟的結果中，選取具有最長相符類別前置字元的規則。 如果找不到符合的項目，請選取未指定類別的所有規則。
* 如果選取多個規則，請使用**最後**一個。
* 如果未選取任何規則，請使用 `MinimumLevel`。

<a name="dnrvs"></a>

## <a name="logging-output-from-dotnet-run-and-visual-studio"></a>記錄來自 dotnet 執行和 Visual Studio 的輸出

系統會顯示使用[預設記錄提供者](#lp)所建立的記錄：

* 在 Visual Studio 中
  * 在調試時的 [調試輸出] 視窗中。
  * 在 [ASP.NET Core Web 服務器] 視窗中。
* 在以執行應用程式時的主控台視窗中 `dotnet run` 。

開頭為 "Microsoft" 類別的記錄來自 ASP.NET Core framework 程式碼。 ASP.NET Core 和應用程式程式碼會使用相同的記錄 API 和提供者。

<a name="lcat"></a>

## <a name="log-category"></a>記錄分類

`ILogger`建立物件時，會指定*分類*。 該類別會包含在每個由該 `ILogger` 執行個體所產生的記錄訊息中。 類別字串是任意的，但慣例是使用類別名稱。 例如，在控制器中，名稱可能是 `"TodoApi.Controllers.TodoController"` 。 ASP.NET Core web 應用程式 `ILogger<T>` 會使用來自動取得 `ILogger` 使用的完整類型名稱 `T` 做為分類的實例：

[!code-csharp[](index/samples/3.x/TodoApiDTO/Pages/Privacy.cshtml.cs?name=snippet)]

若要明確指定類別，請呼叫 `ILoggerFactory.CreateLogger`：

[!code-csharp[](index/samples/3.x/TodoApiDTO/Pages/Contact.cshtml.cs?name=snippet)]

`ILogger<T>` 相當於使用 `T`的完整類型名稱來呼叫 `CreateLogger`。

<a name="llvl"></a>

## <a name="log-level"></a>記錄層級

下表列出 <xref:Microsoft.Extensions.Logging.LogLevel> 值、便利性 `Log{LogLevel}` 擴充方法，以及建議的使用方式：

| LogLevel | 值 | 方法 | 描述 |
| -------- | ----- | ------ | ----------- |
| [追蹤](xref:Microsoft.Extensions.Logging.LogLevel) | 0 | <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogTrace%2A> | 包含最詳細的訊息。 這些訊息可能包含敏感性應用程式資料。 這些訊息預設為停用，且***不***應在生產環境中啟用。 |
| [偵錯](xref:Microsoft.Extensions.Logging.LogLevel) | 1 | <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogDebug%2A> | 用於偵錯工具和開發。 請在生產環境中小心使用，因為有大量的數量。 |
| [資訊](xref:Microsoft.Extensions.Logging.LogLevel) | 2 | <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation%2A> | 追蹤應用程式的一般流程。 可能具有長期值。 |
| [警告](xref:Microsoft.Extensions.Logging.LogLevel) | 3 | <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogWarning%2A> | 針對異常或非預期的事件。 通常會包含不會導致應用程式失敗的錯誤或狀況。 |
| [錯誤](xref:Microsoft.Extensions.Logging.LogLevel) | 4 | <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError%2A> | 發生無法處理的錯誤和例外狀況。 這些訊息表示目前的作業或要求失敗，而不是整個應用程式的失敗。 |
| [嚴重](xref:Microsoft.Extensions.Logging.LogLevel) | 5 | <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogCritical%2A> | 發生需要立即注意的失敗。 範例：資料遺失情況、磁碟空間不足。 |
| [None](xref:Microsoft.Extensions.Logging.LogLevel) | 6 | | 指定記錄類別不應寫入任何訊息。 |

在上表中， `LogLevel` 會從最低到最高的嚴重性列出。

[記錄](xref:Microsoft.Extensions.Logging.LoggerExtensions)方法的第一個參數， <xref:Microsoft.Extensions.Logging.LogLevel> 表示記錄檔的嚴重性。 `Log(LogLevel, ...)`大部分的開發人員會呼叫[Log {LogLevel}](xref:Microsoft.Extensions.Logging.LoggerExtensions)擴充方法，而不是呼叫。 `Log{LogLevel}`擴充方法會[呼叫 Log 方法並指定 LogLevel](https://github.com/dotnet/extensions/blob/release/3.1/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs)。 例如，下列兩個記錄呼叫的功能相同，且會產生相同的記錄檔：

[!code-csharp[](index/samples/3.x/TodoApiDTO/Controllers/TestController.cs?name=snippet0&highlight=6-7)]

`MyLogEvents.TestItem`這是事件識別碼。 `MyLogEvents`是範例應用程式的一部分，而且會顯示在 [[記錄檔事件識別碼](#leid)] 區段中。

[!INCLUDE[](~/includes/MyDisplayRouteInfoBoth.md)]

下列程式碼會建立 `Information` 與 `Warning` 記錄：

[!code-csharp[](index/samples/3.x/TodoApiDTO/Controllers/TodoItemsController.cs?name=snippet_CallLogMethods&highlight=4,10)]

在上述程式碼中，第一個 `Log{LogLevel}` 參數 `MyLogEvents.GetItem` 是[記錄事件識別碼](#leid)。 第二個參數是訊息範本，其中的預留位置會置入其餘方法參數所提供的引數值。 本檔稍後的「[訊息範本](#lmt)」一節中會說明方法參數。

呼叫適當的 `Log{LogLevel}` 方法，以控制要將多少記錄輸出寫入特定的儲存媒體。 例如：

* 在生產環境中：
  * 在 `Trace` 或 `Information` 層級記錄會產生大量的詳細記錄訊息。 若要控制成本，而不超過資料儲存體限制，請記錄 `Trace` 和 `Information` 層級訊息到高容量、低成本的資料存放區。 請考慮 `Trace` 將和限制 `Information` 在特定的類別目錄。
  * `Warning`透過 `Critical` 層級登入應該會產生幾個記錄訊息。
    * 成本和儲存體限制通常不是問題。
    * 少數記錄可讓您更有彈性地選擇資料存放區。
* 開發中：
  * 設定為 `Warning`。
  * `Trace` `Information` 疑難排解時新增或訊息。 若要限制輸出， `Trace` 請 `Information` 針對 [調查] 底下的類別設定或。

ASP.NET Core 會寫入架構事件的記錄。 例如，請考慮的記錄輸出：

* Razor使用 ASP.NET Core 範本建立的頁面應用程式。
* 記錄設定為`Logging:Console:LogLevel:Microsoft:Information`
* 流覽至 [隱私權] 頁面：

```console
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 GET https://localhost:5001/Privacy
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint '/Privacy'
info: Microsoft.AspNetCore.Mvc.RazorPages.Infrastructure.PageActionInvoker[3]
      Route matched with {page = "/Privacy"}. Executing page /Privacy
info: Microsoft.AspNetCore.Mvc.RazorPages.Infrastructure.PageActionInvoker[101]
      Executing handler method DefaultRP.Pages.PrivacyModel.OnGet - ModelState is Valid
info: Microsoft.AspNetCore.Mvc.RazorPages.Infrastructure.PageActionInvoker[102]
      Executed handler method OnGet, returned result .
info: Microsoft.AspNetCore.Mvc.RazorPages.Infrastructure.PageActionInvoker[103]
      Executing an implicit handler method - ModelState is Valid
info: Microsoft.AspNetCore.Mvc.RazorPages.Infrastructure.PageActionInvoker[104]
      Executed an implicit handler method, returned result Microsoft.AspNetCore.Mvc.RazorPages.PageResult.
info: Microsoft.AspNetCore.Mvc.RazorPages.Infrastructure.PageActionInvoker[4]
      Executed page /Privacy in 74.5188ms
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint '/Privacy'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 149.3023ms 200 text/html; charset=utf-8
```

下列 JSON 集合 `Logging:Console:LogLevel:Microsoft:Information` ：

[!code-json[](index/samples/3.x/TodoApiDTO/appsettings.MSFT.json)]

<a name="leid"></a>

## <a name="log-event-id"></a>記錄事件識別碼

每個記錄都可以指定「事件識別碼」**。 範例應用程式會使用 `MyLogEvents` 類別來定義事件識別碼：

<!-- Review: to bad there is no way to use an enum for event ID's -->
[!code-csharp[](index/samples/3.x/TodoApiDTO/Models/MyLogEvents.cs?name=snippet_LoggingEvents)]

[!code-csharp[](index/samples/3.x/TodoApiDTO/Controllers/TodoItemsController.cs?name=snippet_CallLogMethods&highlight=4,10)]

事件識別碼會將一組事件關聯。 例如，與在頁面上顯示項目清單相關的所有記錄可能都是 1001。

記錄提供者可能將事件識別碼存放到識別碼欄位、記錄訊息中或完全不存放。 偵錯提供者不會顯示事件識別碼。 主控台提供者會在類別後面以括弧顯示事件識別碼：

```console
info: TodoApi.Controllers.TodoItemsController[1002]
      Getting item 1
warn: TodoApi.Controllers.TodoItemsController[4000]
      Get(1) NOT FOUND
```

有些記錄提供者會將事件識別碼儲存在欄位中，這可讓您篩選識別碼。

<a name="lmt"></a>

## <a name="log-message-template"></a>記錄訊息範本
<!-- Review, Each log API uses a message template. -->
每個記錄 API 都會使用訊息範本。 訊息範本可以包含有關提供哪個引數的預留位置。 使用名稱而非數字做為預留位置。

[!code-csharp[](index/samples/3.x/TodoApiDTO/Controllers/TodoItemsController.cs?name=snippet_CallLogMethods&highlight=4,10)]

預留位置的順序 (而不是其名稱) 會決定使用哪些參數來提供其值。 在下列程式碼中，參數名稱在訊息範本中不是順序的：

```csharp
string p1 = "param1";
string p2 = "param2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

上述程式碼會依序建立具有參數值的記錄訊息：

```text
Parameter values: param1, param2
```

這種方法可讓記錄提供者執行[語義或結構化記錄](https://github.com/NLog/NLog/wiki/How-to-use-structured-logging)。 引數本身會被傳遞到記錄系統，而不只是格式化的訊息範本。 這可讓記錄提供者將參數值儲存為欄位。 例如，請考慮下列記錄器方法：

```csharp
_logger.LogInformation("Getting item {Id} at {RequestTime}", id, DateTime.Now);
```

例如，記錄至 Azure 表格儲存體：

* 每個 Azure 資料表實體都可以有 `ID` 和 `RequestTime` 屬性。
* 具有屬性的資料表會簡化記錄資料的查詢。 例如，查詢可以尋找特定範圍內的所有記錄， `RequestTime` 而不需要剖析文字訊息的時間。

## <a name="log-exceptions"></a>記錄例外狀況

記錄器方法具有接受例外狀況參數的多載：

[!code-csharp[](index/samples/3.x/TodoApiDTO/Controllers/TestController.cs?name=snippet_Exp)]

[!INCLUDE[](~/includes/MyDisplayRouteInfoBoth.md)]

例外狀況記錄是提供者特有的。

### <a name="default-log-level"></a>預設記錄層級

如果未設定預設記錄層級，則預設的記錄層級值為 `Information` 。

例如，請考慮下列 web 應用程式：

* 使用 ASP.NET web 應用程式範本建立。
* 已刪除或重新命名時， *appsettings.json*和*appsettings.Development.js* 。

在上述設定中，流覽至 [隱私權] 或 [首頁] `Trace` 會 `Debug` `Information` `Microsoft` 在類別名稱中產生許多、和訊息。

下列程式碼會設定預設記錄層級未設定在 configuration 中時的預設記錄層級：

[!code-csharp[](index/samples/3.x/MyMain/Program.cs?name=snippet_MinLevel&highlight=10)]

<!-- review required: I say this a couple times -->
一般來說，記錄層級應指定于設定中，而不是程式碼。

### <a name="filter-function"></a>Filter 函數

針對未透過設定或程式碼指派規則的所有提供者和類別，會叫用篩選函數：

[!code-csharp[](index/samples/3.x/MyMain/Program.cs?name=snippet_FilterFunction)]

上述程式碼會在分類包含 `Controller` 或 `Microsoft` 且記錄層級為 `Information` 或更高時，顯示主控台記錄。

一般來說，記錄層級應指定于設定中，而不是程式碼。

## <a name="aspnet-core-and-ef-core-categories"></a>ASP.NET Core 和 EF Core 分類

下表包含 ASP.NET Core 和 Entity Framework Core 所使用的一些分類，以及記錄檔的相關注意事項：

| 類別                            | 注意 |
| ----------------------------------- | ----- |
| Microsoft.AspNetCore                | 一般 ASP.NET Core 診斷。 |
| Microsoft.AspNetCore.DataProtection | 已考慮、發現及使用哪些金鑰。 |
| Microsoft.AspNetCore.HostFiltering  | 允許主機。 |
| Microsoft.AspNetCore.Hosting        | HTTP 要求花了多少時間完成，以及其開始時間。 載入了哪些裝載啟動組件。 |
| Microsoft.AspNetCore.Mvc            | MVC 和 Razor 診斷。 模型繫結、篩選執行、檢視編譯、動作選取。 |
| Microsoft.AspNetCore.Routing        | 路由比對資訊。 |
| Microsoft.AspNetCore.Server         | 連線開始、停止與保持運作回應。 HTTPS 憑證資訊。 |
| Microsoft.AspNetCore.StaticFiles    | 提供的檔案。 |
| Microsoft.EntityFrameworkCore       | 一般 Entity Framework Core 診斷。 資料庫活動與設定、變更偵測、移轉。 |

若要在主控台視窗中查看更多類別，請將**appsettings.Development.js**設定為下列內容：

[!code-json[](index/samples/3.x/MyMain/appsettings.Trace.json)]

<!-- Review: What other providers support scopes? Console is not generally used in staging/production  -->

<a name="logscopes"></a>

## <a name="log-scopes"></a>記錄範圍

 「範圍」** 可用來將邏輯作業組成群組。 此分組功能可用來將相同的資料附加到已建立為集合之一部分的每個記錄。 例如，在處理邀交易時建立的每個記錄都可以包括該交易識別碼。

範圍：

* 是 <xref:System.IDisposable> 方法所傳回的類型 <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> 。
* 會持續到處置為止。

下列提供者支援範圍：

* `Console`
* [AzureAppServicesFile 和 AzureAppServicesBlob](xref:Microsoft.Extensions.Logging.AzureAppServices.BatchingLoggerOptions.IncludeScopes)

透過將記錄器呼叫封裝在 `using` 區塊中以使用範圍：

[!code-csharp[](index/samples/3.x/TodoApiDTO/Controllers/TestController.cs?name=snippet_Scopes)]

下列 JSON 會啟用主控台提供者的範圍：

[!code-json[](index/samples/3.x/TodoApiDTO/appsettings.Scopes.json)]

下列程式碼會啟用主控台提供者的範圍：

[!code-csharp[](index/samples/3.x/MyMain/Program.cs?name=snippet_Scopes)]

一般來說，您應該在設定中指定記錄，而不是在程式碼中指定。

<a name="bilp"></a>

## <a name="built-in-logging-providers"></a>內建記錄提供者

ASP.NET Core 包括下列記錄提供者：

* [主控台](#console)
* [偵錯](#debug)
* [EventSource](#event-source)
* [EventLog](#welog)
* [AzureAppServicesFile 和 AzureAppServicesBlob](#azure-app-service)
* [ApplicationInsights](#azure-application-insights)

如需有關 `stdout` 使用 ASP.NET Core 模組的記錄和偵錯工具的詳細資訊，請參閱 <xref:test/troubleshoot-azure-iis> 和 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection> 。

### <a name="console"></a>主控台

`Console`提供者會將輸出記錄到主控台。 如需有關在開發中查看記錄的詳細資訊 `Console` ，請參閱[dotnet run 和 Visual Studio 的記錄輸出](#dnrvs)。

### <a name="debug"></a>偵錯

`Debug`提供者會使用 system.servicemodel 類別來寫入[System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug)記錄輸出。 呼叫以 `System.Diagnostics.Debug.WriteLine` 寫入 `Debug` 提供者。

在 Linux 上， `Debug` 提供者記錄檔位置與散發相依，而且可能是下列其中一項：

* */var/log/message*
* */var/log/syslog*

### <a name="event-source"></a>事件來源

`EventSource`提供者會寫入名為的跨平臺事件來源 `Microsoft-Extensions-Logging` 。 在 Windows 上，提供者會使用[ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803)。

#### <a name="dotnet-trace-tooling"></a>dotnet 追蹤工具

[Dotnet 追蹤](/dotnet/core/diagnostics/dotnet-trace)工具是一種跨平臺 CLI 全域工具，可讓您收集執行中進程的 .net Core 追蹤。 此工具會 <xref:Microsoft.Extensions.Logging.EventSource> 使用來收集提供者資料 <xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource> 。

如需安裝指示，請參閱[dotnet-trace](/dotnet/core/diagnostics/dotnet-trace) 。

使用 dotnet 追蹤工具，從應用程式收集追蹤：

1. 使用命令執行應用程式 `dotnet run` 。
1. 判斷 .NET Core 應用程式的處理序識別碼（PID）：
   * 在 Windows 上，請使用下列其中一種方法：
     * 工作管理員（Ctrl + Alt + Del）
     * [tasklist 命令](/windows-server/administration/windows-commands/tasklist)
     * [取得進程 Powershell 命令](/powershell/module/microsoft.powershell.management/get-process)
   * 在 Linux 上，請使用[pidof 命令](https://refspecs.linuxfoundation.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/pidof.html)。

   尋找與應用程式元件同名之進程的 PID。

1. 執行 `dotnet trace` 命令。

   一般命令語法：

   ```dotnetcli
   dotnet trace collect -p {PID} 
       --providers Microsoft-Extensions-Logging:{Keyword}:{Event Level}
           :FilterSpecs=\"
               {Logger Category 1}:{Event Level 1};
               {Logger Category 2}:{Event Level 2};
               ...
               {Logger Category N}:{Event Level N}\"
   ```

   使用 PowerShell 命令 shell 時，將 `--providers` 值以單引號括住（ `'` ）：

   ```dotnetcli
   dotnet trace collect -p {PID} 
       --providers 'Microsoft-Extensions-Logging:{Keyword}:{Event Level}
           :FilterSpecs=\"
               {Logger Category 1}:{Event Level 1};
               {Logger Category 2}:{Event Level 2};
               ...
               {Logger Category N}:{Event Level N}\"'
   ```

   在非 Windows 平臺上，新增 `-f speedscope` 選項以將輸出追蹤檔案的格式變更為 `speedscope` 。

   | 關鍵字 | 描述 |
   | :-----: | ----------- |
   | 1       | 記錄有關的中繼事件 `LoggingEventSource` 。 不會記錄來自的事件 `ILogger` ）。 |
   | 2       | `Message`當呼叫時，開啟事件 `ILogger.Log()` 。 以程式設計方式（未格式化）提供資訊。 |
   | 4       | `FormatMessage`當呼叫時，開啟事件 `ILogger.Log()` 。 提供資訊的格式化字串版本。 |
   | 8       | `MessageJson`當呼叫時，開啟事件 `ILogger.Log()` 。 提供引數的 JSON 標記法。 |

   | 事件層級 | 描述     |
   | :---------: | --------------- |
   | 0           | `LogAlways`     |
   | 1           | `Critical`      |
   | 2           | `Error`         |
   | 3           | `Warning`       |
   | 4           | `Informational` |
   | 5           | `Verbose`       |

   `FilterSpecs`和的 `{Logger Category}` 專案 `{Event Level}` 代表其他記錄篩選準則。 `FilterSpecs`以分號（）分隔專案 `;` 。

   使用 Windows 命令 shell 的範例（值前後**沒有**單引號 `--providers` ）：

   ```dotnetcli
   dotnet trace collect -p {PID} --providers Microsoft-Extensions-Logging:4:2:FilterSpecs=\"Microsoft.AspNetCore.Hosting*:4\"
   ```

   上述命令會啟用：

   * 事件來源記錄器，用來產生 `4` 錯誤（）的格式化字串（） `2` 。
   * `Microsoft.AspNetCore.Hosting`在 `Informational` 記錄層級進行記錄（ `4` ）。

1. 按 Enter 鍵或 Ctrl + C 來停止 dotnet 追蹤工具。

   追蹤會以名稱*nettrace*儲存在命令執行所在的資料夾中 `dotnet trace` 。

1. 使用[Perfview](#perfview)開啟追蹤。 開啟*nettrace*檔案，並流覽追蹤事件。

如果應用程式未使用建立主機 `CreateDefaultBuilder` ，請將[事件來源提供者](#event-source-provider)新增至應用程式的記錄設定。

如需詳細資訊，請參閱

* [效能分析公用程式追蹤（dotnet-追蹤）](/dotnet/core/diagnostics/dotnet-trace) （.net Core 檔）
* [效能分析公用程式追蹤（dotnet 追蹤）](https://github.com/dotnet/diagnostics/blob/master/documentation/dotnet-trace-instructions.md) （dotnet/診斷 GitHub 存放庫檔）
* [LoggingEventSource 類別](xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource)（.Net API 瀏覽器）
* <xref:System.Diagnostics.Tracing.EventLevel>
* [LoggingEventSource 參考來源（3.0）](https://github.com/dotnet/extensions/blob/release/3.0/src/Logging/Logging.EventSource/src/LoggingEventSource.cs)：若要取得不同版本的參考來源，請將分支變更為 `release/{Version}` ，其中 `{Version}` 是所需 ASP.NET Core 的版本。
* [Perfview](#perfview)：適用于查看事件來源追蹤。

#### <a name="perfview"></a>Perfview

使用[PerfView 公用程式](https://github.com/Microsoft/perfview)來收集及查看記錄。 此外還有一些其他工具可檢視 ETW 記錄，但 PerfView 提供處理 ASP.NET Core 所發出 ETW 事件的最佳體驗。

若要設定 PerfView 以收集此提供者所記錄的事件，請將字串 `*Microsoft-Extensions-Logging` 新增至 [其他提供者]**** 清單 請不要錯過 `*` 字串開頭的。

<a name="welog"></a>

### <a name="windows-eventlog"></a>Windows EventLog

`EventLog`提供者會將記錄輸出傳送至 Windows 事件記錄檔。 與其他提供者不同的是， `EventLog` 提供者***不***會繼承預設的非提供者設定。 如果 `EventLog` 未指定記錄檔設定，它們會[預設為 LogLevel。](https://github.com/dotnet/extensions/blob/release/3.1/src/Hosting/Hosting/src/Host.cs#L99-L103)

若要記錄低於 <xref:Microsoft.Extensions.Logging.LogLevel.Warning?displayProperty=nameWithType> 的事件，請明確設定記錄層級。 下列範例會將事件記錄檔的預設記錄層級設定為 <xref:Microsoft.Extensions.Logging.LogLevel.Information?displayProperty=nameWithType> ：

```json
"Logging": {
  "EventLog": {
    "LogLevel": {
      "Default": "Information"
    }
  }
}
```

[AddEventLog](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions)多載可以傳入 <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings> 。 若 `null` 未指定，則會使用下列預設設定：

* `LogName`：「應用程式」
* `SourceName`： ".NET Runtime"
* `MachineName`：使用本機電腦名稱稱。

下列程式碼會將的 `SourceName` 預設值變更 `".NET Runtime"` 為 `MyLogs` ：

[!code-csharp[](index/samples/3.x/MyMain/Program.cs?name=snippetEventLog)]

### <a name="azure-app-service"></a>Azure App Service

[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) 提供者套件會將記錄寫入至 Azure App Service 應用程式檔案系統中的文字檔，並寫入至 Azure 儲存體帳戶中的 [Blob 儲存體](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage)。

該提供者套件並未包含在共用架構中。 若要使用提供者，請將提供者套件新增至專案。

如果要進行提供者設定，請使用 <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> 和 <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>，如以下範例中所示：

[!code-csharp[](index/samples/3.x/MyMain/Program.cs?name=snippet_AzLogOptions)]

當部署到 Azure App Service 時，應用程式會使用 Azure 入口網站 [ **App Service** ] 頁面的 [ [App Service 記錄](/azure/app-service/web-sites-enable-diagnostic-log/#enable-application-logging-windows)] 區段中的設定。 當下列設定更新時，變更會立即生效，而不需要重新啟動或重新部署應用程式。

* **應用程式記錄 (檔案系統)**
* **應用程式記錄 (Blob)**

記錄檔的預設位置為 *D:\\home\\LogFiles\\Application* 資料夾，而預設檔案名稱為 *diagnostics-yyyymmdd.txt*。 預設檔案大小限制為 10 MB，而預設保留的檔案數目上限為 2。 預設 Blob 名稱為 *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*。

此提供者只會在專案于 Azure 環境中執行時記錄。

#### <a name="azure-log-streaming"></a>Azure 記錄資料流

Azure 記錄串流支援從下列時間即時查看記錄活動：

* 應用程式伺服器
* 網頁伺服器
* 失敗的要求追蹤

若要設定 Azure 記錄資料流：

* 從應用程式的入口網站頁面流覽至 [ **App Service 記錄**] 頁面。
* 將 [應用程式記錄 (檔案系統)]**** 設定為 [開啟]****。
* 選擇記錄 [層級]****。 此設定僅適用于 Azure 記錄串流。

流覽至 [**記錄資料流程**] 頁面以查看記錄。 記錄的訊息會以介面記錄 `ILogger` 。

### <a name="azure-application-insights"></a>Azure Application Insights

[ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights)提供者套件會將記錄寫入[Azure 應用程式深入](/azure/azure-monitor/app/cloudservices)解析。 Application Insights 是可監視 Web 應用程式的服務，並提供可用來查詢及分析遙測資料的工具。 如果您使用此提供者，就可以使用 Application Insights 工具來查詢及分析記錄。

記錄提供者會以 [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) \(英文\) 的相依性形式隨附，這是針對 ASP.NET Core 提供所有可用遙測的套件。 如果您使用此套件，就不需安裝提供者套件。

[ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web)套件適用于 ASP.NET 4.x，而不是 ASP.NET Core。

如需詳細資訊，請參閱下列資源：

* [Application Insights 概觀](/azure/application-insights/app-insights-overview)
* [適用於 ASP.NET Core 應用程式的 Application Insights](/azure/azure-monitor/app/asp-net-core)：如果您想要實作完整範圍的 Application Insights 遙測以及記錄，請從這裡開始。
* [適用於 .NET Core ILogger 記錄的 ApplicationInsightsLoggerProvider](/azure/azure-monitor/app/ilogger)：如果您想要實作記錄提供者，而不需要 Application Insights 遙測的其餘部分，請從這裡開始。
* [Application Insights logging adapters](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs) (Application Insights 記錄配接器)。
* [安裝、設定及初始化 Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights)：Microsoft Learn 網站上的互動式教學課程。

## <a name="third-party-logging-providers"></a>協力廠商記錄提供者

可搭配 ASP.NET Core 使用的協力廠商記錄架構：

* [elmah.io](https://elmah.io/) ([GitHub 存放庫](https://github.com/elmahio/Elmah.Io.Extensions.Logging))
* [Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub 存放庫](https://github.com/mattwcole/gelf-extensions-logging))
* [JSNLog](https://jsnlog.com/) ([GitHub 存放庫](https://github.com/mperdeck/jsnlog))
* [KissLog.net](https://kisslog.net/) ([GitHub 存放庫](https://github.com/catalingavan/KissLog-net))
* [Log4Net](https://logging.apache.org/log4net/) （[GitHub](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore)存放庫）
* [Loggr](https://loggr.net/) ([GitHub 存放庫](https://github.com/imobile3/Loggr.Extensions.Logging))
* [NLog](https://nlog-project.org/) ([GitHub 存放庫](https://github.com/NLog/NLog.Extensions.Logging))
* [PLogger](https://www.nuget.org/packages/InvertedSoftware.PLogger.Core/) （[GitHub](https://github.com/invertedsoftware/InvertedSoftware.PLogger.Core)存放庫）
* [Sentry](https://sentry.io/welcome/) ([GitHub 存放庫](https://github.com/getsentry/sentry-dotnet))
* [Serilog](https://serilog.net/) ([GitHub 存放庫](https://github.com/serilog/serilog-aspnetcore))
* [Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github 存放庫](https://github.com/googleapis/google-cloud-dotnet))

某些協力廠商架構可以執行[語意記錄 (也稱為結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) \(英文\)。

使用協力廠商架構類似於使用內建的提供者之一：

1. 將 NuGet 套件新增至專案。
1. 呼叫 `ILoggerFactory` 記錄架構所提供的擴充方法。

如需詳細資訊，請參閱每個提供者的文件。 Microsoft 不支援第三方記錄提供者。

<a name="nhca"></a>

## <a name="non-host-console-app"></a>非主機主控台應用程式

如需如何在非 web 主控台應用程式中使用泛型主機的範例，請參閱[背景工作範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples)的*Program.cs*檔案（ <xref:fundamentals/host/hosted-services> ）。

不含一般主機的應用程式記錄程式碼，會因[新增提供者](#add-providers)和[建立記錄器](#create-logs)的方式而有所不同。 

### <a name="logging-providers"></a>記錄提供者

在非主機主控台應用程式中，於建立 `LoggerFactory` 時呼叫提供者的 `Add{provider name}` 擴充方法：

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=11-12)]

### <a name="create-logs"></a>建立記錄

若要建立記錄，請使用 <xref:Microsoft.Extensions.Logging.ILogger%601> 物件。 使用 `LoggerFactory` 來建立 `ILogger` 。

下列範例會使用 `LoggingConsoleApp.Program` 做為類別目錄來建立記錄器。

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=14)]

在下列 ASP.NET 核心範例中，記錄器是用來建立記錄，並以 `Information` 做為層級。 記錄「層級」** 指出已記錄事件的嚴重性。

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=15)]

本檔會更詳細地說明[層級](#log-level)和[類別](#log-category)。

<a name="lhc"></a>

## <a name="log-during-host-construction"></a>在主機結構中記錄

不直接支援在主機結構期間進行記錄。 不過，您可以使用個別的記錄器。 在下列範例中，會使用 [Serilog](https://serilog.net/) 記錄器來記錄 `CreateHostBuilder`。  `AddSerilog`會使用中指定的靜態設定 `Log.Logger` ：

```csharp
using System;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Logging;

public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args)
    {
        var builtConfig = new ConfigurationBuilder()
            .AddJsonFile("appsettings.json")
            .AddCommandLine(args)
            .Build();

        Log.Logger = new LoggerConfiguration()
            .WriteTo.Console()
            .WriteTo.File(builtConfig["Logging:FilePath"])
            .CreateLogger();

        try
        {
            return Host.CreateDefaultBuilder(args)
                .ConfigureServices((context, services) =>
                {
                    services.AddRazorPages();
                })
                .ConfigureAppConfiguration((hostingContext, config) =>
                {
                    config.AddConfiguration(builtConfig);
                })
                .ConfigureLogging(logging =>
                {   
                    logging.AddSerilog();
                })
                .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseStartup<Startup>();
                });
        }
        catch (Exception ex)
        {
            Log.Fatal(ex, "Host builder error");

            throw;
        }
        finally
        {
            Log.CloseAndFlush();
        }
    }
}
```

<a name="csdi"></a>

## <a name="configure-a-service-that-depends-on-ilogger"></a>設定相依于 ILogger 的服務

記錄器的建構函式插入 `Startup` 適用於舊版 ASP.NET Core，因為會為 Web 主機建立個別的 DI 容器。 如需為何只為一般主機建立一個容器的資訊，請參閱[重大變更公告](https://github.com/aspnet/Announcements/issues/353)。

若要設定相依于的服務 `ILogger<T>` ，請使用「處理常式」插入或提供 factory 方法。 只有在沒有其他選項時，才建議使用 Factory 方法。 例如，假設有一個服務需要 DI 所 `ILogger<T>` 提供的實例：

[!code-csharp[](index/samples/3.x/TodoApiSample/Startup2.cs?name=snippet_ConfigureServices&highlight=6-10)]

上述反白顯示的程式碼是在第一次使用 DI 容器來建立實例時，所執行的[Func](/dotnet/api/system.func-2) `MyService` 。 您可以用此方式存取任何已註冊的服務。

<a name="clms"></a>

## <a name="create-logs-in-main"></a>在 Main 中建立記錄

下列程式碼會在 `Main` `ILogger` 建立主機之後，從 DI 取得實例來登入：

[!code-csharp[](index/samples/3.x/TodoApiDTO/Program.cs?name=snippet_LogProgram)]

### <a name="create-logs-in-startup"></a>在啟動中建立記錄

下列程式碼會寫入中的記錄 `Startup.Configure` ：

[!code-csharp[](index/samples/3.x/TodoApiDTO/Startup.cs?name=snippet_Configure)]

在以 `Startup.ConfigureServices` 方法完成 DI 容器設定之前，不支援寫入記錄檔：

* 不支援將記錄器插入 `Startup` 建構函式。
* 不支援將記錄器插入 `Startup.ConfigureServices` 方法簽章

這項限制的原因是記錄相依於 DI 和組態，而後者相依於 DI。 在 `ConfigureServices` 完成後才會設定 DI 容器。

如需設定服務的相關資訊，而這項服務會相依于 `ILogger<T>` 或為何要 `Startup` 在舊版中處理記錄器的程式，請參閱[設定相依于 ILogger 的服務](#csdi)

### <a name="no-asynchronous-logger-methods"></a>無非同步記錄器方法

記錄速度應該很快，不值得花費非同步程式碼的效能成本來處理。 如果記錄資料存放區的速度很慢，請不要直接寫入。 請考慮一開始先將記錄檔訊息寫入快速存放區，然後再將它們移到慢速存放區。 例如，當記錄到 SQL Server 時，請不要直接在方法中執行這項操作 `Log` ，因為 `Log` 方法是同步的。 相反地，以同步方式將記錄訊息新增到記憶體內佇列，並讓背景工作角色提取出佇列的訊息，藉此執行推送資料到 SQL Server 的非同步工作。 如需詳細資訊，請參閱[此](https://github.com/dotnet/AspNetCore.Docs/issues/11801)GitHub 問題。

<a name="clib"></a>

## <a name="change-log-levels-in-a-running-app"></a>變更執行中應用程式的記錄層級

記錄 API 不包含在應用程式執行時變更記錄層級的案例。 不過，某些設定提供者能夠重載設定，這會立即影響記錄設定。 例如，檔案設定[提供者](xref:fundamentals/configuration/index#file-configuration-provider)預設會重載記錄設定。 如果應用程式正在執行時，程式碼中的設定有所變更，應用程式可以呼叫[IConfigurationRoot](xref:Microsoft.Extensions.Configuration.IConfigurationRoot.Reload*)來更新應用程式的記錄設定。

## <a name="ilogger-and-iloggerfactory"></a>ILogger 和 ILoggerFactory

<xref:Microsoft.Extensions.Logging.ILogger%601>和 <xref:Microsoft.Extensions.Logging.ILoggerFactory> 介面和實作為包含在 .NET Core SDK 中。 下列 NuGet 套件也提供這些功能：  

* 介面位於[Microsoft. Extensions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/)。
* 預設的實作為 [[記錄](https://www.nuget.org/packages/microsoft.extensions.logging/)]。

<!-- review. Why would you want to hard code filtering rules in code? -->
<a name="fric"></a>

## <a name="apply-log-filter-rules-in-code"></a>在程式碼中套用記錄篩選規則

設定記錄篩選規則的慣用方法是使用[Configuration](xref:fundamentals/configuration/index)。

下列範例說明如何在程式碼中註冊篩選規則：

[!code-csharp[](index/samples/3.x/MyMain/Program.cs?name=snippet_FilterInCode)]

`logging.AddFilter("System", LogLevel.Debug)`指定 `System` 類別目錄和記錄層級 `Debug` 。 篩選準則會套用至所有提供者，因為未設定特定的提供者。

`AddFilter<DebugLoggerProvider>("Microsoft", LogLevel.Information)`指出

* `Debug`記錄提供者。
* 記錄層級 `Information` 和更新版本。
* 所有以為開頭的分類 `"Microsoft"` 。

## <a name="create-a-custom-logger"></a>建立自訂記錄器

若要新增自訂記錄器，請新增 <xref:Microsoft.Extensions.Logging.ILoggerProvider> 具有 <xref:Microsoft.Extensions.Logging.ILoggerFactory> 下列內容的：

```csharp
public void Configure(
    IApplicationBuilder app,
    IWebHostEnvironment env,
    ILoggerFactory loggerFactory)
{
    loggerFactory.AddProvider(new CustomLoggerProvider(new CustomLoggerConfiguration()));
```

會 `ILoggerProvider` 建立一或多個 `ILogger` 實例。 `ILogger`架構會使用實例來記錄資訊。

### <a name="sample-custom-logger-configuration"></a>範例自訂記錄器設定

此範例：

* 的設計是非常基本的範例，會依照事件識別碼和記錄層級設定記錄主控台的色彩。 記錄器通常不會因為事件識別碼而變更，而且不是記錄層級特有的。
* 使用下列設定類型，為每個記錄層級和事件識別碼建立不同的色彩主控台專案：

[!code-csharp[](index/samples/3.x/CustomLogger/ColorConsoleLogger/ColorConsoleLoggerConfiguration.cs?name=snippet)]

上述程式碼會將預設層級設為 `Warning` ，並將色彩設定為 `Yellow` 。 如果 `EventId` 設定為0，我們將會記錄所有事件。

### <a name="create-the-custom-logger"></a>建立自訂記錄器

`ILogger`執行分類名稱通常是記錄來源。 例如，建立記錄器的類型：

[!code-csharp[](index/samples/3.x/CustomLogger/ColorConsoleLogger/ColorConsoleLogger.cs?name=snippet)]

上述程式碼：

* 建立每個類別目錄名稱的記錄器實例。
* 簽 `logLevel == _config.LogLevel` 入 `IsEnabled` ，讓每個 `logLevel` 都有唯一的記錄器。 通常，記錄器也應該針對所有較高的記錄層級啟用：

```csharp
public bool IsEnabled(LogLevel logLevel)
{
    return logLevel >= _config.LogLevel;
}
```

### <a name="create-the-custom-loggerprovider"></a>建立自訂 LoggerProvider

`LoggerProvider`是建立記錄器實例的類別。 可能不需要為每個類別建立記錄器實例，但這對某些記錄器（例如 NLog 或 log4net）而言是合理的。 如此一來，您也可以視需要選擇每個類別的不同記錄輸出目標：

[!code-csharp[](index/samples/3.x/CustomLogger/ColorConsoleLogger/ColorConsoleLoggerProvider.cs?name=snippet)]

在上述程式碼中， <xref:Microsoft.Build.Logging.LoggerDescription.CreateLogger*> `ColorConsoleLogger` 會建立每個分類名稱的單一實例，並將其儲存在中 [`ConcurrentDictionary<TKey,TValue>`](/dotnet/api/system.collections.concurrent.concurrentdictionary-2) 。

### <a name="usage-and-registration-of-the-custom-logger"></a>自訂記錄器的使用方式和註冊

在中註冊記錄器 `Startup.Configure` ：

[!code-csharp[](index/samples/3.x/CustomLogger/Startup.cs?name=snippet)]

針對上述程式碼，為提供至少一個擴充方法 `ILoggerFactory` ：

[!code-csharp[](index/samples/3.x/CustomLogger/ColorConsoleLogger/ColorConsoleLoggerExtensions.cs?name=snippet)]

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/logging/loggermessage>
* 記錄錯誤應該會在[github.com/dotnet/runtime/](https://github.com/dotnet/runtime/issues)存放庫中建立。
* <xref:blazor/fundamentals/logging>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Steve Smith](https://ardalis.com/)

.NET Core 支援記錄 API，此 API 能與各種內建和第三方記錄提供者搭配使用。 此文章說明如何搭配內建提供者使用 API。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="add-providers"></a>新增提供者

記錄提供者會顯示或儲存記錄。 例如，主控台提供者會在主控台上顯示記錄，而 Azure Application Insights 提供者則會將記錄儲存在 Azure Application Insights 中。 您可以透過新增多個提供者的方式來將記錄傳送到多個目的地。

若要新增提供者，請在 *Program.cs* 中呼叫提供者的 `Add{provider name}` 擴充方法：

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=18-20)]

上述程式碼需要對 `Microsoft.Extensions.Logging` 與 `Microsoft.Extensions.Configuration` 的參考。

預設專案範本會呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>，該項目會新增下列記錄提供者：

* 主控台
* 偵錯
* EventSource (從 ASP.NET Core 2.2 開始)

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

若您使用 `CreateDefaultBuilder`，您可以使用您想要的提供者來取代預設提供者。 呼叫 <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> 並新增您要的提供者。

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

在此文章中深入了解[內建記錄提供者](#built-in-logging-providers)與[第三方記錄提供者](#third-party-logging-providers)。

## <a name="create-logs"></a>建立記錄

若要建立記錄，請使用 <xref:Microsoft.Extensions.Logging.ILogger%601> 物件。 在 Web 應用程式或託管服務中，從相依性插入 (DI) 取得 `ILogger`。 在非主機主控台應用程式中，使用 `LoggerFactory` 來建立 `ILogger`。

下列 ASP.NET Core 範例會建立以 `TodoApiSample.Pages.AboutModel` 作為類別的記錄器。 記錄「類別」** 是與每個記錄關聯的字串。 由 DI 提供的 `ILogger<T>` 執行個體，會建立使用型別 `T` 作為類別的完整名稱記錄。 

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3,5,7)]

在下列 ASP.NET Core 和主控台應用程式範例中，記錄器會用於以 `Information` 作為層級來建立記錄。 記錄「層級」** 指出已記錄事件的嚴重性。

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

此文章稍後將詳細說明[層級](#log-level)與[類別](#log-category)。

### <a name="create-logs-in-startup"></a>在啟動中建立記錄

若要在 `Startup` 類別中寫入記錄，請在建構函式簽章中包括 `ILogger` 參數：

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-the-program-class"></a>在 Program 類別中建立記錄

若要在 `Program` 類別中寫入記錄，請從 DI 取得 `ILogger` 執行個體：

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

不直接支援在主機結構期間進行記錄。 不過，您可以使用個別的記錄器。 在下列範例中，會使用 [Serilog](https://serilog.net/) 記錄器來記錄 `CreateWebHostBuilder`。  `AddSerilog`會使用中指定的靜態設定 `Log.Logger` ：

```csharp
using System;
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;

public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var builtConfig = new ConfigurationBuilder()
            .AddJsonFile("appsettings.json")
            .AddCommandLine(args)
            .Build();

        Log.Logger = new LoggerConfiguration()
            .WriteTo.Console()
            .WriteTo.File(builtConfig["Logging:FilePath"])
            .CreateLogger();

        try
        {
            return WebHost.CreateDefaultBuilder(args)
                .ConfigureServices((context, services) =>
                {
                    services.AddMvc();
                })
                .ConfigureAppConfiguration((hostingContext, config) =>
                {
                    config.AddConfiguration(builtConfig);
                })
                .ConfigureLogging(logging =>
                {
                    logging.AddSerilog();
                })
                .UseStartup<Startup>();
        }
        catch (Exception ex)
        {
            Log.Fatal(ex, "Host builder error");

            throw;
        }
        finally
        {
            Log.CloseAndFlush();
        }
    }
}
```

### <a name="no-asynchronous-logger-methods"></a>無非同步記錄器方法

記錄速度應該很快，不值得花費非同步程式碼的效能成本來處理。 若您的記錄資料存放區很慢，請不要直接寫入其中。 請考慮一開始將記錄寫入到快速的存放區，稍後再將它們移到慢速存放區。 例如，如果您要記錄到 SQL Server，您不希望在 `Log` 方法中直接執行，因為 `Log` 方法是同步的。 相反地，以同步方式將記錄訊息新增到記憶體內佇列，並讓背景工作角色提取出佇列的訊息，藉此執行推送資料到 SQL Server 的非同步工作。 如需詳細資訊，請參閱[此](https://github.com/dotnet/AspNetCore.Docs/issues/11801)GitHub 問題。

## <a name="configuration"></a>組態

記錄提供者設定是由一或多個記錄提供者提供：

* 檔案格式 (INI、JSON 及 XML)。
* 命令列引數。
* 環境變數。
* 記憶體內部 .NET 物件。
* 未加密的[祕密管理員](xref:security/app-secrets)儲存體。
* 類似 [Azure Key Vault](xref:security/key-vault-configuration)的加密使用者存放區。
* 自訂提供者 (已安裝或已建立)。

例如，記錄設定通常是由應用程式的 `Logging` 區段所提供的。 下列範例顯示一般 *appsettings.Development.json* 檔案的內容：

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    },
    "Console":
    {
      "IncludeScopes": true
    }
  }
}
```

`Logging` 屬性可以有 `LogLevel` 與記錄提供者屬性 (會顯示主控台)。

`Logging` 下的 `LogLevel` 屬性會指定要針對所選類記錄的最小[層級](#log-level)。 在範例中，`System` 與d `Microsoft` 類別會在 `Information` 層級記錄，而所有其他記錄則會在 `Debug` 層級記錄。

`Logging` 下的其他屬性可指定記錄提供者。 範例使用主控台提供者。 如果提供者支援[記錄範圍](#log-scopes)，則 `IncludeScopes` 指出是否已啟用。 提供者屬性 (例如範例中的 `Console`) 可能也會指定 `LogLevel` 屬性。 提供者下的 `LogLevel` 會指定提供者的記錄層級。

若已在 `Logging.{providername}.LogLevel` 中指定層級，它們會覆寫 `Logging.LogLevel` 中設定的所有項目。 例如，請考慮下列 JSON：

[!code-json[](index/samples/3.x/TodoApiDTO/appsettings.MSFT.json)]

在先前的 JSON 中， `Console` 提供者設定會覆寫先前的（預設）記錄層級。

記錄 API 不包含在應用程式執行時變更記錄層級的案例。 不過，某些設定提供者能夠重載設定，這會立即影響記錄設定。 例如，檔案設定[提供者](xref:fundamentals/configuration/index#file-configuration-provider)（由新增） `CreateDefaultBuilder` 以讀取設定檔案，預設會重載記錄設定。 如果應用程式正在執行時，程式碼中的設定有所變更，應用程式可以呼叫[IConfigurationRoot](xref:Microsoft.Extensions.Configuration.IConfigurationRoot.Reload*)來更新應用程式的記錄設定。

如需有關如何實作設定提供者的詳細資訊，請參閱 <xref:fundamentals/configuration/index>。

## <a name="sample-logging-output"></a>範例記錄輸出

使用上一節中顯示的範例程式碼時，當從命令列執行應用程式時，記錄會出現在主控台中。 主控台輸出範例如下：

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 42.9286ms
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 148.889ms 404
```

上述記錄是透過向位於 `http://localhost:5000/api/todo/0` 的範例應用程式建立 HTTP Get 要求所產生。

以下是您在 Visual Studio 中執行相同應用程式時，出現在 [偵錯] 視窗中的相同記錄範例：

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

這些透過上一節中所示 `ILogger` 呼叫所建立的記錄是以 "TodoApi" 為開頭。 開頭為 "Microsoft" 類別的記錄則是來自 ASP.NET Core 架構程式碼。 ASP.NET Core 與應用程式程式碼會使用相同的記錄 API 與提供者。

本文的其餘部分將說明記錄的一些詳細資料和選項。

## <a name="nuget-packages"></a>NuGet 套件

`ILogger` 和 `ILoggerFactory` 介面位於 [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/) 中，其預設實作則位於 [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) 中。

## <a name="log-category"></a>記錄分類

建立 `ILogger` 物件時，會為它指定「類別」**。 該類別會包含在每個由該 `ILogger` 執行個體所產生的記錄訊息中。 類別可以是任意字串，但慣例是使用類別名稱，例如 "TodoApi.Controllers.TodoController"。

使用 `ILogger<T>` 來取得`ILogger` 執行個體，它使用 `T` 的完整類型名稱做為類別：

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

若要明確指定類別，請呼叫 `ILoggerFactory.CreateLogger`：

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

`ILogger<T>` 相當於使用 `T`的完整類型名稱來呼叫 `CreateLogger`。

## <a name="log-level"></a>記錄層級

每個記錄都會指定 <xref:Microsoft.Extensions.Logging.LogLevel> 值。 記錄層級表示嚴重性或重要性。 例如，您可以在方法不正常終止時寫入 `Information` 記錄，並在方法傳回 *404 找不到* 狀態碼時寫入 `Warning` 記錄。

下列程式碼會建立 `Information` 與 `Warning` 記錄：

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

在上述程式碼中， `MyLogEvents.GetItem` 和 `MyLogEvents.GetItemNotFound` 參數是[記錄事件識別碼](#log-event-id)。 第二個參數是訊息範本，其中的預留位置會置入其餘方法參數所提供的引數值。 在本文的[記錄訊息範本一節](#lmt)中會說明方法參數。

在方法名稱中包含層級的記錄方法 (例如 `LogInformation` 與 `LogWarning`) 是 [ILogger 的擴充方法](xref:Microsoft.Extensions.Logging.LoggerExtensions)。 這些方法會呼叫接受 `Log` 參數的 `LogLevel`。 您可以直接呼叫 `Log` 方法，而不是呼叫其中一個擴充方法，但語法會更複雜。 如需詳細資訊，請參閱 <xref:Microsoft.Extensions.Logging.ILogger> 與[記錄器延伸模組原始程式碼](https://github.com/dotnet/extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs)。

ASP.NET Core 定義下列記錄層級，並從最低嚴重性排列到最高嚴重性。

* 追蹤 = 0

  針對通常只對偵錯有價值的資訊。 這些訊息可能包含敏感性應用程式資料，因此不應該在生產環境中啟用。 *預設為停用。*

* 偵錯 = 1

  針對可在開發與偵錯中使用的資訊。 範例：`Entering method Configure with flag set to true.` 由於記錄的數目很龐大，因此除非您正在進行疑難排解，否則通常不會在生產環境中啟用 `Debug` 層級記錄。

* 資訊 = 2

  針對一般應用程式流程的追蹤。 這些記錄通常有一些長期值。 範例： `Request received for path /api/todo`

* 警告 = 3

  針對應用程式流程中發生的異常或意外事件。 這些記錄可能包含不會造成應用程式停止，但可能需要進行調查的錯誤或其他狀況。 已處理的例外狀況即為使用 `Warning` 記錄層級的常見位置。 範例： `FileNotFoundException for file quotes.txt.`

* 錯誤 = 4

  發生無法處理的錯誤和例外狀況。 這些訊息指出目前活動或作業 (例如目前的 HTTP 要求) 中發生失敗，這不是整個應用程式的失敗。 範例記錄訊息：`Cannot insert record due to duplicate key violation.`

* 重大 = 5

  發生需要立即注意的失敗。 範例：資料遺失情況、磁碟空間不足。

使用此記錄層級來控制要寫入至特定儲存媒體或顯示視窗的記錄輸出量。 例如：

* 在生產環境中：
  * `Trace`透過層級的記錄會 `Information` 產生大量的詳細記錄訊息。 若要控制成本，而不超過資料儲存體限制，請將 `Trace` `Information` 層級訊息記錄到高容量、低成本的資料存放區。
  * `Warning`透過 `Critical` 層級登入通常會產生較少、較小的記錄檔訊息。 因此，成本和儲存體限制通常不會造成問題，因此可讓您更靈活地選擇資料存放區。
* 在開發期間：
  * `Warning` `Critical` 將訊息記錄到主控台。
  * `Trace` `Information` 進行疑難排解時，新增訊息。

本文稍後的[記錄篩選](#log-filtering)一節將說明如何控制提供者所處理的記錄層級。

ASP.NET Core 會寫入架構事件的記錄。 此文章稍早的記錄範例已排除 `Information` 層級以下的記錄，因此未建立 `Debug` 或 `Trace` 層級的任何記錄。 以下是透過執行設定為顯示 `Debug` 記錄之範例應用程式所產生的主控台記錄範例：

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:62555/api/todo/0
dbug: Microsoft.AspNetCore.Routing.Tree.TreeRouter[1]
      Request successfully matched the route with name 'GetTodo' and template 'api/Todo/{id}'.
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Update (TodoApi)' with id '089d59b6-92ec-472d-b552-cc613dfd625d' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Delete (TodoApi)' with id 'f3476abe-4bd9-4ad3-9261-3ead09607366' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action TodoApi.Controllers.TodoController.GetById (TodoApi)
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action method TodoApi.Controllers.TodoController.GetById (TodoApi), returned result Microsoft.AspNetCore.Mvc.NotFoundResult.
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 0.8788ms
dbug: Microsoft.AspNetCore.Server.Kestrel[9]
      Connection id "0HL6L7NEFF2QD" completed keep alive response.
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 2.7286ms 404
```

## <a name="log-event-id"></a>記錄事件識別碼

每個記錄都可以指定「事件識別碼」**。 範例應用程式透過使用本機定義的 `LoggingEvents` 類別來執行此動作：

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

事件識別碼會將一組事件關聯。 例如，與在頁面上顯示項目清單相關的所有記錄可能都是 1001。

記錄提供者可能將事件識別碼存放到識別碼欄位、記錄訊息中或完全不存放。 偵錯提供者不會顯示事件識別碼。 主控台提供者會在類別後面以括弧顯示事件識別碼：

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a>記錄訊息範本

每個記錄都會指定訊息範本。 訊息範本可以包含有關提供哪個引數的預留位置。 使用名稱而非數字做為預留位置。

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

預留位置的順序 (而不是其名稱) 會決定使用哪些參數來提供其值。 請注意，在下列程式碼中，參數名稱在訊息範本中未依順序出現：

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

此程式碼會使用依順序的參數值建立記錄訊息：

```text
Parameter values: parm1, parm2
```

記錄架構以這種方式運作，因此記錄提供者可以實作[語意記錄 (亦稱為結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。 引數本身會被傳遞到記錄系統，而不只是格式化的訊息範本。 此資訊可讓記錄提供者將參數值儲存為欄位。 例如，假設記錄器方法呼叫看起來像這樣：

```csharp
_logger.LogInformation("Getting item {Id} at {RequestTime}", id, DateTime.Now);
```

若您正在將記錄傳送到 Azure 表格儲存體，每個 Azure 資料表實體都可以有 `ID` 與 `RequestTime` 屬性，以簡化記錄資料的查詢。 查詢可以尋找特定 `RequestTime` 範圍內的所有記錄，而不需要從文字訊息剖析時間。

## <a name="logging-exceptions"></a>記錄例外狀況

記錄器方法具有多載，可讓您傳入例外狀況，如下列範例所示：

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

不同提供者處理例外狀況資訊的方式會不同。 以下是來自上述程式碼中的偵錯提供者輸出範例。

```text
TodoApiSample.Controllers.TodoController: Warning: GetById(55) NOT FOUND

System.Exception: Item not found exception.
   at TodoApiSample.Controllers.TodoController.GetById(String id) in C:\TodoApiSample\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>記錄篩選

您可以指定特定提供者和類別的最低記錄層級，也可以指定所有提供者或所有類別的最低記錄層級。 最低層級以下的任何記錄都不會傳遞給該提供者，因此不會顯示或儲存。

若要隱藏所有記錄，請指定 `LogLevel.None` 作為最低記錄層級。 `LogLevel.None` 的整數值為 6，高於 `LogLevel.Critical` (5)。

### <a name="create-filter-rules-in-configuration"></a>在組態中建立篩選規則

專案範本程式碼會呼叫 `CreateDefaultBuilder` 以設定主控台、Debug 和 EventSource （ASP.NET Core 2.2 或更新版本）提供者的記錄。 `CreateDefaultBuilder` 方法會設定記錄以尋找 `Logging` 區段中的組態，如[本文先前所述](#configuration)。

組態資料會依提供者和類別指定最低記錄層級，如下列範例所示：

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

此 JSON 會建立六個篩選規則：一個適用於偵錯提供者、四個適用於主控台提供者，一個適用於所有提供者。 建立 `ILogger` 物件時，會為每個提供者選取一個規則。

### <a name="filter-rules-in-code"></a>程式碼中的篩選規則

下列範例說明如何在程式碼中註冊篩選規則：

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

第二個 `AddFilter` 會使用其類型名稱來指定偵錯提供者。 第一個 `AddFilter` 由於未指定提供者類型，因此適用於所有提供者。

### <a name="how-filtering-rules-are-applied"></a>如何套用篩選規則

組態資料和上述範例中所示的 `AddFilter` 程式碼會建立下表中所示的規則。 前六項來自組態範例，最後兩項來自程式碼範例。

| Number | 提供者      | 開頭如下的類別...          | 最低記錄層級 |
| :----: | ------------- | --------------------------------------- | ----------------- |
| 1      | 偵錯         | 所有類別                          | 資訊       |
| 2      | 主控台       | AspNetCore Razor 。內部 | 警告           |
| 3      | 主控台       | AspNetCore Razor 。Razor    | 偵錯             |
| 4      | 主控台       | AspNetCore。Razor          | 錯誤             |
| 5      | 主控台       | 所有類別                          | 資訊       |
| 6      | 所有提供者 | 所有類別                          | 偵錯             |
| 7      | 所有提供者 | 系統                                  | 偵錯             |
| 8      | 偵錯         | Microsoft                               | 追蹤             |

建立 `ILogger` 物件時，`ILoggerFactory` 物件會針對每個提供者選取一個規則來套用到該記錄器。 由 `ILogger` 執行個體寫入的所有訊息都會根據選取的規則進行篩選。 系統會從可用的規則中，盡可能選取對每個提供者和類別配對最明確的規則。

當建立指定類別的 `ILogger` 時，系統會針對每個提供者使用下列演算法：

* 選取所有符合提供者或其別名的規則。 如果找不到符合的項目，請選取所有規則搭配空白提供者。
* 從上一個步驟的結果中，選取具有最長相符類別前置字元的規則。 如果找不到符合的項目，請選取未指定類別的所有規則。
* 如果選取多個規則，請使用**最後**一個。
* 如果未選取任何規則，請使用 `MinimumLevel`。

在上述的規則清單中，假設您建立了 `ILogger` 類別 "AspNetCore Razor Razor " 的物件。ViewEngine":

* 針對偵錯提供者，規則 1、6 和 8 均適用。 規則 8 最明確，因此會選取此規則。
* 針對主控台提供者，規則 3、4、5 和 6 均適用。 規則 3 最明確。

產生的 `ILogger` 執行個體會傳送 `Trace` 層級以上的記錄到偵錯提供者。 `Debug` 層級以上的記錄會傳送到主控台提供者。

### <a name="provider-aliases"></a>提供者別名

每個提供者都會定義「別名」**，可在設定中用來取代完整類型名稱。  針對內建提供者，請使用下列別名：

* 主控台
* 偵錯
* EventSource
* EventLog
* TraceSource
* AzureAppServicesFile
* AzureAppServicesBlob
* ApplicationInsights

### <a name="default-minimum-level"></a>預設最低層級

只有組態或程式碼中沒有適用於指定提供者和類別的規則時，最低層級設定才會生效。 下列範例示範如何設定最低層級：

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

如果您未明確設定最低層級，預設值為 `Information`，這表示會略過 `Trace` 和 `Debug` 記錄。

### <a name="filter-functions"></a>篩選函數

針對組態或程式碼未指派規則的所有提供者和類別，會叫用篩選函式。 函式中的程式碼可以存取提供者類型、類別與記錄層級。 例如：

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

## <a name="system-categories-and-levels"></a>系統類別與層級

以下是由 ASP.NET Core 與 Entity Framework Core 所使用的一些類別，以及有關它們可傳回哪些記錄的附註：

| 類別                            | 注意 |
| ----------------------------------- | ----- |
| Microsoft.AspNetCore                | 一般 ASP.NET Core 診斷。 |
| Microsoft.AspNetCore.DataProtection | 已考慮、發現及使用哪些金鑰。 |
| Microsoft.AspNetCore.HostFiltering  | 允許主機。 |
| Microsoft.AspNetCore.Hosting        | HTTP 要求花了多少時間完成，以及其開始時間。 載入了哪些裝載啟動組件。 |
| Microsoft.AspNetCore.Mvc            | MVC 和 Razor 診斷。 模型繫結、篩選執行、檢視編譯、動作選取。 |
| Microsoft.AspNetCore.Routing        | 路由比對資訊。 |
| Microsoft.AspNetCore.Server         | 連線開始、停止與保持運作回應。 HTTPS 憑證資訊。 |
| Microsoft.AspNetCore.StaticFiles    | 提供的檔案。 |
| Microsoft.EntityFrameworkCore       | 一般 Entity Framework Core 診斷。 資料庫活動與設定、變更偵測、移轉。 |

## <a name="log-scopes"></a>記錄範圍

 「範圍」** 可用來將邏輯作業組成群組。 此分組功能可用來將相同的資料附加到已建立為集合之一部分的每個記錄。 例如，在處理邀交易時建立的每個記錄都可以包括該交易識別碼。

範圍是 <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> 方法所傳回的 `IDisposable` 類型，並會持續到被處置為止。 透過將記錄器呼叫封裝在 `using` 區塊中以使用範圍：

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

下列程式碼會啟用主控台提供者的範圍：

*Program.cs*：

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> 您必須設定 `IncludeScopes` 主控台記錄器選項才能啟用範圍記錄。
>
> 如需有關設定的詳細資訊，請參閱[設定](#configuration)一節。

每個記錄訊息包含範圍資訊：

```
info: TodoApiSample.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a>內建記錄提供者

ASP.NET Core 隨附下列提供者：

* [主控台](#console-provider)
* [偵錯](#debug-provider)
* [EventSource](#event-source-provider)
* [EventLog](#windows-eventlog-provider)
* [TraceSource](#tracesource-provider)
* [AzureAppServicesFile](#azure-app-service-provider)
* [AzureAppServicesBlob](#azure-app-service-provider)
* [ApplicationInsights](#azure-application-insights-trace-logging)

如需 ASP.NET Core 模組的 StdOut 和偵錯記錄相關資訊，請參閱 <xref:test/troubleshoot-azure-iis> 和 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。

### <a name="console-provider"></a>Console 提供者

[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) 提供者套件會將記錄輸出傳送至主控台。 

```csharp
logging.AddConsole();
```

若要查看主控台記錄輸出，請在專案資料夾中開啟命令提示字元，然後執行下列命令：

```dotnetcli
dotnet run
```

### <a name="debug-provider"></a>Debug 提供者

[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) 提供者套件使用 [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) 類別 (`Debug.WriteLine` 方法呼叫) 來寫入記錄輸出。

在 Linux 上，此提供者會將記錄寫入至 */var/log/message*。

```csharp
logging.AddDebug();
```

### <a name="event-source-provider"></a>事件來源提供者

在跨平臺的事件來源中，會寫入名為的[記錄](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) `Microsoft-Extensions-Logging` 檔。 在 Windows 上，提供者會使用[ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803)。

```csharp
logging.AddEventSourceLogger();
```

呼叫以建立主機時，會自動新增事件來源提供者 `CreateDefaultBuilder` 。

使用[PerfView 公用程式](https://github.com/Microsoft/perfview)來收集及查看記錄。 此外還有一些其他工具可檢視 ETW 記錄，但 PerfView 提供處理 ASP.NET Core 所發出 ETW 事件的最佳體驗。

若要設定 PerfView 以收集此提供者所記錄的事件，請將字串 `*Microsoft-Extensions-Logging` 新增至 [其他提供者]**** 清單 (請勿遺漏字串開頭的星號)。

![PerfView 的其他提供者](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a>Windows EventLog 提供者

[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) 提供者套件會將記錄輸出傳送至 Windows 事件記錄檔。

```csharp
logging.AddEventLog();
```

[AddEventLog 多載](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions)可讓您傳入 <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>。 若 `null` 未指定，則會使用下列預設設定：

* `LogName`：「應用程式」
* `SourceName`： ".NET Runtime"
* `MachineName`：使用本機電腦名稱稱。

記錄[警告層級和更新版本](#log-level)的事件。 下列範例會將事件記錄檔的預設記錄層級設定為 <xref:Microsoft.Extensions.Logging.LogLevel.Information?displayProperty=nameWithType> ：

```json
"Logging": {
  "EventLog": {
    "LogLevel": {
      "Default": "Information"
    }
  }
}
```

### <a name="tracesource-provider"></a>TraceSource 提供者

[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) 提供者套件使用 <xref:System.Diagnostics.TraceSource> 程式庫與提供者。

```csharp
logging.AddTraceSource(sourceSwitchName);
```

[AddTraceSource 多載](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions)可讓您傳入來源參數和追蹤接聽項。

若要使用此提供者，應用程式必須在 .NET Framework (而非 .NET Core) 上執行。 該提供者可讓您將訊息路由傳送到種不同的[接聽程式](/dotnet/framework/debug-trace-profile/trace-listeners)，例如範例應用程式中所使用的 <xref:System.Diagnostics.TextWriterTraceListener>。

### <a name="azure-app-service-provider"></a>Azure App Service 提供者

[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) 提供者套件會將記錄寫入至 Azure App Service 應用程式檔案系統中的文字檔，並寫入至 Azure 儲存體帳戶中的 [Blob 儲存體](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage)。

```csharp
logging.AddAzureWebAppDiagnostics();
```

[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)未包含提供者套件。 當以 .NET Framework 為目標或是參考 `Microsoft.AspNetCore.App` 中繼套件時，請將提供者套件新增至專案。 

<xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> 多載可讓您傳入 <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>。 設定物件可覆寫預設設定，例如記錄輸出範本、Blob 名稱與檔案大小限制。 (*輸出範本*是訊息範本，它除了會套用到 `ILogger` 方法呼叫所提供的記錄之外，還會套用到所有記錄。)

當您部署到 App Service 應用程式時，應用程式會遵循 Azure 入口網站 [App Service]**** 頁面中 [App Service 記錄](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag)區段的設定。 當下列設定更新時，變更會立即生效，而不需要重新啟動或重新部署應用程式。

* **應用程式記錄 (檔案系統)**
* **應用程式記錄 (Blob)**

記錄檔的預設位置為 *D:\\home\\LogFiles\\Application* 資料夾，而預設檔案名稱為 *diagnostics-yyyymmdd.txt*。 預設檔案大小限制為 10 MB，而預設保留的檔案數目上限為 2。 預設 Blob 名稱為 *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*。

此提供者僅適用於專案在 Azure 環境中執行的情況。 若在本機執行專案，不會有任何作用，即它不會寫入本機檔案或 blob 的本機開發儲存體。

#### <a name="azure-log-streaming"></a>Azure 記錄資料流

Azure 記錄串流可讓您即時檢視來自下列位置的記錄活動：

* 應用程式伺服器
* 網頁伺服器
* 失敗的要求追蹤

若要設定 Azure 記錄資料流：

* 從您應用程式的入口網站頁面瀏覽到 [App Service 記錄]****。
* 將 [應用程式記錄 (檔案系統)]**** 設定為 [開啟]****。
* 選擇記錄 [層級]****。 此設定僅適用于 Azure 記錄串流，而不適用於應用程式中的其他記錄提供者。

瀏覽到 [記錄資料流]**** 頁面以檢視應用程式訊息。 這些是應用程式透過 `ILogger` 介面產生的訊息。

### <a name="azure-application-insights-trace-logging"></a>Azure Application Insights 追蹤記錄

[Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) \(英文\) 提供者套件會將記錄寫入至 Azure Application Insights。 Application Insights 是可監視 Web 應用程式的服務，並提供可用來查詢及分析遙測資料的工具。 如果您使用此提供者，就可以使用 Application Insights 工具來查詢及分析記錄。

記錄提供者會以 [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) \(英文\) 的相依性形式隨附，這是針對 ASP.NET Core 提供所有可用遙測的套件。 如果您使用此套件，就不需安裝提供者套件。

不要使用 [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) \(英文\) 套件&mdash;該套件適用於 ASP.NET 4.x。

如需詳細資訊，請參閱下列資源：

* [Application Insights 概觀](/azure/application-insights/app-insights-overview)
* [適用於 ASP.NET Core 應用程式的 Application Insights](/azure/azure-monitor/app/asp-net-core)：如果您想要實作完整範圍的 Application Insights 遙測以及記錄，請從這裡開始。
* [適用於 .NET Core ILogger 記錄的 ApplicationInsightsLoggerProvider](/azure/azure-monitor/app/ilogger)：如果您想要實作記錄提供者，而不需要 Application Insights 遙測的其餘部分，請從這裡開始。
* [Application Insights logging adapters](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs) (Application Insights 記錄配接器)。
* [安裝、設定及初始化 Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights)：Microsoft Learn 網站上的互動式教學課程。

## <a name="third-party-logging-providers"></a>協力廠商記錄提供者

可搭配 ASP.NET Core 使用的協力廠商記錄架構：

* [elmah.io](https://elmah.io/) ([GitHub 存放庫](https://github.com/elmahio/Elmah.Io.Extensions.Logging))
* [Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub 存放庫](https://github.com/mattwcole/gelf-extensions-logging))
* [JSNLog](https://jsnlog.com/) ([GitHub 存放庫](https://github.com/mperdeck/jsnlog))
* [KissLog.net](https://kisslog.net/) ([GitHub 存放庫](https://github.com/catalingavan/KissLog-net))
* [Log4Net](https://logging.apache.org/log4net/) （[GitHub](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore)存放庫）
* [Loggr](https://loggr.net/) ([GitHub 存放庫](https://github.com/imobile3/Loggr.Extensions.Logging))
* [NLog](https://nlog-project.org/) ([GitHub 存放庫](https://github.com/NLog/NLog.Extensions.Logging))
* [Sentry](https://sentry.io/welcome/) ([GitHub 存放庫](https://github.com/getsentry/sentry-dotnet))
* [Serilog](https://serilog.net/) ([GitHub 存放庫](https://github.com/serilog/serilog-aspnetcore))
* [Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github 存放庫](https://github.com/googleapis/google-cloud-dotnet))

某些協力廠商架構可以執行[語意記錄 (也稱為結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) \(英文\)。

使用協力廠商架構類似於使用內建的提供者之一：

1. 將 NuGet 套件新增至專案。
1. 呼叫 `ILoggerFactory` 記錄架構所提供的擴充方法。

如需詳細資訊，請參閱每個提供者的文件。 Microsoft 不支援第三方記錄提供者。

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/logging/loggermessage>

::: moniker-end
