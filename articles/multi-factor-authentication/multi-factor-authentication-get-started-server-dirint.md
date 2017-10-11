---
title: Directory-Integration von Azure Multi-Factor Authentication und Active Directory
description: "Dies ist der Azure Multi-Factor Authentication-Seite, die beschreibt, wie Azure Multi-Factor Authentication-Server mit Active Directory integriert, damit Sie die Verzeichnisse synchronisieren können."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: def7a534-cfb2-492a-9124-87fb1148ab1f
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/16/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: c469dfaccf515bcd1ced43279decfefe6be8375b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="directory-integration-between-azure-mfa-server-and-active-directory"></a>Directory-Integration von Azure MFA-Server und Active Directory
Verwenden von Azure MFA Server Abschnitts "Verzeichnisintegration" für die Integration von Active Directory oder ein anderes LDAP-Verzeichnis an. Sie können konfigurieren, Attribute, um dem Verzeichnisschema entsprechen und die automatische Synchronisierung einrichten.

## <a name="settings"></a>Einstellungen
Standardmäßig ist der Azure Multi-Factor Authentication (MFA)-Server zum Importieren oder Synchronisieren von Benutzern aus Active Directory konfiguriert.  Die Registerkarte "Verzeichnisintegration" können Sie das Standardverhalten überschreiben und an ein anderes LDAP-Verzeichnis, ein ADAM-Verzeichnis oder einen bestimmten Active Directory-Domänencontroller binden.  Darüber hinaus für die Verwendung von LDAP-Authentifizierung für LDAP-Proxyvorgänge oder zur LDAP-Bindung als RADIUS-Ziel, Vorauthentifizierung für IIS-Authentifizierung oder primäre Authentifizierung für das Benutzerportal.  Die folgende Tabelle beschreibt die einzelnen Einstellungen.

![Einstellungen](./media/multi-factor-authentication-get-started-server-dirint/dirint.png)

| Funktion | Description |
| --- | --- |
| Verwenden Sie Active Directory |Wählen Sie die Option Active Directory verwenden, verwenden Sie Active Directory für Import und Synchronisierung.  Dies ist die Standardeinstellung. <br>Hinweis: Für die Active Directory-Integration ordnungsgemäß funktioniert, fügen Sie den Computer einer Domäne hinzu, und melden Sie sich mit einem Domänenkonto. |
| Vertrauenswürdige Domänen einschließen |Überprüfen Sie **vertrauenswürdige Domänen einschließen** beteiligt sind, haben den Agent, Domänen, denen der aktuellen Domäne, einer anderen Domäne in der Gesamtstruktur oder zwischen Domänen Herstellen einer Verbindung mit einer Gesamtstruktur-Vertrauensstellung.  Wenn Sie nicht importieren oder Synchronisieren von Benutzern aus einer vertrauenswürdigen Domäne, deaktivieren Sie das Kontrollkästchen, um die Leistung zu verbessern.  Standardmäßig ist diese Option aktiviert. |
| Spezifische LDAP-Konfiguration verwenden |Wählen Sie die Option "LDAP verwenden" ", die zum Importieren und synchronisieren angegebenen LDAP-Einstellungen verwenden. Hinweis: Wenn "LDAP verwenden" ausgewählt ist, die Benutzeroberfläche Verweise aus Active Directory zu LDAP geändert. |
| Schaltfläche "Bearbeiten" |Die Schaltfläche "Bearbeiten" können die aktuellen LDAP-Konfigurationseinstellungen geändert. |
| Attributbereichsabfragen verwenden |Gibt an, ob attributbereichsabfragen verwendet werden soll.  Attributbereichsabfragen ermöglichen effiziente Verzeichnissuchen Datensätze je nach den Einträgen im Attribut eines anderen Datensatzes qualifiziert.  Azure Multi-Factor Authentication-Server verwendet attributbereichsabfragen zum effizienten Abfragen der Benutzer, die Mitglied einer Sicherheitsgruppe sind.   <br>Hinweis: Es gibt einige Fälle, in denen attributbereichsabfragen werden unterstützt, jedoch sollte nicht verwendet werden.  Beispielsweise kann Active Directory Probleme mit attributbereichsabfragen haben, wenn eine Sicherheitsgruppe Mitglieder aus mehr als eine Domäne enthält. In diesem Fall, deaktivieren Sie das Kontrollkästchen. |

Die folgende Tabelle beschreibt die LDAP-Konfigurationseinstellungen.

| Funktion | Description |
| --- | --- |
| Server |Geben Sie den Hostnamen oder IP-Adresse des Servers, auf dem LDAP-Verzeichnis.  Ein Sicherungsserver kann auch durch ein Semikolon getrennt angegeben werden. <br>Hinweis: Wenn der Bindungstyp SSL ist, muss ein vollständig qualifizierter Hostnamen. |
| Basis-DN |Geben Sie den distinguished Name des basisverzeichnisobjekts an, von dem alle Verzeichnisabfragen gestartet, werden soll.  Zum Beispiel: dc = Abc, dc = com. |
| Bindungstyp – Abfragen |Wählen Sie den geeigneten Bindungstyp für Bindungen zum Durchsuchen des LDAP-Verzeichnisses.  Dies wird für Importe, Synchronisierung und die benutzernamenauflösung verwendet. <br><br>  Anonyme - eine anonyme Bindung ausgeführt wird.  Bindungs-DN und Bindungskennwort werden nicht verwendet.  Dies funktioniert nur, wenn anonyme Bindungen für das LDAP-Verzeichnis zugelassen und die Berechtigungen zur Abfrage der entsprechenden Datensätze und Attribute.  <br><br> Simple - Bindungs-DN und Bindungskennwort werden als nur-Text zum Binden an das LDAP-Verzeichnis übergeben.  Dies ist für Testzwecke wird, stellen Sie sicher, dass der Server erreichbar ist und das Bindungskonto über die erforderlichen Zugriffsrechte verfügt. Nachdem das entsprechende Zertifikat installiert wurde, verwenden Sie stattdessen SSL.  <br><br> SSL - Bindungs-DN und Bindungskennwort werden mithilfe von SSL zum Binden an das LDAP-Verzeichnis verschlüsselt.  Installieren Sie ein Zertifikat lokal, der im LDAP-Verzeichnis vertraut.  <br><br> Windows - Bindungsbenutzername und Bindungskennwort werden verwendet, um sichere Verbindungen mit einem Active Directory-Domänencontroller oder einem ADAM-Verzeichnis herzustellen.  Wenn der Bindungsbenutzername nicht leer ist, wird Konto des angemeldeten Benutzers verwendet, zu binden. |
| Bindungstyp – Authentifizierungen |Wählen Sie den geeigneten Bindungstyp für die Verwendung bei der LDAP-bindungsauthentifizierung aus.  Die Beschreibungen Bindungstypen finden Sie unter Bindungstyp – Abfragen.  Sie können beispielsweise anonymen Bindung für Abfragen verwendet werden, während der SSL-Bindung zur Sicherung von LDAP-bindungsauthentifizierungen ist. |
| Bindungs-DN oder bindungsbenutzername |Geben Sie den distinguished Name des Benutzerdatensatzes für das Konto beim Binden an das LDAP-Verzeichnis verwendet.<br><br>Die Bindungs-DN wird nur verwendet, wenn der Bindungstyp "einfach" und "SSL" ist.  <br><br>Geben Sie den Benutzernamen des Windows-Kontos zu verwendende Bindung an das LDAP-Verzeichnis, wenn der Bindungstyp Windows ist.  Wenn leer, wird Konto des angemeldeten Benutzers verwendet, zu binden. |
| Bindungskennwort |Geben Sie das bindungskennwort für die Bindungs-DN oder den Benutzernamen zum Binden an das LDAP-Verzeichnis ein.  Um das Kennwort für den AdSync-Dienst von Multi-Factor Authentication-Server konfigurieren, Aktivieren der Synchronisierung aus, und stellen Sie sicher, dass der Dienst auf dem lokalen Computer ausgeführt wird.  Das Kennwort wird in den Windows gespeicherten Benutzernamen und Kennwörtern unter dem Konto gespeichert, der AdSync-Dienst von Multi-Factor Authentication-Server ausgeführt wird.  Das Kennwort wird auch gespeichert, unter dem Konto die Benutzeroberfläche des Multi-Factor Authentication-Server ausgeführt wird und unter dem Konto der Multi-Factor Authentication-Serverdienst ausgeführt wird.  <br><br>Das Kennwort wird nur in Windows gespeicherten Benutzernamen und Kennwörtern des lokalen Servers gespeichert ist, wiederholen Sie diesen Schritt auf jedem Multi-Factor Authentication-Server, die Zugriff auf das Kennwort benötigt. |
| Der Abfragegröße |Geben Sie die maximale Größe für die maximale Anzahl von Benutzern, die eine Suche im Verzeichnis zurückgibt.  Dieser Grenzwert sollte die Konfiguration im LDAP-Verzeichnis übereinstimmen.  Für umfangreiche suchen, in dem für die Auslagerung nicht unterstützt, importieren und die Synchronisierung wird versucht, die Benutzer in Batches abrufen.  Wenn das Größenlimit hier Wert ist größer als der im LDAP-Verzeichnis konfigurierte Grenzwert angegebene, werden möglicherweise Benutzer ausgelassen. |
| Schaltfläche "testen" |Klicken Sie auf **testen** Bindung an den LDAP-Server zu testen.  <br><br>Sie müssen nicht wählen die **"LDAP verwenden"** Option aus, um die Bindung zu testen. Dies ermöglicht die Bindung getestet werden, bevor Sie die LDAP-Konfiguration verwenden. |

## <a name="filters"></a>Filter
Filter können Sie Kriterien zum Qualifizieren von Datensätzen, beim Ausführen eines Suchvorgangs im Verzeichnis festlegen.  Wenn Sie den Filter festlegen, können Sie die Objekte beschränken, die Sie synchronisieren möchten.  

![Filter](./media/multi-factor-authentication-get-started-server-dirint/dirint2.png)

Azure Multi-Factor Authentication hat die folgenden drei Filteroptionen:

* **Containerfilter** -Geben Sie die Filterkriterien zum Qualifizieren von containerdatensätzen beim Ausführen eines Suchvorgangs im Verzeichnis verwendet.  Für Active Directory und ADAM wird (| () objectClass=organizationalUnit)(objectClass=container)) wird häufig verwendet.  Verwenden Sie für andere LDAP-Verzeichnisse Filterkriterien, die jeden Typ von Containerobjekt je nach Verzeichnisschema qualifiziert.  <br>Hinweis: Wenn kein ((objectClass=organizationalUnit)(objectClass=container)) wird standardmäßig verwendet.
* **Sicherheits-Gruppenfilter** -Geben Sie die Filterkriterien zum Qualifizieren von Datensätzen Sicherheit beim Ausführen eines Suchvorgangs im Verzeichnis verwendet.  Für Active Directory und ADAM wird (&(objectCategory=Group) (GroupType:1.2.840.113556.1.4.804:=-2147483648)) wird häufig verwendet.  Verwenden Sie für andere LDAP-Verzeichnisse Filterkriterien, die jeden Typ von Sicherheitsgruppenobjekt, je nach Verzeichnisschema qualifiziert.  <br>Hinweis: Wenn kein (&(objectCategory=Group) (GroupType:1.2.840.113556.1.4.804:=-2147483648)) wird standardmäßig verwendet.
* **Benutzerfilter** -Geben Sie die Filterkriterien zum Qualifizieren von Benutzerdatensätzen beim Ausführen eines Suchvorgangs im Verzeichnis verwendet.  Für Active Directory und ADAM, (& (objectClass=user)(objectCategory=Person)) wird häufig verwendet.  Verwenden Sie für andere LDAP-Verzeichnisse (ObjectClass = InetOrgPerson) oder einer ähnlichen, je nach Verzeichnisschema. <br>Hinweis: Wenn das Feld leer gelassen, (& ((objectCategory=Person)(objectClass=user)) wird standardmäßig verwendet.

## <a name="attributes"></a>Attribute
Sie können Attribute nach Bedarf für ein bestimmtes Verzeichnis anpassen.  Dadurch können Sie benutzerdefinierte Attribute hinzufügen, und optimieren die Synchronisierung nur die Attribute, die Sie benötigen. Verwenden Sie den Namen des der Attricute, wie in der Directory-Schema für den Wert von jedem Attributfeld definiert. Die folgende Tabelle enthält zusätzliche Informationen zu jeder Funktion.

Attribute können manuell eingegeben werden und müssen nicht mit Attributen in der Attributliste übereinstimmen.

![Attribute](./media/multi-factor-authentication-get-started-server-dirint/dirint3.png)

| Funktion | Description |
| --- | --- |
| Eindeutiger Bezeichner |Geben Sie den Attributnamen des Attributs ein, das als eindeutiger Bezeichner von Container, Sicherheitsgruppe und Benutzerdatensätzen fungiert.  In Active Directory ist dies normalerweise "objectGUID". Anderen LDAP-Implementierungen können es sich um "entryuuid" o. ä. verwenden.  Der Standardwert ist "objectGUID". |
| Typ des eindeutigen Bezeichners |Wählen Sie den Typ des Attributs ein eindeutiger Bezeichner.  In Active Directory weist das Attribut "objectGUID" den Typ GUID. In anderen LDAP-Implementierungen können ASCII-Bytearray oder Zeichenfolge verwendet werden.  Die Standardeinstellung lautet GUID. <br><br>Es ist wichtig, die dieser Typ ordnungsgemäß festgelegt werden, da Synchronisierungselemente über ihren eindeutigen Bezeichner verwiesen wird. Typ des eindeutigen Bezeichners wird verwendet, um das Objekt direkt in das Verzeichnis zu suchen.  Dieses Typs festlegen in eine Zeichenfolge, wenn das Verzeichnis den Wert tatsächlich speichert, als ein Bytearray aus ASCII-Zeichen verhindert, dass die Synchronisierung nicht ordnungsgemäß ausgeführt. |
| Definierter name |Geben Sie den Attributnamen des Attributs ein, den distinguished Name für jeden Datensatz enthält.  In Active Directory ist dies normalerweise "distinguishedName". Anderen LDAP-Implementierungen können es sich um "entrydn" o. ä. verwenden.  Der Standardwert ist "distinguishedName". <br><br>Wenn ein Attribut, das nur den distinguished Name enthält nicht vorhanden ist, kann das Attribut "ADsPath" verwendet werden.  Der "LDAP: / /\<Server\>/"-Teil des Pfads wird automatisch übrig bleibt, wenn Sie nur den distinguished Name des Objekts. |
| Containername |Geben Sie den Attributnamen des Attributs ein, den Namen eines containerdatensatzes enthält.  Der Wert dieses Attributs wird in der Containerhierarchie angezeigt, wenn aus Active Directory importiert oder Synchronisierungselemente hinzugefügt.  Der Standardwert ist Name. <br><br>Wenn unterschiedliche Container unterschiedliche Attribute als Namen verwendet werden, verwenden Sie Semikolons zum Trennen mehrere Containername-Attribute.  Das erste Container Name-Attribut in einem Containerobjekt gefunden wird verwendet, um die Anzeige des Namens. |
| Name der Sicherheitsgruppe |Geben Sie den Attributnamen des Attributs ein, den Namen in einem sicherheitsgruppendatensatz enthält.  Der Wert dieses Attributs wird in der Liste mit Sicherheitsgruppen angezeigt, wenn aus Active Directory importiert oder Synchronisierungselemente hinzugefügt.  Der Standardwert ist Name. |
| Benutzername |Geben Sie den Attributnamen des Attributs ein, den Benutzernamen in einem Benutzerdatensatz enthält.  Der Wert dieses Attributs wird als Benutzername des Multi-Factor Authentication-Servers verwendet.  Ein zweites Attribut kann als eine Sicherung des ersten angegeben werden.  Das zweite Attribut wird nur verwendet, wenn das erste Attribut keinen Wert für den Benutzer enthält.  Die Standardwerte lauten "userPrincipalName" und "sAMAccountName". |
| Vorname |Geben Sie den Attributnamen des Attributs ein, den Vornamen in einem Benutzerdatensatz enthält.  Die Standardeinstellung lautet "givenName". |
| Nachname |Geben Sie den Attributnamen des Attributs ein, den Nachnamen in einem Benutzerdatensatz enthält.  Die Standardeinstellung lautet "sn". |
| E-Mail-Adresse |Geben Sie den Attributnamen des Attributs ein, die e-Mail-Adresse in einem Benutzerdatensatz enthält.  E-Mail-Adresse wird verwendet, Begrüßung und zu senden an den Benutzer-e-Mails zu aktualisieren.  Der Standardwert ist die e-Mail-Nachrichten. |
| Benutzergruppe |Geben Sie den Attributnamen des Attributs ein, das die Benutzergruppe in einem Benutzerdatensatz enthält.  Benutzergruppe kann verwendet werden, um Benutzer im Agent sowie in Berichten im Multi-Factor Authentication Server-Verwaltungsportal zu filtern. |
| Description |Geben Sie den Attributnamen des Attributs ein, die Beschreibung in einem Benutzerdatensatz enthält.  Beschreibung wird nur für die Suche verwendet.  Der Standardwert ist die Beschreibung. |
| Sprache für Telefonanrufe |Geben Sie den Attributnamen des Attributs ein, das den Kurznamen der für Sprachanrufe für den Benutzer zu verwendenden Sprache enthält. |
| SMS-Sprache |Geben Sie den Attributnamen des Attributs ein, das den Kurznamen der für SMS-Textnachrichten für den Benutzer zu verwendenden Sprache enthält. |
| Sprache der mobilen Anwendung |Geben Sie den Attributnamen des Attributs ein, das den Kurznamen der für Textnachrichten der Telefon-app für den Benutzer zu verwendenden Sprache enthält. |
| Oath-tokensprache |Geben Sie den Attributnamen des Attributs ein, das den Kurznamen der für oath-token-Textnachrichten für den Benutzer zu verwendenden Sprache enthält. |
| Telefon (geschäftlich) |Geben Sie den Attributnamen des Attributs ein, das die Geschäftsnummer in einem Benutzerdatensatz enthält.  Die Standardeinstellung lautet "telephoneNumber". |
| Telefon privat |Geben Sie den Attributnamen des Attributs ein, das die Privatnummer in einem Benutzerdatensatz enthält.  Der Standardwert ist "HomePhone". |
| Pager |Geben Sie den Attributnamen des Attributs ein, das die Pagernummer in einem Benutzerdatensatz enthält.  Die Standardeinstellung lautet "Pager". |
| Mobiltelefon |Geben Sie den Attributnamen des Attributs ein, das die Mobiltelefonnummer in einem Benutzerdatensatz enthält.  Der Standardwert ist mobile. |
| Fax |Geben Sie den Attributnamen des Attributs ein, das die Faxnummer in einem Benutzerdatensatz enthält.  Der Standardwert ist "facsimiletelephonenumber". |
| IP-Telefon |Geben Sie den Attributnamen des Attributs ein, die IP-Telefonnummer in einem Benutzerdatensatz enthält.  Der Standardwert ist "ipPhone". |
| Benutzerdefiniert |Geben Sie den Attributnamen des Attributs ein, das eine benutzerdefinierte Telefonnummer in einem Benutzerdatensatz enthält.  Der Standardwert ist leer. |
| Erweiterung |Geben Sie den Attributnamen des Attributs ein, das die Durchwahl in einem Benutzerdatensatz enthält.  Der Wert des durchwahlfelds wird als die Erweiterung nur die Nummer des primären Anschlusses verwendet.  Der Standardwert ist leer. <br><br>Wenn das durchwahlattribut nicht angegeben ist, können Durchwahlnummern als Teil der telefonattributs einbezogen werden. In diesem Fall vor die Erweiterung mit einem 'X' ausgeführt werden, damit er ordnungsgemäß analysiert ruft.  Beispielsweise würde die 555-123-4567 x890 555-123-4567 als Telefonnummer und 890 als Durchwahl. |
| Schaltfläche Standards wiederherstellen |Klicken Sie auf **Standards wiederherstellen** zurückzugebenden alle Attribute auf ihren Standardwert zurück.  Die Standardwerte sollte mit dem normalen Active Directory- oder ADAM-Schema ordnungsgemäß funktionieren. |

Um Attribute zu bearbeiten, klicken Sie auf **bearbeiten** auf der Registerkarte "Attribute".  Daraufhin wird ein Fenster, in dem Sie die Attribute bearbeiten können. Wählen Sie die **...**  neben jedes Attribut, um ein Fenster zu öffnen, in dem können Sie auswählen, welche Attribute anzuzeigen.

![Bearbeiten von Attributen](./media/multi-factor-authentication-get-started-server-dirint/dirint4.png)

## <a name="synchronization"></a>Synchronization
Die Synchronisierung hält die Azure MFA-Benutzerdatenbank mit Benutzern in Active Directory oder einem anderen Lightweight Directory Access Protocol (LDAP)-Verzeichnis. Der Prozess ähnelt dem manuellen Importieren von Benutzern aus Active Directory jedoch Active Directory-Benutzer und sicherheitsgruppenänderungen verarbeiten regelmäßig abgerufen.  Zusätzlich werden deaktiviert oder entfernt Benutzer, die aus einem Container, Sicherheitsgruppe oder Active Directory entfernt wurden.

Der AdSync-Dienst von Multi-Factor Authentication-Dienst ist ein Windows-Dienst, der den regelmäßigen Abruf von Active Directory ausführt.  Dies ist nicht zu verwechseln mit Azure AD Sync oder Azure AD Connect.  der Multi-Factor Authentication ADSync bezieht sich zwar auf einer ähnlichen Codebasis erstellt auf der Azure Multi-Factor Authentication-Server.  Es wird im Status "beendet" installiert und konfiguriert den Multi-Factor Authentication-Server-Dienst zur Ausführung gestartet wird.  Wenn Sie eine Multi-Factor Authentication-Server-Konfiguration mit mehreren Servern verwendet wird, kann der AdSync-Dienst des Multi-Factor Authentication nur auf einem einzelnen Server ausgeführt werden.

Der AdSync-Dienst von Multi-Factor Authentication-Dienst verwendet die DirSync-LDAP-servererweiterung, die von Microsoft bereitgestellt, um Änderungen effizient abzurufen.  Dieser DirSync-steuerungsaufrufer muss "Verzeichnis Änderungen abrufen" direkt und DS-Replication-Get-Changes erweiterte Zugriffssteuerungsrecht.  Standardmäßig sind diese Rechte den Konten Administrator und "LocalSystem" auf Domänencontrollern zugewiesen.  Der AdSync-Dienst von Multi-Factor Authentication-Dienst wird standardmäßig zum Ausführen als "LocalSystem" konfiguriert.  Aus diesem Grund ist es am einfachsten, den Dienst auf einem Domänencontroller auszuführen.  Wenn Sie den Dienst so, dass immer eine vollständige Synchronisierung konfigurieren, können sie als ein Konto mit weniger Berechtigungen ausführen.  Dies ist weniger effizient, erfordert jedoch weniger Kontoberechtigungen.

Wenn das LDAP-Verzeichnis unterstützt und ist für DirSync, konfiguriert werden, Abrufen von Änderungen für Benutzer und Sicherheitsgruppen Gruppe identisch verwendet werden, wie bei Active Directory.  Wenn das LDAP-Verzeichnis die DirSync-Steuerung nicht unterstützt, wird eine vollständige Synchronisierung bei jedem Zyklus ausgeführt.

![Synchronization](./media/multi-factor-authentication-get-started-server-dirint/dirint5.png)

Die folgende Tabelle enthält zusätzliche Informationen zu jedem Einstellungen auf der Registerkarte Synchronisierung.

| Funktion | Description |
| --- | --- |
| Synchronisierung mit Active Directory aktivieren |Dieses Kontrollkästchen aktiviert, fragt der Multi-Factor Authentication-Serverdienst in regelmäßigen Abständen Active Directory nach Änderungen ab. <br><br>Hinweis: mindestens ein Synchronisierungselement muss hinzugefügt werden und müssen jetzt synchronisieren ausgeführt werden, bevor Sie den Multi-Factor Authentication-Server-Dienst die Verarbeitung von Änderungen gestartet wird. |
| Synchronisieren alle |Geben Sie das Zeitintervall an, das die Multi-Factor Authentication-Serverdienst zwischen dem Abrufen und Verarbeiten von Änderungen abgewartet wird. <br><br> Hinweis: Das angegebene Intervall ist die Zeitdauer zwischen dem Beginn der jeweiligen Zyklen.  Wenn die Änderungen bei der Verarbeitung des Intervalls überschreiten, wird der Dienst sofort wieder einen Abruf. |
| Entfernen von Benutzern nicht mehr in Active Directory |Wenn dieses Kontrollkästchen aktiviert, wird der Multi-Factor Authentication-Serverdienst Active Directory gelöschte benutzertombstones verarbeitet und entfernen die entsprechenden Multi-Factor Authentication-Server-Benutzer. |
| Immer vollständige Synchronisierung ausführen |Wenn dieses Kontrollkästchen aktiviert, wird der Multi-Factor Authentication-Serverdienst immer eine vollständige Synchronisierung auszuführen.  Bei Deaktivierung dieser Option wird der Multi-Factor Authentication-Serverdienst eine inkrementelle Synchronisierung ausgeführt, indem Sie Abfragen nur Benutzer, die geändert wurden.  Die Standardeinstellung ist deaktiviert. <br><br>Bei Deaktivierung dieser Option führt Azure MFA Server inkrementelle Synchronisierung nur, wenn das Verzeichnis die DirSync-Steuerung unterstützt, und das Konto Binden an das Verzeichnis über Berechtigungen zum Ausführen von inkrementeller "DirSync"-Abfragen verfügt.  Wenn das Konto verfügt nicht über die entsprechenden Berechtigungen, oder mehrere Domänen der Synchronisierung beteiligt sind, führt Azure MFA-Server eine vollständige Synchronisierung aus. |
| Müssen Sie Genehmigung durch Administrator erforderlich, wenn mehr als X Benutzer deaktiviert oder entfernt werden |Synchronisierungselemente können so konfiguriert werden, um deaktivieren oder entfernen Sie Benutzer, die nicht mehr Mitglied des Containers oder der Sicherheitsgruppe des Elements sind.  Als Schutzmechanismus kann die Genehmigung durch den Administrator erforderlich sein, wenn die Anzahl der Benutzer zu deaktivieren oder entfernen Sie einen Schwellenwert überschreitet.  Wenn dieses Kontrollkästchen aktiviert, ist eine Genehmigung erforderlich ist, für den angegebenen Schwellenwert.  Der Standard ist 5 und der Bereich liegt zwischen 1 und 999. <br><br> Die Genehmigung wird ermöglicht, indem zuerst eine e-Mail-Benachrichtigung an Administratoren gesendet. Die e-Mail-Benachrichtigung enthält Anweisungen zum Überprüfen und Genehmigen der Deaktivierung oder Entfernung von Benutzern.  Wenn die Benutzeroberfläche des Multi-Factor Authentication-Server gestartet wird, wird es zur Genehmigung aufgefordert. |

Die **jetzt synchronisieren** Schaltfläche können Sie für die angegebenen Synchronisierungselemente eine vollständige Synchronisierung ausgeführt.  Eine vollständige Synchronisierung ist erforderlich, wenn Synchronisierungselemente hinzugefügt, geändert, entfernt oder neu angeordnet werden.  Es ist auch erforderlich, bevor der Multi-Factor Authentication AdSync-Dienst betriebsbereit ist, da damit der Startpunkt festgelegt, aus dem vom Dienst inkrementelle Änderungen abgerufen werden.  Wenn Änderungen an synchronisierungselementen vorgenommen wurden, aber noch keine vollständige Synchronisierung ausgeführt wurde, werden Sie zur sofortigen Synchronisierung aufgefordert werden.

Die **entfernen** Schaltfläche kann der Administrator ein oder mehrere Synchronisierungselemente aus der Elementliste des Multi-Factor Authentication-Server-Synchronisierung zu löschen.

> [!WARNING]
> Ein Datensatz von synchronisierungselementen entfernt wurde, kann nicht wiederhergestellt werden. Sie müssen die Synchronisierung Datensatz erneut hinzufügen, wenn Sie versehentlich gelöscht.

Die Synchronisierung bzw. die Synchronisierungselemente wurden aus Multi-Factor Authentication-Server entfernt.  Der Multi-Factor Authentication-Serverdienst werden die Synchronisierungselemente nicht mehr verarbeitet werden.

Die Schaltflächen nach oben und nach unten können der Administrator die Reihenfolge der Synchronisierungselemente ändern.  Die Reihenfolge ist wichtig, da derselbe Benutzer Mitglied von mehr als einem synchronisierungselement (z. B. einen Container und eine Sicherheitsgruppe) sein kann.  Die Einstellungen für den Benutzer während der Synchronisierung stammen aus dem ersten synchronisierungselement in der Liste, die der Benutzer zugeordnet ist.  Aus diesem Grund sollten die Synchronisierungselemente in der Reihenfolge ihrer Priorität abgelegt werden.

> [!TIP]
> Nach dem Entfernen der Synchronisierungselemente, muss eine vollständige Synchronisierung ausgeführt werden.  Nach dem Sortieren der Synchronisierungselemente, muss eine vollständige Synchronisierung ausgeführt werden.  Klicken Sie auf **jetzt synchronisieren** eine vollständige Synchronisierung ausführen.

## <a name="multi-factor-auth-servers"></a>Multi-Factor Authentication-Server
Zusätzliche Multi-Factor Authentication-Server können eingerichtet werden, um als RADIUS-sicherungsproxys, LDAP-Proxy oder für die IIS-Authentifizierung zu dienen. Die Konfiguration der Synchronisierung wird von allen Agents gemeinsam genutzt. Jedoch möglicherweise nur eine dieser Agents das Multi-Factor Authentication-Server-Dienst wird ausgeführt. Auf dieser Registerkarte können Sie den Multi-Factor Authentication-Server auswählen, die für die Synchronisierung aktiviert werden soll.

![Multi-Factor Authentication-Server](./media/multi-factor-authentication-get-started-server-dirint/dirint6.png)
