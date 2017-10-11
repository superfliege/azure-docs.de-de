---
title: "Datenbankverschlüsselung ruhender: Azure-Cosmos-Datenbank | Microsoft Docs"
description: "Erfahren Sie, wie Azure-Cosmos-DB standardverschlüsselung aller Daten bereitstellt."
services: cosmos-db
author: voellm
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 99725c52-d7ca-4bfa-888b-19b1569754d3
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: voellm
ms.openlocfilehash: d8967d4504a8ccabb444c7f3d5635e2d00f287c5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-database-encryption-at-rest"></a>Ruhende datenbankverschlüsselung Azure Cosmos-DB

Ruhende Verschlüsselung ist ein Ausdruck, der häufig auf die Verschlüsselung von Daten auf Geräten nicht flüchtigen Speicher wie solid-State-Laufwerken (SSDs) verweist, nach dem und Festplattenlaufwerken (HDDs). COSMOS DB speichert die primären Datenbanken auf SSDs. Ihre Medien Anlagen und die Sicherungen werden im Azure-Blob-Speicher gespeichert, der in der Regel durch HDDs gesichert wird. Mit der Version der Verschlüsselung von ruhenden für Cosmos-DB werden alle Datenbanken, Anhänge und Sicherungen verschlüsselt. Ihre Daten werden jetzt während der Übertragung verschlüsselt, (über das Netzwerk) und im Ruhezustand (permanentem Speicher), wodurch Sie End-to-End-Verschlüsselung.

Wie ein PaaS-Dienst, Cosmos DB sehr einfach zu verwenden ist. Da alle in der Cosmos-Datenbank gespeicherte Daten werden verschlüsselt, ruhende und Transport, müssen Sie keine Maßnahmen ergreifen. Wird von einem anderen Verfahren, um dies abzulegen, die Verschlüsselung von ruhenden finden Sie in der Standardeinstellung "auf". Es sind keine Steuerelemente, die es ein- bzw. auszuschalten. Wir diese Funktion bereitstellen, während wir weiterhin erfüllen unsere [Verfügbarkeit und Leistung SLAs](https://azure.microsoft.com/support/legal/sla/cosmos-db).

## <a name="implement-encryption-at-rest"></a>Implementieren Sie die Verschlüsselung im Ruhezustand

Verschlüsselung ruhender wird mithilfe einer Reihe von sicherheitstechnologien, einschließlich der sicheren Schlüssel Speichersystemen, verschlüsselte Netzwerke und Kryptografie-APIs implementiert. Auf Systemen, die zu entschlüsseln und Verarbeiten von Daten sind zum Kommunizieren mit Systemen, die Schlüssel zu verwalten. Das Diagramm zeigt, wie die Speicherung von verschlüsselten Daten und die Verwaltung von Schlüsseln getrennt ist. 

![Designdiagramm](./media/database-encryption-at-rest/design-diagram.png)

Die grundlegende Vorgehensweise eine benutzeranforderung lautet wie folgt:
- Das Benutzerkonto für die Datenbank zur Verfügung gestellt wird und Speicherschlüssel über eine Anforderung an den Management-Ressource Dienstanbieter abgerufen werden.
- Ein Benutzer erstellt eine Verbindung mit der Cosmos-Datenbank über HTTPS/secure Transport. (Die Details der SDKs abstrahieren.)
- Der Benutzer sendet ein JSON-Dokument über die zuvor erstellte sichere Verbindung gespeichert werden soll.
- Das JSON-Dokument wird indiziert, es sei denn, die Indizierung der Benutzer deaktiviert hat.
- Die JSON-Dokument und den Index der Daten werden geschrieben, um Speicher zu sichern.
- In regelmäßigen Abständen, Daten aus dem sicheren Speicher lesen und in der Azure-verschlüsselte Blob-Speicher gesichert.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="q-how-much-more-does-azure-storage-cost-if-storage-service-encryption-is-enabled"></a>F: wie viel kostet Azure-Speicher Wenn Speicherdienstverschlüsselung aktiviert ist?
A: Es fallen keine zusätzlichen Kosten.

### <a name="q-who-manages-the-encryption-keys"></a>F: wer die Verschlüsselungsschlüssel verwaltet?
A: die Schlüssel werden von Microsoft verwaltet.

### <a name="q-how-often-are-encryption-keys-rotated"></a>F: wie oft werden die Verschlüsselungsschlüssel gedreht?
A: Microsoft hat es sich um einen Satz von internen Richtlinien für die Rotation der Verschlüsselungsschlüssel, der Cosmos-DB folgt. Die bestimmten Richtlinien werden nicht veröffentlicht. Microsoft werden veröffentlicht, die [Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl/default.aspx), die als eine Teilmenge der internen Anleitung angezeigt wird, und ist nützlich, bewährte Methoden für Entwickler.

### <a name="q-can-i-use-my-own-encryption-keys"></a>F: verwenden kann ich meinen eigenen Verschlüsselungsschlüssel?
A: Cosmos-DB ist ein PaaS-Dienst, und wir arbeitete hart daran, den Dienst einfach zu verwendende beibehalten. Wir haben festgestellt, dass dieser Frage häufig als Proxy für die Erfüllung einer Anforderung Kompatibilität wie PCI-DSS Frage gestellt wird. Im Rahmen der Erstellung dieses Feature arbeitet wir mit Kompatibilität "Prüfer", um sicherzustellen, dass Kunden Cosmos-DB-verwenden ihre Anforderungen ohne den Schlüssel selbst verwalten.
Daher bieten wir derzeit keine Benutzer die Möglichkeit, selbst mit schlüsselverwaltung versehen an.

### <a name="q-what-regions-have-encryption-turned-on"></a>F: Was Regionen den aktivierter Verschlüsselung haben?
A: alle Azure-Cosmos-DB-Regionen haben Verschlüsselung für alle Benutzerdaten aktiviert.

### <a name="q-does-encryption-affect-the-performance-latency-and-throughput-slas"></a>F: wirkt Verschlüsselung die Leistung Latenzzeit und Durchsatz SLAs sich?
A: ist keine Auswirkungen oder Änderungen für die Leistung SLAs nun, dass die Verschlüsselung von ruhenden für alle vorhandenen und neuen Konten aktiviert ist. Erfahren Sie mehr auf die [SLA für Cosmos-DB](https://azure.microsoft.com/support/legal/sla/cosmos-db) Bild, um die neuesten Garantien anzuzeigen.

### <a name="q-does-the-local-emulator-support-encryption-at-rest"></a>F: unterstützt lokale Emulator ruhende Verschlüsselung?
A: der Emulator ist ein eigenständiges Dev/Test-Tool und verwendet nicht die Key Management-Dienste, die der verwaltete Cosmos-DB-Dienst verwendet. Unsere Empfehlung ist zum Aktivieren von BitLocker auf einem Laufwerk, auf dem Sie vertrauliche Emulator Testdaten speichern. Die [Emulator unterstützt das Ändern der Standarddatenverzeichnis](local-emulator.md) sowie mit einem bekannten Speicherort.

## <a name="next-steps"></a>Nächste Schritte

Einen Überblick über Cosmos-DB-Sicherheit und die neuesten Verbesserungen finden Sie unter [Azure Cosmos-DB-datenbanksicherheit](database-security.md).
Weitere Informationen zu Microsoft-Zertifizierungen finden Sie unter der [Azure Trust Center](https://azure.microsoft.com/en-us/support/trust-center/).
