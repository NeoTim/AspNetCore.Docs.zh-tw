---
title: 將 ASP.NET Core Razor 元件整合至 Razor 頁面和 MVC 應用程式
author: guardrex
description: 瞭解應用程式中元件和 DOM 元素的資料系結案例 Blazor 。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/25/2020
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
uid: blazor/components/integrate-components-into-razor-pages-and-mvc-apps
ms.openlocfilehash: e2045d7d169e81c85f4c7dbd97357455ecd70ea3
ms.sourcegitcommit: 65add17f74a29a647d812b04517e46cbc78258f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/19/2020
ms.locfileid: "88628478"
---
# <a name="integrate-aspnet-core-no-locrazor-components-into-no-locrazor-pages-and-mvc-apps"></a>將 ASP.NET Core Razor 元件整合至 Razor 頁面和 MVC 應用程式

依 [Luke Latham](https://github.com/guardrex) 和 [Daniel Roth](https://github.com/danroth27)

Razor 元件可以整合至 Razor 頁面和 MVC 應用程式。 轉譯頁面或視圖時，可同時資源清單元件。

[準備好應用程式](#prepare-the-app)之後，請根據應用程式的需求，使用下列各節中的指導方針：

* 可路由的元件：適用于直接從使用者要求路由傳送的元件。 當訪客應能在其瀏覽器中使用指示詞為元件發出 HTTP 要求時，請遵循此指導 [`@page`](xref:mvc/views/razor#page) 方針。
  * [在頁面應用程式中使用可路由的元件 Razor](#use-routable-components-in-a-razor-pages-app)
  * [在 MVC 應用程式中使用可路由的元件](#use-routable-components-in-an-mvc-app)
* [從頁面或視圖呈現元件](#render-components-from-a-page-or-view)：適用于無法直接從使用者要求路由傳送的元件。 當應用程式使用元件標籤協助程式將元件內嵌至現有的頁面和流覽 [器](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper)時，請遵循此指引。

## <a name="prepare-the-app"></a>準備應用程式

現有的 Razor 頁面或 MVC 應用程式可以將 Razor 元件整合至頁面和流覽中：

1. 在應用程式的版面配置檔案中 (`_Layout.cshtml`) ：

   * 將下列 `<base>` 標記新增至 `<head>` 元素：

     ```html
     <base href="~/" />
     ```

     `href`上述範例中的*應用程式基底路徑*)  (值假設應用程式位於根 URL 路徑 (`/`) 。 如果應用程式是子應用程式，請遵循本文的「 *應用程式基底路徑* 」一節中的指導方針 <xref:blazor/host-and-deploy/index#app-base-path> 。

     檔案 `_Layout.cshtml` 位於 MVC 應用程式中的 pages */shared* 資料夾 Razor 或 MVC 應用程式中的 *Views/shared* 資料夾內。

   * 將 `<script>` *blazor.server.js* 腳本的標記新增至結尾 `</body>` 標記之前：

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     架構會將 *blazor.server.js* 腳本新增至應用程式。 不需要手動將腳本新增至應用程式。

1. `_Imports.razor`使用下列內容將檔案新增至專案的根資料夾， (將最後一個命名空間變更 `MyAppNamespace` 為應用程式) 的命名空間：

   ```razor
   @using System.Net.Http
   @using Microsoft.AspNetCore.Authorization
   @using Microsoft.AspNetCore.Components.Authorization
   @using Microsoft.AspNetCore.Components.Forms
   @using Microsoft.AspNetCore.Components.Routing
   @using Microsoft.AspNetCore.Components.Web
   @using Microsoft.JSInterop
   @using MyAppNamespace
   ```

1. 在中 `Startup.ConfigureServices` ，註冊 Blazor Server 服務：

   ```csharp
   services.AddServerSideBlazor();
   ```

1. 在中 `Startup.Configure` ，將 Blazor 中樞端點新增至 `app.UseEndpoints` ：

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. 將元件整合至任何頁面或視圖中。 如需詳細資訊，請參閱 [從頁面或視圖區段呈現元件](#render-components-from-a-page-or-view) 。

## <a name="use-routable-components-in-a-no-locrazor-pages-app"></a>在頁面應用程式中使用可路由的元件 Razor

*本節適用于新增直接可從使用者要求路由傳送的元件。*

支援 Razor 頁面應用程式中可路由的元件 Razor ：

1. 遵循「 [準備應用程式](#prepare-the-app) 」一節中的指導方針。

1. `App.razor`使用下列內容將檔案新增至專案根目錄：

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="@typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. 將檔案新增 `_Host.cshtml` 至 `Pages` 具有下列內容的資料夾：

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   元件會使用共用檔案 `_Layout.cshtml` 進行版面配置。

   <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> 設定元件是否為 `App` ：

   * 會資源清單到頁面中。
   * 會在頁面上轉譯為靜態 HTML，或包含從使用者代理程式啟動應用程式所需的資訊 Blazor 。

   | 轉譯模式 | 描述 |
   | ----------- | ----------- |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | 將 `App` 元件轉譯為靜態 HTML，並包含 Blazor Server 應用程式的標記。 當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。 |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | 轉譯應用程式的標記 Blazor Server 。 `App`不包含元件的輸出。 當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。 |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | 將 `App` 元件轉譯為靜態 HTML。 |

   如需元件標記協助程式的詳細資訊，請參閱 <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper> 。

1. 將頁面的低優先順序路由新增 `_Host.cshtml` 至中的端點設定 `Startup.Configure` ：

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. 將可路由傳送的元件新增至應用程式。 例如：

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

如需命名空間的詳細資訊，請參閱 [元件命名空間](#component-namespaces) 一節。

## <a name="use-routable-components-in-an-mvc-app"></a>在 MVC 應用程式中使用可路由的元件

*本節適用于新增直接可從使用者要求路由傳送的元件。*

支援 Razor MVC 應用程式中的可路由元件：

1. 遵循「 [準備應用程式](#prepare-the-app) 」一節中的指導方針。

1. `App.razor`使用下列內容將檔案新增至專案的根目錄：

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="@typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. 將檔案新增 `_Host.cshtml` 至 `Views/Home` 具有下列內容的資料夾：

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   元件會使用共用檔案 `_Layout.cshtml` 進行版面配置。
   
   <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> 設定元件是否為 `App` ：

   * 會資源清單到頁面中。
   * 會在頁面上轉譯為靜態 HTML，或包含從使用者代理程式啟動應用程式所需的資訊 Blazor 。

   | 轉譯模式 | 描述 |
   | ----------- | ----------- |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | 將 `App` 元件轉譯為靜態 HTML，並包含 Blazor Server 應用程式的標記。 當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。 |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | 轉譯應用程式的標記 Blazor Server 。 `App`不包含元件的輸出。 當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。 |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | 將 `App` 元件轉譯為靜態 HTML。 |

   如需元件標記協助程式的詳細資訊，請參閱 <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper> 。

1. 將動作新增至 Home 控制器：

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. 新增控制器動作的低優先順序路由，將 `_Host.cshtml` 視圖傳回至中的端點設定 `Startup.Configure` ：

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. 建立 `Pages` 資料夾，並將可路由傳送的元件新增至應用程式。 例如：

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

如需命名空間的詳細資訊，請參閱 [元件命名空間](#component-namespaces) 一節。

## <a name="render-components-from-a-page-or-view"></a>從頁面或視圖呈現元件

*本節適用于將元件新增至頁面或視圖，其中元件無法直接從使用者要求路由傳送。*

若要從頁面或視圖轉譯元件，請使用 [元件標記](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper)協助程式。

### <a name="render-stateful-interactive-components"></a>轉譯具狀態的互動式元件

可設定狀態的互動式元件可以加入至 Razor 頁面或視圖中。

當頁面或視圖呈現時：

* 此元件是使用頁面或視圖所資源清單。
* 用於進行可呈現的初始元件狀態會遺失。
* 建立連接時，會建立新的元件狀態 SignalR 。

下列 Razor 頁面會呈現 `Counter` 元件：

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@functions {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

如需詳細資訊，請參閱<xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>。

### <a name="render-noninteractive-components"></a>轉譯非互動式元件

在下 Razor 一個頁面中， `Counter` 系統會使用表單所指定的初始值，以靜態方式呈現元件。 由於元件是以靜態方式轉譯，因此元件並非互動式：

```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>

<component type="typeof(Counter)" render-mode="Static" 
    param-InitialValue="InitialValue" />

@functions {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

如需詳細資訊，請參閱<xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>。

## <a name="component-namespaces"></a>元件命名空間

使用自訂資料夾來保存應用程式的元件時，請將代表資料夾的命名空間新增至頁面/視圖或檔案 `_ViewImports.cshtml` 。 在下例中︰

* 變更 `MyAppNamespace` 為應用程式的命名空間。
* 如果未使用名為 *元件* 的資料夾來保存元件，請變更 `Components` 為元件所在的資料夾。

```cshtml
@using MyAppNamespace.Components
```

檔案 `_ViewImports.cshtml` 位於 `Pages` Razor 頁面應用程式的資料夾或 `Views` MVC 應用程式的資料夾中。

如需詳細資訊，請參閱<xref:blazor/components/index#namespaces>。
