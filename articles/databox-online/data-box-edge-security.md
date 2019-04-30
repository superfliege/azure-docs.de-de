---
title: Azure Data Box Edge-Sicherheit | Microsoft-Dokumentation
description: Hier werden die Sicherheits- und Datenschutzfeatures beschrieben, die Ihr Azure Data Box Edge-Gerät, Ihren Dienst und Ihre Daten sowohl lokal als auch in der Cloud schützen.
services: Data Box Edge
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 04/15/2019
ms.author: alkohli
ms.openlocfilehash: 5316ddf9d456731f2789241434926366f732993a
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59682099"
---
# <a name="azure-data-box-edge-security-and-data-protection"></a>Azure Data Box Edge-Sicherheit und -Datenschutz

Sicherheit ist bei der Einführung neuer Technologien immer ein zentrales Anliegen. Das gilt insbesondere, wenn die Technologie mit vertraulichen oder proprietären Daten verwendet wird. Die Microsoft Azure Data Box-Edge-Lösung trägt dazu bei, dass Ihre Daten nur von autorisierten Entitäten angezeigt, geändert oder gelöscht werden können.

In diesem Artikel werden die Sicherheitsfeatures von Data Box Edge beschrieben, mit denen die einzelnen Komponenten der Lösung und die darin gespeicherten Daten geschützt werden.

Die Azure Data Box Edge-Lösung besteht aus vier Hauptkomponenten, die miteinander interagieren:

- **In Azure gehosteter Data Box Edge-Dienst:** Die Verwaltungsressource zum Erstellen des Geräteauftrags, zum Konfigurieren des Geräts sowie zum anschließenden Nachverfolgen des Auftrags bis zur Erfüllung.
- **Data Box Edge-Gerät:** Das Übertragungsgerät, das an Sie gesendet wird, um Ihre lokalen Daten in Azure zu importieren.
- **Mit dem Gerät verbundene Clients/Hosts:** Die Clients in Ihrer Infrastruktur, die mit dem Data Box Edge-Gerät verbunden werden und zu schützende Daten enthalten.
- **Cloud-Speicher** – Der Speicherort in der Azure-Cloud, an dem die Daten gespeichert werden. Hierbei handelt es sich in der Regel um das Speicherkonto, das mit der von Ihnen erstellten Data Box Edge-Ressource verknüpft ist.

## <a name="data-box-edge-service-protection"></a>Schutz des Data Box Edge-Diensts

Der Data Box Edge-Dienst ist ein in Microsoft Azure gehosteter Verwaltungsdienst. Der Dienst wird zum Konfigurieren und Verwalten des Geräts verwendet.

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-service-protection.md)]

## <a name="data-box-edge-device-protection"></a>Schutz des Data Box Edge-Geräts

Das Data Box Edge-Gerät ist ein lokales Gerät, das Sie bei der Datentransformation unterstützt. Hierzu werden die Daten lokal verarbeitet und anschließend an Azure gesendet. Für Ihr Gerät gilt Folgendes:

- Es benötigt einen Aktivierungsschlüssel für den Zugriff auf den Data Box Edge-Dienst.
- Es ist jederzeit durch ein Gerätekennwort geschützt.
- Es ist ein gesperrtes Gerät. BMC und BIOS des Geräts sind kennwortgeschützt (mit eingeschränktem Benutzerzugriff auf das BIOS).
- Sicherer Start ist aktiviert.
- Windows Defender Device Guard wird darauf ausgeführt. Device Guard stellt sicher, dass nur vertrauenswürdige Anwendungen ausgeführt werden können, die Sie in Ihren Codeintegritätsrichtlinien definieren.

### <a name="protect-the-device-via-activation-key"></a>Schützen des Geräts mittels Aktivierungsschlüssel

Dem Data Box Edge-Dienst, den Sie in Ihrem Azure-Abonnement erstellt haben, können nur autorisierte Data Box Edge-Geräte hinzugefügt werden. Um ein Gerät zu autorisieren, müssen Sie es mithilfe eines Aktivierungsschlüssels für den Data Box Edge-Dienst aktivieren.

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-activation-key.md)]

Weitere Informationen finden Sie unter [Abrufen des Aktivierungsschlüssels](data-box-edge-deploy-prep.md#get-the-activation-key).

### <a name="protect-the-device-via-password"></a>Schützen des Geräts mittels Kennwort

Kennwörter sorgen dafür, dass nur autorisierte Benutzer auf Ihre Daten zugreifen können. Data Box Edge-Geräte sind nach dem Start gesperrt.

Ihre Möglichkeiten:

- Stellen Sie über einen Browser eine Verbindung mit der lokalen Webbenutzeroberfläche des Geräts her, und geben Sie ein Kennwort an, um sich bei dem Gerät anzumelden.
- Stellen Sie eine HTTP-Remoteverbindung mit der PowerShell-Schnittstelle des Geräts her. Die Remoteverwaltung ist standardmäßig aktiviert. Anschließend können Sie das Gerätekennwort angeben, um sich bei dem Gerät anzumelden. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit der PowerShell-Schnittstelle](data-box-edge-connect-powershell-interface.md#connect-to-the-powershell-interface).

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-password-best-practices.md)]
- Verwenden Sie zum [Ändern des Kennworts](data-box-edge-manage-access-power-connectivity-mode.md#manage-device-access) die lokale Webbenutzeroberfläche. Sollten Sie das Kennwort ändern, benachrichtigen Sie alle Benutzer mit Remotezugriff, damit bei ihnen keine Anmeldefehler auftreten.

## <a name="protect-the-data"></a>Schützen der Daten

In diesem Abschnitt werden die Data Box Edge-Sicherheitsfeatures für Daten während der Übertragung sowie für gespeicherte Daten beschrieben.

### <a name="protect-data-at-rest"></a>Schutz gespeicherter Daten

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-data-rest.md)]

### <a name="protect-data-in-flight"></a>Schützen von Daten während der Übertragung

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-data-flight.md)]

### <a name="protect-data-via-storage-accounts"></a>Schützen von Daten über Speicherkonten

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-protect-data-storage-accounts.md)]
- Rotieren und [synchronisieren Sie Ihre Speicherkontoschlüssel](data-box-edge-manage-shares.md#sync-storage-keys) regelmäßig, um zu vermeiden, dass nicht autorisierte Benutzer auf Ihr Speicherkonto zugreifen können.

## <a name="manage-personal-information"></a>Verwalten persönlicher Informationen

Der Data Box Edge-Dienst erfasst persönliche Informationen in folgenden Fällen:

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-manage-personal-data.md)]

Eine Anleitung zum Anzeigen der Liste mit den Benutzern, die auf eine Freigabe zugreifen oder sie löschen können, finden Sie unter [Verwalten von Freigaben auf Ihrer Azure Data Box Gateway-Ressource über das Azure-Portal](data-box-edge-manage-shares.md).

Weitere Informationen finden Sie im [Trust Center](https://www.microsoft.com/trustcenter) in der Microsoft-Datenschutzrichtlinie.

## <a name="next-steps"></a>Nächste Schritte

[Tutorial: Vorbereiten der Bereitstellung von Azure Data Box Edge](data-box-edge-deploy-prep.md)
