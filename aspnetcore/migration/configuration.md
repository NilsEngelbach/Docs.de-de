---
title: Migrieren der Konfiguration
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 8468d859-ff32-4a92-9e62-08c4a9e36594
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/configuration
ms.openlocfilehash: 62660f7e58467a69f540966df188747b6fde57fe
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="migrating-configuration"></a><span data-ttu-id="b5b4e-103">Migrieren der Konfiguration</span><span class="sxs-lookup"><span data-stu-id="b5b4e-103">Migrating Configuration</span></span>

<span data-ttu-id="b5b4e-104">Durch [Steve Smith](https://ardalis.com/) und [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="b5b4e-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="b5b4e-105">In der vorherigen Artikel wir begann [Migrieren eines ASP.NET MVC-Projekts zu ASP.NET Core MVC](mvc.md).</span><span class="sxs-lookup"><span data-stu-id="b5b4e-105">In the previous article, we began [migrating an ASP.NET MVC project to ASP.NET Core MVC](mvc.md).</span></span> <span data-ttu-id="b5b4e-106">In diesem Artikel wir die Konfiguration migrieren.</span><span class="sxs-lookup"><span data-stu-id="b5b4e-106">In this article, we migrate configuration.</span></span>

[<span data-ttu-id="b5b4e-107">Anzeigen oder Herunterladen von Beispielcode</span><span class="sxs-lookup"><span data-stu-id="b5b4e-107">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples)

## <a name="setup-configuration"></a><span data-ttu-id="b5b4e-108">Setup-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="b5b4e-108">Setup Configuration</span></span>

<span data-ttu-id="b5b4e-109">ASP.NET Core wird nicht mehr verwendet, die *"Global.asax"* und *"Web.config"* Dateien, die frühere Versionen von ASP.NET verwendet.</span><span class="sxs-lookup"><span data-stu-id="b5b4e-109">ASP.NET Core no longer uses the *Global.asax* and *web.config* files that previous versions of ASP.NET utilized.</span></span> <span data-ttu-id="b5b4e-110">In früheren Versionen von ASP.NET Startlogik für die Anwendung im positioniert wurde ein `Application_StartUp` Methode innerhalb *"Global.asax"*.</span><span class="sxs-lookup"><span data-stu-id="b5b4e-110">In earlier versions of ASP.NET, application startup logic was placed in an `Application_StartUp` method within *Global.asax*.</span></span> <span data-ttu-id="b5b4e-111">Später werden Sie in ASP.NET MVC eine *Startup.cs* Datei im Stammverzeichnis des Projekts; enthalten war, und es wurde aufgerufen, wenn die Anwendung gestartet.</span><span class="sxs-lookup"><span data-stu-id="b5b4e-111">Later, in ASP.NET MVC, a *Startup.cs* file was included in the root of the project; and, it was called when the application started.</span></span> <span data-ttu-id="b5b4e-112">ASP.NET Core hat dieser Ansatz vollständig von übernommen platzieren Startlogik für alle in der *Startup.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="b5b4e-112">ASP.NET Core has adopted this approach completely by placing all startup logic in the *Startup.cs* file.</span></span>

<span data-ttu-id="b5b4e-113">Die *"Web.config"* Datei auch in ASP.NET Core ersetzt wurde.</span><span class="sxs-lookup"><span data-stu-id="b5b4e-113">The *web.config* file has also been replaced in ASP.NET Core.</span></span> <span data-ttu-id="b5b4e-114">Konfiguration selbst kann nun konfiguriert werden, und als Teil der Schritte beim Starten einer Anwendung in beschriebenen *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="b5b4e-114">Configuration itself can now be configured, as part of the application startup procedure described in *Startup.cs*.</span></span> <span data-ttu-id="b5b4e-115">Konfiguration kann XML-Dateien weiterhin nutzen, sondern in der Regel ASP.NET Core Projekte werden platzieren Konfigurationswerte in einer JSON-formatierten Datei wie z. B. *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="b5b4e-115">Configuration can still utilize XML files, but typically ASP.NET Core projects will place configuration values in a JSON-formatted file, such as *appsettings.json*.</span></span> <span data-ttu-id="b5b4e-116">ASP.NET-Core-Konfigurationssystem kann leicht Umgebungsvariablen, zugreifen, die einen Speicherort im sicheren und stabilen für umgebungsspezifische Werte angeben können.</span><span class="sxs-lookup"><span data-stu-id="b5b4e-116">ASP.NET Core's configuration system can also easily access environment variables, which can provide a more secure and robust location for environment-specific values.</span></span> <span data-ttu-id="b5b4e-117">Dies gilt insbesondere für geheime Schlüssel, wie z. B. Verbindungszeichenfolgen und API-Schlüssel, die nicht in die quellcodeverwaltung überprüft werden sollen.</span><span class="sxs-lookup"><span data-stu-id="b5b4e-117">This is especially true for secrets like connection strings and API keys that should not be checked into source control.</span></span> <span data-ttu-id="b5b4e-118">Finden Sie unter [Konfiguration](../fundamentals/configuration.md) Weitere Informationen zur Konfiguration in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b5b4e-118">See [Configuration](../fundamentals/configuration.md) to learn more about configuration in ASP.NET Core.</span></span>

<span data-ttu-id="b5b4e-119">Für diesen Artikel beginnen wir mit dem teilweise migriert ASP.NET Core-Projekt aus [vorherigen Artikel](mvc.md).</span><span class="sxs-lookup"><span data-stu-id="b5b4e-119">For this article, we are starting with the partially-migrated ASP.NET Core project from [the previous article](mvc.md).</span></span> <span data-ttu-id="b5b4e-120">Zum Einrichten des Konfiguration fügen die folgenden Konstruktor und die Eigenschaft, um die *Startup.cs* Datei befindet sich im Stammverzeichnis des Projekts:</span><span class="sxs-lookup"><span data-stu-id="b5b4e-120">To setup configuration, add the following constructor and property to the *Startup.cs* file located in the root of the project:</span></span>

<span data-ttu-id="b5b4e-121">[!code-csharp[Main](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-21)]</span><span class="sxs-lookup"><span data-stu-id="b5b4e-121">[!code-csharp[Main](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-21)]</span></span>

<span data-ttu-id="b5b4e-122">Beachten Sie, dass an diesem Punkt der *Startup.cs* Datei wird nicht kompiliert werden, da wir benötigen Sie Folgendes hinzufügen `using` Anweisung:</span><span class="sxs-lookup"><span data-stu-id="b5b4e-122">Note that at this point, the *Startup.cs* file will not compile, as we still need to add the following `using` statement:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="b5b4e-123">Hinzufügen einer *appsettings.json* Datei im Stammverzeichnis des Projekts mit der entsprechenden Elementvorlage:</span><span class="sxs-lookup"><span data-stu-id="b5b4e-123">Add an *appsettings.json* file to the root of the project using the appropriate item template:</span></span>

![Hinzufügen von "appSettings" JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a><span data-ttu-id="b5b4e-125">Migrieren von Konfigurationseinstellungen aus der Datei "Web.config"</span><span class="sxs-lookup"><span data-stu-id="b5b4e-125">Migrate Configuration Settings from web.config</span></span>

<span data-ttu-id="b5b4e-126">Unsere ASP.NET MVC-Projekt enthielt, die erforderliche Datenbank-Verbindungszeichenfolge in *"Web.config"*in die `<connectionStrings>` Element.</span><span class="sxs-lookup"><span data-stu-id="b5b4e-126">Our ASP.NET MVC project included the required database connection string in *web.config*, in the `<connectionStrings>` element.</span></span> <span data-ttu-id="b5b4e-127">In unserem Projekt ASP.NET Core Kegel zum Speichern dieser Informationen in den *appsettings.json* Datei.</span><span class="sxs-lookup"><span data-stu-id="b5b4e-127">In our ASP.NET Core project, we are going to store this information in the *appsettings.json* file.</span></span> <span data-ttu-id="b5b4e-128">Open *appsettings.json*, und beachten Sie, dass sie bereits die Folgendes umfasst:</span><span class="sxs-lookup"><span data-stu-id="b5b4e-128">Open *appsettings.json*, and note that it already includes the following:</span></span>

<span data-ttu-id="b5b4e-129">[!code-json[Main](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]</span><span class="sxs-lookup"><span data-stu-id="b5b4e-129">[!code-json[Main](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]</span></span>


<span data-ttu-id="b5b4e-130">In der markierten Zeile in der oben dargestellten, ändern Sie den Namen der Datenbank von **_CHANGE_ME** auf den Namen der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="b5b4e-130">In the highlighted line depicted above, change the name of the database from **_CHANGE_ME** to the name of your database.</span></span>

## <a name="summary"></a><span data-ttu-id="b5b4e-131">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="b5b4e-131">Summary</span></span>

<span data-ttu-id="b5b4e-132">ASP.NET Core setzt alle Startlogik für die Anwendung in eine einzelne Datei, in der die erforderlichen Dienste und Abhängigkeiten definiert und konfiguriert werden können.</span><span class="sxs-lookup"><span data-stu-id="b5b4e-132">ASP.NET Core places all startup logic for the application in a single file, in which the necessary services and dependencies can be defined and configured.</span></span> <span data-ttu-id="b5b4e-133">Er ersetzt die *"Web.config"* Datei in eine flexible Konfiguration-Funktion, die eine Vielzahl von Dateiformaten wie JSON sowie Umgebungsvariablen nutzen können.</span><span class="sxs-lookup"><span data-stu-id="b5b4e-133">It replaces the *web.config* file with a flexible configuration feature that can leverage a variety of file formats, such as JSON, as well as environment variables.</span></span>