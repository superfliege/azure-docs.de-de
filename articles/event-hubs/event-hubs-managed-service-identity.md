---
title: Verwaltete Dienstidentität mit Azure Event Hubs – Vorschau | Microsoft-Dokumentation
description: Verwenden von verwalteten Dienstidentitäten mit Azure Event Hubs
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/18/2017
ms.author: sethm
ms.openlocfilehash: dd50e4f6ebc5fdf5496a5127fde20bd052087b59
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/20/2017
ms.locfileid: "26783347"
---
# <a name="managed-service-identity-preview"></a>Verwaltete Dienstidentität (Vorschau)

Eine verwaltete Dienstidentität (Managed Service Identity, MSI) ist ein Azure-übergreifendes Feature, mit dem Sie eine sichere Identität für die Bereitstellung erstellen können, in der Ihr Anwendungscode ausgeführt wird. Sie können dieser Identität dann Zugriffssteuerungsrollen zuordnen, um benutzerdefinierte Berechtigungen für den Zugriff auf bestimmte Azure-Ressourcen zu gewähren, die Ihre Anwendung benötigt. 

Die Azure-Plattform verwaltet diese Laufzeitidentität mit einer verwalteten Dienstidentität. Sie müssen keine Zugriffsschlüssel in Ihrem Anwendungscode speichern und schützen – weder für die Identität selbst noch für die Ressourcen, auf die zugegriffen werden muss. Eine Event Hubs-Client-App, die in einer Azure App Service-Anwendung oder auf einem virtuellem Computer mit aktivierter MSI-Unterstützung ausgeführt wird, muss keine SAS-Regeln und -Schlüssel oder andere Zugriffstoken verarbeiten. Die Client-App benötigt nur die Endpunktadresse des Event Hubs-Namespace. Wenn die App eine Verbindung herstellt, bindet Event Hubs den MSI-Kontext an den Client. Der entsprechende Vorgang wird weiter unten in diesem Artikel in einem Beispiel gezeigt.

Nach der Zuordnung zu einer verwalteten Dienstidentität kann ein Event Hubs-Client alle autorisierten Vorgänge ausführen. Die Autorisierung wird durch Zuordnung von Event Hubs-Rollen zu verwalteten Dienstidentitäten gewährt. 

## <a name="event-hubs-roles-and-permissions"></a>Event Hubs-Rollen und -Berechtigungen

In der ersten öffentlichen Vorschauversion können Sie eine verwaltete Dienstidentität nur zu den Rollen „Besitzer“ oder „Mitwirkender“ eines Event Hubs-Namespace hinzufügen. Damit erhält die Identität vollständige Kontrolle über alle Entitäten im Namespace. Verwaltungsvorgänge, bei denen die Namespacetopologie geändert wird, werden anfänglich jedoch nur über Azure Resource Manager unterstützt, nicht über die native Event Hubs-REST-Verwaltungsschnittstelle. Das bedeutet auch, dass Sie das .NET Framework-Clientobjekt [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) innerhalb einer verwalteten Dienstidentität nicht verwenden können. 
 
## <a name="use-event-hubs-with-a-managed-service-identity"></a>Verwenden von Event Hubs mit einer verwalteten Dienstidentität

Der folgende Abschnitt beschreibt die Schritte, die erforderlich sind, um eine Beispielanwendung zu erstellen und bereitzustellen, die mit einer verwalteten Dienstidentität ausgeführt wird. Zudem erläutert der Abschnitt, wie Sie dieser Identität Zugriff auf einen Event Hubs-Namespace gewähren und wie die Anwendung mit den Event Hubs-Entitäten interagiert, die diese Identität verwenden.

Diese Einführung beschreibt eine Webanwendung, die in [Azure App Service](https://azure.microsoft.com/services/app-service/) gehostet wird. Die Schritte für eine durch einen virtuellen Computer gehostete Anwendung sind ähnlich.

### <a name="create-an-app-service-web-application"></a>Erstellen einer App Service-Webanwendung

Der erste Schritt besteht darin, eine App Service-ASP.NET-Anwendung zu erstellen. Wenn Sie mit der Vorgehensweise in Azure nicht vertraut sind, befolgen Sie diese [Anleitung](../app-service/app-service-web-get-started-dotnet-framework.md). Anstatt jedoch, wie in diesem Tutorial gezeigt, eine MVC-Anwendung zu erstellen, erstellen Sie eine Web Forms-Anwendung.

### <a name="set-up-the-managed-service-identity"></a>Einrichten der verwalteten Dienstidentität

Nachdem Sie die Anwendung erstellt haben, navigieren Sie im Azure-Portal zu der neu erstellten Webanwendung (ebenfalls in der Anleitung gezeigt). Navigieren Sie dann zur Seite **Verwaltete Dienstidentität**, und aktivieren Sie das Feature: 

![](./media/event-hubs-managed-service-identity/msi1.png)
 
Wenn Sie das Feature aktiviert haben, wird in Ihrem Azure Active Directory eine neue Dienstidentität erstellt und auf dem App Service-Host konfiguriert.

### <a name="create-a-new-event-hubs-namespace"></a>Erstellen eines neuen Event Hubs-Namespace

Als Nächstes [erstellen Sie einen Event Hubs-Namespace](event-hubs-create.md) in einer der Azure-Regionen, die Vorschauunterstützung für die rollenbasierte Zugriffssteuerung für verwaltete Dienstidentitäten bieten: **USA, Osten**, **USA, Osten 2** oder **Europa, Westen**. 

Navigieren Sie im Portal zur Seite **Zugriffssteuerung (IAM)** des Namespace, und klicken Sie auf **Hinzufügen**, um die verwaltete Dienstidentität zur Rolle **Besitzer** hinzuzufügen. Suchen Sie hierzu im Bereich **Berechtigungen hinzufügen** im Feld **Auswählen** nach dem Namen der Webanwendung, und klicken Sie auf den entsprechenden Eintrag. Klicken Sie anschließend auf **Speichern**.

![](./media/event-hubs-managed-service-identity/msi2.png)
 
Die verwaltete Dienstidentität der Webanwendung verfügt jetzt über Zugriff auf den Event Hubs-Namespace und den zuvor erstellten Event Hub. 

### <a name="run-the-app"></a>Ausführen der App

Bearbeiten Sie jetzt die Standardseite der von Ihnen erstellten ASP.NET-Anwendung. Sie können auch den Webanwendungscode aus [diesem GitHub-Repository](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/MSI/EventHubsMSIDemoWebApp) verwenden. 

Nachdem Sie Ihre App gestartet haben, zeigen Sie in Ihrem Browser auf „EventHubsMSIDemo.aspx“. Sie können die Seite auch als Startseite festlegen. Sie finden den Code in der Datei „EventHubsMSIDemo.aspx.cs“. Das Ergebnis ist eine kleine Webanwendung mit einigen wenigen Eingabefeldern sowie Schaltflächen zum **Senden** und **Empfangen**, mit denen eine Verbindung mit Event Hubs hergestellt wird, um Nachrichten zu senden oder zu empfangen. 

Beachten Sie, wie das Objekt [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) initialisiert wird. Der Code verwendet nicht den SAS-Tokenanbieter (Shared Access Signature), sondern erstellt mit dem Aufruf `TokenProvider.CreateManagedServiceIdentityTokenProvider(ServiceAudience.EventHubAudience)` einen Tokenanbieter für die verwaltete Dienstidentität. Daher müssen keine Geheimnisse aufbewahrt und verwendet werden. Der Flow vom MSI-Kontext zu Event Hubs und der Autorisierungshandshake werden automatisch vom Tokenanbieter verarbeitet. Dies ist ein einfacheres Modell als die Verwendung von SAS.

Nachdem Sie diese Änderungen vorgenommen haben, veröffentlichen Sie die Anwendung und führen sie aus. Eine einfache Möglichkeit, die richtigen Veröffentlichungsdaten zu erhalten, besteht darin, ein Veröffentlichungsprofil herunterzuladen und in Visual Studio zu importieren:

![](./media/event-hubs-managed-service-identity/msi3.png)
 
Um Nachrichten zu senden oder zu empfangen, geben Sie den Namen des Namespace und der von Ihnen erstellten Entität ein, und klicken Sie entweder auf **Senden** oder **Empfangen**. 
 
Beachten Sie, dass die verwaltete Dienstidentität nur innerhalb der Azure-Umgebung und nur in der App Service-Bereitstellung funktioniert, in der Sie sie konfiguriert haben. Beachten Sie auch, dass verwaltete Dienstidentitäten derzeit noch nicht mit App Service-Bereitstellungsslots funktionieren.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Event Hubs erhalten Sie unter den folgenden Links:

* Erste Schritte mit einem [Event Hubs-Tutorial](event-hubs-dotnet-standard-getstarted-send.md)
* [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)
* [Event Hubs – Preisübersicht](https://azure.microsoft.com/pricing/details/event-hubs/)
* [Beispielanwendungen mit Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)