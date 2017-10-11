---
title: Azure Deutschland IoT-Dienste | Microsoft Docs
description: "Bietet einen Ausgangspunkt für die IoT Suite für Azure-Deutschland"
services: germany
cloud: na
documentationcenter: na
author: gitralf
manager: rainerst
ms.assetid: na
ms.service: germany
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/13/2017
ms.author: ralfwi
ms.openlocfilehash: d1c98667f35db4873202ff5e32361cc872aa2d47
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-germany-iot-services"></a>Azure Deutschland IoT-Diensten.

##<a name="iot-suite"></a>IoT Suite
Alle erforderlichen Dienste für Azure IoT Suite stehen in Azure Deutschland. 

### <a name="variations"></a>Variationen
Die Homepage für Azure IoT Suite in Azure Deutschland unterscheidet sich von der Seite in der globalen Azure. Verwendung [http://www.azureiotsuite.de](http://www.azureiotsuite.de) und den Anweisungen folgen. 

## <a name="preconfigured-solutions"></a>Vorkonfigurierter Lösungen
Möglicherweise möchten mit einem der folgenden vorkonfigurierten Lösungen zu starten. 

### <a name="remote-monitoring"></a>Remote-Überwachung
Die IoT Suite remote überwachen vorkonfigurierte Lösung ist eine Implementierung von einer End-to-End-überwachungslösung für mehrere Computer an Remotestandorten ausgeführt wird. Die Lösung kombiniert wichtige Azure-Dienste, um eine generische Implementierung von Business-Szenario bereitzustellen. Sie können die Projektmappe als Ausgangspunkt für Ihre eigene Implementierung verwenden und anpassen, um bestimmte geschäftsanforderungen zu erfüllen.

### <a name="predictive-maintenance"></a>Vorbeugende Wartung
Die IoT Suite vorbeugende Wartung vorkonfigurierte Lösung ist eine End-to-End-Lösung für ein Geschäftsszenario, das den Punkt vorhersagt, an dem ein Fehler unwahrscheinlich ist. Sie können diese vorkonfigurierte Lösung für Aktivitäten wie das Optimieren der Wartung proaktiv verwenden. Die Lösung kombiniert die zentrale Azure IoT Suite-Dienste, z. B. Azure IoT Hub und Stream Analytics Machine Learning-Arbeitsbereich. Dieser Arbeitsbereich enthält ein Modell, basierend auf Öffentliche Stichprobendataset, um die verbleibenden nützlich Lebensdauer (RUL) der Motor Luftfahrzeug vorherzusagen. Die Lösung implementiert IoT-Geschäftsszenario vollständig als Ausgangspunkt für die Sie zum Planen und Implementieren einer Lösung, die Ihren geschäftsanforderungen zu erfüllen.


## <a name="deploying-the-preconfigured-solutions"></a>Bereitstellen der vorkonfigurierten Lösungen

Beide Lösungen können auf zwei Arten, Website oder über PowerShell bereitgestellt werden.

### <a name="deploy-via-website"></a>Bereitstellen über website

Befolgen Sie die Anweisungen in der [für die vorkonfigurierten Lösungen Tutorial](../iot-suite/iot-suite-getstarted-preconfigured-solutions.md) mithilfe der zuvor erwähnten Startseite.

### <a name="deploy-via-powershell"></a>Bereitstellen über PowerShell

Es ist eine Vollversion (mit Vorlagen für Azure-Ressourcen-Manager und Visual Studio) für die *Remoteüberwachung* Lösung. Laden Sie von der [Azure-IoT-Remote-Überwachung-Repository auf GitHub](https://github.com/Azure/azure-iot-remote-monitoring). Die PowerShell-Bereitstellung ist für andere Umgebungen, z. B. Azure Deutschland bereit. Geben Sie die *Umgebung* Parameter "AzureGermanCloud", sieht es etwa wie folgt:

    build.cmd cloud debug AzureGermanCloud

Bing Maps ist zurzeit nicht verfügbar in Deutschland von Azure und aus diesem Grund kann nicht abonniert werden automatisch. Sie können dieses Problem beheben, durch Abonnieren des Diensts in den globalen Azure, und verwenden den Dienst vorhanden. 

> [!NOTE]
> Bei Verwendung von Bing Maps, die es wird beschrieben, wie hier lassen Sie die Azure-Deutschland-Umgebung.

So sieht wie Sie dies durchführen:

1. Erstellen Sie einen Bing Maps-API im globalen Azure-Portal, indem Sie auf **+ neu**, Suchen nach einem **Bing Maps-API für Unternehmen**, und den Anweisungen folgen.
2. Abrufen der Bing Maps-API für den Enterprise-Schlüssel vom globalen Azure-Portal: 
    1. Navigieren Sie zu der Ressourcengruppe, in denen die Bing Maps-API für Unternehmen in den globalen Azure-Portal ist.
    2. Klicken Sie auf **alle Einstellungen** > **Schlüsselverwaltung**. 
    3. Sie sehen, dass zwei Schlüssel: MasterKey und QueryKey. Kopieren Sie den Wert für QueryKey.
3. Ruft den aktuellen Code aus der [Azure-IoT-Remote-Überwachung-Repository auf GitHub](https://github.com/Azure/azure-iot-remote-monitoring).
4. Führen Sie eine Cloud-Bereitstellung in Ihrer Umgebung durch die Bereitstellung über die Befehlszeile Anweisungen in der `/docs/` Repository-Ordner. 
5. Nachdem Sie die Bereitstellung ausgeführt haben, suchen Sie im Stammordner für die **. user.config** Datei während der Bereitstellung erstellt. Öffnen Sie diese Datei in einem Text-Editor. 
6. Ändern Sie die folgende Zeile ein, um den Wert ein, dem Sie für QueryKey kopiert haben:`<setting name="MapApiQueryKey" value="" />`
7. Stellen Sie die Lösung, indem Sie wiederholen Schritt 4.
 


## <a name="next-steps"></a>Nächste Schritte
Zusätzliche Informationen und Updates, abonnieren Sie die [Deutschland Azure Blog](https://blogs.msdn.microsoft.com/azuregermany/).
