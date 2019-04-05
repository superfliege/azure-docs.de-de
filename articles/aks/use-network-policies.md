---
title: Sichere Pods mit Richtlinien für Netzwerke in Azure Kubernetes Service (AKS)
description: Es wird beschrieben, wie Sie ein- und ausgehenden Datenverkehr bei Pods mittels Kubernetes-Netzwerkrichtlinien in Azure Kubernetes Service (AKS) schützen.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 02/12/2019
ms.author: iainfou
ms.openlocfilehash: a20dfcd9e2ef12252235b74455964d115d9aef9b
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58181485"
---
# <a name="preview---secure-traffic-between-pods-using-network-policies-in-azure-kubernetes-service-aks"></a>Vorschauversion: Sicherer Datenverkehr zwischen Pods durch Netzwerkrichtlinien in Azure Kubernetes Service (AKS)

Wenn Sie moderne, auf Microservices basierende Anwendungen in Kubernetes ausführen, können Sie steuern, welche Komponenten miteinander kommunizieren können. Bei der Festlegung, wie Datenverkehr zwischen Pods in einem AKS-Cluster (Azure Kubernetes Service) übermittelt werden kann, sollte das Prinzip der geringsten Rechte angewendet werden. Angenommen, Sie möchten den direkten Datenverkehr zu Back-End-Anwendungen blockieren. Mit dem Feature *Netzwerkrichtlinie* in Kubernetes können Sie die Regeln für eingehenden und ausgehenden Datenverkehr zwischen Pods in einem Cluster definieren.

Calico, eine Open Source-Lösung für Netzwerke und Netzwerksicherheit von Tigera, verfügt über ein Modul für Netzwerkrichtlinien, mit dem Regeln für Kubernetes-Netzwerkrichtlinien implementiert werden können. In diesem Artikel wird veranschaulicht, wie Sie das Calico-Modul für Netzwerkrichtlinien installieren und Kubernetes-Netzwerkrichtlinien erstellen, um den Datenverkehrsfluss zwischen Pods in AKS zu steuern.

> [!IMPORTANT]
> AKS-Previewfunktionen stehen gemäß dem Self-Service- und Aktivierungsprinzip zur Verfügung. Vorschauversionen werden zum Sammeln von Feedback und Fehlern mithilfe unserer Community bereitgestellt. Allerdings werden sie vom technischen Support von Azure nicht unterstützt. Wenn Sie einen Cluster erstellen oder diese Features bereits vorhandenen Clustern hinzufügen, wird dieser Cluster erst dann unterstützt, wenn das Feature nicht mehr in der Vorschauphase ist und die allgemeine Verfügbarkeit erreicht ist.
>
> Wenn Sie Probleme mit Preview-Funktionen haben, öffnen Sie [ein Problemticket im GitHub-Repository von AKS ][aks-github] mit dem Namen der Preview-Funktion im Fehlertitel.

## <a name="before-you-begin"></a>Voraussetzungen

Es muss die Azure CLI-Version 2.0.56 oder höher installiert und konfiguriert sein. Führen Sie  `az --version` aus, um die Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie weitere Informationen unter [Installieren der Azure CLI][install-azure-cli].

Um einen AKS-Cluster zu erstellen, für den Netzwerkrichtlinien verwendet werden können, aktivieren Sie zuerst ein Featureflag für Ihr Abonnement. Um das Featureflag *EnableNetworkPolicy* zu registrieren, verwenden Sie den Befehl [az feature register][az-feature-register] wie im folgenden Beispiel gezeigt:

```azurecli-interactive
az feature register --name EnableNetworkPolicy --namespace Microsoft.ContainerService
```

Es dauert einige Minuten, bis der Status *Registered (Registriert)* angezeigt wird. Sie können den Registrierungsstatus mithilfe des Befehls [az feature list][az-feature-list] überprüfen:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/EnableNetworkPolicy')].{Name:name,State:properties.state}"
```

Wenn der Vorgang abgeschlossen ist, können Sie die Registrierung des *Microsoft.ContainerService*-Ressourcenanbieters mit dem Befehl [az provider register][az-provider-register] aktualisieren:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="overview-of-network-policy"></a>Übersicht über die Netzwerkrichtlinie

Standardmäßig können alle Pods in einem AKS-Cluster Datenverkehr ohne Einschränkungen senden und empfangen. Zur Verbesserung der Sicherheit können Sie Regeln definieren, die den Datenverkehrsfluss steuern. Back-End-Anwendungen werden häufig nur für erforderliche Front-End-Dienste verfügbar gemacht. Oder Datenbankkomponenten sind nur für die Anwendungsebenen zugänglich, die eine Verbindung damit herstellen.

Netzwerkrichtlinien sind eine Kubernetes-Ressource, mit der Sie den Datenverkehrsfluss zwischen Pods steuern können. Anhand von Einstellungen wie zugewiesene Bezeichnungen, Namespace oder Port für den Datenverkehr können Sie Datenverkehr zulassen oder ablehnen. Netzwerkrichtlinien werden als YAML-Manifeste definiert. Diese Richtlinien können Teil eines umfassenderen Manifests sein, mit dem auch eine Bereitstellung oder ein Dienst erstellt wird.

Um Netzwerkrichtlinien in Aktion zu sehen, erstellen Sie eine Richtlinie, die Datenverkehr definiert, und erweitern sie dann:

* Lehnen Sie jeglichen Datenverkehr zum Pod ab.
* Lassen Sie Datenverkehr basierend auf Podbezeichnungen zu.
* Lassen Sie Datenverkehr basierend auf dem Namespace zu.

## <a name="create-an-aks-cluster-and-enable-network-policy"></a>Erstellen eines AKS-Clusters und Aktivieren der Netzwerkrichtlinie

Die Netzwerkrichtlinie kann nur aktiviert werden, wenn der Cluster erstellt wird. Ohne einen vorhandenen AKS-Cluster können Sie keine Netzwerkrichtlinie aktivieren. 

Um eine Netzwerkrichtlinie mit einem AKS-Cluster zu verwenden, müssen Sie das [Azure CNI-Plug-In][azure-cni] verwenden und eigene virtuelle Netzwerke und Subnetze definieren. Detaillierte Informationen zur Planung der erforderlichen Subnetzadressbereiche finden Sie unter [Konfigurieren von Azure CNI-Netzwerken in Azure Kubernetes Service (AKS)][use-advanced-networking].

Das folgende Beispielskript:

* Erstellt ein virtuelles Netzwerk und ein Subnetz.
* Erstellt einen Azure AD-Dienstprinzipal (Azure Active Directory) für die Verwendung mit dem AKS-Cluster.
* Weist dem Dienstprinzipal des AKS-Clusters in einen virtuellen Netzwerk *Mitwirkender*-Berechtigungen zu.
* Erstellt einen AKS-Cluster im definierten virtuellen Netzwerk und aktiviert die Netzwerkrichtlinie.

Geben Sie Ihr eigenes sicheres *SP_PASSWORD* ein. Sie können die Variablen *RESOURCE_GROUP_NAME* und *CLUSTER_NAME* ersetzen:

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
# Enable network policy by using the `--network-policy` parameter
az aks create \
    --resource-group $RESOURCE_GROUP_NAME \
    --name $CLUSTER_NAME \
    --node-count 1 \
    --kubernetes-version 1.12.6 \
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

Die Erstellung des Clusters dauert einige Minuten. Wenn der Cluster bereit ist, können Sie `kubectl` mit dem Befehl [az aks get-credentials][az-aks-get-credentials] konfigurieren, um die Verbindung mit Ihrem Kubernetes-Cluster herzustellen. Mit diesem Befehl werden die Anmeldeinformationen heruntergeladen, und die Kubernetes-Befehlszeilenschnittstelle wird für deren Verwendung konfiguriert:

```azurecli-interactive
az aks get-credentials --resource-group $RESOURCE_GROUP_NAME --name $CLUSTER_NAME
```

## <a name="deny-all-inbound-traffic-to-a-pod"></a>Verweigern sämtlichen eingehenden Datenverkehrs zu einem Pod

Bevor Sie Regeln zum Zulassen bestimmten Netzwerkdatenverkehrs definieren, erstellen Sie zuerst eine Netzwerkrichtlinie, um sämtlichen Datenverkehr abzulehnen. Diese Richtlinie bietet Ihnen einen Ausgangspunkt, um nur den gewünschten Datenverkehr auf die Whitelist zu setzen. Sie können auch deutlich erkennen, dass Datenverkehr bei Anwendung der Netzwerkrichtlinie verworfen wird.

Für die Beispielanwendungsumgebung und die Regeln für den Netzwerkdatenverkehr erstellen wir zunächst einen Namespace mit dem Namen *development* zur Ausführung der Beispielpods:

```console
kubectl create namespace development
kubectl label namespace/development purpose=development
```

Erstellen Sie einen Beispiel-Back-End-Pod, der NGINX ausführt. Dieser Back-End-Pod kann verwendet werden, um eine Beispielanwendung auf Back-End-Web-Basis zu simulieren. Erstellen Sie diesen Pod im *development* -Namespace, und öffnen Sie Port *80* für den Webdatenverkehr. Geben Sie dem Pod die Bezeichnung *app=webapp,role=backend*, damit wir im nächsten Abschnitt eine auf ihn zielgerichtete Netzwerkrichtlinie einfügen können:

```console
kubectl run backend --image=nginx --labels app=webapp,role=backend --namespace development --expose --port 80 --generator=run-pod/v1
```

Erstellen Sie einen weiteren Pod, und fügen Sie eine Terminalsitzung an, um zu testen, ob Sie die NGINX-Standardwebseite erreichen können:

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

Schließen Sie die angefügte Terminalsitzung. Der Testpod wird automatisch gelöscht.

```console
exit
```

### <a name="create-and-apply-a-network-policy"></a>Erstellen und Anwenden einer Netzwerkrichtlinie

Nachdem Sie sich nun vergewissert haben, dass Sie die grundlegende NGINX-Webseite auf dem Beispiel-Back-End-Pod verwenden können, können Sie eine Netzwerkrichtlinie erstellen, um sämtlichen Datenverkehr abzulehnen. Erstellen Sie eine Datei namens `backend-policy.yaml`, und fügen Sie das folgende YAML-Manifest ein. Dieses Manifest verwendet einen *podSelector* zum Anfügen der Richtlinie an Pods, die die Bezeichnung *app:webapp,role:backend* haben (wie Ihr Beispiel-NGINX-Pod). Da unter *ingress* keine Regeln definiert sind, wird sämtlicher in den Pod eingehender Datenverkehr verweigert:

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


Jetzt prüfen wir, ob Sie die NGINX-Webseite auf dem Back-End-Pod verwenden können. Erstellen Sie einen anderen Testpod, und fügen Sie an diesen eine Terminalsitzung an:

```console
kubectl run --rm -it --image=alpine network-policy --namespace development --generator=run-pod/v1
```

Vergewissern Sie sich an der Shelleingabeaufforderung mit `wget`, dass Sie auf die NGINX-Standardwebseite zugreifen können. Legen Sie dieses Mal einen Timeoutwert auf *2* Sekunden fest. Die Netzwerkrichtlinie blockiert jetzt sämtlichen eingehenden Datenverkehr, damit die Seite nicht geladen werden kann. Dies ist im folgenden Beispiel dargestellt:

```console
$ wget -qO- --timeout=2 http://backend

wget: download timed out
```

Schließen Sie die angefügte Terminalsitzung. Der Testpod wird automatisch gelöscht.

```console
exit
```

## <a name="allow-inbound-traffic-based-on-a-pod-label"></a>Zulassen eingehenden Datenverkehrs basierend auf einer Podbezeichnung

Im vorherigen Abschnitt wurde ein Back-End-NGINX-Pod geplant und eine Netzwerkrichtlinie zum Ablehnen des gesamten Datenverkehrs erstellt. Wir erstellen nun einen Front-End-Pod und aktualisieren die Netzwerkrichtlinie, damit Datenverkehr von Front-End-Pods zugelassen wird.

Aktualisieren Sie die Netzwerkrichtlinie zum Zulassen von Datenverkehr von Pods mit den Bezeichnungen *app:webapp,role:frontend* und in einem beliebigen Namespace. Bearbeiten Sie die vorherige *backend-policy.yaml*-Datei, und fügen Sie Eingangsregeln vom Typ *matchLabels* hinzu, damit das Manifest dem folgenden Beispiel entspricht:

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
> Diese Netzwerkrichtlinie verwendet ein *namespaceSelector*- und ein *podSelector*-Element für die Eingangsregel. Die YAML-Syntax ist wichtig, damit die Eingangsregeln additiv sind. In diesem Beispiel müssen beide Elemente übereinstimmen, damit die Eingangsregel angewendet wird. Kubernetes-Versionen vor *1.12* interpretieren diese Elemente unter Umständen nicht korrekt und beschränken den Netzwerkdatenverkehr ggf. nicht wie erwartet. Weitere Informationen zu diesem Verhalten finden Sie unter [Behavior of to and from selectors][policy-rules] (Verhalten von to- und from-Selektoren).

Wenden Sie die aktualisierte Netzwerkrichtlinie über den Befehl [kubectl apply][kubectl-apply] an, und geben Sie den Namen Ihres YAML-Manifests an:

```azurecli-interactive
kubectl apply -f backend-policy.yaml
```

Planen Sie einen Pod mit der Bezeichnung *app=webapp,role=frontend*, und fügen Sie eine Terminalsitzung an:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace development --generator=run-pod/v1
```

Vergewissern Sie sich an der Shelleingabeaufforderung mit `wget`, dass Sie auf die NGINX-Standardwebseite zugreifen können:

```console
wget -qO- http://backend
```

Da die Eingangsregel Datenverkehr mit Pods zulässt, die die Bezeichnungen *app: webapp,role: frontend* haben, wird der Datenverkehr vom Front-End-Pod zugelassen. Die folgende Beispielausgabe zeigt, dass die NGINX-Standardwebseite zurückgegeben wird:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

Schließen Sie die angefügte Terminalsitzung. Der Pod wird automatisch gelöscht.

```console
exit
```

### <a name="test-a-pod-without-a-matching-label"></a>Testen eines Pods ohne passende Bezeichnung

Die Netzwerkrichtlinie lässt Datenverkehr von Pods mit der Bezeichnung *app: webapp,role: frontend* zu, sollte aber den gesamten sonstigen Datenverkehr verweigern. Wir testen nun, ob ein anderer Pod ohne diese Bezeichnungen auf den Back-End-NGINX-Pod zugreifen kann. Erstellen Sie einen anderen Testpod, und fügen Sie an diesen eine Terminalsitzung an:

```console
kubectl run --rm -it --image=alpine network-policy --namespace development --generator=run-pod/v1
```

Vergewissern Sie sich an der Shelleingabeaufforderung mit `wget`, dass Sie auf die NGINX-Standardwebseite zugreifen können. Die Netzwerkrichtlinie blockiert sämtlichen eingehenden Datenverkehr, damit die Seite nicht geladen werden kann. Dies ist im folgenden Beispiel dargestellt:

```console
$ wget -qO- --timeout=2 http://backend

wget: download timed out
```

Schließen Sie die angefügte Terminalsitzung. Der Testpod wird automatisch gelöscht.

```console
exit
```

## <a name="allow-traffic-only-from-within-a-defined-namespace"></a>Zulassen von Datenverkehr nur, wenn er von einem definierten Namespace ausgeht

In den vorherigen Beispielen haben Sie eine Netzwerkrichtlinie erstellt, die sämtlichen Datenverkehr verweigert, und haben die Richtlinie dann aktualisiert, um Datenverkehr von Pods mit einer bestimmten Bezeichnung zuzulassen. Eine weitere häufige Anforderung ist das Begrenzen des Datenverkehrs auf einen bestimmten Namespace. Wenn die vorherigen Beispiele sich auf den Datenverkehr in einem *development*-Namespace beziehen würden, könnten Sie eine Netzwerkrichtlinie erstellen, die verhindert, dass Datenverkehr aus einem anderen Namespace, z. B. *production*, die Pods erreicht.

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

Da die Bezeichnungen für den Pod mit den derzeitigen Zulässigkeitsvorgaben der Netzwerkrichtlinie übereinstimmen, wird der Datenverkehr zugelassen. Die Netzwerkrichtlinie beachtet nicht die Namespaces, sondern nur die Podbezeichnungen. Die folgende Beispielausgabe zeigt, dass die NGINX-Standardwebseite zurückgegeben wird:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

Schließen Sie die angefügte Terminalsitzung. Der Testpod wird automatisch gelöscht.

```console
exit
```

### <a name="update-the-network-policy"></a>Aktualisieren der Netzwerkrichtlinie

Wir aktualisieren jetzt den Eingangsregelabschnitt *namespaceSelector*, um nur vom *development*-Namespace ausgehenden Datenverkehr zuzulassen. Bearbeiten Sie die Manifestdatei *backend-policy.yaml* wie im folgenden Beispiel gezeigt:

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

In komplexeren Beispielen können Sie mehrere Eingangsregeln definieren, z. B. die Verwendung eines *namespaceSelector*-Elements und dann eines *podSelector*-Elements.

Wenden Sie die aktualisierte Netzwerkrichtlinie über den Befehl [kubectl apply][kubectl-apply] an, und geben Sie den Namen Ihres YAML-Manifests an:

```azurecli-interactive
kubectl apply -f backend-policy.yaml
```

### <a name="test-the-updated-network-policy"></a>Testen der aktualisierten Netzwerkrichtlinie

Planen Sie einen weiteren Pod im *production*-Namespace, und fügen Sie eine Terminalsitzung an:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace production --generator=run-pod/v1
```

Vergewissern Sie sich an der Shelleingabeaufforderung mit `wget`, dass die Netzwerkrichtlinie Datenverkehr nun ablehnt:

```console
$ wget -qO- --timeout=2 http://backend.development

wget: download timed out
```

Schließen Sie den Test-Pod:

```console
exit
```

Da der Datenverkehr vom *production*-Namespace jetzt abgelehnt wird, können Sie einen Testpod im *development*-Namespace planen und eine Terminalsitzung anfügen:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace development --generator=run-pod/v1
```

Vergewissern Sie sich an der Shelleingabeaufforderung mit `wget`, dass die Netzwerkrichtlinie den Datenverkehr zulässt:

```console
wget -qO- http://backend
```

Der Datenverkehr wird zugelassen, da der Pod in dem Namespace geplant ist, der mit den derzeitigen Zulässigkeitsvorgaben der Netzwerkrichtlinie übereinstimmt. Die folgende Beispielausgabe zeigt, dass die NGINX-Standardwebseite zurückgegeben wird:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

Schließen Sie die angefügte Terminalsitzung. Der Testpod wird automatisch gelöscht.

```console
exit
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In diesem Artikel haben wir zwei Namespaces erstellt und eine Netzwerkrichtlinie angewendet. Um diese Ressourcen zu bereinigen, verwenden Sie den Befehl [kubectl delete][kubectl-delete] und geben die Ressourcennamen an:

```console
kubectl delete namespace production
kubectl delete namespace development
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Netzwerkressourcen in Kubernetes finden Sie unter [Netzwerkkonzepte für Anwendungen in Azure Kubernetes Service (AKS)][concepts-network].

Weitere Informationen zur Verwendung von Richtlinien finden Sie unter [Network Policies][kubernetes-network-policies] (Netzwerkrichtlinien).

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubernetes-network-policies]: https://kubernetes.io/docs/concepts/services-networking/network-policies/
[azure-cni]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[policy-rules]: https://kubernetes.io/docs/concepts/services-networking/network-policies/#behavior-of-to-and-from-selectors
[aks-github]: https://github.com/azure/aks/issues]

<!-- LINKS - internal -->
[install-azure-cli]: /cli/azure/install-azure-cli
[use-advanced-networking]: configure-advanced-networking.md
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[concepts-network]: concepts-network.md
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
