---
title: Synchronisieren von Gastbenutzerkonten mit Azure-Anmeldung per E-Mail-Adresse | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie Gastbenutzerkonten synchronisieren, die für die Anmeldung bei Anwendungen eine alternative ID verwenden.
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 04/19/2018
ms.author: billmath
ms.openlocfilehash: d21f124858a7f98227eb301a97b9837e3adbba68
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="synchronizing-guest-user-accounts-that-use-email-for-sign-in-preview"></a>Synchronisieren von Gastbenutzerkonten mit Anmeldung per E-Mail-Adresse (Vorschauversion)

>[!NOTE]
> Dieses Feature ist zurzeit als öffentliche Preview verfügbar.

Im folgenden Szenario sind in Ihrer lokalen AD-Umgebung externe Benutzer (beispielsweise Partner) vorhanden, die eine alternative Anmeldemethode verwenden.

Im vorherigen Beispiel arbeitet Nina Morin für Fabrikam und hat folgende E-Mail-Adresse: nmorin@fabrikam.com. Nina ist Contoso-Partner und benötigt Zugriff auf bestimmte Anwendungen von Contoso. Contoso hat ein Konto für Nina erstellt und ihr mitgeteilt, dass sie für die Anmeldung bei Anwendungen Ihre E-Mail-Adresse verwenden soll.

Dieses Szenario ist für die lokalen Anwendungen bestens geeignet. Contoso migriert diese Anwendungen nun aber in die Cloud und möchte die Vorgehensweise für seine Partner beibehalten.

Das ist die Situation, um die es in diesem Szenario geht.


## <a name="pre-requisites-and-assumptions"></a>Voraussetzungen und Annahmen
Dieser Abschnitt enthält eine Liste mit Voraussetzungen und Annahmen, mit denen Sie sich vertraut machen müssen, bevor Sie das Szenario einrichten.

### <a name="pre-requisites"></a>Voraussetzungen
- Globale Administratoranmeldeinformationen zum Konfigurieren von Azure AD Connect, zum Überprüfen von Domänen und zum Konfigurieren von Domänenverbundeinstellungen
- Azure AD Connect ab Version 1.1.524.0
- Überprüfte Domäne zum Festlegen des Cloud-UPN externer Benutzer (Beispiel: bmcontoso.com)
- Verbunddienst zur Authentifizierung Ihrer externen Benutzer. Mindestversion bei Verwendung von AD FS: 2012 R2
- Installation von MSOL PowerShell v1.1 auf einem Computer zur Überprüfung von Verbundeinstellungen. Weitere Informationen finden Sie unter [Azure ActiveDirectory (MSOnline)](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-1.0).


### <a name="assumptions"></a>Annahmen 
- Azure AD Connect wurde bereits erfolgreich eingerichtet und installiert. Informationen zum Installieren von Azure AD Connect finden Sie unter [Azure AD Connect und Verbund](active-directory-aadconnectfed-whatis.md).
In diesem Dokument wird Folgendes angenommen:
- Sie haben einen Verbunddienst eingerichtet, mit dem Benutzer erfolgreich authentifiziert werden.
- Externe Benutzer können sich mithilfe ihrer externen E-Mail-Adresse authentifizieren.
- - Die Anmeldung mit einer alternativen ID wurde eingerichtet und konfiguriert. Benutzer können sich mit ihrer alternativen ID authentifizieren. Weitere Informationen zum Einrichten einer alternativen ID mit AD FS finden Sie unter [Configure Alternate Login ID](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id) (Konfigurieren einer alternativen Anmelde-ID).

## <a name="task-1--prepare-the-environment"></a>Aufgabe 1: Vorbereiten der Umgebung
Die folgende Aufgabe dient in erster Linie zur Vorbereitung der Synchronisierung Ihrer externen Konten, um die Anmeldung per alternativer ID (beispielsweise unter Verwendung des E-Mail-Attributs) zu ermöglichen.

Definieren Sie die Elemente in der folgenden Tabelle, bevor Sie mit der zweiten Aufgabe fortfahren.

![Architecture](media/active-directory-aadconnect-guest-sync/guest2.png)

|Umgebungsaspekt|Verwendungszweck|Implementierung in Ihrer Umgebung|
|-----|-----|-----|
|Cloud-UPN-Attribut|Das Attribut, das den UPN der externen Benutzerobjekte in der Cloud auffüllt. Das UPN-Suffix für die externen Konten muss dem bei der Vorbereitung definierten Wert entsprechen. Hierbei handelt es sich um die überprüfte Domäne.|*Beispiel: userPrincipalName (nmorin@bmcontoso.com)|
|Anmeldeadresse|Das Attribut, das externe Benutzer bei der Anmeldung eingeben. Dieses Attribut muss das Format einer E-Mail-Adresse haben und entspricht in den meisten Fällen der tatsächlichen E-Mail-Adresse des externen Benutzers.|* Beispiel: mail (nmorin@fabrikam.com)|
|Azure AD Connect-Bereichsfilter|Der Filter, der es ermöglicht, die externen Identitäten als Ziel zu verwenden und den Geltungsbereich der Synchronisierungsregeln festzulegen, die im weiteren Verlauf dieses Leitfadens definiert werden. Der Bereich kann unter anderem mit einer vordefinierten Organisationseinheit in der Organisation, mit einer bestimmten Namenskonvention oder mit einer bestimmten Domäne festgelegt werden.|*Beispiel: OU enthält Externals|
|Azure AD-Mandant|Der Anzeigename des Azure AD-Mandanten für Azure AD Connect.|Beispiel: contoso.onmicrosoft.com|

Im folgenden Screenshot sind drei Elemente hervorgehoben:
- Die Organisationseinheit **Externals**, die im Azure AD Connect-Bereichsfilter verwendet wird und den Ort der externen Benutzer angibt.
- Das Attribut **mail**, das von den externen Benutzern zur Anmeldung verwendet wird.
- Das Attribut **userPrincipalName**, bei dem es sich um die überprüfte Domäne handelt, mit der die lokale Umgebung verbunden ist.

![Benutzer](media/active-directory-aadconnect-guest-sync/guest1.png)

## <a name="task-2--configure-azure-ad-connect"></a>Aufgabe 2: Konfiguration von Azure AD Connect
Nachdem Sie die obigen Informationen definiert haben, können Sie die Azure AD Connect-Synchronisierungsregeln einrichten. Verwenden Sie hierzu den Synchronisierungsregel-Editor von Azure AD Connect. Weitere Informationen zu diesem Editor finden Sie unter [Azure AD Connect-Synchronisierung: Grundlegendes zur deklarativen Bereitstellung](active-directory-aadconnectsync-understanding-declarative-provisioning.md).

### <a name="how-to-configure-the-synchronization-rule"></a>Konfigurieren der Synchronisierungsregel
Gehen Sie zum Konfigurieren von Azure AD Connect wie folgt vor:

1. Öffnen Sie unter **Start > Azure AD Connect > Synchronisierungsregel-Editor** den Synchronisierungsregel-Editor von Azure AD Connect.
2. Vergewissern Sie sich im Bildschirm **Synchronisierungsregel-Editor**, dass die Richtung **eingehend** lautet, und klicken Sie auf der rechten Seite auf **Neue Regel hinzufügen**.
3. Konfigurieren Sie auf der Seite **Beschreibung** Folgendes, und klicken Sie anschließend auf **Weiter**:
    - **Name:** Geben Sie einen Namen für die Regel ein. 
    - **Connected System** (Verbundenes System): Die lokale AD-Umgebung.
    - **Connected System Object Type** (Objekttyp des verbundenen Systems): Benutzer
    - **Metaverse Object Type** (Metaverse-Objekttyp): Person
    - **Linktyp:** Join
    - **Rangfolge:** 90
    - 
![](media/active-directory-aadconnect-guest-sync/guest3.png)

4. Klicken Sie im Bildschirm für den **Bereichsfilter** auf **Gruppe hinzufügen**.
5. Konfigurieren Sie den Filter mithilfe der Dropdownfelder. Geben Sie Folgendes ein, und klicken Sie anschließend auf **Weiter**. Dadurch wird ein Filter erstellt, der nur für Objekte in der externen Organisationseinheit gilt.
    - **Attribut:** dn
    - **Operator:** ENTHÄLT
    - **Wert:** Externals
 
 ![Filter](media/active-directory-aadconnect-guest-sync/guest4.png)

6. Klicken Sie im Bildschirm für die **Verknüpfungsregeln** auf **Weiter**.
7. Klicken Sie im Bildschirm **Transformationen** auf **Transformation hinzufügen**. Geben Sie Folgendes ein:
    - **FlowType:** Konstant
    - **Zielattribut:** userType
    - **Quelle:** Gast
8. Klicken Sie im Bildschirm **Transformationen** auf **Transformation hinzufügen**. Geben Sie Folgendes ein:
    - **FlowType:** Direkt
    - **Zielattribut:** onPremisesUserPrincipalName
    - **Quelle:** mail
9. Klicken Sie im Bildschirm **Transformationen** auf **Transformation hinzufügen**. Geben Sie Folgendes ein:
    - **FlowType:** Direkt
    - **Zielattribut:** userPrincipalName
    - **Quelle:** userPrincipalName ![Transformation](media/active-directory-aadconnect-guest-sync/guest5.png)
10. Klicken Sie auf **Hinzufügen**. 
11. Vergewissern Sie sich im Bildschirm **Synchronisierungsregel-Editor**, dass die Richtung **ausgehend** lautet, und klicken Sie auf der rechten Seite auf **Neue Regel hinzufügen**.
12. Konfigurieren Sie auf der Seite **Beschreibung** Folgendes, und klicken Sie anschließend auf **Weiter**:
    - **Name:** Geben Sie einen Namen für die Regel ein. 
    - **Connected System** (Verbundenes System): Der Azure AD-Mandant
    - **Connected System Object Type** (Objekttyp des verbundenen Systems): Benutzer
    - **Metaverse Object Type** (Metaverse-Objekttyp): Person
    - **Linktyp:** Join
    - **Rangfolge:** 90
    - 
![Regel](media/active-directory-aadconnect-guest-sync/guest6.png)

13. Klicken Sie im Bildschirm für den **Bereichsfilter** auf **Weiter**.
14. Klicken Sie im Bildschirm für die **Verknüpfungsregeln** auf **Weiter**.
15. Klicken Sie im Bildschirm **Transformationen** auf **Transformation hinzufügen**.  Geben Sie Folgendes ein:
    - **FlowType:** Direkt
    - **Zielattribut:** userType
    - **Quelle:** userType
9. Klicken Sie im Bildschirm **Transformationen** auf **Transformation hinzufügen**. Geben Sie Folgendes ein:
    - **FlowType:** Direkt
    - **Zielattribut:** onPremisesUserPrincipalName
    - **Quelle:** onPremisesUserPrincipalName ![Transformation](media/active-directory-aadconnect-guest-sync/guest7.png)
10. Klicken Sie auf **Hinzufügen**. 
11. Nachdem Sie diese Regeln konfiguriert haben, müssen Sie eine vollständige Synchronisierung ausführen. Verwenden Sie PowerShell, um eine vollständige Synchronisierung zu starten. Nach Abschluss der Synchronisierung können Sie mit dem nächsten Schritt fortfahren.

``` powershell
    Start-ADSyncSyncCycle -PolicyType Initial
```

## <a name="task-3--federation"></a>Aufgabe 3: Erstellen des Verbunds
Die folgende Aufgabe liefert Informationen zu einigen Dingen, die Sie für das Szenario benötigen.

Ihre Verbundeinstellungen können Sie mithilfe von Azure AD PowerShell überprüfen. In diesem Dokument wird MSOL PowerShell v1.1 verwendet. Eine Installationsanleitung für diese Version finden Sie [hier](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-1.0).

### <a name="verify-the-federation-settings"></a>Überprüfen der Verbundeinstellungen
Gehen Sie wie folgt vor, um die Verbundeinstellungen zu überprüfen:
1. Öffnen Sie Windows PowerShell, und stellen Sie mithilfe des folgenden Befehls eine Verbindung mit Azure AD her:
``` powershell
      Connect-MSOLservice
```
2.  Geben Sie die globalen Administratoranmeldeinformationen ein.
3.  Geben Sie als Nächstes den folgenden Befehl ein:
  ``` powershell
      Get-MSOLDomainFederationSettings
  ```
4.   Die Verbundinformationen werden zurückgegeben. **ActiveLogonUri** ist die URL des Verbundservers.

  ![Verbund](media/active-directory-aadconnect-guest-sync/guest8.png)

### <a name="verify-alternate-login-id"></a>Überprüfen der alternativen Anmelde-ID
In diesem Dokument wird AD FS als Identitätsanbieter (Identity Provider, IDP) verwendet. Bei Verwendung eines anderen IDPs müssen ggf. etwas andere Schritte ausgeführt werden.

1. Öffnen Sie Windows PowerShell, und geben Sie den folgenden Befehl ein:
   ```powershell
    Get-ADFSClaimsProviderTrust
   ```
2. Die AD FS-Informationen werden angezeigt.  Beachten Sie die Werte für **AlternateLoginID** und **LookupForests**.

![ADFS](media/active-directory-aadconnect-guest-sync/guest9.png)

## <a name="task-4--testing"></a>Aufgabe 4: Testen
Um sich zu vergewissern, dass alles ordnungsgemäß funktioniert, müssen Sie sich bei einem Endpunkt anmelden, der für die Authentifizierung mit dem IDP konfiguriert wurde. Wir haben in Azure eine Website zu Testzwecken bereitgestellt und verwenden die folgende URL: contososampapp.azurewebsites.net

### <a name="verify-that-you-can-sign-in-with-the-alternate-id"></a>Vergewissern, dass Sie sich mit der alternativen ID anmelden können
1. Melden Sie bei dem Endpunkt an.</br>
![Endpunkt](media/active-directory-aadconnect-guest-sync/guest10.png)
1. Nach Eingabe Ihres Benutzernamens werden Sie zur Verbundanmeldeseite weitergeleitet.
![Verbund](media/active-directory-aadconnect-guest-sync/guest11.png)
1. Geben Sie Ihre Anmeldeinformationen ein.
2. Sie sollten nun erfolgreich angemeldet sein.
![Erfolgreich](media/active-directory-aadconnect-guest-sync/guest12.png)

## <a name="next-steps"></a>Nächste Schritte
- [Eigenschaften eines Azure Active Directory B2B-Zusammenarbeitsbenutzers](../../active-directory/active-directory-b2b-user-properties.md#key-properties-of-the-azure-ad-b2b-collaboration-user)
- [Configuring Alternate Login ID](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id) (Konfigurieren einer alternativen Anmelde-ID)
- [Azure AD Connect: Versionsfreigabeverlauf](active-directory-aadconnect-version-history.md)
