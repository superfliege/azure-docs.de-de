---
title: Anzeigen von Azure Monitor für Containerprotokolle in Echtzeit | Microsoft-Dokumentation
description: In diesem Artikel wird die Echtzeitansicht von Containerprotokollen (stdout/stderr) ohne Verwendung von kubectl mit Azure Monitor für Container beschrieben.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/06/2018
ms.author: magoedte
ms.openlocfilehash: da11bb0669bf6bde2c65b2a7a0badaa1ae35abda
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53189121"
---
# <a name="how-to-view-container-logs-real-time-with-azure-monitor-for-containers-preview"></a>Anzeigen von Containerprotokollen in Echtzeit mit Azure Monitor für Container (Vorschauversion)
Dieses Feature, das sich derzeit in der Vorschauversion befindet, bietet eine Echtzeitansicht Ihrer Azure Kubernetes Service-Containerprotokolle (stdout/stderr), ohne dass Sie kubectl-Befehle ausführen müssen. Wenn Sie diese Option auswählen, wird ein neuer Bereich unterhalb der Tabelle der Containerleistungsdaten in der Ansicht **Container** angezeigt, der die von der Container-Engine erzeugte Liveprotokollierung veranschaulicht, um Sie bei der Fehlerbehebung in Echtzeit zu unterstützen.  

Liveprotokolle unterstützen drei verschiedene Methoden, um den Zugriff auf die Protokolle zu steuern:

1. AKS ohne aktivierte Kubernetes RBAC-Autorisierung 
2. Mit Kubernetes RBAC-Autorisierung aktivierter AKS
3. Mit auf SSO basiertem Azure Active Directory (AD) SAML aktiviertes AKS 

## <a name="kubernetes-cluster-without-rbac-enabled"></a>Kubernetes-Cluster ohne aktiviertes RBAC
 
Wenn Sie über einen Kubernetes-Cluster verfügen, der nicht mit der Kubernetes RBAC-Autorisierung konfiguriert oder mit einmaligem Anmelden von Azure AD integriert ist, brauchen Sie diese Schritte nicht auszuführen. Da die Kubernetes-Autorisierung die kube-api verwendet, sind schreibgeschützte Berechtigungen erforderlich.

## <a name="kubernetes-rbac-authorization"></a>Kubernetes RBAC-Autorisierung
Wenn die Kubernetes RBAC-Autorisierung aktiviert ist, müssen Sie die Clusterrollenbindung anwenden. Die folgenden Beispielschritte zeigen, wie Sie über diese yaml-Konfigurationsvorlage die Clusterrollenbindung konfigurieren.   

1. Kopieren Sie die yaml-Datei, fügen Sie sie anschließend ein, und speichern Sie sie als LogReaderRBAC.yaml.  

   ```
   kind: ClusterRole 
   apiVersion: rbac.authorization.k8s.io/v1 
   metadata:   
      name: containerHealth-log-reader 
   rules: 
      - apiGroups: [""]   
        resources: ["pods/log"]   
        verbs: ["get"] 
   --- 
   kind: ClusterRoleBinding 
   apiVersion: rbac.authorization.k8s.io/v1 
   metadata:   
      name: containerHealth-read-logs-global 
   subjects:   
      - kind: User     
        name: clusterUser
        apiGroup: rbac.authorization.k8s.io 
    roleRef:   
       kind: ClusterRole
       name: containerHealth-log-reader
       apiGroup: rbac.authorization.k8s.io 
   ```

2. Erstellen Sie die Clusterrollenbindung, indem Sie den folgenden Befehl ausführen: `kubectl create -f LogReaderRBAC.yaml`. 

## <a name="configure-aks-with-azure-active-directory"></a>Konfigurieren von AKS mit Azure Active Directory
Azure Kubernetes Service (AKS) kann für die Verwendung von Azure Active Directory (AD) für die Benutzerauthentifizierung konfiguriert werden. Informationen zur erstmaligen Konfiguration finden Sie unter [Integrieren von Azure Active Directory in Azure Kubernetes Service](../../aks/aad-integration.md). Während der Schritte zum Erstellen der [Clientanwendung](../../aks/aad-integration.md#create-client-application) und zum Angeben des **Umleitungs-URIs** müssen Sie einen weiteren URI zur Liste ** https://ininprodeusuxbase.microsoft.com/*** hinzufügen.  

>[!NOTE]
>Die Konfiguration der Authentifizierung mit Azure Active Directory für einmaliges Anmelden kann nur während der anfänglichen Bereitstellung eines neuen AKS-Clusters durchgeführt werden. Sie können einmaliges Anmelden nicht für einen bereits bereitgestellten AKS-Cluster konfigurieren.  
> 

## <a name="view-live-logs"></a>Anzeigen von Liveprotokollen
Wenn Sie **Container** anzeigen, können Sie **Containerprotokolle** oder **Containerliveprotokolle** anzeigen.  Wenn Sie **Containerliveprotokolle anzeigen** auswählen, wird ein neuer Bereich unterhalb der Tabelle der Containerleistungsdaten angezeigt, der die von der Container-Engine erzeugte Liveprotokollierung veranschaulicht, um Sie bei der Fehlerbehebung in Echtzeit zu unterstützen.  
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
2. Wählen Sie im Menü **Microsoft Azure** die Option **Überwachen** und dann **Container** aus.  
3. Wählen Sie einen Container aus der Liste unter der Ansicht **Überwachte Container** aus.  
4. Wählen Sie die Ansicht **Container** aus und im Eigenschaftenbereich für einen ausgewählten Container wird der Link **Containerliveprotokolle anzeigen** aufgeführt.  
5. Wenn der AKS-Cluster mit SSO über AAD konfiguriert ist, werden Sie bei der ersten Verwendung während dieser Browsersitzung zur Authentifizierung aufgefordert. Wählen Sie Ihr Konto aus und führen Sie die Authentifizierung mit Azure durch.  

Nach erfolgreicher Authentifizierung wird der Liveprotokollbereich im unteren Abschnitt des mittleren Bereichs angezeigt. Wenn die Abrufstatusanzeige ein grünes Häkchen anzeigt, das sich ganz rechts im Bereich befindet, bedeutet dies, dass Daten abgerufen werden können.
    
  ![Abgerufene Daten des Liveprotokollbereichs](./media/container-insights-live-logs/live-logs-pane-01.png)  

In der Suchleiste können Sie nach Schlüsselwort filtern, um den Text im Protokoll hervorzuheben.   

  ![Beispiel für den Filter des Liveprotokollbereichs](./media/container-insights-live-logs/live-logs-pane-filter-01.png)

Um das automatische Scrollen zu unterbrechen und das Verhalten des Bereichs zu steuern sowie manuell durch die gelesenen neuen Protokolldaten zu scrollen, klicken Sie auf die Option **Scrollen**.  Klicken Sie einfach erneut auf die Option **Scrollen**, um das automatische Scrollen wieder zu aktivieren.  Sie können den Abruf von Protokolldaten auch anhalten, indem Sie auf die Option **Anhalten** klicken. Wenn Sie bereit sind, den Vorgang fortzusetzen, klicken Sie einfach auf **Wiedergeben**.  

![Anhalten der Liveansicht im Liveprotokollbereich](./media/container-insights-live-logs/live-logs-pane-pause-01.png)

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Verwendung von Azure Monitor und zum Überwachen anderer Aspekte Ihres AKS-Clusters finden Sie unter [Anzeigen der Azure Kubernetes Service-Integrität](container-insights-analyze.md).