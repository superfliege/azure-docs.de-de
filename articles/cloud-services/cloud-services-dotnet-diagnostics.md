---
title: Verwendung von Azure-Diagnose (.NET) mit Cloud-Dienste | Microsoft Docs
description: "Mithilfe der Azure-Diagnose zum Sammeln von Daten aus Azure Cloud Services für das Debuggen, zum Messen der Leistung, Überwachen der Analyse des Datenverkehrs und mehr."
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: 
ms.assetid: 89623a0e-4e78-4b67-a446-7d19a35a44be
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/22/2017
ms.author: robb
ms.openlocfilehash: 333d2f26ce043a167fb84858c8327cb39e868ffa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="enabling-azure-diagnostics-in-azure-cloud-services"></a>Aktivieren der Azure-Diagnose in Azure Cloud Services
Finden Sie unter [Übersicht über die Azure-Diagnose](../azure-diagnostics.md) als Hintergrundbild auf Azure-Diagnose.

## <a name="how-to-enable-diagnostics-in-a-worker-role"></a>Gewusst wie: Aktivieren der Diagnose in einer Workerrolle
In dieser exemplarischen Vorgehensweise wird beschrieben, wie Sie eine Azure-workerrolle implementieren, die mithilfe der .NET EventSource-Klasse Telemetriedaten ausgibt. Azure-Diagnose wird verwendet, um die Telemetriedaten erfassen und speichern Sie ihn in Azure Storage-Konto. Wenn Sie eine workerrolle erstellen, kann Visual Studio automatisch Diagnose 1.0 als Teil der Lösung in Azure SDKs für .NET 2.4 und früher. Die folgenden Anweisungen beschreiben den Prozess zum Erstellen der workerrolle Diagnose 1.0 aus der Projektmappe, und Bereitstellen von Diagnose 1.2 oder 1.3 zu Ihrer workerrolle zu deaktivieren.

### <a name="prerequisites"></a>Voraussetzungen
In diesem Artikel wird davon ausgegangen, Azure-Abonnement besitzen und Visual Studio mit dem Azure SDK verwenden. Wenn Sie nicht über ein Azure-Abonnement verfügen, registrieren Sie sich für die [kostenlose Testversion][Free Trial]. Stellen Sie sicher, dass [installieren und Konfigurieren von Azure PowerShell, Version 0.8.7 oder höher][Install and configure Azure PowerShell version 0.8.7 or later].

### <a name="step-1-create-a-worker-role"></a>Schritt 1: Erstellen einer Workerrolle
1. Starten Sie **Visual Studio**.
2. Erstellen einer **Azure-Cloud-Dienst** Projekt aus der **Cloud** Vorlage, die auf .NET Framework 4.5 abzielt.  Nennen Sie das Projekt "WadExample", und klicken Sie auf Ok.
3. Wählen Sie **Workerrolle** , und klicken Sie auf Ok. Das Projekt wird erstellt.
4. In **Projektmappen-Explorer**, doppelklicken Sie auf die **WorkerRole1** Eigenschaftendatei.
5. In der **Konfiguration** Registerkarte, deaktivieren Sie **Aktivieren der Diagnose** Diagnose 1.0 (Azure SDK 2.4 und früher) zu deaktivieren.
6. Erstellen Sie die Projektmappe, um sicherzustellen, dass keine Fehler auftreten.

### <a name="step-2-instrument-your-code"></a>Schritt 2: Instrumentiert Ihren code
Ersetzen Sie den Inhalt der WorkerRole.cs durch den folgenden Code ein. Die Klasse SampleEventSourceWriter, geerbt werden, von der [EventSource-Klasse][EventSource Class], vier Protokollierungsmethoden implementiert: **SendEnums**, **MessageMethod**, **SetOther** und **HighFreq**. Der erste Parameter für die **WriteEvent** Methode definiert die ID für das jeweilige Ereignis. Die "Run"-Methode implementiert eine Endlosschleife, die jeweils von den Protokollierungsmethoden implementiert Aufrufe der **SampleEventSourceWriter** Klasse alle 10 Sekunden.

```csharp
using Microsoft.WindowsAzure.ServiceRuntime;
using System;
using System.Diagnostics;
using System.Diagnostics.Tracing;
using System.Net;
using System.Threading;

namespace WorkerRole1
{
    sealed class SampleEventSourceWriter : EventSource
    {
        public static SampleEventSourceWriter Log = new SampleEventSourceWriter();
        public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); }// Cast enums to int for efficient logging.
        public void MessageMethod(string Message) { if (IsEnabled())  WriteEvent(2, Message); }
        public void SetOther(bool flag, int myInt) { if (IsEnabled())  WriteEvent(3, flag, myInt); }
        public void HighFreq(int value) { if (IsEnabled()) WriteEvent(4, value); }

    }

    enum MyColor
    {
        Red,
        Blue,
        Green
    }

    [Flags]
    enum MyFlags
    {
        Flag1 = 1,
        Flag2 = 2,
        Flag3 = 4
    }

    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
            // This is a sample worker implementation. Replace with your logic.
            Trace.TraceInformation("WorkerRole1 entry point called");

            int value = 0;

            while (true)
            {
                Thread.Sleep(10000);
                Trace.TraceInformation("Working");

                // Emit several events every time we go through the loop
                for (int i = 0; i < 6; i++)
                {
                    SampleEventSourceWriter.Log.SendEnums(MyColor.Blue, MyFlags.Flag2 | MyFlags.Flag3);
                }

                for (int i = 0; i < 3; i++)
                {
                    SampleEventSourceWriter.Log.MessageMethod("This is a message.");
                    SampleEventSourceWriter.Log.SetOther(true, 123456789);
                }

                if (value == int.MaxValue) value = 0;
                SampleEventSourceWriter.Log.HighFreq(value++);
            }
        }

        public override bool OnStart()
        {
            // Set the maximum number of concurrent connections
            ServicePointManager.DefaultConnectionLimit = 12;

            // For information on handling configuration changes
            // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.

            return base.OnStart();
        }
    }
}
```


### <a name="step-3-deploy-your-worker-role"></a>Schritt 3: Bereitstellen der Workerrolle

[!INCLUDE [cloud-services-wad-warning](../../includes/cloud-services-wad-warning.md)]

1. Die Worker-Rolle in Azure aus Visual Studio bereitstellen, durch Auswählen der **WadExample** Projekt im Projektmappen-Explorer klicken Sie dann **veröffentlichen** aus der **erstellen** Menü.
2. Wählen Sie Ihr Abonnement aus.
3. In der **Microsoft Azure-Veröffentlichungseinstellungen** wählen Sie im Dialogfeld **neu erstellen...** .
4. In der **Cloud-Dienst erstellen und das Speicherkonto** Dialogfeld, geben Sie einen **Namen** (z. B. "WadExample"), und wählen Sie eine Region oder Affinitätsgruppe.
5. Legen Sie die **Umgebung** auf **Staging**.
6. Ändern Sie alle anderen **Einstellungen** als zutreffend, und klicken Sie auf **veröffentlichen**.
7. Nach Abschluss der Bereitstellung, vergewissern Sie sich im Azure-Portal, das in der Cloud-Dienst ist ein **ausführen** Zustand.

### <a name="step-4-create-your-diagnostics-configuration-file-and-install-the-extension"></a>Schritt 4: Erstellen der diagnosekonfigurationsdatei und installieren Sie die Erweiterung
1. Herunterladen der Schemadefinition der öffentlichen Konfiguration-Datei durch den folgenden PowerShell-Befehl ausführen:

    ```powershell
    (Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'
    ```
2. Fügen Sie eine XML-Datei auf Ihrer **WorkerRole1** Projekt, indem Sie mit der rechten Maustaste auf die **WorkerRole1** Projekt, und wählen Sie **hinzufügen** -> **neues Element...** -> **Visual C# items** -> **Data** -> **XML File**. Nennen Sie die Datei "WadExample.xml".

   ![CloudServices_diag_add_xml](./media/cloud-services-dotnet-diagnostics/AddXmlFile.png)
3. Ordnen Sie die Konfigurationsdatei mit den WadConfig.xsd. Stellen Sie sicher, dass die WadExample.xml-Editor-Fenster das aktive Fenster ist. Drücken Sie **F4** So öffnen die **Eigenschaften** Fenster. Klicken Sie auf die **Schemas** Eigenschaft in der **Eigenschaften** Fenster. Klicken Sie auf der **...** in der **Schemas** Eigenschaft. Klicken Sie auf die **hinzufügen...** Schaltfläche, und navigieren Sie zu dem Speicherort, in dem die XSD-Datei gespeichert, und wählen Sie die Datei WadConfig.xsd. Klicken Sie auf **OK**.

4. Ersetzen Sie den Inhalt der Konfigurationsdatei WadExample.xml durch das folgende XML, und speichern Sie die Datei. Diese Konfigurationsdatei definiert einige Leistungsindikatoren sammeln: eine für CPU-Auslastung und eine für die arbeitsspeicherverwendung. Klicken Sie dann definiert die Konfiguration der vier Ereignisse, die Methoden in der Klasse SampleEventSourceWriter entspricht.

```xml
<?xml version="1.0" encoding="utf-8"?>
<PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <WadCfg>
    <DiagnosticMonitorConfiguration overallQuotaInMB="25000">
      <PerformanceCounters scheduledTransferPeriod="PT1M">
        <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />
        <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT1M" unit="bytes"/>
      </PerformanceCounters>
      <EtwProviders>
        <EtwEventSourceProviderConfiguration provider="SampleEventSourceWriter" scheduledTransferPeriod="PT5M">
          <Event id="1" eventDestination="EnumsTable"/>
          <Event id="2" eventDestination="MessageTable"/>
          <Event id="3" eventDestination="SetOtherTable"/>
          <Event id="4" eventDestination="HighFreqTable"/>
          <DefaultEvents eventDestination="DefaultTable" />
        </EtwEventSourceProviderConfiguration>
      </EtwProviders>
    </DiagnosticMonitorConfiguration>
  </WadCfg>
</PublicConfig>
```

### <a name="step-5-install-diagnostics-on-your-worker-role"></a>Schritt 5: Installieren der Diagnose auf die Worker-Rolle
Die PowerShell-Cmdlets zum Verwalten der Diagnose für eine Web- oder Workerrollen-Rolle sind: Set-AzureServiceDiagnosticsExtension, Get-AzureServiceDiagnosticsExtension und Remove-AzureServiceDiagnosticsExtension.

1. Öffnen Sie Azure PowerShell.
2. Führen Sie das Skript zum Installieren von Diagnose auf die Worker-Rolle (ersetzen Sie *StorageAccountKey* mit der speicherkontoschlüssel für Ihr Speicherkonto Wadexample und *Config_path* durch den Pfad zu der *WadExample.xml* Datei):

```powershell
$storage_name = "wadexample"
$key = "<StorageAccountKey>"
$config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExample\WorkerRole1\WadExample.xml"
$service_name="wadexample"
$storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Staging -Role WorkerRole1
```

### <a name="step-6-look-at-your-telemetry-data"></a>Schritt 6: Sehen Sie sich die Telemetriedaten
In Visual Studio **Server-Explorer**, navigieren Sie zu dem Speicherkonto Wadexample. Nachdem Sie der Cloud-Dienst ungefähr fünf (5) Minuten ausgeführt wurde, sehen Sie die Tabellen **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** und **WADSetOtherTable**. Doppelklicken Sie auf eine der Tabellen, die Telemetrie anzuzeigen, die gesammelt wurden.

![CloudServices_diag_tables](./media/cloud-services-dotnet-diagnostics/WadExampleTables.png)

## <a name="configuration-file-schema"></a>Konfigurationsdateischema
Die diagnosekonfigurationsdatei definiert Werte, die verwendet werden, um diagnosekonfigurationseinstellungen zu initialisieren, wenn die Diagnose-Agent gestartet wird. Finden Sie unter der [neueste Schemareferenz](https://msdn.microsoft.com/library/azure/mt634524.aspx) gültige Werte und Beispiele.

## <a name="troubleshooting"></a>Problembehandlung
Wenn Sie Probleme haben, finden Sie unter [Problembehandlung bei Azure-Diagnose](../azure-diagnostics-troubleshooting.md) Hilfe bei allgemeinen Problemen.

## <a name="next-steps"></a>Nächste Schritte
[Finden Sie eine Liste der verknüpften Azure VM-Diagnose Artikel](../monitoring-and-diagnostics/azure-diagnostics.md#cloud-services-using-azure-diagnostics) zum Ändern der Daten Sie sammeln Behandlung von Problemen mit, oder erfahren Sie mehr über Diagnose im Allgemeinen.

[EventSource Class]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Free Trial]: http://azure.microsoft.com/pricing/free-trial/
[Install and configure Azure PowerShell version 0.8.7 or later]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
