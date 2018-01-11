---
title: "Konfigurieren der automatischen Anmeldebeschleunigung für eine Anwendung mit einer Richtlinie für die Startbereichsermittlung | Microsoft Docs"
description: "Dieser Artikel erläutert, was ein Azure AD-Mandant ist und wie Azure über Azure Active Directory verwaltet wird."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: it-pro
ms.date: 11/09/2017
ms.author: billmath
ms.openlocfilehash: e2e6e5c40dc4a9f67f94c45f8394512db3f777f5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2017
---
# <a name="configure-sign-in-auto-acceleration-for-an-application-by-using-a-home-realm-discovery-policy"></a>Konfigurieren der automatischen Anmeldebeschleunigung für eine Anwendung mit einer Richtlinie für die Startbereichsermittlung (Home Realm Discovery, HDR)

Das folgende Dokument bietet eine Einführung in die Startbereichsermittlung und automatische Beschleunigung.

## <a name="home-realm-discovery"></a>Startbereichsermittlung (Home Realm Discovery, HDR)
Startbereichsermittlung (Home Realm Discovery, HDR) bezeichnet den Prozess, der es Azure Active Directory (Azure AD) gestattet, während der Anmeldung zu ermitteln, wo sich ein Benutzer authentifizieren muss.  Wenn sich ein Benutzer bei einem Azure AD-Mandanten oder auf der allgemeinen Anmeldeseite von Azure AD anmeldet, um auf eine Ressource zuzugreifen, gibt er einen Benutzernamen (UPN) ein. Azure AD verwendet diesen, um zu ermitteln, wo sich der Benutzer anmelden muss. 

Der Benutzer muss möglicherweise an einen der folgenden Speicherorte weitergeleitet werden, um sich zu authentifizieren:

- Basismandant des Benutzers (dies kann der gleiche Mandant wie die Ressource sein, auf die der Benutzer zugreifen möchte). 

- Microsoft-Konto:  Der Benutzer ist ein Gast des Ressourcenmandanten.

- Anderer Identitätsanbieter, der einen Verbund mit dem Azure AD-Mandanten bildet.

-  Lokaler Identitätsanbieter, z.B. Active Directory-Verbunddienste (AD FS).

## <a name="auto-acceleration"></a>Automatische Beschleunigung 
Einige Unternehmen konfigurieren ihren Azure Active Directory-Mandanten so, dass er mit einem anderen Identitätsanbieter wie AD FS zur Benutzerauthentifizierung einen Verbund bildet.  

Wenn sich ein Benutzer unter diesen Umständen bei einer Anwendung anmeldet, wird zunächst eine Azure AD-Anmeldeseite angezeigt. Nachdem er seinen UPN eingegeben hat, wird er auf die IdP-Anmeldeseite weitergeleitet. Unter bestimmten Umständen können Administratoren Benutzer auf die Anmeldeseite weiterleiten, wenn sie sich bei bestimmten Anwendungen anmelden. 

Das bedeutet, dass Benutzer die erste Seite von Azure Active Directory überspringen können. Dieser Vorgang wird als „automatische Beschleunigung der Anmeldung“ bezeichnet.

In den Fällen, in denen der Mandant mit einem anderen IdP für die Anmeldung einen Verbund bildet, wird die Benutzeranmeldung durch die automatische Beschleunigung optimiert.  Sie können die automatische Beschleunigung für einzelne Anwendungen konfigurieren.

>[!NOTE]
>Wenn Sie eine Anwendung für die automatische Beschleunigung konfigurieren, können sich Gastbenutzer nicht anmelden. Wenn Sie einen Benutzer direkt an einen Verbund-IdP für die Authentifizierung weiterleiten, gibt es für ihn keine Möglichkeit, zur Anmeldeseite von Azure Active Directory zurückzukehren. Gastbenutzer, die möglicherweise an andere Mandanten oder an einen externen IdP wie ein Microsoft-Konto weitergeleitet werden müssen, können sich nicht bei dieser Anwendung anmelden, weil sie den Schritt „Startbereichsermittlung“ (Home Realm Discovery, HDR) überspringen.  

Es gibt zwei Möglichkeiten, die automatische Beschleunigung zu einem Verbundidentitätsanbieter zu steuern:   

- Verwenden Sie einen Domänenhinweis für Authentifizierungsanforderungen für eine Anwendung. 
- Konfigurieren Sie eine Richtlinie zur Startbereichsermittlung, um die automatische Beschleunigung zu aktivieren.

## <a name="domain-hints"></a>Domänenhinweise 
Domänenhinweise sind in der Authentifizierungsanforderung einer Anwendung enthaltene Anweisungen. Sie können verwendet werden, um die beschleunigte Anmeldung des Benutzers auf der Anmeldeseite seines Verbundidentitätsanbieters zu ermöglichen. Sie können auch von einer Anwendung für mehrere Mandanten verwendet werden, um den Benutzer beschleunigt direkt zur organisationsspezifischen Azure AD-Anmeldeseite für ihren Mandanten zu leiten.  

Die Anwendung „largeapp.com“ könnte es z.B. ihren Kunden ermöglichen, auf die Anwendung über eine benutzerdefinierte URL wie „contoso.largeapp.com“ zuzugreifen. Dabei könnte die App einen Domänenhinweis auf „contoso.com“ in die Authentifizierungsanforderung einbeziehen. 

Die Syntax der Domänenhinweise variiert je nach verwendetem Protokoll und wird in der Regel in der Anwendung konfiguriert.

**WS-Verbund**: „whr=contoso.com“ in der Abfragezeichenfolge.

**SAML**: Eine SAML-Authentifizierungsanforderung, die einen Domänenhinweis enthält, oder eine Abfragezeichenfolge „whr=contoso.com“.

**Open ID Connect**: Eine Abfragezeichenfolge „domain_hint=contoso.com“. 

Wenn ein Domänenhinweis in der Authentifizierungsanforderung der Anwendung enthalten und der Mandant mit dieser Domäne verbunden ist, versucht Azure AD, die Anmeldung an den für diese Domäne konfigurierten Identitätsanbieter umzuleiten. 

Wenn sich der Domänenhinweis nicht auf eine überprüfte Verbunddomäne bezieht, wird diese ignoriert und die normale Startbereichsermittlung (Home Realm Discovery, HDR) aufgerufen.

Weitere Informationen zur automatischen Beschleunigung unter Verwendung der Domänenhinweise, die von Azure Active Directory unterstützt werden, finden Sie im Blog [Enterprise Mobility + Security](https://cloudblogs.microsoft.com/enterprisemobility/2015/02/11/using-azure-ad-to-land-users-on-their-custom-login-page-from-within-your-app/).

>[!NOTE]
>Wenn ein Domänenhinweis in einer Authentifizierungsanforderung enthalten ist, setzt sein Vorhandensein jede Richtlinie zur Startbereichsermittlung außer Kraft, die für die Anwendung festgelegt ist.

## <a name="home-realm-discovery-policy"></a>Richtlinie zur Startbereichsermittlung
Einige Anwendungen bieten keine Möglichkeit, die von ihnen ausgegebene Authentifizierungsanforderung zu konfigurieren. In diesen Fällen ist es nicht möglich, Domänenhinweise zur Steuerung der automatischen Beschleunigung zu verwenden. Die automatische Beschleunigung kann über Richtlinien konfiguriert werden, um das gleiche Verhalten zu erreichen.  

### <a name="set-hrd-policy"></a>Festlegen der Richtlinie zur Startbereichsermittlung
Das Festlegen der automatischen Beschleunigung für die Anmeldung an einer Anwendung umfasst drei Schritte:


1. Erstellen einer Startbereichsermittlungsrichtlinie für die automatische Beschleunigung.

2. Ermitteln des Dienstprinzipals, an den die Richtlinie angefügt wird.

3. Anfügen der Richtlinie an den Dienstprinzipal Die Richtlinien wurden möglicherweise in einem Mandanten erstellt, aber sie besitzen keinerlei Auswirkungen, bis sie an eine Entität angefügt werden. 

Eine Richtlinie zur Startbereichsermittlung kann an einen Dienstprinzipal angefügt werden, und es kann jeweils nur eine Richtlinie gleichzeitig für eine bestimmte Entität aktiv sein.  

Sie können entweder direkt die Microsoft Azure Active Directory Graph-API oder die Azure Active Directory PowerShell-Cmdlets verwenden, um die automatische Beschleunigung mithilfe der Richtlinie zur Startbereichsermittlung einzurichten.

Die Graph-API, die die Richtlinie manipuliert, wird im Artikel [Vorgänge für die Richtlinie](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) auf MSDN beschrieben.

Das folgende Beispiel zeigt eine Definition für eine Richtlinie zur Startbereichsermittlung:
    
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

Wenn **AccelerateToFederatedDomain** FALSE ist, besitzt die Richtlinie keine Auswirkungen.

**PreferredDomain** sollte eine Domäne angeben, zu der die Beschleunigung erfolgt. Die Angabe kann ausgelassen werden, wenn der Mandant nur eine Verbunddomäne besitzt.  Wenn diese Angabe ausgelassen wird und mehrere überprüfte Verbunddomänen verfügbar sind, besitzt die Richtlinie keine Auswirkungen.

Wenn **PreferredDomain** angegeben wird, muss diese Angabe mit einer überprüften Verbunddomäne für den Mandanten übereinstimmen. Alle Benutzer der Anwendung müssen sich bei der Domäne anmelden können.

### <a name="priority-and-evaluation-of-hrd-policies"></a>Priorität und Auswertung von Richtlinien zur Startbereichsermittlung
Richtlinien zur Startbereichsermittlung können erstellt und dann bestimmten Organisationen und Dienstprinzipalen zugewiesen werden. Dies bedeutet, dass mehrere Richtlinien auf eine bestimmte Anwendung angewandt werden können. Die Richtlinie zur Startbereichsermittlung, die in Kraft tritt, folgt diesen Regeln:


- Wenn in der Authentifizierungsanforderung ein Domänenhinweis vorhanden ist, wird jede Richtlinie zur Startbereichsermittlung ignoriert. Das Verhalten, das vom Domänenhinweis angegeben wird, wird verwendet.

- Andernfalls wird sie erzwungen, wenn eine Richtlinie explizit dem Dienstprinzipal zugewiesen ist. 

- Wenn kein Domänenhinweis vorhanden und dem Dienstprinzipal nicht explizit eine Richtlinie zugewiesen ist, wird eine Richtlinie erzwungen, die ausdrücklich der übergeordneten Organisation des Dienstprinzipals zugewiesen ist. 

- Wenn kein Domänenhinweis vorhanden ist und dem Dienstprinzipal oder der Organisation keine Richtlinie zugewiesen wurde, wird das standardmäßige Verhalten der Startbereichsermittlung verwendet.

## <a name="tutorial-for-setting-sign-in-auto-acceleration-on-an-application-by-using-an-hrd-policy"></a>Tutorial zum Festlegen der automatischen Beschleunigung für eine Anmeldeseite für eine Anwendung mithilfe einer Richtlinie zur Startbereichsermittlung
Wir verwenden Azure AD PowerShell-Cmdlets, um einige Szenarien zu erläutern, darunter:


- Einrichten der automatischen Beschleunigung für eine Anwendung für einen Mandanten mit einer einzigen Verbunddomäne.

- Einrichten der automatischen Beschleunigung für eine Anwendung für eine von mehreren Domänen, die für Ihren Mandanten überprüft wurden.

- Auflisten der Anwendungen, für die eine Richtlinie konfiguriert ist.

### <a name="prerequisites"></a>Voraussetzungen
In den folgenden Beispielen erstellen, aktualisieren, verknüpfen und löschen Sie Richtlinien für Anwendungsdienstprinzipale in Azure AD.

1.  Laden Sie zunächst die aktuelle Preview zu Azure AD PowerShell-Cmdlets herunter. 

2.  Nach dem Herunterladen der Azure AD PowerShell-Cmdlets führen Sie den Befehl „Connect“ aus, um sich bei Azure AD mit Ihrem Administratorkonto anzumelden:

    ``` powershell
    Connect-AzureAD -Confirm
    ```
3.  Führen Sie den folgenden Befehl aus, um alle Richtlinien in Ihrer Organisation anzuzeigen:

    ``` powershell
    Get-AzureADPolicy
    ```

Wenn nichts zurückgegeben wird, bedeutet dies, dass in Ihrem Mandanten keine Richtlinien erstellt wurden.

### <a name="example-set-auto-acceleration-for-an-application"></a>Beispiel: Festlegen der automatischen Beschleunigung für eine Anwendung 
In diesem Beispiel erstellen Sie eine Richtlinie, die Benutzer beim Anmelden bei einer Anwendung automatisch beschleunigt zu einem AD FS-Anmeldebildschirm weiterleitet. Benutzer können sich bei AD FS anmelden, ohne zuvor einen Benutzernamen auf der Anmeldeseite von Azure AD eingeben zu müssen. 

#### <a name="step-1-create-an-hrd-policy"></a>Schritt 1: Erstellen einer Richtlinie zur Startbereichsermittlung
``` powershell
New-AzureADPoly -Definition @("{`"HomeRealmDiscoveryPolicy`":{`"AccelerateToFederatedDomain`":true}}") -DisplayName BasicAutoAccelerationPolicy -Type HomeRealmDiscoveryPolicy
```

Wenn Sie über eine einzige Verbunddomäne verfügen, die Benutzer für Anwendungen authentifiziert, müssen Sie nur eine Richtlinie zur Startbereichsermittlung erstellen.  

Um Ihre neue Richtlinie und deren **ObjectId** abzurufen, führen Sie den folgenden Befehl aus:

``` powershell
Get-AzureADPolicy
```


Um die automatische Beschleunigung zu aktivieren, nachdem Sie über eine Richtlinie zur Startbereichsermittlung verfügen, können Sie diese mehreren Anwendungsdienstprinzipalen zuweisen.

#### <a name="step-2-locate-the-service-principal-to-which-to-assign-the-policy"></a>Schritt 2: Ermitteln des Dienstprinzipals, dem die Richtlinie zugewiesen werden soll  
Sie benötigen die **ObjectID** der Dienstprinzipale, denen Sie die Richtlinie zuweisen möchten. Es gibt mehrere Möglichkeiten, die **ObjectID** von Dienstprinzipalen zu ermitteln.    

Sie können das Portal verwenden, oder Sie können [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) abfragen. Sie können auch zum [Graph-Tester](https://graphexplorer.cloudapp.net/) navigieren und sich bei Ihrem Azure AD-Konto anmelden, um alle Dienstprinzipale Ihrer Organisation anzuzeigen. Da Sie PowerShell verwenden, können Sie das Cmdlet „get-AzureADServicePrincipal“, verwenden, um die Dienstprinzipale sowie deren IDs aufzulisten.

#### <a name="step-3-assign-the-policy-to-your-service-principal"></a>Schritt 3: Zuweisen der Richtlinie zu Ihrem Dienstprinzipal  
Nachdem Sie über die **ObjectID** des Dienstprinzipals der Anwendung verfügen, für die Sie automatische Beschleunigung konfigurieren möchten, führen Sie den folgenden Befehl aus. Dieser Befehl ordnet die Richtlinie zur Startbereichsermittlung, die Sie in Schritt 1 erstellt haben, dem Dienstprinzipal zu, den Sie in Schritt 2 ermittelt haben.

``` powershell
Add-AzureADServicePrincipalPolicy -Id <ObjectID of the Service Principal> -RefObjectId <ObjectId of the Policy>
```

Sie können diesen Befehl für jeden Dienstprinzipal wiederholen, dem Sie die Richtlinie hinzufügen möchten.

#### <a name="step-4-check-which-application-service-principals-your-auto-acceleration-policy-is-assigned-to"></a>Schritt 4: Überprüfen, welchen Anwendungsdienstprinzipalen Ihre Richtlinie zur automatischen Beschleunigung zugewiesen ist
Um zu überprüfen, welche Anwendungen über eine konfigurierte Richtlinie zur automatischen Beschleunigung verfügen, verwenden Sie das Cmdlet **Get-AzureADPolicyAppliedObject**. Übergeben Sie ihm die **ObjectID** der Richtlinie, die Sie überprüfen möchten.

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```
#### <a name="step-5-youre-done"></a>Schritt 5: Sie haben es geschafft!
Testen Sie die Anwendung, um zu überprüfen, ob die neue Richtlinie funktioniert.

### <a name="example-list-the-applications-for-which-an-auto-acceleration-policy-is-configured"></a>Beispiel: Auflisten der Anwendungen, für die eine Richtlinie zur automatischen Beschleunigung konfiguriert ist

#### <a name="step-1-list-all-policies-that-were-created-in-your-organization"></a>Schritt 1: Auflisten aller Richtlinien, die in Ihrer Organisation erstellt wurden 

``` powershell
Get-AzureADPolicy
```

Notieren Sie sich die **ObjectID** der Richtlinie, für die Sie Zuordnungen auflisten möchten.

#### <a name="step-2-list-the-service-principals-to-which-the-policy-is-assigned"></a>Schritt 2: Auflisten der Dienstprinzipale, denen die Richtlinie zugewiesen ist  

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```

### <a name="example-remove-an-auto-acceleration-policy-for-an-application"></a>Beispiel: Entfernen einer Richtlinie zur automatischen Beschleunigung für eine Anwendung
#### <a name="step-1-get-the-objectid"></a>Schritt 1: Abrufen der ObjectID
Verwenden Sie das vorherige Beispiel, um die **ObjectID** der Richtlinie und die ObjectID des Anwendungsdienstprinzipals abzurufen, von dem Sie sie entfernen möchten. 

#### <a name="step-2-remove-the-policy-assignment-from-the-application-service-principal"></a>Schritt 2: Entfernen der Richtlinienzuordnung vom Anwendungsdienstprinzipal  

``` powershell
Remove-AzureADApplicationPolicy -ObjectId <ObjectId of the Service Principal>  -PolicyId <ObjectId of the policy>
```

#### <a name="step-3-check-removal-by-listing-the-service-principals-to-which-the-policy-is-assigned"></a>Schritt 3: Überprüfen der Entfernung durch Auflisten der Dienstprinzipale, denen die Richtlinie zugewiesen ist 

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```
## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zur Funktionsweise der Authentifizierung in Azure AD finden Sie unter [Authentifizierungsszenarien für Azure AD](develop/active-directory-authentication-scenarios.md).
- Weitere Informationen zum einmaligen Anmelden von Benutzern finden Sie unter [Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory](active-directory-enterprise-apps-manage-sso.md).
- Im [Active Directory-Entwicklerhandbuch](develop/active-directory-developers-guide.md) finden Sie eine Übersicht über alle für Entwickler relevanten Inhalte.
