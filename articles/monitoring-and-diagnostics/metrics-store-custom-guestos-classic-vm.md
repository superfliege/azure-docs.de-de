---
title: Senden von Metriken des Gastbetriebssystems an den Azure Monitor-Datenspeicher für einen virtuellen Windows-Computer (klassisch)
description: Senden von Metriken des Gastbetriebssystems an den Azure Monitor-Datenspeicher für einen virtuellen Windows-Computer (klassisch)
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ancav
ms.component: ''
ms.openlocfilehash: cb803450f7765ae62292ff3afb7f32209b437f78
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46978926"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-data-store-for-a-windows-virtual-machine-classic"></a>Senden von Metriken des Gastbetriebssystems an den Azure Monitor-Datenspeicher für einen virtuellen Windows-Computer (klassisch)

Die [Windows Azure-Diagnoseerweiterung](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) (WAD) von Azure Monitor ermöglicht es Ihnen, Metriken und Protokolle vom Gastbetriebssystem zu erfassen, das als Teil eines virtuellen Computers, eines Clouddiensts oder eines Service Fabric-Clusters ausgeführt wird. Die Erweiterung kann Telemetriedaten an viele verschiedene Standorte senden, die im zuvor verlinkten Artikel aufgeführt sind.

In diesem Artikel wird der Prozess zum Senden von Leistungsmetriken des Gastbetriebssystems für einen virtuellen Windows-Computer (klassisch) an den Azure Monitor-Metrikspeicher beschrieben. Ab WAD Version 1.11 können Sie Metriken direkt in den Azure Monitor-Metrikspeicher schreiben, in dem bereits Metriken der Standardplattformen gesammelt werden. Wenn Sie sie an dieser Position speichern, können Sie auf die gleichen Aktionen zugreifen, die auch für Plattformmetriken verfügbar sind.  Zu den Maßnahmen gehören zeitnahe Benachrichtigung, Diagrammerstellung, Routing, Zugriff über die REST-API und vieles mehr.  In der Vergangenheit hat die WAD-Erweiterung in Azure Storage geschrieben, aber nicht der Azure Monitor-Datenspeicher. 

Der in diesem Artikel beschriebene Prozess funktioniert nur mit klassischen virtuellen Computern unter dem Windows-Betriebssystem.

## <a name="pre-requisites"></a>Voraussetzungen

- Sie müssen ein [Dienstadministrator oder Co-Administrator](https://docs.microsoft.com/azure/billing/billing-add-change-azure-subscription-administrator.md) für Ihr Azure-Abonnement sein. 

- Ihr Abonnement muss mit [Microsoft.Insights](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) registriert werden. 

- Sie müssen entweder [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) installiert haben, oder Sie können [Azure CloudShell](https://docs.microsoft.com/azure/cloud-shell/overview.md) verwenden. 

## <a name="create-a-classic-virtual-machine-and-storage-account"></a>Erstellen eines klassischen virtuellen Computers und eines Speicherkontos

1. Erstellen eines klassischen virtuellen Computers über das Azure-Portal ![Erstellen eines klassischen virtuellen Computers](./media/metrics-store-custom-guestos-classic-vm/create-classic-vm.png).

1. Wählen Sie beim Erstellen dieses virtuellen Computers aus, ob Sie ein neues klassisches Speicherkonto erstellen möchten. Wir verwenden dieses Speicherkonto in späteren Schritten.

1. Navigieren Sie im Azure-Portal zum Ressourcenblatt „Speicherkonto“, und wählen Sie die **Schlüssel** aus. Notieren Sie sich den Namen und Schlüssel des Speicherkontos. Sie benötigen diese Schlüssel in späteren Schritten ![Speicherzugriffsschlüssel](./media/metrics-store-custom-guestos-classic-vm/storage-access-keys.png)

## <a name="create-a-service-principal"></a>Erstellen eines Dienstprinzipals

Erstellen Sie einen Dienstprinzipal in Ihrem Azure Active Directory-Mandanten unter Verwendung der Anweisungen unter [Erstellen eines Dienstprinzipals](../azure-resource-manager/resource-group-create-service-principal-portal.md). Beachten Sie Folgendes, während Sie diesen Prozess durchlaufen: 
- Erstellen Sie einen neuen geheimen Client für diese App.  
- Speichern Sie den Schlüssel und die Client-ID für die Verwendung in späteren Schritten.

Erteilen Sie der App die Berechtigungen „Herausgeber von Überwachungsmetriken“ für die Ressource, für die Sie Metriken ausgeben möchten. Sie können eine Ressourcengruppe oder ein vollständiges Abonnement verwenden.  

> [!NOTE]
> Die Diagnoseerweiterung verwendet den Dienstprinzipal, um sich gegenüber Azure Monitor zu authentifizieren und Metriken für Ihren klassischen virtuellen Computer auszugeben.

## <a name="author-diagnostics-extension-configuration"></a>Erstellen der Konfiguration der Diagnoseerweiterung

1. Bereiten Sie Ihre Konfigurationsdatei für die WAD-Diagnoseerweiterung vor. Diese Datei bestimmt, welche Protokolle und Leistungsindikatoren die Diagnoseerweiterung für Ihren klassischen virtuellen Computer sammeln soll. Unten ist ein Beispiel angegeben.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        <WadCfg>
        <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="applicationInsights.errors">
            <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" />
            <Directories scheduledTransferPeriod="PT1M">
                <IISLogs containerName="wad-iis-logfiles" />
                <FailedRequestLogs containerName="wad-failedrequestlogs" />
            </Directories>
            <PerformanceCounters scheduledTransferPeriod="PT1M">
                <PerformanceCounterConfiguration counterSpecifier="\Processor(*)\% Processor Time" sampleRate="PT15S" />
                <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT15S" />
                <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT15S" />
                <PerformanceCounterConfiguration counterSpecifier="\Memory\% Committed Bytes" sampleRate="PT15S" />
                <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(*)\Disk Read Bytes/sec" sampleRate="PT15S" />
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
            <Metrics resourceId="/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicCompute/virtualMachines/MyClassicVM">
                <MetricAggregation scheduledTransferPeriod="PT1M" />
                <MetricAggregation scheduledTransferPeriod="PT1H" />
            </Metrics>
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
1. Definieren Sie im Abschnitt „SinksConfig“ Ihrer Diagnosedatei eine neue Azure Monitor-Senke:

    ```xml
    <SinksConfig>
        <Sink name="AzMonSink">
            <AzureMonitor>
                <ResourceId>Provide your Classic VM’s Resource ID </ResourceId>
                <Region>Region your VM is deployed in</Region>
            </AzureMonitor>
        </Sink>
    </SinksConfig>
    ```

1. Leiten Sie in dem Abschnitt Ihrer Konfigurationsdatei, in dem Sie die zu sammelnden Leistungsindikatoren auflisten, die Leistungsindikatoren an die Azure Monitor-Senke „AzMonSink“ weiter.

    ```xml
    <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="AzMonSink">
        <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" />
    ...
    </PerformanceCounters>
    ```

1. Definieren Sie in der privaten Konfiguration das Azure Monitor-Konto, und fügen Sie die Informationen des Dienstprinzipals hinzu, den Sie zum Ausgeben von Metriken verwenden.

    ```xml
    <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <StorageAccount name="" endpoint="" />
        <AzureMonitorAccount>
            <ServicePrincipalMeta>
                <PrincipalId>clientId for your service principal</PrincipalId>
                <Secret>client secret of your service principal</Secret>
            </ServicePrincipalMeta>
        </AzureMonitorAccount>
    </PrivateConfig>
    ```

1. Speichern Sie diese Datei lokal.

## <a name="deploy-diagnostics-extension-to-your-cloud-service"></a>Bereitstellen der Diagnoseerweiterung für Ihren Clouddienst

1. Starten Sie PowerShell, und melden Sie sich an.

    ```powershell
    Login-AzureRmAccount
    ```

1. Beginnen Sie damit, den Kontext auf Ihren klassischen virtuellen Computer festzulegen.

    ```powershell
    $VM = Get-AzureVM -ServiceName <VM’s Service_Name> -Name <VM Name>
    ```

1. Legen Sie den Kontext des klassischen Speicherkontos fest, das mit dem virtuellen Computer erstellt wurde.

    ```powershell
    $StorageContext = New-AzureStorageContext -StorageAccountName <name of your storage account from earlier steps> -storageaccountkey "<storage account key from earlier steps>"
    ```

1.  Legen Sie den Diagnosedateipfad mit dem folgenden Befehl in einer Variablen fest.

    ```powershell
    $diagconfig = “<path of the diagnostics configuration file with the Azure Monitor sink configured>”
    ```

1.  Bereiten Sie das Update für Ihren klassischen virtuellen Computer mit der Diagnosedatei mit der konfigurierten Azure Monitor-Senke vor.

    ```powershell
    $VM_Update = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $diagconfig -VM $VM -StorageContext $Storage_Context
    ```

1.  Stellen Sie das Update auf Ihrem virtuellen Computer bereit, indem Sie den folgenden Befehl ausführen.

    ```powershell
    Update-AzureVM -ServiceName "ClassicVMWAD7216" -Name "ClassicVMWAD" -VM $VM_Update.VM
    ```

> [!NOTE]
> Sie müssen im Rahmen der Installation der Diagnoseerweiterung weiterhin ein Speicherkonto angeben. Alle Protokolle und/oder Leistungsindikatoren, die in der Diagnosekonfigurationsdatei angegeben sind, werden in das angegebene Speicherkonto geschrieben.

## <a name="plot-the-metrics-in-the-azure-portal"></a>Darstellen der Metriken im Azure-Portal

1.  Navigieren Sie zum Azure-Portal.

1.  Klicken Sie im linken Menü auf „Überwachen“.

1.  Klicken Sie auf dem Blatt „Überwachen“ auf **Metriken**
   ![Metriken navigieren](./media/metrics-store-custom-guestos-classic-vm/navigate-metrics.png).

1. Wählen Sie in der Dropdownliste der Ressourcen Ihren klassischen virtuellen Computer aus.

1. Wählen Sie in der Dropdownliste der Namespaces die Option **azure.vm.windows.guest** aus.

1. Wählen Sie in der Dropdownliste der Metriken **Arbeitsspeicher\Verwendete zugesicherte Bytes**
   ![Metriken darstellen](./media/metrics-store-custom-guestos-classic-vm/plot-metrics.png) aus.


## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [benutzerdefinierte Metriken](metrics-custom-overview.md).
