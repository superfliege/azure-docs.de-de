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
ms.openlocfilehash: 7cb8c992b4c131b7f28eca6c2f35ee9facdf8d4e
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50416102"
---
# <a name="migrate-from-the-custom-speech-service-to-the-speech-service"></a>Migrieren vom Custom Speech Service zum Speech-Dienst

Verwenden Sie die Informationen in diesem Artikel, um Ihre Anwendungen vom Custom Speech Service zum Speech-Dienst zu migrieren.

Der Custom Speech Service ist nun Teil des Speech-Diensts. Wechseln Sie zum Speech-Dienst, um von den neuesten Updates zu Qualität und Features zu profitieren.
 
## <a name="migration-for-new-customers"></a>Migration für Neukunden

Das Preismodell ist einfacher, da für den Speech-Dienst ein stundenbasiertes Preismodell verwendet wird.   

1. Erstellen Sie eine Azure-Ressource in jeder Region, in der Ihre Anwendung verfügbar ist. Der Name der Azure-Ressource lautet **Speech**. Sie können eine einzelne Azure-Ressource für die folgenden Dienste in der gleichen Region verwenden, anstatt separate Ressourcen zu erstellen:

    * Spracherkennung
    * Benutzerdefinierte Spracherkennung
    * Text-zu-Sprache
    * Sprachübersetzung

2. Laden Sie das [Spracherkennungs-SDK](speech-sdk.md) herunter. 

3. Befolgen Sie die Schnellstarts und die SDK-Beispiele, um die richtigen APIs zu verwenden. Wenn Sie die REST-APIs verwenden, müssen Sie auch die richtigen Endpunkte und Ressourcenschlüssel verwenden. 

4. Aktualisieren Sie die Clientanwendung, um den Speech-Dienst und die APIs zu verwenden. 

> [!NOTE]
> * Wenn Sie Speech in Language Understanding (LUIS) aktiviert haben, funktioniert eine einzelne LUIS-Ressource in derselben Region für LUIS und den Speech-Dienst. Weitere Informationen finden Sie unter [Erkennen von Absichten anhand von gesprochener Sprache](how-to-recognize-intents-from-speech-csharp.md).
> * Die Textübersetzung ist nicht Teil des Speech-Diensts. Diese Funktionalität erfordert ein eigenes Azure-Ressourcenabonnement.
  


## <a name="migration-for-existing-customers"></a>Migration für Bestandskunden

Migrieren Sie Ihre vorhandenen Ressourcenschlüssel im Speech-Dienst-Portal zum Speech-Dienst. Führen Sie die folgenden Schritte aus: 

> [!NOTE] 
> Ressourcenschlüssel können nur innerhalb derselben Region migriert werden. 

1. Melden Sie sich im Portal [cris.ai](http://www.cris.ai) an, und wählen Sie das Abonnement im Menü rechts oben aus. 

2. Wählen Sie **Migrate selected subscription** (Ausgewähltes Abonnement migrieren) aus.

3. Geben Sie den Abonnementschlüssel in das Textfeld ein, und wählen Sie **Migrieren** aus.

## <a name="next-steps"></a>Nächste Schritte

* [Testen Sie den Speech-Dienst kostenlos](get-started.md).
* Lernen Sie die Konzepte der [Spracherkennung](./speech-to-text.md) kennen.

## <a name="see-also"></a>Weitere Informationen

* [Worum handelt es sich beim Speech-Dienst?](overview.md)
* [Spracherkennungsdienst und SDK: Dokumentation](speech-sdk.md#get-the-sdk)