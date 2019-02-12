---
title: Installieren von Istio in Azure Kubernetes Service (AKS)
description: Erfahren Sie, wie Sie Istio installieren und zum Erstellen eines Service Mesh in einem AKS-Cluster (Azure Kubernetes Service) verwenden.
services: container-service
author: paulbouwer
ms.service: container-service
ms.topic: article
ms.date: 12/3/2018
ms.author: pabouwer
ms.openlocfilehash: f34d8c547738921374eaf5edcfcec4911423d9dc
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2019
ms.locfileid: "55699210"
---
# <a name="install-and-use-istio-in-azure-kubernetes-service-aks"></a>Installieren und Verwenden von Istio in Azure Kubernetes Service (AKS)

[Istio][istio-github] ist ein Open Source-Service Mesh, das eine Reihe wichtiger Funktionen für Microservices in einem Kubernetes-Cluster bietet. Zu diesen Funktionen gehören: Verwaltung des Datenverkehrs, Dienstidentität und -sicherheit, Richtlinienerzwingung und Telemetrie. Weitere Informationen zu Istio finden Sie in der offiziellen Dokumentation [What is Istio? (Was ist Istio)][istio-docs-concepts].

In diesem Artikel wird gezeigt, wie Sie Istio installieren. Die Istio-Clientbinärdatei `istioctl` wird auf Ihrem Clientcomputer installiert, anschließend werden die Istio-Komponenten in einem Kubernetes-Cluster in AKS installiert. Diese Anweisungen beziehen sich auf die Istio-Version *1.0.4*. Weitere Istio-Versionen finden Sie unter [GitHub – Istio-Releases][istio-github-releases].

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Herunterladen von Istio
> * Installieren der Istio-Clientbinärdatei
> * Installieren der Kubernetes-Komponenten von Istio
> * Überprüfen der Installation

## <a name="before-you-begin"></a>Voraussetzungen

Bei den in diesem Artikel beschriebenen Schritten wird vorausgesetzt, dass Sie einen AKS-Cluster (Kubernetes 1.10 und höher, RBAC aktiviert) erstellt und eine `kubectl`-Verbindung zum Cluster hergestellt haben. Wenn Sie Hilfe bei einem dieser Elemente benötigen, beachten Sie das Dokument [AKS-Schnellstart][aks-quickstart].

Um Istio installieren zu können, müssen Sie [Helm] [ helm] Version *2.10.0* oder höher ordnungsgemäß installiert und in Ihrem Cluster konfiguriert haben. Wenn Sie Hilfe bei der Installation von Helm benötigen, beachten Sie den [AKS Helm-Installationsleitfaden][helm-install]. Wenn Sie nicht Version *2.10.0* oder höher von Helm installiert haben, müssen Sie ein Upgrade durchführen. Beachten Sie alternativ den Artikel [Istio – Installation with Helm (Istio – Installation mit Helm)] [ istio-install-helm], in dem andere Installationsoptionen beschrieben werden.

Im vorliegenden Artikel wird die Istio-Installationsanleitung in mehrere Einzelschritte untergliedert. Jeder dieser Schritte wird beschrieben, sodass Sie erfahren, wie Sie Istio installieren und wie Istio mit Kubernetes funktioniert. Das Endergebnis hat dieselbe Struktur wie beim Befolgen der offiziellen [Anleitung][istio-install-k8s-quickstart] für die Installation von Istio.

## <a name="download-istio"></a>Herunterladen von Istio

Laden Sie zunächst das neueste Istio-Release herunter, und extrahieren Sie dieses. Die Schritte für eine Bash-Shell unter MacOS, Linux oder einem Windows-Subsystem für Linux und eine PowerShell-Befehlsshell unterscheiden sich leicht. Wählen Sie einen der folgenden Installationsschritte für Ihre bevorzugte Umgebung aus:

* [Bash unter MacOS, Linux oder Windows-Subsystem für Linux](#bash)
* [PowerShell](#powershell)

### <a name="bash"></a>Bash

Verwenden Sie unter MacOS `curl` zum Herunterladen das neueste Istio-Release, und extrahieren Sie dieses dann wie folgt mit `tar`:

```bash
# Specify the Istio version that will be leveraged throughout these instructions
ISTIO_VERSION=1.0.4

# MacOS
curl -sL "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-osx.tar.gz" | tar xz
```

Verwenden Sie unter Linux oder Windows-Subsystem für Linux `curl`, um das neueste Istio-Release herunterzuladen, und extrahieren Sie dieses dann wie folgt mit `tar`:

```bash
# Specify the Istio version that will be leveraged throughout these instructions
ISTIO_VERSION=1.0.4

curl -sL "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-linux.tar.gz" | tar xz
```

### <a name="powershell"></a>PowerShell

Verwenden Sie in PowerShell [Invoke-WebRequest][Invoke-WebRequest], um das neueste Istio-Release herunterzuladen, und extrahieren Sie dieses dann wie folgt mit [Expand-Archive][Expand-Archive]:

```powershell
# Specify the Istio version that will be leveraged throughout these instructions
$ISTIO_VERSION="1.0.4"

# Windows
$ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -URI "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-win.zip" -OutFile "istio-$ISTIO_VERSION.zip"
Expand-Archive -Path "istio-$ISTIO_VERSION.zip" -DestinationPath .
```

## <a name="install-the-istio-client-binary"></a>Installieren der Istio-Clientbinärdatei

Die `istioctl`-Clientbinärdatei wird auf dem Clientcomputer ausgeführt und ermöglicht es Ihnen, die Istio-Routingregeln und -Richtlinien zu verwalten. Auch hier unterscheiden sich die Installationsschritte bei den verschiedenen Client-Betriebssystemen leicht voneinander. Wählen Sie einen der folgenden Installationsschritte für Ihre bevorzugte Umgebung aus.

> [!IMPORTANT]
> Führen Sie alle verbleibenden Schritte im Ordner der obersten Ebene des Istio-Release aus, das Sie im vorherigen Abschnitt heruntergeladen und extrahiert haben.

### <a name="macos"></a>macOS

Verwenden Sie die folgenden Befehle, um die Istio-Clientbinärdatei `istioctl` in einer Bash-basierten Shell unter MacOS zu installieren. Diese Befehle kopieren die `istioctl`-Clientbinärdatei an den Speicherort des Standardbenutzerprogramms in Ihrem `PATH`.

```bash
cd istio-$ISTIO_VERSION
chmod +x ./bin/istioctl
sudo mv ./bin/istioctl /usr/local/bin/istioctl
```

Wenn Sie die Befehlszeilen-Vervollständigung für die Istio-Clientbinärdatei `istioctl` wünschen, richten Sie diese wie folgt ein:

```bash
# Generate the bash completion file and source it in your current shell
mkdir -p ~/completions && istioctl collateral --bash -o ~/completions
source ~/completions/istioctl.bash

# Source the bash completion file in your .bashrc so that the command-line completions
# are permanently available in your shell
echo "source ~/completions/istioctl.bash" >> ~/.bashrc
```

Wechseln Sie jetzt zum Abschnitt [Installieren der Kubernetes-Komponenten von Istio](#install-the-istio-kubernetes-components).

### <a name="linux-or-windows-subsystem-for-linux"></a>Linux oder Windows-Subsystem für Linux

Verwenden Sie die folgenden Befehle, um die Istio-Clientbinärdatei `istioctl` in einer Bash-basierten Shell unter Linux oder [Windows-Subsystem für Linux][install-wsl] zu installieren. Diese Befehle kopieren die `istioctl`-Clientbinärdatei an den Speicherort des Standardbenutzerprogramms in Ihrem `PATH`.

```bash
cd istio-$ISTIO_VERSION
chmod +x ./bin/istioctl
sudo mv ./bin/istioctl /usr/local/bin/istioctl
```

Wenn Sie die Befehlszeilen-Vervollständigung für die Istio-Clientbinärdatei `istioctl` wünschen, richten Sie diese wie folgt ein:

```bash
# Generate the bash completion file and source it in your current shell
mkdir -p ~/completions && istioctl collateral --bash -o ~/completions
source ~/completions/istioctl.bash

# Source the bash completion file in your .bashrc so that the command-line completions
# are permanently available in your shell
echo "source ~/completions/istioctl.bash" >> ~/.bashrc
```

Wechseln Sie jetzt zum Abschnitt [Installieren der Kubernetes-Komponenten von Istio](#install-the-istio-kubernetes-components).

### <a name="windows"></a>Windows

Verwenden Sie die folgenden Befehle, um die Istio-Clientbinärdatei `istioctl` in einer Powershell-basierten Shell unter Windows zu installieren. Diese Befehle kopieren die `istioctl`-Clientbinärdatei an einen neuen Benutzerprogrammspeicherort und stellen sie über Ihren `PATH` zur Verfügung.

```powershell
cd istio-$ISTIO_VERSION
New-Item -ItemType Directory -Force -Path "C:/Program Files/Istio"
mv ./bin/istioctl.exe "C:/Program Files/Istio/"
$PATH = [environment]::GetEnvironmentVariable("PATH", "User")
[environment]::SetEnvironmentVariable("PATH", $PATH + "; C:\Program Files\Istio\", "User")
```

## <a name="install-the-istio-kubernetes-components"></a>Installieren der Kubernetes-Komponenten von Istio

Verwenden Sie Helm, um die Istio-Komponenten in Ihrem AKS-Cluster zu installieren. Installieren Sie die Istio-Ressourcen im `istio-system`-Namespace, und aktivieren Sie wie folgt zusätzliche Optionen für die Sicherheit und die Überwachung:

```azurecli
helm install install/kubernetes/helm/istio --name istio --namespace istio-system \
  --set global.controlPlaneSecurityEnabled=true \
  --set grafana.enabled=true \
  --set tracing.enabled=true \
  --set kiali.enabled=true
```

Das Helm-Diagramm stellt eine große Zahl von Objekten bereit. Je nach Clusterumgebung kann die Bereitstellung 2 bis 3 Minuten dauern.

## <a name="validate-the-installation"></a>Überprüfen der Installation

Um sicherzustellen, dass Sie Istio erfolgreich bereitgestellt haben, muss die Installation überprüft werden.

Vergewissern Sie sich zunächst, dass die erwarteten Dienste erstellt wurden. Verwenden Sie den Befehl [Kubectl get svc][kubectl-get], um die ausgeführten Dienste anzuzeigen. Fragen Sie den Namespace *istio-system* ab, in dem die Istio- und Add-On-Komponenten vom Helm-Diagramm installiert wurden:

```console
kubectl get svc --namespace istio-system --output wide
```

Die folgende Beispielausgabe zeigt die Dienste, die jetzt ausgeführt werden sollten:

- *istio-**-Dienste
- *jaeger-**, *Ablaufverfolgung* und *zipkin*-Add-On-Ablaufverfolgungsdienste
- *prometheus*-Add-On-Metrikdienst
- *grafana*-Add-On-Analysen- und -Überwachungsdashboarddienst
- *kiali*-Add-On-Service Mesh-Dashboarddienst

Wenn das `istio-ingressgateway` für die externe IP-Adresse `<pending>` anzeigt, warten Sie einige Minuten, bis vom Azure-Netzwerk eine IP-Adresse zugewiesen wurde.

```console
NAME                     TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)                                                                                                                   AGE       SELECTOR
grafana                  ClusterIP      10.0.26.60     <none>           3000/TCP                                                                                                                  3m        app=grafana
istio-citadel            ClusterIP      10.0.88.87     <none>           8060/TCP,9093/TCP                                                                                                         3m        istio=citadel
istio-egressgateway      ClusterIP      10.0.115.115   <none>           80/TCP,443/TCP                                                                                                            3m        app=istio-egressgateway,istio=egressgateway
istio-galley             ClusterIP      10.0.104.183   <none>           443/TCP,9093/TCP                                                                                                          3m        istio=galley
istio-ingressgateway     LoadBalancer   10.0.12.216    52.187.250.239   80:31380/TCP,443:31390/TCP,31400:31400/TCP,15011:30469/TCP,8060:31999/TCP,853:31235/TCP,15030:32000/TCP,15031:30293/TCP   3m        app=istio-ingressgateway,istio=ingressgateway
istio-pilot              ClusterIP      10.0.38.134    <none>           15010/TCP,15011/TCP,8080/TCP,9093/TCP                                                                                     3m        istio=pilot
istio-policy             ClusterIP      10.0.253.81    <none>           9091/TCP,15004/TCP,9093/TCP                                                                                               3m        istio-mixer-type=policy,istio=mixer
istio-sidecar-injector   ClusterIP      10.0.181.186   <none>           443/TCP                                                                                                                   3m        istio=sidecar-injector
istio-telemetry          ClusterIP      10.0.177.113   <none>           9091/TCP,15004/TCP,9093/TCP,42422/TCP                                                                                     3m        istio-mixer-type=telemetry,istio=mixer
jaeger-agent             ClusterIP      None           <none>           5775/UDP,6831/UDP,6832/UDP                                                                                                3m        app=jaeger
jaeger-collector         ClusterIP      10.0.112.81    <none>           14267/TCP,14268/TCP                                                                                                       3m        app=jaeger
jaeger-query             ClusterIP      10.0.179.193   <none>           16686/TCP                                                                                                                 3m        app=jaeger
kiali                    ClusterIP      10.0.211.63    <none>           20001/TCP                                                                                                                 3m        app=kiali
prometheus               ClusterIP      10.0.70.113    <none>           9090/TCP                                                                                                                  3m        app=prometheus
tracing                  ClusterIP      10.0.139.121   <none>           80/TCP                                                                                                                    3m        app=jaeger
zipkin                   ClusterIP      10.0.60.155    <none>           9411/TCP                                                                                                                  3m        app=jaeger
```

Vergewissern Sie sich als Nächstes, dass die erforderlichen Pods erstellt wurden. Verwenden Sie den Befehl [kubectl get pods][kubectl-get], und fragen Sie erneut den Namespace *istio-system* ab:

```console
kubectl get pods --namespace istio-system
```

Die folgende Beispielausgabe zeigt die Pods, die ausgeführt werden:

- die *istio-**-Pods
- der *prometheus-**-Add-On-Metrik-Pod
- der *grafana-**-Add-On-Analytik- und Überwachungsdashboard-Pod
- der *kiali*-Add-On-Service Mesh-Dashboard-Pod

```console
NAME                                     READY     STATUS      RESTARTS   AGE
grafana-59b787b9b-cr6d7                  1/1       Running     0          6m
istio-citadel-78df8c67d9-9lfpf           1/1       Running     0          6m
istio-egressgateway-6b96cd7f5-k848h      1/1       Running     0          6m
istio-galley-58f566cb66-8mhbv            1/1       Running     0          6m
istio-ingressgateway-6cbbf596f6-6jz8g    1/1       Running     0          6m
istio-pilot-8449d555fc-sl6kp             2/2       Running     0          6m
istio-policy-6b99d88bc5-55s52            2/2       Running     0          6m
istio-sidecar-injector-b88dfb954-8m86s   1/1       Running     0          6m
istio-telemetry-675cb4cb9d-8s7wd         2/2       Running     0          6m
istio-tracing-7596597bd7-sbnt9           1/1       Running     0          6m
kiali-5fbd6ffb-4qcxw                     1/1       Running     0          6m
prometheus-76db5fddd5-2tkxs              1/1       Running     0          6m
```

Alle Pods haben den Status `Running`. Wenn Ihre Pods nicht diesen Status haben, warten Sie ein oder zwei Minuten, bis dies der Fall ist. Wenn einer der Pods ein Problem meldet, verwenden Sie den Befehl [kubectl describe pod] [ kubectl-describe], um die Ausgabe und den Status des betreffenden Pods zu überprüfen.

## <a name="accessing-the-add-ons"></a>Zugreifen auf die Add-Ons

Einige der bei der obigen Einrichtung installierten Add-ons für Istio bieten zusätzliche Funktionen. Die Benutzeroberflächen für die Add-Ons werden nicht über eine externe IP-Adresse öffentlich verfügbar gemacht. Um auf diese Add-On-Benutzeroberflächen zuzugreifen, verwenden Sie den Befehl [kubectl port-forward][kubectl-port-forward]. Dieser Befehl erstellt eine sichere Verbindung zwischen einem lokalen Port auf Ihrem Clientcomputer und dem relevanten Pod in Ihrem AKS-Cluster.

### <a name="grafana"></a>Grafana

Die Analyse- und Überwachungsdashboards für Istio werden von [Grafana][grafana] bereitgestellt. Leiten Sie den lokalen Port *3000* auf Ihrem Clientcomputer an Port *3000* auf dem Pod weiter, auf dem Grafana in Ihrem AKS-Cluster ausgeführt wird:

```console
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000
```

Die folgende Beispielausgabe zeigt die für Grafana konfigurierte Portweiterleitung:

```console
Forwarding from 127.0.0.1:3000 -> 3000
Forwarding from [::1]:3000 -> 3000
```

Sie können jetzt unter folgender URL auf Ihrem Clientcomputer auf Grafana zugreifen – [ http://localhost:3000 ](http://localhost:3000).

### <a name="prometheus"></a>Prometheus

Der Ausdrucksbrowser für die Metrik wird von [Prometheus][prometheus] bereitgestellt. Leiten Sie den lokalen Port *9090* auf Ihrem Clientcomputer an Port *9090* auf dem Pod weiter, auf dem Prometheus in Ihrem AKS-Cluster ausgeführt wird:

```console
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=prometheus -o jsonpath='{.items[0].metadata.name}') 9090:9090
```

Die folgende Beispielausgabe zeigt die für Prometheus konfigurierte Portweiterleitung:

```console
Forwarding from 127.0.0.1:9090 -> 9090
Forwarding from [::1]:9090 -> 9090
```

Sie erreichen jetzt den Prometheus-Ausdrucksbrowser unter folgender URL auf Ihrem Clientcomputer – [ http://localhost:9090 ](http://localhost:9090).

### <a name="jaeger"></a>Jaeger

Die Benutzeroberfläche für die Ablaufverfolgung wird von [Jaeger][jaeger] bereitgestellt. Leiten Sie den lokalen Port *16686* auf Ihrem Clientcomputer an Port *16686* auf dem Pod weiter, auf dem Jaeger in Ihrem AKS-Cluster ausgeführt wird:

```console
kubectl port-forward -n istio-system $(kubectl get pod -n istio-system -l app=jaeger -o jsonpath='{.items[0].metadata.name}') 16686:16686
```

Die folgende Beispielausgabe zeigt die für Jaeger konfigurierte Portweiterleitung:

```console
Forwarding from 127.0.0.1:16686 -> 16686
Forwarding from [::1]:16686 -> 16686
```

Sie erreichen jetzt die Benutzeroberfläche von Jaeger für die Ablaufverfolgung unter folgender URL auf Ihrem Clientcomputer – [ http://localhost:16686 ](http://localhost:16686).

### <a name="kiali"></a>Kiali

Ein Service Mesh-Telemetriedashboard wird von [Kiali][kiali] bereitgestellt. Leiten Sie den lokalen Port *20001* auf Ihrem Clientcomputer an Port *20001* auf dem Pod weiter, auf dem Kiali in Ihrem AKS-Cluster ausgeführt wird:

```console
kubectl port-forward -n istio-system $(kubectl get pod -n istio-system -l app=kiali -o jsonpath='{.items[0].metadata.name}') 20001:20001
```

Die folgende Beispielausgabe zeigt die für Kiali konfigurierte Portweiterleitung:

```console
Forwarding from 127.0.0.1:20001 -> 20001
Forwarding from [::1]:20001 -> 20001
```

Sie erreichen jetzt das Service Mesh-Telemetriedashboard von Kiali unter der folgenden URL auf Ihrem Clientcomputer – [ http://localhost:20001 ](http://localhost:20001).

Der Standardbenutzername und das Standardkennwort für das Kiali-Dashboard lauten *Benutzername:admin/Kennwort:admin*. Diese Anmeldeinformationen können über die Helm-Werte *kiali.dashboard.username* und *kiali.dashboard.passphrase* konfiguriert werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen, wie Sie Istio verwenden können, um intelligentes Routing zwischen mehreren Versionen einer Anwendung bereitzustellen und das Rollout eines Canary-Releases durchzuführen, finden Sie in der folgenden Dokumentation:

> [!div class="nextstepaction"]
> [AKS Istio intelligent routing scenario (Szenario für intelligentes Routing mit AKS Istio)][istio-scenario-routing]

Um mehr über Installations- und Konfigurationsoptionen für Istio zu erfahren, beachten Sie die folgenden offiziellen Istio-Artikel:

- [Istio – Kubernetes installation quickstart (Istio – Schnellstartanleitung für die Kubernetes-Installation)][istio-install-k8s-quickstart]
- [Istio – Helm installation guide (Istio – Installationshandbuch für Helm)][istio-install-helm]
- [Istio – Helm installation options (Istio – Installationsoptionen für Helm)][istio-install-helm-options]

Mithilfe des [Istio Bookinfo-Anwendungsbeispiels][istio-bookinfo-example] können Sie sich weitere Szenarios ansehen.

<!-- LINKS - external -->
[istio]: https://istio.io
[helm]: https://helm.sh
[istio-docs-concepts]: https://istio.io/docs/concepts/what-is-istio/
[istio-github]: https://github.com/istio/istio
[istio-github-releases]: https://github.com/istio/istio/releases
[istio-install-download]: https://istio.io/docs/setup/kubernetes/download-release/
[istio-install-k8s-quickstart]: https://istio.io/docs/setup/kubernetes/quick-start/
[istio-install-helm]: https://istio.io/docs/setup/kubernetes/helm-install/
[istio-install-helm-options]: https://istio.io/docs/reference/config/installation-options/
[istio-bookinfo-example]: https://istio.io/docs/examples/bookinfo/
[install-wsl]: https://docs.microsoft.com/windows/wsl/install-win10
[kubernetes-crd]: https://kubernetes.io/docs/tasks/access-kubernetes-api/custom-resources/custom-resource-definitions/
[grafana]: https://grafana.com/
[prometheus]: https://prometheus.io/
[jaeger]: https://www.jaegertracing.io/
[kiali]: https://www.kiali.io/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-port-forward]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[istio-scenario-routing]: ./istio-scenario-routing.md
[helm-install]: ./kubernetes-helm.md
[Invoke-WebRequest]: /powershell/module/microsoft.powershell.utility/invoke-webrequest
[Expand-Archive]: /powershell/module/Microsoft.PowerShell.Archive/Expand-Archive
