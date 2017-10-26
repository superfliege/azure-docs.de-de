---
title: Aktualisieren und Skalieren einer Azure API Management-Instanz | Microsoft-Dokumentation
description: In diesem Thema wird beschrieben, wie Sie eine Azure API Management-Instanz aktualisieren und skalieren.
services: api-management
documentationcenter: 
author: vladvino
manager: anneta
editor: 
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 08/17/2017
ms.author: apimpm
ms.openlocfilehash: 22cc917eb6f296724bf535e48b0dd6ba8927e5d3
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2017
---
# <a name="upgrade-and-scale-an-api-management-instance"></a>Aktualisieren und Skalieren einer API Management-Instanz 

Kunden können eine APIM-Instanz (API Management) skalieren, indem sie Einheiten hinzufügen und entfernen. Eine **Einheit** umfasst dedizierte Azure-Ressourcen und verfügt über eine bestimmte Lastkapazität, die als Anzahl von API-Aufrufen pro Monat ausgedrückt wird. Der tatsächliche Durchsatz und die Latenz variieren aufgrund der folgenden Faktoren erheblich: Anzahl und Übertragungsrate von gleichzeitigen Verbindungen, Anzahl und Art von konfigurierten Richtlinien, Umfang von Anforderungen und Antworten sowie Back-End-Latenzzeiten. 

Die Kapazität und der Preis der einzelnen Einheiten richtet sich nach dem **Tarif** der Einheit. Sie können zwischen drei Tarifen wählen: **Developer**, **Standard** und **Premium**. Wenn Sie die Kapazität für einen Dienst eines Tarifs erhöhen müssen, sollten Sie eine Einheit hinzufügen. Falls der Tarif, der in Ihrer APIM-Instanz derzeit ausgewählt ist, das Hinzufügen von weiteren Einheiten nicht zulässt, müssen Sie ein Upgrade auf einen höheren Tarif durchführen. 

Der Preis jeder Einheit, die Möglichkeit zum Hinzufügen bzw. Entfernen von Einheiten und das Vorhandensein bestimmter Features (z.B. Bereitstellung in mehreren Regionen) richtet sich nach dem Tarif, den Sie für Ihre APIM-Instanz gewählt haben. Im Artikel zu den [Preisdetails](https://azure.microsoft.com/pricing/details/api-management/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) wird beschrieben, welchen Preis pro Einheit und welche Features Sie pro Tarif erhalten. 

>[!NOTE]
>Im Artikel zu den [Preisdetails](https://azure.microsoft.com/pricing/details/api-management/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) sind ungefähre Werte für die Einheitenkapazität der einzelnen Tarife angegeben. Um noch genauere Werte zu erhalten, sollten Sie sich ein realistisches Szenario für Ihre APIs ansehen. Informationen hierzu finden Sie unten im Abschnitt „Planen der Kapazität“.

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen der in diesem Artikel beschriebenen Schritte benötigen Sie Folgendes:

+ Ein aktives Azure-Abonnement.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ Eine APIM-Instanz. Weitere Informationen finden Sie unter [Erstellen einer neuen Azure API Management-Dienstinstanz](get-started-create-service-instance.md).

## <a name="how-to-plan-for-capacity"></a>Wie kann die Kapazität geplant werden?

Testen Sie die zu erwartenden Workloads, um zu ermitteln, ob ausreichend Einheiten zur Bewältigung Ihres Datenverkehrs vorhanden sind. 

Wie oben bereits erwähnt: Die Anzahl von Anforderungen, die per APIM pro Sekunde verarbeitet werden können, hängt von vielen Variablen ab. Beispiele hierfür sind: das Verbindungsmuster, die Größe der Anforderung und Antwort, für die einzelnen APIs konfigurierte Richtlinien und die Anzahl von Clients, die Anforderungen senden.

Nutzen Sie **Metriken** (mit Verwendung von Azure Monitor-Funktionen), um zu verstehen, wie viel Kapazität jeweils verwendet wird.

### <a name="use-the-azure-portal-to-examine-metrics"></a>Verwenden des Azure-Portals zum Untersuchen von Metriken 

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) zu Ihrer APIM-Instanz.
2. Klicken Sie auf **Metriken**.
3. Wählen Sie unter **Verfügbare Metriken** die Metrik **Kapazität**. 

    Anhand der Metrik „Kapazität“ können Sie ermitteln, wie viel Kapazität in Ihrem Mandanten genutzt wird. Sie können beim Testen die Last immer weiter erhöhen, um herauszufinden, wie hoch Ihre Spitzenlast ist. Sie können eine Metrikwarnung festlegen, damit Sie benachrichtigt werden, wenn etwas Unerwartetes passiert. Beispiel: Ihre APIM-Instanz überschreitet die Kapazitätsgrenze mehr als fünf Minuten lang. 

    >[!TIP]
    > Sie können die Warnung so konfigurieren, dass Sie benachrichtigt werden, wenn die Kapazität für Ihren Dienst knapp wird, oder dass eine Logik-App aufgerufen wird, die durch das Hinzufügen einer Einheit automatisch eine Skalierung durchführt.

## <a name="upgrade-and-scale"></a>Aktualisieren und Skalieren 

Wie bereits erwähnt, können Sie zwischen drei Tarifen wählen: **Developer**, **Standard** und **Premium**. Der Tarif **Developer** ist zum Evaluieren des Diensts und nicht zur Verwendung in der Produktion bestimmt. Der Tarif **Developer** verfügt nicht über eine Vereinbarung zum Servicelevel (SLA) und ist nicht skalierbar (Einheiten hinzufügen/entfernen). 

Bei **Standard** und **Premium** handelt es sich um Tarife für die Produktion, die über eine Vereinbarung zum Servicelevel (SLA) verfügen und skaliert werden können. Im Tarif **Standard** kann eine Skalierung auf bis zu vier Einheiten durchgeführt werden. Im Tarif **Premium** können Sie eine beliebige Anzahl von Einheiten hinzufügen. 

Bei Verwendung des Tarifs **Premium** können Sie eine einzelne API Management-Instanz auf eine beliebige Anzahl von Azure-Regionen verteilen. Wenn Sie einen API Management-Dienst erstellen, enthält die Instanz nur eine Einheit und ist nur in einer Azure-Region angeordnet. Diese erste Region wird als **primäre** Region bezeichnet. Zusätzliche Regionen können leicht hinzugefügt werden. Beim Hinzufügen einer Region geben Sie die Anzahl von Einheiten an, die Sie zuordnen möchten. Beispielsweise können Sie eine Einheit in der **primären** Region und fünf Einheiten in einer anderen Region verwenden. Sie können diese Einrichtung je nach Datenverkehr in den einzelnen Regionen genau anpassen. Weitere Informationen finden Sie unter [Bereitstellen einer Azure API Management-Dienstinstanz für mehrere Azure-Regionen](api-management-howto-deploy-multi-region.md).

Sie können für alle Tarife ein Upgrade oder ein Downgrade durchführen. Beachten Sie Folgendes: Bei einem Downgrade können ggf. einige Features wegfallen, z.B. VNETs oder die Bereitstellung in mehreren Regionen bei Downgrades vom Premium-Tarif auf den Standard-Tarif.

>[!NOTE]
>Es kann zwischen 15 und 30 Minuten dauern, bis der Upgrade- bzw. Skalierungsprozess abgeschlossen ist. Sie erhalten nach Abschluss des Prozesses eine Benachrichtigung.

### <a name="use-the-azure-portal-to-upgrade-and-scale"></a>Verwenden des Azure-Portals zum Durchführen von Upgrades und Skalierungen

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) zu Ihrer APIM-Instanz.
2. Wählen Sie die Option **Skalierung und Preise**.
3. Wählen Sie den gewünschten Tarif aus.
4. Geben Sie die Anzahl von **Einheiten** an, die Sie hinzufügen möchten. Sie können entweder den Schieberegler verwenden oder die Anzahl von Einheiten eingeben.<br/>
    Wenn Sie den Tarif **Premium** wählen, müssen Sie zuerst eine Region auswählen.
5. Klicken Sie auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen einer Azure API Management-Dienstinstanz für mehrere Azure-Regionen](api-management-howto-deploy-multi-region.md)

