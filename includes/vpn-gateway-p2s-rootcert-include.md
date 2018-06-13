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
ms.openlocfilehash: b2a96aad793d956b3ea3de06fba74bcf68e50786
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
ms.locfileid: "30197043"
---
Sie können entweder ein Stammzertifikat verwenden, das mit einer Unternehmenslösung generiert wurde (empfohlen), oder ein selbstsigniertes Zertifikat generieren. Exportieren Sie nach dem Erstellen des Stammzertifikats die Daten des öffentlichen Zertifikats (nicht den privaten Schlüssel) als Base64-codierte X.509-CER-Datei, und laden die Daten des öffentlichen Zertifikats in Azure hoch.

* **Unternehmenszertifikat:** Bei einer Unternehmenslösung können Sie Ihre vorhandene Zertifikatkette verwenden. Rufen Sie die CER-Datei für das Stammzertifikat ab, das Sie verwenden möchten.
* **Selbstsigniertes Stammzertifikat:** Wenn Sie keine Unternehmenszertifikatlösung verwenden, müssen Sie ein selbstsigniertes Stammzertifikat generieren. Führen Sie unbedingt die Schritte in einem der unten aufgeführten Artikel zu P2S-Zertifikaten aus. Andernfalls sind die erstellten Zertifikate nicht mit P2S-Verbindungen kompatibel, und auf Clients wird beim Verbindungsversuch ein Verbindungsfehler angezeigt. Sie können Azure PowerShell, MakeCert oder OpenSSL verwenden. Mit den Schritten in den bereitgestellten Artikeln wird ein kompatibles Zertifikat generiert:

  * [Generieren und Exportieren von Zertifikaten für Punkt-zu-Site-Verbindungen mithilfe von PowerShell unter Windows 10](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md): Für diese Anweisungen sind Windows 10 und PowerShell erforderlich, um Zertifikate zu generieren. Clientzertifikate, die von der Stammzertifizierungsstelle generiert werden, können auf jedem unterstützten P2S-Client installiert werden.
  * [Generieren und Exportieren von Zertifikaten für Point-to-Site-Verbindungen mithilfe von MakeCert](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md): Verwenden Sie MakeCert, wenn Sie keinen Zugriff auf einen Windows 10-Computer haben, um Zertifikate zu generieren. MakeCert ist veraltet, Sie können damit jedoch trotzdem Zertifikate generieren. Clientzertifikate, die von der Stammzertifizierungsstelle generiert werden, können auf jedem unterstützten P2S-Client installiert werden.
