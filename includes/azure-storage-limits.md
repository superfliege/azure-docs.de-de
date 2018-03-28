| Ressource | Standardlimit |
| --- | --- |
| Anzahl von Speicherkonten pro Abonnement | 200<sup>1</sup> |
| Maximale Speicherkontokapazität | 500 TiB<sup>2</sup> |
| Maximale Anzahl an Blob-Containern, Blobs, Dateifreigaben, Tabellen, Warteschlangen, Entitäten oder Meldungen pro Speicherkonto | Keine Begrenzung |
| Maximale Anforderungsrate pro Speicherkonto | 20.000 Anforderungen pro Sekunde<sup>2</sup> |
| Max. Eingang<sup>3</sup> pro Speicherkonto (US-Regionen) | 10 GBit/s bei aktiviertem GRS/ZRS<sup>4</sup>, 20 GBit/s für LRS<sup>2</sup> |
| Max. Ausgang<sup>3</sup> pro Speicherkonto (US-Regionen) | 20 GBit/s bei aktiviertem RA-GRS/GRS/ZRS<sup>4</sup>, 30 GBit/s für LRS<sup>2</sup> |
| Max. Eingang<sup>3</sup> pro Speicherkonto (US-fremde Regionen) | 5 GBit/s bei aktiviertem GRS/ZRS<sup>4</sup>, 10 GBit/s für LRS<sup>2</sup> |
| Max. Ausgang<sup>3</sup> pro Speicherkonto (US-fremde Regionen) | 10 GBit/s bei aktiviertem RA-GRS/GRS/ZRS<sup>4</sup>, 15 GBit/s für LRS<sup>2</sup> |

<sup>1</sup> Dies umfasst sowohl Standard- als auch Premium-Speicherkonten. Wenn Sie mehr als 200 Speicherkonten benötigen, stellen Sie eine Anfrage an den [Azure-Support](https://azure.microsoft.com/support/faq/). Das Azure Storage-Team überprüft Ihr Geschäftsszenario und kann bis zu 250 Speicherkonten genehmigen. 

<sup>2</sup> Wenn Sie erweiterte Grenzwerte für Ihr Speicherkonto benötigen, wenden Sie sich an den [Azure-Support](https://azure.microsoft.com/support/faq/). Das Azure Storage-Team prüft Ihre Anforderung und entscheidet von Fall zu Fall, ob eine Erhöhung gewährt wird. Sowohl Universal- als auch Blobspeicherkonten unterstützen eine erhöhte Kapazität, eingehende/ausgehende Daten und eine Anforderungsrate auf Anfrage. Die neuen Höchstwerte für Blobspeicherkonten finden Sie im Blog zur [Ankündigung größerer Speicherkonten mit größerer Skalierung](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/).

<sup>3</sup> Die einzigen Beschränkungen sind die Grenzwerte für Kontoeingang und -ausgang. *Eingang* bezieht sich auf alle Daten (Anforderungen), die an ein Speicherkonto gesendet werden. *Ausgang* bezieht sich auf alle Daten (Antworten), die von einem Speicherkonto empfangen werden.  

<sup>4</sup> Redundanzoptionen für Azure Storage sind:
* **RA-GRS**: Georedundanter Speicher mit Lesezugriff (RA-GRS) Wenn RA-GRS aktiviert ist, sind die Ausgangsziele für den sekundären Standort mit denen für den primären Standort identisch.
* **GRS**: Georedundanter Speicher 
* **ZRS**: Zonenredundanter Speicher Derzeit nur für Blockblobs verfügbar. 
* **LRS**: Lokal redundanter Speicher 
