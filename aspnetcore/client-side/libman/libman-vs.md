---
title: 在 Visual Studio 中使用 LibMan 搭配 ASP.NET Core
author: scottaddie
description: 瞭解如何在具有 Visual Studio 的 ASP.NET Core 專案中使用 LibMan。
ms.author: scaddie
ms.custom: mvc
ms.date: 08/20/2018
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
uid: client-side/libman/libman-vs
ms.openlocfilehash: 77cfced18edd9db5be21265469b42f32e3de274d
ms.sourcegitcommit: 65add17f74a29a647d812b04517e46cbc78258f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/19/2020
ms.locfileid: "88625670"
---
# <a name="use-libman-with-aspnet-core-in-visual-studio"></a>在 Visual Studio 中使用 LibMan 搭配 ASP.NET Core

作者：[Scott Addie](https://twitter.com/Scott_Addie)

Visual Studio 在 ASP.NET Core 專案中有內建的 [LibMan](xref:client-side/libman/index) 支援，包括：

* 支援在組建上設定和執行 LibMan 還原作業。
* 用於觸發 LibMan 還原和清除作業的功能表項目。
* 搜尋對話方塊來尋找程式庫，並將檔案新增至專案。
* 在 LibMan 資訊清單檔案*上編輯libman.js*的支援 &mdash; 。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/client-side/libman/samples/) [ (如何下載) ](xref:index#how-to-download-a-sample)

## <a name="prerequisites"></a>Prerequisites

* **ASP.NET 和 網頁程式開發**工作負載的[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)

## <a name="add-library-files"></a>新增程式庫檔案

您可以使用兩種不同的方式，將程式庫檔案新增至 ASP.NET Core 專案：

1. [使用 [新增用戶端程式庫] 對話方塊](#use-the-add-client-side-library-dialog)
1. [手動設定 LibMan 資訊清單檔案專案](#manually-configure-libman-manifest-file-entries)

### <a name="use-the-add-client-side-library-dialog"></a>使用 [新增用戶端程式庫] 對話方塊

請依照下列步驟安裝用戶端程式庫：

* 在 **方案總管**中，以滑鼠右鍵按一下要加入檔案的專案資料夾。 選擇 [**新增**  >  **客戶**端程式庫]。 [ **新增客戶** 端程式庫] 對話方塊隨即出現：

  ![[新增用戶端程式庫] 對話方塊](_static/add-library-dialog.png)

* 從 [ **提供者** ] 下拉式清單中選取 [程式庫提供者]。 CDNJS 是預設的提供者。
* 在 [連結 **庫** ] 文字方塊中，輸入要提取的程式庫名稱。 IntelliSense 會提供以提供的文字開頭的程式庫清單。
* 從 IntelliSense 清單中選取媒體櫃。 請注意，程式庫名稱的結尾是 `@` 符號以及所選提供者已知的最新穩定版本。
* 決定要包含的檔案：
  * 選取 [ **包含所有文件庫** 檔案] 選項按鈕，以包含所有程式庫的檔案。
  * 選取 [ **選擇特定** 檔案] 選項按鈕以包含文件庫檔案的子集。 選取選項按鈕時，就會啟用檔案選取器樹狀目錄。 勾選檔案名左邊的方塊以下載。
* 在 [ **目標位置** ] 文字方塊中，指定用來儲存檔案的專案資料夾。 建議您將每個程式庫儲存在不同的資料夾中。

  建議的 **目標位置** 資料夾是以啟動對話方塊的位置為基礎：

  * 如果從專案根目錄啟動：
    * 如果*wwwroot*存在，則使用*wwwroot/lib* 。
    * 如果*wwwroot*不存在，就會使用*lib* 。
  * 如果從專案資料夾啟動，則會使用對應的資料夾名稱。

  資料夾的建議是以程式庫名稱做為尾碼。 下表說明在頁面專案中安裝 jQuery 時的資料夾建議 Razor 。
  
  |啟動位置                           |建議的資料夾      |
  |------------------------------------------|----------------------|
  |如果 *wwwroot* 存在，則專案根目錄 ()         |*wwwroot/lib/jquery/* |
  |如果 *wwwroot* 不存在，則專案根目錄 ()  |*lib/jquery/*         |
  |Project 中的*Pages*資料夾                 |*Pages/jquery/*       |

* 按一下 [ **安裝** ] 按鈕，根據 *libman.js*中的設定下載檔案。
* 請參閱 [**輸出**] 視窗的 [連結**庫管理員**摘要]，以取得安裝詳細資料。 例如：

  ```console
  Restore operation started...
  Restoring libraries for project LibManSample
  Restoring library jquery@3.3.1... (LibManSample)
  wwwroot/lib/jquery/jquery.min.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.min.map written to destination (LibManSample)
  Restore operation completed
  1 libraries restored in 2.32 seconds
  ```

### <a name="manually-configure-libman-manifest-file-entries"></a>手動設定 LibMan 資訊清單檔案專案

Visual Studio 中的所有 LibMan 作業都是以專案根目錄 LibMan 資訊清單的內容為基礎，) * 上 (libman.js* 。 您可以手動編輯 *libman.js* ，以設定專案的程式庫檔案。 Visual Studio 儲存 *libman.js上的* 所有程式庫檔案。

若要開啟 *libman.json* 進行編輯，有下列選項：

* 在**方案總管**中，按兩下檔案*libman.js* 。
* 以滑鼠右鍵按一下 **方案總管** 中的專案，然後選取 [ **管理客戶**端程式庫]。 **&#8224;**
* 從 Visual Studio 的 [**專案**] 功能表中選取 [**管理客戶**端程式庫]。 **&#8224;**

**&#8224;** 如果專案根目錄中還沒有 *libman.js* 的檔案，則會以預設專案範本內容建立。

Visual Studio 提供豐富的 JSON 編輯支援，例如顏色標示、格式化、IntelliSense 和架構驗證。 您可以在中找到 LibMan 資訊清單的 JSON 架構 [https://json.schemastore.org/libman](https://json.schemastore.org/libman) 。

使用下列資訊清單檔時，LibMan 會根據屬性中定義的設定來抓取檔案 `libraries` 。 以下是中定義的物件常值的說明 `libraries` ：

* 從 CDNJS 提供者取出 [jQuery](https://jquery.com/) 版本3.3.1 的子集。 子集定義于 `files` 屬性 &mdash; *jquery.min.js*、 *jquery.js*和*jquery. 對應*中。 檔案會放在專案的 *wwwroot/lib/jquery* 資料夾中。
* 完整的 [啟動](https://getbootstrap.com/) 程式版本4.1.3 會被抓取並放在 *wwwroot/lib/啟動* 程式資料夾中。 物件常值的 `provider` 屬性會覆寫 `defaultProvider` 屬性值。 LibMan 會從 unpkg 提供者抓取啟動載入器檔案。
* 組織內的管理主體已核准 [lodash 所](https://lodash.com/) 的子集。 *lodash.js*和*lodash.min.js*的檔案會從本機檔案系統中取出，位於*C \\ ： \\ temp \\ lodash 所*。 這些檔案會複製到專案的 *wwwroot/lib/lodash 所* 資料夾。

[!code-json[](samples/LibManSample/libman.json)]

> [!NOTE]
> LibMan 只支援每個提供者的一個版本的程式庫。 如果檔案的libman.js包含兩個具有相同程式庫名稱的程式庫，則不會 * 在* 架構驗證時失敗。

## <a name="restore-library-files"></a>還原程式庫檔案

若要從 Visual Studio 內還原程式庫檔案，專案根目錄中的檔案上必須有有效的 *libman.js* 。 還原的檔案會放在專案中的每個程式庫所指定的位置。

您可以用兩種方式在 ASP.NET Core 專案中還原程式庫檔案：

1. [在組建期間還原檔](#restore-files-during-build)
1. [手動還原檔](#restore-files-manually)

### <a name="restore-files-during-build"></a>在組建期間還原檔

LibMan 可將定義的程式庫檔案還原為組建程式的一部分。 依預設，會停用 *還原時版本* 的行為。

若要啟用並測試還原的組建行為：

* 以滑鼠右鍵按一下**方案總管**中的 [ *libman.js* ]，然後在內容功能表上選取 [從**組建還原客戶**端程式庫]。
* 當系統提示您安裝 NuGet 套件時，請按一下 [ **是]** 按鈕。 LibraryManager 會將 [組建](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) NuGet 套件新增至專案：

  [!code-xml[](samples/LibManSample/LibManSample.csproj?name=snippet_RestoreOnBuildPackage)]

* 建立專案以確認發生 LibMan 檔案還原。 `Microsoft.Web.LibraryManager.Build`封裝會插入在專案的組建作業期間執行 LibMan 的 MSBuild 目標。
* 檢查 LibMan 活動記錄的**輸出**視窗的**組建**摘要：

  ```console
  1>------ Build started: Project: LibManSample, Configuration: Debug Any CPU ------
  1>
  1>Restore operation started...
  1>Restoring library jquery@3.3.1...
  1>Restoring library bootstrap@4.1.3...
  1>
  1>2 libraries restored in 10.66 seconds
  1>LibManSample -> C:\LibManSample\bin\Debug\netcoreapp2.1\LibManSample.dll
  ========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
  ```

當啟用還原後的行為時，內容功能表上的 [ *libman.js* ] 會顯示 [ **在組建時停用還原客戶** 端程式庫] 選項。 選取此選項會移除 `Microsoft.Web.LibraryManager.Build` 專案檔中的套件參考。 因此，每個組建上都不會再還原用戶端程式庫。

不論是在組建上進行的設定為何，您都可以從內容功能表 * 上的libman.js* 隨時手動還原。 如需詳細資訊，請參閱 [手動還原檔](#restore-files-manually)。

### <a name="restore-files-manually"></a>手動還原檔

手動還原程式庫檔案：

* 針對方案中的所有專案：
  * 以滑鼠右鍵按一下 **方案總管**中的方案名稱。
  * 選取 [ **還原客戶** 端程式庫] 選項。
* 針對特定專案：
  * 在**方案總管**中，以滑鼠右鍵按一下檔案*libman.js* 。
  * 選取 [ **還原客戶** 端程式庫] 選項。

當還原作業正在執行時：

* Visual Studio 狀態列上的工作狀態中心 (TSC) 圖示將會進行動畫處理，並會 *開始讀取還原*作業。 按一下圖示會開啟工具提示，其中列出已知的背景工作。
* 訊息將會傳送至 [**輸出**] 視窗的狀態列和連結**庫管理員**摘要。 例如：

  ```console
  Restore operation started...
  Restoring libraries for project LibManSample
  Restoring library jquery@3.3.1... (LibManSample)
  wwwroot/lib/jquery/jquery.min.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.min.map written to destination (LibManSample)
  Restore operation completed
  1 libraries restored in 2.32 seconds
  ```

## <a name="delete-library-files"></a>刪除程式庫檔案

若要執行 *清除* 作業，這會刪除先前在 Visual Studio 中還原的程式庫檔案：

* 在**方案總管**中，以滑鼠右鍵按一下檔案*libman.js* 。
* 選取 [ **清除客戶** 端程式庫] 選項。

為了避免意外移除非程式庫檔案，清除作業並不會刪除整個目錄。 它只會移除先前還原中包含的檔案。

清除作業正在執行時：

* Visual Studio 狀態列上的 TSC 圖示將會進行動畫處理，並會 *開始讀取用戶端程式庫*作業。 按一下圖示會開啟工具提示，其中列出已知的背景工作。
* 訊息會傳送至 [**輸出**] 視窗的狀態列和連結**庫管理員**摘要。 例如：

```console
Clean libraries operation started...
Clean libraries operation completed
2 libraries were successfully deleted in 1.91 secs
```

清除作業只會刪除專案中的檔案。 程式庫檔案會留在快取中，以便更快抓取未來的還原作業。 若要管理儲存在本機電腦快取中的程式庫檔案，請使用 [LIBMAN CLI](xref:client-side/libman/libman-cli)。

## <a name="uninstall-library-files"></a>卸載程式庫檔案

卸載程式庫檔案：

* 開啟 *libman.js開啟*]。
* 將插入號放在對應的 `libraries` 物件常值內。
* 按一下出現在左邊界的燈泡圖示，然後選取 [**卸載 \<library_name> @ \<library_version> **]：

  ![卸載程式庫內容功能表選項](_static/uninstall-menu-option.png)

或者，您也可以 * 在) 上* 手動編輯和儲存 LibMan 資訊清單 (libman.js。 儲存檔案時，會執行 [還原](#restore-library-files) 作業。 在 *libman.js* 中不再定義的程式庫檔案，會從專案中移除。

## <a name="update-library-version"></a>更新程式庫版本

若要檢查更新的程式庫版本：

* 開啟 *libman.js開啟*]。
* 將插入號放在對應的 `libraries` 物件常值內。
* 按一下出現在左邊界中的燈泡圖示。 將滑鼠停留在 **檢查更新的**上方。

LibMan 會檢查是否有比安裝的版本還要新的程式庫版本。 可能會發生下列結果：

* 如果已經安裝最新版本，則會顯示 [ **找不到更新** ] 的訊息。
* 如果尚未安裝，則會顯示最新的穩定版本。

  ![檢查更新內容功能表選項](_static/update-menu-option.png)

* 如果發行前版本比安裝的版本還要新，則會顯示發行前版本。

若要降級為較舊的程式庫版本，請手動編輯檔案 * 上的libman.js* 。 儲存檔案時，LibMan [還原](#restore-library-files)作業：

* 從舊版移除多餘的檔案。
* 從新版本加入新的和更新的檔案。

## <a name="additional-resources"></a>其他資源

* <xref:client-side/libman/libman-cli>
* [LibMan GitHub 存放庫](https://github.com/aspnet/LibraryManager)
