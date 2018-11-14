---
title: Problembehandlung der Bereitstellung von Kubernetes in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Probleme bei Ihrer Bereitstellung von Kubernetes in Azure Stack behandeln können.
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
ms.date: 10/29/2018
ms.author: mabrigg
ms.reviewer: waltero
ms.openlocfilehash: f7f23a6d645a1d8e16e42e751050d8d91b49e2b3
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51007824"
---
# <a name="troubleshoot-your-deployment-to-kubernetes-to-azure-stack"></a>Problembehandlung der Bereitstellung von Kubernetes in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

> [!Note]  
> Kubernetes in Azure Stack befindet sich in der Vorschauphase.

Der folgende Artikel beschäftigt sich mit der Problembehandlung Ihres Kubernetes-Clusters. Sie können die Bereitstellungswarnung untersuchen und den Status Ihrer Bereitstellung anhand der für die Bereitstellung erforderlichen Elemente überprüfen. Sie müssen möglicherweise die Bereitstellungsprotokolle von Azure Stack oder den Linux-VMs erfassen, die Kubernetes hosten. Darüber hinaus müssen Sie möglicherweise mit Ihrem Azure Stack-Administrator zusammenarbeiten, um Protokolle von einem Verwaltungsendpunkt abzurufen.

## <a name="overview-of-deployment"></a>Übersicht über die Bereitstellung

Bevor Sie sich mit der Problembehandlung Ihres Clusters beginnen, möchten Sie vielleicht den Bereitstellungsprozess des Azure Stack Kubernetes-Clusters überprüfen. Die Bereitstellung verwendet eine Azure Resource Manager-Lösungsvorlage, um die virtuellen Computer zu erstellen, und installiert die ACS-Engine für Ihren Cluster.

### <a name="deployment-workflow"></a>Bereitstellungsworkflow

Die folgende Abbildung zeigt den allgemeinen Ablauf der Bereitstellung des Clusters.

![Vorgang der Bereitstellung von Kubernetes](media/azure-stack-solution-template-kubernetes-trouble/002-Kubernetes-Deploy-Flow.png)

### <a name="deployment-steps"></a>Bereitstellungsschritte

1. Erfassen Sie die Eingabeparameter aus dem Marketplace-Element.

    Geben Sie die Werte ein, die Sie für die Einrichtung des Kubernetes-Clusters benötigen, einschließlich:
    -  **Benutzername**: Der Benutzername für die virtuellen Linux-Computer, die Teil des Kubernetes-Clusters und der DVM (Deployment Virtual Machine, virtueller Computer für die Bereitstellung) sind.
    -  **Öffentlicher SSH-Schlüssel**: Der Schlüssel, der für die Autorisierung bei allen Linux-Computern verwendet wird, die als Teil des Kubernetes-Clusters und der DVM erstellt werden.
    -  **Dienstprinzipal**: Die ID, die vom Kubernetes Azure-Cloudanbieter verwendet wird. Die als Anwendungs-ID identifizierte Client-ID, wenn Sie Ihren Dienstprinzipal erstellt haben. 
    -  **Clientgeheimnis**: Der Schlüssel, den Sie beim Erstellen Ihres Dienstprinzipals erstellt haben.

2. Erstellen Sie die Bereitstellungs-VM und die benutzerdefinierte Skripterweiterung.
    -  Erstellen Sie die Linux-Bereitstellungs-VM mit dem Marketplace-Linux-Image **Ubuntu Server 16.04-LTS**.
    -  Herunterladen Sie die benutzerdefinierte Skripterweiterung aus Marketplace herunter und führen Sie sie aus. Das Skript ist das **Benutzerdefinierte Skript für Linux 2.0**.
    -  Führen Sie das benutzerdefinierte DVM-Skript aus. Das Skript führt folgende Aufgaben aus:
        1. Ruft den Katalogendpunkt aus dem Azure Resource Manager-Metadatenendpunkt ab.
        2. Ruft die Active Directory-Ressourcen-ID aus dem Azure Resource Manager-Metadatenendpunkt ab.
        3. Lädt das API-Modell für die ACS-Engine.
        4. Stellt die ACS-Engine für den Kubernetes-Cluster bereit und speichert das Azure Stack-Cloudprofil in `/etc/kubernetes/azurestackcloud.json`.
3. Erstellen Sie die Master-VMs.

4. Laden Sie die Skripterweiterungen des Kunden herunter und führen Sie sie aus.

5. Führen Sie das Masterskript aus.

    Das Skript führt folgende Aufgaben aus:
    - Installiert etcd, Docker und Kubernetes-Ressourcen wie Kubelet. etcd ist ein verteilter Schlüsselwertspeicher, der eine Möglichkeit zum Speichern von Daten in einem Cluster aus Computern bereitstellt. Docker unterstützt reine Virtualisierungen auf Betriebssystemebene, die als Container bezeichnet werden. Kubelet ist der Knoten-Agent, der auf jedem Kubernetes-Knoten ausgeführt wird.
    - Richtet den etcd-Dienst ein.
    - Richtet den Kubelet-Dienst ein.
    - Startet Kubelet. Diese Aufgabe umfasst die folgenden Schritte:
        1. Starten des API-Diensts.
        2. Starten des Controllerdiensts.
        3. Starten des Schedulerdiensts.
6. Führen Sie die Agent-VMs aus.

7. Laden Sie die Skripterweiterung des Kunden herunter und führen Sie sie aus.

7. Führen Sie das Agent-Skript aus. Das benutzerdefinierte Agent-Skript führt folgende Aufgaben aus:
    - Installiert etcd.
    - Richtet den Kubelet-Dienst ein.
    - Tritt dem Kubernetes-Cluster bei.

## <a name="steps-for-troubleshooting"></a>Schritte zur Problembehandlung

Sie können Protokolle auf den virtuellen Computern erfassen, die Ihren Kubernetes-Cluster unterstützen. Sie können auch das Bereitstellungsprotokoll überprüfen. Möglicherweise müssen Sie sich auch an Ihren Azure Stack-Administrator wenden, um die von Ihnen verwendete Version von Azure Stack zu überprüfen und Protokolle von Azure Stack im Zusammenhang mit Ihrer Bereitstellung abzurufen.

1. Überprüfen Sie den [Bereitstellungsstatus](#review-deployment-status) und das [Abrufen der Protokolle](#get-logs-from-a-vm) vom Masterknoten in Ihrem Kubernetes-Cluster.
2. Stellen Sie sicher, dass Sie die neueste Version von Azure Stack verwenden. Wenn Sie sich nicht sicher sind, welche Version Sie verwenden sind, wenden Sie sich an den Azure Stack-Administrator. Für das Marketplace-Element Kubernetes-Cluster 0.3.0 ist Azure Stack Version 1808 oder höher erforderlich.
3.  Überprüfen Sie Ihre VM-Erstellungsdateien. Es können die folgenden Probleme auftreten:  
    - Der öffentliche Schlüssel ist möglicherweise ungültig. Überprüfen Sie den Schlüssel, den Sie erstellt haben.  
    - Die Erstellung des virtuellen Computers kann einen internen Fehler oder einen Erstellungsfehler ausgelöst haben. Fehler können durch eine Reihe von Faktoren verursacht werden, einschließlich Kapazitätseinschränkungen für Ihr Azure Stack-Abonnement.
    - Stellen Sie sicher, dass der vollqualifizierte Domänenname (FQDN) für den virtuellen Computer mit einem doppelten Präfix beginnt.
4.  Wenn der virtuelle Computer **OK** ist, untersuchen Sie die DVM. Wenn für die DVM eine Fehlermeldung vorliegt, sind die folgenden Ursachen möglich:

    - Der öffentliche Schlüssel ist möglicherweise ungültig. Überprüfen Sie den Schlüssel, den Sie erstellt haben.  
    - Sie müssen sich an Ihren Azure Stack-Administrator wenden, um die Protokolle für Azure Stack mit den privilegierten Endpunkten abzurufen. Weitere Informationen finden Sie unter [Azure Stack-Diagnosetools](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostics).
5. Wenn Sie Fragen zu Ihrer Bereitstellung haben, können Sie diese im [Azure Stack-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) stellen bzw. ermitteln, ob jemand die Frage dort bereits beantwortet hat. 

## <a name="review-deployment-status"></a>Überprüfen des Bereitstellungsstatus

Sie können den Bereitstellungsstatus überprüfen, wenn Sie Ihren Kubernetes-Cluster bereitstellen, um Probleme zu überprüfen.

1. Öffnen Sie das [Azure Stack-Portal](https://portal.local.azurestack.external).
2. Wählen Sie **Ressourcengruppen** aus, und wählen Sie dann den Namen der Ressourcengruppe aus, die beim Bereitstellen des Kubernetes-Clusters verwendet wurde.
3. Wählen Sie **Bereitstellungen** und dann den **Bereitstellungsnamen** aus.

    ![Problembehandlung](media/azure-stack-solution-template-kubernetes-trouble/azure-stack-kub-trouble-report.png)

4.  Sehen Sie sich das Problembehandlungsfenster an. Jede bereitgestellte Ressource stellt die folgenden Informationen zur Verfügung.
    
    | Eigenschaft | BESCHREIBUNG |
    | ----     | ----        |
    | Ressource | Der Name der Ressource. |
    | Typ | Der Ressourcenanbieter und der Typ der Ressource. |
    | Status | Der Status des Elements. |
    | TimeStamp | Der UTC-Zeitstempel. |
    | Vorgangsdetails | Die Details des Vorgangs, z.B. der Ressourcenanbieter für den Vorgang, der Ressourcenendpunkt und der Name der Ressource. |

    Jedes Element weist ein grünes oder rotes Statussymbol auf.

## <a name="get-logs-from-a-vm"></a>Abrufen von Protokollen von einem virtuellen Computer

Sie müssen eine Verbindung mit der Master-VM für Ihren Cluster herstellen, eine Bash-Eingabeaufforderung öffnen und ein Skript ausführen, um die Protokolle zu generieren. Den Master mit dem Namen `k8s-master-<sequence-of-numbers>` finden Sie in Ihrer Clusterressourcengruppe. 

### <a name="prerequisites"></a>Voraussetzungen

Sie benötigen eine Bash-Eingabeaufforderung auf dem Computer, den Sie zur Verwaltung von Azure Stack verwenden. Verwenden Sie Bash zum Ausführen der Skripts, die auf die Protokolle zugreifen. Auf einem Windows-Computer können Sie die mit Git installierte Bash-Eingabeaufforderung verwenden. Die neueste Version von Git finden Sie unter [Git-Downloads](https://git-scm.com/downloads).

### <a name="get-logs"></a>Abrufen von Protokollen

Führen Sie zum Abrufen der Protokolle die folgenden Schritte aus:

1. Öffnen Sie eine Bash-Eingabeaufforderung. Wenn Sie Git auf einem Windows-Computer verwenden, können Sie eine Bash-Eingabeaufforderung aus dem folgenden Pfad öffnen: `c:\programfiles\git\bin\bash.exe`.
2. Führen Sie die folgenden Bash-Befehle aus:

    ```Bash  
    mkdir -p $HOME/kuberneteslogs
    cd $HOME/kuberneteslogs
    curl -O https://raw.githubusercontent.com/msazurestackworkloads/azurestack-gallery/master/diagnosis/getkuberneteslogs.sh
    sudo chmod 744 getkuberneteslogs.sh
    ```

    > [!Note]  
    > Unter Windows müssen Sie `sudo` nicht ausführen. Sie können stattdessen `chmod 744 getkuberneteslogs.sh` verwenden.

3. Führen Sie in der gleichen Sitzung den folgenden Befehl aus, wobei die Parameter entsprechend Ihrer Umgebung angepasst werden müssen.

    ```Bash  
    ./getkuberneteslogs.sh --identity-file id_rsa --user azureuser --vmdhost 192.168.102.37
    ```

4. Überprüfen Sie die Parameter, und legen Sie die Werte basierend auf Ihrer Umgebung fest.
    | Parameter           | BESCHREIBUNG                                                                                                      | Beispiel                                                                       |
    |---------------------|------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|
    | -i, --identity-file | Die Datei mit dem privaten RSA-Schlüssel zum Herstellen einer Verbindung mit der Kubernetes-Master-VM. Der Schlüssel muss mit `-----BEGIN RSA PRIVATE KEY-----` beginnen. | C:\data\privatekey.pem                                                        |
    | -h, --host          | Die öffentliche IP-Adresse oder der vollqualifizierte Domänenname (FQDN) der Master-VM des Kubernetes-Clusters. Der Name des virtuellen Computers beginnt mit `k8s-master-`.                       | IP: 192.168.102.37<br><br>FQDN: k8s-12345.local.cloudapp.azurestack.external      |
    | -u, --user          | Der Benutzername der Master-VM des Kubernetes-Clusters. Dieser Name wird festgelegt, wenn Sie das Marketplace-Element konfigurieren.                                                                    | azureuser                                                                     |
    | -d, --vmdhost       | Die öffentliche IP-Adresse oder der FQDN der DVM. Der Name des virtuellen Computers beginnt mit `vmd-`.                                                       | IP: 192.168.102.38<br><br>DNS: vmd-dnsk8-frog.local.cloudapp.azurestack.external |

   Wenn Sie Ihre Parameterwerte hinzufügen, könnte es wie im folgenden Code aussehen:

    ```Bash  
    ./getkuberneteslogs.sh --identity-file "C:\secretsecret.pem" --user azureuser --vmdhost 192.168.102.37
     ```

    Bei einer erfolgreiche Ausführung werden die Protokolle erstellt.

    ![Generierte Protokolle](media/azure-stack-solution-template-kubernetes-trouble/azure-stack-generated-logs.png)


4. Rufen Sie die Protokolle in den vom Befehl erstellten Ordnern ab. Der Befehl erstellt neue Ordner und versieht sie mit Zeitstempeln.
    - KubernetesLogs*JJJJ-MM-TT-XX-XX-XX-XXX*
        - Dvmlogs
        - Acsengine-kubernetes-dvm.log

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen von Kubernetes in Azure Stack](azure-stack-solution-template-kubernetes-deploy.md)

[Hinzufügen eines Kubernetes-Clusters zum Marketplace (für Azure Stack-Bediener)](..\azure-stack-solution-template-kubernetes-cluster-add.md)

[Kubernetes in Azure](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
