---
title: 'Tutorial: Erstellen und Verwalten eines Stream Analytics-Auftrags mit dem Azure-Portal | Microsoft-Dokumentation'
description: In diesem Tutorial wird umfassend dargestellt, wie Sie Azure Stream Analytics zum Analysieren von betrügerischen Anrufen in einem Telefonanruf-Datenstrom verwenden.
services: stream-analytics
author: SnehaGunda
ms.author: sngun
manager: kfile
ms.service: stream-analytics
ms.workload: data-services
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/04/2018
ms.openlocfilehash: 524b15747a275c76fec6c529e4f00d0da1b41420
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "32778188"
---
# <a name="create-a-stream-analytics-job-to-analyze-phone-call-data-and-visualize-results-in-a-power-bi-dashboard"></a>Erstellen eines Stream Analytics-Auftrags zum Analysieren von Telefonanrufdaten und Visualisieren von Ergebnissen in einem Power BI-Dashboard

In diesem Tutorial wird gezeigt, wie Sie Azure Stream Analytics zum Analysieren eines Beispieltelefonanrufs verwenden, der von einer Clientanwendung generiert wird. Die von der Clientanwendung generierten Telefonanrufdaten enthalten einige betrügerische Anrufe, und wir definieren einen Stream Analytics-Auftrag, um diese Anrufe zu filtern.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Generieren von Beispiel-Telefonanrufdaten und Senden von Anrufen an Azure Event Hubs  
> * Erstellen eines Stream Analytics-Auftrags   
> * Konfigurieren der Ein- und Ausgabe für den Auftrag  
> * Definieren einer Abfrage zum Filtern von betrügerischen Anrufen  
> * Testen und Starten des Auftrags  
> * Visualisieren der Ergebnisse in Power BI 

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie zunächst sicher, dass Sie über Folgendes verfügen:

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen.  
* Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.  
* Laden Sie die App [TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) zum Generieren von Anrufereignissen aus dem Microsoft Download Center herunter, oder rufen Sie den Quellcode auf [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator) ab.  

## <a name="create-an-azure-event-hub"></a>Erstellen eines Azure Event Hubs 

Bevor Stream Analytics den Datenstrom mit den betrügerischen Anrufen analysieren kann, sollten Sie die Daten an Azure senden. In diesem Tutorial senden Sie Daten an Azure, indem Sie [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/event-hubs-what-is-event-hubs) verwenden. Für dieses Tutorial erstellen Sie einen Event Hub und weisen die App zum Generieren von Anrufereignissen an, Anrufdaten an den betreffenden Event Hub zu senden. Führen Sie die folgenden Schritte aus, um einen Event Hub zu erstellen:

1. Melden Sie sich beim Azure-Portal an.  
2. Wählen Sie **Ressource erstellen** > **Internet der Dinge (IoT)** > **Event Hubs**.  

   ![Suchen nach dem Event Hub](media/stream-analytics-manage-job/find-eh.png)
3. Fügen Sie im Bereich **Namespace erstellen** die folgenden Werte ein:  

   |**Einstellung**  |**Empfohlener Wert** |**Beschreibung**  |
   |---------|---------|---------|
   |NAME     | myEventHubNS        |  Ein eindeutiger Name zum Identifizieren des Event Hub-Namespace.       |
   |Abonnement     |   \<Ihr Abonnement\>      |   Wählen Sie ein Azure-Abonnement aus, unter dem Sie den Event Hub erstellen möchten.      |
   |Ressourcengruppe     |   MyASADemoRG      |  Wählen Sie **Neu erstellen** aus, und geben Sie einen neuen Ressourcengruppennamen für Ihr Konto ein.       |
   |Speicherort     |   USA, Westen 2      |    Der Standort, an dem der Event Hub-Namespace bereitgestellt werden kann.     |

4. Verwenden Sie für die verbleibenden Einstellungen die Standardoptionen, und wählen Sie **Erstellen**.  

   ![Erstellen eines Event Hub-Namespace](media/stream-analytics-manage-job/create-ehns.png)

5. Nachdem die Bereitstellung des Namespace abgeschlossen ist, können Sie zu **Alle Ressourcen** navigieren, in der Liste mit den Azure-Ressourcen nach „myEventHubNS“ suchen und den Eintrag dann auswählen, um ihn zu öffnen.  
6. Wählen Sie als Nächstes **+Event Hub** > **Name**, und geben Sie dem Event Hub den Namen „MyEventHub“. Sie können auch einen anderen Namen verwenden. Verwenden Sie für die verbleibenden Einstellungen die Standardoptionen, wählen Sie **Erstellen**, und warten Sie auf den erfolgreichen Abschluss der Bereitstellung.

   ![Event Hub erstellen](media/stream-analytics-manage-job/create-eh.png)

### <a name="grant-access-to-the-event-hub-and-get-a-connection-string"></a>Gewähren des Zugriffs auf den Event Hub und Abrufen einer Verbindungszeichenfolge

Bevor eine Anwendung Daten an Azure Event Hubs senden kann, muss der Event Hub mit einer Richtlinie versehen werden, die einen entsprechenden Zugriff ermöglicht. Die Zugriffsrichtlinie erzeugt eine Verbindungszeichenfolge, die Autorisierungsinformationen enthält.

1. Navigieren Sie zu den **Event Hubs**, die Sie im vorherigen Schritt erstellt haben („MyEventHub“), und wählen Sie im Event Hub-Bereich die Option **Freigegebene Zugriffsrichtlinien** und dann **+Hinzufügen**.  
2. Legen Sie den Richtliniennamen auf **Mypolicy** fest, und wählen Sie **Verwalten** und dann **Erstellen**.  

   ![Erstellen einer SAS-Richtlinie für den Event Hub](media/stream-analytics-manage-job/create-ehpolicy.png)

3. Nachdem die Richtlinie bereitgestellt wurde, können Sie sie auswählen, um sie zu öffnen. Suchen Sie nach **Verbindungszeichenfolge – Primärschlüssel**, und wählen Sie neben der Verbindungszeichenfolge die Option **Kopieren**.  
4. Fügen Sie die Verbindungszeichenfolge in einen Texteditor ein. Sie benötigen diese Verbindungszeichenfolge im nächsten Abschnitt.  

   Die Verbindungszeichenfolge sieht wie folgt aus:

   `Endpoint=sb://<Your event hub namespace>.servicebus.windows.net/;SharedAccessKeyName=<Your shared access policy name>;SharedAccessKey=<generated key>;EntityPath=<Your event hub name>` 

   Beachten Sie, dass die Verbindungszeichenfolge mehrere Schlüssel-Wert-Paare enthält, die durch Semikolons getrennt sind: Endpoint, SharedAccessKeyName, SharedAccessKey und EntityPath.  

5. Entfernen Sie das EntityPath-Paar aus der Verbindungszeichenfolge (einschließlich des vorangestellten Semikolons).

## <a name="start-the-event-generator-application"></a>Starten der Ereignisgenerator-Anwendung

Vor dem Starten der TelcoGenerator-App sollten Sie diese so konfigurieren, dass Daten an die zuvor erstellten Azure Event Hubs gesendet werden.

1. Extrahieren Sie den Inhalt der Datei [TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip).  
2. Öffnen Sie die Datei `TelcoGenerator\TelcoGenerator\telcodatagen.exe.config` in einem Text-Editor Ihrer Wahl. (Da mehr als eine CONFIG-Datei vorhanden ist, sollten Sie darauf achten, dass Sie die richtige Datei öffnen.)  

3. Aktualisieren Sie das <appSettings>-Element in der CONFIG-Datei mit den folgenden Details:

   * Legen Sie den Wert des EventHubName-Schlüssels auf den Wert von EntityPath in der Verbindungszeichenfolge fest.  
   * Legen Sie den Wert des Microsoft.ServiceBus.ConnectionString-Schlüssels auf die Verbindungszeichenfolge ohne den EntityPath-Wert fest, bei dem es sich um den Wert handelt, den Sie im vorherigen Abschnitt in Schritt 5 erhalten haben.

4. Speichern Sie die Datei .  
5. Öffnen Sie als Nächstes ein Befehlsfenster, ändern Sie den Ordner an dem Speicherort, an dem Sie die TelcoGenerator-Anwendung entzippt haben, und geben Sie den folgenden Befehl ein:

   ```
   telcodatagen.exe 1000 .2 2
   ```

   Für diesen Befehl werden die folgenden Parameter verwendet:
   * **Anzahl von Anrufdatensätzen pro Stunde**.  
   * **Betrugswahrscheinlichkeit in Prozent**: Gibt an, wie oft die App einen betrügerischen Anruf simulieren soll. Der Wert „.2“ bedeutet, dass etwa 20 % der Anrufdatensätze betrügerische Anrufe simulieren.  
   * **Dauer in Stunden**: Der Zeitraum von Stunden, über den die App ausgeführt werden soll. Sie können die App auch jederzeit beenden, indem Sie den Prozess in der Befehlszeile beenden (STRG+C).

   Nach wenigen Sekunden werden auf dem Bildschirm in der App die Telefonanrufdatensätze angezeigt, während diese an den Event Hub gesendet werden. Die Telefonanrufdaten enthalten die folgenden Felder:

   |**Datensatz**  |**Definition**  |
   |---------|---------|
   |CallrecTime    |  Der Zeitstempel für die Startzeit des Anrufs.       |
   |SwitchNum     |  Die für die Anrufverbindung verwendete Vermittlungsstelle. In diesem Beispiel werden die Vermittlungen durch Zeichenfolgen ausgedrückt, die das Ursprungsland (USA, China, Großbritannien, Deutschland oder Australien) darstellen.       |
   |CallingNum     |  Die Telefonnummer des Anrufers.       |
   |CallingIMSI     |  Die IMSI (International Mobile Subscriber Identity). Dies ist eine eindeutige ID des Anrufers.       |
   |CalledNum     |   Die Telefonnummer des Angerufenen.      |
   |CalledIMSI     |  International Mobile Subscriber Identity (IMSI). Dies ist eine eindeutige ID des Empfängers.       |

## <a name="create-a-stream-analytics-job"></a>Erstellen eines Stream Analytics-Auftrags 

Nachdem Sie nun über einen Datenstrom mit Anrufereignissen verfügen, können Sie einen Stream Analytics-Auftrag erstellen, mit dem Daten aus dem Event Hub gelesen werden.

1. Navigieren Sie zum Azure-Portal, um einen Stream Analytics-Auftrag zu erstellen.  

2. Wählen Sie **Ressource erstellen** > **Internet der Dinge (IoT)** > **Stream Analytics-Auftrag**.  

3. Fügen Sie die folgenden Werte in den Bereich **Neuer Stream Analytics-Auftrag** ein:  

   |**Einstellung**  |**Empfohlener Wert**  |**Beschreibung**  |
   |---------|---------|---------|
   |Auftragsname     |  ASATutorial       |   Ein eindeutiger Name zum Identifizieren des Event Hub-Namespace.      |
   |Abonnement    |  \<Ihr Abonnement\>   |   Wählen Sie ein Azure-Abonnement aus, unter dem Sie den Auftrag erstellen möchten.       |
   |Ressourcengruppe   |   MyASADemoRG      |   Wählen Sie **Vorhandene verwenden**, und geben Sie einen neuen Ressourcengruppennamen für Ihr Konto ein.      |
   |Speicherort   |    USA, Westen 2     |      Der Standort, an dem der Auftrag bereitgestellt werden kann. Es empfiehlt sich, den Auftrag und den Event Hub in derselben Region zu platzieren, damit Sie die optimale Leistung erzielen und Ihnen keine Kosten für die Übertragung von Daten zwischen Regionen entstehen.      |
   |Hosting-Umgebung    | Cloud        |     Für Stream Analytics-Aufträge ist eine Cloud- oder Edge-Bereitstellung möglich. Mit der Option „Cloud“ können die Aufträge in der Azure Cloud und mit der Option „Edge“ auf einem IoT Edge-Gerät bereitgestellt werden.    |
   |Streamingeinheiten     |    1       |      Streamingeinheiten sind die Computingressourcen, die für die Ausführung eines Auftrags erforderlich sind. Standardmäßig ist dieser Wert auf 1 festgelegt. Informationen zum Skalieren von Streamingeinheiten finden Sie im Artikel [Überblick über Streamingeinheiten und Informationen zu Anpassungen](stream-analytics-streaming-unit-consumption.md).      |

   ![Erstellen eines Auftrags](media/stream-analytics-manage-job/create-a-job.png)   

4. Verwenden Sie für die verbleibenden Einstellungen die Standardoptionen, wählen Sie **Erstellen**, und warten Sie auf den erfolgreichen Abschluss der Bereitstellung.

## <a name="configure-job-input"></a>Konfigurieren einer Auftragseingabe

Im nächsten Schritt wird eine Eingabequelle für den Auftrag zum Lesen von Daten definiert. Für dieses Tutorial verwenden Sie den Event Hub, den Sie im vorherigen Schritt erstellt haben, als Eingabe. Führen Sie die folgenden Schritte aus, um die Eingabe für Ihren Auftrag zu konfigurieren:

1. Öffnen Sie im Azure-Portal den Bereich **Alle Ressourcen**, und suchen Sie nach dem Stream Analytics-Auftrag „ASATutorial“.  

2. Wählen Sie im Bereich „Stream Analytics-Auftrag“ im Abschnitt **Auftragstopologie** die Option **Eingaben**.  

3. Wählen Sie **+Datenstromeingabe hinzufügen** (die Referenzeingabe bezieht sich auf statische Nachschlagedaten, die Sie in diesem Tutorial nicht nutzen) und **Event Hub**, und fügen Sie die folgenden Werte in den Bereich ein:  

   |**Einstellung**  |**Empfohlener Wert**  |**Beschreibung**  |
   |---------|---------|---------|
   |Eingabealias     |  CallStream       |  Geben Sie einen Anzeigenamen zum Identifizieren Ihrer Eingabe an. Der Eingabealias darf nur alphanumerische Zeichen, Bindestriche und Unterstriche enthalten und muss zwischen 3 und 63 Zeichen lang sein.       |
   |Abonnement    |   \<Ihr Abonnement\>      |   Wählen Sie das Azure-Abonnement aus, unter dem Sie den Event Hub erstellt haben. Der Event Hub kann sich unter demselben oder einem anderen Abonnement wie der Stream Analytics-Auftrag befinden.       |
   |Event Hub-Namespace    |  MyEventHubNS       |  Wählen Sie den Event Hub-Namespace aus, den Sie im vorherigen Abschnitt erstellt haben. Alle Event Hub-Namespaces, die unter Ihrem aktuellen Abonnement verfügbar sind, sind in der Dropdownliste aufgeführt.       |
   |Event Hub-Name    |   MyEventHub      |  Wählen Sie den Event Hub aus, den Sie im vorherigen Abschnitt erstellt haben. Alle Event Hubs, die unter Ihrem aktuellen Abonnement verfügbar sind, sind in der Dropdownliste aufgeführt.       |
   |Event Hub-Richtlinienname   |  Mypolicy       |  Wählen Sie die Event Hub-SAS-Richtlinie aus, die Sie im vorherigen Abschnitt erstellt haben. Alle Event Hub-Richtlinien, die unter Ihrem aktuellen Abonnement verfügbar sind, sind in der Dropdownliste aufgeführt.       |

   ![Konfigurieren der Eingabe](media/stream-analytics-manage-job/configure-input.png) 

4. Verwenden Sie für die verbleibenden Einstellungen die Standardoptionen, wählen Sie **Speichern**, und warten Sie auf den erfolgreichen Abschluss der Bereitstellung.

## <a name="configure-job-output"></a>Konfigurieren der Auftragsausgabe 

Der letzte Schritt umfasst das Definieren einer Ausgabesenke für den Auftrag, in die die transformierten Daten geschrieben werden können. Für dieses Tutorial geben Sie die Ergebnisse in Power BI aus und visualisieren die Daten. Führen Sie die folgenden Schritte aus, um die Ausgabe für Ihren Auftrag zu konfigurieren:

1. Öffnen Sie im Azure-Portal den Bereich **Alle Ressourcen** und den Stream Analytics-Auftrag „ASATutorial“.  

2. Wählen Sie im Bereich „Stream Analytics-Auftrag“ im Abschnitt **Auftragstopologie** die Option **Ausgaben**.  

3. Wählen Sie **+Hinzufügen** > **Power BI**, und fügen Sie die folgenden Details in das Formular ein (Sie können einen Anzeigenamen zum Identifizieren von Ausgabealias, Datasetname und Tabellenname angeben, wie in der Tabelle gezeigt). Wählen Sie anschließend **Autorisieren**:  

   |**Einstellung**  |**Empfohlener Wert**  |
   |---------|---------|---------|
   |Ausgabealias  |  MyPBIoutput  |
   |Datasetname  |   ASAdataset  | 
   |Tabellenname |  ASATable  | 

   ![Konfigurieren der Ausgabe](media/stream-analytics-manage-job/configure-output.png)  

4. Nachdem Sie **Autorisieren** gewählt haben, werden Sie in einem Popupfenster aufgefordert, Anmeldeinformationen für die Authentifizierung Ihres Power BI-Kontos anzugeben. Nachdem die Autorisierung erfolgreich abgeschlossen wurde, können Sie die Einstellungen speichern, indem Sie **Speichern** wählen. 

## <a name="define-a-query-to-analyze-input-data"></a>Definieren einer Abfrage zum Analysieren von Eingabedaten

Nachdem Sie einen Stream Analytics-Auftrag eingerichtet haben, um einen eingehenden Datenstrom zu lesen, ist der nächste Schritt die Erstellung einer Transformation, bei der Daten in Echtzeit analysiert werden. Sie definieren die Transformationsabfrage mit der [Stream Analytics-Abfragesprache](https://msdn.microsoft.com/library/dn834998.aspx). In diesem Tutorial definieren Sie eine Abfrage, mit der betrügerische Anrufe erkannt werden, die in den Telefondaten enthalten sind. 

Bei diesem Beispiel werden Anrufe als betrügerisch angesehen, die von demselben Benutzer stammen, aber von verschiedenen Standorten ausgehen, und für die die Dauer zwischen beiden Anrufen fünf Sekunden beträgt. Beispielsweise kann derselbe Benutzer nicht gleichzeitig einen legitimen Anruf aus den USA und aus Australien tätigen. Führen Sie die folgenden Schritte aus, um die Transformationsabfrage für Ihren Stream Analytics-Auftrag zu definieren:

1. Öffnen Sie im Azure-Portal den Bereich **Alle Ressourcen** und dann den Stream Analytics-Auftrag **ASATutorial**, den Sie weiter oben erstellt haben.  

2. Wählen Sie im Bereich „Stream Analytics-Auftrag“ im Abschnitt **Auftragstopologie** die Option **Abfrage**. Im Popupfenster sind die Ein- und Ausgaben aufgeführt, die für den Auftrag konfiguriert sind, und Sie können darin eine Abfrage zum Transformieren des Eingabedatenstroms erstellen.  

3. Ersetzen Sie als Nächstes die vorhandene Abfrage im Editor durch die folgenden Daten. Mit dieser Abfrage wird eine Selbstverknüpfung für ein 5-Sekunden-Intervall der Anrufdaten durchgeführt:

   ```sql
   SELECT System.Timestamp AS WindowEnd, COUNT(*) AS FraudulentCalls
   INTO "MyPBIoutput"
   FROM "CallStream" CS1 TIMESTAMP BY CallRecTime
   JOIN "CallStream" CS2 TIMESTAMP BY CallRecTime
   ON CS1.CallingIMSI = CS2.CallingIMSI
   AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
   WHERE CS1.SwitchNum != CS2.SwitchNum
   GROUP BY TumblingWindow(Duration(second, 1))
   ```

   Sie sollten für die Streamingdaten eine Selbstverknüpfung basierend auf dem Wert `CallRecTime` durchführen, um die Daten auf betrügerische Anrufe zu prüfen. Anschließend können Sie nach Anrufdatensätzen suchen, bei denen der Wert `CallingIMSI` (die ursprüngliche Anzahl) identisch ist, aber nicht der Wert `SwitchNum` (Ursprungsland). Wenn Sie eine Verknüpfung per JOIN-Vorgang für Streamingdaten durchführen, müssen bei der Verknüpfung einige Beschränkungen dazu festgelegt werden, welchen Zeitabstand die übereinstimmenden Zeilen haben können. Da die Streamingdaten endlos sind, werden die Zeitgrenzen für die Beziehung in der ON-Klausel der Verknüpfung angegeben. Hierfür wird die Funktion [DATEDIFF](https://msdn.microsoft.com/azure/stream-analytics/reference/datediff-azure-stream-analytics) verwendet. 

   Diese Abfrage entspricht bis auf die Funktion DATEDIFF einer regulären SQL-Verknüpfung. Die in dieser Abfrage verwendete Funktion DATEDIFF gilt spezifisch für Streaming Analytics und muss in der `ON...BETWEEN`-Klausel verwendet werden.  

4. **Speichern** Sie die Abfrage.  

   ![Definieren der Abfrage](media/stream-analytics-manage-job/define-query.png) 

## <a name="test-your-query"></a>Testen Ihrer Abfrage

Sie können eine Abfrage über den Abfrage-Editor testen, und Sie benötigen für diesen Vorgang Beispieldaten. In dieser exemplarischen Vorgehensweise extrahieren Sie Beispieldaten aus dem Telefonanruf-Datenstrom, der vom Event Hub stammt. Führen Sie die folgenden Schritte aus, um die Abfrage zu testen:

1. Stellen Sie sicher, dass die TelcoGenerator-App ausgeführt wird und Telefonanruf-Datensätze erzeugt.  

2. Wählen Sie im Bereich **Abfrage** die Punkte neben der Eingabe „CallStream“, und wählen Sie anschließend die Option **Beispieldaten aus Eingabe**. Es wird ein Bereich geöffnet, in dem Sie angeben können, wie viele Beispieldaten aus dem Eingabedatenstrom gelesen werden sollen.  

   ![Beispieleingabedaten](media/stream-analytics-manage-job/sample-input-data.png)  

3. Legen Sie **Minuten** auf 3 fest, und wählen Sie **OK**. Für einen Zeitraum von drei Minuten werden Stichprobendaten aus dem Eingabedatenstrom entnommen, und Sie werden benachrichtigt, wenn die Beispieldaten bereit sind. Sie können den Status der Stichprobenentnahme in der Benachrichtigungsleiste anzeigen. 

   Die Beispieldaten werden vorübergehend gespeichert und sind verfügbar, solange das Abfragefenster geöffnet ist. Wenn Sie das Abfragefenster schließen, werden die Beispieldaten verworfen, und Sie müssen einen neuen Satz von Beispieldaten erstellen. Alternativ können Sie über [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample Data/telco.json) eine JSON-Datei mit Beispieldaten abrufen und diese JSON-Datei dann zur Verwendung als Beispieldaten für die Eingabe „CallStream“ hochladen.  

4. Wählen Sie **Testen**, um die Abfrage zu testen. Sie sollten Ausgabeergebnisse wie im folgenden Screenshot erhalten:  

   ![Testausgabe](media/stream-analytics-manage-job/test-output.png)

## <a name="start-the-job-and-visualize-output"></a>Starten des Auftrags und Visualisieren der Ausgabe

1. Navigieren Sie zum Starten des Auftrags zum Bereich **Übersicht** Ihres Auftrags, und wählen Sie **Starten**.  

2. Wählen Sie als Startzeit für die Auftragsausgabe die Option **Jetzt** und anschließend **Starten**. Der Auftrag wird innerhalb weniger Minuten gestartet, und Sie können den Status in der Benachrichtigungsleiste anzeigen.  

3. Navigieren Sie nach dem erfolgreichen Abschluss des Auftrags zu [Powerbi.com](https://powerbi.com/), und melden Sie sich mit Ihrem Geschäfts-, Schul- oder Unikonto an. Sofern die Stream Analytics-Auftragsabfrage Ergebnisse ausgibt, werden Sie feststellen, dass das Dataset bereits erstellt wurde. Navigieren Sie zur Registerkarte **Datasets**, auf der Sie ein Dataset mit dem Namen „ASAdataset“ anzeigen können.  

4. Wählen Sie in Ihrem Arbeitsbereich die Option **+Erstellen**. Erstellen Sie ein neues Dashboard, und geben Sie ihm den Namen „Betrügerische Anrufe“. Sie fügen diesem Dashboard zwei Kacheln hinzu. Eine Kachel wird zum Anzeigen der Anzahl von betrügerischen Anrufen einer bestimmten Instanz verwendet, und die andere Kachel enthält eine Visualisierung per Liniendiagramm.  

5. Wählen Sie oben im Fenster die Option **Kachel hinzufügen** und dann Folgendes:**Benutzerdefinierte Streamingdaten** > „Weiter“ > **ASAdataset** (für „Visualisierungstyp“) > **Karte** und **fraudulentcalls** (für „Felder“). Wählen Sie **Weiter**, geben Sie einen Namen für die Kachel ein, und wählen Sie **Übernehmen**.  

   ![Erstellen von Kacheln](media/stream-analytics-manage-job/create-tiles.png)

6. Führen Sie die vier Schritte mit den folgenden Optionen erneut aus:
   * Wählen Sie unter „Visualisierungstyp“ die Option „Liniendiagramm“.  
   * Fügen Sie eine Achse hinzu, und wählen Sie **windowend** aus.  
   * Fügen Sie einen Wert hinzu, und wählen Sie **fraudulentcalls**.  
   * Wählen Sie für **Das anzuzeigende Zeitfenster** die letzten zehn Minuten aus.  

7. Nachdem beide Kacheln hinzugefügt wurden, sieht Ihr Dashboard wie im folgenden Screenshot aus. Beachten Sie Folgendes: Wenn Ihre Event Hub-Absenderanwendung und Streaming Analytics-Anwendung ausgeführt werden, wird Ihr Power BI-Dashboard regelmäßig aktualisiert, wenn neue Daten eintreffen.  

   ![Power BI-Ergebnisse](media/stream-analytics-manage-job/power-bi-results.png)

## <a name="embedding-your-powerbi-dashboard-in-a-web-application"></a>Einbetten Ihres Power BI-Dashboards in eine Webanwendung

Für diesen Teil des Tutorials verwenden Sie eine [ASP.NET](http://asp.net/)-Beispielwebanwendung, die vom Power BI-Team zum Einbetten Ihres Dashboards erstellt wurde. Weitere Informationen zum Einbetten von Dashboards finden Sie im Artikel [Einbetten mit Power BI](https://docs.microsoft.com/power-bi/developer/embedding).

In diesem Tutorial führen wir die Schritte für den Benutzer aus, bei dem es sich um den Besitzer der Datenanwendung handelt. Navigieren Sie zum Einrichten der Anwendung zum GitHub-Repository [PowerBI-Developer-Samples](https://github.com/Microsoft/PowerBI-Developer-Samples), und befolgen Sie die Anleitung im Abschnitt **User Owns Data** (Benutzer ist Besitzer der Daten). (Verwenden Sie die Umleitungs- und Startseiten-URLs im Unterabschnitt **integrate-dashboard-web-app**.) Da wir das Dashboard-Beispiel verwenden, können Sie den Beispielcode für „integrate-dashboard-web-app“ aus dem [GitHub-Repository](https://github.com/Microsoft/PowerBI-Developer-Samples/tree/master/User Owns Data/integrate-dashboard-web-app) nutzen.
Nachdem die Anwendung in Ihrem Browser ausgeführt wird, können Sie das Dashboard, das Sie zuvor erstellt haben, mit diesen Schritten in die Webseite einbetten:

1. Wählen Sie die Option **Bei Power BI anmelden**, mit der der Anwendung Zugriff auf die Dashboards in Ihrem Power BI-Konto gewährt wird.  

2. Wählen Sie die Schaltfläche **Get Dashboards** (Dashboards abrufen), mit der die Dashboards Ihres Kontos in einer Tabelle angezeigt werden. Suchen Sie nach dem Namen des zuvor erstellten Dashboards (powerbi-embedded-dashboard), und kopieren Sie die entsprechende **EmbedUrl**.  

3. Fügen Sie die **EmbedUrl** abschließend in das entsprechende Textfeld ein, und wählen Sie **Embed Dashboard** (Dashboard einbetten). Sie können das Dashboard jetzt als eingebettetes Dashboard einer Webanwendung anzeigen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen einfachen Stream Analytics-Auftrag erstellt, die eingehenden Daten analysiert und Ergebnisse in einem Power BI-Dashboard dargestellt. Weitere Informationen zu Stream Analytics-Aufträgen erhalten Sie im nächsten Tutorial:

> [!div class="nextstepaction"]
> [Ausführen von Azure Functions in Azure Stream Analytics-Aufträgen](stream-analytics-with-azure-functions.md)
