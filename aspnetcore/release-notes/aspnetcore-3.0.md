---
title: 3\.0 ASP.NET Core 的新功能
author: rick-anderson
description: 深入瞭解 ASP.NET Core 3.0 中的新功能。
ms.author: riande
ms.custom: mvc
ms.date: 09/26/2019
uid: aspnetcore-3.0
ms.openlocfilehash: c1b61fee7264b972c70dbfa8f1461e33e3645746
ms.sourcegitcommit: e644258c95dd50a82284f107b9bf3becbc43b2b2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317651"
---
# <a name="whats-new-in-aspnet-core-30"></a><span data-ttu-id="30dfe-103">3\.0 ASP.NET Core 的新功能</span><span class="sxs-lookup"><span data-stu-id="30dfe-103">What's new in ASP.NET Core 3.0</span></span>

<span data-ttu-id="30dfe-104">本文將重點放在 ASP.NET Core 3.0 中最重要的變更，並提供相關檔的連結。</span><span class="sxs-lookup"><span data-stu-id="30dfe-104">This article highlights the most significant changes in ASP.NET Core 3.0 with links to relevant documentation.</span></span>

## <a name="blazor"></a><span data-ttu-id="30dfe-105">Blazor</span><span class="sxs-lookup"><span data-stu-id="30dfe-105">Blazor</span></span>

<span data-ttu-id="30dfe-106">Blazor 是 ASP.NET Core 中的新架構，可讓您使用 .NET 建立互動式用戶端 web UI：</span><span class="sxs-lookup"><span data-stu-id="30dfe-106">Blazor is a new framework in ASP.NET Core for building interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="30dfe-107">使用 C# 而不是 JavaScript 來建立豐富的互動式 UI。</span><span class="sxs-lookup"><span data-stu-id="30dfe-107">Create rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="30dfe-108">共用以 .NET 撰寫的伺服器端與用戶端應用程式邏輯。</span><span class="sxs-lookup"><span data-stu-id="30dfe-108">Share server-side and client-side app logic written in .NET.</span></span>
* <span data-ttu-id="30dfe-109">將 UI 轉譯為 HTML 和 CSS 以支援寬瀏覽器，包括行動裝置瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="30dfe-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="30dfe-110">Blazor framework 支援的案例：</span><span class="sxs-lookup"><span data-stu-id="30dfe-110">Blazor framework supported scenarios:</span></span>

* <span data-ttu-id="30dfe-111">可重複使用的 UI 元件（Razor 元件）</span><span class="sxs-lookup"><span data-stu-id="30dfe-111">Reusable UI components (Razor components)</span></span>
* <span data-ttu-id="30dfe-112">用戶端路由</span><span class="sxs-lookup"><span data-stu-id="30dfe-112">Client-side routing</span></span>
* <span data-ttu-id="30dfe-113">元件版面配置</span><span class="sxs-lookup"><span data-stu-id="30dfe-113">Component layouts</span></span>
* <span data-ttu-id="30dfe-114">支援相依性插入</span><span class="sxs-lookup"><span data-stu-id="30dfe-114">Support for dependency injection</span></span>
* <span data-ttu-id="30dfe-115">表單和驗證</span><span class="sxs-lookup"><span data-stu-id="30dfe-115">Forms and validation</span></span>
* <span data-ttu-id="30dfe-116">使用 Razor 類別庫建立元件程式庫</span><span class="sxs-lookup"><span data-stu-id="30dfe-116">Build component libraries with Razor class libraries</span></span>
* <span data-ttu-id="30dfe-117">JavaScript Interop</span><span class="sxs-lookup"><span data-stu-id="30dfe-117">JavaScript interop</span></span>

<span data-ttu-id="30dfe-118">如需詳細資訊，請參閱<xref:blazor/index>。</span><span class="sxs-lookup"><span data-stu-id="30dfe-118">For more information, see <xref:blazor/index>.</span></span>

### <a name="blazor-server"></a><span data-ttu-id="30dfe-119">Blazor 伺服器</span><span class="sxs-lookup"><span data-stu-id="30dfe-119">Blazor Server</span></span>

<span data-ttu-id="30dfe-120">Blazor 會將元件轉譯邏輯與套用 UI 更新的方式分隔。</span><span class="sxs-lookup"><span data-stu-id="30dfe-120">Blazor decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="30dfe-121">Blazor 伺服器提供在 ASP.NET Core 應用程式中將 Razor 元件裝載在伺服器上的支援。</span><span class="sxs-lookup"><span data-stu-id="30dfe-121">Blazor Server provides support for hosting Razor components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="30dfe-122">UI 更新透過 SignalR 連線處理。</span><span class="sxs-lookup"><span data-stu-id="30dfe-122">UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="30dfe-123">ASP.NET Core 3.0 支援 Blazor 伺服器。</span><span class="sxs-lookup"><span data-stu-id="30dfe-123">Blazor Server is supported in ASP.NET Core 3.0.</span></span>

### <a name="blazor-webassembly-preview"></a><span data-ttu-id="30dfe-124">Blazor WebAssembly （預覽）</span><span class="sxs-lookup"><span data-stu-id="30dfe-124">Blazor WebAssembly (Preview)</span></span>

<span data-ttu-id="30dfe-125">Blazor apps 也可以直接在瀏覽器中使用以 WebAssembly 為基礎的 .NET 執行時間來執行。</span><span class="sxs-lookup"><span data-stu-id="30dfe-125">Blazor apps can also be run directly in the browser using a WebAssembly-based .NET runtime.</span></span> <span data-ttu-id="30dfe-126">Blazor WebAssembly 處於預覽狀態，ASP.NET Core 3.0*不*支援。</span><span class="sxs-lookup"><span data-stu-id="30dfe-126">Blazor WebAssembly is in preview and *not* supported in ASP.NET Core 3.0.</span></span> <span data-ttu-id="30dfe-127">ASP.NET Core 的未來版本將會支援 Blazor WebAssembly。</span><span class="sxs-lookup"><span data-stu-id="30dfe-127">Blazor WebAssembly will be supported in a future release of ASP.NET Core.</span></span>

### <a name="razor-components"></a><span data-ttu-id="30dfe-128">Razor 元件</span><span class="sxs-lookup"><span data-stu-id="30dfe-128">Razor components</span></span>

<span data-ttu-id="30dfe-129">Blazor 應用程式是從元件建立而成。</span><span class="sxs-lookup"><span data-stu-id="30dfe-129">Blazor apps are built from components.</span></span> <span data-ttu-id="30dfe-130">元件是獨立的使用者介面（UI）區塊，例如頁面、對話方塊或表單。</span><span class="sxs-lookup"><span data-stu-id="30dfe-130">Components are self-contained chunks of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="30dfe-131">元件是定義 UI 呈現邏輯和用戶端事件處理常式的一般 .NET 類別。</span><span class="sxs-lookup"><span data-stu-id="30dfe-131">Components are normal .NET classes that define UI rendering logic and client-side event handlers.</span></span> <span data-ttu-id="30dfe-132">您可以建立豐富的互動式 web 應用程式，而不需要 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="30dfe-132">You can create rich interactive web apps without JavaScript.</span></span>

<span data-ttu-id="30dfe-133">Blazor 中的元件通常是使用 Razor 語法（這是 HTML 和C#的自然 blend）來撰寫。</span><span class="sxs-lookup"><span data-stu-id="30dfe-133">Components in Blazor are typically authored using Razor syntax, a natural blend of HTML and C#.</span></span> <span data-ttu-id="30dfe-134">Razor 元件與 Razor Pages 和 MVC 的觀點相似，因為它們都使用 Razor。</span><span class="sxs-lookup"><span data-stu-id="30dfe-134">Razor components are similar to Razor Pages and MVC views in that they both use Razor.</span></span> <span data-ttu-id="30dfe-135">不同于以要求-回應模型為基礎的頁面和視圖，元件是專門用來處理 UI 組合。</span><span class="sxs-lookup"><span data-stu-id="30dfe-135">Unlike pages and views, which are based on a request-response model, components are used specifically for handling UI composition.</span></span>

## <a name="grpc"></a><span data-ttu-id="30dfe-136">gRPC</span><span class="sxs-lookup"><span data-stu-id="30dfe-136">gRPC</span></span>

<span data-ttu-id="30dfe-137">[gRPC](https://grpc.io/)：</span><span class="sxs-lookup"><span data-stu-id="30dfe-137">[gRPC](https://grpc.io/):</span></span>

* <span data-ttu-id="30dfe-138">是一種熱門、高效能的 RPC （遠端程序呼叫）架構。</span><span class="sxs-lookup"><span data-stu-id="30dfe-138">Is a popular, high-performance RPC (remote procedure call) framework.</span></span>
* <span data-ttu-id="30dfe-139">提供固定合約優先的 API 開發方法。</span><span class="sxs-lookup"><span data-stu-id="30dfe-139">Offers an opinionated contract-first approach to API development.</span></span>
* <span data-ttu-id="30dfe-140">使用現代化技術，例如：</span><span class="sxs-lookup"><span data-stu-id="30dfe-140">Uses modern technologies such as:</span></span>

  * <span data-ttu-id="30dfe-141">用於傳輸的 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="30dfe-141">HTTP/2 for transport.</span></span>
  * <span data-ttu-id="30dfe-142">通訊協定緩衝區，做為介面描述語言。</span><span class="sxs-lookup"><span data-stu-id="30dfe-142">Protocol Buffers as the interface description language.</span></span>
  * <span data-ttu-id="30dfe-143">二進位序列化格式。</span><span class="sxs-lookup"><span data-stu-id="30dfe-143">Binary serialization format.</span></span>
* <span data-ttu-id="30dfe-144">提供下列功能：</span><span class="sxs-lookup"><span data-stu-id="30dfe-144">Provides features such as:</span></span>

  * <span data-ttu-id="30dfe-145">驗證</span><span class="sxs-lookup"><span data-stu-id="30dfe-145">Authentication</span></span>
  * <span data-ttu-id="30dfe-146">雙向串流和流量控制。</span><span class="sxs-lookup"><span data-stu-id="30dfe-146">Bidirectional streaming and flow control.</span></span>
  * <span data-ttu-id="30dfe-147">取消和超時。</span><span class="sxs-lookup"><span data-stu-id="30dfe-147">Cancellation and timeouts.</span></span>

<span data-ttu-id="30dfe-148">ASP.NET Core 3.0 中的 gRPC 功能包括：</span><span class="sxs-lookup"><span data-stu-id="30dfe-148">gRPC functionality in ASP.NET Core 3.0 includes:</span></span>

* <span data-ttu-id="30dfe-149">[Grpc. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) &ndash;用來裝載 Grpc 服務的 ASP.NET Core 架構。</span><span class="sxs-lookup"><span data-stu-id="30dfe-149">[Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) &ndash; An ASP.NET Core framework for hosting gRPC services.</span></span> <span data-ttu-id="30dfe-150">ASP.NET Core 上的 gRPC 與標準 ASP.NET Core 功能整合，例如記錄、相依性插入（DI）、驗證和授權。</span><span class="sxs-lookup"><span data-stu-id="30dfe-150">gRPC on ASP.NET Core integrates with standard ASP.NET Core features like logging, dependency injection (DI), authentication and authorization.</span></span>
* <span data-ttu-id="30dfe-151">[Grpc .net. 用戶端](https://www.nuget.org/packages/Grpc.Net.Client) &ndash; ： .net Core 的 Grpc 用戶端，建基於`HttpClient`熟悉的。</span><span class="sxs-lookup"><span data-stu-id="30dfe-151">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) &ndash; A gRPC client for .NET Core that builds upon the familiar `HttpClient`.</span></span>
* <span data-ttu-id="30dfe-152">[Grpc .net. ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) &ndash; Grpc 用戶端與`HttpClientFactory`整合。</span><span class="sxs-lookup"><span data-stu-id="30dfe-152">[Grpc.Net.ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) &ndash; gRPC client integration with `HttpClientFactory`.</span></span>

<span data-ttu-id="30dfe-153">如需詳細資訊，請參閱<xref:grpc/index>。</span><span class="sxs-lookup"><span data-stu-id="30dfe-153">For more information, see <xref:grpc/index>.</span></span>

## <a name="signalr"></a><span data-ttu-id="30dfe-154">SignalR</span><span class="sxs-lookup"><span data-stu-id="30dfe-154">SignalR</span></span>

<span data-ttu-id="30dfe-155">如需遷移指示，請參閱[更新 SignalR 程式碼](xref:migration/22-to-30#signalr)。</span><span class="sxs-lookup"><span data-stu-id="30dfe-155">See [Update SignalR code](xref:migration/22-to-30#signalr) for migration instructions.</span></span> <span data-ttu-id="30dfe-156">SignalR 現在會`System.Text.Json`使用來序列化/還原序列化 JSON 訊息。</span><span class="sxs-lookup"><span data-stu-id="30dfe-156">SignalR now uses `System.Text.Json` to serialize/deserialize JSON messages.</span></span> <span data-ttu-id="30dfe-157">如需還原`Newtonsoft.Json`為基礎之序列化程式的指示，請參閱[切換至 Newtonsoft。](xref:migration/22-to-30#switch-to-newtonsoftjson)</span><span class="sxs-lookup"><span data-stu-id="30dfe-157">See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson) for instructions to restore the `Newtonsoft.Json`-based serializer.</span></span>

<span data-ttu-id="30dfe-158">在適用于 SignalR 的 JavaScript 和 .NET 用戶端中，已加入自動重新連接的支援。</span><span class="sxs-lookup"><span data-stu-id="30dfe-158">In the JavaScript and .NET Clients for SignalR, support was added for automatic reconnection.</span></span> <span data-ttu-id="30dfe-159">根據預設，用戶端會嘗試立即重新連線，並在2、10和30秒後重試（如有必要）。</span><span class="sxs-lookup"><span data-stu-id="30dfe-159">By default, the client tries to reconnect immediately and retry after 2, 10, and 30 seconds if necessary.</span></span> <span data-ttu-id="30dfe-160">如果用戶端成功重新連接，則會收到新的連線識別碼。</span><span class="sxs-lookup"><span data-stu-id="30dfe-160">If the client successfully reconnects, it receives a new connection ID.</span></span> <span data-ttu-id="30dfe-161">自動重新連線是加入宣告的：</span><span class="sxs-lookup"><span data-stu-id="30dfe-161">Automatic reconnect is opt-in:</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

<span data-ttu-id="30dfe-162">藉由傳遞以毫秒為依據的持續時間陣列，可以指定重新連接間隔：</span><span class="sxs-lookup"><span data-stu-id="30dfe-162">The reconnection intervals can be specified by passing an array of millisecond-based durations:</span></span>

```javascript
.withAutomaticReconnect([0, 3000, 5000, 10000, 15000, 30000])
//.withAutomaticReconnect([0, 2000, 10000, 30000]) The default intervals.
```

<span data-ttu-id="30dfe-163">您可以傳入自訂的執行，以取得重新連接間隔的完整控制。</span><span class="sxs-lookup"><span data-stu-id="30dfe-163">A custom implementation can be passed in for full control of the reconnection intervals.</span></span>

<span data-ttu-id="30dfe-164">如果在上一次重新連線間隔之後失敗重新連接：</span><span class="sxs-lookup"><span data-stu-id="30dfe-164">If the reconnection fails after the last reconnect interval:</span></span>

* <span data-ttu-id="30dfe-165">用戶端會將連接視為離線。</span><span class="sxs-lookup"><span data-stu-id="30dfe-165">The client considers the connection is offline.</span></span>
* <span data-ttu-id="30dfe-166">用戶端停止嘗試重新連線。</span><span class="sxs-lookup"><span data-stu-id="30dfe-166">The client stops trying to reconnect.</span></span>

<span data-ttu-id="30dfe-167">在重新連線嘗試期間，更新應用程式 UI，以通知使用者正在嘗試重新連接。</span><span class="sxs-lookup"><span data-stu-id="30dfe-167">During reconnection attempts, update the app UI to notify the user that the reconnection is being attempted.</span></span>

<span data-ttu-id="30dfe-168">若要在連接中斷時提供 UI 意見反應，SignalR 用戶端 API 已擴充為包含下列事件處理常式：</span><span class="sxs-lookup"><span data-stu-id="30dfe-168">To provide UI feedback when the connection is interrupted, the SignalR client API has been expanded to include the following event handlers:</span></span>

* <span data-ttu-id="30dfe-169">`onreconnecting`:讓開發人員有機會停用 UI，或讓使用者知道應用程式已離線。</span><span class="sxs-lookup"><span data-stu-id="30dfe-169">`onreconnecting`:  Gives developers an opportunity to disable UI or to let users know the app is offline.</span></span>
* <span data-ttu-id="30dfe-170">`onreconnected`:讓開發人員有機會在連接重新建立後更新 UI。</span><span class="sxs-lookup"><span data-stu-id="30dfe-170">`onreconnected`: Gives developers an opportunity to update the UI once the connection is reestablished.</span></span>

<span data-ttu-id="30dfe-171">下列程式碼會`onreconnecting`在嘗試連接時使用來更新 UI：</span><span class="sxs-lookup"><span data-stu-id="30dfe-171">The following code uses `onreconnecting` to update the UI while trying to connect:</span></span>

```javascript
connection.onreconnecting((error) => {
    const status = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messageInput").disabled = true;
    document.getElementById("sendButton").disabled = true;
    document.getElementById("connectionStatus").innerText = status;
});
```

<span data-ttu-id="30dfe-172">下列程式碼會`onreconnected`使用來更新連接上的 UI：</span><span class="sxs-lookup"><span data-stu-id="30dfe-172">The following code uses `onreconnected` to update the UI on connection:</span></span>

```javascript
connection.onreconnected((connectionId) => {
    const status = `Connection reestablished. Connected.`;
    document.getElementById("messageInput").disabled = false;
    document.getElementById("sendButton").disabled = false;
    document.getElementById("connectionStatus").innerText = status;
});
```

<span data-ttu-id="30dfe-173">當中樞方法需要授權時，SignalR 3.0 和更新版本會將自訂資源提供給授權處理常式。</span><span class="sxs-lookup"><span data-stu-id="30dfe-173">SignalR 3.0 and later provides a custom resource to authorization handlers when a hub method requires authorization.</span></span> <span data-ttu-id="30dfe-174">資源是的實例`HubInvocationContext`。</span><span class="sxs-lookup"><span data-stu-id="30dfe-174">The resource is an instance of `HubInvocationContext`.</span></span> <span data-ttu-id="30dfe-175">`HubInvocationContext`包含：</span><span class="sxs-lookup"><span data-stu-id="30dfe-175">The `HubInvocationContext` includes the:</span></span>

* `HubCallerContext`
* <span data-ttu-id="30dfe-176">所叫用之中樞方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="30dfe-176">Name of the hub method being invoked.</span></span>
* <span data-ttu-id="30dfe-177">中樞方法的引數。</span><span class="sxs-lookup"><span data-stu-id="30dfe-177">Arguments to the hub method.</span></span>

<span data-ttu-id="30dfe-178">請考慮下列聊天室應用程式範例，讓多個組織能夠透過 Azure Active Directory 進行登入。</span><span class="sxs-lookup"><span data-stu-id="30dfe-178">Consider the following example of a chat room app allowing multiple organization sign-in via Azure Active Directory.</span></span> <span data-ttu-id="30dfe-179">具有 Microsoft 帳戶的任何人都可以登入交談，但只有擁有組織的成員可以禁止使用者或觀看使用者的聊天記錄。</span><span class="sxs-lookup"><span data-stu-id="30dfe-179">Anyone with a Microsoft account can sign in to chat, but only members of the owning organization can ban users or view users’ chat histories.</span></span> <span data-ttu-id="30dfe-180">應用程式可能會限制特定使用者的特定功能。</span><span class="sxs-lookup"><span data-stu-id="30dfe-180">The app could restrict certain functionality from specific users.</span></span>

```csharp
public class DomainRestrictedRequirement :
    AuthorizationHandler<DomainRestrictedRequirement, HubInvocationContext>,
    IAuthorizationRequirement
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context,
        DomainRestrictedRequirement requirement,
        HubInvocationContext resource)
    {
        if (context.User?.Identity?.Name == null)
        {
            return Task.CompletedTask;
        }

        if (IsUserAllowedToDoThis(resource.HubMethodName, context.User.Identity.Name))
        {
            context.Succeed(requirement);
        }

        return Task.CompletedTask;
    }

    private bool IsUserAllowedToDoThis(string hubMethodName, string currentUsername)
    {
        if (hubMethodName.Equals("banUser", StringComparison.OrdinalIgnoreCase))
        {
            return currentUsername.Equals("bob42@jabbr.net", StringComparison.OrdinalIgnoreCase);
        }

        return currentUsername.EndsWith("@jabbr.net", StringComparison.OrdinalIgnoreCase));
    }
}
```

<span data-ttu-id="30dfe-181">在上述程式碼中`DomainRestrictedRequirement` ，會作為自`IAuthorizationRequirement`定義。</span><span class="sxs-lookup"><span data-stu-id="30dfe-181">In the preceding code, `DomainRestrictedRequirement` serves as a custom `IAuthorizationRequirement`.</span></span> <span data-ttu-id="30dfe-182">因為傳入`HubInvocationContext`的是資源參數，所以內部邏輯可以：</span><span class="sxs-lookup"><span data-stu-id="30dfe-182">Because the `HubInvocationContext` resource parameter is being passed in, the internal logic can:</span></span>

* <span data-ttu-id="30dfe-183">檢查正在呼叫中樞的內容。</span><span class="sxs-lookup"><span data-stu-id="30dfe-183">Inspect the context in which the Hub is being called.</span></span>
* <span data-ttu-id="30dfe-184">請決定是否要讓使用者執行個別的中樞方法。</span><span class="sxs-lookup"><span data-stu-id="30dfe-184">Make decisions on allowing the user to execute individual Hub methods.</span></span>

<span data-ttu-id="30dfe-185">您可以使用程式碼在執行時間檢查的原則名稱來裝飾個別的中樞方法。</span><span class="sxs-lookup"><span data-stu-id="30dfe-185">Individual Hub methods can be decorated with the name of the policy the code checks at run-time.</span></span> <span data-ttu-id="30dfe-186">當用戶端嘗試呼叫個別的中樞方法時`DomainRestrictedRequirement` ，處理常式會執行並控制方法的存取。</span><span class="sxs-lookup"><span data-stu-id="30dfe-186">As clients attempt to call individual Hub methods, the `DomainRestrictedRequirement` handler runs and controls access to the methods.</span></span> <span data-ttu-id="30dfe-187">根據`DomainRestrictedRequirement`控制項存取的方式：</span><span class="sxs-lookup"><span data-stu-id="30dfe-187">Based on the way the `DomainRestrictedRequirement` controls access:</span></span>

* <span data-ttu-id="30dfe-188">所有登入的使用者都可以呼叫`SendMessage`方法。</span><span class="sxs-lookup"><span data-stu-id="30dfe-188">All logged-in users can call the `SendMessage` method.</span></span>
* <span data-ttu-id="30dfe-189">只有已使用`@jabbr.net`電子郵件地址登入的使用者可以查看使用者的歷程記錄。</span><span class="sxs-lookup"><span data-stu-id="30dfe-189">Only users who have logged in with a `@jabbr.net` email address can view users’ histories.</span></span>
* <span data-ttu-id="30dfe-190">僅`bob42@jabbr.net`能禁止來自聊天室的使用者。</span><span class="sxs-lookup"><span data-stu-id="30dfe-190">Only `bob42@jabbr.net` can ban users from the chat room.</span></span>

```csharp
[Authorize]
public class ChatHub : Hub
{
    public void SendMessage(string message)
    {
    }

    [Authorize("DomainRestricted")]
    public void BanUser(string username)
    {
    }

    [Authorize("DomainRestricted")]
    public void ViewUserHistory(string username)
    {
    }
}
```

<span data-ttu-id="30dfe-191">建立原則`DomainRestricted`可能牽涉到：</span><span class="sxs-lookup"><span data-stu-id="30dfe-191">Creating the `DomainRestricted` policy might involve:</span></span>

* <span data-ttu-id="30dfe-192">在*Startup.cs*中，新增原則。</span><span class="sxs-lookup"><span data-stu-id="30dfe-192">In *Startup.cs*, adding the new policy.</span></span>
* <span data-ttu-id="30dfe-193">提供自訂`DomainRestrictedRequirement`需求做為參數。</span><span class="sxs-lookup"><span data-stu-id="30dfe-193">Provide the custom `DomainRestrictedRequirement` requirement as a parameter.</span></span>
* <span data-ttu-id="30dfe-194">向`DomainRestricted`授權中介軟體註冊。</span><span class="sxs-lookup"><span data-stu-id="30dfe-194">Registering `DomainRestricted` with the authorization middleware.</span></span>

```csharp
services
    .AddAuthorization(options =>
    {
        options.AddPolicy("DomainRestricted", policy =>
        {
            policy.Requirements.Add(new DomainRestrictedRequirement());
        });
    });
```

<span data-ttu-id="30dfe-195">SignalR 中樞會使用[端點路由](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="30dfe-195">SignalR hubs use [Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="30dfe-196">SignalR 中樞連線先前已明確完成：</span><span class="sxs-lookup"><span data-stu-id="30dfe-196">SignalR hub connection was previously done explicitly:</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("hubs/chat");
});
```

<span data-ttu-id="30dfe-197">在先前的版本中，開發人員需要在各種不同的位置連接控制器、Razor 頁面和中樞。</span><span class="sxs-lookup"><span data-stu-id="30dfe-197">In the previous version, developers needed to wire up controllers, Razor pages, and hubs in a variety of different places.</span></span> <span data-ttu-id="30dfe-198">明確連接會產生一系列幾乎相同的路由區段：</span><span class="sxs-lookup"><span data-stu-id="30dfe-198">Explicit connection results in a series of nearly-identical routing segments:</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("hubs/chat");
});

app.UseRouting(routes =>
{
    routes.MapRazorPages();
});
```

<span data-ttu-id="30dfe-199">SignalR 3.0 中樞可以透過端點路由來路由傳送。</span><span class="sxs-lookup"><span data-stu-id="30dfe-199">SignalR 3.0 hubs can be routed via endpoint routing.</span></span> <span data-ttu-id="30dfe-200">使用端點路由，通常可以在中`UseRouting`設定所有路由：</span><span class="sxs-lookup"><span data-stu-id="30dfe-200">With endpoint routing, typically all routing can be configured in `UseRouting`:</span></span>

```csharp
app.UseRouting(routes =>
{
    routes.MapRazorPages();
    routes.MapHub<ChatHub>("hubs/chat");
});
```

<span data-ttu-id="30dfe-201">已新增 ASP.NET Core 3.0 SignalR：</span><span class="sxs-lookup"><span data-stu-id="30dfe-201">ASP.NET Core 3.0 SignalR added:</span></span>

<span data-ttu-id="30dfe-202">用戶端對伺服器串流。</span><span class="sxs-lookup"><span data-stu-id="30dfe-202">Client-to-server streaming.</span></span> <span data-ttu-id="30dfe-203">使用用戶端對伺服器串流，伺服器端方法可以接受`IAsyncEnumerable<T>`或`ChannelReader<T>`的實例。</span><span class="sxs-lookup"><span data-stu-id="30dfe-203">With client-to-server streaming, server-side methods can take instances of either an `IAsyncEnumerable<T>` or `ChannelReader<T>`.</span></span> <span data-ttu-id="30dfe-204">在下列C#範例中，中樞`UploadStream`上的方法會接收來自用戶端的字串資料流程：</span><span class="sxs-lookup"><span data-stu-id="30dfe-204">In the following C# sample, the `UploadStream` method on the Hub will receive a stream of strings from the client:</span></span>

```csharp
public async Task UploadStream(IAsyncEnumerable<string> stream)
{
    await foreach (var item in stream)
    {
        // process content
    }
}
```

<span data-ttu-id="30dfe-205">.Net 用戶端應用程式可以傳遞`IAsyncEnumerable<T>`或`ChannelReader<T>`實例`UploadStream`作為`stream`上述中樞方法的引數。</span><span class="sxs-lookup"><span data-stu-id="30dfe-205">.NET client apps can pass either an `IAsyncEnumerable<T>` or `ChannelReader<T>` instance as the `stream` argument of the `UploadStream` Hub method above.</span></span>

<span data-ttu-id="30dfe-206">在`for`迴圈完成且區域函式結束後，就會傳送資料流程完成：</span><span class="sxs-lookup"><span data-stu-id="30dfe-206">After the `for` loop has completed and the local function exits, the stream completion is sent:</span></span>

```csharp
async IAsyncEnumerable<string> clientStreamData()
{
    for (var i = 0; i < 5; i++)
    {
        var data = await FetchSomeData();
        yield return data;
    }
}

await connection.SendAsync("UploadStream", clientStreamData());
```

<span data-ttu-id="30dfe-207">JavaScript 用戶端應用程式會`Subject`將 SignalR （或[RxJS 主體](https://rxjs.dev/api/index/class/Subject)）用於`stream`上述`UploadStream`中樞方法的引數。</span><span class="sxs-lookup"><span data-stu-id="30dfe-207">JavaScript client apps use the SignalR `Subject` (or an [RxJS Subject](https://rxjs.dev/api/index/class/Subject)) for the `stream` argument of the `UploadStream` Hub method above.</span></span>

```javascript
let subject = new signalR.Subject();
await connection.send("StartStream", "MyAsciiArtStream", subject);
```

<span data-ttu-id="30dfe-208">JavaScript 程式碼可以使用`subject.next`方法來處理已捕捉並準備傳送至伺服器的字串。</span><span class="sxs-lookup"><span data-stu-id="30dfe-208">The JavaScript code could use the `subject.next` method to handle strings as they are captured and ready to be sent to the server.</span></span>

```javascript
subject.next("example");
subject.complete();
```

<span data-ttu-id="30dfe-209">使用上述兩個程式碼片段這類程式碼，可以建立即時串流體驗。</span><span class="sxs-lookup"><span data-stu-id="30dfe-209">Using code like the two preceding snippets, real-time streaming experiences can be created.</span></span>

### <a name="new-json-serialization"></a><span data-ttu-id="30dfe-210">新的 JSON 序列化</span><span class="sxs-lookup"><span data-stu-id="30dfe-210">New JSON serialization</span></span>

<span data-ttu-id="30dfe-211">ASP.NET Core 3.0 現在會<xref:System.Text.Json>使用 JSON 序列化的預設值：</span><span class="sxs-lookup"><span data-stu-id="30dfe-211">ASP.NET Core 3.0 now uses <xref:System.Text.Json> by default for JSON serialization:</span></span>

* <span data-ttu-id="30dfe-212">非同步讀取和寫入 JSON。</span><span class="sxs-lookup"><span data-stu-id="30dfe-212">Reads and writes JSON asynchronously.</span></span>
* <span data-ttu-id="30dfe-213">已針對 UTF-8 文字進行優化。</span><span class="sxs-lookup"><span data-stu-id="30dfe-213">Is optimized for UTF-8 text.</span></span>
* <span data-ttu-id="30dfe-214">通常比`Newtonsoft.Json`更高的效能。</span><span class="sxs-lookup"><span data-stu-id="30dfe-214">Typically higher performance than `Newtonsoft.Json`.</span></span>

<span data-ttu-id="30dfe-215">若要將 Json.NET 新增至 ASP.NET Core 3.0，請參閱[新增 Newtonsoft 以 json 為基礎的 json 格式支援](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support)。</span><span class="sxs-lookup"><span data-stu-id="30dfe-215">To add Json.NET to ASP.NET Core 3.0, see [Add Newtonsoft.Json-based JSON format support](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support).</span></span>

## <a name="new-razor-directives"></a><span data-ttu-id="30dfe-216">新的 Razor 指示詞</span><span class="sxs-lookup"><span data-stu-id="30dfe-216">New Razor directives</span></span>

<span data-ttu-id="30dfe-217">下列清單包含新的 Razor 指示詞：</span><span class="sxs-lookup"><span data-stu-id="30dfe-217">The following list contains new Razor directives:</span></span>

* <span data-ttu-id="30dfe-218">[@attribute](xref:mvc/views/razor#attribute)&ndash; 指示詞會將指定的屬性套用至所產生頁面`@attribute`或視圖的類別。</span><span class="sxs-lookup"><span data-stu-id="30dfe-218">[@attribute](xref:mvc/views/razor#attribute) &ndash; The `@attribute` directive applies the given attribute to the class of the generated page or view.</span></span> <span data-ttu-id="30dfe-219">例如： `@attribute [Authorize]` 。</span><span class="sxs-lookup"><span data-stu-id="30dfe-219">For example, `@attribute [Authorize]`.</span></span>
* <span data-ttu-id="30dfe-220">[@implements](xref:mvc/views/razor#implements)&ndash; 指示詞會為所產生的`@implements`類別實作為介面。</span><span class="sxs-lookup"><span data-stu-id="30dfe-220">[@implements](xref:mvc/views/razor#implements) &ndash; The `@implements` directive implements an interface for the generated class.</span></span> <span data-ttu-id="30dfe-221">例如： `@implements IDisposable` 。</span><span class="sxs-lookup"><span data-stu-id="30dfe-221">For example, `@implements IDisposable`.</span></span>

## <a name="identityserver4-supports-authentication-and-authorization-for-web-apis-and-spas"></a><span data-ttu-id="30dfe-222">IdentityServer4 支援 web Api 和 Spa 的驗證和授權</span><span class="sxs-lookup"><span data-stu-id="30dfe-222">IdentityServer4 supports authentication and authorization for web APIs and SPAs</span></span>

<span data-ttu-id="30dfe-223">[IdentityServer4](https://identityserver.io)是適用于 ASP.NET Core 3.0 的 OpenID Connect 和 OAuth 2.0 架構。</span><span class="sxs-lookup"><span data-stu-id="30dfe-223">[IdentityServer4](https://identityserver.io) is an OpenID Connect and OAuth 2.0 framework for ASP.NET Core 3.0.</span></span> <span data-ttu-id="30dfe-224">IdentityServer4 可啟用下列安全性功能：</span><span class="sxs-lookup"><span data-stu-id="30dfe-224">IdentityServer4 enables the following security features:</span></span>

* <span data-ttu-id="30dfe-225">驗證即服務（AaaS）</span><span class="sxs-lookup"><span data-stu-id="30dfe-225">Authentication as a Service (AaaS)</span></span>
* <span data-ttu-id="30dfe-226">多個應用程式類型的單一登入/關閉（SSO）</span><span class="sxs-lookup"><span data-stu-id="30dfe-226">Single sign-on/off (SSO) over multiple application types</span></span>
* <span data-ttu-id="30dfe-227">Api 的存取控制</span><span class="sxs-lookup"><span data-stu-id="30dfe-227">Access control for APIs</span></span>
* <span data-ttu-id="30dfe-228">同盟閘道</span><span class="sxs-lookup"><span data-stu-id="30dfe-228">Federation Gateway</span></span>

<span data-ttu-id="30dfe-229">如需詳細資訊，請參閱[歡迎使用 IdentityServer4](http://docs.identityserver.io/en/latest/index.html)。</span><span class="sxs-lookup"><span data-stu-id="30dfe-229">For more information, see [Welcome to IdentityServer4](http://docs.identityserver.io/en/latest/index.html).</span></span>

## <a name="certificate-and-kerberos-authentication"></a><span data-ttu-id="30dfe-230">憑證和 Kerberos 驗證</span><span class="sxs-lookup"><span data-stu-id="30dfe-230">Certificate and Kerberos authentication</span></span>

<span data-ttu-id="30dfe-231">憑證驗證需要：</span><span class="sxs-lookup"><span data-stu-id="30dfe-231">Certificate authentication requires:</span></span>

* <span data-ttu-id="30dfe-232">正在設定伺服器以接受憑證。</span><span class="sxs-lookup"><span data-stu-id="30dfe-232">Configuring the server to accept certificates.</span></span>
* <span data-ttu-id="30dfe-233">在中`Startup.Configure`新增驗證中介軟體。</span><span class="sxs-lookup"><span data-stu-id="30dfe-233">Adding the authentication middleware in `Startup.Configure`.</span></span>
* <span data-ttu-id="30dfe-234">在中`Startup.ConfigureServices`新增憑證驗證服務。</span><span class="sxs-lookup"><span data-stu-id="30dfe-234">Adding the certificate authentication service in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(
        CertificateAuthenticationDefaults.AuthenticationScheme)
            .AddCertificate();
    // Other service configuration removed.
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseAuthentication();
    // Other app configuration removed.
}
```

<span data-ttu-id="30dfe-235">憑證驗證的選項包括下列功能：</span><span class="sxs-lookup"><span data-stu-id="30dfe-235">Options for certificate authentication include the ability to:</span></span>

* <span data-ttu-id="30dfe-236">接受自我簽署憑證。</span><span class="sxs-lookup"><span data-stu-id="30dfe-236">Accept self-signed certificates.</span></span>
* <span data-ttu-id="30dfe-237">檢查憑證是否已撤銷。</span><span class="sxs-lookup"><span data-stu-id="30dfe-237">Check for certificate revocation.</span></span>
* <span data-ttu-id="30dfe-238">檢查好處憑證中是否有正確的使用方式旗標。</span><span class="sxs-lookup"><span data-stu-id="30dfe-238">Check that the proffered certificate has the right usage flags in it.</span></span>

<span data-ttu-id="30dfe-239">預設的使用者主體會從憑證屬性來建立。</span><span class="sxs-lookup"><span data-stu-id="30dfe-239">A default user principal is constructed from the certificate properties.</span></span> <span data-ttu-id="30dfe-240">使用者主體包含的事件可讓您補充或取代主體。</span><span class="sxs-lookup"><span data-stu-id="30dfe-240">The user principal contains an event that enables supplementing or replacing the principal.</span></span> <span data-ttu-id="30dfe-241">如需詳細資訊，請參閱<xref:security/authentication/certauth>。</span><span class="sxs-lookup"><span data-stu-id="30dfe-241">For more information, see <xref:security/authentication/certauth>.</span></span>

<span data-ttu-id="30dfe-242">[Windows 驗證](/windows-server/security/windows-authentication/windows-authentication-overview)已擴充到 Linux 和 macOS。</span><span class="sxs-lookup"><span data-stu-id="30dfe-242">[Windows Authentication](/windows-server/security/windows-authentication/windows-authentication-overview) has been extended onto Linux and macOS.</span></span> <span data-ttu-id="30dfe-243">在先前的版本中，Windows 驗證僅限於[IIS](xref:host-and-deploy/iis/index)和[HttpSys](xref:fundamentals/servers/httpsys)。</span><span class="sxs-lookup"><span data-stu-id="30dfe-243">In previous versions, Windows Authentication was limited to [IIS](xref:host-and-deploy/iis/index) and [HttpSys](xref:fundamentals/servers/httpsys).</span></span> <span data-ttu-id="30dfe-244">在 ASP.NET Core 3.0 中， [Kestrel](xref:fundamentals/servers/kestrel)可以在 windows、Linux 和 macOS 上針對已加入網域的 windows 主機使用 Negotiate、 [Kerberos](/windows-server/security/kerberos/kerberos-authentication-overview)和[NTLM](/windows-server/security/kerberos/ntlm-overview)。</span><span class="sxs-lookup"><span data-stu-id="30dfe-244">In ASP.NET Core 3.0, [Kestrel](xref:fundamentals/servers/kestrel) has the ability to use Negotiate, [Kerberos](/windows-server/security/kerberos/kerberos-authentication-overview), and [NTLM on Windows](/windows-server/security/kerberos/ntlm-overview), Linux, and macOS for Windows domain-joined hosts.</span></span> <span data-ttu-id="30dfe-245">這些驗證配置的 Kestrel 支援是由 AspNetCore 所提供。 [Negotiate NuGet](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate)套件。</span><span class="sxs-lookup"><span data-stu-id="30dfe-245">Kestrel support of these authentication schemes is provided by the [Microsoft.AspNetCore.Authentication.Negotiate NuGet](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) package.</span></span> <span data-ttu-id="30dfe-246">如同其他驗證服務，請將驗證應用程式設定為 [寬]，然後設定服務：</span><span class="sxs-lookup"><span data-stu-id="30dfe-246">As with the other authentication services, configure authentication app wide, then configure the service:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(NegotiateDefaults.AuthenticationScheme)
        .AddNegotiate();
    // Other service configuration removed.
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseAuthentication();
    // Other app configuration removed.
}
```

<span data-ttu-id="30dfe-247">主機需求：</span><span class="sxs-lookup"><span data-stu-id="30dfe-247">Host requirements:</span></span>

* <span data-ttu-id="30dfe-248">Windows 主機必須將[服務主體名稱](/windows/win32/ad/service-principal-names)（spn）新增至裝載應用程式的使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="30dfe-248">Windows hosts must have [Service Principal Names](/windows/win32/ad/service-principal-names) (SPNs) added to the user account hosting the app.</span></span>
* <span data-ttu-id="30dfe-249">Linux 和 macOS 機器必須加入網域。</span><span class="sxs-lookup"><span data-stu-id="30dfe-249">Linux and macOS machines must be joined to the domain.</span></span>
  * <span data-ttu-id="30dfe-250">必須為 web 進程建立 Spn。</span><span class="sxs-lookup"><span data-stu-id="30dfe-250">SPNs must be created for the web process.</span></span>
  * <span data-ttu-id="30dfe-251">必須在主機電腦上產生和設定[Keytab](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/)檔案。</span><span class="sxs-lookup"><span data-stu-id="30dfe-251">[Keytab files](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) must be generated and configured on the host machine.</span></span>

<span data-ttu-id="30dfe-252">如需詳細資訊，請參閱<xref:security/authentication/windowsauth>。</span><span class="sxs-lookup"><span data-stu-id="30dfe-252">For more information, see <xref:security/authentication/windowsauth>.</span></span>

## <a name="template-changes"></a><span data-ttu-id="30dfe-253">範本變更</span><span class="sxs-lookup"><span data-stu-id="30dfe-253">Template changes</span></span>

<span data-ttu-id="30dfe-254">Web UI 範本（Razor Pages、具有控制器和 views 的 MVC）已移除下列各項：</span><span class="sxs-lookup"><span data-stu-id="30dfe-254">The web UI templates (Razor Pages, MVC with controller and views) have the following removed:</span></span>

* <span data-ttu-id="30dfe-255">Cookie 同意 UI 已不再包含在內。</span><span class="sxs-lookup"><span data-stu-id="30dfe-255">The cookie consent UI is no longer included.</span></span> <span data-ttu-id="30dfe-256">若要在 ASP.NET Core 3.0 範本產生的應用程式中啟用 cookie 同意功能<xref:security/gdpr>，請參閱。</span><span class="sxs-lookup"><span data-stu-id="30dfe-256">To enable the cookie consent feature in an ASP.NET Core 3.0 template generated app, see <xref:security/gdpr>.</span></span>
* <span data-ttu-id="30dfe-257">腳本和相關的靜態資產現在會當做本機檔案來參考，而不是使用 Cdn。</span><span class="sxs-lookup"><span data-stu-id="30dfe-257">Scripts and related static assets are now referenced as local files instead of using CDNs.</span></span> <span data-ttu-id="30dfe-258">如需詳細資訊，請參閱[腳本和相關靜態資產現在會當做本機檔案參考，而不是根據目前的環境使用 cdn （aspnet/AspNetCore #14350）](https://github.com/aspnet/AspNetCore.Docs/issues/14350)。</span><span class="sxs-lookup"><span data-stu-id="30dfe-258">For more information, see [Scripts and related static assets are now referenced as local files instead of using CDNs based on the current environment (aspnet/AspNetCore.Docs #14350)](https://github.com/aspnet/AspNetCore.Docs/issues/14350).</span></span>

<span data-ttu-id="30dfe-259">「角度」範本已更新為使用「角度8」。</span><span class="sxs-lookup"><span data-stu-id="30dfe-259">The Angular template updated to use Angular 8.</span></span>

<span data-ttu-id="30dfe-260">根據預設，Razor 類別庫（RCL）範本預設為 Razor 元件開發。</span><span class="sxs-lookup"><span data-stu-id="30dfe-260">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="30dfe-261">Visual Studio 中的新範本選項會提供頁面和視圖的範本支援。</span><span class="sxs-lookup"><span data-stu-id="30dfe-261">A new template option in Visual Studio provides template support for pages and views.</span></span> <span data-ttu-id="30dfe-262">從命令 shell 中的範本建立 RCL 時，請傳遞`-support-pages-and-views`選項（`dotnet new razorclasslib -support-pages-and-views`）。</span><span class="sxs-lookup"><span data-stu-id="30dfe-262">When creating an RCL from the template in a command shell, pass the `-support-pages-and-views` option (`dotnet new razorclasslib -support-pages-and-views`).</span></span>

## <a name="generic-host"></a><span data-ttu-id="30dfe-263">一般主機</span><span class="sxs-lookup"><span data-stu-id="30dfe-263">Generic Host</span></span>

<span data-ttu-id="30dfe-264">ASP.NET Core 3.0 範本會使用<xref:fundamentals/host/generic-host>。</span><span class="sxs-lookup"><span data-stu-id="30dfe-264">The ASP.NET Core 3.0 templates use <xref:fundamentals/host/generic-host>.</span></span> <span data-ttu-id="30dfe-265">先前使用<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>的版本。</span><span class="sxs-lookup"><span data-stu-id="30dfe-265">Previous versions used <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="30dfe-266">使用 .net Core 泛型主機（<xref:Microsoft.Extensions.Hosting.HostBuilder>）可讓 ASP.NET Core 應用程式與非 web 特定的其他伺服器案例更緊密整合。</span><span class="sxs-lookup"><span data-stu-id="30dfe-266">Using the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>) provides better integration of ASP.NET Core apps with other server scenarios that are not web specific.</span></span> <span data-ttu-id="30dfe-267">如需詳細資訊，請參閱[HostBuilder 取代 WebHostBuilder](xref:migration/22-to-30?view=aspnetcore-2.2#hostbuilder-replaces-webhostbuilder)。</span><span class="sxs-lookup"><span data-stu-id="30dfe-267">For more information, see [HostBuilder replaces WebHostBuilder](xref:migration/22-to-30?view=aspnetcore-2.2#hostbuilder-replaces-webhostbuilder).</span></span>

### <a name="host-configuration"></a><span data-ttu-id="30dfe-268">主機組態</span><span class="sxs-lookup"><span data-stu-id="30dfe-268">Host configuration</span></span>

<span data-ttu-id="30dfe-269">在 ASP.NET Core 3.0 發行之前， `ASPNETCORE_`已載入前面加上的環境變數，以進行 Web 主機的主機設定。</span><span class="sxs-lookup"><span data-stu-id="30dfe-269">Prior to the release of ASP.NET Core 3.0, environment variables prefixed with `ASPNETCORE_` were loaded for host configuration of the Web Host.</span></span> <span data-ttu-id="30dfe-270">在3.0 中`AddEnvironmentVariables` ，是用來載入`DOTNET_`前面加上之主機`CreateDefaultBuilder`設定的環境變數。</span><span class="sxs-lookup"><span data-stu-id="30dfe-270">In 3.0, `AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for host configuration with `CreateDefaultBuilder`.</span></span>

### <a name="changes-to-startup-contructor-injection"></a><span data-ttu-id="30dfe-271">啟動 contructor 插入的變更</span><span class="sxs-lookup"><span data-stu-id="30dfe-271">Changes to Startup contructor injection</span></span>

<span data-ttu-id="30dfe-272">泛型主機僅支援下列型別來`Startup`插入函式：</span><span class="sxs-lookup"><span data-stu-id="30dfe-272">The Generic Host only supports the following types for `Startup` constructor injection:</span></span>

* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

<span data-ttu-id="30dfe-273">所有服務仍然可以直接插入做為方法的`Startup.Configure`引數。</span><span class="sxs-lookup"><span data-stu-id="30dfe-273">All services can still be injected directly as arguments to the `Startup.Configure` method.</span></span> <span data-ttu-id="30dfe-274">如需詳細資訊，請參閱[泛型主機限制啟動函數插入（aspnet/公告 #353）](https://github.com/aspnet/Announcements/issues/353)。</span><span class="sxs-lookup"><span data-stu-id="30dfe-274">For more information, see [Generic Host restricts Startup constructor injection (aspnet/Announcements #353)](https://github.com/aspnet/Announcements/issues/353).</span></span>

## <a name="kestrel"></a><span data-ttu-id="30dfe-275">Kestrel</span><span class="sxs-lookup"><span data-stu-id="30dfe-275">Kestrel</span></span>

* <span data-ttu-id="30dfe-276">Kestrel 設定已更新，可供遷移至一般主機。</span><span class="sxs-lookup"><span data-stu-id="30dfe-276">Kestrel configuration has been updated for the migration to the Generic Host.</span></span> <span data-ttu-id="30dfe-277">在3.0 中，Kestrel 是在所提供`ConfigureWebHostDefaults`的 web 主機產生器上設定。</span><span class="sxs-lookup"><span data-stu-id="30dfe-277">In 3.0, Kestrel is configured on the web host builder provided by `ConfigureWebHostDefaults`.</span></span>
* <span data-ttu-id="30dfe-278">連接介面卡已從 Kestrel 中移除，並以連線中介軟體取代，類似于 ASP.NET Core 管線中的 HTTP 中介軟體，但較低層級的連接。</span><span class="sxs-lookup"><span data-stu-id="30dfe-278">Connection Adapters have been removed from Kestrel and replaced with Connection Middleware, which is similar to HTTP Middleware in the ASP.NET Core pipeline but for lower-level connections.</span></span>
* <span data-ttu-id="30dfe-279">Kestrel 傳輸層已公開為中`Connections.Abstractions`的公用介面。</span><span class="sxs-lookup"><span data-stu-id="30dfe-279">The Kestrel transport layer has been exposed as a public interface in `Connections.Abstractions`.</span></span>
* <span data-ttu-id="30dfe-280">標頭和尾端之間的多義性已藉由將尾端標頭移至新集合來解決。</span><span class="sxs-lookup"><span data-stu-id="30dfe-280">Ambiguity between headers and trailers has been resolved by moving trailing headers to a new collection.</span></span>
* <span data-ttu-id="30dfe-281">同步 IO api （例如`HttpReqeuest.Body.Read`）是執行緒耗盡的常見來源，導致應用程式當機。</span><span class="sxs-lookup"><span data-stu-id="30dfe-281">Synchronous IO APIs, such as `HttpReqeuest.Body.Read`, are a common source of thread starvation leading to app crashes.</span></span> <span data-ttu-id="30dfe-282">在3.0 中`AllowSynchronousIO` ，預設為停用。</span><span class="sxs-lookup"><span data-stu-id="30dfe-282">In 3.0, `AllowSynchronousIO` is disabled by default.</span></span>

<span data-ttu-id="30dfe-283">如需詳細資訊，請參閱<xref:migration/22-to-30#kestrel>。</span><span class="sxs-lookup"><span data-stu-id="30dfe-283">For more information, see <xref:migration/22-to-30#kestrel>.</span></span>

## <a name="http2-enabled-by-default"></a><span data-ttu-id="30dfe-284">預設啟用 HTTP/2</span><span class="sxs-lookup"><span data-stu-id="30dfe-284">HTTP/2 enabled by default</span></span>

<span data-ttu-id="30dfe-285">在 HTTPS 端點的 Kestrel 中，預設會啟用 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="30dfe-285">HTTP/2 is enabled by default in Kestrel for HTTPS endpoints.</span></span> <span data-ttu-id="30dfe-286">受作業系統支援時，會啟用 IIS 或 HTTP.sys 的 HTTP/2 支援。</span><span class="sxs-lookup"><span data-stu-id="30dfe-286">HTTP/2 support for IIS or HTTP.sys is enabled when supported by the operating system.</span></span>

## <a name="eventcounters-on-request"></a><span data-ttu-id="30dfe-287">要求 EventCounters</span><span class="sxs-lookup"><span data-stu-id="30dfe-287">EventCounters on request</span></span>

<span data-ttu-id="30dfe-288">主控 EventSource `Microsoft.AspNetCore.Hosting`會發出下列與連入要求<xref:System.Diagnostics.Tracing.EventCounter>相關的新類型：</span><span class="sxs-lookup"><span data-stu-id="30dfe-288">The Hosting EventSource, `Microsoft.AspNetCore.Hosting`, emits the following new <xref:System.Diagnostics.Tracing.EventCounter> types related to incoming requests:</span></span>

* `requests-per-second`
* `total-requests`
* `current-requests`
* `failed-requests`

## <a name="endpoint-routing"></a><span data-ttu-id="30dfe-289">端點路由</span><span class="sxs-lookup"><span data-stu-id="30dfe-289">Endpoint routing</span></span>

<span data-ttu-id="30dfe-290">端點路由可讓架構（例如 MVC）適用于中介軟體，已增強：</span><span class="sxs-lookup"><span data-stu-id="30dfe-290">Endpoint Routing, which allows frameworks (for example, MVC) to work well with middleware, is enhanced:</span></span>

* <span data-ttu-id="30dfe-291">中介軟體和端點的順序可在的要求處理管線`Startup.Configure`中設定。</span><span class="sxs-lookup"><span data-stu-id="30dfe-291">The order of middleware and endpoints is configurable in the request processing pipeline of `Startup.Configure`.</span></span>
* <span data-ttu-id="30dfe-292">端點和中介軟體會與其他以 ASP.NET Core 為基礎的技術（例如健康狀態檢查）妥善地撰寫。</span><span class="sxs-lookup"><span data-stu-id="30dfe-292">Endpoints and middleware compose well with other ASP.NET Core-based technologies, such as Health Checks.</span></span>
* <span data-ttu-id="30dfe-293">端點可以在中介軟體和 MVC 中執行原則，例如 CORS 或授權。</span><span class="sxs-lookup"><span data-stu-id="30dfe-293">Endpoints can implement a policy, such as CORS or authorization, in both middleware and MVC.</span></span>
* <span data-ttu-id="30dfe-294">篩選器和屬性可以放在控制器中的方法上。</span><span class="sxs-lookup"><span data-stu-id="30dfe-294">Filters and attributes can be placed on methods in controllers.</span></span>

<span data-ttu-id="30dfe-295">如需詳細資訊，請參閱<xref:fundamentals/routing#routing-basics>。</span><span class="sxs-lookup"><span data-stu-id="30dfe-295">For more information, see <xref:fundamentals/routing#routing-basics>.</span></span>

## <a name="health-checks"></a><span data-ttu-id="30dfe-296">健康情況檢查</span><span class="sxs-lookup"><span data-stu-id="30dfe-296">Health Checks</span></span>

<span data-ttu-id="30dfe-297">健全狀況檢查會搭配泛型主機使用端點路由。</span><span class="sxs-lookup"><span data-stu-id="30dfe-297">Health Checks use endpoint routing with the Generic Host.</span></span> <span data-ttu-id="30dfe-298">在`Startup.Configure`中， `MapHealthChecks`使用端點 URL 或相對路徑在端點產生器上呼叫：</span><span class="sxs-lookup"><span data-stu-id="30dfe-298">In `Startup.Configure`, call `MapHealthChecks` on the endpoint builder with the endpoint URL or relative path:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
});
```

<span data-ttu-id="30dfe-299">健康情況檢查端點可以：</span><span class="sxs-lookup"><span data-stu-id="30dfe-299">Health Checks endpoints can:</span></span>

* <span data-ttu-id="30dfe-300">指定一或多個允許的主機/埠。</span><span class="sxs-lookup"><span data-stu-id="30dfe-300">Specify one or more permitted hosts/ports.</span></span>
* <span data-ttu-id="30dfe-301">需要授權。</span><span class="sxs-lookup"><span data-stu-id="30dfe-301">Require authorization.</span></span>
* <span data-ttu-id="30dfe-302">需要 CORS。</span><span class="sxs-lookup"><span data-stu-id="30dfe-302">Require CORS.</span></span>

<span data-ttu-id="30dfe-303">如需詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="30dfe-303">For more information, see the following articles:</span></span>

* <xref:migration/22-to-30#health-checks>
* <xref:host-and-deploy/health-checks>

## <a name="pipes-on-httpcontext"></a><span data-ttu-id="30dfe-304">HttpCoNtext 上的管道</span><span class="sxs-lookup"><span data-stu-id="30dfe-304">Pipes on HttpContext</span></span>

<span data-ttu-id="30dfe-305">現在可以讀取要求本文，並使用<xref:System.IO.Pipelines> API 寫入回應主體。</span><span class="sxs-lookup"><span data-stu-id="30dfe-305">It's now possible to read the request body and write the response body using the <xref:System.IO.Pipelines> API.</span></span> <span data-ttu-id="30dfe-306">必須提供</span><span class="sxs-lookup"><span data-stu-id="30dfe-306">The</span></span> <!-- <xref:Microsoft.AspNetCore.Http.HttpRequest.BodyReader> --> <span data-ttu-id="30dfe-307">`HttpRequest.BodyReader`屬性會提供<xref:System.IO.Pipelines.PipeReader> ，可用於讀取要求本文。</span><span class="sxs-lookup"><span data-stu-id="30dfe-307">`HttpRequest.BodyReader` property provides a <xref:System.IO.Pipelines.PipeReader> that can be used to read the request body.</span></span> <span data-ttu-id="30dfe-308">必須提供</span><span class="sxs-lookup"><span data-stu-id="30dfe-308">The</span></span> <!-- <xref:Microsoft.AspNetCore.Http.> --> <span data-ttu-id="30dfe-309">`HttpResponse.BodyWriter`屬性提供<xref:System.IO.Pipelines.PipeWriter>可用於寫入回應主體的。</span><span class="sxs-lookup"><span data-stu-id="30dfe-309">`HttpResponse.BodyWriter` property provides a <xref:System.IO.Pipelines.PipeWriter> that can be used to write the response body.</span></span> <span data-ttu-id="30dfe-310">`HttpRequest.BodyReader`是`HttpRequest.Body`資料流程的類比。</span><span class="sxs-lookup"><span data-stu-id="30dfe-310">`HttpRequest.BodyReader` is an analogue of the `HttpRequest.Body` stream.</span></span> <span data-ttu-id="30dfe-311">`HttpResponse.BodyWriter`是`HttpResponse.Body`資料流程的類比。</span><span class="sxs-lookup"><span data-stu-id="30dfe-311">`HttpResponse.BodyWriter` is an analogue of the `HttpResponse.Body` stream.</span></span>

<!-- indirectly related, https://github.com/dotnet/docs/pull/14414 won't be published by 9/23  -->

## <a name="improved-error-reporting-in-iis"></a><span data-ttu-id="30dfe-312">改善 IIS 中的錯誤報表</span><span class="sxs-lookup"><span data-stu-id="30dfe-312">Improved error reporting in IIS</span></span>

<span data-ttu-id="30dfe-313">在 IIS 中裝載 ASP.NET Core 應用程式時，啟動錯誤現在會產生更豐富的診斷資料。</span><span class="sxs-lookup"><span data-stu-id="30dfe-313">Startup errors when hosting ASP.NET Core apps in IIS now produce richer diagnostic data.</span></span> <span data-ttu-id="30dfe-314">這些錯誤會在適用的情況下向 Windows 事件記錄檔回報堆疊追蹤。</span><span class="sxs-lookup"><span data-stu-id="30dfe-314">These errors are reported to the Windows Event Log with stack traces wherever applicable.</span></span> <span data-ttu-id="30dfe-315">此外，所有的警告、錯誤和未處理的例外狀況都會記錄到 Windows 事件記錄檔中。</span><span class="sxs-lookup"><span data-stu-id="30dfe-315">In addition, all warnings, errors, and unhandled exceptions are logged to the Windows Event Log.</span></span>

## <a name="worker-service-and-worker-sdk"></a><span data-ttu-id="30dfe-316">背景工作服務和背景工作角色 SDK</span><span class="sxs-lookup"><span data-stu-id="30dfe-316">Worker Service and Worker SDK</span></span>

<span data-ttu-id="30dfe-317">.NET Core 3.0 引進了新的背景工作服務應用程式範本。</span><span class="sxs-lookup"><span data-stu-id="30dfe-317">.NET Core 3.0 introduces the new Worker Service app template.</span></span> <span data-ttu-id="30dfe-318">此範本可提供在 .NET Core 中撰寫長時間執行服務的起點。</span><span class="sxs-lookup"><span data-stu-id="30dfe-318">This template is provides a starting point for writing long running services in .NET Core.</span></span>

<span data-ttu-id="30dfe-319">如需詳細資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="30dfe-319">For more information, see:</span></span>

* [<span data-ttu-id="30dfe-320">.NET Core 背景工作角色做為 Windows 服務</span><span class="sxs-lookup"><span data-stu-id="30dfe-320">.NET Core Workers as Windows Services</span></span>](https://devblogs.microsoft.com/aspnet/net-core-workers-as-windows-services/)
* <xref:fundamentals/host/hosted-services>
* <xref:host-and-deploy/windows-service>

## <a name="forwarded-headers-middleware-improvements"></a><span data-ttu-id="30dfe-321">轉送的標頭中介軟體改善</span><span class="sxs-lookup"><span data-stu-id="30dfe-321">Forwarded Headers Middleware improvements</span></span>

<span data-ttu-id="30dfe-322">在舊版的 ASP.NET Core 中，當<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>部署<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>到 Azure Linux 或 IIS 以外的任何反向 proxy 後方時，呼叫和會有問題。</span><span class="sxs-lookup"><span data-stu-id="30dfe-322">In previous versions of ASP.NET Core, calling <xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*> and  <xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*> were problematic when deployed to an Azure Linux or behind any reverse proxy other than IIS.</span></span> <span data-ttu-id="30dfe-323">先前版本的修正已記載于[轉送 Linux 和非 IIS 反向 proxy 的配置](xref:host-and-deploy/proxy-load-balancer#forward-the-scheme-for-linux-and-non-iis-reverse-proxies)中。</span><span class="sxs-lookup"><span data-stu-id="30dfe-323">The fix for previous versions is documented in [Forward the scheme for Linux and non-IIS reverse proxies](xref:host-and-deploy/proxy-load-balancer#forward-the-scheme-for-linux-and-non-iis-reverse-proxies).</span></span>

<span data-ttu-id="30dfe-324">此案例已在 ASP.NET Core 3.0 中修正。</span><span class="sxs-lookup"><span data-stu-id="30dfe-324">This scenario is fixed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="30dfe-325">當`ASPNETCORE_FORWARDEDHEADERS_ENABLED`環境變數設定為`true`時，主機會啟用[轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer#forwarded-headers-middleware-options)。</span><span class="sxs-lookup"><span data-stu-id="30dfe-325">The host enables the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers-middleware-options) when the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span> <span data-ttu-id="30dfe-326">`ASPNETCORE_FORWARDEDHEADERS_ENABLED`在我們的`true`容器映射中設定為。</span><span class="sxs-lookup"><span data-stu-id="30dfe-326">`ASPNETCORE_FORWARDEDHEADERS_ENABLED` is set to `true` in our container images.</span></span>

## <a name="performance-improvements"></a><span data-ttu-id="30dfe-327">效能改善</span><span class="sxs-lookup"><span data-stu-id="30dfe-327">Performance improvements</span></span>

<span data-ttu-id="30dfe-328">ASP.NET Core 3.0 包含許多增強功能，可減少記憶體使用量並改善輸送量：</span><span class="sxs-lookup"><span data-stu-id="30dfe-328">ASP.NET Core 3.0 includes many improvements that reduce memory usage and improve throughput:</span></span>

* <span data-ttu-id="30dfe-329">針對範圍服務使用內建的相依性插入容器時，減少記憶體使用量。</span><span class="sxs-lookup"><span data-stu-id="30dfe-329">Reduction in memory usage when using the built-in dependency injection container for scoped services.</span></span>
* <span data-ttu-id="30dfe-330">減少整個架構的配置，包括中介軟體案例和路由。</span><span class="sxs-lookup"><span data-stu-id="30dfe-330">Reduction in allocations across the framework, including middleware scenarios and routing.</span></span>
* <span data-ttu-id="30dfe-331">減少 WebSocket 連接的記憶體使用量。</span><span class="sxs-lookup"><span data-stu-id="30dfe-331">Reduction in memory usage for WebSocket connections.</span></span>
* <span data-ttu-id="30dfe-332">HTTPS 連線的記憶體減少和輸送量改善。</span><span class="sxs-lookup"><span data-stu-id="30dfe-332">Memory reduction and throughput improvements for HTTPS connections.</span></span>
* <span data-ttu-id="30dfe-333">新的優化和完全非同步 JSON 序列化程式。</span><span class="sxs-lookup"><span data-stu-id="30dfe-333">New optimized and fully asynchronous JSON serializer.</span></span>
* <span data-ttu-id="30dfe-334">減少在表單剖析中的記憶體使用量和輸送量改善。</span><span class="sxs-lookup"><span data-stu-id="30dfe-334">Reduction in memory usage and throughput improvements in form parsing.</span></span>

## <a name="aspnet-core-30-only-runs-on-net-core-30"></a><span data-ttu-id="30dfe-335">ASP.NET Core 3.0 只會在 .NET Core 3.0 上執行</span><span class="sxs-lookup"><span data-stu-id="30dfe-335">ASP.NET Core 3.0 only runs on .NET Core 3.0</span></span>

<span data-ttu-id="30dfe-336">從 ASP.NET Core 3.0，.NET Framework 不再是支援的目標架構。</span><span class="sxs-lookup"><span data-stu-id="30dfe-336">As of ASP.NET Core 3.0, .NET Framework is no longer a supported target framework.</span></span> <span data-ttu-id="30dfe-337">以 .NET Framework 為目標的專案可以使用[.Net Core 2.1 LTS 版本](https://www.microsoft.com/net/download/dotnet-core/2.1)，以完全支援的方式繼續進行。</span><span class="sxs-lookup"><span data-stu-id="30dfe-337">Projects targeting .NET Framework can continue in a fully supported fashion using the [.NET Core 2.1 LTS release](https://www.microsoft.com/net/download/dotnet-core/2.1).</span></span> <span data-ttu-id="30dfe-338">大部分的 ASP.NET Core 2.1. x 相關套件將會無限期地提供支援，超過 .NET Core 2.1 的3年 LTS 期限。</span><span class="sxs-lookup"><span data-stu-id="30dfe-338">Most ASP.NET Core 2.1.x related packages will be supported indefinitely, beyond the 3 year LTS period for .NET Core 2.1.</span></span>

<span data-ttu-id="30dfe-339">如需遷移資訊，請參閱[將您的程式碼從 .NET Framework 移植到 .Net Core](/dotnet/core/porting/)。</span><span class="sxs-lookup"><span data-stu-id="30dfe-339">For migration information, see [Port your code from .NET Framework to .NET Core](/dotnet/core/porting/).</span></span>

## <a name="use-the-aspnet-core-shared-framework"></a><span data-ttu-id="30dfe-340">使用 ASP.NET Core 共用架構</span><span class="sxs-lookup"><span data-stu-id="30dfe-340">Use the ASP.NET Core shared framework</span></span>

<span data-ttu-id="30dfe-341">[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)中所包含的 ASP.NET Core 3.0 共用架構，不再需要專案檔中的明確`<PackageReference />`元素。</span><span class="sxs-lookup"><span data-stu-id="30dfe-341">The ASP.NET Core 3.0 shared framework, contained in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), no longer requires an explicit `<PackageReference />` element in the project file.</span></span> <span data-ttu-id="30dfe-342">在專案檔中使用`Microsoft.NET.Sdk.Web` SDK 時，會自動參考共用架構：</span><span class="sxs-lookup"><span data-stu-id="30dfe-342">The shared framework is automatically referenced when using the `Microsoft.NET.Sdk.Web` SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

## <a name="assemblies-removed-from-the-aspnet-core-shared-framework"></a><span data-ttu-id="30dfe-343">從 ASP.NET Core 共用架構移除的元件</span><span class="sxs-lookup"><span data-stu-id="30dfe-343">Assemblies removed from the ASP.NET Core shared framework</span></span>

<span data-ttu-id="30dfe-344">從 ASP.NET Core 3.0 共用架構中移除的最顯著元件如下：</span><span class="sxs-lookup"><span data-stu-id="30dfe-344">The most notable assemblies removed from the ASP.NET Core 3.0 shared framework are:</span></span>

* <span data-ttu-id="30dfe-345">[Newtonsoft. Json](https://www.nuget.org/packages/Newtonsoft.Json/) （Json.NET）。</span><span class="sxs-lookup"><span data-stu-id="30dfe-345">[Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/) (Json.NET).</span></span> <span data-ttu-id="30dfe-346">若要將 Json.NET 新增至 ASP.NET Core 3.0，請參閱[新增 Newtonsoft 以 json 為基礎的 json 格式支援](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support)。</span><span class="sxs-lookup"><span data-stu-id="30dfe-346">To add Json.NET to ASP.NET Core 3.0, see [Add Newtonsoft.Json-based JSON format support](xref:web-api/advanced/formatting#add-newtonsoftjson-based-json-format-support).</span></span> <span data-ttu-id="30dfe-347">ASP.NET Core 3.0 引進`System.Text.Json`讀取和寫入 JSON。</span><span class="sxs-lookup"><span data-stu-id="30dfe-347">ASP.NET Core 3.0 introduces `System.Text.Json` for reading and writing JSON.</span></span> <span data-ttu-id="30dfe-348">如需詳細資訊，請參閱本檔中的[新 JSON 序列化](#new-json-serialization)。</span><span class="sxs-lookup"><span data-stu-id="30dfe-348">For more information, see [New JSON serialization](#new-json-serialization) in this document.</span></span>
* [<span data-ttu-id="30dfe-349">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="30dfe-349">Entity Framework Core</span></span>](/ef/core/)

<span data-ttu-id="30dfe-350">如需從共用架構中移除之元件的完整清單，請參閱[從 3.0 AspNetCore 中移除的元件](https://github.com/aspnet/AspNetCore/issues/3755)。</span><span class="sxs-lookup"><span data-stu-id="30dfe-350">For a complete list of assemblies removed from the shared framework, see [Assemblies being removed from Microsoft.AspNetCore.App 3.0](https://github.com/aspnet/AspNetCore/issues/3755).</span></span> <span data-ttu-id="30dfe-351">如需這項變更動機的詳細資訊，請參閱[3.0 中 AspNetCore 應用程式的重大變更](https://github.com/aspnet/Announcements/issues/325)，以及[第一次介紹 ASP.NET Core 3.0](https://devblogs.microsoft.com/aspnet/a-first-look-at-changes-coming-in-asp-net-core-3-0/)中的變更。</span><span class="sxs-lookup"><span data-stu-id="30dfe-351">For more information on the motivation for this change, see [Breaking changes to Microsoft.AspNetCore.App in 3.0](https://github.com/aspnet/Announcements/issues/325) and [A first look at changes coming in ASP.NET Core 3.0](https://devblogs.microsoft.com/aspnet/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span></span>

<!-- 
## Additional information
For the complete list of changes, see the [ASP.NET Core 3.0 Release Notes](WHERE IS THIS????).
-->