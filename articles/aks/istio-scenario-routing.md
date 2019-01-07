---
title: Intelligentes Routing und Canary-Releases mit Istio in Azure Kubernetes Service (AKS)
description: Erfahren Sie, wie Sie mit Istio intelligentes Routing und Canary-Releases in einem Cluster von Azure Kubernetes Service (AKS) bereitstellen
services: container-service
author: paulbouwer
ms.service: container-service
ms.topic: article
ms.date: 12/3/2018
ms.author: pabouwer
ms.openlocfilehash: 0a4e5e7e310a9949ee59291c2032eafda46955a9
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2018
ms.locfileid: "52893091"
---
# <a name="use-intelligent-routing-and-canary-releases-with-istio-in-azure-kubernetes-service-aks"></a>Verwenden von intelligentem Routing und Canary-Releases mit Istio in Azure Kubernetes Service (AKS)

[Istio][istio-github] ist ein Open Source-Service Mesh, das eine Reihe wichtiger Funktionen für Microservices in einem Kubernetes-Cluster bietet. Zu diesen Funktionen gehören: Verwaltung des Datenverkehrs, Dienstidentität und -sicherheit, Richtlinienerzwingung und Telemetrie. Weitere Informationen zu Istio finden Sie in der offiziellen Dokumentation [What is Istio? (Was ist Istio)][istio-docs-concepts].

In diesem Artikel wird die Verwendung der Funktion zur Verwaltung des Datenverkehrs von Istio beschrieben. Um intelligentes Routing und Canary-Releases zu untersuchen, wird eine als Beispiel dienende AKS-Abstimmungs-App verwendet.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Bereitstellen der Anwendung
> * Aktualisieren der Anwendung
> * Rollout eines Canary-Releases der Anwendung
> * Abschließen des Rollouts

## <a name="before-you-begin"></a>Voraussetzungen

Bei den in diesem Artikel beschriebenen Schritten wird vorausgesetzt, dass Sie einen AKS-Cluster (Kubernetes 1.10 und höher, RBAC aktiviert) erstellt und eine `kubectl`-Verbindung zum Cluster hergestellt haben. Ferner muss Istio in Ihrem Cluster installiert sein.

Wenn Sie Hilfe bei einem dieser Elemente benötigen, beachten Sie die Dokumente [AKS-Schnellstart][aks-quickstart] und [Install Istio in AKS (Istio in AKS installieren)][istio-install].

## <a name="about-this-application-scenario"></a>Informationen zu diesem Anwendungsszenario

In der als Beispiel dienenden AKS-Abstimmungs-App können Benutzer zwischen zwei Abstimmungsoptionen (Katzen oder Hunde) wählen. Es gibt eine Speicherkomponente, die die Anzahl der abgegebenen Stimmen für die einzelne Optionen persistent speichert. Darüber hinaus gibt es eine Analysekomponente, die Details zu den für jede Option abgegebenen Stimmen bereitstellt.

In diesem Artikel stellen Sie zunächst Version *1.0* der Abstimmungs-App und Version *1.0* der Analysekomponente bereit. Die Analysekomponente bietet einfache Zähler für die Anzahl der Stimmen. Die Abstimmungs-App und die Analysekomponente interagieren mit der Version *1.0* der durch Redis unterstützten Speicherkomponente.

Sie führen ein Upgrade der Analysekomponente auf Version *1.1* durch, die Zähler und jetzt auch Summen und Prozentwerte bietet.

Eine Teilmenge der Benutzer testet Version *2.0* der App über ein Canary-Release. Diese neue Version verwendet eine Speicherkomponente, die durch eine MySQL-Datenbank unterstützt wird.

Sobald Sie sicher sind, dass Version *2.0* bei Ihrer Benutzerteilmenge wie erwartet funktioniert, führen Sie für alle Ihre Benutzer ein Rollout von Version *2.0* durch.

## <a name="deploy-the-application"></a>Bereitstellen der Anwendung

Wir beginnen mit der Bereitstellung der Anwendung in Ihrem AKS-Cluster (Azure Kubernetes Service). Das folgende Diagramm zeigt, was am Ende dieses Abschnitts ausgeführt wird: Version *1.0* aller Komponenten, wobei eingehende Anforderungen über das Istio-Eingangsgateway verarbeitet werden:

![Die Komponenten der AKS-Abstimmungs-App und das Routing.](media/istio/components-and-routing-01.png)

Die Artefakte, die Sie für diesen Artikel benötigen, finden Sie im GitHub-Repository [Azure-Samples/Aks-voting-app] [ github-azure-sample]. Sie können entweder die Artefakte herunterladen oder das Repository wie folgt klonen:

```console
git clone https://github.com/Azure-Samples/aks-voting-app.git
```

Wechseln Sie im heruntergeladenen bzw. geklonten Repository in den folgenden Ordner, und führen Sie alle nachfolgenden Schritte aus diesem Ordner aus:

```console
cd scenarios/intelligent-routing-with-istio
```

Erstellen Sie zunächst wie folgt für die als Beispiel dienende AKS-Abstimmungs-App einen Namespace mit dem Namen *voting* in Ihrem AKS-Cluster:

```console
kubectl create namespace voting
```

Bezeichnen Sie den Namespace mit `istio-injection=enabled`. Diese Bezeichnung weist Istio an, automatisch die Istio-Proxys als Sidecars in alle Ihre Pods in diesem Namespace einzufügen.

```console
kubectl label namespace voting istio-injection=enabled
```

Nun erstellen wir die Komponenten für die AKS-Abstimmungs-App. Erstellen Sie diese Komponenten in dem *voting*-Namespace, den Sie oben erstellt haben.

```console
kubectl apply -f kubernetes/step-1-create-voting-app.yaml --namespace voting
```

Die folgende Beispielausgabe zeigt, dass die Ressourcen erfolgreich erstellt wurden:

```
deployment.apps/voting-storage-1-0 created
service/voting-storage created
deployment.apps/voting-analytics-1-0 created
service/voting-analytics created
deployment.apps/voting-app-1-0 created
service/voting-app created
```

> [!NOTE]
> Istio verfügt über bestimmte Anforderungen für Pods und Dienste. Weitere Informationen hierzu finden Sie in der Dokumentation [Istio Requirements for Pods and Services (Istio-Anforderungen für Pods und Dienste)][istio-requirements-pods-and-services].

Um die erstellten Pods anzuzeigen, verwenden Sie wie folgt den Befehl [kubectl get pods][kubectl-get]:

```console
kubectl get pods -n voting
```

Die folgende Beispielausgabe zeigt, dass es drei Instanzen des Pods *voting-app* und je eine Instanz der Pods *voting-analytics* und *voting-storage* gibt. Jede der Pods verfügt über zwei Container. Einer dieser Container ist die Komponente, der andere der *Istio-Proxy*:

```
NAME                                    READY     STATUS    RESTARTS   AGE
voting-analytics-1-0-669f99dcc8-lzh7k   2/2       Running   0          1m
voting-app-1-0-6c65c4bdd4-bdmld         2/2       Running   0          1m
voting-app-1-0-6c65c4bdd4-gcrng         2/2       Running   0          1m
voting-app-1-0-6c65c4bdd4-strzc         2/2       Running   0          1m
voting-storage-1-0-7954799d96-5fv9r     2/2       Running   0          1m
```

Um Informationen zu dem Pod anzuzeigen, verwenden Sie [kubectl describe pod][kubectl-describe]. Ersetzen Sie den Namen des Pods durch den Namen eines Pods in Ihrem eigenen AKS-Cluster aus der vorherigen Ausgabe:

```console
kubectl describe pod voting-app-1-0-6c65c4bdd4-bdmld --namespace voting
```

Der Container *istio-proxy* wurde automatisch von Istio eingefügt, um den Netzwerkdatenverkehr zu und von Ihren Komponenten zu verwalten. Dies zeigt die folgende Beispielausgabe:

```
[...]
Containers:
  voting-app:
    Image:         mcr.microsoft.com/aks/samples/voting/app:1.0
    ...
  istio-proxy:
    Image:         docker.io/istio/proxyv2:1.0.4
[...]
```

Sie können erst dann eine Verbindung zu der Abstimmungs-App herstellen, nachdem Sie das [Gateway][istio-reference-gateway] und den [virtuellen Dienst][istio-reference-virtualservice] für Istio erstellt haben. Diese Istio-Ressourcen leiten Datenverkehr vom Standard-Istio-Eingangsgateway an Ihre Anwendung weiter.

> [!NOTE]
> Ein *Gateway* ist eine Komponente am Rand des Service Mesh, die eingehenden oder ausgehenden HTTP- und TCP-Datenverkehr empfängt.
>
> Ein *virtueller Dienst* definiert Routingregeln für einen oder mehrere Zieldienste.

Verwenden Sie die `istioctl`-Clientbinärdatei, um das Gateway und den virtuellen Dienst yaml bereitzustellen. Denken Sie wie beim Befehl `kubectl apply` daran, den Namespace anzugeben, in dem diese Ressourcen bereitgestellt werden.

```console
istioctl create -f istio/step-1-create-voting-app-gateway.yaml --namespace voting
```

Rufen Sie mit dem folgenden Befehl die IP-Adresse des Istio-Eingangsgateways ab:

```console
kubectl get service istio-ingressgateway --namespace istio-system -o jsonpath='{.status.loadBalancer.ingress[0].ip}'
```

Die folgende Beispielausgabe zeigt die IP-Adresse des Eingangsgateways:

```
52.187.250.239
```

Öffnen Sie einen Browser, und fügen Sie die IP-Adresse ein. Die als Beispiel dienende AKS-Abstimmungs-App wird angezeigt.

![Die AKS-Abstimmungs-App wird in Ihrem für Istio aktivierten AKS-Cluster ausgeführt.](media/istio/deploy-app-01.png)

Aus den Informationen am unteren Rand des Bildschirms geht hervor, dass die App Version *1.0* der *Abstimmungs-App* und Version *1.0* (Redis) als Speicheroption verwendet.

## <a name="update-the-application"></a>Aktualisieren der Anwendung

Nun stellen wir eine neue Version der Analysekomponente bereit. Diese neue Version *1.1* zeigt zusätzlich zum Zähler für jede Kategorie auch Summen und Prozentwerte an.

Das folgende Diagramm zeigt, was am Ende dieses Abschnitts ausgeführt wird – Datenverkehr der *voting-app*-Komponente wird nur an Version *1.1* unserer *voting-analytics*-Komponente weitergeleitet. Zwar wird Version *1.0* unserer *voting-analytics*-Komponente weiterhin ausgeführt und vom *voting-analytics*-Dienst referenziert, die Istio-Proxys erlauben jedoch keinen Datenverkehr von und zu dieser Komponente.

![Die Komponenten der AKS-Abstimmungs-App und das Routing.](media/istio/components-and-routing-02.png)

Stellen wir nun Version *1.1* der *voting-analytics*-Komponente bereit. Erstellen Sie diese Komponente im Namespace *voting*:

```console
kubectl apply -f kubernetes/step-2-update-voting-analytics-to-1.1.yaml --namespace voting
```

Öffnen Sie die als Beispiel dienende AKS-Abstimmungs-App erneut in einem Browser. Verwenden Sie dabei die IP-Adresse des Istio-Eingangsgateways, die Sie im vorherigen Schritt ermittelt haben.

Ihr Browser wechselt zwischen den beiden unten gezeigten Ansichten. Da Sie für die *voting-analytics*-Komponente einen Kubernetes [Service][kubernetes-service] mit nur einem Bezeichnungsselektor (`app: voting-analytics`) verwenden, verwendet Kubernetes das Standardverhalten „Roundrobin“ zwischen den Pods, die diesem Selektor entsprechen. In diesem Fall handelt es sich dabei um die Versionen *1.0* und *1.1* Ihrer *voting-analytics*-Pods.

![Version 1.0 der Analytics-Komponente, die in unserer AKS-Abstimmungs-App ausgeführt wird.](media/istio/deploy-app-01.png)

![Version 1.1 der Analytics-Komponente, die in unserer AKS-Abstimmungs-App ausgeführt wird.](media/istio/update-app-01.png)

Das Umschalten zwischen den beiden Versionen der *voting-analytics*-Komponente können Sie wie folgt sichtbar machen. Denken Sie daran, die IP-Adresse Ihres eigenen Istio-Eingangsgateways zu verwenden.

```console
INGRESS_IP=52.187.250.239
for i in {1..5}; do curl -si $INGRESS_IP | grep results; done
```

Die folgende Beispielausgabe zeigt den relevanten Teil der zurückgegebenen Website, während die Website zwischen den Versionen wechselt:

```
  <div id="results"> Cats: 2 | Dogs: 4 </div>
  <div id="results"> Cats: 2 | Dogs: 4 </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2 | Dogs: 4 </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
```

### <a name="lock-down-traffic-to-version-11-of-the-application"></a>Begrenzen des Datenverkehrs auf Version 1.1 der Anwendung

Nun wollen wir den Datenverkehr auf Version *1.1* der *voting-analytics*-Komponente und Version *1.0* der *voting-storage*-Komponente begrenzen. Anschließend definieren Sie Routingregeln für alle übrigen Komponenten.

> * Ein *virtueller Dienst* definiert Routingregeln für einen oder mehrere Zieldienste.
> * Eine *Zielregel* definiert Datenverkehrsrichtlinien und versionsspezifische Richtlinien.
> * Eine *Richtlinie* definiert, welche Authentifizierungsmethoden für Workloads akzeptiert werden können.

Sie verwenden die `istioctl`-Clientbinärdatei, um die Definition Ihrer *voting-app* für den virtuellen Dienst zu ersetzen und [Zielregeln][istio-reference-destinationrule] und [virtuelle Dienste][istio-reference-virtualservice] für die übrigen Komponenten hinzuzufügen.

Ferner fügen Sie eine [Richtlinie][istio-reference-policy] zum *voting*-Namespace hinzu, um sicherzustellen, dass die gesamte Kommunikation zwischen Diensten durch gegenseitige TLS- und Clientzertifikate geschützt wird.

Da für *voting-app* eine Definition für den virtuellen Dienst vorhanden ist, die Sie ersetzen, verwenden Sie wie folgt den Befehl `istioctl replace`:

```console
istioctl replace -f istio/step-2a-update-voting-app-virtualservice.yaml --namespace voting
```

Die folgende Beispielausgabe zeigt, dass der virtuelle Dienst von Istio erfolgreich aktualisiert wurde:

```
Updated config virtual-service/voting/voting-app to revision 141902
```

Verwenden Sie als Nächstes den Befehl `istioctl create`, um die neue Richtlinie sowie die neuen Zielregeln und virtuellen Dienste für alle übrigen Komponenten hinzuzufügen.

* Für die Richtlinie ist `peers.mtls.mode` auf `STRICT` festgelegt, um sicherzustellen, dass innerhalb des *voting*-Namespace gegenseitiges TLS zwischen Ihren Diensten erzwungen wird.
* Legen Sie außerdem `trafficPolicy.tls.mode` in allen Ihren Zielregeln auf `ISTIO_MUTUAL` fest. Istio stellt Dienste mit starken Identitäten bereit und schützt die Kommunikation zwischen Diensten mithilfe von gegenseitigen TLS- und Clientzertifikaten, die Istio transparent verwaltet.

```console
istioctl create -f istio/step-2b-add-routing-for-all-components.yaml --namespace voting
```

Die folgende Beispielausgabe zeigt, dass die neue Richtlinie, die Zielregeln und die virtuellen Dienste erfolgreich erstellt wurden:

```
Created config policy/voting/default to revision 142118
Created config destination-rule/voting/voting-app at revision 142119
Created config destination-rule/voting/voting-analytics at revision 142120
Created config virtual-service/voting/voting-analytics at revision 142121
Created config destination-rule/voting/voting-storage at revision 142122
Created config virtual-service/voting/voting-storage at revision 142123
```

Wenn Sie die AKS-Abstimmungs-App erneut in einem Browser öffnen, wird von der *voting-app*-Komponente nur die neue Version *1.1* der *voting-analytics*-Komponente verwendet.

![Version 1.1 der Analytics-Komponente, die in unserer AKS-Abstimmungs-App ausgeführt wird.](media/istio/update-app-01.png)

Sie können wie folgt besser aufzeigen, dass das Routing jetzt nur zu Version *1.1* Ihrer *voting-analytics*-Komponente erfolgt. Denken Sie daran, die IP-Adresse Ihres Istio-Eingangsgateways zu verwenden.

Sie können wie folgt aufzeigen, dass das Routing jetzt nur zu Version *1.1* Ihrer *voting-analytics*-Komponente erfolgt. Denken Sie daran, die IP-Adresse Ihres eigenen Istio-Eingangsgateways zu verwenden:

```azurecli-interactive
INGRESS_IP=52.187.250.239
for i in {1..5}; do curl -si $INGRESS_IP | grep results; done
```

Die folgende Beispielausgabe zeigt den relevanten Teil der zurückgegebenen Website:

```
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
```

Vergewissern Sie sich, dass Istio gegenseitige TLS zum Schützen der Kommunikation zwischen Ihren einzelnen Diensten verwendet. Die folgenden Befehle überprüfen die TLS-Einstellungen für jeden der *voting-app*-Dienste:

```console
istioctl authn tls-check voting-app.voting.svc.cluster.local
istioctl authn tls-check voting-analytics.voting.svc.cluster.local
istioctl authn tls-check voting-storage.voting.svc.cluster.local
```

Die folgende Beispielausgabe zeigt, dass für jeden der Dienste gegenseitiges TLS über die Richtlinie und die Zielregeln erzwungen wird:

```
HOST:PORT                                    STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-app.voting.svc.cluster.local:8080     OK         mTLS       mTLS       default/voting     voting-app/voting

HOST:PORT                                          STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-analytics.voting.svc.cluster.local:8080     OK         mTLS       mTLS       default/voting     voting-analytics/voting

HOST:PORT                                        STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-storage.voting.svc.cluster.local:6379     OK         mTLS       mTLS       default/voting     voting-storage/voting
```

## <a name="roll-out-a-canary-release-of-the-application"></a>Rollout eines Canary-Releases der Anwendung

Nun stellen wir eine neue Version *2.0* der Komponenten *voting-app*, *voting-analytics* und *voting-storage* bereit. Die neue *voting-storage*-Komponente verwendet MySQL anstelle von Redis, und die Komponenten *voting-app* und *voting-analytics* werden aktualisiert, damit sie diese neue *voting-storage*-Komponente verwenden können.

Die *voting-app*-Komponente unterstützt nun die Funktion „Featureflags“. Mit diesem Featureflag können Sie die Eignung des Canary-Release von Istio für eine Teilmenge von Benutzern testen.

Das folgende Diagramm zeigt, was am Ende dieses Abschnitts ausgeführt wird.

* Version *1.0* der *voting-app*-Komponente, Version *1.1* der *voting-analytics*-Komponente und Version *1.0* der *voting-storage*-Komponente können miteinander kommunizieren.
* Version *2.0* der *voting-app*-Komponente, Version *2.0* der *voting-analytics*-Komponente und Version *2.0* der *voting-storage*-Komponente können miteinander kommunizieren.
* Auf Version *2.0* der *voting-app*-Komponente können nur Benutzer zugreifen, für die ein bestimmtes Featureflag festgelegt ist. Diese Änderung wird mit einem Featureflag über ein Cookie verwaltet.

![Die Komponenten der AKS-Abstimmungs-App und das Routing.](media/istio/components-and-routing-03.png)

Aktualisieren Sie zunächst die Istio-Zielregeln und die virtuellen Dienste, sodass diese neuen Komponenten berücksichtigt werden. Diese Aktualisierungen stellen sicher, dass Sie keinen Datenverkehr falsch an die neuen Komponenten weiterleiten und die Benutzer keinen unerwarteten Zugriff erhalten:

```console
istioctl replace -f istio/step-3-add-routing-for-2.0-components.yaml --namespace voting
```

Die folgende Beispielausgabe zeigt, dass die Zielregeln und virtuellen Dienste erfolgreich aktualisiert wurden:

```
Updated config destination-rule/voting/voting-app to revision 150930
Updated config virtual-service/voting/voting-app to revision 150931
Updated config destination-rule/voting/voting-analytics to revision 150937
Updated config virtual-service/voting/voting-analytics to revision 150939
Updated config destination-rule/voting/voting-storage to revision 150940
Updated config virtual-service/voting/voting-storage to revision 150941
```

Nun fügen wir die Kubernetes-Objekte für die Komponenten der neuen Version *2.0* hinzu. Sie aktualisieren außerdem den *voting-storage*-Dienst so, dass auch der Port *3306* für MySQL eingeschlossen wird:

```console
kubectl apply -f kubernetes/step-3-update-voting-app-with-new-storage.yaml --namespace voting
```

Die folgende Beispielausgabe zeigt, dass die Kubernetes-Objekte erfolgreich aktualisiert bzw. erstellt wurden:

```
service/voting-storage configured
secret/voting-storage-secret created
deployment.apps/voting-storage-2-0 created
persistentvolumeclaim/mysql-pv-claim created
deployment.apps/voting-analytics-2-0 created
deployment.apps/voting-app-2-0 created
```

Warten Sie, bis alle Pods der Version *2.0* ausgeführt werden. Verwenden Sie den Befehl [kubectl get pods][kubectl-get], um alle Pods im Namespace *voting* anzuzeigen:

```azurecli-interactive
kubectl get pods --namespace voting
```

Sie sollten jetzt zwischen Version *1.0* und Version *2.0* (Canary) der Abstimmungsanwendung wechseln können. Der Featureflag-Umschalter am unteren Bildschirmrand setzt ein Cookie. Dieses Cookie wird vom virtuellen Dienst der *voting-app* verwendet, um Benutzer an die neue Version *2.0* zu leiten.

![Version 1.0 der AKS-Abstimmungs-App: Featureflag IST NICHT festgelegt.](media/istio/canary-release-01.png)

![Version 2.0 der AKS-Abstimmungs-App: Featureflag IST festgelegt.](media/istio/canary-release-02.png)

Die Abstimmungszähler unterscheiden sich in den verschiedenen Versionen der Anwendung. An diesem Unterschied ist ersichtlich, dass Sie zwei verschiedene Speicher-Back-Ends verwenden.

## <a name="finalize-the-rollout"></a>Abschließen des Rollouts

Nachdem Sie das Canary-Release erfolgreich getestet haben, aktualisieren Sie den virtuellen Dienst der *voting-app*, um den gesamten Datenverkehr an Version *2.0* der *voting-app* zu leiten. Alle Benutzer sehen dann Version *2.0* der Anwendung, unabhängig davon, ob das Featureflag festgelegt ist oder nicht:

![Die Komponenten der AKS-Abstimmungs-App und das Routing.](media/istio/components-and-routing-04.png)

Aktualisieren Sie alle Zielregeln, um diejenigen Versionen der Komponenten zu entfernen, die nicht mehr aktiv sein sollen. Aktualisieren Sie anschließend alle virtuellen Dienste, damit nicht mehr auf diese Versionen verwiesen wird.

Da es nun keinen Datenverkehr mehr zu den älteren Versionen der Komponenten gibt, können Sie jetzt alle Bereitstellungen für diese Komponenten sicher löschen.

![Die Komponenten der AKS-Abstimmungs-App und das Routing.](media/istio/components-and-routing-05.png)

Sie haben nun erfolgreich das Rollout einer neuen Version der AKS-Abstimmungs-App durchgeführt.

## <a name="next-steps"></a>Nächste Schritte

Mithilfe der [Istio-Beispielanwendung „Bookinfo“][istio-bookinfo-example] können Sie sich weitere Szenarios ansehen.

<!-- LINKS - external -->
[github-azure-sample]: https://github.com/Azure-Samples/aks-voting-app
[istio]: https://istio.io
[istio-docs-concepts]: https://istio.io/docs/concepts/what-is-istio/
[istio-github]: https://github.com/istio/istio
[istio-requirements-pods-and-services]: https://istio.io/docs/setup/kubernetes/spec-requirements/
[istio-reference-gateway]: https://istio.io/docs/reference/config/istio.networking.v1alpha3/#Gateway
[istio-reference-policy]: https://istio.io/docs/reference/config/istio.authentication.v1alpha1/#Policy
[istio-reference-virtualservice]: https://istio.io/docs/reference/config/istio.networking.v1alpha3/#VirtualService
[istio-reference-destinationrule]: https://istio.io/docs/reference/config/istio.networking.v1alpha3/#DestinationRule
[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/
[istio-bookinfo-example]: https://istio.io/docs/examples/bookinfo/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[istio-install]: ./istio-install.md
