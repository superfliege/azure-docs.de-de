---
title: Ausführen eines Azure Service Fabric-Diensts als AD-Benutzer oder -Gruppe | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie einen Dienst als Active Directory-Benutzer oder -Gruppe auf einem eigenständigen, Windows-basierten Service Fabric-Cluster ausführen.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: ''
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/21/2018
ms.author: mfussell
ms.openlocfilehash: 1cf23a8f564553e65ac2c0fd34d44d81fe2327ea
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="run-a-service-as-an-active-directory-user-or-group"></a>Ausführen eines Diensts als Active Directory-Benutzer oder -Gruppe
Durch Verwenden von Azure Service Fabric können Sie Anwendungen sichern, die im Cluster unter verschiedenen Benutzerkonten ausgeführt werden. So lässt sich erreichen, dass ausgeführte Anwendungen auch in einer gemeinsamen gehosteten Umgebung sicher voneinander abgegrenzt sind. Standardmäßig werden Service Fabric-Anwendungen unter dem Konto ausgeführt, unter dem der Prozess „Fabric.exe“ ausgeführt wird. Für einen eigenständigen Windows Server-Cluster können Sie einen Dienst unter Verwendung einer RunAs-Richtlinie als [gruppenverwaltetes Dienstkonto (group Managed Service Account, gMSA)](service-fabric-run-service-as-gmsa.md) oder als Active Directory-Benutzer oder -Gruppe ausführen. Hinweis: Hierbei wird Active Directory lokal in Ihrer Domäne verwendet, nicht Azure Active Directory (Azure AD).

Indem Sie einen Domänenbenutzer oder eine -gruppe verwenden, können Sie anschließend auf andere Ressourcen in der Domäne (z.B. Dateifreigaben) zugreifen, für die Berechtigungen gewährt wurden.

Im folgenden Beispiel wird ein Active Directory-Benutzer mit dem Namen *TestUser* verwendet, dessen Domänenkennwort mit dem Zertifikat *MyCert* verschlüsselt wird. Sie können den PowerShell-Befehl `Invoke-ServiceFabricEncryptText` verwenden, um den Verschlüsselungstext für den geheimen Schlüssel zu erstellen. Weitere Informationen finden Sie unter [Verwalten von Geheimnissen in Service Fabric-Anwendungen](service-fabric-application-secret-management.md).

Sie müssen den privaten Schlüssel des Zertifikats zum Entschlüsseln des Kennworts mit einer Out-of-Band-Methode auf dem lokalen Computer bereitstellen (in Azure erfolgt dies über Azure Resource Manager). Wenn Service Fabric das Dienstpaket dann auf dem Computer bereitstellt, kann Service Fabric den geheimen Schlüssel entschlüsseln und (zusammen mit dem Benutzernamen) zum Authentifizieren bei Active Directory verwenden, sodass die Ausführung mit diesen Anmeldeinformationen erfolgt.

```xml
<Principals>
  <Users>
    <User Name="TestUser" AccountType="DomainUser" AccountName="Domain\User" Password="[Put encrypted password here using MyCert certificate]" PasswordEncrypted="true" />
  </Users>
</Principals>
<Policies>
  <DefaultRunAsPolicy UserRef="TestUser" />
  <SecurityAccessPolicies>
    <SecurityAccessPolicy ResourceRef="MyCert" PrincipalRef="TestUser" GrantRights="Full" ResourceType="Certificate" />
  </SecurityAccessPolicies>
</Policies>
<Certificates>
```

> [!NOTE] 
> Wenn Sie eine RunAs-Richtlinie auf einen Dienst anwenden und das Dienstmanifest Endpunktressourcen mit dem HTTP-Protokoll deklariert, müssen Sie auch eine Richtlinie vom Typ **SecurityAccessPolicy** angeben.  Weitere Informationen finden Sie unter [Zuweisen einer Sicherheitszugriffsrichtlinie für HTTP- und HTTPS-Endpunkte](service-fabric-assign-policy-to-endpoint.md). 
>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
Lesen Sie als Nächstes die folgenden Artikel:
* [Informationen zum Anwendungsmodell](service-fabric-application-model.md)
* [Angeben von Ressourcen in einem Dienstmanifest](service-fabric-service-manifest-resources.md)
* [Bereitstellen von Anwendungen](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
