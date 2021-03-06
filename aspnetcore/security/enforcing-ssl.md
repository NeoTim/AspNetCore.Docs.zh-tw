---
title: 在 ASP.NET Core 中強制使用 HTTPS
author: rick-anderson
description: 瞭解如何要求 ASP.NET Core web 應用程式中的 HTTPS/TLS。
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2019
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
uid: security/enforcing-ssl
ms.openlocfilehash: b5260084c2fdd296168e918f06d8b54faf1865d5
ms.sourcegitcommit: 24106b7ffffc9fff410a679863e28aeb2bbe5b7e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90722653"
---
# <a name="enforce-https-in-aspnet-core"></a>在 ASP.NET Core 中強制使用 HTTPS

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

本檔說明如何：

* 所有要求都需要 HTTPS。
* 將所有 HTTP 要求都重新導向至 HTTPS。

沒有任何 API 可以防止用戶端在第一個要求上傳送機密資料。

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> ## <a name="api-projects"></a>API 專案
>
> 請勿 **在** 接收敏感資訊的 Web api 上使用 [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) 。 `RequireHttpsAttribute` 使用 HTTP 狀態碼將瀏覽器從 HTTP 重新導向至 HTTPS。 API 用戶端可能無法理解或遵守從 HTTP 到 HTTPS 的重新導向。 這類用戶端可能會透過 HTTP 傳送資訊。 Web Api 必須：
>
> * 未在 HTTP 上接聽。
> * 關閉狀態碼為400的連線 (錯誤的要求) ，而不提供要求。
>
> ## <a name="hsts-and-api-projects"></a>HSTS 和 API 專案
>
> 預設 API 專案不包含 [HSTS](#hsts) ，因為 HSTS 通常是僅限瀏覽器的指令。 其他呼叫端（例如電話或桌面應用程式） **不會** 遵守指令。 即使在瀏覽器中，透過 HTTP 對 API 進行的單一驗證呼叫也會對不安全的網路產生風險。 安全的方法是將 API 專案設定為只透過 HTTPS 接聽和回應。

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

> [!WARNING]
> ## <a name="api-projects"></a>API 專案
>
> 請勿 **在** 接收敏感資訊的 Web api 上使用 [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) 。 `RequireHttpsAttribute` 使用 HTTP 狀態碼將瀏覽器從 HTTP 重新導向至 HTTPS。 API 用戶端可能無法理解或遵守從 HTTP 到 HTTPS 的重新導向。 這類用戶端可能會透過 HTTP 傳送資訊。 Web Api 必須：
>
> * 未在 HTTP 上接聽。
> * 關閉狀態碼為400的連線 (錯誤的要求) ，而不提供要求。

::: moniker-end

## <a name="require-https"></a>需要 HTTPS

建議 ASP.NET Core web apps 使用生產環境：

* HTTPS 重新導向中介軟體 (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) 將 HTTP 要求重新導向至 HTTPS。
* HSTS 中介軟體 ([UseHsts](#http-strict-transport-security-protocol-hsts)) ，以將 HTTP Strict 傳輸安全性通訊協定 (HSTS) 標頭傳送給用戶端。

> [!NOTE]
> 部署在反向 proxy 設定中的應用程式，可讓 proxy 處理 (HTTPS) 的連線安全性。 如果 proxy 也處理 HTTPS 重新導向，則不需要使用 HTTPS 重新導向中介軟體。 如果 proxy 伺服器也會處理寫入 HSTS 標頭 (例如， [IIS 10.0 (1709) 或更新版本) 中的原生 HSTS 支援](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support) ，則應用程式不需要 HSTS 中介軟體。 如需詳細資訊，請參閱 [在建立專案時退出宣告 HTTPS/HSTS](#opt-out-of-httpshsts-on-project-creation)。

### <a name="usehttpsredirection"></a>UseHttpsRedirection

下列程式碼會呼叫 `UseHttpsRedirection` 類別中的 `Startup` ：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet1&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet1&highlight=13)]

::: moniker-end

上述反白顯示的程式碼：

* 使用預設的 [HttpsRedirectionOptions. RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)) 。
* 除非由環境變數或 >iserveraddressesfeature 覆寫，否則會使用預設的[HttpsRedirectionOptions. HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) 。 `ASPNETCORE_HTTPS_PORT` [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature)

我們建議使用暫時重新導向，而不是永久重新導向。 連結快取可能會在開發環境中造成不穩定的行為。 如果您想要在應用程式處於非開發環境時傳送永久的重新導向狀態碼，請參閱「 [在生產環境中設定永久重新導向](#configure-permanent-redirects-in-production) 」一節。 建議您使用 [HSTS](#http-strict-transport-security-protocol-hsts) 來通知用戶端，只將安全的資源要求傳送至應用程式 (只能在生產) 中傳送給應用程式。

### <a name="port-configuration"></a>連接埠組態

必須有埠才能讓中介軟體將不安全的要求重新導向至 HTTPS。 如果沒有可用的埠：

* 不會重新導向至 HTTPS。
* 中介軟體會記錄「無法判斷重新導向的 HTTPs 埠」警告。

使用下列任何一種方法來指定 HTTPS 埠：

* 設定 [HttpsRedirectionOptions. HttpsPort](#options)。

::: moniker range=">= aspnetcore-3.0"

* 設定 `https_port` [主機設定](../fundamentals/host/generic-host.md?view=aspnetcore-3.0#https_port)：

  * 在 [主機設定] 中。
  * 藉由設定 `ASPNETCORE_HTTPS_PORT` 環境變數。
  * 藉由在 *appsettings.js*中新增最上層專案：

    [!code-json[](enforcing-ssl/sample-snapshot/3.x/appsettings.json?highlight=2)]

* 使用 [ASPNETCORE_URLS 環境變數](../fundamentals/host/generic-host.md?view=aspnetcore-3.0#urls)來表示具有安全配置的埠。 環境變數會設定伺服器。 中介軟體會透過，間接探索 HTTPS 埠 <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> 。 這種方法不適用於反向 proxy 部署。

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

* 設定 `https_port` [主機設定](xref:fundamentals/host/web-host#https-port)：

  * 在 [主機設定] 中。
  * 藉由設定 `ASPNETCORE_HTTPS_PORT` 環境變數。
  * 藉由在 *appsettings.js*中新增最上層專案：

    [!code-json[](enforcing-ssl/sample-snapshot/2.x/appsettings.json?highlight=2)]

* 使用 [ASPNETCORE_URLS 環境變數](xref:fundamentals/host/web-host#server-urls)來表示具有安全配置的埠。 環境變數會設定伺服器。 中介軟體會透過，間接探索 HTTPS 埠 <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> 。 這種方法不適用於反向 proxy 部署。

::: moniker-end

* 在開發中，請在 *launchsettings.js*中設定 HTTPS URL。 使用 IIS Express 時啟用 HTTPS。

* 針對 [Kestrel](xref:fundamentals/servers/kestrel) server 或 [HTTP.sys](xref:fundamentals/servers/httpsys) server 的公眾面向邊緣部署設定 HTTPS URL 端點。 應用程式只會使用 **一個 HTTPS 埠** 。 中介軟體會透過來探索埠 <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> 。

> [!NOTE]
> 當應用程式在反向 proxy 設定中執行時， <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> 無法使用。 使用本節中所述的其中一種方法來設定埠。

### <a name="edge-deployments"></a>Edge 部署 

當 Kestrel 或 HTTP.sys 用作公眾對應的 edge server 時，必須將 Kestrel 或 HTTP.sys 設定為同時接聽兩者：

* 用戶端重新導向的安全埠 (通常是在生產環境中為443，而開發) 的5001。
* 不安全的 (埠在生產環境中通常是80，而開發) 是5000。

用戶端必須能夠存取不安全的埠，應用程式才能接收不安全的要求，並將用戶端重新導向至安全的埠。

如需詳細資訊，請參閱 [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) 或 <xref:fundamentals/servers/httpsys> 。

### <a name="deployment-scenarios"></a>部署案例

用戶端與伺服器之間的任何防火牆也必須針對流量開啟通訊埠。

如果在反向 proxy 設定中轉送要求，請在呼叫 HTTPS 重新導向中介軟體之前，先使用 [轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer) 。 轉送的標頭中介軟體會 `Request.Scheme` 使用 `X-Forwarded-Proto` 標頭來更新。 中介軟體允許重新導向 Uri 和其他安全性原則正確運作。 未使用轉送的標頭中介軟體時，後端應用程式可能不會收到正確的配置，最後會以重新導向迴圈結束。 常見的終端使用者錯誤訊息是發生太多重新導向。

部署至 Azure App Service 時，請遵循教學課程 [：將現有的自訂 SSL 憑證系結至 Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl)中的指導方針。

### <a name="options"></a>選項

下列反白顯示的程式碼會呼叫 [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) 來設定中介軟體選項：


::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet2&highlight=14-18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet2&highlight=14-18)]

::: moniker-end


`AddHttpsRedirection`只有變更或的值才需要呼叫 `HttpsPort` `RedirectStatusCode` 。

上述反白顯示的程式碼：

* 將 [HttpsRedirectionOptions RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) 為 <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect> ，這是預設值。 使用類別的欄位來 <xref:Microsoft.AspNetCore.Http.StatusCodes> 指派給 `RedirectStatusCode` 。
* 將 HTTPS 埠設定為5001。

#### <a name="configure-permanent-redirects-in-production"></a>在生產環境中設定永久重新導向

中介軟體預設為傳送具有所有重新導向的 [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) 。 如果您想要在應用程式處於非開發環境時傳送永久的重新導向狀態碼，請在非開發環境的條件式檢查中包裝中介軟體選項設定。

::: moniker range=">= aspnetcore-3.0"

在 *Startup.cs*中設定服務時：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // IWebHostEnvironment (stored in _env) is injected into the Startup class.
    if (!_env.IsDevelopment())
    {
        services.AddHttpsRedirection(options =>
        {
            options.RedirectStatusCode = StatusCodes.Status308PermanentRedirect;
            options.HttpsPort = 443;
        });
    }
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

在 *Startup.cs*中設定服務時：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // IHostingEnvironment (stored in _env) is injected into the Startup class.
    if (!_env.IsDevelopment())
    {
        services.AddHttpsRedirection(options =>
        {
            options.RedirectStatusCode = StatusCodes.Status308PermanentRedirect;
            options.HttpsPort = 443;
        });
    }
}
```

::: moniker-end


## <a name="https-redirection-middleware-alternative-approach"></a>HTTPS 重新導向中介軟體替代方法

使用 HTTPS 重新導向中介軟體 () 的替代方式， `UseHttpsRedirection` 是使用 URL 重寫中介軟體 (`AddRedirectToHttps`) 。 `AddRedirectToHttps` 也可以在執行重新導向時設定狀態碼和埠。 如需詳細資訊，請參閱 [URL 重寫中介軟體](xref:fundamentals/url-rewriting)。

重新導向至 HTTPS 而不需要額外的重新導向規則時，建議使用 HTTPS 重新導向中介軟體 (`UseHttpsRedirection`) 本主題中所述。

<a name="hsts"></a>

## <a name="http-strict-transport-security-protocol-hsts"></a>HTTP Strict Transport Security Protocol (HSTS) 

根據 [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project)， [HTTP Strict TRANSPORT Security (HSTS) ](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html) 是由 web 應用程式透過使用回應標頭所指定的選擇性加入安全性增強功能。 當 [支援 HSTS 的瀏覽器](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Protection_Cheat_Sheet.html#browser-support) 收到此標頭時：

* 瀏覽器會儲存網域的設定，以防止透過 HTTP 傳送任何通訊。 瀏覽器會強制透過 HTTPS 進行所有通訊。
* 瀏覽器會防止使用者使用未受信任或不正確憑證。 瀏覽器會停用允許使用者暫時信任這類憑證的提示。

由於 HSTS 是由用戶端強制執行，因此有一些限制：

* 用戶端必須支援 HSTS。
* HSTS 至少需要一個成功的 HTTPS 要求才能建立 HSTS 原則。
* 應用程式必須檢查每個 HTTP 要求，並重新導向或拒絕 HTTP 要求。

ASP.NET Core 2.1 和更新版本會使用擴充方法來執行 HSTS `UseHsts` 。 `UseHsts`當應用程式不在[開發模式](xref:fundamentals/environments)時，會呼叫下列程式碼：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet1&highlight=11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet1&highlight=10)]

::: moniker-end

`UseHsts` 在開發中不建議使用，因為 HSTS 設定可由瀏覽器高度快取。 預設會 `UseHsts` 排除本機回送位址。

若是第一次執行 HTTPS 的生產環境，請使用其中一種方法將初始[HstsOptions](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*)設定為較小的值。 <xref:System.TimeSpan> 如果您需要將 HTTPS 基礎結構還原為 HTTP，請將值從小時設定為不超過一天。 在您確信 HTTPS 設定的持續性之後，請增加 HSTS `max-age` 值; 常用的值是一年。

下列程式碼：


::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet2&highlight=5-12)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet2&highlight=5-12)]

::: moniker-end


* 設定標頭的預先載入參數 `Strict-Transport-Security` 。 預先載入不是 [RFC HSTS 規格](https://tools.ietf.org/html/rfc6797)的一部分，但是網頁瀏覽器支援在全新安裝時預先載入 HSTS 網站。 如需詳細資訊，請參閱 [https://hstspreload.org/](https://hstspreload.org/)。
* 啟用 [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2)，這會將 HSTS 原則套用至主機子域。
* 將 `max-age` 標頭的參數明確設定 `Strict-Transport-Security` 為60天。 如果未設定，則預設為30天。 如需詳細資訊，請參閱 [最大壽命](https://tools.ietf.org/html/rfc6797#section-6.1.1)指示詞。
* 新增 `example.com` 至要排除的主機清單。

`UseHsts` 排除下列回送主機：

* `localhost` ： IPv4 回送位址。
* `127.0.0.1` ： IPv4 回送位址。
* `[::1]` ： IPv6 回送位址。

## <a name="opt-out-of-httpshsts-on-project-creation"></a>在建立專案時退出宣告 HTTPS/HSTS

在某些後端服務案例中，連線安全性是在網路公眾面向的邊緣處理的，因此不需要在每個節點上設定連接安全性。 從 Visual Studio 中的範本或從 [dotnet new](/dotnet/core/tools/dotnet-new) 命令產生的 Web 應用程式會啟用 [HTTPS](#require-https) 重新導向和 [HSTS](#http-strict-transport-security-protocol-hsts)。 針對不需要這些案例的部署，您可以在從範本建立應用程式時退出宣告 HTTPS/HSTS。

退出宣告 HTTPS/HSTS：

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio) 

取消核取 [ **針對 HTTPS 進行設定** ] 核取方塊。

::: moniker range=">= aspnetcore-3.0"

![新的 ASP.NET Core Web 應用程式] 對話方塊，顯示未選取的 [設定 HTTPS] 核取方塊。](enforcing-ssl/_static/out-vs2019.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

![新的 ASP.NET Core Web 應用程式] 對話方塊，顯示未選取的 [設定 HTTPS] 核取方塊。](enforcing-ssl/_static/out.png)

::: moniker-end


# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli) 

使用 `--no-https` 選項。 例如

```dotnetcli
dotnet new webapp --no-https
```

---

<a name="trust"></a>

## <a name="trust-the-aspnet-core-https-development-certificate-on-windows-and-macos"></a>信任 Windows 和 macOS 上的 ASP.NET Core HTTPS 開發憑證

.NET Core SDK 包含 HTTPS 開發憑證。 憑證會在首次執行體驗中安裝。 例如，會 `dotnet --info` 產生下列輸出的變化：

```
ASP.NET Core
------------
Successfully installed the ASP.NET Core HTTPS Development Certificate.
To trust the certificate run 'dotnet dev-certs https --trust' (Windows and macOS only).
For establishing trust on other platforms refer to the platform specific documentation.
For more information on configuring HTTPS see https://go.microsoft.com/fwlink/?linkid=848054.
```

安裝 .NET Core SDK 會將 ASP.NET Core HTTPS 開發憑證安裝至本機使用者憑證存放區。 憑證已安裝，但不受信任。 若要信任憑證，請執行一次性步驟來執行 dotnet `dev-certs` 工具：

```dotnetcli
dotnet dev-certs https --trust
```

下列命令會提供 `dev-certs` 工具的說明：

```dotnetcli
dotnet dev-certs https --help
```

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a>如何設定 Docker 的開發人員憑證

請參閱[這個 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/6199)。

<a name="ssl-linux"></a>

## <a name="trust-https-certificate-on-linux"></a>信賴 Linux 上的 HTTPS 憑證

<!-- Instructions to be updated by engineering team after 5.0 RTM. -->

如需 Linux 的指示，請參閱散發檔。

<a name="wsl"></a>

## <a name="trust-https-certificate-from-windows-subsystem-for-linux"></a>信任 Windows 子系統 Linux 版的 HTTPS 憑證

Windows 子系統 Linux 版 (WSL) 會產生 HTTPS 自我簽署憑證。若要設定 Windows 憑證存放區以信任 WSL 憑證：

* 執行下列命令以匯出 WSL 產生的憑證：

  ```
  dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p <cryptic-password>
  ```
* 在 WSL 視窗中，執行下列命令：

  ```
    ASPNETCORE_Kestrel__Certificates__Default__Password="<cryptic-password>" 
    ASPNETCORE_Kestrel__Certificates__Default__Path=/mnt/c/Users/user-name/.aspnet/https/aspnetapp.pfx
    dotnet watch run
  ```

  上述命令會設定環境變數，讓 Linux 使用 Windows 受信任的憑證。

## <a name="troubleshoot-certificate-problems"></a>針對憑證問題進行疑難排解

本節提供 [安裝和信任](#trust)ASP.NET Core HTTPS 開發憑證時的說明，但您仍有不信任憑證的瀏覽器警告。 [Kestrel](xref:fundamentals/servers/kestrel)會使用 ASP.NET Core HTTPS 開發憑證。

### <a name="all-platforms---certificate-not-trusted"></a>所有平臺-憑證不受信任

執行下列命令：

```dotnetcli
dotnet dev-certs https --clean
dotnet dev-certs https --trust
```

關閉任何開啟的瀏覽器實例。 開啟新的瀏覽器視窗以使用應用程式。 瀏覽器會快取憑證信任。

上述命令會解決大部分的瀏覽器信任問題。 如果瀏覽器仍不信任憑證，請遵循下列平臺特定的建議。

### <a name="docker---certificate-not-trusted"></a>Docker-憑證不受信任

* 刪除 *C:\Users \{ USER} \AppData\Roaming\ASP.NET\Https* 資料夾。
* 清除方案。 刪除 [bin]** 和 [obj]** 資料夾。
* 重新開機開發工具。 例如，Visual Studio、Visual Studio Code 或 Visual Studio for Mac。

### <a name="windows---certificate-not-trusted"></a>Windows-憑證不受信任

* 檢查證書存儲中的憑證。 `localhost` `ASP.NET Core HTTPS development certificate` 在和下應該有易記名稱的憑證 `Current User > Personal > Certificates``Current User > Trusted root certification authorities > Certificates`
* 從個人和受信任的根憑證授權單位移除所有找到的憑證。 請勿 **移除 IIS Express** localhost 憑證。
* 執行下列命令：

```dotnetcli
dotnet dev-certs https --clean
dotnet dev-certs https --trust
```

關閉任何開啟的瀏覽器實例。 開啟新的瀏覽器視窗以使用應用程式。

### <a name="os-x---certificate-not-trusted"></a>OS X-憑證不受信任

* 開啟 [KeyChain 存取]。
* 選取 [系統 keychain]。
* 檢查 localhost 憑證是否存在。
* 檢查其是否包含 `+` 圖示上的符號，以指出它是針對所有使用者所信任。
* 從系統 keychain 移除憑證。
* 執行下列命令：

```dotnetcli
dotnet dev-certs https --clean
dotnet dev-certs https --trust
```

關閉任何開啟的瀏覽器實例。 開啟新的瀏覽器視窗以使用應用程式。

請參閱 [使用 IIS Express (dotnet/AspNetCore #16892) ](https://github.com/dotnet/AspNetCore/issues/16892) 進行 Visual Studio 的憑證問題疑難排解的 HTTPS 錯誤。

### <a name="iis-express-ssl-certificate-used-with-visual-studio"></a>IIS Express 與 Visual Studio 搭配使用的 SSL 憑證

若要修正 IIS Express 憑證的問題，請選取 Visual Studio 安裝程式中的 [ **修復** ]。 如需詳細資訊，請參閱 [此 GitHub 問題](https://github.com/dotnet/aspnetcore/issues/16892)。

## <a name="additional-information"></a>其他資訊

* <xref:host-and-deploy/proxy-load-balancer>
* [使用 Apache： HTTPS 設定的 Linux 上的主機 ASP.NET Core](xref:host-and-deploy/linux-apache#https-configuration)
* [使用 Nginx 的 Linux 上的主機 ASP.NET Core： HTTPS 設定](xref:host-and-deploy/linux-nginx#https-configuration)
* [如何在 IIS 上設定 SSL](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [OWASP HSTS 瀏覽器支援](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)