---
title: Data Box Edge-Sicherheit | Microsoft-Dokumentation
description: Hier werden die Sicherheits- und Datenschutzfeatures beschrieben, die Ihr Data Box Edge-Gerät, Ihren Dienst und Ihre Daten sowohl lokal als auch in der Cloud schützen.
services: Data Box Edge
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 04/02/2019
ms.author: alkohli
ms.openlocfilehash: de737f20147e8208dd18388eedcac11583c8cb97
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2019
ms.locfileid: "58891802"
---
# <a name="data-box-edge-security-and-data-protection"></a>Data Box Edge-Sicherheit und -Datenschutz

Sicherheit ist bei der Einführung neuer Technologien immer ein zentrales Anliegen. Das gilt insbesondere, wenn die Technologie mit vertraulichen oder proprietären Daten verwendet wird. Die Microsoft Azure Data Box-Edge-Lösung trägt dazu bei, dass Ihre Daten nur von autorisierten Entitäten angezeigt, geändert oder gelöscht werden können.

In diesem Artikel werden die Sicherheitsfeatures von Data Box Edge beschrieben, mit denen die einzelnen Komponenten der Lösung und die darin gespeicherten Daten geschützt werden.

Die Azure Data Box Edge-Lösung besteht aus vier Hauptkomponenten, die miteinander interagieren:

- **In Azure gehosteter Data Box Edge/Data Box Gateway-Dienst:** Die Verwaltungsressource zum Erstellen des Geräteauftrags, zum Konfigurieren des Geräts sowie zum anschließenden Nachverfolgen des Auftrags bis zur Erfüllung.
- **Data Box Edge-Gerät:** Das Übertragungsgerät, das an Sie gesendet wird, um Ihre lokalen Daten in Azure zu importieren.
- **Mit dem Gerät verbundene Clients/Hosts:** Die Clients in Ihrer Infrastruktur, die mit dem Data Box Edge-Gerät verbunden werden und zu schützende Daten enthalten.
- **Cloud-Speicher** – Der Speicherort in der Azure-Cloud, an dem die Daten gespeichert werden. Hierbei handelt es sich in der Regel um das Speicherkonto, das mit der von Ihnen erstellten Data Box Edge-Ressource verknüpft ist.


## <a name="data-box-edgedata-box-gateway-service-protection"></a>Schutz des Data Box Edge/Data Box Gateway-Diensts

Der Data Box Edge/Data Box Gateway-Dienst ist ein in Microsoft Azure gehosteter Verwaltungsdienst. Der Dienst wird zum Konfigurieren und Verwalten des Geräts verwendet.

- Um auf den Data Box Edge/Data Box Gateway-Dienst zugreifen zu können, muss Ihre Organisation über ein EA-Abonnement (Enterprise Agreement) oder über ein CSP-Abonnement (Cloud Solution Provider) verfügen. Weitere Informationen finden Sie unter [Sign up for Microsoft Azure](https://azure.microsoft.com/resources/videos/sign-up-for-microsoft-azure/) (Registrieren für Microsoft Azure).
- Da Ihr Verwaltungsdienst in Azure gehostet wird, ist er durch die Azure-Sicherheitsfeatures geschützt. Weitere Informationen zu den Sicherheitsfeatures von Microsoft Azure finden Sie im [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/security/).
- Bei SDK-Verwaltungsvorgängen steht der Verschlüsselungsschlüssel für Ihre Data Box Edge-/Data Box Gateway-Ressource unter **Geräteeigenschaften** zur Verfügung. Sie können den Verschlüsselungsschlüssel nur anzeigen, wenn Sie über Berechtigungen für die Resource Graph-API verfügen.

## <a name="data-box-edge-device-protection"></a>Schutz des Data Box Edge-Geräts

Das Data Box Edge-Gerät ist ein lokales Gerät, das Sie bei der Datentransformation unterstützt. Hierzu werden die Daten lokal verarbeitet und anschließend an Azure gesendet. Für Ihr Gerät gilt Folgendes:

- Es benötigt einen Aktivierungsschlüssel für den Zugriff auf den Data Box Edge/Data Box Gateway-Dienst.
- Es ist jederzeit durch ein Gerätekennwort geschützt.
- Es ist ein gesperrtes Gerät. BMC und BIOS des Geräts sind kennwortgeschützt (mit eingeschränktem Benutzerzugriff auf das BIOS).
- Sicherer Start ist aktiviert.
- Windows Defender Device Guard wird darauf ausgeführt. Device Guard stellt sicher, dass nur vertrauenswürdige Anwendungen ausgeführt werden können, die Sie in Ihren Codeintegritätsrichtlinien definieren.
- Enthält einen Schlüssel in der vorderen Abdeckung, mit dem das Gerät verschlossen werden kann. Wir empfehlen, dass Sie nach dem Konfigurieren des Geräts die Abdeckung öffnen. Suchen Sie den Schlüssel, und verschließen Sie dann die Abdeckung, um jeden unbefugten Zugriff auf Datenträger zu verhindern, die sich auf der Vorderseite des Geräts befinden.

### <a name="protect-the-device-via-activation-key"></a>Schützen des Geräts mittels Aktivierungsschlüssel

Dem Data Box Edge/Data Box Gateway-Dienst, den Sie in Ihrem Azure-Abonnement erstellt haben, können nur autorisierte Data Box Edge-Geräte hinzugefügt werden. Um ein Gerät zu autorisieren, müssen Sie es mithilfe eines Aktivierungsschlüssels für den Data Box Edge/Data Box Gateway-Dienst aktivieren. Weitere Informationen finden Sie unter [Abrufen des Aktivierungsschlüssels](data-box-edge-deploy-prep.md#get-the-activation-key).

Für den verwendeten Aktivierungsschlüssel gilt Folgendes:

- Er ist ein auf Azure Active Directory (AAD) basierender Authentifizierungsschlüssel.
- Er läuft nach drei Tagen ab.
- Er wird nach der Geräteaktivierung nicht mehr verwendet.
 
Nach der Aktivierung eines Geräts werden Token für die Kommunikation mit Microsoft Azure verwendet.

### <a name="protect-the-device-via-password"></a>Schützen des Geräts mittels Kennwort

Kennwörter sorgen dafür, dass nur autorisierte Benutzer auf Ihre Daten zugreifen können. Data Box Edge- und Data Box Gateway-Geräte sind nach dem Start gesperrt.

Ihre Möglichkeiten:

- Stellen Sie über einen Browser eine Verbindung mit der lokalen Webbenutzeroberfläche des Geräts her, und geben Sie ein Kennwort an, um sich bei dem Gerät anzumelden.
- Stellen Sie eine HTTP-Remoteverbindung mit der PowerShell-Schnittstelle des Geräts her. Die Remoteverwaltung ist standardmäßig aktiviert. Anschließend können Sie das Gerätekennwort angeben, um sich bei dem Gerät anzumelden. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit der PowerShell-Schnittstelle](data-box-edge-connect-powershell-interface.md#connect-to-the-powershell-interface).

Beachten Sie die folgenden bewährten Methoden:

- Der Data Box Edge-Dienst kann keine vorhandenen Kennwörter abrufen; er kann sie nur über das Azure-Portal zurücksetzen. Es wird empfohlen, alle Kennwörter an einem sicheren Ort zu speichern, damit Sie ein Kennwort nicht zurücksetzen müssen, wenn Sie es vergessen haben. Wenn Sie ein Kennwort zurücksetzen, vergessen Sie nicht, alle Benutzer zuvor zu benachrichtigen.
- Verwenden Sie zum [Ändern des Kennworts](data-box-gateway-manage-access-power-connectivity-mode.md#manage-device-access) die lokale Webbenutzeroberfläche. Sollten Sie das Kennwort ändern, benachrichtigen Sie alle Benutzer mit Remotezugriff, damit bei ihnen keine Anmeldefehler auftreten.
- Auf die Windows PowerShell-Oberfläche Ihres Geräts können Sie remote über HTTP zugreifen. Aus Sicherheitsgründen empfiehlt es sich, HTTP nur in vertrauenswürdigen Netzwerken zu verwenden.
- Gerätekennwörter müssen sicher und gut geschützt sein. Berücksichtigen Sie die [bewährten Methoden für Kennwörter](https://docs.microsoft.com/azure/security/azure-security-identity-management-best-practices#enable-password-management).

## <a name="protect-the-data"></a>Schützen der Daten

In diesem Abschnitt werden die Data Box Edge-Sicherheitsfeatures für Daten während der Übertragung sowie für gespeicherte Daten beschrieben.

### <a name="protect-data-at-rest"></a>Schutz gespeicherter Daten

Für ruhende Daten gilt Folgendes:

- Für ruhende Daten verwendet Data Box Edge die XTS-AES-BitLocker-Verschlüsselung mit 256 Bit, um die lokalen Daten zu schützen.
- Für die Daten, die auf Freigaben gespeichert sind, ist der Zugriff auf die Freigaben eingeschränkt.

    - Für SMB-Clients, die auf die Freigabedaten zugreifen, sind Benutzeranmeldeinformationen erforderlich, die der Freigabe zugeordnet sind. Diese Anmeldeinformationen werden bei der Freigabeerstellung definiert.
    - Für NFS-Clients, die auf die Freigaben zugreifen, müssen bei der Freigabeerstellung die IP-Adressen der Clients hinzugefügt werden.


### <a name="protect-data-in-flight"></a>Schützen von Daten während der Übertragung

Während der Übertragung gilt für Daten Folgendes:

- Für Daten, die zwischen dem Gerät und Azure übertragen werden, wird TLS 1.2 (Standard) verwendet. Es erfolgt kein Fallback auf TLS 1.1 oder auf ältere Versionen. Sollte TLS 1.2 nicht unterstützt werden, wird die Agent-Kommunikation blockiert. TLS 1.2 wird auch für portal- und SDK-basierte Verwaltungsvorgänge benötigt.
- Wenn die Clients über die lokale Webbenutzeroberfläche in einem Browser auf Ihr Gerät zugreifen, wird TLS 1.2 (Standard) als sicheres Standardprotokoll verwendet.

    - Es empfiehlt sich, Ihren Browser für die Verwendung von TLS 1.2 zu konfigurieren.
    - Sollte der Browser TLS 1.2 nicht unterstützen, können Sie TLS 1.1 oder TLS 1.0 verwenden.
- Um die Daten während des Kopierens von Ihren Datenservern zu schützen, empfiehlt sich die Verwendung von SMB 3.0 mit Verschlüsselung.

### <a name="protect-data-via-storage-accounts"></a>Schützen von Daten über Speicherkonten

Ihr Gerät ist einem Speicherkonto zugeordnet, das als Ziel für Ihre Daten in Azure verwendet wird. Der Zugriff auf das Speicherkonto wird über das Abonnement und zwei Zugriffsschlüssel mit 512 Bit gesteuert, die dem Speicherkonto zugeordnet sind.

Einer der Schlüssel dient zur Authentifizierung, wenn das Data Box Edge-Gerät auf das Speicherkonto zugreift. Der andere Schlüssel dient als Reserve und gestattet Ihnen, die Schlüssel regelmäßig zu rotieren.

Aus Sicherheitsgründen ist in vielen Datencentern eine Schlüsselrotation erforderlich. Es wird empfohlen, diese bewährten Methoden für die Schlüsselrotation zu befolgen:

- Ihr Speicherkontoschlüssel ähnelt dem Stammkennwort für das Speicherkonto. Achten Sie darauf, dass Ihr Kontoschlüssel immer gut geschützt ist. Geben Sie das Kennwort nicht an andere Benutzer weiter, vermeiden Sie das Hartcodieren, und speichern Sie es nicht als Klartext an einem Ort, auf den andere Benutzer Zugriff haben.
- [Generieren Sie Ihren Kontoschlüssel über das Azure-Portal neu](../storage/common/storage-account-manage.md#regenerate-access-keys), wenn Sie der Meinung sind, dass er nicht mehr sicher ist.
- Rotieren und [synchronisieren Sie Ihre Speicherkontoschlüssel](data-box-gateway-manage-shares.md#sync-storage-keys) regelmäßig, um zu vermeiden, dass nicht autorisierte Benutzer auf Ihr Speicherkonto zugreifen können.
- Der Azure-Administrator sollte den primären oder sekundären Schlüssel regelmäßig über den Abschnitt „Storage“ im Azure-Portal neu generieren, um direkt auf das Speicherkonto zuzugreifen.


## <a name="manage-personal-information"></a>Verwalten persönlicher Informationen

Der Data Box Edge/Data Box Gateway-Dienst erfasst persönliche Informationen in folgenden Fällen:

- **Auftragsdetails**: Nach der Erstellung des Auftrags werden die Versandadresse, die E-Mail-Adresse und die Kontaktinformationen von Benutzern im Azure-Portal gespeichert. Zu den gespeicherten Informationen gehört Folgendes:
  - Kontaktname
  - Telefonnummer
  - E-Mail
  - Anschrift
  - City
  - Postleitzahl
  - Zustand
  - Land/Provinz/Region
  - Nachverfolgungsnummer für den Versand

    Die Auftragsdetails werden verschlüsselt und im Dienst gespeichert. Der Dienst bewahrt die Informationen auf, bis Sie die Ressource oder den Auftrag explizit löschen. Zwischen dem Versand des Geräts und dem Zeitpunkt, zu dem das Gerät wieder bei Microsoft eingeht, ist das Löschen der Ressource und des entsprechenden Auftrags nicht möglich.

- **Lieferanschrift**: Nach der Auftragserteilung gibt der Data Box-Dienst die Lieferanschrift an externe Transportdienstleister (beispielsweise UPS) weiter.

- **Freigabebenutzer**: Benutzer auf Ihrem Gerät können auch auf die Daten in den Freigaben zugreifen. Eine Liste mit Benutzern, die auf die Freigabedaten zugreifen können, kann angezeigt werden. Wenn die Freigaben gelöscht werden, wird diese Liste ebenfalls gelöscht. Eine Anleitung zum Anzeigen der Liste mit den Benutzern, die auf eine Freigabe zugreifen oder sie löschen können, finden Sie unter [Verwalten von Freigaben auf Ihrer Azure Data Box Gateway-Ressource über das Azure-Portal](data-box-gateway-manage-shares.md).

Weitere Informationen finden Sie im [Trust Center](https://www.microsoft.com/trustcenter) in der Microsoft-Datenschutzrichtlinie.

## <a name="next-steps"></a>Nächste Schritte

[Tutorial: Vorbereiten der Bereitstellung von Azure Data Box Edge](data-box-edge-deploy-prep.md)

