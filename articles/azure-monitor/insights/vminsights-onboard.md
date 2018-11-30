---
title: Onboarding von Azure Monitor für VMs (Vorschau) | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie das Onboarding und die Konfiguration von Azure Monitor for VMs durchführen, damit Sie erfahren können, wie gut die Leistung Ihrer verteilten Anwendung ist und welche Integritätsprobleme erkannt wurden.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/13/2018
ms.author: magoedte
ms.openlocfilehash: 93cbb9a891f83eed623f095f208319c9071e287e
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/30/2018
ms.locfileid: "52635542"
---
# <a name="how-to-onboard-the-azure-monitor-for-vms-preview"></a>Onboardingmethoden für den Azure Monitor für VMs (Vorschau)
In diesem Artikel wird beschrieben, wie Sie Azure Monitor für VMs zur Überwachung der Betriebssystemintegrität Ihrer Azure-VMs und -VM-Skalierungsgruppen sowie der VMs in Ihrer Umgebung einrichten (einschließlich Erkennung und Zuordnung von Anwendungsabhängigkeiten der ggf. auf ihnen gehosteten Anwendungen).  

Das Aktivieren von Azure Monitor for VMs erfolgt mithilfe einer der im Folgenden vorgestellten Methoden; Details zur Verwendung der einzelnen Methoden finden Sie weiter unten im Artikel.  

* Für eine einzelne Azure-VM durch Auswählen von **Insights (Vorschau)** direkt in der VM.
* Für mehrere Azure-VMs mithilfe von Azure Policy, um sicherzustellen, dass auf vorhandenen und neuen bewerteten VMs die erforderlichen Abhängigkeiten installiert und ordnungsgemäß konfiguriert sind.  Nicht konforme VMs werden gemeldet, so dass Sie je nachdem, wo die Konformität nicht eingehalten wird, entscheiden können, wie Sie den Zustand korrigieren möchten.  
* Für mehrere Azure-VMs oder VM-Skalierungsgruppen innerhalb eines angegebenen Abonnements oder einer Ressourcengruppe mithilfe von PowerShell.

## <a name="prerequisites"></a>Voraussetzungen
Sie sollten, bevor Sie beginnen, unbedingt die in den folgenden Unterabschnitten beschriebenen Informationen verstanden haben.

### <a name="log-analytics"></a>Log Analytics 

Ein Log Analytics-Arbeitsbereich wird aktuell in den folgenden Regionen unterstützt:

  - USA, Westen-Mitte  
  - USA (Ost)  
  - Europa, Westen  
  - Asien, Südosten<sup>1</sup>  

<sup>1</sup> Diese Region unterstützt derzeit das Integritätsfeature von Azure Monitor for VMs nicht.   

>[!NOTE]
>Das Onboarding von virtuellen Azure-Computern kann aus jeder Region erfolgen, und es besteht keine Einschränkung auf die Regionen, die den Log Analytics-Arbeitsbereich unterstützen.
>

Wenn Sie nicht über einen Arbeitsbereich verfügen, können Sie ihn über das [Azure CLI](../../log-analytics/log-analytics-quick-create-workspace-cli.md), [PowerShell](../../log-analytics/log-analytics-quick-create-workspace-posh.md), das [Azure-Portal](../../log-analytics/log-analytics-quick-create-workspace.md) oder mithilfe von [Azure Resource Manager](../../log-analytics/log-analytics-template-workspace-configuration.md) erstellen.  Wenn Sie im Azure-Portal die Überwachung für eine einzelne Azure VM aktivieren, haben Sie die Option, während dieses Vorgangs einen Arbeitsbereich zu erstellen.  

Das Aktivieren der Lösung für das Szenario im großen Maßstab erfordert zunächst die folgenden Konfigurationen in Ihrem Log Analytics-Arbeitsbereich:

* Installieren Sie die Projektmappen **ServiceMap** und **InfrastructureInsights**. Dazu müssen Sie eine Azure Resource Manager-Vorlage verwenden, die in diesem Artikel bereitgestellt wird.   
* Konfigurieren Sie den Log Analytics-Arbeitsbereich zur Erfassung von Leistungsindikatoren.

Informationen zum Konfigurieren Ihres Arbeitsbereichs für das Szenario im großen Maßstab finden Sie unter [Einrichten des Log Analytics-Arbeitsbereichs für die Bereitstellung im großen Maßstab](#setup-log-analytics-workspace).

### <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

Die folgende Tabelle enthält die Windows- und Linux-Betriebssysteme, die für Azure Monitor for VMs unterstützt werden.  Eine vollständige Liste mit ausführlicheren Informationen zu den unterstützten größeren und kleineren Release- und Kernelversionen des Linux-Betriebssystems ist weiter unten in diesem Abschnitt angegeben.

|Betriebssystemversion |Leistung |Karten |Health |  
|-----------|------------|-----|-------|  
|Windows Server 2016 1803 | X | X | X |
|Windows Server 2016 | X | X | X |  
|Windows Server 2012 R2 | X | X | |  
|Windows Server 2012 | X | X | |  
|Windows Server 2008 R2 | X | X| |  
|RHEL 7, 6| X | X| X |  
|Ubuntu 18.04, 16.04, 14.04 | X | X | X |  
|Cent OS Linux 7, 6 | X | X | X |  
|SLES 12 | X | X | X |  
|Oracle Linux 7 | X<sup>1</sup> | | X |  
|Oracle Linux 6 | X | X | X |  
|Debian 9.4, 8 | X<sup>1</sup> | | X | 

<sup>1</sup> Das Leistungsfeature von Azure Monitor for VMs steht nur in Azure Monitor zur Verfügung, nicht aber beim direkten Zugriff über den linken Bereich der Azure-VM.  

>[!NOTE]
>Die folgenden Informationen gelten für die Unterstützung des Linux-Betriebssystems:  
> - Es werden nur die Standardversion und SMP-Version des Linux-Kernels unterstützt.  
> - Nicht-Standardversionen des Kernels, z.B. PAE und Xen, werden für keine Linux-Distribution unterstützt. Beispielsweise wird ein System mit der Versionszeichenfolge „2.6.16.21-0.8-xen“ nicht unterstützt.  
> - Benutzerdefinierte Kernels, einschließlich Neukompilierungen von Standardkernels, werden nicht unterstützt.  
> - Der CentOSPlus-Kernel wird nicht unterstützt.  


#### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| Betriebssystemversion | Kernelversion |
|:--|:--|
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |
| 7.3 | 3.10.0-514 |
| 7.4 | 3.10.0-693 |
| 7,5 | 3.10.0-862 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| Betriebssystemversion | Kernelversion |
|:--|:--|
| 6,0 | 2.6.32-71 |
| 6.1 | 2.6.32-131 |
| 6.2 | 2.6.32-220 |
| 6.3 | 2.6.32-279 |
| 6.4. | 2.6.32-358 |
| 6,5 | 2.6.32-431 |
| 6.6 | 2.6.32-504 |
| 6.7 | 2.6.32-573 |
| 6,8 | 2.6.32-642 |
| 6.9 | 2.6.32-696 |

#### <a name="ubuntu-server"></a>Ubuntu Server

| Betriebssystemversion | Kernelversion |
|:--|:--|
| Ubuntu 18.04 | Kernel 4.15.* |
| Ubuntu 16.04.3 | Kernel 4.15.* |
| 16.04 | 4.4.\*<br>4.8.\*<br>4.10.\*<br>4.11.\*<br>4.13.\* |
| 14.04 | 3.13.\*<br>4.4.\* |

#### <a name="oracle-enterprise-linux-6-with-unbreakable-enterprise-kernel"></a>Oracle Enterprise Linux 6 mit Unbreakable Enterprise Kernel
| Betriebssystemversion | Kernelversion
|:--|:--|
| 6.2 | Oracle 2.6.32-300 (UEK R1) |
| 6.3 | Oracle 2.6.39-200 (UEK R2) |
| 6.4. | Oracle 2.6.39-400 (UEK R2) |
| 6,5 | Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-enterprise-linux-5-with-unbreakable-enterprise-kernel"></a>Oracle Enterprise Linux 5 mit Unbreakable Enterprise Kernel

| Betriebssystemversion | Kernelversion
|:--|:--|
| 5.10 | Oracle 2.6.39-400 (UEK R2) |
| 5.11 | Oracle 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-12-enterprise-server"></a>SUSE Linux 12 Enterprise Server

| Betriebssystemversion | Kernelversion
|:--|:--|
|12 SP2 | 4.4.* |
|12 SP3 | 4.4.* |

### <a name="microsoft-dependency-agent"></a>Microsoft Dependency-Agent
Das Zuordnungsfeature in Azure Monitor for VMs erhält seine Daten vom Microsoft Dependency-Agent. Der Dependency Agent baut für seine Verbindung mit Log Analytics auf dem Log Analytics-Agent auf, daher muss auf einem System zusammen mit dem Dependency-Agent der Log Analytics-Agent installiert und konfiguriert sein. Wenn Sie Azure Monitor für VMs für eine einzelne Azure-VM aktivieren oder die Methoden für die Bereitstellung im großen Maßstab verwenden, wird die Azure-VM-Erweiterung für den Depency-Agent verwendet, um den Agent im Rahmen des Onboardings zu installieren. In einer Hybridumgebung kann der Dependency-Agent manuell oder mithilfe einer automatisierten Bereitstellungsmethode auf die außerhalb von Azure gehosteten VMs heruntergeladen und installiert werden.  

In der folgenden Tabelle sind die verbundenen Quellen beschrieben, die vom Zuordnungsfeature in einer Hybridumgebung unterstützt werden.

| Verbundene Quelle | Unterstützt | BESCHREIBUNG |
|:--|:--|:--|
| Windows-Agents | JA | Zusätzlich zum [Log Analytics-Agent für Windows](../../azure-monitor/platform/log-analytics-agent.md) erfordern Windows-Agents den Microsoft Dependency-Agent. Eine vollständige Liste der Betriebssystemversionen finden Sie unter [Unterstützte Betriebssysteme](#supported-operating-systems). |
| Linux-Agents | JA | Zusätzlich zum [Log Analytics-Agent für Linux](../../azure-monitor/platform/log-analytics-agent.md) erfordern Linux-Agents den Microsoft Dependency-Agent. Eine vollständige Liste der Betriebssystemversionen finden Sie unter [Unterstützte Betriebssysteme](#supported-operating-systems). |
| System Center Operations Manager-Verwaltungsgruppe | Nein  | |  

Der Dependency-Agent kann von den folgenden Speicherorten heruntergeladen werden.

| Datei | Betriebssystem | Version | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.7.1 | 55030ABF553693D8B5112569FB2F97D7C54B66E9990014FC8CC43EFB70DE56C6 |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.7.1 | 43C75EF0D34471A0CBCE5E396FFEEF4329C9B5517266108FA5D6131A353D29FE |

## <a name="role-based-access-control"></a>Rollenbasierte Zugriffssteuerung
Der folgende Zugriff muss Ihren Benutzern gewährt werden, damit sie Funktionen in Azure Monitor für VMs aktivieren und auf diese zugreifen können.  
  
- Um die Lösung zu aktivieren, müssen Sie als Mitglied der Rolle „Log Analytics-Mitwirkender“ hinzugefügt werden.  

- Um die Leistungs-, Integritäts- und Zuordnungsdaten anzuzeigen, müssen Sie als Mitglied der Rolle „Überwachungsleser“ für die Azure-VM und den mit Azure Monitor für VMs konfigurierten Log Analytics-Arbeitsbereich hinzugefügt werden.   

Weitere Informationen zur Zugriffssteuerung auf einen Log Analytics-Arbeitsbereich finden Sie unter [Verwalten von Arbeitsbereichen](../../log-analytics/log-analytics-manage-access.md).

## <a name="enable-from-the-azure-portal"></a>Aktivieren im Azure-Portal
Gehen Sie wie folgt vor, um die Überwachung Ihrer Azure-VM im Azure-Portal zu aktivieren:

1. Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an. 
2. Wählen Sie im Azure-Portal die Option **Virtual Machines** aus. 
3. Wählen Sie einen virtuellen Computer in der Liste aus. 
4. Wählen Sie auf der Seite der VM im Abschnitt **Überwachung** den Eintrag **Insights (Vorschau)** aus.
5. Wählen Sie auf der Seite **Insights (Vorschau)** **Jetzt testen** aus.

    ![Aktivieren von Azure Monitor for VMs für eine VM](./media/vminsights-onboard/enable-vminsights-vm-portal-01.png)

5. Wenn Sie im selben Abonnement einen Log Analytics-Arbeitsbereich haben, wählen Sie ihn auf der Seite **Azure Monitor Insights Onboarding** (Onboarding von Azure Monitor Insights) in der Dropdownliste aus.  Mit der Liste werden der Standardarbeitsbereich und der Speicherort, an dem die VM im Abonnement bereitgestellt wird, vorausgewählt. 

    >[!NOTE]
    >Befolgen Sie die Anweisungen unter [Erstellen eines Log Analytics-Arbeitsbereichs](../../log-analytics/log-analytics-quick-create-workspace.md) für eine der oben aufgeführten unterstützten Regionen, wenn Sie einen neuen Log Analytics-Arbeitsbereich zum Speichern der Überwachungsdaten aus der VM erstellen möchten.   

Nach dem Aktivieren der Überwachung kann es ca. 10 Minuten dauern, bis Integritätsmetriken für die VM angezeigt werden. 

![Aktivieren von Azure Monitor for VMs – Verarbeiten der Überwachungsbereitstellung](./media/vminsights-onboard/onboard-vminsights-vm-portal-status.png)


## <a name="on-boarding-at-scale"></a>Onboarding im großen Maßstab
Dieser Abschnitt bietet Anweisungen zum Ausführen der Bereitstellung von Azure Monitor für VMs im großen Maßstab, mithilfe von Azure Policy oder Azure PowerShell.  

Im Folgenden sind die Schritte zusammengefasst, die Sie vor dem Onboarding Ihrer VMs ausführen müssen, um den Log Analytics-Arbeitsbereich vorab zu konfigurieren.

1. Erstellen Sie einen neuen Arbeitsbereich, sofern noch kein Arbeitsbereich vorhanden ist, der zur Unterstützung von Azure Monitor für VMs verwendet werden kann. Lesen Sie vor dem Erstellen eines neuen Arbeitsbereichs den Artikel [Verwalten von Arbeitsbereichen](../../log-analytics/log-analytics-manage-access.md?toc=/azure/azure-monitor/toc.json), um sich mit den Kosten, der Verwaltung und den Konformitätsanforderungen vertraut zu machen.       
2. Aktivieren Sie im Arbeitsbereich Leistungsindikatoren für die Erfassung auf Linux- und Windows-VMs.
3. Installieren und aktivieren Sie die Projektmappen **ServiceMap** und **InfrastructureInsights** in Ihrem Arbeitsbereich.  

### <a name="setup-log-analytics-workspace"></a>Setup des Log Analytics-Arbeitsbereichs
Wenn Sie nicht über einen Log Analytics-Arbeitsbereich verfügen, gehen Sie die im Abschnitt [Voraussetzungen](#log-analytics) vorgeschlagenen Methoden durch, um einen zu erstellen.  

#### <a name="enable-performance-counters"></a>Aktivieren von Leistungsindikatoren
Wenn der Log Analytics-Arbeitsbereich, auf den die Lösung verweist, noch nicht für die Erfassung der von der Lösung benötigten Leistungsindikatoren konfiguriert ist, müssen Sie diese aktivieren. Dies kann manuell erfolgen, wie [hier](../../azure-monitor/platform/data-sources-performance-counters.md) beschrieben, oder durch Herunterladen und Ausführen eines PowerShell-Skripts, das im [Azure Powershell-Katalog](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1) verfügbar ist.
 
#### <a name="install-the-servicemap-and-infrastructureinsights-solutions"></a>Installieren der Projektmappen „ServiceMap“ und „InfrastructureInsights“
Diese Methode beinhaltet eine JSON-Vorlage, die die Konfiguration angibt, um die Lösungskomponenten für Ihren Log Analytics-Arbeitsbereich zu aktivieren.  

Wenn Sie mit der Bereitstellung von Ressourcen mithilfe einer Vorlage nicht vertraut sind, finden Sie weitere Informationen unter:
* [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md)
* [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure CLI](../../azure-resource-manager/resource-group-template-deploy-cli.md) 

Wenn Sie die Azure CLI verwenden möchten, müssen Sie sie zuerst installieren und lokal verwenden. Sie benötigen Azure CLI 2.0.27 oder höher. Um Ihre Version zu ermitteln, führen Sie `az --version` aus. Informationen zur Installation und zum Upgrade von Azure CLI finden Sie unter [Installieren von Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). 

1. Kopieren Sie die folgende JSON-Syntax, und fügen Sie sie in Ihre Datei ein:

    ```json
    {

    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "WorkspaceName": {
            "type": "string"
        },
        "WorkspaceLocation": {
            "type": "string"
        }
    },
    "resources": [
        {
            "apiVersion": "2017-03-15-preview",
            "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('WorkspaceName')]",
            "location": "[parameters('WorkspaceLocation')]",
            "resources": [
                {
                    "apiVersion": "2015-11-01-preview",
                    "location": "[parameters('WorkspaceLocation')]",
                    "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                    "type": "Microsoft.OperationsManagement/solutions",
                    "dependsOn": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    ],
                    "properties": {
                        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    },

                    "plan": {
                        "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                        "publisher": "Microsoft",
                        "product": "[Concat('OMSGallery/', 'ServiceMap')]",
                        "promotionCode": ""
                    }
                },
                {
                    "apiVersion": "2015-11-01-preview",
                    "location": "[parameters('WorkspaceLocation')]",
                    "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                    "type": "Microsoft.OperationsManagement/solutions",
                    "dependsOn": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    ],
                    "properties": {
                        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    },
                    "plan": {
                        "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                        "publisher": "Microsoft",
                        "product": "[Concat('OMSGallery/', 'InfrastructureInsights')]",
                        "promotionCode": ""
                    }
                }
            ]
        }
    ]
    ```

2. Speichern Sie diese Datei als **installsolutionsforvminsights.json** in einem lokalen Ordner.
3. Bearbeiten Sie die Werte für **WorkspaceName**, **ResourceGroupName** und **WorkspaceLocation**.  Der Wert für **WorkspaceName** ist die vollständige Ressourcen-ID des Log Analytics-Arbeitsbereichs, die den Namen des Arbeitsbereichs beinhaltet, und der Wert für **WorkspaceLocation** ist die Region, in der der Arbeitsbereich definiert ist.
4. Sie können diese Vorlage mithilfe des folgenden PowerShell-Befehls bereitstellen:

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
    ```

    Die Änderung der Konfiguration kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Meldung angezeigt, die der folgenden ähnelt und das Ergebnis anzeigt:

    ```powershell
    provisioningState       : Succeeded
    ```

### <a name="enable-using-azure-policy"></a>Aktivieren mithilfe von Azure Policy
Um Azure Monitor für VMs im großen Maßstab in einer Weise bereitzustellen, die durchgehende Compliance und automatische Aktivierung für neu bereitgestellte VMs sicherstellt, wird die Verwendung von [Azure Policy](../../azure-policy/azure-policy-introduction.md) empfohlen. Diese Richtlinien:

* Bereitstellen von Log Analytics-Agent und Dependency-Agent 
* Complianceergebnisse werden gemeldet 
* Wartung für nicht konforme VMs

Für das Aktivieren von Azure Monitor für VMs in Ihrem Mandanten mithilfe von Richtlinien ist Folgendes erforderlich: 

- Zuweisen der Initiative zu einem Bereich – Verwaltungsgruppe, Abonnement oder Ressourcengruppe 
- Überprüfung und Wartung der Compliance-Ergebnisse  

Weitere Informationen zur Zuweisung von Azure Policy finden Sie unter [Azure Policy – Übersicht](../../governance/policy/overview.md#policy-assignment), und arbeiten Sie die [Übersicht zu Verwaltungsgruppen](../../governance/management-groups/index.md) durch, bevor Sie fortfahren.  

In der folgenden Tabelle sind die bereitgestellten Richtliniendefinitionen aufgelistet.  

|NAME |BESCHREIBUNG |Typ |  
|-----|------------|-----|  
|[Vorschau]: Aktivieren von Azure Monitor für VMs |Hiermit aktivieren Sie Azure Monitor für die virtuellen Computer (VMs) in dem angegebenen Bereich (Verwaltungsgruppe, Abonnement oder Ressourcengruppe). Akzeptiert den Log Analytics-Arbeitsbereich als Parameter. |Initiative |  
|[Vorschau]: Überwachen der Bereitstellung des Dependency-Agents – VM-Image (Betriebssystem) nicht aufgelistet |Meldet VMs als nicht konform, wenn das VM-Image (Betriebssystem) nicht in der definierten Liste vorhanden und der Agent nicht installiert ist. |Richtlinie |  
|[Vorschau]: Überwachen der Bereitstellung des Log Analytics-Agents – VM-Image (Betriebssystem) nicht aufgelistet |Meldet VMs als nicht konform, wenn das VM-Image (Betriebssystem) nicht in der definierten Liste vorhanden und der Agent nicht installiert ist. |Richtlinie |  
|[Vorschau]: Bereitstellen des Dependency-Agents für Linux-VMs |Stellen Sie den Dependency-Agent für Linux-VMs bereit, wenn das VM-Image (Betriebssystem) in der definierten Liste vorhanden und der Agent nicht installiert ist. |Richtlinie |  
|[Vorschau]: Bereitstellen des Dependency-Agents für Windows VMs |Stellen Sie den Dependency-Agent für Windows-VMs bereit, wenn das VM-Image (Betriebssystem) in der definierten Liste vorhanden und der Agent nicht installiert ist. |Richtlinie |  
|[Vorschau]: Bereitstellen des Log Analytics-Agents für Linux-VMs |Stellen Sie den Log Analytics-Agent für Linux-VMs bereit, wenn das VM-Image (Betriebssystem) in der definierten Liste vorhanden und der Agent nicht installiert ist. |Richtlinie |  
|[Vorschau]: Bereitstellen des Log Analytics-Agents für Windows-VMs |Stellen Sie den Log Analytics-Agent für Windows-VMs bereit, wenn das VM-Image (Betriebssystem) in der definierten Liste vorhanden und der Agent nicht installiert ist. |Richtlinie |  

Eigenständige Richtlinie (nicht in der Initiative enthalten) 

|NAME |BESCHREIBUNG |Typ |  
|-----|------------|-----|  
|[Vorschau]: Überwachen des Log Analytics-Arbeitsbereichs für VM – Berichtskonflikt |Meldet VMs als nicht konform, wenn sie keine Protokolle an den in der Richtlinien-/Initiativenzuweisung angegebenen Log Analytics-Arbeitsbereich senden. |Richtlinie |

#### <a name="assign-azure-monitor-initiative"></a>Zuweisen der Azure Monitor-Initiative
In dieser ursprünglichen Version können Sie die Richtlinienzuweisung nur im Azure-Portal erstellen. Informationen zum Ausführen dieser Schritte finden Sie unter  [Erstellen einer Richtlinienzuweisung im Azure-Portal](../../governance/policy/assign-policy-portal.md). 

1. Starten Sie den Azure Policy-Dienst über das Azure-Portal, indem Sie auf **Alle Dienste** klicken und dann nach **Richtlinie** suchen und die entsprechende Option auswählen. 
2. Wählen Sie links auf der Seite „Azure Policy“ die Option **Zuweisungen**. Eine Zuweisung ist eine zugewiesene Richtlinie, die innerhalb eines bestimmten Bereichs angewendet werden soll.
3. Wählen Sie im oberen Bereich der Seite **Richtlinien – Zuweisungen** die Option **Initiative zuweisen** aus.
4. Wählen Sie auf der Seite **Initiative zuweisen** den **Bereich** aus, indem Sie auf die Auslassungspunkte klicken und eine Verwaltungsgruppe oder ein Abonnement und optional eine Ressourcengruppe auswählen. Ein Bereich schränkt die Richtlinienzuweisung in unserem Fall zur Durchsetzung auf eine Gruppierung virtueller Computer ein. Klicken Sie unten auf der Seite **Bereich** auf **Auswählen**, um Ihre Änderungen zu speichern.
5. Mithilfe von **Ausschlüssen** können Sie optional eine oder mehrere Ressourcen aus dem Bereich ausschließen. 
6. Wählen Sie die Auslassungspunkte **Initiativendefinition** aus, um die Liste der verfügbaren Definitionen zu öffnen, und wählen Sie dann **[Vorschau] Azure Monitor für VMs aktivieren** in der Liste aus, und klicken Sie auf **Auswählen**.
7. Der **Zuweisungsname** wird automatisch mit dem Namen der von Ihnen ausgewählten Initiative aufgefüllt, Sie können ihn aber ändern. Geben Sie ggf. auch eine **Beschreibung** ein. **Zugewiesen von** wird automatisch mit dem angemeldeten Benutzer aufgefüllt; dieses Feld ist optional.
8. Wählen Sie in der Dropdownliste, die in der unterstützen Region verfügbar ist, einen **Log Analytics-Arbeitsbereich** aus.

    >[!NOTE]
    >Wenn der Arbeitsbereich außerhalb des Bereichs der Zuweisung liegt, müssen Sie der Prinzipal-ID der Richtlinienzuweisung **Log Analytics-Mitwirkender**-Berechtigungen erteilen. Geschieht dies nicht, tritt möglicherweise ein Fehler bei der Bereitstellung auf, z.B. dieser: `The client '343de0fe-e724-46b8-b1fb-97090f7054ed' with object id '343de0fe-e724-46b8-b1fb-97090f7054ed' does not have authorization to perform action 'microsoft.operationalinsights/workspaces/read' over scope ... ` Unter [how to manually configure the managed identity](../../governance/policy/how-to/remediate-resources.md#manually-configure-the-managed-identity) (Manuelle Konfiguration der verwalteten Identität) finden Sie Informationen zum Erteilen des Zugriffs.
    >

9. Beachten Sie, dass die Option **Verwaltete Identität** aktiviert ist. Sie ist aktiviert, wenn die zuzuweisende Initiative eine Richtlinie mit dem Effekt „deployIfNotExists“ enthält. Wählen Sie in der Dropdownliste **Manage Identity location** (Speicherort der Identität verwalten) die passende Region aus.  
10. Klicken Sie auf **Zuweisen**.

#### <a name="review-and-remediate-the-compliance-results"></a>Überprüfen und Warten der Compliance-Ergebnisse 

Informationen zum Überprüfen von Compliance-Ergebnissen finden Sie unter [identify non-compliance results](../../governance/policy/assign-policy-portal.md#identify-non-compliant-resources) (Erkennen von Nichtkompatibilitätsergebnissen). Wählen Sie links auf der Seite **Compliance** aus, und suchen Sie die Initiative **[Vorschau] Azure Monitor für VMs aktivieren**, die nicht mit der von Ihnen erstellten Zuweisung kompatibel ist.

![Richtlinienkonformität für Azure-VMs](./media/vminsights-onboard/policy-view-compliance-01.png)

Auf der Grundlage der in der Initiative enthaltenen Richtlinien werden VMs in den folgenden Szenarien als nicht kompatibel gemeldet:  
  
1. Log Analytics oder Dependency-Agent wurde nicht bereitgestellt.  
   Dies ist typisch für einen Bereich mit vorhandenen virtuellen Computern. Um das Problem abzumildern, [erstellen Sie Korrekturaufgaben](../../governance/policy/how-to/remediate-resources.md) für eine nicht konforme Richtlinie, um die erforderlichen Agents bereitzustellen.    
 
    - [Vorschau]: Deploy Dependency Agent for Linux VMs   
    - [Vorschau]: Deploy Dependency Agent for Windows VMs  
    - [Vorschau]: Deploy Log Analytics Agent for Linux VMs  
    - [Vorschau]: Deploy Log Analytics Agent for Windows VMs  

2. Das VM-Image (Betriebssystem) befindet sich nicht in der Liste, die bei der Richtliniendefinition identifiziert wurde.  
   Die Kriterien der Bereitstellungsrichtlinie schließen nur VMs ein, die aus bekannten Azure VM-Images bereitgestellt werden. Überprüfen Sie anhand der Dokumentation, ob das Betriebssystem der VM unterstützt wird oder nicht. Ist das nicht der Fall, müssen Sie die Bereitstellungsrichtlinie duplizieren und sie aktualisieren/ändern, um das Image kompatibel zu machen. 
  
    - [Vorschau]: Überwachen der Bereitstellung des Dependency-Agents – VM-Image (Betriebssystem) nicht aufgelistet  
    - [Vorschau]: Überwachen der Bereitstellung des Log Analytics-Agents – VM-Image (Betriebssystem) nicht aufgelistet

3. VMs protokollieren nicht in den angegebenen LA-Arbeitsbereich.  
Es ist möglich, dass einige VMs im Bereich der Initiative ihre Protokolle in einem anderen als dem in der Richtlinienzuordnung angegebenen LA-Arbeitsbereich speichern. Diese Richtlinie ist ein Tool, um zu bestimmen, welche VMs an einen nicht kompatiblen Arbeitsbereich berichten.  
 
    - [Vorschau]: Audit Log Analytics Workspace for VM - Report Mismatch  

### <a name="enable-with-powershell"></a>Aktivieren mithilfe von PowerShell
Um Azure Monitor für VMs für mehrere VMs oder VM-Skalierungsgruppen zu aktivieren, können Sie ein bereitgestelltes PowerShell-Skript – [Install-VMInsights.ps1](https://www.powershellgallery.com/packages/Install-VMInsights/1.0) – verwenden, das im Azure PowerShell-Katalog verfügbar ist.  Dieses Skript durchläuft alle virtuellen Computer und VM-Skalierungsgruppen in Ihrem Abonnement in der bereichsbezogenen Ressourcengruppe gemäß der Angabe in *ResourceGroup* oder eine einzelne VM oder VM-Skalierungsgruppe, die in *Name* angegeben ist.  Für jede VM oder VM-Skalierungsgruppe überprüft das Skript, ob die VM-Erweiterung bereits installiert ist, und versucht, sie zu installieren, wenn das nicht der Fall ist.  Andernfalls fährt sie mit der Installation der Log Analytics- und Dependency-Agent-VM-Erweiterungen fort.   

Das Skript benötigt mindestens die Azure PowerShell-Modulversion 5.7.0. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.

Wenn Sie Hilfe zum Skript benötigen, können Sie durch Ausführen von `Get-Help` eine Liste mit Argumentdetails und Syntaxbeispielen anzeigen.   

```powershell
Get-Help .\Install-VMInsights.ps1 -Detailed

SYNOPSIS
    This script installs VM extensions for Log Analytics and Dependency Agent as needed for VM Insights.


SYNTAX
    .\Install-VMInsights.ps1 [-WorkspaceId] <String> [-WorkspaceKey] <String> [-SubscriptionId] <String> [[-ResourceGroup]
    <String>] [[-Name] <String>] [[-PolicyAssignmentName] <String>] [-ReInstall] [-TriggerVmssManualVMUpdate] [-Approve] [-WorkspaceRegion] <String>
    [-WhatIf] [-Confirm] [<CommonParameters>]


DESCRIPTION
    This script installs or re-configures following on VM's and VM Scale Sets:
    - Log Analytics VM Extension configured to supplied Log Analytics Workspace
    - Dependency Agent VM Extension

    Can be applied to:
    - Subscription
    - Resource Group in a Subscription
    - Specific VM/VM Scale Set
    - Compliance results of a policy for a VM or VM Extension

    Script will show you list of VM's/VM Scale Sets that will apply to and let you confirm to continue.
    Use -Approve switch to run without prompting, if all required parameters are provided.

    If the extensions are already installed will not install again.
    Use -ReInstall switch if you need to for example update the workspace.

    Use -WhatIf if you would like to see what would happen in terms of installs, what workspace configured to, and status of the extension.


PARAMETERS
    -WorkspaceId <String>
        Log Analytics WorkspaceID (GUID) for the data to be sent to

    -WorkspaceKey <String>
        Log Analytics Workspace primary or secondary key

    -SubscriptionId <String>
        SubscriptionId for the VMs/VM Scale Sets
        If using PolicyAssignmentName parameter, subscription that VM's are in

    -ResourceGroup <String>
        <Optional> Resource Group to which the VMs or VM Scale Sets belong to

    -Name <String>
        <Optional> To install to a single VM/VM Scale Set

    -PolicyAssignmentName <String>
        <Optional> Take the input VM's to operate on as the Compliance results from this Assignment
        If specified will only take from this source.

    -ReInstall [<SwitchParameter>]
        <Optional> If VM/VM Scale Set is already configured for a different workspace, set this to change to the new workspace

    -TriggerVmssManualVMUpdate [<SwitchParameter>]
        <Optional> Set this flag to trigger update of VM instances in a scale set whose upgrade policy is set to Manual

    -Approve [<SwitchParameter>]
        <Optional> Gives the approval for the installation to start with no confirmation prompt for the listed VM's/VM Scale Sets

    -WorkspaceRegion <String>
        Region the Log Analytics Workspace is in
        Suported values: "East US","eastus","Southeast Asia","southeastasia","West Central US","westcentralus","West Europe","westeurope"
        For Health supported is: "East US","eastus","West Central US","westcentralus"

    -WhatIf [<SwitchParameter>]
        <Optional> See what would happen in terms of installs.
        If extension is already installed will show what workspace is currently configured, and status of the VM extension

    -Confirm [<SwitchParameter>]
        <Optional> Confirm every action

    <CommonParameters>
        This cmdlet supports the common parameters: Verbose, Debug,
        ErrorAction, ErrorVariable, WarningAction, WarningVariable,
        OutBuffer, PipelineVariable, and OutVariable. For more information, see
        about_CommonParameters (https:/go.microsoft.com/fwlink/?LinkID=113216).

    -------------------------- EXAMPLE 1 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -ResourceGroup <ResourceGroup>

    Install for all VM's in a Resource Group in a subscription

    -------------------------- EXAMPLE 2 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -ResourceGroup <ResourceGroup> -ReInstall

    Specify to ReInstall extensions even if already installed, for example to update to a different workspace

    -------------------------- EXAMPLE 3 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -PolicyAssignmentName a4f79f8ce891455198c08736 -ReInstall

    Specify to use a PolicyAssignmentName for source, and to ReInstall (move to a new workspace)
```

Das folgende Beispiel veranschaulicht die Verwendung der PowerShell-Befehle im Ordner, um Azure Monitor for VMs zu aktivieren und die erwartete Ausgabe zu verstehen:

```powershell
$WorkspaceId = "<GUID>"
$WorkspaceKey = "<Key>"
$SubscriptionId = "<GUID>"
.\Install-VMInsights.ps1 -WorkspaceId $WorkspaceId -WorkspaceKey $WorkspaceKey -SubscriptionId $SubscriptionId -WorkspaceRegion eastus

Getting list of VM's or VM ScaleSets matching criteria specified

VM's or VM ScaleSets matching criteria:

db-ws-1 VM running
db-ws2012 VM running

This operation will install the Log Analytics and Dependency Agent extensions on above 2 VM's or VM Scale Sets.
VM's in a non-running state will be skipped.
Extension will not be re-installed if already installed. Use -ReInstall if desired, for example to update workspace

Confirm
Continue?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y

db-ws-1 : Deploying DependencyAgentWindows with name DAExtension
db-ws-1 : Successfully deployed DependencyAgentWindows
db-ws-1 : Deploying MicrosoftMonitoringAgent with name MMAExtension
db-ws-1 : Successfully deployed MicrosoftMonitoringAgent
db-ws2012 : Deploying DependencyAgentWindows with name DAExtension
db-ws2012 : Successfully deployed DependencyAgentWindows
db-ws2012 : Deploying MicrosoftMonitoringAgent with name MMAExtension
db-ws2012 : Successfully deployed MicrosoftMonitoringAgent

Summary:

Already Onboarded: (0)

Succeeded: (4)
db-ws-1 : Successfully deployed DependencyAgentWindows
db-ws-1 : Successfully deployed MicrosoftMonitoringAgent
db-ws2012 : Successfully deployed DependencyAgentWindows
db-ws2012 : Successfully deployed MicrosoftMonitoringAgent

Connected to different workspace: (0)

Not running - start VM to configure: (0)

Failed: (0)
```

## <a name="enable-for-hybrid-environment"></a>Aktivieren für Hybridumgebungen
In diesem Abschnitt wird erläutert, wie das Onboarding von VMs oder physischen Computern, die in Ihrem Rechenzentrum oder einer Cloudumgebung gehostet sind, für die Überwachung mithilfe von Azure Monitor for VMs erfolgt.  

Der Dependency-Agent von Azure Monitor for VMs Map selbst überträgt keine Daten und erfordert keine Änderungen an Firewalls oder Ports. Die Zuordnungsdaten werden immer vom Log Analytics-Agent an den Azure Monitor-Dienst übertragen, entweder direkt oder über das [OMS-Gateway](../../azure-monitor/platform/gateway.md), falls Ihre IT-Sicherheitsrichtlinien es nicht zulassen, dass Computer im Netzwerk eine Verbindung mit dem Internet herstellen.

Überprüfen Sie die Anforderungen und Bereitstellungsmethoden für den [Log Analytics-Linux- und -Windows-Agent](../../azure-monitor/platform/log-analytics-agent.md).  

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

Zusammenfassung der Schritte:

1. Installieren Sie den Log Analytics-Agent für Windows oder Linux
2. Laden Sie den Dependency-Agent für das Zuordnungsfeature von Azure Monitor für VMs für [Windows](https://aka.ms/dependencyagentwindows) oder [Linux](https://aka.ms/dependencyagentlinux) herunter, und installieren Sie ihn.
3. Aktivieren Sie die Erfassung von Leistungsindikatoren
4. Führen Sie das Onboarding von Azure Monitor for VMs durch

### <a name="install-the-dependency-agent-on-windows"></a>Installieren des Dependency-Agents unter Windows 
Der Dependency-Agent kann durch Ausführen von `InstallDependencyAgent-Windows.exe` manuell auf Windows-Computern installiert werden. Wenn Sie diese ausführbare Datei ohne Optionen ausführen, wird ein Setup-Assistent gestartet, mit dem Sie die Installation interaktiv durchführen können.  

>[!NOTE]
>Zum Installieren oder Deinstallieren des Agent sind Administratorrechte erforderlich.

Die folgende Tabelle hebt die spezifischen Parameter hervor, die von Setup für den Agent auf der Befehlszeile unterstützt werden.  

| Parameter | BESCHREIBUNG |
|:--|:--|
| /? | Gibt eine Liste der Befehlszeilenoptionen zurück. |
| /S | Führt eine automatische Installation ohne Benutzereingriff aus. |

Um das Installationsprogramm beispielsweise mit dem Parameter `/?` auszuführen, geben Sie `InstallDependencyAgent-Windows.exe /?` ein.

Dateien für den Windows Dependency-Agent werden standardmäßig in `C:\Program Files\Microsoft Dependency Agent` installiert.  Falls der Dependency-Agent nach dem Abschluss von Setup nicht gestartet wird, suchen Sie in den Protokollen nach ausführlichen Fehlerinformationen. Das Protokollverzeichnis ist `%Programfiles%\Microsoft Dependency Agent\logs`. 

### <a name="install-the-dependency-agent-on-linux"></a>Installieren des Dependency-Agents unter Linux
Der Dependency-Agent wird auf Linux-Servern mit `InstallDependencyAgent-Linux64.bin` installiert, einem Shellskript mit einer selbstextrahierenden Binärdatei. Sie können die Datei mit `sh` ausführen oder der Datei selbst Ausführungsberechtigungen hinzufügen.

>[!NOTE]
> Zum Installieren oder Konfigurieren des Agent ist Root-Zugriff erforderlich.
> 

| Parameter | BESCHREIBUNG |
|:--|:--|
| -help | Ruft eine Liste der Befehlszeilenoptionen ab. |
| -s | Führt eine automatische Installation ohne Benutzereingaben aus. |
| --check | Überprüft Berechtigungen und das Betriebssystem, ohne den Agent zu installieren. |

Um das Installationsprogramm beispielsweise mit dem Parameter `-help` auszuführen, geben Sie `InstallDependencyAgent-Linux64.bin -help` ein.

Installieren Sie den Dependency-Agent für Linux mit Root-Berechtigungen, indem Sie den folgenden Befehl ausführen: `sh InstallDependencyAgent-Linux64.bin`.
    
Falls der Dependency-Agent nicht gestartet wird, suchen Sie in den Protokollen nach ausführlichen Fehlerinformationen. Für Linux-Agents lautet das Protokollverzeichnis `/var/opt/microsoft/dependency-agent/log`.

Dateien für den Dependency-Agent werden in den folgenden Verzeichnissen gespeichert:

| Dateien | Standort |
|:--|:--|
| Hauptdateien | /opt/microsoft/dependency-agent |
| Protokolldateien | /var/opt/microsoft/dependency-agent/log |
| Konfigurationsdateien | /etc/opt/microsoft/dependency-agent/config |
| Ausführbare Dienstdateien | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| Binäre Speicherdateien | /var/opt/microsoft/dependency-agent/storage |

### <a name="enable-performance-counters"></a>Aktivieren von Leistungsindikatoren
Wenn der Log Analytics-Arbeitsbereich, auf den die Lösung verweist, noch nicht für die Erfassung der von der Lösung benötigten Leistungsindikatoren konfiguriert ist, müssen Sie diese aktivieren. Dies kann manuell erfolgen, wie [hier](../../azure-monitor/platform/data-sources-performance-counters.md) beschrieben, oder durch Herunterladen und Ausführen eines PowerShell-Skripts, das im [Azure Powershell-Katalog](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1) verfügbar ist.
 
### <a name="onboard-azure-monitor-for-vms"></a>Führen Sie das Onboarding von Azure Monitor for VMs durch
Diese Methode beinhaltet eine JSON-Vorlage, die die Konfiguration angibt, um die Lösungskomponenten für Ihren Log Analytics-Arbeitsbereich zu aktivieren.  

Wenn Sie mit der Bereitstellung von Ressourcen mithilfe einer Vorlage nicht vertraut sind, finden Sie weitere Informationen unter:
* [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md)
* [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure CLI](../../azure-resource-manager/resource-group-template-deploy-cli.md) 

Wenn Sie die Azure CLI verwenden möchten, müssen Sie sie zuerst installieren und lokal verwenden. Sie benötigen Azure CLI 2.0.27 oder höher. Um Ihre Version zu ermitteln, führen Sie `az --version` aus. Informationen zur Installation und zum Upgrade von Azure CLI finden Sie unter [Installieren von Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). 

#### <a name="create-and-execute-a-template"></a>Erstellen und Ausführen von Vorlagen

1. Kopieren Sie die folgende JSON-Syntax, und fügen Sie sie in Ihre Datei ein:

    ```json
    {

    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "WorkspaceName": {
            "type": "string"
        },
        "WorkspaceLocation": {
            "type": "string"
        }
    },
    "resources": [
        {
            "apiVersion": "2017-03-15-preview",
            "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('WorkspaceName')]",
            "location": "[parameters('WorkspaceLocation')]",
            "resources": [
                {
                    "apiVersion": "2015-11-01-preview",
                    "location": "[parameters('WorkspaceLocation')]",
                    "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                    "type": "Microsoft.OperationsManagement/solutions",
                    "dependsOn": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    ],
                    "properties": {
                        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    },

                    "plan": {
                        "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                        "publisher": "Microsoft",
                        "product": "[Concat('OMSGallery/', 'ServiceMap')]",
                        "promotionCode": ""
                    }
                },
                {
                    "apiVersion": "2015-11-01-preview",
                    "location": "[parameters('WorkspaceLocation')]",
                    "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                    "type": "Microsoft.OperationsManagement/solutions",
                    "dependsOn": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    ],
                    "properties": {
                        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    },
                    "plan": {
                        "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                        "publisher": "Microsoft",
                        "product": "[Concat('OMSGallery/', 'InfrastructureInsights')]",
                        "promotionCode": ""
                    }
                }
            ]
        }
    ]
    ```

2. Speichern Sie diese Datei als **installsolutionsforvminsights.json** in einem lokalen Ordner.
3. Bearbeiten Sie die Werte für **WorkspaceName**, **ResourceGroupName** und **WorkspaceLocation**.  Der Wert für **WorkspaceName** ist die vollständige Ressourcen-ID des Log Analytics-Arbeitsbereichs, die den Namen des Arbeitsbereichs beinhaltet, und der Wert für **WorkspaceLocation** ist die Region, in der der Arbeitsbereich definiert ist.
4. Sie können diese Vorlage mithilfe des folgenden PowerShell-Befehls bereitstellen:

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
    ```

    Die Änderung der Konfiguration kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Meldung angezeigt, die der folgenden ähnelt und das Ergebnis anzeigt:

    ```powershell
    provisioningState       : Succeeded
    ```
Nach dem Aktivieren der Überwachung kann es ca. 10 Minuten dauern, bis Integritätsstatus und Metriken für den Hybridcomputer angezeigt werden. 

## <a name="performance-counters-enabled"></a>Aktivierte Leistungsindikatoren
Azure Monitor for VMs konfiguriert einen Log Analytics-Arbeitsbereich, um die von der Lösung verwendeten Leistungsindikatoren zu sammeln.  In der folgenden Tabelle sind die von der Lösung konfigurierten Objekte und Indikatoren aufgelistet, die alle 60 Sekunden erfasst werden.

### <a name="windows-performance-counters"></a>Windows-Leistungsindikatoren

|Objektname |Name des Leistungsindikators |  
|------------|-------------|  
|Logischer Datenträger |% freier Speicher |  
|Logischer Datenträger |Durchschn. Datenträger s/gelesen |  
|Logischer Datenträger |Durchschn. Datenträger s/übertragen |  
|Logischer Datenträger |Durchschn. Datenträger s/geschrieben |  
|Logischer Datenträger |Datenträger Bytes/s |  
|Logischer Datenträger |Byte gelesen/s  |  
|Logischer Datenträger |Lesevorgänge/s  |  
|Logischer Datenträger |Übertragungen/s |  
|Logischer Datenträger | Byte geschrieben/s |  
|Logischer Datenträger | Schreibvorgänge/s |  
|Logischer Datenträger |Freie Megabytes |  
|Arbeitsspeicher |Verfügbare MB |  
|Netzwerkadapter |Empfangene Byte/Sek. |  
|Netzwerkadapter |Gesendete Byte/Sek. |  
|Prozessor |% Prozessorzeit |  

### <a name="linux-performance-counters"></a>Leistungsindikatoren von Linux

|Objektname |Name des Leistungsindikators |  
|------------|-------------|  
|Logischer Datenträger |% verwendeter Speicher |  
|Logischer Datenträger |Byte gelesen/s  |  
|Logischer Datenträger |Lesevorgänge/s  |  
|Logischer Datenträger |Übertragungen/s |  
|Logischer Datenträger | Byte geschrieben/s |  
|Logischer Datenträger | Schreibvorgänge/s |  
|Logischer Datenträger |Freie Megabytes |  
|Logischer Datenträger |Logischer Datenträger Bytes/s |  
|Arbeitsspeicher |Verfügbarer Speicher in MB |  
|Netzwerk |Summe empfangene Bytes |  
|Netzwerk |Summe übertragene Bytes |  
|Prozessor |% Prozessorzeit |  

## <a name="diagnostic-and-usage-data"></a>Diagnose- und Nutzungsdaten
Wenn Sie den Azure Monitor-Dienst verwenden, sammelt Microsoft automatisch Nutzungs- und Leistungsdaten. Microsoft verwendet diese Daten, um die Qualität, Sicherheit und Integrität des Diensts sicherzustellen und zu verbessern. Zu den Daten vom Zuordnungsfeature gehören Informationen zur Konfiguration Ihrer Software, z.B. Betriebssystem und Betriebssystemversion sowie IP-Adresse, DNS-Name und Name der Arbeitsstation, um exakte und effiziente Funktionen für die Problembehandlung bereitzustellen. Microsoft erfasst weder Namen noch Adressen oder andere Kontaktinformationen.

Weitere Informationen zur Sammlung und Nutzung von Daten finden Sie in den [Datenschutzbestimmungen für Onlinedienste von Microsoft](https://go.microsoft.com/fwlink/?LinkId=512132).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]
## <a name="next-steps"></a>Nächste Schritte

Bei aktivierter Überwachung für Ihre VM stehen diese Informationen für die Analyse mit Azure Monitor for VMs zur Verfügung.  Informationen zur Verwendung des Integritätsfeatures finden Sie unter [View Azure Monitor for VMs Health](vminsights-health.md) (Azure Monitor for VMs – Integrität anzeigen); Informationen zum Anzeigen entdeckter Anwendungsabhängigkeiten finden Sie unter [View Azure Monitor for VMs Map](vminsights-maps.md) (Azure Monitor for VMs – Zuordnung anzeigen).  