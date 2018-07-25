---
title: Bereitstellen und Überwachen von Modulen für Azure IoT Edge | Microsoft-Dokumentation
description: Verwalten von Modulen, die auf Edge-Geräten ausgeführt werden
keywords: ''
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/07/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 0432a1782ab5e3aef471666e2f0a3ca18dead73e
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39001506"
---
# <a name="deploy-and-monitor-iot-edge-modules-at-scale-using-the-azure-portal"></a>Bedarfsgerechtes Bereitstellen und Überwachen von IoT Edge-Modulen mithilfe des Azure-Portals

[!INCLUDE [iot-edge-how-to-deploy-monitor-selector](../../includes/iot-edge-how-to-deploy-monitor-selector.md)]

Mit Azure IoT Edge können Sie Analysen in die Edge-Ebene verlagern und erhalten eine Cloudschnittstelle, um Ihre IoT Edge-Geräte auch ohne physischen Zugriff darauf zu verwalten und zu überwachen. Die Möglichkeit zur Remoteverwaltung von Geräten wird immer wichtiger, weil Lösungen für das Internet der Dinge immer umfangreicher und komplexer werden. Azure IoT Edge wurde entwickelt, um Ihre Unternehmensziele unabhängig davon zu unterstützen, wie viele Geräte Sie hinzufügen.

Sie können einzelne Geräte verwalten und die dafür erforderlichen Modelle einzeln bereitstellen. Wenn Sie jedoch Änderungen an Geräten in großem Umfang vornehmen möchten, können Sie eine **automatische IoT Edge-Bereitstellung** erstellen, das Teil der automatischen Geräteverwaltung in IoT Hub ist. Bereitstellungen sind dynamische Prozesse, mit denen Sie mehrere Module gleichzeitig auf mehreren Geräten bereitstellen, Status und Integrität der Module nachverfolgen und bei Bedarf Änderungen vornehmen können. 

## <a name="identify-devices-using-tags"></a>Identifizieren von Geräten mithilfe von Tags

Bevor Sie eine Bereitstellung erstellen können, müssen Sie angeben können, welche Geräte Sie ansprechen möchten. Azure IoT Edge erkennt Geräte anhand von **Tags** im Gerätezwilling. Jedes Gerät kann mehrere Tags aufweisen, und Sie können diese auf jede beliebige Weise definieren, die für Ihre Lösung sinnvoll erscheint. Wenn Sie beispielsweise einen Campus mit intelligenten Gebäuden verwalten, können Sie einem Gerät etwa die folgenden Tags hinzufügen:

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

1. Navigieren Sie im [Azure-Portal][lnk-portal] zu Ihrem IoT-Hub. 
1. Wählen Sie **IoT Edge** aus.
1. Wählen Sie **IoT Edge-Bereitstellung hinzufügen** aus.

Zum Erstellen einer Bereitstellung müssen fünf Schritte ausgeführt werden. Diese werden in den folgenden Abschnitten exemplarisch beschrieben. 

### <a name="step-1-name-and-label"></a>Schritt 1: Name und Bezeichnung

1. Geben Sie Ihrer Bereitstellung einen eindeutigen Namen, der bis zu 128 Kleinbuchstaben umfasst. Verwenden Sie dabei weder Leerzeichen noch die folgenden ungültigen Zeichen: `& ^ [ ] { } \ | " < > /`.
1. Fügen Sie Bezeichnungen hinzu, um Ihre Bereitstellungen im Blick zu behalten. Bezeichnungen sind **Name-****Wert**-Paare, die Ihre Bereitstellung beschreiben. Beispiel: `HostPlatform, Linux` oder `Version, 3.0.1`.
1. Klicken Sie auf **Weiter**, um mit Schritt 2 fortzufahren. 

### <a name="step-2-add-modules-optional"></a>Schritt 2: Hinzufügen von Modulen (optional)

Es gibt zwei Arten von Modulen, die Sie einer Bereitstellung hinzufügen können. Das erste Modul basiert dabei auf einem Azure-Dienst wie Storage Account oder Stream Analytics. Das zweite dagegen entstammt Ihrem eigenen Code. Sie können einer Bereitstellung mehrere Module beider Typen hinzufügen. 

Wenn Sie eine Bereitstellung ohne Module erstellen, werden alle aktuellen Module von den Geräten entfernt. 

>[!NOTE]
>Azure Machine Learning und Azure Functions unterstützen die automatisierte Azure-Dienstbereitstellung noch nicht. Verwenden Sie die benutzerdefinierte Modulbereitstellung, um diese Dienste manuell zu Ihrer Bereitstellung hinzuzufügen. 

Führen Sie die folgenden Schritte aus, um ein Modul aus Azure Stream Analytics hinzuzufügen:
1. Klicken Sie im Abschnitt **Bereitstellungsmodule** der Seite auf **Hinzufügen**.
1. Klicken Sie auf **Azure Stream Analytics-Modul**.
1. Wählen Sie im Dropdownmenü Ihr **Abonnement** aus.
1. Wählen Sie im Dropdownmenü Ihren **Edgeauftrag** aus.
1. Klicken Sie auf **Speichern**, um Ihr Modul zur Bereitstellung hinzuzufügen. 

So fügen Sie benutzerdefinierten Code als Modul oder aber ein Azure-Dienstmodul manuell hinzu:
1. Geben Sie im Abschnitt **Registrierungseinstellungen** der Seite die Namen und Anmeldeinformationen für private Containerregistrierungen an, die die Modulimages für diese Bereitstellung enthalten. Vom Edge-Agent wird der Fehler 500 zurückgegeben, wenn die Anmeldeinformationen einer Containerregistrierung für ein Docker-Image nicht gefunden wurden.
1. Klicken Sie im Abschnitt **Bereitstellungsmodule** der Seite auf **Hinzufügen**.
1. Klicken Sie auf **IoT Edge-Modul**.
1. Geben Sie dem Modul einen **Namen**.
1. Geben Sie im Feld **Image-URI** das Containerimage für Ihr Modul ein. 
1. Legen Sie alle **Optionen zur Containererstellung** fest, die an den Container übergeben werden sollen. Weitere Informationen finden Sie unter [docker create][lnk-docker-create].
1. Wählen Sie über das Dropdownmenü eine **Neustartrichtlinie** aus. Sie können zwischen folgenden Optionen wählen: 
   * **Immer**: Das Modul wird stets neu gestartet, wenn es aus irgendeinem Grund beendet wird.
   * **Nie**: Das Modul wird niemals neu gestartet, wenn es aus irgendeinem Grund beendet wird.
   * **On-failed** (Bei Absturz): Das Modul wird neu gestartet, wenn es abgestürzt ist, aber nicht ordnungsgemäß heruntergefahren wurde. 
   * **On-unhealthy** (Bei Fehler): Das Modul wird neu gestartet, wenn es abstürzt oder einen Integritätsfehlerstatus zurückgibt. Die Implementierung der Integritätsstatusfunktion ist modulspezifisch unterschiedlich. 
1. Wählen Sie über das Dropdownmenü den **Gewünschten Status** für das Modul aus. Sie können zwischen folgenden Optionen wählen:
   * **Wird ausgeführt**: Dies ist die Standardoption. Das Modul wird unmittelbar nach Bereitstellung gestartet.
   * **Beendet**: Nach der Bereitstellung verbleibt das Modul im Leerlauf, bis der Start durch Sie oder ein anderes Modul ausgelöst wird.
1. Wählen Sie **Aktivieren** aus, wenn Sie dem Modulzwilling Tags oder gewünschte Eigenschaften hinzufügen möchten. 
1. Geben Sie **Umgebungsvariablen** für dieses Modul ein. Umgebungsvariablen stellen zusätzliche Informationen für ein Modul bereit und erleichtern so den Konfigurationsprozess.
1. Klicken Sie auf **Speichern**, um Ihr Modul zur Bereitstellung hinzuzufügen. 

Nachdem Sie alle Module für eine Bereitstellung konfiguriert haben, klicken Sie auf **Weiter**, um mit Schritt 3 fortzufahren.

### <a name="step-3-specify-routes-optional"></a>Schritt 3: Angeben von Routen (optional)

Routen definieren, wie Module innerhalb einer Bereitstellung miteinander kommunizieren. Standardmäßig erhalten Sie vom Assistenten eine Route mit dem Namen **route**, die als **FROM /* INTO $upstream** definiert ist. Dies bedeutet, dass alle Nachrichten, die von Modulen ausgegeben werden, an Ihren IoT Hub gesendet werden.  

Fügen Sie die Routen mit Informationen aus dem Abschnitt [Deklarieren von Routen](module-composition.md#declare-routes) hinzu, oder aktualisieren Sie diese, und wählen Sie dann **Weiter** aus, um mit dem Abschnitt zur Überprüfung fortzufahren.


### <a name="step-4-target-devices"></a>Schritt 4: Festlegen von Zielgeräten

Mit der Tageigenschaft Ihrer Geräte wählen Sie bestimmte Geräte als Ziele aus, die diese Bereitstellung erhalten sollen. 

Da mehrere Bereitstellungen dasselbe Gerät als Ziel verwenden können, sollten Sie für jede Bereitstellung eine Priorität festlegen. Tritt irgendwann ein Konflikt auf, dann „gewinnt“ die Bereitstellung mit der höchsten Priorität (höhere Werte deuten auf eine höhere Priorität hin). Haben zwei Bereitstellungen dieselbe Priorität, dann wird jeweils diejenige verwendet, die später erstellt wurde. 

1. Geben Sie eine positive ganze Zahl als **Priorität** für die Bereitstellung ein. Wenn mindestens zwei Bereitstellungen auf dasselbe Gerät ausgerichtet sind, wird die Bereitstellung mit dem höchsten numerischen Wert für die Priorität angewendet.
1. Geben Sie unter **Zielbedingung** eine Bedingung ein, um festzulegen, auf welche Geräte diese Bereitstellung angewendet werden soll. Die Bedingung basiert auf den Gerätezwillingstags oder auf den gewünschten Eigenschaften des Gerätezwillings und muss dem Ausdrucksformat entsprechen. Beispiel: `tags.environment='test'` oder `properties.desired.devicemodel='4000x'`. 
1. Klicken Sie auf **Weiter**, um mit dem letzten Schritt fortzufahren.

### <a name="step-5-review-template"></a>Schritt 5: Überprüfen der Vorlage

Überprüfen Sie die Bereitstellungsinformationen, und klicken Sie dann auf **Senden**.

## <a name="monitor-a-deployment"></a>Überwachen einer Bereitstellung

So zeigen Sie ausführliche Informationen zu einer Bereitstellung an und überwachen die Geräte, die diese ausführen:

1. Melden Sie sich beim [Azure-Portal][lnk-portal] an, und navigieren Sie zu Ihrem IoT Hub. 
1. Wählen Sie **IoT Edge** aus.
1. Wählen Sie **IoT Edge-Bereitstellungen** aus. 

   ![Anzeigen von IoT Edge-Bereitstellungen][1]

1. Prüfen Sie die Bereitstellungsliste. Für jede Bereitstellung können Sie die folgenden Details anzeigen:
   * **ID**: Name der Bereitstellung
   * **Zielbedingung**: Tag, das zur Definition von Zielgeräten verwendet wird
   * **Priorität**: Prioritätsnummer, die der Bereitstellung zugewiesen wurde
   * Mit **System metrics** - **Targeted** (Systemmetriken – Ziel) wird die Anzahl von Gerätezwillingen in IoT Hub angegeben, die die Zielbedingung erfüllen, und **Angewendet** gibt die Anzahl von Geräten an, auf deren Gerätezwillinge in IoT Hub die Bereitstellungsinhalte angewendet wurde. 
   * **Gerätemetriken**: Die Anzahl von Edge-Geräten in der Bereitstellung, für die von der IoT Edge-Clientruntime eine Erfolgs- oder Fehlermeldung ausgegeben wurde.
   * **Erstellungszeit**: Zeitstempel der Bereitstellungserstellung. Dieser Zeitstempel wird zur Konfliktlösung verwendet, wenn zwei Bereitstellungen dieselbe Priorität haben. 
2. Wählen Sie die Bereitstellung aus, die Sie überwachen möchten.  
3. Überprüfen Sie die Bereitstellungsdetails. Sie können über Registerkarten die Details der Bereitstellung überprüfen.

## <a name="modify-a-deployment"></a>Ändern einer Bereitstellung

Wenn Sie Änderungen an einer Bereitstellung vornehmen, werden diese sofort auf alle Zielgeräte repliziert. 

Wenn Sie die Zielbedingung ändern, erfolgen die nachfolgend aufgeführten Anpassungen:
* Wenn ein Gerät die alte Zielbedingung nicht erfüllt, wohl aber die neue, und diese Bereitstellung die höchste Priorität für das Gerät hat, wird diese Bereitstellung auf das Gerät angewendet. 
* Wenn ein Gerät, auf dem diese Bereitstellung gegenwärtig ausgeführt wird, die Zielbedingung nicht mehr erfüllt, wird diese Bereitstellung deinstalliert, und die Bereitstellung mit der nächsthöheren Priorität wird angewendet. 
* Wenn ein Gerät, auf dem diese Bereitstellung gegenwärtig ausgeführt wird, diese Zielbedingung wie auch die Zielbedingungen aller anderen Bereitstellungen nicht erfüllt, erfolgt keine Änderung auf dem Gerät. Das Gerät führt die aktuellen Module im aktuellen Zustand weiter aus, wird aber nicht mehr als Teil dieser Bereitstellung verwaltet. Sobald es die Zielbedingung einer anderen Bereitstellung erfüllt, wird diese Bereitstellung deinstalliert, und die neue Bereitstellung wird angewendet. 

Gehen Sie wie folgt vor, um Änderungen an einer Bereitstellung vorzunehmen: 

1. Melden Sie sich beim [Azure-Portal][lnk-portal] an, und navigieren Sie zu Ihrem IoT Hub. 
1. Wählen Sie **IoT Edge** aus.
1. Wählen Sie **IoT Edge-Bereitstellungen** aus. 

   ![Anzeigen von IoT Edge-Bereitstellungen][1]

1. Wählen Sie die Bereitstellung aus, die Sie ändern möchten. 
1. Nehmen Sie die gewünschten Änderungen an den folgenden Feldern vor: 
   * Zielbedingung 
   * Bezeichnungen 
   * Priorität 
1. Wählen Sie **Speichern**aus.
1. Führen Sie die unter [Überwachen eine Bereitstellung][anchor-monitor] beschriebenen Schritte durch, um den Rollout der Änderungen zu beobachten. 

## <a name="delete-a-deployment"></a>Löschen einer Bereitstellung

Wenn Sie eine Bereitstellung löschen, übernehmen alle Geräte die Bereitstellung mit der jeweils nächsthöheren Priorität. Wenn Ihre Geräte die Zielbedingung keiner anderen Bereitstellung erfüllen, werden die Module beim Löschen der Bereitstellung nicht entfernt. 

1. Melden Sie sich beim [Azure-Portal][lnk-portal] an, und navigieren Sie zu Ihrem IoT Hub. 
1. Wählen Sie **IoT Edge** aus.
1. Wählen Sie **IoT Edge-Bereitstellungen** aus. 

   ![Anzeigen von IoT Edge-Bereitstellungen][1]

1. Wählen Sie die zu löschende Bereitstellung durch Aktivieren des zugehörigen Kontrollkästchens aus. 
1. Klicken Sie auf **Löschen**.
1. Eine Meldung informiert Sie darüber, dass diese Aktion diese Bereitstellung löschen wird und der vorherige Status auf allen Geräten wiederhergestellt wird.  Dies bedeutet, dass eine Bereitstellung mit einer niedrigeren Priorität angewendet wird.  Wenn keine andere Zielbereitstellung für das Gerät vorhanden ist, werden keine Module entfernt. Wenn Sie alle Module vom Gerät entfernen möchten, erstellen Sie eine Bereitstellung ohne Module, und stellen Sie sie auf den gleichen Geräten bereit. Wählen Sie **Yes** (Ja), um fortzufahren. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über das [Bereitstellen von Modulen auf Edge-Geräten][lnk-deployments]

<!-- Images -->
[1]: ./media/how-to-deploy-monitor/iot-edge-deployments.png

<!-- Links -->
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-portal]: https://portal.azure.com
[lnk-docker-create]: https://docs.docker.com/engine/reference/commandline/create/
[lnk-deployments]: module-deployment-monitoring.md

<!-- Anchor links -->
[anchor-monitor]: #monitor-a-deployment
