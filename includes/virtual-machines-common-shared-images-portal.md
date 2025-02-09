---
title: Includedatei
description: Includedatei
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 04/29/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 66ec923f12c59d606056d48c09f1ba346e9a4d32
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66170387"
---
## <a name="sign-in-to-azure"></a>Anmelden bei Azure 

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

> [!NOTE]
> Wenn Sie sich bei der Vorschauversion für die Verwendung des Katalogs mit freigegebenen Images registriert haben, müssen Sie den `Microsoft.Compute`-Anbieter möglicherweise erneut registrieren. Öffnen Sie [Cloud Shell](https://shell.azure.com/bash) und geben Sie ein: `az provider register -n Microsoft.Compute`.

## <a name="create-an-image-gallery"></a>Erstellen eines Imagekatalogs

Ein Imagekatalog ist die primäre Ressource, die zur Ermöglichung des Teilens von Images verwendet wird. Zulässige Zeichen für Katalognamen sind Groß- und Kleinbuchstaben, Zahlen und Punkte. Der Katalogname darf keine Bindestriche enthalten.  Katalognamen müssen innerhalb Ihres Abonnements eindeutig sein. 

Im folgenden Beispiel wird der Katalog *myGallery* in der Ressourcengruppe *myGalleryRG* erstellt.

1. Klicken Sie im Azure-Portal links oben auf **Ressource erstellen**.
1. Geben Sie in das Suchfeld **Katalog mit freigegebenen Images** ein und wählen Sie in den Ergebnissen **Katalog mit freigegebenen Images** aus.
1. Klicken Sie auf der Seite **Katalog mit freigegebenen Images** auf **Erstellen**.
1. Wählen Sie das richtige Abonnement aus.
1. Wählen Sie in **Ressourcengruppe** **Neu erstellen** aus, und geben Sie *myGalleryRG* für den Namen ein.
1. Geben Sie unter **Name** *myGallery* als Namen des Katalogs ein.
1. Übernehmen Sie den Standardwert für **Region**.
1. Sie können eine kurze Beschreibung des Katalogs eingeben, wie *Mein Imagekatalog zum Testen*. Klicken Sie dann auf **Überprüfen + Erstellen**.
1. Wenn die Überprüfung erfolgreich war, wählen Sie **Erstellen** aus.
1. Wählen Sie nach Abschluss der Bereitstellung die Option **Zu Ressourcengruppe wechseln**.
   
## <a name="create-an-image-definition"></a>Erstellen einer Imagedefinition 

Imagedefinitionen erstellen eine logische Gruppierung von Images. Sie werden verwendet, um Informationen über die Imageversionen zu verwalten, die in ihnen erstellt werden. Namen für Imagedefinition können aus Groß- und Kleinbuchstaben, Zahlen, Punkten und (Binde)Strichen bestehen. Weitere Informationen zu den Werten, die Sie für eine Imagedefinition angeben können, finden Sie unter [Imagedefinitionen](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries#image-definitions).

Erstellen Sie die Definition des Katalogimages innerhalb Ihres Katalogs. In diesem Beispiel heißt das Katalogimage *myImageDefinition*.

1. Wählen Sie auf der Seite Ihres neuen Imagekatalogs **Neue Imagedefinition hinzufügen** oben auf der Seite aus. 
1. Geben Sie für **Name der Imagedefinition** *myImageDefinition* ein.
1. Wählen Sie für **Betriebssystem** die richtige Option basierend auf Ihrem Quellimage.
1. Geben Sie für **Herausgeber** *myPublisher* ein. 
1. Geben Sie für **Angebot** *myOffer* ein.
1. Geben Sie für **SKU** *mySKU* ein.
1. Wenn Sie fertig sind, wählen Sie **Überprüfen und erstellen** aus.
1. Nachdem die Imagedefinition erfolgreich überprüft wurde, wählen Sie **Erstellen**.
1. Wählen Sie nach Abschluss der Bereitstellung die Option **Zu Ressourcengruppe wechseln**.



## <a name="create-an-image-version"></a>Erstellen einer Imageversion

Erstellen Sie eine Imageversion aus einem verwalteten Image. In diesem Beispiel lautet die Imageversion *1.0.0*. Sie wird in den Rechenzentren *USA, Westen-Mitte* und *USA, Süden-Mitte* repliziert. Bei der Auswahl der Zielregionen für die Replikation ist zu beachten, dass Sie auch die Region *Quelle* als Ziel für die Replikation angeben müssen.

Zulässige Zeichen für die Imageversion sind Zahlen und Punkte. Zahlen müssen im Bereich einer ganzen 32-Bit-Zahl liegen. Format: *MajorVersion*.*MinorVersion*.*Patch*.

1. Wählen Sie oben auf der Seite für Ihre Imagedefinition **Version hinzufügen** aus.
1. Wählen Sie unter **Region** die Region, in der Ihr verwaltetes Image gespeichert ist. Imageversionen müssen in der gleichen Region erstellt werden wie das verwaltete Image, aus dem sie erstellt wurden.
1. Geben Sie für **Name** *1.0.0* ein. Der Name der Imageversion sollte dem Format *major*.*minor*.*patch* mit ganzen Zahlen entsprechen. 
1. Wählen Sie unter **Quellimage** aus der Dropdownliste das von der Quelle verwaltete Image aus.
1. Übernehmen Sie für **Aus aktueller Version ausschließen** den Standardwert *Nein*.
1. Wählen Sie unter **Datum für Ende des Lebenszyklus** ein Datum aus dem Kalender aus, das einige Monate in der Zukunft liegt.
1. Belassen Sie in **Replikation** die **Anzahl von Standardreplikaten** bei 1. Die Replikation muss in den Quellbereich ausgeführt werden. Also belassen Sie das erste Replikat als Standard bei und wählen Sie dann einen zweiten Replikatbereich als *USA, Osten* aus.
1. Wählen Sie abschließend **Überprüfen + Erstellen** aus. Azure überprüft die Konfiguration.
1. Nachdem die Imageversion erfolgreich überprüft wurde, wählen Sie **Erstellen**.
1. Wählen Sie nach Abschluss der Bereitstellung die Option **Zu Ressourcengruppe wechseln**.

Es kann eine Weile dauern, bis das Image in alle Zielregionen repliziert ist.
