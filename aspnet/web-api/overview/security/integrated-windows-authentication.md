---
uid: web-api/overview/security/integrated-windows-authentication
title: Integrierte Windows-Authentifizierung | Microsoft-Dokumentation
author: MikeWasson
description: Beschreibt die Verwendung der integrierten Windows-Authentifizierung in ASP.NET Web-API.
ms.author: riande
ms.date: 12/18/2012
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: 13dead421abf7ded73cbb2e5f87e54b1a869b5d4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823962"
---
<a name="integrated-windows-authentication"></a>Integrierte Windows-Authentifizierung
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Integrierte Windows-Authentifizierung kann Benutzer melden Sie sich mit ihren Windows-Anmeldeinformationen, die mit Kerberos oder NTLM. Der Client sendet die Anmeldeinformationen in der Authorization-Header. Windows-Authentifizierung ist für Intranetumgebungen am besten geeignet. Weitere Informationen finden Sie unter [Windows-Authentifizierung](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).

| Vorteile | Nachteile |
| --- | --- |
| – In IIS erstellt. – Die Anmeldeinformationen des Benutzers ist nicht in der Anforderung gesendet werden. – Wenn der Client-Computer mit der Domäne (z. B. Intranetanwendung) gehört, muss der Benutzer nicht, Anmeldeinformationen einzugeben. | -Nicht für die Internet-Anwendungen empfohlen. – Unterstützung für Kerberos oder NTLM in der Client erfordert. -Client muss in Active Directory-Domäne sein. |

> [!NOTE]
> Wenn Ihre Anwendung in Azure gehostet wird, und einer lokalen Active Directory-Domäne haben, sollten Sie Ihre lokalen AD mit Azure Active Directory in einem Verbund zusammenfassen. Auf diese Weise können Benutzer sich mit ihren lokalen Anmeldeinformationen anmelden, aber die Authentifizierung von Azure AD erfolgt. Weitere Informationen finden Sie unter [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).


Zum Erstellen einer Anwendung, die integrierte Windows-Authentifizierung verwendet wird, wählen Sie die Vorlage "Intranet-Anwendung" im Assistenten für MVC 4. Diese Projektvorlage fügt die folgende Einstellung in der Datei "Web.config":

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

Auf dem Client, der integrierten Windows-Authentifizierung funktioniert mit jedem Browser, die unterstützt die [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) -Authentifizierungsschema, das meisten gängigen Browser enthält. Für .NET-Clientanwendungen die **"HttpClient"** Klasse unterstützt die Windows-Authentifizierung:

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

Windows-Authentifizierung ist anfällig für websiteübergreifende anforderungsfälschungen (CSRF). Finden Sie unter [verhindern von Angriffen Cross-Site Request Forgery (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).
