---
title: Includedatei
description: Includedatei
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 9fa18b14b82376a25bb434acd770d340b1ef9262
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
ms.locfileid: "30197049"
---
Überprüfen Sie die folgenden Punkte, falls bei der Verbindungsherstellung Probleme auftreten:

- Falls Sie ein Clientzertifikat exportiert haben, vergewissern Sie sich, dass Sie es als PFX-Datei mit dem Standardwert „Wenn möglich, alle Zertifikate im Zertifizierungspfad einbeziehen“ exportiert haben. Bei Verwendung dieses Werts werden auch die Stammzertifikatinformationen exportiert. Wenn das Zertifikat auf dem Clientcomputer installiert wird, wird auch das in der PFX-Datei enthaltene Stammzertifikat auf dem Clientcomputer installiert. Die Stammzertifikatinformationen müssen auf dem Clientcomputer installiert sein. Öffnen Sie zur Prüfung **Benutzerzertifikate verwalten**, und navigieren Sie zu **Vertrauenswürdige Stammzertifizierungsstellen\Zertifikate**. Vergewissern Sie sich, dass das Stammzertifikat aufgeführt ist. Das Stammzertifikat muss vorhanden sein, damit die Authentifizierung funktioniert.

- Wenn Sie bei Verwendung eines Zertifikats, das mit einer Lösung einer Unternehmenszertifizierungsstelle ausgestellt wurde, Probleme mit der Authentifizierung haben, ist es ratsam, die Authentifizierungsreihenfolge des Clientzertifikats zu überprüfen. Sie können die Reihenfolge der Authentifizierungsliste überprüfen, indem Sie auf das Clientzertifikat doppelklicken und zu **Details > Erweiterte Schlüsselverwendung** navigieren. Achten Sie darauf, dass in der Liste „Client Authentication“ als erster Eintrag aufgeführt ist. Wenn nicht, müssen Sie ein Clientzertifikat basierend auf der User-Vorlage ausstellen, die als ersten Eintrag in der Liste „Client Authentication“ enthält.

- Zusätzliche Informationen zur Problembehandlung für P2S finden Sie unter [Problembehandlung: Azure Punkt-zu-Standort-Verbindungsprobleme](../articles/vpn-gateway/vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).