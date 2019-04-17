---
title: Containerunterstützung
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Docker-Container Cognitive Services näher an Ihre Daten bringen können.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: article
ms.date: 03/04/2019
ms.author: diberry
ms.openlocfilehash: c6cf3f188309891b0456b034e3809a0f0209c124
ms.sourcegitcommit: 6e32f493eb32f93f71d425497752e84763070fad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2019
ms.locfileid: "59469538"
---
# <a name="container-support-in-azure-cognitive-services"></a>Containerunterstützung in Azure Cognitive Services

Dank der Containerunterstützung in Azure Cognitive Services können Entwickler die gleichen umfangreichen APIs verwenden wie in Azure und dabei flexibel entscheiden, wo die mit [Docker-Containern](https://www.docker.com/what-container) verbundenen Dienste bereitgestellt und gehostet werden. Die Containerunterstützung ist derzeit als Vorschauversion für einen Teil von Azure Cognitive Services verfügbar, einschließlich [Maschinelles Sehen](Computer-vision/Home.md), [Gesichtserkennung](Face/Overview.md) und [Textanalyse](text-analytics/overview.md) sowie [Language Understanding (LUIS)](LUIS/luis-container-howto.md).

Die Verwendung von Containern ist ein Ansatz zur Softwareverteilung, bei dem eine Anwendung oder ein Dienst, einschließlich der Abhängigkeiten und der Konfiguration, als Containerimage zusammengepackt wird. Das Containerimage kann mit geringer oder ganz ohne Bearbeitung auf einem Containerhost bereitgestellt werden. Container sind voneinander und vom zugrunde liegenden Betriebssystem isoliert und nehmen weniger Speicher in Anspruch als ein virtueller Computer. Container können für kurzfristige Aufgaben über Containerimages instanziiert und wieder entfernt werden, wenn sie nicht mehr benötigt werden.

Das folgende Video veranschaulicht die Verwendung eines Cognitive Services-Containers.

[![CContainerdemo für Cognitive Services(./media/index/containers-video-image.png)](https://azure.microsoft.com/resources/videos/containers-support-of-cognitive-services)

Die Dienste [Maschinelles Sehen](Computer-vision/Home.md), [Gesichtserkennung](Face/Overview.md), [Textanalyse](text-analytics/overview.md) und [Language Understanding (LUIS)](LUIS/what-is-luis.md) stehen in [Microsoft Azure](https://azure.microsoft.com) zur Verfügung. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, um Azure-Ressourcen für diese Dienste zu erstellen und zu erkunden.

## <a name="features-and-benefits"></a>Features und Vorteile

- **Kontrolle über Daten:** Ermöglichen Sie Kunden, auszuwählen, wo Cognitive Services ihre Daten verarbeitet. Dies ist wichtig für Kunden, die keine Daten in die Cloud senden können, aber auf die Cognitive Services-Technologie zugreifen müssen. Einheitliche Unterstützung in Hybridumgebungen – für Daten, Verwaltung, Identität und Sicherheit.
- **Kontrolle über Modellaktualisierungen:** Bietet Kunden Flexibilität bei der Versionsverwaltung und Aktualisierung der in ihren Lösungen eingesetzten Modelle.
- **Portable Architektur:** Ermöglicht die Erstellung einer portablen Anwendungsarchitektur, die in Azure, lokal und am Edge eingesetzt werden kann. Container können direkt für [Azure Kubernetes Service](../aks/index.yml), [Azure Container Instances](../container-instances/index.yml) oder einen [Kubernetes](https://kubernetes.io/)-Cluster mit Bereitstellung in [Azure Stack](../azure-stack/index.yml) bereitgestellt werden. Weitere Informationen finden Sie unter [Bereitstellen von Kubernetes in Azure Stack](../azure-stack/user/azure-stack-solution-template-kubernetes-deploy.md).
- **Hoher Durchsatz/niedrige Latenz:** Bietet Kunden die Möglichkeit, für hohe Durchsatzraten und niedrige Latenzanforderungen zu skalieren, indem Cognitive Services physisch in der Nähe ihrer Anwendungslogik und -daten ausgeführt wird. Container erzwingen keine Obergrenzen für die Transaktionen pro Sekunde (TPS) und können für die zentrale und horizontale Skalierung konfiguriert werden, um bei Bedarf die erforderlichen Hardwareressourcen bereitzustellen. 


## <a name="containers-in-azure-cognitive-services"></a>Container in Azure Cognitive Services

Azure Cognitive Services-Container bieten den folgenden Satz von Docker-Containern, von denen jeder eine Teilmenge der Funktionalität von Diensten in Azure Cognitive Services enthält:

| Dienst | Unterstützter Tarif | Container | BESCHREIBUNG |
|---------|----------|----------|-------------|
|[Maschinelles Sehen](Computer-vision/computer-vision-how-to-install-containers.md) |F0, S1|**Texterkennung** |Extrahiert gedruckten Text in Bildern von verschiedensten Objekten mit unterschiedlichen Oberflächen und Hintergründen, wie z.B. Belege, Poster, und Visitenkarten.<br/><br/>**Wichtig:** Der Texterkennungscontainer funktioniert derzeit nur für Englisch.<br>[Zugriff anfordern](Computer-vision/computer-vision-how-to-install-containers.md#request-access-to-the-private-container-registry)|
|[Gesicht](Face/face-how-to-install-containers.md) |F0, S0|**Gesicht** |Erkennt menschliche Gesichter in Bildern und identifiziert zugehörige Attribute wie Gesichtsmerkmale (z.B. Nasen und Augen), Geschlecht, Alter und andere vom Computer vorhergesagte Gesichtsmerkmale. Zusätzlich zur Erkennung kann die Gesichtserkennung über eine Zuverlässigkeitsbewertung überprüfen, ob zwei Gesichter in einem oder verschiedenen Bildern identisch sind, oder Gesichter mit einer Datenbank vergleichen, um festzustellen, ob ein ähnliches oder identisches Gesicht bereits vorhanden ist. Sie kann auch ähnliche Gesichter in Gruppen mit gemeinsamen Gesichtsmerkmalen organisieren.<br>[Zugriff anfordern](Face/face-how-to-install-containers.md#request-access-to-the-private-container-registry) |
|[LUIS](LUIS/luis-container-howto.md) |F0, S0|**LUIS** ([Image](https://go.microsoft.com/fwlink/?linkid=2043204))|Lädt Ihr trainiertes oder veröffentlichtes Language Understanding-Modell (auch als LUIS-App bezeichnet) in einen Docker-Container und ermöglicht den Zugriff auf die Abfragevorhersagen von den API-Endpunkten des Containers. Sie können Abfrageprotokolle vom Container erfassen und wieder in das [LUIS-Portal](https://www.luis.ai) hochladen, um die Vorhersagegenauigkeit der App zu verbessern.|
|[Textanalyse](text-analytics/how-tos/text-analytics-how-to-install-containers.md) |F0, S|**Schlüsselbegriffserkennung** ([Bild](https://go.microsoft.com/fwlink/?linkid=2018757)) |Extrahiert die Schlüsselbegriffe, um die wichtigsten Punkte zu ermitteln. Wenn der eingegebene Text beispielsweise „Das Essen war köstlich, und es gab hervorragendes Personal“ lautet, gibt die API die Kernpunkte „Essen“ und „hervorragendes Personal“ zurück. |
|[Textanalyse](text-analytics/how-tos/text-analytics-how-to-install-containers.md)|F0, S|**Sprachenerkennung** ([Bild](https://go.microsoft.com/fwlink/?linkid=2018759)) |Erkennt die Sprache von Eingabetexten für bis zu 120 Sprachen und meldet einen einzigen Sprachcode für jedes Dokument, das auf Anforderung gesendet wird. Der Sprachcode ist mit einem Wert kombiniert, der die Stärke der Bewertung angibt. |
|[Textanalyse](text-analytics/how-tos/text-analytics-how-to-install-containers.md)|F0, S|**Standpunktanalyse** ([Bild](https://go.microsoft.com/fwlink/?linkid=2018654)) |Analysiert unformatierten Text auf Hinweise auf positive oder negative Stimmungen. Die API gibt für jedes Dokument eine Bewertung des Standpunkts zwischen 0 und 1 zurück. Hierbei entspricht 1 einer positiven Bewertung. Die Analysemodelle sind mithilfe großer Textmengen und Microsoft-Technologien für natürliche Sprache vortrainiert. Für [ausgewählte Sprachen](./text-analytics/language-support.md) kann die API jeden bereitgestellten unformatierten Text analysieren und bewerten und der aufrufenden Anwendung direkt Ergebnisse zurückgeben. |

Darüber hinaus werden Container im [All-In-One-Angebot](https://azure.microsoft.com/pricing/details/cognitive-services/) von Cognitive Services unterstützt. Sie können eine einzelne All-In-One-Ressource der Cognitive Services erstellen und für alle oben genannten Containertypen denselben Abrechnungsschlüssel verwenden.

## <a name="container-availability-in-azure-cognitive-services"></a>Containerverfügbarkeit in Azure Cognitive Services

Azure Cognitive Services-Container sind über Ihr Azure-Abonnement öffentlich zugänglich, und Docker-Containerimages können entweder aus der Microsoft Container Registry oder dem Docker-Hub abgerufen werden. Verwenden Sie den Befehl [docker pull](https://docs.docker.com/engine/reference/commandline/pull/), um ein Containerimage aus dem entsprechenden Repository herunterzuladen.

> [!IMPORTANT]
> Derzeit müssen Sie einen Anmeldeprozess abschließen, um auf die Container [Gesichtserkennung](Face/face-how-to-install-containers.md) und [Texterkennung](Computer-vision/computer-vision-how-to-install-containers.md) zuzugreifen, in denen Sie einen Fragebogen mit Fragen zu Ihnen, Ihrem Unternehmen und dem Anwendungsfall, für den Sie die Container implementieren möchten, ausfüllen und einreichen. Sobald Sie Zugriff und Zugangsdaten erhalten haben, können Sie die Containerimages für die Container „Gesichtserkennung“ und „Texterkennung“ aus einer privaten Containerregistrierung abrufen, die von Azure Container Registry gehostet wird.

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen die folgenden Voraussetzungen erfüllen, bevor Sie die Container der Azure Cognitive Services verwenden können:

**Docker-Engine:** Die Docker-Engine muss lokal installiert sein. Docker stellt Pakete zur Konfiguration der Docker-Umgebung unter [macOS](https://docs.docker.com/docker-for-mac/), [Linux](https://docs.docker.com/engine/installation/#supported-platforms) und [Windows](https://docs.docker.com/docker-for-windows/) zur Verfügung. Unter Windows muss Docker für die Unterstützung von Linux-Containern konfiguriert werden. Docker-Container können auch direkt für [Azure Kubernetes Service](../aks/index.yml) oder [Azure Container Instances](../container-instances/index.yml) bereitgestellt werden.

Docker muss so konfiguriert werden, dass die Container eine Verbindung mit Azure herstellen und Abrechnungsdaten an Azure senden können.

**Kenntnisse von Microsoft Container Registry und Docker:** Sie sollten über Grundkenntnisse der Konzepte von Microsoft Container Registry und Docker verfügen, einschließlich Registrierungen, Repositorys, Container und Containerimages, und die grundlegenden `docker`-Befehle kennen.

Eine Einführung in Docker und Container finden Sie in der [Docker-Übersicht](https://docs.docker.com/engine/docker-overview/).

Einzelne Container können auch ihre eigenen Anforderungen haben, wie z.B. Anforderungen an die Server- und Speicherzuordnung.

## <a name="developer-samples"></a>Entwicklerbeispiele

Beispiele für Entwickler finden Sie in unserem [GitHub-Repository](https://github.com/Azure-Samples/cognitive-services-containers-samples).

## <a name="next-steps"></a>Nächste Schritte

Installieren und erkunden Sie die Funktionalität der Container in Azure Cognitive Services:

* [Installieren und Verwenden von Containern für maschinelles Sehen](Computer-vision/computer-vision-how-to-install-containers.md)
* [Installieren und Verwenden von Containern für die Gesichtserkennung](Face/face-how-to-install-containers.md)
* [Installieren und Verwenden von Containern für die Textanalyse](text-analytics/how-tos/text-analytics-how-to-install-containers.md)
* [Installieren und Verwenden von LUIS-Containern (Language Understanding)](LUIS/luis-container-howto.md)
