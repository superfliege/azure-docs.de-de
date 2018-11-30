---
title: Herstellen einer Verbindung zwischen Configuration Manager und Log Analytics | Microsoft Docs
description: Dieser Artikel beschreibt die Schritte zum Verbinden von Configuration Manager mit Log Analytics und den Beginn der Analyse von Daten.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: f2298bd7-18d7-4371-b24a-7f9f15f06d66
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/22/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: a4c429b1c54c6bf71175abbc763a178792281a0f
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/30/2018
ms.locfileid: "52633145"
---
# <a name="connect-configuration-manager-to-log-analytics"></a>Herstellen einer Verbindung zwischen Configuration Manager und Log Analytics
Sie können Ihre System Center Configuration Manager-Umgebung mit Azure Log Analytics verbinden, um die Daten der Gerätesammlung zu synchronisieren und auf diese Sammlungen in Log Analytics und Azure Automation zu verweisen.  

## <a name="prerequisites"></a>Voraussetzungen

Log Analytics unterstützt System Center Configuration Manager Current Branch, Version 1606 und höher.  

## <a name="configuration-overview"></a>Konfigurationsübersicht
In den folgenden Schritten werden die Schritte zum Konfigurieren der Configuration Manager-Integration in Log Analytics zusammengefasst.  

1. Registrieren Sie im Azure-Portal den Configuration Manager als Webanwendung und/oder Web API-App, und stellen Sie sicher, dass Sie die Client-ID und den geheimen Clientschlüssel aus der Registrierung von Azure Active Directory haben. Weitere Informationen zum Durchführen dieses Schritts finden Sie unter [Erstellen einer Active Directory-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals](../active-directory/develop/howto-create-service-principal-portal.md).
2. Im Azure-Portal [gewähren Sie Configuration Manager (der registrierten Web-App) die Zugriffsberechtigung für Log Analytics](#grant-configuration-manager-with-permissions-to-log-analytics).
3. Im Configuration Manager [fügen Sie eine Verbindung mit dem Assistenten zum Hinzufügen von OMS-Verbindungen hinzu](#add-an-oms-connection-to-configuration-manager).
4. Im Configuration Manager [aktualisieren Sie die Verbindungseigenschaften](#update-oms-connection-properties), wenn das Kennwort oder der geheime Clientschlüssel abläuft oder verloren geht.
5. [Laden und installieren Sie den Microsoft Monitoring Agent](#download-and-install-the-agent) auf dem Computer, auf dem die Standortsystemrolle des Configuration Manager-Dienstverbindungspunkts ausgeführt wird. Der Agent sendet die Configuration Manager-Daten zum Log Analytics-Arbeitsbereich.
6. In Log Analytics [importieren Sie Sammlungen von Configuration Manager](#import-collections) als Computergruppen.
7. In Log Analytics zeigen Sie die Daten von Configuration Manager als [Computergruppen](log-analytics-computer-groups.md) an.

Weitere Informationen zum Herstellen einer Verbindung zwischen Configuration Manager und Log Analytics finden Sie unter [Synchronisieren von Daten aus Configuration Manager mit Azure Log Analytics](https://technet.microsoft.com/library/mt757374.aspx).

## <a name="grant-configuration-manager-with-permissions-to-log-analytics"></a>Bereitstellen von Configuration Manager mit Berechtigungen für Log Analytics
Im Folgenden weisen Sie in Ihrem Log Analytics-Arbeitsbereich dem AD-Anwendungs- und Dienstprinzipal, den Sie zuvor für den Configuration Manager erstellt haben, die Rolle *Mitwirkender* zu.  Wenn Sie noch über keinen Arbeitsbereich verfügen, lesen Sie zunächst [Erstellen eines Log Analytics-Arbeitsbereichs im Azure-Portal](log-analytics-quick-create-workspace.md), bevor Sie fortfahren.  Auf diese Weise kann Configuration Manager Ihren Log Analytics-Arbeitsbereich authentifizieren und darauf zugreifen.  

> [!NOTE]
> Sie müssen in Log Analytics Berechtigungen für Configuration Manager festlegen. Andernfalls erhalten Sie eine Fehlermeldung, wenn Sie den Konfigurations-Assistenten in Configuration Manager verwenden.
>

1. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**. Geben Sie in der Liste mit den Ressourcen **Log Analytics** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Log Analytics**.<br><br> ![Azure-Portal](media/log-analytics-sccm/azure-portal-01.png)<br><br>  
2. Wählen Sie in der Liste mit den Log Analytics-Arbeitsbereichen den zu ändernden Arbeitsbereich aus.
3. Klicken Sie im linken Bereich auf **Zugriffssteuerung (IAM)**.
4. Klicken Sie auf der Seite der Zugriffssteuerung auf **Hinzufügen**. Der Bereich **Berechtigungen hinzufügen** wird angezeigt.
5. Wählen Sie im Bereich **Berechtigungen hinzufügen** in der Dropdownliste **Rolle** die Rolle **Mitwirkender** aus.  
6. Wählen Sie unter der Dropdownliste **Zugriff zuweisen zu** die zuvor in AD erstellte Configuration Manager-Anwendung, und klicken Sie dann auf **OK**.  

## <a name="download-and-install-the-agent"></a>Herunterladen und Installieren des Agents
Lesen Sie den Artikel [Verbinden von Windows-Computern mit dem Log Analytics-Dienst in Azure](../azure-monitor/platform/agent-windows.md), um sich über die verfügbaren Methoden zur Installation des Microsoft Monitoring Agents auf dem Computer zu informieren, auf dem sich die Standortsystemrolle des Configuration Manager-Dienstverbindungspunkts befindet.  

## <a name="add-a-log-analytics-connection-to-configuration-manager"></a>Hinzufügen einer Log Analytics-Verbindung zu Configuration Manager
Zum Hinzufügen einer Log Analytics-Verbindung muss für die Configuration Manager-Umgebung ein [Dienstverbindungspunkt](https://technet.microsoft.com/library/mt627781.aspx) für den Onlinemodus konfiguriert sein.

1. Wählen Sie im Arbeitsbereich **Verwaltung** des Configuration Manager **OMS-Connector**. Daraufhin wird der **Assistent zum Hinzufügen einer Log Analytics-Verbindung** geöffnet. Klicken Sie auf **Weiter**.

   >[!NOTE]
   >OMS wird jetzt als Log Analytics bezeichnet.
   
2. Auf dem Bildschirm **Allgemein** vergewissern Sie sich, dass Sie die folgenden Aktionen durchgeführt haben und dass Sie für jedes Element Detailinformationen haben, und wählen Sie anschließend **Weiter** aus.

   1. Im Azure-Portal haben Sie Configuration Manager als Webanwendung und/oder Web API-App registriert, und Sie haben die [Client-ID von der Registrierung](../active-directory/develop/quickstart-v1-add-azure-ad-app.md) vorliegen.
   2. Im Azure-Portal haben Sie einen geheimen App-Schlüssel für die registrierte Anwendung in Azure Active Directory erstellt.  
   3. Im Azure-Portal haben Sie der registrierten Web-App eine Zugriffsberechtigung für Log Analytics erteilt.  
      ![Verbindung mit Log Analytics: Seite „Allgemein“ des Assistenten](./media/log-analytics-sccm/sccm-console-general01.png)
3. Auf dem Bildschirm **Azure Active Directory** konfigurieren Sie Ihre Verbindungseinstellungen zu Log Analytics durch Angabe von **Mandant**, **Client-ID**, und des **geheimen Clientschlüssels**, und wählen Sie anschließend **Weiter** aus.  
   ![Verbindung mit Log Analytics: Seite „Azure Active Directory“ des Assistenten](./media/log-analytics-sccm/sccm-wizard-tenant-filled03.png)
4. Wenn Sie alle anderen Verfahren erfolgreich abgeschlossen haben, erscheinen die Informationen auf dem Bildschirm **OMS-Verbindungskonfiguration** automatisch auf dieser Seite. Informationen für die Verbindungseinstellungen sollten für **Azure-Abonnement**, **Azure-Ressourcengruppe**, und **Operations Management Suite-Arbeitsbereich** angezeigt werden.  
   ![Verbindung mit Log Analytics: Seite „Log Analytics-Verbindung“ des Assistenten](./media/log-analytics-sccm/sccm-wizard-configure04.png)
5. Der Assistent stellt eine Verbindung mit dem Log Analytics-Dienst mithilfe der Informationen her, die Sie eingegeben haben. Wählen Sie die Gerätesammlungen, die Sie zur Synchronisierung mit dem Dienst verwenden möchten, und klicken Sie anschließend auf **Hinzufügen**.  
   ![Sammlungen auswählen](./media/log-analytics-sccm/sccm-wizard-add-collections05.png)
6. Überprüfen Sie die Verbindungseinstellungen auf dem Bildschirm **Zusammenfassung**, und wählen Sie **Weiter** aus. Der Bildschirm **Status** zeigt den Verbindungsstatus, sollte **vollständig** sein.

> [!NOTE]
> Sie müssen Log Analytics mit der obersten Ebene in der Hierarchie verbinden. Wenn Sie einen eigenständigen primären Standort mit Log Analytics verbinden und dann eine zentrale Verwaltung der Umgebung hinzufügen, müssen Sie die Verbindung innerhalb der neuen Hierarchie löschen und neu erstellen.
>
>

Nachdem Sie Configuration Manager mit Log Analytics verknüpft haben, können Sie Sammlungen hinzufügen oder entfernen und die Eigenschaften der Verbindung anzeigen.

## <a name="update-log-analytics-connection-properties"></a>Aktualisieren von Log Analytics-Verbindungseigenschaften
Wenn das Kennwort oder der geheime Clientschlüssel abläuft oder verloren geht, müssen Sie die Log Analytics-Verbindungseigenschaften manuell aktualisieren.

1. Navigieren Sie im Configuration Manager zu **Clouddienste**, und wählen Sie dann **OMS-Connector** zum Öffnen der Seite **OMS-Verbindungseigenschaften** aus.
2. Klicken Sie auf dieser Seite auf die Registerkarte **Azure Active Directory** zur Anzeige von **Mandant**, **Client-ID**, **Ablauf des geheimen Clientschlüssels**. **Überprüfen Sie** Ihren **geheimen Clientschlüssel**, falls er abgelaufen ist.

## <a name="import-collections"></a>Importieren von Sammlungen
Nachdem Sie eine Log Analytics-Verbindung mit Configuration Manager hinzugefügt und den Agent auf dem Computer mit der Standortsystemrolle des Configuration Manager-Dienstverbindungspunkts installiert haben, müssen als Nächstes Sammlungen aus Configuration Manager als Computergruppen in Log Analytics importiert werden.

Nachdem Sie die Erstkonfiguration für den Import von Gerätesammlungen aus Ihrer Hierarchie abgeschlossen haben, werden alle drei Stunden die Mitgliedschaftsinformationen zur Sammlung abgerufen, um die Mitgliedschaft aktuell zu halten. Sie können diese Einstellung jederzeit deaktivieren.

1. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**. Geben Sie in der Liste mit den Ressourcen **Log Analytics** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Log Analytics**.
2. Wählen Sie in der Liste der Log Analytics-Arbeitsbereiche den Arbeitsbereich aus, in dem Configuration Manager registriert ist.  
3. Wählen Sie **Erweiterte Einstellungen**.<br><br> ![Erweiterte Einstellungen für Log Analytics](media/log-analytics-sccm/log-analytics-advanced-settings-01.png)<br><br>  
4. Wählen Sie hierzu **Computergruppen** und anschließend **SCCM** aus.  
5. Wählen Sie **Configuration Manager-Sammlungsmitgliedschaften importieren**aus, und klicken Sie anschließend auf **Speichern**.  
   ![Computergruppen – Registerkarte „SCCM“](./media/log-analytics-sccm/sccm-computer-groups01.png)

## <a name="view-data-from-configuration-manager"></a>Anzeigen von Daten aus Configuration Manager
Nachdem Sie eine Log Analytics-Verbindung mit Configuration Manager hinzugefügt und den Agent auf dem Computer mit der Standortsystemrolle des Configuration Manager-Dienstverbindungspunkts installiert haben, werden Daten vom Agent an Log Analytics gesendet. In Log Analytics werden die Configuration Manager-Sammlungen als [Computergruppen](log-analytics-computer-groups.md) angezeigt. Die Anzeige von Gruppen wählen Sie auf der Seite **Configuration Manager** unter **Einstellungen\Computergruppen** aus.

Nach dem Import der Sammlungen können Sie sehen, wie viele Computer mit Sammlungsmitgliedschaften erkannt wurden. Sie sehen auch die Anzahl der Sammlungen, die importiert wurden.

![Computergruppen – Registerkarte „SCCM“](./media/log-analytics-sccm/sccm-computer-groups02.png)

Wenn Sie auf eine klicken, wird die Suche geöffnet, wobei entweder alle importierten Gruppen oder alle Computer angezeigt werden, die zu jeder Gruppe gehören. Mithilfe der [Protokollsuche](log-analytics-queries.md) können Sie detaillierte Analysen für Configuration Manager-Daten starten.

## <a name="next-steps"></a>Nächste Schritte
* Verwenden Sie [Protokollsuche](log-analytics-queries.md) für ausführliche Informationen zu den Configuration Manager-Daten.
