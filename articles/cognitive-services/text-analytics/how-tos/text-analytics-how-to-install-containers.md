---
title: Installieren und Ausführen von Containern
titleSuffix: Text Analytics -  Azure Cognitive Services
description: Informationen zum Herunterladen, Installieren und Ausführen von Containern für die Textanalyse in diesem Schritt-für-Schritt-Tutorial.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 11/14/2018
ms.author: diberry
ms.openlocfilehash: 11798c3bfd4032ad10c738032a816a2a0488ce67
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53090532"
---
# <a name="install-and-run-containers"></a>Installieren und Ausführen von Containern

Die Verwendung von Containern ist ein Ansatz zur Softwareverteilung, bei dem eine Anwendung oder ein Dienst als Containerimage gepackt wird. Die Konfiguration und die Abhängigkeiten der Anwendung bzw. des Diensts sind im Containerimage enthalten. Das Containerimage kann anschließend mit geringer oder ganz ohne Bearbeitung auf einem Containerhost bereitgestellt werden. Container sind voneinander und vom zugrunde liegenden Betriebssystem isoliert und nehmen weniger Speicher in Anspruch als ein virtueller Computer. Container können für kurzfristige Aufgaben über Containerimages instanziiert und wieder entfernt werden, wenn sie nicht mehr benötigt werden.

Die Textanalyse stellt die folgenden Docker-Container bereit, die jeweils eine Teilmenge der Funktionen enthalten:

| Container| BESCHREIBUNG |
|----------|-------------|
|Schlüsselwortextraktion | Extrahiert die Schlüsselbegriffe, um die wichtigsten Punkte zu ermitteln. Wenn der eingegebene Text beispielsweise „Das Essen war köstlich, und es gab hervorragendes Personal“ lautet, gibt die API die Kernpunkte „Essen“ und „hervorragendes Personal“ zurück. |
|Spracherkennung | Erkennt für bis zu 120 Sprachen, in welcher Sprache der eingegebene Text geschrieben wurde, und gibt diese aus. Der Container gibt einen einzelnen Sprachcode für jedes Dokument aus, das in der Anforderung enthalten ist. Der Sprachcode ist mit einem Wert kombiniert, der die Stärke der Bewertung angibt. |
|Standpunktanalyse | Analysiert unformatierten Text auf Hinweise auf positive oder negative Stimmungen. Die API gibt für jedes Dokument eine Bewertung des Standpunkts zwischen 0 und 1 zurück. Hierbei entspricht 1 einer positiven Bewertung. Die Analysemodelle sind mithilfe großer Textmengen und Microsoft-Technologien für natürliche Sprache vortrainiert. Für [ausgewählte Sprachen](../language-support.md) kann die API jeden bereitgestellten unformatierten Text analysieren und bewerten und der aufrufenden Anwendung direkt Ergebnisse zurückgeben. |

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="preparation"></a>Vorbereitung

Zur Verwendung des Containers für die Textanalyse müssen die folgenden Voraussetzungen erfüllt sein:

**Docker-Engine:** Die Docker-Engine muss lokal installiert sein. Docker stellt Pakete zur Konfiguration der Docker-Umgebung unter [macOS](https://docs.docker.com/docker-for-mac/), [Linux](https://docs.docker.com/engine/installation/#supported-platforms) und [Windows](https://docs.docker.com/docker-for-windows/) zur Verfügung. Unter Windows muss Docker für die Unterstützung von Linux-Containern konfiguriert werden. Docker-Container können auch direkt für [Azure Kubernetes Service](/azure/aks/), [Azure Container Instances](/azure/container-instances/) oder einen [Kubernetes](https://kubernetes.io/)-Cluster mit Bereitstellung in [Azure Stack](/azure/azure-stack/) bereitgestellt werden. Weitere Informationen zum Bereitstellen von Kubernetes in Azure Stack finden Sie unter [Bereitstellen von Kubernetes in Azure Stack](/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy).

Docker muss so konfiguriert werden, dass die Container eine Verbindung mit Azure herstellen und Abrechnungsdaten an Azure senden können.

**Kenntnisse von Microsoft Container Registry und Docker:** Sie sollten über Grundkenntnisse der Konzepte von Microsoft Container Registry und Docker verfügen, einschließlich Registrierungen, Repositorys, Container und Containerimages, und die grundlegenden `docker`-Befehle kennen.  

Eine Einführung in Docker und Container finden Sie in der [Docker-Übersicht](https://docs.docker.com/engine/docker-overview/).

### <a name="container-requirements-and-recommendations"></a>Containeranforderungen und -empfehlungen

In der folgenden Tabelle werden die Mindestanforderungen und Empfehlungen für CPU-Kerne (mindestens 2,6 GHz) und Arbeitsspeicher in Gigabyte (GB) angegeben, die für jeden Container für die Textanalyse zugewiesen werden müssen.

| Container | Minimum | Empfohlen |
|-----------|---------|-------------|
|Schlüsselwortextraktion | 1 Kern, 2 GB Arbeitsspeicher | 1 Kern, 4 GB Arbeitsspeicher |
|Spracherkennung | 1 Kern, 2 GB Arbeitsspeicher | 1 Kern, 4 GB Arbeitsspeicher |
|Standpunktanalyse | 1 Kern, 2 GB Arbeitsspeicher | 1 Kern, 4 GB Arbeitsspeicher |

## <a name="download-container-images-from-microsoft-container-registry"></a>Herunterladen von Containerimages aus der Microsoft Container Registry

In der Microsoft Container Registry stehen Containerimages für die Textanalyse zur Verfügung. Die folgende Tabelle enthält die Repositorys, die über die Microsoft Container Registry für Container für die Textanalyse verfügbar sind. Jedes Repository enthält ein Containerimage, das heruntergeladen werden muss, um den Container lokal auszuführen.

| Container | Repository |
|-----------|------------|
|Schlüsselwortextraktion | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |
|Spracherkennung | `mcr.microsoft.com/azure-cognitive-services/language` |
|Standpunktanalyse | `mcr.microsoft.com/azure-cognitive-services/sentiment` |

Verwenden Sie den Befehl [docker pull](https://docs.docker.com/engine/reference/commandline/pull/), um ein Containerimage aus einem Repository herunterzuladen. Verwenden Sie beispielsweise den folgenden Befehl, um das neueste Containerimage für die Schlüsselbegriffserkennung aus dem Repository herunterzuladen:

```Docker
docker pull mcr.microsoft.com/azure-cognitive-services/keyphrase:latest
```

Eine vollständige Beschreibung der verfügbaren Tags für die Container für die Textanalyse finden Sie in den folgenden Containern im Docker-Hub:

* [Schlüsselbegriffserkennung](https://go.microsoft.com/fwlink/?linkid=2018757)
* [Sprachenerkennung](https://go.microsoft.com/fwlink/?linkid=2018759)
* [Standpunktanalyse](https://go.microsoft.com/fwlink/?linkid=2018654)

> [!TIP]
> Mithilfe des Befehls [docker images](https://docs.docker.com/engine/reference/commandline/images/) können Sie Ihre heruntergeladenen Containerimages auflisten. Mit dem folgenden Befehl werden beispielsweise die ID, das Repository und das Tag jedes heruntergeladenen Containerimages in Form einer Tabelle aufgelistet:
>
>  ```Docker
>  docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
>  ```
>

## <a name="instantiate-a-container-from-a-downloaded-container-image"></a>Instanziieren eines Containers über ein heruntergeladenes Containerimage

Verwenden Sie den Befehl [docker run](https://docs.docker.com/engine/reference/commandline/run/), um einen Container über ein heruntergeladenes Containerimage zu instanziieren. Der folgende Befehl führt beispielsweise folgende Aktionen durch:

* Instanziieren eines Containers über das Containerimage für die Standpunktanalyse
* Zuweisen von einem CPU-Kern und 8 GB Arbeitsspeicher
* Verfügbarmachen des TCP-Ports 5000 und Zuweisen einer Pseudo-TTY-Verbindung für den Container
* Automatisches Entfernen des Containers, nachdem er beendet wurde

```Docker
docker run --rm -it -p 5000:5000 --memory 8g --cpus 1 mcr.microsoft.com/azure-cognitive-services/sentiment Eula=accept Billing=https://westus.api.cognitive.microsoft.com/text/analytics/v2.0 ApiKey=0123456789

```

> [!IMPORTANT]
> Die Befehlszeilenoptionen `Eula`, `Billing` und `ApiKey` müssen angegeben werden, um den Container zu instanziieren, andernfalls wird der Container nicht gestartet.  Weitere Informationen finden Sie unter [Abrechnung](#billing).

Nach der Instanziierung können Sie Vorgänge aus dem Container über den Host-URI des Containers aufrufen. Beispielsweise steht der folgende Host-URI für den Container für die Standpunktanalyse, der im vorherigen Beispiel instanziiert wurde:

```http
http://localhost:5000/
```

> [!TIP]
> Über den relativen `/swagger`-URI für einen Container können Sie auf die [OpenAPI-Spezifikation](https://swagger.io/docs/specification/about/) (früher Swagger-Spezifikation) zugreifen, in der die vom instanziierten Container unterstützten Vorgänge beschrieben werden. Der folgende URI bietet beispielsweise Zugriff auf die OpenAPI-Spezifikation für den Container für die Standpunktanalyse, der im vorherigen Beispiel instanziiert wurde:
>
>  ```http
>  http://localhost:5000/swagger
>  ```

Sie können entweder die in Ihrem Container verfügbaren [REST-API-Vorgänge aufrufen](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-call-api) oder die [Clientbibliothek für die Textanalyse-API von Azure Cognitive Services](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics) verwenden, um diese Vorgänge aufzurufen.  
> [!IMPORTANT]
> Sie benötigen Version 2.1.0 oder höher des Azure Cognitive Services SDK für die Textanalyse, wenn Sie die Clientbibliothek mit Ihrem Container verwenden möchten.

Der einzige Unterschied zwischen dem Aufrufen eines bestimmten Vorgangs aus dem Container und dem Aufrufen desselben Vorgangs von einem entsprechenden Dienst in Azure ist, dass Sie den Host-URI des Containers anstelle des Host-URI einer Azure-Region verwenden, um den Vorgang aufzurufen. Wenn Sie z.B. eine Instanz der Textanalyse, die in der Azure-Region „USA, Westen“ ausgeführt wird, verwenden möchten, rufen Sie den folgenden REST-API-Vorgang auf:

```http
POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases
```

Wenn Sie einen Container für die Schlüsselbegriffserkennung, der auf dem lokalen Computer ausgeführt wird, mit der Standardkonfiguration verwenden möchten, rufen Sie den folgenden REST-API-Vorgang auf:

```http
POST http://localhost:5000/text/analytics/v2.0/keyPhrases
```

### <a name="billing"></a>Abrechnung

Der Container für die Textanalyse sendet Abrechnungsinformationen an Azure und verwendet dafür eine entsprechende Textanalyseressource in Ihrem Azure-Konto. Die folgenden Befehlszeilenoptionen werden zu Abrechnungszwecken vom Container für die Textanalyse verwendet:

| Option | BESCHREIBUNG |
|--------|-------------|
| `ApiKey` | Der API-Schlüssel der Textanalyseressource, der zum Nachzuverfolgen von Abrechnungsinformationen verwendet wird.<br/>Der Wert dieser Option muss für die bereitgestellte Azure-Ressource für die Textanalyse, die in `Billing` angegeben wurde, auf einen API-Schlüssel festgelegt werden. |
| `Billing` | Der Endpunkt der Textanalyseressource, der zum Nachzuverfolgen von Abrechnungsinformationen verwendet wird.<br/>Der Wert dieser Option muss auf den Endpunkt-URI einer bereitgestellten Azure-Ressource für die Textanalyse festgelegt werden.|
| `Eula` | Gibt an, dass Sie die Lizenz für den Container akzeptiert haben.<br/>Der Wert dieser Option muss auf `accept` festgelegt werden. |

> [!IMPORTANT]
> Alle drei Optionen müssen mit gültigen Werten angegeben werden, damit der Container gestartet wird.

Weitere Informationen zu diesen Optionen finden Sie unter [Konfigurieren von Containern](../text-analytics-resource-container-config.md).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben Sie die Konzepte und den Workflow zum Herunterladen, Installieren und Ausführen von Containern für die Textanalyse kennengelernt. Zusammenfassung:

* Die Textanalyse stellt drei Linux-Container für Docker bereit: Schlüsselbegriffserkennung und Sprachenerkennung und Standpunktanalyse.
* Containerimages werden aus Microsoft Container Registry (MCR) in Azure heruntergeladen.
* Containerimages werden in Docker ausgeführt.
* Sie können entweder die REST-API oder das SDK verwenden, um Vorgänge in Containern für die Textanalyse über den Host-URI des Containers aufzurufen.
* Bei der Instanziierung eines Containers müssen Sie Abrechnungsinformationen angeben.

> [!IMPORTANT]
> Für die Ausführung von Cognitive Services-Containern besteht keine Lizenz, wenn sie nicht zu Messzwecken mit Azure verbunden sind. Kunden müssen sicherstellen, dass Container jederzeit Abrechnungsinformationen an den Messungsdienst übermitteln können. Cognitive Services-Container senden keine Kundendaten (z.B. das analysierte Bild oder den analysierten Text) an Microsoft.

## <a name="next-steps"></a>Nächste Schritte

* Konfigurationseinstellungen finden Sie unter [Konfigurieren von Containern](../text-analytics-resource-container-config.md).
* In der [Übersicht zur Textanalyse](../overview.md) finden Sie weitere Informationen zu Schlüsselbegriffserkennung, Sprachenerkennung und Standpunktanalyse  
* Details zu den vom Container unterstützten Methoden finden Sie unter [Textanalyse-API](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6).
* Unter [Häufig gestellte Fragen (FAQ)](../text-analytics-resource-faq.md) finden Sie Informationen zum Beheben von Problemen im Zusammenhang mit der Funktionalität des maschinellen Sehens.