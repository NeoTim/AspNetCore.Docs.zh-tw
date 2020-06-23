---
title: 保護 ASP.NET Core Blazor WebAssembly
author: guardrex
description: 瞭解如何以 Blazor 單一頁面應用程式（spa）保護 WebAssemlby 應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/01/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/security/webassembly/index
ms.openlocfilehash: 877b2bb4b055cca25d64258383cdb39d812e2d6a
ms.sourcegitcommit: 066d66ea150f8aab63f9e0e0668b06c9426296fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/23/2020
ms.locfileid: "85243235"
---
# <a name="secure-aspnet-core-blazor-webassembly"></a>保護 ASP.NET Core Blazor WebAssembly

By [Javier Calvarro Nelson](https://github.com/javiercn)

BlazorWebAssembly apps 的保護方式與單一頁面應用程式（Spa）相同。 有數種方法可以向 Spa 驗證使用者，但最常見且完整的方法是使用以[OAuth 2.0 通訊協定](https://oauth.net/)為基礎的執行，例如[Open ID Connect （OIDC）](https://openid.net/connect/)。

## <a name="authentication-library"></a>驗證程式庫

BlazorWebAssembly 支援透過程式庫使用 OIDC 來驗證和授權應用程式 [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) 。 程式庫提供一組基本類型，可針對 ASP.NET Core 後端順暢地進行驗證。 程式庫整合 Identity 了 ASP.NET Core，並在[ Identity 伺服器](https://identityserver.io/)之上建立 API 授權支援。 程式庫可以針對支援 OIDC 的任何協力廠商 Identity 提供者（IP）進行驗證，稱為 OpenID 提供者（OP）。

WebAssembly 中的驗證支援 Blazor 是建置於程式庫之上 `oidc-client.js` ，用來處理基礎驗證通訊協定的詳細資料。

驗證 Spa 的其他選項是否存在，例如使用 SameSite cookie。 不過，WebAssembly 的工程設計 Blazor 是以 OAuth 和 OIDC 作為 WebAssembly 應用程式中驗證的最佳選項來進行 Blazor 。 以[JSON Web 權杖（jwt）](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html)為基礎的[權杖型驗證](xref:security/anti-request-forgery#token-based-authentication)是針對功能和安全性原因而選擇，而不是以[cookie 為基礎的驗證](xref:security/anti-request-forgery#cookie-based-authentication)：

* 使用以權杖為基礎的通訊協定提供較小的受攻擊面，因為權杖不會在所有要求中傳送。
* 伺服器端點不需要保護[跨網站偽造要求（CSRF）](xref:security/anti-request-forgery) ，因為權杖是明確傳送的。 這可讓您將 Blazor WebAssembly 應用程式與 MVC 或 Razor pages 應用程式裝載在一起。
* 權杖的許可權比 cookie 窄。 例如，除非明確地執行這類功能，否則權杖無法用來管理使用者帳戶或變更使用者的密碼。
* 權杖的存留期很短，預設為一小時，這會限制攻擊時段。 權杖也可以隨時撤銷。
* 獨立 Jwt 提供驗證程式的用戶端和伺服器保證。 例如，用戶端的方法是偵測並驗證它所收到的權杖是否合法，並在指定的驗證程式中發出。 如果協力廠商嘗試在驗證程式中途切換權杖，用戶端就可以偵測出切換的權杖，並避免使用它。
* OAuth 和 OIDC 的權杖不依賴使用者代理程式正確運作，以確保應用程式的安全。
* 以權杖為基礎的通訊協定，例如 OAuth 和 OIDC，可讓您使用相同的安全性特性集來驗證和授權託管和獨立應用程式。

## <a name="authentication-process-with-oidc"></a>使用 OIDC 的驗證程式

連結 [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) 庫提供數個基本專案，以使用 OIDC 來執行驗證和授權。 就廣義而言，驗證的運作方式如下：

* 當匿名使用者選取 [登入] 按鈕或要求已套用屬性的頁面時 [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) ，系統會將使用者重新導向至應用程式的登入頁面（ `/authentication/login` ）。
* 在登入頁面中，驗證程式庫會準備重新導向至授權端點。 授權端點位於 Blazor WebAssembly 應用程式之外，而且可以在不同的來源託管。 端點負責判斷使用者是否已驗證，以及是否要在回應中發出一或多個權杖。 驗證程式庫會提供登入回呼，以接收驗證回應。
  * 如果使用者未經過驗證，則會將使用者重新導向至基礎驗證系統，這通常是 ASP.NET Core Identity 。
  * 如果使用者已經過驗證，則授權端點會產生適當的權杖，並將瀏覽器重新導向回登入回呼端點（ `/authentication/login-callback` ）。
* 當 Blazor WebAssembly 應用程式載入登入回呼端點（ `/authentication/login-callback` ）時，就會處理驗證回應。
  * 如果驗證程式成功完成，則會驗證使用者，並選擇性地將其傳送回給使用者要求的原始受保護 URL。
  * 如果驗證程式因任何原因而失敗，則會將使用者傳送至登入失敗頁面（ `/authentication/login-failed` ），並顯示錯誤。

## <a name="authorization"></a>授權

在 Blazor WebAssembly apps 中，可以略過授權檢查，因為使用者可以修改所有的用戶端程式代碼。 這同樣也適用於所有的用戶端應用程式技術，包括 JavaScript SPA 架構或任何作業系統的原生應用程式。

**請一律在由您用戶端應用程式所存取之任何 API 端點內的伺服器上執行授權檢查。**

## <a name="refresh-tokens"></a>重新整理權杖

重新整理權杖無法在 WebAssembly apps 的用戶端上受到保護 Blazor 。 因此，不應將重新整理權杖傳送至應用程式，以供直接使用。

在託管的 WebAssembly 解決方案中，伺服器端應用程式可以維護及使用重新整理權杖 Blazor 來存取協力廠商 api。 如需詳細資訊，請參閱 <xref:blazor/security/webassembly/additional-scenarios#authenticate-users-with-a-third-party-provider-and-call-protected-apis-on-the-host-server-and-the-third-party>。

## <a name="implementation-guidance"></a>實作指引

此*總覽*底下的文章提供有關在 Blazor WebAssembly apps 中針對特定提供者驗證使用者的資訊。

獨立 Blazor WebAssembly 應用程式：

* [OIDC 提供者和 WebAssembly Authentication 程式庫的一般指引](xref:blazor/security/webassembly/standalone-with-authentication-library)
* [Microsoft 帳戶](xref:blazor/security/webassembly/standalone-with-microsoft-accounts)
* [Azure Active Directory (AAD)](xref:blazor/security/webassembly/standalone-with-azure-active-directory)
* [Azure Active Directory （AAD） B2C](xref:blazor/security/webassembly/standalone-with-azure-active-directory-b2c)

託管的 Blazor WebAssembly 應用程式：

* [Azure Active Directory (AAD)](xref:blazor/security/webassembly/hosted-with-azure-active-directory)
* [Azure Active Directory （AAD） B2C](xref:blazor/security/webassembly/hosted-with-azure-active-directory-b2c)
* [Identity伺服器](xref:blazor/security/webassembly/hosted-with-identity-server)

如需設定的進一步指引，請參閱 <xref:blazor/security/webassembly/additional-scenarios> 。