---
title: Verwalten von Rollen in Azure-Clouddiensten mit Visual Studio | Microsoft Docs
description: "Erfahren Sie, wie Sie Rollen in Azure-Clouddiensten mit Visual Studio hinzufügen und entfernen."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 5ec9ae2e-8579-4e5d-999e-8ae05b629bd1
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 6ed857b857cf8c14506ca39725c214a7fea4fc95
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="managing-roles-in-azure-cloud-services-with-visual-studio"></a>Verwalten von Rollen in Azure-Clouddiensten mit Visual Studio
Nach dem Erstellen des Azure-Clouddiensts können Sie ihm neue Rollen hinzufügen oder vorhandene Rollen daraus entfernen. Darüber hinaus können Sie ein vorhandenes Projekt importieren und es in eine Rolle konvertieren. Sie können z. B. eine ASP.NET-Webanwendung importieren und sie als Webrolle festlegen.

## <a name="adding-a-role-to-an-azure-cloud-service"></a>Hinzufügen einer Rolle zu einem Azure-Clouddienst
Mit den folgenden Schritten können Sie eine Web- oder Workerrolle einem Azure-Clouddienstprojekt in Visual Studio hinzufügen.

1. Erstellen oder öffnen Sie ein Azure-Clouddienstprojekt in Visual Studio.

1. Erweitern Sie im **Projektmappen-Explorer** den Projektknoten.

1. Klicken Sie mit der rechten Maustaste auf den Knoten **Rollen**, um das Kontextmenü anzuzeigen. Wählen Sie im Kontextmenü die Option **Hinzufügen** und dann eine vorhandene Web- oder Workerrolle in der aktuellen Projektmappe aus, oder erstellen Sie ein Web- oder Workerrollenprojekt. Sie können auch ein entsprechendes Projekt auswählen, z.B. ein ASP.NET-Webanwendungsprojekt, und es einem Rollenprojekt zuordnen.

    ![Menüoptionen zum Hinzufügen einer Rolle zu einem Azure-Clouddienstprojekt](media/vs-azure-tools-cloud-service-project-managing-roles/add-role.png)

## <a name="removing-a-role-from-an-azure-cloud-service"></a>Entfernen einer Rolle aus einem Azure-Clouddienst
Mit den folgenden Schritten können Sie eine Web- oder Workerrolle aus einem Azure-Clouddienstprojekt in Visual Studio entfernen.

1. Erstellen oder öffnen Sie ein Azure-Clouddienstprojekt in Visual Studio.

1. Erweitern Sie im **Projektmappen-Explorer** den Projektknoten.

1. Erweitern Sie den Knoten **Rollen**.

1. Klicken Sie mit der rechten Maustaste auf den zu entfernenden Knoten, und wählen Sie im Kontextmenü die Option **Entfernen** aus. 

    ![Menüoptionen zum Hinzufügen einer Rolle zu einem Azure-Clouddienst](media/vs-azure-tools-cloud-service-project-managing-roles/remove-role.png)

## <a name="readding-a-role-to-an-azure-cloud-service-project"></a>Erneutes Hinzufügen einer Rolle zu einem Azure-Clouddienstprojekt
Wenn Sie eine Rolle aus Ihrem Clouddienstprojekt entfernen, sie aber später wieder zum Projekt hinzufügen möchten, werden nur die Rollendeklaration und grundlegende Attribute wie Endpunkte und Diagnoseinformationen hinzugefügt. Der Datei `ServiceDefinition.csdef` oder der Datei `ServiceConfiguration.cscfg` werden keine zusätzlichen Ressourcen oder Verweise hinzugefügt. Wenn Sie diese Informationen hinzufügen möchten, müssen Sie sie diesen Dateien manuell erneut hinzufügen.

Beispielsweise könnten Sie eine Webdienstrolle entfernen und diese Rolle später wieder der Projektmappe hinzufügen. In diesem Fall tritt ein Fehler auf. Fügen Sie zur Vermeidung des Fehlers das im folgenden XML-Code dargestellte `<LocalResources>`-Element wieder der Datei `ServiceDefinition.csdef` hinzu. Verwenden Sie den Namen der Webdienstrolle, die Sie dem Projekt wieder hinzugefügt haben, als Teil des Namensattributs für das **<LocalStorage>** -Element. In diesem Beispiel lautet der Name der Webdienstrolle **WCFServiceWebRole1**.

    <WebRole name="WCFServiceWebRole1">
        <Sites>
          <Site name="Web">
            <Bindings>
              <Binding name="Endpoint1" endpointName="Endpoint1" />
            </Bindings>
          </Site>
        </Sites>
        <Endpoints>
          <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
          <Import moduleName="Diagnostics" />
        </Imports>
       <LocalResources>
          <LocalStorage name="WCFServiceWebRole1.svclog" sizeInMB="1000" cleanOnRoleRecycle="false" />
       </LocalResources>
    </WebRole>

## <a name="next-steps"></a>Nächste Schritte
- [Konfigurieren der Rollen für einen Azure-Clouddienst mit Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md)
