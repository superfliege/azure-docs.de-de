---
title: Problembehandlung für häufige Fehler
description: Erfahren Sie mehr über die Problembehandlung durch die Erstellung und Zuweisung von Blaupausen
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/25/2018
ms.topic: troubleshooting
ms.service: blueprints
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 40668fed2fcc2a04e39fa3a4d7e8e8923c75ae05
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53315523"
---
# <a name="troubleshoot-errors-using-azure-blueprints"></a>Problembehandlung mit Azure Blueprints

Beim Erstellen oder Zuweisen von Blaupausen können Probleme auftreten. In diesem Artikel wird beschrieben, welche Fehler auftreten und wie diese behoben werden können.

## <a name="finding-error-details"></a>Ermitteln von Fehlerdetails

Viele Fehler sind das Ergebnis der Zuweisung einer Blaupause zu einem Bereich. Wenn bei einer Zuweisung ein Fehler auftritt, finden Sie in der Blaupause Informationen zur fehlerhaften Bereitstellung. Diese Informationen enthalten Hinweise zum Problem, damit Sie es beheben können und die nächste Bereitstellung erfolgreich verläuft.

1. Klicken Sie auf **Alle Dienste**, suchen Sie im linken Bereich nach der Option **Richtlinie**, und wählen Sie sie aus. Klicken Sie auf der Seite **Richtlinie** auf **Blaupausen**.

1. Wählen Sie **Zugewiesene Blaupausen** auf der linken Seite und verwenden Sie das Suchfeld, um die Blaupausenzuweisungen zu filtern und die fehlerhafte Zuweisung zu finden. Sie können die Tabelle der Zuweisungen auch nach der Spalte **Bereitstellungsstatus** sortieren, um alle fehlerhaften Zuweisungen gruppiert anzuzeigen.

1. Klicken Sie mit der linken Maustaste auf die Blaupause mit dem Status _Fehlerhaft_, oder klicken Sie mit der rechten Maustaste und wählen Sie **Zuweisungsdetails anzeigen**.

1. In einem roten Banner oben auf der Seite „Blaupausenzuweisung“ wird darauf hingewiesen, dass die Zuweisung fehlerhaft ist. Klicken Sie auf eine beliebige Stelle auf das Banner, um weitere Details anzuzeigen.

Es ist üblich, dass der Fehler durch ein Artefakt und nicht durch die Blaupause als Ganzes verursacht wird. Wenn ein Artefakt eine Key Vault-Instanz erstellt und Azure Policy die Key Vault-Erstellung verhindert, tritt für die gesamte Zuweisung ein Fehler auf.

## <a name="general-errors"></a>Allgemeine Fehler

### <a name="policy-violation"></a>Szenario: Richtlinienverletzung

#### <a name="issue"></a>Problem

Bei der Vorlagenbereitstellung ist aufgrund einer Richtlinienverletzung ein Fehler aufgetreten.

#### <a name="cause"></a>Ursache

Eine Richtlinie kann aus verschiedenen Gründen einer Bereitstellung widersprechen:

- Die zu erstellende Ressource ist durch Richtlinien eingeschränkt (üblicherweise SKU- oder Standortbeschränkungen).
- Die Bereitstellung legt Felder fest, die über die Richtlinie konfiguriert werden (häufig mit Tags).

#### <a name="resolution"></a>Lösung

Ändern Sie die Blaupause so, dass sie nicht in Konflikt mit den Richtlinien in den Fehlerdetails steht. Wenn diese Änderung nicht möglich ist, besteht eine alternative Option darin, den Umfang der Richtlinienzuordnung so zu ändern, dass die Blaupause der Richtlinie nicht mehr widerspricht.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

- Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
- Mit [@AzureSupport](https://twitter.com/azuresupport) verbinden – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit durch Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.
- Wenn Sie weitere Hilfe benötigen, können Sie einen Azure-Supportvorgang anlegen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.