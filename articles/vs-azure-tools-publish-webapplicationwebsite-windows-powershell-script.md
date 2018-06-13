---
title: Publish-WebApplicationWebSite (Windows PowerShell-Skript) | Microsoft Docs
description: Informationen zum Veröffentlichen eines Webprojekts auf einer Azure-Website. Dieses Skript erstellt die erforderlichen Ressourcen in Ihrem Azure-Abonnement, wenn sie noch nicht vorhanden sind.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 63cfaa2d-f04d-40dc-8677-345385c278d5
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 11/11/2016
ms.author: ghogen
ms.openlocfilehash: b540734f17ed11e4c438e1248ed9612fb892e89a
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2018
ms.locfileid: "31792513"
---
# <a name="publish-webapplicationwebsite-windows-powershell-script"></a>Publish-WebApplicationWebSite (Windows PowerShell-Skript)
## <a name="syntax"></a>Syntax
Veröffentlicht ein Webprojekt auf einer Azure-Website. Das Skript erstellt die erforderlichen Ressourcen in Ihrem Azure-Abonnement, wenn sie noch nicht vorhanden sind.

    Publish-WebApplicationWebSite
    –Configuration <configuration>
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose


## <a name="configuration"></a>Konfiguration
Der Pfad zur JSON-Konfigurationsdatei, in der die Details der Bereitstellung beschrieben sind.

| Parameter | Standardwert |
| --- | --- |
| Aliase |none |
| Erforderlich? |true |
| Position |benannt |
| Standardwert |none |
| Pipelineeingabe akzeptieren? |false |
| Platzhalterzeichen akzeptieren? |false |

## <a name="subscriptionname"></a>SubscriptionName
Der Name des Azure-Abonnements, in dem Sie die Website erstellen möchten.

| Parameter | Standardwert |
| --- | --- |
| Aliase |none |
| Erforderlich? |false |
| Position |benannt |
| Standardwert |none |
| Pipelineeingabe akzeptieren? |false |
| Platzhalterzeichen akzeptieren? |false |

## <a name="webdeploypackage"></a>WebDeployPackage
Der Pfad zum Webbereitstellungspaket für die Veröffentlichung auf der Website. Sie können dieses Paket in Visual Studio mithilfe des Assistenten "Web veröffentlichen" erstellen. Weitere Informationen finden Sie unter [Erste Schritte mit Azure-Clouddiensten und ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).

| Parameter | Standardwert |
| --- | --- |
| Aliase |none |
| Erforderlich? |false |
| Position |benannt |
| Standardwert |none |
| Pipelineeingabe akzeptieren? |false |
| Platzhalterzeichen akzeptieren? |false |

## <a name="databaseserverpassword"></a>DatabaseServerPassword
Der Benutzername und das Kennwort für die SQL-Datenbank in Azure.

| Parameter | Standardwert |
| --- | --- |
| Aliase |none |
| Erforderlich? |false |
| Position |benannt |
| Standardwert |none |
| Pipelineeingabe akzeptieren? |false |
| Platzhalterzeichen akzeptieren? |false |

## <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput
Falls "true", werden Nachrichten vom Skript in den Ausgabedatenstrom ausgegeben.

| Parameter | Standardwert |
| --- | --- |
| Aliase |none |
| Erforderlich? |false |
| Position |benannt |
| Standardwert |false |
| Pipelineeingabe akzeptieren? |false |
| Platzhalterzeichen akzeptieren? |false |

## <a name="remarks"></a>Anmerkungen
Eine ausführliche Erläuterung der Verwendung des Skripts zum Erstellen von Entwicklungs- und Testumgebungen finden Sie unter [Verwenden von Windows PowerShell-Skripts zum Veröffentlichen in Entwicklungs- und Testumgebungen](vs-azure-tools-publishing-using-powershell-scripts.md).

In der JSON-Konfigurationsdatei sind die Details angegeben, was bereitgestellt werden muss. Dazu zählen die Informationen, die Sie beim Erstellen des Projekts angegeben haben, z. B. den Namen und Benutzernamen für die Website. Sie umfassen auch die bereitzustellende Datenbank, sofern vorhanden. Der folgende Code zeigt ein Beispiel einer JSON-Konfigurationsdatei:

    {
        "environmentSettings": {
            "webSite": {
                "name": "WebApplication10554",
                "location": "West US"
            },
            "databases": [
                {
                    "connectionStringName": "DefaultConnection",
                    "databaseName": "WebApplication10554_db",
                    "serverName": "iss00brc88",
                    "user": "sqluser2",
                    "password": "",
                    "edition": "",
                    "size": "",
                    "collation": "",
                    "location": "West US"
                }
            ]
        }
    }

Sie können die JSON-Konfigurationsdatei bearbeiten, um den Umfang der Bereitstellung zu ändern. Der Abschnitt "Website" ist erforderlich, der Abschnitt "Datenbank" optional.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter [Publish-WebApplicationWebSite (Windows PowerShell-Skript)](vs-azure-tools-publish-webapplicationvm.md)

