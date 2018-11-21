---
title: Containerunterstützung in Azure Cognitive Services
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Docker-Container Cognitive Services näher an Ihre Daten bringen können.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.topic: article
ms.date: 11/14/2018
ms.author: diberry
ms.openlocfilehash: b66d6bc93f9d2df3eccabf0f0f9edb0384bab70f
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51634910"
---
# <a name="container-support-in-azure-cognitive-services"></a>Containerunterstützung in Azure Cognitive Services

Mithilfe der Containerunterstützung in Azure Cognitive Services können Entwickler dieselben umfassenden APIs wie in Azure verwenden, haben dabei aber die Flexibilität von [Docker-Containern](https://www.docker.com/what-container). Die Containerunterstützung ist derzeit als Vorschauversion für einige Azure Cognitive Services verfügbar, einschließlich [Maschinelles Sehen](Computer-vision/Home.md), [Gesichtserkennung](Face/Overview.md) und [Textanalyse](text-analytics/overview.md).

Die Verwendung von Containern ist ein Ansatz zur Softwareverteilung, bei dem eine Anwendung oder ein Dienst, einschließlich der Abhängigkeiten und der Konfiguration, als Containerimage zusammengepackt wird. Das Containerimage kann mit geringer oder ganz ohne Bearbeitung auf einem Containerhost bereitgestellt werden. Container sind voneinander und vom zugrunde liegenden Betriebssystem isoliert und nehmen weniger Speicher in Anspruch als ein virtueller Computer. Container können für kurzfristige Aufgaben über Containerimages instanziiert und wieder entfernt werden, wenn sie nicht mehr benötigt werden.

Sehen Sie sich zur Demonstration dieses [Kurzvideo](https://azure.microsoft.com/resources/videos/containers-support-of-cognitive-services) an.

Die Dienste [Maschinelles Sehen](Computer-vision/Home.md), [Gesichtserkennung](Face/Overview.md) und [Textanalyse](text-analytics/overview.md) stehen in [Microsoft Azure](https://azure.microsoft.com) zur Verfügung. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, um Azure-Ressourcen für diese Dienste zu erstellen und zu erkunden.

## <a name="features-and-benefits"></a>Features und Vorteile

- **Kontrolle über Daten**: ermöglicht es Kunden, Cognitive Services mit vollständiger Kontrolle über ihre Daten zu verwenden.  Dies ist wichtig für Kunden, die keine Daten in die Cloud senden können, aber auf die Cognitive Services-Technologie zugreifen müssen. Einheitliche Unterstützung in Hybridumgebungen – für Daten, Verwaltung, Identität und Sicherheit.
- **Kontrolle über Modellaktualisierungen**: bietet Kunden Flexibilität bei der Versionsverwaltung und Aktualisierung der in ihren Lösungen eingesetzten Modelle.
- **Portable Architektur**: ermöglicht die Erstellung einer portablen Anwendungsarchitektur, die in der Cloud, vor Ort und am Edge eingesetzt werden kann. Container können auch direkt für [Azure Kubernetes Service](/azure/aks/), [Azure Container Instances](/azure/container-instances/) oder einen [Kubernetes](https://kubernetes.io/)-Cluster mit Bereitstellung in [Azure Stack](/azure/azure-stack/) bereitgestellt werden. Weitere Informationen finden Sie unter [Bereitstellen von Kubernetes in Azure Stack](/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy).
- **Hoher Durchsatz/niedrige Latenz**: bietet Kunden die Möglichkeit, für hohe Durchsatzraten und niedrige Latenzanforderungen zu skalieren, indem Cognitive Services im Azure Kubernetes Service physisch in der Nähe ihrer Anwendungslogik und -daten ausgeführt werden.


## <a name="containers-in-azure-cognitive-services"></a>Container in Azure Cognitive Services

Azure Cognitive Services-Container bieten den folgenden Satz von Docker-Containern, von denen jeder eine Teilmenge der Funktionalität von Diensten in Azure Cognitive Services enthält:

| Dienst | Container| BESCHREIBUNG |
|---------|----------|-------------|
|[Maschinelles Sehen](Computer-vision/computer-vision-how-to-install-containers.md) |**Texterkennung** |Extrahiert gedruckten Text in Bildern von verschiedensten Objekten mit unterschiedlichen Oberflächen und Hintergründen, wie z.B. Belege, Poster, und Visitenkarten.<br/><br/>**Wichtig**: Der Texterkennungscontainer funktioniert derzeit nur für Englisch.<br>[Zugriff anfordern](Computer-vision/computer-vision-how-to-install-containers.md#request-access-to-the-private-container-registry)|
|[Gesichtserkennung](Face/face-how-to-install-containers.md) |**Gesichtserkennung** |Erkennt menschliche Gesichter in Bildern und identifiziert zugehörige Attribute wie Gesichtsmerkmale (z.B. Nasen und Augen), Geschlecht, Alter und andere vom Computer vorhergesagte Gesichtsmerkmale. Zusätzlich zur Erkennung kann die Gesichtserkennung über eine Zuverlässigkeitsbewertung überprüfen, ob zwei Gesichter in einem oder verschiedenen Bildern identisch sind, oder Gesichter mit einer Datenbank vergleichen, um festzustellen, ob ein ähnliches oder identisches Gesicht bereits vorhanden ist. Sie kann auch ähnliche Gesichter in Gruppen mit gemeinsamen Gesichtsmerkmalen organisieren.<br>[Zugriff anfordern](Face/face-how-to-install-containers.md#request-access-to-the-private-container-registry) |
|[Textanalyse](text-analytics/how-tos/text-analytics-how-to-install-containers.md) |**Schlüsselbegriffserkennung** ([Bild](https://go.microsoft.com/fwlink/?linkid=2018757)) |Extrahiert die Schlüsselbegriffe, um die wichtigsten Punkte zu ermitteln. Wenn der eingegebene Text beispielsweise „Das Essen war köstlich, und es gab hervorragendes Personal“ lautet, gibt die API die Kernpunkte „Essen“ und „hervorragendes Personal“ zurück. |
|[Textanalyse](text-analytics/how-tos/text-analytics-how-to-install-containers.md)|**Sprachenerkennung** ([Bild](https://go.microsoft.com/fwlink/?linkid=2018759)) |Erkennt die Sprache von Eingabetexten für bis zu 120 Sprachen und meldet einen einzigen Sprachcode für jedes Dokument, das auf Anforderung gesendet wird. Der Sprachcode ist mit einem Wert kombiniert, der die Stärke der Bewertung angibt. |
|[Textanalyse](text-analytics/how-tos/text-analytics-how-to-install-containers.md)|**Standpunktanalyse** ([Bild](https://go.microsoft.com/fwlink/?linkid=2018654)) |Analysiert unformatierten Text auf Hinweise auf positive oder negative Stimmungen. Die API gibt für jedes Dokument eine Bewertung des Standpunkts zwischen 0 und 1 zurück. Hierbei entspricht 1 einer positiven Bewertung. Die Analysemodelle sind mithilfe großer Textmengen und Microsoft-Technologien für natürliche Sprache vortrainiert. Für [ausgewählte Sprachen](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages.md) kann die API jeden bereitgestellten unformatierten Text analysieren und bewerten und der aufrufenden Anwendung direkt Ergebnisse zurückgeben. |

## <a name="container-availability-in-azure-cognitive-services"></a>Containerverfügbarkeit in Azure Cognitive Services

Azure Cognitive Services-Container sind über Ihr Azure-Abonnement öffentlich zugänglich, und Docker-Containerimages können entweder aus der Microsoft Container Registry oder dem Docker-Hub abgerufen werden. Verwenden Sie den Befehl [docker pull](https://docs.docker.com/engine/reference/commandline/pull/), um ein Containerimage aus dem entsprechenden Repository herunterzuladen.

> [!IMPORTANT]
> Derzeit müssen Sie einen Anmeldeprozess abschließen, um auf die Container [Gesichtserkennung](Face/face-how-to-install-containers.md) und [Texterkennung](Computer-vision/computer-vision-how-to-install-containers.md) zuzugreifen, in denen Sie einen Fragebogen mit Fragen zu Ihnen, Ihrem Unternehmen und dem Anwendungsfall, für den Sie die Container implementieren möchten, ausfüllen und einreichen. Sobald Sie Zugriff und Zugangsdaten erhalten haben, können Sie die Containerimages für die Container „Gesichtserkennung“ und „Texterkennung“ aus einer privaten Containerregistrierung abrufen, die von Azure Container Registry gehostet wird.

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen die folgenden Voraussetzungen erfüllen, bevor Sie die Container der Azure Cognitive Services verwenden können:

**Docker-Engine:** Die Docker-Engine muss lokal installiert sein. Docker stellt Pakete zur Konfiguration der Docker-Umgebung unter [macOS](https://docs.docker.com/docker-for-mac/), [Linux](https://docs.docker.com/engine/installation/#supported-platforms) und [Windows](https://docs.docker.com/docker-for-windows/) zur Verfügung. Unter Windows muss Docker für die Unterstützung von Linux-Containern konfiguriert werden. Docker-Container können auch direkt für [Azure Kubernetes Service](/azure/aks/) oder [Azure Container Instances](/azure/container-instances/) bereitgestellt werden.

Docker muss so konfiguriert werden, dass die Container eine Verbindung mit Azure herstellen und Abrechnungsdaten an Azure senden können.

**Kenntnisse von Microsoft Container Registry und Docker:** Sie sollten über Grundkenntnisse der Konzepte von Microsoft Container Registry und Docker, einschließlich Registrierungen, Repositorys, Container und Containerimages, verfügen und die grundlegenden `docker`-Befehle kennen.  

Eine Einführung in Docker und Container finden Sie in der [Docker-Übersicht](https://docs.docker.com/engine/docker-overview/).

Einzelne Container können auch ihre eigenen Anforderungen haben, wie z.B. Anforderungen an die Server- und Speicherzuordnung.

## <a name="developer-samples"></a>Entwicklerbeispiele

Beispiele für Entwickler finden Sie in unserem [Github-Repository](https://github.com/Azure-Samples/cognitive-services-containers-samples). 

## <a name="next-steps"></a>Nächste Schritte

Installieren und erkunden Sie die Funktionalität der Container in Azure Cognitive Services:

* [Installieren und Verwenden von Containern für maschinelles Sehen](Computer-vision/computer-vision-how-to-install-containers.md)
* [Installieren und Verwenden von Containern für die Gesichtserkennung](Face/face-how-to-install-containers.md)
* [Installieren und Verwenden von Containern für die Texterkennung](text-analytics/how-tos/text-analytics-how-to-install-containers.md)