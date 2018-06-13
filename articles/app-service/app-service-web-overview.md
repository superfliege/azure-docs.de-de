---
title: Web-Apps – Übersicht | Microsoft Docs
description: Es wird beschrieben, wie Azure App Service Sie beim Entwickeln und Hosten von Webanwendungen unterstützt.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
ms.assetid: 94af2caf-a2ec-4415-a097-f60694b860b3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 01/04/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 79828193e283f0dcb80035cae0c11b050a1639ea
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
ms.locfileid: "33935642"
---
# <a name="web-apps-overview"></a>Web-Apps – Übersicht

*Azure App Service-Web-Apps* (oder einfach Web-Apps) ist ein Dienst zum Hosten von Webanwendungen, REST-APIs und mobilen Back-Ends. Sie können in Ihrer bevorzugten Sprache entwickeln, z.B. .NET, .NET Core, Java, Ruby, Node.js, PHP oder Python. Anwendungen können in Windows-basierten Umgebungen problemlos ausgeführt und skaliert werden. Informationen zu Linux-basierten Umgebungen finden Sie unter [Einführung in Azure App Service unter Linux](containers/app-service-linux-intro.md). 

Web-Apps ergänzen Ihre Anwendung nicht nur mit der Leistungsfähigkeit von Microsoft Azure, z.B. Sicherheit, Lastenausgleich, automatische Skalierung und automatisierte Verwaltung. Sie können auch die Vorteile ihrer DevOps-Funktionen nutzen, z.B. dauerhafte Bereitstellung über VSTS, GitHub, Docker Hub und andere Quellen, Paketverwaltung, Stagingumgebung, benutzerdefinierte Domäne und SSL-Zertifikate. 

Mit App Service zahlen Sie nur für die Azure-Computeressourcen, die Sie verwenden. Die Computeressourcen, die Sie verwenden, richten sich nach dem _App Service-Plan_, nach dem Sie Ihre Web-Apps ausführen. Weitere Informationen hierzu finden Sie unter [Azure App Service-Pläne – Detaillierte Übersicht](azure-web-sites-web-hosting-plans-in-depth-overview.md).

## <a name="why-use-web-apps"></a>Gründe für die Verwendung von Web-Apps

Hier sind einige wichtige Features von App Service-Web-Apps aufgeführt:

* **Mehrere Sprachen und Frameworks**: Web-Apps bieten erstklassige Unterstützung für ASP.NET, ASP.NET Core, Java, Ruby, Node.js, PHP oder Python. Sie können [PowerShell und weitere Skripts oder ausführbare Dateien](web-sites-create-web-jobs.md) auch als Hintergrunddienst ausführen.
* **DevOps-Optimierung**: Richten Sie mit Visual Studio Team Services, GitHub, BitBucket, Docker Hub oder Azure Container Registry die [fortlaufende Integration und Bereitstellung](app-service-continuous-deployment.md) ein. Stufen Sie Updates über [Test- und Stagingumgebungen](web-sites-staged-publishing.md)herauf. Verwalten Sie Ihre Apps in Web-Apps mithilfe von [Azure PowerShell](/powershell/azureps-cmdlets-docs) oder der [plattformübergreifenden Befehlszeilenschnittstelle (Command-Line Interface, CLI)](/cli/azure/install-azure-cli).
* **Globale Skalierung mit Hochverfügbarkeit** – Skalieren Sie manuell oder automatisch [zentral](web-sites-scale.md) oder [horizontal](../monitoring-and-diagnostics/insights-how-to-scale.md) hoch. Hosten Sie Ihre Apps überall in der globalen Rechenzentrumsinfrastruktur von Microsoft. Die App Service-[SLA](https://azure.microsoft.com/support/legal/sla/app-service/) garantiert Hochverfügbarkeit.
* **Verbindungen mit SaaS-Plattformen und lokalen Daten**: Wählen Sie aus über 50 [Connectors](../connectors/apis-list.md) für Unternehmenssysteme (z.B. SAP), SaaS-Dienste (z.B. Salesforce) sowie Internetdienste (z.B. Facebook). Greifen Sie über [Hybridverbindungen](../biztalk-services/integration-hybrid-connection-overview.md) und [Azure Virtual Networks](web-sites-integrate-with-vnet.md) auf lokale Daten zu.
* **Sicherheit und Compliance** – App Service ist [ISO-, SOC- und PCI-konform](https://www.microsoft.com/en-us/trustcenter). Authentifizieren Sie Benutzer mit [Azure Active Directory](app-service-mobile-how-to-configure-active-directory-authentication.md) oder Anmeldungen bei sozialen Netzwerken ([Google](app-service-mobile-how-to-configure-google-authentication.md), [Facebook](app-service-mobile-how-to-configure-facebook-authentication.md), [Twitter](app-service-mobile-how-to-configure-twitter-authentication.md) und [ Microsoft](app-service-mobile-how-to-configure-microsoft-authentication.md)). Erstellen Sie [IP-Adresseinschränkungen](app-service-ip-restrictions.md) und [verwalten Sie Dienstidentitäten](app-service-managed-service-identity.md).
* **Anwendungsvorlagen**: Wählen Sie im [Azure Marketplace](https://azure.microsoft.com/marketplace/) aus zahlreichen Anwendungsvorlagen wie WordPress, Joomla und Drupal.
* **Visual Studio-Integration** – Dedizierte Tools in Visual Studio optimieren das Erstellen, Bereitstellen und Debuggen.
* **API und mobile Features**: Web-Apps bieten sofort einsetzbare CORS-Unterstützung für RESTful-API-Szenarien und vereinfachen Szenarien mit mobilen Apps durch Aktivieren der Authentifizierung, Offlinedatensynchronisierung, Pushbenachrichtigungen und mehr.
* **Serverloser Code**: Führen Sie einen Codeausschnitt oder ein Skript bei Bedarf aus, ohne explizit eine Infrastruktur bereitstellen oder verwalten zu müssen, und zahlen Sie nur für die Rechenzeit, die für Ihren Code tatsächlich erforderlich ist (siehe [Dokumentation zu Azure Functions](/azure/azure-functions/)).

Neben Web-Apps in App Service bietet Azure noch andere Dienste an, die zum Hosten von Websites und Webanwendungen verwendet werden können. In den meisten Fällen sind Web-Apps die beste Wahl.  Informationen zur Microservicearchitektur finden Sie in der [Azure Service Fabric-Dokumentation](https://azure.microsoft.com/documentation/services/service-fabric). Wenn Sie eine bessere Kontrolle über die virtuellen Computer benötigen, auf denen Ihr Code ausgeführt wird, können Sie [Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/) einsetzen. Weitere Informationen zur Wahl zwischen diesen Azure-Diensten finden Sie unter [Azure App Service, Azure Cloud Services, Azure Virtual Machines und Azure Service Fabric im Vergleich](choose-web-site-cloud-service-vm.md).

## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie Ihre erste Web-App.

> [!div class="nextstepaction"]
> [ASP.NET Core](app-service-web-get-started-dotnet.md)

> [!div class="nextstepaction"]
> [ASP.NET](app-service-web-get-started-dotnet-framework.md)

> [!div class="nextstepaction"]
> [PHP](app-service-web-get-started-php.md)

> [!div class="nextstepaction"]
> [Ruby (unter Linux)](containers/quickstart-ruby.md)

> [!div class="nextstepaction"]
> [Node.js](app-service-web-get-started-nodejs.md)

> [!div class="nextstepaction"]
> [Java](app-service-web-get-started-java.md)

> [!div class="nextstepaction"]
> [Python](app-service-web-get-started-python.md)

> [!div class="nextstepaction"]
> [HTML](app-service-web-get-started-html.md)
