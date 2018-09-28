---
title: Erfassen von benutzerdefinierten Metriken für einen virtuellen Linux-Computer mit dem InfluxData Telegraf Agent
description: Erfassen von benutzerdefinierten Metriken für einen virtuellen Linux-Computer mit dem InfluxData Telegraf Agent
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: howto
ms.date: 09/24/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: 651706b269cf24eca85e0601384ef63cb6ed06a2
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990704"
---
# <a name="collect-custom-metrics-for-a-linux-vm-with-the-influxdata-telegraf-agent"></a>Erfassen von benutzerdefinierten Metriken für einen virtuellen Linux-Computer mit dem InfluxData Telegraf Agent

Mit Azure Monitor können Sie benutzerdefinierte Metriken über Ihre Anwendungstelemetrie, einen auf Ihren Azure-Ressourcen ausgeführten Agent, oder sogar über ein außerhalb des Unternehmens befindliches Überwachungssystem gesammelt und direkt an Azure Monitor übermittelt werden. Dieser Artikel konzentriert sich auf die Bereitstellung von Anweisungen zur Bereitstellung des [InfluxData](https://www.influxdata.com/) Telegraf Agent auf einem virtuellen Linux-Computer in Azure und die Konfiguration des Agent zur Veröffentlichung von Metriken in Azure Monitor. 

## <a name="influxdata-telegraf-agent"></a>InfluxData Telegraf Agent 

[Telegraf](https://docs.influxdata.com/telegraf/v1.7/) ist ein Plug-In-gesteuerter Agent, der die Erfassung von Metriken aus über 150 verschiedenen Quellen ermöglicht. Abhängig davon, welche Workloads auf Ihrer VM ausgeführt werden (z. B. MySQL, NGINX, Apache usw.), können Sie den Agent konfigurieren, um spezielle Eingabe-Plug-Ins zum Erfassen von Metriken zu nutzen. Ausgabe-Plug-Ins ermöglichen es dem Agent anschließend, in Ziele Ihrer Wahl zu schreiben. Der Telegraf-Agent ist direkt in die benutzerdefinierte REST-API von Azure Monitor integriert und unterstützt ein „Azure Monitor-Ausgabe-Plug-In“. Auf diese Weise kann der Agent Workload-spezifische Metriken auf Ihrem virtuellen Linux-Computer sammeln und als benutzerdefinierte Metriken an Azure Monitor übermitteln. 

 ![Übersicht über den Telegraf-Agent](./media/metrics-store-custom-linux-telegraf/telegraf-agent-overview.png)

## <a name="send-custom-metrics"></a>Senden benutzerdefinierter Metriken 

Im Rahmen dieses Tutorials wird ein virtueller Linux-Computer mit dem Betriebssystem Ubuntu 16.04 LTS bereitgestellt. Der Telegraf-Agent wird für die meisten Linux-Betriebssysteme unterstützt. Sowohl Debian- als auch RPM-Pakete sind zusammen mit nicht gepackten Linux-Binärdateien über das InfluxData-Downloadportal verfügbar. Weitere Informationen zu Installationsanweisungen und -optionen für Telegraf finden Sie in dieser Installationsanleitung. 

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

So erstellen Sie einen neuen virtuellen Linux-Computer 

1. Klicken Sie im linken Navigationsbereich auf die Option  **Ressource erstellen** . 
1. Suchen Sie nach *Virtueller Computer*.  
1. Wählen Sie *Ubuntu 16.04 LTS* aus, und klicken Sie auf **Erstellen**. 
1. Geben Sie einen VM-Namen an, z. B.  *MyTelegrafVM*.  
1. Übernehmen Sie den Datenträgertyp **SSD**, und geben Sie einen **Benutzernamen** (z. B.  *azureuser*) an. 
1. Wählen Sie für  *Authentifizierungstyp* die Option  **Kennwort** aus, und geben Sie dann ein Kennwort ein, das Sie später für SSH mit dieser VM verwenden werden. 
1. Wählen Sie  **Neue Ressourcengruppe erstellen** aus, und geben Sie dann einen Namen an, z. B. *myResourceGroup*.  Wählen Sie den gewünschten Standort, und klicken Sie dann auf  **OK**. 

     ![Erstellen eines virtuellen Ubuntu-Computers](./media/metrics-store-custom-linux-telegraf/create-vm.png)

1. Wählen Sie eine Größe für den virtuellen Computer. Sie können z. B. nach „Computetyp“ oder „Datenträgertyp“ filtern. 

     ![Größe des virtuellen Computers für den Telegraf-Agent – Übersicht](./media/metrics-store-custom-linux-telegraf/vm-size.png)

1. Wählen Sie auf der Seite  **Einstellungen** unter  **Netzwerk** > **Netzwerksicherheitsgruppe** > **Öffentliche Eingangsports hinzufügen** die Optionen  *HTTP*  und  *SSH (22)* aus. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie  **OK** aus. 

1. Wählen Sie auf der Seite „Zusammenfassung“ die Option „Erstellen“ aus, um die Bereitstellung des virtuellen Computers zu starten. 

1. Der virtuelle Computer wird auf dem Dashboard des Azure-Portals angeheftet. Nach Abschluss der Bereitstellung wird automatisch die VM-Zusammenfassung geöffnet. 

1. Navigieren Sie auf dem Blatt „VM“ zur Registerkarte **Identität** und stellen Sie sicher, dass Ihr virtueller Computer über eine vom System zugewiesene Identität( *Ein*) verfügt. 
 
![Füllung](./media/metrics-store-custom-linux-telegraf/connect-to-VM.png)
 
## <a name="connect-to-the-vm"></a>Herstellen der Verbindung zur VM 

Stellen Sie eine SSH-Verbindung mit dem virtuellen Computer her. Klicken Sie auf der Seite „Übersicht“ des virtuellen Computers auf die Schaltfläche „Verbinden“. 

![Füllung](./media/metrics-store-custom-linux-telegraf/connect-VM-button2.png)

Übernehmen Sie auf der Seite zum Herstellen der Verbindung mit dem virtuellen Computer die Standardoptionen, um basierend auf dem DNS-Namen eine Verbindung über Port 22 herzustellen. Unter „Login using VM local account“ (Anmelden mit einem lokalen VM-Konto) wird ein Verbindungsbefehl angezeigt. Klicken Sie auf die Schaltfläche, um den Befehl zu kopieren. Das folgende Beispiel zeigt den Befehl für die SSH-Verbindung: 

```cmd
ssh azureuser@XXXX.XX.XXX 
```

Fügen Sie den Befehl für die SSH-Verbindung in eine Shell wie Azure Cloud Shell, Bash unter Ubuntu oder Windows ein, oder verwenden Sie einen SSH-Client Ihrer Wahl, um die Verbindung zu erstellen. 

## <a name="install-and-configure-telegraf"></a>Installieren und Konfigurieren von Telegraf 

Führen Sie die folgenden Befehle über Ihre SSH-Sitzung aus, um das Debian-Paket für Telegraf auf dem virtuellen Computer zu installieren: 

```cmd
# download the package to the VM 
wget https://dl.influxdata.com/telegraf/releases/telegraf_1.8.0~rc1-1_amd64.deb 
# install the package 
sudo dpkg -i telegraf_1.8.0~rc1-1_amd64.deb
```
Die Konfigurationsdatei von Telegraf definiert die Vorgänge von Telegraf. Standardmäßig wird eine Beispielkonfigurationsdatei im Pfad „/etc/telegraf/telegraf/telegraf.conf“ installiert. Die Beispielkonfigurationsdatei listet alle möglichen Ein- und Ausgabe-Plug-Ins auf. Wir werden jedoch eine benutzerdefinierte Konfigurationsdatei erstellen und den Agent auffordern, sie zu verwenden, indem wir die folgenden Befehle ausführen: 

```cmd
# generate the new Telegraf config file in the current directory 
telegraf --input-filter cpu:mem --output-filter azure_monitor config > azm-telegraf.conf 

# replace the example config with the new generated config 
sudo cp azm-telegraf.conf /etc/telegraf/telegraf.conf 
```

> [!NOTE]
> Die obige Option aktiviert nur zwei Eingabe-Plug-Ins, „cpu“ und „mem“. Sie können jedoch in Abhängigkeit von der Workload auf Ihrem Computer jederzeit weitere Eingabe-Plug-Ins (Docker, MySQL, NGINX usw.) hinzufügen. Eine vollständige Liste der Eingabe-Plug-Ins finden Sie hier. 

Damit der Agent mit der Verwendung der neuen Konfiguration beginnen kann, wird schließlich erzwungen, dass der Agent beendet und erneut gestartet wird, indem die folgenden Befehle ausgeführt werden: 

```cmd
# stop the telegraf agent on the VM 
sudo systemctl stop telegraf 
# start the telegraf agent on the VM to ensure it picks up the latest configuration 
sudo systemctl start telegraf 
```
Jetzt erfasst der Agent die Metriken von jedem der angegebenen Eingabe-Plug-Ins und sendet sie an Azure Monitor. 

## <a name="plot-your-telegraf-metrics-in-the-azure-portal"></a>Darstellen der Telegraf-Metriken im Azure-Portal 

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com). 

1. Navigieren Sie zur neuen Registerkarte „Überwachen“, und wählen Sie dann die Option  **Metriken** aus.  
     ![Füllung](./media/metrics-store-custom-linux-telegraf/metrics.png)

1. Auswählen Ihres virtuellen Computers in der Ressourcenauswahl

     ![Füllung](./media/metrics-store-custom-linux-telegraf/metric-chart.png)

1. Wählen Sie den Namespace *Telegraf/CPU* und die Metrik *usage_system* aus. Sie können nach den Dimensionen dieser Metrik filtern oder nach ihnen aufteilen.  

     ![Füllung](./media/metrics-store-custom-linux-telegraf/VM-resource-selector.png)

## <a name="additional-configuration"></a>Zusätzliche Konfiguration 

Die obige exemplarische Vorgehensweise enthält Informationen darüber, wie Sie den Telegraf-Agent konfigurieren, um Metriken aus einigen wenigen grundlegenden Eingabe-Plug-Ins zu erfassen. Der Telegraf-Agent unterstützt über 150 Eingabe-Plug-Ins, wobei einige zusätzliche Konfigurationsoptionen unterstützt werden. InfluxData hat eine [Liste der unterstützten Plug-Ins](https://docs.influxdata.com/telegraf/v1.7/plugins/inputs/) und Anweisungen zu deren [Konfiguration](https://docs.influxdata.com/telegraf/v1.7/administration/configuration/) veröffentlicht.  

Darüber hinaus hat diese exemplarische Vorgehensweise es Ihnen ermöglicht, mit dem Telegraf-Agent Metriken über den virtuellen Computer, auf dem der Agent bereitgestellt wird, auszugeben. Der Telegraf-Agent kann auch als Collector und zur Weiterleitung von Metriken für andere Ressourcen verwendet werden. Informationen zur Konfiguration des Agent für die Ausgabe von Metriken für andere Azure-Ressourcen finden Sie im Artikel zur [benutzerdefinierten Metrikausgabe von Azure Monitor für Telegraf](https://github.com/influxdata/telegraf/blob/fb704500386214655e2adb53b6eb6b15f7a6c694/plugins/outputs/azure_monitor/README.md).  

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen 

Wenn Ressourcengruppe, virtueller Computer und alle zugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie löschen. Wählen Sie dazu die Ressourcengruppe für den virtuellen Computer aus, klicken Sie auf „Löschen“, und bestätigen Sie den Namen der zu löschenden Ressourcengruppe. 

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [benutzerdefinierte Metriken](metrics-custom-overview.md).


