---
title: Problembehandlung der Bereitstellung von Kubernetes (K8) in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Probleme bei Ihrer Bereitstellung von Kubernetes (K8) in Azure Stack behandeln können.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2018
ms.author: mabrigg
ms.reviewer: waltero
ms.openlocfilehash: fbb51d8dc3b1ea4c6b34120e8fe35474ae949cf2
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2018
ms.locfileid: "49116911"
---
# <a name="troubleshoot-your-deployment-to-kubernetes-to-azure-stack"></a>Problembehandlung der Bereitstellung von Kubernetes in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

> [!Note]  
> Kubernetes in Azure Stack befindet sich in der Vorschauphase.

Der folgende Artikel beschäftigt sich mit der Problembehandlung Ihres Kubernetes-Clusters. Sie können die Bereitstellungswarnung untersuchen und den Status Ihrer Bereitstellung anhand der für die Bereitstellung erforderlichen Elemente überprüfen. Sie müssen möglicherweise die Bereitstellungsprotokolle von Azure Stack oder den Linux-VMs erfassen, die Kubernetes hosten. Darüber hinaus müssen Sie möglicherweise mit Ihrem Azure Stack-Administrator zusammenarbeiten, um Protokolle von einem Verwaltungsendpunkt abzurufen.

## <a name="overview-of-deployment"></a>Übersicht über die Bereitstellung

Bevor Sie sich mit den Schritten zur Problembehandlung Ihres Clusters befassen, möchten Sie vielleicht den Bereitstellungsprozess des Azure Stack Kubernetes-Clusters überprüfen. Die Bereitstellung verwendet eine Azure Resource Manager-Lösungsvorlage, um die virtuellen Computer zu erstellen, und installiert die ACS-Engine für Ihren Cluster.

### <a name="deployment-workflow"></a>Bereitstellungsworkflow

Die folgende Abbildung zeigt den allgemeinen Ablauf der Bereitstellung des Clusters.

![Vorgang der Bereitstellung von Kubernetes](media/azure-stack-solution-template-kubernetes-trouble/002-Kubernetes-Deploy-Flow.png)

### <a name="deployment-steps"></a>Bereitstellungsschritte

1. Erfassen der Eingabeparameter aus dem Marketplace-Element.

    Geben Sie die Werte ein, die Sie für die Einrichtung des Kubernetes-Clusters benötigen, einschließlich:
    -  **Benutzername**: Hierbei handelt es sich um den Benutzernamen für die virtuellen Linux-Computer, die Teil des Kubernetes-Clusters und der DVM (Deployment Virtual Machine, virtueller Computer für die Bereitstellung) sind.
    -  **Öffentlicher SSH-Schlüssel**: Der Schlüssel, der für die Autorisierung bei allen Linux-Computern verwendet wird, die als Teil des Kubernetes-Clusters und der DVM erstellt werden.
    -  **Dienstprinzipal**: Die ID, die vom Kubernetes Azure-Cloudanbieter verwendet wird. Die als Anwendungs-ID identifizierte Client-ID, wenn Sie Ihren Dienstprinzipal erstellen. 
    -  **Clientgeheimnis**: Der Schlüssel, den Sie beim Erstellen Ihres Dienstprinzipals erstellt haben.

2. Erstellen der Bereitstellungs-VM und der benutzerdefinierten Skripterweiterung.
    -  Erstellen der Linux-Bereitstellungs-VM mit dem Marketplace-Linux-Image **Ubuntu Server 16.04-LTS**.
    -  Herunterladen der benutzerdefinierten Skripterweiterung aus Marketplace und Ausführen der Skripterweiterung. Das Skript ist das **Benutzerdefinierte Skript für Linux 2.0**.
    -  Ausführen des benutzerdefinierten DVM-Skripts. Das Skript:
        1. Ruft den Katalogendpunkt aus dem Azure Resource Manager-Metadatenendpunkt ab.
        2. Ruft die Active Directory-Ressourcen-ID aus dem Azure Resource Manager-Metadatenendpunkt ab.
        3. Lädt das API-Modell für die ACS-Engine.
        4. Stellt die ACS-Engine für den Kubernetes-Cluster bereit und speichert das Azure Stack-Cloudprofil in `/etc/kubernetes/azurestackcloud.json`.
3. Erstellt Master-VMs.

    Lädt die benutzerdefinierte Skripterweiterung herunter und führt sie aus.

4. Führt das Masterskript aus.

    Das Skript:
    - Installiert etcd, Docker und Kubernetes-Ressourcen wie Kubelet. etcd ist ein verteilter Schlüsselwertspeicher, der eine Möglichkeit zum Speichern von Daten in einem Cluster aus Computern bereitstellt. Docker unterstützt reine Virtualisierungen auf Betriebssystemebene, die als Container bezeichnet werden. Kubelet ist der Knoten-Agent, der auf jedem Kubernetes-Knoten ausgeführt wird.
    - Richtet den etcd-Dienst ein.
    - Richtet den Kubelet-Dienst ein.
    - Startet Kubelet. Dieser Vorgang umfasst Folgendes:
        1. Starten des API-Diensts.
        2. Starten des Controllerdiensts.
        3. Starten des Scheduler-Diensts.
5. Erstellen der Agent-VMs.

    Herunterladen und Ausführen der benutzerdefinierten Skripterweiterung.

6. Ausführen des Agent-Skripts. Das benutzerdefinierte Agent-Skript:
    - Installiert etcd.
    - Richtet den Kubelet-Dienst ein.
    - Tritt dem Kubernetes-Cluster bei.

## <a name="steps-for-troubleshooting"></a>Schritte zur Problembehandlung

Sie können Protokolle auf den virtuellen Computern erfassen, die Ihren Kubernetes-Cluster unterstützen. Sie können auch das Bereitstellungsprotokoll überprüfen. Möglicherweise müssen Sie sich auch an Ihren Azure Stack-Administrator wenden, um die von Ihnen verwendete Version von Azure Stack zu überprüfen und Protokolle von Azure Stack im Zusammenhang mit Ihrer Bereitstellung abzurufen.

1. Überprüfen Sie den [Bereitstellungsstatus](#review-deployment-status) und das [Abrufen der Protokolle](#get-logs-from-a-vm) vom Masterknoten in Ihrem Kubernetes-Cluster.
2. Sie müssen die neueste Version von Azure Stack verwenden. Wenn Sie nicht sicher sind, welche Version von Azure Stack verwendet wird, wenden Sie sich an den Azure Stack-Administrator. Für das Marketplace-Element Kubernetes-Cluster 0.3.0 ist Azure Stack Version 1808 oder höher erforderlich.
3.  Überprüfen Sie Ihre VM-Erstellungsdateien. Die folgenden Probleme können aufgetreten sein:  
    - Der öffentliche Schlüssel ist möglicherweise ungültig. Überprüfen Sie den Schlüssel, den Sie erstellt haben.  
    - Die Erstellung des virtuellen Computers kann einen internen Fehler oder einen Erstellungsfehler ausgelöst haben. Fehler können durch eine Reihe von Faktoren (einschließlich Kapazitätseinschränkungen für Ihr Azure Stack-Abonnement) verursacht werden.
    - Beginnt der vollqualifizierte Domänenname (FQDN) für den virtuellen Computer mit einem doppelten Präfix?
4.  Wenn der virtuelle Computer **OK** ist, untersuchen Sie die DVM. Wenn für die DVM eine Fehlermeldung vorliegt, sind die folgenden Ursachen möglich:

    - Der öffentliche Schlüssel ist möglicherweise ungültig. Überprüfen Sie den Schlüssel, den Sie erstellt haben.  
     - Sie müssen sich an Ihren Azure Stack-Administrator wenden, um die Protokolle für Azure Stack mit den privilegierten Endpunkten abzurufen. Weitere Informationen finden Sie unter [Azure Stack-Diagnosetools](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostics).
5. Wenn Sie Fragen zu Ihrer Bereitstellung haben, können Sie Ihre Frage im [Azure Stack-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) stellen bzw. ermitteln, ob jemand die Frage dort bereits beantwortet hat. 

## <a name="review-deployment-status"></a>Überprüfen des Bereitstellungsstatus

Sie können den Bereitstellungsstatus überprüfen, wenn Sie Ihren Kubernetes-Cluster bereitstellen, um Probleme zu überprüfen.

1. Öffnen Sie das [Azure Stack-Portal](https://portal.local.azurestack.external).
2. Wählen Sie **Ressourcengruppen** aus, und wählen Sie dann den Namen der Ressourcengruppe aus, die beim Bereitstellen des Kubernetes-Clusters verwendet wird.
3. Wählen Sie **Bereitstellungen** und dann den **Bereitstellungsnamen** aus.

    ![Problembehandlung](media/azure-stack-solution-template-kubernetes-trouble/azure-stack-kub-trouble-report.png)

4.  Sehen Sie sich das Problembehandlungsfenster an. Jede bereitgestellte Ressource stellt die folgenden Informationen zur Verfügung.
    
    | Eigenschaft | BESCHREIBUNG |
    | ----     | ----        |
    | Ressource | Der Name der Ressource. |
    | Typ | Der Ressourcenanbieter und der Typ der Ressource. |
    | Status | Der Status des Elements. |
    | TimeStamp | Der UTC-Zeitstempel. |
    | Vorgangsdetails | Die Details des Vorgangs (z.B. der Ressourcenanbieter für den Vorgang, der Ressourcenendpunkt und der Name der Ressource). |

    Jedes Element weist ein grünes oder rotes Statussymbol auf.

## <a name="get-logs-from-a-vm"></a>Abrufen von Protokollen von einem virtuellen Computer

Sie müssen eine Verbindung mit der Master-VM für Ihren Cluster herstellen, eine Bash-Eingabeaufforderung öffnen und ein Skript ausführen, um die Protokolle zu generieren. Den Master mit dem Namen `k8s-master-<sequence-of-numbers>` finden Sie in Ihrer Clusterressourcengruppe. 

### <a name="prerequisites"></a>Voraussetzungen

Sie benötigen eine Bash-Eingabeaufforderung auf dem Computer, den Sie zur Verwaltung von Azure Stack verwenden. Verwenden Sie Bash zum Ausführen der Skripts, die auf die Protokolle zugreifen. Auf einem Windows-Computer können Sie die mit Git installierte Bash-Eingabeaufforderung verwenden. Die neueste Version von Git finden Sie unter [Git-Downloads](https://git-scm.com/downloads).

### <a name="get-logs"></a>Abrufen von Protokollen

1. Öffnen Sie eine Bash-Eingabeaufforderung. Wenn Sie Git auf einem Windows-Computer verwenden, können Sie eine Bash-Eingabeaufforderung aus dem folgenden Pfad öffnen: `c:\programfiles\git\bin\bash.exe`.
2. Führen Sie die folgenden Bash-Befehle aus:

    ```Bash  
    mkdir -p $HOME/kuberneteslogs
    cd $HOME/kuberneteslogs
    curl -O https://raw.githubusercontent.com/msazurestackworkloads/azurestack-gallery/master/diagnosis/getkuberneteslogs.sh
    sudo chmod 744 getkuberneteslogs.sh
    ```

    > [!Note]  
    > Unter Windows müssen Sie `sudo` nicht ausführen und können einfach `chmod 744 getkuberneteslogs.sh` verwenden.

3. Führen Sie in der gleichen Sitzung den folgenden Befehl aus, wobei die Parameter entsprechend Ihrer Umgebung angepasst werden müssen.

    ```Bash  
    ./getkuberneteslogs.sh --identity-file id_rsa --user azureuser --vmdhost 192.168.102.37
    ```

    Überprüfen Sie die Parameter, und legen Sie die Werte basierend auf Ihrer Umgebung fest.
    | Parameter           | BESCHREIBUNG                                                                                                      | Beispiel                                                                       |
    |---------------------|------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|
    | -i, --identity-file | Die Datei mit dem privaten RSA-Schlüssel zum Herstellen einer Verbindung mit der Kubernetes-Master-VM. Der Schlüssel muss mit `-----BEGIN RSA PRIVATE KEY-----` beginnen. | C:\data\privatekey.pem                                                        |
    | -h, --host          | Die öffentliche IP-Adresse oder der vollqualifizierte Domänenname (FQDN) der Master-VM des Kubernetes-Clusters. Der Name des virtuellen Computers beginnt mit `k8s-master-`.                       | IP: 192.168.102.37<br><br>FQDN: k8s-12345.local.cloudapp.azurestack.external      |
    | -u, --user          | Der Benutzername der Master-VM des Kubernetes-Clusters. Dieser Name wird festgelegt, wenn Sie das Marketplace-Element konfigurieren.                                                                    | azureuser                                                                     |
    | -d, --vmdhost       | Die öffentliche IP-Adresse oder der FQDN der DVM. Der Name des virtuellen Computers beginnt mit `vmd-`.                                                       | IP: 192.168.102.38<br><br>DNS: vmd-dnsk8-frog.local.cloudapp.azurestack.external |

   Wenn Sie Ihre Parameterwerte hinzufügen, kann dies wie folgt aussehen:

    ```Bash  
    ./getkuberneteslogs.sh --identity-file "C:\secretsecret.pem" --user azureuser --vmdhost 192.168.102.37
     ```

    Bei einer erfolgreiche Ausführung werden die Protokolle erstellt.

    ![Generierte Protokolle](media/azure-stack-solution-template-kubernetes-trouble/azure-stack-generated-logs.png)


4. Rufen Sie die Protokolle in den vom Befehl erstellten Ordnern ab. Der Befehl erstellt einen neuen Ordner und versieht ihn mit einem Zeitstempel.
    - KubernetesLogs*JJJJ-MM-TT-XX-XX-XX-XXX*
        - Dvmlogs
        - Acsengine-kubernetes-dvm.log

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen von Kubernetes in Azure Stack](azure-stack-solution-template-kubernetes-deploy.md)

[Hinzufügen von Kubernetes zum Marketplace (für den Azure Stack-Operator)](..\azure-stack-solution-template-kubernetes-cluster-add.md)

[Kubernetes in Azure](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
