---
title: Migrieren vom Custom Speech Service zum Speech-Dienst
titlesuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie vom Custom Speech Service zum Speech-Dienst migrieren.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: panosper
ms.openlocfilehash: 656a5df21d1f7dd2e7662dc3a0415581eae02b00
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49471236"
---
# <a name="migrate-from-the-custom-speech-service-to-the-speech-service"></a>Migrieren vom Custom Speech Service zum Speech-Dienst

Verwenden Sie diesen Artikel, um Ihre Anwendungen vom Custom Speech Service zum Speech-Dienst zu migrieren.

Der Custom Speech Service ist nun Teil des Speech-Diensts. Wechseln Sie zum Speech-Dienst, um von den neuesten Updates zu Qualität und Features zu profitieren.
 
## <a name="migration-for-new-customers"></a>Migration für Neukunden

Das Preismodell ist einfacher, da Sie zu einem stundenbasierten Preismodell für den Speech-Dienst wechseln.   

1. Erstellen Sie eine Azure-Ressource in jeder Region, in der Ihre Anwendung verfügbar ist. Der Name der Azure-Ressource lautet **Speech**. Sie können eine einzelne Azure-Ressource für die folgenden Dienste in der gleichen Region verwenden, anstatt separate Ressourcen zu erstellen:

    * Spracherkennung
    * Benutzerdefinierte Spracherkennung
    * Text-zu-Sprache
    * Sprachübersetzung

2. Laden Sie das [Spracherkennungs-SDK](speech-sdk.md) herunter. 

3. Befolgen Sie die Schnellstarts und die SDK-Beispiele, um die richtigen APIs zu verwenden. Wenn Sie die REST-APIs verwenden, müssen Sie auch die richtigen Endpunkte und Ressourcenschlüssel verwenden. 

4. Aktualisieren Sie die Clientanwendung, um den Speech-Dienst und die APIs zu verwenden. 

> [!NOTE]
> * LUIS: Wenn Sie Speech in Language Understanding (LUIS) aktiviert haben, funktioniert eine einzelne LUIS-Ressource in derselben Region für LUIS und den Speech-Dienst. Lesen Sie die Dokumentation zur [Absichtserkennung anhand von Sprache](how-to-recognize-intents-from-speech-csharp.md).
> * Die Textübersetzung ist nicht Teil des Speech-Diensts. Dafür benötigen Sie ein eigenes Azure-Ressourcenabonnement.
  


## <a name="migration-for-existing-customers"></a>Migration für Bestandskunden

Bestandskunden müssen ihre vorhandenen Ressourcenschlüssel im Speech-Dienst-Portal zum Speech-Dienst migrieren. Führen Sie die folgenden Schritte aus: 

> [!NOTE] 
> Ressourcenschlüssel können nur innerhalb derselben Region migriert werden. 

1. Melden Sie sich im Portal [cris.ai](http://www.cris.ai) an, und wählen Sie das Abonnement im Menü rechts oben aus. 

2. Wählen Sie **Migrate selected subscription** (Ausgewähltes Abonnement migrieren) aus.

3. Geben Sie den Abonnementschlüssel in das Textfeld ein, und wählen Sie **Migrieren** aus.

## <a name="next-steps"></a>Nächste Schritte

* [Kostenloses Testen des Spracherkennungsdiensts](get-started.md)
* Lernen Sie die Konzepte der [Spracherkennung](./speech-to-text.md) kennen.

## <a name="see-also"></a>Weitere Informationen

* [Worum handelt es sich beim Spracherkennungsdienst?](overview.md)
* [Spracherkennungsdienst und SDK: Dokumentation](speech-sdk.md#get-the-sdk)