---
title: Java-App zur Erkennung von Anomalien – Microsoft Cognitive Services | Microsoft-Dokumentation
description: Lernen Sie eine Java-App kennen, die die API zur Anomalieerkennung in Microsoft Cognitive Services verwendet. Senden Sie originale Datenpunkte an die API, und rufen Sie den erwarteten Wert und Anomaliepunkte ab.
services: cognitive-services
author: wenya
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: wenya
ms.openlocfilehash: 228d440da358eba1322e2228c54f21e925e36ecd
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38597947"
---
# <a name="anomaly-detection-java-application"></a>Java-Anwendung zur Anomalieerkennung

Dieser Artikel veranschaulicht die Verwendung einer einfachen Java-Anwendung zum Aufrufen der Anomalieerkennungs-API.  
Im Beispiel werden die Zeitreihendaten mit Ihrem Abonnementschlüssel an die Anomalieerkennungs-API übermittelt, anschließend werden alle Anomaliepunkte und der erwartete Wert für jeden Datenpunkt bei der API abgerufen.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="platform-requirements"></a>Plattformanforderungen

Dieses Tutorial wurde mit [IntelliJ IDEA](https://www.jetbrains.com/idea) entwickelt. Sie müssen darüber hinaus das [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/index.html), Version 1.8+, und ein aktuelles [Apache Maven](http://maven.apache.org/)-Buildtool installieren.

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Abonnieren der Anomalieerkennung und Abrufen eines Abonnementschlüssels 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]
 

## <a name="download-the-tutorial-project"></a>Herunterladen des Tutorialprojekts

1. Navigieren Sie zum MicrosoftAnomalyDetection-[Java-Repository](https://github.com/MicrosoftAnomalyDetection/java-sample).
2. Klicken Sie auf die Schaltfläche „Clone“ (Klonen) oder „Download“ (Herunterladen).
3. Klicken Sie auf „Download ZIP“ (ZIP-Datei herunterladen), um eine ZIP-Datei des Tutorialprojekts herunterzuladen.

<a name="Step1"></a>
### <a name="open-the-tutorial-project"></a>Öffnen des Tutorialprojekts

1. Extrahieren Sie die ZIP-Datei des Tutorialprojekts.
2. Klicken Sie in IntelliJ IDEA auf **File > Open** (Datei > Öffnen), das Dialogfeld „Open File“ (Datei öffnen) oder „Project“ (Projekt) wird angezeigt.
3. Wählen Sie den Stammpfad des extrahierten Projekts aus, und klicken Sie auf „OK“.
4. Erweitern Sie im Bereich „Projects“ (Projekte) **src > main > java**.
5. Doppelklicken Sie auf „com.microsoft.cognitiveservice.anomalydetection.Main.java“, um die Datei in den Editor zu laden.

<a name="Step2"></a>
### <a name="replace-subscriptionkey-and-uri-region"></a>Ersetzen Sie die Werte für „subscriptionKey“ und „URI region“

```
// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
public static final String subscriptionKey = "<Subscription Key>";

public static final String uriBase = "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection";

```

<a name="Step3"></a>
### <a name="build-and-run-the-tutorial-project"></a>Erstellen und Ausführen des Tutorialprojekts

1. Rufen Sie das Menü auf, indem Sie auf der com.microsoft.cognitiveservice.anomalydetection.Main.java-Quellcode-Registerkarte an beliebiger Stelle mit der rechten Maustaste klicken. 
2. Wählen Sie „Run 'Main.main()'“ aus
3. Das Ergebnis der Beispielanforderung wird zurückgegeben und im Terminal angezeigt.

### <a name="result-of-the-tutorial-project"></a>Ergebnis des Tutorialprojekts

[!INCLUDE [diagrams](../includes/diagrams.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [REST-API-Referenz](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
