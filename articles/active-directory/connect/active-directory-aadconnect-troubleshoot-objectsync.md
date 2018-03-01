---
title: 'Azure AD Connect: Beheben von Fehlern bei der Objektsynchronisierung | Microsoft-Dokumentation'
description: "Dieses Thema enthält Schritte zum Beheben von Problemen bei der Objektsynchronisierung mithilfe der Aufgaben zur Problembehandlung."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: curtand
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2018
ms.author: billmath
ms.openlocfilehash: 10ebe039b9d266d15696b397b9cdef9bc7ec2a10
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="troubleshoot-object-synchronization-with-azure-ad-connect-sync"></a>Beheben von Problemen bei der Objektsynchronisierung mit der Azure AD Connect-Synchronisierung
Dieses Dokument enthält Schritte zum Beheben von Problemen bei der Objektsynchronisierung mithilfe der Aufgaben zur Problembehandlung.

## <a name="troubleshooting-task"></a>Aufgaben zur Problembehandlung
Verwenden Sie für die Bereitstellung von Azure Active Directory (AAD) Connect mit Version <verison> oder höher die Problembehandlungsaufgabe im Assistenten, um Probleme bei der Objektsynchronisierung zu beheben. Für frühere Versionen führen Sie die Problembehandlung manuell wie [hier](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md) beschrieben aus.

### <a name="run-the-troubleshooting-task-in-the-wizard"></a>Ausführen der Problembehandlungsaufgabe im Assistenten
Um die Aufgabe zur Problembehandlung im Assistenten auszuführen, führen Sie die folgenden Schritte aus:

1.  Öffnen Sie mit der Option „Als Administrator ausführen“ eine neue Windows PowerShell-Sitzung auf Ihrem Azure AD Connect-Server.
2.  Führen Sie `Set-ExecutionPolicy RemoteSigned` oder `Set-ExecutionPolicy Unrestricted` aus.
3.  Starten Sie den Azure AD Connect-Assistenten.
4.  Navigieren Sie zur Seite „Weitere Aufgaben“, wählen Sie „Problembehandlung“ aus, und klicken Sie auf „Weiter“.
5.  Klicken Sie auf der Seite „Problembehandlung“ auf „Starten“, um das Menü zur Problembehandlung in PowerShell zu starten.
6.  Wählen Sie im Hauptmenü „Problembehandlung bei der Objektsynchronisierung“ aus.

### <a name="troubleshooting-input-parameters"></a>Eingabeparameter für die Problembehandlung
Die folgenden Eingabeparameter sind für die Problembehandlungsaufgabe erforderlich:
1.  **Distinguished Name des Objekts:** Dies ist der Distinguished Name des Objekts, für das die Problembehandlung durchgeführt werden soll.
2.  **AD-Connectorname:** Dies ist der Name der AD-Gesamtstruktur, in der sich das oben genannte Objekt befindet.
3.  Geben Sie die Anmeldeinformationen Ihres Azure AD-Mandanten als globaler Administrator ein: ![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch1.png).

### <a name="understand-the-results-of-the-troubleshooting-task"></a>Grundlegendes zu den Ergebnissen der Problembehandlungsaufgabe
Die Problembehandlungsaufgabe führt die folgenden Überprüfungen durch:

1.  Ermitteln von UPN-Konflikten, wenn das Objekt mit Azure Active Directory synchronisiert wird
2.  Überprüfen, ob das Objekt aufgrund einer Domänenfilterung gefiltert wird
3.  Überprüfen, ob das Objekt aufgrund einer Organisationseinheitenfilterung gefiltert wird

Im restlichen Teil dieses Abschnitts werden bestimmte Ergebnisse beschrieben, die von der Aufgabe zurückgegeben werden. In jedem Fall stellt die Aufgabe eine Analyse gefolgt von den empfohlenen Aktionen zum Beheben des Problems bereit.

## <a name="detect-upn-mismatch-if-object-is-synced-to-azure-active-directory"></a>Ermitteln von UPN-Konflikten, wenn das Objekt mit Azure Active Directory synchronisiert wird
### <a name="upn-suffix-is-not-verified-with-azure-ad-tenant"></a>Das UPN-Suffix wird NICHT mit dem Azure AD-Mandanten überprüft.
Wenn das UPN- (UserPrincipalName) bzw. das alternative Anmelde-ID-Suffix nicht mit dem Azure AD-Mandanten überprüft wird, ersetzt Azure Active Directory die UPN-Suffixe durch den Standarddomänennamen „onmicrosoft.com“.

![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch2.png)

### <a name="changing-upn-suffix-from-one-federated-domain-to-another-federated-domain"></a>Ändern des UPN-Suffix von einer Verbunddomäne in eine andere Verbunddomäne
Azure Active Directory erlaubt nicht die Synchronisierung des UPN- (UserPrincipalName) bzw. des alternativen Anmelde-ID-Suffix von einer Verbunddomäne in eine andere Verbunddomäne. Dies gilt für Domänen, die mit dem Azure AD-Mandanten überprüft werden und den Authentifizierungstyp „Verbund“ aufweisen.

![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch3.png) 

### <a name="azure-ad-tenant-dirsync-feature-synchronizeupnformanagedusers-is-disabled"></a>Das DirSync-Feature „SynchronizeUpnForManagedUsers“ ist für Azure AD-Mandanten deaktiviert.
Wenn das DirSync-Feature „SynchronizeUpnForManagedUsers“ für Azure AD-Mandanten deaktiviert ist, erlaubt Azure Active Directory keine Synchronisierungsupdates für den Benutzerprinzipalnamen bzw. die alternative Anmelde-ID für lizenzierte Benutzerkonten mit verwalteter Authentifizierung.

![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch4.png)

## <a name="object-is-filtered-due-to-domain-filtering"></a>Das Objekt wird aufgrund einer Domänenfilterung gefiltert
### <a name="domain-is-not-configured-to-sync"></a>Die Domäne ist nicht für die Synchronisierung konfiguriert
Das Objekt liegt außerhalb des gültigen Bereichs, da die Domäne nicht konfiguriert wurde. Im folgenden Beispiel befindet sich das Objekt nicht im gültigen Synchronisierungsbereich, da die Domäne, zu der es gehört, aus der Synchronisierung gefiltert wird.

![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch5.png)

### <a name="domain-is-configured-to-sync-but-is-missing-run-profilesrun-steps"></a>Die Domäne ist für die Synchronisierung konfiguriert, aber es fehlen die Ausführungsprofile/Ausführungsschritte
Das Objekt befindet sich außerhalb des gültigen Bereichs, da die Domäne über keine Ausführungsprofile/Ausführungsschritte verfügt. Im folgenden Beispiel befindet sich das Objekt nicht im gültigen Synchronisierungsbereich, da die Domäne, zu der es gehört, über keine Ausführungsschritte für das Ausführungsprofil für den vollständigen Import verfügt.
![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch6.png)

### <a name="object-is-filtered-due-to-ou-filtering"></a>Das Objekt wird aufgrund einer Organisationseinheitenfilterung gefiltert
Das Objekt befindet sich aufgrund einer Filterkonfiguration nach Organisationseinheit nicht im gültigen Synchronisierungsbereich. Im folgenden Beispiel gehört das Objekt zur Organisationseinheit „OU=NoSync,DC=bvtadwbackdc,DC=com“.  Diese Organisationseinheit ist nicht im gültigen Synchronisierungsbereich enthalten.
![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch7.png)

## <a name="html-report"></a>HTML-Bericht
Zusätzlich zur Analyse des Objekts generiert die Problembehandlungsaufgabe auch einen HTML-Bericht, der alle bekannten Informationen über das Objekt enthält. Dieser HTML-Bericht kann bei Bedarf zur weiteren Problembehandlung für das Supportteam freigegeben werden.

![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch8.png)

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).