---
title: Rollenbasierte Zugriffssteuerung in Azure Event Hubs (Vorschau) | Microsoft-Dokumentation
description: Rollenbasierte Zugriffssteuerung in Azure Event Hubs
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 07/05/2018
ms.author: sethm
ms.openlocfilehash: 76c929f482659bb81e0cabb7fb6bffa5331082bf
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2018
ms.locfileid: "39502135"
---
# <a name="active-directory-role-based-access-control-preview"></a>Rollenbasierte Zugriffssteuerung in Active Directory (Vorschau)

Microsoft Azure bietet eine integrierte Verwaltung der Zugriffssteuerung für Ressourcen und Anwendungen, basierend auf Azure Active Directory (Azure AD). Mit Azure AD können Sie Benutzerkonten und Anwendungen speziell für Ihre Azure-basierten Anwendungen verwalten. Alternativ dazu können Sie einen Verbund aus Ihrer vorhandenen Active Directory-Infrastruktur und Azure AD erstellen, um eine unternehmensweite Funktion für das einmalige Anmelden zu erhalten, die auch für Azure-Ressourcen und in Azure gehostete Anwendungen gilt. Diese Benutzer- und Anwendungsidentitäten aus Azure AD können Sie dann globalen und dienstspezifischen Rollen zuweisen, um Zugriff auf Azure-Ressourcen zu gewähren.

Bei Azure Event Hubs ist die Verwaltung der Namespaces und aller zugehörigen Ressourcen über das Azure-Portal und die Azure-Ressourcenverwaltungs-API bereits durch das Modell der *rollenbasierten Zugriffssteuerung* geschützt. Das Feature der rollenbasierten Zugriffssteuerung für Laufzeitvorgänge ist jetzt in der öffentlichen Vorschau verfügbar. 

Eine Anwendung, die die rollenbasierte Zugriffssteuerung von Azure AD verwendet, muss keine SAS-Regeln und Schlüssel oder andere Zugriffstoken für Event Hubs verarbeiten. Die Client-App interagiert mit Azure AD, um einen Authentifizierungskontext herzustellen, und erwirbt ein Zugriffstoken für Event Hubs. Bei Domänenbenutzerkonten, die eine interaktive Anmeldung erfordern, verarbeitet die Anwendung die Anmeldeinformationen niemals direkt.

## <a name="event-hubs-roles-and-permissions"></a>Event Hubs-Rollen und -Berechtigungen

In der ersten öffentlichen Vorschauversion können Sie Azure AD-Konten und -Dienstprinzipale nur zu den Rollen „Besitzer“ und „Mitwirkender“ eines Event Hubs-Namespace hinzufügen. Damit erhält die Identität vollständige Kontrolle über alle Entitäten im Namespace. Verwaltungsvorgänge, bei denen die Namespacetopologie geändert wird, werden anfänglich nur über die Azure-Ressourcenverwaltung unterstützt, nicht über die native Event Hubs-REST-Verwaltungsschnittstelle. Das bedeutet auch, dass das .NET Framework-Clientobjekt [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) nicht mit einem Azure AD-Konto verwendet werden kann.  

## <a name="use-event-hubs-with-an-azure-ad-domain-user-account"></a>Verwenden von Event Hubs mit einem Azure AD-Domänenbenutzerkonto

Der folgende Abschnitt beschreibt die Schritte, die erforderlich sind, um eine Beispielanwendung zu erstellen und bereitzustellen, die einen Benutzer zur interaktiven Azure AD-Anmeldung auffordert. Zudem wird erläutert, wie Sie Event Hubs Zugriff auf dieses Benutzerkonto gewähren und wie Sie diese Identität für den Zugriff auf Event Hubs verwenden. 

Diese Einführung beschreibt eine einfache Konsolenanwendung. Den [zugehörigen Code finden Sie auf GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Rbac/EventHubsSenderReceiverRbac/).

### <a name="create-an-active-directory-user-account"></a>Erstellen eines Active Directory-Benutzerkontos

Dieser erste Schritt ist optional. Jedes Azure-Abonnement ist automatisch an einen Azure Active Directory-Mandanten gekoppelt, und wenn Sie Zugriff auf ein Azure-Abonnement haben, ist Ihr Benutzerkonto bereits registriert. Das bedeutet, dass Sie einfach Ihr Konto verwenden können. 

Wenn Sie dennoch ein spezifisches Konto für dieses Szenario erstellen möchten, [führen Sie diese Schritte aus](../automation/automation-create-aduser-account.md). Sie müssen über die Berechtigung verfügen, Konten im Azure Active Directory-Mandanten zu erstellen. Dies ist in größeren Unternehmensszenarien möglicherweise nicht der Fall.

### <a name="create-an-event-hubs-namespace"></a>Erstellen eines Event Hubs-Namespace

Als Nächstes [erstellen Sie einen Event Hubs-Namespace](event-hubs-create.md) in einer der Azure-Regionen, die Vorschauunterstützung für die rollenbasierte Zugriffssteuerung für Event Hubs bieten: **USA, Osten**, **USA, Osten 2** oder **Europa, Westen**. 

Wenn der Namespace erstellt wurde, navigieren Sie im Portal zur Seite **Zugriffssteuerung (IAM)** des Namespace, und klicken Sie auf **Hinzufügen**, um das Azure AD-Benutzerkonto zur Rolle „Besitzer“ hinzuzufügen. Wenn Sie Ihr eigenes Benutzerkonto verwenden und den Namespace selbst erstellt haben, befinden Sie sich bereits in der Rolle „Besitzer“. Um der Rolle ein anderes Konto hinzuzufügen, suchen Sie im Bereich **Berechtigungen hinzufügen** im Feld **Auswählen** nach dem Namen der Webanwendung, und klicken Sie auf den entsprechenden Eintrag. Klicken Sie anschließend auf **Speichern**.
 
![](./media/event-hubs-role-based-access-control/rbac1.PNG)

Das Benutzerkonto verfügt jetzt über Zugriff auf den Event Hubs-Namespace und den zuvor erstellten Event Hub.
 
### <a name="register-the-application"></a>Registrieren der Anwendung

Bevor Sie die Beispielanwendung ausführen können, registrieren Sie sie in Azure AD, und bestätigen Sie die Zustimmungsaufforderung, die der Anwendung in Ihrem Namen Zugriff auf Event Hubs gewährt. 

Da es sich bei der Beispielanwendung um eine Konsolenanwendung handelt, müssen Sie eine native Anwendung registrieren und den „erforderlichen Berechtigungen“ API-Berechtigungen für **Microsoft.EventHub** hinzufügen. Native Anwendungen benötigen auch einen **redirect-URI** in Azure AD, der als Bezeichner fungiert. Bei dem URI muss es sich nicht um ein Netzwerkziel handeln. Verwenden Sie in diesem Beispiel `http://eventhubs.microsoft.com`, da der Beispielcode diesen URI bereits verwendet.

Die detaillierten Schritte für die Registrierung werden in [diesem Tutorial](../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md) erläutert. Führen Sie die Schritte zum Registrieren einer **nativen** App aus, und befolgen Sie dann die Aktualisierungsanweisungen, um die **Microsoft.EventHub**-API zu den erforderlichen Berechtigungen hinzuzufügen. Notieren Sie sich die Werte für **TenantId** und **ApplicationId** – diese werden Sie zum Ausführen der Anwendung benötigen.

### <a name="run-the-app"></a>Ausführen der App

Bevor Sie das Beispiel ausführen können, müssen Sie die Datei „App.config“ bearbeiten und gemäß Ihrem Szenario die folgenden Werte festlegen:

- `tenantId`: Legen Sie diesen Parameter auf den Wert von **TenantId** fest.
- `clientId`: Legen Sie diesen Parameter auf den Wert von **ApplicationId** fest. 
- `clientSecret`: Wenn Sie beim Anmelden einen geheimen Clientschlüssel verwenden möchten, erstellen Sie diesen in Azure AD. Verwenden Sie zudem eine Web-App oder eine API anstelle einer nativen App. Fügen Sie die App außerdem unter **Zugriffssteuerung (IAM)** zum zuvor erstellten Namespace hinzu.
- `eventHubNamespaceFQDN`: Legen Sie diesen Parameter auf den vollqualifizierten DNS-Namen Ihres neu erstellten Event Hubs-Namespace fest, z.B. `example.servicebus.windows.net`.
- `eventHubName`: Legen Sie diesen Parameter auf den von Ihnen erstellten Event Hub fest.
- Der Umleitungs-URI, den Sie in den vorherigen Schritten in Ihrer App angegeben haben.
 
Wenn Sie die Konsolenanwendung ausführen, werden Sie aufgefordert, ein Szenario auszuwählen. Klicken Sie nach Eingabe der zugehörigen Zahl auf **Interaktive Benutzeranmeldung**, und drücken Sie die EINGABETASTE. Die Anwendung zeigt ein Anmeldefenster an, fordert Ihre Zustimmung zum Zugriff auf Event Hubs an und verwendet den Dienst dann, um das Senden/Empfangen-Szenario unter Verwendung der Anmeldeidentität auszuführen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Event Hubs erhalten Sie unter den folgenden Links:

* Erste Schritte mit einem [Event Hubs-Tutorial](event-hubs-dotnet-standard-getstarted-send.md)
* [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)
* [Event Hubs – Preisübersicht](https://azure.microsoft.com/pricing/details/event-hubs/)
* [Beispielanwendungen mit Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)