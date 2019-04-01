---
title: Erstellen eines Sicherheitsmodulzwillings für ASC für IoT (Vorschauversion) | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie einen ASC für IoT-Modulzwilling zur Verwendung mit ASC für IoT erstellen.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: c782692e-1284-4c54-9d76-567bc13787cc
ms.service: ascforiot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 89802a638944ec220186e943d5fdc33524b2d4e9
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541745"
---
# <a name="quickstart-create-an-asc-for-iot-module-twin"></a>Schnellstart: Erstellen eines Modulzwillings für ASC für IoT

> [!IMPORTANT]
> ASC für IoT befindet sich derzeit in der öffentlichen Vorschauphase (Public Preview). Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

In dieser Schnellstartanleitung wird beschrieben, wie Sie individuelle ASC für IoT-Modulzwillinge für neue Geräte oder im Batchmodus Modulzwillinge für alle Geräte eines IoT-Hubs erstellen.  

## <a name="understanding-asc-for-iot-module-twins"></a>Grundlegendes zu ASC für IoT-Modulzwillingen 

Für in Azure erstellte IoT-Lösungen spielen Gerätezwillinge eine wichtige Rolle – sowohl bei der Geräteverwaltung als auch bei der Prozessautomatisierung. 

ASC für IoT ermöglicht die vollständige Integration in Ihre vorhandene Plattform für die IoT-Geräteverwaltung. Auf diese Weise können Sie Ihren Gerätesicherheitsstatus verwalten und die vorhandenen Funktionen für die Gerätesteuerung nutzen. Die ASC für IoT-Integration wird erreicht, indem der Mechanismus für IoT Hub-Zwillinge verwendet wird.  

Weitere Informationen zum allgemeinen Konzept in Bezug auf Modulzwillinge in Azure IoT Hub finden Sie unter [Verstehen und Verwenden von Modulzwillingen in IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-module-twins). 
 
Unter ASC für IoT wird der Mechanismus für Modulzwillinge genutzt und für jedes Gerät ein Sicherheitsmodulzwilling eingesetzt. Der Sicherheitsmodulzwilling enthält alle Informationen, die für die Gerätesicherheit Ihrer einzelnen Geräte jeweils relevant sind. 
 
Zur vollständigen Nutzung von ASC für IoT-Features müssen Sie diese Sicherheitsmodulzwillinge für jedes Gerät des Diensts erstellen, konfigurieren und verwenden.  

## <a name="create-asc-for-iot-module-twin"></a>Erstellen eines Modulzwillings für ASC für IoT 

ASC für IoT-Modulzwillinge können per Standardkonfiguration im Batchmodus oder einzeln mit spezifischen Konfigurationen für jedes Gerät erstellt werden. Verwenden Sie das [Batchskript für Module](https://aka.ms/iot-security-github-create-module), um die Batcherstellung für neue Geräte oder Geräte ohne Modulzwilling durchzuführen. 

>[!NOTE] 
> Beim Verwenden der Batchmethode werden vorhandene Modulzwillinge nicht überschrieben. Wenn NUR die Batchmethode verwendet wird, werden neue Modulzwillinge für Geräte erstellt, die nicht bereits über einen Modulzwilling verfügen. 

Informationen zum Modifizieren oder Ändern der Konfiguration eines vorhandenen Modulzwillings finden Sie unter [Ändern eines Modulzwillings für Sicherheit](how-to-modify-security-module-twin.md). 

Verwenden Sie die folgende Anleitung, um einen neuen ASC für IoT-Modulzwilling für ein Gerät zu erstellen: 

1. Suchen Sie auf Ihrem IoT-Hub nach dem Gerät, für das Sie einen Sicherheitsmodulzwilling erstellen möchten, und wählen Sie es aus. 
1. Geben Sie im Feld **Microsoft Identity Name** (Microsoft-Identitätsname) den Namen **ascforiotsecurity** ein.
1. Klicken Sie auf **Speichern**. 

## <a name="verify-creation-of-a-module-twin"></a>Überprüfen der Erstellung eines Modulzwillings

Überprüfen Sie wie folgt, ob für ein bestimmtes Gerät ein Sicherheitsmodulzwilling vorhanden ist:

1. Wählen Sie auf Ihrer Azure IoT Hub-Instanz im Menü **Explorer** die Option **IoT-Geräte**.    
1. Geben Sie die Geräte-ID ein, oder wählen Sie im Feld **Gerät abfragen** eine Option aus, und klicken Sie auf **Geräte abfragen**. 
    ![Geräte abfragen](./media/quickstart/verify-security-module-twin.png)
1. Wählen Sie das Gerät aus, oder doppelklicken Sie darauf, um die Seite „Gerätedetails“ zu öffnen. 
1. Wählen Sie das Menü **Modulidentitäten** aus, und bestätigen Sie das Vorhandensein des Moduls **ascforiotsecurity** und den Status **Verbunden** für den **Verbindungsstatus** in der Liste mit den Modulidentitäten, die dem Gerät zugeordnet sind. 
    ![Einem Gerät zugeordnete Module](./media/quickstart/verify-security-module-twin-2.png)


Weitere Informationen zum Anpassen der Eigenschaften von ASC für IoT-Modulzwillingen finden Sie unter [Agent-Konfiguration](concept-agent-configuration.md).

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem nächsten Artikel fort, um sich über das Konfigurieren von benutzerdefinierten Warnungen zu informieren.

> [!div class="nextstepaction"]
> [Konfigurieren von benutzerdefinierten Warnungen](quickstart-create-custom-alerts.md)
