---
title: Was ist die Formularerkennung?
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie die Formularerkennung verwenden, um Formular- und Tabellendaten zu analysieren.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: overview
ms.date: 04/08/2019
ms.author: pafarley
ms.openlocfilehash: 2a120a59a58eb8d7a017cce0dd85c21038bdcf51
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65143217"
---
# <a name="what-is-form-recognizer"></a>Was ist die Formularerkennung?

Die Azure-Formularerkennung ist ein kognitiver Dienst, der mithilfe von Machine Learning-Technologie Schlüssel-Wert-Paare und Tabellendaten aus Formulardokumenten identifiziert und extrahiert. Anschließend werden strukturierte Daten ausgegeben, die die Beziehungen in der Originaldatei enthalten. Sie können Ihr benutzerdefiniertes Formularerkennungsmodell mithilfe einer einfachen REST-API aufrufen, um die Komplexität zu reduzieren und es einfach in Ihren Workflow oder Ihre Anwendung zu integrieren. Um zu beginnen, benötigen Sie nur fünf Formulardokumente oder ein leeres Formular desselben Typs wie Ihr Eingabematerial. Sie können schnell, präzise und auf Ihre spezifischen Inhalte zugeschnittene Ergebnisse erzielen, ohne dass komplizierte manuelle Eingriffe oder umfangreiche datenwissenschaftliche Kenntnisse erforderlich sind.

## <a name="request-access"></a>Zugriff anfordern
Die Formularerkennung ist als Vorschau mit eingeschränktem Zugriff verfügbar. Um Zugriff auf die Vorschau zu erhalten, füllen Sie das [Formular zum Anfordern des Zugriffs auf die Cognitive Services-Formularerkennung](https://aka.ms/FormRecognizerRequestAccess) aus, und reichen Sie es ein. Im Formular werden Informationen über Sie, Ihr Unternehmen und das Benutzerszenario angefordert, für das Sie die Formularerkennung verwenden möchten. Wenn Ihre Anforderung vom Azure Cognitive Services-Team genehmigt wird, erhalten Sie eine E-Mail mit Anweisungen für den Zugriff auf den Dienst.

## <a name="what-it-does"></a>Funktionsbeschreibung

Anhand der übermittelten Eingabedaten wird der Algorithmus trainiert: Er gruppiert die Formulare nach Typ, erkennt, welche Schlüssel und Tabellen vorhanden sind, und lernt, Schlüsseln Werte und Tabellen Einträge zuzuordnen. Nicht überwachtes Lernen ermöglicht es dem Modell, das Layout und die Beziehungen zwischen Feldern und Einträgen zu verstehen, und zwar ohne manuelle Datenbeschriftungen oder intensive Codierung und Verwaltung. Vorab trainierte Machine Learning-Modelle hingegen benötigen standardisierte Daten und sind weniger genau bei Eingabematerial, das von traditionellen Formaten abweicht, wie z.B. branchenspezifische Formulare.

Nachdem das Modell trainiert wurde, können Sie es testen, neu trainieren und schließlich verwenden, um Daten aus weiteren Formularen zuverlässig nach Ihren Bedürfnissen zu extrahieren.

## <a name="what-it-includes"></a>Lieferumfang

Die Formularerkennung ist als eine REST-API verfügbar. Sie können ein Modell erstellen, trainieren und bewerten, indem Sie die API aufrufen, und Sie können das Modell optional in einem lokalen Docker-Container ausführen.

## <a name="input-requirements"></a>Eingabeanforderungen

Die Formularerkennung arbeitet mit Eingabedokumenten, die die folgenden Anforderungen erfüllen:

* JPG-, PNG- oder PDF-Format (Text oder überprüft). In Text eingebettete PDFs sind vorzuziehen, da beim Extrahieren von Zeichen und Speicherort keine Fehler entstehen können.
* Dateigröße muss weniger als 4 MB betragen.
* Abmessungen für Bilder zwischen 50x50 und 4200x4200 Pixel
* Werden Papierdokumente eingescannt, sollten dies Scans mit hoher Qualität sein.
* Es muss das lateinische Alphabet (englische Zeichen) verwendet werden.
* Gedruckte (keine handschriftlichen) Daten
* Es müssen Schlüssel und Werte enthalten sein.
* Schlüssel können über oder links neben den Werten, aber nicht unter oder rechts davon angezeigt werden.

Darüber hinaus unterstützt die Formularerkennung die folgenden Eingabedatentypen noch nicht:

* Komplexe Tabellen (geschachtelte Tabellen, zusammengeführten Header oder Zellen usw.) 
* Kontrollkästchen oder Optionsfelder
* PDF-Dokumente mit mehr als 50 Seiten

## <a name="where-do-i-start"></a>Wo beginne ich?

**Schritt 1:** Erstellen Sie eine Formularerkennungsressource im Azure-Portal.

**Schritt 2:** Sammeln Sie praktische Erfahrungen mithilfe eines Schnellstarts:
* [Schnellstart: Trainieren eines Formularerkennungsmodells und Extrahieren von Formulardaten unter Verwendung der REST-API mit cURL](quickstarts/curl-train-extract.md)
* [Schnellstart: Trainieren eines Formularerkennungsmodells und Extrahieren von Formulardaten unter Verwendung der REST-API mit Python](quickstarts/python-train-extract.md)

Zu Lernzwecken sollten Sie den kostenlosen Dienst verwenden. Beachten Sie jedoch, dass die Anzahl kostenloser Seiten auf 500 Seiten pro Monat beschränkt ist.

**Schritt 3:** Überprüfen Sie, wie die REST-API die folgenden APIs zum Trainieren und Extrahieren strukturierter Daten aus Formularen verwendet.

| REST-API | BESCHREIBUNG |
|-----|-------------|
| Trainieren | Trainieren eines neuen Modells für die Analyse Ihrer Formulare mit fünf Formularen vom gleichen Typ oder einem leeren Formular.  |
| Analysieren  |Analysieren eines einzelnen Dokuments, das als Stream übergeben wird, um Schlüssel-Wert-Paare und Tabellen aus dem Formular mit Ihrem benutzerdefinierten Modell zu extrahieren.  |

Lesen Sie das [REST-API-Referenzdokument](https://aka.ms/form-recognizer/api). 

## <a name="data-privacy-and-security"></a>Datenschutz und Sicherheit

Der Dienst wird als [Vorschau](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) eines Azure-Diensts unter den [Nutzungsbedingungen für Onlineservice](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) bereitgestellt. Wie bei allen Cognitive Services-Diensten müssen Entwickler, die den Formularerkennungsdienst nutzen, die Microsoft-Richtlinien zu Kundendaten beachten. Weitere Informationen finden Sie im Microsoft Trust Center auf der [Seite zu Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices).

## <a name="next-steps"></a>Nächste Schritte

Befolgen Sie einen [Schnellstart](quickstarts/curl-train-extract.md) zu den ersten Schritten mit den [Formularerkennungs-APIs](https://aka.ms/form-recognizer/api).
