---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: "Einführung in das Lernprogramm NerdDinner | Microsoft Docs"
author: shanselman
description: "Die beste Möglichkeit, erfahren, ein neues Framework besteht darin mit erstellen. In diesem Lernprogramm erläutert, wie eine kleine, aber vollständige, Anwendung, die mit ASP.NET.. erstellen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 57eedb224e26867c78cc399b89f91b95f722074d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="introducing-the-nerddinner-tutorial"></a><span data-ttu-id="f40c3-104">Einführung in das Lernprogramm NerdDinner</span><span class="sxs-lookup"><span data-stu-id="f40c3-104">Introducing the NerdDinner Tutorial</span></span>
====================
<span data-ttu-id="f40c3-105">durch [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="f40c3-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

[<span data-ttu-id="f40c3-106">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="f40c3-106">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="f40c3-107">Die beste Möglichkeit, erfahren, ein neues Framework besteht darin mit erstellen.</span><span class="sxs-lookup"><span data-stu-id="f40c3-107">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="f40c3-108">Dieses Lernprogramm führt Sie durch die Schritte zum Erstellen einer kleines, jedoch abgeschlossen, Anwendung, die mithilfe von ASP.NET MVC-1, und eine Einführung in die entsprechende kernbegriffe.</span><span class="sxs-lookup"><span data-stu-id="f40c3-108">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC 1, and introduces some of the core concepts behind it.</span></span>
> 
> <span data-ttu-id="f40c3-109">Bei Verwendung von ASP.NET MVC 3 empfehlen wir führen Sie die [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Lernprogramme.</span><span class="sxs-lookup"><span data-stu-id="f40c3-109">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-tutorial"></a><span data-ttu-id="f40c3-110">NerdDinner-Lernprogramm</span><span class="sxs-lookup"><span data-stu-id="f40c3-110">NerdDinner Tutorial</span></span>

<span data-ttu-id="f40c3-111">Die beste Möglichkeit, erfahren, ein neues Framework besteht darin mit erstellen.</span><span class="sxs-lookup"><span data-stu-id="f40c3-111">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="f40c3-112">Dieses Lernprogramm führt Sie durch die Schritte zum Erstellen einer kleines, jedoch abgeschlossen haben, mithilfe von ASP.NET MVC, Anwendung und eine Einführung in die entsprechende kernbegriffe.</span><span class="sxs-lookup"><span data-stu-id="f40c3-112">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC, and introduces some of the core concepts behind it.</span></span>

<span data-ttu-id="f40c3-113">Die Anwendung, die wir erstellen möchten, wird "NerdDinner" aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="f40c3-113">The application we are going to build is called "NerdDinner".</span></span> <span data-ttu-id="f40c3-114">NerdDinner bietet eine einfache Möglichkeit für Benutzer zum Suchen und Organisieren von Abendessen online:</span><span class="sxs-lookup"><span data-stu-id="f40c3-114">NerdDinner provides an easy way for people to find and organize dinners online:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image1.png)

<span data-ttu-id="f40c3-115">NerdDinner kann registrierte Benutzer erstellen, bearbeiten und Löschen von Abendessen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f40c3-115">NerdDinner enables registered users to create, edit and delete dinners.</span></span> <span data-ttu-id="f40c3-116">Er erzwingt einen konsistenten Satz von Überprüfung und Geschäftsregeln in der Anwendung:</span><span class="sxs-lookup"><span data-stu-id="f40c3-116">It enforces a consistent set of validation and business rules across the application:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image2.png)

<span data-ttu-id="f40c3-117">Besucher können eine AJAX-basierte Zuordnung um zu suchende bevorstehende Abendessen in der Nähe sie gespeichert werden:</span><span class="sxs-lookup"><span data-stu-id="f40c3-117">Visitors can use an AJAX-based map to search for upcoming dinners being held near them:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image3.png)

<span data-ttu-id="f40c3-118">Klicken eine Dinner gelangen sie zur eine Seite "Details", in denen erfahren sie mehr darüber:</span><span class="sxs-lookup"><span data-stu-id="f40c3-118">Clicking a dinner will take them to a details page where they can learn more about it:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image4.png)

<span data-ttu-id="f40c3-119">Wenn sie interessiert der Teilnahme an der Dinner können auf der Website registrieren oder anmelden:</span><span class="sxs-lookup"><span data-stu-id="f40c3-119">If they are interested in attending the dinner they can login or register on the site:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image5.png)

<span data-ttu-id="f40c3-120">Sie können einen AJAX-basierten Antwort-Link, um das Ereignis studiert klicken:</span><span class="sxs-lookup"><span data-stu-id="f40c3-120">They can then click an AJAX-based RSVP link to attend the event:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a><span data-ttu-id="f40c3-121">NerdDinner implementieren</span><span class="sxs-lookup"><span data-stu-id="f40c3-121">Implementing NerdDinner</span></span>

<span data-ttu-id="f40c3-122">Wir werden unsere NerdDinner Anwendung beginnen, indem Sie mithilfe von Datei -&gt;Befehl Neues Projekt in Visual Studio zum Erstellen eines brandneuen ASP.NET MVC-Projekts.</span><span class="sxs-lookup"><span data-stu-id="f40c3-122">We are going to begin our NerdDinner application by using the File-&gt;New Project command within Visual Studio to create a brand new ASP.NET MVC project.</span></span> <span data-ttu-id="f40c3-123">Wir wird dann schrittweise Funktionen erweitert werden.</span><span class="sxs-lookup"><span data-stu-id="f40c3-123">We will then incrementally add functionality and features.</span></span> <span data-ttu-id="f40c3-124">Dabei werden folgende Themen behandelt:</span><span class="sxs-lookup"><span data-stu-id="f40c3-124">Along the way we'll cover:</span></span>

1. [<span data-ttu-id="f40c3-125">Vorgehensweise: Erstellen Sie ein neues ASP.NET MVC-Projekt</span><span class="sxs-lookup"><span data-stu-id="f40c3-125">How to create a new ASP.NET MVC Project</span></span>](# "erstellen Sie ein neues ASP.NET MVC-Projekt")
2. [<span data-ttu-id="f40c3-126">Gewusst wie: Erstellen einer Datenbank</span><span class="sxs-lookup"><span data-stu-id="f40c3-126">How to create a database</span></span>](# "erstellen Sie eine Datenbank")
3. [<span data-ttu-id="f40c3-127">Gewusst wie: Erstellen eines Modells mit Business Regel Überprüfungen</span><span class="sxs-lookup"><span data-stu-id="f40c3-127">How to build a model with business rule validations</span></span>](# "Erstellen eines Modells mit Business Regel Überprüfungen")
4. [<span data-ttu-id="f40c3-128">Gewusst wie: Controller und Ansichten verwenden, um eine Liste/Detail-Benutzeroberfläche implementieren</span><span class="sxs-lookup"><span data-stu-id="f40c3-128">How to use controllers and views to implement a listing/details UI</span></span>](# "verwenden Controller und Ansichten zum Implementieren einer Auflistung/Detail-Benutzeroberfläche")
5. <span data-ttu-id="f40c3-129">[Gewusst wie: Bereitstellen von CRUD (erstellen, lesen, aktualisieren und löschen) Daten bilden Eintrag Unterstützung](# "bieten CRUD (Create, Read, Update, Delete) Daten Formular Dateneingabe zu unterstützen")</span><span class="sxs-lookup"><span data-stu-id="f40c3-129">[How to provide CRUD (create, read, update, delete) data form entry support](# "Provide CRUD (Create, Read, Update, Delete) Data Form Entry Support")</span></span>
6. [<span data-ttu-id="f40c3-130">Zum Verwenden von ViewData und Implementierung von Klassen ViewModel</span><span class="sxs-lookup"><span data-stu-id="f40c3-130">How to use ViewData and implement ViewModel classes</span></span>](# "ViewData verwenden und Implementieren von ViewModel-Klassen")
7. [<span data-ttu-id="f40c3-131">Gewusst wie: Verwenden Sie die Benutzeroberfläche mit Masterseiten und Replikatsgruppenelemente erneut</span><span class="sxs-lookup"><span data-stu-id="f40c3-131">How to re-use UI using master pages and partials</span></span>](# "Benutzeroberfläche mithilfe von Masterseiten erneut verwenden und Replikatsgruppenelemente")
8. [<span data-ttu-id="f40c3-132">Zum Implementieren von Paging effiziente</span><span class="sxs-lookup"><span data-stu-id="f40c3-132">How to implement efficient data paging</span></span>](# "implementieren effizient Daten Paging")
9. [<span data-ttu-id="f40c3-133">Sichern von Anwendungen mithilfe von Authentifizierung und Autorisierung</span><span class="sxs-lookup"><span data-stu-id="f40c3-133">How to secure applications using authentication and authorization</span></span>](# "sichere Anwendungen mithilfe von Authentifizierung und Autorisierung")
10. [<span data-ttu-id="f40c3-134">Wie Sie mithilfe von AJAX dynamische Updates bereitstellen</span><span class="sxs-lookup"><span data-stu-id="f40c3-134">How to use AJAX to deliver dynamic updates</span></span>](# "AJAX verwenden, um dynamische Updates bieten")
11. [<span data-ttu-id="f40c3-135">Wie AJAX zuordnungsszenarien implementieren mit</span><span class="sxs-lookup"><span data-stu-id="f40c3-135">How to use AJAX to implement mapping scenarios</span></span>](# "AJAX verwenden, um Zuordnungsszenarien implementieren")
12. [<span data-ttu-id="f40c3-136">So aktivieren Sie automatisierte Komponententests</span><span class="sxs-lookup"><span data-stu-id="f40c3-136">How to enable automated unit testing</span></span>](# "automatisierte Komponententest aktivieren")

<span data-ttu-id="f40c3-137">Sie können Ihr eigenes Exemplar des NerdDinner Erstellen von Grund auf neu, indem Abschließen jeder Schritt wir Exemplarische Vorgehensweise in diesem Kapitel.</span><span class="sxs-lookup"><span data-stu-id="f40c3-137">You can build your own copy of NerdDinner from scratch by completing each step we walkthrough in this chapter.</span></span> <span data-ttu-id="f40c3-138">Alternativ können Sie eine abgeschlossene Version des Quellcodes hier herunterladen: [NerdDinner auf GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span><span class="sxs-lookup"><span data-stu-id="f40c3-138">Alternatively, you can download a completed version of the source code here: [NerdDinner on GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span></span> <span data-ttu-id="f40c3-139">Sie können auch optional auch [Laden Sie eine kostenlose PDF-Version dieses Lernprogramms](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) , wenn Sie das Lernprogramm offline lesen möchten.</span><span class="sxs-lookup"><span data-stu-id="f40c3-139">You can also optionally also [download a free PDF version of this tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) if you want to read the tutorial offline.</span></span>

<span data-ttu-id="f40c3-140">Sie können Visual Studio 2008 oder die kostenlose Visual Web Developer 2008 Express verwenden, zum Erstellen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="f40c3-140">You can use either Visual Studio 2008 or the free Visual Web Developer 2008 Express to build the application.</span></span> <span data-ttu-id="f40c3-141">Sie können SQL Server oder den freien SQL Server Express für die Datenbank verwenden.</span><span class="sxs-lookup"><span data-stu-id="f40c3-141">You can use either SQL Server or the free SQL Server Express for the database.</span></span>

<span data-ttu-id="f40c3-142">Sie können ASP.NET MVC, Visual Web Developer 2008 Express und SQL Server Express (kostenlos) mithilfe von V2 installieren die [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span><span class="sxs-lookup"><span data-stu-id="f40c3-142">You can install ASP.NET MVC, Visual Web Developer 2008 Express, and SQL Server Express (all free) using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span></span>

### <a name="now-lets-get-started"></a><span data-ttu-id="f40c3-143">Jetzt sehen wir beginnen...</span><span class="sxs-lookup"><span data-stu-id="f40c3-143">Now let's get started....</span></span>

<span data-ttu-id="f40c3-144">Wir haben behandelt, was NerdDinner ist, wir werden unsere Hüllen als Rollup angegeben, und Code schreiben.</span><span class="sxs-lookup"><span data-stu-id="f40c3-144">Now that we've covered what NerdDinner is, let's roll up our sleeves and write some code.</span></span>

<span data-ttu-id="f40c3-145">Wir beginnen, indem Sie mithilfe von Datei -&gt;neues Projekt in Visual Studio die NerdDinner-Anwendung erstellen.</span><span class="sxs-lookup"><span data-stu-id="f40c3-145">We'll begin by using File-&gt;New Project within Visual Studio to create the NerdDinner application.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="f40c3-146">Nächste</span><span class="sxs-lookup"><span data-stu-id="f40c3-146">Next</span></span>](create-a-new-aspnet-mvc-project.md)