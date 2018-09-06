---
title: Azure Service Bus-Verwaltungsbibliotheken| Microsoft-Dokumentation
description: Verwalten von Service Bus-Namespaces und -Messagingentitäten über .NET.
services: service-bus-messaging
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/05/2018
ms.author: spelluru
ms.openlocfilehash: a959687fbf6e296cab7e0d8ca49ae97a005622cf
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2018
ms.locfileid: "43696574"
---
# <a name="service-bus-management-libraries"></a>Service Bus-Verwaltungsbibliotheken

Die Azure Service Bus-Verwaltungsbibliotheken können dynamisch Service Bus-Namespaces und -Entitäten bereitstellen. Dies ermöglicht komplexe Bereitstellungen und Messagingszenarios sowie eine programmgesteuerte Bestimmung der bereitzustellenden Entitäten. Die Bibliotheken sind derzeit für .NET verfügbar.

## <a name="supported-functionality"></a>Unterstützte Funktionen

* Erstellen, Aktualisieren und Löschen von Namespaces
* Erstellen, Aktualisieren und Löschen von Warteschlangen
* Erstellen, Aktualisieren und Löschen von Themen
* Erstellen, Aktualisieren und Löschen von Abonnements

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung der Service Bus-Verwaltungsbibliotheken müssen Sie sich zunächst beim Azure Active Directory-Dienst (Azure AD) authentifizieren. Azure AD erfordert, dass Sie sich als Dienstprinzipal authentifizieren. Dadurch erhalten Sie Zugriff auf Ihre Azure-Ressourcen. Informationen zum Erstellen eines Dienstprinzipals finden Sie in einem der folgenden Artikel:  

* [Erstellen einer Active Directory-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals](/azure/azure-resource-manager/resource-group-create-service-principal-portal)
* [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe von Azure PowerShell](/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe der Azure-Befehlszeilenschnittstelle](/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)

In diesen Tutorials erhalten Sie Werte für `AppId` (Client-ID), `TenantId` und `ClientSecret` (Authentifizierungsschlüssel). Diese werden von den Verwaltungsbibliotheken für die Authentifizierung verwendet. Für die Ressourcengruppe, für welche die Ausführung erfolgen soll, müssen Sie über Berechtigungen vom Typ **Besitzer** verfügen.

## <a name="programming-pattern"></a>Muster für die Programmierung

Das Muster zum Bearbeiten einer beliebigen Service Bus-Ressource folgt einem gemeinsamen Protokoll:

1. Rufen Sie mithilfe der **Microsoft.IdentityModel.Clients.ActiveDirectory**-Bibliothek ein Token aus Azure AD ab:
   ```csharp
   var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

   var result = await context.AcquireTokenAsync("https://management.core.windows.net/", new ClientCredential(clientId, clientSecret));
   ```
2. Erstellen Sie das `ServiceBusManagementClient`-Objekt:

   ```csharp
   var creds = new TokenCredentials(token);
   var sbClient = new ServiceBusManagementClient(creds)
   {
       SubscriptionId = SettingsCache["SubscriptionId"]
   };
   ```
3. Legen Sie die `CreateOrUpdate`-Parameter auf Ihre spezifischen Werte fest:

   ```csharp
   var queueParams = new QueueCreateOrUpdateParameters()
   {
       Location = SettingsCache["DataCenterLocation"],
       EnablePartitioning = true
   };
   ```
4. Führen Sie den Aufruf aus:

   ```csharp
   await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, QueueName, queueParams);
   ```

## <a name="next-steps"></a>Nächste Schritte

* [.NET-Verwaltungsbeispiel](https://github.com/Azure-Samples/service-bus-dotnet-management/)
* [Microsoft.Azure.Management.ServiceBus-API-Referenz](/dotnet/api/Microsoft.Azure.Management.ServiceBus)
