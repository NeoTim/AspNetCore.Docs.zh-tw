---
title: Blazor WebAssembly使用 Azure Active Directory 保護 ASP.NET Core 託管應用程式
author: guardrex
description: 瞭解如何使用 Azure Active Directory 保護 ASP.NET Core Blazor WebAssembly 託管應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: devx-track-csharp, mvc
ms.date: 10/08/2020
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
uid: blazor/security/webassembly/hosted-with-azure-active-directory
ms.openlocfilehash: e6f514793a2efde120f70ac58f4ad4be7516ada7
ms.sourcegitcommit: daa9ccf580df531254da9dce8593441ac963c674
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "91900832"
---
# <a name="secure-an-aspnet-core-no-locblazor-webassembly-hosted-app-with-azure-active-directory"></a>Blazor WebAssembly使用 Azure Active Directory 保護 ASP.NET Core 託管應用程式

由 [Javier Calvarro Nelson](https://github.com/javiercn) 和 [Luke Latham](https://github.com/guardrex)

本文說明如何建立使用[Azure Active Directory (AAD) ](https://azure.microsoft.com/services/active-directory/)進行驗證的[託管 Blazor WebAssembly 應用程式](xref:blazor/hosting-models#blazor-webassembly)。

## <a name="register-apps-in-aad-and-create-solution"></a>在 AAD 中註冊應用程式並建立解決方案

### <a name="create-a-tenant"></a>建立租用戶

遵循快速入門中的指導方針 [：設定租](/azure/active-directory/develop/quickstart-create-new-tenant) 使用者以在 AAD 中建立租使用者。

### <a name="register-a-server-api-app"></a>註冊伺服器 API 應用程式

遵循快速入門中的指導方針 [：使用 Microsoft 身分識別平臺註冊應用程式](/azure/active-directory/develop/quickstart-register-app) ，以及後續的 Azure AAD 主題以註冊 *伺服器 API 應用* 程式的 AAD 應用程式，然後執行下列動作：

1. 在**Azure Active Directory**  >  **應用程式註冊**中，選取 [**新增註冊**]。
1. 提供應用程式的**名稱** (例如， ** Blazor Server AAD**) 。
1. 選擇 **支援的帳戶類型**。 您可以選取 **此組織目錄中的帳戶，只** (此體驗的單一租使用者) 。
1. 在此案例中， *伺服器 API 應用程式* 不需要重新 **導向 uri** ，因此請將下拉式清單保持設定為 [ **Web** ]，而不要輸入重新導向 uri。
1. 清除 [授**與系統**  >  **管理員同意 openid 和 offline_access 許可權**] 核取方塊。
1. 選取 [註冊]。

記錄下列資訊：

* *伺服器 API 應用程式* 應用程式 (用戶端) 識別碼 (例如 `41451fa7-82d9-4673-8fa5-69eff5a761fd`) 
* 目錄 (租使用者) 識別碼 (例如 `e86c78e2-8bb4-4c41-aefd-918e0565a45e`) 
* AAD 主要/發行者/租使用者網域 (例如 `contoso.onmicrosoft.com`) ：網域可作為已註冊應用程式之 Azure 入口網站的 [**商標**] 分頁中的**發行者網域**。

在 [ **API 許可權**] 中，移除**Microsoft Graph**的  >  **使用者。讀取**許可權，因為應用程式不需要登入或使用者設定檔存取權。

在中 **公開 API**：

1. 選取 [新增範圍]  。
1. 選取 [儲存並繼續]  。
1. 提供 **範圍名稱** (例如 `API.Access`) 。
1. 提供系統 **管理員同意顯示名稱** (例如 `Access API`) 。
1. 提供 **管理員同意描述** (例如 `Allows the app to access server app API endpoints.`) 。
1. 確認 **狀態** 設定為 [ **已啟用**]。
1. 選取 [新增範圍]。

記錄下列資訊：

* 應用程式識別碼 URI (例如， `api://41451fa7-82d9-4673-8fa5-69eff5a761fd` 、 `https://contoso.onmicrosoft.com/41451fa7-82d9-4673-8fa5-69eff5a761fd` 或您提供的自訂值) 
* 範圍名稱 (例如， `API.Access`) 

### <a name="register-a-client-app"></a>註冊用戶端應用程式

遵循快速入門中的指導方針 [：使用 Microsoft 身分識別平臺註冊應用程式](/azure/active-directory/develop/quickstart-register-app) ，並使用後續的 Azure AAD 主題來為應用程式註冊 AAD 應用程式 *`Client`* ，然後執行下列動作：

::: moniker range=">= aspnetcore-5.0"

1. 在 **Azure Active Directory** > **應用程式註冊**中，選取 [ **新增註冊**]。
1. 提供應用程式的**名稱** (例如， ** Blazor 用戶端 AAD**) 。
1. 選擇 **支援的帳戶類型**。 您可以選取 **此組織目錄中的帳戶，只** (此體驗的單一租使用者) 。
1. 將 [重新 **導向 uri** ] 下拉式清單設定為 **單一頁面應用程式 (SPA) ** 並提供下列重新導向 uri： `https://localhost:{PORT}/authentication/login-callback` 。 在 Kestrel 上執行之應用程式的預設埠是5001。 如果應用程式是在不同的 Kestrel 埠上執行，請使用應用程式的埠。 針對 IIS Express，可在 [ *`Server`* **調試** 程式] 面板的應用程式屬性中找到應用程式隨機產生的埠。 由於應用程式目前不存在，且 IIS Express 埠未知，因此在建立應用程式之後，請返回此步驟，並更新重新導向 URI。 批註會出現在 [ [建立應用程式](#create-the-app) ] 區段中，以提醒 IIS Express 使用者更新重新導向 URI。
1. 清除 [授 **與系統** > **管理員同意 openid 和 offline_access 許可權** ] 核取方塊。
1. 選取 [註冊]。

記錄 *`Client`* 應用程式應用程式 (用戶端) 識別碼 (例如 `4369008b-21fa-427c-abaa-9b53bf58e538`) 。

在「**驗證**平臺設定」的 > **Platform configurations** > **單一頁面應用程式中， (SPA) **：

1. 確認的重新 **導向 URI** `https://localhost:{PORT}/authentication/login-callback` 存在。
1. 針對**隱含授**與，請確定**未**選取**存取權杖**和**識別碼權杖**的核取方塊。
1. 此體驗可接受應用程式的其餘預設值。
1. 選取 [儲存] 按鈕。

::: moniker-end

::: moniker range="< aspnetcore-5.0"

1. 在 **Azure Active Directory** > **應用程式註冊**中，選取 [ **新增註冊**]。
1. 提供應用程式的**名稱** (例如， ** Blazor 用戶端 AAD**) 。
1. 選擇 **支援的帳戶類型**。 您可以選取 **此組織目錄中的帳戶，只** (此體驗的單一租使用者) 。
1. 將 [重新 **導向 URI** ] 下拉式清單保持設定為 [ **Web** ]，並提供下列重新導向 uri： `https://localhost:{PORT}/authentication/login-callback` 。 在 Kestrel 上執行之應用程式的預設埠是5001。 如果應用程式是在不同的 Kestrel 埠上執行，請使用應用程式的埠。 針對 IIS Express，可在 [ *`Server`* **調試** 程式] 面板的應用程式屬性中找到應用程式隨機產生的埠。 由於應用程式目前不存在，且 IIS Express 埠未知，因此在建立應用程式之後，請返回此步驟，並更新重新導向 URI。 批註會出現在 [ [建立應用程式](#create-the-app) ] 區段中，以提醒 IIS Express 使用者更新重新導向 URI。
1. 清除 [授 **與系統** > **管理員同意 openid 和 offline_access 許可權** ] 核取方塊。
1. 選取 [註冊]。

記錄 *`Client`* 應用程式應用程式 (用戶端) 識別碼 (例如 `4369008b-21fa-427c-abaa-9b53bf58e538`) 。

在 [ **驗證** > **平臺**設定] > **Web**：

1. 確認的重新 **導向 URI** `https://localhost:{PORT}/authentication/login-callback` 存在。
1. 針對 **[隱含授**與]，選取 **存取權杖** 和 **識別碼權杖**的核取方塊。
1. 此體驗可接受應用程式的其餘預設值。
1. 選取 [儲存] 按鈕。

::: moniker-end

在 **API 許可權**中：

1. 確認應用程式已**Microsoft Graph**  >  **使用者。讀取**許可權。
1. 選取 [ **新增] 許可權** ，然後選取 [我的 **api**]。
1. 從 [**名稱**] 資料行中選取*伺服器 API 應用程式* (例如， ** Blazor Server AAD**) 。
1. 開啟 **API** 清單。
1. 啟用對 API 的存取 (例如 `API.Access`) 。
1. 選取 [新增權限]。
1. 選取 [ **授與系統管理員同意 {租使用者名稱}** ] 按鈕。 選取 [是]  確認。

### <a name="create-the-app"></a>建立應用程式

在空的資料夾中，將下列命令中的預留位置取代為先前記錄的資訊，然後在命令 shell 中執行命令：

```dotnetcli
dotnet new blazorwasm -au SingleOrg --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{SERVER API APP ID URI}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{TENANT DOMAIN}" -ho -o {APP NAME} --tenant-id "{TENANT ID}"
```

| 預留位置                  | Azure 入口網站名稱                                     | 範例                                      |
| ---------------------------- | ----------------------------------------------------- | -------------------------------------------- |
| `{APP NAME}`                 | &mdash;                                               | `BlazorSample`                               |
| `{CLIENT APP CLIENT ID}`     | 應用程式 (用戶端) 的應用程式識別碼 *`Client`*        | `4369008b-21fa-427c-abaa-9b53bf58e538`       |
| `{DEFAULT SCOPE}`            | 範圍名稱                                            | `API.Access`                                 |
| `{SERVER API APP CLIENT ID}` | *伺服器 API 應用*程式 (用戶端) 識別碼      | `41451fa7-82d9-4673-8fa5-69eff5a761fd`       |
| `{SERVER API APP ID URI}`    | 應用程式識別碼 URI                                    | `api://41451fa7-82d9-4673-8fa5-69eff5a761fd` |
| `{TENANT DOMAIN}`            | 主要/發行者/租使用者網域                       | `contoso.onmicrosoft.com`                    |
| `{TENANT ID}`                | 目錄 (租用戶) 識別碼                                 | `e86c78e2-8bb4-4c41-aefd-918e0565a45e`       |

使用選項指定的輸出位置會 `-o|--output` 建立專案資料夾（如果不存在），而且會成為應用程式名稱的一部分。

::: moniker range=">= aspnetcore-5.0"

> [!NOTE]
> 使用具有未驗證發行者網域的 Azure 租使用者時，可能需要進行設定變更，如 [應用程式設定](#app-settings) 一節中所述。

::: moniker-end

::: moniker range="< aspnetcore-5.0"

> [!NOTE]
> 使用具有未驗證發行者網域的 Azure 租使用者時，可能需要進行設定變更，如 [存取權杖範圍](#access-token-scopes) 一節中所述。

::: moniker-end

> [!NOTE]
> 在 Azure 入口網站中， *`Client`* 會針對使用預設設定在 Kestrel 伺服器上執行的應用程式，將應用程式的平臺設定重新 **導向 URI** 設定為使用埠5001。
>
> 如果 *`Client`* 應用程式是在隨機的 IIS Express 埠上執行，則可以在 [**調試**程式] 面板中的*伺服器 API 應用程式*屬性中找到應用程式的埠。
>
> 如果先前未使用 *`Client`* 應用程式的已知埠設定埠，請返回 *`Client`* Azure 入口網站中的應用程式註冊，然後以正確的埠更新重新導向 URI。

## <a name="server-app-configuration"></a>*`Server`* 應用程式設定

*本節適用于解決方案的 **`Server`** 應用程式。*

### <a name="authentication-package"></a>驗證套件

::: moniker range=">= aspnetcore-5.0"

使用 Microsoft 平臺對 ASP.NET Core Web Api 進行驗證和授權呼叫的支援 Identity 是由下列套件提供：

* [`Microsoft.Identity.Web`](https://www.nuget.org/packages/Microsoft.Identity.Web)
* [`Microsoft.Identity.Web.UI`](https://www.nuget.org/packages/Microsoft.Identity.Web.UI)

```xml
<PackageReference Include="Microsoft.Identity.Web" Version="{VERSION}" />
<PackageReference Include="Microsoft.Identity.Web.UI" Version="{VERSION}" />
```

針對預留位置 `{VERSION}` ，可以在套件的 **版本歷程記錄** （NuGet.org）中找到符合應用程式共用架構版本的最新穩定版本套件。

::: moniker-end

::: moniker range="< aspnetcore-5.0"

對 ASP.NET Core Web Api 的驗證和授權呼叫支援是由 [`Microsoft.AspNetCore.Authentication.AzureAD.UI`](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.AzureAD.UI) 套件提供：

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureAD.UI" 
  Version="{VERSION}" />
```

針對預留位置 `{VERSION}` ，可以在套件的 **版本歷程記錄** （ [NuGet.org](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.AzureAD.UI)）中找到符合應用程式共用架構版本的最新穩定版本套件。

::: moniker-end

### <a name="authentication-service-support"></a>驗證服務支援

::: moniker range=">= aspnetcore-5.0"

方法會在 `AddAuthentication` 應用程式中設定驗證服務，並將 JWT 持有人處理常式設定為預設驗證方法。 方法會設定 <xref:Microsoft.Identity.Web.MicrosoftIdentityWebApiAuthenticationBuilderExtensions.AddMicrosoftIdentityWebApi%2A> 服務，以使用 Microsoft Platform v2.0 來保護 WEB API Identity 。 此方法需要 `AzureAd` 應用程式設定中的區段，並具有初始化驗證選項所需的設定。

```csharp
services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddMicrosoftIdentityWebApi(Configuration.GetSection("AzureAd"));
```

::: moniker-end

::: moniker range="< aspnetcore-5.0"

方法會在 `AddAuthentication` 應用程式中設定驗證服務，並將 JWT 持有人處理常式設定為預設驗證方法。 <xref:Microsoft.AspNetCore.Authentication.AzureADAuthenticationBuilderExtensions.AddAzureADBearer%2A>方法會設定 JWT 持有人處理常式中的特定參數，以驗證 Azure Active Directory 所發出的權杖：

```csharp
services.AddAuthentication(AzureADDefaults.BearerAuthenticationScheme)
    .AddAzureADBearer(options => Configuration.Bind("AzureAd", options));
```

::: moniker-end

<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication%2A> 並 <xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization%2A> 確定：

* 應用程式會嘗試剖析並驗證傳入要求的權杖。
* 任何嘗試在沒有適當認證的情況下存取受保護資源的要求都會失敗。

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="userno-locidentityname"></a>使用者 ... Identity名字

根據預設， *`Server`* 應用程式 API 會 `User.Identity.Name` 以宣告類型的值填入 `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` (例如 `2d64b3da-d9d5-42c6-9352-53d8df33d770@contoso.onmicrosoft.com`) 。

若要將應用程式設定為接收來自宣告 `name` 類型的值，請 <xref:Microsoft.IdentityModel.Tokens.TokenValidationParameters.NameClaimType?displayProperty=nameWithType> <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerOptions> 在中設定 `Startup.ConfigureServices` ：

```csharp
using Microsoft.AspNetCore.Authentication.JwtBearer;

...

services.Configure<JwtBearerOptions>(
    AzureADDefaults.JwtBearerAuthenticationScheme, options =>
    {
        options.TokenValidationParameters.NameClaimType = "name";
    });
```

### <a name="app-settings"></a>應用程式設定

::: moniker range=">= aspnetcore-5.0"

檔案 `appsettings.json` 包含設定用來驗證存取權杖之 JWT 持有人處理常式的選項：

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "{DOMAIN}",
    "TenantId": "{TENANT ID}",
    "ClientId": "{SERVER API APP CLIENT ID}",
    "CallbackPath": "/signin-oidc"
  }
}
```

範例：

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "contoso.onmicrosoft.com",
    "TenantId": "e86c78e2-8bb4-4c41-aefd-918e0565a45e",
    "ClientId": "41451fa7-82d9-4673-8fa5-69eff5a761fd",
    "CallbackPath": "/signin-oidc"
  }
}
```

[!INCLUDE[](~/includes/blazor-security/azure-scope-5x.md)]

::: moniker-end

::: moniker range="< aspnetcore-5.0"

檔案 `appsettings.json` 包含設定用來驗證存取權杖之 JWT 持有人處理常式的選項：

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "{DOMAIN}",
    "TenantId": "{TENANT ID}",
    "ClientId": "{SERVER API APP CLIENT ID}",
  }
}
```

範例：

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "contoso.onmicrosoft.com",
    "TenantId": "e86c78e2-8bb4-4c41-aefd-918e0565a45e",
    "ClientId": "41451fa7-82d9-4673-8fa5-69eff5a761fd",
  }
}
```

::: moniker-end

### <a name="weatherforecast-controller"></a>WeatherForecast 控制器

WeatherForecast 控制器 (*控制器/WeatherForecastController*) 會公開受保護的 API，並將 [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) 屬性套用至控制器。 請 **務必** 瞭解：

* [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)此 api 控制器中的屬性是保護此 api 不會遭到未經授權存取的唯一做法。
* [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)應用程式中使用的屬性 Blazor WebAssembly 僅做為應用程式的提示，使用者應獲得授權才能讓應用程式正常運作。

```csharp
[Authorize]
[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
{
    [HttpGet]
    public IEnumerable<WeatherForecast> Get()
    {
        ...
    }
}
```

## <a name="client-app-configuration"></a>*`Client`* 應用程式設定

*本節適用于解決方案的 **`Client`** 應用程式。*

### <a name="authentication-package"></a>驗證套件

建立應用程式以使用 () 的公司或學校帳戶時 `SingleOrg` ，應用程式會自動收到 [Microsoft 驗證程式庫](/azure/active-directory/develop/msal-overview) 的套件參考 ([`Microsoft.Authentication.WebAssembly.Msal`](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal)) 。 此套件提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。

如果將驗證新增至應用程式，請手動將套件新增到應用程式的專案檔：

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
  Version="{VERSION}" />
```

針對預留位置 `{VERSION}` ，可以在套件的 **版本歷程記錄** （ [NuGet.org](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal)）中找到符合應用程式共用架構版本的最新穩定版本套件。

[`Microsoft.Authentication.WebAssembly.Msal`](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal)封裝可傳遞將 [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication) 套件新增至應用程式。

### <a name="authentication-service-support"></a>驗證服務支援

在對 <xref:System.Net.Http.HttpClient> 伺服器專案提出要求時，會加入包含存取權杖的實例支援。

`Program.cs`:

```csharp
builder.Services.AddHttpClient("{APP ASSEMBLY}.ServerAPI", client => 
        client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress))
    .AddHttpMessageHandler<BaseAddressAuthorizationMessageHandler>();

builder.Services.AddScoped(sp => sp.GetRequiredService<IHttpClientFactory>()
    .CreateClient("{APP ASSEMBLY}.ServerAPI"));
```

預留位置 `{APP ASSEMBLY}` 是應用程式的元件名稱 (例如 `BlazorSample.ServerAPI`) 。

驗證使用者的支援是在服務容器中註冊，並具有 <xref:Microsoft.Extensions.DependencyInjection.MsalWebAssemblyServiceCollectionExtensions.AddMsalAuthentication%2A> 由封裝提供的擴充方法 [`Microsoft.Authentication.WebAssembly.Msal`](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal) 。 此方法會設定應用程式 Identity (IP) 與提供者互動所需的服務。

`Program.cs`:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    builder.Configuration.Bind("AzureAd", options.ProviderOptions.Authentication);
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

<xref:Microsoft.Extensions.DependencyInjection.MsalWebAssemblyServiceCollectionExtensions.AddMsalAuthentication%2A>方法會接受回呼來設定驗證應用程式所需的參數。 當您註冊應用程式時，可以從 Azure 入口網站 AAD 設定取得設定應用程式所需的值。

設定是由檔案提供 `wwwroot/appsettings.json` ：

```json
{
  "AzureAd": {
    "Authority": "https://login.microsoftonline.com/{TENANT ID}",
    "ClientId": "{CLIENT APP CLIENT ID}",
    "ValidateAuthority": true
  }
}
```

範例：

```json
{
  "AzureAd": {
    "Authority": "https://login.microsoftonline.com/e86c78e2-...-918e0565a45e",
    "ClientId": "4369008b-21fa-427c-abaa-9b53bf58e538",
    "ValidateAuthority": true
  }
}
```

### <a name="access-token-scopes"></a>存取權杖範圍

預設存取權杖範圍代表存取權杖範圍的清單，這些是：

* 預設包含在登入要求中。
* 用來在驗證之後立即布建存取權杖。

所有範圍都必須屬於相同的應用程式（依 Azure Active Directory 規則）。 您可以視需要為其他 API 應用程式新增其他範圍：

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

使用下列內容指定其他範圍 `AdditionalScopesToConsent` ：

```csharp
options.ProviderOptions.AdditionalScopesToConsent.Add("{ADDITIONAL SCOPE URI}");
```

::: moniker range="< aspnetcore-5.0"

[!INCLUDE[](~/includes/blazor-security/azure-scope-3x.md)]

::: moniker-end

如需詳細資訊，請參閱下列 *其他案例* 文章的各節：

* [要求其他存取權杖](xref:blazor/security/webassembly/additional-scenarios#request-additional-access-tokens)
* [將權杖附加至傳出要求](xref:blazor/security/webassembly/additional-scenarios#attach-tokens-to-outgoing-requests)

::: moniker range=">= aspnetcore-5.0"

### <a name="login-mode"></a>登入模式

[!INCLUDE[](~/includes/blazor-security/msal-login-mode.md)]

::: moniker-end

### <a name="imports-file"></a>匯入檔案

[!INCLUDE[](~/includes/blazor-security/imports-file-hosted.md)]

### <a name="index-page"></a>索引頁面

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

### <a name="app-component"></a>應用程式元件

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a>RedirectToLogin 元件

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a>LoginDisplay 元件

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

### <a name="authentication-component"></a>驗證元件

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a>FetchData 元件

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a>執行應用程式

從伺服器專案執行應用程式。 使用 Visual Studio 時，您可以：

* 將工具列中的 [ **啟始專案** ] 下拉式清單設定為 *伺服器 API 應用程式* ，然後選取 [ **執行** ] 按鈕。
* 在 **方案總管** 中選取伺服器專案，然後選取工具列中的 [ **執行** ] 按鈕，或從 [ **調試** 程式] 功能表啟動應用程式。

<!-- HOLD
[!INCLUDE[](~/includes/blazor-security/usermanager-signinmanager.md)]
-->

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a>其他資源

* <xref:blazor/security/webassembly/additional-scenarios>
* [具有安全預設用戶端之應用程式中未經驗證或未經授權的 web API 要求](xref:blazor/security/webassembly/additional-scenarios#unauthenticated-or-unauthorized-web-api-requests-in-an-app-with-a-secure-default-client)
* <xref:blazor/security/webassembly/aad-groups-roles>
* <xref:security/authentication/azure-active-directory/index>
* [Microsoft 身分識別平台文件](/azure/active-directory/develop/)
