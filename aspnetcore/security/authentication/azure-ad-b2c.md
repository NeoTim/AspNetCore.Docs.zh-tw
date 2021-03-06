---
title: ASP.NET Core 中的 Azure Active Directory B2C 雲端驗證
author: camsoper
description: 探索如何設定 ASP.NET Core 的 Azure Active Directory B2C 驗證。
ms.author: casoper
ms.custom: devx-track-csharp, mvc
ms.date: 01/21/2019
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
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: edacded5df4d5f4819b3657bc7eff99e6d96d394
ms.sourcegitcommit: 9a90b956af8d8584d597f1e5c1dbfb0ea9bb8454
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/21/2020
ms.locfileid: "88712541"
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a>ASP.NET Core 中的 Azure Active Directory B2C 雲端驗證

作者 [Cam Soper](https://twitter.com/camsoper)

[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) 是適用于 web 和行動應用程式的雲端身分識別管理解決方案。 服務會為裝載于雲端和內部部署中的應用程式提供驗證。 驗證類型包括個別帳戶、社交網路帳戶和同盟企業帳戶。 此外，Azure AD B2C 可以使用基本設定來提供多重要素驗證。

> [!TIP]
> Azure Active Directory (Azure AD) 和 Azure AD B2C 是不同的產品供應專案。 Azure AD 租使用者代表一個組織，而 Azure AD B2C 租使用者代表一組要與信賴憑證者應用程式搭配使用的身分識別。 若要深入瞭解，請參閱 [Azure AD B2C：常見問題) 常見問題 (](/azure/active-directory-b2c/active-directory-b2c-faqs)。

在本教學課程中，您將了解如何：

> [!div class="checklist"]
> * 建立 Active Directory B2C 租用戶
> * 在 Azure AD B2C 中註冊應用程式
> * 使用 Visual Studio 建立 ASP.NET Core web 應用程式，並將其設定為使用 Azure AD B2C 租使用者進行驗證
> * 設定控制 Azure AD B2C 租使用者行為的原則

## <a name="prerequisites"></a>先決條件

本逐步解說需要下列各項：

* [Microsoft Azure 訂用帳戶](https://azure.microsoft.com/free/dotnet/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>建立 Azure Active Directory B2C 租使用者

建立 Azure Active Directory B2C 租使用者， [如檔中所述](/azure/active-directory-b2c/active-directory-b2c-get-started)。 出現提示時，請將租使用者與 Azure 訂用帳戶產生關聯，這在本教學課程中為選擇性。

## <a name="register-the-app-in-azure-ad-b2c"></a>在 Azure AD B2C 中註冊應用程式

在新建立的 Azure AD B2C 租使用者中，使用 [**註冊 web 應用程式**] 區段下[的檔中的步驟](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application)註冊您的應用程式。 在 [ **建立 web 應用程式用戶端密碼** ] 區段上停止。 本教學課程不需要用戶端密碼。 

輸入下列值：

| 設定                       | 值                     | 注意                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                      | *&lt;應用程式名稱&gt;*        | 輸入應用程式的 **名稱** ，以向取用者說明您的應用程式。                                                                                                                                 |
| **包含 Web 應用程式 / Web API** | 是                       |                                                                                                                                                                                                    |
| **允許隱含流程**       | 是                       |                                                                                                                                                                                                    |
| **回覆 URL**                 | `https://localhost:44300/signin-oidc` | 回覆 URL 是 Azure AD B2C 傳回您應用程式要求之任何權杖的所在端點。 Visual Studio 提供要使用的回復 URL。 現在，請輸入 `https://localhost:44300/signin-oidc` 來完成表單。 |
| **應用程式識別碼 URI**                | 保留空白               | 本教學課程不需要。                                                                                                                                                                    |
| **包含原生用戶端**     | 否                        |                                                                                                                                                                                                    |

> [!WARNING]
> 如果設定非 localhost 回復 URL，請留意 [[回復 url] 清單中允許的條件約束](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application)。 

註冊應用程式之後，就會顯示租使用者中的應用程式清單。 選取剛註冊的應用程式。 選取 [**應用程式識別碼**] 欄位右邊的**複製**圖示，將它複製到剪貼簿。

目前不能在 Azure AD B2C 租使用者中設定任何專案，但請將瀏覽器視窗保持開啟。 建立 ASP.NET Core 應用程式之後，會有更多設定。

## <a name="create-an-aspnet-core-app-in-visual-studio"></a>在 Visual Studio 中建立 ASP.NET Core 應用程式

Visual Studio Web 應用程式範本可以設定為使用 Azure AD B2C 租使用者進行驗證。

在 Visual Studio 中：

1. 建立新的 ASP.NET Core Web 應用程式。 
2. 從範本清單中選取 [ **Web 應用程式** ]。
3. 選取 [ **變更驗證** ] 按鈕。
    
    ![變更驗證按鈕](./azure-ad-b2c/_static/changeauth.png)

4. 在 [ **變更驗證** ] 對話方塊中，選取 [ **個別使用者帳戶**]，然後在下拉式清單中選取 [連線 **到雲端中現有的使用者存放區]** 。 
    
    ![[變更驗證] 對話方塊](./azure-ad-b2c/_static/changeauthdialog.png)

5. 以下列值填寫表單：
    
    | 設定                       | 值                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **功能變數名稱**               | *&lt;B2C 租使用者的功能變數名稱&gt;*          |
    | **應用程式識別碼**            | *&lt;貼上剪貼簿中的應用程式識別碼&gt;* |
    | **回呼路徑**             | *&lt;使用預設值&gt;*                       |
    | **註冊或登入原則** | `B2C_1_SiUpIn`                                        |
    | **重設密碼原則**     | `B2C_1_SSPR`                                          |
    | **編輯設定檔原則**       | *&lt;保留空白&gt;*                                 |
    
    選取 [**回復 uri** ] 旁的 [**複製**] 連結，將回復 uri 複製到剪貼簿。 選取 **[確定** ] 以關閉 [ **變更驗證** ] 對話方塊。 選取 **[確定]** 以建立 web 應用程式。

## <a name="finish-the-b2c-app-registration"></a>完成 B2C 應用程式註冊

返回仍開啟 B2C 應用程式屬性的瀏覽器視窗。 將稍早指定的暫時 **回復 URL** 變更為從 Visual Studio 複製的值。 選取視窗頂端的 [ **儲存** ]。

> [!TIP]
> 如果您沒有複製回復 URL，請從 Web 專案屬性的 [調試] 索引標籤使用 HTTPS 位址，然後從*appsettings.js*附加**CallbackPath**值。

## <a name="configure-policies"></a>設定原則

使用 Azure AD B2C 檔中的步驟來 [建立註冊或登入原則](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions)，然後 [建立密碼重設原則](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions)。 使用提供** Identity 者**的檔中提供的範例值、**註冊屬性**和**應用程式宣告**。 使用 [ **立即執行** ] 按鈕來測試原則（如檔中所述）是選擇性的。

> [!WARNING]
> 請確定原則名稱與檔中所述的完全相同，因為這些原則是在 Visual Studio 的 [ **變更驗證** ] 對話方塊中使用。 原則名稱可以在 *appsettings.js*中進行驗證。

## <a name="configure-the-underlying-openidconnectoptionsjwtbearerno-loccookie-options"></a>設定基礎 OpenIdConnectOptions/JwtBearer/ Cookie 選項

若要直接設定基礎選項，請在中使用適當的架構常數 `Startup.ConfigureServices` ：

```csharp
services.Configure<OpenIdConnectOptions>(
    AzureAD[B2C]Defaults.OpenIdScheme, options => 
    {
        // Omitted for brevity
    });

services.Configure<CookieAuthenticationOptions>(
    AzureAD[B2C]Defaults.CookieScheme, options => 
    {
        // Omitted for brevity
    });

services.Configure<JwtBearerOptions>(
    AzureAD[B2C]Defaults.JwtBearerAuthenticationScheme, options => 
    {
        // Omitted for brevity
    });
```

## <a name="run-the-app"></a>執行應用程式

在 Visual Studio 中，按 **F5** 以建立並執行應用程式。 在 web 應用程式啟動後，選取 [ **接受** ] 以接受使用 cookie s (如果出現提示) ，然後選取 [登 **入**]。

![登入應用程式](./azure-ad-b2c/_static/signin.png)

瀏覽器會重新導向至 Azure AD B2C 的租使用者。 使用現有的帳戶登入 (如果已建立測試原則) 或選取 [ **立即註冊** ] 以建立新的帳戶。 **忘記密碼？** link 可用來重設忘記的密碼。

![Azure AD B2C 登入](./azure-ad-b2c/_static/b2csts.png)

成功登入之後，瀏覽器會重新導向至 web 應用程式。

![成功](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已了解如何：

> [!div class="checklist"]
> * 建立 Active Directory B2C 租用戶
> * 在 Azure AD B2C 中註冊應用程式
> * 使用 Visual Studio 建立設定為使用 Azure AD B2C 租使用者進行驗證的 ASP.NET Core Web 應用程式
> * 設定控制 Azure AD B2C 租使用者行為的原則

現在 ASP.NET Core 應用程式已設定為使用 Azure AD B2C 進行驗證， [授權屬性](xref:security/authorization/simple) 可用來保護您的應用程式。 藉由學習來繼續開發您的應用程式：

* [自訂 Azure AD B2C 的使用者介面](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization)。
* [設定密碼複雜度需求](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity)。
* [啟用多重要素驗證](/azure/active-directory-b2c/active-directory-b2c-reference-mfa)。
* 設定其他身分識別提供者，例如 [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app)、 [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app)、 [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app)、 [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app)、 [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)和其他身分識別提供者。
* [使用 Azure AD 圖形 API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) 從 Azure AD B2C 租使用者中取出額外的使用者資訊，例如群組成員資格。
* [如何使用 Azure AD B2C 保護以 ASP.NET Core 建立的 WEB API](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/4-WebApp-your-API/4-2-B2C)。
* [教學課程：使用 Azure Active Directory B2C 授與 ASP.NET WEB API 的存取權](/azure/active-directory-b2c/tutorial-web-api-dotnet)。
