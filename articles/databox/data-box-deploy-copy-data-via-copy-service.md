---
title: Kopieren von Daten in Ihre Microsoft Azure Data Box über SMB | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Daten über den Datenkopierdienst auf Ihre Azure Data Box kopieren.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 01/10/2019
ms.author: alkohli
ms.openlocfilehash: a71635abd036bb89546dd3421af97cd9b88f4327
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54439945"
---
# <a name="tutorial-use-data-copy-service-to-directly-ingest-data-into-azure-data-box-preview"></a>Tutorial: Direktes Erfassen von Daten in Azure Data Box (Vorschauversion) mithilfe des Datenkopierdiensts

In diesem Tutorial erfahren Sie, wie Sie Daten mithilfe des Datenkopierdiensts ohne Zwischenhost erfassen. Der Datenkopierdienst wird lokal auf der Data Box ausgeführt, stellt eine SMB-Verbindung mit Ihrem NAS-Gerät her und kopiert Daten auf die Data Box.

Der Datenkopierdienst eignet sich für Folgendes:

- NAS-Umgebungen (Network Attached Storage), in denen die Zwischenhosts unter Umständen nicht verfügbar sind.
- Kleine Dateien, bei denen das Erfassen und Hochladen von Daten mehrere Wochen dauert. Dieser Dienst verkürzt die Erfassungs- und Uploadzeit erheblich.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Kopieren von Daten auf die Data Box
> * Vorbereiten der Data Box für den Versand

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie Folgendes sicher, bevor Sie beginnen:

1. Sie haben die Schritte unter [Tutorial: Einrichten der Azure Data Box](data-box-deploy-set-up.md) ausgeführt.
2. Sie haben Ihre Data Box erhalten, und die Bestellung wird im Portal mit dem Status **Übermittelt** angezeigt.
3. Sie verfügen über die Anmeldeinformationen des NAS-Quellgeräts, mit dem Sie eine Verbindung für den Datenkopiervorgang herstellen.
4. Sie verfügen über eine Verbindung mit einem Hochgeschwindigkeitsnetzwerk. Mindestens eine 10-GbE-Verbindung wird dringend empfohlen. Falls keine 10-GbE-Verbindung verfügbar ist, verwenden Sie eine 1-GbE-Datenverbindung, die Geschwindigkeit der Kopiervorgänge wird dadurch jedoch beeinträchtigt.

## <a name="copy-data-to-data-box"></a>Kopieren von Daten auf die Data Box

Nachdem Sie die Verbindung mit dem NAS-Gerät hergestellt haben, kopieren Sie im nächsten Schritt Ihre Daten. Bevor Sie mit dem Kopieren der Daten beginnen, sollten Sie folgende Aspekte beachten:

- Stellen Sie beim Kopieren der Daten sicher, dass für die Datengröße die Größenbeschränkungen eingehalten werden, die im Artikel zu den [Grenzwerten für Azure Storage und Data Box](data-box-limits.md) beschrieben sind.
- Falls von der Data Box hochgeladene Daten gleichzeitig von anderen Anwendungen außerhalb der Data Box hochgeladen werden, kann dies zu Fehlern bei Uploadaufträgen und zu Datenbeschädigungen führen.
- Wenn die Daten geändert werden, während sie gerade vom Datenkopierdienst gelesen werden, kommt es ggf. zu Fehlern oder Datenbeschädigungen.

Sie müssen einen Auftrag erstellen, um Daten mit dem Datenkopierdienst zu kopieren. Führen Sie die folgenden Schritte aus, um einen Auftrag zum Kopieren von Daten zu erstellen:

1. Navigieren Sie auf der lokalen Webbenutzeroberfläche Ihrer Data Box zu **Verwalten > Daten kopieren**.
2. Klicken Sie auf der Seite **Daten kopieren** auf **Erstellen**.

    ![Klicken auf **Erstellen** auf der Seite **Daten kopieren**](media/data-box-deploy-copy-data-via-copy-service/click-create.png)

3. Geben Sie im Dialogfeld **Auftrag konfigurieren und starten** Folgendes an:
    
    |Feld                          |Wert    |
    |-------------------------------|---------|
    |Auftragsname                       |Eindeutiger Name für den Auftrag (weniger als 230 Zeichen). Auftragsnamen dürfen keines der folgenden Zeichen enthalten: \<, \>, \|, \?, \*, \\, \:, \/ und \\\.         |
    |Quellspeicherort                |Geben Sie den SMB-Pfad der Datenquelle im Format `\\<ServerIPAddress>\<ShareName>` oder `\\<ServerName>\<ShareName>` an.        |
    |Benutzername                       |Benutzername für den Zugriff auf die Datenquelle.        |
    |Kennwort                       |Kennwort für den Zugriff auf die Datenquelle.           |
    |Zielspeicherkonto    |Wählen Sie in der Dropdownliste das Zielspeicherkonto aus, in das die Daten hochgeladen werden sollen.         |
    |Zieltyp       |Wählen Sie den Zielspeichertyp aus: Blockblob, Seitenblob oder Azure Files.        |
    |Zielcontainer/-freigabe    |Geben Sie den Namen des Containers oder der Freigabe für das Hochladen der Daten in Ihr Zielspeicherkonto an. Hierbei kann es sich um einen Freigabenamen oder um einen Containernamen handeln. Beispiel: `myshare` oder `mycontainer`. Die Eingabe kann auch das Format `sharename\directory_name` oder `containername\virtual_directory_name` (in der Cloud) haben.        |
    |Mit Muster übereinstimmende Dateien kopieren    | Geben Sie das gewünschte Dateinamenmuster ein. Dabei haben Sie zwei Optionen:<ul><li>**Verwenden von Platzhalterausdrücken:** In Platzhalterausdrücken werden nur `*` und `?` unterstützt. Beispiel: Der Ausdruck `*.vhd` entspricht allen Dateien mit der Erweiterung „.vhd“. Analog dazu entspricht `*.dl?` allen Dateien, die entweder die Erweiterung `.dl` oder die Erweiterung `.dll` besitzen. `*foo` entspricht allen Dateien, deren Name mit `foo` endet.<br>Platzhalterausdrücke können direkt in das Feld eingegeben werden. Der in das Feld eingegebene Wert wird standardmäßig als Platzhalterausdruck behandelt.</li><li>**Verwenden von regulären Ausdrücken:** POSIX-basierte reguläre Ausdrücke werden unterstützt. Der reguläre Ausdruck `.*\.vhd` entspricht beispielsweise allen Dateien mit der Erweiterung `.vhd`. Wenn Sie einen regulären Ausdruck verwenden möchten, geben Sie das Muster (`<pattern>`) direkt als `regex(<pattern>)` an. <li>Weitere Informationen zu regulären Ausdrücken finden Sie unter [Sprachelemente für reguläre Ausdrücke – Kurzübersicht](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference).</li><ul>|
    |Dateioptimierung              |Ist diese Option aktiviert, werden die Dateien bei der Erfassung gepackt. Das beschleunigt das Kopieren kleiner Dateien.        |
 
4. Klicken Sie auf **Start**. Die Eingaben werden überprüft. Ist die Überprüfung erfolgreich, wird ein Auftrag gestartet. Es dauert möglicherweise ein paar Minuten, bis der Auftrag gestartet wird.

    ![Starten eines Auftrags über das Dialogfeld „Auftrag konfigurieren und starten“](media/data-box-deploy-copy-data-via-copy-service/configure-and-start.png)

5. Ein Auftrag mit den angegebenen Einstellungen wird erstellt. Wenn Sie das Kontrollkästchen aktivieren, können Sie einen Auftrag anhalten, fortsetzen, abbrechen oder neu starten.

    ![Verwalten eines Auftrags über die Seite „Daten kopieren“](media/data-box-deploy-copy-data-via-copy-service/select-job.png)
    
    - Der Auftrag kann angehalten werden, sollte er die NAS-Ressourcen zu Spitzenzeiten beeinträchtigen.

        ![Anhalten eines Auftrags](media/data-box-deploy-copy-data-via-copy-service/pause-job.png)

        Sie können den Auftrag später fortsetzen, wenn die Auslastung geringer ist.

        ![Fortsetzen eines Auftrags](media/data-box-deploy-copy-data-via-copy-service/resume-job.png)

    - Ein Auftrag kann jederzeit abgebrochen werden.

        ![Abbrechen eines Auftrags](media/data-box-deploy-copy-data-via-copy-service/cancel-job.png) Beim Abbrechen eines Auftrags wird eine Bestätigung angefordert.

        ![Bestätigen eines Auftragsabbruchs](media/data-box-deploy-copy-data-via-copy-service/confirm-cancel-job.png)

        Wenn Sie einen Auftrag abbrechen, werden die bereits kopierten Daten nicht gelöscht. Setzen Sie das Gerät zurück, um die auf Ihre Data Box kopierten Daten zu löschen.

        ![Zurücksetzen des Geräts](media/data-box-deploy-copy-data-via-copy-service/reset-device.png)

        >[!NOTE]
        > Wenn Sie einen Auftrag abbrechen oder anhalten, werden große Dateien, die gerade kopiert wurden, möglicherweise unvollständig kopiert. Diese Dateien werden im gleichen Zustand in Azure hochgeladen. Vergewissern Sie sich, dass Ihre Dateien ordnungsgemäß kopiert wurden, wenn Sie einen Auftrag abbrechen oder anhalten. Sehen Sie sich zur Überprüfung der Dateien die SMB-Freigaben an, oder laden Sie die BOM-Datei herunter.

    - Sie können einen Auftrag neu starten, wenn dieser aufgrund eines plötzlichen vorübergehenden Fehlers (etwa aufgrund einer Netzwerkstörung) nicht erfolgreich war. Aufträge, die einen Endzustand erreicht haben (also beispielsweise erfolgreich oder mit Fehlern abgeschlossen wurden), können nicht erneut gestartet werden. Die Fehler können auf Probleme mit dem Dateinamen oder mit der Dateigröße zurückzuführen sein. Solche Fehler werden zwar protokolliert, der Auftrag kann jedoch nicht erneut gestartet werden, nachdem er abgeschlossen wurde.

        ![Neustarten eines nicht erfolgreichen Auftrags](media/data-box-deploy-copy-data-via-copy-service/restart-failed-job.png)

        Sollte ein Fehler aufgetreten sein und der Auftrag nicht erneut gestartet werden können, laden Sie die Fehlerprotokolle herunter, und sehen Sie sich den Fehler in den Protokolldateien an. Nachdem Sie das Problem behoben haben, können Sie einen neuen Auftrag erstellen, um die Dateien zu kopieren. Alternativ können Sie die [Dateien über SMB kopieren](data-box-deploy-copy-data.md).
    
    - In diesem Release können Sie keine Aufträge löschen.
    
    - Sie können eine unbegrenzte Anzahl von Aufträgen erstellen. Es können jedoch immer nur maximal zehn Aufträge parallel ausgeführt werden.
    - Bei aktivierter Dateioptimierung werden die kleinen Dateien im Rahmen der Erfassung gepackt, um die Kopierleistung zu verbessern. In diesen Fällen sehen Sie eine gepackte Datei (mit einem GUID als Namen), wie auf dem folgenden Screenshot gezeigt:

        ![Beispiel einer gepackten Datei](media/data-box-deploy-copy-data-via-copy-service/packed-file-on-ingest.png)

6. Während der Auftragsausführung gilt für die Seite **Daten kopieren** Folgendes:

    - In der Spalte **Status** wird der Status des Kopierauftrags angezeigt. Mögliche Statuswerte:
        - **Wird ausgeführt**
        - **Fehler**
        - **Erfolgreich**
        - **Wird angehalten**
        - **Angehalten**
        - **Wird abgebrochen**
        - **Abgebrochen**
        - **Mit Fehlern abgeschlossen**
    - In der Spalte **Dateien** werden Gesamtanzahl und -größe der kopierten Dateien angezeigt.
    - In der Spalte **Verarbeitet** werden Anzahl und Größe der verarbeiteten Dateien angezeigt.
    - Klicken Sie in der Spalte **Details** auf **Anzeigen**, um die Auftragsdetails anzuzeigen.
    - Sollten während des Kopiervorgangs Fehler auftreten (siehe Spalte **Anzahl von Fehlern**), navigieren Sie zur Spalte **Fehlerprotokoll**, und laden Sie zur Problembehandlung die Fehlerprotokolle herunter.

Warten Sie, bis die Kopieraufträge abgeschlossen sind. Da einige Fehler nur auf der Seite **Verbindung herstellen und Daten kopieren** protokolliert werden, vergewissern Sie sich, dass die Kopieraufträge fehlerfrei abgeschlossen wurden, bevor Sie mit dem nächsten Schritt fortfahren.

![Keine Fehler auf der Seite **Verbindung herstellen und Daten kopieren**](media/data-box-deploy-copy-data-via-copy-service/verify-no-errors-on-connect-and-copy.png)

Um die Datenintegrität zu gewährleisten, wird inline eine Prüfsumme berechnet, während die Daten kopiert werden. Überprüfen Sie nach Abschluss des Kopiervorgangs den belegten Speicherplatz und den freien Speicherplatz auf Ihrem Gerät.
    
![Überprüfen des freien und belegten Speicherplatzes im Dashboard](media/data-box-deploy-copy-data-via-copy-service/verify-used-space-dashboard.png)

Nach Abschluss des Kopierauftrags können Sie zu **Für Versand vorbereiten** wechseln.

>[!NOTE]
> „Für Versand vorbereiten“ kann nicht ausgeführt werden, solange noch Kopieraufträge ausgeführt werden.

## <a name="prepare-to-ship"></a>Vorbereiten des Versands

[!INCLUDE [data-box-prepare-to-ship](../../includes/data-box-prepare-to-ship.md)]


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Informationen zu Azure Data Box-Themen erhalten, darunter die folgenden:

> [!div class="checklist"]
> * Kopieren von Daten auf die Data Box
> * Vorbereiten der Data Box für den Versand

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihre Data Box zurück an Microsoft senden.

> [!div class="nextstepaction"]
> [Zurücksenden der Azure Data Box an Microsoft](./data-box-deploy-picked-up.md)

