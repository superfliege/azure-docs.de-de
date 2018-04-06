---
title: Ausführen eines Azure Service Fabric-Diensts unter System- und lokalen Sicherheitskonten | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Service Fabric-Anwendung unter System- und lokalen Sicherheitskonten ausführen.  Erstellen Sie Sicherheitsprinzipale, und übernehmen Sie die Run-As-Richtlinie zur sicheren Ausführung der Dienste.
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
ms.openlocfilehash: 62917a1d342158ec2114a9204ee1ca9e447284fa
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="run-a-service-as-a-local-user-account-or-local-system-account"></a>Ausführen eines Diensts als lokales Benutzerkonto oder lokales Systemkonto
Durch Verwenden von Azure Service Fabric können Sie Anwendungen sichern, die im Cluster unter verschiedenen Benutzerkonten ausgeführt werden. Standardmäßig werden Service Fabric-Anwendungen unter dem Konto ausgeführt, unter dem der Prozess „Fabric.exe“ ausgeführt wird. Darüber hinaus bietet Service Fabric die Möglichkeit zur Ausführung von Anwendungen in einem lokalen Benutzer- oder Systemkonto, wozu eine Run-As-Richtlinie im Manifest der Anwendung angegeben wird. Unterstützte lokale Systemkontotypen sind **LocalUser**, **NetworkService**, **LocalService** und **LocalSystem**.

Sie können auch Benutzergruppen definieren und erstellen und dann jeder Gruppe Benutzer zur gemeinsamen Verwaltung hinzufügen. Dies ist nützlich, wenn es für verschiedene Diensteinstiegspunkte mehrere Benutzer gibt, die auf Gruppenebene bestimmte allgemeine Berechtigungen benötigen.

> [!NOTE] 
> Wenn Sie eine RunAs-Richtlinie auf einen Dienst anwenden und das Dienstmanifest Endpunktressourcen mit dem HTTP-Protokoll deklariert, müssen Sie eine Richtlinie vom Typ **SecurityAccessPolicy** angeben.  Weitere Informationen finden Sie unter [Zuweisen einer Sicherheitszugriffsrichtlinie für HTTP- und HTTPS-Endpunkte](service-fabric-assign-policy-to-endpoint.md). 
>

## <a name="create-local-user-groups"></a>Erstellen lokaler Benutzergruppen
Sie können Benutzergruppen definieren und erstellen werden, die es ermöglichen, dass der Gruppe ein oder mehrere Benutzer hinzugefügt werden. Dies ist nützlich, falls es für verschiedene Diensteinstiegspunkte mehrere Benutzer gibt, die auf Gruppenebene bestimmte allgemeine Berechtigungen benötigen. Das folgende Beispiel zeigt eine lokale Gruppe namens **LocalAdminGroup**, die über Administratorrechte verfügt. Zwei Benutzer, „Customer1“ und „Customer2“, wurden dieser lokalen Gruppe in diesem Anwendungsmanifestbeispiel hinzugefügt:

```xml
<Principals>
 <Groups>
   <Group Name="LocalAdminGroup">
     <Membership>
       <SystemGroup Name="Administrators"/>
     </Membership>
   </Group>
 </Groups>
  <Users>
     <User Name="Customer1">
        <MemberOf>
           <Group NameRef="LocalAdminGroup" />
        </MemberOf>
     </User>
    <User Name="Customer2">
      <MemberOf>
        <Group NameRef="LocalAdminGroup" />
      </MemberOf>
    </User>
  </Users>
</Principals>
```

## <a name="create-local-users"></a>Erstellen lokaler Benutzer
Sie können einen lokalen Benutzer erstellen, der zum Sichern eines Diensts in der Anwendung dienen kann. Wenn ein Konto vom Typ **LocalUser** im Abschnitt „Principals“ des Anwendungsmanifests angegeben wird, erstellt Service Fabric lokale Benutzerkonten auf Computern, auf denen die Anwendung bereitgestellt wird. Standardmäßig müssen diese Konten nicht die gleichen Namen wie im Anwendungsmanifest haben (z.B. „Customer3“ im folgenden Anwendungsmanifestbeispiel). Stattdessen werden sie dynamisch generiert und verfügen über zufällige Kennwörter.

```xml
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
```

Wenn eine Anwendung erfordert, dass das Benutzerkonto und Kennwort auf allen Computern gleich ist (beispielsweise zum Zwecke der NTLM-Authentifizierung), muss im Clustermanifest **NTLMAuthenticationEnabled** auf „True“ festgelegt werden. Das Clustermanifest muss darüber hinaus ein **NTLMAuthenticationPasswordSecret**-Element enthalten, das zum Erstellen des gleichen Kennworts auf allen Computern verwendet wird.

```xml
<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
```

## <a name="assign-policies-to-the-service-code-packages"></a>Zuweisen von Richtlinien zu den Dienstcodepaketen
Der Abschnitt **RunAsPolicy** für ein **ServiceManifestImport**-Element gibt das Konto aus dem Abschnitt „Principals“ an, das zum Ausführen eines Codepakets verwendet werden soll. Außerdem werden damit Codepakete aus dem Dienstmanifest Benutzerkonten im Abschnitt „Principals“ zugeordnet. Sie können dies für die Setup-Einstiegspunkte oder primären Einstiegspunkte angeben oder `All` angeben, damit diese Einstellung für beide gilt. Das folgende Beispiel zeigt die Anwendung unterschiedlicher Richtlinien:

```xml
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
```

Wenn **EntryPointType** nicht angegeben ist, wird die Standardeinstellung auf `EntryPointType=”Main”` festgelegt. Das Angeben von **SetupEntryPoint** ist besonders nützlich, wenn Sie bestimmte Setupvorgänge mit erhöhten Berechtigungen in einem Systemkonto ausführen möchten. Weitere Informationen finden Sie unter [Dienststartskript als lokaler Benutzer oder Systemkonto ausführen](service-fabric-run-script-at-service-startup.md). Der tatsächliche Dienstcode kann unter einem Konto mit weniger hohen Berechtigungen ausgeführt werden.

## <a name="apply-a-default-policy-to-all-service-code-packages"></a>Anwenden einer Standardrichtlinie auf alle Dienstcodepakete
Verwenden Sie den Abschnitt **DefaultRunAsPolicy**, um ein Standardbenutzerkonto für alle Codepakete anzugeben, für die keine bestimmte **RunAsPolicy** definiert ist. Wenn die meisten Codepakete, die im Dienstmanifest einer Anwendung angegeben sind, im gleichen Benutzerkonto ausgeführt werden müssen, kann für die Anwendung einfach eine RunAs-Standardrichtlinie mit diesem Benutzerkonto definiert werden. Im folgenden Beispiel wird beispielsweise Folgendes angegeben: Wenn für ein Codepaket keine **RunAsPolicy** angegeben ist, soll das Codepaket unter dem Standardkonto **MyDefaultAccount** ausgeführt werden, das im Abschnitt „Principals“ angegeben ist.

```xml
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
```

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte
* [Informationen zum Anwendungsmodell](service-fabric-application-model.md)
* [Angeben von Ressourcen in einem Dienstmanifest](service-fabric-service-manifest-resources.md)
* [Bereitstellen von Anwendungen](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
