---
title: Verwaltete Dienstidentität mit Azure Service Bus – Vorschau | Microsoft-Dokumentation
description: Verwenden von verwalteten Dienstidentitäten mit Azure Service Bus
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/19/2017
ms.author: sethm
ms.openlocfilehash: 7b9901ee3478cb193c808b65d2dbbcf8b596a3c1
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
ms.locfileid: "29874651"
---
# <a name="managed-service-identity-preview"></a>Verwaltete Dienstidentität (Vorschau)

Eine verwaltete Dienstidentität (Managed Service Identity, MSI) ist ein Azure-übergreifendes Feature, mit dem Sie eine sichere Identität für die Bereitstellung erstellen können, in der Ihr Anwendungscode ausgeführt wird. Sie können dieser Identität dann Zugriffssteuerungsrollen zuordnen, um benutzerdefinierte Berechtigungen für den Zugriff auf bestimmte Azure-Ressourcen zu gewähren, die Ihre Anwendung benötigt.

Die Azure-Plattform verwaltet diese Laufzeitidentität mit einer verwalteten Dienstidentität. Sie müssen keine Zugriffsschlüssel in Ihrem Anwendungscode speichern und schützen – weder für die Identität selbst noch für die Ressourcen, auf die zugegriffen werden muss. Eine Service Bus-Client-App, die in einer Azure App Service-Anwendung oder auf einem virtuellem Computer mit aktivierter MSI-Unterstützung ausgeführt wird, muss keine SAS-Regeln und -Schlüssel oder andere Zugriffstoken verarbeiten. Die Client-App benötigt nur die Endpunktadresse des Service Bus Messaging-Namespace. Wenn die App eine Verbindung herstellt, bindet Service Bus den MSI-Kontext an den Client. Der entsprechende Vorgang wird weiter unten in diesem Artikel in einem Beispiel gezeigt. 

Nach der Zuordnung zu einer verwalteten Dienstidentität kann ein Service Bus-Client alle autorisierten Vorgänge ausführen. Die Autorisierung wird durch Zuordnung von Service Bus-Rollen zu verwalteten Dienstidentitäten gewährt. 

## <a name="service-bus-roles-and-permissions"></a>Service Bus-Rollen und -Berechtigungen

In der ersten öffentlichen Vorschauversion können Sie eine verwaltete Dienstidentität nur zu den Rollen „Besitzer“ oder „Mitwirkender“ eines Service Bus-Namespace hinzufügen. Damit erhält die Identität vollständige Kontrolle über alle Entitäten im Namespace. Verwaltungsvorgänge, bei denen die Namespacetopologie geändert wird, werden anfänglich jedoch nur über Azure Resource Manager unterstützt, nicht über die native Service Bus-REST-Verwaltungsschnittstelle. Das bedeutet auch, dass Sie das .NET Framework-Clientobjekt [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) innerhalb einer verwalteten Dienstidentität nicht verwenden können.

## <a name="use-service-bus-with-a-managed-service-identity"></a>Verwenden von Service Bus mit einer verwalteten Dienstidentität

Der folgende Abschnitt beschreibt die Schritte, die erforderlich sind, um eine Beispielanwendung zu erstellen und bereitzustellen, die mit einer verwalteten Dienstidentität ausgeführt wird. Zudem erläutert der Abschnitt, wie Sie dieser Identität Zugriff auf einen Service Bus Messaging-Namespace gewähren und wie die Anwendung mit den Service Bus-Entitäten interagiert, die diese Identität verwenden.

Diese Einführung beschreibt eine Webanwendung, die in [Azure App Service](https://azure.microsoft.com/services/app-service/) gehostet wird. Die Schritte für eine durch einen virtuellen Computer gehostete Anwendung sind ähnlich.

### <a name="create-an-app-service-web-application"></a>Erstellen einer App Service-Webanwendung

Der erste Schritt besteht darin, eine App Service-ASP.NET-Anwendung zu erstellen. Wenn Sie mit der Vorgehensweise in Azure nicht vertraut sind, befolgen Sie diese [Anleitung](../app-service/app-service-web-get-started-dotnet-framework.md). Anstatt jedoch, wie in diesem Tutorial gezeigt, eine MVC-Anwendung zu erstellen, erstellen Sie eine Web Forms-Anwendung.

### <a name="set-up-the-managed-service-identity"></a>Einrichten der verwalteten Dienstidentität

Nachdem Sie die Anwendung erstellt haben, navigieren Sie im Azure-Portal zu der neu erstellten Webanwendung (ebenfalls in der Anleitung gezeigt). Navigieren Sie dann zur Seite **Verwaltete Dienstidentität**, und aktivieren Sie das Feature: 

![](./media/service-bus-managed-service-identity/msi1.png)

Wenn Sie das Feature aktiviert haben, wird in Ihrem Azure Active Directory eine neue Dienstidentität erstellt und auf dem App Service-Host konfiguriert.

### <a name="create-a-new-service-bus-messaging-namespace"></a>Erstellen eines neuen Service Bus-Namespace

Als Nächstes [erstellen Sie einen Service Bus Messaging-Namespace](service-bus-create-namespace-portal.md) in einer der Azure-Regionen, die Vorschauunterstützung für die rollenbasierte Zugriffssteuerung bieten: **USA, Osten**, **USA, Osten 2** oder **Europa, Westen**. 

Navigieren Sie im Portal zur Seite **Zugriffssteuerung (IAM)** des Namespace, und klicken Sie auf **Hinzufügen**, um die verwaltete Dienstidentität zur Rolle **Besitzer** hinzuzufügen. Suchen Sie hierzu im Bereich **Berechtigungen hinzufügen** im Feld **Auswählen** nach dem Namen der Webanwendung, und klicken Sie auf den entsprechenden Eintrag. Klicken Sie anschließend auf **Speichern**.

![](./media/service-bus-managed-service-identity/msi2.png)
 
Die verwaltete Dienstidentität der Webanwendung verfügt jetzt über Zugriff auf den Service Bus-Namespace und die zuvor erstellte Warteschlange. 

### <a name="run-the-app"></a>Ausführen der App

Bearbeiten Sie jetzt die Standardseite der von Ihnen erstellten ASP.NET-Anwendung. Sie können auch den Webanwendungscode aus [diesem GitHub-Repository](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/ManagedServiceIdentity) verwenden.

Die Seite „Default.aspx“ ist Ihre Zielseite. Sie finden den Code in der Datei „Default.aspx.cs“. Das Ergebnis ist eine kleine Webanwendung mit einigen wenigen Eingabefeldern sowie Schaltflächen zum **Senden** und **Empfangen**, mit denen eine Verbindung mit Service Bus hergestellt wird, um Nachrichten zu senden oder zu empfangen.

Beachten Sie, wie das Objekt [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) initialisiert wird. Der Code verwendet nicht den SAS-Tokenanbieter (Shared Access Signature), sondern erstellt mit dem Aufruf `TokenProvider.CreateManagedServiceIdentityTokenProvider(ServiceAudience.ServiceBusAudience)` einen Tokenanbieter für die verwaltete Dienstidentität. Daher müssen keine Geheimnisse aufbewahrt und verwendet werden. Der Flow vom MSI-Kontext zu Service Bus und der Autorisierungshandshake werden automatisch vom Tokenanbieter verarbeitet. Dies ist ein einfacheres Modell als die Verwendung von SAS.

Nachdem Sie diese Änderungen vorgenommen haben, veröffentlichen Sie die Anwendung und führen sie aus. Eine einfache Möglichkeit, die richtigen Veröffentlichungsdaten zu erhalten, besteht darin, ein Veröffentlichungsprofil herunterzuladen und in Visual Studio zu importieren:

![](./media/service-bus-managed-service-identity/msi3.png)
 
Um Nachrichten zu senden oder zu empfangen, geben Sie den Namen des Namespace und der von Ihnen erstellten Entität ein, und klicken Sie entweder auf **Senden** oder **Empfangen**.
 
Beachten Sie, dass die verwaltete Dienstidentität nur innerhalb der Azure-Umgebung und nur in der App Service-Bereitstellung funktioniert, in der Sie sie konfiguriert haben. Beachten Sie auch, dass verwaltete Dienstidentitäten derzeit noch nicht mit App Service-Bereitstellungsslots funktionieren.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Service Bus Messaging finden Sie in folgenden Themen.

* [Service Bus – Grundlagen](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-Warteschlangen, -Themen und -Abonnements](service-bus-queues-topics-subscriptions.md)
* [Erste Schritte mit Service Bus-Warteschlangen](service-bus-dotnet-get-started-with-queues.md)
* [Verwenden von Service Bus-Themen und -Abonnements](service-bus-dotnet-how-to-use-topics-subscriptions.md)