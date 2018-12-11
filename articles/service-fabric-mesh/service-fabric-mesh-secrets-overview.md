---
title: Speichern und Verwenden von Azure Service Fabric Mesh-Anwendungsgeheimnissen | Microsoft-Dokumentation
description: Speichern und Verwenden von Service Fabric Mesh-Geheimnissen
services: service-fabric-mesh
keywords: secrets
author: v-steg
ms.author: v-steg
ms.date: 10/25/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: jeanpaul.connock
ms.openlocfilehash: 63ac34e184d537eba2b915d4d108c7c1d8368aba
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2018
ms.locfileid: "52891930"
---
# <a name="service-fabric-mesh-application-secrets"></a>Service Fabric Mesh-Anwendungsgeheimnisse
Service Fabric Mesh unterstützt Geheimnisse als Azure-Ressourcen. Bei einem Service Fabric Mesh-Geheimnis kann es sich um beliebige sensible Informationen in Textform handeln, z.B. Speicherverbindungszeichenfolgen, Kennwörter oder andere Werte, die sicher gespeichert und übertragen werden sollten.

![Übersicht über Mesh-Geheimnisse][sf-mesh-secrets-overview]

## <a name="mesh-secrets-resources"></a>Mesh-Geheimnisressourcen
Ein Mesh-Anwendungsgeheimnis umfasst Folgendes:
* Eine Ressource **Geheimnisse**, bei der es sich um einen Container zum Speichern von Textgeheimnissen handelt. In der Ressource **Geheimnisse** enthaltene Geheimnisse werden auf sichere Weise gespeichert und übertragen.
* Mindestens eine Ressource vom Typ **Geheimnisse/Werte**, die im Ressourcencontainer **Geheimnisse** gespeichert ist. Jede Ressource **Geheimnisse/Werte** verfügt über eine Versionsnummer.

## <a name="inline-or-stored-in-azure-key-vault"></a>Inline oder in Azure Key Vault gespeichert
- Mesh-Anwendungen und -Dienstressourcen beinhalten eine verwaltete Dienstidentität (MSI) mit Azure Active Directory (AAD), um auf Geheimnisse in Azure Key Vault zugreifen zu können.
- Für Geheimnisse und Zertifikate kann mit Richtlinien ein automatischer Rollover ausgeführt werden.

## <a name="next-steps"></a>Nächste Schritte 
Weitere Informationen zu Service Fabric Mesh-Geheimnissen finden Sie in den folgenden Artikeln:
- [Verwalten von Service Fabric Mesh-Anwendungsgeheimnissen](service-fabric-mesh-howto-manage-secrets.md)
- [Einführung in das Service Fabric-Ressourcenmodell](service-fabric-mesh-service-fabric-resources.md)

<!-- pics -->
[sf-mesh-secrets-overview]: ./media/service-fabric-mesh-secrets-overview/MeshAppSecretsOverview.png
