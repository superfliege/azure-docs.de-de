---
title: Verwenden der Altersbeschränkung in Azure Active Directory B2C | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Minderjährige identifizieren, die Ihre Anwendung verwenden.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/29/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: a1020dfcb6c8d312001fbdb1c170987e1216c5d5
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49318859"
---
# <a name="using-age-gating-in-azure-ad-b2c"></a>Verwenden einer Altersbeschränkung in Active Directory B2C

>[!IMPORTANT]
>Dieses Feature befindet sich in der privaten Vorschau.  Einzelheiten finden Sie in unserem [Dienstblog](https://blogs.msdn.microsoft.com/azureadb2c/), sobald dieser verfügbar ist, oder wenden Sie sich an AADB2CPreview@microsoft.com.  Verwenden Sie dies NICHT für Produktionsverzeichnisse, da die Nutzung dieser neuen Funktionen zu Datenverlusten und zu unerwarteten Verhaltensänderungen führen kann, solange sie noch nicht allgemein verfügbar sind.  
>

## <a name="age-gating"></a>Altersbeschränkung
Mithilfe einer Altersbeschränkung in Active Directory B2C können Sie Minderjährige identifizieren, die Ihre Anwendung nutzen.  Sie können den Benutzer für die Anmeldung in der Anwendung sperren oder ihm erlauben, die Anwendung weiterhin zu nutzen, wenn zusätzliche Angaben gemacht werden. Diese beziehen sich auf die Altersgruppe des Benutzers und den Status bezüglich der Zustimmung eines Erziehungsberechtigten.  

>[!NOTE]
>Die Zustimmung eines Erziehungsberechtigten wird in einem Benutzerattribut namens `consentProvidedForMinor` verfolgt.  Sie können diese Eigenschaft über die Graph-API aktualisieren, die dieses Feld dann beim Aktualisieren von `legalAgeGroupClassification` verwendet.
>

## <a name="setting-up-your-directory-for-age-gating"></a>Einrichten von Ihrem Verzeichnis für die Altersbeschränkung
Um die Altersbeschränkung in einem Benutzerflow zu verwenden, müssen Sie für Ihr Verzeichnis zusätzliche Eigenschaften konfigurieren. Dieser Vorgang kann im Menü über `Properties` ausgeführt werden. (Diese Option ist nur verfügbar, wenn Sie über die private Vorschau verfügen.)  
1. Klicken Sie in der Active Directory B2C-Erweiterung links im Menü für Ihren Mandanten auf **Eigenschaften**.
2. Klicken Sie unter dem Abschnitt **Altersbeschränkung** auf die Schaltfläche **Konfigurieren**.
3. Warten Sie, bis der Vorgang abgeschlossen ist und Ihr Verzeichnis für die Altersbeschränkung eingerichtet wird.

## <a name="enabling-age-gating-in-your-user-flow"></a>Aktivieren der Altersbeschränkung im Benutzerflow
Sobald Ihr Verzeichnis zur Verwendung der Altersbeschränkung eingerichtet ist, können Sie diese Funktion in den Benutzerflows der Vorschauversion verwenden.  Diese Funktion erfordert Änderungen, die dazu führen, dass sie mit bestehenden Benutzerflowtypen inkompatibel wird.  Zum Aktivieren der Altersbeschränkung führen Sie die folgenden Schritte aus:
1. Erstellen Sie einen Benutzerflow in der Vorschauversion.
2. Wechseln Sie anschließend im Menü zu **Eigenschaften**.
3. Drücken Sie im Bereich **Altersbeschränkung** die Umschaltfläche, um die Funktion zu aktivieren.
4. Sie können dann wählen, wie Sie Benutzer verwalten möchten, die sich als Minderjährige identifizieren.

## <a name="what-does-enabling-age-gating-do"></a>Wozu dient die Aktivierung der Altersbeschränkung?
Nach der Aktivierung der Altersbeschränkung in Ihrem Benutzerflow ändert sich die Benutzererfahrung.  Bei der Anmeldung werden die Benutzer nun nach ihrem Geburtsdatum und dem Land ihres Wohnsitzes sowie nach den für den Benutzerflow konfigurierten Benutzerattributen gefragt.  Benutzer, die noch kein Geburtsdatum und kein Wohnsitzland angegeben haben, werden bei der nächsten Anmeldung nach diesen Informationen gefragt.  Aus diesen beiden Werten erkennt Active Directory B2C, ob der Benutzer minderjährig ist, und aktualisiert das Feld `ageGroup`. Der Wert kann `null`, `Undefined`, `Minor`, `Adult` oder `NotAdult` sein.  Die Felder `ageGroup` und `consentProvidedForMinor` werden dann zur Berechnung von `legalAgeGroupClassification` verwendet. 

## <a name="age-gating-options"></a>Optionen für die Altersbeschränkung
Sie können wählen, ob Sie Minderjährige ohne Zustimmung eines Erziehungsberechtigten in Active Directory B2C sperren lassen möchten oder ob Sie die Anwendung darüber entscheiden soll, wie weiter mit ihnen verfahren wird.  

### <a name="allowing-minors-without-parental-consent"></a>Zulassen von Minderjährigen ohne Zustimmung eines Erziehungsberechtigten
Bei Benutzerflows, die entweder eine Registrierung, eine Anmeldung oder beides zulassen, können Sie wählen, ob Minderjährige ohne Zustimmung Ihre Anwendung verwenden können sollen.  Minderjährigen ohne Zustimmung eines Erziehungsberechtigten ist es gestattet, sich normal anzumelden oder zu registrieren. Dann stellt Azure AD B2C ein ID-Token mit dem `legalAgeGroupClassification`-Anspruch aus.  Bei Verwendung dieses Anspruchs können Sie wählen, welche Benutzeroberfläche diesen Benutzern angezeigt werden soll, wie z.B. die, über die eine Zustimmung eines Erziehungsberechtigten eingeholt wird (wobei wird das Feld `consentProvidedForMinor` aktualisiert wird).

### <a name="blocking-minors-without-parental-consent"></a>Sperren von Minderjährigen ohne Zustimmung eines Erziehungsberechtigten
Bei Benutzerflows, die entweder eine Registrierung, eine Anmeldung oder beides zulassen, können Sie festlegen, ob Minderjährige ohne Zustimmung für die Anwendung gesperrt werden.  Es gibt zwei Optionen, wie in Active Directory B2C gesperrte Benutzer behandelt werden sollen:
* JSON-Funktion zurück an die Anwendung senden: Diese Option sendet eine Antwort zurück an die Anwendung, dass ein Minderjähriger gesperrt wurde.
* Fehlerseite anzeigen: Dem Benutzer wird eine Seite angezeigt, die ihn darüber informiert, dass er nicht auf die Anwendung zugreifen kann.
