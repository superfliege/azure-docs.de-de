---
title: "Bereitstellen und Überwachen von Modulen für Azure IoT Edge | Microsoft-Dokumentation"
description: "Verwalten von Modulen, die auf Edge-Geräten ausgeführt werden"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 12/05/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: b507b9108dca2fd3aee4acdac231acad9c9154e8
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2017
---
# <a name="deploy-and-monitor-iot-edge-modules-at-scale---preview"></a>Bereitstellen und Überwachen von IoT Edge-Modulen im großen Maßstab – Vorschau

Mit Azure IoT Edge können Sie Analysen in die Edge-Ebene verlagern und erhalten eine Cloudschnittstelle, um Ihre IoT Edge-Geräte auch ohne physischen Zugriff darauf zu verwalten und zu überwachen. Die Möglichkeit zur Remoteverwaltung von Geräten wird immer wichtiger, weil Lösungen für das Internet der Dinge immer umfangreicher und komplexer werden. Azure IoT Edge wurde entwickelt, um Ihre Unternehmensziele unabhängig davon zu unterstützen, wie viele Geräte Sie hinzufügen.

Sie können einzelne Geräte verwalten und die dafür erforderlichen Modelle einzeln bereitstellen. Wenn Sie jedoch in großem Umfang Änderungen an den Geräten vornehmen möchten, können Sie eine **IoT Edge-Bereitstellung** erstellen. Bereitstellungen sind dynamische Prozesse, mit denen Sie mehrere Module gleichzeitig auf mehreren Geräten bereitstellen, Status und Integrität der Module nachverfolgen und bei Bedarf Änderungen vornehmen können. 

## <a name="identify-devices-using-tags"></a>Identifizieren von Geräten mithilfe von Tags

Bevor Sie eine Bereitstellung erstellen können, müssen Sie angeben können, welche Geräte Sie ansprechen möchten. Azure IoT Edge erkennt Geräte anhand von **Tags** im Gerätezwilling. Jedes Gerät kann mehrere Tags aufweisen, und Sie können diese auf jede beliebige Weise definieren, die für Ihre Lösung sinnvoll erscheint. Wenn Sie beispielsweise einen Campus mit intelligenten Gebäuden verwalten, könnten Sie für ein Gerät etwa die folgenden Tags hinzufügen:

```json
"tags":{
    "location":{
        "building": "20",
        "floor": "2"
    },
    "roomtype": "conference",
    "environment": "prod"
}
```

Weitere Informationen zu Gerätezwillingen und Tags finden Sie unter [Verstehen und Verwenden von Gerätezwillingen in IoT Hub][lnk-device-twin].

## <a name="create-a-deployment"></a>Erstellen einer Bereitstellung

1. Melden Sie sich beim [Azure-Portal][lnk-portal] an, und navigieren Sie zu Ihrem IoT Hub. 
1. Wählen Sie **IoT Edge (Vorschau)** aus.
1. Wählen Sie **IoT Edge-Bereitstellung hinzufügen** aus.

Zum Erstellen einer Bereitstellung müssen fünf Schritte ausgeführt werden. Diese werden in den folgenden Abschnitten exemplarisch beschrieben. 

### <a name="step-1-name-and-label"></a>Schritt 1: Name und Bezeichnung

1. Weisen Sie Ihrer Bereitstellung einen eindeutigen Namen zu. Verwenden Sie dabei weder Leerzeichen noch die folgenden ungültigen Zeichen: `& ^ [ ] { } \ | " < > /`.
1. Fügen Sie Bezeichnungen hinzu, um Ihre Bereitstellungen im Blick zu behalten. Bezeichnungen sind **Name-****Wert**-Paare, die Ihre Bereitstellung beschreiben. Beispiel: `HostPlatform, Linux` oder `Version, 3.0.1`.
1. Klicken Sie auf **Weiter**, um mit Schritt 2 fortzufahren. 

### <a name="step-2-add-modules-optional"></a>Schritt 2: Hinzufügen von Modulen (optional)

Es gibt zwei Arten von Modulen, die Sie einer Bereitstellung hinzufügen können. Das erste Modul basiert dabei auf einem Azure-Dienst wie Storage Account oder Stream Analytics. Das zweite dagegen entstammt Ihrem eigenen Code. Sie können einer Bereitstellung mehrere Module beider Typen hinzufügen. 

Wenn Sie eine Bereitstellung ohne Module erstellen, werden alle vorhandenen Module von den Geräten entfernt. 

>[!NOTE]
>Azure Machine Learning und Azure Functions unterstützen die automatisierte Azure-Dienstbereitstellung noch nicht. Verwenden Sie die benutzerdefinierte Modulbereitstellung, um diese Dienste manuell zu Ihrer Bereitstellung hinzuzufügen. 

So fügen Sie ein Modul aus einem Azure-Dienst hinzu:
1. Wählen Sie **Add Azure Service IoT Edge Module** (IoT Edge-Modul aus Azure-Dienst hinzufügen) aus.
1. Wählen Sie über die Dropdownmenüs die Azure-Dienstinstanzen aus, die Sie bereitstellen möchten.
1. Klicken Sie auf **Speichern**, um die Module zur Bereitstellung hinzuzufügen. 

So fügen Sie benutzerdefinierten Code als Modul oder aber ein Azure-Dienstmodul manuell hinzu:
1. Wählen Sie **Add Custom IoT Edge Module** (Benutzerdefiniertes IoT Edge-Modul hinzufügen) aus.
1. Geben Sie dem Modul einen **Namen**.
1. Geben Sie im Feld **Image** das Docker-Containerimage für dieses Modul an: `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
1. Geben Sie über die Dropdownmenüs unter **OS** (Betriebssystem) und **Architektur** die Eigenschaften des Docker-Containers an, der dieses Modul darstellt. 
1. Geben Sie unter **Create Options** (Erstellungsoptionen) alle erforderlichen Optionen an, die an den Container übergeben werden sollen. Weitere Informationen finden Sie unter [docker create][lnk-docker-create].
1. Wählen Sie über das Dropdownmenü eine **Neustartrichtlinie** aus. Sie können zwischen folgenden Optionen wählen: 
   * **Immer**: Das Modul wird stets neu gestartet, wenn es aus irgendeinem Grund beendet wird.
   * **Nie**: Das Modul wird niemals neu gestartet, wenn es aus irgendeinem Grund beendet wird.
   * **On-failed** (Bei Absturz): Das Modul wird neu gestartet, wenn es abgestürzt ist, aber nicht ordnungsgemäß heruntergefahren wurde. 
   * **On-unhealthy** (Bei Fehler): Das Modul wird neu gestartet, wenn es abstürzt oder einen Integritätsfehlerstatus zurückgibt. Die Implementierung der Integritätsstatusfunktion ist modulspezifisch unterschiedlich. 
1. Wählen Sie über das Dropdownmenü den **Startstatus** für das Modul aus. Sie können zwischen folgenden Optionen wählen:
   * **Wird ausgeführt**: Dies ist die Standardoption. Das Modul wird unmittelbar nach Bereitstellung gestartet.
   * **Beendet**: Nach der Bereitstellung verbleibt das Modul im Leerlauf, bis der Start durch Sie oder ein anderes Modul ausgelöst wird.
1. Klicken Sie auf **Edit Module Twin** (Modulzwilling bearbeiten), wenn Sie Tags oder gewünschte Eigenschaften zum Modul hinzufügen möchten. 
1. Klicken Sie auf **Speichern**, um Ihr Modul zur Bereitstellung hinzuzufügen. 

Nachdem Sie alle Module für eine Bereitstellung konfiguriert haben, klicken Sie auf **Weiter**, um mit Schritt 3 fortzufahren.

### <a name="step-3-specify-routes-optional"></a>Schritt 3: Angeben von Routen (optional)

Routen definieren, wie Module innerhalb einer Bereitstellung miteinander kommunizieren. Geben Sie sämtliche Routen für die Bereitstellung an, und klicken Sie dann auf **Weiter**, um mit Schritt 4 fortzufahren. 

### <a name="step-4-target-devices"></a>Schritt 4: Festlegen von Zielgeräten

Mit der Tageigenschaft Ihrer Geräte wählen Sie bestimmte Geräte als Ziele aus, die diese Bereitstellung erhalten sollen. 

Da mehrere Bereitstellungen dasselbe Gerät als Ziel verwenden können, sollten Sie für jede Bereitstellung eine Priorität festlegen. Tritt irgendwann ein Konflikt auf, dann „gewinnt“ die Bereitstellung mit der höchsten Priorität. Haben zwei Bereitstellungen dieselbe Priorität, dann wird jeweils diejenige verwendet, die später erstellt wurde. 

1. Geben Sie eine positive ganze Zahl als **Priorität** für die Bereitstellung ein.
1. Geben Sie unter **Zielbedingung** eine Bedingung ein, um festzulegen, auf welche Geräte diese Bereitstellung angewendet werden soll. Die Bedingung basiert auf den Tags des Gerätezwillings und muss dem Ausdrucksformat entsprechen. Beispiel: `tags.environment='test'`. 
1. Klicken Sie auf **Weiter**, um mit dem letzten Schritt fortzufahren.

### <a name="step-5-review-template"></a>Schritt 5: Überprüfen der Vorlage

Überprüfen Sie die Bereitstellungsinformationen, und klicken Sie dann auf **Senden**.

## <a name="monitor-a-deployment"></a>Überwachen einer Bereitstellung

So zeigen Sie ausführliche Informationen zu einer Bereitstellung an und überwachen die Geräte, die diese ausführen:

1. Melden Sie sich beim [Azure-Portal][lnk-portal] an, und navigieren Sie zu Ihrem IoT Hub. 
1. Wählen Sie **IoT Edge (Vorschau)** aus.
1. Wählen Sie **IoT Edge-Bereitstellungen** aus. 

   ![Anzeigen von IoT Edge-Bereitstellungen][1]

1. Prüfen Sie die Bereitstellungsliste. Für jede Bereitstellung können Sie die folgenden Details anzeigen:
   * **ID**: Name der Bereitstellung
   * **Zielbedingung**: Tag, das zur Definition von Zielgeräten verwendet wird
   * **Priorität**: Prioritätsnummer, die der Bereitstellung zugewiesen wurde
   * **IoT Edge-Agent-Status**: Anzahl der Geräte, die die Bereitstellung empfangen, und deren Integritätsstatus 
   * **Unhealthy Modules** (Fehlerbehaftete Module): Anzahl der Module in der Bereitstellung, die Fehler gemeldet haben 
   * **Erstellungszeit**: Zeitstempel der Bereitstellungserstellung. Dieser Zeitstempel wird zur Konfliktlösung verwendet, wenn zwei Bereitstellungen dieselbe Priorität haben. 
1. Wählen Sie die Bereitstellung aus, die Sie überwachen möchten.  
1. Überprüfen Sie die Bereitstellungsdetails. Auf Registerkarten können Sie bestimmte Details zu den Geräten anzeigen, die die Bereitstellung erhalten haben: 
   * **Ziel**: Edge-Geräte, die die Zielbedingung erfüllen 
   * **Angewendet**: Edge-Zielgeräte, für die keine andere Bereitstellung mit höherer Priorität vorhanden ist. Dies sind die Geräte, die die Bereitstellung tatsächlich erhalten. 
   * **Reporting Success** (Erfolgsmeldung): Edge-Geräte mit angewandter Bereitstellung, die dem Dienst zurückgemeldet haben, dass die Module erfolgreich bereitgestellt wurden 
   * **Reporting Failure** (Fehlermeldung): Edge-Geräte mit angewandter Bereitstellung, die dem Dienst zurückgemeldet haben, dass mindestens ein Modul nicht erfolgreich bereitgestellt werden konnte. Zur näheren Untersuchung des Fehlers müssen Sie mit diesen Geräten eine Remoteverbindung herstellen und die Protokolldateien anzeigen. 
   * **Reporting Unhealthy Modules** (Meldung fehlerbehafteter Module): Edge-Geräte mit angewandter Bereitstellung, die dem Dienst zurückgemeldet haben, dass mindestens ein Modul zwar erfolgreich bereitgestellt werden konnte, nun aber Fehler meldet 

## <a name="modify-a-deployment"></a>Ändern einer Bereitstellung

Wenn Sie Änderungen an einer Bereitstellung vornehmen, werden diese sofort auf alle Zielgeräte repliziert. 

Wenn Sie die Zielbedingung ändern, erfolgen die nachfolgend aufgeführten Anpassungen:
* Wenn ein Gerät die alte Zielbedingung nicht erfüllt, wohl aber die neue, und diese Bereitstellung die höchste Priorität für das Gerät hat, wird diese Bereitstellung auf das Gerät angewendet. 
* Wenn ein Gerät, auf dem diese Bereitstellung gegenwärtig ausgeführt wird, die Zielbedingung nicht mehr erfüllt, wird diese Bereitstellung deinstalliert, und die Bereitstellung mit der nächsthöheren Priorität wird angewendet. 
* Wenn ein Gerät, auf dem diese Bereitstellung gegenwärtig ausgeführt wird, diese Zielbedingung wie auch die Zielbedingungen aller anderen Bereitstellungen nicht erfüllt, erfolgt keine Änderung auf dem Gerät. Das Gerät führt die aktuellen Module im aktuellen Zustand weiter aus, wird aber nicht mehr als Teil dieser Bereitstellung verwaltet. Sobald es die Zielbedingung einer anderen Bereitstellung erfüllt, wird diese Bereitstellung deinstalliert, und die neue Bereitstellung wird angewendet. 

Gehen Sie wie folgt vor, um Änderungen an einer Bereitstellung vorzunehmen: 

1. Melden Sie sich beim [Azure-Portal][lnk-portal] an, und navigieren Sie zu Ihrem IoT Hub. 
1. Wählen Sie **IoT Edge (Vorschau)** aus.
1. Wählen Sie **IoT Edge-Bereitstellungen** aus. 

   ![Anzeigen von IoT Edge-Bereitstellungen][1]

1. Wählen Sie die Bereitstellung aus, die Sie ändern möchten. 
1. Nehmen Sie die gewünschten Änderungen an den folgenden Feldern vor: 
   * Zielbedingung 
   * Bezeichnungen 
   * Priority 
1. Wählen Sie **Speichern** aus.
1. Führen Sie die unter [Überwachen eine Bereitstellung][anchor-monitor] beschriebenen Schritte durch, um den Rollout der Änderungen zu beobachten. 

## <a name="delete-a-deployment"></a>Löschen einer Bereitstellung

Wenn Sie eine Bereitstellung löschen, übernehmen alle Geräte die Bereitstellung mit der jeweils nächsthöheren Priorität. Wenn Ihre Geräte die Zielbedingung keiner anderen Bereitstellung erfüllen, werden die Module beim Löschen der Bereitstellung nicht entfernt. 

1. Melden Sie sich beim [Azure-Portal][lnk-portal] an, und navigieren Sie zu Ihrem IoT Hub. 
1. Wählen Sie **IoT Edge (Vorschau)** aus.
1. Wählen Sie **IoT Edge-Bereitstellungen** aus. 

   ![Anzeigen von IoT Edge-Bereitstellungen][1]

1. Wählen Sie die zu löschende Bereitstellung durch Aktivieren des zugehörigen Kontrollkästchens aus. 
1. Klicken Sie auf **Löschen**.
1. Eine Meldung informiert Sie darüber, dass diese Aktion diese Bereitstellung löschen wird und der vorherige Status auf allen Geräten wiederhergestellt wird.  Dies bedeutet, dass eine Bereitstellung mit einer niedrigeren Priorität angewendet wird.  Wenn keine andere Zielbereitstellung für das Gerät vorhanden ist, werden keine Module entfernt. Auf Wunsch können Kunden eine Bereitstellung mit null Modulen erstellen und diese auf denselben Geräten bereitstellen. Wählen Sie **Ja**, um den Vorgang fortzusetzen. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über das [Bereitstellen von Modulen auf Edge-Geräten][lnk-deployments]

<!-- Images -->
[1]: ./media/how-to-deploy-monitor/view-deployments.png

<!-- Links -->
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-portal]: https://portal.azure.com
[lnk-docker-create]: https://docs.docker.com/engine/reference/commandline/create/
[lnk-deployments]: module-deployment-monitoring.md

<!-- Anchor links -->
[anchor-monitor]: #monitor-a-deployment
