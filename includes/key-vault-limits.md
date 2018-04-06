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

Geheimnisse, Schlüssel für verwaltete Speicherkonten und Tresortransaktionen:
| Transaktionstyp | Max. Transaktionen innerhalb von 10 Sekunden pro Tresor und Region<sup>1</sup> |
| --- | --- |
| Alle Transaktionen |2000 |
|

Informationen zur Handhabung der Drosselung bei Überschreiten dieser Grenzwerte finden Sie unter [Anleitung zur Drosselung von Azure Key Vault](../articles/key-vault/key-vault-ovw-throttling.md).

<sup>1</sup> Für alle Transaktionsarten gilt ein abonnementweites Limit (das Fünffache des Schlüsseltresorlimits.) „HSM – andere Transaktionen“ pro Abonnement ist beispielsweise auf 5.000 Transaktionen in 10 Sekunden pro Abonnent beschränkt.
