---
title: Problembehandlung mit Azure Blueprints
description: Erfahren Sie mehr über die Problembehandlung durch die Erstellung und Zuweisung von Blaupausen
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: dd1163ece225c2e9a9b082f5e8364f34b06a10ae
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46982289"
---
# <a name="troubleshoot-errors-using-azure-blueprints"></a>Problembehandlung mit Azure Blueprints

Beim Erstellen oder Zuweisen von Blaupausen können Probleme auftreten. In diesem Artikel wird beschrieben, welche Fehler auftreten und wie diese behoben werden können.

## <a name="finding-error-details"></a>Ermitteln von Fehlerdetails

Viele Fehler sind das Ergebnis der Zuweisung einer Blaupause zu einem Bereich. Wenn bei einer Zuweisung ein Fehler auftritt, finden Sie in Blueprint Informationen zur fehlerhaften Bereitstellung. Diese Informationen zeigen das Problem an, sodass es behoben werden kann und die nachfolgende Bereitstellung erfolgreich ist.

1. Starten Sie den Azure Blueprint-Dienst im Azure-Portal. Klicken Sie dazu auf **Alle Dienste**, und suchen Sie im linken Bereich nach **Richtlinie**. Klicken Sie auf der Seite **Richtlinie** auf **Blaupausen**.

1. Wählen Sie **Zugewiesene Blaupausen** auf der linken Seite und verwenden Sie das Suchfeld, um die Blaupausenzuweisungen zu filtern und die fehlerhafte Zuweisung zu finden. Sie können die Tabelle der Zuweisungen auch nach der Spalte **Bereitstellungsstatus** sortieren, um alle fehlerhaften Zuweisungen gruppiert anzuzeigen.

1. Klicken Sie mit der linken Maustaste auf die Blaupause mit dem Status _Fehlerhaft_, oder klicken Sie mit der rechten Maustaste und wählen Sie **Zuweisungsdetails anzeigen**.

1. Oben auf der Seite „Blaupausenzuweisung“ befindet sich ein rotes Banner, das darauf hinweist, dass die Zuweisung fehlerhaft ist. Klicken Sie auf eine beliebige Stelle auf das Banner, um weitere Details anzuzeigen.

Es ist üblich, dass der Fehler durch ein Artefakt in der Blaupause und nicht durch die Blaupause als Ganzes verursacht wird. Wenn die Blaupause beispielsweise ein Artefakt zum Erstellen eines Schlüsseltresors enthält, die Erstellung des Schlüsseltresors jedoch durch die Azur Policy verhindert wird, schlägt die gesamte Zuweisung fehl.

## <a name="general-errors"></a>Allgemeine Fehler

### <a name="policy-violation"></a>Szenario: Richtlinienverletzung

#### <a name="issue"></a>Problem

Bei der Vorlagenbereitstellung ist aufgrund einer Richtlinienverletzung ein Fehler aufgetreten.

#### <a name="cause"></a>Ursache

Eine Richtlinie kann aus verschiedenen Gründen einer Bereitstellung widersprechen:

- Die zu erstellende Ressource ist durch Richtlinien eingeschränkt (üblicherweise SKU- oder Standortbeschränkungen).
- Die Bereitstellung legt Felder fest, die über die Richtlinie konfiguriert werden (häufig mit Tags).

#### <a name="resolution"></a>Lösung

Passen Sie die Blaupause so an, dass sie nicht im Widerspruch zu den in den Fehlerinformationen aufgeführten Richtlinien steht. Wenn dies nicht möglich ist, besteht eine alternative Option darin, den Umfang der Richtlinienzuordnung so zu ändern, dass die Blaupause nicht mehr der Richtlinie widerspricht.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

- Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
- Mit [@AzureSupport](https://twitter.com/azuresupport) verbinden – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit durch Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.
- Wenn Sie weitere Hilfe benötigen, können Sie einen Azure-Supportvorgang anlegen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.