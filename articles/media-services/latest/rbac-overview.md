---
title: Rollenbasierte Zugriffssteuerung für Media Services-Konten – Azure | Microsoft-Dokumentation
description: Dieser Artikel beschreibt die rollenbasierte Zugriffssteuerung (RBAC) für Azure Media Services-Konten.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/08/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 212d7f5352b58ee8f5b2c119bb1f5f828591f6bf
ms.sourcegitcommit: 6e32f493eb32f93f71d425497752e84763070fad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2019
ms.locfileid: "59471780"
---
# <a name="role-based-access-control-rbac-for-media-services-accounts"></a>Rollenbasierte Zugriffssteuerung (RBAC) für Media Services-Konten

Derzeit definiert Azure Media Services keine benutzerdefinierten dienstspezifischen Rollen. Kunden können die integrierten Rollen **Besitzer** oder **Mitwirkender** verwenden, um Vollzugriff auf ein Media Services-Konto zu erhalten. Der Hauptunterschied zwischen diesen Rollen ist, dass der **Besitzer** im Gegensatz zu einem **Mitwirkenden** steuern kann, wer Zugriff auf eine Ressource hat. Das integrierte Lesekonto verfügt nur über Lesezugriff auf das Media Services-Konto. 

## <a name="design-principles"></a>Entwurfsprinzipien

Eines der wichtigsten Entwurfsprinzipien der v3-API ist es, die API sicherer zu machen. v3-APIs geben bei einem **Get**- oder **List**-Vorgang keine geheimen Schlüssel oder Anmeldeinformationen zurück. Die Schlüssel sind immer NULL, leer oder aus der Antwort bereinigt. Der Benutzer muss eine separate Aktionsmethode zum Abrufen von Geheimnissen oder Anmeldeinformationen aufrufen. Die **Leser**-Rolle kann keine Vorgänge aufrufen, also auch keine Vorgänge wie „Asset.ListContainerSas“, „StreamingLocator.ListContentKeys“ oder „ContentKeyPolicies.GetPolicyPropertiesWithSecrets“. Getrennte Aktionen ermöglichen es Ihnen, bei Bedarf noch genauer abgestimmte RBAC-Sicherheitsberechtigungen in einer benutzerdefinierten Rolle festzulegen.

So listen Sie die von Media Services unterstützten Vorgänge auf

```csharp
foreach (Microsoft.Azure.Management.Media.Models.Operation a in client.Operations.List())
{
    Console.WriteLine($"{a.Name} - {a.Display.Operation} - {a.Display.Description}");
}
```

Im Artikel zu den [integrierten Rollendefinitionen](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) erfahren Sie genau, was die Rolle gewährt. 

Weitere Informationen finden Sie in den folgenden Artikeln:

- [Administratorrollen für klassische Abonnements, Azure RBAC-Rollen und Azure AD-Administratorrollen](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles)
- [Was ist die rollenbasierte Zugriffssteuerung (RBAC) für Azure-Ressourcen?](https://docs.microsoft.com/azure/role-based-access-control/overview)
- [Verwenden der rollenbasierten Zugriffssteuerung (RBAC) zum Verwalten des Zugriffs](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest)
- [Abrufen von Richtlinien für symmetrische Schlüssel mithilfe von Media Services – .NET](get-content-key-policy-dotnet-howto.md)

## <a name="next-steps"></a>Nächste Schritte

[Entwickeln mit Media Services v3-APIs](media-services-apis-overview.md)
