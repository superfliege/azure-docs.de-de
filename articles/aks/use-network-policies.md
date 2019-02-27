---
title: Sichere Pods mit Richtlinien für Netzwerke in Azure Kubernetes Service (AKS)
description: Erfahren Sie, wie Sie ein- und ausgehenden Datenverkehr bei Pods mittels Kubernetes-Netzwerkrichtlinien in Azure Kubernetes Service (AKS) sichern
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 02/12/2019
ms.author: iainfou
ms.openlocfilehash: 250c4fc6e51bacc68c965394b9fd430b1b75a52c
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56447173"
---
# <a name="secure-traffic-between-pods-using-network-policies-in-azure-kubernetes-service-aks"></a>Sicherer Datenverkehr zwischen Pods durch Netzwerkrichtlinien in Azure Kubernetes Service (AKS)

Wenn Sie moderne, auf Microservices basierende Anwendungen in Kubernetes ausführen, können Sie steuern, welche Komponenten miteinander kommunizieren können. Bei der Festlegung, wie Datenverkehr zwischen Pods in einem AKS-Cluster übermittelt werden kann, sollte das Prinzip der geringsten Rechte angewendet werden. Sie möchten beispielsweise wahrscheinlich den direkten Datenverkehr zu Back-End-Anwendungen blockieren. In Kubernetes können Sie mit dem Feature *Network Policy (Netzwerkrichtlinie)* die Regeln für eingehenden und ausgehenden Datenverkehr zwischen Pods in einem Cluster definieren.

Dieser Artikel veranschaulicht die Verwendung der Netzwerkrichtlinien, um den Fluss des Datenverkehrs zwischen Pods in AKS zu steuern.

> [!IMPORTANT]
> Diese Funktion steht derzeit als Vorschau zur Verfügung. Wenn Sie Vorschauversionen nutzen möchten, müssen Sie die [zusätzlichen Nutzungsbedingungen][terms-of-use] akzeptieren. Einige Aspekte dieses Features werden bis zur allgemeinen Verfügbarkeit unter Umständen noch geändert.

## <a name="before-you-begin"></a>Voraussetzungen

Es muss die Azure CLI-Version 2.0.56 oder höher installiert und konfiguriert sein. Führen Sie  `az --version` aus, um die Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie weitere Informationen unter [Installieren der Azure CLI][install-azure-cli].

Um einen AKS-Cluster mit Netzwerkrichtlinie zu erstellen, aktivieren Sie zuerst Sie ein Featureflag für Ihr Abonnement. Um das Featureflag *EnableNetworkPolicy* zu registrieren, verwenden Sie den Befehl [az feature register][az-feature-register] wie im folgenden Beispiel gezeigt:

```azurecli-interactive
az feature register --name EnableNetworkPolicy --namespace Microsoft.ContainerService
```

Es dauert einige Minuten, bis der Status *Registered (Registriert)* angezeigt wird. Sie können den Registrierungsstatus mithilfe des Befehls [az feature list][az-feature-list] überprüfen:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/EnableNetworkPolicy')].{Name:name,State:properties.state}"
```

Wenn Sie fertig sind, aktualisieren Sie die Registrierung des *Microsoft.ContainerService*-Ressourcenanbieters mit dem Befehl [az provider register][az-provider-register]:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="overview-of-network-policy"></a>Übersicht über die Netzwerkrichtlinie

Standardmäßig können alle Pods in einem AKS-Cluster Datenverkehr ohne Einschränkungen senden und empfangen. Zur Verbesserung der Sicherheit können Sie Regeln definieren, die den Datenverkehrsfluss steuern. Back-End-Anwendungen werden z.B. häufig nur für erforderlichen Front-End-Dienste verfügbar gemacht, oder Datenbankkomponenten sind nur für die Anwendungsebenen zugänglich, die eine Verbindung zu ihnen herstellen.

Netzwerkrichtlinien sind eine Kubernetes-Ressource, mit der Sie den Datenverkehrsfluss zwischen Pods steuern können. Anhand von Einstellungen wie zugewiesene Bezeichnungen, Namespace oder Port für den Datenverkehr können Sie Datenverkehr zulassen oder verweigern. Netzwerkrichtlinien werden als YAML-Manifeste definiert und können als Bestandteil eines größeren Manifests, das auch eine Bereitstellung oder einen Dienst erstellt, einbezogen werden.

Um Netzwerkrichtlinien in Aktion zu sehen, erstellen Sie eine Richtlinie, die Datenverkehr wie folgt definiert, und erweitern sie dann:

* Lehnen Sie jeglichen Datenverkehr zum Pod ab.
* Lassen Sie Datenverkehr basierend auf Podbezeichnungen zu.
* Lassen Sie Datenverkehr basierend auf dem Namespace zu.

## <a name="create-an-aks-cluster-and-enable-network-policy"></a>Erstellen eines AKS-Clusters und Aktivieren der Netzwerkrichtlinie

Die Netzwerkrichtlinie kann nur aktiviert werden, wenn der Cluster erstellt wird. Ohne einen vorhandenen AKS-Cluster können Sie keine Netzwerkrichtlinie aktivieren. 

Um eine Netzwerkrichtlinie mit einem AKS-Cluster zu verwenden, müssen Sie das [Azure CNI-Plug-In][azure-cni] verwenden und eigene virtuelle Netzwerke und Subnetze definieren. Detaillierte Informationen zur Planung der erforderlichen Subnetzadressbereiche finden Sie unter [Konfigurieren von Azure CNI-Netzwerken in Azure Kubernetes Service (AKS)][use-advanced-networking].

Das folgende Beispielskript:

* Erstellt ein virtuelles Netzwerk und ein Subnetz.
* Erstellt einen Azure Active Directory-Dienstprinzipal (AD) für die Verwendung mit dem AKS-Cluster.
* Weist dem Dienstprinzipal des AKS-Clusters in einen virtuellen Netzwerk *Mitwirkender*-Berechtigungen zu.
* Erstellt einen AKS-Cluster im definierten virtuellen Netzwerk und aktiviert die Netzwerkrichtlinie.

Geben Sie Ihr eigenes sicheres *SP_PASSWORD* ein. Ersetzen Sie ggf. die Variablen *RESOURCE_GROUP_NAME* und *CLUSTER_NAME*:

```azurecli-interactive
SP_PASSWORD=mySecurePassword
RESOURCE_GROUP_NAME=myResourceGroup-NP
CLUSTER_NAME=myAKSCluster
LOCATION=canadaeast

# Create a resource group
az group create --name $RESOURCE_GROUP_NAME --location $LOCATION

# Create a virtual network and subnet
az network vnet create \
    --resource-group $RESOURCE_GROUP_NAME \
    --name myVnet \
    --address-prefixes 10.0.0.0/8 \
    --subnet-name myAKSSubnet \
    --subnet-prefix 10.240.0.0/16

# Create a service principal and read in the application ID
SP_ID=$(az ad sp create-for-rbac --password $SP_PASSWORD --skip-assignment --query [appId] -o tsv)

# Wait 15 seconds to make sure that service principal has propagated
echo "Waiting for service principal to propagate..."
sleep 15

# Get the virtual network resource ID
VNET_ID=$(az network vnet show --resource-group $RESOURCE_GROUP_NAME --name myVnet --query id -o tsv)

# Assign the service principal Contributor permissions to the virtual network resource
az role assignment create --assignee $SP_ID --scope $VNET_ID --role Contributor

# Get the virtual network subnet resource ID
SUBNET_ID=$(az network vnet subnet show --resource-group $RESOURCE_GROUP_NAME --vnet-name myVnet --name myAKSSubnet --query id -o tsv)

# Create the AKS cluster and specify the virtual network and service principal information
# Enable network policy using the `--network-policy` parameter
az aks create \
    --resource-group $RESOURCE_GROUP_NAME \
    --name $CLUSTER_NAME \
    --node-count 1 \
    --kubernetes-version 1.12.4 \
    --generate-ssh-keys \
    --network-plugin azure \
    --service-cidr 10.0.0.0/16 \
    --dns-service-ip 10.0.0.10 \
    --docker-bridge-address 172.17.0.1/16 \
    --vnet-subnet-id $SUBNET_ID \
    --service-principal $SP_ID \
    --client-secret $SP_PASSWORD \
    --network-policy calico
```

Die Erstellung des Clusters dauert einige Minuten. Konfigurieren Sie anschließend `kubectl` mit dem Befehl [az aks get-credentials][az-aks-get-credentials], um die Verbindung mit Ihrem Kubernetes-Cluster herzustellen. Mit diesem Befehl werden die Anmeldeinformationen heruntergeladen, und die Kubernetes-Befehlszeilenschnittstelle wird für deren Verwendung konfiguriert:

```azurecli-interactive
az aks get-credentials --resource-group $RESOURCE_GROUP_NAME --name $CLUSTER_NAME
```

## <a name="deny-all-inbound-traffic-to-a-pod"></a>Verweigern sämtlichen eingehenden Datenverkehrs zu einem Pod

Bevor Sie Regeln zum Zulassen bestimmten Netzwerkdatenverkehrs definieren, erstellen Sie zuerst eine Netzwerkrichtlinie, um sämtlichen Datenverkehr abzulehnen. Diese Richtlinie bietet Ihnen einen Ausgangspunkt, um nur den gewünschten Datenverkehr auf die Whitelist zu setzen. Sie können auch deutlich erkennen, dass Datenverkehr bei Anwendung der Netzwerkrichtlinie verworfen wird.

Für unsere Beispielanwendungsumgebung und die Regeln für den Netzwerkdatenverkehr erstellen wir zunächst einen Namespace mit dem Namen *development* zur Ausführung unserer Beispielpods:

```console
kubectl create namespace development
kubectl label namespace/development purpose=development
```

Nun erstellen Sie einen Beispiel-Back-End-Pod, der NGINX ausführt. Dieser Back-End-Pod kann verwendet werden, um eine Beispielanwendung auf Back-End-Web-Basis zu simulieren. Erstellen Sie diesen Pod im *development* -Namespace, und öffnen Sie Port *80* für den Webdatenverkehr. Geben Sie dem Pod die Bezeichnung *app=webapp,role=backend*, damit wir im nächsten Abschnitt eine auf ihn zielgerichtete Netzwerkrichtlinie einfügen können:

```console
kubectl run backend --image=nginx --labels app=webapp,role=backend --namespace development --expose --port 80 --generator=run-pod/v1
```

So testen Sie, ob Sie erfolgreich die NGINX-Standardwebseite erreichen, einen anderen Pod erstellen und eine Terminalsitzung anfügen können:

```console
kubectl run --rm -it --image=alpine network-policy --namespace development --generator=run-pod/v1
```

Vergewissern Sie sich an der Shelleingabeaufforderung mit `wget`, dass Sie auf die NGINX-Standardwebseite zugreifen können:

```console
wget -qO- http://backend
```

Die folgende Beispielausgabe zeigt, dass die NGINX-Standardwebseite zurückgegeben wird:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

Schließen Sie die angefügte Terminalsitzung. Der Testpod wird automatisch gelöscht:

```console
exit
```

### <a name="create-and-apply-a-network-policy"></a>Erstellen und Anwenden einer Netzwerkrichtlinie

Nun haben Sie sich vergewissert, dass Sie auf die grundlegende NGINX-Webseite auf dem Beispiel-Back-End-Pod zugreifen können, und erstellen eine Netzwerkrichtlinie, um sämtlichen Datenverkehr abzulehnen. Erstellen Sie eine Datei namens `backend-policy.yaml`, und fügen Sie das folgende YAML-Manifest ein. Dieses Manifest verwendet einen *podSelector* zum Anfügen der Richtlinie an Pods, die die Bezeichnung *app:webapp,role:backend* wie Ihr Beispiel-NGINX-Pod haben. Da unter *ingress* keine Regeln definiert sind, wird sämtlicher in den Pod eingehender Datenverkehr verweigert:

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: backend-policy
  namespace: development
spec:
  podSelector:
    matchLabels:
      app: webapp
      role: backend
  ingress: []
```

Wenden Sie die Netzwerkrichtlinie über den Befehl [kubectl apply][kubectl-apply] an, und geben Sie den Namen Ihres YAML-Manifests an:

```azurecli-interactive
kubectl apply -f backend-policy.yaml
```

### <a name="test-the-network-policy"></a>Testen der Netzwerkrichtlinie

Jetzt werden wir sehen, ob Sie wieder auf dem Back-End-Pod auf die NGINX-Webseite zugreifen können. Erstellen Sie einen anderen Testpod, und fügen Sie an diesen eine Terminalsitzung an:

```console
kubectl run --rm -it --image=alpine network-policy --namespace development --generator=run-pod/v1
```

Vergewissern Sie sich an der Shelleingabeaufforderung mit `wget`, dass Sie auf die NGINX-Standardwebseite zugreifen können. Legen Sie dieses Mal einen Timeoutwert auf *2* Sekunden fest. Die Netzwerkrichtlinie blockiert jetzt sämtlichen eingehenden Datenverkehr, damit die Seite nicht geladen werden kann, wie im folgenden Beispiel gezeigt:

```console
$ wget -qO- --timeout=2 http://backend

wget: download timed out
```

Schließen Sie die angefügte Terminalsitzung. Der Testpod wird automatisch gelöscht:

```console
exit
```

## <a name="allow-inbound-traffic-based-on-a-pod-label"></a>Zulassen eingehenden Datenverkehrs basierend auf einer Podbezeichnung

Im vorherigen Abschnitt wurde ein Back-End-NGINX-Pod geplant, und eine Netzwerkrichtlinie zum Verweigern des gesamten Datenverkehrs wurde erstellt. Nun erstellen Sie einen Front-End-Pod und aktualisieren die Netzwerkrichtlinie, sodass Datenverkehr von Front-End-Pods zugelassen wird.

Aktualisieren Sie die Netzwerkrichtlinie zum Zulassen von Datenverkehr von Pods mit den Bezeichnungen *app:webapp,role:frontend* und in einem beliebigen Namespace. Bearbeiten Sie die vorherige *backend-policy.yaml*-Datei, und fügen Sie eine Eingangsregel *matchLabels* hinzu, sodass das Manifest dem folgenden Beispiel entspricht:

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: backend-policy
  namespace: development
spec:
  podSelector:
    matchLabels:
      app: webapp
      role: backend
  ingress:
  - from:
    - namespaceSelector: {}
      podSelector:
        matchLabels:
          app: webapp
          role: frontend
```

> [!NOTE]
> Diese Netzwerkrichtlinie verwendet ein *namespaceSelector*- und ein *podSelector*-Element für die Eingangsregel. Die YAML-Syntax ist wichtig in Bezug darauf, ob die Eingangsregeln additiv sind oder nicht. In diesem Beispiel müssen beide Elemente übereinstimmen, damit die Eingangsregel angewendet wird. Kubernetes-Versionen vor *1.12* interpretieren diese Elemente unter Umständen nicht korrekt und beschränken den Netzwerkdatenverkehr ggf. nicht wie erwartet. Weitere Informationen finden Sie unter [Behavior of to and from selectors][policy-rules] (Verhalten der Selektoren „to“ und „from“).

Wenden Sie die aktualisierte Netzwerkrichtlinie über den Befehl [kubectl apply][kubectl-apply] an, und geben Sie den Namen Ihres YAML-Manifests an:

```azurecli-interactive
kubectl apply -f backend-policy.yaml
```

Planen Sie jetzt einen Pod mit der Bezeichnung *app=webapp,role=frontend*, und fügen Sie eine Terminalsitzung an:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace development --generator=run-pod/v1
```

Vergewissern Sie sich an der Shelleingabeaufforderung mit `wget`, dass Sie auf die NGINX-Standardwebseite zugreifen können:

```console
wget -qO- http://backend
```

Da die Eingangsregel Datenverkehr mit Pods zulässt, die die Bezeichnungen *app: webapp,role: frontend* haben, ist der Datenverkehr vom Front-End-Pod zugelassen. Die folgende Beispielausgabe zeigt, dass die NGINX-Standardwebseite zurückgegeben wird:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

Schließen Sie die angefügte Terminalsitzung. Der Pod wird automatisch gelöscht:

```console
exit
```

### <a name="test-a-pod-without-a-matching-label"></a>Testen eines Pods ohne passende Bezeichnung

Die Netzwerkrichtlinie lässt Datenverkehr von Pods mit der Bezeichnung *app: webapp,role: frontend* zu, sollte aber den gesamten sonstigen Datenverkehr verweigern. Testen Sie nun, ob ein anderer Pod ohne diese Bezeichnungen nicht auf den Back-End-NGINX-Pod zugreifen kann. Erstellen Sie einen anderen Testpod, und fügen Sie an diesen eine Terminalsitzung an:

```console
kubectl run --rm -it --image=alpine network-policy --namespace development --generator=run-pod/v1
```

Vergewissern Sie sich an der Shelleingabeaufforderung mit `wget`, dass Sie auf die NGINX-Standardwebseite zugreifen können. Die Netzwerkrichtlinie blockiert sämtlichen eingehenden Datenverkehr, damit die Seite nicht geladen werden kann, wie im folgenden Beispiel gezeigt:

```console
$ wget -qO- --timeout=2 http://backend

wget: download timed out
```

Schließen Sie die angefügte Terminalsitzung. Der Testpod wird automatisch gelöscht:

```console
exit
```

## <a name="allow-traffic-only-from-within-a-defined-namespace"></a>Zulassen von Datenverkehr nur, wenn er von einem definierten Namespace ausgeht

In den vorherigen Beispielen erstellten Sie eine Netzwerkrichtlinie, die sämtlichen Datenverkehr verweigerte, und aktualisierten die Richtlinie dann, um Datenverkehr von Pods mit einer bestimmten Bezeichnung zuzulassen. Eine weitere häufige Anforderung ist das Begrenzen des Datenverkehrs auf einen bestimmten Namespace. Wenn die vorherigen Beispiele sich auf den Datenverkehr in einem *development*-Namespace bezögen, könnten Sie eine Netzwerkrichtlinie erstellen, die verhindert, dass Datenverkehr aus einem anderen Namespace wie etwa *production* die Pods erreicht.

Erstellen Sie zunächst einen neuen Namespace, um einen production-Namespace zu simulieren:

```console
kubectl create namespace production
kubectl label namespace/production purpose=production
```

Planen Sie einen Testpod im *production*-Namespace mit der Bezeichnung *app=webapp,role=frontend*. Fügen Sie eine Terminalsitzung an:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace production --generator=run-pod/v1
```

Vergewissern Sie sich an der Shelleingabeaufforderung mit `wget`, dass Sie auf die NGINX-Standardwebseite zugreifen können:

```console
wget -qO- http://backend.development
```

Wenn die Bezeichnungen für den Pod mit dem übereinstimmen, was derzeit in der Netzwerkrichtlinie zulässig ist, wird der Datenverkehr zugelassen. Die Netzwerkrichtlinie beachtet nicht die Namespaces, sondern nur die Podbezeichnungen. Die folgende Beispielausgabe zeigt, dass die NGINX-Standardwebseite zurückgegeben wird:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

Schließen Sie die angefügte Terminalsitzung. Der Testpod wird automatisch gelöscht:

```console
exit
```

### <a name="update-the-network-policy"></a>Aktualisieren der Netzwerkrichtlinie

Jetzt aktualisieren wir den Eingangsregelabschnitt *namespaceSelector*, um nur vom *development*-Namespace ausgehenden Datenverkehr zuzulassen. Bearbeiten Sie die Manifestdatei *backend-policy.yaml* wie im folgenden Beispiel gezeigt:

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: backend-policy
  namespace: development
spec:
  podSelector:
    matchLabels:
      app: webapp
      role: backend
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          purpose: development
      podSelector:
        matchLabels:
          app: webapp
          role: frontend
```

In komplexeren Beispielen könnten Sie mehrere Eingangsregeln definieren, z.B. die Verwendung eines *namespaceSelector* und dann eines *podSelector*.

Wenden Sie die aktualisierte Netzwerkrichtlinie über den Befehl [kubectl apply][kubectl-apply] an, und geben Sie den Namen Ihres YAML-Manifests an:

```azurecli-interactive
kubectl apply -f backend-policy.yaml
```

### <a name="test-the-updated-network-policy"></a>Testen der aktualisierten Netzwerkrichtlinie

Planen Sie nun einen anderen Pod im *production*-Namespace, und fügen Sie eine Terminalsitzung an:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace production --generator=run-pod/v1
```

Vergewissern Sie sich an der Shelleingabeaufforderung mit `wget`, dass die Netzwerkrichtlinie jetzt Datenverkehr verweigert:

```console
$ wget -qO- --timeout=2 http://backend.development

wget: download timed out
```

Schließen Sie den Test-Pod:

```console
exit
```

Da jetzt der Datenverkehr vom *production*-Namespace verweigert wird, planen Sie wieder einen Testpod im *development*-Namespace, und fügen Sie eine Terminalsitzung an:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace development --generator=run-pod/v1
```

Vergewissern Sie sich an der Shelleingabeaufforderung mit `wget`, dass die Netzwerkrichtlinie Datenverkehr zulässt:

```console
wget -qO- http://backend
```

Wenn der Pod in dem Namespace geplant wird, der mit dem übereinstimmt, was derzeit in der Netzwerkrichtlinie zulässig ist, wird der Datenverkehr zugelassen. Die folgende Beispielausgabe zeigt, dass die NGINX-Standardwebseite zurückgegeben wird:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

Schließen Sie die angefügte Terminalsitzung. Der Testpod wird automatisch gelöscht:

```console
exit
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In diesem Artikel haben wir zwei Namespaces erstellt und eine Netzwerkrichtlinie angewendet. Um diese Ressourcen zu bereinigen, verwenden Sie den [kubectl delete] [ kubectl-delete]-Befehl, und geben Sie die Ressourcennamen wie folgt an:

```console
kubectl delete namespace production
kubectl delete namespace development
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Netzwerkressourcen in Kubernetes finden Sie unter [Netzwerkkonzepte für Anwendungen in Azure Kubernetes Service (AKS)][concepts-network].

Weitere Informationen zur Verwendung von Richtlinien finden Sie unter [Network Policies (Netzwerkrichtlinien)][kubernetes-network-policies].

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubernetes-network-policies]: https://kubernetes.io/docs/concepts/services-networking/network-policies/
[azure-cni]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[policy-rules]: https://kubernetes.io/docs/concepts/services-networking/network-policies/#behavior-of-to-and-from-selectors

<!-- LINKS - internal -->
[install-azure-cli]: /cli/azure/install-azure-cli
[use-advanced-networking]: configure-advanced-networking.md
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[concepts-network]: concepts-network.md
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
