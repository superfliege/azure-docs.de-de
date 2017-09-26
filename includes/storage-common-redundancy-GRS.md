Georedundanter Speicher (GRS) repliziert Ihre Daten in eine sekundäre Region, die Hunderte von Kilometern von der primären Region entfernt ist. Wenn für Ihr Speicherkonto GRS aktiviert ist, sind Ihre Daten beständig gespeichert, selbst bei einem regionalen Komplettausfall oder einem Notfall, nach dem die primäre Region nicht mehr wiederherstellbar ist.

Bei ein Speicherkonto mit aktiviertem GRS wird für ein Update zuerst ein Commit in der primären Region ausgeführt, in der es drei Mal repliziert wird. Anschließend wird das Update asynchron in der sekundären Region repliziert, wo es ebenfalls dreimal repliziert wird.

Bei GRS werden Replikate sowohl in der primären als auch der sekundären Region über separate Fehlerdomänen und Upgradedomänen hinweg in einer Speicherskalierungseinheit verwaltet (wie bei LRS beschrieben).

Überlegungen:

* Da eine asynchrone Replikation eine Verzögerung einschließt, gehen bei einem regionalen Notfall Änderungen, die noch nicht in der sekundären Region repliziert wurden, unter Umständen verloren, wenn die Daten nicht aus der primären Region wiederhergestellt werden können.
* Das Replikat ist nicht verfügbar, sofern von Microsoft kein Failover in die sekundäre Region initiiert wird. Wenn Microsoft ein Failover auf die sekundäre Region initiiert, erhalten Sie nach Abschluss des Failovers Lese- und Schreibzugriff auf diese Daten. Weitere Informationen finden Sie im [Leitfaden zur Notfallwiederherstellung](../articles/storage/common/storage-disaster-recovery-guidance.md). 
* Wenn eine Anwendung Daten aus der sekundären Region lesen können soll, muss der Benutzer RA-GRS aktivieren.

Wenn Sie ein Speicherkonto erstellen, wählen Sie die primäre Region für das Konto aus. Die sekundäre Region wird basierend auf der primären Region bestimmt und kann nicht geändert werden. In der folgenden Tabelle werden die Paare primärer und sekundärer Regionen gezeigt:

| Primär | Sekundär |
| --- | --- |
| USA (Mitte/Norden) | USA, Süden-Mitte |
| USA, Süden-Mitte | USA (Mitte/Norden) |
| USA (Ost) | USA (West) |
| USA (West) | USA (Ost) |
| USA (Ost 2) | USA (Mitte) |
| USA (Mitte) | USA (Ost 2) |
| Nordeuropa | Europa, Westen |
| Europa, Westen | Nordeuropa |
| Südostasien | Ostasien |
| Ostasien | Südostasien |
| Ostchina | Nordchina |
| Nordchina | Ostchina |
| Japan Ost | Japan (Westen) |
| Japan (Westen) | Japan Ost |
| Brasilien Süd | USA, Süden-Mitte |
| Australien (Osten) | Australien, Südosten |
| Australien, Südosten | Australien (Osten) |
| Indien, Süden | Indien, Mitte |
| Indien, Mitte | Indien, Süden |
| Indien, Westen | Indien, Süden |
| US Government, Iowa | US Government, Virginia |
| US Government, Virginia | US Gov Texas |
| US Gov Texas | US Gov Arizona |
| US Gov Arizona | US Gov Texas |
| Kanada, Mitte | Kanada, Osten |
| Kanada, Osten | Kanada, Mitte |
| UK, Westen | UK, Süden |
| UK, Süden | UK, Westen |
| Deutschland, Mitte | Deutschland, Nordosten |
| Deutschland, Nordosten | Deutschland, Mitte |
| USA, Westen 2 | USA, Westen-Mitte |
| USA, Westen-Mitte | USA, Westen 2 |

Aktuelle Informationen zu von Azure unterstützten Regionen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/).

>[!NOTE]  
> Die sekundäre Region von „USA Gov Virginia“ ist „USA Gov Texas“. Bisher wurde „USA Gov Iowa“ als sekundäre Region von „USA Gov Virginia“ verwendet. Speicherkonten, für die noch „USA Gov Iowa“ als sekundäre Region verwendet wird, werden auf die Verwendung von „USA Gov Texas“ als sekundäre Region umgestellt. 
> 
> 