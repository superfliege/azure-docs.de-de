---
title: Azure Service Fabric Docker Compose (Vorschau) | Microsoft-Dokumentation
description: "Azure Service Fabric akzeptiert das Docker Compose-Format für eine einfachere Orchestrierung vorhandener Container mithilfe von Service Fabric. Die Unterstützung für Docker Compose befindet sich derzeit in der Vorschauphase."
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
ms.openlocfilehash: 772e51519d1ad45ababa0f4c1f4b402d280f9c14
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2017
---
# <a name="use-docker-volume-plug-ins-and-logging-drivers-in-your-container"></a>Verwenden von Docker-Volume-Plug-Ins und Docker-Protokollierungstreibern im Container
Azure Service Fabric unterstützt die Angabe von [Docker-Volume-Plug-Ins](https://docs.docker.com/engine/extend/plugins_volume/) und [Docker-Protokollierungstreibern](https://docs.docker.com/engine/admin/logging/overview/) für Ihren Containerdienst. Sie können Ihre Daten in [Azure Files](https://azure.microsoft.com/services/storage/files/) beibehalten, wenn Ihr Container auf einen anderen Host verschoben oder neu gestartet wird.

Derzeit werden nur Volumetreiber für Linux-Container unterstützt. Wenn Sie Windows-Container verwenden, können Sie ein Volume ohne einen Volumetreiber zu einer [SMB3-Freigabe](https://blogs.msdn.microsoft.com/clustering/2017/08/10/container-storage-support-with-cluster-shared-volumes-csv-storage-spaces-direct-s2d-smb-global-mapping/) von Azure Files zuordnen. Aktualisieren Sie für diese Zuordnung Ihre virtuellen Computer in Ihrem Cluster auf die neueste Version 1709 von Windows Server.


## <a name="install-the-docker-volumelogging-driver"></a>Installieren des Docker-Volume-/Protokollierungstreibers

Wenn der Docker-Volume-/Protokollierungstreiber nicht auf dem Computer installiert ist, können Sie ihn mithilfe der RDP/SSH-Protokolle manuell installieren. Sie können die Installation mit diesen Protokollen über ein [VMSS-Startskript](https://azure.microsoft.com/resources/templates/201-vmss-custom-script-windows/) oder ein [SetupEntryPoint-Skript](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-model#describe-a-service) durchführen.

Beispiel für das Skript zum Installieren des [Docker-Volumetreibers für Azure](https://docs.docker.com/docker-for-azure/persistent-data-volumes/):

```bash
docker plugin install --alias azure --grant-all-permissions docker4x/cloudstor:17.09.0-ce-azure1  \
    CLOUD_PLATFORM=AZURE \
    AZURE_STORAGE_ACCOUNT="[MY-STORAGE-ACCOUNT-NAME]" \
    AZURE_STORAGE_ACCOUNT_KEY="[MY-STORAGE-ACCOUNT-KEY]" \
    DEBUG=1
```

## <a name="specify-the-plug-in-or-driver-in-the-manifest"></a>Angeben des Plug-Ins oder Treibers im Manifest
Die Plug-Ins sind im Anwendungsmanifest wie folgt angegeben:

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

Das **Source**-Tag für das **Volume**-Element verweist auf den Quellordner. Der Quellordner kann ein Ordner auf dem virtuellen Computer, der die Container hostet, oder ein persistenter Remotespeicher sein. Das **Destination**-Tag ist der Speicherort, dem **Source** im ausgeführten Container zugeordnet ist. Das Ziel kann also nicht ein bereits vorhandener Speicherort innerhalb des Containers sein.

Wenn Sie ein Volume-Plug-In angeben, erstellt Service Fabric das Volume automatisch mit den angegebenen Parametern. Das **Source**-Tag ist der Name des Volumes, und das **Driver**-Tag gibt das Volumetreiber-Plug-In an. Optionen können mithilfe des **DriverOption**-Tags wie folgt angegeben werden:

```xml
<Volume Source="myvolume1" Destination="c:\testmountlocation4" Driver="azure" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
</Volume>
```
Wenn ein Docker-Protokolltreiber angegeben wird, müssen Sie Agents (oder Container) für die Verarbeitung der Protokolle im Cluster bereitstellen. Mit dem **DriverOption**-Tag können Optionen für den Protokolltreiber angegeben werden.

## <a name="next-steps"></a>Nächste Schritte
Informationen zum Bereitstellen von Containern in einem Service Fabric-Cluster finden Sie unter [Bereitstellen eines Containers in Service Fabric](service-fabric-deploy-container.md).
