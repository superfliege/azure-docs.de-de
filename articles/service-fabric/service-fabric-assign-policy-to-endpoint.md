---
title: Zuweisen von Zugriffsrichtlinien zu Azure Service Fabric-Dienstendpunkten | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie HTTP- oder HTTPS-Endpunkten in Ihrem Service Fabric-Dienst Sicherheitszugriffsrichtlinien zuweisen.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: ''
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/21/2018
ms.author: mfussell
ms.openlocfilehash: 33d6d83d4dcd0ec9bebf8d4197684e0e52c48ac5
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34701317"
---
# <a name="assign-a-security-access-policy-for-http-and-https-endpoints"></a>Zuweisen einer Sicherheitszugriffsrichtlinie für HTTP- und HTTPS-Endpunkte
Wenn Sie eine RunAs-Richtlinie anwenden und das Dienstmanifest HTTP-Endpunktressourcen deklariert, müssen Sie eine Richtlinie vom Typ **SecurityAccessPolicy** angeben.  **SecurityAccessPolicy** stellt sicher, dass Ports, die diesen Endpunkten zugeordnet sind, ordnungsgemäß auf das Benutzerkonto beschränkt werden, als das der Dienst ausgeführt wird. Andernfalls hat **http.sys** keinen Zugriff auf den Dienst, sodass beim Aufrufen vom Client Fehler auftreten. Im folgenden Beispiel wird das Customer1-Konto auf den Endpunkt **EndpointName** angewendet, und es werden vollständige Zugriffsrechte gewährt.

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
```

Geben Sie für einen HTTPS-Endpunkt auch den Namen des Zertifikats für die Rückgabe an den Client an. Auf das Zertifikat wird mithilfe von **EndpointBindingPolicy** verwiesen.  Das Zertifikat wird im Anwendungsmanifest im Abschnitt **Zertifikate** definiert.

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if the EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
```

> [!WARNING] 
> Wenn HTTPS verwendet wird, verwenden Sie nicht den gleichen Port und das gleiche Zertifikat für verschiedene Dienstinstanzen (unabhängig von der Anwendung), die für den gleichen Knoten bereitgestellt werden. Das Upgrade von zwei verschiedenen Diensten unter Verwendung desselben Ports in verschiedenen Anwendungsinstanzen führt zu einem Upgradefehler. Weitere Informationen finden Sie unter [Aktualisieren von mehreren Anwendungen mit HTTPS-Endpunkten](service-fabric-application-upgrade.md#upgrading-multiple-applications-with-https-endpoints).
> 

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
Lesen Sie als Nächstes die folgenden Artikel:
* [Informationen zum Anwendungsmodell](service-fabric-application-model.md)
* [Angeben von Ressourcen in einem Dienstmanifest](service-fabric-service-manifest-resources.md)
* [Bereitstellen von Anwendungen](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
