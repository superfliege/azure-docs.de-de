---
title: IT Service Management Connector in Azure Log Analytics | Microsoft-Dokumentation
description: "Verwenden Sie den IT Service Management Connector zum zentralen Überwachen und Verwalten von ITSM-Arbeitselementen in Azure Log Analytics und zum schnellen Beheben von Problemen."
services: log-analytics
documentationcenter: 
author: JYOTHIRMAISURI
manager: riyazp
editor: 
ms.assetid: 0b1414d9-b0a7-4e4e-a652-d3a6ff1118c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: v-jysur
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 411d6103852cbf534d3c420d5ea7b2146df5164e
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="centrally-manage-itsm-work-items-using-it-service-management-connector-preview"></a>Zentrales Verwalten von ITSM-Arbeitselementen mit dem IT Service Management Connector (Vorschau)

![Symbol für den IT Service Management Connector](./media/log-analytics-itsmc/itsmc-symbol.png)

Der IT Service Management-Connector bietet eine bidirektionale Integration zwischen einem unterstützten IT Service Management (ITSM)-Produkt/-Dienst und Log Analytics.  Über diese Verbindung können Sie Incidents, Warnungen oder Ereignisse im ITSM-Produkt basierend auf Warnungen oder Protokolldatensätzen von Log Analytics erstellen. Der Connector importiert zudem Daten, z.B. Incidents und Änderungsanforderungen, aus dem ITSM-Produkt in OMS Log Analytics.

Mit dem IT Service Management Connector können Sie folgende Aktionen ausführen:

  - Integrieren Sie betriebsbedingte Warnungen mit Ihren Incident Management-Methoden in das ITSM-Tool Ihrer Wahl.
    - Erstellen Sie Arbeitselemente (z.B. Warnungen, Ereignisse, Incidents) aus OMS-Warnungen und über die Protokollsuche in ITSM.
    - Erstellen Sie Arbeitselemente anhand Ihrer Azure-Aktivitätsprotokoll-Warnungen durch ITSM-Aktionen in Aktionsgruppen. 
  
  - Vereinheitlichen Sie die in Ihrem Unternehmen verwendeten Überwachungs-, Protokoll- und Dienstverwaltungsdaten.
    - Korrelieren Sie Incident- und Änderungsanforderungsdaten aus Ihren ITSM-Tools mit relevanten Protokolldaten im Log Analytics-Arbeitsbereich.   
    - Zeigen Sie Dashboards auf oberster Ebene an, um eine Übersicht der Incidents, Änderungsanforderungen und betroffenen Systeme zu erhalten.
    - Schreiben Sie Log Analytics-Abfragen, um Einblicke in Dienstverwaltungsdaten zu erhalten.
      
## <a name="adding-the-it-service-management-connector-solution"></a>Hinzufügen der IT Service Management Connector-Lösung

Fügen Sie mithilfe des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Verfahrens Ihrem Log Analytics-Arbeitsbereich die IT Service Management Connector-Lösung hinzu.

Kachel für den IT Service Management Connector im Lösungskatalog:

![Connector-Kachel](./media/log-analytics-itsmc/itsmc-solutions-tile.png)

Nach dem erfolgreichen Hinzufügen wird der IT Service Management Connector unter **OMS** > **Einstellungen** > **Verbundene Quellen** angezeigt.

![ITSMC verbunden](./media/log-analytics-itsmc/itsmc-overview-solution-in-connected-sources.png)

> [!NOTE]

> Standardmäßig aktualisiert der IT Service Management Connector die Daten der Verbindung einmal alle 24 Stunden. Um die Daten Ihrer Verbindung bei Änderungen oder Vorlagenupdates, die Sie vornehmen, sofort zu aktualisieren, klicken Sie auf die Aktualisierungsschaltfläche, die neben der Verbindung angezeigt wird.

 ![ITSMC-Aktualisierung](./media/log-analytics-itsmc/itsmc-connection-refresh.png)


## <a name="configuring-the-connection-with-your-itsm-software"></a>Konfigurieren der Verbindung mit Ihrer ITSM-Software

IT Service Management-Connector-Lösung unterstützt Verbindungen mit **System Center Service Manager**, **ServiceNow**, **Provance** und **Cherwell**. Konfigurieren Sie Ihre Verbindung mit

- [System Center Service Manager (SCSM)](log-analytics-itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-oms)

- [ServiceNow](log-analytics-itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-oms)

- [Provance](log-analytics-itsmc-connections.md#connect-provance-to-it-service-management-connector-in-oms)  

- [Cherwell](log-analytics-itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-oms)

## <a name="using-the-solution"></a>Verwenden der Lösung

Sobald Sie den IT Service Management Connector mit Ihren ITSM-Softwaredetails konfiguriert haben, beginnt der Connector mit dem Sammeln von Daten aus dem verbundenen ITSM-Produkt/-Dienst. Abhängig von der Anzahl der Incidents und Änderungsanforderungen im ITSM-Produkt/-Dienst sollte die anfängliche Synchronisierung innerhalb weniger Minuten abgeschlossen werden. 

> [!NOTE]
> - Daten, die von der IT Service Management Connector-Lösung aus dem ITSM-Produkt importiert werden, werden in Log Analytics als Protokolldatensätze des Typs **ServiceDesk_CL** angezeigt.
> - Der Protokolldatensatz enthält ein Feld mit dem Namen **ServiceDeskWorkItemType_s**, das entweder ein Incident oder eine Änderungsanforderung ist, d.h. eine der zwei Arten von Daten, die aus dem ITSM-Produkt importiert werden.

## <a name="data-synced-from-itsm-product"></a>Synchronisierte Daten aus dem ITSM-Produkt
Incidents und Änderungsanforderungen aus dem ITSM-Produkt werden mit Ihrem Log Analytics-Arbeitsbereich synchronisiert. Die folgenden Informationen sind Beispiele für Daten, die vom IT Service Management Connector gesammelt werden:

> [!NOTE]
> Abhängig vom in Log Analytics importierten Arbeitselementtyp enthält **ServiceDesk_CL** die folgenden Felder:

**Arbeitselement:** **Incidents**  
ServiceDeskWorkItemType_s="Incident"

**Felder**

- ServiceDeskConnectionName
- Service Desk-ID
- Zustand
- Dringlichkeit
- Auswirkung
- Priority
- Eskalation
- Erstellt von
- Gelöst von
- Geschlossen von
- Quelle
- Zugewiesen zu
- Kategorie
- Titel
- Beschreibung
- Erstellt am
- Geschlossen am
- Gelöst am
- Datum der letzten Änderung
- Computer


**Arbeitselement:** **Änderungsanforderungen**

ServiceDeskWorkItemType_s="ChangeRequest"

**Felder**
- ServiceDeskConnectionName
- Service Desk-ID
- Erstellt von
- Geschlossen von
- Quelle
- Zugewiesen zu
- Titel
- Typ
- Kategorie
- Zustand
- Eskalation
- Konfliktstatus
- Dringlichkeit
- Priority
- Risiko
- Auswirkung
- Zugewiesen zu
- Erstellt am
- Geschlossen am
- Datum der letzten Änderung
- Anforderungsdatum
- Geplantes Startdatum
- Geplantes Enddatum
- Startdatum der Arbeit
- Enddatum der Arbeit
- Beschreibung
- Computer

## <a name="output-data-for-a-servicenow-incident"></a>Ausgabedaten für einen ServiceNow-Incident

| OMS-Feld | ITSM-Feld |
|:--- |:--- |
| ServiceDeskId_s| NUMBER |
| IncidentState_s | Zustand |
| Urgency_s |Dringlichkeit |
| Impact_s |Auswirkung|
| Priority_s | Priority |
| CreatedBy_s | Geöffnet von |
| ResolvedBy_s | Gelöst von|
| ClosedBy_s  | Geschlossen von |
| Source_s| Kontakttyp |
| AssignedTo_s | Zugewiesen zu  |
| Category_s | Kategorie |
| Title_s|  Kurzbeschreibung |
| Description_s|  Hinweise |
| CreatedDate_t|  Geöffnet |
| ClosedDate_t| closed|
| ResolvedDate_t|Gelöst|
| Computer  | Konfigurationselement |

## <a name="output-data-for-a-servicenow-change-request"></a>Ausgabedaten für eine ServiceNow-Änderungsanforderung

| OMS-Feld | ITSM-Feld |
|:--- |:--- |
| ServiceDeskId_s| NUMBER |
| CreatedBy_s | Angefordert von |
| ClosedBy_s | Geschlossen von |
| AssignedTo_s | Zugewiesen zu  |
| Title_s|  Kurzbeschreibung |
| Type_s|  Typ |
| Category_s|  Kategorie |
| CRState_s|  Zustand|
| Urgency_s|  Dringlichkeit |
| Priority_s| Priority|
| Risk_s| Risiko|
| Impact_s| Auswirkung|
| RequestedDate_t  | Datum der Anforderung |
| ClosedDate_t | Geschlossen am |
| PlannedStartDate_t  |     Geplantes Startdatum |
| PlannedEndDate_t  |   Geplantes Enddatum |
| WorkStartDate_t  | Tatsächliches Startdatum |
| WorkEndDate_t | Tatsächliches Enddatum|
| Description_s | Beschreibung |
| Computer  | Konfigurationselement |

**Log Analytics-Beispielbildschirm für ITSM-Daten:**

![Log Analytics-Bildschirm](./media/log-analytics-itsmc/itsmc-overview-sample-log-analytics.png)

## <a name="it-service-management-connector--integration-with-other-oms-solutions"></a>IT Service Management Connector – Integration in andere OMS-Lösungen

Der IT Service Management Connector unterstützt derzeit die Integration in die Service Map-Lösung.

Service Map ermittelt automatisch die Anwendungskomponenten auf Windows- und Linux-Systemen und stellt die Kommunikation zwischen Diensten dar. In dieser Lösung können Sie die Server ihrer Funktion gemäß anzeigen – als verbundene Systeme, die wichtige Dienste bereitstellen. Service Map zeigt Verbindungen zwischen Servern, Prozessen und Ports über die gesamte TCP-Verbindungsarchitektur an. Außer der Installation eines Agents ist keine weitere Konfiguration erforderlich. Weitere Informationen: [Service Map](../operations-management-suite/operations-management-suite-service-map.md).

Wenn Sie auch die Service Map-Lösung verwenden, können Sie die Service Desk-Elemente anzeigen, die in den ITSM-Lösungen erstellt werden, wie im folgenden Beispiel gezeigt:

![Service Map-Integration](./media/log-analytics-itsmc/itsmc-overview-integrated-solutions.png)
## <a name="create-itsm-work-items-for-oms-alerts"></a>Erstellen von ITSM-Arbeitselementen für OMS-Warnungen

Mit der ITSM Connector-Lösung können Sie wie folgt OMS-Warnungen konfigurieren, um die Erstellung von Arbeitselementen in Ihrem verbundenen ITSM-Tool auszulösen:

1. Führen Sie im Fenster **Protokollsuche** eine Protokollsuchabfrage zum Anzeigen von Daten aus. Abfrageergebnisse sind die Quelle für Arbeitselemente.
2. Klicken Sie in **Protokollsuche** auf **Warnung**, um die Seite **Warnungsregel hinzufügen** zu öffnen.

    ![Log Analytics-Bildschirm](./media/log-analytics-itsmc/itsmc-work-items-for-oms-alerts.png)

3. Geben Sie im Fenster **Warnungsregel hinzufügen** die erforderlichen Details für **Name**, **Schweregrad**, **Suchabfrage** und **Warnungskriterien** (Zeitfenster/metrische Maßeinheit) an.
4. Wählen Sie **Ja** für **ITSM-Aktionen** aus.
5. Wählen Sie Ihre ITSM-Verbindung aus der Liste **Verbindung auswählen** aus.
6. Geben Sie die Details nach Bedarf an.
7. Um ein gesondertes Arbeitselement für jeden Protokolleintrag dieser Warnung zu erstellen, aktivieren Sie das Kontrollkästchen **Einzelne Arbeitselemente für jeden Protokolleintrag erstellen**.

    Oder

    Lassen Sie dieses Kontrollkästchen deaktiviert, um nur ein Arbeitselement für eine beliebige Anzahl von Protokolleinträgen zu dieser Warnung zu erstellen.

7. Klicken Sie auf **Speichern**.

Die OMS-Warnung, die Sie erstellt haben, wird unter **Einstellungen**>**Warnungen** angezeigt. Die Arbeitselemente der entsprechenden ITSM-Verbindung werden erstellt, wenn die Bedingung der angegebenen Warnung erfüllt ist.

## <a name="create-itsm-work-items-from-oms-logs"></a>Erstellen von ITSM-Arbeitselementen aus OMS-Protokollen

Sie können auch wie folgt Arbeitselemente direkt über einen Protokolldatensatz in den verbundenen ITSM-Quellen erstellen:

1. Suchen Sie in **Protokollsuche** nach den erforderlichen Daten, wählen Sie die Details aus, und klicken Sie auf **Arbeitselement erstellen**.

    Das Fenster **ITSM-Arbeitselement erstellen** wird angezeigt:

    ![Log Analytics-Bildschirm](media/log-analytics-itsmc/itsmc-work-items-from-oms-logs.png)

2.   Fügen Sie die folgenden Details hinzu:

  - **Arbeitselementtitel**: Titel für das Arbeitselement.
  - **Arbeitselementbeschreibung**: Beschreibung für das neue Arbeitselement.
  - **Betroffener Computer**: Name des Computers, auf dem diese Protokolldaten gefunden wurden.
  - **Verbindung auswählen**: ITSM-Verbindung, in der dieses Arbeitselement erstellt werden soll.
  - **Arbeitselement**: Typ des Arbeitselements.

3. Um eine vorhandene Arbeitselementvorlage für einen Incident zu verwenden, klicken Sie unter **Arbeitselement basierend auf Vorlage generieren** auf **Ja**, und klicken Sie dann auf **Erstellen**.

    Oder

    Klicken Sie auf **Nein**, wenn Sie Ihre benutzerdefinierten Werte bereitstellen möchten.

4. Geben Sie die entsprechenden Werte in die Textfelder **Kontakttyp**, **Auswirkung**, **Dringlichkeit**, **Kategorie** und **Unterkategorie** ein, und klicken Sie dann auf **Erstellen**.

## <a name="create-itsm-work-items-from-azure-alerts"></a>Erstellen von ITSM-Arbeitselementen aus Azure-Warnungen
Der ITSM Connector ist jetzt in Aktionsgruppen integriert. Mit den wiederverwendbaren [Aktionsgruppen](../monitoring-and-diagnostics/monitoring-action-groups.md) können Sie modular Aktionen für Ihre Azure-Warnungen auszulösen. Die ITSM-Aktion in Aktionsgruppen erstellt mit einer vorhandenen ITSM Connector-Lösung Arbeitselemente in Ihrem ITSM-Produkt.

1. Klicken Sie im Azure-Portal auf **Überwachen**.
2. Klicken Sie im linken Bereich auf **Aktionsgruppen**.

    ![Aktionsgruppen](media/log-analytics-itsmc/ActionGroups.png)

3. Geben Sie **Name** und **Kurzname** für Ihre Aktionsgruppe ein. Wählen Sie die **Ressourcengruppe** und das **Abonnement**, wo Sie Ihre Aktionsgruppe erstellen möchten.

    ![Aktionsgruppendetail](media/log-analytics-itsmc/ActionGroupsDetail.png)

4. Wählen Sie in der Liste „Aktionen“ **ITSM** aus der Dropdownliste für **Aktionstyp**. Stellen Sie einen **Namen** für die Aktion bereit, und klicken Sie auf **Details bearbeiten**.


5. Wählen Sie das **Abonnement**, in dem sich Ihr Log Analytics-Arbeitsbereich befindet. Wählen Sie die **Verbindung**, d.h. Ihren ITSM Connector-Name gefolgt vom Namen Ihres Arbeitsbereichs. Beispiel: „MyITSMMConnector(MyWorkspace)“.

    ![ITSM-Aktionsdetails](./media/log-analytics-itsmc/ITSMActionDetails.png)

6. Wählen Sie in der Dropdownliste den Typ **Arbeitselement** aus.
7. Wählen Sie eine vorhandene Vorlage, oder füllen Sie die erforderlichen Felder Ihres ITSM-Produkts aus.
8. Klicken Sie auf **OK**

Verwenden Sie beim Erstellen/Bearbeiten einer Azure-Warnungsregel eine Aktionsgruppe, die eine ITSM-Aktion enthält. Wenn die Warnung ausgelöst wird, wird das Arbeitselement im ITSM-Tool erstellt. 

>[!NOTE]
>Derzeit unterstützen nur Aktivitätsprotokollwarnungen die ITSM-Aktion. Für andere Azure-Warnungen ist diese Aktion keine Option.
>


## <a name="troubleshoot-itsm-connections-in-oms"></a>Problembehandlung bei ITSM-Verbindungen in OMS
1.  Wenn für die Verbindung auf der Benutzeroberfläche der verbundenen Quelle ein Fehler auftritt, und die Fehlermeldung **Fehler beim Speichern der Verbindung** angezeigt wird, gehen Sie folgendermaßen vor:
 - Stellen Sie für ServiceNow-, Cherwell- und Provance-Verbindungen sicher,
    - dass Sie Benutzername, Kennwort, Client-ID  und geheimen Clientschlüssel für jede der Verbindungen richtig eingegeben haben.
    - Überprüfen Sie, ob Sie über ausreichende Berechtigungen für das entsprechende ITSM-Produkt verfügen, um die Verbindung herzustellen.
 - Stellen Sie für Service Manager sicher,
     - dass die Web-App erfolgreich bereitgestellt und die Hybridverbindung erstellt wird. Um zu überprüfen, ob die Verbindung mit dem lokalen Service Manager-Computer erfolgreich hergestellt wird, besuchen Sie die Web-App-URL, wie in der Dokumentation zum Herstellen der [Hybridverbindung](log-analytics-itsmc-connections.md#configure-the-hybrid-connection) erläutert.
     
2.  Wenn Daten von ServiceNow nicht in Log Analytics synchronisiert werden, stellen Sie sicher, dass sich die ServiceNow-Instanz nicht im Energiesparmodus befindet. ServiceNow-Entwicklungsinstanzen wechseln manchmal nach längerem Leerlauf in den Energiesparmodus. Melden Sie das Problem andernfalls.
3.  Wenn OMS-Warnungen ausgelöst werden, aber keine Arbeitselemente im ITSM-Produkt erstellt werden, oder Konfigurationselemente nicht erstellt/nicht mit Arbeitselementen verknüpft werden, oder für sonstige allgemeine Informationen, nutzen Sie folgende Quellen:
 -  **IT Service Management Connector-Lösung**: Die Lösung zeigt eine Zusammenfassung der Verbindungen/Arbeitselemente/Computer usw. Klicken Sie auf die Kachel **Connectorstatus**, sodass Sie zur **Protokollsuche** mit der relevanten Abfrage gelangen. Untersuchen Sie die Protokolldatensätze, für die „LogType_S“ den Wert „ERROR“ enthält, auf weitere Informationen.
 - Sie können auch die Fehler/zugehörigen Informationen mithilfe der Abfrage *Type=ServiceDeskLog_CL* direkt auf der Seite **Protokollsuche** anzeigen.

## <a name="troubleshoot-service-manager-web-app-deployment"></a>Problembehandlung bei der Service Manager-Web-App-Bereitstellung
1.  Stellen Sie bei Problemen mit der Web-App-Bereitstellung sicher, dass Sie für das angegebene Abonnement über ausreichende Berechtigungen zum Erstellen/Bereitstellen von Ressourcen verfügen.
2.  Wenn die Fehlermeldung **Objektverweis ist nicht auf eine Instanz eines Objekts festgelegt** angezeigt wird, während Sie das [Skript](log-analytics-itsmc-service-manager-script.md) ausführen, stellen Sie sicher, dass Sie im Abschnitt **Benutzerkonfiguration** gültige Werte eingegeben haben.
3.  Wenn Sie den Service Bus Relay-Namespace nicht erstellen, stellen Sie sicher, dass der erforderliche Ressourcenanbieter im Abonnement registriert ist. Wenn er nicht registriert ist, erstellen Sie den Service Bus Relay-Namespace manuell über das Azure-Portal. Sie können ihn auch beim [Erstellen der Hybridverbindung](log-analytics-itsmc-connections.md#configure-the-hybrid-connection) über das Azure-Portal erstellen.


## <a name="contact-us"></a>Kontakt

Kontaktieren Sie uns bei Fragen oder Feedback zum IT Service Management Connector über [omsitsmfeedback@microsoft.com](mailto:omsitsmfeedback@microsoft.com).

## <a name="next-steps"></a>Nächste Schritte
[Hinzufügen von ITSM-Produkten/-Diensten zum IT Service Management Connector](log-analytics-itsmc-connections.md).

