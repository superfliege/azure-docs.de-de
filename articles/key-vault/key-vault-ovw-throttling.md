---
title: Anleitung zur Drosselung von Azure Key Vault
description: Die Key Vault-Drosselung schränkt die Anzahl gleichzeitiger Aufrufe ein, um eine übermäßige Nutzung von Ressourcen zu verhindern.
services: key-vault
documentationcenter: ''
author: lleonard-msft
manager: mbaldwin
tags: ''
ms.assetid: 9b7d065e-1979-4397-8298-eeba3aec4792
ms.service: key-vault
ms.workload: identity
ms.topic: article
ms.date: 05/10/2018
ms.author: alleonar
ms.openlocfilehash: 59968f2bccbe2828ebe5fb33c57ed28d4f8509b6
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2018
ms.locfileid: "34067689"
---
# <a name="azure-key-vault-throttling-guidance"></a>Anleitung zur Drosselung von Azure Key Vault

Die Drosselung ist ein Vorgang, den Sie einleiten, um die Anzahl gleichzeitiger Aufrufe für den Azure-Dienst einzuschränken und so eine übermäßige Nutzung von Ressourcen zu verhindern. Azure Key Vault (AKV) dient der Verarbeitung einer große Anzahl von Anforderungen. Sollten jedoch zu viele Anforderungen vorliegen, wird mittels Drosselung die optimale Leistung und Zuverlässigkeit des AKV-Diensts bewahrt.

Die Grenzwerte für die Drosselung sind je nach Szenario unterschiedlich. So ist eine Drosselung beispielsweise wahrscheinlicher, wenn eine Vielzahl von Schreibvorgängen durchgeführt werden, als wenn nur Lesevorgänge durchgeführt werden.

## <a name="how-does-key-vault-handle-its-limits"></a>Wie geht Key Vault mit den Grenzen um?

Bei Key Vault wird mithilfe von Diensteinschränkungen der Missbrauch von Ressourcen verhindert und die Qualität des Dienstes für alle Clients von Key Vault sichergestellt. Wenn der Schwellenwert für einen Dienst überschritten wird, begrenzt Key Vault weitere Anforderungen von diesem Client für eine bestimmte Zeitdauer. In diesem Fall gibt Key Vault den HTTP-Statuscode 429 (zu viele Anforderungen) zurück, und die Anforderungen schlagen fehl. Auch fehlgeschlagene Anforderungen, die 429 zurückgeben, zählen zu dem von Key Vault überwachten Drosselungsgrenzwert. 

Wenn bei Ihnen ein gültiger Geschäftsvorgang für höhere Drosselungsgrenzwerte vorliegt, wenden Sie sich bitte an uns.


## <a name="how-to-throttle-your-app-in-response-to-service-limits"></a>Drosseln einer App als Reaktion auf Diensteinschränkungen

Nachfolgend finden Sie die **bewährten Methoden** zum Drosseln einer App:
- Reduzieren Sie die Anzahl von Vorgängen pro Anforderung.
- Reduzieren Sie die Häufigkeit der Anforderungen.
- Vermeiden Sie schnelle Wiederholungsversuche. 
    - Alle Anforderungen zählen zu Ihren Nutzungseinschränkungen.

Verwenden Sie zur Implementierung der Fehlerbehandlung bei Ihrer App den HTTP-Fehlercode 429, um zu ermitteln, ob eine clientseitige Drosselung erforderlich ist. Wenn die Anforderung mit dem HTTP-Fehlercode 429 erneut fehlschlägt, liegt weiterhin eine Azure-Diensteinschränkung vor. Verwenden Sie weiterhin die empfohlene clientseitige Drosselungsmethode, und wiederholen Sie die Anforderung, bis sie erfolgreich abgeschlossen wird.

### <a name="recommended-client-side-throttling-method"></a>Empfohlene Methode für die clientseitige Drosselungsmethode

Wenn der HTTP-Fehlercode 429 vorliegt, beginnen Sie die Drosselung Ihres Clients mithilfe eines exponentiellen Backoff-Konzepts:

1. 1 Sekunde warten, Anforderung wiederholen
2. Liegt weiterhin eine Drosselung vor: 2 Sekunden warten, Anforderung wiederholen
3. Liegt weiterhin eine Drosselung vor: 4 Sekunden warten, Anforderung wiederholen
4. Liegt weiterhin eine Drosselung vor: 8 Sekunden warten, Anforderung wiederholen
5. Liegt weiterhin eine Drosselung vor: 16 Sekunden warten, Anforderung wiederholen

Nun sollte der HTTP-Antwortcode 429 nicht mehr angezeigt werden.

## <a name="see-also"></a>Weitere Informationen

Ausführlichere Informationen zur Drosselung in der Microsoft Cloud finden Sie unter [Throttling Pattern (Drosselungsmuster)](https://docs.microsoft.com/azure/architecture/patterns/throttling).

