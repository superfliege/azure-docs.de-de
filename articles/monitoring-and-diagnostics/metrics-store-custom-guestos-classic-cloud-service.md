---
title: Senden von Metriken des Gastbetriebssystems an den klassischen Clouddienst des Azure Monitor-Metrikspeichers
description: Senden von Metriken des Gastbetriebssystems an den klassischen Clouddienst des Azure Monitor-Metrikspeichers
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: howto
ms.date: 09/24/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: be27ff3f8dda3209a011c3ad79d1a7a1f1d259fe
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46986913"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-metric-store-classic-cloud-service"></a>Senden von Metriken des Gastbetriebssystems an den klassischen Clouddienst des Azure Monitor-Metrikspeichers

Die [Windows Azure-Diagnoseerweiterung](azure-diagnostics.md) (WAD) von Azure Monitor ermöglicht es Ihnen, Metriken und Protokolle vom Gastbetriebssystem zu erfassen, das als Teil eines virtuellen Computers, eines Clouddiensts oder eines Service Fabric-Clusters ausgeführt wird.  Die Erweiterung kann Telemetriedaten an viele verschiedene Standorte senden, die im zuvor verlinkten Artikel aufgeführt sind.  

In diesem Artikel wird der Prozess zum Senden von Leistungsmetriken des Gastbetriebssystems für klassische Azure-Clouddienste an den Azure Monitor-Metrikspeicher beschrieben. Ab WAD Version 1.11 können Sie Metriken direkt in den Azure Monitor-Metrikspeicher schreiben, in dem bereits Metriken der Standardplattformen gesammelt werden. Wenn Sie sie an dieser Position speichern, können Sie auf die gleichen Aktionen zugreifen, die auch für Plattformmetriken verfügbar sind.  Zu den Maßnahmen gehören zeitnahe Benachrichtigung, Diagrammerstellung, Routing, Zugriff über die REST-API und vieles mehr.  In der Vergangenheit hat die WAD-Erweiterung in Azure Storage geschrieben, aber nicht der Azure Monitor-Datenspeicher.  

Der in diesem Artikel beschriebene Prozess funktioniert nur für Leistungsindikatoren für Azure Cloud Services. Er funktioniert nicht für andere benutzerdefinierte Metriken. 
   

## <a name="pre-requisites"></a>Voraussetzungen

- Sie müssen ein [Dienstadministrator oder Co-Administrator](https://docs.microsoft.com/azure/billing/billing-add-change-azure-subscription-administrator.md) für Ihr Azure-Abonnement sein. 

- Ihr Abonnement muss mit [Microsoft.Insights](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) registriert werden. 

- Sie müssen entweder [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) installiert haben, oder Sie können [Azure CloudShell](https://docs.microsoft.com/azure/cloud-shell/overview.md) verwenden. 


## <a name="provision-cloud-service-and-storage-account"></a>Bereitstellung von Clouddienst und Speicherkonto 

1. Erstellen Sie einen klassischen Clouddienst, und stellen Sie ihn bereit (ein Beispiel einer klassischen Clouddienst-Anwendung und -Bereitstellung finden Sie [hier](../cloud-services/cloud-services-dotnet-get-started.md)). 

2. Sie können ein vorhandenes Speicherkonto verwenden oder ein neues Speicherkonto bereitstellen. Es wird empfohlen, dass sich das Speicherkonto in derselben Region wie der klassische Clouddienst befindet, den Sie gerade erstellt haben. Navigieren Sie im Azure-Portal zum Ressourcenblatt „Speicherkonto“ und wählen Sie die **Schlüssel** aus. Notieren Sie sich den Namen und den Schlüssel des Speicherkonto, die Sie in späteren Schritten benötigen.

   ![Speicherkontoschlüssel](./media/metrics-store-custom-guestos-classic-cloud-service/storage-keys.png)



## <a name="create-a-service-principal"></a>Erstellen eines Dienstprinzipals 

Erstellen Sie einen Dienstprinzipal in Ihrem Azure Active Directory-Mandanten unter Verwendung der Anweisungen unter ../azure/azure/azure-resource-manager/resource-group-create-service-principal-portal. Beachten Sie Folgendes, während Sie diesen Prozess durchlaufen: 
  - Sie können eine beliebige URL für die Anmelde-URL eingeben.  
  - Erstellen Sie einen neuen geheimen Client für diese App.  
  - Speichern Sie den Schlüssel und die Client-ID für die Verwendung in späteren Schritten.  

Erteilen Sie der App, die im vorherigen Schritt *Herausgeber von Überwachungsmetriken* erstellt wurde, Berechtigungen für die Ressource, für die Sie Metriken ausgeben möchten. Wenn Sie planen, die App zu verwenden, um benutzerdefinierte Metriken für viele Ressourcen auszugeben, können Sie diese Berechtigungen auf der Ebene der Ressourcengruppe oder des Abonnements vergeben.  

> [!NOTE]
> Die Diagnoseerweiterung verwendet den Dienstprinzipal, um sich gegenüber Azure Monitor zu authentifizieren und Metriken für Ihren Clouddienst auszugeben. 

## <a name="author-diagnostics-extension-configuration"></a>Erstellen der Konfiguration der Diagnoseerweiterung 

Bereiten Sie Ihre Konfigurationsdatei für die WAD-Diagnoseerweiterung vor. Diese Datei bestimmt, welche Protokolle und Leistungsindikatoren die Diagnoseerweiterung für Ihren Clouddienst sammeln soll. Nachfolgend finden Sie eine Beispieldatei für die Diagnosekonfiguration.  

```XML
<?xml version="1.0" encoding="utf-8"?> 
<DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"> 
  <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"> 
    <WadCfg> 
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096"> 
        <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" /> 
        <Directories scheduledTransferPeriod="PT1M"> 
          <IISLogs containerName="wad-iis-logfiles" /> 
          <FailedRequestLogs containerName="wad-failedrequestlogs" /> 
        </Directories> 
        <PerformanceCounters scheduledTransferPeriod="PT1M"> 
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" /> 
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT15S" /> 
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT15S" /> 
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Page Faults/sec" sampleRate="PT15S" /> 
        </PerformanceCounters> 
        <WindowsEventLog scheduledTransferPeriod="PT1M"> 
          <DataSource name="Application!*[System[(Level=1 or Level=2 or Level=3)]]" /> 
          <DataSource name="Windows Azure!*[System[(Level=1 or Level=2 or Level=3 or Level=4)]]" /> 
        </WindowsEventLog> 
        <CrashDumps> 
          <CrashDumpConfiguration processName="WaIISHost.exe" /> 
          <CrashDumpConfiguration processName="WaWorkerHost.exe" /> 
          <CrashDumpConfiguration processName="w3wp.exe" /> 
        </CrashDumps> 
        <Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Error" /> 
      </DiagnosticMonitorConfiguration> 
      <SinksConfig> 
      </SinksConfig> 
    </WadCfg> 
    <StorageAccount /> 
  </PublicConfig> 
  <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"> 
    <StorageAccount name="" endpoint="" /> 
</PrivateConfig> 
  <IsEnabled>true</IsEnabled> 
</DiagnosticsConfiguration> 
```

Definieren Sie im Abschnitt „SinksConfig“ Ihrer Diagnosedatei eine neue Azure Monitor-Senke: 

```XML
  <SinksConfig> 
    <Sink name="AzMonSink"> 
    <AzureMonitor> 
      <ResourceId>-Provide ClassicCloudService’s Resource ID-</ResourceId> 
      <Region>-Azure Region your Cloud Service is deployed in-</Region> 
    </AzureMonitor> 
    </Sink> 
  </SinksConfig> 
```

Fügen Sie in dem Abschnitt Ihrer Konfigurationsdatei, in dem Sie die zu sammelnden Leistungsindikatoren auflisten, die Azure Monitor-Senke hinzu. Dieser Eintrag stellt sicher, dass alle angegebenen Leistungsindikatoren als Metriken an Azure Monitor weitergeleitet werden. Sie können Leistungsindikatoren nach Bedarf hinzufügen/entfernen. 

```XML
<PerformanceCounters scheduledTransferPeriod="PT1M" sinks="AzMonSink"> 
 <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" /> 
  … 
</PerformanceCounters> 
```
Fügen Sie schließlich in der privaten Konfiguration den Abschnitt *Azure Monitor-Konto* hinzu. Geben Sie die Client-ID des Dienstprinzipals und den geheimen Schlüssel ein, die in einem früheren Schritt erstellt wurden. 

```XML
<PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"> 
  <StorageAccount name="" endpoint="" /> 
    <AzureMonitorAccount> 
      <ServicePrincipalMeta> 
        <PrincipalId>clientId from step 3</PrincipalId> 
        <Secret>client secret from step 3</Secret> 
      </ServicePrincipalMeta> 
    </AzureMonitorAccount> 
</PrivateConfig> 
```
 
Speichern Sie diese Diagnosedatei lokal.  

## <a name="deploy-the-diagnostics-extension-to-your-cloud-service"></a>Bereitstellen der Diagnoseerweiterung für Ihren Clouddienst 

Starten von PowerShell und Anmelden an Azure 

```PowerShell
Login-AzureRmAccount 
```

Speichern Sie die Details zu den in einem früheren Schritt erstellten Details des Speicherkontos mit den folgenden Befehlen in Variablen. 

```PowerShell
$storage_account = <name of your storage account from step 3> 
$storage_keys = <storage account key from step 3> 
```
 
Legen Sie ebenso den Diagnosedateipfad mit dem folgenden Befehl in einer Variablen fest. 

```PowerShell
$diagconfig = “<path of the diagnostics configuration file with the Azure Monitor sink configured>” 
```
 
Stellen Sie die Diagnoseerweiterung mit dem nachfolgenden Befehl für Ihren Clouddienst mit der Diagnosedatei mit der konfigurierten Azure Monitor-Senke bereit. 

```PowerShell
Set-AzureServiceDiagnosticsExtension -ServiceName <classicCloudServiceName> -StorageAccountName $storage_account -StorageAccountKey $storage_keys -DiagnosticsConfigurationPath $diagconfig 
```
 
> [!NOTE] 
> Sie müssen im Rahmen der Installation der Diagnoseerweiterung weiterhin ein Speicherkonto angeben. Alle Protokolle und/oder Leistungsindikatoren, die in der Diagnosekonfigurationsdatei angegeben sind, werden in das angegebene Speicherkonto geschrieben.  

## <a name="plot-metrics-in-the-azure-portal"></a>Darstellen von Metriken im Azure-Portal 

Navigieren Sie zum Azure-Portal. 

 ![Metriken – Azure-Portal](./media/metrics-store-custom-guestos-classic-cloud-service/navigate-metrics.png)

1. Klicken Sie im linken Menü auf „Überwachen“. 

1. Klicken Sie auf dem Blatt „Überwachen“ auf die Registerkarte „Metriken (Vorschau)“. 

1. Wählen Sie in der Dropdownliste der Ressourcen Ihren klassischen Clouddienst aus. 

1. Wählen Sie in der Dropdownliste der Namespaces **azure.vm.windows.guest** aus. 

1. Wählen Sie in der Dropdownliste der Metriken *Arbeitsspeicher\Verwendete zugesicherte Bytes* aus. 

Sie können wählen, ob Sie den gesamten von einer bestimmten Rolle und den einzelnen Rolleninstanzen verwendeten Arbeitsspeicher anzeigen möchten, indem Sie die Funktionen zum Filtern und Aufteilen von Dimensionen verwenden. 

 ![Metriken – Azure-Portal](./media/metrics-store-custom-guestos-classic-cloud-service/metrics-graph.png)

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [benutzerdefinierte Metriken](metrics-custom-overview.md).



