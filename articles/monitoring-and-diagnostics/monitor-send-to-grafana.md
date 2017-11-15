---
title: "Überwachen von Azure-Diensten und -Anwendungen mit Grafana | Microsoft-Dokumentation"
description: Es wird beschrieben, wie Sie Azure Monitor- und Application Insights-Daten weiterleiten, um sie in Grafana anzuzeigen.
services: monitoring-and-diagnostics
keywords: 
author: rboucher
ms.author: robb
ms.date: 11/06/2017
ms.topic: article
ms.service: monitoring-and-diagnostics
ms.openlocfilehash: 709a98f8bcdb75962f8e41de348ca7a41c677610
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2017
---
# <a name="monitor-your-azure-services-in-grafana"></a>Überwachen Ihrer Azure-Dienste in Grafana
Sie können Azure-Dienste und -Anwendungen jetzt auch mit [Grafana](https://grafana.com/) überwachen, indem Sie das [Azure Monitor-Datenquellen-Plug-In](https://grafana.com/plugins/grafana-azure-monitor-datasource) verwenden. Mit dem Plug-In werden Daten zur Anwendungsleistung, die mit dem Application Insights SDK gesammelt werden, sowie von Azure Monitor bereitgestellte Infrastrukturdaten erfasst. Sie können diese Daten dann in Ihrem Grafana-Dashboard anzeigen.

Das Plug-In befindet sich derzeit in der Vorschauphase.

Führen Sie die folgenden Schritte aus, um einen Grafana-Server über Azure Marketplace einzurichten und Dashboards für Metriken aus Azure Monitor und Application Insights zu erstellen.

## <a name="set-up-a-grafana-instance"></a>Einrichten einer Grafana-Instanz
1. Navigieren Sie zum Azure Marketplace, und wählen Sie Grafana von Grafana Labs aus.

2. Geben Sie Namen und Details an. Erstellen Sie eine neue Ressourcengruppe. Bewahren Sie die Informationen auf, die Sie für den VM-Benutzernamen, das VM-Kennwort und das Grafana-Serveradministratorkennwort angeben.  

3. Wählen Sie die VM-Größe und ein Speicherkonto aus.

4. Konfigurieren Sie die Einstellungen für die Netzwerkkonfiguration.

5. Zeigen Sie die Zusammenfassung an, und wählen Sie **Erstellen**, nachdem Sie die Nutzungsbedingungen akzeptiert haben.

## <a name="log-in-to-grafana"></a>Anmelden an Grafana
1. Wählen Sie nach Abschluss der Bereitstellung die Option **Zu Ressourcengruppe wechseln**. Eine Liste mit den neu erstellten Ressourcen wird angezeigt. 

    ![Grafana-Ressourcengruppenobjekte](.\media\monitor-how-to-grafana\grafana1.png) 

    Wenn Sie die Netzwerksicherheitsgruppe auswählen (hier *grafana-nsg*), sehen Sie, dass Port 3000 zum Zugreifen auf den Grafana-Server verwendet wird. 

2. Wechseln Sie zurück zur Liste mit den Ressourcen, und wählen Sie **Öffentliche IP-Adresse**. Verwenden Sie die Werte in diesem Fenster, und geben Sie *http://<IP address>:3000* oder *<DNSName>:3000* in Ihrem Browser ein. Eine Anmeldeseite für den zuvor erstellten Grafana-Server wird angezeigt.
    
    ![Grafana-Anmeldebildschirm](.\media\monitor-how-to-grafana\grafana2.png) 

3. Melden Sie sich mit dem Benutzernamen *admin* und dem zuvor erstellten Grafana-Serveradministratorkennwort an. 

## <a name="configure-data-source-plugin"></a>Konfigurieren des Datenquellen-Plug-Ins

Nach der erfolgreichen Anmeldung sollte zu sehen sein, dass das Azure Monitor-Datenquellen-Plug-In bereits vorhanden ist.

![Anzeige des Azure Monitor-Plug-Ins in Grafana](.\media\monitor-how-to-grafana\grafana3.png) 

1. Wählen Sie die Option **Datenquelle hinzufügen**, um Azure Monitor und Application Insights zu konfigurieren. 
    
2. Wählen Sie einen Namen für die Datenquelle aus, und wählen Sie in der Dropdownliste **Azure Monitor** als Datenquelle aus.
    
    
## <a name="create-a-service-principal"></a>Erstellen eines Dienstprinzipals 

Grafana nutzt einen Azure Active Directory-Dienstprinzipal zum Herstellen einer Verbindung mit Azure Monitor-APIs und Sammeln von Metrikdaten. Sie müssen einen Dienstprinzipal erstellen, um den Zugriff auf Ihre Azure-Ressourcen zu verwalten.

1. Informationen zum Erstellen eines Dienstprinzipals finden Sie in [dieser Anleitung](../azure-resource-manager/resource-group-create-service-principal-portal.md). Kopieren Sie Ihre Mandanten-ID, Client-ID und einen geheimen Clientschlüssel.

2. Informationen zum Zuweisen der Leserrolle zur Azure Active Directory-Anwendung finden Sie unter [Zuweisen einer Anwendung zur Rolle](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal#assign-application-to-role).   

3. Bei Verwendung von Application Insights können Sie auch Ihre Application Insights-API und die Anwendungs-ID einbinden, um Application Insights-basierte Metriken zu sammeln. Weitere Informationen finden Sie unter [Getting your API key and Application ID](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID) (Abrufen Ihres API-Schlüssels und der Anwendungs-ID).

4. Nachdem Sie alle diese Informationen eingegeben haben, können Sie **Speichern** wählen. Die API wird dann von Grafana getestet. Es wird in etwa folgende Meldung angezeigt:  

    ![Anzeige des Azure Monitor-Plug-Ins in Grafana](.\media\monitor-how-to-grafana\grafana4.png) 
    
## <a name="build-a-grafana-dashboard"></a>Erstellen eines Grafana-Dashboards

1. Navigieren Sie zur Startseite, und wählen Sie **Neues Dashboard**.

2. Wählen Sie im neuen Dashboard die Option **Graph**. Sie können auch andere Diagrammoptionen ausprobieren, aber in diesem Artikel wird *Graph* als Beispiel verwendet. 

    ![Grafana: Neues Dashboard](.\media\monitor-how-to-grafana\grafana5.png) 

3. Im Dashboard wird ein leeres Diagramm angezeigt. 

4. Klicken Sie auf den Bereichstitel, und wählen Sie **Bearbeiten**, um die Details der Daten einzugeben, die Sie in diesem Graphdiagramm darstellen möchten.
    
5. Nachdem Sie alle richtigen VMs ausgewählt haben, können Sie damit beginnen, die Metriken im Dashboard anzuzeigen. 

Unten ist ein einfaches Dashboard mit zwei Diagrammen angegeben. Im linken Diagramm wird der CPU-Prozentsatz der beiden VMs angezeigt. Im rechten Diagramm werden die Transaktionen in einem Azure Storage-Konto unterteilt nach Transaktions-API-Typ angezeigt.
    
![Grafana-Beispiel mit zwei Diagrammen](.\media\monitor-how-to-grafana\grafana6.png) 
    

## <a name="optional-create-dashboard-playlists"></a>Optional: Erstellen von Dashboard-Wiedergabelisten

Eines der vielen nützlichen Features von Grafana ist die Dashboard-Wiedergabeliste. Sie können mehrere Dashboards erstellen und einer Wiedergabeliste hinzufügen und dann ein Intervall für die Anzeige jedes Dashboards konfigurieren. Wählen Sie **Play** (Wiedergabe), um den Durchlauf der Dashboards zu verfolgen. Sie können sie beispielsweise auf einem großen Monitor an der Wand als „Statusübersicht“ für Ihre Gruppe anzeigen. 
    
![Grafana-Beispiel für eine Wiedergabeliste](.\media\monitor-how-to-grafana\grafana7.png) 


## <a name="optional-monitor-your-custom-metrics-in-the-same-grafana-server"></a>Optional: Überwachen Ihrer benutzerdefinierten Metriken auf demselben Grafana-Server

Sie können auch Telegraf und InfluxDB installieren, um sowohl benutzerdefinierte als auch Agent-basierte Metriken unter derselben Grafana-Instanz zu sammeln und auszugeben. Es gibt viele Datenquellen-Plug-Ins, die Sie verwenden können, um diese Metriken in einem Dashboard zusammenzufassen. 
    
Sie können diese Einrichtung auch wiederverwenden, um Metriken von Ihrem Prometheus-Server einzubeziehen. Verwenden Sie das Prometheus-Datenquellen-Plug-In im Plug-In-Katalog von Grafana.
    
Hier sind einige gute Referenzartikel zur Verwendung von Telegraf, InfluxDB, Prometheus und Docker angegeben:
 - [How To Monitor System Metrics with the TICK Stack on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-monitor-system-metrics-with-the-tick-stack-on-ubuntu-16-04) (Überwachen von Systemmetriken per TICK-Stapel unter Ubuntu 16.04)

 - [Monitor Docker resource metrics with Grafana, InfluxDB, and Telegraf](https://blog.vpetkov.net/2016/08/04/monitor-docker-resource-metrics-with-grafana-influxdb-and-telegraf/) (Überwachen von Docker-Ressourcenmetriken mit Grafana, InfluxDB und Telegraf)

 - [A monitoring solution for Docker hosts, containers, and containerized services](https://stefanprodan.com/2016/a-monitoring-solution-for-docker-hosts-containers-and-containerized-services/) (Eine Überwachungslösung für Docker-Hosts, -Container und -Dienste in Containern)

Hier ist eine Abbildung mit einem vollständigen Grafana-Dashboard angegeben, das Metriken aus Azure Monitor und Application Insights enthält.
![Grafana-Beispiel für Metriken](.\media\monitor-how-to-grafana\grafana8.png) 


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Ihnen werden für ausgeführte VMs unabhängig davon, ob Sie sie nutzen oder nicht, Gebühren berechnet. Bereinigen Sie die in diesem Artikel erstellten Ressourcen, um zu verhindern, dass zusätzliche Gebühren anfallen. 

1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Ressourcengruppen** und dann auf **Grafana**. 
2. Klicken Sie auf der Seite mit Ihrer Ressourcengruppe auf **Löschen**, geben Sie im Textfeld **Grafana** ein, und klicken Sie dann auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte
* [Überblick über Metriken in Microsoft Azure](monitoring-overview-metrics.md)


