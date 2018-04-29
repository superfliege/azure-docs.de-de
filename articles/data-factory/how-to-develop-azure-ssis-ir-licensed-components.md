---
title: Entwickeln kostenpflichtiger oder lizenzierter Komponenten für Azure SSIS Integration Runtime | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie ein ISV kostenpflichtige oder lizenzierte benutzerdefinierten Komponenten für die Azure SSIS Integration Runtime entwickeln und installieren kann
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2018
ms.author: douglasl
ms.openlocfilehash: e22ca4bd5b749e8752f800590938199e06abbd34
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="develop-paid-or-licensed-custom-components-for-the-azure-ssis-integration-runtime"></a>Entwickeln kostenpflichtiger oder lizenzierter benutzerdefinierter Komponenten für Azure SSIS Integration Runtime

## <a name="problem---the-azure-ssis-ir-requires-a-different-approach"></a>Problem: Die Azure SSIS IR erfordert einen anderen Ansatz

Die Eigenart der Azure SSIS Integration Runtime bringt verschiedene Herausforderungen mit sich, aufgrund derer die typischen Lizenzierungsmethoden für die lokale Installation benutzerdefinierter Komponenten unzureichend sind.

-   Die Knoten der Azure SSIS IR sind flüchtig und können jederzeit zugeordnet oder freigegeben werden. Beispielsweise können Sie Knoten starten oder beenden, um die Kosten im Griff zu behalten, oder durch verschiedene Knotengrößen zentral hoch- und herunterskalieren. Dadurch ist es nicht mehr möglich, eine Komponentenlizenz eines Drittanbieters über computerspezifische Informationen wie MAC-Adresse oder CPU-ID an einen bestimmten Knoten zu binden.

-   Sie können die Azure SSIS IR auch horizontal herunter- oder hochskalieren, sodass die Anzahl der Knoten jederzeit verkleinert oder vergrößert werden kann.

## <a name="solution---windows-environment-variables-and-ssis-system-variables-for-license-binding-and-validation"></a>Lösung: Windows-Umgebungsvariablen und SSIS-Systemvariablen für Lizenzbindung und -validierung

Aufgrund der im vorherigen Abschnitt beschriebenen Einschränkungen herkömmlicher Lizenzierungsmethoden stellt die Azure SSIS IR Windows-Umgebungsvariablen und SSIS-Systemvariablen für die Lizenzbindung und -validierung von Komponenten von Drittanbietern zur Verfügung. ISVs können diese Variablen nutzen, um eindeutige und persistente Informationen für eine Azure SSIS IR zu erhalten, wie z.B. Cluster-ID und Anzahl der Clusterknoten. Mit dieser Information können ISVs die Lizenz für ihre Komponente *als Cluster* an eine Azure SSIS IR binden. Dies erfolgt mit einer ID, die sich nicht ändert, wenn Kunden die Azure SSIS IR starten oder beenden, zentral oder horizontal hoch- oder herunterskalieren oder auf andere Weise neu konfigurieren.

Die folgende Abbildung zeigt die typischen Installations-, Aktivierungs- und Lizenzbindungsabläufe für Komponenten von Drittanbietern, die diese neuen Variablen verwenden:

![Installation lizenzierter Komponenten](media/how-to-configure-azure-ssis-ir-licensed-components/licensed-component-installation.png)

## <a name="instructions"></a>Anleitung
1. ISVs können ihre lizenzierten Komponenten in verschiedenen SKUs oder Stufen anbieten (z.B. Einzelknoten, bis zu 5 Knoten, bis zu 10 Knoten usw.). Der ISV stellt beim Kauf eines Produkts den entsprechenden Product Key zur Verfügung. Der ISV kann auch einen Azure Storage-Blobcontainer bereitstellen, der ein ISV-Setupskript und zugehörige Dateien enthält. Kunden können diese Dateien in ihren eigenen Speichercontainer kopieren und mit ihrem eigenen Product Key modifizieren (z.B. durch Ausführen von `IsvSetup.exe -pid xxxx-xxxx-xxxx`). Kunden können dann die Azure SSIS IR mit dem SAS-URI ihres Containers als Parameter bereitstellen oder neu konfigurieren. Weitere Informationen finden Sie unter [Custom setup for the Azure-SSIS integration runtime](how-to-configure-azure-ssis-ir-custom-setup.md) (Benutzerdefinierte Einrichtung der Azure SSIS Integration Runtime).

2. Wenn die Azure SSIS IR bereitgestellt oder neu konfiguriert wurde, wird das ISV-Setup auf jedem Knoten ausgeführt, um die Windows-Umgebungsvariablen `SSIS_CLUSTERID` und `SSIS_CLUSTERNODECOUNT` abzufragen. Anschließend sendet die Azure SSIS IR ihre Cluster-ID und den Product Key für das lizenzierte Produkt an den Aktivierungsserver des ISV, um einen Aktivierungsschlüssel zu generieren.

3. Nach Empfang des Aktivierungsschlüssels kann das ISV-Setup den Schlüssel lokal auf jedem Knoten (z.B. in der Registrierung) speichern.

4. Wenn Kunden ein Paket ausführen, das die lizenzierte Komponente des ISV auf einem Knoten der Azure SSIS IR verwendet, liest das Paket den lokal gespeicherten Aktivierungsschlüssel und validiert ihn im Abgleich mit der Cluster-ID des Knotens. Das Paket kann dem ISV-Aktivierungsserver optional auch die Anzahl der Clusterknoten melden.

    Hier ein Codebeispiel, das den Aktivierungsschlüssel validiert und die Anzahl der Clusterknoten meldet:

    ```csharp
    public override DTSExecResult Validate(Connections, VariableDispenser, IDTSComponentEvents componentEvents, IDTSLogging log) 
                                                                                                                               
    {                                                                                                                             
                                                                                                                               
    Variables vars = null;                                                                                                        
                                                                                                                               
    variableDispenser.LockForRead("System::ClusterID");                                                                           
                                                                                                                               
    variableDispenser.LockForRead("System::ClusterNodeCount");                                                                    
                                                                                                                               
    variableDispenser.GetVariables(ref vars);                                                                                     
                                                                                                                               
    // Validate Activation Key with ClusterID                                                                                     
                                                                                                                               
    // Report on ClusterNodeCount                                                                                                 
                                                                                                                               
    vars.Unlock();                                                                                                                
                                                                                                                               
    return base.Validate(connections, variableDispenser, componentEvents, log);                                                   
                                                                                                                               
    }
    ```

## <a name="next-steps"></a>Nächste Schritte

-   [Benutzerdefiniertes Setup von Azure SSIS Integration Runtime](how-to-configure-azure-ssis-ir-custom-setup.md)

-   [Enterprise Edition von Azure SSIS Integration Runtime](how-to-configure-azure-ssis-ir-enterprise-edition.md)