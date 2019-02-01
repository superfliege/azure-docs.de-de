---
title: Azure Storage-Explorer – Leitfaden zur Problembehandlung | Microsoft-Dokumentation
description: Übersicht über die beiden Debugfeatures von Azure
services: virtual-machines
author: Deland-Han
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.subservice: common
ms.openlocfilehash: 180780c3a3a644a8da0fa544c37bc8cd252c982f
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55469497"
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a>Azure Storage-Explorer – Leitfaden zur Problembehandlung

Bei Microsoft Azure Storage-Explorer handelt es sich um eine eigenständige App, über die Sie unter Windows, macOS und Linux komfortabel mit Azure Storage-Daten arbeiten können. Die App kann mit Storage-Konten verbunden werden, die in Azure, nationalen Clouds und Azure Stack gehostet werden.

In diesem Leitfaden sind Lösungen für häufig aufgetretene Probleme im Storage-Explorer zusammengefasst.

## <a name="error-self-signed-certificate-in-certificate-chain-and-similar-errors"></a>Fehler selbstsigniertes Zertifikat in der Zertifikatkette (und vergleichbare Fehler)

Zertifikatfehler werden durch eine der beiden folgenden Situationen verursacht:

1. Die App ist über einen „transparenten Proxy“ verbunden, d.h., ein Server (z.B. Ihr Firmenserver) fängt HTTPS-Datenverkehr ab, entschlüsselt ihn und verschlüsselt ihn dann mit einem selbstsignierten Zertifikat.
2. Sie führen eine Anwendung aus, die ein selbstsigniertes SSL-Zertifikat in die empfangenen HTTPS-Nachrichten einfügt. Beispiele für Anwendungen, die Zertifikate einfügen, sind u. a. Antivirenprogramme und Prüfsoftware für den Netzwerkverkehr.

Wenn Storage-Explorer ein selbstsigniertes oder nicht vertrauenswürdiges Zertifikat sieht, kann er nicht mehr erkennen, ob die empfangene HTTPS-Nachricht geändert wurde. Wenn Sie über eine Kopie des selbstsignierten Zertifikats verfügen, können Sie Storage-Explorer anweisen, es als vertrauenswürdig zu behandeln. Führen Sie dazu die folgenden Schritte aus:

1. Abrufen einer X.509-Kopie (CER) des Zertifikats mit Base64-Verschlüsselung
2. Klicken Sie auf **Bearbeiten** > **SSL-Zertifikate** > **Zertifikate importieren**, suchen Sie über die Dateiauswahl die CER-Datei, wählen Sie sie aus, und öffnen Sie sie.

Dieses Problem kann auch das Ergebnis mehrerer Zertifikate (Stamm- und Zwischenzertifikat) sein. Beide Zertifikate müssen hinzugefügt werden, um den Fehler zu beheben.

Wenn Sie nicht sicher sind, woher das Zertifikat kommt, können Sie zur Lösung die folgenden Schritte ausführen:

1. Installieren Sie OpenSSL.

    * [Windows](https://slproweb.com/products/Win32OpenSSL.html) (eine Light-Version sollte ausreichend sein)
    * Mac und Linux: sollte im Betriebssystem enthalten sein
2. Führen Sie OpenSSL aus.

    * Windows: Öffnen Sie das Installationsverzeichnis, klicken Sie auf **/bin/**, und doppelklicken Sie dann auf **openssl.exe**.
    * Mac und Linux: Führen Sie **openssl** über ein Terminal aus.
3. Führen Sie `s_client -showcerts -connect microsoft.com:443` aus.
4. Suchen Sie nach selbstsignierten Zertifikaten. Wenn Sie nicht sicher sind, welche Zertifikate selbstsigniert sind, suchen Sie nach Stellen, an denen der Antragssteller (`("s:")`) und der Zertifikataussteller (`("i:")`) identisch sind.
5. Wenn Sie selbstsignierte Zertifikate gefunden haben, kopieren Sie für jedes Zertifikat den gesamten Inhalt von einschließlich **-----BEGIN CERTIFICATE-----** bis **-----END CERTIFICATE-----**, und fügen Sie ihn in eine neue CER-Datei ein.
6. Öffnen Sie den Storage-Explorer, klicken Sie auf **Bearbeiten** > **SSL-Zertifikate** > **Zertifikate importieren**, suchen Sie über die Dateiauswahl die erstellten CER-Dateien, wählen Sie sie aus, und öffnen Sie sie.

Wenn Sie mit den oben beschriebenen Schritten keine selbstsignierten Zertifikate finden, wenden Sie sich über das Feedbacktool an uns, um weitere Unterstützung zu erhalten. Alternativ können Sie Storage-Explorer auch über die Befehlszeile mit dem `--ignore-certificate-errors`-Flag starten. Wenn Azure Storage-Explorer mit diesem Flag gestartet wird, ignoriert er Zertifikatfehler.

## <a name="sign-in-issues"></a>Probleme bei der Anmeldung

### <a name="reauthentication-loop-or-upn-change"></a>Schleife für eine erneute Authentifizierung oder Änderung des UPN
Wenn Sie sich in einer Schleife für eine erneute Authentifizierung befinden oder den UPN von einem Ihrer Konten geändert haben, versuchen Sie Folgendes:
1. Entfernen Sie alle Konten, und schließen Sie dann den Storage-Explorer.
2. Löschen Sie den Ordner „.IdentityService“ von Ihrem Computer. Der Ordner befindet sich unter Windows unter `C:\users\<username>\AppData\Local`. Bei Mac und Linux finden Sie den Ordner im Stammverzeichnis Ihres Benutzerverzeichnisses.
3. Wenn Sie Mac oder Linux verwenden, müssen Sie auch den Eintrag „Microsoft.Developer.IdentityService“ aus Ihrem Betriebssystemkeystore löschen. Unter Mac ist der Keystore die Anwendung „Gnome Keychain“. Bei Linux wird die Anwendung in der Regel als „Schlüsselbund“ bezeichnet, der Name kann jedoch abhängig von Ihrer Distribution abweichen.

### <a name="conditional-access"></a>Bedingter Zugriff
Bedingter Zugriff wird nicht unterstützt, wenn Storage-Explorer unter Windows 10, Linux oder macOS verwendet wird. Dies liegt an einer Einschränkung der AAD-Bibliothek, die Storage-Explorer verwendet.

## <a name="mac-keychain-errors"></a>Mac-Keychainfehler
Die macOS-Keychain kann sich manchmal in einem Zustand befinden, der Probleme in Verbindung mit der Storage Explorer-Authentifizierungsbibliothek verursacht. Versuchen Sie, diesen Zustand der Keychain mit folgenden Schritte aufzuheben:
1. Schließen Sie Storage Explorer.
2. Öffnen Sie die Keychain (**BEFEHL+ LEER**, geben Sie die Keychain ein, drücken Sie EINGABE).
3. Wählen Sie den Keychain-Wert „login“ (Anmelden).
4. Klicken Sie auf das Vorhängeschlosssymbol, um die Keychain zu sperren (das Schloss wird nach Abschluss des Vorgangs in gesperrter Position dargestellt; dies kann ein paar Sekunden dauern, je nachdem, welche Apps Sie geöffnet haben).

    ![image](./media/storage-explorer-troubleshooting/unlockingkeychain.png)

5. Starten Sie Storage Explorer.
6. Ein Popupfenster mit einem Text wie dem Folgenden sollte angezeigt werden: „Servicehub möchte auf den Schlüsselbund zugreifen.“ Geben Sie in diesem Fall das Kennwort Ihres Mac-Administratorkontos ein, und klicken Sie auf **Immer zulassen** (oder **Zulassen**, falls **Immer zulassen** nicht verfügbar ist).
7. Versuchen Sie, sich anzumelden.

### <a name="general-sign-in-troubleshooting-steps"></a>Allgemeine Schritte zur Behandlung von Anmeldeproblemen
* Wenn Sie unter macOS arbeiten und im Anmeldefenster nie das Dialogfeld „Warten auf Authentifizierung...“ angezeigt wird, versuchen Sie diese [Schritte](#mac-keychain-errors).
* Neustart von Storage-Explorer
* Wenn das Fenster „Authentifizierung“ leer ist, warten Sie mindestens eine Minute, bevor Sie das Dialogfeld für die Authentifizierung schließen.
* Stellen Sie sicher, dass Ihre Proxy- und Zertifikateinstellungen für Ihren Computer und den Storage-Explorer ordnungsgemäß konfiguriert sind.
* Wenn Sie unter Windows arbeiten und auf dem gleichen Computer auf Visual Studio 2017 zugreifen und sich anmelden können, versuchen Sie, sich bei Visual Studio 2017 anzumelden. Nach einer erfolgreichen Anmeldung bei Visual Studio 2017 sollten Sie den Storage-Explorer öffnen und Ihr Konto im Kontobereich sehen können.

Wenn keine dieser Methoden funktioniert, [eröffnen Sie auf GitHub ein Problem](https://github.com/Microsoft/AzureStorageExplorer/issues).

### <a name="missing-subscriptions-and-broken-tenants"></a>Fehlende Abonnements und fehlerhafte Mandanten

Wenn Sie nach erfolgreicher Anmeldung Ihre Abonnements nicht abrufen können, probieren Sie die folgenden Methoden zur Problembehebung:

* Überprüfen Sie, ob Ihr Konto Zugriff auf die erwarteten Abonnements hat. Sie können den Zugriff überprüfen, indem Sie sich am Portal für die Azure-Umgebung anmelden, die Sie verwenden möchten.
* Stellen Sie sicher, dass die Anmeldung mit der richtigen Azure-Umgebung (Azure, Azure China, Azure Deutschland, Azure US-Regierung oder Benutzerdefinierte Umgebung) erfolgt ist.
* Wenn Sie sich hinter einem Proxy befinden, stellen Sie sicher, dass Sie den Storage-Explorer-Proxy richtig konfiguriert haben.
* Entfernen Sie das Konto, und fügen Sie es wieder hinzu.
* Wenn ein Link namens „Weitere Informationen“ vorhanden ist, prüfen Sie, welche Fehlermeldungen für die fehlerhaften Mandanten gemeldet werden. Wenn Sie sich nicht sicher sind, wie Sie mit den angezeigten Fehlermeldungen umgehen sollen, können Sie jederzeit [ein Thema auf GitHub erstellen](https://github.com/Microsoft/AzureStorageExplorer/issues).

## <a name="cannot-remove-attached-account-or-storage-resource"></a>Zugeordnetes Konto oder verbundene Speicherressource kann nicht entfernt werden

Wenn ein zugeordnetes Konto oder eine verbundene Speicherressource über die Benutzeroberfläche nicht entfernt werden kann, können Sie alle zugeordneten Ressourcen manuell löschen, indem Sie die folgenden Ordner löschen:

* Windows: `%AppData%/StorageExplorer`
* macOS: `/Users/<your_name>/Library/Application Support/StorageExplorer`
* Linux: `~/.config/StorageExplorer`

> [!NOTE]
>  Schließen Sie Storage-Explorer, bevor Sie die oben genannten Ordner löschen.

> [!NOTE]
>  Falls Sie irgendwann SSL-Zertifikate importiert haben, sichern Sie den Inhalt des Verzeichnisses `certs`. Anschließend können Sie mithilfe der Sicherung die SSL-Zertifikate wieder importieren.

## <a name="proxy-issues"></a>Proxyprobleme

Vergewissern Sie sich zunächst, dass Sie die folgenden Informationen richtig eingegeben haben:

* Proxy-URL und Portnummer
* Benutzername und Kennwort, sofern für den Proxy erforderlich

Beachten Sie, dass der Storage-Explorer keine PAC-Dateien für die Konfiguration von Proxyeinstellungen unterstützt.

### <a name="common-solutions"></a>Gängige Lösungen

Wenn weiterhin Probleme auftreten, probieren Sie die folgenden Methoden zur Problembehandlung:

* Wenn Sie ohne Verwendung des Proxys eine Verbindung mit dem Internet herstellen können, überprüfen Sie, ob der Storage-Explorer ohne aktivierte Proxyeinstellungen ausgeführt wird. Wenn dies der Fall ist, liegt möglicherweise ein Problem mit den Proxyeinstellungen vor. Identifizieren Sie die Probleme zusammen mit dem Administrator für den Proxyserver.
* Überprüfen Sie, ob andere Anwendungen, die den Proxyserver verwenden, wie erwartet ausgeführt werden.
* Prüfen Sie, ob Sie sich für die Azure-Umgebung, die Sie verwenden möchten, am Portal anmelden können.
* Vergewissern Sie sich, dass Antworten von den Dienstendpunkten empfangen werden können. Geben Sie eine der Endpunkt-URLs im Browser ein. Wenn Sie eine Verbindung herstellen können, sollten Sie eine XML-Antwort wie „InvalidQueryParameterValue“ oder eine ähnliche erhalten.
* Wenn eine andere Person auch den Storage-Explorer mit Ihrem Proxyserver verwendet, überprüfen Sie, ob diese Person eine Verbindung herstellen kann. Wenn dies der Fall ist, müssen Sie sich möglicherweise an den Administrator für den Proxyserver wenden.

### <a name="tools-for-diagnosing-issues"></a>Tools zum Diagnostizieren von Problemen

Wenn Sie über Netzwerktools verfügen, z.B. Fiddler für Windows, können Sie die Probleme unter Umständen wie folgt diagnostizieren:

* Wenn Sie über den Proxy arbeiten müssen, müssen Sie das Netzwerktool möglicherweise so konfigurieren, dass eine Verbindung über den Proxy hergestellt wird.
* Überprüfen Sie die vom Netzwerktool verwendete Portnummer.
* Geben Sie die URL des lokalen Hosts und die Portnummer des Netzwerktools als Proxyeinstellungen im Storage-Explorer ein. Bei korrekter Eingabe beginnt das Netzwerktool mit der Protokollierung der vom Storage-Explorer an Verwaltungs- und Dienstendpunkte gesendeten Netzwerkanforderungen. Wenn Sie in einem Browser beispielsweise https://cawablobgrs.blob.core.windows.net/ für den Blogendpunkt eingeben, erhalten Sie eine Antwort ähnlich der folgenden, die besagt, dass die Ressource vorhanden ist, Sie jedoch keinen Zugriff darauf haben.

![Codebeispiel](./media/storage-explorer-troubleshooting/4022502_en_2.png)

### <a name="contact-proxy-server-admin"></a>Kontaktaufnahme mit dem Administrator für den Proxyserver

Wenn Ihre Proxyeinstellungen richtig sind, müssen Sie sich möglicherweise an den Administrator für den Proxyserver wenden und

* sicherstellen, dass der Proxy nicht den Datenverkehr zu Azure Verwaltungs- oder Ressourcenendpunkten blockiert.
* das von Ihrem Proxyserver verwendete Authentifizierungsprotokoll überprüfen. NTLM-Proxys werden derzeit vom Storage-Explorer nicht unterstützt.

## <a name="unable-to-retrieve-children-error-message"></a>Fehlermeldung: Untergeordnete Elemente können nicht abgerufen werden

Wenn Sie die Verbindung mit Azure über einen Proxy hergestellt haben, überprüfen Sie, ob die Proxyeinstellungen richtig sind. Wenn Ihnen vom Besitzer des Abonnements oder Kontos Zugriff auf eine Ressource erteilt wurde, vergewissern Sie sich, dass Sie über die Berechtigung zum Lesen oder Auflisten für diese Ressource verfügen.

## <a name="connection-string-does-not-have-complete-configuration-settings"></a>Verbindungszeichenfolge weist keine vollständigen Konfigurationseinstellungen auf.

Wenn Sie diese Fehlermeldung erhalten, verfügen Sie möglicherweise nicht über die erforderlichen Berechtigungen zum Abrufen der Schlüssel für Ihr Speicherkonto. Um zu überprüfen, ob dies der Fall ist, navigieren Sie zum Portal, und suchen Sie Ihr Speicherkonto. Klicken Sie dazu einfach mit der rechten Maustaste auf den Knoten für Ihr Speicherkonto, und klicken Sie dann auf „Open in Portal“ (Im Portal öffnen). Wechseln Sie dann zum Blatt „Zugriffsschlüssel“. Wenn Sie nicht über Berechtigungen zum Anzeigen von Schlüsseln verfügen, wird eine Seite mit der Meldung „Sie haben keinen Zugriff“ angezeigt. Um dieses Problem zu umgehen, können Sie den Kontoschlüssel von einer anderen Person erhalten und mit Name und Schlüssel anfügen, oder Sie können eine andere Person um die SAS für das Speicherkonto bitten und mit dieser das Speicherkonto anfügen.

Wenn die Kontoschlüssel angezeigt werden, melden Sie ein Problem bei GitHub, damit wir Ihnen beim Beheben des Problems helfen können.

## <a name="issues-with-sas-url"></a>Probleme mit der SAS-URL
Wenn Sie eine Verbindung mit einem Dienst über eine SAS-URL herstellen und dieser Fehler auftritt:

* Stellen Sie sicher, dass die URL die erforderlichen Berechtigungen zum Lesen oder Auflisten von Ressourcen umfasst.
* Vergewissern Sie sich, dass die URL nicht abgelaufen ist.
* Wenn die SAS-URL auf einer Zugriffsrichtlinie basiert, stellen Sie sicher, dass die Zugriffsrichtlinie nicht aufgehoben wurde.

Wenn Sie versehentlich eine ungültige SAS-URL angefügt haben und diese nicht trennen können, führen Sie diese Schritte aus:
1.  Drücken Sie im Storage-Explorer F12, um das Fenster mit den Entwicklertools zu öffnen.
2.  Klicken Sie auf der Registerkarte „Anwendung“ und dann in der Struktur auf der linken Seite auf „Lokaler Speicher“ > „file://“.
3.  Suchen Sie den Schlüssel, der dem Diensttyp des problematischen SAS-URI zugeordnet ist. Wenn z.B. der fehlerhafte SAS-URI für einen Blobcontainer gilt, suchen Sie nach dem Schlüssel mit dem Namen `StorageExplorer_AddStorageServiceSAS_v1_blob`.
4.  Der Wert des Schlüssels sollte ein JSON-Array sein. Suchen Sie das Objekt, das dem fehlerhaften URI zugeordnet ist, und entfernen Sie es.
5.  Drücken Sie STRG+R, um den Storage-Explorer neu zu laden.

## <a name="linux-dependencies"></a>Linux-Abhängigkeiten

Für andere Linux-Distributionen als Ubuntu 16.04 müssen Sie einige Abhängigkeiten ggf. manuell installieren. Die folgenden Pakete sind im Allgemeinen erforderlich:
* [.NET Core 2.x](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x)
* `libsecret`
* `libgconf-2-4`
* Aktuelle GCC

Abhängig von Ihrer Distribution müssen Sie möglicherweise weitere Pakete installieren. Die [Anmerkungen zu dieser Version](https://go.microsoft.com/fwlink/?LinkId=838275&clcid=0x409) des Storage-Explorers enthalten spezielle Schritte für einige Distributionen.

## <a name="next-steps"></a>Nächste Schritte

Wenn keine der Lösungen funktioniert, [eröffnen Sie auf GitHub ein Problem](https://github.com/Microsoft/AzureStorageExplorer/issues). Sie können auch über die Schaltfläche „Problem auf GitHub melden“ unten links schnell auf GitHub zugreifen.

![Feedback](./media/storage-explorer-troubleshooting/feedback-button.PNG)
