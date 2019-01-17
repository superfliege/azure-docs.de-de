---
title: Bereitstellen von Azure Monitor für VMs (Vorschauversion) | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie das Bereitstellen und die Konfiguration von Azure Monitor für VMs durchführen, damit Sie erfahren können, wie gut die Leistung Ihrer verteilten Anwendung ist und welche Integritätsprobleme erkannt wurden.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/07/2018
ms.author: magoedte
ms.openlocfilehash: cfbe1ce39d7f68dd6ea2510b5c6cbddf4eb71710
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2019
ms.locfileid: "54331995"
---
# <a name="deploy-azure-monitor-for-vms-preview"></a>Bereitstellen von Azure Monitor für VMs (Vorschauversion)
In diesem Artikel wird beschrieben, wie Sie Azure Monitor für VMs einrichten. Der Dienst überwacht die Integrität des Betriebssystems für Ihre virtuellen Azure-Computer (VMs) und VM-Skalierungsgruppen sowie der virtuellen Computer in Ihrer Umgebung. Diese Überwachung umfasst die Erkennung und Zuordnung von Anwendungsabhängigkeiten, die möglicherweise auf ihnen gehostet werden. 

Sie aktivieren Azure Monitor für VMs mithilfe einer der folgenden Methoden:

* Aktivieren Sie einen einzelnen virtuellen Azure-Computer durch direktes Auswählen von **Insights (Vorschau)** auf dem virtuellen Computer.
* Aktivieren Sie mindestens zwei virtuelle Azure-Computer mithilfe von Azure Policy. Durch diese Methode werden die erforderlichen Abhängigkeiten von bestehenden und neuen virtuellen Computern installiert und ordnungsgemäß konfiguriert. Nicht konforme VMs werden gemeldet, sodass Sie entscheiden können, ob Sie sie aktivieren und wie Sie den Zustand korrigieren möchten.
* Aktivieren Sie mindestens zwei virtuelle Azure-Computer oder VM-Skalierungsgruppen über ein bestimmtes Abonnement oder eine bestimmte Ressourcengruppe mithilfe von PowerShell.

Weitere Informationen zu den einzelnen Methoden finden Sie weiter unten im Artikel.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie beginnen, stellen Sie sicher, dass Sie die Informationen in den folgenden Abschnitten verstanden haben.

### <a name="log-analytics"></a>Log Analytics

Ein Log Analytics-Arbeitsbereich wird aktuell in den folgenden Regionen unterstützt:

- USA, Westen-Mitte
- USA (Ost)
- Europa, Westen
- Asien, Südosten<sup>1</sup>

<sup>1</sup> Diese Region unterstützt derzeit das Integritätsfeature von Azure Monitor für VMs nicht.

>[!NOTE]
>Die Bereitstellung von virtuellen Azure-Computern kann aus jeder Region erfolgen, und es besteht keine Einschränkung auf die Regionen, die den Log Analytics-Arbeitsbereich unterstützen.
>

Wenn Sie über keinen Arbeitsbereich verfügen, können Sie diesen mit einer der folgenden Methoden erstellen:
* [Die Azure-CLI](../../azure-monitor/learn/quick-create-workspace-cli.md)
* [PowerShell](../../azure-monitor/learn/quick-create-workspace-posh.md)
* [Azure-Portal](../../azure-monitor/learn/quick-create-workspace.md)
* [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md)

Wenn Sie im Azure-Portal die Überwachung für einen einzelnen virtuellen Azure-Computer aktivieren, können Sie während dieses Vorgangs einen Arbeitsbereich erstellen.

Das Aktivieren der Lösung für das bedarfsorientierte Szenario erfordert zunächst die folgenden Konfigurationen in Ihrem Log Analytics-Arbeitsbereich:

* Installieren Sie die Projektmappen „ServiceMap“ und „InfrastructureInsights“. Sie können diese Installation nur mit einer Azure Resource Manager-Vorlage abschließen, die in diesem Artikel bereitgestellt wird.
* Konfigurieren Sie den Log Analytics-Arbeitsbereich zur Erfassung von Leistungsindikatoren.

Informationen zum Konfigurieren Ihres Arbeitsbereichs für das bedarfsorientierte Szenario finden Sie unter [Einrichten des Log Analytics-Arbeitsbereichs für die bedarfsorientierte Bereitstellung](#setup-log-analytics-workspace).

### <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

Die folgende Tabelle enthält die Windows- und Linux-Betriebssysteme, die für Azure Monitor for VMs unterstützt werden. Eine vollständige Liste mit ausführlicheren Informationen zu den größeren und kleineren Release- und unterstützen Kernelversionen des Linux-Betriebssystems ist weiter unten in diesem Abschnitt angegeben.

|Betriebssystemversion |Leistung |Karten |Health |
|-----------|------------|-----|-------|
|Windows Server 2016 1803 | X | X | X |
|Windows Server 2016 | X | X | X |
|Windows Server 2012 R2 | X | X | |
|Windows Server 2012 | X | X | |
|Windows Server 2008 R2 | X | X| |
|Red Hat Enterprise Linux (RHEL) 7, 6| X | X| X |
|Ubuntu 18.04, 16.04, 14.04 | X | X | X |
|CentOS Linux 7, 6 | X | X | X |
|SUSE Linux Enterprise Server (SLES) 12 | X | X | X |
|Oracle Linux 7 | X<sup>1</sup> | | X |
|Oracle Linux 6 | X | X | X |
|Debian 9.4, 8 | X<sup>1</sup> | | X |

<sup>1</sup> Das Leistungsfeature von Azure Monitor für VMs ist nur über Azure Monitor verfügbar. Es ist nicht verfügbar, wenn Sie direkt aus dem linken Bereich des virtuellen Azure-Computers darauf zugreifen.

>[!NOTE]
>Die folgenden Informationen gelten für die Unterstützung des Linux-Betriebssystems:
> - Es werden nur die Standardversion und SMP-Version des Linux-Kernels unterstützt.
> - Nicht-Standardversionen des Kernels, z. B. PAE (Physical Address Extension) und Xen, werden für keine Linux-Distribution unterstützt. Beispielsweise wird ein System mit der Versionszeichenfolge *2.6.16.21-0.8-xen* nicht unterstützt.
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

### <a name="the-microsoft-dependency-agent"></a>Microsoft Dependency-Agent
Das Zuordnungsfeature in Azure Monitor für VMs erhält seine Daten vom Microsoft Dependency-Agent. Der Dependency-Agent baut auf dem Log Analytics-Agent auf und ist auf dessen Verbindung mit Log Analytics angewiesen. Daher muss für Ihr System der Log Analytics-Agent installiert und mit dem Dependency-Agent konfiguriert sein.

Wenn Sie Azure Monitor für VMs für eine einzelne Azure-VM aktivieren oder die Methoden für die bedarfsorientierte Bereitstellung verwenden, wird die Azure-VM-Erweiterung für den Depency-Agent verwendet, um den Agent im Rahmen der Erfahrung zu installieren.

In einer Hybridumgebung gibt es zwei Möglichkeiten, um den Dependency-Agent herunterzuladen und zu installieren: Manuell oder durch die Verwendung einer automatisierten Bereitstellungsmethode für virtuelle Computer, die außerhalb von Azure gehostet werden.

In der folgenden Tabelle sind die verbundenen Quellen beschrieben, die vom Zuordnungsfeature in einer Hybridumgebung unterstützt werden.

| Verbundene Quelle | Unterstützt | BESCHREIBUNG |
|:--|:--|:--|
| Windows-Agents | JA | Zusätzlich zum [Log Analytics-Agent für Windows](../../azure-monitor/platform/log-analytics-agent.md) erfordern Windows-Agents den Microsoft Dependency-Agent. Eine vollständige Liste der Betriebssystemversionen finden Sie unter [Unterstützte Betriebssysteme](#supported-operating-systems). |
| Linux-Agents | JA | Zusätzlich zum [Log Analytics-Agent für Linux](../../azure-monitor/platform/log-analytics-agent.md) erfordern Linux-Agents den Microsoft Dependency-Agent. Eine vollständige Liste der Betriebssystemversionen finden Sie unter [Unterstützte Betriebssysteme](#supported-operating-systems). |
| System Center Operations Manager-Verwaltungsgruppe | Nein  | |

Der Dependency-Agent kann von den folgenden Speicherorten heruntergeladen werden:

| Datei | Betriebssystem | Version | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.7.4 | A111B92AB6CF28EB68B696C60FE51F980BFDFF78C36A900575E17083972989E0 |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.7.4 | AB58F3DB8B1C3DEE7512690E5A65F1DFC41B43831543B5C040FCCE8390F2282C |

## <a name="role-based-access-control"></a>Rollenbasierte Zugriffssteuerung
Um die Features in Azure Monitor für VMs zu aktivieren und darauf zuzugreifen, müssen Ihnen die folgenden Zugriffsrollen zugewiesen werden:

- Sie müssen über die Rolle *Log Analytics-Mitwirkender* verfügen, damit Sie die Lösung aktivieren können.

- Sie müssen über die Rolle *Überwachungsleser* für den virtuellen Azure-Computer verfügen, um Leistungs-, Integritäts- und Zuordnungsdaten anzeigen zu können. Der Log Analytics-Arbeitsbereich muss für Azure Monitor für VMs konfiguriert sein.

Weitere Informationen zur Zugriffssteuerung auf einen Log Analytics-Arbeitsbereich finden Sie unter [Verwalten von Arbeitsbereichen](../../azure-monitor/platform/manage-access.md).

## <a name="enable-monitoring-in-the-azure-portal"></a>Aktivieren der Überwachung im Azure-Portal
Gehen Sie wie folgt vor, um die Überwachung Ihrer Azure-VM im Azure-Portal zu aktivieren:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie **Virtuelle Computer**.

1. Wählen Sie einen virtuellen Computer in der Liste aus.

1. Wählen Sie auf der Seite der VM im Abschnitt **Überwachung** den Eintrag **Insights (Vorschau)** aus.

1. Wählen Sie auf der Seite **Insights (Vorschau)** **Jetzt testen** aus.

    ![Aktivieren von Azure Monitor for VMs für eine VM](./media/vminsights-onboard/enable-vminsights-vm-portal-01.png)
1. Wenn Sie im selben Abonnement einen Log Analytics-Arbeitsbereich haben, wählen Sie ihn auf der Seite **Azure Monitor Insights Onboarding** (Onboarding von Azure Monitor Insights) in der Dropdownliste aus.  
    Mit der Liste werden der Standardarbeitsbereich und der Speicherort, an dem die VM im Abonnement bereitgestellt wird, vorausgewählt. 

    >[!NOTE]
    >Befolgen Sie die Anweisungen unter [Erstellen eines Log Analytics-Arbeitsbereichs](../../azure-monitor/learn/quick-create-workspace.md) für eine der oben aufgeführten unterstützten Regionen, wenn Sie einen neuen Log Analytics-Arbeitsbereich zum Speichern der Überwachungsdaten aus der VM erstellen möchten.

Nach dem Aktivieren der Überwachung kann es ca. 10 Minuten dauern, bis die Integritätsmetriken für den virtuellen Computer angezeigt werden.

![Aktivieren von Azure Monitor for VMs – Verarbeiten der Überwachungsbereitstellung](./media/vminsights-onboard/onboard-vminsights-vm-portal-status.png)


## <a name="deploy-at-scale"></a>Bedarfsorientiertes Bereitstellen
In diesem Abschnitt stellen Sie Azure Monitor für VMs bedarfsorientiert mit Azure Policy oder Azure PowerShell bereit.

Bevor Sie Ihre virtuellen Computer bereitstellen, konfigurieren Sie Ihren Log Analytics-Arbeitsbereich vorab, indem Sie wie folgt vorgehen:

1. Wenn Sie nicht bereits über einen Arbeitsbereich verfügen, erstellen Sie einen, der Azure Monitor für VMs unterstützen kann.  
    Lesen Sie den Artikel [Verwalten von Arbeitsbereichen](../../log-analytics/log-analytics-manage-access.md?toc=/azure/azure-monitor/toc.json), um sich mit den Kosten, der Verwaltung und den Konformitätsanforderungen vertraut zu machen, bevor Sie den Vorgang fortsetzen.

1. Erstellen Sie einen neuen Arbeitsbereich, sofern noch kein Arbeitsbereich vorhanden ist, der zur Unterstützung von Azure Monitor für VMs verwendet werden kann. Lesen Sie vor dem Erstellen eines neuen Arbeitsbereichs den Artikel [Verwalten von Arbeitsbereichen](../../azure-monitor/platform/manage-access.md?toc=/azure/azure-monitor/toc.json), um sich mit den Kosten, der Verwaltung und den Konformitätsanforderungen vertraut zu machen.

1. Aktivieren Sie im Arbeitsbereich Leistungsindikatoren für die Erfassung auf Linux- und Windows-VMs.

1. Installieren und aktivieren Sie die Projektmappen „ServiceMap“ und „InfrastructureInsights“ in Ihrem Arbeitsbereich.

### <a name="set-up-a-log-analytics-workspace"></a>Einrichten eines Log Analytics-Arbeitsbereichs
Wenn Sie keinen Log Analytics-Arbeitsbereich besitzen, erstellen Sie einen, indem Sie die Methoden überprüfen, die im Abschnitt [Voraussetzungen](#log-analytics) empfohlen werden.

#### <a name="enable-performance-counters"></a>Aktivieren von Leistungsindikatoren
Wenn der Log Analytics-Arbeitsbereich, auf den die Lösung verweist, noch nicht für die Erfassung der von der Lösung benötigten Leistungsindikatoren konfiguriert ist, müssen Sie diese aktivieren. Dies kann auf zwei Arten erfolgen:
* Manuell, wie in [Windows- und Linux-Leistungsindikatoren in Log Analytics](../../azure-monitor/platform/data-sources-performance-counters.md) beschrieben.
* Durch Herunterladen und Ausführen eines PowerShell-Skripts, das über den [Azure PowerShell-Katalog](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1) verfügbar ist.

#### <a name="install-the-servicemap-and-infrastructureinsights-solutions"></a>Installieren der Projektmappen „ServiceMap“ und „InfrastructureInsights“
Diese Methode umfasst eine JSON-Vorlage, die die Konfiguration zum Aktivieren der Lösungskomponenten für Ihren Log Analytics-Arbeitsbereich angibt.

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
    }
    ```

1. Speichern Sie diese Datei als *installsolutionsforvminsights.json* in einem lokalen Ordner.

1. Bearbeiten Sie die Werte für *WorkspaceName*, *ResourceGroupName* und *WorkspaceLocation*. Der Wert für *WorkspaceName* ist die vollständige Ressourcen-ID Ihres Log Analytics-Arbeitsbereichs, die den Namen des Arbeitsbereichs umfasst. Der Wert für *WorkspaceLocation* ist die Region, in der der Arbeitsbereich definiert ist.

1. Sie können diese Vorlage mithilfe des folgenden PowerShell-Befehls bereitstellen:

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName <ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
    ```

    Die Änderung der Konfiguration kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Meldung angezeigt, die der folgenden ähnelt und das Ergebnis anzeigt:

    ```powershell
    provisioningState       : Succeeded
    ```

### <a name="enable-by-using-azure-policy"></a>Aktivieren mithilfe von Azure Policy
Um Azure Monitor für VMs bedarfsorientiert zu aktivieren, damit eine konsistente Konformität und die automatische Aktivierung der neu bereitgestellten VMs sichergestellt ist, wird [Azure Policy](../../azure-policy/azure-policy-introduction.md) empfohlen. Diese Richtlinien:

* Stellen den Log Analytics-Agent und Dependency-Agent bereit
* Melden Konformitätsergebnisse
* Beheben nicht konforme VMs

So aktivieren Sie Azure Monitor für VMs mithilfe von Azure Policy in Ihrem Mandanten

- Zuweisen der Initiative zu einem Bereich: Verwaltungsgruppe, Abonnement oder Ressourcengruppe
- Überprüfen und Beheben der Konformitätsergebnisse

Weitere Informationen zur Zuweisung von Azure Policy finden Sie unter [Azure Policy – Übersicht](../../governance/policy/overview.md#policy-assignment), und arbeiten Sie die [Übersicht zu Verwaltungsgruppen](../../governance/management-groups/index.md) durch, bevor Sie fortfahren.

Die Richtliniendefinitionen sind in der folgenden Tabelle aufgeführt:

|NAME |BESCHREIBUNG |Typ |
|-----|------------|-----|
|[Vorschau]: Aktivieren von Azure Monitor für VMs |Hiermit aktivieren Sie Azure Monitor für die virtuellen Computer (VMs) in dem angegebenen Bereich (Verwaltungsgruppe, Abonnement oder Ressourcengruppe). Akzeptiert den Log Analytics-Arbeitsbereich als Parameter. |Initiative |
|[Vorschau]: Überwachen der Bereitstellung des Dependency-Agent – VM-Image (Betriebssystem) nicht aufgelistet |Meldet VMs als nicht konform, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |Richtlinie |
|[Vorschau]: Überwachen der Bereitstellung des Log Analytics-Agents – VM-Image (Betriebssystem) nicht aufgelistet |Meldet VMs als nicht konform, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |Richtlinie |
|[Vorschau]: Bereitstellen des Dependency-Agent für Linux-VMs |Stellen Sie den Dependency-Agent für Linux-VMs bereit, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |Richtlinie |
|[Vorschau]: Bereitstellen des Dependency-Agent für Windows-VMs |Stellen Sie den Dependency-Agent für Windows-VMs bereit, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |Richtlinie |
|[Vorschau]: Bereitstellen des Log Analytics-Agents für Linux-VMs |Stellen Sie den Log Analytics-Agent für Linux-VMs bereit, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |Richtlinie |
|[Vorschau]: Bereitstellen des Log Analytics-Agents für Windows-VMs |Stellen Sie den Log Analytics-Agent für Windows-VMs bereit, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |Richtlinie |

Die eigenständige Richtlinie (nicht in der Initiative enthalten) wird hier beschrieben:

|NAME |BESCHREIBUNG |Typ |
|-----|------------|-----|
|[Vorschau]: Überwachen des Log Analytics-Arbeitsbereichs für VM – Berichtskonflikt |Meldet VMs als nicht konform, wenn sie keine Protokolle an den in der Richtlinien-/Initiativenzuweisung angegebenen Log Analytics-Arbeitsbereich senden. |Richtlinie |

#### <a name="assign-the-azure-monitor-initiative"></a>Zuweisen der Azure Monitor-Initiative
In dieser ursprünglichen Version können Sie die Richtlinienzuweisung nur im Azure-Portal erstellen. Informationen zum Ausführen dieser Schritte finden Sie unter  [Erstellen einer Richtlinienzuweisung im Azure-Portal](../../governance/policy/assign-policy-portal.md).

1. Wählen Sie zum Starten des Azure Policy-Diensts im Azure-Portal die Option **Alle Dienste** aus, und suchen Sie dann **Richtlinie**, um diese Option auszuwählen.

1. Wählen Sie im linken Bereich der Seite „Azure Policy“ die Option **Zuweisungen** aus.  
    Eine Zuweisung ist eine zugewiesene Richtlinie, die innerhalb eines bestimmten Bereichs angewendet werden soll.
    
1. Wählen Sie im oberen Bereich der Seite **Richtlinien – Zuweisungen** die Option **Initiative zuweisen** aus.

1. Wählen Sie auf der Seite **Initiative zuweisen** den **Bereich** aus, indem Sie auf die Auslassungspunkte (...) klicken und eine Verwaltungsgruppe oder ein Abonnement auswählen.  
    In unserem Beispiel schränkt ein Bereich die Richtlinienzuweisung zur Durchsetzung auf eine Gruppierung virtueller Computer ein.
    
1. Speichern Sie unten auf der Seite **Bereich** Ihre Änderungen, indem Sie **Auswählen** auswählen.

1. (Optional) Um eine oder mehrere Ressourcen aus dem Bereich zu entfernen, wählen Sie **Ausschlüsse** aus.

1. Wählen Sie die Auslassungspunkte (...) **Initiativendefinition** aus, um die Liste der verfügbaren Definitionen anzuzeigen, und wählen Sie dann **[Vorschau] Azure Monitor für VMs aktivieren** und anschließend **Auswählen** aus.  
    Das Feld **Zuweisungsname** wird automatisch mit dem Namen der von Ihnen ausgewählten Initiative aufgefüllt, Sie können ihn aber ändern. Geben Sie ggf. auch eine Beschreibung ein. Das Feld **Zugewiesen von** wird automatisch mit dem angemeldeten Benutzer aufgefüllt und dieser Wert ist optional.
    
1. Wählen Sie in der Dropdownliste **Log Analytics-Arbeitsbereich** für die unterstützte Region einen Arbeitsbereich aus.

    >[!NOTE]
    >Wenn der Arbeitsbereich außerhalb des Bereichs der Zuweisung liegt, erteilen Sie der Prinzipal-ID der Richtlinienzuweisung *Log Analytics-Mitwirkender*-Berechtigungen. Andernfalls wird möglicherweise ein Bereitstellungsfehler wie der Folgende angezeigt: `The client '343de0fe-e724-46b8-b1fb-97090f7054ed' with object id '343de0fe-e724-46b8-b1fb-97090f7054ed' does not have authorization to perform action 'microsoft.operationalinsights/workspaces/read' over scope ... ` Lesen Sie zum Erteilen des Zugriffs, [wie Sie die verwaltete Identität manuell konfigurieren](../../governance/policy/how-to/remediate-resources.md#manually-configure-the-managed-identity).
    >  
    Das Kontrollkästchen **Verwaltete Identität** ist aktiviert, da die zugewiesene Initiative eine Richtlinie mit dem Effekt *deployIfNotExists* umfasst.
    
1. Wählen Sie in der Dropdownliste **Manage Identity location** (Speicherort der Identität verwalten) die passende Region aus.

1. Wählen Sie **Zuweisen** aus.

#### <a name="review-and-remediate-the-compliance-results"></a>Überprüfen und Warten der Compliance-Ergebnisse

Informationen zum Überprüfen von Compliance-Ergebnissen finden Sie unter [identify non-compliance results](../../governance/policy/assign-policy-portal.md#identify-non-compliant-resources) (Erkennen von Nichtkompatibilitätsergebnissen). Wählen Sie im linken Bereich **Konformität** aus, und suchen Sie dann nach der Initiative **[Vorschau] Azure Monitor für VMs aktivieren** für VMs, die gemäß der von Ihnen erstellten Zuweisung nicht konform sind.

![Richtlinienkonformität für Azure-VMs](./media/vminsights-onboard/policy-view-compliance-01.png)

Auf der Grundlage der in der Initiative enthaltenen Richtlinien werden VMs in den folgenden Szenarien als nicht konform gemeldet:

* Log Analytics oder der Dependency-Agent wurde nicht bereitgestellt.  
    Dieses Szenario ist typisch für einen Bereich mit vorhandenen virtuellen Computern. Um das Problem abzumildern, [erstellen Sie Wartungstasks](../../governance/policy/how-to/remediate-resources.md) für eine nicht konforme Richtlinie, um die erforderlichen Agents bereitzustellen.  
    - [Vorschau]: Deploy Dependency Agent for Linux VMs
    - [Vorschau]: Deploy Dependency Agent for Windows VMs
    - [Vorschau]: Deploy Log Analytics Agent for Linux VMs
    - [Vorschau]: Deploy Log Analytics Agent for Windows VMs

* Das VM-Image (Betriebssystem) wird in der Richtliniendefinition nicht identifiziert.  
    Die Kriterien der Bereitstellungsrichtlinie schließen nur VMs ein, die aus bekannten Azure VM-Images bereitgestellt werden. Überprüfen Sie anhand der Dokumentation, ob das Betriebssystem der VM unterstützt wird. Ist das nicht der Fall, müssen Sie die Bereitstellungsrichtlinie duplizieren und sie aktualisieren oder ändern, damit das Image konform wird.  
    - [Vorschau]: Überwachen der Bereitstellung des Dependency-Agent – VM-Image (Betriebssystem) nicht aufgelistet
    - [Vorschau]: Überwachen der Bereitstellung des Log Analytics-Agents – VM-Image (Betriebssystem) nicht aufgelistet

* VMs melden sich nicht am angegebenen Log Analytics-Arbeitsbereich an.  
    Es ist möglich, dass sich einige VMs im Bereich der Initiative bei einem anderen Log Analytics-Arbeitsbereich anmelden, als dem, der in der Richtlinienzuweisung angegeben ist. Diese Richtlinie ist ein Tool, um zu bestimmen, welche VMs an einen nicht kompatiblen Arbeitsbereich berichten.  
    - [Vorschau]: Audit Log Analytics Workspace for VM - Report Mismatch

### <a name="enable-with-powershell"></a>Aktivieren mithilfe von PowerShell
Um Azure Monitor für VMs für mehrere VMs oder VM-Skalierungsgruppen zu aktivieren, können Sie das PowerShell-Skript [Install-VMInsights.ps1](https://www.powershellgallery.com/packages/Install-VMInsights/1.0) verwenden, das im Azure PowerShell-Katalog verfügbar ist. Dieses Skript durchläuft alle virtuellen Computer und VM-Skalierungsgruppen in Ihrem Abonnement in der bereichsbezogenen Ressourcengruppe gemäß der Angabe in *ResourceGroup* oder eine einzelne VM oder VM-Skalierungsgruppe, die in *Name* angegeben ist. Für jede VM oder VM-Skalierungsgruppe überprüft das Skript, ob die VM-Erweiterung bereits installiert ist. Wenn die VM-Erweiterung nicht installiert ist, versucht das Skript, sie neu zu installieren. Wenn die VM-Erweiterung installiert ist, installiert das Skript die Log Analytics- und Dependency-Agent-VM-Erweiterungen.

Das Skript benötigt mindestens die Azure PowerShell-Modulversion 5.7.0. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.

Führen Sie `Get-Help` aus, um eine Liste der Argumentendetails des Skripts und der Beispielverwendung zu erhalten.

```powershell
Get-Help .\Install-VMInsights.ps1 -Detailed

SYNOPSIS
    This script installs VM extensions for Log Analytics and the Dependency agent as needed for VM Insights.


SYNTAX
    .\Install-VMInsights.ps1 [-WorkspaceId] <String> [-WorkspaceKey] <String> [-SubscriptionId] <String> [[-ResourceGroup]
    <String>] [[-Name] <String>] [[-PolicyAssignmentName] <String>] [-ReInstall] [-TriggerVmssManualVMUpdate] [-Approve] [-WorkspaceRegion] <String>
    [-WhatIf] [-Confirm] [<CommonParameters>]


DESCRIPTION
    This script installs or re-configures following on VMs and VM Scale Sets:
    - Log Analytics VM Extension configured to supplied Log Analytics Workspace
    - Dependency Agent VM Extension

    Can be applied to:
    - Subscription
    - Resource Group in a Subscription
    - Specific VM/VM Scale Set
    - Compliance results of a policy for a VM or VM Extension

    Script will show you list of VMs/VM Scale Sets that will apply to and let you confirm to continue.
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
        If using PolicyAssignmentName parameter, subscription that VMs are in

    -ResourceGroup <String>
        <Optional> Resource Group to which the VMs or VM Scale Sets belong

    -Name <String>
        <Optional> To install to a single VM/VM Scale Set

    -PolicyAssignmentName <String>
        <Optional> Take the input VMs to operate on as the Compliance results from this Assignment
        If specified will only take from this source.

    -ReInstall [<SwitchParameter>]
        <Optional> If VM/VM Scale Set is already configured for a different workspace, set this to change to the new workspace

    -TriggerVmssManualVMUpdate [<SwitchParameter>]
        <Optional> Set this flag to trigger update of VM instances in a scale set whose upgrade policy is set to Manual

    -Approve [<SwitchParameter>]
        <Optional> Gives the approval for the installation to start with no confirmation prompt for the listed VMs/VM Scale Sets

    -WorkspaceRegion <String>
        Region the Log Analytics Workspace is in
        Supported values: "East US","eastus","Southeast Asia","southeastasia","West Central US","westcentralus","West Europe","westeurope"
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

    Install for all VMs in a Resource Group in a subscription

    -------------------------- EXAMPLE 2 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -ResourceGroup <ResourceGroup> -ReInstall

    Specify to reinstall extensions even if already installed, for example to update to a different workspace

    -------------------------- EXAMPLE 3 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -PolicyAssignmentName a4f79f8ce891455198c08736 -ReInstall

    Specify to use a PolicyAssignmentName for source, and to reinstall (move to a new workspace)
```

Das folgende Beispiel veranschaulicht die Verwendung der PowerShell-Befehle im Ordner, um Azure Monitor for VMs zu aktivieren und die erwartete Ausgabe zu verstehen:

```powershell
$WorkspaceId = "<GUID>"
$WorkspaceKey = "<Key>"
$SubscriptionId = "<GUID>"
.\Install-VMInsights.ps1 -WorkspaceId $WorkspaceId -WorkspaceKey $WorkspaceKey -SubscriptionId $SubscriptionId -WorkspaceRegion eastus

Getting list of VMs or VM ScaleSets matching criteria specified

VMs or VM ScaleSets matching criteria:

db-ws-1 VM running
db-ws2012 VM running

This operation will install the Log Analytics and Dependency agent extensions on above 2 VMs or VM Scale Sets.
VMs in a non-running state will be skipped.
Extension will not be reinstalled if already installed. Use -ReInstall if desired, for example to update workspace

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

## <a name="enable-for-a-hybrid-environment"></a>Aktivieren für eine Hybridumgebung
In diesem Abschnitt wird erläutert, wie das Bereitstellen von VMs oder physischen Computern, die in Ihrem Rechenzentrum oder anderen Cloudumgebungen gehostet sind, für die Überwachung mithilfe von Azure Monitor für VMs erfolgt.

Der Dependency-Agent für das Zuordnungsfeature von Azure Monitor für VMs überträgt selbst keine Daten und erfordert keine Änderungen an Firewalls oder Ports. Die Zuordnungsdaten werden immer vom Log Analytics-Agent an den Azure Monitor-Dienst übertragen, entweder direkt oder über das [OMS-Gateway](../../azure-monitor/platform/gateway.md), falls Ihre IT-Sicherheitsrichtlinien es nicht zulassen, dass Computer im Netzwerk eine Verbindung mit dem Internet herstellen.

Überprüfen Sie die Anforderungen und Bereitstellungsmethoden für den [Log Analytics-Linux- und -Windows-Agent](../../log-analytics/log-analytics-agent-overview.md).

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

Die erforderlichen Schritte sind wie folgt zusammengefasst:

1. Installieren Sie den Log Analytics-Agent für Windows oder Linux.

1. Laden Sie den Dependency-Agent für das Zuordnungsfeature von Azure Monitor für VMs für [Windows](https://aka.ms/dependencyagentwindows) oder [Linux](https://aka.ms/dependencyagentlinux) herunter, und installieren Sie ihn.

1. Aktivieren Sie das Sammeln von Leistungsindikatoren.

1. Stellen Sie Azure Monitor für VMs bereit.

### <a name="install-the-dependency-agent-on-windows"></a>Installieren des Dependency-Agents unter Windows
Sie können den Dependency-Agent durch Ausführen von `InstallDependencyAgent-Windows.exe` manuell auf Windows-Computern installieren. Wenn Sie diese ausführbare Datei ohne Optionen ausführen, wird ein Setup-Assistent gestartet, mit dem Sie die Installation des Agent interaktiv durchführen können.

>[!NOTE]
>Zum Installieren oder Deinstallieren des Agent sind *Administratorrechte* erforderlich.

In der folgenden Tabelle sind die Parameter hervorgehoben, die von Setup für den Agent auf der Befehlszeile unterstützt werden.

| Parameter | BESCHREIBUNG |
|:--|:--|
| /? | Gibt eine Liste der Befehlszeilenoptionen zurück. |
| /S | Führt eine automatische Installation ohne Benutzereingriff aus. |

Um das Installationsprogramm beispielsweise mit dem Parameter `/?` auszuführen, geben Sie **InstallDependencyAgent-Windows.exe /?** ein.

Dateien für den Dependency-Agent für Windows werden standardmäßig in *C:\Programme\Microsoft Dependency Agent* installiert. Falls der Dependency-Agent nach dem Abschluss von Setup nicht gestartet wird, suchen Sie in den Protokollen nach ausführlichen Fehlerinformationen. Das Protokollverzeichnis ist *%Programfiles%\Microsoft Dependency Agent\logs*.

### <a name="install-the-dependency-agent-on-linux"></a>Installieren des Dependency-Agents unter Linux
Der Dependency-Agent wird auf Linux-Servern mit *InstallDependencyAgent-Linux64.bin* installiert, einem Shellskript mit einer selbstextrahierenden Binärdatei. Sie können die Datei mit `sh` ausführen oder der Datei selbst Ausführungsberechtigungen hinzufügen.

>[!NOTE]
> Zum Installieren oder Konfigurieren des Agent ist Root-Zugriff erforderlich.
>

| Parameter | BESCHREIBUNG |
|:--|:--|
| -help | Ruft eine Liste der Befehlszeilenoptionen ab. |
| -s | Führt eine automatische Installation ohne Benutzereingaben aus. |
| --check | Überprüft Berechtigungen und das Betriebssystem, ohne den Agent zu installieren. |

Um das Installationsprogramm beispielsweise mit dem Parameter `-help` auszuführen, geben Sie **InstallDependencyAgent-Linux64.bin -help** ein.

Installieren Sie den Dependency-Agent für Linux mit Root-Berechtigungen, indem Sie den folgenden Befehl ausführen: `sh InstallDependencyAgent-Linux64.bin`.

Falls der Dependency-Agent nicht gestartet wird, suchen Sie in den Protokollen nach ausführlichen Fehlerinformationen. Für Linux-Agents lautet das Protokollverzeichnis */var/opt/microsoft/dependency-agent/log*.

Dateien für den Dependency-Agent werden in den folgenden Verzeichnissen gespeichert:

| Dateien | Standort |
|:--|:--|
| Hauptdateien | /opt/microsoft/dependency-agent |
| Protokolldateien | /var/opt/microsoft/dependency-agent/log |
| Konfigurationsdateien | /etc/opt/microsoft/dependency-agent/config |
| Ausführbare Dienstdateien | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| Binäre Speicherdateien | /var/opt/microsoft/dependency-agent/storage |

### <a name="enable-performance-counters"></a>Aktivieren von Leistungsindikatoren
Wenn der Log Analytics-Arbeitsbereich, auf den die Lösung verweist, noch nicht für die Erfassung der von der Lösung benötigten Leistungsindikatoren konfiguriert ist, müssen Sie diese aktivieren. Dies kann auf zwei Arten erfolgen:
* Manuell, wie in [Windows- und Linux-Leistungsindikatoren in Log Analytics](../../azure-monitor/platform/data-sources-performance-counters.md) beschrieben.
* Durch Herunterladen und Ausführen eines PowerShell-Skripts, das über den [Azure PowerShell-Katalog](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1) verfügbar ist.

### <a name="deploy-azure-monitor-for-vms"></a>Bereitstellen von Azure Monitor für VMs
Diese Methode umfasst eine JSON-Vorlage, die die Konfiguration zum Aktivieren der Lösungskomponenten für Ihren Log Analytics-Arbeitsbereich angibt.

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
    }
    ```

1. Speichern Sie diese Datei als *installsolutionsforvminsights.json* in einem lokalen Ordner.

1. Bearbeiten Sie die Werte für *WorkspaceName*, *ResourceGroupName* und *WorkspaceLocation*. Der Wert für *WorkspaceName* ist die vollständige Ressourcen-ID Ihres Log Analytics-Arbeitsbereichs, die den Namen des Arbeitsbereichs umfasst. Der Wert für *WorkspaceLocation* ist die Region, in der der Arbeitsbereich definiert ist.

1. Sie können diese Vorlage mithilfe des folgenden PowerShell-Befehls bereitstellen:

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
    ```

    Die Änderung der Konfiguration kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Meldung angezeigt, die der folgenden ähnelt und das Ergebnis anzeigt:

    ```powershell
    provisioningState       : Succeeded
    ```
Nach dem Aktivieren der Überwachung kann es ca. 10 Minuten dauern, bis Integritätszustand und Metriken für den Hybridcomputer angezeigt werden.

## <a name="performance-counters-enabled"></a>Aktivierte Leistungsindikatoren
Azure Monitor für VMs konfiguriert einen Log Analytics-Arbeitsbereich, um die von der Lösung verwendeten Leistungsindikatoren zu sammeln. In der folgenden Tabelle sind die von der Lösung konfigurierten Objekte und Indikatoren aufgelistet, die alle 60 Sekunden erfasst werden.

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
Wenn Sie den Azure Monitor-Dienst verwenden, sammelt Microsoft automatisch Nutzungs- und Leistungsdaten. Microsoft verwendet diese Daten, um die Qualität, Sicherheit und Integrität des Diensts sicherzustellen und zu verbessern. Zu den Daten vom Zuordnungsfeature gehören Informationen zur Konfiguration Ihrer Software, z. B. Betriebssystem und Betriebssystemversion sowie IP-Adresse, DNS-Name und Name der Arbeitsstation, um exakte und effiziente Funktionen für die Problembehandlung bereitzustellen. Microsoft erfasst weder Namen noch Adressen oder andere Kontaktinformationen.

Weitere Informationen zur Sammlung und Nutzung von Daten finden Sie in den [Datenschutzbestimmungen für Onlinedienste von Microsoft](https://go.microsoft.com/fwlink/?LinkId=512132).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]
## <a name="next-steps"></a>Nächste Schritte

Nachdem die Überwachung für Ihren virtuellen Computer aktiviert ist, stehen diese Informationen für die Analyse mit Azure Monitor für VMs zur Verfügung. Informationen zum Verwenden des Integritätsfeatures finden Sie unter [Azure Monitor für VMs – Integrität anzeigen](vminsights-health.md). Informationen zu ermittelten Anwendungsabhängigkeiten finden Sie unter [Azure Monitor für VMs – Zuordnung anzeigen](vminsights-maps.md).
