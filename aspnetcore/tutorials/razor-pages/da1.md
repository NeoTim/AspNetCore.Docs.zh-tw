---
title: 第5部分：在 ASP.NET Core 應用程式中更新產生的頁面
author: rick-anderson
description: 頁面上教學課程系列的第5部分 Razor 。
ms.author: riande
ms.date: 12/20/2018
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
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 04479e5c3a0b1e9badbb4e58043cf059beefb4ab
ms.sourcegitcommit: 65add17f74a29a647d812b04517e46cbc78258f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/19/2020
ms.locfileid: "88632794"
---
# <a name="part-5-update-the-generated-pages-in-an-aspnet-core-app"></a>第5部分：在 ASP.NET Core 應用程式中更新產生的頁面

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

Scaffolded 電影應用程式是一個不錯的起點，但其呈現效果卻不理想。 **ReleaseDate** 應該是 **Release Date** (兩個字)。

![在 Chrome 中開啟的電影應用程式](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>更新產生的程式碼

開啟 *Models/Movie.cs* 檔案，然後新增下列程式碼中顯示的醒目提示行：

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateFixed.cs?name=snippet_1&highlight=3,12,17)]

`[Column(TypeName = "decimal(18, 2)")]` 資料註解可讓 Entity Framework Core 將 `Price` 正確對應到資料庫中的貨幣。 如需詳細資訊，請參閱[資料類型](/ef/core/modeling/relational/data-types)。

接下來的教學課程會涵蓋 [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6)。 [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) 屬性指定要顯示的欄位名稱 (在本例中為 "Release Date"，而不是 "ReleaseDate")。 [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) 屬性指定資料的類型 (Date)，因此不會顯示儲存在欄位中的時間資訊。

瀏覽至 Pages/Movies，然後將滑鼠停留在 **Edit** 連結，以查看目標 URL。

![滑鼠停留在 Edit 連結並顯示 http://localhost:1234/Movies/Edit/5 的 Url 的瀏覽器視窗](~/tutorials/razor-pages/da1/edit7.png)

在 *Pages/Movies/Index.cshtml* 檔案中，**Edit**、**Details ** 和 **Delete** 連結是由[錨點標記協助程式](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)所產生。

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

[標記](xref:mvc/views/tag-helpers/intro) 協助程式可讓伺服器端程式碼參與建立和轉譯檔案中的 HTML 元素 Razor 。 在上述程式碼中， `AnchorTagHelper` 會以動態方式 `href` 從頁面產生 HTML 屬性值 Razor (路由是相對) 、 `asp-page` 和路由識別碼 (`asp-route-id`) 。 如需詳細資訊，請參閱[頁面的 URL 產生](xref:razor-pages/index#url-generation-for-pages)。

從您最愛的瀏覽器中使用 [檢視原始檔]**** 來檢查產生的標記。 產生的 HTML 部分如下所示：

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

動態產生的連結會傳遞含有查詢字串的電影識別碼 (例如 `https://localhost:5001/Movies/Details?id=1` 中的 `?id=1`)。

### <a name="add-route-template"></a>新增路由範本

更新 [編輯]、[詳細資料] 和 [刪除] Razor 頁面，以使用 "{id： int}" 路由範本。 將這些頁面每一頁的頁面指示詞從 `@page` 變更為 `@page "{id:int}"`。 執行應用程式，然後檢視原始檔。 產生的 HTML 將識別碼新增至 URL 的路徑部分：

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

對使用 "{id:int}" 路由範本的頁面提出的要求若**未**包含整數，將傳回 HTTP 404 (找不到) 錯誤。 例如，`http://localhost:5000/Movies/Details` 會傳回 404 錯誤。 若要使識別碼成為選擇性，請將 `?` 附加至路由條件約束：

 ```cshtml
@page "{id:int?}"
```

若要測試 `@page "{id:int?}"` 的行為：

* 將 *Pages/Movies/Details.cshtml* 中的頁面指示詞設定為 `@page "{id:int?}"`。
* 設定 `public async Task<IActionResult> OnGetAsync(int? id)` (位於 *Pages/Movies/Details.cshtml.cs*) 中的中斷點。
* 瀏覽至 `https://localhost:5001/Movies/Details/`。

使用 `@page "{id:int}"` 指示詞，永遠不會叫用中斷點。 路由引擎會傳回 HTTP 404。 使用 `@page "{id:int?}"`，`OnGetAsync` 方法會傳回 `NotFound` (HTTP 404)。

### <a name="review-concurrency-exception-handling"></a>檢閱並行存取例外狀況處理

在 *Pages/Movies/Edit.cshtml.cs* 檔案中檢閱 `OnPostAsync` 方法：

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Edit.cshtml.cs?name=snippet)]

當一個用戶端刪除電影，而另一個用戶端發佈對電影的變更時，先前的程式碼會偵測並行存取例外狀況。

若要測試 `catch` 區段：

* 在 `catch (DbUpdateConcurrencyException)`上設定中斷點
* 針對電影選取 [編輯]****，進行變更，但不要輸入 [儲存]****。
* 在另一個瀏覽器視窗中，選取相同電影的 **Delete** 連結，然後刪除電影。
* 在先前的瀏覽器視窗中，發佈對電影的變更。

實際執行程式碼可能需要偵測並行存取衝突。 如需詳細資訊，請參閱[處理並行存取衝突](xref:data/ef-rp/concurrency)。

### <a name="posting-and-binding-review"></a>發佈和繫結檢閱內容

檢查 *Pages/Movies/Edit.cshtml.cs* 檔案：

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/SnapShots/Edit.cshtml.cs?name=snippet2)]

對 Movies/Edit 頁面提出 HTTP GET 要求時 (例如，`http://localhost:5000/Movies/Edit/2`)：

* `OnGetAsync` 方法會從資料庫擷取電影，並傳回 `Page` 方法。
* 方法會轉譯 `Page` *頁面/電影/編輯的 cshtml* Razor 頁面。 *Pages/Movies/Edit.cshtml* 檔案包含模型指示詞 (`@model RazorPagesMovie.Pages.Movies.EditModel`)，這會讓電影模型可以在頁面上使用。
* Edit 表單會顯示來自電影的值。

發佈 Movies/Edit 頁面時：

* 頁面上的表單值會繫結至 `Movie` 屬性。 `[BindProperty]` 屬性可讓[模型繫結](xref:mvc/models/model-binding)。

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* 如果模型狀態中有錯誤 (例如 `ReleaseDate` 無法轉換為日期)，則會以提交的值重新顯示表單。
* 如果沒有任何模型錯誤，則會儲存電影。

[索引]、[建立] 和 [刪除] 頁面中的 HTTP GET 方法會 Razor 遵循類似的模式。 在 [建立] 頁面中的 HTTP POST 方法，會 `OnPostAsync` Razor 遵循 `OnPostAsync` [編輯] 頁面中方法的類似模式 Razor 。

## <a name="additional-resources"></a>其他資源

> [!div class="step-by-step"]
> [上一步：使用資料庫](xref:tutorials/razor-pages/sql) 
> [下一步：新增搜尋](xref:tutorials/razor-pages/search)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Scaffolded 電影應用程式是一個不錯的起點，但其呈現效果卻不理想。 **ReleaseDate** 應該是 **Release Date** (兩個字)。

![在 Chrome 中開啟的電影應用程式](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>更新產生的程式碼

開啟 *Models/Movie.cs* 檔案，然後新增下列程式碼中顯示的醒目提示行：

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateFixed.cs?name=snippet_1&highlight=3,12,17)]

`[Column(TypeName = "decimal(18, 2)")]` 資料註解可讓 Entity Framework Core 將 `Price` 正確對應到資料庫中的貨幣。 如需詳細資訊，請參閱[資料類型](/ef/core/modeling/relational/data-types)。

接下來的教學課程會涵蓋 [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6)。 [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) 屬性指定要顯示的欄位名稱 (在本例中為 "Release Date"，而不是 "ReleaseDate")。 [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) 屬性指定資料的類型 (Date)，因此不會顯示儲存在欄位中的時間資訊。

瀏覽至 Pages/Movies，然後將滑鼠停留在 **Edit** 連結，以查看目標 URL。

![滑鼠停留在 Edit 連結並顯示 http://localhost:1234/Movies/Edit/5 的 Url 的瀏覽器視窗](~/tutorials/razor-pages/da1/edit7.png)

在 *Pages/Movies/Index.cshtml* 檔案中，**Edit**、**Details ** 和 **Delete** 連結是由[錨點標記協助程式](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)所產生。

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

[標記](xref:mvc/views/tag-helpers/intro) 協助程式可讓伺服器端程式碼參與建立和轉譯檔案中的 HTML 元素 Razor 。 在上述程式碼中， `AnchorTagHelper` 會以動態方式 `href` 從頁面產生 HTML 屬性值 Razor (路由是相對) 、 `asp-page` 和路由識別碼 (`asp-route-id`) 。 如需詳細資訊，請參閱[頁面的 URL 產生](xref:razor-pages/index#url-generation-for-pages)。

從您最愛的瀏覽器中使用 [檢視原始檔]**** 來檢查產生的標記。 產生的 HTML 部分如下所示：

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

動態產生的連結會傳遞含有查詢字串的電影識別碼 (例如 `https://localhost:5001/Movies/Details?id=1` 中的 `?id=1`)。

更新 [編輯]、[詳細資料] 和 [刪除] Razor 頁面，以使用 "{id： int}" 路由範本。 將這些頁面每一頁的頁面指示詞從 `@page` 變更為 `@page "{id:int}"`。 執行應用程式，然後檢視原始檔。 產生的 HTML 將識別碼新增至 URL 的路徑部分：

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

對使用 "{id:int}" 路由範本的頁面提出的要求若**未**包含整數，將傳回 HTTP 404 (找不到) 錯誤。 例如，`http://localhost:5000/Movies/Details` 會傳回 404 錯誤。 若要使識別碼成為選擇性，請將 `?` 附加至路由條件約束：

 ```cshtml
@page "{id:int?}"
```

若要測試 `@page "{id:int?}"` 的行為：

* 將 *Pages/Movies/Details.cshtml* 中的頁面指示詞設定為 `@page "{id:int?}"`。
* 設定 `public async Task<IActionResult> OnGetAsync(int? id)` (位於 *Pages/Movies/Details.cshtml.cs*) 中的中斷點。
* 瀏覽至 `https://localhost:5001/Movies/Details/`。

使用 `@page "{id:int}"` 指示詞，永遠不會叫用中斷點。 路由引擎會傳回 HTTP 404。 使用 `@page "{id:int?}"`，`OnGetAsync` 方法會傳回 `NotFound` (HTTP 404)。

### <a name="review-concurrency-exception-handling"></a>檢閱並行存取例外狀況處理

在 *Pages/Movies/Edit.cshtml.cs* 檔案中檢閱 `OnPostAsync` 方法：

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Edit.cshtml.cs?name=snippet)]

當一個用戶端刪除電影，而另一個用戶端發佈對電影的變更時，先前的程式碼會偵測並行存取例外狀況。

若要測試 `catch` 區段：

* 在 `catch (DbUpdateConcurrencyException)`上設定中斷點
* 針對電影選取 [編輯]****，進行變更，但不要輸入 [儲存]****。
* 在另一個瀏覽器視窗中，選取相同電影的 **Delete** 連結，然後刪除電影。
* 在先前的瀏覽器視窗中，發佈對電影的變更。

實際執行程式碼可能需要偵測並行存取衝突。 如需詳細資訊，請參閱[處理並行存取衝突](xref:data/ef-rp/concurrency)。

### <a name="posting-and-binding-review"></a>發佈和繫結檢閱內容

檢查 *Pages/Movies/Edit.cshtml.cs* 檔案：

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit21.cshtml.cs?name=snippet2)]

對 Movies/Edit 頁面提出 HTTP GET 要求時 (例如，`http://localhost:5000/Movies/Edit/2`)：

* `OnGetAsync` 方法會從資料庫擷取電影，並傳回 `Page` 方法。 
* 方法會轉譯 `Page` *頁面/電影/編輯的 cshtml* Razor 頁面。 *Pages/Movies/Edit.cshtml* 檔案包含模型指示詞 (`@model RazorPagesMovie.Pages.Movies.EditModel`)，這會讓電影模型可以在頁面上使用。
* Edit 表單會顯示來自電影的值。

發佈 Movies/Edit 頁面時：

* 頁面上的表單值會繫結至 `Movie` 屬性。 `[BindProperty]` 屬性可讓[模型繫結](xref:mvc/models/model-binding)。

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* 如果模型狀態中有錯誤 (例如 `ReleaseDate` 無法轉換為日期)，則會以提交的值顯示表單。
* 如果沒有任何模型錯誤，則會儲存電影。

[索引]、[建立] 和 [刪除] 頁面中的 HTTP GET 方法會 Razor 遵循類似的模式。 在 [建立] 頁面中的 HTTP POST 方法，會 `OnPostAsync` Razor 遵循 `OnPostAsync` [編輯] 頁面中方法的類似模式 Razor 。

搜尋會在接下來的教學課程中新增。

## <a name="additional-resources"></a>其他資源

* [本教學課程的 YouTube 版本](https://youtu.be/yLnnleREMtQ)

> [!div class="step-by-step"]
> [上一步：使用資料庫](xref:tutorials/razor-pages/sql) 
> [下一步：新增搜尋](xref:tutorials/razor-pages/search)

::: moniker-end
