---
title: "Übersicht über die Azure Files-Freigabemomentaufnahme (Vorschau) | Microsoft-Dokumentation"
description: "Die Azure Files-Freigabemomentaufnahme ist eine schreibgeschützte Version einer Azure Files-Freigabe, die zu einem bestimmten Zeitpunkt erstellt wird. Nachdem eine Freigabemomentaufnahme erstellt wurde, kann sie zwar gelesen, kopiert oder gelöscht, aber nicht geändert werden. Freigabemomentaufnahmen bieten eine Möglichkeit, die Freigabe so zu sichern, wie sie zu einem bestimmten Zeitpunkt dargestellt wird."
services: storage
documentationcenter: .net
author: renash
manager: aungoo
editor: tysonn
ms.assetid: edabe3ee-688b-41e0-b34f-613ac9c3fdfd
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2017
ms.author: renash
ms.openlocfilehash: 70b7fec318b527c6c67bd281892c10814712bbc6
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-files-share-snapshot-preview-overview"></a>Übersicht über die Azure Files-Freigabemomentaufnahme (Vorschau)
Azure Files bietet die Möglichkeit, Freigabemomentaufnahmen von Dateifreigaben zu erstellen. Freigabemomentaufnahmen (Vorschau) erfassen den Freigabestatus zum jeweiligen Zeitpunkt. In diesem Artikel werden die Funktionen von Freigabemomentaufnahmen beschrieben. Zudem wird erläutert, wie Sie sie in Ihrem benutzerdefinierten Anwendungsfall nutzen können.


## <a name="when-to-use-share-snapshots"></a>Verwendung von Freigabemomentaufnahmen

### <a name="protection-against-application-error-and-data-corruption"></a>Schutz vor Anwendungsfehlern und Datenbeschädigung
-------------------------------------------------------------------------

Anwendungen, in denen Azure-Dateifreigaben verwendet werden, führen verschiedene Vorgänge durch, z.B. Schreib- und Lesevorgänge, Speicherung, Übertragung oder Verarbeitung. Eine Anwendung kann falsch konfiguriert werden, oder ein unbeabsichtigter Fehler kann eingeführt werden. Dies führt zu versehentlichem Überschreiben oder zur Beschädigung einiger Datenblöcke. Zum Schutz vor diesen Szenarien können Sie eine Freigabemomentaufnahme erstellen, bevor Sie neuen Anwendungscode bereitstellen. Wenn mit der neuen Bereitstellung ein Fehler oder Anwendungsfehler eingeführt wird, können Sie eine vorherige Version der Daten verwenden, die sich in dieser Dateifreigabe befindet. 

### <a name="protection-against-accidental-deletions-or-unintended-changes"></a>Schutz vor versehentlichem Löschen oder unbeabsichtigten Änderungen
----------------------------------------------------------------------------------------------

Stellen Sie sich einen Benutzer vor, der eine Textdatei in einer Dateifreigabe bearbeitet. Nachdem die Textdatei geschlossen wurde, können Änderungen nicht mehr rückgängig gemacht werden. In diesem Fall muss eine vorherige Version der Datei wiederhergestellt werden. Mit Freigabemomentaufnahmen können Sie vorherige Versionen der Datei wiederherstellen, wenn diese versehentlich umbenannt oder gelöscht wird.

### <a name="general-backup-purposes"></a>Allgemeine Sicherung
---------------------------

Nach dem Erstellen einer Azure-Dateifreigabe können Sie regelmäßig eine Freigabemomentaufnahme der Azure-Dateifreigabe erstellen, um sie für die Datensicherung zu verwenden. Regelmäßig erstellte Freigabemomentaufnahmen helfen bei der Verwaltung vorheriger Versionen von Daten, die für künftige Überprüfungsanforderungen oder zur Notfallwiederherstellung verwendet werden können.

## <a name="share-snapshot-capabilities"></a>Funktionen von Freigabemomentaufnahmen

Eine Freigabemomentaufnahme ist eine schreibgeschützte Kopie Ihrer Daten zu einem bestimmten Zeitpunkt. Sie können Momentaufnahmen mit der REST-API erstellen, löschen und verwalten. Die gleichen Funktionen sind auch in der Clientbibliothek, der Befehlszeilenschnittstelle und im Azure-Portal verfügbar. Auch die PowerShell-Integration für Freigabemomentaufnahmen ist bald verfügbar. Sie können Momentaufnahmen einer Freigabe über die REST-API und über SMB anzeigen. Kunden können die Liste der Versionen eines Verzeichnisses oder einer Datei abrufen und zudem eine spezifische Version direkt als Laufwerk einbinden. Nachdem eine Freigabemomentaufnahme erstellt wurde, kann sie zwar gelesen, kopiert oder gelöscht, aber nicht geändert werden. Sie können keine gesamte Freigabemomentaufnahme in ein anderes Speicherkonto kopieren. Stattdessen müssen Sie die Dateien einzeln mithilfe von AzCopy oder anderen Kopiermechanismen kopieren.

Die Funktion für Freigabemomentaufnahmen wird auf Ebene der Dateifreigabe bereitgestellt. Der Abruf wird dagegen auf Ebene der einzelnen Dateien bereitgestellt, sodass einzelne Dateien wiederhergestellt werden können. Sie können eine vollständige Dateifreigabe über SMB, die REST-API, das Portal oder die Clientbibliothek wiederherstellen oder dazu PowerShell oder die Befehlszeilenschnittstelle verwenden.

Eine Freigabemomentaufnahme einer Dateifreigabe ist mit der zugehörigen Basisdateifreigabe identisch, nur dass an den Freigabe-URI ein **DateTime**-Wert angefügt ist. Hiermit wird der Zeitpunkt angegeben, zu dem die Freigabemomentaufnahme erstellt wurde. Wenn der URI einer Dateifreigabe beispielsweise „http://storagesample.core.file.windows.net/myshare“ lautet, ähnelt der URI der Freigabemomentaufnahme dem folgenden URI:
```
http://storagesample.core.file.windows.net/myshare?snapshot=2011-03-09T01:42:34.9360000Z.
```

Freigabemomentaufnahmen werden beibehalten, bis sie explizit gelöscht werden. Eine Freigabemomentaufnahme kann die zugehörige Basisdateifreigabe nicht überdauern. Sie können alle einer Basisdateifreigabe zugeordneten Momentaufnahmen auflisten, um die aktuell vorhandenen Momentaufnahmen nachzuverfolgen. Wenn Sie eine Freigabemomentaufnahme einer Dateifreigabe erstellen, werden die Dateien in den Systemeigenschaften der Freigabe mit den gleichen Werten in die Freigabemomentaufnahme kopiert. Die Basisdateien und die Metadaten der Dateifreigabe werden auch in die Freigabemomentaufnahme kopiert, sofern Sie beim Erstellen keine separaten Metadaten für die Freigabemomentaufnahme angeben.

Sie können eine Freigabe, die Freigabemomentaufnahmen enthält, nicht löschen, ohne vorher alle zugehörigen Freigabemomentaufnahmen zu löschen.


## <a name="share-snapshot-space-usage"></a>Speicherplatzbelegung durch Freigabemomentaufnahmen 

Freigabemomentaufnahmen sind inkrementell. Das bedeutet, dass nur die Daten gespeichert werden, die sich nach der letzten Freigabemomentaufnahme geändert haben. Dadurch reduziert sich die erforderliche Zeit zum Erstellen der Freigabemomentaufnahme, und zudem werden Speicherkosten eingespart. Alle Schreibvorgänge im Objekt und alle Vorgänge zum Aktualisieren von Eigenschaften oder Metadaten werden als „geänderter Inhalt“ gezählt und in der Freigabemomentaufnahme gespeichert. 

Um Speicherplatz zu sparen, können Sie die Freigabemomentaufnahme löschen, in deren Zeitraum die meisten Änderungen vorgenommen wurden.

Obwohl Freigabemomentaufnahmen inkrementell gespeichert werden, ist der Löschvorgang für Freigabemomentaufnahmen so konzipiert, dass nur die jeweils letzte Freigabemomentaufnahme beibehalten werden muss, um die Freigabe wiederherstellen zu können. Wenn Sie eine Freigabemomentaufnahme löschen, werden nur die für diese Freigabemomentaufnahme eindeutigen Daten entfernt. Aktive Momentaufnahmen enthalten alle erforderlichen Informationen zum Durchsuchen und Wiederherstellen der Daten (ab dem Zeitpunkt, zu dem die Freigabemomentaufnahme erstellt wurde) am ursprünglichen oder einem anderen Speicherort. Die Wiederherstellung kann auf Elementebene erfolgen.

Momentaufnahmen werden auf Ihr Limit für Freigaben von 5 TB nicht angerechnet. Es gibt keine Beschränkung für die Belegung von Speicherplatz durch Freigabemomentaufnahmen. Jedoch gelten die Beschränkungen für Speicherkonten weiterhin.

## <a name="limits"></a>Einschränkungen

Derzeit sind in Azure Files maximal 200 Freigabemomentaufnahmen zulässig. Nach 200 Freigabemomentaufnahmen müssen Benutzer ältere Freigabemomentaufnahmen löschen, um neue Freigabemomentaufnahmen erstellen zu können. Es gibt keine Einschränkung für gleichzeitige Aufrufe zum Erstellen von Freigabemomentaufnahmen.
Für den Speicherplatz, den Freigabemomentaufnahmen einer bestimmten Dateifreigabe belegen können, gelten keine Einschränkungen. 

## <a name="copy-data-back-to-a-share-from-share-snapshot"></a>Kopieren von Daten von einer Freigabemomentaufnahme zurück in eine Freigabe

Für Kopiervorgänge, die Dateien und Freigabemomentaufnahmen betreffen, gelten folgende Regeln:

Sie können einzelne Dateien in einer Dateifreigabemomentaufnahme in die zugehörige Basisfreigabe oder in jeden anderen Speicherort kopieren. Sie können eine frühere Version einer Datei wiederherstellen oder eine gesamte Dateifreigabe wiederherstellen, indem Sie die einzelnen Dateien nacheinander von der Freigabemomentaufnahme kopieren. Die Freigabemomentaufnahme wird nicht auf die Basisfreigabe heraufgestuft. Die Freigabemomentaufnahme bleibt nach dem Kopieren intakt, die Basisdateifreigabe wird dagegen durch eine in der Freigabemomentaufnahme verfügbare Kopie der Daten überschrieben. Alle wiederhergestellten Dateien werden als „geänderter Inhalt“ gezählt.

Sie können eine Datei in einer Freigabemomentaufnahme in ein Ziel mit einem anderen Namen kopieren. Die entsprechende Zieldatei ist eine beschreibbare Datei und keine Freigabemomentaufnahme.

Wenn eine Zieldatei durch eine Kopie überschrieben wird, bleiben alle der ursprünglichen Zieldatei zugeordneten Freigabemomentaufnahmen erhalten.

## <a name="general-best-practices"></a>Allgemeine bewährte Methoden 

Beim Ausführen der Infrastruktur in Azure sollten Sie Sicherungen zur Datenwiederherstellung nach Möglichkeit automatisieren. Automatisierte Aktionen sind zuverlässiger als manuelle Vorgänge und verbessern so den Schutz und die Wiederherstellbarkeit von Daten. Für die Automatisierung können Sie die REST-API, das Client SKD oder Skripts verwenden.

Überlegen Sie vor der Bereitstellung des Freigabemomentaufnahmen-Planers sorgfältig, in welchem Intervall die Freigabemomentaufnahmen erstellt werden sollen und welche Aufbewahrungseinstellungen Sie festlegen möchten, um zu vermeiden, dass unnötige Kosten für Freigabemomentaufnahmen anfallen.

Freigabemomentaufnahmen bieten nur Schutz auf Dateiebene. Versehentliche Tippfehler bei Löschvorgängen in der Dateifreigabe oder im Speicherkonto werden durch Freigabemomentaufnahmen nicht verhindert. Sie können Ihr Speicherkonto oder Ihre Ressourcengruppe sperren, um das Speicherkonto vor versehentlichem Löschen zu schützen.

## <a name="next-steps"></a>Nächste Schritte
* [Arbeiten mit Azure Files-Freigabemomentaufnahmen (Vorschau)](storage-how-to-use-files-snapshots.md)
* [Häufig gestellte Fragen zu Azure Files](storage-files-faq.md)

