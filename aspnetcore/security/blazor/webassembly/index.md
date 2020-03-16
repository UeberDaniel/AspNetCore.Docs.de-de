---
title: Sichern von Blazor-WebAssembly für ASP.NET Core
author: guardrex
description: Erfahren Sie, wie Sie Blazor-WebAssembly-Apps als Single-Page-Anwendungen (SPAs) sichern.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/index
ms.openlocfilehash: 48136d0717998df3311dd5177688e062d0009176
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083548"
---
# <a name="secure-aspnet-core-opno-locblazor-webassembly"></a>Sichern von Blazor-WebAssembly für ASP.NET Core

Von [Javier Calvarro Nelson](https://github.com/javiercn)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Blazor-WebAssembly-Apps werden auf die gleiche Weise gesichert wie Single-Page-Anwendungen (SPAs). Es gibt mehrere Ansätze für die Authentifizierung von Benutzern bei SPAs, der gängigste und umfassendste Ansatz besteht jedoch darin, eine auf dem [OAuth 2.0-Protokoll](https://oauth.net/) basierende Implementierung wie [Open ID Connect (OIDC)](https://openid.net/connect/) zu verwenden.

## <a name="authentication-library"></a>Authentifizierungsbibliothek

Blazor-WebAssembly unterstützt die Authentifizierung und Autorisierung von Apps mit OIDC über die `Microsoft.AspNetCore.Components.WebAssembly.Authentication`-Bibliothek. Die Bibliothek stellt mehrere Primitive für die nahtlose Authentifizierung für ASP.NET Core-Back-Ends bereit. Die Bibliothek integriert die ASP.NET Core-Identität mit der API-Authentifizierungsunterstützung, die auf [Identity Server](https://identityserver.io/) basiert. Die Bibliothek kann bei einem Drittanbieter-Identitätsanbieter authentifiziert werden, der OIDC unterstützt (die OpenID-Anbieter).

Die Authentifizierungsunterstützung in Blazor-WebAssembly basiert auf der *oidc-client.js*-Bibliothek, die für die Verarbeitung der zugrunde liegenden Details des Authentifizierungsprotokolls verwendet wird.

Eine weitere Option zum Authentifizieren von SPAs ist die Verwendung von SameSite-Cookies. Das Entwurfsdesign von Blazor-WebAssembly verwendet jedoch OAuth und OIDC als bewährte Option für die Authentifizierung bei Blazor-WebAssembly-Apps. Die auf [JSON Web Token](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) gestützte [tokenbasierte Authentifizierung](xref:security/anti-request-forgery#token-based-authentication) wurde aus Funktions- und Sicherheitsgründen der [cookiebasierten Authentifizierung](xref:security/anti-request-forgery#cookie-based-authentication) vorgezogen.

* Ein tokenbasiertes Protokoll bietet eine kleinere Angriffsfläche, da die Token nicht in allen Anforderungen gesendet werden.
* Serverendpunkte erfordern keinen Schutz gegenüber [websiteübergreifender Anforderungsfälschung (CSRF, Cross-Site Request Forgery)](xref:security/anti-request-forgery), da die Token explizit gesendet werden. Dadurch können Sie Blazor-WebAssembly-Apps zusammen mit MVC- oder Razor Pages-Apps hosten.
* Token haben eingeschränktere Berechtigungen als Cookies. Beispielsweise können Token nicht zum Verwalten des Benutzerkontos oder zur Änderung eines Benutzerkennworts verwendet werden, es sei denn, diese Funktionalität wird explizit implementiert.
* Token haben eine kürzere Lebensdauer, standardmäßig eine Stunde, wodurch sich das Angriffsfenster verkleinert. Token können auch zu jeder Zeit widerrufen werden.
* Eigenständige JWTs bieten dem Client und dem Server Garantien zum Authentifizierungsprozess. Ein Client hat zum Beispiel die Möglichkeiten, zu erkennen und zu überprüfen, dass die von ihm empfangenen Token legitim sind und als Teil eines gegebenen Authentifizierungsprozesses ausgegeben wurden. Wenn Dritte versuchen, ein Token inmitten des Authentifizierungsprozesses zu tauschen, kann der Client das ausgetauschte Token erkennen und so die Nutzung vermeiden.
* Token mit OAuth und OIDC, die sich nicht auf den Benutzer-Agent verlassen, verhalten sich ordnungsgemäß, um zu gewährleisten, dass die App sicher ist.
* Tokenbasierte Protokolle wie OAuth und OIDC lassen die Authentifizierung und Autorisierung gehosteter und eigenständiger Apps mit denselben Sicherheitsmerkmalen zu.

## <a name="authentication-process-with-oidc"></a>Authentifizierungsprozess mit OIDC

Die `Microsoft.AspNetCore.Components.WebAssembly.Authentication`-Bibliothek bietet mehrere Primitive zum Implementieren der Authentifizierung und Autorisierung mithilfe von OIDC. Im Allgemeinen funktioniert die Authentifizierung wie folgt:

* Wenn ein anonymer Benutzer auf die Anmeldeschaltfläche klickt oder eine Seite mit angewendetem [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)-Attribut anfordert, wird der Benutzer zur Anmeldeseite (`/authentication/login`) der App weitergeleitet.
* Auf der Anmeldeseite bereitet die Authentifizierungsbibliothek eine Umleitung zum Autorisierungsendpunkt vor. Der Autorisierungsendpunkt befindet sich außerhalb der Blazor-WebAssembly-App und kann als separater Ursprung gehostet werden. Der Endpunkt ist dafür verantwortlich, zu bestimmen, ob der Benutzer authentifiziert ist, und dass ein oder mehrere Token als Antwort ausgegeben werden. Die Authentifizierungsbibliothek stellt einen Anmelderückruf zum Empfangen der Authentifizierungsantwort bereit.
  * Wenn der Benutzer nicht authentifiziert ist, wird er an das zugrunde liegende Authentifizierungssystem umgeleitet (normalerweise die ASP.NET Core Identität).
  * Wenn der Benutzer bereits authentifiziert wurde, generiert der Authentifizierungsendpunkt die entsprechenden Token und leitet den Browser zurück an den Anmelderückrufendpunkt (`/authentication/login-callback`).
* Wenn die Blazor-WebAssembly-App den Anmelderückrufendpunkt (`/authentication/login-callback`) lädt, wird die Authentifizierungsantwort verarbeitet.
  * Sobald der Authentifizierungsprozess erfolgreich abgeschlossen wird, wird der Benutzer authentifiziert und optional an die ursprüngliche geschützte URL weitergeleitet, die vom Benutzer angefordert wurde.
  * Sollte der Authentifizierungsprozess aus irgend einem Grund fehlschlagen, wird der Benutzer auf die Seite „Fehler bei der Anmeldung“ (`/authentication/login-failed`) weitergeleitet, und ein Fehler wird angezeigt.