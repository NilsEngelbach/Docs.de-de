---
title: ASP.NET Core-Middleware
author: rick-anderson
description: "Erfahren Sie mehr über ASP.NET Core-Middleware und die Anforderungspipeline."
manager: wpickett
ms.author: riande
ms.date: 01/22/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/middleware/index
ms.openlocfilehash: 158f11875f22f8f9dba6f7f109123717b9da8d18
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2018
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="a3168-103">ASP.NET Core-Middleware</span><span class="sxs-lookup"><span data-stu-id="a3168-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="a3168-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="a3168-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="a3168-105">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a3168-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-middleware"></a><span data-ttu-id="a3168-106">Was ist Middleware?</span><span class="sxs-lookup"><span data-stu-id="a3168-106">What is middleware?</span></span>

<span data-ttu-id="a3168-107">Middleware ist Software, die zu einer Anwendungspipeline zusammengesetzt wird, um Anforderungen und Antworten zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="a3168-107">Middleware is software that's assembled into an application pipeline to handle requests and responses.</span></span> <span data-ttu-id="a3168-108">Jede Komponente kann Folgendes ausführen:</span><span class="sxs-lookup"><span data-stu-id="a3168-108">Each component:</span></span>

* <span data-ttu-id="a3168-109">Entscheiden, ob die Anforderung an die nächste Komponente in der Pipeline übergeben werden soll.</span><span class="sxs-lookup"><span data-stu-id="a3168-109">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="a3168-110">Arbeiten, bevor oder nachdem die nächste Komponente in der Pipeline aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="a3168-110">Can perform work before and after the next component in the pipeline is invoked.</span></span> 

<span data-ttu-id="a3168-111">Anforderungsdelegaten werden verwendet, um die Anforderungspipeline zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="a3168-111">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="a3168-112">Die Anforderungsdelegaten behandeln jede HTTP-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="a3168-112">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="a3168-113">Sie werden mit [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions)-, [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions)- und [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions)-Erweiterungsmethoden konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="a3168-113">Request delegates are configured using [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), and [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) extension methods.</span></span> <span data-ttu-id="a3168-114">Ein einzelner Anforderungsdelegat kann inline als anonyme Methode angegeben werden (sogenannte Inline-Middleware), oder er kann in einer wiederverwendbaren Klasse definiert werden.</span><span class="sxs-lookup"><span data-stu-id="a3168-114">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="a3168-115">Diese wiederverwendbaren Klassen und anonymen Inline-Methoden sind *Middleware* oder *Middlewarekomponenten*.</span><span class="sxs-lookup"><span data-stu-id="a3168-115">These reusable classes and in-line anonymous methods are *middleware*, or *middleware components*.</span></span> <span data-ttu-id="a3168-116">Jede Middlewarekomponente in der Anforderungspipeline ist für das Aufrufen der jeweils nächsten Komponente in der Pipeline oder für das Kurzschließen der Kette, wenn nötig, zuständig.</span><span class="sxs-lookup"><span data-stu-id="a3168-116">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline, or short-circuiting the chain if appropriate.</span></span>

<span data-ttu-id="a3168-117">Unter [Migrating HTTP Modules to Middleware (Migrieren von HTTP-Modulen zu Middleware)](xref:migration/http-modules) wird der Unterschied zwischen Anforderungspipelines in ASP.NET Core und ASP.NET 4.x erklärt. Außerdem werden dort weitere Beispiele für Middleware gegeben.</span><span class="sxs-lookup"><span data-stu-id="a3168-117">[Migrating HTTP Modules to Middleware](xref:migration/http-modules) explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="a3168-118">Erstellen einer Middlewarepipeline mit IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="a3168-118">Creating a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="a3168-119">Die ASP.NET Core-Anforderungspipeline besteht, wie in folgendem Diagramm veranschaulicht, aus einer Sequenz von Anforderungsdelegaten, die nacheinander aufgerufen werden (der Ausführungsthread wird durch die schwarzen Pfeile dargestellt):</span><span class="sxs-lookup"><span data-stu-id="a3168-119">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other, as this diagram shows (the thread of execution follows the black arrows):</span></span>

![Anforderungsverarbeitungsmuster mit eingehender Anforderung, deren Verarbeitung von drei Middlewares und die ausgehende Antwort der Anwendung.](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="a3168-123">Jeder Delegat kann Vorgänge vor und nach dem nächsten Delegaten ausführen.</span><span class="sxs-lookup"><span data-stu-id="a3168-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="a3168-124">Ein Delegat kann sich auch dagegen entscheiden, eine Anforderung an den nächsten Delegaten zu übergeben. Dies wird als Kurzschluss einer Anforderungspipeline bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="a3168-124">A delegate can also decide to not pass a request to the next delegate, which is called short-circuiting the request pipeline.</span></span> <span data-ttu-id="a3168-125">Das Kurzschließen ist oft sinnvoll, da es unnötige Arbeit verhindert.</span><span class="sxs-lookup"><span data-stu-id="a3168-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="a3168-126">Die Middleware für statische Dateien kann z.B. eine Anforderung einer statischen Datei zurückgeben und den Rest der Pipeline kurzschließen.</span><span class="sxs-lookup"><span data-stu-id="a3168-126">For example, the static file middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="a3168-127">Die Ausnahmebehandlungsdelegaten müssen am Anfang der Pipeline aufgerufen werden, sodass sie Ausnahmen abfangen können, die zu einem späteren Zeitpunkt in der Pipeline ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="a3168-127">Exception-handling delegates need to be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="a3168-128">Die einfachste mögliche ASP.NET Core-App enthält einen einzigen Anforderungsdelegaten, der alle Anforderungen verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="a3168-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="a3168-129">In diesem Fall ist keine tatsächliche Anforderungspipeline vorhanden.</span><span class="sxs-lookup"><span data-stu-id="a3168-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="a3168-130">Stattdessen wird eine einzelne anonyme Funktion als Antwort auf jede HTTP-Anforderung aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="a3168-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[Main](index/sample/Middleware/Startup.cs)]

<span data-ttu-id="a3168-131">Der erste [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions)-Delegat beendet die Pipeline.</span><span class="sxs-lookup"><span data-stu-id="a3168-131">The first [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegate terminates the pipeline.</span></span>

<span data-ttu-id="a3168-132">Mit [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) können Sie mehrere Anforderungedelegate miteinander verknüpfen.</span><span class="sxs-lookup"><span data-stu-id="a3168-132">You can chain multiple request delegates together with [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span></span> <span data-ttu-id="a3168-133">Der Parameter `next` steht für den nächsten Delegaten in der Pipeline.</span><span class="sxs-lookup"><span data-stu-id="a3168-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="a3168-134">(Denken Sie daran, dass Sie die Pipeline kurzschließen können, indem Sie den Parameter *next* *nicht* aufrufen.) Normalerweise können Sie Aktionen sowohl vor als auch nach dem nächsten Delegaten durchführen. Dies wird in folgendem Beispiel veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="a3168-134">(Remember that you can short-circuit the pipeline by *not* calling the *next* parameter.) You can typically perform actions both before and after the next delegate, as this example demonstrates:</span></span>

[!code-csharp[Main](index/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> <span data-ttu-id="a3168-135">Rufen Sie `next.Invoke` nicht auf, nachdem die Antwort an den Client gesendet wurde.</span><span class="sxs-lookup"><span data-stu-id="a3168-135">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="a3168-136">An `HttpResponse` vorgenommene Änderungen lösen nach dem Start der Antwort eine Ausnahme aus.</span><span class="sxs-lookup"><span data-stu-id="a3168-136">Changes to `HttpResponse` after the response has started will throw an exception.</span></span> <span data-ttu-id="a3168-137">Änderungen wie das Festlegen von Headern oder Statuscode usw. lösen beispielsweise eine Ausnahme aus.</span><span class="sxs-lookup"><span data-stu-id="a3168-137">For example, changes such as setting headers, status code, etc,  will throw an exception.</span></span> <span data-ttu-id="a3168-138">Wenn Sie nach dem Aufruf von `next` in den Antworttext schreiben, kann dies:</span><span class="sxs-lookup"><span data-stu-id="a3168-138">Writing to the response body after calling `next`:</span></span>
> - <span data-ttu-id="a3168-139">einen Protokollverstoß verursachen,</span><span class="sxs-lookup"><span data-stu-id="a3168-139">May cause a protocol violation.</span></span> <span data-ttu-id="a3168-140">wenn Sie z.B. mehr als das genannte `content-length`-Objekt schreiben.</span><span class="sxs-lookup"><span data-stu-id="a3168-140">For example, writing more than the stated `content-length`.</span></span>
> - <span data-ttu-id="a3168-141">Fehler im Textformat auslösen,</span><span class="sxs-lookup"><span data-stu-id="a3168-141">May corrupt the body format.</span></span> <span data-ttu-id="a3168-142">wenn Sie z.B. eine HTML-Fußzeile in eine CSS-Datei schreiben.</span><span class="sxs-lookup"><span data-stu-id="a3168-142">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="a3168-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) ist ein nützlicher Hinweis, der angibt, ob Header gesendet wurden und/oder ob in den Text geschrieben wurde.</span><span class="sxs-lookup"><span data-stu-id="a3168-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) is a useful hint to indicate if headers have been sent and/or the body has been written to.</span></span>

## <a name="ordering"></a><span data-ttu-id="a3168-144">Sortieren</span><span class="sxs-lookup"><span data-stu-id="a3168-144">Ordering</span></span>

<span data-ttu-id="a3168-145">Die Reihenfolge, in der Middlewarekomponenten in der `Configure`-Methode hinzugefügt werden, legt die Reihenfolge fest, in der sie bei Anforderungen aufgerufen werden. Bei Antworten gilt die umgekehrte Reihenfolge.</span><span class="sxs-lookup"><span data-stu-id="a3168-145">The order that middleware components are added in the `Configure` method defines the order in which they're invoked on requests, and the reverse order for the response.</span></span> <span data-ttu-id="a3168-146">Diese Reihenfolge trägt wesentlich zur Sicherheit, Leistung und Funktionalität bei.</span><span class="sxs-lookup"><span data-stu-id="a3168-146">This ordering is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="a3168-147">Die Methode „Configure“ (siehe unten) fügt die folgenden Middlewarekomponenten hinzu:</span><span class="sxs-lookup"><span data-stu-id="a3168-147">The Configure method (shown below) adds the following middleware components:</span></span>

1. <span data-ttu-id="a3168-148">Ausnahme-/Fehlerbehandlung</span><span class="sxs-lookup"><span data-stu-id="a3168-148">Exception/error handling</span></span>
2. <span data-ttu-id="a3168-149">Statischer Dateiserver</span><span class="sxs-lookup"><span data-stu-id="a3168-149">Static file server</span></span>
3. <span data-ttu-id="a3168-150">Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="a3168-150">Authentication</span></span>
4. <span data-ttu-id="a3168-151">MVC</span><span class="sxs-lookup"><span data-stu-id="a3168-151">MVC</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a3168-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a3168-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseAuthentication();               // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a3168-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a3168-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseIdentity();                     // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

-----------

<span data-ttu-id="a3168-154">Im obenstehenden Code ist `UseExceptionHandler` die erste zur Pipeline hinzugefügte Middlewarekomponente. Deshalb fängt sie alle Ausnahmen ab, die in späteren Aufrufen ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="a3168-154">In the code above, `UseExceptionHandler` is the first middleware component added to the pipeline—therefore, it catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="a3168-155">Die Middeware für statische Dateien wird am Anfang der Pipeline aufgerufen, damit sie Anforderungen und Kurzschlüsse verarbeiten kann, ohne dass die verbleibenden Komponenten durchlaufen werden müssen.</span><span class="sxs-lookup"><span data-stu-id="a3168-155">The static file middleware is called early in the pipeline so it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="a3168-156">Die Middleware für statische Dateien stellt **keine** Autorisierungsüberprüfungen bereit.</span><span class="sxs-lookup"><span data-stu-id="a3168-156">The static file middleware provides **no** authorization checks.</span></span> <span data-ttu-id="a3168-157">Alle Dateien, die von ihr bearbeitet werden, Dateien unter *wwwroot* inbegriffen, sind öffentlich verfügbar.</span><span class="sxs-lookup"><span data-stu-id="a3168-157">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="a3168-158">Unter [Arbeiten mit statischen Dateien](xref:fundamentals/static-files) erfahren Sie, wie Sie statische Dateien sichern können.</span><span class="sxs-lookup"><span data-stu-id="a3168-158">See [Working with static files](xref:fundamentals/static-files) for an approach to secure static files.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a3168-159">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a3168-159">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


<span data-ttu-id="a3168-160">Wenn die Anforderung nicht von der Middleware für statische Dateien verarbeitet wird, wird sie an die Identity-Middleware (`app.UseAuthentication`) übergeben, welche die Authentifizierung durchführt.</span><span class="sxs-lookup"><span data-stu-id="a3168-160">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseAuthentication`), which performs authentication.</span></span> <span data-ttu-id="a3168-161">Identity schließt keine unautorisierten Anforderungen kurz.</span><span class="sxs-lookup"><span data-stu-id="a3168-161">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="a3168-162">Auch wenn Identity Anforderungen authentifiziert, erfolgt die Autorisierung (und Ablehnung) erst dann, wenn MVC eine spezifische Razor-Seite oder einen Controller und eine Aktion ausgewählt hat.</span><span class="sxs-lookup"><span data-stu-id="a3168-162">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific Razor Page or controller and action.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a3168-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a3168-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a3168-164">Wenn die Anforderung nicht von der Middleware für statische Dateien verarbeitet wird, wird sie an die Identity-Middleware (`app.UseIdentity`) übergeben, welche die Authentifizierung durchführt.</span><span class="sxs-lookup"><span data-stu-id="a3168-164">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseIdentity`), which performs authentication.</span></span> <span data-ttu-id="a3168-165">Identity schließt keine unautorisierten Anforderungen kurz.</span><span class="sxs-lookup"><span data-stu-id="a3168-165">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="a3168-166">Auch wenn Identity Anforderungen authentifiziert, erfolgt die Autorisierung (und Ablehnung) erst dann, wenn  MVC einen spezifischen Controller und eine Aktion ausgewählt hat.</span><span class="sxs-lookup"><span data-stu-id="a3168-166">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

-----------

<span data-ttu-id="a3168-167">Im folgenden Beispiel wird eine Middlewarereihenfolge veranschaulicht, bei der Anforderungen für statische Dateien von der Middleware für statische Dateien vor der Middleware für die Antwortkomprimierung verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="a3168-167">The following example demonstrates a middleware ordering where requests for static files are handled by the static file middleware before the response compression middleware.</span></span> <span data-ttu-id="a3168-168">Statische Dateien werden durch diese Middlewarereihenfolge nicht komprimiert.</span><span class="sxs-lookup"><span data-stu-id="a3168-168">Static files are not compressed with this ordering of the middleware.</span></span> <span data-ttu-id="a3168-169">Die MVC-Antworten von [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) können komprimiert werden.</span><span class="sxs-lookup"><span data-stu-id="a3168-169">The MVC responses from [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name="middleware-run-map-use"></a>

### <a name="use-run-and-map"></a><span data-ttu-id="a3168-170">Use, Run und Map</span><span class="sxs-lookup"><span data-stu-id="a3168-170">Use, Run, and Map</span></span>

<span data-ttu-id="a3168-171">Sie können die HTTP-Pipeline mit `Use`, `Run` und `Map` konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a3168-171">You configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="a3168-172">Die `Use`-Methode kann die Pipeline kurzschließen (wenn sie keinen `next`-Anforderungsdelegaten aufruft).</span><span class="sxs-lookup"><span data-stu-id="a3168-172">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="a3168-173">`Run` ist eine Konvention. Einige Middlewarekomponenten machen möglicherweise `Run[Middleware]`-Methoden verfügbar, die am Ende einer Pipeline ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="a3168-173">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="a3168-174">`Map*`-Erweiterungen werden als Konvention zum Branchen der Pipeline verwendet.</span><span class="sxs-lookup"><span data-stu-id="a3168-174">`Map*` extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="a3168-175">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) brancht die Anforderungspipeline auf Grundlage von Übereinstimmungen des angegebenen Anforderungspfads.</span><span class="sxs-lookup"><span data-stu-id="a3168-175">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="a3168-176">Wenn der Anforderungspfad mit dem angegebenen Pfad beginnt, wird der Branch ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="a3168-176">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[Main](index/sample/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="a3168-177">In der folgenden Tabelle sind die Anforderungen und Antworten von `http://localhost:1234` mit dem oben stehenden Code aufgelistet:</span><span class="sxs-lookup"><span data-stu-id="a3168-177">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="a3168-178">Anforderung</span><span class="sxs-lookup"><span data-stu-id="a3168-178">Request</span></span> | <span data-ttu-id="a3168-179">Antwort</span><span class="sxs-lookup"><span data-stu-id="a3168-179">Response</span></span> |
| --- | --- |
| <span data-ttu-id="a3168-180">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="a3168-180">localhost:1234</span></span> | <span data-ttu-id="a3168-181">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="a3168-181">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="a3168-182">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="a3168-182">localhost:1234/map1</span></span> | <span data-ttu-id="a3168-183">Map Test 1</span><span class="sxs-lookup"><span data-stu-id="a3168-183">Map Test 1</span></span> |
| <span data-ttu-id="a3168-184">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="a3168-184">localhost:1234/map2</span></span> | <span data-ttu-id="a3168-185">Map Test 2</span><span class="sxs-lookup"><span data-stu-id="a3168-185">Map Test 2</span></span> |
| <span data-ttu-id="a3168-186">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="a3168-186">localhost:1234/map3</span></span> | <span data-ttu-id="a3168-187">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="a3168-187">Hello from non-Map delegate.</span></span>  |

<span data-ttu-id="a3168-188">Wenn `Map` verwendet wird, werden die übereinstimmenden Pfadsegmente bzw. das übereinstimmende Pfadsegment aus `HttpRequest.Path` entfernt und für jede Anforderung an `HttpRequest.PathBase` angehängt.</span><span class="sxs-lookup"><span data-stu-id="a3168-188">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="a3168-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) brancht die Anforderungspipeline auf Grundlage des Ergebnisses des angegebenen Prädikats.</span><span class="sxs-lookup"><span data-stu-id="a3168-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="a3168-190">Jedes Prädikat vom Typ `Func<HttpContext, bool>` kann verwendet werden, um Anforderungen einem neuen Branch der Pipeline zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="a3168-190">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="a3168-191">Im folgenden Beispiel wird ein Prädikat verwendet, um das Vorhandensein der Abfragezeichenfolgenvariablen `branch` zu ermitteln:</span><span class="sxs-lookup"><span data-stu-id="a3168-191">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[Main](index/sample/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="a3168-192">In der folgenden Tabelle sind die Anforderungen und Antworten von `http://localhost:1234` mit dem oben stehenden Code aufgelistet:</span><span class="sxs-lookup"><span data-stu-id="a3168-192">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="a3168-193">Anforderung</span><span class="sxs-lookup"><span data-stu-id="a3168-193">Request</span></span> | <span data-ttu-id="a3168-194">Antwort</span><span class="sxs-lookup"><span data-stu-id="a3168-194">Response</span></span> |
| --- | --- |
| <span data-ttu-id="a3168-195">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="a3168-195">localhost:1234</span></span> | <span data-ttu-id="a3168-196">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="a3168-196">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="a3168-197">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="a3168-197">localhost:1234/?branch=master</span></span> | <span data-ttu-id="a3168-198">Branch used = master</span><span class="sxs-lookup"><span data-stu-id="a3168-198">Branch used = master</span></span>|

<span data-ttu-id="a3168-199">`Map` unterstützt das Schachteln, wie z.B. in folgendem Code:</span><span class="sxs-lookup"><span data-stu-id="a3168-199">`Map` supports nesting, for example:</span></span>

```csharp
app.Map("/level1", level1App => {
       level1App.Map("/level2a", level2AApp => {
           // "/level1/level2a"
           //...
       });
       level1App.Map("/level2b", level2BApp => {
           // "/level1/level2b"
           //...
       });
   });
   ```

<span data-ttu-id="a3168-200">`Map` kann auch mehrere Segmente auf einmal zuordnen, z.B.:</span><span class="sxs-lookup"><span data-stu-id="a3168-200">`Map` can also match multiple segments at once, for example:</span></span>

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a><span data-ttu-id="a3168-201">Integrierte Middleware</span><span class="sxs-lookup"><span data-stu-id="a3168-201">Built-in middleware</span></span>

<span data-ttu-id="a3168-202">ASP.NET Core wird mit den folgenden Middlewarekomponenten ausgeliefert. Diese Tabelle enthält auch die Reihenfolge, in der diese hinzugefügt werden sollten:</span><span class="sxs-lookup"><span data-stu-id="a3168-202">ASP.NET Core ships with the following middleware components, as well as a description of the order in which they should be added:</span></span>

| <span data-ttu-id="a3168-203">Middleware</span><span class="sxs-lookup"><span data-stu-id="a3168-203">Middleware</span></span> | <span data-ttu-id="a3168-204">description</span><span class="sxs-lookup"><span data-stu-id="a3168-204">Description</span></span> | <span data-ttu-id="a3168-205">Reihenfolge</span><span class="sxs-lookup"><span data-stu-id="a3168-205">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="a3168-206">Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="a3168-206">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="a3168-207">Bietet Unterstützung für Authentifizierungen.</span><span class="sxs-lookup"><span data-stu-id="a3168-207">Provides authentication support.</span></span> | <span data-ttu-id="a3168-208">Bevor `HttpContext.User` erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="a3168-208">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="a3168-209">Terminal für OAuth-Rückrufe.</span><span class="sxs-lookup"><span data-stu-id="a3168-209">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="a3168-210">CORS</span><span class="sxs-lookup"><span data-stu-id="a3168-210">CORS</span></span>](xref:security/cors) | <span data-ttu-id="a3168-211">Konfiguriert die Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross-Origin Resource Sharing, CORS).</span><span class="sxs-lookup"><span data-stu-id="a3168-211">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="a3168-212">Vor Komponenten, die CORS verwenden.</span><span class="sxs-lookup"><span data-stu-id="a3168-212">Before components that use CORS.</span></span> |
| [<span data-ttu-id="a3168-213">Diagnose</span><span class="sxs-lookup"><span data-stu-id="a3168-213">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="a3168-214">Konfiguriert Diagnosen.</span><span class="sxs-lookup"><span data-stu-id="a3168-214">Configures diagnostics.</span></span> | <span data-ttu-id="a3168-215">Vor Komponenten, die Fehler erzeugen.</span><span class="sxs-lookup"><span data-stu-id="a3168-215">Before components that generate errors.</span></span> |
| [<span data-ttu-id="a3168-216">Umgeleitete Header/HttpOverrides</span><span class="sxs-lookup"><span data-stu-id="a3168-216">ForwardedHeaders/HttpOverrides</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="a3168-217">Leitet Proxyheader an die aktuelle Anforderung weiter.</span><span class="sxs-lookup"><span data-stu-id="a3168-217">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="a3168-218">Vor Komponenten, welche die aktualisierten Felder verwenden (z.B. Scheme, Host, ClientIP, Method).</span><span class="sxs-lookup"><span data-stu-id="a3168-218">Before components that consume the updated fields (examples: Scheme, Host, ClientIP, Method).</span></span> |
| [<span data-ttu-id="a3168-219">Zwischenspeichern von Antworten</span><span class="sxs-lookup"><span data-stu-id="a3168-219">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="a3168-220">Bietet Unterstützung für das Zwischenspeichern von Antworten.</span><span class="sxs-lookup"><span data-stu-id="a3168-220">Provides support for caching responses.</span></span> | <span data-ttu-id="a3168-221">Vor Komponenten, für die das Zwischenspeichern erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="a3168-221">Before components that require caching.</span></span> |
| [<span data-ttu-id="a3168-222">Antwortkomprimierung</span><span class="sxs-lookup"><span data-stu-id="a3168-222">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="a3168-223">Bietet Unterstützung für das Komprimieren von Antworten.</span><span class="sxs-lookup"><span data-stu-id="a3168-223">Provides support for compressing responses.</span></span> | <span data-ttu-id="a3168-224">Vor Komponenten, für die das Komprimieren erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="a3168-224">Before components that require compression.</span></span> |
| [<span data-ttu-id="a3168-225">Anforderungslokalisierung</span><span class="sxs-lookup"><span data-stu-id="a3168-225">RequestLocalization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="a3168-226">Bietet Unterstützung für die Lokalisierung.</span><span class="sxs-lookup"><span data-stu-id="a3168-226">Provides localization support.</span></span> | <span data-ttu-id="a3168-227">Vor der Lokalisierung vertraulicher Komponenten.</span><span class="sxs-lookup"><span data-stu-id="a3168-227">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="a3168-228">Routing</span><span class="sxs-lookup"><span data-stu-id="a3168-228">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="a3168-229">Definiert Anforderungsrouten und schränkt diese ein.</span><span class="sxs-lookup"><span data-stu-id="a3168-229">Defines and constrains request routes.</span></span> | <span data-ttu-id="a3168-230">Terminal für entsprechende Routen.</span><span class="sxs-lookup"><span data-stu-id="a3168-230">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="a3168-231">Sitzung</span><span class="sxs-lookup"><span data-stu-id="a3168-231">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="a3168-232">Bietet Unterstützung für das Verwalten von Benutzersitzungen.</span><span class="sxs-lookup"><span data-stu-id="a3168-232">Provides support for managing user sessions.</span></span> | <span data-ttu-id="a3168-233">Vor Komponenten, für die Sitzungen erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="a3168-233">Before components that require Session.</span></span> |
| [<span data-ttu-id="a3168-234">Statische Dateien</span><span class="sxs-lookup"><span data-stu-id="a3168-234">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="a3168-235">Bietet Unterstützung für das Verarbeiten statischer Dateien und das Durchsuchen des Verzeichnisses.</span><span class="sxs-lookup"><span data-stu-id="a3168-235">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="a3168-236">Terminal, wenn eine Anforderung mit den Dateien übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="a3168-236">Terminal if a request matches files.</span></span> |
| [<span data-ttu-id="a3168-237">URL-Umschreibung</span><span class="sxs-lookup"><span data-stu-id="a3168-237">URL Rewriting </span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="a3168-238">Bietet Unterstützung für das Umschreiben von URLs und das Umleiten von Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="a3168-238">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="a3168-239">Vor Komponenten, die die URL nutzen.</span><span class="sxs-lookup"><span data-stu-id="a3168-239">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="a3168-240">WebSockets</span><span class="sxs-lookup"><span data-stu-id="a3168-240">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="a3168-241">Aktiviert das WebSockets-Protokoll.</span><span class="sxs-lookup"><span data-stu-id="a3168-241">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="a3168-242">Vor Komponenten, die WebSocket-Anforderungen annehmen müssen.</span><span class="sxs-lookup"><span data-stu-id="a3168-242">Before components that are required to accept WebSocket requests.</span></span> |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a><span data-ttu-id="a3168-243">Schreiben von Middleware</span><span class="sxs-lookup"><span data-stu-id="a3168-243">Writing middleware</span></span>

<span data-ttu-id="a3168-244">Für gewöhnlich ist Middleware in einer Klasse gekapselt und wird mit einer Erweiterungsmethode verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="a3168-244">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="a3168-245">Sehen Sie sich folgende Middleware an, die die Kultur der aktuellen Anforderung über die Abfragezeichenfolge festlegt:</span><span class="sxs-lookup"><span data-stu-id="a3168-245">Consider the following middleware, which sets the culture for the current request from the query string:</span></span>

[!code-csharp[Main](index/sample/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="a3168-246">Hinweis: Der oben stehende Beispielcode veranschaulicht die Erstellung einer Middlewarekomponente.</span><span class="sxs-lookup"><span data-stu-id="a3168-246">Note: The sample code above is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="a3168-247">Lesen Sie den Artikel zu [Globalisierung und Lokalisierung in ASP.NET Core](xref:fundamentals/localization), um mehr über die integrierte Lokalisierungsunterstützung zu erfahren.</span><span class="sxs-lookup"><span data-stu-id="a3168-247">See [ Globalization and localization](xref:fundamentals/localization) for ASP.NET Core's built-in localization support.</span></span>

<span data-ttu-id="a3168-248">Sie können die Middleware testen, indem Sie die Kultur übergeben (z.B. `http://localhost:7997/?culture=no`).</span><span class="sxs-lookup"><span data-stu-id="a3168-248">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="a3168-249">Im folgenden Code wird der Middlewaredelegat in eine Klasse verschoben:</span><span class="sxs-lookup"><span data-stu-id="a3168-249">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[Main](index/sample/Culture/RequestCultureMiddleware.cs)]

<span data-ttu-id="a3168-250">Die folgende Erweiterungsmethode macht die Middleware über [IAppllicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) verfügbar:</span><span class="sxs-lookup"><span data-stu-id="a3168-250">The following extension method exposes the middleware through [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span></span>

[!code-csharp[Main](index/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="a3168-251">Der folgende Code ruft die Methode von `Configure` auf:</span><span class="sxs-lookup"><span data-stu-id="a3168-251">The following code calls the middleware from `Configure`:</span></span>

[!code-csharp[Main](index/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="a3168-252">Middleware sollte das [Prinzip der expliziten Abhängigkeiten](http://deviq.com/explicit-dependencies-principle/) befolgen, indem sie ihre Abhängigkeiten in ihrem Konstruktor verfügbar macht.</span><span class="sxs-lookup"><span data-stu-id="a3168-252">Middleware should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="a3168-253">Middleware wird einmal während der *Anwendungslebensdauer* erstellt.</span><span class="sxs-lookup"><span data-stu-id="a3168-253">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="a3168-254">Lesen Sie den folgenden Abschnitt zu *anforderungsbasierten Abhängigkeiten*, wenn Sie Dienste für Middleware innerhalb einer Anforderung gemeinsam verwenden müssen.</span><span class="sxs-lookup"><span data-stu-id="a3168-254">See *Per-request dependencies* below if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="a3168-255">Middlewarekomponenten können Ihre Abhängigkeiten über Dependency Injection mit Konstruktorparametern auflösen.</span><span class="sxs-lookup"><span data-stu-id="a3168-255">Middleware components can resolve their dependencies from dependency injection through constructor parameters.</span></span> <span data-ttu-id="a3168-256">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) kann auch direkt zusätzliche Parameter annehmen.</span><span class="sxs-lookup"><span data-stu-id="a3168-256">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="a3168-257">Voranforderungsbasierte Abhängigkeiten</span><span class="sxs-lookup"><span data-stu-id="a3168-257">Per-request dependencies</span></span>

<span data-ttu-id="a3168-258">Weil Middleware zum Zeitpunkt des Anwendungsstarts erstellt wird (und nicht voranforderungsbasiert), werden *bereichsbezogene* Lebensdauerdienste von Middlewarekonstruktoren in den einzelnen Anforderungen nicht gemeinsam mit anderen Typen mit Dependency Injection verwendet.</span><span class="sxs-lookup"><span data-stu-id="a3168-258">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors are not  shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="a3168-259">Wenn Sie einen *bereichsbezogenen* Dienst sowohl in Ihrer Middleware als auch in anderen Typen verwenden müssen, fügen Sie diese Dienste zur Signatur der `Invoke`-Methode hinzu.</span><span class="sxs-lookup"><span data-stu-id="a3168-259">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="a3168-260">Die `Invoke`-Methode kann zusätzliche Parameter akzeptieren, die durch Dependency Injection aufgefüllt werden.</span><span class="sxs-lookup"><span data-stu-id="a3168-260">The `Invoke` method can accept additional parameters that are populated by dependency injection.</span></span> <span data-ttu-id="a3168-261">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a3168-261">For example:</span></span>

```c#
public class MyMiddleware
{
    private readonly RequestDelegate _next;

    public MyMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="a3168-262">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="a3168-262">Additional resources</span></span>

* [<span data-ttu-id="a3168-263">Migrating HTTP Modules to Middleware (Migration von HTTP-Modulen zu Middleware)</span><span class="sxs-lookup"><span data-stu-id="a3168-263">Migrating HTTP Modules to Middleware</span></span>](xref:migration/http-modules)
* [<span data-ttu-id="a3168-264">Application Startup (Starten von Anwendungen)</span><span class="sxs-lookup"><span data-stu-id="a3168-264">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="a3168-265">Erforderliche Funktionen</span><span class="sxs-lookup"><span data-stu-id="a3168-265">Request Features</span></span>](xref:fundamentals/request-features)
* [<span data-ttu-id="a3168-266">Factorybezogene Middlewareaktivierung</span><span class="sxs-lookup"><span data-stu-id="a3168-266">Factory-based middleware activation</span></span>](xref:fundamentals/middleware/extensibility)
* [<span data-ttu-id="a3168-267">Factorybezogene Middlewareaktivierung mit einem Drittanbietercontainer</span><span class="sxs-lookup"><span data-stu-id="a3168-267">Factory-based middleware activation with a third-party container</span></span>](xref:fundamentals/middleware/extensibility-third-party-container)