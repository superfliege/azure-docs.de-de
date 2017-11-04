---
title: Sammeln von Daten von lokalen Linux-Computern mit Azure Log Analytics | Microsoft-Dokumentation
description: "Erfahren Sie, wie der Log Analytics-Agent für Linux bereitgestellt wird, und aktivieren Sie die Sammlung von Daten von diesem Betriebssystem mit Log Analytics."
services: log-analytics
documentationcenter: log-analytics
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/13/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: d22fe6456c3bd886f8f8863d362c0084fbe03da3
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2017
---
# <a name="collect-data-from-linux-computers-hosted-in-your-environment"></a>Sammeln von Daten von Linux-Computern, die in Ihrer Umgebung gehostet werden
[Azure Log Analytics](log-analytics-overview.md) kann Daten direkt von Ihren physischen oder virtuellen Linux-Computern und anderen Ressourcen in Ihrer Umgebung zur detaillierten Analyse und Korrelation in einem einzelnen Repository sammeln.  Dieser Schnellstart zeigt Ihnen, wie Sie in wenigen einfachen Schritten Daten von Ihrem Linux-Computer konfigurieren und sammeln.  Informationen zu virtuellen Azure Linux-Computern finden Sie im folgenden Thema [Sammeln von Daten zu Azure Virtual Machines](log-analytics-quick-collect-azurevm.md).  
 
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="log-in-to-azure-portal"></a>Anmelden beim Azure-Portal
Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) im Azure-Portal an. 

## <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs
1. Klicken Sie im Azure-Portal unten links auf **Weitere Dienste**. Geben Sie in der Liste mit den Ressourcen **Log Analytics** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Log Analytics**.<br><br> ![Azure-Portal](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
2. Klicken Sie auf **Erstellen**, und wählen Sie anschließend Optionen für die folgenden Elemente aus:

  * Geben Sie einen Namen für den neuen **OMS-Arbeitsbereich** ein, wie z.B. *DefaultLAWorkspace*. 
  * Wählen Sie ein **Abonnement** aus, mit dem eine Verknüpfung erstellt werden soll, indem Sie in der Dropdownliste einen anderen Eintrag auswählen, falls der Standardeintrag nicht geeignet ist.
  * Wählen Sie für **Ressourcengruppe** eine vorhandene Ressourcengruppe aus, die einen oder mehrere virtuelle Azure-Computer enthält.  
  * Wählen Sie den **Speicherort** für die Bereitstellung Ihrer virtuellen Computer aus.  Weitere Informationen finden Sie auf der Seite zur [Verfügbarkeit von Log Analytics in den einzelnen Regionen](https://azure.microsoft.com/regions/services/).
  * In Log Analytics können Sie zwischen drei verschiedenen **Tarifen** wählen, aber wählen Sie für diesen Schnellstart den **kostenlosen** Tarif.  Weitere Informationen zu den einzelnen Tarifen finden Sie unter [Log Analytics – Preise](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Create Log Analytics resource blade](./media/log-analytics-quick-collect-azurevm/create-loganalytics-workspace-01.png)<br>  
3. Klicken Sie nach dem Bereitstellen der erforderlichen Informationen im Bereich **OMS-Arbeitsbereich** auf **OK**.  

Die Informationen werden überprüft, und der Arbeitsbereich wird erstellt. Sie können den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen. 

## <a name="obtain-workspace-id-and-key"></a>Abrufen von Arbeitsbereichs-ID und -Schlüssel
Vor der Installation des OMS-Agents für Linux benötigen Sie die Arbeitsbereichs-ID und den Schlüssel für Ihren Log Analytics-Arbeitsbereich.  Diese Informationen sind für das Wrapperskript des Agents erforderlich, um den Agent ordnungsgemäß zu konfigurieren und sicherzustellen, dass er erfolgreich mit Log Analytics kommunizieren kann.  

1. Klicken Sie im Azure-Portal unten links auf **Weitere Dienste**. Geben Sie in der Liste mit den Ressourcen **Log Analytics** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Log Analytics**.
2. Wählen Sie in der Liste der Log Analytics-Arbeitsbereiche den zuvor erstellten *DefaultLAWorkspace*.
3. Wählen Sie **Erweiterte Einstellungen**.<br><br> ![Erweiterte Einstellungen für Log Analytics](media/log-analytics-quick-collect-azurevm/log-analytics-advanced-settings-01.png)<br><br>  
4. Wählen Sie **Verbundene Quellen** und dann **Linux Server** aus.   
5. Der Wert rechts von **Arbeitsbereichs-ID** und **Primärschlüssel**. Kopieren Sie beide Angaben, und fügen Sie sie in den von Ihnen bevorzugten Editor ein.   

## <a name="install-the-agent-for-linux"></a>Installieren des Agents für Linux
Die folgenden Schritte konfigurieren das Setup des Agents für Log Analytics in Azure und Azure Government Cloud.  

1. Führen Sie den folgenden Befehl aus, und stellen Sie die zuvor kopierte Arbeitsbereichs-ID sowie den Primärschlüssel bereit, um den Linux-Computer so zu konfigurieren, dass er eine Verbindung mit Log Analytics herstellt.  Mit diesem Befehl wird der Agent heruntergeladen, die Prüfsumme des Agents überprüft und der Agent dann installiert. 
    
    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY>
    ```

2. Führen Sie den folgenden Befehl aus, und stellen Sie die zuvor kopierte Arbeitsbereichs-ID sowie den Primärschlüssel bereit, um den Linux-Computer so zu konfigurieren, dass er eine Verbindung mit Log Analytics in Azure Government Cloud herstellt.  Mit diesem Befehl wird der Agent heruntergeladen, die Prüfsumme des Agents überprüft und der Agent dann installiert. 

    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY> -d opinsights.azure.us
    ``` 

## <a name="configure-agent-to-communicate-with-a-proxy-server"></a>Konfigurieren des Agents für die Kommunikation mit einem Proxyserver

Führen Sie die folgenden Schritte aus, wenn Ihre Linux-Computer über einen Proxyserver mit Log Analytics kommunizieren müssen.  Der Wert für die Proxykonfiguration weist die folgende Syntax auf: `[protocol://][user:password@]proxyhost[:port]`.

1. Bearbeiten Sie die Datei `/etc/opt/microsoft/omsagent/proxy.conf`, indem Sie die folgenden Befehle ausführen. Ändern Sie die Werte, damit Sie Ihren spezifischen Einstellungen entsprechen.

    ```
    proxyconf="https://proxyuser:proxypassword@proxyserver01:30443"
    sudo echo $proxyconf >>/etc/opt/microsoft/omsagent/proxy.conf
    sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf 
    ```

2. Starten Sie den Agent neu, indem Sie den folgenden Befehl ausführen: 

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
    ``` 

## <a name="collect-event-and-performance-data"></a>Sammeln von Ereignis- und Leistungsdaten
Log Analytics kann Ereignisdaten aus dem Linux-Syslog und Leistungsindikatoren sammeln, die Sie für längerfristige Analysen und Berichte angeben, und Maßnahmen einleiten, wenn eine bestimmte Bedingung erkannt wird.  Führen Sie diese Schritte aus, um die Sammlung von Ereignissen aus dem Linux-Syslog sowie mehreren allgemeinen Leistungsindikatoren zu konfigurieren.  

1. Wählen Sie **Syslog**.  
2. Sie können ein Ereignisprotokoll hinzufügen, indem Sie den Namen des Protokolls eingeben.  Geben Sie **Syslog** ein, und klicken Sie dann auf das Pluszeichen **+**.  
3. Deaktivieren Sie in der Tabelle die Schweregrade **Info**, **Hinweis** und **Debuggen**. 
4. Klicken Sie ganz oben auf der Seite auf **Speichern**, um die Konfiguration zu speichern.
5. Wählen Sie **Linux-Leistungsdaten**, um die Sammlung von Leistungsindikatoren auf einem Windows-Computer zu aktivieren. 
6. Wenn Sie die Linux-Leistungsindikatoren zum ersten Mal für einen neuen Log Analytics-Arbeitsbereich konfigurieren, haben Sie die Möglichkeit, schnell mehrere allgemeine Indikatoren zu erstellen. Diese werden in einer Liste aufgeführt, und neben jedem Indikator finden Sie ein Kontrollkästchen.<br><br> ![Standardmäßige Windows-Leistungsindikatoren ausgewählt](media/log-analytics-quick-collect-azurevm/linux-perfcounters-default.png).<br><br> Klicken Sie auf **Ausgewählte Leistungsindikatoren hinzufügen**.  Sie werden hinzugefügt und mit einem Stichprobenintervall von zehn Sekunden voreingestellt.  
7. Klicken Sie ganz oben auf der Seite auf **Speichern**, um die Konfiguration zu speichern.

## <a name="view-data-collected"></a>Anzeigen gesammelter Daten
Jetzt haben Sie die Datensammlung aktiviert und können ein einfaches Protokollsuchebeispiel ausführen, um einige Daten vom Zielcomputer anzuzeigen.  

1. Navigieren Sie im Azure-Portal zu Log Analytics, und wählen Sie den zuvor erstellten Arbeitsbereich aus.
2. Klicken Sie auf die Kachel **Protokollsuche** und im Bereich „Protokollsuche“ auf den Abfragefeldtyp `Perf`, und drücken Sie dann auf EINGABE, oder klicken Sie auf die Suchschaltfläche rechts neben dem Abfragefeld.<br><br> ![Beispiel für Log Analytics-Protokollsucheabfrage](media/log-analytics-quick-collect-linux-computer/log-analytics-portal-queryexample.png)<br><br> Die Abfrage in der folgenden Abbildung gibt z. B. 735 Leistungsdatensätze zurück.<br><br> ![Log Analytics-Protokollsucheergebnis](media/log-analytics-quick-collect-linux-computer/log-analytics-search-perf.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn er nicht mehr benötigt wird, können Sie den Agent vom Linux-Computer entfernen und den Log Analytics-Arbeitsbereich löschen.  

Führen Sie die folgenden Schritte aus, um den Agent zu entfernen.

1. Laden Sie das [universelle Skript](https://github.com/Microsoft/OMS-Agent-for-Linux/releases) des Linux-Agents auf den Computer herunter.
2. Führen Sie die SH-Bündeldatei mit dem Argument *--purge* auf dem Computer aus, um den Agent und dessen Konfiguration vollständig zu entfernen.

    `sudo sh ./omsagent-<version>.universal.x64.sh --purge`

Wählen Sie zum Löschen des Arbeitsbereichs den Log Analytics-Arbeitsbereich aus, den Sie zuvor erstellt haben, und klicken Sie auf der Ressourcenseite auf **Löschen**.<br><br> ![Löschen von Log Analytics-Ressourcen](media/log-analytics-quick-collect-azurevm/log-analytics-portal-delete-resource.png)

## <a name="next-steps"></a>Nächste Schritte
Jetzt sammeln Sie Betriebs- und Leistungsdaten von Ihrem lokalen Linux-Computer und können problemlos beginnen, *kostenlos* Daten zu untersuchen, zu analysieren und Aktionen an ihnen vorzunehmen.  

Um zu erfahren, wie Sie die Daten anzeigen und analysieren, fahren Sie mit dem Tutorial fort.   

> [!div class="nextstepaction"]
> [Anzeigen oder Analysieren der Daten in Log Analytics](log-analytics-tutorial-viewdata.md)
