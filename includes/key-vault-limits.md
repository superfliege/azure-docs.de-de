---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: efa367157a8fd896cdc9680bf2ab6ba6a9e3dbb0
ms.sourcegitcommit: b254db346732b64678419db428fd9eb200f3c3c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2018
ms.locfileid: "53429968"
---
Schlüsseltransaktionen (Max. Transaktionen innerhalb von 10 Sekunden pro Tresor und Region<sup>1</sup>):

|Schlüsseltyp|HSM-Schlüssel<br>Schlüssel erstellen|HSM-Schlüssel<br>Alle anderen Transaktionen|Softwareschlüssel<br>Schlüssel erstellen|Softwareschlüssel<br>Alle anderen Transaktionen|
|:---|---:|---:|---:|---:|
|RSA 2048 Bit|5|1000|10|2000|
|RSA 3072 Bit|5|250|10|500|
|RSA 4096 Bit|5|125|10|250|
|ECC P-256|5|1000|10|2000|
|ECC P-384|5|1000|10|2000|
|ECC P-521|5|1000|10|2000|
|ECC SECP256K1|5|1000|10|2000|
|

> [!NOTE]
> In der folgenden Tabelle sehen Sie, dass wir für softwaregestützte Schlüssel 2.000 Transaktionen pro 10 Sekunden und für HSM-gestützte Schlüssel 1.000 Transaktionen pro 10 Sekunden zulassen. Das Verhältnis von softwaregestützten Transaktionen für 3.072 Schlüssel zu 2.048 Schlüssel ist 500/2000 oder 0,4. Das bedeutet, dass ein Kunde sein maximales Limit erreicht, wenn er 500 3.072 Schlüsseltransaktionen in 10 Sekunden durchführt und keine anderen Schlüsseloperationen durchführen kann. 
   
|Schlüsseltyp  | Softwareschlüssel |HSM-Schlüssel  |
|---------|---------|---------|
|RSA 2048 Bit     |    2000     |   1000    |
|RSA 3072 Bit     |     500    |    250     |
|RSA 4096 Bit     |    125     |    250     |
|ECC P-256     |    2000     |  1000     |
|ECC P-384     |    2000     |  1000     |
|ECC P-521     |    2000     |  1000     |
|ECC SECP256K1     |    2000     |  1000     |


Geheimnisse, Schlüssel für verwaltete Speicherkonten und Tresortransaktionen:
| Transaktionstyp | Max. Transaktionen innerhalb von 10 Sekunden pro Tresor und Region<sup>1</sup> |
| --- | --- |
| Alle Transaktionen |2000 |
|

Informationen zur Handhabung der Drosselung bei Überschreiten dieser Grenzwerte finden Sie unter [Anleitung zur Drosselung von Azure Key Vault](../articles/key-vault/key-vault-ovw-throttling.md).

<sup>1</sup> Für alle Transaktionsarten gilt ein abonnementweites Limit (das Fünffache des Schlüsseltresorlimits.) „HSM – andere Transaktionen“ pro Abonnement ist beispielsweise auf 5.000 Transaktionen in 10 Sekunden pro Abonnent beschränkt.
