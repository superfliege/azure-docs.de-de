---
title: Azure Service Fabric Docker Compose (Vorschau) | Microsoft-Dokumentation
description: "Azure Service Fabric akzeptiert das Docker Compose Format für eine einfachere Orchestrierung vorhandener Container mithilfe von Service Fabric. Diese Unterstützung befindet sich derzeit in der Vorschauphase."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 955f84e5656bbf568234cbaf69faa4dd0a741206
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2017
---
# <a name="using-volume-plugins-and-logging-drivers-in-your-container"></a>Verwenden von Volume-Plug-Ins und Protokollierungstreibern im Container
Service Fabric unterstützt die Angabe von [Docker-Volume-Plug-Ins](https://docs.docker.com/engine/extend/plugins_volume/) und [Docker-Protokollierungstreibern](https://docs.docker.com/engine/admin/logging/overview/) für Ihren Containerdienst.  Dies ermöglicht es Ihnen, Ihre Daten in [Azure Files](https://azure.microsoft.com/en-us/services/storage/files/) beizubehalten, auch wenn Ihr Container auf einen anderen Host verschoben oder neu gestartet wird.

Derzeit gibt es nur Volumetreiber für Linux-Container, wie unten gezeigt.  Wenn Sie Windows-Container verwenden, ist es möglich, ein Volume einer [SMB3-Freigabe](https://blogs.msdn.microsoft.com/clustering/2017/08/10/container-storage-support-with-cluster-shared-volumes-csv-storage-spaces-direct-s2d-smb-global-mapping/) von Azure Files ohne Volumetreiber mit der neuesten Version (1709) von Windows Server zuzuordnen. Dazu müssen Sie Ihre Virtual Machines in Ihrem Cluster auf Windows Server, Version 1709, aktualisieren.


## <a name="install-volumelogging-driver"></a>Installieren des Volume-/Protokollierungstreibers

Wenn der Docker-Volume-/Protokollierungstreiber auf dem Computer nicht installiert ist, installieren Sie ihn manuell, indem Sie über RDP/SSH eine Verbindung mit dem Computer herstellen oder ein [VMSS-Startskript](https://azure.microsoft.com/en-us/resources/templates/201-vmss-custom-script-windows/) oder ein [SetupEntryPoint](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-application-model#describe-a-service)-Skript verwenden. Wenn Sie eine der aufgeführten Methoden auswählen, können Sie ein Skript zum Installieren des [Docker-Volumetreibers für Azure](https://docs.docker.com/docker-for-azure/persistent-data-volumes/) schreiben:


```bash
docker plugin install --alias azure --grant-all-permissions docker4x/cloudstor:17.09.0-ce-azure1  \
    CLOUD_PLATFORM=AZURE \
    AZURE_STORAGE_ACCOUNT="[MY-STORAGE-ACCOUNT-NAME]" \
    AZURE_STORAGE_ACCOUNT_KEY="[MY-STORAGE-ACCOUNT-KEY]" \
    DEBUG=1
```

## <a name="specify-the-plugin-or-driver-in-the-manifest"></a>Angeben des Plug-Ins oder Treibers im Manifest
Die Plug-Ins werden im Anwendungsmanifest angegeben. Siehe dazu das folgende Manifest:

```xml
?xml version="1.0" encoding="UTF-8"?>
<ApplicationManifest ApplicationTypeName="WinNodeJsApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <Description>Calculator Application</Description>
    <Parameters>
        <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
      <Parameter Name="MyCpuShares" DefaultValue="3"></Parameter>
    </Parameters>
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeServicePackage" ServiceManifestVersion="1.0"/>
     <Policies>
       <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv"> 
        <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
        <RepositoryCredentials PasswordEncrypted="false" Password="****" AccountName="test"/>
        <LogConfig Driver="etwlogs" >
          <DriverOption Name="test" Value="vale"/>
        </LogConfig>
        <Volume Source="c:\workspace" Destination="c:\testmountlocation1" IsReadOnly="false"></Volume>
        <Volume Source="d:\myfolder" Destination="c:\testmountlocation2" IsReadOnly="true"> </Volume>
        <Volume Source="myvolume1" Destination="c:\testmountlocation2" Driver="azure" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
        </Volume>
       </ContainerHostPolicies>
   </Policies>
    </ServiceManifestImport>
    <ServiceTemplates>
        <StatelessService ServiceTypeName="StatelessNodeService" InstanceCount="5">
            <SingletonPartition></SingletonPartition>
        </StatelessService>
    </ServiceTemplates>
</ApplicationManifest>
```

Im Beispiel oben bezieht sich das `Source`-Tag für das `Volume` auf den Quellordner. Der Quellordner kann ein Ordner auf dem virtuellen Computer, der die Container hostet, oder ein persistenter Remotespeicher sein. Das `Destination`-Tag ist der Speicherort, dem `Source` im ausgeführten Container zugeordnet ist.  Ihr Ziel kann also nicht ein bereits vorhandener Speicherort innerhalb Ihres Containers sein.

Wenn Sie ein Volume-Plug-In angeben, erstellt Service Fabric das Volume automatisch mit den angegebenen Parametern. Das `Source`-Tag ist der Name des Volumes, und das `Driver`-Tag gibt das Volumetreiber-Plug-In an. Optionen können wie im folgenden Codeausschnitt mithilfe des `DriverOption`-Tags angegeben werden:

```xml
<Volume Source="myvolume1" Destination="c:\testmountlocation4" Driver="azure" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
</Volume>
```
Wenn ein Docker-Protokolltreiber angegeben wird, müssen Agents (oder Container) für die Verarbeitung der Protokolle im Cluster bereitgestellt werden.  Mit dem `DriverOption`-Tag können außerdem Protokolltreiberoptionen angegeben werden.

Informationen zum Bereitstellen von Containern in einem Service Fabric-Cluster finden Sie in den folgenden Artikeln:


[Bereitstellen eines Containers in Service Fabric](service-fabric-deploy-container.md)

