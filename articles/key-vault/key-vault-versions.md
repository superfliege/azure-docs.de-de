---
title: Key Vault-Versionen
description: Die verschiedenen Versionen von Azure Key Vault
services: key-vault
documentationcenter: ''
author: bryanla
manager: barbkess
tags: azure-resource-manager
ms.assetid: e8622dcc-59a3-4f4b-9f63-cd2232515a65
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: bryanla
ms.openlocfilehash: d74bf8603e3d5a394935994bc61fa0e96905ed25
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2019
ms.locfileid: "56115027"
---
# <a name="key-vault-versions"></a>Key Vault-Versionen

## <a name="2016-10-01---managed-storage-account-keys"></a>2016-10-01: Schlüssel für verwaltete Speicherkonten

Sommer 2017: Das hinzugefügte Feature „Speicherkontoschlüssel“ erleichtert die Integration in Azure Storage. Weitere Informationen finden Sie im Übersichtsthema [Azure Key Vault-Speicherkontoschlüssel](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-storage-keys).

## <a name="2016-10-01---soft-delete"></a>2016-10-01: Vorläufiges Löschen

Sommer 2017: Das hinzugefügte Feature „Vorläufiges Löschen“ verbessert den Datenschutz Ihrer Schlüsseltresore und der Schlüsseltresorobjekte. Weitere Informationen finden Sie im Übersichtsthema [Übersicht über die Azure Key Vault-Funktion für vorläufiges Löschen](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete).

## <a name="2015-06-01---certificate-management"></a>2015-06-01: Zertifikatverwaltung

Zertifikatverwaltung wird als Funktion der GA-Version 2015-06-01 am 26. September 2016 hinzugefügt.

## <a name="2015-06-01---general-availability"></a>2015-06-01: Allgemeine Verfügbarkeit

Die allgemein verfügbare Version 2015-06-01, am 24. Juni 2015 angekündigt.

Die folgenden Änderungen wurden in dieser Version vorgenommen:

- Löschen eines Schlüssels: Feld zur Verwendung entfernt.
- Abrufen von Informationen über einen Tresor: Feld zur Verwendung entfernt.
- Importieren eines Schlüssels in einen Tresor: Feld zur Verwendung entfernt.
- Wiederherstellen eines Schlüssels: Feld zur Verwendung entfernt.
- Für RSA-Algorithmen „RSA_OAEP“ in „RSA-OAEP“ geändert. Siehe [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](about-keys-secrets-and-certificates.md).

## <a name="2015-02-01-preview"></a>2015-02-01-preview 

Zweite Vorschauversion 2015-02-01-preview, am 20. April 2015 angekündigt. Weitere Informationen finden Sie im Blogbeitrag zum [REST-API-Update](http://blogs.technet.com/b/kv/archive/2015/04/20/empty-3.aspx).

Die folgenden Aufgaben wurden aktualisiert:

- Auflisten der Schlüssel in einem Tresor: Unterstützung für Paginierung wurde zum Vorgang hinzugefügt.
- Auflisten der Versionen eines Schlüssels: Vorgang zum Auflisten der Versionen eines Schlüssels wurde hinzugefügt.
- Auflisten der Geheimnisse in einem Tresor: Unterstützung für Paginierung wurde hinzugefügt.
- Auflisten der Versionen eines Geheimnisses: Vorgang zum Auflisten der Versionen eines Geheimnisses wurde hinzugefügt.
- Alle Vorgänge: Attributen wurden Zeitstempel der Erstellung/Aktualisierung hinzugefügt.
- Erstellen eines Geheimnisses:Geheimnissen wurde ein Inhaltstyp hinzugefügt.
- Erstellen eines Schlüssels: Tags wurden als optionale Informationen hinzugefügt.
- Erstellen eines Geheimnisses: Tags wurden als optionale Informationen hinzugefügt.
- Aktualisieren eines Schlüssels: Tags wurden als optionale Informationen hinzugefügt.
- Aktualisieren eines Geheimnisses: Tags wurden als optionale Informationen hinzugefügt.
- Maximale Größe für Geheimnisse wurde von 10.000 in 25.000 Bytes geändert. Siehe [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](about-keys-secrets-and-certificates.md).

## <a name="2014-12-08-preview"></a>2014-12-08-preview

Erste Vorschauversion 2014-12-08-preview, am 8. Januar 2015 angekündigt.

## <a name="see-also"></a>Weitere Informationen
- [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](about-keys-secrets-and-certificates.md)
