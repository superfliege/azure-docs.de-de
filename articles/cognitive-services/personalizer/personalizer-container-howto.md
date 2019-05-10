---
title: 'Installieren und Ausführen von Containern: Personalisierung'
titleSuffix: Azure Cognitive Services
description: Hier finden Sie Informationen zum Herunterladen, Installieren und Ausführen von Containern für die Personalisierung.
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/09/2019
ms.author: edjez
ms.openlocfilehash: a197531a7c78823271c0a5fa5413b76746f63a9a
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2019
ms.locfileid: "65507027"
---
# <a name="install-and-run-personalizer-containers"></a>Installieren und Ausführen von Personalisierungscontainern

Der Personalisierungsdienst enthält die folgenden Container: 

|Funktion|Features|
|-|-|
|Personalisierung|Bestimmen Sie auf der Grundlage des aktuellen Inhalts- und Benutzerkontexts die optimale Aktion.|

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung der Container des Personalisierungsdiensts müssen die folgenden Voraussetzungen erfüllt sein:

|Erforderlich|Zweck|
|--|--|
|Docker-Engine| Die Docker-Engine muss auf einem [Hostcomputer](#the-host-computer) installiert sein. Für die Docker-Umgebung stehen Konfigurationspakete für [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) und [Linux](https://docs.docker.com/engine/installation/#supported-platforms) zur Verfügung. Eine Einführung in Docker und Container finden Sie in der [Docker-Übersicht](https://docs.docker.com/engine/docker-overview/).<br><br> Docker muss so konfiguriert werden, dass die Container eine Verbindung mit Azure herstellen und Abrechnungsdaten an Azure senden können. <br><br> **Unter Windows** muss Docker auch für die Unterstützung von Linux-Containern konfiguriert werden.<br><br>|
|Kenntnisse zu Docker | Sie sollten über Grundkenntnisse der Konzepte von Docker, einschließlich Registrierungen, Repositorys, Container und Containerimages, verfügen und die grundlegenden `docker`-Befehle kennen.| 
|Personalisierungsdienstressource |Um diese Container zu verwenden, benötigen Sie Folgendes:<br><br>Eine Azure-Ressource vom Typ _Personalisierungsdienst_, um den entsprechenden Abrechnungsschlüssel und den URI des Abrechnungsendpunkts zu erhalten. Beide Werte stehen im Azure-Portal auf der Übersichts- und auf der Schlüsselseite des Personalisierungsdiensts zur Verfügung und werden zum Starten des Containers benötigt.<br><br>**{BILLING_KEY}**: Der Ressourcenschlüssel.<br><br>**{BILLING_ENDPOINT_URI}**: Der Endpunkt-URI. Beispiel: `https://westus.api.cognitive.microsoft.com/`|

### <a name="the-host-computer"></a>Der Hostcomputer

Der **Host** ist der Computer, auf dem der Docker-Container ausgeführt wird. Dies kann ein lokaler Computer oder ein Docker-Hostingdienst in Azure sein, einschließlich:

* [Azure Kubernetes Service](https://docs.microsoft.com/aks/index.yml)
* [Azure Container Instances](https://docs.microsoft.com/container-instances/index.yml)
* Ein in [Azure Stack](https://docs.microsoft.com/azure-stack/index.yml) bereitgestellter [Kubernetes](https://kubernetes.io/)-Cluster. Weitere Informationen finden Sie unter [Bereitstellen von Kubernetes in Azure Stack](https://docs.microsoft.com/azure-stack/user/azure-stack-solution-template-kubernetes-deploy.md).

### <a name="container-requirements-and-recommendations"></a>Containeranforderungen und -empfehlungen

In der folgenden Tabelle werden die Mindestanforderungen und empfohlenen Werte für die CPU-Kerne und den Arbeitsspeicher für die einzelnen Container des Personalisierungsdiensts beschrieben:

| Container | Minimum | Empfohlen |
|-----------|---------|-------------|
|Personalisierung | 1 Kern, 4 GB Arbeitsspeicher | Zwei Kerne, 8 GB Arbeitsspeicher |

Jeder Kern muss eine Geschwindigkeit von mindestens 2,6 GHz aufweisen.

Kern und Arbeitsspeicher entsprechen den Einstellungen `--cpus` und `--memory`, die im Rahmen des Befehls `docker run` verwendet werden.

Der Container muss eine Verbindung mit Azure EventHub herstellen können, um Informationen in den Rangfolge- und Relevanzaufrufen an den Personalisierungsserver in Azure weiterzuleiten. Darüber hinaus muss er eine Verbindung mit der Personalisierungs-API herstellen können, um die erforderliche Konfiguration und die aktuellen Machine Learning-Modelle laden zu können.

## <a name="get-the-container-image-with-docker-pull"></a>Abrufen des Containerimages mit `docker pull`

Für den Personalisierungsdienst stehen Containerimages zur Verfügung. 

| Container | Repository |
|-----------|------------|
| Personalisierung | `mcr.microsoft.com/azure-cognitive-services/personalizer:latest` |

> [!TIP]
> Mithilfe des Befehls [docker images](https://docs.docker.com/engine/reference/commandline/images/) können Sie Ihre heruntergeladenen Containerimages auflisten. Mit dem folgenden Befehl werden beispielsweise die ID, das Repository und das Tag jedes heruntergeladenen Containerimages in Form einer Tabelle aufgelistet:
>
>  ```Docker
>  docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
>
>  IMAGE ID            REPOSITORY                                                                TAG
>  ebbee78a6baa        mcr.microsoft.com/azure-cognitive-services/personalizer                 latest
>  ``` 

### <a name="docker-pull-for-the-personalizer-service-container"></a>Docker-Pullvorgang für den Personalisierungsdienstcontainer

```Docker
docker pull mcr.microsoft.com/azure-cognitive-services/personalizer:latest
```

## <a name="how-the-container-works"></a>Funktionsweise des Containers

Der Personalisierungscontainer stellt den Clientteil des Personalisierungsdiensts dar. Bei der Ausführung des Containers werden die Modelle und Konfigurationseinstellungen aus dem Personalisierungsdienst in der Azure-Cloud abgerufen. Der Containerdienst nutzt diese Informationen, um auf **Rangfolge-** und **Relevanzanforderungen** zu reagieren. Der Container sendet diese Anforderungen außerdem an die Personalisierungsressource in der Azure-Cloud. Diese Informationen werden dann basierend auf der aktuellen Einstellung für die Häufigkeit der Modellaktualisierung zum Trainieren Ihres Personalisierungsschleifenmodells verwendet. Das aktualisierte Modell wird zurück an den Container gesendet. 

![Lokale Architektur für den Client des Personalisierungscontainers](./media/personalizer-container-howto/personalizer-container-architecture.png)

## <a name="how-to-use-the-container"></a>Verwenden des Containers

Wenn sich der Container auf dem [Hostcomputer](#the-host-computer) befindet, können Sie über den folgenden Prozess mit dem Container arbeiten.

1. [Führen Sie den Container aus](#run-the-container-with-docker-run), und verwenden Sie dabei die erforderlichen Abrechnungseinstellungen. Es sind noch weitere [Beispiele](personalizer-container-configuration.md#example-docker-run-commands) für den Befehl `docker run` verfügbar. 
1. [Fragen Sie den Vorhersageendpunkt des Containers ab.](#query-the-containers-prediction-endpoint) 

## <a name="run-the-container-with-docker-run"></a>Ausführen des Containers mit `docker run`

Verwenden Sie den Befehl [docker run](https://docs.docker.com/engine/reference/commandline/run/), um einen der drei Container auszuführen. Für den Befehl werden die folgenden Parameter verwendet:

| Platzhalter | Wert |
|-------------|-------|
|{BILLING_KEY} | Dieser Schlüssel wird zum Starten des Containers verwendet und steht im Azure-Portal auf der Schlüsselseite für den Personalisierungsdienst zur Verfügung.  |
|{BILLING_ENDPOINT_URI} | Den URI des Abrechnungsendpunkts finden Sie im Azure-Portal auf der Übersichtsseite für den Personalisierungsdienst.|

Ersetzen Sie im folgenden Beispiel für den Befehl `docker run` diese Parameter durch Ihre eigenen Werte.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/personalizer\
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Dieser Befehl:

* Führt einen Container für den Personalisierungsdienst auf der Grundlage des Containerimages aus.
* Weist einen einzelnen CPU-Kern und 4 GB Arbeitsspeicher zu
* Verfügbarmachen des TCP-Ports 5000 und Zuweisen einer Pseudo-TTY-Verbindung für den Container
* Entfernt den Container automatisch, nachdem er beendet wurde. Das Containerimage ist auf dem Hostcomputer weiterhin verfügbar. 

> [!IMPORTANT]
> Die Optionen `Eula`, `Billing` und `ApiKey` müssen angegeben werden, um den Container auszuführen, andernfalls wird der Container nicht gestartet.  Weitere Informationen finden Sie unter [Abrechnung](#billing).

### <a name="run-multiple-containers-on-the-same-host"></a>Ausführen mehrerer Container auf dem gleichen Host

Wenn Sie beabsichtigen, mehrere Container mit offengelegten Ports auszuführen, stellen Sie sicher, dass jeder Container mit einem anderen Port ausgeführt wird. Führen Sie beispielsweise den ersten Container an Port 5000 und den zweiten Container an Port 5001 aus.

Ersetzen Sie `<container-registry>` und `<container-name>` durch die Werte der von Ihnen verwendeten Container. Dabei muss es sich nicht um die gleichen Container handeln. Sie können den Personalisierungscontainer und den LUIS-Container zusammen auf dem HOST ausführen, oder Sie können mehrere Personalisierungscontainer ausführen. 

Führen Sie den ersten Container an Port 5000 aus. 

```bash 
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Führen Sie den zweiten Container an Port 5001 aus.


```bash 
docker run --rm -it -p 5000:5001 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Jeder weitere Container sollte an einem anderen Port ausgeführt werden. 

## <a name="query-the-containers-prediction-endpoint"></a>Abfragen des Vorhersageendpunkts des Containers

Der Container stellt REST-basierte Endpunkt-APIs für die Abfragevorhersage bereit. 

Verwenden Sie für Container-APIs den Host https://localhost:5000.

## <a name="stop-the-container"></a>Beenden des Containers

Zum Herunterfahren des Containers drücken Sie in der Befehlszeilenumgebung, in der der Container ausgeführt wird, **STRG+C**.

## <a name="troubleshoot"></a>Problembehandlung

Wenn Sie den Container mit einer [Ausgabenbereitstellung](personalizer-container-configuration.md#mount-settings) ausführen und die Protokollierung aktiviert ist, generiert der Container Protokolldateien. Diese sind hilfreich, um Probleme beim Starten oder Ausführen des Containers zu beheben. 

## <a name="container-api-documentation"></a>Dokumentation zur Container-API

Der Container stellt eine umfassende Dokumentation für die Endpunkte sowie die Funktion `Try it now` bereit. Dieses Feature ermöglicht Ihnen die Eingabe Ihrer Einstellungen in einem webbasierten HTML-Formular, sodass Sie die Abfrage ausführen können, ohne Code schreiben zu müssen. Nach der Rückgabe der Abfrage wird ein cURL-Beispielbefehl bereitgestellt, der das erforderliche Format für HTTP-Header und -Text veranschaulicht. 

> [!TIP]
> In der [OpenAPI-Spezifikation](https://swagger.io/docs/specification/about/) werden die API-Vorgänge beschrieben, die vom Container vom relativen URI `/swagger` unterstützt werden. Beispiel: 
>
>  ```http
>  http://localhost:5000/swagger
>  ```

## <a name="billing"></a>Abrechnung

Personalisierungsdienstcontainer senden Abrechnungsinformationen an Azure und verwenden dafür eine Ressource vom Typ _Personalisierungsdienst_ in Ihrem Azure-Konto. 

Für die Ausführung von Cognitive Services-Containern besteht keine Lizenz, wenn sie nicht zu Messzwecken mit Azure verbunden sind. Kunden müssen sicherstellen, dass Container jederzeit Abrechnungsinformationen an den Messungsdienst übermitteln können.  

Der Befehl `docker run` verwendet folgende Argumente für Abrechnungszwecke:

| Option | BESCHREIBUNG |
|--------|-------------|
| `ApiKey` | Der API-Schlüssel der Ressource vom Typ _Personalisierungsdienst_, der zum Nachverfolgen von Abrechnungsinformationen verwendet wird. |
| `Billing` | Der Endpunkt der Ressource vom Typ _Personalisierungsdienst_, der zum Nachverfolgen von Abrechnungsinformationen verwendet wird.|
| `Eula` | Gibt an, dass Sie die Lizenz für den Container akzeptiert haben.<br/>Der Wert dieser Option muss auf `accept` festgelegt werden. |

> [!IMPORTANT]
> Alle drei Optionen müssen mit gültigen Werten angegeben werden, damit der Container gestartet wird.

Weitere Informationen zu diesen Optionen finden Sie unter [Konfigurieren von Containern](personalizer-container-configuration.md).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben Sie die Konzepte und den Workflow zum Herunterladen, Installieren und Ausführen von Personalisierungsdienstcontainern kennengelernt. Zusammenfassung:

* Der Personalisierungsdienst stellt einen Linux-Container für Docker bereit, der die Personalisierung kapselt.
* Containerimages werden aus Microsoft Container Registry (MCR) in Azure heruntergeladen.
* Containerimages werden in Docker ausgeführt.
* Sie können entweder die REST-API oder das SDK verwenden, um Vorgänge in Personalisierungsdienstcontainern über den Host-URI des Containers aufzurufen.
* Bei der Instanziierung eines Containers müssen Sie Abrechnungsinformationen angeben.

> [!IMPORTANT]
> Für die Ausführung von Cognitive Services-Containern besteht keine Lizenz, wenn sie nicht zu Messzwecken mit Azure verbunden sind. Kunden müssen sicherstellen, dass Container jederzeit Abrechnungsinformationen an den Messungsdienst übermitteln können. Die Personalisierungscontainer senden die Anforderungsdaten darüber hinaus für das Onlinetraining an den entsprechenden Dienst in Azure und rufen in regelmäßigen Abständen aktualisierte Modelle aus Azure ab.

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren eines Docker-Containers für den Befehl `Docker Run`](personalizer-container-configuration.md)