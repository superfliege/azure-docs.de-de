---
title: Konfigurieren des eingehenden Datenverkehrs mit einem Azure Kubernetes Service-Cluster (AKS)
description: Installieren und Konfigurieren eines NGINX-Eingangscontrollers in einem Azure Kubernetes Service-Cluster (AKS)
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 04/28/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 9246fccb1713f69d2c6c655b09f0daf51055596f
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="https-ingress-on-azure-kubernetes-service-aks"></a>Eingehender HTTPS-Datenverkehr in Azure Kubernetes Service (AKS)

Ein Eingangscontroller ist eine Softwarekomponente, die einen Reverseproxy, konfigurierbare Datenverkehrsweiterleitung und TLS-Terminierung für Kubernetes-Dienste bereitstellt. Mithilfe von Ressourcen für eingehende Kubernetes-Daten werden Eingangsregeln und Routen für einzelne Kubernetes-Dienste konfiguriert. Durch die Verwendung von einem Eingangscontroller und Eingangsregeln kann eine einzelne externe Adresse zum Weiterleiten von Datenverkehr an mehrere Dienste in einem Kubernetes-Cluster verwendet werden.

Dieses Dokument begleitet Sie durch eine Beispielbereitstellung des [NGINX-Eingangscontrollers][nginx-ingress] in einem Azure Kubernetes Service-Cluster (AKS). Darüber hinaus wird das Projekt [KUBE-LEGO][kube-lego] verwendet, um automatisch [Let‘s Encrypt][lets-encrypt]-Zertifikate zu generieren und zu konfigurieren. Schließlich werden verschiedene Anwendungen im AKS-Cluster ausgeführt, die jeweils über eine einzelne Adresse zugänglich sind.

## <a name="prerequisite"></a>Voraussetzung

Installieren Sie die Helm CLI. Installationsanweisungen finden Sie in der [Dokumentation][helm-cli] zur Helm CLI.

## <a name="install-an-ingress-controller"></a>Installieren eines Eingangscontrollers

Verwenden Sie Helm, um den NGINX-Eingangscontroller zu installieren. Ausführliche Bereitstellungsinformationen finden Sie in der [Dokumentation][nginx-ingress] des NGINX-Eingangscontrollers.

Aktualisieren Sie das Diagrammrepository.

```console
helm repo update
```

Installieren Sie den NGINX-Eingangscontroller. In diesem Beispiel wird der Controller im Namespace `kube-system` installiert. Dieser kann in einen Namespace Ihrer Wahl geändert werden.

```
helm install stable/nginx-ingress --namespace kube-system
```

Während der Installation wird eine öffentliche Azure-IP-Adresse für den Eingangscontroller erstellt. Sie rufen die öffentliche IP-Adresse mit dem Befehl kubectl get service ab. Es dauert möglicherweise einige Zeit, bis die IP-Adresse dem Dienst zugewiesen wird.

```console
$ kubectl get service -l app=nginx-ingress --namespace kube-system

NAME                                       TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
eager-crab-nginx-ingress-controller        LoadBalancer   10.0.182.160   51.145.155.210  80:30920/TCP,443:30426/TCP   20m
eager-crab-nginx-ingress-default-backend   ClusterIP      10.0.255.77    <none>          80/TCP                       20m
```

Da noch keine Eingangsregeln erstellt wurden, werden Sie zur Standard-404-Seite des NGINX-Eingangscontrollers weitergeleitet, wenn Sie zur öffentlichen IP-Adresse browsen.

![NGINX-Standard-Back-End](media/ingress/default-back-end.png)

## <a name="configure-dns-name"></a>Konfigurieren des DNS-Namens

Da HTTPS-Zertifikate verwendet werden, müssen Sie einen vollqualifizierten Domänennamen für die IP-Adresse des Eingangscontrollers konfigurieren. In diesem Beispiel wird ein vollqualifizierter Azure-Domänenname mit der Azure-Befehlszeilenschnittstelle erstellt. Aktualisieren Sie das Skript mit der IP-Adresse des Eingangscontrollers und des Namens, den Sie im vollqualifizierten Domänennamen verwenden möchten.

```
#!/bin/bash

# Public IP address
IP="51.145.155.210"

# Name to associate with public IP address
DNSNAME="demo-aks-ingress"

# Get resource group and public ip name
RESOURCEGROUP=$(az network public-ip list --query "[?ipAddress!=null]|[?contains(ipAddress, '$IP')].[resourceGroup]" --output tsv)
PIPNAME=$(az network public-ip list --query "[?ipAddress!=null]|[?contains(ipAddress, '$IP')].[name]" --output tsv)

# Update public ip address with dns name
az network public-ip update --resource-group $RESOURCEGROUP --name  $PIPNAME --dns-name $DNSNAME
```

Der Eingangscontroller sollte jetzt über den vollqualifizierten Domänennamen zugänglich sein.

## <a name="install-kube-lego"></a>Installieren von KUBE-LEGO

Der NGINX-Eingangscontroller unterstützt TLS-Terminierung. Es gibt mehrere Methoden zum Abrufen und Konfigurieren von Zertifikaten für HTTPS. In diesem Dokument wird die Verwendung von [KUBE-LEGO][kube-lego] veranschaulicht, das automatisches Generieren von [Let‘s Encrypt][lets-encrypt]-Zertifikaten und -Verwaltungsfunktionen bietet.

Verwenden Sie zum Installieren des KUBE-LEGO-Controllers den folgenden Helm-Installationsbefehl. Ersetzen Sie die E-Mail-Adresse durch eine aus Ihrer Organisation.

```
helm install stable/kube-lego \
  --set config.LEGO_EMAIL=user@contoso.com \
  --set config.LEGO_URL=https://acme-v01.api.letsencrypt.org/directory
```

Weitere Informationen zur Konfiguration von KUBE-LEGO finden Sie in der [KUBE-LEGO-Dokumentation][kube-lego].

## <a name="run-application"></a>Ausführen der Anwendung

Zum jetzigen Zeitpunkt wurden ein Eingangscontroller und eine Zertifikatsverwaltungslösung konfiguriert. Führen Sie nun einige Anwendungen in Ihrem AKS-Cluster aus.

In diesem Beispiel wird Helm verwendet, um mehrere Instanzen einer einfachen Hallo-Welt-Anwendung auszuführen.

Fügen Sie vor dem Ausführen der Anwendung das Azure-Beispielrepository für Helm auf Ihrem Entwicklungssystem hinzu.

```
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

 Führen Sie das Hallo-Welt-AKS-Diagramm mit dem folgenden Befehl aus:

```
helm install azure-samples/aks-helloworld
```

Installieren Sie nun eine zweite Instanz der Hallo-Welt-Anwendung.

Geben Sie für die zweite Instanz einen neuen Titel an, sodass die beiden Anwendungen sich visuell unterscheiden. Sie müssen auch einen eindeutigen Dienstnamen angeben. Diese Konfigurationen sind im folgenden Befehl zu erkennen.

```console
helm install azure-samples/aks-helloworld --set title="AKS Ingress Demo" --set serviceName="ingress-demo"
```

## <a name="create-ingress-route"></a>Erstellen der Eingangsroute

Beide Anwendungen werden nun in Ihrem Kubernetes-Cluster ausgeführt, wurden jedoch mit einem Dienst vom Typ `ClusterIP` konfiguriert. Daher kann nicht über das Internet auf die Anwendungen zugegriffen werden. Um sie verfügbar zu machen, erstellen Sie eine Kubernetes-Eingangsressource. Die Eingangsressource konfiguriert Regeln, die den Datenverkehr an eine der beiden Anwendungen weiterleiten.

Erstellen Sie eine Datei namens `hello-world-ingress.yaml`, und fügen Sie den folgenden YAML-Code ein.

Beachten Sie, dass der Datenverkehr an die Adresse `https://demo-aks-ingress.eastus.cloudapp.azure.com/` an den Dienst mit dem Namen `aks-helloworld` weitergeleitet wird. Datenverkehr an die Adresse `https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two` wird an den Dienst `ingress-demo` weitergeleitet.

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/tls-acme: "true"
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  tls:
  - hosts:
    - demo-aks-ingress.eastus.cloudapp.azure.com
    secretName: tls-secret
  rules:
  - host: demo-aks-ingress.eastus.cloudapp.azure.com
    http:
      paths:
      - path: /
        backend:
          serviceName: aks-helloworld
          servicePort: 80
      - path: /hello-world-two
        backend:
          serviceName: ingress-demo
          servicePort: 80
```

Erstellen Sie die Eingangsressource mit dem Befehl `kubectl apply`.

```console
kubectl apply -f hello-world-ingress.yaml
```

## <a name="test-the-ingress-configuration"></a>Testen der Konfiguration für eingehende Daten

Navigieren Sie zum vollqualifizierten Domänennamen des Kubernetes-Eingangscontrollers. Die Hallo-Welt-Anwendung sollte angezeigt werden.

![Anwendungsbeispiel 1](media/ingress/app-one.png)

Navigieren Sie nun zum vollqualifizierten Domänennamen des Eingangscontrollers mit dem Pfad `/hello-world-two`. Die Hallo-Welt-Anwendung mit dem benutzerdefinierten Titel sollte angezeigt werden.

![Anwendungsbeispiel 2](media/ingress/app-two.png)

Beachten Sie außerdem, dass die Verbindung verschlüsselt ist und ein von Let‘s Encrypt ausgestelltes Zertifikat verwendet wird.

![Let‘s Encrypt-Zertifikat](media/ingress/certificate.png)

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die in diesem Dokument demonstrierte Software.

- [Helm CLI][helm-cli]
- [NGINX-Eingangscontroller][nginx-ingress]
- [KUBE-LEGO][kube-lego]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm#install-helm-cli
[kube-lego]: https://github.com/jetstack/kube-lego
[lets-encrypt]: https://letsencrypt.org/
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
