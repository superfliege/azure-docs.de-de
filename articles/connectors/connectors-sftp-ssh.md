---
title: Herstellen einer Verbindung mit einem SFTP-Konto über Azure Logic Apps | Microsoft-Dokumentation
description: Automatisieren von Aufgaben und Workflows, die Dateien auf einem SFTP-Server mithilfe von Azure Logic Apps überwachen, erstellen, verwalten, senden und empfangen
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
tags: connectors
ms.date: 09/24/2018
ms.openlocfilehash: cfee4f06479d2504b88f4a5d5a0a2417154e3b03
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47064499"
---
# <a name="monitor-create-and-manage-sftp-files-by-using-azure-logic-apps-and-sftp-ssh-connector"></a>Überwachen, Erstellen und Verwalten von SFTP-Dateien mithilfe von Azure Logic Apps und dem SFTP-SSH-Connector

Mit Azure Logic Apps und dem SFTP-SSH-Connector können Sie automatisierte Aufgaben und Workflows erstellen, die Dateien über Ihr Konto auf einem [SFTP](https://www.ssh.com/ssh/sftp/)-Server überwachen, erstellen, senden und empfangen sowie weitere Aktionen ausführen, wie z.B. folgende:

* Sie können die Vorgänge überwachen, wenn Dateien hinzugefügt oder geändert werden.
* Sie können Dateien abrufen, erstellen, kopieren, umbenennen, aktualisieren, auflisten und löschen.
* Sie können Ordner erstellen.
* Sie können Dateiinhalte und Metadaten abrufen.
* Sie können Archive in Ordner extrahieren.

Sie können Trigger verwenden, die Antworten von Ihrem SFTP-Server erhalten und die Ausgabe für andere Aktionen verfügbar machen. Sie können Aktionen in Ihren Logik-Apps verwenden, um Aufgaben mit Dateien auf Ihrem SFTP-Server auszuführen. Sie können die Ausgaben von SFTP-Aktionen auch in anderen Aktionen verwenden. Wenn Sie z.B. regelmäßig Dateien von Ihrem SFTP-Server abrufen, können Sie mithilfe des Office 365 Outlook-Connectors oder mithilfe des Outlook.com-Connectors E-Mails zu diesen Dateien und ihren Inhalten senden.
Falls Sie noch nicht mit Logik-Apps vertraut sind, finden Sie weitere Informationen unter [Was ist Azure Logic Apps?](../logic-apps/logic-apps-overview.md).

## <a name="sftp-ssh-versus-sftp"></a>SFTP-SSH im Vergleich zu SFTP

Hier sind einige der Hauptunterschiede zwischen dem SFTP-SSH-Connector und dem [SFTP](../connectors/connectors-create-api-sftp.md)-Connector aufgelistet. Der SFTP-SSH-Connector bietet folgende Funktionen:

* Er verwendet die <a href="https://github.com/sshnet/SSH.NET" target="_blank">**SSH.NET**</a>-Bibliothek, die eine Open Source-SSH-Bibliothek (Secure Shell) für .NET ist.

* Er stellt Unterstützung für große Dateien (bis zu **1 GB**) bereit. Der Connector kann Dateien bis zu 1 GB lesen oder schreiben.

* Er stellt die Aktion **Ordner erstellen** bereit, wodurch ein Ordner unter dem angegebenen Pfad auf dem SFTP-Server erstellt wird.

* Er stellt die Aktion **Datei umbenennen** bereit, wodurch eine Datei auf dem SFTP-Server umbenannt wird.

* Er speichert die Verbindung zum SFTP-Server, wodurch die Leistung verbessert und die Anzahl der Verbindungsversuche auf dem Server reduziert wird. 

  Sie können die für das Zwischenspeichern der Verbindung benötigte Dauer steuern, indem Sie die Eigenschaft <a href="http://man.openbsd.org/sshd_config#ClientAliveInterval" target="_blank">**ClientAliveInterval**</a> auf Ihrem SFTP-Server einrichten. 

## <a name="how-trigger-polling-works"></a>Funktionsweise des Triggerabrufs

Die SFTP-SSH-Trigger rufen das SFTP-Dateisystem ab und suchen nach jeder Datei, die seit des letzten Abrufs geändert wurde. Mit einigen Tools können Sie den Zeitstempel beibehalten, wenn die Dateien geändert werden. In diesen Fällen müssen Sie dieses Feature jedoch deaktivieren, damit der Trigger funktioniert. Hier sind einige gängige Einstellungen:

| SFTP-Client | Aktion | 
|-------------|--------| 
| Winscp | Optionen > Einstellungen... > Übertragen > Bearbeiten... > Preserve timestamp (Zeitstempel beibehalten) > Deaktivieren |
| FileZilla | Übertragen > Preserve timestamps of transferred files (Zeitstempel übertragener Dateien beibehalten) > Deaktivieren | 
||| 

Wenn ein Trigger eine neue Datei findet, überprüft er, ob die neue Datei vollständig ist und nicht nur teilweise geschrieben wurde. Zum Beispiel werden bei einer Datei möglicherweise gerade Änderungen vorgenommen, wenn der Trigger den Dateiserver überprüft. Um zu vermeiden, dass eine nur zum Teil geschriebene Datei zurückgegeben wird, vermerkt der Trigger den Zeitstempel für die Datei mit den kürzlichen Änderungen, gibt diese Datei jedoch nicht sofort zurück. Der Trigger gibt die Datei erst dann zurück, wenn der Server erneut abgerufen wird. Dieses Verhalten kann in manchen Fällen zu Verzögerungen führen, die bis zu zweimal länger als das Abrufintervall des Triggers sein können. 

Wenn der Trigger Dateiinhalte anfordert, ruft er keine Dateien ab, die größer als 50 MB sind. Befolgen Sie das folgende Muster, um Dateien aufzunehmen, die größer als 50 MB sind:

* Verwenden Sie einen Trigger, der Dateieigenschaften zurückgibt, z.B. **Beim Hinzufügen oder Ändern einer Datei (nur Eigenschaften)**. 
* Der Trigger sollte von einer Aktion gefolgt werden, die die gesamte Datei liest, z.B. **Dateiinhalt anhand des Pfads abrufen**.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich <a href="https://azure.microsoft.com/free/" target="_blank">für ein kostenloses Azure-Konto registrieren</a>. 

* Ihre SFTP-Hostserveradresse und Anmeldeinformationen, die Ihre Logik-App zur Erstellung einer Verbindung mit Ihrem SFTP-Konto sowie zum Zugriff auf das Konto autorisieren.

  Kopieren Sie den vollständigen Inhalt für den privaten SSH-Schlüssel in die Eigenschaft **SSH private key**, befolgen Sie dazu das mehrzeilige Format. 
  Die folgenden Beispielschritte zeigen Ihnen, wie Sie den privaten SSH-Schlüssel über Notepad.exe bereitstellen:
    
  1. Öffnen Sie eine Datei für den privaten SSH-Schlüssel in Notepad.exe.
  2. Wählen Sie über das Menü **Bearbeiten** **Alle auswählen** aus.
  3. Klicken Sie auf **Bearbeiten** > **Kopieren**.
  4. Wenn Sie die Verbindung erstellen, fügen Sie in der Eigenschaft **SSH private key** den Schlüssel ein. Bearbeiten Sie die Eigenschaft **SSH private key** nicht manuell.

     Der Connector verwendet die SSH.NET-Bibliothek, die folgende SSH-Schlüsselformate unterstützt und nur den MD5-Fingerabdruck:

     * RSA 
     * DSA

* Grundlegende Kenntnisse über die [Erstellung von Logik-Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Die Logik-App, in der Sie auf Ihr SFTP-Konto zugreifen möchten. Um mit einem SFTP-Trigger zu beginnen, [erstellen Sie eine leere Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md). Um eine SFTP-Aktion zu verwenden, starten Sie Ihre Logik-App mit einem anderen Trigger, z.B. dem **Wiederholungstrigger**.

## <a name="connect-to-sftp"></a>Herstellen einer Verbindung mit SFTP

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und öffnen Sie Ihre Logik-App im Logik-App-Designer, sofern sie nicht bereits geöffnet ist.

1. Geben Sie im Fall einer leeren Logik-App im Suchfeld die Zeichenfolge „sftp“ als Filter ein. Wählen Sie in der Triggerliste den gewünschten Trigger aus. 

   Oder

   Wählen Sie für vorhandene Logik-Apps im letzten Schritt zum Hinzufügen einer Aktion die Option **Neuer Schritt** aus. 
   Geben Sie im Suchfeld den Begriff „sftp“ als Filter ein. 
   Wählen Sie in der Liste mit den Aktionen die gewünschte Aktion aus.

   Wenn Sie zwischen Schritten eine Aktion einfügen möchten, bewegen Sie den Mauszeiger über den Pfeil zwischen den Schritten. 
   Wählen Sie das daraufhin angezeigte Pluszeichen (**+**) und dann **Aktion hinzufügen** aus.

1. Geben Sie die erforderlichen Informationen zu Ihrer Verbindung ein, und wählen Sie dann **Erstellen** aus.

1. Geben Sie die erforderlichen Details für Ihren ausgewählten Trigger oder Ihre ausgewählte Aktion an, und fahren Sie mit dem Erstellen Ihres Logik-App-Workflows fort.

## <a name="examples"></a>Beispiele

### <a name="sftp-trigger-when-a-file-is-added-or-modified"></a>SFTP-Trigger: wenn eine Datei hinzugefügt oder geändert wird

Dieser Trigger startet einen Logik-App-Workflow, wenn er erkennt, dass eine Datei auf einem SFTP-Server hinzugefügt oder geändert wurde. Sie können z.B. eine Bedingung hinzufügen, die den Inhalt der Datei überprüft und entscheidet, ob dieser Inhalt abgerufen wird. Die Entscheidung basiert darauf, ob der Inhalt eine bestimmte Bedingung erfüllt oder nicht. Zum Schluss können Sie eine Aktion hinzufügen, die den Inhalt der Datei abruft und in einem Ordner auf dem SFTP-Server ablegt. 

**Beispiel für Unternehmen**: Sie können mit diesem Trigger beispielsweise einen SFTP-Ordner auf neue Dateien überwachen, die Kundenbestellungen darstellen. Dann können Sie eine SFTP-Aktion wie z.B. **Dateiinhalt abrufen** verwenden, um den Inhalt einer Bestellung zur weiteren Verarbeitung und Speicherung in einer Bestelldatenbank abzurufen.

### <a name="sftp-action-get-content"></a>SFTP-Aktion: Inhalt abrufen

Diese Aktion ruft den Inhalt einer Datei auf einem SFTP-Server ab. Sie können z.B. den Trigger aus dem vorherigen Beispiel sowie eine Bedingung hinzufügen, die vom Dateiinhalt erfüllt werden muss. Wenn die Bedingung als TRUE ausgewertet wird, kann die Aktion ausgeführt werden, die den Inhalt abruft. 

## <a name="connector-reference"></a>Connector-Referenz

Technische Details zu Triggern, Aktionen und Beschränkungen aus der OpenAPI-Beschreibung (ehemals Swagger) des Connectors finden Sie auf der [Referenzseite](/connectors/sftpconnector/) des Connectors.

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Wenn Sie Features vorschlagen oder für Vorschläge abstimmen möchten, besuchen Sie die [Website für Logic Apps-Benutzerfeedback](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu anderen [Logic Apps-Connectors](../connectors/apis-list.md)