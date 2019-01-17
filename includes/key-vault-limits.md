---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 9a39abf77a7396302f93e5a423271402b7c3edb3
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54084003"
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
> Die oben genannten Schwellenwerte sind gewichtet und ihre Summe wird erzwungen. Sie können 125 RSA-HSM-4k- und 0 RSA-HSM-2k- oder 124 RSA-HSM-4-k- und 16 RSA-HSM-2k-Vorgänge durchführen. Anschließend verursacht im gleichen 10-Sekunden-Intervall jeder andere Vorgang eine AKV-Clientausnahme.

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
