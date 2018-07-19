---
title: Attributbasierte Regeln für eine dynamische Gruppenmitgliedschaft in Azure Active Directory | Microsoft-Dokumentation
description: Erstellen erweiterter Regeln für eine dynamische Gruppenmitgliedschaft, einschließlich unterstützter Ausdrucksregeloperatoren und -parameter
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 07/05/2018
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: a48dcff6eedc2aa6e8bb6cd5b0668af72259493b
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2018
ms.locfileid: "37869090"
---
# <a name="create-attribute-based-rules-for-dynamic-group-membership-in-azure-active-directory"></a>Erstellen attributbasierter Regeln für dynamische Gruppenmitgliedschaft in Azure Active Directory
In Azure Active Directory (Azure AD) können Sie benutzerdefinierte Regeln für die Aktivierung von komplexen, attributbasierten dynamischen Mitgliedschaften für Gruppen erstellen. In diesem Artikel werden Attribute und Syntax zum Erstellen der Regeln für dynamische Mitgliedschaft für Benutzer oder Geräte erläutert. Sie können eine Regel für die dynamische Mitgliedschaft für Sicherheits- oder Office 365-Gruppen einrichten.

Wenn sich Attribute eines Benutzers oder Geräts ändern, bewertet das System alle dynamischen Gruppenregel in einem Verzeichnis, um zu ermitteln, ob die Änderung irgendwelche Vorgänge zum Hinzufügen oder Löschen von Gruppen auslöst. Falls ein Benutzer oder Gerät wird als Mitglied zu einer Gruppe hinzugefügt, wenn eine Regel dieser Gruppe erfüllt wird. Wenn sie diese Regel nicht mehr erfüllen, werden sie entfernt.

> [!NOTE]
> Dieses Feature erfordert für jeden eindeutigen Benutzer, der Mitglied mindestens einer dynamischen Gruppe ist, eine Azure AD Premium P1-Lizenz. Sie müssen den Benutzern keine Lizenzen zuweisen, damit sie Mitglieder dynamischer Gruppen werden können, aber Sie müssen die Mindestanzahl von Lizenzen im Mandanten haben, um alle diese Benutzer abzudecken. Beispiel: Wenn Sie über insgesamt 1.000 eindeutige Benutzer in allen dynamischen Gruppen Ihres Mandanten verfügen, benötigen Sie mindestens 1.000 Lizenzen für Azure AD Premium P1 oder höher, um die Lizenzanforderung zu erfüllen.
>
> Sie können zwar eine dynamische Gruppe für Geräte oder Benutzer erstellen, jedoch keine Regel festlegen, die sowohl Benutzer als auch Geräte enthält.
> 
> Derzeit ist es nicht möglich, eine Gerätegruppe basierend auf den Attributen der zuständigen Benutzer zu erstellen. Die Regeln für die Gerätemitgliedschaft können nur auf unmittelbare Attribute von Geräteobjekten im Verzeichnis verweisen.

## <a name="to-create-an-advanced-rule"></a>Erstellen einer erweiterten Regel
1. Melden Sie sich beim [Azure AD Admin Center](https://aad.portal.azure.com) über ein Konto an, das als globaler Administrator oder Benutzerkontoadministrator konfiguriert ist.
2. Wählen Sie **Benutzer und Gruppen**.
3. Wählen Sie **Alle Gruppen** und **Neue Gruppe** aus.

   ![Neue Gruppe hinzufügen](./media/groups-dynamic-membership/new-group-creation.png)

4. Geben Sie auf dem Blatt **Gruppe** einen Namen und eine Beschreibung für die neue Gruppe ein. Wählen Sie als **Mitgliedschaftstyp** entweder **Dynamischer Benutzer** oder **Dynamisches Gerät** aus, je nachdem, ob Sie eine Regel für Benutzer oder Geräte erstellen möchten. Wählen Sie anschließend **Dynamische Abfrage hinzufügen** aus. Mit dem Regel-Generator können Sie eine einfache Regel erstellen oder selbst eine erweiterte Regel schreiben. Dieser Artikel enthält weitere Informationen zu verfügbaren Benutzer- und Geräteattributen sowie Beispiele für erweiterte Regeln.

   ![Hinzufügen einer Regel für eine dynamische Mitgliedschaft](./media/groups-dynamic-membership/add-dynamic-group-rule.png)

5. Wählen Sie nach der Erstellung der Regel unten auf dem Blatt **Abfrage hinzufügen**.
6. Wählen Sie **Erstellen** auf dem Blatt **Erstellen** aus, um die Gruppe zu erstellen.

> [!TIP]
> Die Gruppenerstellung misslingt, wenn die von Ihnen eingegebene Regel falsch gebildet wurde oder nicht gültig ist. Rechts oben im Portal wird eine Benachrichtigung angezeigt, die erläutert, warum die Regel nicht verarbeitet werden konnte. Lesen Sie sie sorgfältig, um zu erfahren, wie die Regel angepasst werden muss, damit sie gültig ist.

## <a name="status-of-the-dynamic-rule"></a>Status der dynamischen Regel

Auf der Seite „Übersicht“ für Ihre dynamische Gruppe sehen Sie den Verarbeitungsstatus der Mitgliedschaft und das zuletzt geänderte Datum.
  
  ![Status der dynamischen Gruppe anzeigen](./media/groups-dynamic-membership/group-status.png)


Für **Verarbeitungsstatus der Mitgliedschaft** können die folgenden Statusmeldungen angezeigt werden:
* **Wird ausgewertet**: Die Gruppenänderung wurde empfangen, die Aktualisierungen werden ausgewertet.
* **Wird verarbeitet**: Die Updates werden verarbeitet.
* **Aktualisierung abgeschlossen**: Die Verarbeitung wurde abgeschlossen, alle anwendbaren Aktualisierungen wurden vorgenommen.
* **Verarbeitungsfehler**: Bei der Auswertung der Mitgliedschaftsregel ist ein Fehler aufgetreten, sodass die Verarbeitung nicht abgeschlossen werden konnte.
* **Aktualisierung angehalten**: Aktualisierungen der Regeln für die dynamische Mitgliedschaft wurden vom Administrator angehalten. „MembershipRuleProcessingState“ ist auf „Paused“ festgelegt.

Für **Letzte Aktualisierung der Mitgliedschaft** können die folgenden Statusmeldungen angezeigt werden:
* &lt;**Datum und Uhrzeit**&gt;: Der Zeitpunkt der letzten Aktualisierung der Mitgliedschaft.
* **In Bearbeitung**: Aktualisierungen sind derzeit in Bearbeitung.
* **Unbekannt**: Der Zeitpunkt der letzten Aktualisierung kann nicht abgerufen werden. Das kann daran liegen, dass die Gruppe neu erstellt wurde.

Wenn bei der Verarbeitung der Mitgliedschaftsregel für eine bestimmte Gruppe ein Fehler auftritt, wird oben auf der Seite **Übersicht** der Gruppe eine Warnung angezeigt. Wenn für alle Gruppen innerhalb des Mandanten für mehr als 24 Stunden keine ausstehenden Aktualisierungen der dynamischen Mitgliedschaft verarbeitet werden können, wird oben in **Alle Gruppen** eine Warnung angezeigt.

![Verarbeiten der Fehlermeldung](./media/groups-dynamic-membership/processing-error.png)

## <a name="constructing-the-body-of-an-advanced-rule"></a>Erstellen des Texts einer erweiterten Regel
Die erweiterte Regel, die Sie für die dynamischen Mitgliedschaften für Gruppen erstellen können, ist im Wesentlichen ein binärer Ausdruck, der aus drei Teilen besteht und ein wahres oder falsches Ergebnis liefert. Die drei Teile sind folgende:

* Linker Parameter
* Binärer Operator
* Rechte Konstante

Eine vollständige erweiterte Regel sieht etwa wie folgt aus: (leftParameter binaryOperator "RightConstant"). Für den gesamten Binärausdruck sind öffnende und schließende Klammern optional. Doppelte Anführungszeichen sind ebenfalls optional und werden nur für die rechte Konstante benötigt, falls diese eine Zeichenfolge ist. Die Syntax für den linken Parameter lautet "user.property". Eine erweiterte Regel kann aus mehreren binären Ausdrücke bestehen, die durch die logischen Operatoren "-and", "-or" und "-not" getrennt sind.

Hier finden Sie Beispiele für eine richtig aufgebaute erweiterte Regel:
```
(user.department -eq "Sales") -or (user.department -eq "Marketing")
(user.department -eq "Sales") -and -not (user.jobTitle -contains "SDE")
```
Eine vollständige Liste der unterstützten Parameter und Ausdrucksregeloperatoren finden Sie in den folgenden Abschnitten. Informationen zu den Attributen für Geräteregeln finden Sie unter [Verwenden von Attributen zum Erstellen von Regeln für Geräteobjekte](#using-attributes-to-create-rules-for-device-objects).

Die Gesamtlänge des Texts der erweiterten Regel darf 2048 Zeichen nicht überschreiten.

> [!NOTE]
> Bei Vorgängen mit Zeichenfolgen und regulären Ausdrücken wird die Groß- und Kleinschreibung nicht beachtet. Sie können auch NULL-Prüfungen durchführen, indem Sie *null* als Konstante verwenden. Beispiel: user.department -eq *null*.
> Zeichenfolgen mit Anführungszeichen (") sollten mit einem Escapezeichen (') maskiert werden. Beispiel: user.department -eq \`"Sales".

## <a name="supported-expression-rule-operators"></a>Unterstützte Ausdrucksregeloperatoren
Die folgende Tabelle enthält alle Ausdrucksregeloperatoren und ihre Syntax zur Verwendung im Text der erweiterten Regel:

| Operator | Syntax |
| --- | --- |
| Not Equals |-ne |
| Equals |-eq |
| Not Starts With |-notStartsWith |
| Starts With |-startsWith |
| Not Contains |-notContains |
| Contains |-contains |
| Not Match |-notMatch |
| Match |-match |
| Geben Sie in | -in |
| Not In | -notIn |

## <a name="operator-precedence"></a>Rangfolge der Operatoren

Alle Operatoren werden unten nach Rangfolge von niedrig nach hoch aufgeführt. Operatoren in derselben Zeile haben denselben Rang:
````
-any -all
-or
-and
-not
-eq -ne -startsWith -notStartsWith -contains -notContains -match –notMatch -in -notIn
````
Alle Operatoren können mit oder ohne vorangestellten Bindestrich verwendet werden. Klammern sind nur erforderlich, wenn die Rangfolge nicht Ihren Anforderungen entspricht.
Beispiel: 
```
   user.department –eq "Marketing" –and user.country –eq "US"
```
entspricht:
```
   (user.department –eq "Marketing") –and (user.country –eq "US")
```
## <a name="using-the--in-and--notin-operators"></a>Mithilfe der Operatoren „-In“ und „-notIn“

Wenn Sie den Wert eines Benutzerattributs mit einer Reihe unterschiedlicher Werte vergleichen möchten, können Sie mit den Operatoren „-In“ oder „-notIn“. Hier ist ein Beispiel für den Operator „-In“:
```
   user.department -In ["50001","50002","50003",“50005”,“50006”,“50007”,“50008”,“50016”,“50020”,“50024”,“50038”,“50039”,“51100”]
```
Beachten Sie die Verwendung der eckigen Klammern „[“ und „]“ am Anfang und Ende der Werteliste. Diese Bedingung wertet nach TRUE aus, wenn der Wert von „user.department“ einem der Werte in der Liste entspricht.


## <a name="query-error-remediation"></a>Korrektur von Abfragefehlern
In der folgenden Tabelle werden häufige Fehler und deren Behandlung aufgeführt.

| Abfrageanalysefehler | Fehlerverwendung | Korrigierte Verwendung |
| --- | --- | --- |
| Fehler: Das Attribut nicht unterstützt. |(user.invalidProperty -eq "Value") |(user.department -eq "value")<br/><br/>Stellen Sie sicher, dass das Attribut in der [Liste der unterstützten Eigenschaften](#supported-properties) aufgeführt wird. |
| Fehler: Der Operator wird für das Attribut nicht unterstützt. |(user.accountEnabled -contains true) |(user.accountEnabled -eq true)<br/><br/>Der Operator wird für den Eigenschaftentyp nicht unterstützt (in diesem Beispiel kann „-contains“ nicht für den Booleschen Typ verwendet werden). Verwenden Sie die richtige Operatoren für den Eigenschaftentyp. |
| Fehler: Abfragekompilierungsfehler. |1. (user.department -eq "Sales") (user.department -eq "Marketing")<br/><br/>2. (user.userPrincipalName -match "*@domain.ext") |1. Fehlender Operator. Verwenden Sie „-and“ oder „-or“, um zwei Prädikate zu verknüpfen.<br/><br/>(user.department -eq "Sales") -or (user.department -eq "Marketing")<br/><br/>2. Fehler im regulären Ausdruck, der mit „-match“ verwendet wird<br/><br/>(user.userPrincipalName -match ".*@domain.ext"), alternativ: (user.userPrincipalName -match "\@domain.ext$")|

## <a name="supported-properties"></a>Unterstützte Eigenschaften
Im Folgenden werden alle Benutzereigenschaften aufgelistet, die Sie in der erweiterten Regel verwenden können:

### <a name="properties-of-type-boolean"></a>Eigenschaften vom Typ "boolesch"
Zulässige Operatoren

* -eq
* -ne

| Eigenschaften | Zulässige Werte | Verwendung |
| --- | --- | --- |
| accountEnabled |true false |user.accountEnabled -eq true |
| dirSyncEnabled |true false |user.dirSyncEnabled -eq true |

### <a name="properties-of-type-string"></a>Eigenschaften vom Typ "string"
Zulässige Operatoren

* -eq
* -ne
* -notStartsWith
* -startsWith
* -contains
* -notContains
* -match
* -notMatch
* -in
* -notIn

| Eigenschaften | Zulässige Werte | Verwendung |
| --- | --- | --- |
| city |Jeder string-Wert oder *null* |(user.city -eq "value") |
| country |Jeder string-Wert oder *null* |(user.country -eq "value") |
| companyName | Jeder string-Wert oder *null* | (user.companyName -eq "value") |
| department |Jeder string-Wert oder *null* |(user.department -eq "value") |
| displayName |Jeder string-Wert. |(user.displayName -eq "value") |
| employeeId |Jeder string-Wert. |(user.employeeId -eq "value")<br>(user.employeeId -ne *null*) |
| facsimileTelephoneNumber |Jeder string-Wert oder *null* |(user.facsimileTelephoneNumber -eq "value") |
| givenName |Jeder string-Wert oder *null* |(user.givenName -eq "value") |
| jobTitle |Jeder string-Wert oder *null* |(user.jobTitle -eq "value") |
| mail |Jeder string-Wert oder *null* (SMTP-Adresse des Benutzers) |(user.mail -eq "value") |
| mailNickName |Jeder string-Wert (E-Mail-Alias des Benutzers) |(user.mailNickName -eq "value") |
| mobile |Jeder string-Wert oder *null* |(user.mobile -eq "value") |
| objectId |GUID des Benutzerobjekts. |(user.objectId -eq "11111111-1111-1111-1111-111111111111") |
| onPremisesSecurityIdentifier | Lokale Sicherheits-ID (SID) für Benutzer, deren Daten von einem lokalen Standort in die Cloud synchronisiert wurden. |(user.onPremisesSecurityIdentifier -eq "S-1-1-11-1111111111-1111111111-1111111111-1111111") |
| passwordPolicies |None DisableStrongPassword DisablePasswordExpiration DisablePasswordExpiration, DisableStrongPassword |(user.passwordPolicies -eq "DisableStrongPassword") |
| physicalDeliveryOfficeName |Jeder string-Wert oder *null* |(user.physicalDeliveryOfficeName -eq "value") |
| postalCode |Jeder string-Wert oder *null* |(user.postalCode -eq "value") |
| preferredLanguage |ISO 639-1 code |(user.preferredLanguage -eq "en-US") |
| sipProxyAddress |Jeder string-Wert oder *null* |(user.sipProxyAddress -eq "value") |
| state |Jeder string-Wert oder *null* |(user.state -eq "value") |
| streetAddress |Jeder string-Wert oder *null* |(user.streetAddress -eq "value") |
| surname |Jeder string-Wert oder *null* |(user.surname -eq "value") |
| telephoneNumber |Jeder string-Wert oder *null* |(user.telephoneNumber -eq "value") |
| usageLocation |Aus zwei Buchstaben bestehender Ländercode. |(user.usageLocation -eq "US") |
| userPrincipalName |Jeder string-Wert. |(user.userPrincipalName -eq "alias@domain") |
| userType |member-Gast *null* |(user.userType -eq "Member") |

### <a name="properties-of-type-string-collection"></a>Eigenschaften vom Typ "string collection"
Zulässige Operatoren

* -contains
* -notContains

| Eigenschaften | Zulässige Werte | Verwendung |
| --- | --- | --- |
| otherMails |Jeder string-Wert. |(user.otherMails -contains "alias@domain") |
| proxyAddresses |SMTP: alias@domain smtp: alias@domain |(user.proxyAddresses -contains "SMTP: alias@domain") |

## <a name="multi-value-properties"></a>Mehrwertige Eigenschaften
Zulässige Operatoren

* „-any“ (erfüllt, wenn mindestens ein Element in der Auflistung der Bedingung entspricht)
* „-all“ (erfüllt, wenn alle Elemente in der Auflistung der Bedingung entsprechen)

| Eigenschaften | Werte | Verwendung |
| --- | --- | --- |
| assignedPlans |Jedes Objekt in der Sammlung macht folgende Zeichenfolgeneigenschaften verfügbar: capabilityStatus, service, servicePlanId |user.assignedPlans -any (assignedPlan.servicePlanId -eq "efb87545-963c-4e0d-99df-69c6916d9eb0" -and assignedPlan.capabilityStatus -eq "Enabled") |

Mehrwertige Eigenschaften sind Sammlungen von Objekten desselben Typs. Sie können die Operatoren „-any“ und „-all“ verwenden, um eine Bedingung auf ein Element oder alle Elemente in der Sammlung anzuwenden. Beispiel: 

assignedPlans ist eine mehrwertige-Eigenschaft, die alle Service-Pläne auflistet, die dem Benutzer zugewiesen sind. Der folgende Ausdruck wird Benutzer auswählen, die über einen Service-Plan von Exchange Online (Plan 2) verfügen, der sich im Zustand „Enabled“ befindet:

```
user.assignedPlans -any (assignedPlan.servicePlanId -eq "efb87545-963c-4e0d-99df-69c6916d9eb0" -and assignedPlan.capabilityStatus -eq "Enabled")
```

(Der Guid-Bezeichner kennzeichnet den Service-Plan von Exchange Online (Plan 2))

> [!NOTE]
> Dieser Vorgang ist sehr nützlich, wenn Sie alle Benutzer identifizieren möchten, für die eine Funktion für Office 365 (oder einen anderen Microsoft-Onlinedienst) aktiviert wurde, um diesen beispielsweise einen bestimmten Satz von Richtlinien zuzuweisen.

Der folgende Ausdruck wählt alle Benutzer aus, die über einen Service-Plan verfügen, der mit dem Intune-Dienst (identifizierbar anhand des Dienstnamens „SCO“) verknüpft ist:
```
user.assignedPlans -any (assignedPlan.service -eq "SCO" -and assignedPlan.capabilityStatus -eq "Enabled")
```

## <a name="use-of-null-values"></a>Verwenden von NULL-Werten

Zum Angeben eines NULL-Werts in einer Regel können Sie den Wert *null* verwenden. Achten Sie darauf, keine Anführungszeichen um das Wort *null* zu verwenden – andernfalls werden diese als literaler Zeichenfolgenwert interpretiert. Der -not-Operator kann nicht als Vergleichsoperator für NULL verwendet werden. Wenn Sie ihn verwenden, erhalten Sie eine Fehlermeldung, egal ob Sie „null“ oder „$null“ verwenden. Verwenden Sie stattdessen „-eq“ oder „-ne“. Die richtige Referenzierung des NULL-Werts erfolgt auf diese Weise:
```
   user.mail –ne $null
```

## <a name="extension-attributes-and-custom-attributes"></a>Erweiterungsattribute und benutzerdefinierte Attribute
Erweiterungsattribute und benutzerdefinierte Attribute werden in Regeln für dynamische Mitgliedschaft unterstützt.

Erweiterungsattribute werden von einer lokalen Windows Server AD-Instanz synchronisiert und erhalten folgendes Format: ExtensionAttributeX. Dabei entspricht X 1 bis 15.
Beispiel für eine Regel, die ein Erweiterungsattribut verwendet:
```
(user.extensionAttribute15 -eq "Marketing")
```
Benutzerdefinierte Attribute werden von einer lokalen Windows Server AD-Instanz oder von einer verbundenen SaaS-Anwendung aus synchronisiert und erhalten das Format „user.extension_[GUID]\__[Attribute]“. Dabei ist [GUID] der eindeutige Bezeichner in AAD für die Anwendung, die das Attribut in AAD erstellt hat, und [Attribute] ist der Name des Attributs bei seiner Erstellung.
Beispiel für eine Regel, die ein benutzerdefiniertes Attribut verwendet:
```
user.extension_c272a57b722d4eb29bfe327874ae79cb__OfficeNumber  
```
Den Namen des benutzerdefinierten Attributs finden Sie im Verzeichnis. Fragen Sie dazu das Attribut eines Benutzers mithilfe des Graph-Explorers ab, und suchen Sie nach dem Attributnamen.

## <a name="direct-reports-rule"></a>Mitarbeiterregel
Sie können eine Gruppe erstellen, die alle einem Manager direkt unterstellten Mitarbeiter enthält. Wenn sich die direkt unterstellten Mitarbeiter eines Managers in der Zukunft ändern, wird die Mitgliedschaft in der Gruppe automatisch angepasst.

> [!NOTE]
> 1. Damit diese Regel funktioniert, stellen Sie sicher, dass die Eigenschaft **Manager ID** (Manager-ID) für die Benutzer in Ihrem Mandanten korrekt festgelegt ist. Sie können den aktuellen Wert für einen Benutzer in deren **Registerkarte „Profil“** überprüfen.
> 2. Diese Regel gilt nur für **direct** (direkt) unterstellte Mitarbeiter. Es ist derzeit nicht möglich, eine Gruppe für eine geschachtelte Hierarchie zu erstellen, wie zum Beispiel eine Gruppe, die direkt unterstellte Mitarbeiter und deren unterstellte Mitarbeiter enthält.
> 3. Diese Regel kann nicht mit anderen erweiterten Regeln kombiniert werden.

**To configure the group** (Konfigurieren der Gruppe)

1. Führen Sie die Schritte 1 bis 5 im Abschnitt [To create the advanced rule (Erstellen einer erweiterten Regel)](#to-create-the-advanced-rule), aus, und wählen Sie als **Membership type** (Mitgliedschaftstyp) die Option **Dynamic User** (Dynamischer Benutzer) aus.
2. Geben Sie auf dem Blatt **Regeln für dynamische Mitgliedschaft** die Regel mit der folgenden Syntax ein:

    *Direct Reports for "{objectID_of_manager}"* (Direkt unterstellte Mitarbeiter für „{objectID_of_manager}“)

    Beispiel für eine gültige Regel:
```
                    Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863"
```
    where “62e19b97-8b3d-4d4a-a106-4ce66896a863” is the objectID of the manager. The object ID can be found on manager's **Profile tab**.
3. Nachdem die Regel gespeichert wurde, werden alle Benutzer der Gruppe hinzugefügt, die über den festgelegten Wert der Manager-ID verfügen.

## <a name="using-attributes-to-create-rules-for-device-objects"></a>Verwenden von Attributen zum Erstellen von Regeln für Geräteobjekte
Sie können auch eine Regel erstellen, die Geräteobjekte für die Mitgliedschaft in einer Gruppe auswählt. Die folgenden Geräteattribute können verwendet werden.

 Geräteattribut  | Werte | Beispiel
 ----- | ----- | ----------------
 accountEnabled | true false | (device.accountEnabled -eq true)
 displayName | Jeder string-Wert. |(device.displayName -eq "Rob Iphone”)
 deviceOSType | Jeder string-Wert. | (device.deviceOSType -eq "iPad") -or (device.deviceOSType -eq "iPhone")
 deviceOSVersion | Jeder string-Wert. | (device.OSVersion -eq "9.1")
 deviceCategory | ein gültiger Gerätekategoriename | (device.deviceCategory -eq "BYOD")
 deviceManufacturer | Jeder string-Wert. | (device.deviceManufacturer -eq "Samsung")
 deviceModel | Jeder string-Wert. | (device.deviceModel -eq "iPad Air")
 deviceOwnership | Personal, Unternehmen, Unbekannt | (device.deviceOwnership -eq "Company")
 domainName | Jeder string-Wert. | (device.domainName -eq "contoso.com")
 enrollmentProfileName | Profilname für Apple-Geräteregistrierung oder für Windows AutoPilot | (device.enrollmentProfileName -eq "DEP iPhones")
 isRooted | true false | (device.isRooted -eq true)
 managementType | MDM (bei mobilen Geräten)<br>PC (bei Computern, die vom Intune-PC-Agent verwaltet werden) | (device.managementType -eq "MDM")
 organizationalUnit | ein beliebiger Zeichenfolgenwert, der mit dem Namen der Organisationseinheit, die von einem lokalen Active Directory festgelegt wurde, übereinstimmt | (device.organizationalUnit -eq "US PCs")
 deviceId | eine gültige Azure AD-Geräte-ID | (device.deviceId -eq "d4fe7726-5966-431c-b3b8-cddc8fdb717d")
 objectId | eine gültige Azure AD-Objekt-ID |  (device.objectId -eq 76ad43c9-32c5-45e8-a272-7b58b58f596d")



## <a name="changing-dynamic-membership-to-static-and-vice-versa"></a>Ändern der dynamischen Mitgliedschaft in statisch (und umgekehrt)
Sie können ändern, wie die Mitgliedschaft in einer Gruppe verwaltet wird. Dies ist hilfreich, wenn Sie im System den gleichen Gruppennamen und die dazugehörige ID beibehalten möchten, damit alle vorhandenen Verweise auf die Gruppe weiterhin gültig sind. Wenn eine neue Gruppe erstellt wird, müssen diese Verweise aktualisiert werden.

Wir haben das Azure AD Admin Center um Unterstützung für diese Funktionalität erweitert. Nun können Kunden entweder über Azure AD Admin Center oder PowerShell-Cmdlets vorhandene Gruppen aus der dynamischen Mitgliedschaft in zugewiesene Mitgliedschaft umwandeln und umgekehrt, wie unten dargestellt.

> [!WARNING]
> Wenn Sie eine vorhandene statische Gruppe in eine dynamische Gruppe ändern, werden alle vorhandenen Mitglieder aus der Gruppe entfernt, und anschließend wird die Mitgliedsschaftsregel verarbeitet, um neue Mitglieder hinzuzufügen. Wenn die Gruppe verwendet wird, um den Zugriff auf Apps oder Ressourcen zu steuern, haben die ursprünglichen Mitglieder unter Umständen erst wieder Zugriff, wenn die Mitgliedsschaftsregel vollständig verarbeitet wurde.
>
> Sie sollten die neue Mitgliedsschaftsregel vorher testen, um sicherzustellen, dass sich die neue Mitgliedschaft in der Gruppe wie erwartet verhält.

### <a name="using-azure-ad-admin-center-to-change-membership-management-on-a-group"></a>Verwenden von Azure AD Admin Center zum Ändern der Mitgliedschaftsverwaltung für eine Gruppe 

1. Melden Sie sich beim [Azure AD Admin Center](https://aad.portal.azure.com) über ein Konto an, das als globaler Administrator oder Benutzerkontoadministrator in Ihrem Mandanten konfiguriert ist.
2. Wählen Sie **Gruppen** aus.
3. Öffnen Sie in der Liste **Alle Gruppen** die Gruppe, die Sie ändern möchten.
4. Wählen Sie **Eigenschaften** aus.
5. Wählen Sie auf der Seite **Eigenschaften** für die Gruppe einen **Mitgliedschaftstyp** aus, je nach gewünschtem Mitgliedschaftstyp „Zugewiesen (statisch)“, „Dynamischer Benutzer“ oder „Dynamisches Gerät“. Für die dynamische Mitgliedschaft können Sie im Regel-Generator Optionen für eine einfache Regel auswählen oder selbst eine erweiterte Regel schreiben. 

Die folgenden Schritte sind ein Beispiel für die Änderung der Mitgliedschaft einer Gruppe von Benutzern von statisch zu dynamisch. 

1. Wählen Sie auf der Seite **Eigenschaften** für Ihre ausgewählte Gruppe den **Mitgliedschaftstyp** **Dynamischer Benutzer** aus, und wählen Sie dann in dem Dialogfeld, in dem die Änderungen der Gruppenmitgliedschaft erläutert werden, zum Fortfahren die Option „Ja“ aus. 
  
   ![Mitgliedschaftstyp „Dynamischer Benutzer“ auswählen](./media/groups-dynamic-membership/select-group-to-convert.png)
  
2. Wählen Sie **Dynamische Abfrage hinzufügen** aus, und geben Sie dann die Regel an.
  
   ![Regel eingeben](./media/groups-dynamic-membership/enter-rule.png)
  
3. Wählen Sie nach dem Erstellen der Regel unten auf der Seite **Abfrage hinzufügen** aus.
4. Wählen Sie für die Gruppe **Speichern** auf der Seite **Eigenschaften** aus, um die Änderungen zu speichern. Der **Mitgliedschaftstyp** der Gruppe wird sofort in der Gruppenliste aktualisiert.

> [!TIP]
> Wenn die eingegebene erweiterte Regel falsch war, kann dies zu Fehlern bei der Gruppenkonvertierung führen. Oben rechts im Portal wird eine Benachrichtigung angezeigt, die erläutert, warum die Regel nicht vom System angenommen werden konnte. Lesen Sie sie sorgfältig, um zu erfahren, wie die Regel angepasst werden kann, damit sie gültig ist.

### <a name="using-powershell-to-change-membership-management-on-a-group"></a>Verwenden von PowerShell zum Ändern der Mitgliedschaftsverwaltung für eine Gruppe

> [!NOTE]
> Zum Ändern der Eigenschaften dynamischer Gruppen müssen Sie Cmdlets aus **der Vorschauversion von** [Azure AD PowerShell Version 2](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0) verwenden. Sie können die Vorschau aus dem [PowerShell-Katalog](https://www.powershellgallery.com/packages/AzureADPreview) installieren.

Hier folgt ein Beispiel für Funktionen, die die Verwaltung der Mitgliedschaft für eine vorhandene Gruppe wechseln. In diesem Beispiel wird darauf geachtet, dass die GroupTypes-Eigenschaft ordnungsgemäß geändert wird und alle Werte erhalten bleiben, die dort vorhanden sind und keine Beziehung zur dynamischen Mitgliedschaft aufweisen.

```
#The moniker for dynamic groups as used in the GroupTypes property of a group object
$dynamicGroupTypeString = "DynamicMembership"

function ConvertDynamicGroupToStatic
{
    Param([string]$groupId)

    #existing group types
    [System.Collections.ArrayList]$groupTypes = (Get-AzureAdMsGroup -Id $groupId).GroupTypes

    if($groupTypes -eq $null -or !$groupTypes.Contains($dynamicGroupTypeString))
    {
        throw "This group is already a static group. Aborting conversion.";
    }


    #remove the type for dynamic groups, but keep the other type values
    $groupTypes.Remove($dynamicGroupTypeString)

    #modify the group properties to make it a static group: i) change GroupTypes to remove the dynamic type, ii) pause execution of the current rule
    Set-AzureAdMsGroup -Id $groupId -GroupTypes $groupTypes.ToArray() -MembershipRuleProcessingState "Paused"
}

function ConvertStaticGroupToDynamic
{
    Param([string]$groupId, [string]$dynamicMembershipRule)

    #existing group types
    [System.Collections.ArrayList]$groupTypes = (Get-AzureAdMsGroup -Id $groupId).GroupTypes

    if($groupTypes -ne $null -and $groupTypes.Contains($dynamicGroupTypeString))
    {
        throw "This group is already a dynamic group. Aborting conversion.";
    }
    #add the dynamic group type to existing types
    $groupTypes.Add($dynamicGroupTypeString)

    #modify the group properties to make it a static group: i) change GroupTypes to add the dynamic type, ii) start execution of the rule, iii) set the rule
    Set-AzureAdMsGroup -Id $groupId -GroupTypes $groupTypes.ToArray() -MembershipRuleProcessingState "On" -MembershipRule $dynamicMembershipRule
}
```
So erstellen Sie eine statische Gruppe
```
ConvertDynamicGroupToStatic "a58913b2-eee4-44f9-beb2-e381c375058f"
```
So erstellen Sie eine dynamische Gruppe
```
ConvertStaticGroupToDynamic "a58913b2-eee4-44f9-beb2-e381c375058f" "user.displayName -startsWith ""Peter"""
```
## <a name="next-steps"></a>Nächste Schritte
Diese Artikel enthalten zusätzliche Informationen zu Gruppen in Azure Active Directory.

* [Anzeigen vorhandener Gruppen](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Erstellen einer neuen Gruppe und Hinzufügen von Mitgliedern](../fundamentals/active-directory-groups-create-azure-portal.md)
* [Verwalten der Einstellungen einer Gruppe](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Verwalten der Mitgliedschaften einer Gruppe](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Verwalten dynamischer Regeln für Benutzer in einer Gruppe](groups-dynamic-membership.md)
