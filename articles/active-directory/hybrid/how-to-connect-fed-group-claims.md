---
title: Konfigurieren von Gruppenansprüchen für Anwendungen mit Azure Active Directory | Microsoft-Dokumentation
description: Hier finden Sie Informationen zum Konfigurieren von Gruppenansprüchen zur Verwendung mit Azure AD.
services: active-directory
documentationcenter: ''
ms.reviewer: paulgarn
manager: daveba
ms.component: hybrid
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 02/27/2019
ms.author: billmath
author: billmath
ms.openlocfilehash: 622a3ce0f80bd09bd09fa7ff097f68155318142d
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58080355"
---
# <a name="configure-group-claims-for-applications-with-azure-active-directory-public-preview"></a>Konfigurieren von Gruppenansprüchen für Anwendungen mit Azure Active Directory (öffentliche Vorschau)

Azure Active Directory kann Informationen zur Gruppenmitgliedschaft eines Benutzer in Token bereitstellen, damit diese in Anwendungen verwendet werden können.  Zwei grundlegende Muster werden unterstützt:

- Gruppe, die durch ihren Azure Active Directory-Objektbezeichner (OID) identifiziert werden (allgemein verfügbar)
- Gruppen, die durch einen SAMAccountName oder eine GroupSID für in Active Directory (AD) synchronisierte Gruppen und Benutzer identifiziert werden (öffentliche Vorschau)

> [!Note]
> Durch die Unterstützung von Namen und lokalen Sicherheits-IDs (SIDs) ist es möglich, vorhandene Anwendungen aus AD FS (Active Directory-Verbunddienste) zu verlagern.    In Azure AD verwaltete Gruppen enthalten die Attribute nicht, die zur Ausgabe dieser Ansprüche erforderlich sind.

## <a name="group-claims-for-applications-migrating-from-ad-fs-and-other-idps"></a>Gruppenansprüche für Anwendungen, die aus AD FS und anderen IDPs migriert werden

Viele Anwendungen, die für die Authentifizierung bei AD FS konfiguriert sind, benötigen Informationen zur Gruppenmitgliedschaft in Form von Windows AD-Gruppenattributen.   Diese Attribute sind der SAMAccountName der Gruppe, der per Domänenname qualifiziert werden kann, oder die Windows-Gruppen-SID.  Wenn die Anwendung in einen Verbund mit AD FS platziert wird, verwendet AD FS die TokenGroups-Funktion, um die Gruppenmitgliedschaften der Benutzer abzurufen.

Um das Token abzugleichen, das eine App von AD FS empfängt, können Gruppen- und Rollenansprüche ausgegeben werden, die den qualifizierten SAMAccountName der Domäne anstelle der Azure Active Directory-objectID der Gruppe enthalten.

Folgende Formate für Gruppenansprüche werden unterstützt:

- **Azure Active Directory-GroupObjectId** (für alle Gruppen verfügbar)
- **SAMAccountName** (für Gruppen verfügbar, die aus Active Directory synchronisiert werden)
- **NetbiosDomain\samAccountName** (für Gruppen verfügbar, die aus Active Directory synchronisiert werden)
- **DNSDomainName\samAccountName** (für Gruppen verfügbar, die aus Active Directory synchronisiert werden)

> [!NOTE]
> Die Attribute „SAMAccountName“ und „OnPremisesGroupSID“ sind nur für Group-Objekte verfügbar, die aus Active Directory synchronisiert werden.   Für Gruppen, die in Azure Active Directory oder Office365 erstellt werden, sind diese Attribute nicht verfügbar.   Anwendungen, die von lokalen Group-Attributen abhängig sind, erhalten die Attribute nur für synchronisierte Gruppen.

## <a name="options-for-applications-to-consume-group-information"></a>Optionen für die Verwendung von Group-Informationen in Anwendungen

Eine Möglichkeit, mit der Anwendungen Gruppeninformationen abrufen können, besteht darin, den Graph-Gruppenendpunkt aufzurufen, um die Gruppenmitgliedschaft für den authentifizierten Benutzer abzurufen. Dieser Aufruf stellt sicher, dass alle Gruppen, in denen ein Benutzer Mitglied ist, verfügbar sind, auch wenn eine große Anzahl von Gruppen beteiligt sind und die Anwendung alle Gruppen aufzählen muss, in denen der Benutzer Mitglied ist.  Die Gruppenenumeration ist dann unabhängig von Einschränkungen hinsichtlich der Tokengröße.

Wenn eine vorhandene Anwendung jedoch bereits erwartet, Gruppeninformationen über Ansprüche im empfangenen Token zu erhalten, kann Azure Active Directory mit einer Reihe unterschiedlicher Anspruchsoptionen konfiguriert werden, um die Anforderungen der Anwendung zu erfüllen.  Betrachten Sie die folgenden Optionen:

- Beim Verwenden der Gruppenmitgliedschaft zum Zweck der Autorisierung einer Anwendung (unabhängig davon, ob die Gruppenmitgliedschaft aus dem Token oder aus Graph abgerufen wird) sollte bevorzugt die Group ObjectID verwendet werden, die unveränderlich, in Azure Active Directory eindeutig und für alle Gruppen verfügbar ist.
- Wenn Sie den SAMAccountName der Gruppe zur Autorisierung nutzen, verwenden Sie qualifizierte Domänennamen – diese bergen ein geringeres Risiko, dass Situationen mit Namenskonflikten eintreten. SAMAccountName alleine kann innerhalb einer Active Directory-Domäne eindeutig sein; wenn jedoch mehrere Active Directory-Domänen mit einem Azure Active Directory-Mandanten synchronisiert werden, besteht die Möglichkeit, dass mehrere Gruppen den gleichen Namen aufweisen.
- Erwägen Sie die Verwendung von [Anwendungsrollen](../../active-directory/develop/howto-add-app-roles-in-azure-ad-apps.md), um eine Dereferenzierungsschicht zwischen der Gruppenmitgliedschaft und der Anwendung bereitzustellen.   Die Anwendung trifft dann interne Autorisierungsentscheidungen basierend auf Rollenansprüchen im Token.
- Wenn die Anwendung für den Abruf von Gruppenattributen konfiguriert ist, die aus Active Directory synchronisiert werden, und ein Group-Objekt diese Attribute nicht enthält, wird die entsprechende Gruppe in den Ansprüchen nicht eingeschlossen.
- Gruppenansprüche in Token enthalten geschachtelte Gruppen.   Wenn ein Benutzer Mitglied von Gruppe B und Gruppe B Mitglied von Gruppe A ist, enthalten die Gruppenansprüche für den Benutzer sowohl Gruppe A als auch Gruppe B. In Organisationen, in denen in hohem Maß geschachtelte Gruppen und Benutzer mit sehr vielen Gruppenmitgliedschaften verwendet werden, kann die Anzahl der in den Token enthaltenen Gruppen die Tokengröße erhöhen.   Azure Active Directory begrenzt die Anzahl von Gruppen, die in einem Token ausgegeben werden, auf 150 für SAML-Assertionen und 200 für JWT.

> Voraussetzungen für die Verwendung von Group-Attributen, die aus Active Directory synchronisiert werden:   Die Gruppen müssen über Azure AD Connect aus Active Directory synchronisiert werden.

Die Konfiguration von Azure Active Directory für die Ausgabe von Gruppennamen für Active Directory-Gruppen erfolgt in zwei Schritten.

1. **Synchronisieren von Gruppennamen aus Active Directory**: Bevor Azure Active Directory Gruppennamen oder lokale Gruppen-SIDs in Gruppen- oder Rollenansprüchen ausgeben kann, müssen die erforderlichen Attribute aus Active Directory synchronisiert werden.  Sie müssen Azure AD Connect 1.2.70 oder höher ausführen.   Vor Version 1.2.70 synchronisiert Azure AD Connect die Gruppenobjekte aus Active Directory, schließt aber die erforderlichen Gruppennamenattribute nicht standardmäßig ein.  Sie sollten ein Upgrade auf die aktuelle Version durchführen.

2. **Konfigurieren der Anwendungsregistrierung in Azure Active Directory, sodass Gruppenansprüche in Token eingeschlossen werden**: Gruppenansprüche können entweder im Abschnitt „Unternehmensanwendungen“ für eine aus dem Katalog oder nicht aus dem Katalog stammende SAML-SSO-Anwendung oder mithilfe des Anwendungsmanifests im Abschnitt „Anwendungsregistrierungen“ konfiguriert werden.  Informationen zum Konfigurieren von Gruppenansprüchen im Anwendungsmanifest finden Sie im Abschnitt „Konfigurieren der Azure AD-Anwendungsregistrierung für Gruppenattribute“ weiter unten.

## <a name="configure-group-claims-for-saml-applications-using-sso-configuration"></a>Konfigurieren von Gruppenansprüchen für SAML-Anwendungen mithilfe der SSO-Konfiguration

Um Gruppenansprüche für eine SAML-Anwendung (aus dem Katalog oder nicht aus dem Katalog) zu konfigurieren, öffnen Sie „Unternehmensanwendungen“, klicken Sie in der Liste auf die Anwendung, und wählen Sie „SSO-Konfiguration“ aus.

Wählen Sie das Bearbeitungssymbol neben „In Token zurückgegebene Gruppen“ aus.

![Benutzeroberfläche für Ansprüche](media/how-to-connect-fed-group-claims/group-claims-ui-1.png)

Verwenden Sie die Optionsfelder, um die Gruppen auszuwählen, die im Token enthalten sein sollen.

![Benutzeroberfläche für Ansprüche](media/how-to-connect-fed-group-claims/group-claims-ui-2.png)

| Auswahl | BESCHREIBUNG |
|----------|-------------|
| **Alle Gruppen** | Gibt Sicherheitsgruppen und Verteilerlisten aus.   Diese Option führt auch dazu, dass Verzeichnisrollen, denen der Benutzer zugewiesen ist, in einem „wids“-Anspruch und alle Anwendungsrollen, denen der Benutzer zugewiesen ist, im Rollenanspruch ausgegeben werden. |
| **Sicherheitsgruppen** | Gibt im Gruppenanspruch Sicherheitsgruppen aus, in denen der Benutzer Mitglied ist. |
| **Verteilerliste** | Gibt Verteilerlisten aus, in denen der Benutzer Mitglied ist. |
| **Verzeichnisrolle** | Wenn dem Benutzer Verzeichnisrollen zugewiesen sind, werden diese als „wids“-Anspruch ausgegeben (Gruppenansprüche werden nicht ausgegeben). |

Um z.B. alle Sicherheitsgruppen auszugeben, in denen der Benutzer Mitglied ist, wählen Sie „Sicherheitsgruppen“ aus.

![Benutzeroberfläche für Ansprüche](media/how-to-connect-fed-group-claims/group-claims-ui-3.png)

### <a name="advanced-options"></a>Erweiterte Optionen

Die Art und Weise, in der Gruppenansprüche ausgegeben werden, kann durch die Einstellungen in den erweiterten Optionen geändert werden.

Name des Gruppenanspruchs anpassen:  Wenn diese Option aktiviert ist, kann für Gruppenansprüche ein anderer Anspruchstyp angegeben werden.   Geben Sie den Anspruchstyp im Feld „Name“ und den optionalen Namespace für den Anspruch im Feld „Namespace“ ein.

![Benutzeroberfläche für Ansprüche](media/how-to-connect-fed-group-claims/group-claims-ui-4.png)

Um Gruppen mithilfe von Active Directory-Attributen anstatt Azure AD-objectIDs auszugeben, aktivieren Sie das Kontrollkästchen „Gruppen als Namen anstatt IDs zurückgeben“, und wählen Sie aus der Dropdownliste das gewünschte Format aus.  Damit wird die Objekt-ID in den Ansprüchen durch Zeichenfolgenwerte mit Gruppennamen ersetzt.   In den Ansprüchen sind nur Gruppen enthalten, die aus Active Directory synchronisiert werden.

![Benutzeroberfläche für Ansprüche](media/how-to-connect-fed-group-claims/group-claims-ui-5.png)

Bei einigen Anwendungen ist es erforderlich, dass die Informationen zur Gruppenmitgliedschaft im Anspruch „role“ enthalten sind. Sie können optional die Gruppen des Benutzers als Rollen ausgeben, indem Sie das Kontrollkästchen „Gruppen als Rollenansprüche ausgeben“ aktivieren.

![Benutzeroberfläche für Ansprüche](media/how-to-connect-fed-group-claims/group-claims-ui-6.png)

> [!NOTE]
> Wenn die Option zum Ausgeben der Gruppendaten als Rollen verwendet wird, sind im Rollenanspruch nur Gruppen enthalten.  Anwendungsrollen, denen der Benutzer zugewiesen ist, sind im Rollenanspruch nicht enthalten.

## <a name="configure-the-azure-ad-application-registration-for-group-attributes"></a>Konfigurieren der Azure AD-Anwendungsregistrierung für Gruppenattribute

Gruppenansprüche können auch im Abschnitt [Optionale Ansprüche](../../active-directory/develop/active-directory-optional-claims.md) des [Anwendungsmanifests](../../active-directory/develop/reference-app-manifest.md) konfiguriert werden.

1. Wählen Sie im Portal folgende Optionen aus: Azure Active Directory > Anwendungsregistrierungen > Anwendung auswählen > Manifest.

2. Aktivieren Sie Gruppenmitgliedschaftsansprüche, indem Sie den groupMembershipClaim-Wert ändern.

   Gültige Werte sind:

   - All
   - SecurityGroup
   - DistributionList
   - DirectoryRole

   Beispiel: 

   ```json
   "groupMembershipClaims": "SecurityGroup"
   ```

   Standardmäßig werden ObjectIDs von Gruppen im Wert des Gruppenanspruchs ausgegeben.  Um den Anspruchswert so zu ändern, dass er lokale Gruppenattribute enthält, oder um den Anspruchstyp zu „role“ zu ändern, verwenden Sie die Konfiguration für optionale Ansprüche wie folgt:

3. Legen Sie optionale Ansprüche für die Gruppennamenkonfiguration fest.

   Wenn die Gruppen im Token die lokalen AD-Gruppenattribute im Abschnitt mit optionalen Ansprüchen enthalten sollen, geben Sie an, welcher optionale Anspruch des Tokentyps angewendet werden soll. Geben Sie außerdem den Namen des angeforderten optionalen Anspruchs sowie weitere gewünschte Eigenschaften an.  Es können mehrere Tokentypen aufgelistet werden:

   - idToken für das OIDC-ID-Token
   - accessToken für das OAuth/OIDC-Zugriffstoken
   - Saml2Token für SAML-Token

   > [!NOTE]
   > Der Typ „Saml2Token“ gilt für Token sowohl im SAML1.1- als auch im SAML2.0-Format.

   Ändern Sie für jeden relevanten Tokentyp den Gruppenanspruch so, dass der Abschnitt „Optionale Ansprüche“ im Manifest verwendet wird. Das Schema für optionale Ansprüche sieht folgendermaßen aus:

   ```json
   {
   "name": "groups",
   "source": null,
   "essential": false,
   "additionalProperties": []
   }
   ```

   | Schema für optionale Ansprüche | Wert |
   |----------|-------------|
   | **name:** | Muss „groups“ lauten. |
   | **source:** | Wird nicht verwendet. Auslassen oder NULL angeben. |
   | **essential:** | Wird nicht verwendet. Auslassen oder FALSE angeben. |
   | **additionalProperties:** | Liste zusätzlicher Eigenschaften.  Gültige Optionen sind „sam_account_name“, „dns_domain_and_sam_account_name“, „netbios_domain_and_sam_account_name“, „emit_as_roles“. |

   In „additionalProperties“ ist nur eine dieser Optionen erforderlich: „sam_account_name“, „dns_domain_and_sam_account_name“, „netbios_domain_and_sam_account_name“.  Wenn mehrere dieser Optionen vorhanden sind, wird die erste verwendet, alle weiteren werden ignoriert.

   Einige Anwendungen erfordern Gruppeninformationen über Benutzer im Rollenanspruch.  Um den Anspruchstyp von einem Gruppenanspruch zu einem Rollenanspruch zu ändern, fügen Sie „emit_as_roles“ zu den zusätzlichen Eigenschaften hinzu.  Die Gruppenwerte werden im Rollenanspruch ausgegeben.

   > [!NOTE]
   > Wenn „emit_as_roles“ verwendet wird, sind konfigurierte Anwendungsrollen, denen der Benutzer zugewiesen ist, nicht im Rollenanspruch enthalten.

### <a name="examples"></a>Beispiele

Ausgeben von Gruppen als Gruppennamen in OAuth-Zugriffstoken im Format „dnsDomainName\SAMAccountName“:

```json
"optionalClaims": {
    "accessToken": [{
        "name": "groups",
        "additionalProperties": ["dns_domain_and_sam_account_name"]
    }]
}
 ```

Ausgeben von Gruppennamen, die im Format „netbiosDomain\samAccountName“ als Rollenanspruch in SAML- und OIDC-ID-Token zurückgegeben werden sollen:

```json
"optionalClaims": {
    "saml2Token": [{
        "name": "groups",
        "additionalProperties": ["netbios_name_and_sam_account_name", "emit_as_roles"]
    }],

    "idToken": [{
        "name": "groups",
        "additionalProperties": ["netbios_name_and_sam_account_name", "emit_as_roles"]
    }]
 }
 ```

## <a name="next-steps"></a>Nächste Schritte

[Was ist eine Hybrididentität?](whatis-hybrid-identity.md)