---
title: ASP.NET Core 簡介
author: rick-anderson
description: 取得 ASP.NET Core 的簡介，ASP.NET Core 是一種跨平台且高效能的開放原始碼架構，用於建置現代化、雲端式、網際網路連線的應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- SignalR
uid: index
ms.openlocfilehash: fd7fa9dd70502f51222e457dd887ef668d377278
ms.sourcegitcommit: 4b166b49ec557a03f99f872dd069ca5e56faa524
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/27/2020
ms.locfileid: "80366656"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="d0aa5-103">ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="d0aa5-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="d0aa5-104">由 [Daniel Roth](https://github.com/danroth27)、[Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Shaun Luttin](https://twitter.com/dicshaunary) 提供</span><span class="sxs-lookup"><span data-stu-id="d0aa5-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="d0aa5-105">ASP.NET Core 是一種跨平台且高效能的[開放原始碼](https://github.com/aspnet/home)架構，用於建置現代化、雲端式、網際網路連線的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="d0aa5-106">利用 ASP.NET Core，您可以：</span><span class="sxs-lookup"><span data-stu-id="d0aa5-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="d0aa5-107">建置 Web 應用程式和服務、[IoT](https://www.microsoft.com/internet-of-things/) 應用程式，以及行動後端。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="d0aa5-108">在 Windows、macOS 和 Linux 上使用您最愛的開發工具。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="d0aa5-109">部署到雲端或在內部部署。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="d0aa5-110">在 [.NET Core 或 .NET Framework](/dotnet/articles/standard/choosing-core-framework-server) 上執行。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-110">Run on [.NET Core or .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-choose-aspnet-core"></a><span data-ttu-id="d0aa5-111">為什麼要選擇 ASP.NET Core？</span><span class="sxs-lookup"><span data-stu-id="d0aa5-111">Why choose ASP.NET Core?</span></span>

<span data-ttu-id="d0aa5-112">數百萬名開發人員使用或已使用[ASP.NET](/aspnet/overview) 4.x 來建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-112">Millions of developers use or have used [ASP.NET 4.x](/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="d0aa5-113">ASP.NET Core 是 ASP.NET 4.x 的重新設計，其架構變更可產生更為精簡且更加模組化的架構。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="d0aa5-114">使用 ASP.NET Core MVC 建置 Web API 和 Web UI</span><span class="sxs-lookup"><span data-stu-id="d0aa5-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="d0aa5-115">ASP.NET Core MVC 提供了建置 [Web API](xref:tutorials/first-web-api) 和 [Web 應用程式](xref:tutorials/razor-pages/index)的功能：</span><span class="sxs-lookup"><span data-stu-id="d0aa5-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/first-web-api) and [web apps](xref:tutorials/razor-pages/index):</span></span>

* <span data-ttu-id="d0aa5-116">[模型檢視控制器 (MVC) 模式](xref:mvc/overview)有助於讓您的 Web API 和 Web 應用程式可測試。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps testable.</span></span>
* <span data-ttu-id="d0aa5-117">[Razor Pages](xref:razor-pages/index) 是以頁面為基礎的程式設計模型，可讓建置 Web UI 更輕鬆與更具生產力。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-117">[Razor Pages](xref:razor-pages/index) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="d0aa5-118">[Razor 標記](xref:mvc/views/razor)提供了適用於 [Razor 頁面](xref:razor-pages/index)和 [MVC 檢視](xref:mvc/views/overview)的高效率語法。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="d0aa5-119">[標記協助程式](xref:mvc/views/tag-helpers/intro)可啟用伺服器端程式碼，以參與建立和轉譯 Razor 檔案中的 HTML 元素。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="d0aa5-120">[多個資料格式和內容交涉](xref:web-api/advanced/formatting)的內建支援可讓您的 Web API 連線到各種用戶端，包括瀏覽器和行動裝置。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="d0aa5-121">[模型繫結](xref:mvc/models/model-binding)會自動將 HTTP 要求中的資料對應至動作方法參數。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="d0aa5-122">[模型驗證](xref:mvc/models/validation)會自動執行用戶端和伺服器端驗證。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="d0aa5-123">用戶端開發</span><span class="sxs-lookup"><span data-stu-id="d0aa5-123">Client-side development</span></span>

<span data-ttu-id="d0aa5-124">ASP.NET Core 可完美整合常用的用戶端架構和程式庫，包括 [Blazor](xref:blazor/index)、[Angular](xref:spa/angular)、[React](xref:spa/react) 與 [Bootstrap](https://getbootstrap.com/)。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Blazor](xref:blazor/index), [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="d0aa5-125">如需詳細資訊，請參閱 <xref:blazor/index> 及*用戶端開發*下的相關主題。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-125">For more information, see <xref:blazor/index> and related topics under *Client-side development*.</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="d0aa5-126">將目標指向 .NET Framework 的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d0aa5-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="d0aa5-127">ASP.NET Core 2.x 的目標可以是 NET Core 或 .NET Framework。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-127">ASP.NET Core 2.x can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="d0aa5-128">將目標指向 .NET Framework 的 ASP.NET Core 應用程式無法跨平台&mdash;而只能在 Windows 上執行。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="d0aa5-129">ASP.NET Core 2.x 通常會包含 [.NET Standard](/dotnet/standard/net-standard) 程式庫。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-129">Generally, ASP.NET Core 2.x is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="d0aa5-130">使用 .NET Standard 2.0 撰寫的程式庫可在[任何實作 .NET Standard 2.0 的 .NET 平台](/dotnet/standard/net-standard#net-implementation-support)上執行。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-130">Libraries written with .NET Standard 2.0 run on any [.NET platform that implements .NET Standard 2.0](/dotnet/standard/net-standard#net-implementation-support).</span></span>

<span data-ttu-id="d0aa5-131">實作 .NET Standard 2.0 的 .NET Framework 版本支援 ASP.NET Core 2.x：</span><span class="sxs-lookup"><span data-stu-id="d0aa5-131">ASP.NET Core 2.x is supported on .NET Framework versions that implement .NET Standard 2.0:</span></span>

* <span data-ttu-id="d0aa5-132">強烈建議使用最新版本的 .NET Framework。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-132">.NET Framework latest version is strongly recommended.</span></span>
* <span data-ttu-id="d0aa5-133">.NET Framework 4.6.1 和更新版本。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-133">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="d0aa5-134">ASP.NET Core 3.0 及更新版本只可在.NET Core 上執行。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-134">ASP.NET Core 3.0 and later will only run on .NET Core.</span></span> <span data-ttu-id="d0aa5-135">如需此變更的詳細資料，請參閱[A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/) (搶先看 ASP.NET Core 3.0 的變更)。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-135">For more details regarding this change, see [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span></span>

<span data-ttu-id="d0aa5-136">將目標指向 .NET Core 有多個好處，而這些好處也隨著版本更新越來越多。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-136">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="d0aa5-137">NET Core 較 .NET Framework 多的好處包含：</span><span class="sxs-lookup"><span data-stu-id="d0aa5-137">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="d0aa5-138">跨平台。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-138">Cross-platform.</span></span> <span data-ttu-id="d0aa5-139">可在 macOS、Linux 及 Windows 上執行。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-139">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="d0aa5-140">提升效能</span><span class="sxs-lookup"><span data-stu-id="d0aa5-140">Improved performance</span></span>
* [<span data-ttu-id="d0aa5-141">並存版本控制</span><span class="sxs-lookup"><span data-stu-id="d0aa5-141">Side-by-side versioning</span></span>](/dotnet/standard/choosing-core-framework-server#a-need-for-side-by-side-of-net-versions-per-application-level)
* <span data-ttu-id="d0aa5-142">新的 API</span><span class="sxs-lookup"><span data-stu-id="d0aa5-142">New APIs</span></span>
* <span data-ttu-id="d0aa5-143">開放原始碼</span><span class="sxs-lookup"><span data-stu-id="d0aa5-143">Open source</span></span>

<span data-ttu-id="d0aa5-144">我們正致力於縮短 .NET Framework 與 .NET Core 之間的 API 差距。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-144">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="d0aa5-145">[Windows 相容性套件](/dotnet/core/porting/windows-compat-pack)在 .NET Core 中發佈了上千個僅供 Windows 使用的 API。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-145">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="d0aa5-146">這些 API 並不適用於 .NET Core 1.x。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-146">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="recommended-learning-path"></a><span data-ttu-id="d0aa5-147">建議學習路徑</span><span class="sxs-lookup"><span data-stu-id="d0aa5-147">Recommended learning path</span></span>

<span data-ttu-id="d0aa5-148">我們建議遵循一系列的教學課程和文章，取得開發 ASP.NET Core 應用程式的簡介：</span><span class="sxs-lookup"><span data-stu-id="d0aa5-148">We recommend the following sequence of tutorials and articles for an introduction to developing ASP.NET Core apps:</span></span>

1. <span data-ttu-id="d0aa5-149">遵循您想要開發或維護應用程式類型的教學課程：</span><span class="sxs-lookup"><span data-stu-id="d0aa5-149">Follow a tutorial for the type of app you want to develop or maintain:</span></span>

   |<span data-ttu-id="d0aa5-150">應用程式類型</span><span class="sxs-lookup"><span data-stu-id="d0aa5-150">App type</span></span>  |<span data-ttu-id="d0aa5-151">狀況</span><span class="sxs-lookup"><span data-stu-id="d0aa5-151">Scenario</span></span>  |<span data-ttu-id="d0aa5-152">教學課程</span><span class="sxs-lookup"><span data-stu-id="d0aa5-152">Tutorial</span></span>  |
   |----------|----------|----------|
   |<span data-ttu-id="d0aa5-153">Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="d0aa5-153">Web app</span></span>                   | <span data-ttu-id="d0aa5-154">針對全新開發</span><span class="sxs-lookup"><span data-stu-id="d0aa5-154">For new development</span></span>        |[<span data-ttu-id="d0aa5-155">開始使用 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="d0aa5-155">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start) |
   |<span data-ttu-id="d0aa5-156">Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="d0aa5-156">Web app</span></span>                   | <span data-ttu-id="d0aa5-157">針對維護 MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="d0aa5-157">For maintaining an MVC app</span></span> |[<span data-ttu-id="d0aa5-158">開始使用 MVC</span><span class="sxs-lookup"><span data-stu-id="d0aa5-158">Get started with MVC</span></span>](xref:tutorials/first-mvc-app/start-mvc)|
   |<span data-ttu-id="d0aa5-159">Web API</span><span class="sxs-lookup"><span data-stu-id="d0aa5-159">Web API</span></span>                   |                            |<span data-ttu-id="d0aa5-160">[建立 Web API](xref:tutorials/first-web-api)\*</span><span class="sxs-lookup"><span data-stu-id="d0aa5-160">[Create a web API](xref:tutorials/first-web-api)\*</span></span>  |
   |<span data-ttu-id="d0aa5-161">即時應用程式</span><span class="sxs-lookup"><span data-stu-id="d0aa5-161">Real-time app</span></span>             |                            |[<span data-ttu-id="d0aa5-162">開始使用 SignalR</span><span class="sxs-lookup"><span data-stu-id="d0aa5-162">Get started with SignalR</span></span>](xref:tutorials/signalr) |
   |<span data-ttu-id="d0aa5-163">Blazor 應用程式</span><span class="sxs-lookup"><span data-stu-id="d0aa5-163">Blazor app</span></span>                |                            |[<span data-ttu-id="d0aa5-164">開始使用 Blazor</span><span class="sxs-lookup"><span data-stu-id="d0aa5-164">Get started with Blazor</span></span>](xref:blazor/get-started) |
   |<span data-ttu-id="d0aa5-165">遠端程序呼叫應用程式</span><span class="sxs-lookup"><span data-stu-id="d0aa5-165">Remote Procedure Call app</span></span> |                            |[<span data-ttu-id="d0aa5-166">開始使用 gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="d0aa5-166">Get started with a gRPC service</span></span>](xref:tutorials/grpc/grpc-start) |

1. <span data-ttu-id="d0aa5-167">遵循示範如何進行基本資料存取的教學課程：</span><span class="sxs-lookup"><span data-stu-id="d0aa5-167">Follow a tutorial that shows how to do basic data access:</span></span>

   |<span data-ttu-id="d0aa5-168">狀況</span><span class="sxs-lookup"><span data-stu-id="d0aa5-168">Scenario</span></span>  |<span data-ttu-id="d0aa5-169">教學課程</span><span class="sxs-lookup"><span data-stu-id="d0aa5-169">Tutorial</span></span>  |
   |----------|----------|
   | <span data-ttu-id="d0aa5-170">針對全新開發</span><span class="sxs-lookup"><span data-stu-id="d0aa5-170">For new development</span></span>        |[<span data-ttu-id="d0aa5-171">搭配 Entity Framework Core 的 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="d0aa5-171">Razor Pages with Entity Framework Core</span></span>](xref:data/ef-rp/intro) |
   | <span data-ttu-id="d0aa5-172">針對維護 MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="d0aa5-172">For maintaining an MVC app</span></span> |[<span data-ttu-id="d0aa5-173">搭配 Entity Framework Core 的 MVC</span><span class="sxs-lookup"><span data-stu-id="d0aa5-173">MVC with Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

1. <span data-ttu-id="d0aa5-174">閱讀適用於所有應用程式類型的 ASP.NET Core 功能概觀：</span><span class="sxs-lookup"><span data-stu-id="d0aa5-174">Read an overview of ASP.NET Core features that apply to all app types:</span></span>

   * [<span data-ttu-id="d0aa5-175">基礎</span><span class="sxs-lookup"><span data-stu-id="d0aa5-175">Fundamentals</span></span>](xref:fundamentals/index)

1. <span data-ttu-id="d0aa5-176">瀏覽其他您感興趣主題的目錄。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-176">Browse the Table of Contents for other topics of interest.</span></span>

<span data-ttu-id="d0aa5-177">\*目前已有新的 [Web API 教學課程，可讓您在瀏覽器中完整地遵循](https://docs.microsoft.com/learn/modules/build-web-api-net-core)，而無須進行本機 IDE 安裝。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-177">\* There is a new [web API tutorial that you follow entirely in the browser](https://docs.microsoft.com/learn/modules/build-web-api-net-core), no local IDE installation required.</span></span>  <span data-ttu-id="d0aa5-178">程式碼會在 [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/) 中執行，且會使用 [curl](https://curl.haxx.se/) 來進行測試。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-178">The code runs in an [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/), and [curl](https://curl.haxx.se/) is used for testing.</span></span>

## <a name="migration-from-the-net-framework"></a><span data-ttu-id="d0aa5-179">從 .NET Framework 遷移</span><span class="sxs-lookup"><span data-stu-id="d0aa5-179">Migration from the .NET Framework</span></span>

<span data-ttu-id="d0aa5-180">如需將 ASP.NET 應用程式遷移至 ASP.NET Core 的參考指南，請參閱 <xref:migration/proper-to-2x/index>。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-180">For a reference guide to migrating ASP.NET apps to ASP.NET Core, see <xref:migration/proper-to-2x/index>.</span></span>

## <a name="how-to-download-a-sample"></a><span data-ttu-id="d0aa5-181">如何下載範例</span><span class="sxs-lookup"><span data-stu-id="d0aa5-181">How to download a sample</span></span>

<span data-ttu-id="d0aa5-182">許多文章及教學課程都有包含範例程式碼的連結。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-182">Many of the articles and tutorials include links to sample code.</span></span>

1. <span data-ttu-id="d0aa5-183">[下載 ASP.NET 存放庫 ZIP 檔案](https://codeload.github.com/dotnet/AspNetCore.Docs/zip/master)。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-183">[Download the ASP.NET repository zip file](https://codeload.github.com/dotnet/AspNetCore.Docs/zip/master).</span></span>
1. <span data-ttu-id="d0aa5-184">解壓縮 *Docs-master.zip* 檔案。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-184">Unzip the *Docs-master.zip* file.</span></span>
1. <span data-ttu-id="d0aa5-185">使用範例連結中的 URL，協助您巡覽至範例目錄。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-185">Use the URL in the sample link to help you navigate to the sample directory.</span></span>

### <a name="preprocessor-directives-in-sample-code"></a><span data-ttu-id="d0aa5-186">範例程式碼中的前置處理器指示詞</span><span class="sxs-lookup"><span data-stu-id="d0aa5-186">Preprocessor directives in sample code</span></span>

<span data-ttu-id="d0aa5-187">為了示範多個案例，範例應用程式會使用 `#define` 並 `#if-#else/#elif-#endif` 預處理器指示詞，以選擇性地編譯和執行範例程式碼的不同區段。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-187">To demonstrate multiple scenarios, sample apps use the `#define` and `#if-#else/#elif-#endif` preprocessor directives to selectively compile and run different sections of sample code.</span></span> <span data-ttu-id="d0aa5-188">針對使用這種方法的範例，請在檔案頂端C#設定 `#define` 指示詞，以定義與您要執行之情節相關聯的符號。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-188">For those samples that make use of this approach, set the `#define` directive at the top of the C# files to define the symbol associated with the scenario that you want to run.</span></span> <span data-ttu-id="d0aa5-189">有些範例需要在多個檔案的頂端定義符號，才能執行案例。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-189">Some samples require defining the symbol at the top of multiple files in order to run a scenario.</span></span>

<span data-ttu-id="d0aa5-190">例如下列 `#define` 符號清單指出其提供四個情節 (每個符號一個情節)。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-190">For example, the following `#define` symbol list indicates that four scenarios are available (one scenario per symbol).</span></span> <span data-ttu-id="d0aa5-191">目前的範例設定會執行 `TemplateCode` 情節：</span><span class="sxs-lookup"><span data-stu-id="d0aa5-191">The current sample configuration runs the `TemplateCode` scenario:</span></span>

```csharp
#define TemplateCode // or LogFromMain or ExpandDefault or FilterInCode
```

<span data-ttu-id="d0aa5-192">若要變更此範例，以執行 `ExpandDefault` 情節，請定義 `ExpandDefault` 符號，並將剩餘的符號設為註解：</span><span class="sxs-lookup"><span data-stu-id="d0aa5-192">To change the sample to run the `ExpandDefault` scenario, define the `ExpandDefault` symbol and leave the remaining symbols commented-out:</span></span>

```csharp
#define ExpandDefault // TemplateCode or LogFromMain or FilterInCode
```

<span data-ttu-id="d0aa5-193">如需如何使用 [C# 前置處理器指示詞](/dotnet/csharp/language-reference/preprocessor-directives/)，以選擇編譯不同之程式碼區段的詳細資訊，請參閱 [#define (C# 參考)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) 及 [#if (C# 參考)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if)。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-193">For more information on using [C# preprocessor directives](/dotnet/csharp/language-reference/preprocessor-directives/) to selectively compile sections of code, see [#define (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) and [#if (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span></span>

### <a name="regions-in-sample-code"></a><span data-ttu-id="d0aa5-194">範例程式碼中的區域</span><span class="sxs-lookup"><span data-stu-id="d0aa5-194">Regions in sample code</span></span>

<span data-ttu-id="d0aa5-195">某些範例應用程式包含以[#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region)和[#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) C#指示詞括住的程式碼區段。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-195">Some sample apps contain sections of code surrounded by [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) and [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) C# directives.</span></span> <span data-ttu-id="d0aa5-196">文件建置系統會將這些區域插入轉譯的文件主題中。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-196">The documentation build system injects these regions into the rendered documentation topics.</span></span>  

<span data-ttu-id="d0aa5-197">區域名稱通常包含字組 "snippet"。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-197">Region names usually contain the word "snippet."</span></span> <span data-ttu-id="d0aa5-198">下列範例顯示了名為 `snippet_WebHostDefaults` 的區域：</span><span class="sxs-lookup"><span data-stu-id="d0aa5-198">The following example shows a region named `snippet_WebHostDefaults`:</span></span>

```csharp
#region snippet_WebHostDefaults
Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.UseStartup<Startup>();
    });
#endregion
```

<span data-ttu-id="d0aa5-199">上述的 C# 程式碼片段會在主題的 Markdown 檔案中，透過以下程式行加以參考：</span><span class="sxs-lookup"><span data-stu-id="d0aa5-199">The preceding C# code snippet is referenced in the topic's markdown file with the following line:</span></span>

```md
[!code-csharp[](sample/SampleApp/Program.cs?name=snippet_WebHostDefaults)]
```

<span data-ttu-id="d0aa5-200">您可以放心地忽略（或移除）程式碼周圍的 `#region` 和 `#endregion` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-200">You may safely ignore (or remove) the `#region` and `#endregion` directives that surround the code.</span></span> <span data-ttu-id="d0aa5-201">如果您打算執行主題中所述的範例案例，請不要更改這些指示詞內的程式碼。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-201">Don't alter the code within these directives if you plan to run the sample scenarios described in the topic.</span></span> <span data-ttu-id="d0aa5-202">您可以在試驗其他案例時自由改變程式碼。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-202">Feel free to alter the code when experimenting with other scenarios.</span></span>

<span data-ttu-id="d0aa5-203">如需詳細資訊，請參閱 [Contribute to the ASP.NET documentation: Code snippets](https://github.com/dotnet/AspNetCore.Docs/blob/master/CONTRIBUTING.md#code-snippets) (參與 ASP.NET 文件：程式碼片段)。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-203">For more information, see [Contribute to the ASP.NET documentation: Code snippets](https://github.com/dotnet/AspNetCore.Docs/blob/master/CONTRIBUTING.md#code-snippets).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d0aa5-204">後續步驟</span><span class="sxs-lookup"><span data-stu-id="d0aa5-204">Next steps</span></span>

<span data-ttu-id="d0aa5-205">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="d0aa5-205">For more information, see the following resources:</span></span>

* <xref:getting-started>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="d0aa5-206">ASP.NET Core 基本概念</span><span class="sxs-lookup"><span data-stu-id="d0aa5-206">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="d0aa5-207">[每週的 ASP.NET 社群之聲](https://live.asp.net/) \(英文\) 涵蓋了小組的進度和計劃，</span><span class="sxs-lookup"><span data-stu-id="d0aa5-207">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="d0aa5-208">並提供新的部落格和協力廠商軟體。</span><span class="sxs-lookup"><span data-stu-id="d0aa5-208">It features new blogs and third-party software.</span></span>