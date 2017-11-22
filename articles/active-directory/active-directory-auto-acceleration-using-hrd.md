---
title: "Konfigurieren der automatischen Anmeldebeschleunigung für eine Anwendung per Richtlinie für die Startbereichsermittlung | Microsoft-Dokumentation"
description: "Hier wird erklärt, was ein Azure AD-Mandant ist und wie Azure über Azure Active Directory verwaltet wird."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: it-pro
ms.date: 11/09/2017
ms.author: billmath
ms.openlocfilehash: b1f0e5da09ef1d6acd234b72878cc71d66bb551e
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2017
---
# <a name="configure-sign-in-auto-acceleration-for-an-application-using-home-realm-discovery-hrd-policy"></a>Konfigurieren der automatischen Anmeldebeschleunigung für eine Anwendung per Richtlinie für die Startbereichsermittlung (HDR)

## <a name="introduction-to-home-realm-discovery-and-auto-acceleration"></a>Einführung in die Startbereichsermittlung und automatische Beschleunigung
Das folgende Dokument bietet eine Einführung in die Startbereichsermittlung und automatische Beschleunigung.

### <a name="home-realm-discovery"></a>Startbereichsermittlung
Startbereichsermittlung bezeichnet den Prozess, der es Azure Active Directory gestattet, während der Anmeldung zu ermitteln, wo sich ein Benutzer authentifizieren muss.  Wenn der Benutzer sich bei einem Azure AD-Mandanten oder auf der allgemeinen Anmeldeseite von Azure AD anmeldet, um auf eine Ressource zuzugreifen, gibt er einen Benutzernamen (UPN) ein.  Azure AD verwendet diesen, um zu ermitteln, wo sich der Benutzer anmelden muss.   Der Benutzer muss möglicherweise zu einer der folgenden Optionen geführt werden, um sich zu authentifizieren:

- Basismandant des Benutzers (dies kann derselbe Mandant wie die Ressource sein, auf die der Benutzer zuzugreifen möchte) 
- Microsoft-Konto:   Der Benutzer ist ein Gast des Ressourcenmandanten
- Anderer Identitätsanbieter, der vom Azure AD-Mandanten unterstützt wird  Lokaler Identitätsanbieter wie AD FS

### <a name="auto-acceleration"></a>Automatische Beschleunigung 
Einige Unternehmen konfigurieren ihren Azure Active Directory-Mandanten so, dass er mit einem anderen Identitätsanbieter wie AD FS zur Benutzerauthentifizierung einen Verbund bildet.  In diesen Fällen wird dem Benutzer bei der Anmeldung an einer Anwendung zunächst eine Azure AD-Anmeldeseite angezeigt. Nachdem er seinen UPN eingegeben hat, wird er zur Anmeldeseite des Identitätsanbieters weitergeleitet.  In Situationen, in denen es sinnvoll ist, können Administratoren es möglicherweise als vorteilhaft empfinden, wenn der Benutzer direkt auf die Anmeldeseite geleitet werden kann, wenn er sich bei bestimmten Anwendungen anmeldet, wobei die anfängliche Azure Active Directory-Seite übersprungen wird. Dies wird als „automatische Beschleunigung der Anmeldung“ bezeichnet.

In Fällen, in denen sich der Mandant für die Anmeldung mit einem anderen Identitätsanbieter verbündet, gestaltet sich die Benutzeranmeldung durch die Aktivierung der automatischen Beschleunigung in Situationen effizienter, in denen Sie wissen, dass jeder, der sich anmeldet, von diesem Identitätsanbieter authentifiziert werden kann.  Sie können die automatische Beschleunigung für einzelne Anwendungen konfigurieren.

>[!NOTE]
>Wenn Sie eine Anwendung für die automatische Beschleunigung konfigurieren, können sich Gastbenutzer nicht anmelden. Den Benutzer für die Authentifizierung direkt zu einem verbündeten Identitätsanbieter zu leiten, ist eine unidirektionale Option, da es keine Möglichkeit gibt, zurück zur Azure Active Directory-Anmeldeseite zu gelangen.  Gastbenutzer, die möglicherweise zur Authentifizierung an andere Mandanten oder einen externen Identitätsanbieter (z. B. Microsoft-Konto) weitergeleitet werden müssen, können sich nicht bei dieser Anwendung anmelden, da der Schritt zur Startbereichsermittlung übersprungen wird.  

Es gibt zwei Möglichkeiten, die automatische Beschleunigung zu einem verbündeten Identitätsanbieter zu steuern.  Optionen:    

- Verwenden Sie einen Domänenhinweis auf Authentifizierungsanforderungen für eine Anwendung. 
- Konfigurieren Sie eine Richtlinie zur Startbereichsermittlung, um die automatische Beschleunigung zu aktivieren.

## <a name="domain-hints"></a>Domänenhinweise 
Domänenhinweise sind in der Authentifizierungsanforderung einer Anwendung enthaltene Anweisungen.  Sie können verwendet werden, um den Benutzer beschleunigt zur Anmeldeseite des Partneridentitätsanbieters zu leiten, oder sie können von einer Anwendung für mehrere Mandanten verwendet werden, um den Benutzer beschleunigt direkt zur organisationsspezifischen Azure AD-Anmeldeseite für ihren Mandanten zu leiten.  Die Anwendung „largeapp.com“ könnte es z. B. ihren Kunden ermöglichen, auf die Anwendung über eine benutzerdefinierte URL wie „contoso.largeapp.com“ zuzugreifen. Dabei könnte sie einen Domänenhinweis auf „Contoso.com“ in die Authentifizierungsanforderung einbeziehen. Die Syntax der Domänenhinweise variiert je nach verwendetem Protokoll und wird in der Regel in der Anwendung konfiguriert.

**WS-Verbund**: „whr=contoso.com“ in der Abfragezeichenfolge

**SAML**: Entweder eine SAML-Authentifizierungsanforderung, die einen Domänenhinweis enthält, oder eine Abfragezeichenfolge „whr=contoso.com“

**Open ID Connect**: Eine Abfragezeichenfolge „domain_hint=contoso.com“ 

Wenn ein Domänenhinweis in der Authentifizierungsanforderung der Anwendung enthalten ist und der Mandant mit dieser Domäne verbunden ist, versucht Azure AD, die Anmeldung an den für diese Domäne konfigurierten Identitätsanbieter umzuleiten.  Wenn sich der Domänenhinweis nicht auf eine verifizierte Verbunddomäne bezieht, wird er ignoriert und die normale Startbereichsermittlung wird aufgerufen.

Weitere Informationen zur automatischen Beschleunigung mithilfe von Domänenhinweisen, die von Azure Active Directory unterstützt werden, finden Sie in diesem [Blogbeitrag](https://cloudblogs.microsoft.com/enterprisemobility/2015/02/11/using-azure-ad-to-land-users-on-their-custom-login-page-from-within-your-app/).

>[!NOTE]
>Wenn ein Domänenhinweis in einer Authentifizierungsanforderung enthalten ist, setzt seine Anwesenheit jede Richtlinie zur Startbereichsermittlung außer Kraft, die für die Anwendung festgelegt ist.

## <a name="home-realm-discovery-hrd-policy"></a>Richtlinie zur Startbereichsermittlung (Home Realm Discovery, HDR)
Einige Anwendungen bieten keine Möglichkeit, die von ihnen gesendete Authentifizierungsanforderung zu konfigurieren, und in diesen Fällen ist es nicht möglich, Domänenhinweise zur Steuerung der automatischen Beschleunigung zu verwenden.   Die automatische Beschleunigung kann über Richtlinien konfiguriert werden, um das gleiche Verhalten zu erreichen.  

### <a name="setting-hrd-policy"></a>Festlegen der Richtlinie zur Startbereichsermittlung
Das Festlegen der automatischen Beschleunigung für die Anmeldung an einer Anwendung umfasst drei Schritte.


1. Erstellen einer Startbereichsermittlungsrichtlinie für die automatische Beschleunigung
2. Suchen des Dienstprinzipals, an den die Richtlinie angefügt wird
3. Anfügen der Richtlinie an den Dienstprinzipal  Die Richtlinien wurden möglicherweise in einem Mandanten erstellt, aber sie haben keinerlei Auswirkung, bis sie an eine Entität angefügt werden.  Eine Richtlinie zur Startbereichsermittlung kann an einen Dienstprinzipal angefügt werden, und es kann jeweils nur eine Richtlinie gleichzeitig für eine bestimmte Entität aktiv sein.  

Sie können entweder direkt die Microsoft Azure Active Directory Graph-API oder die Azure Active Directory PowerShell-Cmdlets verwenden, um die automatische Beschleunigung mithilfe der Richtlinie zur Startbereichsermittlung einzurichten.

Die Graph-API, die Richtlinien beeinflusst, wird [hier](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) beschrieben.

Hier folgt eine Beispieldefinition für eine Richtlinie zur Startbereichsermittlung:
    
 ```
   {  
    "HomeRealmDiscoveryPolicy":
    {  
    "AccelerateToFederatedDomain":true,
    "PreferredDomain":"federated.example.edu"
    }
   }
```

Der Richtlinientyp ist „HomeRealmDiscoveryPolicy“.

Wenn **AccelerateToFederatedDomain** den Wert „false“ aufweist, hat die Richtlinie keine Auswirkung. **PreferredDomain** sollte eine Domäne für die Beschleunigung angeben und kann ausgelassen werden, wenn der Mandant nur eine einzige Verbunddomäne aufweist.  Wenn diese Angabe ausgelassen wird und mehrere überprüfte Verbunddomänen verfügbar sind, hat die Richtlinie keine Auswirkung.

Wenn **PreferredDomain** angegeben ist, muss es mit einer überprüften Verbunddomäne für den Mandanten übereinstimmen, und alle Benutzer der betreffenden Anwendung müssen sich bei dieser Domäne anmelden können.

### <a name="priority-and-evaluation-of-hrd-policies"></a>Priorität und Auswertung von Richtlinien zur Startbereichsermittlung
Richtlinien zur Startbereichsermittlung können erstellt und dann bestimmten Organisationen und Dienstprinzipalen zugewiesen werden. Dies bedeutet, dass mehrere Richtlinien auf eine bestimmte Anwendung angewandt werden können. Die Richtlinie zur Startbereichsermittlung, die in Kraft tritt, folgt diesen Regeln:


- Wenn in der Authentifizierungsanforderung ein Domänenhinweis vorhanden ist, wird jede Richtlinie zur Startbereichsermittlung ignoriert und das durch den Domänenhinweis angegebene Verhalten verwendet.
- Andernfalls wird sie erzwungen, wenn eine Richtlinie explizit dem Dienstprinzipal zugewiesen ist. 
- Wenn kein Domänenhinweis vorhanden und dem Dienstprinzipal nicht explizit eine Richtlinie zugewiesen ist, wird eine Richtlinie erzwungen, die ausdrücklich der übergeordneten Organisation des Dienstprinzipals zugewiesen ist. 
- Wenn kein Domänenhinweis vorhanden ist und dem Dienstprinzipal oder der Organisation keine Richtlinie zugewiesen wurde, wird das standardmäßige Verhalten der Startbereichsermittlung verwendet.

## <a name="tutorial-for-setting-sign-in-auto-acceleration-on-an-application-using-hrd-policy-with-azure-active-directory-powershell-cmdlets"></a>Tutorial zum Festlegen der automatischen Beschleunigung der Anmeldung für eine Anwendung per Richtlinie zur Startbereichsermittlung mit PowerShell-Cmdlets von Azure Active Directory
Wir werden einige Szenarios durchgehen, beispielsweise:


- Einrichten der automatischen Beschleunigung für eine Anwendung für einen Mandanten mit einer einzelnen Verbunddomäne
- Einrichten der automatischen Beschleunigung für eine Anwendung für eine von mehreren Domänen, die für Ihren Mandanten überprüft wurden
- Auflisten der Anwendungen, für die eine Richtlinie konfiguriert ist

### <a name="prerequisites"></a>Voraussetzungen
In den folgenden Beispielen erstellen, aktualisieren, verknüpfen und löschen Sie Richtlinien für Anwendungsdienstprinzipale in Azure AD.

1.  Laden Sie zunächst das aktuelle Azure AD PowerShell-Cmdlet (Vorschau) herunter. 
2.  Nach dem Herunterladen des Azure AD PowerShell-Cmdlets führen Sie den Befehl „Connect“ aus, um sich bei Azure AD mit Ihrem Administratorkonto anzumelden.

    ``` powershell
    Connect-AzureAD -Confirm
    ```
3.  Führen Sie den folgenden Befehl aus, um alle Richtlinien in Ihrer Organisation anzuzeigen.

    ``` powershell
    Get-AzureADPolicy
    ```

Wenn nichts zurückgegeben wird, haben Sie keine Richtlinien in Ihrem Mandanten erstellt.

### <a name="example-setting-auto-acceleration-for-an-application"></a>Beispiel: Festlegen der automatischen Beschleunigung für eine Anwendung 
In diesem Beispiel erstellen Sie eine Richtlinie, die Benutzer beim Anmelden an einer Anwendung automatisch beschleunigt zu einem AD FS-Anmeldebildschirm leitet.  Dies geschieht, ohne dass sie zuerst einen Benutzernamen auf der Azure AD-Anmeldeseite eingeben müssen. 

#### <a name="step-1-create-an-hrd-policy"></a>Schritt 1: Erstellen einer Richtlinie zur Startbereichsermittlung
``` powershell
New-AzureADPoly -Definition @("{`"HomeRealmDiscoveryPolicy`":{`"AccelerateToFederatedDomain`":true}}") -DisplayName BasicAutoAccelerationPolicy -Type HomeRealmDiscoveryPolicy
```

Wenn Sie über eine einzige Verbunddomäne verfügen, die Benutzer für Anwendungen authentifiziert, müssen Sie nur eine Richtlinie zur Startbereichsermittlung erstellen.  
Um Ihre neue Richtlinie und deren ObjectID abzurufen, führen Sie den folgenden Befehl aus.

``` powershell
Get-AzureADPolicy
```


Sobald Sie über eine Richtlinie zur Startbereichsermittlung verfügen, können Sie diese mehreren Anwendungsdienstprinzipalen zuordnen, um die automatische Beschleunigung zu aktivieren.

#### <a name="step-2-locate-the-service-principal-to-assign-the-policy-to"></a>Schritt 2: Suchen des Dienstprinzipals, dem die Richtlinie zugewiesen wird  
Sie benötigen die Objekt-ID der Dienstprinzipale, denen Sie die Richtlinie zuweisen möchten. Es gibt mehrere Möglichkeiten, die Objekt-ID von Dienstprinzipalen zu ermitteln.    

Sie können das Portal verwenden, [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) abfragen oder zu unserem [Graph-Testertool](https://graphexplorer.cloudapp.net/) wechseln und sich bei Ihrem Azure AD-Konto anmelden, um alle Dienstprinzipale Ihrer Organisation anzuzeigen, oder Sie können, da Sie PowerShell verwenden, das Cmdlet „get-AzureADServicePrincipal“ verwenden, um die Dienstprinzipale und ihre IDs aufzulisten.

#### <a name="step-3-assign-the-policy-to-your-service-principal"></a>Schritt 3: Zuweisen der Richtlinie zu Ihrem Dienstprinzipal  
Sobald Sie über die Objekt-ID des Dienstprinzipals der Anwendung verfügen, für die Sie die automatische Beschleunigung konfigurieren möchten, führen Sie den folgenden Befehl aus, um die in Schritt 1 erstellte Richtlinie zur Startbereichsermittlung mit dem Dienstprinzipal zu verknüpfen, den Sie in Schritt 2 ermittelt haben.

``` powershell
Add-AzureADServicePrincipalPolicy -Id <ObjectID of the Service Principal> -RefObjectId <ObjectId of the Policy>
```

Sie können diesen Befehl für jeden Dienstprinzipal wiederholen, dem Sie die Richtlinie hinzufügen möchten.

#### <a name="step-4-check-which-application-service-principals-your-auto-acceleration-policy-is-assigned-to"></a>Schritt 4: Überprüfen, welchen Anwendungsdienstprinzipalen Ihre Richtlinie zur automatischen Beschleunigung zugewiesen ist
Um zu überprüfen, welche Anwendungen über eine konfigurierte Richtlinie zur automatischen Beschleunigung verfügen, verwenden Sie das Cmdlet „Get-AzureADPolicyAppliedObject“ und übergeben Sie ihm die Objekt-ID der Richtlinie, die Sie überprüfen möchten.

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```
#### <a name="step-5-youre-done"></a>Schritt 5: Sie haben es geschafft!
Testen Sie die Anwendung, um zu überprüfen, ob die neue Richtlinie funktioniert.

### <a name="example-listing-the-applications-for-which-an-auto-acceleration-policy-is-configured"></a>Beispiel: Auflisten der Anwendungen, für die eine Richtlinie zur automatischen Beschleunigung konfiguriert ist

#### <a name="step-1-list-all-policies-created-in-your-organization"></a>Schritt 1: Auflisten aller Richtlinien, die in Ihrer Organisation erstellt wurden 

``` powershell
Get-AzureADPolicy
```

Notieren Sie die **Objekt-ID** der Richtlinie, für die Sie Zuweisungen auflisten möchten.

#### <a name="step-2-list-the-service-principals-the-policy-is-assigned-to"></a>Schritt 2: Auflisten der Dienstprinzipale, denen die Richtlinie zugewiesen ist  

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```

### <a name="example-removing-an-auto-acceleration-policy-for-an-application"></a>Beispiel: Entfernen einer Richtlinie zur automatischen Beschleunigung für eine Anwendung
#### <a name="step-1-use-the-previous-example-to-get-the-objectid-of-the-policy-and-that-of-the-application-service-principal-you-wish-to-remove-it-from"></a>Schritt 1: Verwenden des vorherigen Beispiels, um die Objekt-ID der Richtlinie und die Objekt-ID des Anwendungsdienstprinzipals zu erhalten, von dem Sie sie entfernen möchten
#### <a name="step-2-remove-the-policy-assignment-from-the-application-service-principal"></a>Schritt 2: Entfernen der Richtlinienzuweisung vom Anwendungsdienstprinzipal  

``` powershell
Remove-AzureADApplicationPolicy -ObjectId <ObjectId of the Service Principal>  -PolicyId <ObjectId of the policy>
```

#### <a name="step-3-check-removal-by-listing-the-service-principals-the-policy-is-assigned-to"></a>Schritt 3: Überprüfen der Entfernung durch Auflisten der Dienstprinzipale, denen die Richtlinie zugewiesen ist 

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```
## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zur Funktionsweise der Authentifizierung in Azure AD finden Sie unter [Authentifizierungsszenarien für Azure AD](develop/active-directory-authentication-scenarios.md).
- Weitere Informationen zum einmaligen Anmelden von Benutzern finden Sie unter [Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory](active-directory-enterprise-apps-manage-sso.md).
- Im [Active Directory-Entwicklerhandbuch](develop/active-directory-developers-guide.md) finden Sie eine Übersicht über alle für Entwickler relevanten Inhalte.
