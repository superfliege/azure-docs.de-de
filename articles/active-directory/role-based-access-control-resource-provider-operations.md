---
title: Vorgänge für Azure Resource Manager-Anbieter | Microsoft-Dokumentation
description: Hier finden Sie ausführliche Informationen zu den verfügbaren Vorgängen für Microsoft Azure Resource Manager-Ressourcenanbieter.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/06/2018
ms.author: rolyon
ms.openlocfilehash: 0b8c8823c6d21df96dcfd926db1855169f1570e4
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="azure-resource-manager-resource-provider-operations"></a>Vorgänge für Azure Resource Manager-Ressourcenanbieter

In diesem Dokument werden die Vorgänge aufgeführt, die jedem Microsoft Azure Resource Manager-Ressourcenanbieter zur Verfügung stehen. Sie können in benutzerdefinierten Rollen verwendet werden, um präzise RBAC-Berechtigungen (Role-Based Access Control; rollenbasierte Zugriffssteuerung) für Ressourcen in Azure bereitzustellen. Hinweis: Hierbei handelt es sich nicht um eine umfassende Liste, und bei Anbieterupdates werden unter Umständen Vorgänge hinzugefügt oder entfernt. Vorgangszeichenfolgen weisen das Format `Microsoft.<ProviderName>/<ChildResourceType>/<action>` auf. 

> [!NOTE]
> Verwenden Sie zum Anzeigen einer umfassenden und aktuellen Liste entweder `Get-AzureRmProviderOperation` (PowerShell) oder `az provider operation list` (Azure-Befehlszeilenschnittstelle v2), um Vorgänge von Azure-Ressourcenanbietern aufzulisten.

## <a name="microsoftaad"></a>Microsoft.AAD

| Vorgang | BESCHREIBUNG |
|---|---|
|/domainServices/delete|Löscht Domänendienste.|
|/domainServices/read|Liest Domänendienste.|
|/domainServices/write|Schreibt Domänendienste.|
|/locations/operationresults/read|Liest den Status eines asynchronen Vorgangs.|
|/Operations/read|Die lokalisierte Beschreibung für den Vorgang in der für den Benutzer bestimmten Darstellung.|

## <a name="microsoftaadiam"></a>microsoft.aadiam

| Vorgang | BESCHREIBUNG |
|---|---|
|/diagnosticsettings/delete|Löscht eine Diagnoseeinstellung.|
|/diagnosticsettings/read|Liest eine Diagnoseeinstellung.|
|/diagnosticsettings/write|Schreibt eine Diagnoseeinstellung.|
|/diagnosticsettingscategories/read|Liest die Kategorien einer Diagnoseeinstellung.|
|/tenants/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/tenants/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/tenants/providers/Microsoft.Insights/logDefinitions/read|Ruft die verfügbaren Protokolle für Mandanten ab.|

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

| Vorgang | BESCHREIBUNG |
|---|---|
|/configuration/action|Aktualisiert die Mandantenkonfiguration.|
|/configuration/read|Liest die Mandantenkonfiguration.|
|/configuration/write|Erstellt eine Mandantenkonfiguration.|
|/services/action|Aktualisiert eine Dienstinstanz im Mandanten.|
|/services/alerts/read|Liest die Warnungen für einen Dienst.|
|/services/alerts/read|Liest die Warnungen für einen Dienst.|
|/services/delete|Löscht eine Dienstinstanz im Mandanten.|
|/services/read|Liest die Dienstinstanz im Mandanten.|
|/services/servicemembers/action|Erstellt eine Dienstmemberinstanz im Dienst.|
|/services/servicemembers/alerts/read|Liest die Warnungen für einen Dienstmember.|
|/services/servicemembers/delete|Löscht eine Dienstmemberinstanz im Dienst.|
|/services/servicemembers/read|Liest die Dienstmemberinstanz im Dienst.|
|/services/write|Erstellt eine Dienstinstanz im Mandanten.|

## <a name="microsoftadvisor"></a>Microsoft.Advisor

| Vorgang | BESCHREIBUNG |
|---|---|
|/configurations/read|Konfigurationen abrufen|
|/configurations/write|Erstellt oder aktualisiert Konfigurationen.|
|/generateRecommendations/action|Generiert Empfehlungen.|
|/generateRecommendations/read|Ruft den Status der Empfehlungsgenerierung ab.|
|/operations/read|Ruft die Vorgänge für den Microsoft Advisor ab.|
|/recommendations/read|Liest Empfehlungen.|
|/recommendations/suppressions/delete|Löscht Unterdrückungen.|
|/recommendations/suppressions/read|Ruft Unterdrückungen ab.|
|/recommendations/suppressions/write|Erstellt/Aktualisiert Unterdrückungen.|
|/register/action|Registriert das Abonnement für den Microsoft Advisor.|
|/suppressions/delete|Löscht Unterdrückungen.|
|/suppressions/read|Ruft Unterdrückungen ab.|
|/suppressions/write|Erstellt/Aktualisiert Unterdrückungen.|
|/unregister/action|Hebt die Registrierung für das Abonnement für den Microsoft Advisor auf.|

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

| Vorgang | BESCHREIBUNG |
|---|---|
|/alerts/read|Ruft alle Benachrichtigungen für die Eingabefilter ab.|
|/alerts/resolve/action|Ändert den Status der Benachrichtigung in „Auflösen“.|
|/alertsSummary/read|Ruft die Zusammenfassung der Benachrichtigungen ab.|
|/Operations/read|Liest die angegebenen Vorgänge.|

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

| Vorgang | BESCHREIBUNG |
|---|---|
|/locations/checkNameAvailability/action|Prüft, ob der angegebene Analysis-Servername gültig ist oder bereits verwendet wird.|
|/locations/operationresults/read|Ruft die Informationen des angegebenen Vorgangsergebnisses ab.|
|/locations/operationstatuses/read|Ruft die Informationen des angegebenen Vorgangsstatus ab.|
|/operations/read|Ruft die Informationen von Vorgängen ab.|
|/register/action|Registriert den Analysis Services-Ressourcenanbieter.|
|/servers/delete|Löscht den Analysis-Server.|
|/servers/listGatewayStatus/action|Listet den Status des Gateways auf, der dem Server zugeordnet ist.|
|/servers/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für den Analysis-Server ab.|
|/servers/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für den Analysis-Server.|
|/servers/providers/Microsoft.Insights/logDefinitions/read|Ruft die verfügbaren Protokolle für Server ab.|
|/servers/providers/Microsoft.Insights/metricDefinitions/read|Ruft die verfügbaren Metriken für den Analysis-Server ab.|
|/servers/read|Ruft die Informationen des angegebenen Analysis-Servers ab.|
|/servers/resume/action|Setzt den Analysis-Server fort.|
|/servers/skus/read|Ruft die verfügbaren SKU-Informationen für den Server ab.|
|/servers/suspend/action|Hält den Analysis-Server an.|
|/servers/write|Erstellt oder aktualisiert den angegebenen Analysis-Server.|
|/skus/read|Ruft die Informationen der SKUs ab.|

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

| Vorgang | BESCHREIBUNG |
|---|---|
|/checkNameAvailability/read|Prüft, ob der angegebene Dienstname verfügbar ist.|
|/operations/read|Liest alle API-Vorgänge, die für die Microsoft.ApiManagement-Ressource verfügbar sind.|
|/register/action|Dient zum Registrieren des Microsoft.ApiManagement-Ressourcenanbieters.|
|/reports/read|Ruft Berichte ab, die nach Zeiträumen, geografischer Region, Entwicklern, Produkten, APIs, Vorgängen, Abonnement und byRequest aggregiert sind.|
|/service/apis/delete|Dient zum Entfernen vorhandener APIs.|
|/service/apis/diagnostics/delete|Entfernt eine vorhandene Diagnose.|
|/service/apis/diagnostics/loggers/delete|Entfernt die Zuordnung zwischen einer Protokollierung und einer Diagnoseeinstellung.|
|/service/apis/diagnostics/loggers/read|Ruft die Liste der vorhandenen Diagnoseprotokollierungen ab.|
|/service/apis/diagnostics/loggers/write|Ordnet eine Protokollierung einer Diagnoseeinstellung zu.|
|/service/apis/diagnostics/read|Ruft die Liste der Diagnosen oder Details einer Diagnose ab.|
|/service/apis/diagnostics/write|Fügt eine neue Diagnose hinzu oder aktualisiert die Details vorhandener Diagnosen.|
|/service/apis/operations/delete|Dient zum Entfernen vorhandener API-Vorgänge.|
|/service/apis/operations/policies/delete|Entfernt die Richtlinienkonfiguration von Richtlinien für API-Vorgänge.|
|/service/apis/operations/policies/read|Ruft Richtlinien für API-Vorgänge oder Richtlinienkonfigurationsdetails für API-Vorgänge ab.|
|/service/apis/operations/policies/write|Dient zum Festlegen von Richtlinienkonfigurationsdetails für API-Vorgänge.|
|/service/apis/operations/policy/delete|Dient zum Entfernen der Richtlinienkonfiguration von Vorgängen.|
|/service/apis/operations/policy/read|Dient zum Abrufen von Richtlinienkonfigurationsdetails für Vorgänge.|
|/service/apis/operations/policy/write|Dient zum Festlegen von Richtlinienkonfigurationsdetails für Vorgänge.|
|/service/apis/operations/read|Dient zum Abrufen einer Liste mit vorhandenen API-Vorgängen oder zum Abrufen von Details zu API-Vorgängen.|
|/service/apis/operations/tags/delete|Löscht die Zuordnung zwischen einer vorhandenen Markierung und einem vorhandenen Vorgang.|
|/service/apis/operations/tags/read|Ruft die dem Vorgang zugeordneten Markierungen oder Markierungsdetails ab.|
|/service/apis/operations/tags/write|Ordnet eine vorhandene Markierung einem vorhandenen Vorgang zu.|
|/service/apis/operations/write|Dient zum Erstellen eines neuen API-Vorgangs oder zum Aktualisieren vorhandener API-Vorgänge.|
|/service/apis/operationsByTags/read|Ruft die Liste der Zuordnungen zwischen Vorgängen und Markierungen ab.|
|/service/apis/policies/delete|Dient zum Entfernen der Richtlinienkonfiguration aus API-Richtlinien.|
|/service/apis/policies/read|Ruft Richtlinien für APIs oder Richtlinienkonfigurationsdetails für APIs ab.|
|/service/apis/policies/write|Dient zum Festlegen von Richtlinienkonfigurationsdetails für APIs.|
|/service/apis/policy/delete|Dient zum Entfernen der Richtlinienkonfiguration von APIs.|
|/service/apis/policy/read|Dient zum Abrufen von Richtlinienkonfigurationsdetails für APIs.|
|/service/apis/policy/write|Dient zum Festlegen von Richtlinienkonfigurationsdetails für APIs.|
|/service/apis/products/read|Ruft alle Produkte ab, denen die API zugehörig ist.|
|/service/apis/read|Dient zum Abrufen einer Liste mit allen registrierten APIs oder zum Abrufen von API-Details.|
|/service/apis/releases/delete|Entfernt ein Release oder alle Releases der API.|
|/service/apis/releases/read|Ruft Releases für eine API oder Details von einem API-Release ab.|
|/service/apis/releases/write|Dient zum Erstellen eines neuen API-Release oder zum Aktualisieren eines bereits vorhandenen API-Release.|
|/service/apis/revisions/delete|Entfernt alle Revisionen einer API.|
|/service/apis/revisions/read|Ruft Revisionen ab, die einer API zugehörig sind.|
|/service/apis/schemas/delete|Entfernt ein vorhandenes Schema.|
|/service/apis/schemas/document/read|Ruft das Dokument ab, das das Schema beschreibt.|
|/service/apis/schemas/document/write|Aktualisiert das Dokument, das das Schema beschreibt.|
|/service/apis/schemas/read|Ruft alle Schemas für eine bestimmte API oder die von der API verwendeten Schemas ab.|
|/service/apis/schemas/write|Legt die von der API verwendeten Schemas fest.|
|/service/apis/tagDescriptions/delete|Entfernt die Markierungsbeschreibung aus der API.|
|/service/apis/tagDescriptions/read|Ruft eine oder mehrere Markierungsbeschreibungen im Bereich der API ab.|
|/service/apis/tagDescriptions/write|Erstellt oder ändert eine Markierungsbeschreibung im Bereich der API.|
|/service/apis/tags/delete|Entfernt vorhandene Zuordnungen zwischen einer API und einer Markierung.|
|/service/apis/tags/read|Ruft alle Zuordnungen zwischen einer API und einer Markierung für die API oder die zugehörigen Details ab.|
|/service/apis/tags/write|Fügt eine neue Zuordnung zwischen einer API und einer Markierung hinzu.|
|/service/apis/write|Dient zum Erstellen einer neuen API oder zum Aktualisieren bereits vorhandener API-Details.|
|/service/apisByTags/read|Ruft die Liste der Zuordnungen zwischen APIs und Markierungen ab.|
|/service/api-version-sets/delete|Dient zum Entfernen vorhandener VersionSet-Eigenschaften.|
|/service/api-version-sets/read|Ruft die Liste der Versionsgruppenentitäten oder Details zu einer VersionSet-Eigenschaft ab.|
|/service/api-version-sets/versions/read|Ruft die Liste der Versionsentitäten ab.|
|/service/api-version-sets/write|Erstellt neue VersionSet-Details oder aktualisiert vorhandene VersionSet-Details.|
|/service/applynetworkconfigurationupdates/action|Aktualisiert die in Virtual Network ausgeführten Microsoft.ApiManagement-Ressourcen, um aktualisierte Netzwerkeinstellungen zu verwenden.|
|/service/authorizationServers/delete|Dient zum Entfernen vorhandener Autorisierungsserver.|
|/service/authorizationServers/read|Dient zum Abrufen einer Liste mit Autorisierungsservern oder zum Abrufen von Autorisierungsserverdetails.|
|/service/authorizationServers/write|Dient zum Erstellen eines neuen Autorisierungsservers oder zum Aktualisieren der Details eines bereits vorhandenen Autorisierungsservers.|
|/service/backends/delete|Dient zum Entfernen vorhandener Back-Ends.|
|/service/backends/read|Dient zum Abrufen einer Liste mit Back-Ends oder zum Abrufen von Back-End-Details.|
|/service/backends/reconnect/action|Erstellt eine Anforderung zum Herstellen einer erneuten Verbindung.|
|/service/backends/write|Dient zum Hinzufügen eines neuen Back-Ends oder zum Aktualisieren bereits vorhandener Back-End-Details.|
|/service/backup/action|Dient zum Sichern des API Management-Diensts im angegebenen Container in einem vom Benutzer bereitgestellten Speicherkonto.|
|/service/certificates/delete|Dient zum Entfernen vorhandener Zertifikate.|
|/service/certificates/read|Dient zum Abrufen einer Liste mit Zertifikaten oder zum Abrufen von Zertifikatdetails.|
|/service/certificates/write|Dient zum Hinzufügen eines neuen Zertifikats.|
|/service/delete|Dient zum Löschen einer API Management-Dienstinstanz.|
|/service/diagnostics/delete|Entfernt eine vorhandene Diagnose.|
|/service/diagnostics/loggers/delete|Entfernt die Zuordnung zwischen einer Protokollierung und einer Diagnoseeinstellung.|
|/service/diagnostics/loggers/read|Ruft die Liste der vorhandenen Diagnoseprotokollierungen ab.|
|/service/diagnostics/loggers/write|Ordnet eine Protokollierung einer Diagnoseeinstellung zu.|
|/service/diagnostics/read|Ruft die Liste der Diagnosen oder Details einer Diagnose ab.|
|/service/diagnostics/write|Fügt eine neue Diagnose hinzu oder aktualisiert die Details vorhandener Diagnosen.|
|/service/getssotoken/action|Ruft ein SSO-Token ab, das verwendet werden kann, um sich als Administrator beim Legacyportal des API Management-Diensts anzumelden.|
|/service/groups/delete|Dient zum Entfernen vorhandener Gruppen.|
|/service/groups/read|Dient zum Abrufen einer Liste mit Gruppen oder zum Abrufen von Details einer Gruppe.|
|/service/groups/users/delete|Dient zum Entfernen von vorhandenen Benutzern aus vorhandenen Gruppen.|
|/service/groups/users/read|Dient zum Abrufen einer Liste mit Gruppenbenutzern.|
|/service/groups/users/write|Dient zum Hinzufügen von vorhandenen Benutzern zu vorhandenen Gruppen.|
|/service/groups/write|Dient zum Erstellen einer neuen Gruppe oder zum Aktualisieren vorhandener Gruppendetails.|
|/service/identityProviders/delete|Dient zum Entfernen bereits vorhandener Identitätsanbieter.|
|/service/identityProviders/read|Dient zum Abrufen der Identitätsanbieterliste oder zum Abrufen von Details zum Identitätsanbieter.|
|/service/identityProviders/write|Dient zum Erstellen eines neuen Identitätsanbieters oder zum Aktualisieren der Details eines bereits vorhandenen Identitätsanbieters.|
|/service/locations/networkstatus/read|Ruft den Netzwerkzugriffsstatus von Ressourcen ab, von denen der Dienst am jeweiligen Standort abhängt.|
|/service/loggers/delete|Dient zum Entfernen vorhandener Protokollierungen.|
|/service/loggers/read|Dient zum Abrufen einer Liste mit Protokollierungen oder zum Abrufen von Protokollierungsdetails.|
|/service/loggers/write|Dient zum Hinzufügen einer neuen Protokollierung oder zum Aktualisieren der Details einer bereits vorhandenen Protokollierung.|
|/service/managedeployments/action|Dient zum Ändern der SKU/Einheiten sowie zum Hinzufügen/Entfernen regionaler Bereitstellungen des API Management-Diensts.|
|/service/networkstatus/read|Ruft den Netzwerkzugriffsstatus von Ressourcen ab, von denen der Dienst abhängt.|
|/service/notifications/action|Sendet Benachrichtigungen an einen angegebenen Benutzer.|
|/service/notifications/read|Ruft alle API Management-Benachrichtigungen für Herausgeber oder die entsprechenden Details ab.|
|/service/notifications/recipientEmails/delete|Entfernt eine vorhandenen E-Mail-Adresse, die einer Benachrichtigung zugeordnet ist.|
|/service/notifications/recipientEmails/read|Ruft die E-Mail-Empfänger ab, die einer API Management-Benachrichtigung für Herausgeber zugeordnet sind.|
|/service/notifications/recipientEmails/write|Erstellt einen neuen E-Mail-Empfänger der Benachrichtigung.|
|/service/notifications/recipientUsers/delete|Entfernt Benutzer, die den Empfängern der Benachrichtigung zugeordnet sind.|
|/service/notifications/recipientUsers/read|Ruft die Empfängerbenutzer ab, die der Benachrichtigung zugeordnet sind.|
|/service/notifications/recipientUsers/write|Fügt einen Benutzer zu den Benachrichtigungsempfängern hinzu.|
|/service/notifications/write|Erstellt oder aktualisiert eine API Management-Benachrichtigung für Herausgeber.|
|/service/openidConnectProviders/delete|Dient zum Entfernen vorhandener OpenID Connect-Anbieter.|
|/service/openidConnectProviders/read|Dient zum Abrufen einer Liste mit OpenID Connect-Anbietern oder zum Abrufen von Details zu OpenID Connect-Anbietern.|
|/service/openidConnectProviders/write|Dient zum Erstellen eines neuen OpenID Connect-Anbieters oder zum Aktualisieren der Details eines vorhandenen OpenID Connect-Anbieters.|
|/service/operationresults/read|Ruft den aktuellen Status eines Vorgangs mit langer Ausführungsdauer ab.|
|/service/policies/delete|Dient zum Entfernen der Richtlinienkonfiguration von Mandantenrichtlinien.|
|/service/policies/read|Ruft Richtlinien für Mandanten oder die Richtlinienkonfigurationsdetails für Mandanten ab.|
|/service/policies/write|Dient zum Festlegen von Richtlinienkonfigurationsdetails für Mandanten.|
|/service/policySnippets/read|Ruft alle Richtlinienausschnitte ab.|
|/service/portalsettings/read|Ruft die Registrierungs-, Anmelde- oder Delegierungseinstellungen für das Portal ab.|
|/service/portalsettings/write|Aktualisiert Anmelde-, Registrierungs- oder Delegierungseinstellungen.|
|/service/products/apis/delete|Dient dazu, vorhandene APIs von vorhandenen Produkten zu entfernen.|
|/service/products/apis/read|Dient zum Abrufen einer Liste mit APIs, die einem vorhandenen Produkt hinzugefügt wurden.|
|/service/products/apis/write|Dient zum Hinzufügen von vorhandenen APIs zu vorhandenen Produkten.|
|/service/products/delete|Dient zum Entfernen vorhandener Produkt.|
|/service/products/groups/delete|Dient zum Löschen der Zuordnung von vorhandenen Entwicklergruppen zu vorhandenen Produkten.|
|/service/products/groups/read|Dient zum Abrufen einer Liste mit Entwicklergruppen, die einem Produkt zugeordnet sind.|
|/service/products/groups/write|Dient zum Zuordnen von vorhandenen Entwicklergruppen zu vorhandenen Produkten.|
|/service/products/policies/delete|Dient zum Entfernen der Richtlinienkonfiguration von Produktrichtlinien.|
|/service/products/policies/read|Ruft Richtlinien für Produkte oder Richtlinienkonfigurationsdetails für Produkte ab.|
|/service/products/policies/write|Dient zum Festlegen von Richtlinienkonfigurationsdetails für Produkte.|
|/service/products/policy/delete|Dient zum Entfernen der Richtlinienkonfiguration von vorhandenen Produkten.|
|/service/products/policy/read|Dient zum Abrufen der Richtlinienkonfiguration vorhandener Produkte.|
|/service/products/policy/write|Dient zum Festlegen der Richtlinienkonfiguration für vorhandene Produkte.|
|/service/products/read|Dient zum Abrufen einer Liste mit Produkten oder zum Abrufen von Produktdetails.|
|/service/products/subscriptions/read|Dient zum Abrufen einer Liste mit Produktabonnements.|
|/service/products/tags/delete|Löscht die Zuordnung zwischen einer vorhandenen Markierung und einem vorhandenen Produkt.|
|/service/products/tags/read|Ruft die dem Produkt zugeordneten Markierungen oder Markierungsdetails ab.|
|/service/products/tags/write|Ordnet eine vorhandene Markierung einem vorhandenen Produkt zu.|
|/service/products/write|Dient zum Erstellen eines neuen Produkts oder zum Aktualisieren vorhandener Produktdetails.|
|/service/properties/delete|Entfernt vorhandene Eigenschaften.|
|/service/properties/read|Dient zum Abrufen einer Liste mit allen Eigenschaften oder zum Abrufen von Details zur angegebenen Eigenschaft.|
|/service/properties/write|Erstellt eine neue Eigenschaft oder aktualisiert den Wert für die angegebene Eigenschaft.|
|/service/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für den API Management-Dienst ab.|
|/service/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für den API Management-Dienst.|
|/service/providers/Microsoft.Insights/logDefinitions/read|Ruft die verfügbaren Protokolle für den API Management-Dienst ab.|
|/service/providers/Microsoft.Insights/metricDefinitions/read|Ruft die verfügbaren Metriken für den API Management-Dienst ab.|
|/service/quotas/periods/read|Ruft den Wert des Kontingentzählers für einen bestimmten Zeitraum ab.|
|/service/quotas/periods/write|Legt den aktuellen Wert für den Kontingentzähler fest.|
|/service/quotas/read|Ruft Kontingentwerte ab.|
|/service/quotas/write|Legt den aktuellen Wert für den Kontingentzähler fest.|
|/service/read|Dient zum Lesen der Metadaten für eine API Management-Dienstinstanz.|
|/service/reports/read|Ruft Berichte ab, die nach Zeiträumen, geografischer Region oder Entwicklern aggregiert sind. Dient alternativ zum Abrufen von nach Produkten aggregierten Berichten. Dient alternativ zum Abrufen von Berichten, die nach APIs, Vorgängen oder Abonnement aggregiert sind. Dient alternativ zum Abrufen von Berichtsdaten für Anforderungen.|
|/service/restore/action|Dient zum Wiederherstellen des API Management-Diensts aus dem angegebenen Container in einem vom Benutzer bereitgestellten Speicherkonto.|
|/service/subscriptions/delete|Dient zum Löschen von Abonnements. Dieser Vorgang kann zum Löschen von Abonnements verwendet werden.|
|/service/subscriptions/read|Dient zum Abrufen einer Liste mit Produktabonnements oder zum Abrufen von Produktabonnementdetails.|
|/service/subscriptions/regeneratePrimaryKey/action|Dient zum erneuten Generieren des primären Abonnementschlüssels.|
|/service/subscriptions/regenerateSecondaryKey/action|Dient zum erneuten Generieren des sekundären Abonnementschlüssels.|
|/service/subscriptions/write|Dient dazu, für einen vorhandenen Benutzer ein Abonnement für ein vorhandenes Produkt zu erstellen oder die Details eines vorhandenen Abonnements zu aktualisieren. Dieser Vorgang kann zum Verlängern von Abonnements verwendet werden.|
|/service/tagResources/read|Ruft die Liste der Markierungen ab, die den Ressourcen zugeordnet sind.|
|/service/tags/delete|Dient zum Entfernen vorhandener Markierungen.|
|/service/tags/read|Dient zum Abrufen einer Liste mit Tags oder zum Abrufen von Markierungsdetails.|
|/service/tags/write|Fügt eine neue Markierung hinzu oder aktualisiert die Details vorhandener Markierungen.|
|/service/templates/delete|Setzt die standardmäßige E-Mail-Vorlage des API Management zurück.|
|/service/templates/read|Ruft alle E-Mail-Vorlagen des API Management oder die entsprechenden Details ab.|
|/service/templates/write|Erstellt oder aktualisiert die E-Mail-Vorlage des API Management.|
|/service/tenant/delete|Dient zum Entfernen der Richtlinienkonfiguration für den Mandanten.|
|/service/tenant/deploy/action|Führt eine Bereitstellungsaufgabe aus, um Änderungen aus der angegebenen Git-Verzweigung auf die Konfiguration in der Datenbank anzuwenden.|
|/service/tenant/operationResults/read|Dient zum Abrufen einer Liste mit Vorgangsergebnissen oder zum Abrufen des Ergebnisses eines bestimmten Vorgangs.|
|/service/tenant/read|Ruft die Richtlinienkonfiguration für den Mandanten oder Zugriffsdetails des Mandanten ab.|
|/service/tenant/regeneratePrimaryKey/action|Dient zum erneuten Generieren des primären Zugriffsschlüssels.|
|/service/tenant/regenerateSecondaryKey/action|Dient zum erneuten Generieren des sekundären Zugriffsschlüssels.|
|/service/tenant/save/action|Erstellt Commit mit Konfigurationsmomentaufnahme in der angegebenen Verzweigung im Repository.|
|/service/tenant/syncState/read|Dient zum Abrufen des Status der letzten Git-Synchronisierung.|
|/service/tenant/validate/action|Überprüft Änderungen aus der angegebenen Git-Verzweigung.|
|/service/tenant/write|Legt die Richtlinienkonfiguration für den Mandanten ab oder aktualisiert die Zugriffsdetails des Mandanten.|
|/service/updatecertificate/action|Dient zum Hochladen eines SSL-Zertifikats für einen API Management-Dienst.|
|/service/updatehostname/action|Dient zum Einrichten, Aktualisieren oder Entfernen benutzerdefinierter Domänennamen für einen API Management-Dienst.|
|/service/users/action|Registriert einen neuen Benutzer.|
|/service/users/applications/attachments/delete|Entfernt eine Anlage.|
|/service/users/applications/attachments/read|Ruft eine oder mehrere Anwendungsanlagen ab.|
|/service/users/applications/attachments/write|Fügt eine Anlage zu einer Anwendung hinzu.|
|/service/users/applications/delete|Entfernt eine vorhandene Anwendung.|
|/service/users/applications/read|Ruft die Liste aller Benutzeranwendungen oder die entsprechenden Details der API Management-Anwendungen ab.|
|/service/users/applications/write|Registriert eine Anwendung beim API Management oder aktualisiert die jeweiligen Anwendungsdetails.|
|/service/users/delete|Dient zum Entfernen von Benutzerkonten.|
|/service/users/generateSsoUrl/action|Dient zum Generieren der SSO-URL. Die URL kann für den Zugriff auf das Verwaltungsportal verwendet werden.|
|/service/users/groups/read|Dient zum Abrufen einer Liste mit Benutzergruppen.|
|/service/users/keys/read|Dient zum Abrufen einer Liste mit Benutzerschlüsseln.|
|/service/users/read|Dient zum Abrufen einer Liste mit registrierten Benutzern oder zum Abrufen der Kontodetails eines Benutzers.|
|/service/users/subscriptions/read|Dient zum Abrufen einer Liste mit Benutzerabonnements.|
|/service/users/token/action|Rufen das Zugriffstoken für einen Benutzer ab.|
|/service/users/write|Dient zum Registrieren eines neuen Benutzers oder zum Aktualisieren der Kontodetails eines vorhandenen Benutzers.|
|/service/write|Dient zum Erstellen einer neuen Instanz des API Management-Diensts.|
|/unregister/action|Dient zum Aufheben der Registrierung des Microsoft.ApiManagement-Ressourcenanbieters.|

## <a name="microsoftauthorization"></a>Microsoft.Authorization

| Vorgang | BESCHREIBUNG |
|---|---|
|/checkAccess/action|Überprüft, ob der Aufrufer zum Ausführen einer bestimmten Aktion berechtigt ist.|
|/classicAdministrators/delete|Entfernt den Administrator aus dem Abonnement.|
|/classicAdministrators/read|Liest die Administratoren für das Abonnement.|
|/classicAdministrators/write|Dient zum Hinzufügen oder Ändern von Administratoren für ein Abonnement.|
|/elevateAccess/action|Gewährt dem Aufrufer Zugriff vom Typ „Benutzerzugriffsadministrator“ auf der Mandantenebene.|
|/locks/delete|Dient zum Löschen von Sperren im angegebenen Bereich.|
|/locks/read|Ruft Sperren im angegebenen Bereich ab.|
|/locks/write|Dient zum Hinzufügen von Sperren im angegebenen Bereich.|
|/permissions/read|Listet alle Berechtigungen auf, über die der Aufrufer in einem bestimmten Bereich verfügt.|
|/policyAssignments/delete|Dient zum Löschen einer Richtlinienzuweisung im angegebenen Bereich.|
|/policyAssignments/read|Dient zum Abrufen von Informationen zu einer Richtlinienzuweisung.|
|/policyAssignments/write|Dient zum Erstellen einer Richtlinienzuweisung im angegebenen Bereich.|
|/policyDefinitions/delete|Dient zum Löschen einer Richtliniendefinition.|
|/policyDefinitions/read|Dient zum Abrufen von Informationen zu einer Richtliniendefinition.|
|/policyDefinitions/write|Dient zum Erstellen einer benutzerdefinierten Richtliniendefinition.|
|/policySetDefinitions/delete|Löscht eine Richtliniengruppendefinition.|
|/policySetDefinitions/read|Ruft Informationen zu einer Richtliniengruppendefinition ab.|
|/policySetDefinitions/write|Erstellt eine benutzerdefinierte Richtliniengruppendefinition.|
|/providerOperations/read|Dient zum Abrufen von Vorgängen für alle Ressourcenanbieter, die in Rollendefinitionen verwendet werden können.|
|/roleAssignments/delete|Dient zum Löschen einer Rollenzuweisung im angegebenen Bereich.|
|/roleAssignments/read|Dient zum Abrufen von Informationen zu einer Rollenzuweisung.|
|/roleAssignments/write|Dient zum Erstellen einer Rollenzuweisung im angegebenen Bereich.|
|/roleDefinitions/delete|Dient zum Löschen der angegebenen benutzerdefinierten Rollendefinition.|
|/roleDefinitions/read|Dient zum Abrufen von Informationen zu einer Rollendefinition.|
|/roleDefinitions/write|Dient zum Erstellen oder Aktualisieren einer benutzerdefinierten Rollendefinition mit angegebenen Berechtigungen und zuweisbaren Bereichen.|

## <a name="microsoftautomation"></a>Microsoft.Automation

| Vorgang | BESCHREIBUNG |
|---|---|
|/automationAccounts/agentRegistrationInformation/read|Liest die Registrierungsinformationen einer Azure Automation DSC-Instanz.|
|/automationAccounts/agentRegistrationInformation/regenerateKey/action|Schreibt eine Anforderung zum erneuten Generieren von Azure Automation DSC-Schlüsseln.|
|/automationAccounts/certificates/delete|Löscht ein Azure Automation-Zertifikatasset.|
|/automationAccounts/certificates/read|Ruft ein Azure Automation-Zertifikatasset ab.|
|/automationAccounts/certificates/write|Erstellt oder aktualisiert ein Azure Automation-Zertifikatasset.|
|/automationAccounts/compilationjobs/read|Liest die Kompilierung einer Azure Automation DSC-Instanz.|
|/automationAccounts/compilationjobs/write|Schreibt die Kompilierung einer Azure Automation DSC-Instanz.|
|/automationAccounts/configurations/delete|Löscht den Inhalt einer Azure Automation DSC-Instanz.|
|/automationAccounts/configurations/getCount/action|Liest den Inhalt einer Azure Automation DSC-Instanz.|
|/automationAccounts/configurations/read|Ruft den Inhalt einer Azure Automation DSC-Instanz ab.|
|/automationAccounts/configurations/write|Schreibt den Inhalt einer Azure Automation DSC-Instanz.|
|/automationAccounts/connections/delete|Löscht ein Azure Automation-Verbindungsasset.|
|/automationAccounts/connections/read|Ruft ein Azure Automation-Verbindungsasset ab.|
|/automationAccounts/connections/write|Erstellt oder aktualisiert ein Azure Automation-Verbindungsasset.|
|/automationAccounts/connectionTypes/delete|Löscht ein Azure Automation-Verbindungstypasset.|
|/automationAccounts/connectionTypes/read|Ruft ein Azure Automation-Verbindungstypasset ab.|
|/automationAccounts/connectionTypes/write|Erstellt ein Azure Automation-Verbindungstypasset.|
|/automationAccounts/credentials/delete|Löscht ein Azure Automation-Anmeldeinformationsasset.|
|/automationAccounts/credentials/read|Ruft ein Azure Automation-Anmeldeinformationsasset ab.|
|/automationAccounts/credentials/write|Erstellt oder aktualisiert ein Azure Automation-Anmeldeinformationsasset.|
|/automationAccounts/delete|Löscht ein Azure Automation-Konto.|
|/automationAccounts/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/automationAccounts/diagnosticSettings/write|Legt die Diagnoseeinstellung für die Ressource fest.|
|/automationAccounts/hybridRunbookWorkerGroups/delete|Löscht Hybrid Runbook Worker-Ressourcen.|
|/automationAccounts/hybridRunbookWorkerGroups/read|Liest Hybrid Runbook Worker-Ressourcen.|
|/automationAccounts/jobs/output/action|Ruft die Ausgabe eines Auftrags ab.|
|/automationAccounts/jobs/output/action|Ruft die Ausgabe eines Auftrags ab.|
|/automationAccounts/jobs/read|Ruft einen Azure Automation-Auftrag ab.|
|/automationAccounts/jobs/read|Ruft einen Azure Automation-Auftrag ab.|
|/automationAccounts/jobs/resume/action|Setzt einen Azure Automation-Auftrag fort.|
|/automationAccounts/jobs/resume/action|Setzt einen Azure Automation-Auftrag fort.|
|/automationAccounts/jobs/runbookContent/action|Ruft den Inhalt des Azure Automation-Runbooks zum Zeitpunkt der Auftragsausführung ab.|
|/automationAccounts/jobs/runbookContent/action|Ruft den Inhalt des Azure Automation-Runbooks zum Zeitpunkt der Auftragsausführung ab.|
|/automationAccounts/jobs/stop/action|Beendet einen Azure Automation-Auftrag.|
|/automationAccounts/jobs/stop/action|Beendet einen Azure Automation-Auftrag.|
|/automationAccounts/jobs/streams/read|Ruft einen Azure Automation-Auftragsdatenstrom ab.|
|/automationAccounts/jobs/streams/read|Ruft einen Azure Automation-Auftragsdatenstrom ab.|
|/automationAccounts/jobs/suspend/action|Hält einen Azure Automation-Auftrag an.|
|/automationAccounts/jobs/suspend/action|Hält einen Azure Automation-Auftrag an.|
|/automationAccounts/jobs/write|Erstellt einen Azure Automation-Auftrag.|
|/automationAccounts/jobs/write|Erstellt einen Azure Automation-Auftrag.|
|/automationAccounts/jobSchedules/delete|Löscht einen Azure Automation-Auftragszeitplan.|
|/automationAccounts/jobSchedules/read|Ruft einen Azure Automation-Auftragszeitplan ab.|
|/automationAccounts/jobSchedules/write|Erstellt einen Azure Automation-Auftragszeitplan.|
|/automationAccounts/linkedWorkspace/read|Ruft den Arbeitsbereich ab, der mit dem Automation-Konto verknüpft ist.|
|/automationAccounts/logDefinitions/read|Ruft die verfügbaren Protokolle für das Automation-Konto ab.|
|/automationAccounts/modules/activities/read|Ruft Azure Automation-Aktivitäten ab.|
|/automationAccounts/modules/delete|Löscht ein Azure Automation-Modul.|
|/automationAccounts/modules/read|Ruft ein Azure Automation-Modul ab.|
|/automationAccounts/modules/write|Erstellt oder aktualisiert ein Azure Automation-Modul.|
|/automationAccounts/nodeConfigurations/delete|Löscht die Knotenkonfiguration einer Azure Automation DSC-Instanz.|
|/automationAccounts/nodeConfigurations/read|Liest die Knotenkonfiguration einer Azure Automation DSC-Instanz.|
|/automationAccounts/nodeConfigurations/readContent/action|Liest den Inhalt der Knotenkonfiguration einer Azure Automation DSC-Instanz.|
|/automationAccounts/nodeConfigurations/write|Schreibt die Knotenkonfiguration einer Azure Automation DSC-Instanz.|
|/automationAccounts/nodes/delete|Löscht Azure Automation DSC-Knoten.|
|/automationAccounts/nodes/read|Liest Azure Automation DSC-Knoten.|
|/automationAccounts/nodes/reports/read|Liest Azure Automation DSC-Berichtsinhalte.|
|/automationAccounts/nodes/reports/read|Liest Azure Automation DSC-Berichte.|
|/automationAccounts/objectDataTypes/fields/read|Ruft TypeFields-Elemente für Azure Automation ab.|
|/automationAccounts/providers/Microsoft.Insights/metricDefinitions/read|Ruft Automation-Metrikdefinitionen ab.|
|/automationAccounts/read|Ruft ein Azure Automation-Konto ab.|
|/automationAccounts/runbooks/delete|Löscht ein Azure Automation-Runbook.|
|/automationAccounts/runbooks/draft/publish/action|Veröffentlicht einen Azure Automation-Runbookentwurf.|
|/automationAccounts/runbooks/draft/read|Ruft einen Azure Automation-Runbookentwurf ab.|
|/automationAccounts/runbooks/draft/readContent/action|Ruft den Inhalt eines Azure Automation-Runbookentwurfs ab.|
|/automationAccounts/runbooks/draft/testJob/read|Ruft einen Testauftrag für einen Azure Automation-Runbookentwurf ab.|
|/automationAccounts/runbooks/draft/testJob/resume/action|Setzt einen Testauftrag für einen Azure Automation-Runbookentwurf fort.|
|/automationAccounts/runbooks/draft/testJob/stop/action|Beendet einen Testauftrag für einen Azure Automation-Runbookentwurf.|
|/automationAccounts/runbooks/draft/testJob/suspend/action|Hält einen Testauftrag für einen Azure Automation-Runbookentwurf an.|
|/automationAccounts/runbooks/draft/testJob/write|Erstellt einen Testauftrag für einen Azure Automation-Runbookentwurf.|
|/automationAccounts/runbooks/draft/undoEdit/action|Macht die Bearbeitung eines Azure Automation-Runbookentwurfs rückgängig.|
|/automationAccounts/runbooks/draft/writeContent/action|Erstellt den Inhalt eines Azure Automation-Runbookentwurfs.|
|/automationAccounts/runbooks/read|Ruft ein Azure Automation-Runbook ab.|
|/automationAccounts/runbooks/readContent/action|Ruft den Inhalt eines Azure Automation-Runbooks ab.|
|/automationAccounts/runbooks/write|Erstellt oder aktualisiert ein Azure Automation-Runbook.|
|/automationAccounts/schedules/delete|Löscht ein Azure Automation-Zeitplanasset.|
|/automationAccounts/schedules/read|Ruft ein Azure Automation-Zeitplanasset ab.|
|/automationAccounts/schedules/write|Erstellt oder aktualisiert ein Azure Automation-Zeitplanasset.|
|/automationAccounts/statistics/read|Ruft die Statistik für Azure Automation ab.|
|/automationAccounts/usages/read|Ruft die Azure Automation-Nutzung ab.|
|/automationAccounts/variables/delete|Löscht ein Azure Automation-Variablenasset.|
|/automationAccounts/variables/read|Liest ein Azure Automation-Variablenasset.|
|/automationAccounts/variables/write|Erstellt oder aktualisiert ein Azure Automation-Variablenasset.|
|/automationAccounts/watchers/streams/read|Ruft einen Azure Automation Watcher-Auftragsdatenstrom ab.|
|/automationAccounts/webhooks/delete|Löscht einen Azure Automation-Webhook. |
|/automationAccounts/webhooks/generateUri/action|Generiert einen URI für einen Azure Automation-Webhook.|
|/automationAccounts/webhooks/read|Liest einen Azure Automation-Webhook.|
|/automationAccounts/webhooks/write|Erstellt oder aktualisiert einen Azure Automation-Webhook.|
|/automationAccounts/write|Erstellt oder aktualisiert ein Azure Automation-Konto.|
|/automationAccounts/write|Erstellt oder aktualisiert ein Azure Automation-Konto.|

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

| Vorgang | BESCHREIBUNG |
|---|---|
|/b2cDirectories/delete|Löscht eine B2C-Verzeichnisressource.|
|/b2cDirectories/read|Zeigt eine B2C-Verzeichnisressource an.|
|/b2cDirectories/write|Erstellt oder aktualisiert eine B2C-Verzeichnisressource.|
|/operations/read|Hiermit lesen Sie alle API-Vorgänge, die für den Microsoft.AzureActiveDirectory-Ressourcenanbieter verfügbar sind.|
|/register/action|Hiermit registrieren Sie das Abonnement für den Microsoft.AzureActiveDirectory-Ressourcenanbieter.|

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

| Vorgang | BESCHREIBUNG |
|---|---|
|/Operations/read|Ruft die Eigenschaften für einen Vorgang eines Ressourcenanbieters ab.|
|/register/action|Registriert ein Abonnement beim Microsoft.AzureStack-Ressourcenanbieter.|
|/registrations/customerSubscriptions/delete|Löscht ein Azure Stack-Kundenabonnement.|
|/registrations/customerSubscriptions/read|Ruft die Eigenschaften eines Azure Stack-Kundenabonnements ab.|
|/registrations/customerSubscriptions/write|Erstellt oder aktualisiert ein Azure Stack-Kundenabonnement.|
|/registrations/delete|Löscht eine Azure Stack-Registrierung.|
|/registrations/getActivationKey/action|Ruft den neuesten Azure Stack-Aktivierungsschlüssel ab.|
|/registrations/products/listDetails/action|Ruft erweiterte Details für ein Azure Stack-Marketplace-Produkt ab.|
|/registrations/products/read|Ruft die Eigenschaften eines Azure Stack-Marketplace-Produkts ab.|
|/registrations/read|Ruft die Eigenschaften einer Azure Stack-Registrierung ab.|
|/registrations/write|Erstellt oder aktualisiert eine Azure Stack-Registrierung.|

## <a name="microsoftbatch"></a>Microsoft.Batch

| Vorgang | BESCHREIBUNG |
|---|---|
|/batchAccounts/applications/delete|Löscht eine Anwendung.|
|/batchAccounts/applications/read|Listet Anwendungen auf oder ruft die Eigenschaften einer Anwendung ab.|
|/batchAccounts/applications/versions/activate/action|Aktiviert ein Anwendungspaket.|
|/batchAccounts/applications/versions/delete|Löscht ein Anwendungspaket.|
|/batchAccounts/applications/versions/read|Ruft die Eigenschaften eines Anwendungspakets ab.|
|/batchAccounts/applications/versions/write|Erstellt ein neues Anwendungspaket oder aktualisiert ein vorhandenes Anwendungspaket.|
|/batchAccounts/applications/write|Erstellt eine neue Anwendung oder aktualisiert eine vorhandene Anwendung.|
|/batchAccounts/certificateOperationResults/read|Ruft die Ergebnisse eines lange andauernden Zertifikatvorgangs für ein Batch-Konto ab.|
|/batchAccounts/certificates/cancelDelete/action|Bricht eine fehlerhafte Löschung eines Zertifikats für ein Batch-Konto ab.|
|/batchAccounts/certificates/delete|Löscht ein Zertifikat aus einem Batch-Konto.|
|/batchAccounts/certificates/read|Listet die Zertifikate für ein Batch-Konto auf oder ruft die Eigenschaften eines Zertifikats ab.|
|/batchAccounts/certificates/write|Erstellt ein neues Zertifikat für ein Batch-Konto oder aktualisiert ein vorhandenes Zertifikat.|
|/batchAccounts/delete|Löscht ein Batch-Konto.|
|/batchAccounts/listkeys/action|Listet Zugriffsschlüssel für ein Batch-Konto auf.|
|/batchAccounts/operationResults/read|Ruft die Ergebnisse eines lange andauernden Vorgangs im Batch-Konto ab.|
|/batchAccounts/poolOperationResults/read|Ruft die Ergebnisse eines lange andauernden Poolvorgangs für ein Batch-Konto ab.|
|/batchAccounts/pools/delete|Löscht einen Pool aus einem Batch-Konto.|
|/batchAccounts/pools/disableAutoscale/action|Deaktiviert die automatische Skalierung für einen Pool des Batch-Kontos.|
|/batchAccounts/pools/read|Listet Pools für ein Batch-Konto auf oder ruft die Eigenschaften eines Pools ab.|
|/batchAccounts/pools/stopResize/action|Beendet einen aktiven Größenänderungsvorgang für einen Pool des Batch-Kontos.|
|/batchAccounts/pools/upgradeOs/action|Führt ein Upgrade für das Betriebssystem eines Batch-Kontopools durch.|
|/batchAccounts/pools/write|Erstellt einen neuen Pool für ein Batch-Konto oder aktualisiert einen vorhandenen Pool.|
|/batchAccounts/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/batchAccounts/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/batchAccounts/providers/Microsoft.Insights/logDefinitions/read|Ruft die verfügbaren Protokolle für den Batch-Dienst ab.|
|/batchAccounts/providers/Microsoft.Insights/metricDefinitions/read|Ruft die verfügbaren Metriken für den Batch-Dienst ab.|
|/batchAccounts/read|Listet Batch-Konten auf oder ruft die Eigenschaften eines Batch-Kontos ab.|
|/batchAccounts/regeneratekeys/action|Generiert Zugriffsschlüssel für ein Batch-Konto neu.|
|/batchAccounts/syncAutoStorageKeys/action|Synchronisiert Zugriffsschlüssel für das automatische Speicherkonto, das für ein Batch-Konto konfiguriert ist.|
|/batchAccounts/write|Erstellt ein neues Batch-Konto oder aktualisiert ein vorhandenes Batch-Konto.|
|/locations/checkNameAvailability/action|Prüft, ob der Kontoname gültig ist und noch nicht verwendet wird.|
|/locations/quotas/read|Ruft Batch-Kontingente des angegebenen Abonnements in der angegebenen Azure-Region ab.|
|/register/action|Registriert das Abonnement für den Batch-Ressourcenanbieter und ermöglicht die Erstellung von Batch-Konten.|
|/unregister/action|Hebt die Registrierung für das Abonnement für den Batch-Ressourcenanbieter auf und verhindert so die Erstellung von Batch-Konten.|

## <a name="microsoftbatchai"></a>Microsoft.BatchAI

| Vorgang | BESCHREIBUNG |
|---|---|
|/clusters/delete|Löscht einen Batch AI-Cluster.|
|/clusters/read|Listet Batch AI-Cluster auf oder ruft die Eigenschaften eines Batch AI-Clusters ab.|
|/clusters/remoteLoginInformation/action|Listet Informationen zur Remoteanmeldung für einen Batch AI-Cluster auf.|
|/clusters/write|Erstellt einen neuen Batch AI-Cluster oder aktualisiert einen vorhandenen Batch AI-Cluster.|
|/fileservers/delete|Löscht einen Batch AI-Dateiserver.|
|/fileservers/read|Listet Batch AI-Dateiserver auf oder ruft die Eigenschaften eines Batch AI-Dateiservers ab.|
|/fileservers/resume/action|Setzt einen Batch AI-Dateiserver fort.|
|/fileservers/suspend/action|Hält einen Batch AI-Dateiserver an.|
|/fileservers/write|Erstellt einen neuen Batch AI-Dateiserver oder aktualisiert einen vorhandenen Batch AI-Dateiserver.|
|/jobs/delete|Löscht einen Batch AI-Auftrag.|
|/jobs/read|Listet Batch AI-Aufträge auf oder ruft die Eigenschaften eines Batch AI-Auftrags ab.|
|/jobs/remoteLoginInformation/action|Listet Informationen zur Remoteanmeldung für einen Batch AI-Auftrag auf.|
|/jobs/terminate/action|Beendet einen Batch AI-Auftrag.|
|/jobs/write|Erstellt einen neues Batch AI-Auftrag oder aktualisiert einen vorhandenen Batch AI-Auftrag.|
|/register/action|Registriert das Abonnement für den Batch AI-Ressourcenanbieter und ermöglicht die Erstellung von Batch AI-Ressourcen.|

## <a name="microsoftbilling"></a>Microsoft.Billing

| Vorgang | BESCHREIBUNG |
|---|---|
|/billingPeriods/read|Listet die verfügbaren Abrechnungszeiträume auf.|
|/invoices/read|Listet verfügbare Rechnungen auf.|

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

| Vorgang | BESCHREIBUNG |
|---|---|
|/mapApis/Delete|Löschvorgang|
|/mapApis/listSecrets/action|Dient zum Auflisten der Geheimnisse.|
|/mapApis/listSingleSignOnToken/action|Dient zum Lesen des SSO-Autorisierungstokens für die Ressource.|
|/mapApis/Read|Lesevorgang|
|/mapApis/regenerateKey/action|Generiert den Schlüssel neu.|
|/mapApis/Write|Schreibvorgang|
|/Operations/read|Beschreibung des Vorgangs.|

## <a name="microsoftcache"></a>Microsoft.Cache

| Vorgang | BESCHREIBUNG |
|---|---|
|/checknameavailability/action|Prüft, ob ein Name für die Verwendung mit einer neuen Redis Cache-Instanz verfügbar ist.|
|/operations/read|Listet die Vorgänge auf, die der Microsoft.Cache-Anbieter unterstützt.|
|/redis/delete|Dient zum Löschen der gesamten Redis Cache-Instanz.|
|/redis/export/action|Dient dazu, Redis-Daten im angegebenen Format in Speicherblobs mit Präfix zu exportieren.|
|/redis/firewallRules/delete|Dient zum Löschen der IP-Firewallregeln einer Redis Cache-Instanz.|
|/redis/firewallRules/read|Dient zum Abrufen der IP-Firewallregeln einer Redis Cache-Instanz.|
|/redis/firewallRules/write|Dient zum Bearbeiten der IP-Firewallregeln einer Redis Cache-Instanz.|
|/redis/forceReboot/action|Dient zum Erzwingen des Neustarts einer Cache-Instanz (ggf. mit Datenverlust).|
|/redis/import/action|Dient zum Importieren von Daten eines angegebenen Formats aus mehreren Blobs in Redis.|
|/redis/linkedservers/delete|Dient zum Löschen von Verbindungsservern aus einer Redis Cache-Instanz.|
|/redis/linkedservers/read|Dient zum Abrufen von Verbindungsservern, die einer Redis Cache-Instanz zugeordnet sind.|
|/redis/linkedservers/write|Dient zum Hinzufügen von Verbindungsservern zu einer Redis Cache-Instanz.|
|/redis/listKeys/action|Dient zum Anzeigen des Werts von Redis Cache-Zugriffsschlüsseln im Verwaltungsportal.|
|/redis/listUpgradeNotifications/read|Dient zum Auflisten der neuesten Upgradebenachrichtigungen für den Cache-Mandanten.|
|/redis/locations/operationresults/read|Ruft das Ergebnis eines lange andauernden Vorgangs ab, dessen Location-Header zuvor an den Client zurückgegeben wurde.|
|/redis/metricDefinitions/read|Ruft die verfügbaren Metriken für eine Redis Cache-Instanz ab.|
|/redis/patchSchedules/delete|Dient zum Löschen des Patchzeitplans einer Redis Cache-Instanz.|
|/redis/patchSchedules/read|Ruft den Patchzeitplan einer Redis Cache-Instanz ab.|
|/redis/patchSchedules/write|Dient zum Ändern des Patchzeitplans einer Redis Cache-Instanz.|
|/redis/read|Dient zum Anzeigen der Redis Cache-Einstellungen und -Konfiguration im Verwaltungsportal.|
|/redis/regenerateKey/action|Dient zum Ändern des Werts von Redis Cache-Zugriffsschlüsseln im Verwaltungsportal.|
|/redis/start/action|Dient zum Starten einer Cache-Instanz.|
|/redis/stop/action|Dient zum Beenden einer Cache-Instanz.|
|/redis/write|Dient zum Ändern der Redis Cache-Einstellungen und -Konfiguration im Verwaltungsportal.|
|/register/action|Registriert den Ressourcenanbieter „Microsoft.Cache“ bei einem Abonnement.|
|/unregister/action|Hebt die Registrierung des Ressourcenanbieters „Microsoft.Cache“ bei einem Abonnement auf.|

## <a name="microsoftcapacity"></a>Microsoft.Capacity

| Vorgang | BESCHREIBUNG |
|---|---|
|/register/action|Registriert den Kapazitätsressourcenanbieter und ermöglicht die Erstellung von Kapazitätsressourcen.|
|/reservationorders/action|Aktualisiert eine Reservierung.|
|/reservationorders/delete|Löscht eine Reservierung.|
|/reservationorders/read|Liest alle Reservierungen.|
|/reservationorders/reservations/action|Aktualisiert eine Reservierung.|
|/reservationorders/reservations/delete|Löscht eine Reservierung.|
|/reservationorders/reservations/read|Liest alle Reservierungen.|
|/reservationorders/reservations/revisions/read|Liest alle Reservierungen.|
|/reservationorders/reservations/write|Erstellt eine Reservierung.|
|/reservationorders/write|Erstellt eine Reservierung.|

## <a name="microsoftcdn"></a>Microsoft.Cdn

| Vorgang | BESCHREIBUNG |
|---|---|
|/CheckNameAvailability/action||
|/CheckResourceUsage/action||
|/edgenodes/delete||
|/edgenodes/read||
|/edgenodes/write||
|/operationresults/delete||
|/operationresults/profileresults/CheckResourceUsage/action||
|/operationresults/profileresults/delete||
|/operationresults/profileresults/endpointresults/CheckResourceUsage/action||
|/operationresults/profileresults/endpointresults/customdomainresults/delete||
|/operationresults/profileresults/endpointresults/customdomainresults/ DisableCustomHttps/action||
|/operationresults/profileresults/endpointresults/customdomainresults/ EnableCustomHttps/action||
|/operationresults/profileresults/endpointresults/customdomainresults/read||
|/operationresults/profileresults/endpointresults/customdomainresults/write||
|/operationresults/profileresults/endpointresults/delete||
|/operationresults/profileresults/endpointresults/Load/action||
|/operationresults/profileresults/endpointresults/originresults/delete||
|/operationresults/profileresults/endpointresults/originresults/read||
|/operationresults/profileresults/endpointresults/originresults/write||
|/operationresults/profileresults/endpointresults/Purge/action||
|/operationresults/profileresults/endpointresults/read||
|/operationresults/profileresults/endpointresults/Start/action||
|/operationresults/profileresults/endpointresults/Stop/action||
|/operationresults/profileresults/endpointresults/ValidateCustomDomain/action||
|/operationresults/profileresults/endpointresults/write||
|/operationresults/profileresults/GenerateSsoUri/action||
|/operationresults/profileresults/GetSupportedOptimizationTypes/action||
|/operationresults/profileresults/read||
|/operationresults/profileresults/write||
|/operationresults/read||
|/operationresults/write||
|/operations/read||
|/profiles/CheckResourceUsage/action||
|/profiles/delete||
|/profiles/endpoints/CheckResourceUsage/action||
|/profiles/endpoints/customdomains/delete||
|/profiles/endpoints/customdomains/DisableCustomHttps/action||
|/profiles/endpoints/customdomains/EnableCustomHttps/action||
|/profiles/endpoints/customdomains/read||
|/profiles/endpoints/customdomains/write||
|/profiles/endpoints/delete||
|/profiles/endpoints/Load/action||
|/profiles/endpoints/origins/delete||
|/profiles/endpoints/origins/read||
|/profiles/endpoints/origins/write||
|/profiles/endpoints/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellungen für die Ressource ab.|
|/profiles/endpoints/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellungen für die Ressource.|
|/profiles/endpoints/providers/Microsoft.Insights/logDefinitions/read|Ruft die verfügbaren Protokolle für Microsoft.Cdn ab.|
|/profiles/endpoints/Purge/action||
|/profiles/endpoints/read||
|/profiles/endpoints/Start/action||
|/profiles/endpoints/Stop/action||
|/profiles/endpoints/ValidateCustomDomain/action||
|/profiles/endpoints/write||
|/profiles/GenerateSsoUri/action||
|/profiles/GetSupportedOptimizationTypes/action||
|/profiles/read||
|/profiles/write||
|/register/action|Hiermit wird das Abonnement für den CDN-Ressourcenanbieter registriert und die Erstellung von CDN-Profilen aktiviert.|
|/ValidateProbe/action||

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

| Vorgang | BESCHREIBUNG |
|---|---|
|/certificateOrders/certificates/Delete|Dient zum Löschen eines vorhandenen Zertifikats.|
|/certificateOrders/certificates/Read|Dient zum Abrufen der Zertifikatliste.|
|/certificateOrders/certificates/Write|Dient zum Hinzufügen eines neuen Zertifikats oder zum Aktualisieren eines bereits vorhandenen.|
|/certificateOrders/Delete|Dient zum Löschen eines vorhandenen AppServiceCertificate-Elements.|
|/certificateOrders/operations/Read|Dient zum Auflisten aller Vorgänge über die App Service-Zertifikatregistrierung.|
|/certificateOrders/Read|Dient zum Abrufen der CertificateOrders-Liste.|
|/certificateOrders/reissue/Action|Dient zum erneuten Ausstellen einer vorhandenen Zertifikatbestellung.|
|/certificateOrders/renew/Action|Dient zum Verlängern einer vorhandenen Zertifikatbestellung.|
|/certificateOrders/resendEmail/Action|Dient zum erneuten Senden von Zertifikat-E-Mails.|
|/certificateOrders/resendRequestEmails/Action|Dient zum erneuten Senden von Anforderungs-E-Mails an eine andere E-Mail-Adresse.|
|/certificateOrders/resendRequestEmails/Action|Dient zum Abrufen eines Websitesiegels für ein ausgestelltes App Service-Zertifikat.|
|/certificateOrders/retrieveCertificateActions/Action|Dient zum Abrufen der Liste mit Zertifikataktionen.|
|/certificateOrders/retrieveEmailHistory/Action|Dient zum Abrufen des Zertifikat-E-Mail-Verlaufs.|
|/certificateOrders/verifyDomainOwnership/Action|Überprüfen des Domänenbesitzes|
|/certificateOrders/Write|Dient zum Hinzufügen eines neuen certificateOrder-Elements oder zum Aktualisieren eines bereits vorhandenen.|
|/provisionGlobalAppServicePrincipalInUserTenant/Action|Dient zum Bereitstellen eines Dienstprinzipals für einen Dienst-App-Prinzipal.|
|/register/action|Dient zum Registrieren des Microsoft-Zertifikatressourcenanbieters für das Abonnement.|
|/validateCertificateRegistrationInformation/Action|Dient zum Überprüfen eines Zertifikaterwerbsobjekts, ohne es zu übermitteln.|

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

| Vorgang | BESCHREIBUNG |
|---|---|
|/capabilities/read|Zeigt die Funktionen an.|
|/checkDomainNameAvailability/action|Prüft die Verfügbarkeit eines angegebenen Domänennamens.|
|/domainNames/active/write|Legt den aktiven Domänennamen fest.|
|/domainNames/availabilitySets/read|Dient zum Anzeigen der Verfügbarkeitsgruppe für die Ressource.|
|/domainNames/capabilities/read|Zeigt die Domänennamenfunktionen an.|
|/domainNames/delete|Dient zum Entfernen der Domänennamen für Ressourcen.|
|/domainNames/extensions/delete|Dient zum Entfernen der Domänennamenerweiterungen.|
|/domainNames/extensions/operationStatuses/read|Liest den Vorgangsstatus für die Domänennamenerweiterungen.|
|/domainNames/extensions/read|Gibt die Domänennamenerweiterungen zurück.|
|/domainNames/extensions/write|Dient zum Hinzufügen der Domänennamenerweiterungen.|
|/domainNames/internalLoadBalancers/delete|Dient zum Entfernen eines neuen internen Lastenausgleichsmoduls.|
|/domainNames/internalLoadBalancers/operationStatuses/read|Liest den Vorgangsstatus für die internen Lastenausgleichsmodule der Domänennamen.|
|/domainNames/internalLoadBalancers/read|Ruft die internen Lastenausgleichsmodule ab.|
|/domainNames/internalLoadBalancers/write|Erstellt ein neues internes Lastenausgleichsmodul.|
|/domainNames/loadBalancedEndpointSets/operationStatuses/read|Liest den Vorgangsstatus für die Endpunktgruppen mit Lastenausgleich der Domänennamen.|
|/domainNames/loadBalancedEndpointSets/read|Zeigt die Endpunktgruppen mit Lastenausgleich an.|
|/domainNames/read|Dient zum Zurückgeben der Domänennamen für Ressourcen.|
|/domainNames/serviceCertificates/delete|Dient zum Löschen der verwendeten Dienstzertifikate.|
|/domainNames/serviceCertificates/operationStatuses/read|Liest den Vorgangsstatus für die Dienstzertifikate der Domänennamen.|
|/domainNames/serviceCertificates/read|Gibt die verwendeten Dienstzertifikate zurück.|
|/domainNames/serviceCertificates/write|Dient zum Hinzufügen oder Ändern der verwendeten Dienstzertifikate.|
|/domainNames/slots/delete|Löscht einen angegebenen Bereitstellungsslot.|
|/domainNames/slots/operationStatuses/read|Liest den Vorgangsstatus für die Domänennamenslots.|
|/domainNames/slots/read|Zeigt die Bereitstellungsslots an.|
|/domainNames/slots/roles/extensionReferences/delete|Dient zum Entfernen des Erweiterungsverweises für die Rolle „Bereitstellungsslot“.|
|/domainNames/slots/roles/extensionReferences/operationStatuses/read|Liest den Vorgangsstatus für die Slotrollen-Erweiterungsverweise der Domänennamen.|
|/domainNames/slots/roles/extensionReferences/read|Gibt den Erweiterungsverweis für die Rolle „Bereitstellungsslot“ zurück.|
|/domainNames/slots/roles/extensionReferences/write|Dient zum Hinzufügen oder Ändern des Erweiterungsverweises für die Rolle „Bereitstellungsslot“.|
|/domainNames/slots/roles/providers/Microsoft.Insights/diagnosticSettings/read|Dient zum Abrufen der Diagnoseeinstellungen.|
|/domainNames/slots/roles/providers/Microsoft.Insights/diagnosticSettings/write|Dient zum Hinzufügen oder Ändern der Diagnoseeinstellungen.|
|/domainNames/slots/roles/providers/Microsoft.Insights/metricDefinitions/read|Ruft die Metrikdefinitionen ab.|
|/domainNames/slots/roles/read|Dient zum Abrufen der Rolle für den Bereitstellungsslot.|
|/domainNames/slots/roles/roleInstances/operationStatuses/read|Liest den Vorgangsstatus für die Slotrollen-Rolleninstanzen der Domänennamen.|
|/domainNames/slots/roles/roleInstances/read|Dient zum Abrufen der Rolleninstanz.|
|/domainNames/slots/roles/roleInstances/rebuild/action|Hiermit wird die Rolleninstanz neu erstellt.|
|/domainNames/slots/roles/roleInstances/reimage/action|Führt ein Reimaging der Rolleninstanz aus.|
|/domainNames/slots/roles/roleInstances/restart/action|Startet Rolleninstanzen neu.|
|/domainNames/slots/start/action|Startet einen Bereitstellungsslot.|
|/domainNames/slots/state/start/write|Ändert den Zustand des Bereitstellungsslots in „Beendet“.|
|/domainNames/slots/state/stop/write|Ändert den Zustand des Bereitstellungsslots in „Gestartet“.|
|/domainNames/slots/stop/action|Hält den Bereitstellungsslot an.|
|/domainNames/slots/upgradeDomain/write|Dient zum Aufrüsten der Domäne.|
|/domainNames/slots/write|Erstellt oder aktualisiert die Bereitstellung.|
|/domainNames/swap/action|Vertauscht Stagingslot und Produktionsslot.|
|/domainNames/write|Dient zum Hinzufügen oder Ändern der Domänennamen für Ressourcen.|
|/moveSubscriptionResources/action|Dient zum Verschieben aller klassischen Ressourcen in ein anderes Abonnement.|
|/operatingSystemFamilies/read|Listet die in Microsoft Azure verfügbaren Gastbetriebssystemfamilien sowie die für jede Familie verfügbaren Betriebssystemversionen auf.|
|/operatingSystems/read|Listet die Versionen des Gastbetriebssystems auf, die derzeit in Microsoft Azure verfügbar sind.|
|/quotas/read|Dient zum Abrufen des Kontingents für das Abonnement.|
|/register/action|Dient zum Registrieren für klassische Computeressourcen.|
|/resourceTypes/skus/read|Ruft die SKU-Liste für unterstützte Ressourcentypen ab.|
|/validateSubscriptionMoveAvailability/action|Dient zum Überprüfen der Verfügbarkeit des Abonnements für einen klassischen Verschiebevorgang.|
|/virtualMachines/associatedNetworkSecurityGroups/delete|Löscht die Netzwerksicherheitsgruppe, die dem virtuellen Computer zugeordnet ist.|
|/virtualMachines/associatedNetworkSecurityGroups/operationStatuses/read|Liest den Vorgangsstatus für die den virtuellen Computern zugeordneten Netzwerksicherheitsgruppen.|
|/virtualMachines/associatedNetworkSecurityGroups/read|Ruft die Netzwerksicherheitsgruppe ab, die dem virtuellen Computer zugeordnet ist.|
|/virtualMachines/associatedNetworkSecurityGroups/write|Fügt eine Netzwerksicherheitsgruppe hinzu, die dem virtuellen Computer zugeordnet ist.|
|/virtualMachines/asyncOperations/read|Ruft die möglichen asynchronen Vorgänge ab.|
|/virtualMachines/attachDisk/action|Fügt einem virtuellen Computer einen Datenträger an.|
|/virtualMachines/delete|Entfernt virtuelle Computer.|
|/virtualMachines/detachDisk/action|Trennt einen Datenträger von einem virtuellen Computer.|
|/virtualMachines/disks/read|Ruft die Liste mit den Datenträgern ab.|
|/virtualMachines/downloadRemoteDesktopConnectionFile/action|Lädt die RDP-Datei für einen virtuellen Computer herunter.|
|/virtualMachines/extensions/operationStatuses/read|Liest den Vorgangsstatus für die VM-Erweiterungen.|
|/virtualMachines/extensions/read|Ruft die VM-Erweiterung ab.|
|/virtualMachines/extensions/write|Legt die VM-Erweiterung fest.|
|/virtualMachines/metrics/read|Ruft die Metriken ab.|
|/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/delete|Löscht die Netzwerksicherheitsgruppe, die der Netzwerkschnittstelle zugeordnet ist.|
|/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/ operationStatuses/read|Liest den Vorgangsstatus für die den virtuellen Computern zugeordneten Netzwerksicherheitsgruppen.|
|/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/read|Ruft die Netzwerksicherheitsgruppe ab, die der Netzwerkschnittstelle zugeordnet ist.|
|/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/write|Fügt eine Netzwerksicherheitsgruppe hinzu, die der Netzwerkschnittstelle zugeordnet ist.|
|/virtualMachines/operationStatuses/read|Liest den Vorgangsstatus für die virtuellen Computer.|
|/virtualMachines/performMaintenance/action|Hiermit wird eine Wartung für den virtuellen Computer durchgeführt.|
|/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/read|Dient zum Abrufen der Diagnoseeinstellungen.|
|/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/write|Dient zum Hinzufügen oder Ändern der Diagnoseeinstellungen.|
|/virtualMachines/providers/Microsoft.Insights/metricDefinitions/read|Ruft die Metrikdefinitionen ab.|
|/virtualMachines/read|Ruft eine Liste mit virtuellen Computern ab.|
|/virtualMachines/redeploy/action|Stellt den virtuellen Computer erneut bereit.|
|/virtualMachines/restart/action|Startet virtuelle Computer neu.|
|/virtualMachines/shutdown/action|Fährt den virtuellen Computer herunter.|
|/virtualMachines/start/action|Starten Sie den virtuellen Computer.|
|/virtualMachines/stop/action|Beendet den virtuellen Computer.|
|/virtualMachines/write|Dient zum Hinzufügen oder Ändern virtueller Computer.|

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

| Vorgang | BESCHREIBUNG |
|---|---|
|/gatewaySupportedDevices/read|Ruft die Liste mit unterstützten Geräten ab.|
|/networkSecurityGroups/delete|Löscht die Netzwerksicherheitsgruppe.|
|/networkSecurityGroups/operationStatuses/read|Liest den Vorgangsstatus für die Netzwerksicherheitsgruppe.|
|/networkSecurityGroups/read|Ruft die Netzwerksicherheitsgruppe ab.|
|/networkSecurityGroups/securityRules/delete|Löscht die Sicherheitsregel.|
|/networkSecurityGroups/securityRules/operationStatuses/read|Liest den Vorgangsstatus für die Sicherheitsregeln der Netzwerksicherheitsgruppe.|
|/networkSecurityGroups/securityRules/read|Ruft die Sicherheitsregel ab.|
|/networkSecurityGroups/securityRules/write|Fügt eine Sicherheitsregel hinzu oder aktualisiert sie.|
|/networkSecurityGroups/write|Fügt eine neue Netzwerksicherheitsgruppe hinzu.|
|/quotas/read|Dient zum Abrufen des Kontingents für das Abonnement.|
|/register/action|Dient zum Registrieren für klassische Netzwerkressourcen.|
|/reservedIps/delete|Dient zum Löschen einer reservierten IP-Adresse.|
|/reservedIps/join/action|Dient zum Einbinden einer reservierten IP-Adresse.|
|/reservedIps/link/action|Dient zum Verknüpfen einer reservierten IP-Adresse.|
|/reservedIps/operationStatuses/read|Liest den Vorgangsstatus für die reservierten IP-Adressen.|
|/reservedIps/read|Ruft die reservierten IP-Adressen ab.|
|/reservedIps/write|Dient zum Hinzufügen einer neuen reservierten IP-Adresse.|
|/virtualNetworks/capabilities/read|Zeigt die Funktionen an.|
|/virtualNetworks/checkIPAddressAvailability/action|Prüft die Verfügbarkeit einer bestimmten IP-Adresse in einem virtuellen Netzwerk.|
|/virtualNetworks/delete|Löscht das virtuelle Netzwerk.|
|/virtualNetworks/gateways/clientRevokedCertificates/delete|Hebt die Sperrung eines Clientzertifikats auf.|
|/virtualNetworks/gateways/clientRevokedCertificates/read|Dient zum Lesen der gesperrten Clientzertifikate.|
|/virtualNetworks/gateways/clientRevokedCertificates/write|Sperrt ein Clientzertifikat.|
|/virtualNetworks/gateways/clientRootCertificates/delete|Löscht das Clientzertifikat des virtuellen Netzwerkgateways.|
|/virtualNetworks/gateways/clientRootCertificates/download/action|Lädt ein Zertifikat nach Fingerabdruck herunter.|
|/virtualNetworks/gateways/clientRootCertificates/listPackage/action|Listet das Zertifikatpaket für das virtuelle Netzwerkgateway auf.|
|/virtualNetworks/gateways/clientRootCertificates/read|Dient zum Suchen nach den Clientstammzertifikaten.|
|/virtualNetworks/gateways/clientRootCertificates/write|Lädt ein neues Clientstammzertifikat hoch.|
|/virtualNetworks/gateways/connections/connect/action|Stellt eine Site-to-Site-Gatewayverbindung her.|
|/virtualNetworks/gateways/connections/disconnect/action|Trennt eine Site-to-Site-Gatewayverbindung.|
|/virtualNetworks/gateways/connections/read|Ruft die Verbindungsliste ab.|
|/virtualNetworks/gateways/connections/test/action|Testet eine Site-to-Site-Gatewayverbindung.|
|/virtualNetworks/gateways/delete|Löscht das virtuelle Netzwerkgateway.|
|/virtualNetworks/gateways/downloadDeviceConfigurationScript/action|Lädt das Gerätekonfigurationsskript herunter.|
|/virtualNetworks/gateways/downloadDiagnostics/action|Lädt die Gatewaydiagnose herunter.|
|/virtualNetworks/gateways/listCircuitServiceKey/action|Ruft den Schlüssel des Verbindungsdiensts ab.|
|/virtualNetworks/gateways/listPackage/action|Listet das Paket für das virtuelle Netzwerkgateway auf.|
|/virtualNetworks/gateways/operationStatuses/read|Liest den Vorgangsstatus für die virtuellen Netzwerkgateways.|
|/virtualNetworks/gateways/packages/read|Ruft das Paket für das virtuelle Netzwerkgateway ab.|
|/virtualNetworks/gateways/read|Ruft die virtuellen Netzwerkgateways ab.|
|/virtualNetworks/gateways/startDiagnostics/action|Startet die Diagnose für das virtuelle Netzwerkgateway.|
|/virtualNetworks/gateways/stopDiagnostics/action|Beendet die Diagnose für das virtuelle Netzwerkgateway.|
|/virtualNetworks/gateways/write|Fügt ein virtuelles Netzwerkgateway hinzu.|
|/virtualNetworks/join/action|Führt zum Beitritt zum virtuellen Netzwerk.|
|/virtualNetworks/operationStatuses/read|Liest den Vorgangsstatus für die virtuellen Netzwerke.|
|/virtualNetworks/peer/action|Richtet ein virtuelles Netzwerk als Peer für ein anderes virtuelles Netzwerk ein.|
|/virtualNetworks/read|Dient zum Abrufen des virtuellen Netzwerks.|
|/virtualNetworks/subnets/associatedNetworkSecurityGroups/delete|Löscht die Netzwerksicherheitsgruppe, die dem Subnetz zugeordnet ist.|
|/virtualNetworks/subnets/associatedNetworkSecurityGroups/operationStatuses/read|Liest den Vorgangsstatus für die dem Subnetz des virtuellen Netzwerks zugeordnete Netzwerksicherheitsgruppe.|
|/virtualNetworks/subnets/associatedNetworkSecurityGroups/read|Ruft die Netzwerksicherheitsgruppe ab, die dem Subnetz zugeordnet ist.|
|/virtualNetworks/subnets/associatedNetworkSecurityGroups/write|Fügt die Netzwerksicherheitsgruppe hinzu, die dem Subnetz zugeordnet ist.|
|/virtualNetworks/write|Dient zum Hinzufügen eines neuen virtuellen Netzwerks.|

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

| Vorgang | BESCHREIBUNG |
|---|---|
|/capabilities/read|Zeigt die Funktionen an.|
|/checkStorageAccountAvailability/action|Prüft die Verfügbarkeit eines Speicherkontos.|
|/disks/read|Gibt den Speicherkontodatenträger zurück.|
|/images/read|Gibt das Image zurück.|
|/osImages/read|Gibt das Betriebssystemimage zurück.|
|/publicImages/read|Ruft das öffentliche VM-Image ab.|
|/quotas/read|Dient zum Abrufen des Kontingents für das Abonnement.|
|/register/action|Dient zum Registrieren für klassischen Speicher.|
|/storageAccounts/delete|Löschen Sie das Speicherkonto.|
|/storageAccounts/disks/delete|Löscht den angegebenen Speicherkontodatenträger.|
|/storageAccounts/disks/operationStatuses/read|Liest den Vorgangsstatus für die Ressource.|
|/storageAccounts/disks/read|Gibt den Speicherkontodatenträger zurück.|
|/storageAccounts/disks/write|Fügt einen Speicherkontodatenträger hinzu.|
|/storageAccounts/images/delete|Löscht das angegebene Speicherkontoimage.|
|/storageAccounts/images/read|Gibt das Speicherkontoimage zurück.|
|/storageAccounts/listKeys/action|Listet die Zugriffsschlüssel für die Speicherkonten auf.|
|/storageAccounts/operationStatuses/read|Liest den Vorgangsstatus für die Ressource.|
|/storageAccounts/osImages/delete|Löscht das angegebene Betriebssystemimage für das Speicherkonto.|
|/storageAccounts/osImages/read|Gibt das Betriebssystemimage für das Speicherkonto zurück.|
|/storageAccounts/read|Dient zum Zurückgeben des Speicherkontos mit dem angegebenen Konto.|
|/storageAccounts/regenerateKey/action|Generiert die vorhandenen Zugriffsschlüssel für das Speicherkonto neu.|
|/storageAccounts/services/diagnosticSettings/read|Dient zum Abrufen der Diagnoseeinstellungen.|
|/storageAccounts/services/diagnosticSettings/write|Dient zum Hinzufügen oder Ändern der Diagnoseeinstellungen.|
|/storageAccounts/services/metricDefinitions/read|Ruft die Metrikdefinitionen ab.|
|/storageAccounts/services/metrics/read|Ruft die Metriken ab.|
|/storageAccounts/services/read|Dient zum Abrufen der verfügbaren Dienste.|
|/storageAccounts/write|Fügt ein neues Speicherkonto hinzu.|

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

| Vorgang | BESCHREIBUNG |
|---|---|
|/accounts/delete|Löscht API-Konten.|
|/accounts/listKeys/action|Dient zum Auflisten von Schlüsseln.|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/accounts/providers/Microsoft.Insights/metricDefinitions/read|Ruft die verfügbaren Metriken für Cognitive Services ab.|
|/accounts/read|Liest API-Konten.|
|/accounts/regenerateKey/action|Dient zum erneuten Generieren eines Schlüssels.|
|/accounts/skus/read|Liest verfügbare SKUs für eine vorhandene Ressource.|
|/accounts/usages/read|Dient zum Abrufen der Kontingentnutzung für eine vorhandene Ressource.|
|/accounts/write|Schreibt API-Konten.|
|/locations/checkSkuAvailability/action|Liest verfügbare SKUs für ein Abonnement.|
|/Operations/read|Listet alle verfügbaren Vorgänge auf.|
|/register/action|Registriert ein Abonnement für Cognitive Services.|
|/skus/read|Liest die verfügbaren SKUs für Cognitive Services.|

## <a name="microsoftcommerce"></a>Microsoft.Commerce

| Vorgang | BESCHREIBUNG |
|---|---|
|/RateCard/read|Gibt Angebotsdaten, Metadaten für Ressourcen/Verbrauchseinheiten und Preise für das angegebene Abonnement zurück.|
|/UsageAggregates/read|Ruft die Nutzung von Microsoft Azure für ein Abonnement ab. Das Ergebnis enthält aggregierte Nutzungsdaten sowie abonnement- und ressourcenbezogene Informationen für einen bestimmten Zeitbereich.|

## <a name="microsoftcompute"></a>Microsoft.Compute

| Vorgang | BESCHREIBUNG |
|---|---|
|/availabilitySets/delete|Löscht die Verfügbarkeitsgruppe.|
|/availabilitySets/read|Dient zum Abrufen der Eigenschaften einer Verfügbarkeitsgruppe.|
|/availabilitySets/vmSizes/read|Dient zum Auflisten der Größen, die zum Erstellen oder Aktualisieren eines virtuellen Computers in der Verfügbarkeitsgruppe zur Verfügung stehen.|
|/availabilitySets/write|Erstellt eine neue Verfügbarkeitsgruppe oder aktualisiert eine bereits vorhandene.|
|/disks/beginGetAccess/action|Dient zum Abrufen des SAS-URIs des Datenträgers für den Blobzugriff.|
|/disks/delete|Löscht den Datenträger.|
|/disks/endGetAccess/action|Dient zum Sperren des SAS-URIs des Datenträgers.|
|/disks/read|Dient zum Abrufen der Eigenschaften eines Datenträgers.|
|/disks/write|Erstellt einen neuen Datenträger oder aktualisiert einen bereits vorhandenen.|
|/images/delete|Löscht das Image.|
|/images/read|Dient zum Abrufen der Eigenschaften des Images.|
|/images/write|Erstellt ein neues Image oder aktualisiert ein bereits vorhandenes.|
|/locations/capsOperations/read|Ruft den Status eines asynchronen Vorgangs zum Abrufen von Obergrenzen ab.|
|/locations/diskOperations/read|Ruft den Status eines asynchronen Vorgangs zum Abrufen von Datenträgern ab.|
|/locations/operations/read|Ruft den Status eines asynchronen Vorgangs ab.|
|/locations/publishers/artifacttypes/offers/read|Ruft die Eigenschaften eines Plattformimageangebots ab.|
|/locations/publishers/artifacttypes/offers/skus/read|Ruft die Eigenschaften einer Plattformimage-SKU ab.|
|/locations/publishers/artifacttypes/offers/skus/versions/read|Ruft die Eigenschaften einer Plattformimageversion ab.|
|/locations/publishers/artifacttypes/types/read|Dient zum Abrufen der Eigenschaften eines VMExtension-Typs.|
|/locations/publishers/artifacttypes/types/versions/read|Dient zum Abrufen der Eigenschaften einer VMExtension-Version.|
|/locations/publishers/read|Dient zum Abrufen der Eigenschaften eines Herausgebers.|
|/locations/runCommands/read|Hiermit werden die am Standort verfügbaren Ausführungsbefehle aufgelistet.|
|/locations/usages/read|Ruft Dienstlimits und aktuelle Nutzungsmengen für die Computeressourcen des Abonnements an einem Standort ab.|
|/locations/vmSizes/read|Listet die verfügbaren VM-Größen für einen Standort auf.|
|/operations/read|Listet verfügbare Vorgänge für den Microsoft.Compute-Ressourcenanbieter auf.|
|/register/action|Registriert ein Abonnement beim Microsoft.Compute-Ressourcenanbieter.|
|/restorePointCollections/delete|Löscht die Wiederherstellungspunktsammlung und die darin enthaltenen Wiederherstellungspunkte.|
|/restorePointCollections/read|Dient zum Abrufen der Eigenschaften einer Wiederherstellungspunktsammlung.|
|/restorePointCollections/restorePoints/delete|Löscht den Wiederherstellungspunkt.|
|/restorePointCollections/restorePoints/read|Dient zum Abrufen der Eigenschaften eines Wiederherstellungspunkts.|
|/restorePointCollections/restorePoints/retrieveSasUris/action|Dient zum Abrufen der Eigenschaften eines Wiederherstellungspunkts sowie der Blob-SAS-URIs.|
|/restorePointCollections/restorePoints/write|Erstellt einen neuen Wiederherstellungspunkt.|
|/restorePointCollections/write|Erstellt eine neue Wiederherstellungspunktsammlung oder aktualisiert eine bereits vorhandene.|
|/sharedVMImages/delete|Löscht SharedVMImage.|
|/sharedVMImages/read|Dient zum Abrufen der Eigenschaften von SharedVMImage.|
|/sharedVMImages/versions/delete|Dient zum Löschen von SharedVMImageVersion.|
|/sharedVMImages/versions/read|Ruft die Eigenschaften von SharedVMImageVersion ab.|
|/sharedVMImages/versions/replicate/action|Repliziert SharedVMImageVersion in Zielregionen.|
|/sharedVMImages/versions/write|Erstellt eine neue SharedVMImageVersion oder aktualisiert eine vorhandene.|
|/sharedVMImages/write|Erstellt ein neues SharedVMImage oder aktualisiert ein vorhandenes.|
|/skus/read|Ruft die Liste der verfügbaren Microsoft.Compute-SKUs für Ihr Abonnement ab.|
|/snapshots/beginGetAccess/action|Dient zum Abrufen des SAS-URI der Momentaufnahme für den Blobzugriff.|
|/snapshots/delete|Dient zum Löschen einer Momentaufnahme.|
|/snapshots/endGetAccess/action|Dient zum Sperren des SAS-URI der Momentaufnahme.|
|/snapshots/read|Dient zum Abrufen der Eigenschaften einer Momentaufnahme.|
|/snapshots/write|Dient zum Erstellen einer neuen Momentaufnahme oder zum Aktualisieren einer bereits vorhandenen.|
|/virtualMachines/capture/action|Erfasst den virtuellen Computer, indem die virtuellen Datenträger kopiert werden und eine Vorlage generiert wird, die dann zum Erstellen ähnlicher virtueller Computer verwendet werden kann.|
|/virtualMachines/convertToManagedDisks/action|Konvertiert blobbasierte Datenträger des virtuellen Computers in verwaltete Datenträger.|
|/virtualMachines/deallocate/action|Schaltet den virtuellen Computer aus und gibt die Computeressourcen frei.|
|/virtualMachines/delete|Löscht den virtuellen Computer.|
|/virtualMachines/extensions/delete|Löscht die VM-Erweiterung.|
|/virtualMachines/extensions/read|Dient zum Abrufen der Eigenschaften einer VM-Erweiterung.|
|/virtualMachines/extensions/write|Erstellt eine neue VM-Erweiterung oder aktualisiert eine bereits vorhandene.|
|/virtualMachines/generalize/action|Legt den Zustand des virtuellen Computers auf „Generalisiert“ fest und bereitet den virtuellen Computer auf die Erfassung vor.|
|/virtualMachines/instanceView/read|Ruft den detaillierten Laufzeitstatus des virtuellen Computers und seiner Ressourcen ab.|
|/virtualMachines/performMaintenance/action|Führt den Wartungsvorgang für die VM durch.|
|/virtualMachines/powerOff/action|Schaltet den virtuellen Computer aus. Der virtuelle Computer wird weiterhin in Rechnung gestellt.|
|/virtualMachines/providers/Microsoft.Insights/metricDefinitions/read|Hiermit werden VM-Metrikdefinitionen gelesen.|
|/virtualMachines/read|Dient zum Abrufen der Eigenschaften eines virtuellen Computers.|
|/virtualMachines/redeploy/action|Stellt den virtuellen Computer erneut bereit.|
|/virtualMachines/reimage/action|Hiermit wird ein Reimaging für einen virtuellen Computer durchgeführt, der einen differenzierenden Datenträger verwendet.|
|/virtualMachines/restart/action|Startet den virtuellen Computer neu.|
|/virtualMachines/runCommand/action|Hiermit wird ein vordefiniertes Skript für den virtuellen Computer ausgeführt.|
|/virtualMachines/start/action|Startet den virtuellen Computer.|
|/virtualMachines/vmSizes/read|Listet die verfügbaren Größen auf, auf die der virtuelle Computer aktualisiert werden kann.|
|/virtualMachines/write|Erstellt einen neuen virtuellen Computer oder aktualisiert einen vorhandenen virtuellen Computer.|
|/virtualMachineScaleSets/deallocate/action|Schaltet die Computeressourcen für die Instanzen der VM-Skalierungsgruppe aus und gibt sie frei. |
|/virtualMachineScaleSets/delete|Löscht die VM-Skalierungsgruppe.|
|/virtualMachineScaleSets/delete/action|Löscht die Instanzen der VM-Skalierungsgruppe.|
|/virtualMachineScaleSets/extensions/delete|Hiermit wird die VM-Skalierungsgruppenerweiterung gelöscht.|
|/virtualMachineScaleSets/extensions/read|Hiermit werden die Eigenschaften einer VM-Skalierungsgruppenerweiterung abgerufen.|
|/virtualMachineScaleSets/extensions/write|Hiermit wird eine neue VM-Skalierungsgruppenerweiterung erstellt oder eine vorhandene aktualisiert.|
|/virtualMachineScaleSets/forceRecoveryServiceFabricPlatformUpdateDomainWalk/action|Hiermit werden Plattformupdatedomänen einer Service Fabric-VM-Skalierungsgruppe manuell durchgeführt, um eine ausstehende Aktualisierung fertigzustellen.|
|/virtualMachineScaleSets/instanceView/read|Ruft die Instanzansicht der VM-Skalierungsgruppe ab.|
|/virtualMachineScaleSets/manualUpgrade/action|Aktualisiert Instanzen manuell auf das neueste Modell der VM-Skalierungsgruppe.|
|/virtualMachineScaleSets/networkInterfaces/read|Ruft die Eigenschaften aller Netzwerkschnittstellen einer VM-Skalierungsgruppe ab.|
|/virtualMachineScaleSets/osUpgradeHistory/read|Ruft den Verlauf von Betriebssystemupgrades für eine VM-Skalierungsgruppe ab.|
|/virtualMachineScaleSets/performMaintenance/action|Führt ungeplante Wartungsaufgaben für die Instanzen der VM-Skalierungsgruppe durch.|
|/virtualMachineScaleSets/powerOff/action|Schaltet die Instanzen der VM-Skalierungsgruppe aus.|
|/virtualMachineScaleSets/providers/Microsoft.Insights/metricDefinitions/read|Hiermit werden Metrikdefinitionen für VM-Skalierungsgruppen gelesen.|
|/virtualMachineScaleSets/publicIPAddresses/read|Ruft die Eigenschaften aller öffentlichen IP-Adressen einer VM-Skalierungsgruppe ab.|
|/virtualMachineScaleSets/read|Dient zum Abrufen der Eigenschaften einer VM-Skalierungsgruppe.|
|/virtualMachineScaleSets/redeploy/action|Stellt erneut die Instanzen der VM-Skalierungsgruppe bereit.|
|/virtualMachineScaleSets/reimage/action|Führt ein Reimaging für die Instanzen der VM-Skalierungsgruppe durch.|
|/virtualMachineScaleSets/restart/action|Startet die Instanzen der VM-Skalierungsgruppe neu.|
|/virtualMachineScaleSets/rollingUpgrades/cancel/action|Bricht das parallele Upgrade einer VM-Skalierungsgruppe ab.|
|/virtualMachineScaleSets/rollingUpgrades/read|Hiermit wird der aktuelle Status des parallelen Upgrades für eine VM-Skalierungsgruppe abgerufen.|
|/virtualMachineScaleSets/scale/action|Hiermit wird überprüft, ob eine vorhandene VM-Skalierungsgruppe horizontal auf die angegebene Anzahl von Instanzen hoch- oder herunterskaliert werden kann.|
|/virtualMachineScaleSets/skus/read|Listet die gültigen SKUs für eine vorhandene VM-Skalierungsgruppe auf.|
|/virtualMachineScaleSets/start/action|Startet die Instanzen der VM-Skalierungsgruppe.|
|/virtualMachineScaleSets/virtualMachines/deallocate/action|Schaltet die Computeressourcen für einen virtuellen Computer in einer VM-Skalierungsgruppe aus und gibt sie frei.|
|/virtualMachineScaleSets/virtualMachines/delete|Dient zum Löschen eines bestimmten virtuellen Computers in einer VM-Skalierungsgruppe.|
|/virtualMachineScaleSets/virtualMachines/instanceView/read|Ruft die Instanzansicht eines virtuellen Computers in einer VM-Skalierungsgruppe ab.|
|/virtualMachineScaleSets/virtualMachines/networkInterfaces/ ipConfigurations/publicIPAddresses/read|Ruft die Eigenschaften einer öffentlichen IP-Adresse ab, die mithilfe einer VM-Skalierungsgruppe erstellt wurde. Eine VM-Skalierungsgruppe kann höchstens eine öffentliche IP pro IP-Konfiguration (private IP) erstellen.|
|/virtualMachineScaleSets/virtualMachines/networkInterfaces/ipConfigurations/read|Ruft die Eigenschaften einer oder aller IP-Konfigurationen einer Netzwerkschnittstelle ab, die mit der VM-Skalierungsgruppe erstellt wurde. IP-Konfigurationen stellen private IP-Adressen dar.|
|/virtualMachineScaleSets/virtualMachines/networkInterfaces/read|Ruft die Eigenschaften einer oder aller Netzwerkschnittstelle eines virtuellen Computers ab, der mithilfe der VM-Skalierungsgruppe erstellt wurde.|
|/virtualMachineScaleSets/virtualMachines/performMaintenance/action|Führt ungeplante Wartungsaufgaben für eine VM-Instanz in einer VM-Skalierungsgruppe durch.|
|/virtualMachineScaleSets/virtualMachines/powerOff/action|Schaltet eine Instanz eines virtuellen Computers in einer VM-Skalierungsgruppe aus.|
|/virtualMachineScaleSets/virtualMachines/providers/ Microsoft.Insights/metricDefinitions/read|Hiermit werden die Metrikdefinitionen virtueller Computer in Skalierungsgruppen gelesen.|
|/virtualMachineScaleSets/virtualMachines/read|Ruft die Eigenschaften eines virtuellen Computers in einer VM-Skalierungsgruppe ab.|
|/virtualMachineScaleSets/virtualMachines/redeploy/action|Hiermit wird eine erneute Bereitstellung für eine VM-Instanz in einer VM-Skalierungsgruppe durchgeführt.|
|/virtualMachineScaleSets/virtualMachines/reimage/action|Hiermit wird ein Reimaging für eine VM-Instanz in einer VM-Skalierungsgruppe durchgeführt.|
|/virtualMachineScaleSets/virtualMachines/restart/action|Startet eine Instanz eines virtuellen Computers in einer VM-Skalierungsgruppe neu.|
|/virtualMachineScaleSets/virtualMachines/start/action|Startet eine Instanz eines virtuellen Computers in einer VM-Skalierungsgruppe.|
|/virtualMachineScaleSets/virtualMachines/write|Aktualisiert die Eigenschaften eines virtuellen Computers in einer VM-Skalierungsgruppe.|
|/virtualMachineScaleSets/write|Erstellt eine neue VM-Skalierungsgruppe oder aktualisiert eine bereits vorhandene.|

## <a name="microsoftconsumption"></a>Microsoft.Consumption

| Vorgang | BESCHREIBUNG |
|---|---|
|/balances/read|Listet die Auslastungszusammenfassung für einen Abrechnungszeitraum für eine Verwaltungsgruppe auf.|
|/budgets/read|Liste die Budgets nach einem Abonnement oder einer Verwaltungsgruppe auf.|
|/budgets/write|Erstellt, aktualisiert und löscht die Budgets nach einem Abonnement oder einer Verwaltungsgruppe.|
|/marketplaces/read|Listet die Nutzungsdetails von Marketplace-Ressourcen für einen Bereich für EA- und WebDirect-Abonnements auf.|
|/operations/read|Listet alle vom Microsoft.Consumption-Ressourcenanbieter unterstützte Vorgänge auf.|
|/pricesheets/read|Listet die Daten zu Preisblättern nach einem Abonnement oder einer Verwaltungsgruppe auf.|
|/reservationDetails/read|Listet die Auslastungsdetails für reservierte Instanzen nach Reservierungsauftrag oder Verwaltungsgruppe auf. Die Detaildaten gelten pro Instanz und Tag.|
|/reservationSummaries/read|Listet die Auslastungszusammenfassung für reservierte Instanzen nach Reservierungsauftrag oder Verwaltungsgruppe auf. Die Zusammenfassungsdaten gelten entweder pro Monat oder Tag.|
|/reservationTransactions/read|Listet den Transaktionsverlauf für reservierte Instanzen nach Verwaltungsgruppe auf.|
|/terms/read|Liste die Bedingungen für ein Abonnement oder eine Verwaltungsgruppe auf.|
|/usageDetails/read|Listet die Nutzungsdetails für einen Bereich für EA- und WebDirect-Abonnements auf.|

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

| Vorgang | BESCHREIBUNG |
|---|---|
|/containerGroups/containers/logs/read|Hiermit rufen Sie Protokolle für einen bestimmten Container ab.|
|/containerGroups/delete|Hiermit löschen Sie eine bestimmte Containergruppe.|
|/containerGroups/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Containergruppe ab.|
|/containerGroups/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Containergruppe.|
|/containerGroups/providers/Microsoft.Insights/metricDefinitions/read|Ruft die verfügbaren Metriken für die Containergruppe ab.|
|/containerGroups/read|Hiermit rufen Sie alle Containergruppen ab.|
|/containerGroups/write|Hiermit erstellen oder aktualisieren Sie eine bestimmte Containergruppe.|

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

| Vorgang | BESCHREIBUNG |
|---|---|
|/checkNameAvailability/read|Überprüft, ob der Name der Containerregistrierung für die Verwendung verfügbar ist.|
|/locations/operationResults/read|Ruft das Ergebnis eines asynchronen Vorgangs ab.|
|/operations/read|Listet alle verfügbaren REST-API-Vorgänge von Azure Container Registry auf.|
|/register/action|Registriert das Abonnement für den Containerregistrierungs-Ressourcenanbieter und ermöglicht die Erstellung von Containerregistrierungen.|
|/registries/delete|Löscht eine Containerregistrierung.|
|/registries/eventGridFilters/delete|Löscht einen Event Grid-Filter aus einer Containerregistrierung.|
|/registries/eventGridFilters/read|Ruft die Eigenschaften des angegebenen Event Grid-Filters ab oder listet alle Event Grid-Filter für die angegebene Containerregistrierung auf.|
|/registries/eventGridFilters/write|Erstellt oder aktualisiert einen Event Grid-Filter für eine Containerregistrierung mit den angegebenen Parametern.|
|/registries/listCredentials/action|Listet die Anmeldeinformationen für die angegebene Containerregistrierung auf.|
|/registries/listUsages/read|Listet den Kontingentbedarf für die angegebene Containerregistrierung auf.|
|/registries/operationStatuses/read|Ruft den Status eines asynchronen Registrierungsvorgangs ab.|
|/registries/read|Ruft die Eigenschaften der angegebenen Containerregistrierung ab oder listet alle Containerregistrierungen unter der angegebenen Ressourcengruppe oder dem angegebenen Abonnement auf.|
|/registries/regenerateCredential/action|Generiert die Anmeldeinformationen für die angegebene Containerregistrierung neu.|
|/registries/replications/delete|Löscht eine Replikation aus einer Containerregistrierung.|
|/registries/replications/operationStatuses/read|Ruft den Status eines asynchronen Replikationsvorgangs ab.|
|/registries/replications/read|Ruft die Eigenschaften der angegebenen Replikation ab oder listet alle Replikationen für die angegebene Containerregistrierung auf.|
|/registries/replications/write|Erstellt oder aktualisiert eine Replikation für eine Containerregistrierung mit den angegebenen Parametern.|
|/registries/webhooks/delete|Löscht einen Webhook aus einer Containerregistrierung.|
|/registries/webhooks/getCallbackConfig/action|Ruft die Konfiguration des Dienst-URI und benutzerdefinierte Header für den Webhook ab.|
|/registries/webhooks/listEvents/action|Listet die aktuellen Ereignisse für den angegebenen Webhook auf.|
|/registries/webhooks/operationStatuses/read|Ruft den Status eines asynchronen Webhookvorgangs ab.|
|/registries/webhooks/ping/action|Löst ein Pingereignis aus, das an den Webhook gesendet werden muss.|
|/registries/webhooks/read|Ruft die Eigenschaften des angegebenen Webhooks ab oder listet alle Webhooks für die angegebene Containerregistrierung auf.|
|/registries/webhooks/write|Erstellt oder aktualisiert einen Webhook für eine Containerregistrierung mit den angegebenen Parametern.|
|/registries/write|Erstellt oder aktualisiert eine Containerregistrierung mit den angegebenen Parametern.|

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

| Vorgang | BESCHREIBUNG |
|---|---|
|/containerServices/delete|Löscht den angegebenen Containerdienst.|
|/containerServices/read|Ruft den angegebenen Containerdienst ab.|
|/containerServices/write|Legt den angegebenen Containerdienst fest oder aktualisiert ihn.|

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

| Vorgang | BESCHREIBUNG |
|---|---|
|/applications/delete|Löschvorgang|
|/applications/listSecrets/action|Dient zum Auflisten von Geheimnissen.|
|/applications/listSingleSignOnToken/action|Dient zum Lesen von Token für einmaliges Anmelden.|
|/applications/read|Lesevorgang|
|/applications/write|Schreibvorgang|
|/applications/write|Schreibvorgang|
|/listCommunicationPreference/action|Dient zum Auflisten der Kommunikationseinstellungen.|
|/operations/read|Dient zum Lesen von Vorgängen.|
|/updateCommunicationPreference/action|Dient zum Aktualisieren der Kommunikationseinstellungen.|

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

| Vorgang | BESCHREIBUNG |
|---|---|
|/hubs/adobemetadata/action|Dient zum Erstellen oder Aktualisieren von Adobe-Metadaten in Azure Customer Insights.|
|/hubs/adobemetadata/read|Dient zum Lesen von Adobe-Metadaten in Azure Customer Insights.|
|/hubs/authorizationPolicies/delete|Dient zum Löschen einer beliebigen SAS-Richtlinie für Azure Customer Insights.|
|/hubs/authorizationPolicies/read|Dient zum Lesen einer beliebigen SAS-Richtlinie für Azure Customer Insights.|
|/hubs/authorizationPolicies/regeneratePrimaryKey/action|Dient zum erneuten Generieren des primären Schlüssels einer SAS-Richtlinie für Azure Customer Insights.|
|/hubs/authorizationPolicies/regenerateSecondaryKey/action|Dient zum erneuten Generieren des sekundären Schlüssels einer SAS-Richtlinie für Azure Customer Insights.|
|/hubs/authorizationPolicies/write|Dient zum Erstellen oder Aktualisieren einer beliebigen SAS-Richtlinie für Azure Customer Insights.|
|/hubs/connectors/activate/action|Dient zum Aktivieren eines beliebigen Azure Customer Insights-Connectors.|
|/hubs/connectors/activate/action|Dient zum Aktivieren eines beliebigen Azure Customer Insights-Connectors.|
|/hubs/connectors/delete|Dient zum Löschen eines beliebigen Azure Customer Insights-Connectors.|
|/hubs/connectors/getruntimestatus/action|Dient zum Abrufen eines beliebigen Laufzeitstatus des Azure Customer Insights-Connectors.|
|/hubs/connectors/mappings/activate/action|Dient zum Aktivieren einer beliebigen Azure Customer Insights-Connectorzuordnung.|
|/hubs/connectors/mappings/delete|Dient zum Löschen einer beliebigen Azure Customer Insights-Connectorzuordnung.|
|/hubs/connectors/mappings/operations/read|Dient zum Lesen eines beliebigen Vorgangsergebnisses einer Azure Customer Insights-Connectorzuordnung.|
|/hubs/connectors/mappings/read|Dient zum Lesen einer beliebigen Azure Customer Insights-Connectorzuordnung.|
|/hubs/connectors/mappings/write|Dient zum Erstellen oder Aktualisieren einer beliebigen Azure Customer Insights-Connectorzuordnung.|
|/hubs/connectors/operations/read|Dient zum Lesen eines beliebigen Vorgangsergebnisses eines Azure Customer Insights-Connectors.|
|/hubs/connectors/read|Dient zum Lesen eines beliebigen Azure Customer Insights-Connectors.|
|/hubs/connectors/saveauthinfo/action|Dient zum Erstellen oder Aktualisieren von Informationen zur Authentifizierung eines beliebigen Azure Customer Insights-Connectors.|
|/hubs/connectors/update/action|Dient zum Aktualisieren eines beliebigen Azure Customer Insights-Connectors.|
|/hubs/connectors/write|Dient zum Erstellen oder Aktualisieren eines beliebigen Azure Customer Insights-Connectors.|
|/hubs/crmmetadata/action|Dient zum Erstellen oder Aktualisieren von CRM-Metadaten in Azure Customer Insights.|
|/hubs/crmmetadata/read|Dient zum Lesen von CRM-Metadaten in Azure Customer Insights.|
|/hubs/delete|Dient zum Löschen eines beliebigen Azure Customer Insights-Hubs.|
|/hubs/gdpr/delete|Dient zum Löschen einer beliebigen DSGVO für Azure Customer Insights.|
|/hubs/gdpr/read|Dient zum Lesen einer beliebigen Azure Customer Insights-DSGVO.|
|/hubs/gdpr/write|Dient zum Erstellen oder Aktualisieren einer beliebigen Azure Customer Insights-DSGVO.|
|/hubs/getbillingcredits/read|Dient zum Abrufen des Abrechnungsguthaben im Azure Customer Insights-Hub.|
|/hubs/getbillinghistory/read|Dient zum Abrufen des Abrechnungsverlaufs im Azure Customer Insights-Hub.|
|/hubs/images/delete|Dient zum Löschen eines beliebigen Azure Customer Insights-Image.|
|/hubs/images/read|Dient zum Lesen eines beliebigen Azure Customer Insights-Image.|
|/hubs/images/write|Dient zum Erstellen oder Aktualisieren eines beliebigen Azure Customer Insights-Image.|
|/hubs/interactions/delete|Dient zum Löschen beliebiger Azure Customer Insights-Interaktionen.|
|/hubs/interactions/operations/read|Dient zum Lesen eines beliebigen Vorgangsergebnisses von Azure Customer Insights-Interaktionen.|
|/hubs/interactions/read|Dient zum Lesen einer beliebigen Azure Customer Insights-Interaktion.|
|/hubs/interactions/suggestrelationshiplinks/action|Empfiehlt RelationShip-Links für beliebige Azure Insights Customer Insights-Interaktionen.|
|/hubs/interactions/write|Dient zum Erstellen oder Aktualisieren einer beliebigen Azure Customer Insights-Interaktion.|
|/hubs/kpi/delete|Dient zum Löschen eines beliebigen Key Performance Indicators für Azure Customer Insights.|
|/hubs/kpi/operations/read|Dient zum Lesen eines Vorgangsergebnisses für einen beliebigen Key Performance Indicator für Azure Customer Insights.|
|/hubs/kpi/read|Dient zum Lesen eines beliebigen Key Performance Indicators für Azure Customer Insights.|
|/hubs/kpi/reprocess/action|Dient zum erneuten Verarbeiten beliebiger Key Performance Indicators für Azure Customer Insights.|
|/hubs/kpi/write|Dient zum Erstellen oder Aktualisieren eines beliebigen Key Performance Indicators für Azure Customer Insights.|
|/hubs/links/delete|Dient zum Löschen beliebiger Azure Customer Insights-Verknüpfungen.|
|/hubs/links/operations/read|Dient zum Lesen eines beliebigen Vorgangsergebnisses für Azure Customer Insights-Verknüpfungen.|
|/hubs/links/read|Dient zum Lesen beliebiger Azure Customer Insights-Verknüpfungen.|
|/hubs/links/write|Dient zum Erstellen oder Aktualisieren beliebiger Azure Customer Insights-Verknüpfungen.|
|/hubs/msemetadata/action|Dient zum Erstellen oder Aktualisieren von MSE-Metadaten in Azure Customer Insights.|
|/hubs/msemetadata/read|Dient zum Lesen von MSE-Metadaten in Azure Customer Insights.|
|/hubs/operationresults/read|Dient zum Abrufen von Abrechnungsergebnissen im Azure Customer Insights-Hub.|
|/hubs/predictions/delete|Dient zum Löschen beliebiger Azure Customer Insights-Vorhersagen.|
|/hubs/predictions/operations/read|Dient zum Lesen eines beliebigen Vorgangsergebnisses für Azure Customer Insights-Vorhersagen.|
|/hubs/predictions/read|Dient zum Lesen beliebiger Azure Customer Insights-Vorhersagen.|
|/hubs/predictions/write|Dient zum Erstellen oder Aktualisieren beliebiger Azure Customer Insights-Vorhersagen.|
|/hubs/predictivematchpolicies/delete|Dient zum Löschen aller Azure Customer Insights-Richtlinien für den Vorhersageabgleich.|
|/hubs/predictivematchpolicies/operations/read|Dient zum Lesen des Vorgangsergebnisses aller Azure Customer Insights-Richtlinien für den Vorhersageabgleich.|
|/hubs/predictivematchpolicies/read|Dient zum Lesen aller Azure Customer Insights-Richtlinien für den Vorhersageabgleich.|
|/hubs/predictivematchpolicies/write|Dient zum Erstellen oder Aktualisieren aller Azure Customer Insights-Richtlinien für den Vorhersageabgleich.|
|/hubs/profiles/delete|Dient zum Löschen eines beliebigen Azure Customer Insights-Profils.|
|/hubs/profiles/operations/read|Dient zum Lesen eines beliebigen Vorgangsergebnisses für das Azure Customer Insights-Profil.|
|/hubs/profiles/read|Dient zum Lesen eines beliebigen Azure Customer Insights-Profils.|
|/hubs/profiles/write|Dient zum Schreiben eines beliebigen Azure Customer Insights-Profils.|
|/hubs/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/hubs/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/hubs/providers/Microsoft.Insights/logDefinitions/read|Ruft die verfügbaren Protokolle für die Ressource ab.|
|/hubs/providers/Microsoft.Insights/metricDefinitions/read|Ruft die verfügbaren Metriken für die Ressource ab.|
|/hubs/read|Dient zum Lesen eines beliebigen Azure Customer Insights-Hubs.|
|/hubs/relationshiplinks/delete|Dient zum Löschen beliebiger Azure Customer Insights-Beziehungsverknüpfungen.|
|/hubs/relationshiplinks/operations/read|Dient zum Lesen eines beliebigen Vorgangsergebnisses für Azure Customer Insights-Beziehungsverknüpfungen.|
|/hubs/relationshiplinks/read|Dient zum Lesen beliebiger Azure Customer Insights-Beziehungsverknüpfungen.|
|/hubs/relationshiplinks/write|Dient zum Erstellen oder Aktualisieren beliebiger Azure Customer Insights-Beziehungsverknüpfungen.|
|/hubs/relationships/delete|Dient zum Löschen beliebiger Azure Customer Insights-Beziehungen.|
|/hubs/relationships/operations/read|Dient zum Lesen eines beliebigen Vorgangsergebnisses für Azure Customer Insights-Beziehungen.|
|/hubs/relationships/read|Dient zum Lesen beliebiger Azure Customer Insights-Beziehungen.|
|/hubs/relationships/write|Dient zum Erstellen oder Aktualisieren beliebiger Azure Customer Insights-Beziehungen.|
|/hubs/roleAssignments/delete|Dient zum Löschen einer beliebigen Azure Customer Insights-RBAC-Zuweisung.|
|/hubs/roleAssignments/operations/read|Dient zum Lesen eines beliebigen Vorgangsergebnisses für eine Azure Customer Insights-RBAC-Zuweisung.|
|/hubs/roleAssignments/read|Dient zum Lesen einer beliebigen Azure Customer Insights-RBAC-Zuweisung.|
|/hubs/roleAssignments/write|Dient zum Erstellen oder Aktualisieren einer beliebigen Azure Customer Insights-RBAC-Zuweisung.|
|/hubs/roles/read|Dient zum Lesen beliebiger Azure Customer Insights-RBAC-Rollen.|
|/hubs/salesforcemetadata/action|Dient zum Erstellen oder Aktualisieren von Salesforce-Metadaten in Azure Customer Insights.|
|/hubs/salesforcemetadata/read|Dient zum Lesen von Salesforce-Metadaten in Azure Customer Insights.|
|/hubs/segments/delete|Dient zum Löschen beliebiger Azure Customer Insights-Segmente.|
|/hubs/segments/dynamic/action|Dient zur Verwaltung beliebiger Azure Customer Insights-Segmente.|
|/hubs/segments/read|Dient zum Lesen beliebiger Azure Customer Insights-Segmente.|
|/hubs/segments/static/action|Dient zur Verwaltung beliebiger statischer Azure Customer Insights-Segmente.|
|/hubs/segments/write|Dient zum Erstellen oder Aktualisieren beliebiger Azure Customer Insights-Segmente.|
|/hubs/sqlconnectionstrings/delete|Dient zum Löschen beliebiger SqlConnectionStrings-Elemente in Azure Customer Insights.|
|/hubs/sqlconnectionstrings/read|Dient zum Lesen beliebiger SqlConnectionStrings-Elemente in Azure Customer Insights.|
|/hubs/sqlconnectionstrings/write|Dient zum Erstellen oder Aktualisieren beliebiger SqlConnectionStrings-Elemente in Azure Customer Insights.|
|/hubs/suggesttypeschema/action|Generiert ein Empfehlungstypschema von Beispieldaten.|
|/hubs/tenantmanagement/read|Verwaltet alle Einstellungen für Azure Customer Insights-Hubs.|
|/hubs/views/delete|Dient zum Löschen einer beliebigen Azure Customer Insights-App-Ansicht.|
|/hubs/views/read|Dient zum Lesen einer beliebigen Azure Customer Insights-App-Ansicht.|
|/hubs/views/write|Dient zum Erstellen oder Aktualisieren einer beliebigen Azure Customer Insights-App-Ansicht.|
|/hubs/widgettypes/read|Liest Widgettypen der Azure Customer Insights-App.|
|/hubs/write|Dient zum Erstellen oder Aktualisieren eines beliebigen Azure Customer Insights-Hubs.|
|/operations/read|Liest Metadaten der Azure Customer Insights-API.|
|/register/action|Registriert das Abonnement für den Customer Insights-Ressourcenanbieter und ermöglicht die Erstellung von Customer Insights-Ressourcen.|
|/unregister/action|Hebt die Registrierung des Abonnements für den Customer Insights-Ressourcenanbieter auf.|

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

| Vorgang | BESCHREIBUNG |
|---|---|
|/catalogs/delete|Löscht den Katalog.|
|/catalogs/read|Dient zum Abrufen von Eigenschaften für einen Katalog oder für mehrere Kataloge unter einem Abonnement oder einer Ressourcengruppe.|
|/catalogs/write|Erstellt einen Katalog oder aktualisiert die Tags und Eigenschaften für den Katalog.|
|/checkNameAvailability/action|Prüft die Verfügbarkeit von Katalognamen für Mandanten.|
|/operations/read|Listet verfügbare Vorgänge für den Microsoft.DataCatalog-Ressourcenanbieter auf.|
|/register/action|Registriert ein Abonnement beim Microsoft.DataCatalog-Ressourcenanbieter.|
|/unregister/action|Hebt die Registrierung eines Abonnements beim Microsoft.DataCatalog-Ressourcenanbieter auf.|

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

| Vorgang | BESCHREIBUNG |
|---|---|
|/datafactories/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/datafactories/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/datafactories/providers/Microsoft.Insights/metricDefinitions/read|Ruft die verfügbaren Metriken für Data Factorys ab.|
|/factories/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/factories/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/factories/providers/Microsoft.Insights/logDefinitions/read|Ruft die verfügbaren Protokolle für Data Factorys ab.|
|/factories/providers/Microsoft.Insights/metricDefinitions/read|Ruft die verfügbaren Metriken für Data Factorys ab.|

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

| Vorgang | BESCHREIBUNG |
|---|---|
|/accounts/computePolicies/delete|Löscht eine Computerichtlinie.|
|/accounts/computePolicies/read|Ruft Informationen zu einer Computerichtlinie ab.|
|/accounts/computePolicies/write|Erstellt oder aktualisiert eine Computerichtlinie.|
|/accounts/dataLakeStoreAccounts/delete|Hebt die Verknüpfung eines DataLakeStore-Kontos mit einem DataLakeAnalytics-Konto auf.|
|/accounts/dataLakeStoreAccounts/read|Ruft Informationen zu einem verknüpften DataLakeStore-Konto eines DataLakeAnalytics-Kontos ab.|
|/accounts/dataLakeStoreAccounts/write|Erstellt oder aktualisiert ein verknüpftes DataLakeStore-Konto eines DataLakeAnalytics-Kontos.|
|/accounts/delete|Löscht ein DataLakeAnalytics-Konto.|
|/accounts/firewallRules/delete|Dient zum Löschen einer Firewallregel.|
|/accounts/firewallRules/read|Dient zum Abrufen von Informationen zu einer Firewallregel.|
|/accounts/firewallRules/write|Dient zum Erstellen oder Aktualisieren einer Firewallregel.|
|/accounts/operationResults/read|Ruft das Ergebnis eines Vorgangs des DataLakeAnalytics-Kontos ab.|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellungen für das DataLakeAnalytics-Konto ab.|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellungen für das DataLakeAnalytics-Konto.|
|/accounts/providers/Microsoft.Insights/logDefinitions/read|Ruft die verfügbaren Protokolle für das DataLakeAnalytics-Konto ab.|
|/accounts/providers/Microsoft.Insights/metricDefinitions/read|Ruft die verfügbaren Metriken für das DataLakeAnalytics-Konto ab.|
|/accounts/read|Ruft Informationen zu einem vorhandenen DataLakeAnalytics-Konto ab.|
|/accounts/storageAccounts/Containers/listSasTokens/action|Listet SAS-Tokens für Speichercontainer eines verknüpften Speicherkontos eines DataLakeAnalytics-Kontos auf.|
|/accounts/storageAccounts/Containers/read|Ruft Container eines verknüpften Speicherkontos eines DataLakeAnalytics-Kontos ab.|
|/accounts/storageAccounts/delete|Hebt die Verknüpfung eines Speicherkontos mit einem DataLakeAnalytics-Konto auf.|
|/accounts/storageAccounts/read|Ruft Informationen zu einem verknüpften Speicherkonto eines DataLakeAnalytics-Kontos ab.|
|/accounts/storageAccounts/write|Erstellt oder aktualisiert ein verknüpftes Speicherkonto eines DataLakeAnalytics-Kontos.|
|/accounts/TakeOwnership/action|Erteilt Berechtigungen zum Abbrechen von Aufträgen, die von anderen Benutzern übermittelt wurden.|
|/accounts/write|Erstellt oder aktualisiert ein DataLakeAnalytics-Konto.|
|/locations/capability/read|Ruft Funktionsinformationen eines Abonnements bezüglich der Verwendung von DataLakeAnalytics ab.|
|/locations/checkNameAvailability/action|Überprüft die Verfügbarkeit eines DataLakeAnalytics-Kontonamens.|
|/locations/operationResults/read|Ruft das Ergebnis eines Vorgangs des DataLakeAnalytics-Kontos ab.|
|/operations/read|Ruft verfügbare Vorgänge von DataLakeAnalytics ab.|
|/register/action|Registriert ein Abonnement für DataLakeAnalytics.|

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

| Vorgang | BESCHREIBUNG |
|---|---|
|/accounts/delete|Löscht ein DataLakeStore-Konto.|
|/accounts/enableKeyVault/action|Aktiviert KeyVault für ein DataLakeStore-Konto.|
|/accounts/firewallRules/delete|Dient zum Löschen einer Firewallregel.|
|/accounts/firewallRules/read|Dient zum Abrufen von Informationen zu einer Firewallregel.|
|/accounts/firewallRules/write|Dient zum Erstellen oder Aktualisieren einer Firewallregel.|
|/accounts/operationResults/read|Ruft das Ergebnis eines Vorgangs des DataLakeStore-Kontos ab.|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellungen für das DataLakeStore-Konto ab.|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellungen für das DataLakeStore-Konto.|
|/accounts/providers/Microsoft.Insights/logDefinitions/read|Ruft die verfügbaren Protokolle für das DataLakeStore-Konto ab.|
|/accounts/providers/Microsoft.Insights/metricDefinitions/read|Ruft die verfügbaren Metriken für das DataLakeStore-Konto ab.|
|/accounts/read|Ruft Informationen zu einem vorhandenen DataLakeStore-Konto ab.|
|/accounts/Superuser/action|Erteilt Superuser-Berechtigungen für Data Lake Store, sofern diese über „Microsoft.Authorization/roleAssignments/write“ erteilt werden.|
|/accounts/trustedIdProviders/delete|Dient zum Löschen eines vertrauenswürdigen Identitätsanbieters.|
|/accounts/trustedIdProviders/read|Dient zum Abrufen von Informationen zu einem vertrauenswürdigen Identitätsanbieter.|
|/accounts/trustedIdProviders/write|Dient zum Erstellen oder Aktualisieren eines vertrauenswürdigen Identitätsanbieters.|
|/accounts/write|Erstellt oder aktualisiert ein DataLakeStore-Konto.|
|/locations/capability/read|Ruft Funktionsinformationen eines Abonnements bezüglich der Verwendung von DataLakeStore ab.|
|/locations/checkNameAvailability/action|Überprüft die Verfügbarkeit eines DataLakeStore-Kontonamens.|
|/locations/operationResults/read|Ruft das Ergebnis eines Vorgangs des DataLakeStore-Kontos ab.|
|/operations/read|Ruft verfügbare Vorgänge von DataLakeStore ab.|
|/register/action|Registriert ein Abonnement für DataLakeStore.|

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

| Vorgang | BESCHREIBUNG |
|---|---|
|/locations/performanceTiers/read|Gibt die Liste der verfügbaren Leistungsstufen zurück.|
|/performanceTiers/read|Gibt die Liste der verfügbaren Leistungsstufen zurück.|
|/servers/delete|Löscht einen vorhandenen Server.|
|/servers/firewallRules/delete|Löscht eine vorhandene Firewallregel.|
|/servers/firewallRules/read|Gibt die Liste der Firewallregeln für einen Server zurück oder ruft die Eigenschaften für die angegebene Firewallregel ab.|
|/servers/firewallRules/write|Erstellt eine Firewallregel mit den angegebenen Parametern oder aktualisiert eine vorhandene Regel.|
|/servers/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/servers/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/servers/providers/Microsoft.Insights/metricDefinitions/read|Dient zum Zurückgeben verfügbarer Metriktypen für Datenbanken.|
|/servers/read|Gibt die Liste der Server zurück oder ruft die Eigenschaften für den angegebenen Server ab.|
|/servers/recoverableServers/read|Gibt die wiederherstellbaren MySQL Server-Informationen zurück.|
|/servers/virtualNetworkRules/delete|Löscht eine vorhandene Regel für virtuelle Netzwerke.|
|/servers/virtualNetworkRules/read|Gibt die Liste der Regeln für virtuelle Netzwerke zurück oder ruft die Eigenschaften für die angegebene Regel für virtuelle Netzwerke ab.|
|/servers/virtualNetworkRules/write|Erstellt eine Regel für virtuelle Netzwerke mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für die angegebene Regel für virtuelle Netzwerke.|
|/servers/write|Erstellt einen Server mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für den angegebenen Server.|

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

| Vorgang | BESCHREIBUNG |
|---|---|
|/locations/performanceTiers/read|Gibt die Liste der verfügbaren Leistungsstufen zurück.|
|/performanceTiers/read|Gibt die Liste der verfügbaren Leistungsstufen zurück.|
|/servers/delete|Löscht einen vorhandenen Server.|
|/servers/firewallRules/delete|Löscht eine vorhandene Firewallregel.|
|/servers/firewallRules/read|Gibt die Liste der Firewallregeln für einen Server zurück oder ruft die Eigenschaften für die angegebene Firewallregel ab.|
|/servers/firewallRules/write|Erstellt eine Firewallregel mit den angegebenen Parametern oder aktualisiert eine vorhandene Regel.|
|/servers/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/servers/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/servers/providers/Microsoft.Insights/logDefinitions/read|Dient zum Zurückgeben verfügbarer Protokolle für Datenbanken.|
|/servers/providers/Microsoft.Insights/metricDefinitions/read|Dient zum Zurückgeben verfügbarer Metriktypen für Datenbanken.|
|/servers/read|Gibt die Liste der Server zurück oder ruft die Eigenschaften für den angegebenen Server ab.|
|/servers/recoverableServers/read|Gibt die wiederherstellbaren PostgreSQL Server-Informationen zurück.|
|/servers/virtualNetworkRules/delete|Löscht eine vorhandene Regel für virtuelle Netzwerke.|
|/servers/virtualNetworkRules/read|Gibt die Liste der Regeln für virtuelle Netzwerke zurück oder ruft die Eigenschaften für die angegebene Regel für virtuelle Netzwerke ab.|
|/servers/virtualNetworkRules/write|Erstellt eine Regel für virtuelle Netzwerke mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für die angegebene Regel für virtuelle Netzwerke.|
|/servers/write|Erstellt einen Server mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für den angegebenen Server.|

## <a name="microsoftdevices"></a>Microsoft.Devices

| Vorgang | BESCHREIBUNG |
|---|---|
|/checkNameAvailability/Action|Dient zum Prüfen, ob ein IotHub-Name verfügbar ist.|
|/checkProvisioningServiceNameAvailability/Action|Dient zum Prüfen, ob ein IotHub-Name verfügbar ist.|
|/ElasticPools/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/ElasticPools/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/elasticPools/iotHubTenants/Delete|Löscht die IotHub-Mandantenressource.|
|/ElasticPools/IotHubTenants/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/ElasticPools/IotHubTenants/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Delete|Dient zum Löschen von EventHub-Consumergruppen.|
|/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Read|Dient zum Abrufen von EventHub-Consumergruppen.|
|/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Write|Dient zum Erstellen von EventHub-Consumergruppen.|
|/elasticPools/iotHubTenants/exportDevices/Action|Dient zum Exportieren von Geräten.|
|/elasticPools/iotHubTenants/getStats/Read|Ruft die IotHub-Mandantenstatistikressource ab.|
|/elasticPools/iotHubTenants/importDevices/Action|Dient zum Importieren von Geräten.|
|/elasticPools/iotHubTenants/iotHubKeys/listkeys/Action|Ruft den IotHub-Mandantenschlüssel ab.|
|/elasticPools/iotHubTenants/jobs/Read|Dient zum Abrufen von Auftragsdetails, die für eine bestimmte IotHub-Instanz übermittelt wurden.|
|/elasticPools/iotHubTenants/listKeys/Action|Ruft die IotHub-Mandantenschlüssel ab.|
|/ElasticPools/IotHubTenants/logDefinitions/read|Ruft die verfügbaren Protokolldefinitionen für den IotHub-Dienst ab.|
|/ElasticPools/IotHubTenants/metricDefinitions/read|Ruft die verfügbaren Metriken für den IotHub-Dienst ab.|
|/elasticPools/iotHubTenants/quotaMetrics/Read|Dient zum Abrufen von Kontingentmetriken.|
|/elasticPools/iotHubTenants/Read|Ruft die IotHub-Mandantenressource ab.|
|/elasticPools/iotHubTenants/routing/routes/$testall/Action|Dient zum Testen einer Nachricht mit allen vorhandenen Routen.|
|/elasticPools/iotHubTenants/routing/routes/$testnew/Action|Dient zum Testen einer Nachricht mit einer angegebenen Testroute.|
|/elasticPools/iotHubTenants/routingEndpointsHealth/Read|Ruft die Integrität aller Routingendpunkte für eine IotHub-Instanz ab.|
|/elasticPools/iotHubTenants/Write|Erstellt oder aktualisiert die IotHub-Mandantenressource.|
|/ElasticPools/metricDefinitions/read|Ruft die verfügbaren Metriken für den IotHub-Dienst ab.|
|/iotHubs/certificates/generateVerificationCode/Action|Generiert einen Prüfcode.|
|/iotHubs/certificates/verify/Action|Überprüft die Zertifikatsressource.|
|/iotHubs/Delete|Dient zum Löschen von IotHub-Ressourcen.|
|/IotHubs/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/IotHubs/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/iotHubs/eventGridFilters/Delete|Löscht den Event Grid-Filter.|
|/iotHubs/eventGridFilters/Read|Ruft den Event Grid-Filter ab.|
|/iotHubs/eventGridFilters/Write|Erstellt einen neuen Event Grid-Filter oder aktualisiert einen vorhandenen Event Grid-Filter.|
|/iotHubs/eventHubEndpoints/consumerGroups/Delete|Dient zum Löschen von EventHub-Consumergruppen.|
|/iotHubs/eventHubEndpoints/consumerGroups/Read|Dient zum Abrufen von EventHub-Consumergruppen.|
|/iotHubs/eventHubEndpoints/consumerGroups/Write|Dient zum Erstellen von EventHub-Consumergruppen.|
|/iotHubs/exportDevices/Action|Dient zum Exportieren von Geräten.|
|/iotHubs/importDevices/Action|Dient zum Importieren von Geräten.|
|/iotHubs/iotHubKeys/listkeys/Action|Dient zum Abrufen des IotHub-Schlüssels für den angegebenen Namen.|
|/iotHubs/iotHubStats/Read|Dient zum Abrufen der IotHub-Statistik.|
|/iotHubs/jobs/Read|Dient zum Abrufen von Auftragsdetails, die für eine bestimmte IotHub-Instanz übermittelt wurden.|
|/iotHubs/listkeys/Action|Dient zum Abrufen aller IotHub-Schlüssel.|
|/IotHubs/logDefinitions/read|Ruft die verfügbaren Protokolldefinitionen für den IotHub-Dienst ab.|
|/IotHubs/metricDefinitions/read|Ruft die verfügbaren Metriken für den IotHub-Dienst ab.|
|/iotHubs/quotaMetrics/Read|Dient zum Abrufen von Kontingentmetriken.|
|/iotHubs/Read|Ruft die IotHub-Ressourcen ab.|
|/iotHubs/routing/$testall/Action|Dient zum Testen einer Nachricht mit allen vorhandenen Routen.|
|/iotHubs/routing/$testnew/Action|Dient zum Testen einer Nachricht mit einer angegebenen Testroute.|
|/iotHubs/routingEndpointsHealth/Read|Ruft die Integrität aller Routingendpunkte für eine IotHub-Instanz ab.|
|/iotHubs/skus/Read|Dient zum Abrufen gültiger IotHub-SKUs.|
|/iotHubs/Write|Dient zum Erstellen oder Aktualisieren von IotHub-Ressourcen.|
|/operations/Read|Dient zum Abrufen aller ResourceProvider-Vorgänge.|
|/provisioningServices/certificates/generateVerificationCode/Action|Generiert einen Prüfcode.|
|/provisioningServices/certificates/verify/Action|Überprüft die Zertifikatsressource.|
|/provisioningServices/Delete|Löscht die IotDps-Ressource.|
|/provisioningServices/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/provisioningServices/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/provisioningServices/listkeys/Action|Ruft alle IotDps-Schlüssel ab.|
|/provisioningServices/logDefinitions/read|Ruft die verfügbaren Protokolldefinitionen für den Bereitstellungsdienst ab.|
|/provisioningServices/metricDefinitions/read|Ruft die verfügbaren Metriken für den Bereitstellungsdienst ab.|
|/provisioningServices/ProvisioningServiceKeys/listkeys/Action|Ruft IotDps-Schlüssel für Schlüsselnamen ab.|
|/provisioningServices/Read|Ruft die IotDps-Ressource ab.|
|/provisioningServices/skus/Read|Ruft gültige IotDps-SKUs ab.|
|/provisioningServices/Write|Erstellt die IotDps-Ressource.|
|/register/action|Dient zum Registrieren des Abonnements für den IotHub-Ressourcenanbieter und ermöglicht die Erstellung von IotHub-Ressourcen.|
|/register/action|Dient zum Registrieren des Abonnements für den IotHub-Ressourcenanbieter und ermöglicht die Erstellung von IotHub-Ressourcen.|
|/usages/Read|Dient zum Abrufen von Details zur Abonnementnutzung für diesen Anbieter.|

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

| Vorgang | BESCHREIBUNG |
|---|---|
|/labCenters/delete|Löscht die Lab-Center.|
|/labCenters/read|Liest die Lab-Center.|
|/labCenters/write|Dient zum Hinzufügen oder Ändern von Lab-Centern.|
|/labs/artifactSources/armTemplates/read|Dient zum Lesen von Azure Resource Manager-Vorlagen.|
|/labs/artifactSources/artifacts/GenerateArmTemplate/action|Generiert eine ARM-Vorlage für das angegebene Artefakt, lädt die erforderlichen Dateien in ein Speicherkonto hoch und überprüft das generierte Artefakt.|
|/labs/artifactSources/artifacts/read|Dient zum Lesen von Artefakten.|
|/labs/artifactSources/delete|Dient zum Löschen von Artefaktquellen.|
|/labs/artifactSources/read|Dient zum Lesen von Artefaktquellen.|
|/labs/artifactSources/write|Dient zum Hinzufügen oder Ändern von Artefaktquellen.|
|/labs/ClaimAnyVm/action|Dient zum Anfordern eines beliebigen anforderbaren virtuellen Computers im Lab.|
|/labs/costs/read|Dient zum Lesen von Kosten.|
|/labs/costs/write|Dient zum Hinzufügen oder Ändern von Kosten.|
|/labs/CreateEnvironment/action|Dient zum Erstellen von virtuellen Computern in einem Lab.|
|/labs/customImages/delete|Dient zum Löschen benutzerdefinierter Images.|
|/labs/customImages/read|Dient zum Lesen benutzerdefinierter Images.|
|/labs/customImages/write|Dient zum Hinzufügen oder Ändern benutzerdefinierter Images.|
|/labs/delete|Dient zum Löschen von Labs.|
|/labs/ExportResourceUsage/action|Exportiert die Labressourcenverwendung in ein Speicherkonto.|
|/labs/formulas/delete|Dient zum Löschen von Formeln.|
|/labs/formulas/read|Dient zum Lesen von Formeln.|
|/labs/formulas/write|Dient zum Hinzufügen oder Ändern von Formeln.|
|/labs/galleryImages/read|Dient zum Lesen von Katalogimages.|
|/labs/GenerateUploadUri/action|Dient zum Generieren eines URI für das Hochladen benutzerdefinierter Datenträgerimages in ein Lab.|
|/labs/ImportVirtualMachine/action|Importiert einen virtuellen Computer in ein anderes Lab.|
|/labs/ListVhds/action|Dient zum Auflisten von Datenträgerimages, die für die Erstellung benutzerdefinierter Images verfügbar sind.|
|/labs/notificationChannels/delete|Dient zum Löschen von Benachrichtigungskanälen.|
|/labs/notificationChannels/Notify/action|Dient zum Senden einer Benachrichtigung an einen angegebenen Kanal.|
|/labs/notificationChannels/read|Dient zum Lesen von Benachrichtigungskanälen.|
|/labs/notificationChannels/write|Dient zum Hinzufügen oder Ändern von Benachrichtigungskanälen.|
|/labs/policySets/EvaluatePolicies/action|Wertet Labrichtlinien aus.|
|/labs/policySets/policies/delete|Dient zum Löschen von Richtlinien.|
|/labs/policySets/policies/read|Dient zum Lesen von Richtlinien.|
|/labs/policySets/policies/write|Dient zum Hinzufügen oder Ändern von Richtlinien.|
|/labs/read|Dient zum Lesen von Labs.|
|/labs/schedules/delete|Dient zum Löschen von Zeitplänen.|
|/labs/schedules/Execute/action|Dient zum Ausführen eines Zeitplans.|
|/labs/schedules/ListApplicable/action|Listet alle anwendbaren Zeitpläne auf.|
|/labs/schedules/read|Dient zum Lesen von Zeitplänen.|
|/labs/schedules/write|Dient zum Hinzufügen oder Ändern von Zeitplänen.|
|/labs/serviceRunners/delete|Dient zum Löschen von Dienstausführern.|
|/labs/serviceRunners/read|Dient zum Lesen von Dienstausführern.|
|/labs/serviceRunners/write|Dient zum Hinzufügen oder Ändern von Dienstausführern.|
|/labs/users/delete|Dient zum Löschen von Benutzerprofilen.|
|/labs/users/disks/Attach/action|Dient zum Anfügen des Datenträgers an den virtuellen Computer sowie zum Erstellen der Datenträgerlease für den virtuellen Computer.|
|/labs/users/disks/delete|Dient zum Löschen von Datenträgern.|
|/labs/users/disks/Detach/action|Dient zum Trennen des an den virtuellen Computer angefügten Datenträgers sowie zum Beenden der Datenträgerlease für den virtuellen Computer.|
|/labs/users/disks/read|Dient zum Lesen von Datenträgern.|
|/labs/users/disks/write|Dient zum Hinzufügen oder Ändern von Datenträgern.|
|/labs/users/environments/delete|Dient zum Löschen von Umgebungen.|
|/labs/users/environments/read|Dient zum Lesen von Umgebungen.|
|/labs/users/environments/write|Dient zum Hinzufügen oder Ändern von Umgebungen.|
|/labs/users/read|Dient zum Lesen von Benutzerprofilen.|
|/labs/users/secrets/delete|Dient zum Löschen von Geheimnissen.|
|/labs/users/secrets/read|Dient zum Lesen von Geheimnissen.|
|/labs/users/secrets/write|Dient zum Hinzufügen oder Ändern von Geheimnissen.|
|/labs/users/serviceFabrics/delete|Löscht Service Fabrics.|
|/labs/users/serviceFabrics/ListApplicableSchedules/action|Listet alle anwendbaren Zeitpläne auf.|
|/labs/users/serviceFabrics/read|Liest Service Fabrics.|
|/labs/users/serviceFabrics/schedules/delete|Dient zum Löschen von Zeitplänen.|
|/labs/users/serviceFabrics/schedules/Execute/action|Dient zum Ausführen eines Zeitplans.|
|/labs/users/serviceFabrics/schedules/read|Dient zum Lesen von Zeitplänen.|
|/labs/users/serviceFabrics/schedules/write|Dient zum Hinzufügen oder Ändern von Zeitplänen.|
|/labs/users/serviceFabrics/Start/action|Startet ein Service Fabric.|
|/labs/users/serviceFabrics/Stop/action|Beendet ein Service Fabric.|
|/labs/users/serviceFabrics/write|Dient zum Hinzufügen oder Ändern von Service Fabrics.|
|/labs/users/write|Dient zum Hinzufügen oder Ändern von Benutzerprofilen.|
|/labs/virtualMachines/AddDataDisk/action|Dient zum Anfügen eines neuen oder vorhandenen Datenträgers an virtuelle Computer.|
|/labs/virtualMachines/ApplyArtifacts/action|Dient zum Anwenden von Artefakten auf virtuelle Computer.|
|/labs/virtualMachines/Claim/action|Dient dazu, einen vorhandenen virtuellen Computer in Besitz zu nehmen.|
|/labs/virtualMachines/delete|Dient zum Löschen virtueller Computer.|
|/labs/virtualMachines/DetachDataDisk/action|Dient zum Trennen des angegebenen Datenträgers vom virtuellen Computer.|
|/labs/virtualMachines/ListApplicableSchedules/action|Listet alle anwendbaren Zeitpläne auf.|
|/labs/virtualMachines/read|Dient zum Lesen virtueller Computer.|
|/labs/virtualMachines/Restart/action|Startet einen virtuellen Computer.|
|/labs/virtualMachines/schedules/delete|Dient zum Löschen von Zeitplänen.|
|/labs/virtualMachines/schedules/Execute/action|Dient zum Ausführen eines Zeitplans.|
|/labs/virtualMachines/schedules/read|Dient zum Lesen von Zeitplänen.|
|/labs/virtualMachines/schedules/write|Dient zum Hinzufügen oder Ändern von Zeitplänen.|
|/labs/virtualMachines/Start/action|Dient zum Starten eines virtuellen Computers.|
|/labs/virtualMachines/Stop/action|Dient zum Beenden eines virtuellen Computers.|
|/labs/virtualMachines/TransferDisks/action|Hiermit übertragen Sie den Besitz der VM-Datenträger an sich selbst.|
|/labs/virtualMachines/UnClaim/action|Hiermit geben Sie den Besitz eines vorhandenen virtuellen Computers frei.|
|/labs/virtualMachines/write|Dient zum Hinzufügen oder Ändern virtueller Computer.|
|/labs/virtualNetworks/delete|Dient zum Löschen virtueller Netzwerke.|
|/labs/virtualNetworks/read|Dient zum Lesen virtueller Netzwerke.|
|/labs/virtualNetworks/write|Dient zum Hinzufügen oder Ändern virtueller Netzwerke.|
|/labs/write|Dient zum Hinzufügen oder Ändern von Labs.|
|/locations/operations/read|Dient zum Lesen von Vorgängen.|
|/register/action|Registriert das Abonnement.|
|/schedules/delete|Dient zum Löschen von Zeitplänen.|
|/schedules/Execute/action|Dient zum Ausführen eines Zeitplans.|
|/schedules/read|Dient zum Lesen von Zeitplänen.|
|/schedules/Retarget/action|Aktualisiert die Zielressourcen-ID eines Zeitplans.|
|/schedules/write|Dient zum Hinzufügen oder Ändern von Zeitplänen.|

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

| Vorgang | BESCHREIBUNG |
|---|---|
|/databaseAccountNames/read|Prüft die Verfügbarkeit des Namens.|
|/databaseAccounts/changeResourceGroup/action|Dient zum Ändern der Ressourcengruppe eines Datenbankkontos.|
|/databaseAccounts/databases/collections/metricDefinitions/read|Liest die Sammlungsmetrikdefinitionen.|
|/databaseAccounts/databases/collections/metrics/read|Liest die Sammlungsmetriken.|
|/databaseAccounts/databases/collections/partitionKeyRangeId/metrics/read|Liest Metriken auf Partitionsschlüsselebene eines Datenbankkontos.|
|/databaseAccounts/databases/collections/partitions/metrics/read|Liest Metriken auf Partitionsebene eines Datenbankkontos.|
|/databaseAccounts/databases/collections/partitions/usages/read|Liest Verwendungen auf Partitionsebene eines Datenbankkontos.|
|/databaseAccounts/databases/collections/usages/read|Liest Informationen zur Sammlungsverwendung.|
|/databaseAccounts/databases/metricDefinitions/read|Liest die Datenbankmetrikdefinitionen.|
|/databaseAccounts/databases/metrics/read|Liest die Datenbankmetriken.|
|/databaseAccounts/databases/usages/read|Liest Informationen zur Datenbankverwendung.|
|/databaseAccounts/delete|Löscht die Datenbankkonten.|
|/databaseAccounts/failoverPriorityChange/action|Dient zum Ändern der Failoverprioritäten von Regionen eines Datenbankkontos. Wird für manuelle Failovervorgänge verwendet.|
|/databaseAccounts/listConnectionStrings/action|Dient zum Abrufen der Verbindungszeichenfolgen für ein Datenbankkonto.|
|/databaseAccounts/listKeys/action|Dient zum Auflisten von Schlüsseln eines Datenbankkontos.|
|/databaseAccounts/metricDefinitions/read|Liest die Metrikdefinitionen für Datenbankkonten.|
|/databaseAccounts/metrics/read|Liest die Datenbankkontometriken.|
|/databaseAccounts/operationResults/read|Liest den Status eines asynchronen Vorgangs.|
|/databaseAccounts/percentile/metrics/read|Liest Latenzmetriken.|
|/databaseAccounts/percentile/sourceRegion/targetRegion/metrics/read|Liest die Latenzmetriken für eine bestimmte Quell- und Zielregion.|
|/databaseAccounts/percentile/targetRegion/metrics/read|Liest die Latenzmetriken für eine bestimmte Zielregion.|
|/databaseAccounts/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/databaseAccounts/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/databaseAccounts/providers/Microsoft.Insights/logDefinitions/read|Ruft die verfügbaren Protokollkategorien für ein Datenbankkonto ab.|
|/databaseAccounts/providers/Microsoft.Insights/metricDefinitions/read|Ruft die verfügbaren Metriken für ein Datenbankkonto ab.|
|/databaseAccounts/read|Liest ein Datenbankkonto.|
|/databaseAccounts/readonlykeys/action|Liest die schreibgeschützten Schlüssel für Datenbankkonten.|
|/databaseAccounts/readonlykeys/read|Liest die schreibgeschützten Schlüssel für Datenbankkonten.|
|/databaseAccounts/regenerateKey/action|Dient zum Rotieren von Schlüsseln eines Datenbankkontos.|
|/databaseAccounts/region/databases/collections/metrics/read|Liest die Metriken von regionalen Collections.|
|/databaseAccounts/region/databases/collections/partitionKeyRangeId/metrics/read|Liest regionale Metriken auf Partitionsschlüsselebene eines Datenbankkontos.|
|/databaseAccounts/region/databases/collections/partitions/metrics/read|Liest regionale Metriken auf Partitionsebene eines Datenbankkontos.|
|/databaseAccounts/region/databases/collections/partitions/read|Liest die Partitionen eines Datenbankkontos in einer Collection.|
|/databaseAccounts/region/metrics/read|Liest die Region und die Datenbankkontometriken.|
|/databaseAccounts/usages/read|Liest Informationen zur Datenbankkontoverwendung.|
|/databaseAccounts/write|Dient zum Aktualisieren von Datenbankkonten.|
|/locations/deleteVirtualNetworkOrSubnets/action|Benachrichtigt Microsoft.DocumentDB darüber, dass das virtuelle Netzwerk oder Subnetz gelöscht wird.|
|/operationResults/read|Liest den Status eines asynchronen Vorgangs.|
|/operations/read|Liest verfügbare Vorgänge für Microsoft DocumentDB |
|/register/action| Dient zum Registrieren des Microsoft DocumentDB-Ressourcenanbieters für das Abonnement.|

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

| Vorgang | BESCHREIBUNG |
|---|---|
|/checkDomainAvailability/Action|Dient zum Prüfen, ob eine Domäne erworben werden kann.|
|/domains/Delete|Dient zum Löschen einer vorhandenen Domäne.|
|/domains/domainownershipidentifiers/Delete|Löscht die Besitz-ID.|
|/domains/domainownershipidentifiers/Read|Listet die Besitz-IDs auf.|
|/domains/domainownershipidentifiers/Read|Ruft die Besitz-ID ab.|
|/domains/domainownershipidentifiers/Write|Erstellt oder aktualisiert eine ID.|
|/domains/operationresults/Read|Dient zum Abrufen eines Domänenvorgangs.|
|/domains/operations/Read|Dient zum Auflisten aller Vorgänge über die App Service-Domänenregistrierung.|
|/domains/Read|Dient zum Abrufen der Domänenliste.|
|/domains/Read|Ruft die Domäne ab.|
|/domains/renew/Action|Dient zum Verlängern einer vorhandenen Domäne.|
|/domains/Write|Dient zum Hinzufügen einer neuen Domäne oder zum Aktualisieren einer bereits vorhandenen.|
|/generateSsoRequest/Action|Dient zum Generieren einer Anforderung für die Anmeldung beim Domänensteuerungszentrum.|
|/listDomainRecommendations/Action|Dient zum Abrufen der Liste mit Domänenempfehlungen auf der Grundlage von Schlüsselwörtern.|
|/register/action|Dient zum Registrieren des Microsoft-Domänenressourcenanbieters für das Abonnement.|
|/topLevelDomains/listAgreements/Action|Listet die Vereinbarungsaktion auf.|
|/topLevelDomains/Read|Ruft die Domänen der obersten Domäne ab.|
|/topLevelDomains/Read|Ruft die Domäne der obersten Domäne ab.|
|/validateDomainRegistrationInformation/Action|Dient zum Überprüfen eines Domänenerwerbsobjekts, ohne es zu übermitteln.|

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

| Vorgang | BESCHREIBUNG |
|---|---|
|/lcsprojects/clouddeployments/read|Dient zum Anzeigen von Microsoft Dynamics AX 2012 R3 Evaluation-Bereitstellungen in einem Microsoft Dynamics Lifecycle Services-Projekt, die zu einem Benutzer gehören.|
|/lcsprojects/clouddeployments/write|Dient zum Erstellen von Microsoft Dynamics AX 2012 R3 Evaluation-Bereitstellungen in einem Microsoft Dynamics Lifecycle Services-Projekt, die zu einem Benutzer gehören. Bereitstellungen können über das Azure-Verwaltungsportal verwaltet werden.|
|/lcsprojects/connectors/read|Dient zum Lesen von Connectors, die zu einem Microsoft Dynamics Lifecycle Services-Projekt gehören.|
|/lcsprojects/connectors/write|Dient zum Erstellen und Aktualisieren von Connectors, die zu einem Microsoft Dynamics Lifecycle Services-Projekt gehören.|
|/lcsprojects/delete|Dient zum Löschen von Microsoft Dynamics Lifecycle Services-Projekten, die zum Benutzer gehören.|
|/lcsprojects/read|Dient zum Anzeigen von Microsoft Dynamics Lifecycle Services-Projekten, die zu einem Benutzer gehören.|
|/lcsprojects/write|Dient zum Erstellen oder Aktualisieren von Microsoft Dynamics Lifecycle Services-Projekten, die zum Benutzer gehören. Nur die Eigenschaften für Name und Beschreibung können aktualisiert werden. Abonnement und Standort für das Projekt können nach der Erstellung nicht mehr aktualisiert werden.|

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

| Vorgang | BESCHREIBUNG |
|---|---|
|/eventSubscriptions/delete|Löscht eventSubscription.|
|/eventSubscriptions/getFullUrl/action|Ruft die vollständige URL für das Ereignisabonnement ab.|
|/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für Ereignisabonnements ab.|
|/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für Ereignisabonnements.|
|/eventSubscriptions/providers/Microsoft.Insights/metricDefinitions/read|Ruft die verfügbaren Metriken für eventSubscriptions ab.|
|/eventSubscriptions/read|Liest eventSubscription.|
|/eventSubscriptions/write|Erstellt oder aktualisiert eventSubscription.|
|/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Themen ab.|
|/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für Themen.|
|/extensionTopics/providers/Microsoft.Insights/metricDefinitions/read|Ruft die verfügbaren Metriken für Themen ab.|
|/register/action|Registriert das eventSubscription für den EventGrid-Ressourcenanbieter und ermöglicht die Erstellung von Event Grid-Ressourcen.|
|/topics/delete|Löschen eines Themas|
|/topics/listKeys/action|Listet Schlüssel für das Thema auf.|
|/topics/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Themen ab.|
|/topics/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für Themen.|
|/topics/providers/Microsoft.Insights/metricDefinitions/read|Ruft die verfügbaren Metriken für Themen ab.|
|/topics/read|Liest ein Thema.|
|/topics/regenerateKey/action|Generiert erneut einen Schlüssel für das Thema.|
|/topics/write|Erstellt oder aktualisiert ein Thema.|

## <a name="microsofteventhub"></a>Microsoft.EventHub

| Vorgang | BESCHREIBUNG |
|---|---|
|/checkNameAvailability/action|Prüft die Verfügbarkeit des Namespaces unter dem angegebenen Abonnement.|
|/checkNamespaceAvailability/action|Prüft die Verfügbarkeit des Namespaces unter dem angegebenen Abonnement. Diese API ist veraltet. Verwenden Sie stattdessen CheckNameAvailabiltiy.|
|/namespaces/authorizationRules/action|Dient zum Aktualisieren von Namespace-Autorisierungsregeln. Diese API ist veraltet. Verwenden Sie stattdessen einen PUT-Aufruf, um die Namespace-Autorisierungsregel zu aktualisieren. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt.|
|/namespaces/authorizationRules/delete|Dient zum Löschen von Namespace-Autorisierungsregeln. Die standardmäßige Namespace-Autorisierungsregel kann nicht gelöscht werden. |
|/namespaces/authorizationRules/listkeys/action|Dient zum Abrufen der Verbindungszeichenfolge für den Namespace.|
|/namespaces/authorizationRules/read|Dient zum Abrufen der Liste mit Beschreibungen für Namespace-Autorisierungsregeln.|
|/namespaces/authorizationRules/regenerateKeys/action|Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource.|
|/namespaces/authorizationRules/write|Dient zum Erstellen von Autorisierungsregeln auf Namespaceebene und zum Aktualisieren der dazugehörigen Eigenschaften. Aktualisiert werden können die Zugriffsrechte für Autorisierungsregeln sowie der primäre und der sekundäre Schlüssel.|
|/namespaces/Delete|Dient zum Löschen von Namespaceressourcen.|
|/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action|Ruft die Schlüssel der Autorisierungsregeln für den primären Namespace für die Notfallwiederherstellung.|
|/namespaces/disasterRecoveryConfigs/authorizationRules/read|Ruft die Autorisierungsregeln des primären Namespace für die Notfallwiederherstellung ab.|
|/namespaces/disasterRecoveryConfigs/breakPairing/action|Hiermit wird die Notfallwiederherstellung deaktiviert und die Replikation von Änderungen vom primären Namespace zu sekundären Namespaces beendet.|
|/namespaces/disasterrecoveryconfigs/checkNameAvailability/action|Prüft die Verfügbarkeit des Namespacealias unter dem angegebenen Abonnement.|
|/namespaces/disasterRecoveryConfigs/delete|Hiermit wird die dem Namespace zugeordnete Notfallwiederherstellungskonfiguration gelöscht. Dieser Vorgang kann nur über den primären Namespace aufgerufen werden.|
|/namespaces/disasterRecoveryConfigs/failover/action|Hiermit wird ein Failover bei der georedundanten Notfallwiederherstellung ausgelöst und der Namespacealias neu konfiguriert, um auf den sekundären Namespace zu zeigen.|
|/namespaces/disasterRecoveryConfigs/read|Hiermit wird die dem Namespace zugeordnete Notfallwiederherstellungskonfiguration abgerufen.|
|/namespaces/disasterRecoveryConfigs/write|Hiermit wird die dem Namespace zugeordnete Notfallwiederherstellungskonfiguration erstellt oder aktualisiert.|
|/namespaces/eventhubs/authorizationRules/action|Vorgang zum Aktualisieren von EventHub. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. Autorisierungsregeln. Verwenden Sie einen PUT-Aufruf, um die Autorisierungsregel zu aktualisieren.|
|/namespaces/eventhubs/authorizationRules/delete|Vorgang zum Löschen von EventHub-Autorisierungsregeln.|
|/namespaces/eventhubs/authorizationRules/listkeys/action|Dient zum Abrufen der EventHub-Verbindungszeichenfolge.|
|/namespaces/eventhubs/authorizationRules/read| Dient zum Abrufen der Liste mit EventHub-Autorisierungsregeln.|
|/namespaces/eventhubs/authorizationRules/regenerateKeys/action|Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource.|
|/namespaces/eventhubs/authorizationRules/write|Dient zum Erstellen von EventHub-Autorisierungsregeln und zum Aktualisieren der dazugehörigen Eigenschaften. Die Zugriffsrechte für Autorisierungsregeln können aktualisiert werden.|
|/namespaces/eventHubs/consumergroups/Delete|Vorgang zum Löschen von ConsumerGroup-Ressourcen.|
|/namespaces/eventHubs/consumergroups/read|Dient zum Abrufen einer Liste mit ConsumerGroup-Ressourcenbeschreibungen.|
|/namespaces/eventHubs/consumergroups/write|Dient zum Erstellen oder Aktualisieren von ConsumerGroup-Eigenschaften.|
|/namespaces/eventhubs/Delete|Vorgang zum Löschen von EventHub-Ressourcen.|
|/namespaces/eventhubs/read|Dient zum Abrufen einer Liste mit EventHub-Ressourcenbeschreibungen.|
|/namespaces/eventhubs/write|Dient zum Erstellen oder Aktualisieren von EventHub-Eigenschaften.|
|/namespaces/messagingPlan/read|Hiermit wird der Messagingplan für einen Namespace abgerufen. Diese API ist veraltet. Über die MessagingPlan-Ressource verfügbar gemachte Eigenschaften werden in höheren API-Versionen in die (übergeordnete) Namespaceressource verschoben. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt.|
|/namespaces/messagingPlan/write|Hiermit wird der Messagingplan für einen Namespace aktualisiert. Diese API ist veraltet. Über die MessagingPlan-Ressource verfügbar gemachte Eigenschaften werden in höheren API-Versionen in die (übergeordnete) Namespaceressource verschoben. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt.|
|/namespaces/operationresults/read|Hiermit wird der Status des Namespacevorgangs abgerufen.|
|/namespaces/providers/Microsoft.Insights/diagnosticSettings/read|Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacediagnoseeinstellungen.|
|/namespaces/providers/Microsoft.Insights/diagnosticSettings/write|Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacediagnoseeinstellungen.|
|/namespaces/providers/Microsoft.Insights/logDefinitions/read|Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespaceprotokolle.|
|/namespaces/providers/Microsoft.Insights/metricDefinitions/read|Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacemetriken.|
|/namespaces/read|Dient zum Abrufen der Liste mit Beschreibungen für Namespaceressourcen.|
|/namespaces/write|Dient zum Erstellen einer Namespaceressource und zum Aktualisieren ihrer Eigenschaften. Die Eigenschaften für Markierungen und Kapazität des Namespace können aktualisiert werden.|
|/operations/read|Dient zum Abrufen von Vorgängen.|
|/register/action|Registriert das Abonnement für den EventHub-Ressourcenanbieter und ermöglicht die Erstellung von EventHub-Ressourcen.|
|/sku/read|Dient zum Abrufen einer Liste mit SKU-Ressourcenbeschreibungen.|
|/sku/regions/read|Dient zum Abrufen einer Liste mit SkuRegions-Ressourcenbeschreibungen.|
|/unregister/action|Registriert den EventHub-Ressourcenanbieter.|

## <a name="microsoftfeatures"></a>Microsoft.Features

| Vorgang | BESCHREIBUNG |
|---|---|
|/features/read|Ruft die Features eines Abonnements ab.|
|/providers/features/read|Ruft das Feature eines Abonnements in einem angegebenen Ressourcenanbieter ab.|
|/providers/features/register/action|Registriert das Feature für ein Abonnement in einem angegebenen Ressourcenanbieter.|
|/providers/features/unregister/action|Hiermit wird die Registrierung des Features für ein Abonnement in einem vorgegebenen Ressourcenanbieter aufgehoben.|

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

| Vorgang | BESCHREIBUNG |
|---|---|
|/clusters/changerdpsetting/action|Dient zum Ändern der RDP-Einstellung für HDInsight-Cluster.|
|/clusters/configurations/action|Dient zum Aktualisieren der Konfiguration von HDInsight-Clustern.|
|/clusters/configurations/read|Dient zum Abrufen der Konfigurationen von HDInsight-Clustern.|
|/clusters/delete|Dient zum Löschen eines HDInsight-Clusters.|
|/clusters/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für den HDInsight-Cluster der Ressource ab.|
|/clusters/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für den HDInsight-Cluster der Ressource.|
|/clusters/providers/Microsoft.Insights/metricDefinitions/read|Ruft die verfügbaren Metriken für den HDInsight-Cluster ab.|
|/clusters/read|Dient zum Abrufen von Details zu HDInsight-Clustern.|
|/clusters/roles/resize/action|Dient zum Anpassen der Größe eines HDInsight-Clusters.|
|/clusters/write|Dient zum Erstellen oder Aktualisieren von HDInsight-Clustern.|
|/locations/capabilities/read|Dient zum Abrufen von Abonnementfunktionen.|
|/locations/checkNameAvailability/read|Dient zum Prüfen der Verfügbarkeit des Namens.|

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

| Vorgang | BESCHREIBUNG |
|---|---|
|/jobs/delete|Löscht einen vorhandenen Auftrag.|
|/jobs/listBitLockerKeys/action|Ruft die BitLocker-Schlüssel für den angegebenen Auftrag ab.|
|/jobs/read|Ruft die Eigenschaften für den angegebenen Auftrag ab oder gibt die Auftragsliste zurück.|
|/jobs/write|Erstellt einen Auftrag mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Tags für den angegebenen Auftrag.|
|/locations/read|Ruft die Eigenschaften für den angegebenen Standort ab oder gibt die Standortliste zurück.|
|/register/action|Registriert das Abonnement für den Import/Export-Ressourcenanbieter und ermöglicht die Erstellung von Import/Exportaufträgen.|

## <a name="microsoftinsights"></a>Microsoft.Insights

| Vorgang | BESCHREIBUNG |
|---|---|
|/ActionGroups/Delete|Hiermit wird eine Aktionsgruppe gelöscht.|
|/ActionGroups/Read|Hiermit wird eine Aktionsgruppe gelesen.|
|/ActionGroups/Write|Hiermit wird eine Aktionsgruppe geschrieben.|
|/ActivityLogAlerts/Activated/Action|Aktivitätsprotokollwarnung ausgelöst|
|/ActivityLogAlerts/Delete|Hiermit wird eine Aktivitätsprotokollwarnung gelöscht.|
|/ActivityLogAlerts/Read|Hiermit wird eine Aktivitätsprotokollwarnung gelesen.|
|/ActivityLogAlerts/Write|Hiermit wird eine Aktivitätsprotokollwarnung gelesen.|
|/AlertRules/Activated/Action|Warnungsregel aktiviert|
|/AlertRules/Delete|Dient zum Löschen einer Warnungsregelkonfiguration.|
|/AlertRules/Incidents/Read|Dient zum Lesen einer Warnungsregel-Incidentkonfiguration.|
|/AlertRules/Read|Dient zum Lesen einer Warnungsregelkonfiguration.|
|/AlertRules/Resolved/Action|Warnungsregel aufgelöst|
|/AlertRules/Throttled/Action|Warnungsregel gedrosselt|
|/AlertRules/Write|Dient zum Schreiben in eine Warnungsregelkonfiguration.|
|/AutoscaleSettings/Delete|Dient zum Löschen einer Autoskalierungseinstellungskonfiguration.|
|/AutoscaleSettings/providers/Microsoft.Insights/MetricDefinitions/Read|Dient zum Lesen von Metrikdefinitionen.|
|/AutoscaleSettings/Read|Dient zum Lesen einer Autoskalierungseinstellungskonfiguration.|
|/AutoscaleSettings/Scaledown/Action|Automatisches zentrales Herunterskalieren|
|/AutoscaleSettings/Scaleup/Action|Automatisches zentrales Hochskalieren|
|/AutoscaleSettings/Write|Dient zum Schreiben in eine Autoskalierungseinstellungskonfiguration.|
|/Components/AnalyticsItems/Delete|Hiermit wird ein Application Insights-Analyseelement gelöscht.|
|/Components/AnalyticsItems/Read|Hiermit wird ein Application Insights-Analyseelement gelesen.|
|/Components/AnalyticsItems/Write|Hiermit wird ein Application Insights-Analyseelement geschrieben.|
|/Components/AnalyticsTables/Action|Application Insights-Analysetabellenaktion|
|/Components/AnalyticsTables/Delete|Hiermit wird ein Application Insights-Analysetabellenschema gelöscht.|
|/Components/AnalyticsTables/Read|Hiermit wird ein Application Insights-Analysetabellenschema gelesen.|
|/Components/AnalyticsTables/Write|Hiermit wird ein Application Insights-Analysetabellenschema geschrieben.|
|/Components/Annotations/Delete|Hiermit wird eine Application Insights-Anmerkung gelöscht.|
|/Components/Annotations/Read|Hiermit wird eine Application Insights-Anmerkung gelesen.|
|/Components/Annotations/Write|Hiermit wird eine Application Insights-Anmerkung geschrieben.|
|/Components/Api/Read|Hiermit wird die Daten-API für Application Insights-Komponenten gelesen.|
|/Components/ApiKeys/Action|Hiermit wird ein Application Insights-API-Schlüssel generiert.|
|/Components/ApiKeys/Delete|Hiermit wird ein Application Insights-API-Schlüssel gelöscht.|
|/Components/ApiKeys/Read|Hiermit wird ein Application Insights-API-Schlüssel gelesen.|
|/Components/BillingPlanForComponent/Read|Hiermit wird ein Abrechnungsplan für eine Application Insights-Komponente gelesen.|
|/Components/CurrentBillingFeatures/Read|Hiermit werden die aktuellen Abrechnungsfeatures für eine Application Insights-Komponente gelesen.|
|/Components/CurrentBillingFeatures/Write|Hiermit werden die aktuellen Abrechnungsfeatures für eine Application Insights-Komponente geschrieben.|
|/Components/DefaultWorkItemConfig/Read|Hiermit wird die standardmäßige ALM-Integrationskonfiguration von Application Insights gelesen.|
|/Components/Delete|Hiermit wird die Konfiguration der Application Insights-Komponente gelöscht.|
|/Components/ExportConfiguration/Action|Application Insights-Exporteinstellungsaktion|
|/Components/ExportConfiguration/Delete|Hiermit werden die Application Insights-Exporteinstellungen gelöscht.|
|/Components/ExportConfiguration/Read|Hiermit werden die Application Insights-Exporteinstellungen gelesen.|
|/Components/ExportConfiguration/Write|Hiermit werden die Application Insights-Exporteinstellungen geschrieben.|
|/Components/ExtendQueries/Read|Hiermit werden die erweiterten Abfrageergebnisse der Application Insights-Komponenten gelesen.|
|/Components/Favorites/Delete|Hiermit wird ein Application Insights-Favorit gelöscht.|
|/Components/Favorites/Read|Hiermit wird ein Application Insights-Favorit gelesen.|
|/Components/Favorites/Write|Hiermit wird ein Application Insights-Favorit geschrieben.|
|/Components/FeatureCapabilities/Read|Hiermit werden die Features der Application Insights-Komponente gelesen.|
|/Components/GetAvailableBillingFeatures/Read|Hiermit werden die verfügbaren Abrechnungsfeatures für eine Application Insights-Komponente gelesen.|
|/Components/GetToken/Read|Hiermit wird ein Token einer Application Insights-Komponente gelesen.|
|/Components/ListMigrationDate/Action|Hiermit wird das Migrationsdatum für das Abonnement zurückgegeben.|
|/Components/ListMigrationDate/Read|Hiermit wird das Migrationsdatum für das Abonnement zurückgegeben.|
|/Components/MetricDefinitions/Read|Hiermit werden die Metrikdefinitionen für Application Insights-Komponenten gelesen.|
|/Components/Metrics/Read|Hiermit werden die Metriken für Application Insights-Komponenten gelesen.|
|/Components/MigrateToNewpricingModel/Action|Hiermit wird ein Abonnement auf das neue Preismodell migriert.|
|/Components/Move/Action|Hiermit wird eine Application Insights-Komponente auf eine andere Ressourcengruppe oder ein anderes Abonnement migriert.|
|/Components/MyAnalyticsItems/Delete|Hiermit wird ein persönliches Application Insights-Analyseelement gelöscht.|
|/Components/MyAnalyticsItems/Read|Hiermit wird ein persönliches Application Insights-Analyseelement gelesen.|
|/Components/MyAnalyticsItems/Write|Hiermit wird ein persönliches Application Insights-Analyseelement geschrieben.|
|/Components/MyFavorites/Read|Hiermit wird ein persönlicher Application Insights-Favorit gelesen.|
|/Components/PricingPlans/Read|Hiermit wird ein Tarif für eine Application Insights-Komponente gelesen.|
|/Components/PricingPlans/Write|Hiermit wird ein Tarif für eine Application Insights-Komponente geschrieben.|
|/Components/ProactiveDetectionConfigs/Read|Hiermit wird die Konfiguration von Proactive Detection in Application Insights gelesen.|
|/Components/ProactiveDetectionConfigs/Write|Hiermit wird die Konfiguration von Proactive Detection in Application Insights geschrieben.|
|/Components/providers/Microsoft.Insights/MetricDefinitions/Read|Dient zum Lesen von Metrikdefinitionen.|
|/Components/QuotaStatus/Read|Hiermit wird der Kontingentstatus der Application Insights-Komponente gelesen.|
|/Components/Read|Hiermit wird die Konfiguration der Application Insights-Komponente gelesen.|
|/Components/RollbackToLegacyPricingModel/Action|Hiermit wird ein Rollback für das Abonnement auf das Legacypreismodell durchgeführt.|
|/Components/SyntheticMonitorLocations/Read|Hiermit werden die Webteststandorte von Application Insights gelesen.|
|/Components/WorkItemConfigs/Delete|Hiermit wird die standardmäßige ALM-Integrationskonfiguration von Application Insights gelöscht.|
|/Components/WorkItemConfigs/Read|Hiermit wird die ALM-Integrationskonfiguration von Application Insights gelesen.|
|/Components/WorkItemConfigs/Write|Hiermit wird die ALM-Integrationskonfiguration von Application Insights geschrieben.|
|/Components/Write|Hiermit wird in eine Application Insights-Komponentenkonfiguration geschrieben.|
|/DiagnosticSettings/Delete|Dient zum Löschen der Diagnoseeinstellungskonfiguration.|
|/DiagnosticSettings/Read|Dient zum Lesen einer Diagnoseeinstellungskonfiguration.|
|/DiagnosticSettings/Write|Dient zum Schreiben in die Diagnoseeinstellungskonfiguration.|
|/EventCategories/Read|Hiermit wird eine Ereigniskategorie gelesen.|
|/eventtypes/digestevents/Read|Dient zum Lesen des Verwaltungsereignistyp-Digests.|
|/eventtypes/values/Read|Dient zum Lesen von Verwaltungsereignistyp-Werten.|
|/ExtendedDiagnosticSettings/Delete|Dient zum Löschen der erweiterten Diagnoseeinstellungskonfiguration.|
|/ExtendedDiagnosticSettings/Read|Dient zum Lesen einer erweiterten Diagnoseeinstellungskonfiguration.|
|/ExtendedDiagnosticSettings/Write|Dient zum Schreiben in die erweiterte Diagnoseeinstellungskonfiguration.|
|/LogDefinitions/Read|Dient zum Lesen von Protokolldefinitionen.|
|/LogProfiles/Delete|Dient zum Löschen von Protokollprofilkonfigurationen.|
|/LogProfiles/Read|Dient zum Lesen von Protokollprofilen.|
|/LogProfiles/Write|Dient zum Schreiben in eine Protokollprofilkonfiguration.|
|/MetricAlerts/Delete|Hiermit wird eine Metrikwarnung gelöscht.|
|/MetricAlerts/Read|Hiermit wird eine Metrikwarnung gelesen.|
|/MetricAlerts/Write|Hiermit wird eine Metrikwarnung geschrieben.|
|/MetricDefinitions/Microsoft.Insights/Read|Dient zum Lesen von Metrikdefinitionen.|
|/MetricDefinitions/providers/Microsoft.Insights/Read|Dient zum Lesen von Metrikdefinitionen.|
|/MetricDefinitions/Read|Dient zum Lesen von Metrikdefinitionen.|
|/Metrics/providers/Metrics/Read|Dient zum Lesen von Metriken.|
|/Metrics/Read|Dient zum Lesen von Metriken.|
|/Metrics/Write|Hiermit werden Metriken geschrieben.|
|/Operations/Read|Hiermit werden Vorgänge gelesen.|
|/Register/Action|Dient zum Registrieren des Microsoft Insights-Anbieters.|
|/Tenants/Register/Action|Hiermit wird der Microsoft Insights-Anbieter initialisiert.|
|/Unregister/Action|Dient zum Registrieren des Microsoft Insights-Anbieters.|
|/Webtests/Delete|Hiermit wird eine Webtestkonfiguration gelöscht.|
|/Webtests/GetToken/Read|Hiermit wird ein Webtesttoken gelesen.|
|/Webtests/MetricDefinitions/Read|Hiermit werden Webtestmetrikdefinitionen gelesen.|
|/Webtests/Metrics/Read|Hiermit werden Webtestmetriken gelesen.|
|/Webtests/Read|Hiermit wird eine Webtestkonfiguration gelesen.|
|/Webtests/Write|Hiermit wird in eine Webtestkonfiguration geschrieben.|

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

| Vorgang | BESCHREIBUNG |
|---|---|
|/checkNameAvailability/read|Prüft, ob ein Schlüsseltresorname gültig ist und noch nicht verwendet wird.|
|/deletedVaults/read|Dient zum Anzeigen der Eigenschaften vorläufig gelöschter Schlüsseltresore.|
|/hsmPools/delete|Hiermit wird ein HSM-Pool gelöscht.|
|/hsmPools/joinVault/action|Hiermit wird ein Schlüsseltresor zu einem HSM-Pool hinzugefügt.|
|/hsmPools/read|Hiermit werden die Eigenschaften eines HSM-Pools angezeigt.|
|/hsmPools/write|Hiermit wird durch das Aktualisieren der Eigenschaften eines vorhandenen HSM-Pools ein neuer HSM-Pool erstellt.|
|/locations/deletedVaults/purge/action|Dient zum endgültigen Löschen eines vorläufig gelöschten Schlüsseltresors.|
|/locations/deletedVaults/read|Dient zum Anzeigen der Eigenschaften eines vorläufig gelöschten Schlüsseltresors.|
|/locations/deleteVirtualNetworkOrSubnets/action|Hiermit wird Microsoft.KeyVault darüber benachrichtigt, dass ein virtuelles Netzwerk oder Subnetz gelöscht wird.|
|/locations/operationResults/read|Dient zum Überprüfen des Ergebnisses eines Vorgangs mit langer Ausführungszeit.|
|/operations/read|Listet die verfügbaren Vorgänge für den Microsoft.KeyVault-Ressourcenanbieter auf.|
|/register/action|Registriert ein Abonnement.|
|/unregister/action|Hiermit wird die Registrierung für ein Abonnement aufgehoben.|
|/vaults/accessPolicies/write|Dient zum Aktualisieren einer vorhandenen Zugriffsrichtlinie durch Zusammenführen oder Ersetzen oder zum Hinzufügen einer neuen Zugriffsrichtlinie zu einem Tresor.|
|/vaults/delete|Dient zum Löschen eines Schlüsseltresors.|
|/vaults/deploy/action|Ermöglicht den Zugriff auf Geheimnisse in einem Schlüsseltresor beim Bereitstellen von Azure-Ressourcen.|
|/vaults/providers/Microsoft.Insights/diagnosticSettings/Read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/vaults/providers/Microsoft.Insights/diagnosticSettings/Write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/vaults/providers/Microsoft.Insights/logDefinitions/read|Hiermit werden die verfügbaren Protokolle für einen Schlüsseltresor abgerufen.|
|/vaults/providers/Microsoft.Insights/metricDefinitions/read|Hiermit werden die verfügbaren Metriken für einen Schlüsseltresor abgerufen.|
|/vaults/read|Dient zum Anzeigen der Eigenschaften eines Schlüsseltresors.|
|/vaults/secrets/read|Dient zum Anzeigen der Eigenschaften (aber nicht des Werts) eines Geheimnisses.|
|/vaults/secrets/write|Dient zum Erstellen eines neuen Geheimnisses oder zum Aktualisieren des Werts eines vorhandenen Geheimnisses.|
|/vaults/write|Dient zum Erstellen eines neuen Schlüsseltresors oder zum Aktualisieren der Eigenschaften einer vorhandenen Schlüsseltresors.|

## <a name="microsoftlabservices"></a>Microsoft.LabServices

| Vorgang | BESCHREIBUNG |
|---|---|
|/labAccounts/CreateLab/action|Hiermit wird ein Lab in einem Lab-Konto erstellt.|
|/labAccounts/delete|Hiermit werden Lab-Konten gelöscht.|
|/labAccounts/labs/delete|Dient zum Löschen von Labs.|
|/labAccounts/labs/environmentSettings/delete|Hiermit wird eine Umgebungseinstellung gelöscht.|
|/labAccounts/labs/environmentSettings/environments/delete|Dient zum Löschen von Umgebungen.|
|/labAccounts/labs/environmentSettings/environments/read|Dient zum Lesen von Umgebungen.|
|/labAccounts/labs/environmentSettings/environments/write|Dient zum Hinzufügen oder Ändern von Umgebungen.|
|/labAccounts/labs/environmentSettings/Publish/action|Stellt die erforderlichen Ressourcen für eine Umgebungseinstellung anhand des aktuellen Status der Lab- bzw. Umgebungseinstellung bereit oder hebt deren Bereitstellung auf.|
|/labAccounts/labs/environmentSettings/read|Hiermit wird eine Umgebungseinstellung gelesen.|
|/labAccounts/labs/environmentSettings/write|Dient zum Hinzufügen oder Ändern einer Umgebungseinstellung.|
|/labAccounts/labs/read|Dient zum Lesen von Labs.|
|/labAccounts/labs/users/delete|Hiermit werden Benutzer gelöscht.|
|/labAccounts/labs/users/read|Hiermit werden Benutzer gelesen.|
|/labAccounts/labs/users/write|Dient zum Hinzufügen oder Ändern von Benutzern.|
|/labAccounts/labs/write|Dient zum Hinzufügen oder Ändern von Labs.|
|/labAccounts/read|Hiermit werden Lab-Konten gelesen.|
|/labAccounts/write|Dient zum Hinzufügen oder Ändern von Lab-Konten.|
|/locations/operations/read|Dient zum Lesen von Vorgängen.|
|/register/action|Registriert das Abonnement.|

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices

| Vorgang | BESCHREIBUNG |
|---|---|
|/accounts/delete|Hiermit wird ein Location Based Services-Konto gelöscht.|
|/accounts/listKeys/action|Hiermit werden Location Based Services-Kontoschlüssel aufgelistet.|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/accounts/providers/Microsoft.Insights/metricDefinitions/read|Ruft die verfügbaren Metriken für Location Based Services-Konten ab.|
|/accounts/read|Hiermit wird ein Location Based Services-Konto abgerufen.|
|/accounts/regenerateKey/action|Hiermit wird ein primärer oder sekundärer Schlüssel für ein neues Location Based Services-Konto generiert.|
|/accounts/write|Hiermit wird ein Location Based Services-Konto erstellt oder aktualisiert.|
|/register/action|Hiermit wird der Anbieter registriert.|

## <a name="microsoftlogic"></a>Microsoft.Logic

| Vorgang | BESCHREIBUNG |
|---|---|
|/integrationAccounts/agreements/delete|Löscht die Vereinbarung im Integrationskonto.|
|/integrationAccounts/agreements/listContentCallbackUrl/action|Ruft die Rückruf-URL für den Vereinbarungsinhalt im Integrationskonto ab.|
|/integrationAccounts/agreements/read|Liest die Vereinbarung im Integrationskonto.|
|/integrationAccounts/agreements/write|Erstellt oder aktualisiert die Vereinbarung im Integrationskonto.|
|/integrationAccounts/assemblies/delete|Löscht die Assembly im Integrationskonto.|
|/integrationAccounts/assemblies/listContentCallbackUrl/action|Ruft die Rückruf-URL für den Inhalt der Assembly im Integrationskonto ab.|
|/integrationAccounts/assemblies/read|Liest die Assembly im Integrationskonto.|
|/integrationAccounts/assemblies/write|Erstellt oder aktualisiert die Assembly im Integrationskonto.|
|/integrationAccounts/batchConfigurations/delete|Löscht die Batchkonfiguration im Integrationskonto.|
|/integrationAccounts/batchConfigurations/read|Liest die Batchkonfiguration im Integrationskonto.|
|/integrationAccounts/batchConfigurations/write|Erstellt oder aktualisiert die Batchkonfiguration im Integrationskonto.|
|/integrationAccounts/certificates/delete|Löscht das Zertifikat im Integrationskonto.|
|/integrationAccounts/certificates/read|Liest das Zertifikat im Integrationskonto.|
|/integrationAccounts/certificates/write|Erstellt oder aktualisiert das Zertifikat im Integrationskonto.|
|/integrationAccounts/delete|Löscht das Integrationskonto.|
|/integrationAccounts/listCallbackUrl/action|Ruft die Rückruf-URL für das Integrationskonto ab.|
|/integrationAccounts/listKeyVaultKeys/action|Ruft die Schlüssel im Schlüsseltresor ab.|
|/integrationAccounts/logTrackingEvents/action|Protokolliert die Überwachungsereignisse im Integrationskonto.|
|/integrationAccounts/maps/delete|Löscht die Zuordnung im Integrationskonto.|
|/integrationAccounts/maps/listContentCallbackUrl/action|Ruft die Rückruf-URL für den Inhalt der Zuordnung im Integrationskonto ab.|
|/integrationAccounts/maps/read|Liest die Zuordnung im Integrationskonto.|
|/integrationAccounts/maps/write|Erstellt oder aktualisiert die Zuordnung im Integrationskonto.|
|/integrationAccounts/partners/delete|Löscht den Partner im Integrationskonto.|
|/integrationAccounts/partners/listContentCallbackUrl/action|Ruft die Rückruf-URL für den Partnerinhalt im Integrationskonto ab.|
|/integrationAccounts/partners/read|Liest den Partner im Integrationskonto.|
|/integrationAccounts/partners/write|Erstellt oder aktualisiert den Partner im Integrationskonto.|
|/integrationAccounts/providers/Microsoft.Insights/logDefinitions/read|Liest die Protokolldefinitionen des Integrationskontos.|
|/integrationAccounts/read|Liest das Integrationskonto.|
|/integrationAccounts/regenerateAccessKey/action|Generiert die Zugriffsschlüsselgeheimnisse neu.|
|/integrationAccounts/schemas/delete|Löscht das Schema im Integrationskonto.|
|/integrationAccounts/schemas/listContentCallbackUrl/action|Ruft die Rückruf-URL für den Schemainhalt im Integrationskonto ab.|
|/integrationAccounts/schemas/read|Liest das Schema im Integrationskonto.|
|/integrationAccounts/schemas/write|Erstellt oder aktualisiert das Schema im Integrationskonto.|
|/integrationAccounts/sessions/delete|Löscht die Sitzung im Integrationskonto.|
|/integrationAccounts/sessions/read|Liest die Batchkonfiguration im Integrationskonto.|
|/integrationAccounts/sessions/write|Erstellt oder aktualisiert die Sitzung im Integrationskonto.|
|/integrationAccounts/write|Erstellt oder aktualisiert das Integrationskonto.|
|/locations/workflows/validate/action|Überprüft den Workflow.|
|/operations/read|Ruft den Vorgang ab.|
|/register/action|Registriert den Microsoft.Logic-Ressourcenanbieter für ein angegebenes Abonnement.|
|/workflows/accessKeys/delete|Löscht den Zugriffsschlüssel.|
|/workflows/accessKeys/list/action|Listet die Zugriffsschlüsselgeheimnisse auf.|
|/workflows/accessKeys/read|Liest den Zugriffsschlüssel.|
|/workflows/accessKeys/regenerate/action|Generiert die Zugriffsschlüsselgeheimnisse neu.|
|/workflows/accessKeys/write|Erstellt oder aktualisiert den Zugriffsschlüssel.|
|/workflows/delete|Löscht den Workflow.|
|/workflows/disable/action|Deaktiviert den Workflow.|
|/workflows/enable/action|Aktiviert den Workflow.|
|/workflows/listCallbackUrl/action|Ruft die Rückruf-URL für Workflows ab.|
|/workflows/listSwagger/action|Ruft die Swagger-Definitionen für den Workflow ab.|
|/workflows/move/action|Verschiebt den Workflow von der vorhandenen Abonnement-ID, der Ressourcengruppe und/oder dem Namen in eine andere Abonnement-ID, eine andere Ressourcengruppe und/oder einen anderen Namen.|
|/workflows/providers/Microsoft.Insights/diagnosticSettings/read|Liest die Diagnoseeinstellungen des Workflows.|
|/workflows/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung des Workflows.|
|/workflows/providers/Microsoft.Insights/logDefinitions/read|Liest die Protokolldefinitionen des Workflows.|
|/workflows/providers/Microsoft.Insights/metricDefinitions/read|Liest die Metrikdefinitionen des Workflows.|
|/workflows/read|Liest den Workflow.|
|/workflows/regenerateAccessKey/action|Generiert die Zugriffsschlüsselgeheimnisse neu.|
|/workflows/run/action|Startet eine Ausführung des Workflows.|
|/workflows/runs/actions/listExpressionTraces/action|Ruft die Ausdrucksablaufverfolgungen für Workflowausführungsaktionen ab.|
|/workflows/runs/actions/read|Liest die Workflowausführungsaktion.|
|/workflows/runs/actions/repetitions/listExpressionTraces/action|Ruft die Ausdrucksablaufverfolgungen für Wiederholungen von Workflowausführungsaktionen ab.|
|/workflows/runs/actions/repetitions/read|Liest die Wiederholung von Workflowausführungsaktionen.|
|/workflows/runs/actions/scoperepetitions/read|Liest die Bereichswiederholung von Workflowausführungsaktionen.|
|/workflows/runs/cancel/action|Bricht die Ausführung eines Workflows ab.|
|/workflows/runs/operations/read|Liest den Status von Workflowausführungsaktionen.|
|/workflows/runs/read|Liest die Workflowausführung.|
|/workflows/suspend/action|Hält den Workflow an.|
|/workflows/triggers/histories/read|Liest die Triggerverläufe.|
|/workflows/triggers/histories/resubmit/action|Übermittelt den Workflowtrigger erneut.|
|/workflows/triggers/listCallbackUrl/action|Ruft die Rückruf-URL für Trigger ab.|
|/workflows/triggers/read|Liest den Trigger.|
|/workflows/triggers/reset/action|Setzt den Trigger zurück.|
|/workflows/triggers/run/action|Führt den Trigger aus.|
|/workflows/triggers/setState/action|Legt den Triggerzustand fest.|
|/workflows/validate/action|Überprüft den Workflow.|
|/workflows/versions/read|Liest die Workflowversion.|
|/workflows/versions/triggers/listCallbackUrl/action|Ruft die Rückruf-URL für Trigger ab.|
|/workflows/write|Erstellt oder aktualisiert den Workflow.|

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

| Vorgang | BESCHREIBUNG |
|---|---|
|/commitmentPlans/commitmentAssociations/move/action|Dient zum Verschieben beliebiger Machine Learning-Vertragsplanzuordnungen.|
|/commitmentPlans/commitmentAssociations/read|Dient zum Lesen beliebiger Machine Learning-Vertragsplanzuordnungen.|
|/commitmentPlans/delete|Dient zum Löschen beliebiger Machine Learning-Vertragspläne.|
|/commitmentPlans/join/action|Dient zum Beitreten zu beliebigen Machine Learning-Vertragsplänen.|
|/commitmentPlans/read|Dient zum Lesen beliebiger Machine Learning-Vertragspläne.|
|/commitmentPlans/write|Dient zum Erstellen oder Aktualisieren beliebiger Machine Learning-Vertragspläne.|
|/locations/operationresults/read|Ruft das Ergebnis eines Machine Learning-Vorgangs ab.|
|/locations/operationsstatus/read|Ruft den Status eines laufenden Machine Learning-Vorgangs ab.|
|/operations/read|Ruft Machine Learning-Vorgänge ab.|
|/register/action|Registriert das Abonnement für den Machine Learning-Webdienst-Ressourcenanbieter und ermöglicht die Erstellung von Webdiensten.|
|/skus/read|Dient zum Abrufen von SKUs für Machine Learning-Verpflichtungspläne.|
|/webServices/action|Dient zum Erstellen regionaler Webdiensteigenschaften für unterstützte Regionen.|
|/webServices/delete|Dient zum Löschen beliebiger Machine Learning-Webdienste.|
|/webServices/read|Dient zum Lesen beliebiger Machine Learning-Webdienste.|
|/webServices/write|Dient zum Erstellen oder Aktualisieren beliebiger Machine Learning-Webdienste.|
|/Workspaces/delete|Dient zum Löschen beliebiger Machine Learning-Arbeitsbereiche.|
|/Workspaces/listworkspacekeys/action|Dient zum Auflisten von Schlüsseln für einen Machine Learning-Arbeitsbereich.|
|/Workspaces/read|Dient zum Lesen beliebiger Machine Learning-Arbeitsbereiche.|
|/Workspaces/resyncstoragekeys/action|Dient zum erneuten Synchronisieren von Schlüsseln eines für einen Machine Learning-Arbeitsbereich konfigurierten Speicherkontos.|
|/Workspaces/write|Dient zum Erstellen oder Aktualisieren beliebiger Machine Learning-Arbeitsbereiche.|

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute

| Vorgang | BESCHREIBUNG |
|---|---|
|/operationalizationClusters/checkUpdate/action|Hiermit wird überprüft, ob Updates für Systemdienste für den Operationalisierungscluster verfügbar sind.|
|/operationalizationClusters/delete|Hiermit werden alle Hostingkonten gelöscht.|
|/operationalizationClusters/listKeys/action|Hiermit werden die dem Operationalisierungscluster zugeordneten Schlüssel aufgelistet.|
|/operationalizationClusters/read|Hiermit werden alle Hostingkonten gelesen.|
|/operationalizationClusters/updateSystem/action|Hiermit werden die Systemdienste in einem Operationalisierungscluster aktualisiert.|
|/operationalizationClusters/write|Hiermit werden alle Hostingkonten erstellt oder aktualisiert.|
|/register/action|Registriert die Abonnement-ID beim Ressourcenanbieter und ermöglicht die Erstellung von Machine Learning-Computeressourcen.|

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement

| Vorgang | BESCHREIBUNG |
|---|---|
|/accounts/delete|Hiermit werden alle Hostingkonten gelöscht.|
|/accounts/read|Hiermit werden alle Hostingkonten gelesen.|
|/accounts/write|Hiermit werden alle Hostingkonten erstellt oder aktualisiert.|
|/register/action|Registriert die Abonnement-ID beim Ressourcenanbieter und ermöglicht die Erstellung eines Hostingkontos.|

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

| Vorgang | BESCHREIBUNG |
|---|---|
|/userAssignedIdentities/assign/action|RBAC-Aktion für das Zuweisen einer vorhandenen Identität, die einem Benutzer zugewiesen ist, zu einer Ressource|
|/userAssignedIdentities/delete|Löscht eine vorhandene zugewiesene Benutzeridentität.|
|/userAssignedIdentities/read|Ruft eine vorhandene zugewiesene Benutzeridentität ab.|
|/userAssignedIdentities/write|Erstellt eine neue zugewiesene Benutzeridentität oder aktualisiert die Markierungen, die einer vorhandenen zugewiesenen Benutzeridentität zugeordnet sind.|

## <a name="microsoftmanagedlab"></a>Microsoft.ManagedLab

| Vorgang | BESCHREIBUNG |
|---|---|
|/labAccounts/CreateLab/action|Hiermit wird ein Lab in einem Lab-Konto erstellt.|
|/labAccounts/delete|Hiermit werden Lab-Konten gelöscht.|
|/labAccounts/labs/delete|Dient zum Löschen von Labs.|
|/labAccounts/labs/environmentSettings/delete|Hiermit wird eine Umgebungseinstellung gelöscht.|
|/labAccounts/labs/environmentSettings/environments/delete|Dient zum Löschen von Umgebungen.|
|/labAccounts/labs/environmentSettings/environments/read|Dient zum Lesen von Umgebungen.|
|/labAccounts/labs/environmentSettings/environments/write|Dient zum Hinzufügen oder Ändern von Umgebungen.|
|/labAccounts/labs/environmentSettings/read|Hiermit wird eine Umgebungseinstellung gelesen.|
|/labAccounts/labs/environmentSettings/write|Dient zum Hinzufügen oder Ändern einer Umgebungseinstellung.|
|/labAccounts/labs/labVms/delete|Löscht virtuelle Lab-Computer.|
|/labAccounts/labs/labVms/read|Liest virtuelle Lab-Computer.|
|/labAccounts/labs/labVms/write|Dient zum Hinzufügen oder Ändern virtueller Lab-Computer.|
|/labAccounts/labs/read|Dient zum Lesen von Labs.|
|/labAccounts/labs/write|Dient zum Hinzufügen oder Ändern von Labs.|
|/labAccounts/read|Hiermit werden Lab-Konten gelesen.|
|/labAccounts/write|Dient zum Hinzufügen oder Ändern von Lab-Konten.|
|/locations/operations/read|Dient zum Lesen von Vorgängen.|
|/register/action|Registriert das Abonnement.|

## <a name="microsoftmanagement"></a>Microsoft.Management

| Vorgang | BESCHREIBUNG |
|---|---|
|/checkNameAvailability/action|Überprüft, ob der Name der angegebenen Verwaltungsgruppe gültig und eindeutig ist.|
|/getEntities/action|Listet alle Entitäten (Verwaltungsgruppen, Abonnements usw.) für den authentifizierten Benutzer auf.|
|/managementGroups/delete|Löscht eine Verwaltungsgruppe.|
|/managementGroups/read|Listet die Verwaltungsgruppen für den authentifizierten Benutzer auf.|
|/managementGroups/subscriptions/delete|Hebt die Zuordnung des Abonnements mit der Verwaltungsgruppe auf.|
|/managementGroups/subscriptions/write|Ordnet ein vorhandenes Abonnement der Verwaltungsgruppe zu.|
|/managementGroups/write|Erstellt oder aktualisiert eine Verwaltungsgruppe.|

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

| Vorgang | BESCHREIBUNG |
|---|---|
|/ClassicDevServices/delete|Führt einen DELETE-Vorgang für eine Ressource eines klassischen Entwicklerdiensts durch.|
|/ClassicDevServices/listSecrets/action|Ruft die Ressourcenverwaltungsschlüssel eines klassischen Entwicklerdiensts ab.|
|/ClassicDevServices/listSingleSignOnToken/action|Ruft die URL für einmaliges Anmelden für einen klassischen Entwicklerdienst ab.|
|/ClassicDevServices/read|Führt einen GET-Vorgang für einen klassischen Entwicklerdienst durch.|
|/ClassicDevServices/regenerateKey/action|Generiert Ressourcenverwaltungsschlüssel eines klassischen Entwicklerdiensts.|
|/Operations/read|Liest die Vorgänge für alle Ressourcentypen.|

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

| Vorgang | BESCHREIBUNG |
|---|---|
|/agreements/offers/plans/cancel/action|Dient zum Stornieren einer Vereinbarung für ein angegebenes Marketplace-Element.|
|/agreements/offers/plans/read|Dient zum Zurückgeben einer Vereinbarung für ein angegebenes Marketplace-Element.|
|/agreements/offers/plans/sign/action|Dient zum Unterzeichnen einer Vereinbarung für ein angegebenes Marketplace-Element.|
|/agreements/read|Gibt alle Vereinbarungen eines bestimmten Abonnements zurück.|
|/offertypes/publishers/offers/plans/agreements/read|Dient zum Abrufen einer Vereinbarung für ein angegebenes Marketplace-VM-Element.|
|/offertypes/publishers/offers/plans/agreements/write|Dient zum Registrieren oder Stornieren einer Vereinbarung für ein angegebenes Marketplace-VM-Element.|

## <a name="microsoftmedia"></a>Microsoft.Media

| Vorgang | BESCHREIBUNG |
|---|---|
|/checknameavailability/action|Überprüft, ob ein Media Services-Kontoname verfügbar ist.|
|/mediaservices/delete|Löscht ein Media Services-Konto.|
|/mediaservices/listKeys/action|Listet die ACS-Schlüssel für das Media Services-Konto auf.|
|/mediaservices/read|Liest ein Media Services-Konto.|
|/mediaservices/regenerateKey/action|Generiert erneut einen Media Services-ACS-Schlüssel.|
|/mediaservices/syncStorageKeys/action|Synchronisiert die Speicherschlüssel für ein angefügtes Azure Storage-Konto.|
|/mediaservices/write|Erstellt oder aktualisiert ein Media Services-Konto.|
|/operations/read|Liest ein Media Services-Konto.|
|/register/action|Registriert das Abonnement für den Media Services-Ressourcenanbieter und ermöglicht die Erstellung von Media Services-Konten.|

## <a name="microsoftmigrate"></a>Microsoft.Migrate

| Vorgang | BESCHREIBUNG |
|---|---|
|/Operations/read|Liest die verfügbar gemachten Vorgänge.|

## <a name="microsoftnetwork"></a>Microsoft.Network

| Vorgang | BESCHREIBUNG |
|---|---|
|/applicationGatewayAvailableSslOptions/predefinedPolicies/read|Vordefinierte SSL-Richtlinie für Application Gateway|
|/applicationGatewayAvailableSslOptions/read|Verfügbare SSL-Optionen für Application Gateway|
|/applicationGatewayAvailableWafRuleSets/read|Ruft verfügbare WAF-Regelsätze für Application Gateway ab.|
|/applicationGateways/backendAddressPools/join/action|Verknüpft einen Back-End-Adresspool für ein Anwendungsgateway.|
|/applicationGateways/backendhealth/action|Ruft die Back-End-Integrität eines Anwendungsgateways ab.|
|/applicationGateways/delete|Löscht ein Anwendungsgateway.|
|/applicationGateways/effectiveNetworkSecurityGroups/action|Ruft die für Application Gateway konfigurierte Routentabelle ab.|
|/applicationGateways/effectiveRouteTable/action|Ruft die für Application Gateway konfigurierte Routentabelle ab.|
|/applicationGateways/providers/Microsoft.Insights/logDefinitions/read|Ruft die Ereignisse für Application Gateway ab.|
|/applicationGateways/providers/Microsoft.Insights/metricDefinitions/read|Ruft die verfügbaren Metriken für Application Gateway ab.|
|/applicationGateways/read|Ruft ein Anwendungsgateway ab.|
|/applicationGateways/setSecurityCenterConfiguration/action|Legt die Security Center-Konfiguration für Application Gateway fest.|
|/applicationGateways/start/action|Startet ein Anwendungsgateway.|
|/applicationGateways/stop/action|Beendet ein Anwendungsgateway.|
|/applicationGateways/write|Erstellt ein Anwendungsgateway oder aktualisiert ein vorhandenes Anwendungsgateway.|
|/applicationSecurityGroups/delete|Löscht eine Anwendungssicherheitsgruppe.|
|/applicationSecurityGroups/joinIpConfiguration/action|Dient zum Verknüpfen der IP-Konfiguration mit Anwendungssicherheitsgruppen.|
|/applicationSecurityGroups/joinNetworkSecurityRule/action|Dient zum Verknüpfen der Sicherheitsregel mit Anwendungssicherheitsgruppen.|
|/applicationSecurityGroups/read|Ruft eine Anwendungssicherheitsgruppen-ID ab.|
|/applicationSecurityGroups/write|Erstellt eine Anwendungssicherheitsgruppe oder aktualisiert eine vorhandene Anwendungssicherheitsgruppe.|
|/bgpServiceCommunities/read|Dient zum Abrufen von BGP Service Communities.|
|/checkTrafficManagerNameAvailability/action|Prüft die Verfügbarkeit eines relativen Traffic Manager-DNS-Namens.|
|/connections/delete|Löscht VirtualNetworkGatewayConnection-Elemente.|
|/connections/read|Ruft VirtualNetworkGatewayConnection-Elemente ab.|
|/connections/sharedkey/action|Ruft SharedKey-Elemente für VirtualNetworkGatewayConnection ab.|
|/connections/sharedKey/read|Ruft SharedKey-Elemente für „VirtualNetworkGatewayConnection“ ab.|
|/connections/sharedKey/write|Erstellt ein SharedKey-Element für „VirtualNetworkGatewayConnection“ oder aktualisiert ein vorhandenes SharedKey-Element für „VirtualNetworkGatewayConnection“.|
|/connections/vpndeviceconfigurationscript/read|Ruft die VPN-Gerätekonfiguration von VirtualNetworkGatewayConnection ab.|
|/connections/write|Erstellt ein VirtualNetworkGatewayConnection-Element oder aktualisiert ein vorhandenes VirtualNetworkGatewayConnection-Element.|
|/ddosProtectionPlans/ddosProtectionPlanProxies/delete|Löscht den Proxy eines DDoS-Schutzplans.|
|/ddosProtectionPlans/ddosProtectionPlanProxies/read|Ruft die Proxydefinition eines DDoS-Schutzplans ab.|
|/ddosProtectionPlans/ddosProtectionPlanProxies/write|Erstellt einen Proxy für den DDoS-Schutzplan oder aktualisiert einen vorhandenen Proxy für den DDoS-Schutzplan.|
|/ddosProtectionPlans/delete|Löscht einen DDoS-Schutzplan.|
|/ddosProtectionPlans/join/action|Dient zum Verknüpfen eines DDoS-Schutzplans.|
|/ddosProtectionPlans/read|Dient zum Aufrufen eines DDoS-Schutzplans.|
|/ddosProtectionPlans/write|Erstellt oder aktualisiert einen DDoS-Schutzplan. |
|/dnsoperationresults/read|Ruft Ergebnisse eines DNS-Vorgangs ab.|
|/dnsoperationstatuses/read|Ruft den Status eines DNS-Vorgangs ab. |
|/dnszones/A/delete|Dient zum Entfernen des Ressourceneintragssatzes mit einem bestimmten Namen und dem Typ „A“ aus einer DNS-Zone.|
|/dnszones/A/read|Dient zum Abrufen des Ressourceneintragssatzes vom Typ „A“ im JSON-Format. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag.|
|/dnszones/A/write|Dient zum Erstellen oder Aktualisieren eines Ressourceneintragssatzes vom Typ „A“ innerhalb einer DNS-Zone. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt.|
|/dnszones/AAAA/delete|Dient zum Entfernen des Ressourceneintragssatzes mit einem bestimmten Namen und dem Typ „AAAA“ aus einer DNS-Zone.|
|/dnszones/AAAA/read|Dient zum Abrufen des Ressourceneintragssatzes vom Typ „AAAA“ im JSON-Format. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag.|
|/dnszones/AAAA/write|Dient zum Erstellen oder Aktualisieren eines Ressourceneintragssatzes vom Typ „AAAA“ innerhalb einer DNS-Zone. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt.|
|/dnszones/all/read|Ruft DNS-Ressourceneintragssätze typenübergreifend ab.|
|/dnszones/CAA/delete|Dient zum Entfernen der Datensatzgruppe mit einem bestimmten Namen und dem Typ „CAA“ aus einer DNS-Zone.|
|/dnszones/CAA/read|Dient zum Abrufen der Datensatzgruppe vom Typ „CAA“ im JSON-Format. Der Ressourceneintragssatz enthält Gültigkeitsdauer, Tags und ETag.|
|/dnszones/CAA/write|Dient zum Erstellen oder Aktualisieren einer Datensatzgruppe vom Typ „CAA“ innerhalb einer DNS-Zone. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt.|
|/dnszones/CNAME/delete|Dient zum Entfernen des Ressourceneintragssatzes mit einem bestimmten Namen und dem Typ „CNAME“ aus einer DNS-Zone.|
|/dnszones/CNAME/read|Dient zum Abrufen des Ressourceneintragssatzes vom Typ „CNAME“ im JSON-Format. Der Ressourceneintragssatz enthält Gültigkeitsdauer, Tags und ETag.|
|/dnszones/CNAME/write|Dient zum Erstellen oder Aktualisieren eines Ressourceneintragssatzes vom Typ „CNAME“ innerhalb einer DNS-Zone. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt.|
|/dnszones/delete|Dient zum Löschen der DNS-Zone im JSON-Format. Zu den Zoneneigenschaften zählen „tags“, „etag“, „numberOfRecordSets“ und „maxNumberOfRecordSets“.|
|/dnszones/MX/delete|Dient zum Entfernen des Ressourceneintragssatzes mit einem bestimmten Namen und dem Typ „MX“ aus einer DNS-Zone.|
|/dnszones/MX/read|Dient zum Abrufen des Ressourceneintragssatzes vom Typ „MX“ im JSON-Format. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag.|
|/dnszones/MX/write|Dient zum Erstellen oder Aktualisieren eines Ressourceneintragssatzes vom Typ „MX“ innerhalb einer DNS-Zone. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt.|
|/dnszones/NS/delete|Löscht den DNS-Ressourceneintragssatz vom Typ „NS“.|
|/dnszones/NS/read|Ruft DNS-Ressourceneintragssätze vom Typ „NS“ ab.|
|/dnszones/NS/write|Erstellt oder aktualisiert DNS-Ressourceneintragssätze vom Typ „NS“.|
|/dnszones/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellungen für eine DNS-Zone ab.|
|/dnszones/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellungen für eine DNS-Zone.|
|/dnszones/providers/Microsoft.Insights/metricDefinitions/read|Ruft die Metrikdefinitionen der DNS-Zone ab.|
|/dnszones/PTR/delete|Dient zum Entfernen des Ressourceneintragssatzes mit einem bestimmten Namen und dem Typ „PTR“ aus einer DNS-Zone.|
|/dnszones/PTR/read|Dient zum Abrufen des Ressourceneintragssatzes vom Typ „PTR“ im JSON-Format. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag.|
|/dnszones/PTR/write|Dient zum Erstellen oder Aktualisieren eines Ressourceneintragssatzes vom Typ „PTR“ innerhalb einer DNS-Zone. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt.|
|/dnszones/read|Dient zum Abrufen der DNS-Zone im JSON-Format. Zu den Zoneneigenschaften zählen „tags“, „etag“, „numberOfRecordSets“ und „maxNumberOfRecordSets“. Die in der Zone enthaltenen Ressourceneintragssätze werden durch diesen Befehl nicht abgerufen.|
|/dnszones/recordsets/read|Ruft DNS-Ressourceneintragssätze typenübergreifend ab.|
|/dnszones/SOA/read|Ruft DNS-Ressourceneintragssätze vom Typ „SOA“ ab.|
|/dnszones/SOA/write|Erstellt oder aktualisiert DNS-Ressourceneintragssätze vom Typ „SOA“.|
|/dnszones/SRV/delete|Dient zum Entfernen des Ressourceneintragssatzes mit einem bestimmten Namen und dem Typ „SRV“ aus einer DNS-Zone.|
|/dnszones/SRV/read|Dient zum Abrufen des Ressourceneintragssatzes vom Typ „SRV“ im JSON-Format. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag.|
|/dnszones/SRV/write|Dient zum Erstellen oder Aktualisieren von Ressourceneintragssätzen vom Typ „SRV“.|
|/dnszones/TXT/delete|Dient zum Entfernen des Ressourceneintragssatzes mit einem bestimmten Namen und dem Typ „TXT“ aus einer DNS-Zone.|
|/dnszones/TXT/read|Dient zum Abrufen des Ressourceneintragssatzes vom Typ „TXT“ im JSON-Format. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag.|
|/dnszones/TXT/write|Dient zum Erstellen oder Aktualisieren eines Ressourceneintragssatzes vom Typ „TXT“ innerhalb einer DNS-Zone. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt.|
|/dnszones/write|Dient zum Erstellen oder Aktualisieren einer DNS-Zone innerhalb einer Ressourcengruppe.  Wird zum Aktualisieren der Tags für eine DNS-Zonenressource verwendet. Mit diesem Befehl können keine Ressourceneintragssätze innerhalb der Zone erstellt oder aktualisiert werden.|
|/expressRouteCircuits/authorizations/delete|Löscht eine ExpressRouteCircuit-Autorisierung.|
|/expressRouteCircuits/authorizations/read|Ruft eine ExpressRouteCircuit-Autorisierung ab.|
|/expressRouteCircuits/authorizations/write|Erstellt eine ExpressRouteCircuit-Autorisierung oder aktualisiert eine vorhandene ExpressRouteCircuit-Autorisierung.|
|/expressRouteCircuits/delete|Löscht ein ExpressRouteCircuit-Element.|
|/expressRouteCircuits/peerings/arpTables/action|Ruft ein ArpTable-Element für ein ExpressRouteCircuit-Peering ab.|
|/expressRouteCircuits/peerings/connections/delete|Löscht eine ExpressRouteCircuit-Verbindung.|
|/expressRouteCircuits/peerings/connections/read|Ruft eine ExpressRouteCircuit-Verbindung ab.|
|/expressRouteCircuits/peerings/connections/write|Erstellt eine ExpressRouteCircuit-Verbindungsressource oder aktualisiert eine vorhandene ExpressRouteCircuit-Verbindungsressource.|
|/expressRouteCircuits/peerings/delete|Löscht ein ExpressRouteCircuit-Peering.|
|/expressRouteCircuits/peerings/read|Ruft ein ExpressRouteCircuit-Peering ab.|
|/expressRouteCircuits/peerings/routeTables/action|Ruft ein RouteTable-Element für ein ExpressRouteCircuit-Peering ab.|
|/expressRouteCircuits/peerings/routeTablesSummary/action|Ruft eine RouteTable-Zusammenfassung für ein ExpressRouteCircuit-Peering ab.|
|/expressRouteCircuits/peerings/stats/read|Ruft eine ExpressRouteCircuit-Peeringstatistik ab.|
|/expressRouteCircuits/peerings/write|Erstellt ein ExpressRouteCircuit-Peering oder aktualisiert ein vorhandenes ExpressRouteCircuit-Peering.|
|/expressRouteCircuits/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellungen für ExpressRoute-Verbindungen ab.|
|/expressRouteCircuits/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellungen für ExpressRoute-Verbindungen.|
|/expressRouteCircuits/providers/Microsoft.Insights/logDefinitions/read|Ruft die Ereignisse für ExpressRoute-Verbindungen ab.|
|/expressRouteCircuits/providers/Microsoft.Insights/metricDefinitions/read|Ruft die Metrikdefinitionen für ExpressRoute-Verbindungen ab.|
|/expressRouteCircuits/read|Dient zum Abrufen eines ExpressRouteCircuit-Elements.|
|/expressRouteCircuits/stats/read|Ruft eine ExpressRouteCircuit-Statistik ab.|
|/expressRouteCircuits/write|Erstellt ein ExpressRouteCircuit-Element oder aktualisiert ein vorhandenes ExpressRouteCircuit-Element.|
|/expressRouteCrossConnections/delete|Löscht eine ExpressRoute-Querverbindung|
|/expressRouteCrossConnections/join/action|Verknüpft eine ExpressRoute-Querverbindung|
|/expressRouteCrossConnections/peerings/arpTables/action|Ruft eine ARP-Tabelle zum Peering einer ExpressRoute-Querverbindung ab.|
|/expressRouteCrossConnections/peerings/delete|Löscht ein Peering einer ExpressRoute-Querverbindung.|
|/expressRouteCrossConnections/peerings/read|Ruft ein Peering einer ExpressRoute-Querverbindung ab.|
|/expressRouteCrossConnections/peerings/routeTables/action|Ruft eine Routentabelle zum Peering einer ExpressRoute-Querverbindung ab.|
|/expressRouteCrossConnections/peerings/routeTableSummary/action|Ruft eine Routentabellenzusammenfassung zum Peering einer ExpressRoute-Querverbindung ab.|
|/expressRouteCrossConnections/peerings/stats/read|Ruft die Statistik für ein Peering einer ExpressRoute-Querverbindung ab.|
|/expressRouteCrossConnections/peerings/write|Erstellt ein Peering für eine ExpressRoute-Querverbindung oder aktualisiert ein Peering für eine vorhandene ExpressRoute-Querverbindung.|
|/expressRouteCrossConnections/read|Ruft eine ExpressRoute-Querverbindung ab.|
|/expressRouteCrossConnections/write|Erstellt oder aktualisiert eine ExpressRoute-Querverbindung.|
|/expressRouteServiceProviders/read|Ruft ExpressRoute-Dienstanbieter ab.|
|/loadBalancers/backendAddressPools/join/action|Verknüpft einen Back-End-Adresspool für den Lastenausgleich.|
|/loadBalancers/backendAddressPools/read|Ruft eine Back-End-Adresspooldefinition für den Lastenausgleich ab.|
|/loadBalancers/delete|Löscht einen Lastenausgleich.|
|/loadBalancers/frontendIPConfigurations/read|Ruft eine Front-End-IP-Konfigurationsdefinition für den Lastenausgleich ab.|
|/loadBalancers/inboundNatPools/join/action|Verknüpft einen eingehenden NAT-Pool für den Lastenausgleich.|
|/loadBalancers/inboundNatPools/read|Ruft eine eingehende NAT-Pooldefinition für den Lastenausgleich ab.|
|/loadBalancers/inboundNatRules/delete|Löscht eine eingehende NAT-Regel für den Lastenausgleich.|
|/loadBalancers/inboundNatRules/join/action|Verknüpft eine eingehende NAT-Regel für den Lastenausgleich.|
|/loadBalancers/inboundNatRules/read|Ruft eine eingehende NAT-Regeldefinition für den Lastenausgleich ab.|
|/loadBalancers/inboundNatRules/write|Erstellt eine eingehende NAT-Regel für den Lastenausgleich oder aktualisiert eine vorhandene eingehende NAT-Regel für den Lastenausgleich.|
|/loadBalancers/loadBalancingRules/read|Ruft eine Lastenausgleichsregel-Definition für den Lastenausgleich ab.|
|/loadBalancers/networkInterfaces/read|Ruft Verweise auf alle Netzwerkschnittstellen unter einem Lastenausgleich ab.|
|/loadBalancers/outboundNatRules/read|Ruft eine ausgehende NAT-Regeldefinition für den Lastenausgleich ab.|
|/loadBalancers/probes/join/action|Ermöglicht die Verwendung von Prüfpunkten eines Lastenausgleichs. Beispielsweise kann mit dieser Berechtigung die healthProbe-Eigenschaft einer VM-Skalierungsgruppe auf den Prüfpunkt verweisen.|
|/loadBalancers/probes/read|Ruft einen Lastenausgleichstest ab.|
|/loadBalancers/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellungen für den Lastenausgleich ab.|
|/loadBalancers/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellungen für den Lastenausgleich.|
|/loadBalancers/providers/Microsoft.Insights/logDefinitions/read|Ruft die Ereignisse für den Lastenausgleich ab.|
|/loadBalancers/providers/Microsoft.Insights/metricDefinitions/read|Ruft die verfügbaren Metriken für den Lastenausgleich ab.|
|/loadBalancers/read|Ruft eine Lastenausgleichsdefinition ab.|
|/loadBalancers/virtualMachines/read|Ruft Verweise auf alle virtuellen Computer unter einem Lastenausgleich ab.|
|/loadBalancers/write|Erstellt einen Lastenausgleich oder aktualisiert einen vorhandenen Lastenausgleich.|
|/localnetworkgateways/delete|Löscht lokale Netzwerkgateways.|
|/localnetworkgateways/read|Ruft lokale Netzwerkgateways ab.|
|/localnetworkgateways/write|Erstellt ein lokales Netzwerkgateway oder aktualisiert ein vorhandenes lokales Netzwerkgateway.|
|/locations/checkDnsNameAvailability/read|Prüft, ob eine DNS-Bezeichnung am angegebenen Standort verfügbar ist.|
|/locations/operationResults/read|Ruft das Ergebnis eines asynchronen POST- oder DELETE-Vorgangs ab.|
|/locations/operations/read|Ruft die Vorgangsressource ab, die den Status eines asynchronen Vorgangs darstellt.|
|/locations/usages/read|Ruft die Ressourcenverwendungsmetriken ab.|
|/locations/virtualNetworkAvailableEndpointServices/read|Ruft eine Liste der verfügbaren Dienstendpunkte für virtuelle Netzwerke ab.|
|/networkInterfaces/delete|Löscht eine Netzwerkschnittstelle.|
|/networkInterfaces/diagnosticIdentity/read|Ruft die Diagnoseidentität der Ressource ab.|
|/networkInterfaces/effectiveNetworkSecurityGroups/action|Dient zum Abrufen der Netzwerksicherheitsgruppen, die für die Netzwerkschnittstelle des virtuellen Computers konfiguriert sind.|
|/networkInterfaces/effectiveRouteTable/action|Dient zum Abrufen der Routingtabelle, die für die Netzwerkschnittstelle des virtuellen Computers konfiguriert ist.|
|/networkInterfaces/ipconfigurations/read|Ruft eine IP-Konfigurationsdefinition für Netzwerkschnittstellen ab. |
|/networkInterfaces/join/action|Verknüpft einen virtuellen Computer mit einer Netzwerkschnittstelle.|
|/networkInterfaces/loadBalancers/read|Ruft alle Lastenausgleichsmodule ab, denen die Netzwerkschnittstelle angehört.|
|/networkInterfaces/providers/Microsoft.Insights/metricDefinitions/read|Ruft die verfügbaren Metriken für die Netzwerkschnittstelle ab.|
|/networkInterfaces/read|Ruft eine Netzwerkschnittstellendefinition ab. |
|/networkInterfaces/write|Erstellt eine Netzwerkschnittstelle oder aktualisiert eine vorhandene Netzwerkschnittstelle. |
|/networkSecurityGroups/defaultSecurityRules/read|Ruft eine Standardsicherheitsregel-Definition ab.|
|/networkSecurityGroups/delete|Löscht eine Netzwerksicherheitsgruppe.|
|/networkSecurityGroups/join/action|Verknüpft eine Netzwerksicherheitsgruppe.|
|/networksecuritygroups/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellungen für Netzwerksicherheitsgruppen ab.|
|/networksecuritygroups/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellungen für Netzwerksicherheitsgruppen. Dieser Vorgang wird durch den Insights-Ressourcenanbieter unterstützt.|
|/networksecuritygroups/providers/Microsoft.Insights/logDefinitions/read|Ruft die Ereignisse für Netzwerksicherheitsgruppen ab.|
|/networkSecurityGroups/read|Ruft eine Netzwerksicherheitsgruppen-Definition ab.|
|/networkSecurityGroups/securityRules/delete|Löscht eine Sicherheitsregel.|
|/networkSecurityGroups/securityRules/read|Ruft eine Sicherheitsregeldefinition ab.|
|/networkSecurityGroups/securityRules/write|Erstellt eine Sicherheitsregel oder aktualisiert eine vorhandene Sicherheitsregel.|
|/networkSecurityGroups/write|Erstellt eine Netzwerksicherheitsgruppe oder aktualisiert eine vorhandene Netzwerksicherheitsgruppe.|
|/networkWatchers/availableProvidersList/action|Gibt alle verfügbaren Internetdienstanbieter für eine bestimmte Azure-Region zurück.|
|/networkWatchers/azureReachabilityReport/action|Gibt das relative Latenzergebnis für Internetdienstanbieter zwischen einem bestimmten Standort und Azure-Regionen zurück.|
|/networkWatchers/configureFlowLog/action|Konfiguriert die Datenflussprotokollierung für eine Zielressource.|
|/networkWatchers/connectionMonitors/delete|Löscht einen Verbindungsmonitor.|
|/networkWatchers/connectionMonitors/providers/Microsoft.Insights/ diagnosticSettings/read|Ruft die Diagnoseeinstellungen des Verbindungsmonitors ab.|
|/networkWatchers/connectionMonitors/providers/Microsoft.Insights/ diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellungen des Verbindungsmonitors.|
|/networkWatchers/connectionMonitors/providers/Microsoft.Insights/ metricDefinitions/read|Ruft die verfügbaren Metriken für den Verbindungsmonitor ab.|
|/networkWatchers/connectionMonitors/query/action|Fragt die Überwachungskonnektivität zwischen angegebenen Endpunkten ab.|
|/networkWatchers/connectionMonitors/read|Ruft Details zum Verbindungsmonitor ab.|
|/networkWatchers/connectionMonitors/start/action|Startet die Überwachungskonnektivität zwischen angegebenen Endpunkten.|
|/networkWatchers/connectionMonitors/stop/action|Startet oder beendet die Überwachungskonnektivität zwischen angegebenen Endpunkten.|
|/networkWatchers/connectionMonitors/write|Erstellt einen Verbindungsmonitor.|
|/networkWatchers/connectivityCheck/action|Überprüft die Möglichkeit, eine direkte TCP-Verbindung zwischen einem virtuellen Computer und einem angegebenen Endpunkt herzustellen, einschließlich mit anderen VMs oder einem freien Remoteserver.|
|/networkWatchers/delete|Löscht eine Network Watcher-Instanz.|
|/networkWatchers/ipFlowVerify/action|Gibt zurück, ob das Paket an oder von einem bestimmten Ziel zugelassen oder abgelehnt wird.|
|/networkWatchers/lenses/delete|Löscht einen Bereich.|
|/networkWatchers/lenses/query/action|Fragt die Überwachung des Netzwerkdatenverkehrs für einen angegebenen Endpunkt ab.|
|/networkWatchers/lenses/read|Ruft Bereichsdetails ab.|
|/networkWatchers/lenses/start/action|Startet die Überwachung des Netzwerkdatenverkehrs für einen angegebenen Endpunkt.|
|/networkWatchers/lenses/stop/action|Beendet die Überwachung des Netzwerkdatenverkehrs für einen angegebenen Endpunkt oder hält diese an.|
|/networkWatchers/lenses/write|Erstellt einen Bereich.|
|/networkWatchers/nextHop/action|Gibt für ein angegebenes Ziel und eine Ziel-IP-Adresse die Art des nächsten Hops und die IP-Adresse des nächsten Hops zurück.|
|/networkWatchers/packetCaptures/delete|Löscht eine Paketerfassung.|
|/networkWatchers/packetCaptures/queryStatus/action|Ruft Informationen zu Eigenschaften und zum Status einer Paketerfassungsressource ab.|
|/networkWatchers/packetCaptures/read|Dient zum Abrufen der Paketerfassungsdefinition.|
|/networkWatchers/packetCaptures/stop/action|Dient zum Beenden der laufenden Paketerfassungssitzung.|
|/networkWatchers/packetCaptures/write|Erstellt eine Paketerfassung.|
|/networkWatchers/queryFlowLogStatus/action|Ruft den Status der Datenflussprotokollierung für eine Ressource ab.|
|/networkWatchers/queryTroubleshootResult/action|Ruft das Ergebnis des zuvor oder momentan ausgeführten Problembehandlungsvorgangs ab.|
|/networkWatchers/read|Dient zum Abrufen der Network Watcher-Definition.|
|/networkWatchers/securityGroupView/action|Dient zum Anzeigen der konfigurierten und effektiven Netzwerksicherheitsgruppen-Regeln, die auf einen virtuellen Computer angewendet wurden.|
|/networkWatchers/topology/action|Ruft eine Netzwerkebenenansicht mit Ressourcen und deren Beziehungen in einer Ressourcengruppe ab.|
|/networkWatchers/troubleshoot/action|Startet die Problembehandlung für eine Netzwerkressource in Azure.|
|/networkWatchers/write|Erstellt eine Network Watcher-Instanz oder aktualisiert eine vorhandene Network Watcher-Instanz.|
|/operations/read|Dient zum Abrufen verfügbarer Vorgänge.|
|/publicIPAddresses/delete|Löscht eine öffentliche IP-Adresse.|
|/publicIPAddresses/join/action|Verknüpft eine öffentliche IP-Adresse.|
|/publicIPAddresses/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellungen der öffentlichen IP-Adresse ab.|
|/publicIPAddresses/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellungen der öffentlichen IP-Adresse.|
|/publicIPAddresses/providers/Microsoft.Insights/logDefinitions/read|Ruft die Protokolldefinitionen der öffentlichen IP-Adresse ab.|
|/publicIPAddresses/providers/Microsoft.Insights/metricDefinitions/read|Ruft die Metrikdefinitionen der öffentlichen IP-Adresse ab.|
|/publicIPAddresses/read|Ruft eine Definition für eine öffentliche IP-Adresse ab.|
|/publicIPAddresses/write|Erstellt eine öffentliche IP-Adresse oder aktualisiert eine vorhandene öffentliche IP-Adresse. |
|/register/action|Registriert das Abonnement.|
|/routeFilters/delete|Löscht eine Routenfilterdefinition.|
|/routeFilters/join/action|Verknüpft einen Routenfilter.|
|/routeFilters/read|Ruft eine Routenfilterdefinition ab.|
|/routeFilters/routeFilterRules/delete|Löscht eine Routenfilterregel-Definition.|
|/routeFilters/routeFilterRules/read|Ruft eine Routenfilterregel-Definition ab.|
|/routeFilters/routeFilterRules/write|Erstellt eine Routenfilterregel oder aktualisiert eine vorhandene Routenfilterregel.|
|/routeFilters/write|Erstellt einen Routenfilter oder aktualisiert einen vorhandenen Routenfilter.|
|/routeTables/delete|Löscht eine Routingtabellendefinition.|
|/routeTables/join/action|Verknüpft eine Routingtabelle.|
|/routeTables/read|Ruft eine Routingtabellendefinition ab.|
|/routeTables/routes/delete|Löscht eine Routendefinition.|
|/routeTables/routes/read|Ruft eine Routendefinition ab.|
|/routeTables/routes/write|Erstellt eine Route oder aktualisiert eine vorhandene Route.|
|/routeTables/write|Erstellt eine Routingtabelle oder aktualisiert eine vorhandene Routingtabelle.|
|/securegateways/applicationRuleCollections/delete|Löscht eine Collection mit Anwendungsregeln für ein sicheres Gateway.|
|/securegateways/applicationRuleCollections/read|Ruft eine Collection mit Anwendungsregeln für ein sicheres Gateway ab.|
|/securegateways/applicationRuleCollections/write|Erstellt oder aktualisiert eine Collection mit Anwendungsregeln für ein sicheres Gateway.|
|/securegateways/delete|Löscht ein sicheres Gateway.|
|/securegateways/networkRuleCollections/delete|Löscht eine Collection mit Netzwerkregeln für ein sicheres Gateway.|
|/securegateways/networkRuleCollections/read|Ruft eine Collection mit Netzwerkregeln für ein angegebenes sicheres Gateway ab.|
|/securegateways/networkRuleCollections/write|Erstellt oder aktualisiert eine Collection mit Netzwerkregeln für ein sicheres Gateway.|
|/securegateways/read|Ruft ein sicheres Gateway ab.|
|/securegateways/write|Erstellt oder aktualisiert ein sicheres Gateway.|
|/serviceEndpointPolicies/delete|Löscht eine Dienstendpunktrichtlinie.|
|/serviceEndpointPolicies/join/action|Verknüpft eine Dienstendpunktrichtlinie.|
|/serviceEndpointPolicies/joinSubnet/action|Verknüpft ein Subnetz mit Dienstendpunktrichtlinien.|
|/serviceEndpointPolicies/read|Ruft die Beschreibung einer Dienstendpunktrichtlinie ab.|
|/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/delete|Löscht die Definition einer Dienstendpunktrichtlinie.|
|/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/read|Ruft die Beschreibung für die Definition einer Dienstendpunktrichtlinie ab.|
|/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/write|Erstellt eine Definition für eine Dienstendpunktrichtlinie oder aktualisiert eine vorhandene Definition für eine Dienstendpunktrichtlinie.|
|/serviceEndpointPolicies/write|Erstellt eine Dienstendpunktrichtlinie oder aktualisiert eine vorhandene Dienstendpunktrichtlinie.|
|/trafficManagerGeographicHierarchies/read|Ruft die geografische Traffic Manager-Hierarchie mit Regionen ab, die mit der geografischen Routingmethode für Datenverkehr verwendet werden können.|
|/trafficManagerProfiles/azureEndpoints/delete|Löscht einen Azure-Endpunkt aus einem vorhandenen Traffic Manager-Profil an. Der Traffic Manager beendet die Weiterleitung des Datenverkehrs an den gelöschten Azure-Endpunkt.|
|/trafficManagerProfiles/azureEndpoints/read|Ruft einen Azure-Endpunkt ab, der einem Traffic Manager-Profil zugehörig ist, einschließlich aller Eigenschaften dieses Azure-Endpunkts.|
|/trafficManagerProfiles/azureEndpoints/write|Fügt einen neuen Azure-Endpunkt in einem vorhandenen Traffic Manager-Profil hinzu oder aktualisiert die Eigenschaften eines vorhandenen Azure-Endpunkts in diesem Traffic Manager-Profil.|
|/trafficManagerProfiles/delete|Dient zum Löschen des Traffic Manager-Profils. Alle dem Traffic Manager-Profil zugeordneten Einstellungen gehen verloren, und das Profil kann nicht mehr zum Routen von Datenverkehr verwendet werden.|
|/trafficManagerProfiles/externalEndpoints/delete|Löscht einen externen Endpunkt aus einem vorhandenen Traffic Manager-Profil. Der Traffic Manager beendet die Weiterleitung des Datenverkehrs an den gelöschten externen Endpunkt.|
|/trafficManagerProfiles/externalEndpoints/read|Ruft einen externen Endpunkt ab, der einem Traffic Manager-Profil zugehörig ist, einschließlich aller Eigenschaften dieses externen Endpunkts.|
|/trafficManagerProfiles/externalEndpoints/write|Fügt einen neuen externen Endpunkt in einem vorhandenen Traffic Manager-Profil hinzu oder aktualisiert die Eigenschaften eines vorhandenen externen Endpunkts in diesem Traffic Manager-Profil.|
|/trafficManagerProfiles/heatMaps/read|Ruft die Traffic Manager-Heatmap für das angegebene Traffic Manager-Profil ab, das die Anzahl der Abfragen und Latenzdaten nach Standort und Quell-IP enthält.|
|/trafficManagerProfiles/nestedEndpoints/delete|Löscht einen geschachtelten Endpunkt aus einem vorhandenen Traffic Manager-Profil. Der Traffic Manager beendet die Weiterleitung des Datenverkehrs an den gelöschten geschachtelten Endpunkt.|
|/trafficManagerProfiles/nestedEndpoints/read|Ruft einen geschachtelten Endpunkt ab, der einem Traffic Manager-Profil zugehörig ist, einschließlich aller Eigenschaften dieses geschachtelten Endpunkts.|
|/trafficManagerProfiles/nestedEndpoints/write|Fügt einen neuen geschachtelten Endpunkt in einem vorhandenen Traffic Manager-Profil hinzu oder aktualisiert die Eigenschaften eines vorhandenen geschachtelten Endpunkts in diesem Traffic Manager-Profil.|
|/trafficManagerProfiles/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellungen für den Traffic Manager ab.|
|/trafficManagerProfiles/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellungen für den Traffic Manager. Dieser Vorgang wird durch den Insights-Ressourcenanbieter unterstützt.|
|/trafficManagerProfiles/providers/Microsoft.Insights/logDefinitions/read|Ruft die Ereignisse für den Traffic Manager ab.|
|/trafficManagerProfiles/providers/Microsoft.Insights/metricDefinitions/read|Ruft die verfügbaren Metriken für den Traffic Manager ab.|
|/trafficManagerProfiles/read|Dient zum Abrufen der Traffic Manager-Profilkonfiguration. Dazu zählen DNS-Einstellungen, Routingeinstellungen für Datenverkehr, Endpunktüberwachungseinstellungen und die Liste mit den Endpunkten, die durch dieses Traffic Manager-Profil geroutet werden.|
|/trafficManagerProfiles/write|Dient zum Erstellen eines Traffic Manager-Profils oder zum Ändern der Konfiguration eines vorhandenen Traffic Manager-Profils. Dies schließt das Aktivieren oder Deaktivieren eines Profils sowie das Ändern von DNS-Einstellungen, Routingeinstellungen für Datenverkehr oder Endpunktüberwachungseinstellungen ein. Durch das Traffic Manager-Profil geroutete Endpunkte können hinzugefügt, entfernt, aktiviert oder deaktiviert werden.|
|/trafficManagerUserMetricsKeys/delete|Löscht die Schlüssel auf Abonnementebene, die für die Collection für Echtzeitbenutzermetriken verwendet wird.|
|/trafficManagerUserMetricsKeys/read|Ruft die Schlüssel auf Abonnementebene für die Collection mit Echtzeitbenutzermetriken ab.|
|/trafficManagerUserMetricsKeys/write|Erstellt einen neuen Schlüssel auf Abonnementebene, der für die Collection mit Echtzeitbenutzermetriken verwendet werden soll.|
|/unregister/action|Hebt die Registrierung des Abonnements auf.|
|/virtualHubs/delete|Löscht einen virtuellen Hub.|
|/virtualHubs/hubVirtualNetworkConnections/delete|Löscht HubVirtualNetworkConnection.|
|/virtualHubs/hubVirtualNetworkConnections/read|Ruft HubVirtualNetworkConnection ab.|
|/virtualHubs/hubVirtualNetworkConnections/write|Erstellt oder aktualisiert HubVirtualNetworkConnection.|
|/virtualHubs/read|Ruft einen virtuellen Hub ab.|
|/virtualHubs/write|Erstellt oder aktualisiert einen virtuellen Hub.|
|/virtualnetworkgateways/connections/read|Ruft VirtualNetworkGatewayConnection ab.|
|/virtualNetworkGateways/delete|Löscht virtualNetworkGateway.|
|/virtualnetworkgateways/generatevpnclientpackage/action|Generiert ein VpnClient-Paket für virtualNetworkGateway.|
|/virtualnetworkgateways/generatevpnprofile/action|Generiert ein VpnProfile-Paket für VirtualNetworkGateway.|
|/virtualnetworkgateways/getadvertisedroutes/action|Ruft angekündigte virtualNetworkGateway-Routen ab.|
|/virtualnetworkgateways/getbgppeerstatus/action|Ruft den Status des BGP-Peerings für virtualNetworkGateway ab.|
|/virtualnetworkgateways/getlearnedroutes/action|Ruft die erlernten virtualNetworkGateway-Routen ab.|
|/virtualnetworkgateways/getvpnclientipsecparameters/action|Ruft die Vpnclient-IPsec-Parameter für den VirtualNetworkGateway P2S-Client ab.|
|/virtualnetworkgateways/getvpnprofilepackageurl/action|Ruft die URL eines vorab generierten VPN-Clientprofilpakets ab.|
|/virtualNetworkGateways/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellungen für das Gateway des virtuellen Netzwerks ab.|
|/virtualNetworkGateways/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellungen für das Gateway des virtuellen Netzwerks. Dieser Vorgang wird durch den Insights-Ressourcenanbieter unterstützt.|
|/virtualNetworkGateways/providers/Microsoft.Insights/logDefinitions/read|Ruft die Ereignisse für das Gateway des virtuellen Netzwerks ab.|
|/virtualNetworkGateways/providers/Microsoft.Insights/metricDefinitions/read|Ruft die verfügbaren Metriken für das Gateway des virtuellen Netzwerks ab.|
|/virtualNetworkGateways/read|Ruft VirtualNetworkGateway ab.|
|/virtualnetworkgateways/reset/action|Setzt virtualNetworkGateway zurück.|
|/virtualnetworkgateways/setvpnclientipsecparameters/action|Legt die Vpnclient-IPsec-Parameter für den VirtualNetworkGateway P2S-Client fest.|
|/virtualnetworkgateways/supportedvpndevices/action|Listet die unterstützten VPN-Geräte auf.|
|/virtualNetworkGateways/write|Erstellt oder aktualisiert VirtualNetworkGateway.|
|/virtualNetworks/checkIpAddressAvailability/read|Prüft, ob eine IP-Adresse im angegebenen virtuellen Netzwerk verfügbar ist.|
|/virtualNetworks/customViews/get/action|Ruft den Inhalt einer benutzerdefinierten Ansicht eines virtuellen Netzwerks ab.|
|/virtualNetworks/customViews/read|Ruft die Definition einer benutzerdefinierten Ansicht des virtuellen Netzwerks ab.|
|/virtualNetworks/delete|Löscht ein virtuelles Netzwerk.|
|/virtualNetworks/peer/action|Richtet ein virtuelles Netzwerk als Peer für ein anderes virtuelles Netzwerk ein.|
|/virtualNetworks/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellungen des virtuellen Netzwerks ab.|
|/virtualNetworks/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellungen des virtuellen Netzwerks.|
|/virtualNetworks/providers/Microsoft.Insights/logDefinitions/read|Ruft die Protokolldefinitionen des virtuellen Netzwerks ab.|
|/virtualNetworks/providers/Microsoft.Insights/metricDefinitions/read|Ruft die Metrikdefinitionen des virtuellen Netzwerks ab.|
|/virtualNetworks/read|Dient zum Abrufen der Definition des virtuellen Netzwerks.|
|/virtualNetworks/remoteVirtualNetworkPeeringProxies/delete|Löscht den Proxy für das Peering virtueller Netzwerke.|
|/virtualNetworks/remoteVirtualNetworkPeeringProxies/read|Ruft eine Proxydefinition für das Peering virtueller Netzwerke ab.|
|/virtualNetworks/remoteVirtualNetworkPeeringProxies/write|Erstellt ein Proxy für das Peering virtueller Netzwerke oder aktualisiert ein vorhandenes Proxy für das Peering virtueller Netzwerke.|
|/virtualNetworks/subnets/delete|Löscht ein Subnetz für virtuelle Netzwerke.|
|/virtualNetworks/subnets/join/action|Verknüpft ein virtuelles Netzwerk.|
|/virtualNetworks/subnets/joinViaServiceEndpoint/action|Verknüpft Ressourcen wie etwa ein Speicherkonto oder eine SQL-Datenbank mit einem Subnetz.|
|/virtualNetworks/subnets/read|Ruft eine Subnetzdefinition für virtuelle Netzwerke ab.|
|/virtualNetworks/subnets/resourceNavigationLinks/delete|Löscht einen Navigationslink für die Ressource.|
|/virtualNetworks/subnets/resourceNavigationLinks/read|Ruft die Definition des Ressourcennavigationslinks ab.|
|/virtualNetworks/subnets/resourceNavigationLinks/write|Erstellt einen Ressourcennavigationslink oder aktualisiert einen vorhandenen Ressourcennavigationslink.|
|/virtualNetworks/subnets/virtualMachines/read|Ruft Verweise auf alle virtuellen Computer im Subnetz eines virtuellen Netzwerks ab.|
|/virtualNetworks/subnets/write|Erstellt ein Subnetz für virtuelle Netzwerke oder aktualisiert ein vorhandenes Subnetz für virtuelle Netzwerke.|
|/virtualNetworks/taggedTrafficConsumers/delete|Löscht einen markierten Datenverkehrsconsumer.|
|/virtualNetworks/taggedTrafficConsumers/read|Ruft die Definition eines markierten Datenverkehrsconsumers ab.|
|/virtualNetworks/taggedTrafficConsumers/validate/action|Überprüft einen markierten Datenverkehrsconsumer.|
|/virtualNetworks/taggedTrafficConsumers/write|Erstellt einen markierten Datenverkehrsconsumer oder aktualisiert einen vorhandenen markierten Datenverkehrsconsumer.|
|/virtualNetworks/usages/read|Ruft die IP-Adressenbelegungen für jedes Subnetz des virtuellen Netzwerks ab.|
|/virtualNetworks/virtualMachines/read|Ruft Verweise auf alle virtuellen Computer in einem virtuellen Netzwerk ab.|
|/virtualNetworks/virtualNetworkPeerings/delete|Löscht ein Peering virtueller Netzwerke.|
|/virtualNetworks/virtualNetworkPeerings/read|Ruft eine Definition für das Peering virtueller Netzwerke ab.|
|/virtualNetworks/virtualNetworkPeerings/write|Erstellt ein Peering virtueller Netzwerke oder aktualisiert ein vorhandenes Peering virtueller Netzwerke.|
|/virtualNetworks/write|Erstellt ein virtuelles Netzwerk oder aktualisiert ein vorhandenes virtuelles Netzwerk.|
|/virtualNetworkTaps/delete|Löscht einen TAP eines virtuellen Netzwerks.|
|/virtualNetworkTaps/join/action|Verknüpft einen TAP eines virtuellen Netzwerks.|
|/virtualNetworkTaps/read|Ruft einen TAP eines virtuellen Netzwerks ab.|
|/virtualNetworkTaps/write|Erstellt oder aktualisiert einen TAP eines virtuellen Netzwerks.|
|/virtualwans/delete|Löscht ein virtuelles WAN.|
|/virtualwans/read|Ruft ein virtuelles WAN ab.|
|/virtualWans/virtualHubProxies/delete|Löscht einen Proxy für einen virtuellen Hub.|
|/virtualWans/virtualHubProxies/read|Ruft die Definition für den Proxy eines virtuellen Hubs ab.|
|/virtualWans/virtualHubProxies/write|Erstellt oder aktualisiert einen Proxy für einen virtuellen Hub.|
|/virtualwans/virtualHubs/read|Ruft alle virtuellen Hubs ab, die einem virtuellen WAN zugeordnet sind.|
|/virtualwans/vpnconfiguration/read|Ruft eine VPN-Konfiguration ab.|
|/virtualWans/vpnSiteProxies/delete|Löscht einen Proxy für einen VPN-Standort.|
|/virtualWans/vpnSiteProxies/read|Ruft eine Proxydefinition für einen VPN-Standort-ab.|
|/virtualWans/vpnSiteProxies/write|Erstellt einen Proxy für einen VPN-Standort oder aktualisiert einen Proxy für einen VPN-Standort.|
|/virtualwans/vpnSites/read|Ruft alle VPN-Standorte ab, die einem virtuellen WAN zugeordnet sind.|
|/virtualwans/write|Erstellt oder aktualisiert ein virtuelles WAN.|
|/vpnGateways/read|Ruft VpnGateway ab.|
|/vpnGateways/vpnConnections/read|Ruft VpnConnection ab.|
|/vpnGateways/vpnConnections/write|Legt VpnConnection fest.|
|/vpnGateways/write|Legt VpnGateway fest.|
|/vpnsites/delete|Löscht eine Ressource für einen VPN-Standort.|
|/vpnsites/read|Ruft eine Ressource für einen VPN-Standort ab.|
|/vpnsites/write|Erstellt oder aktualisiert eine Ressource für einen VPN-Standort.|

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

| Vorgang | BESCHREIBUNG |
|---|---|
|/CheckNamespaceAvailability/action|Prüft, ob ein bestimmter Name einer Namespaceressource innerhalb des NotificationHub-Diensts verfügbar ist.|
|/Namespaces/authorizationRules/action|Dient zum Abrufen der Liste mit Beschreibungen für Namespace-Autorisierungsregeln.|
|/Namespaces/authorizationRules/delete|Dient zum Löschen von Namespace-Autorisierungsregeln. Die standardmäßige Namespace-Autorisierungsregel kann nicht gelöscht werden. |
|/Namespaces/authorizationRules/listkeys/action|Dient zum Abrufen der Verbindungszeichenfolge für den Namespace.|
|/Namespaces/authorizationRules/read|Dient zum Abrufen der Liste mit Beschreibungen für Namespace-Autorisierungsregeln.|
|/Namespaces/authorizationRules/regenerateKeys/action|Namespace-Autorisierungsregel zum erneuten Generieren des primären/sekundären Schlüssels. Geben Sie den Schlüssel an, der neu generiert werden soll.|
|/Namespaces/authorizationRules/write|Dient zum Erstellen von Autorisierungsregeln auf Namespaceebene und zum Aktualisieren der dazugehörigen Eigenschaften. Aktualisiert werden können die Zugriffsrechte für Autorisierungsregeln sowie der primäre und der sekundäre Schlüssel.|
|/Namespaces/CheckNotificationHubAvailability/action|Prüft, ob ein bestimmter NotificationHub-Name innerhalb eines Namespaces verfügbar ist.|
|/Namespaces/Delete|Dient zum Löschen von Namespaceressourcen.|
|/Namespaces/NotificationHubs/authorizationRules/action|Dient zum Abrufen der Liste mit Notification Hub-Autorisierungsregeln.|
|/Namespaces/NotificationHubs/authorizationRules/delete|Dient zum Löschen von Notification Hub-Autorisierungsregeln.|
|/Namespaces/NotificationHubs/authorizationRules/listkeys/action|Dient zum Abrufen der Notification Hub-Verbindungszeichenfolge.|
|/Namespaces/NotificationHubs/authorizationRules/read|Dient zum Abrufen der Liste mit Notification Hub-Autorisierungsregeln.|
|/Namespaces/NotificationHubs/authorizationRules/regenerateKeys/action|Notification Hub-Autorisierungsregel zum erneuten Generieren des primären/sekundären Schlüssels. Geben Sie den Schlüssel an, der neu generiert werden soll.|
|/Namespaces/NotificationHubs/authorizationRules/write|Dient zum Erstellen von Notification Hub-Autorisierungsregeln und zum Aktualisieren der dazugehörigen Eigenschaften. Aktualisiert werden können die Zugriffsrechte für Autorisierungsregeln sowie der primäre und der sekundäre Schlüssel.|
|/Namespaces/NotificationHubs/debugSend/action|Dient zum Senden einer Test-Pushbenachrichtigung.|
|/Namespaces/NotificationHubs/Delete|Dient zum Löschen von Notification Hub-Ressourcen.|
|/Namespaces/NotificationHubs/metricDefinitions/read|Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacemetriken.|
|/Namespaces/NotificationHubs/pnsCredentials/action|Dient zum Abrufen aller Notification Hub-PNS-Anmeldeinformationen. Hierzu zählen Anmeldeinformationen für WNS, MPNS, APNS, GCM und Baidu.|
|/Namespaces/NotificationHubs/read|Dient zum Abrufen einer Liste mit Notification Hub-Ressourcenbeschreibungen.|
|/Namespaces/NotificationHubs/write|Dient zum Erstellen eines Notification Hubs und zum Aktualisieren seiner Eigenschaften. Zu diesen Einstellungen zählen in erster Linie PNS-Anmeldeinformationen, Autorisierungsregeln und Gültigkeitsdauer.|
|/Namespaces/read|Dient zum Abrufen der Liste mit Beschreibungen für Namespaceressourcen.|
|/Namespaces/write|Dient zum Erstellen einer Namespaceressource und zum Aktualisieren ihrer Eigenschaften. Die Eigenschaften für Markierungen und Kapazität des Namespace können aktualisiert werden.|
|/register/action|Registriert das Abonnement für den NotificiationHubs-Ressourcenanbieter und ermöglicht die Erstellung von Namespaces und Notification Hubs.|

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

| Vorgang | BESCHREIBUNG |
|---|---|
|/linkTargets/read|Listet vorhandene Konten auf, die keinem Azure-Abonnement zugeordnet sind. Verwenden Sie eine Kunden-ID, die von diesem Vorgang in der Eigenschaft für die Kunden-ID des Vorgangs „Arbeitsbereich erstellen“ zurückgegeben wird, um dieses Azure-Abonnement mit einem Arbeitsbereich zu verknüpfen.|
|/register/action|Dient zum Registrieren eines Abonnements bei einem Ressourcenanbieter.|
|/workspaces/analytics/query/action|Führen Sie mit der neuen Engine eine Suche durch.|
|/workspaces/analytics/query/schema/read|Rufen Sie das Suchschema V2 ab.|
|/workspaces/api/query/action|Führen Sie mit der neuen Engine eine Suche durch.|
|/workspaces/api/query/schema/read|Rufen Sie das Suchschema V2 ab.|
|/workspaces/configurationScopes/delete|Dient zum Löschen von Konfigurationsbereichen.|
|/workspaces/configurationScopes/read|Dient zum Abrufen von Konfigurationsbereichen.|
|/workspaces/configurationScopes/write|Dient zum Festlegen von Konfigurationsbereichen.|
|/workspaces/datasources/delete|Dient zum Löschen von Datenquellen unter einem Arbeitsbereich.|
|/workspaces/datasources/read|Dient zum Abrufen von Datenquellen unter einem Arbeitsbereich.|
|/workspaces/datasources/write|Dient zum Erstellen/Aktualisieren von Datenquellen unter einem Arbeitsbereich.|
|/workspaces/delete|Löscht einen Arbeitsbereich. Wenn der Arbeitsbereich bei seiner Erstellung mit einem vorhandenen Arbeitsbereich verknüpft war, wird der Arbeitsbereich, mit dem er verknüpft war, nicht gelöscht.|
|/workspaces/generateregistrationcertificate/action|Generiert ein Registrierungszertifikat für den Arbeitsbereich. Dieses Zertifikat wird verwendet, um Microsoft System Center Operation Manager mit dem Arbeitsbereich zu verbinden.|
|/workspaces/intelligencepacks/disable/action|Deaktiviert ein Intelligence Pack für einen bestimmten Arbeitsbereich.|
|/workspaces/intelligencepacks/enable/action|Aktiviert ein Intelligence Pack für einen bestimmten Arbeitsbereich.|
|/workspaces/intelligencepacks/read|Listet alle für einen angegebenen Arbeitsbereich sichtbaren Intelligence Packs auf und gibt an, ob das Pack für diesen Arbeitsbereich aktiviert oder deaktiviert ist.|
|/workspaces/linkedServices/delete|Löscht verknüpfte Dienste im angegebenen Arbeitsbereich.|
|/workspaces/linkedServices/read|Ruft verknüpfte Dienste im angegebenen Arbeitsbereich ab.|
|/workspaces/linkedServices/write|Erstellt oder aktualisiert verknüpfte Dienste im angegebenen Arbeitsbereich.|
|/workspaces/listKeys/action|Ruft die Listenschlüssel für den Arbeitsbereich ab. Diese Schlüssel werden verwendet, um Microsoft Operational Insights-Agents mit dem Arbeitsbereich zu verbinden.|
|/workspaces/listKeys/read|Ruft die Listenschlüssel für den Arbeitsbereich ab. Diese Schlüssel werden verwendet, um Microsoft Operational Insights-Agents mit dem Arbeitsbereich zu verbinden.|
|/workspaces/managementGroups/read|Ruft die Namen und Metadaten für über System Center Operations Manager verwaltete Gruppen ab, die mit diesem Arbeitsbereich verbunden sind.|
|/workspaces/metricDefinitions/read|Ruft Metrikdefinitionen im Arbeitsbereich ab.|
|/workspaces/notificationSettings/delete|Löscht die Benachrichtigungseinstellungen für den Arbeitsbereich des Benutzers.|
|/workspaces/notificationSettings/read|Ruft die Benachrichtigungseinstellungen für den Arbeitsbereich des Benutzers ab.|
|/workspaces/notificationSettings/write|Legt die Benachrichtigungseinstellungen für den Arbeitsbereich des Benutzers fest.|
|/workspaces/purge/action|Löscht die angegebenen Daten aus dem Arbeitsbereich.|
|/workspaces/read|Ruft einen vorhandenen Arbeitsbereich ab.|
|/workspaces/savedSearches/delete|Löscht eine gespeicherte Suchabfrage.|
|/workspaces/savedSearches/read|Ruft eine gespeicherte Suchabfrage ab.|
|/workspaces/savedSearches/write|Erstellt eine gespeicherte Suchabfrage.|
|/workspaces/schema/read|Ruft das Suchschema für den Arbeitsbereich ab.  Ein Suchschema umfasst die verfügbar gemachten Felder und die dazugehörigen Typen.|
|/workspaces/search/action|Führt eine Suchabfrage aus.|
|/workspaces/sharedKeys/action|Ruft die gemeinsam verwendeten Schlüssel für den Arbeitsbereich ab. Diese Schlüssel werden verwendet, um Microsoft Operational Insights-Agents mit dem Arbeitsbereich zu verbinden.|
|/workspaces/sharedKeys/read|Ruft die gemeinsam verwendeten Schlüssel für den Arbeitsbereich ab. Diese Schlüssel werden verwendet, um Microsoft Operational Insights-Agents mit dem Arbeitsbereich zu verbinden.|
|/workspaces/storageinsightconfigs/delete|Löscht eine Speicherkonfiguration. Daraufhin werden von Microsoft Operational Insights keine Daten mehr aus dem Speicherkonto gelesen.|
|/workspaces/storageinsightconfigs/read|Ruft eine Speicherkonfiguration ab.|
|/workspaces/storageinsightconfigs/write|Erstellt eine neue Speicherkonfiguration. Diese Konfigurationen werden dazu verwendet, Daten von einem Speicherort in einem vorhandenen Speicherkonto abzurufen.|
|/workspaces/usages/read|Ruft Nutzungsdaten für einen Arbeitsbereich ab, einschließlich der durch den Arbeitsbereich gelesenen Datenmenge.|
|/workspaces/write|Erstellt einen neuen Arbeitsbereich oder stellt eine Verknüpfung mit einem vorhandenen Arbeitsbereich her, indem die Kunden-ID für den vorhandenen Arbeitsbereich bereitgestellt wird.|

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

| Vorgang | BESCHREIBUNG |
|---|---|
|/managementAssociations/delete|Löscht eine vorhandene Verwaltungszuordnung.|
|/managementAssociations/read|Ruft eine vorhandene Verwaltungszuordnung ab.|
|/managementAssociations/write|Erstellt eine neue Verwaltungszuordnung.|
|/managementConfigurations/delete|Löscht eine vorhandene Verwaltungskonfiguration.|
|/managementConfigurations/read|Ruft eine vorhandene Verwaltungskonfiguration ab.|
|/managementConfigurations/write|Erstellt eine vorhandene Verwaltungskonfiguration.|
|/register/action|Dient zum Registrieren eines Abonnements bei einem Ressourcenanbieter.|
|/solutions/delete|Dient zum Löschen vorhandener OMS-Lösungen.|
|/solutions/read|Dient zum Abrufen vorhandener OMS-Lösungen.|
|/solutions/write|Dient zum Erstellen neuer OMS-Lösungen.|

## <a name="microsoftportal"></a>Microsoft.Portal

| Vorgang | BESCHREIBUNG |
|---|---|
|/dashboards/delete|Entfernt das Dashboard aus dem Abonnement.|
|/dashboards/read|Liest die Dashboards für das Abonnement.|
|/dashboards/write|Fügt ein Dashboard zu einem Abonnement hinzu oder ändert es.|

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

| Vorgang | BESCHREIBUNG |
|---|---|
|/capacities/checkNameAvailability/action|Überprüft, ob der Name für die angegebene Power BI-Kapazität gültig ist und nicht verwendet wird.|
|/capacities/delete|Löscht die dedizierte Power BI-Kapazität.|
|/capacities/providers/Microsoft.Insights/metricDefinitions/read|Ruft die verfügbaren Metriken für die dedizierte Power BI-Kapazität ab.|
|/capacities/read|Ruft die Informationen der angegebenen dedizierten Power BI-Kapazität ab.|
|/capacities/write|Erstellt oder aktualisiert die angegebene dedizierte Power BI-Kapazität.|

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

| Vorgang | BESCHREIBUNG |
|---|---|
|/locations/allocatedStamp/read|„GetAllocatedStamp“ ist ein interner Vorgang des Diensts.|
|/locations/allocateStamp/action|„AllocateStamp“ ist ein interner Vorgang des Diensts.|
|/locations/backupPreValidateProtection/action||
|/locations/backupStatus/action|Überprüft den Sicherungsstatus für Recovery Services-Tresore.|
|/locations/backupValidateFeatures/action|Überprüft Features.|
|/operations/read|Der Vorgang gibt die Liste der Vorgänge für einen Ressourcenanbieter zurück.|
|/register/action|Registriert das Abonnement für den angegebenen Ressourcenanbieter.|
|/Vaults/backupconfig/read|Gibt die Konfiguration für Recovery Services-Tresore zurück.|
|/Vaults/backupconfig/write|Aktualisiert die Konfiguration für Recovery Services-Tresore.|
|/Vaults/backupEngines/read|Gibt alle Sicherungsverwaltungsserver zurück, die beim Tresor registriert sind.|
|/Vaults/backupFabrics/{fabricName}/protectionContainers/{containerName}/items/read|Ruft alle Elemente in einem Container ab.|
|/Vaults/backupFabrics/backupProtectionIntent/write|Erstellt einen beabsichtigten Sicherungsschutz.|
|/Vaults/backupFabrics/operationResults/read|Gibt den Status des Vorgangs zurück.|
|/Vaults/backupFabrics/protectableContainers/read|Ruft alle schützbaren Container ab.|
|/Vaults/backupFabrics/protectionContainers/inquire/action|Führt die Abfrage für Workloads innerhalb eines Containers durch.|
|/Vaults/backupFabrics/protectionContainers/operationResults/read|Ruft das Ergebnis eines Vorgangs ab, der für den Schutzcontainer ausgeführt wurde.|
|/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action|Führt eine Sicherung für geschützte Elemente aus.|
|/Vaults/backupFabrics/protectionContainers/protectedItems/delete|Löscht geschützte Elemente.|
|/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read|Ruft das Ergebnis eines Vorgangs ab, der für geschützte Elemente ausgeführt wurde.|
|/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read|Gibt den Status eines Vorgangs zurück, der für geschützte Elemente ausgeführt wurde.|
|/Vaults/backupFabrics/protectionContainers/protectedItems/read|Gibt Objektdetails des geschützten Elements zurück.|
|/Vaults/backupFabrics/protectionContainers/protectedItems/ recoveryPoints/provisionInstantItemRecovery/action|Dient zum Bereitstellen der sofortigen Elementwiederherstellung für geschützte Elemente.|
|/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read|Dient zum Abrufen von Wiederherstellungspunkten für geschützte Elemente.|
|/Vaults/backupFabrics/protectionContainers/protectedItems/ recoveryPoints/restore/action|Dient zum Wiederherstellen von Wiederherstellungspunkten für geschützte Elemente.|
|/Vaults/backupFabrics/protectionContainers/protectedItems/ recoveryPoints/revokeInstantItemRecovery/action|Dient zum Widerrufen der sofortigen Elementwiederherstellung für geschützte Elemente.|
|/Vaults/backupFabrics/protectionContainers/protectedItems/write|Dient zum Erstellen eines geschützten Elements für die Sicherung.|
|/Vaults/backupFabrics/protectionContainers/read|Gibt alle registrierten Container zurück.|
|/Vaults/backupFabrics/protectionContainers/write|Erstellt einen registrierten Container.|
|/Vaults/backupFabrics/refreshContainers/action|Aktualisiert die Containerliste.|
|/Vaults/backupJobs/cancel/action|Dient zum Abbrechen des Auftrags.|
|/Vaults/backupJobs/operationResults/read|Gibt das Ergebnis von Auftragsvorgängen zurück.|
|/Vaults/backupJobs/read|Gibt alle Auftragsobjekte zurück.|
|/Vaults/backupJobsExport/action|Dient zum Exportieren von Aufträgen.|
|/Vaults/backupJobsExport/operationResults/read|Gibt das Ergebnis des Vorgangs „Auftrag exportieren“ zurück.|
|/Vaults/backupManagementMetaData/read|Gibt Sicherungsverwaltungs-Metadaten für Recovery Services-Tresore zurück.|
|/Vaults/backupOperationResults/read|Gibt das Ergebnis eines Sicherungsvorgangs für Recovery Services-Tresore zurück.|
|/Vaults/backupOperations/read|Gibt den Status eines Sicherungsvorgangs für Recovery Services-Tresore zurück.|
|/Vaults/backupPolicies/delete|Dient zum Löschen einer Schutzrichtlinie.|
|/Vaults/backupPolicies/operationResults/read|Dient zum Abrufen der Ergebnisse von Richtlinienvorgängen.|
|/Vaults/backupPolicies/operations/read|Dient zum Abrufen des Status von Richtlinienvorgängen.|
|/Vaults/backupPolicies/read|Gibt alle Schutzrichtlinien zurück.|
|/Vaults/backupPolicies/write|Erstellt Schutzrichtlinien.|
|/Vaults/backupProtectableItems/read|Gibt die Liste mit allen schützbaren Elementen zurück.|
|/Vaults/backupProtectedItems/read|Gibt die Liste mit allen geschützten Elementen zurück.|
|/Vaults/backupProtectionContainers/read|Gibt alle zum Abonnement gehörenden Container zurück.|
|/Vaults/backupSecurityPIN/action|Gibt Informationen zur Sicherheits-PIN für Recovery Services-Tresore zurück.|
|/Vaults/backupstorageconfig/read|Gibt die Speicherkonfiguration für Recovery Services-Tresore zurück.|
|/Vaults/backupstorageconfig/write|Aktualisiert Speicherkonfiguration für Recovery Services-Tresore.|
|/Vaults/backupUsageSummaries/read|Gibt Zusammenfassungen für geschützte Elemente und geschützte Server für einen Recovery Services-Tresor zurück.|
|/Vaults/certificates/write|Der Vorgang „Ressourcenzertifikat aktualisieren“ aktualisiert das Zertifikat für die Ressourcen-/Tresoranmeldeinformationen.|
|/Vaults/delete|Der Vorgang „Tresor löschen“ löscht die angegebene Azure-Ressource vom Typ „Tresor“.|
|/Vaults/extendedInformation/delete|Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt.|
|/Vaults/extendedInformation/read|Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt.|
|/Vaults/extendedInformation/write|Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt.|
|/Vaults/monitoringAlerts/read|Ruft die Warnungen für den Recovery Services-Tresor ab.|
|/Vaults/monitoringAlerts/write|Löst die Warnung auf.|
|/Vaults/monitoringConfigurations/read|Ruft die Konfiguration von Recovery Services-Tresorbenachrichtigungen ab.|
|/Vaults/monitoringConfigurations/write|Konfiguriert E-Mail-Benachrichtigungen für den Recovery Services-Tresor.|
|/Vaults/providers/Microsoft.Insights/diagnosticSettings/read|Azure Backup-Diagnose|
|/Vaults/providers/Microsoft.Insights/diagnosticSettings/write|Azure Backup-Diagnose|
|/Vaults/providers/Microsoft.Insights/logDefinitions/read|Azure Backup-Protokolle|
|/Vaults/providers/Microsoft.Insights/metricDefinitions/read|Azure Backup-Metriken|
|/Vaults/read|Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt.|
|/Vaults/registeredIdentities/delete|Der Vorgang „Registrierung des Containers aufheben“ kann zum Aufheben der Registrierung eines Containers verwendet werden.|
|/Vaults/registeredIdentities/operationResults/read|Mit dem Vorgang „Vorgangsergebnisse abrufen“ können der Vorgangsstatus und das Ergebnis für den asynchron übermittelten Vorgang abgerufen werden.|
|/Vaults/registeredIdentities/read|Der Vorgang „Container abrufen“ kann zum Abrufen der für eine Ressource registrierten Container verwendet werden.|
|/Vaults/registeredIdentities/write|Der Vorgang „Dienstcontainer registrieren“ kann zum Registrieren eines Containers beim Wiederherstellungsdienst verwendet werden.|
|/vaults/replicationAlertSettings/read|Dient zum Lesen beliebiger Warnungseinstellungen.|
|/vaults/replicationAlertSettings/write|Dient zum Erstellen oder Aktualisieren beliebiger Warnungseinstellungen.|
|/vaults/replicationEvents/read|Dient zum Lesen beliebiger Ereignisse.|
|/vaults/replicationFabrics/checkConsistency/action|Prüft die Konsistenz des Fabrics.|
|/vaults/replicationFabrics/delete|Dient zum Löschen beliebiger Fabrics.|
|/vaults/replicationFabrics/deployProcessServerImage/action|Dient zum Bereitstellen von Prozessserverimages.|
|/vaults/replicationFabrics/read|Dient zum Lesen beliebiger Fabrics.|
|/vaults/replicationFabrics/reassociateGateway/action|Dient zum Neuzuordnen des Gateways.|
|/vaults/replicationFabrics/remove/action|Dient zum Entfernen von Fabrics.|
|/vaults/replicationFabrics/renewcertificate/action|Verlängert das Zertifikat für Fabrics.|
|/vaults/replicationFabrics/replicationNetworks/read|Dient zum Lesen beliebiger Netzwerke.|
|/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/delete|Dient zum Löschen beliebiger Netzwerkzuordnungen.|
|/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read|Dient zum Lesen beliebiger Netzwerkzuordnungen.|
|/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/write|Dient zum Erstellen oder Aktualisieren beliebiger Netzwerkzuordnungen.|
|/vaults/replicationFabrics/replicationProtectionContainers/ discoverProtectableItem/action|Dient zum Ermitteln des schützbaren Elements.|
|/vaults/replicationFabrics/replicationProtectionContainers/read|Dient zum Lesen beliebiger Schutzcontainer.|
|/vaults/replicationFabrics/replicationProtectionContainers/remove/action|Dient zum Entfernen von Schutzcontainern.|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectableItems/read|Dient zum Lesen beliebiger schützbarer Elemente.|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/applyRecoveryPoint/action|Dient zum Anwenden des Wiederherstellungspunkts.|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/delete|Dient zum Löschen beliebiger geschützter Elemente.|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/failoverCommit/action|Failovercommit|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/plannedFailover/action|Geplantes Failover|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/read|Dient zum Lesen beliebiger geschützter Elemente.|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/recoveryPoints/read|Dient zum Lesen beliebiger Wiederherstellungspunkte für die Replikation.|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/remove/action|Dient zum Entfernen geschützter Elemente.|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/repairReplication/action|Dient zum Reparieren der Replikation.|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/reProtect/action|Dient zum erneuten Schützen geschützter Elemente.|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/testFailover/action|Testfailover|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/testFailoverCleanup/action|Testfailoverbereinigung|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/unplannedFailover/action|Failover|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/updateMobilityService/action|Dient zum Aktualisieren von Mobility Service.|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectedItems/write|Dient zum Erstellen oder Aktualisieren beliebiger geschützter Elemente.|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectionContainerMappings/delete|Dient zum Löschen beliebiger Schutzcontainerzuordnungen.|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectionContainerMappings/read|Dient zum Lesen beliebiger Schutzcontainerzuordnungen.|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectionContainerMappings/remove/action|Dient zum Entfernen von Schutzcontainerzuordnungen.|
|/vaults/replicationFabrics/replicationProtectionContainers/ replicationProtectionContainerMappings/write|Dient zum Erstellen oder Aktualisieren beliebiger Schutzcontainerzuordnungen.|
|/vaults/replicationFabrics/replicationProtectionContainers/switchprotection/action|Dient zum Wechseln von Schutzcontainern.|
|/vaults/replicationFabrics/replicationProtectionContainers/write|Dient zum Erstellen oder Aktualisieren beliebiger Schutzcontainer.|
|/vaults/replicationFabrics/replicationRecoveryServicesProviders/delete|Dient zum Löschen beliebiger Recovery Services-Anbieter.|
|/vaults/replicationFabrics/replicationRecoveryServicesProviders/read|Dient zum Lesen beliebiger Recovery Services-Anbieter.|
|/vaults/replicationFabrics/replicationRecoveryServicesProviders/ refreshProvider/action|Dient zum Aktualisieren des Anbieters.|
|/vaults/replicationFabrics/replicationRecoveryServicesProviders/remove/action|Dient zum Entfernen von Recovery Services-Anbietern.|
|/vaults/replicationFabrics/replicationRecoveryServicesProviders/write|Dient zum Erstellen oder Aktualisieren beliebiger Recovery Services-Anbieter.|
|/vaults/replicationFabrics/replicationStorageClassifications/read|Dient zum Lesen beliebiger Speicherklassifizierungen.|
|/vaults/replicationFabrics/replicationStorageClassifications/ replicationStorageClassificationMappings/delete|Dient zum Löschen beliebiger Speicherklassifizierungszuordnungen.|
|/vaults/replicationFabrics/replicationStorageClassifications/ replicationStorageClassificationMappings/read|Dient zum Lesen beliebiger Speicherklassifizierungszuordnungen.|
|/vaults/replicationFabrics/replicationStorageClassifications/ replicationStorageClassificationMappings/write|Dient zum Erstellen oder Aktualisieren beliebiger Speicherklassifizierungszuordnungen.|
|/vaults/replicationFabrics/replicationvCenters/delete|Dient zum Löschen beliebiger Aufträge.|
|/vaults/replicationFabrics/replicationvCenters/read|Dient zum Lesen beliebiger Aufträge.|
|/vaults/replicationFabrics/replicationvCenters/write|Dient zum Erstellen oder Aktualisieren beliebiger Aufträge.|
|/vaults/replicationFabrics/write|Dient zum Erstellen oder Aktualisieren beliebiger Fabrics.|
|/vaults/replicationJobs/cancel/action|Dient zum Abbrechen von Aufträgen.|
|/vaults/replicationJobs/read|Dient zum Lesen beliebiger Aufträge.|
|/vaults/replicationJobs/restart/action|Dient zum Neustarten von Aufträgen.|
|/vaults/replicationJobs/resume/action|Dient zum Fortsetzen von Aufträgen.|
|/vaults/replicationPolicies/delete|Dient zum Löschen beliebiger Richtlinien.|
|/vaults/replicationPolicies/read|Dient zum Lesen beliebiger Richtlinien.|
|/vaults/replicationPolicies/write|Dient zum Erstellen oder Aktualisieren beliebiger Richtlinien.|
|/vaults/replicationRecoveryPlans/delete|Dient zum Löschen beliebiger Wiederherstellungspläne.|
|/vaults/replicationRecoveryPlans/failoverCommit/action|Wiederherstellungsplan für Failovercommit|
|/vaults/replicationRecoveryPlans/plannedFailover/action|Wiederherstellungsplan für geplantes Failover|
|/vaults/replicationRecoveryPlans/read|Dient zum Lesen beliebiger Wiederherstellungspläne.|
|/vaults/replicationRecoveryPlans/reProtect/action|Wiederherstellungsplan für erneutes Schützen|
|/vaults/replicationRecoveryPlans/testFailover/action|Wiederherstellungsplan für Testfailover|
|/vaults/replicationRecoveryPlans/testFailoverCleanup/action|Wiederherstellungsplan für Testfailoverbereinigung|
|/vaults/replicationRecoveryPlans/unplannedFailover/action|Wiederherstellungsplan für Failover|
|/vaults/replicationRecoveryPlans/write|Dient zum Erstellen oder Aktualisieren beliebiger Wiederherstellungspläne.|
|/Vaults/tokenInfo/read|Gibt Tokeninformationen für Recovery Services-Tresore zurück.|
|/vaults/usages/read|Dient zum Lesen beliebiger Tresorverwendungen.|
|/Vaults/usages/read|Gibt Nutzungsdetails für einen Recovery Services-Tresor zurück.|
|/Vaults/vaultTokens/read|Der Vorgang „Tresortoken“ kann zum Abrufen des Tresortokens für Back-End-Vorgänge auf Tresorebene verwendet werden.|
|/Vaults/write|Der Vorgang „Tresor erstellen“ erstellt eine Azure-Ressource vom Typ „Tresor“.|

## <a name="microsoftrelay"></a>Microsoft.Relay

| Vorgang | BESCHREIBUNG |
|---|---|
|/checkNameAvailability/action|Prüft die Verfügbarkeit des Namespaces unter dem angegebenen Abonnement.|
|/checkNamespaceAvailability/action|Prüft die Verfügbarkeit des Namespaces unter dem angegebenen Abonnement. Diese API ist veraltet. Verwenden Sie stattdessen CheckNameAvailabiltiy.|
|/namespaces/authorizationRules/action|Dient zum Aktualisieren von Namespace-Autorisierungsregeln. Diese API ist veraltet. Verwenden Sie stattdessen einen PUT-Aufruf, um die Namespace-Autorisierungsregel zu aktualisieren. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt.|
|/namespaces/authorizationRules/delete|Dient zum Löschen von Namespace-Autorisierungsregeln. Die standardmäßige Namespace-Autorisierungsregel kann nicht gelöscht werden. |
|/namespaces/authorizationRules/listkeys/action|Dient zum Abrufen der Verbindungszeichenfolge für den Namespace.|
|/namespaces/authorizationRules/read|Dient zum Abrufen der Liste mit Beschreibungen für Namespace-Autorisierungsregeln.|
|/namespaces/authorizationRules/regenerateKeys/action|Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource.|
|/namespaces/authorizationRules/write|Dient zum Erstellen von Autorisierungsregeln auf Namespaceebene und zum Aktualisieren der dazugehörigen Eigenschaften. Aktualisiert werden können die Zugriffsrechte für Autorisierungsregeln sowie der primäre und der sekundäre Schlüssel.|
|/namespaces/Delete|Dient zum Löschen von Namespaceressourcen.|
|/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action|Ruft die Schlüssel der Autorisierungsregeln für den primären Namespace für die Notfallwiederherstellung.|
|/namespaces/disasterRecoveryConfigs/authorizationRules/read|Ruft die Autorisierungsregeln des primären Namespace für die Notfallwiederherstellung ab.|
|/namespaces/disasterRecoveryConfigs/breakPairing/action|Hiermit wird die Notfallwiederherstellung deaktiviert und die Replikation von Änderungen vom primären Namespace zu sekundären Namespaces beendet.|
|/namespaces/disasterrecoveryconfigs/checkNameAvailability/action|Prüft die Verfügbarkeit des Namespacealias unter dem angegebenen Abonnement.|
|/namespaces/disasterRecoveryConfigs/delete|Hiermit wird die dem Namespace zugeordnete Notfallwiederherstellungskonfiguration gelöscht. Dieser Vorgang kann nur über den primären Namespace aufgerufen werden.|
|/namespaces/disasterRecoveryConfigs/failover/action|Hiermit wird ein Failover bei der georedundanten Notfallwiederherstellung ausgelöst und der Namespacealias neu konfiguriert, um auf den sekundären Namespace zu zeigen.|
|/namespaces/disasterRecoveryConfigs/read|Hiermit wird die dem Namespace zugeordnete Notfallwiederherstellungskonfiguration abgerufen.|
|/namespaces/disasterRecoveryConfigs/write|Hiermit wird die dem Namespace zugeordnete Notfallwiederherstellungskonfiguration erstellt oder aktualisiert.|
|/namespaces/HybridConnections/authorizationRules/action|Vorgang zum Aktualisieren von HybridConnection. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. Autorisierungsregeln. Verwenden Sie einen PUT-Aufruf, um die Autorisierungsregel zu aktualisieren.|
|/namespaces/HybridConnections/authorizationRules/delete|Vorgang zum Löschen von HybridConnection-Autorisierungsregeln.|
|/namespaces/HybridConnections/authorizationRules/listkeys/action|Dient zum Abrufen der HybridConnection-Verbindungszeichenfolge.|
|/namespaces/HybridConnections/authorizationRules/read| Ruft die Liste der HybridConnection-Autorisierungsregeln ab.|
|/namespaces/HybridConnections/authorizationRules/regeneratekeys/action|Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource.|
|/namespaces/HybridConnections/authorizationRules/write|Dient zum Erstellen von HybridConnection-Autorisierungsregeln und zum Aktualisieren der dazugehörigen Eigenschaften. Die Zugriffsrechte für Autorisierungsregeln können aktualisiert werden.|
|/namespaces/HybridConnections/Delete|Vorgang zum Löschen von HybridConnection-Ressourcen.|
|/namespaces/HybridConnections/read|Dient zum Abrufen einer Liste mit HybridConnection-Ressourcenbeschreibungen.|
|/namespaces/HybridConnections/write|Dient zum Erstellen oder Aktualisieren von HybridConnection-Eigenschaften.|
|/namespaces/messagingPlan/read|Hiermit wird der Messagingplan für einen Namespace abgerufen. Diese API ist veraltet. Über die MessagingPlan-Ressource verfügbar gemachte Eigenschaften werden in höheren API-Versionen in die (übergeordnete) Namespaceressource verschoben. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt.|
|/namespaces/messagingPlan/write|Hiermit wird der Messagingplan für einen Namespace aktualisiert. Diese API ist veraltet. Über die MessagingPlan-Ressource verfügbar gemachte Eigenschaften werden in höheren API-Versionen in die (übergeordnete) Namespaceressource verschoben. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt.|
|/namespaces/operationresults/read|Hiermit wird der Status des Namespacevorgangs abgerufen.|
|/namespaces/providers/Microsoft.Insights/metricDefinitions/read|Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacemetriken.|
|/namespaces/read|Dient zum Abrufen der Liste mit Beschreibungen für Namespaceressourcen.|
|/namespaces/WcfRelays/authorizationRules/action|Vorgang zum Aktualisieren von WcfRelay. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. Autorisierungsregeln. Verwenden Sie einen PUT-Aufruf, um die Autorisierungsregel zu aktualisieren.|
|/namespaces/WcfRelays/authorizationRules/delete|Vorgang zum Löschen von WcfRelay-Autorisierungsregeln.|
|/namespaces/WcfRelays/authorizationRules/listkeys/action|Dient zum Abrufen der WcfRelay-Verbindungszeichenfolge.|
|/namespaces/WcfRelays/authorizationRules/read| Dient zum Abrufen der WcfRelay-Autorisierungsregeln.|
|/namespaces/WcfRelays/authorizationRules/regeneratekeys/action|Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource.|
|/namespaces/WcfRelays/authorizationRules/write|Dient zum Erstellen von WcfRelay-Autorisierungsregeln und zum Aktualisieren der dazugehörigen Eigenschaften. Die Zugriffsrechte für Autorisierungsregeln können aktualisiert werden.|
|/namespaces/WcfRelays/Delete|Vorgang zum Löschen von WcfRelay-Ressourcen.|
|/namespaces/WcfRelays/read|Dient zum Abrufen einer Liste mit WcfRelay-Ressourcenbeschreibungen.|
|/namespaces/WcfRelays/write|Dient zum Erstellen oder Aktualisieren von WcfRelay-Eigenschaften.|
|/namespaces/write|Dient zum Erstellen einer Namespaceressource und zum Aktualisieren ihrer Eigenschaften. Die Eigenschaften für Markierungen und Kapazität des Namespace können aktualisiert werden.|
|/operations/read|Dient zum Abrufen von Vorgängen.|
|/register/action|Registriert das Abonnement für den Relayressourcenanbieter und ermöglicht die Erstellung von Relayressourcen.|
|/unregister/action|Registriert das Abonnement für den Relayressourcenanbieter und ermöglicht die Erstellung von Relayressourcen.|

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

| Vorgang | BESCHREIBUNG |
|---|---|
|/AvailabilityStatuses/current/read|Ruft den Verfügbarkeitsstatus für die angegebene Ressource ab.|
|/AvailabilityStatuses/read|Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab.|
|/healthevent/action|Zeigt die Änderung des Integritätszustands für die angegebene Ressource an.|
|/healthevent/Activated/action|Zeigt die Änderung des Integritätszustands für die angegebene Ressource an.|
|/healthevent/InProgress/action|Zeigt die Änderung des Integritätszustands für die angegebene Ressource an.|
|/healthevent/Pending/action|Zeigt die Änderung des Integritätszustands für die angegebene Ressource an.|
|/healthevent/Resolved/action|Zeigt die Änderung des Integritätszustands für die angegebene Ressource an.|
|/healthevent/Updated/action|Zeigt die Änderung des Integritätszustands für die angegebene Ressource an.|
|/register/action|Hiermit wird das Abonnement für Microsoft ResourceHealth registriert.|

## <a name="microsoftresources"></a>Microsoft.Resources

| Vorgang | BESCHREIBUNG |
|---|---|
|/checkResourceName/action|Dient zum Überprüfen der Gültigkeit des Ressourcennamens.|
|/deployments/cancel/action|Bricht eine Bereitstellung ab.|
|/deployments/delete|Löscht eine Bereitstellung.|
|/deployments/operations/read|Ruft Bereitstellungsvorgänge ab oder listet sie auf.|
|/deployments/read|Ruft Bereitstellungen ab oder listet sie auf.|
|/deployments/validate/action|Überprüft eine Bereitstellung.|
|/deployments/write|Erstellt oder aktualisiert eine Bereitstellung.|
|/links/delete|Löscht einen Ressourcenlink.|
|/links/read|Ruft Ressourcenlinks ab oder listet sie auf.|
|/links/write|Erstellt oder aktualisiert einen Ressourcenlink.|
|/marketplace/purchase/action|Führt zum Erwerb einer Ressource im Marketplace.|
|/providers/read|Dient zum Abrufen der Anbieterliste.|
|/resources/read|Dient zum Abrufen einer gefilterten Ressourcenliste.|
|/subscriptions/locations/read|Ruft die Liste mit den unterstützten Standorten ab.|
|/subscriptions/operationresults/read|Dient zum Abrufen der Ergebnisse des Abonnementvorgangs.|
|/subscriptions/providers/read|Ruft Ressourcenanbieter ab oder listet sie auf.|
|/subscriptions/read|Ruft die Abonnementliste ab.|
|/subscriptions/resourceGroups/delete|Löscht eine Ressourcengruppe und alle dazugehörigen Ressourcen.|
|/subscriptions/resourcegroups/deployments/operations/read|Ruft Bereitstellungsvorgänge ab oder listet sie auf.|
|/subscriptions/resourcegroups/deployments/operationstatuses/read|Ruft den Status von Bereitstellungsvorgängen ab oder listet ihn auf.|
|/subscriptions/resourcegroups/deployments/read|Ruft Bereitstellungen ab oder listet sie auf.|
|/subscriptions/resourcegroups/deployments/write|Erstellt oder aktualisiert eine Bereitstellung.|
|/subscriptions/resourceGroups/moveResources/action|Verschiebt Ressourcen aus einer Ressourcengruppe in eine andere.|
|/subscriptions/resourceGroups/read|Ruft Ressourcengruppen ab oder listet sie auf.|
|/subscriptions/resourcegroups/resources/read|Ruft die Ressourcen für die Ressourcengruppe ab.|
|/subscriptions/resourceGroups/validateMoveResources/action|Überprüft das Verschieben von Ressourcen aus einer Ressourcengruppe in eine andere.|
|/subscriptions/resourceGroups/write|Erstellt oder aktualisiert eine Ressourcengruppe.|
|/subscriptions/resources/read|Ruft die Ressourcen eines Abonnements ab.|
|/subscriptions/tagNames/delete|Löscht ein Abonnementtag.|
|/subscriptions/tagNames/read|Ruft Abonnementtags ab oder listet sie auf.|
|/subscriptions/tagNames/tagValues/delete|Löscht einen Abonnementtagwert.|
|/subscriptions/tagNames/tagValues/read|Ruft Abonnementtagwerte ab oder listet sie auf.|
|/subscriptions/tagNames/tagValues/write|Fügt einen Abonnementtagwert hinzu.|
|/subscriptions/tagNames/write|Fügt ein Abonnementtag hinzu.|
|/tenants/read|Ruft die Mandantenliste ab.|

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

| Vorgang | BESCHREIBUNG |
|---|---|
|/jobcollections/delete|Löscht Auftragssammlungen.|
|/jobcollections/disable/action|Deaktiviert Auftragssammlungen.|
|/jobcollections/enable/action|Aktiviert Auftragssammlungen.|
|/jobcollections/jobs/delete|Löscht Aufträge.|
|/jobcollections/jobs/generateLogicAppDefinition/action|Generiert Logik-App-Definitionen auf der Grundlage eines Scheduler-Auftrags.|
|/jobcollections/jobs/jobhistories/read|Ruft den Auftragsverlauf ab.|
|/jobcollections/jobs/read|Ruft Aufträge ab.|
|/jobcollections/jobs/run/action|Führt Aufträge aus.|
|/jobcollections/jobs/write|Erstellt oder aktualisiert Aufträge.|
|/jobcollections/read|Dient zum Abrufen von Auftragssammlungen.|
|/jobcollections/write|Erstellt oder aktualisiert Auftragssammlungen.|

## <a name="microsoftsearch"></a>Microsoft.Search

| Vorgang | BESCHREIBUNG |
|---|---|
|/checkNameAvailability/action|Prüft die Verfügbarkeit des Dienstnamens.|
|/register/action|Registriert das Abonnement für den Search-Ressourcenanbieter und ermöglicht die Erstellung von Suchdiensten.|
|/searchServices/createQueryKey/action|Erstellt den Abfrageschlüssel.|
|/searchServices/delete|Löscht den Suchdienst.|
|/searchServices/diagnosticSettings/read|Ruft die gelesene Diagnoseeinstellung für die Ressource ab.|
|/searchServices/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/searchServices/listAdminKeys/action|Liest die Administratorschlüssel.|
|/searchServices/logDefinitions/read|Ruft die verfügbaren Protokolle für den Suchdienst ab.|
|/searchServices/metricDefinitions/read|Ruft die verfügbaren Metriken für den Suchdienst ab.|
|/searchServices/queryKey/delete|Löscht den Abfrageschlüssel.|
|/searchServices/queryKey/read|Liest die Abfrageschlüssel.|
|/searchServices/read|Liest den Suchdienst.|
|/searchServices/regenerateAdminKey/action|Generiert die Administratorschlüssel neu.|
|/searchServices/start/action|Startet den Suchdienst.|
|/searchServices/stop/action|Beendet den Suchdienst.|
|/searchServices/write|Erstellt oder aktualisiert den Suchdienst.|

## <a name="microsoftsecurity"></a>Microsoft.Security

| Vorgang | BESCHREIBUNG |
|---|---|
|/alerts/read|Ruft alle verfügbaren Sicherheitswarnungen ab.|
|/applicationWhitelistings/read|Ruft die Anwendungswhitelists ab.|
|/applicationWhitelistings/write|Erstellt eine neue Anwendungswhitelist oder aktualisiert eine bereits vorhandene.|
|/complianceResults/read|Ruft die Konformitätsergebnisse für die Ressource ab.|
|/locations/alerts/activate/action|Aktiviert eine Sicherheitswarnung.|
|/locations/alerts/dismiss/action|Schließt eine Sicherheitswarnung.|
|/locations/alerts/read|Ruft alle verfügbaren Sicherheitswarnungen ab.|
|/locations/jitNetworkAccessPolicies/initiate/action|Initiiert eine JIT-Netzwerkzugriffsrichtlinie.|
|/locations/jitNetworkAccessPolicies/read|Ruft die JIT-Netzwerkzugriffsrichtlinien ab.|
|/locations/jitNetworkAccessPolicies/write|Erstellt eine neue JIT-Netzwerkzugriffsrichtlinie oder aktualisiert eine bereits vorhandene.|
|/locations/read|Ruft den Speicherort der Sicherheitsdaten ab.|
|/locations/tasks/activate/action|Dient zum Aktivieren einer Sicherheitsempfehlung.|
|/locations/tasks/dismiss/action|Dient zum Verwerfen einer Sicherheitsempfehlung.|
|/locations/tasks/read|Ruft alle verfügbaren Sicherheitsempfehlungen ab.|
|/locations/tasks/resolve/action|Dient zum Auflösen einer Sicherheitsempfehlung.|
|/locations/tasks/start/action|Dient zum Starten einer Sicherheitsempfehlung.|
|/policies/read|Ruft die Sicherheitsrichtlinie ab.|
|/policies/write|Aktualisiert die Sicherheitsrichtlinie.|
|/pricings/delete|Löscht die Tarifeinstellungen für den Bereich.|
|/pricings/read|Ruft die Tarifeinstellungen für den Bereich ab.|
|/pricings/write|Aktualisiert die Tarifeinstellungen für den Bereich.|
|/register/action|Registriert das Abonnement für das Azure Security Center.|
|/securityContacts/delete|Löscht den Sicherheitskontakt.|
|/securityContacts/read|Ruft den Sicherheitskontakt ab.|
|/securityContacts/write|Aktualisiert den Sicherheitskontakt.|
|/securitySolutions/delete|Löscht eine Sicherheitslösung.|
|/securitySolutions/read|Ruft die Sicherheitslösungen ab.|
|/securitySolutions/write|Erstellt eine neue Sicherheitslösung oder aktualisiert eine bereits vorhandene.|
|/securitySolutionsReferenceData/read|Ruft die Verweisdaten für Sicherheitslösungen ab.|
|/securityStatuses/read|Ruft den Sicherheitsintegritätsstatus für Azure-Ressourcen ab.|
|/securityStatusesSummaries/read|Ruft die Zusammenfassungen der Sicherheitsstatus für den Bereich ab.|
|/tasks/read|Ruft alle verfügbaren Sicherheitsempfehlungen ab.|
|/webApplicationFirewalls/delete|Löscht eine Web Application Firewall.|
|/webApplicationFirewalls/read|Ruft die Web Application Firewalls ab.|
|/webApplicationFirewalls/write|Erstellt eine neue Web Application Firewall oder aktualisiert eine bereits vorhandene.|
|/workspaceSettings/connect/action|Ändert die Einstellungen zum erneuten Herstellen einer Verbindung in den Arbeitsbereichseinstellungen.|
|/workspaceSettings/delete|Löscht die Arbeitsbereichseinstellungen.|
|/workspaceSettings/read|Ruft die Arbeitsbereichseinstellungen ab.|
|/workspaceSettings/write|Aktualisiert die Arbeitsbereichseinstellungen.|

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

| Vorgang | BESCHREIBUNG |
|---|---|
|/checkNameAvailability/action|Prüft die Verfügbarkeit des Namespaces unter dem angegebenen Abonnement.|
|/checkNamespaceAvailability/action|Prüft die Verfügbarkeit des Namespaces unter dem angegebenen Abonnement. Diese API ist veraltet. Verwenden Sie stattdessen CheckNameAvailabiltiy.|
|/namespaces/authorizationRules/action|Dient zum Aktualisieren von Namespace-Autorisierungsregeln. Diese API ist veraltet. Verwenden Sie stattdessen einen PUT-Aufruf, um die Namespace-Autorisierungsregel zu aktualisieren. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt.|
|/namespaces/authorizationRules/delete|Dient zum Löschen von Namespace-Autorisierungsregeln. Die standardmäßige Namespace-Autorisierungsregel kann nicht gelöscht werden. |
|/namespaces/authorizationRules/listkeys/action|Dient zum Abrufen der Verbindungszeichenfolge für den Namespace.|
|/namespaces/authorizationRules/read|Dient zum Abrufen der Liste mit Beschreibungen für Namespace-Autorisierungsregeln.|
|/namespaces/authorizationRules/regenerateKeys/action|Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource.|
|/namespaces/authorizationRules/write|Dient zum Erstellen von Autorisierungsregeln auf Namespaceebene und zum Aktualisieren der dazugehörigen Eigenschaften. Aktualisiert werden können die Zugriffsrechte für Autorisierungsregeln sowie der primäre und der sekundäre Schlüssel.|
|/namespaces/Delete|Dient zum Löschen von Namespaceressourcen.|
|/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action|Ruft die Schlüssel der Autorisierungsregeln für den primären Namespace für die Notfallwiederherstellung.|
|/namespaces/disasterRecoveryConfigs/authorizationRules/read|Ruft die Autorisierungsregeln des primären Namespace für die Notfallwiederherstellung ab.|
|/namespaces/disasterRecoveryConfigs/breakPairing/action|Hiermit wird die Notfallwiederherstellung deaktiviert und die Replikation von Änderungen vom primären Namespace zu sekundären Namespaces beendet.|
|/namespaces/disasterrecoveryconfigs/checkNameAvailability/action|Prüft die Verfügbarkeit des Namespacealias unter dem angegebenen Abonnement.|
|/namespaces/disasterRecoveryConfigs/delete|Hiermit wird die dem Namespace zugeordnete Notfallwiederherstellungskonfiguration gelöscht. Dieser Vorgang kann nur über den primären Namespace aufgerufen werden.|
|/namespaces/disasterRecoveryConfigs/failover/action|Hiermit wird ein Failover bei der georedundanten Notfallwiederherstellung ausgelöst und der Namespacealias neu konfiguriert, um auf den sekundären Namespace zu zeigen.|
|/namespaces/disasterRecoveryConfigs/read|Hiermit wird die dem Namespace zugeordnete Notfallwiederherstellungskonfiguration abgerufen.|
|/namespaces/disasterRecoveryConfigs/write|Hiermit wird die dem Namespace zugeordnete Notfallwiederherstellungskonfiguration erstellt oder aktualisiert.|
|/namespaces/eventGridFilters/delete|Löscht den Event Grid-Filter, der dem Namespace zugeordnet ist.|
|/namespaces/eventGridFilters/read|Ruft den Event Grid-Filter ab, der dem Namespace zugeordnet ist.|
|/namespaces/eventGridFilters/write|Erstellt oder aktualisiert den Event Grid-Filter, der dem Namespace zugeordnet ist.|
|/namespaces/eventhubs/read|Dient zum Abrufen einer Liste mit EventHub-Ressourcenbeschreibungen.|
|/namespaces/messagingPlan/read|Hiermit wird der Messagingplan für einen Namespace abgerufen. Diese API ist veraltet. Über die MessagingPlan-Ressource verfügbar gemachte Eigenschaften werden in höheren API-Versionen in die (übergeordnete) Namespaceressource verschoben. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt.|
|/namespaces/messagingPlan/write|Hiermit wird der Messagingplan für einen Namespace aktualisiert. Diese API ist veraltet. Über die MessagingPlan-Ressource verfügbar gemachte Eigenschaften werden in höheren API-Versionen in die (übergeordnete) Namespaceressource verschoben. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt.|
|/namespaces/migrate/action|Migriert den Namespacevorgang.|
|/namespaces/operationresults/read|Hiermit wird der Status des Namespacevorgangs abgerufen.|
|/namespaces/providers/Microsoft.Insights/diagnosticSettings/read|Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacediagnoseeinstellungen.|
|/namespaces/providers/Microsoft.Insights/diagnosticSettings/write|Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacediagnoseeinstellungen.|
|/namespaces/providers/Microsoft.Insights/logDefinitions/read|Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespaceprotokolle.|
|/namespaces/providers/Microsoft.Insights/metricDefinitions/read|Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacemetriken.|
|/namespaces/queues/authorizationRules/action|Vorgang zum Aktualisieren von Queue. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. Autorisierungsregeln. Verwenden Sie einen PUT-Aufruf, um die Autorisierungsregel zu aktualisieren.|
|/namespaces/queues/authorizationRules/delete|Vorgang zum Löschen von Warteschlangen-Autorisierungsregeln.|
|/namespaces/queues/authorizationRules/listkeys/action|Dient zum Abrufen der Warteschlangen-Verbindungszeichenfolge.|
|/namespaces/queues/authorizationRules/read| Dient zum Abrufen der Liste mit Warteschlangen-Autorisierungsregeln.|
|/namespaces/queues/authorizationRules/regenerateKeys/action|Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource.|
|/namespaces/queues/authorizationRules/write|Dient zum Erstellen von Warteschlangen-Autorisierungsregeln und zum Aktualisieren der dazugehörigen Eigenschaften. Die Zugriffsrechte für Autorisierungsregeln können aktualisiert werden.|
|/namespaces/queues/Delete|Vorgang zum Löschen von Warteschlangenressourcen.|
|/namespaces/queues/read|Dient zum Abrufen einer Liste mit Warteschlangen-Ressourcenbeschreibungen.|
|/namespaces/queues/write|Dient zum Erstellen oder Aktualisieren von Warteschlangeneigenschaften.|
|/namespaces/read|Dient zum Abrufen der Liste mit Beschreibungen für Namespaceressourcen.|
|/namespaces/topics/authorizationRules/action|Vorgang zum Aktualisieren von Topic. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. Autorisierungsregeln. Verwenden Sie einen PUT-Aufruf, um die Autorisierungsregel zu aktualisieren.|
|/namespaces/topics/authorizationRules/delete|Vorgang zum Löschen von Themaautorisierungsregeln.|
|/namespaces/topics/authorizationRules/listkeys/action|Dient zum Abrufen der Themaverbindungszeichenfolge.|
|/namespaces/topics/authorizationRules/read| Dient zum Abrufen der Liste mit Themaautorisierungsregeln.|
|/namespaces/topics/authorizationRules/regenerateKeys/action|Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource.|
|/namespaces/topics/authorizationRules/write|Dient zum Erstellen von Themaautorisierungsregeln und zum Aktualisieren der dazugehörigen Eigenschaften. Die Zugriffsrechte für Autorisierungsregeln können aktualisiert werden.|
|/namespaces/topics/Delete|Vorgang zum Löschen von Themaressourcen.|
|/namespaces/topics/read|Dient zum Abrufen einer Liste mit Themaressourcenbeschreibungen.|
|/namespaces/topics/subscriptions/Delete|Vorgang zum Löschen von TopicSubscription-Ressourcen.|
|/namespaces/topics/subscriptions/read|Dient zum Abrufen einer Liste mit TopicSubscription-Ressourcenbeschreibungen.|
|/namespaces/topics/subscriptions/rules/Delete|Vorgang zum Löschen von Regelressourcen.|
|/namespaces/topics/subscriptions/rules/read|Dient zum Abrufen einer Liste mit Regelressourcenbeschreibungen.|
|/namespaces/topics/subscriptions/rules/write|Dient zum Erstellen oder Aktualisieren von Aktualisierungsregeleigenschaften.|
|/namespaces/topics/subscriptions/write|Dient zum Erstellen oder Aktualisieren von TopicSubscription-Eigenschaften.|
|/namespaces/topics/write|Dient zum Erstellen oder Aktualisieren von Themaeigenschaften.|
|/namespaces/write|Dient zum Erstellen einer Namespaceressource und zum Aktualisieren ihrer Eigenschaften. Die Eigenschaften für Markierungen und Kapazität des Namespace können aktualisiert werden.|
|/operations/read|Dient zum Abrufen von Vorgängen.|
|/register/action|Registriert das Abonnement für den ServiceBus-Ressourcenanbieter und ermöglicht die Erstellung von ServiceBus-Ressourcen.|
|/sku/read|Dient zum Abrufen einer Liste mit SKU-Ressourcenbeschreibungen.|
|/sku/regions/read|Dient zum Abrufen einer Liste mit SkuRegions-Ressourcenbeschreibungen.|
|/unregister/action|Registriert das Abonnement für den ServiceBus-Ressourcenanbieter und ermöglicht die Erstellung von ServiceBus-Ressourcen.|

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

| Vorgang | BESCHREIBUNG |
|---|---|
|/clusters/applications/delete|Hiermit löschen Sie eine Anwendung.|
|/clusters/applications/read|Liest eine Anwendung.|
|/clusters/applications/services/delete|Hiermit löschen Sie einen Dienst.|
|/clusters/applications/services/partitions/read|Liest eine Partition.|
|/clusters/applications/services/partitions/replicas/read|Liest ein Replikat.|
|/clusters/applications/services/read|Liest einen Dienst.|
|/clusters/applications/services/statuses/read|Liest einen Dienststatus.|
|/clusters/applications/services/write|Hiermit erstellen oder aktualisieren Sie einen Dienst.|
|/clusters/applications/write|Hiermit erstellen oder aktualisieren Sie eine Anwendung.|
|/clusters/applicationTypes/delete|Hiermit löschen Sie einen Anwendungstyp.|
|/clusters/applicationTypes/read|Hiermit lesen Sie einen Anwendungstyp.|
|/clusters/applicationTypes/versions/delete|Hiermit löschen Sie eine Anwendungstypversion.|
|/clusters/applicationTypes/versions/read|Hiermit lesen Sie eine Anwendungstypversion.|
|/clusters/applicationTypes/versions/write|Hiermit erstellen oder aktualisieren Sie eine Anwendungstypversion.|
|/clusters/applicationTypes/write|Hiermit erstellen oder aktualisieren Sie einen Anwendungstyp.|
|/clusters/delete|Löscht einen Cluster.|
|/clusters/nodes/read|Liest einen Knoten.|
|/clusters/read|Liest einen Cluster.|
|/clusters/statuses/read|Liest einen Clusterstatus.|
|/clusters/write|Erstellt oder aktualisiert einen Cluster.|
|/locations/clusterVersions/read|Liest Clusterversionen.|
|/locations/environments/clusterVersions/read|Hiermit lesen Sie eine Clusterversion für eine bestimmte Umgebung.|
|/locations/operationresults/read|Hiermit lesen Sie Vorgangsergebnisse.|
|/locations/operations/read|Hiermit lesen Sie Vorgänge nach Standort.|
|/operations/read|Hiermit lesen Sie verfügbare Vorgänge.|
|/register/action|Hiermit registrieren Sie eine Aktion.|

## <a name="microsoftsolutions"></a>Microsoft.Solutions

| Vorgang | BESCHREIBUNG |
|---|---|
|/applicationDefinitions/delete|Hiermit wird eine Anwendungsdefinition entfernt.|
|/applicationDefinitions/read|Hiermit wird eine Liste mit Anwendungsdefinitionen abgerufen.|
|/applicationDefinitions/write|Hiermit wird eine Anwendungsdefinition hinzugefügt oder geändert.|
|/applications/delete|Hiermit wird eine Anwendung entfernt.|
|/applications/read|Hiermit wird eine Liste mit Anwendungen abgerufen.|
|/applications/write|Erstellt eine Anwendung|
|/locations/operationStatuses/read|Liest den Vorgangsstatus für die Ressource.|
|/register/action|Hiermit registrieren Sie sich bei Lösungen.|

## <a name="microsoftsql"></a>Microsoft.Sql

| Vorgang | BESCHREIBUNG |
|---|---|
|/checkNameAvailability/action|Überprüfen Sie, ob der angegebene Servername für die weltweite Bereitstellung bei einem bestimmten Abonnement verfügbar ist.|
|/locations/auditingSettingsAzureAsyncOperation/read|Dient zum Abrufen des Ergebnisses des Set-Vorgangs der Richtlinie für die erweiterte Serverblobüberwachung.|
|/locations/auditingSettingsOperationResults/read|Dient zum Abrufen des Ergebnisses des Set-Vorgangs der Richtlinie für die Serverblobüberwachung.|
|/locations/capabilities/read|Ruft die Funktionen für dieses Abonnement in einem bestimmten Standort ab.|
|/locations/databaseAzureAsyncOperation/read|Ruft den Status eines Datenbankvorgangs ab.|
|/locations/databaseOperationResults/read|Ruft den Status eines Datenbankvorgangs ab.|
|/locations/deletedServerAsyncOperation/read|Ruft gegenwärtig ausgeführte Vorgänge für gelöschte Server ab.|
|/locations/deletedServerOperationResults/read|Ruft gegenwärtig ausgeführte Vorgänge für gelöschte Server ab.|
|/locations/deletedServers/read|Gibt die Liste der gelöschten Server zurück oder ruft die Eigenschaften für den angegebenen gelöschten Server ab.|
|/locations/deletedServers/recover/action|Stellt einen gelöschten Server wieder her.|
|/locations/deleteVirtualNetworkOrSubnets/action|Löscht Regeln für virtuelle Netzwerke, die einem virtuellen Netzwerk oder Subnetz zugeordnet sind.|
|/locations/elasticPoolAzureAsyncOperation/read|Ruft den asynchronen Vorgang in Azure für einen asynchronen Vorgang in einem Pool für elastische Datenbank ab.|
|/locations/elasticPoolOperationResults/read|Ruft das Ergebnis eines Vorgangs eines Pools für elastische Datenbanken ab.|
|/locations/extendedAuditingSettingsAzureAsyncOperation/read|Dient zum Abrufen des Ergebnisses des Set-Vorgangs der Richtlinie für die erweiterte Serverblobüberwachung.|
|/locations/extendedAuditingSettingsOperationResults/read|Dient zum Abrufen des Ergebnisses des Set-Vorgangs der Richtlinie für die erweiterte Serverblobüberwachung.|
|/locations/managedDatabaseRestoreAzureAsyncOperation/completeRestore/action|Führt den Vorgang zur Wiederherstellung einer verwalteten Datenbank durch.|
|/locations/managedTransparentDataEncryptionAzureAsyncOperation/read|Ruft gegenwärtig ausgeführte Vorgänge für Transparent Data Encryption einer verwalteten Datenbank ab.|
|/locations/managedTransparentDataEncryptionOperationResults/read|Ruft gegenwärtig ausgeführte Vorgänge für Transparent Data Encryption einer verwalteten Datenbank ab.|
|/locations/read|Ruft die verfügbaren Standorte für ein bestimmtes Abonnement ab.|
|/locations/syncAgentOperationResults/read|Ruft das Ergebnis des Ressourcenvorgangs des Synchronisierungs-Agents ab.|
|/locations/syncDatabaseIds/read|Ruft die Synchronisierungsdatenbank-IDs für eine bestimmte Region und ein bestimmtes Abonnement ab.|
|/locations/syncGroupOperationResults/read|Ruft das Ergebnis des Ressourcenvorgangs der Synchronisierungsgruppe ab.|
|/locations/syncMemberOperationResults/read|Ruft das Ergebnis des Ressourcenvorgangs des Synchronisierungsmitglieds ab.|
|/locations/usages/read|Ruft eine Collection von Nutzungsmetriken für dieses Abonnement an einem Standort ab.|
|/locations/virtualNetworkRulesAzureAsyncOperation/read|Gibt die Details des asynchronen Vorgangs in Azure für die Regeln für das angegebene virtuelle Netzwerk zurück. |
|/locations/virtualNetworkRulesOperationResults/read|Gibt die Details der Regeln für das angegebene virtuelle Netzwerk zurück. |
|/managedInstances/administrators/delete|Löscht einen vorhandenen Administrator einer verwalteten Instanz.|
|/managedInstances/administrators/read|Ruft eine Liste der Administratoren für verwaltete Instanzen ab.|
|/managedInstances/administrators/write|Erstellt oder aktualisiert den Administrator für verwaltete Instanzen mit den angegebenen Parametern.|
|/managedInstances/databases/delete|Löscht eine vorhandene verwaltete Datenbank.|
|/managedInstances/databases/read|Ruft eine vorhandene verwaltete Datenbank ab.|
|/managedInstances/databases/securityAlertPolicies/read|Dient zum Abrufen von Details zur Bedrohungserkennungsrichtlinie von Datenbanken, die für eine bestimmte verwaltete Datenbank konfiguriert ist.|
|/managedInstances/databases/securityAlertPolicies/write|Dient zum Ändern der Bedrohungserkennungsrichtlinie für eine bestimmte verwaltete Datenbank.|
|/managedInstances/databases/securityEvents/read|Ruft die Sicherheitsereignisse für die verwaltete Datenbank ab.|
|/managedInstances/databases/transparentDataEncryption/read|Ruft Details zu Datenbank-TDE (Transparent Data Encryption) für eine bestimmte verwaltete Datenbank ab.|
|/managedInstances/databases/transparentDataEncryption/write|Ändert Transparent Data Encryption für Datenbanken für eine bestimmte verwaltete Datenbank.|
|/managedInstances/databases/write|Erstellt eine neue Datenbank oder aktualisiert eine bereits vorhandene Datenbank|
|/managedInstances/delete|Löscht eine vorhandene verwaltete Instanz.|
|/managedInstances/metricDefinitions/read|Ruft Metrikdefinitionen der verwalteten Instanz ab.|
|/managedInstances/metrics/read|Ruft Metriken der verwalteten Instanz ab.|
|/managedInstances/read|Gibt die Liste der verwalteten Instanzen zurück oder ruft die Eigenschaften für die angegebene verwaltete Instanz ab.|
|/managedInstances/securityAlertPolicies/read|Dient zum Abrufen von Details zur Richtlinie zur Bedrohungserkennung bei verwalteten Servern, die für einen bestimmten verwalteten Server konfiguriert ist.|
|/managedInstances/securityAlertPolicies/write|Dient zum Ändern der Richtlinie zur Bedrohungserkennung bei verwalteten Servern für einen bestimmten verwalteten Server.|
|/managedInstances/write|Erstellt eine verwaltete Instanz mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für die verwaltete Instanz.|
|/operations/read|Ruft die verfügbaren REST-Vorgänge ab.|
|/register/action|Registriert das Abonnement für den Microsoft SQL-Datenbankressourcenanbieter und ermöglicht die Erstellung von Microsoft SQL-Datenbanken.|
|/servers/administratorOperationResults/read|Ruft gegenwärtig ausgeführte Vorgänge für Serveradministratoren ab.|
|/servers/administrators/delete|Löscht Serveradministratoren.|
|/servers/administrators/read|Dient zum Abrufen von Serveradministratordetails.|
|/servers/administrators/write|Dient zum Erstellen oder Aktualisieren von Serveradministratoren.|
|/servers/advisors/read|Gibt eine Liste mit verfügbaren Ratgebern für den Server zurück.|
|/servers/advisors/recommendedActions/read|Gibt eine Liste mit empfohlenen Aktionen des angegebenen Ratgebers für den Server zurück.|
|/servers/advisors/recommendedActions/write|Dient zum Anwenden der empfohlenen Aktion auf den Server.|
|/servers/advisors/write|Aktualisiert den Status der automatischen Ausführung eines Ratgebers auf der Serverebene.|
|/servers/auditingPolicies/read|Dient zum Abrufen von Details zur Richtlinie für die Standardservertabellen-Überwachung, die für einen bestimmten Server konfiguriert ist.|
|/servers/auditingPolicies/write|Dient zum Ändern der Standardservertabellen-Überwachung für einen bestimmten Server.|
|/servers/auditingSettings/operationResults/read|Dient zum Abrufen des Ergebnisses des Set-Vorgangs der Richtlinie für die Serverblobüberwachung.|
|/servers/auditingSettings/read|Dient zum Abrufen von Details zur Richtlinie für die Serverblobüberwachung, die für einen bestimmten Server konfiguriert ist.|
|/servers/auditingSettings/write|Dient zum Ändern der Serverblobüberwachung für einen bestimmten Server.|
|/servers/automaticTuning/read|Gibt die Einstellungen zur automatischen Optimierung für den Server zurück.|
|/servers/automaticTuning/write|Aktualisiert die Einstellungen zur automatischen Optimierung für den Server und gibt die aktualisierten Einstellungen zurück.|
|/servers/backupLongTermRetentionVaults/delete|Löscht einen vorhandenen Sicherungsarchivtresor.|
|/servers/backupLongTermRetentionVaults/read|Vorgang zum Abrufen eines Sicherungstresors für die langfristige Aufbewahrung. Gibt Informationen zu dem Tresor zurück, der bei diesem Server registriert ist.|
|/servers/backupLongTermRetentionVaults/write|Dieser Vorgang wird zum Registrieren eines Sicherungstresors für die langfristige Aufbewahrung bei einem Server verwendet.|
|/servers/communicationLinks/delete|Löscht eine vorhandene Serverkommunikationsverbindung.|
|/servers/communicationLinks/read|Gibt die Liste der Kommunikationsverbindungen eines bestimmten Servers zurück.|
|/servers/communicationLinks/write|Erstellt oder aktualisiert eine Serverkommunikationsverbindung.|
|/servers/connectionPolicies/read|Gibt die Liste der Serververbindungsrichtlinien eines angegebenen Servers zurück.|
|/servers/connectionPolicies/write|Erstellt oder aktualisiert eine Serververbindungsrichtlinie.|
|/servers/databases/advisors/read|Gibt eine Liste mit verfügbaren Ratgebern für die Datenbank zurück.|
|/servers/databases/advisors/recommendedActions/read|Gibt eine Liste mit empfohlenen Aktionen des angegebenen Ratgebers für die Datenbank zurück.|
|/servers/databases/advisors/recommendedActions/write|Dient zum Anwenden der empfohlenen Aktion auf die Datenbank.|
|/servers/databases/advisors/write|Dient zum Aktualisieren des Status der automatischen Ausführung eines Ratgebers auf der Datenbankebene.|
|/servers/databases/auditingPolicies/read|Dient zum Abrufen von Details zur Tabellenüberwachungsrichtlinie, die für eine bestimmte Datenbank konfiguriert ist.|
|/servers/databases/auditingPolicies/write|Dient zum Ändern der Tabellenüberwachungsrichtlinie für eine bestimmte Datenbank.|
|/servers/databases/auditingSettings/read|Dient zum Abrufen von Details zur Blobüberwachungsrichtlinie, die für eine bestimmte Datenbank konfiguriert ist.|
|/servers/databases/auditingSettings/write|Dient zum Ändern der Blobüberwachungsrichtlinie für eine bestimmte Datenbank.|
|/servers/databases/auditRecords/read|Dient zum Abrufen der Datensätze für die Datenbankblobüberwachung.|
|/servers/databases/automaticTuning/read|Gibt die Einstellungen zur automatischen Optimierung für eine Datenbank zurück.|
|/servers/databases/automaticTuning/write|Aktualisiert die Einstellungen zur automatischen Optimierung für eine Datenbank und gibt die aktualisierten Einstellungen zurück.|
|/servers/databases/azureAsyncOperation/read|Ruft den Status eines Datenbankvorgangs ab.|
|/servers/databases/backupLongTermRetentionPolicies/read|Gibt die Liste der Sicherungsarchivierungsrichtlinien einer angegebenen Datenbank zurück.|
|/servers/databases/backupLongTermRetentionPolicies/write|Erstellt oder aktualisiert eine Datenbanksicherungs-Archivierungsrichtlinie.|
|/servers/databases/connectionPolicies/read|Dient zum Abrufen von Details zur Verbindungsrichtlinie, die für eine bestimmte Datenbank konfiguriert ist.|
|/servers/databases/connectionPolicies/write|Dient zum Ändern der Verbindungsrichtlinie für eine bestimmte Datenbank.|
|/servers/databases/dataMaskingPolicies/read|Gibt die Liste der Richtlinien zur Maskierung von Datenbankdaten zurück.|
|/servers/databases/dataMaskingPolicies/rules/delete|Dient zum Löschen von Datenmaskierungsrichtlinien-Regeln für eine bestimmte Datenbank.|
|/servers/databases/dataMaskingPolicies/rules/read|Dient zum Abrufen von Details zur Datenmaskierungsrichtlinien-Regel, die für eine bestimmte Datenbank konfiguriert ist.|
|/servers/databases/dataMaskingPolicies/rules/write|Dient zum Ändern von Datenmaskierungsrichtlinen-Regeln für eine bestimmte Datenbank.|
|/servers/databases/dataMaskingPolicies/write|Dient zum Ändern der Datenmaskierungsrichtlinie für eine bestimmte Datenbank.|
|/servers/databases/dataWarehouseQueries/dataWarehouseQuerySteps/read|Gibt die verteilten Abfrageschrittinformationen von Data Warehouse-Abfragen für die ausgewählte Schritt-ID zurück.|
|/servers/databases/dataWarehouseQueries/read|Gibt die Data Warehouse-Verteilungsabfrageinformationen für die ausgewählte Abfrage-ID zurück.|
|/servers/databases/dataWarehouseUserActivities/read|Ruft die Benutzeraktivitäten einer SQL Data Warehouse-Instanz ab, einschließlich ausgeführte und angehaltene Abfragen.|
|/servers/databases/delete|Löscht eine vorhandene Datenbank.|
|/servers/databases/export/action|Exportiert Azure SQL-Datenbank.|
|/servers/databases/extendedAuditingSettings/read|Dient zum Abrufen von Details zur erweiterten Blobüberwachungsrichtlinie, die für eine bestimmte Datenbank konfiguriert ist.|
|/servers/databases/extendedAuditingSettings/write|Dient zum Ändern der erweiterten Blobüberwachungsrichtlinie für eine bestimmte Datenbank.|
|/servers/databases/extensions/read|Ruft eine Collection von Erweiterungen für die Datenbank ab.|
|/servers/databases/extensions/write|Ändert die Erweiterung für eine bestimmte Datenbank.|
|/servers/databases/geoBackupPolicies/read|Ruft Richtlinien für die Geosicherung einer bestimmten Datenbank ab.|
|/servers/databases/geoBackupPolicies/write|Erstellt oder aktualisiert eine Richtlinie für die Geosicherung einer Datenbank.|
|/servers/databases/importExportOperationResults/read|Ruft gegenwärtig ausgeführte Import-/Exportvorgänge ab.|
|/servers/databases/metricDefinitions/read|Dient zum Zurückgeben verfügbarer Metriktypen für Datenbanken.|
|/servers/databases/metrics/read|Gibt Metriken für Datenbanken zurück.|
|/servers/databases/move/action|Benennt eine Azure SQL-Datenbank um.|
|/servers/databases/operationResults/read|Ruft den Status eines Datenbankvorgangs ab.|
|/servers/databases/operations/cancel/action|Bricht einen ausstehenden asynchronen Vorgang in Azure SQL-Datenbank ab, der noch nicht abgeschlossen ist.|
|/servers/databases/operations/read|Gibt die Liste der Vorgänge zurück, die für die Datenbank ausgeführt werden.|
|/servers/databases/pause/action|Hält die Azure SQL Data Warehouse-Datenbank an.|
|/servers/databases/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/servers/databases/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/servers/databases/providers/Microsoft.Insights/logDefinitions/read|Ruft die verfügbaren Protokolle für Datenbanken ab.|
|/servers/databases/providers/Microsoft.Insights/metricDefinitions/read|Dient zum Zurückgeben verfügbarer Metriktypen für Datenbanken.|
|/servers/databases/queryStore/queryTexts/read|Gibt die Collection von Abfragetexten zurück, die den angegebenen Parametern entsprechen.|
|/servers/databases/queryStore/read|Gibt die aktuellen Werte von Abfragespeichereinstellungen für die Datenbank zurück.|
|/servers/databases/queryStore/write|Aktualisiert die Abfragespeichereinstellung für die Datenbank.|
|/servers/databases/read|Gibt die Liste der Datenbanken zurück oder ruft die Eigenschaften für die angegebene Datenbank ab.|
|/servers/databases/replicationLinks/delete|Dient zum Erzwingen der Beendigung der Replikationsbeziehung mit potenziellem Datenverlust.|
|/servers/databases/replicationLinks/failover/action|Dient zum Ausführen eines Failovers nach der Synchronisierung aller Änderungen der primären Datenbank. Dadurch wird diese Datenbank zur primären Datenbank der Replikationsbeziehung, und die primäre Remotedatenbank wird zur sekundären Datenbank.|
|/servers/databases/replicationLinks/forceFailoverAllowDataLoss/action|Dient zum Ausführen eines sofortigen Failovers mit potenziellem Datenverlust. Dadurch wird diese Datenbank zur primären Datenbank der Replikationsbeziehung, und die primäre Remotedatenbank wird zur sekundären Datenbank.|
|/servers/databases/replicationLinks/read|Dient zum Zurückgeben von Details zu Replikationslinks, die für eine bestimmte Datenbank eingerichtet wurden.|
|/servers/databases/replicationLinks/unlink/action|Dient zum Beenden der Replikationsbeziehung – entweder erzwungen oder nach der Synchronisierung mit dem Partner.|
|/servers/databases/replicationLinks/updateReplicationMode/action|Dient zum Aktualisieren des Replikationsmodus für die Verknüpfung auf den synchronen oder asynchronen Modus.|
|/servers/databases/restorePoints/action|Erstellt einen neuen Wiederherstellungspunkt.|
|/servers/databases/restorePoints/read|Gibt Wiederherstellungspunkte für die Datenbank zurück.|
|/servers/databases/resume/action|Setzt die Azure SQL Data Warehouse-Datenbank fort.|
|/servers/databases/schemas/read|Ruft die Liste der Schemas einer Datenbank ab.|
|/servers/databases/schemas/tables/columns/read|Dient zum Abrufen einer Liste mit Tabellenspalten.|
|/servers/databases/schemas/tables/columns/sensitivityLabels/delete|Löscht die Vertraulichkeitsbezeichnung einer bestimmten Spalte.|
|/servers/databases/schemas/tables/columns/sensitivityLabels/read|Ruft die Vertraulichkeitsbezeichnung einer bestimmten Spalte ab.|
|/servers/databases/schemas/tables/columns/sensitivityLabels/write|Erstellt oder aktualisiert die Vertraulichkeitsbezeichnung einer bestimmten Spalte.|
|/servers/databases/schemas/tables/read|Ruft die Liste der Tabellen einer Datenbank ab.|
|/servers/databases/schemas/tables/recommendedIndexes/read|Dient zum Abrufen einer Liste mit Indexempfehlungen für eine Datenbank.|
|/servers/databases/schemas/tables/recommendedIndexes/write|Dient zum Anwenden von Indexempfehlungen.|
|/servers/databases/securityAlertPolicies/read|Dient zum Abrufen von Details zur Bedrohungserkennungsrichtlinie, die für eine bestimmte Datenbank konfiguriert ist.|
|/servers/databases/securityAlertPolicies/write|Dient zum Ändern der Bedrohungserkennungsrichtlinie für eine bestimmte Datenbank.|
|/servers/databases/securityMetrics/read|Ruft eine Collection von Metriken für Datenbanksicherheit ab.|
|/servers/databases/sensitivityLabels/read|Listet die Vertraulichkeitsbezeichnungen einer bestimmten Datenbank auf.|
|/servers/databases/serviceTierAdvisors/read|Gibt Vorschläge zum zentralen Hoch- oder Herunterskalieren von Datenbanken auf der Grundlage von Abfrageausführungsstatistiken zurück, um die Leistung zu verbessern oder die Kosten zu verringern.|
|/servers/databases/syncGroups/cancelSync/action|Bricht die Synchronisierung von Synchronisierungsgruppen ab.|
|/servers/databases/syncGroups/delete|Löscht eine vorhandene Synchronisierungsgruppe.|
|/servers/databases/syncGroups/hubSchemas/read|Gibt die Liste der Datenbankschemas für Synchronisierungshubs zurück.|
|/servers/databases/syncGroups/logs/read|Gibt die Liste der Synchronisierungsgruppenprotokolle zurück.|
|/servers/databases/syncGroups/read|Gibt die Liste der Synchronisierungsgruppen zurück oder ruft die Eigenschaften für die angegebene Synchronisierungsgruppe ab.|
|/servers/databases/syncGroups/refreshHubSchema/action|Aktualisiert das Datenbankschema des Synchronisierungshubs.|
|/servers/databases/syncGroups/refreshHubSchemaOperationResults/read|Ruft das Ergebnis des Aktualisierungsvorgangs für das Schema des Synchronisierungshubs ab.|
|/servers/databases/syncGroups/syncMembers/delete|Löscht ein vorhandenes Synchronisierungsmitglied.|
|/servers/databases/syncGroups/syncMembers/read|Gibt die Liste der Synchronisierungsmitglieder zurück oder ruft die Eigenschaften für das angegebene Synchronisierungsmitglied ab.|
|/servers/databases/syncGroups/syncMembers/refreshSchema/action|Aktualisiert das Schema für das Synchronisierungsmitglied.|
|/servers/databases/syncGroups/syncMembers/refreshSchemaOperationResults/read|Ruft das Ergebnis des Aktualisierungsvorgangs für das Schema für das Synchronisierungsmitglied ab.|
|/servers/databases/syncGroups/syncMembers/schemas/read|Gibt die Liste von Schemas für Synchronisierungsmitglieder zurück.|
|/servers/databases/syncGroups/syncMembers/write|Erstellt ein Synchronisierungsmitglied mit den angegebenen Parametern oder aktualisiert die Eigenschaften für das angegebene Synchronisierungsmitglied.|
|/servers/databases/syncGroups/triggerSync/action|Löst die Synchronisierung für die Synchronisierungsgruppe aus.|
|/servers/databases/syncGroups/write|Erstellt eine Synchronisierungsgruppe mit den angegebenen Parametern oder aktualisiert die Eigenschaften für die angegebene Synchronisierungsgruppe.|
|/servers/databases/topQueries/queryText/action|Gibt den Transact-SQL-Text für die ausgewählte Abfrage-ID zurück.|
|/servers/databases/topQueries/read|Gibt aggregierte Laufzeitstatistiken für die ausgewählte Abfrage im ausgewählten Zeitraum zurück.|
|/servers/databases/topQueries/statistics/read|Gibt aggregierte Laufzeitstatistiken für die ausgewählte Abfrage im ausgewählten Zeitraum zurück.|
|/servers/databases/transparentDataEncryption/operationResults/read|Ruft gegenwärtig ausgeführte Vorgänge für Transparent Data Encryption ab.|
|/servers/databases/transparentDataEncryption/read|Dient zum Abrufen von Status und Details des Transparent Data Encryption-Sicherheitsfeatures für eine bestimmte Datenbank.|
|/servers/databases/transparentDataEncryption/write|Ändert den Status von Transparent Data Encryption.|
|/servers/databases/upgradeDataWarehouse/action|Führt ein Upgrade für die Azure SQL Data Warehouse-Datenbank durch.|
|/servers/databases/usages/read|Ruft Informationen zur Azure SQL-Datenbanknutzung ab.|
|/servers/databases/vulnerabilityAssessments/delete|Entfernt die Sicherheitsrisikobewertung für eine bestimmte Datenbank.|
|/servers/databases/vulnerabilityAssessments/read|Dient zum Abrufen von Details zur Sicherheitsrisikobewertung, die für eine bestimmte Datenbank konfiguriert ist.|
|/servers/databases/vulnerabilityAssessments/rules/baselines/delete|Entfernt die Baseline für die Regel für die Sicherheitsrisikobewertung für eine bestimmte Datenbank.|
|/servers/databases/vulnerabilityAssessments/rules/baselines/read|Ruft das Baseline für die Regel für die Sicherheitsrisikobewertung für eine bestimmte Datenbank ab.|
|/servers/databases/vulnerabilityAssessments/rules/baselines/write|Ändert die Baseline für die Regel für die Sicherheitsrisikobewertung für eine bestimmte Datenbank.|
|/servers/databases/vulnerabilityAssessments/scans/action|Dient zum Ausführen einer Datenbanküberprüfung zur Bewertung von Sicherheitsrisiken.|
|/servers/databases/vulnerabilityAssessments/scans/export/action|Konvertiert ein vorhandenes Überprüfungsergebnis in ein lesbares Format. Wenn dies bereits der Fall ist, werden keine Vorgänge ausgeführt.|
|/servers/databases/vulnerabilityAssessments/scans/read|Gibt die Liste der Überprüfungsdatensätze für eine Datenbank-Sicherheitsrisikobewertung oder den Überprüfungsdatensatz für die angegebene Überprüfungs-ID zurück.|
|/servers/databases/vulnerabilityAssessments/write|Ändert die Sicherheitsrisikobewertung für eine bestimmte Datenbank.|
|/servers/databases/vulnerabilityAssessmentScans/action|Dient zum Ausführen einer Datenbanküberprüfung zur Bewertung von Sicherheitsrisiken.|
|/servers/databases/vulnerabilityAssessmentScans/operationResults/read|Ruft das Ergebnis des Ausführungsvorgangs für die Datenbank-Sicherheitsrisikobewertung ab.|
|/servers/databases/vulnerabilityAssessmentSettings/read|Dient zum Abrufen von Details zur Sicherheitsrisikobewertung, die für eine bestimmte Datenbank konfiguriert ist.|
|/servers/databases/vulnerabilityAssessmentSettings/write|Ändert die Sicherheitsrisikobewertung für eine bestimmte Datenbank.|
|/servers/databases/write|Erstellt eine Datenbank mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für die angegebene Datenbank.|
|/servers/delete|Löscht einen vorhandenen Server.|
|/servers/disasterRecoveryConfiguration/delete|Löscht vorhandene Notfallwiederherstellungskonfigurationen für einen bestimmten Server.|
|/servers/disasterRecoveryConfiguration/failover/action|Führt ein Failover für DisasterRecoveryConfiguration durch.|
|/servers/disasterRecoveryConfiguration/forceFailoverAllowDataLoss/action|Erzwingen Sie ein Failover für DisasterRecoveryConfiguration.|
|/servers/disasterRecoveryConfiguration/read|Ruft eine Collection von Notfallwiederherstellungskonfigurationen ab, die diesen Server umfassen.|
|/servers/disasterRecoveryConfiguration/write|Ändert die Notfallwiederherstellungskonfiguration des Servers.|
|/servers/elasticPoolEstimates/read|Gibt die Liste mit Schätzwerten für den Pool für elastische Datenbanken zurück, die bereits für diesen Server erstellt wurden.|
|/servers/elasticPoolEstimates/write|Erstellt für die Liste der bereitgestellten Datenbanken neue Schätzwerte für den Pool für elastische Datenbanken.|
|/servers/elasticPools/advisors/read|Gibt eine Liste mit verfügbaren Ratgebern für den Pool für elastische Datenbanken zurück.|
|/servers/elasticPools/advisors/recommendedActions/read|Gibt eine Liste mit empfohlenen Aktionen des angegebenen Ratgebers für den Pool für elastische Datenbanken zurück.|
|/servers/elasticPools/advisors/recommendedActions/write|Dient zum Anwenden der empfohlenen Aktion auf den Pool für elastische Datenbanken.|
|/servers/elasticPools/advisors/write|Dient zum Aktualisieren des Status der automatischen Ausführung eines Ratgebers auf der Ebene von Pools für elastische Datenbanken.|
|/servers/elasticPools/databases/read|Ruft eine Liste der Datenbanken für einen Pool für elastische Datenbanken zurück.|
|/servers/elasticPools/delete|Löscht einen vorhandenen Pool für elastische Datenbanken.|
|/servers/elasticPools/elasticPoolActivity/read|Dient zum Abrufen von Aktivitäten und Details zu einem bestimmten Pool für elastische Datenbanken.|
|/servers/elasticPools/elasticPoolDatabaseActivity/read|Dient zum Abrufen von Aktivitäten und Details zu einer bestimmten Datenbank, die einem Pool für elastische Datenbanken angehört.|
|/servers/elasticPools/metricDefinitions/read|Dient zum Zurückgeben verfügbarer Metriktypen für Pools für elastische Datenbanken.|
|/servers/elasticPools/metrics/read|Gibt Metriken für Pools für elastische Datenbanken zurück.|
|/servers/elasticPools/operations/cancel/action|Bricht einen ausstehenden asynchronen Vorgang in einem Pool für elastische Azure SQL-Datenbanken ab, der noch nicht abgeschlossen ist.|
|/servers/elasticPools/operations/read|Gibt die Liste der Vorgänge zurück, die für den Pool für elastische Datenbanken ausgeführt werden.|
|/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/servers/elasticPools/providers/Microsoft.Insights/metricDefinitions/read|Dient zum Zurückgeben verfügbarer Metriktypen für Pools für elastische Datenbanken.|
|/servers/elasticPools/read|Dient zum Abrufen von Details zu Pools für elastische Datenbanken auf einem bestimmten Server.|
|/servers/elasticPools/skus/read|Ruft eine Collection von SKUs ab, die für diesen Pool für elastische Datenbanken verfügbar ist.|
|/servers/elasticPools/write|Dient zum Erstellen eines neuen Pools für elastische Datenbanken oder zum Ändern von Eigenschaften vorhandener Pools für elastische Datenbanken.|
|/servers/encryptionProtector/read|Gibt eine Liste von Schutzvorrichtungen zur Serververschlüsselung zurück oder ruft die Eigenschaften für die angegebene Schutzvorrichtung zur Serververschlüsselung ab.|
|/servers/encryptionProtector/write|Aktualisiert die Eigenschaften für die angegebene Schutzvorrichtung zur Serververschlüsselung.|
|/servers/extendedAuditingSettings/read|Dient zum Abrufen von Details zur Richtlinie für die erweiterte Serverblobüberwachung, die für einen bestimmten Server konfiguriert ist.|
|/servers/extendedAuditingSettings/write|Dient zum Ändern der erweiterten Serverblobüberwachung für einen bestimmten Server.|
|/servers/failoverGroups/delete|Löscht eine vorhandene Failovergruppe.|
|/servers/failoverGroups/failover/action|Führt das geplante Failover in einer vorhandenen Failovergruppe durch.|
|/servers/failoverGroups/forceFailoverAllowDataLoss/action|Führt ein erzwungenes Failover in einer vorhandenen Failovergruppe aus.|
|/servers/failoverGroups/read|Gibt die Liste der Failovergruppen zurück oder ruft die Eigenschaften für die angegebene Failovergruppe ab.|
|/servers/failoverGroups/write|Erstellt eine Failovergruppe mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für die angegebene Failovergruppe.|
|/servers/firewallRules/delete|Löscht eine vorhandene Serverfirewallregel.|
|/servers/firewallRules/read|Gibt die Liste der Serverfirewallregeln zurück oder ruft die Eigenschaften für die angegebene Serverfirewallregel ab.|
|/servers/firewallRules/write|Erstellt eine Serverfirewallregel mit den angegebenen Parametern, aktualisiert die Eigenschaften für die angegebene Regel oder überschreibt alle vorhandenen Regeln durch neue Serverfirewallregel.|
|/servers/import/action|Dient zum Erstellen einer neuen Datenbank auf dem Server und zum Bereitstellen von Schema und Daten aus einem DACPAC-Paket.|
|/servers/importExportOperationResults/read|Ruft gegenwärtig ausgeführte Import-/Exportvorgänge ab.|
|/servers/keys/delete|Löscht einen vorhandenen Serverschlüssel.|
|/servers/keys/read|Gibt die Liste der Serverschlüssel zurück oder ruft die Eigenschaften für den angegebenen Serverschlüssel ab.|
|/servers/keys/write|Erstellt einen Schlüssel mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für den angegebenen Serverschlüssel.|
|/servers/operationResults/read|Ruft gegenwärtig ausgeführte Servervorgänge ab.|
|/servers/providers/Microsoft.Insights/metricDefinitions/read|Dient zum Zurückgeben verfügbarer Metriken für Server.|
|/servers/read|Gibt die Liste der Server zurück oder ruft die Eigenschaften für den angegebenen Server ab.|
|/servers/recommendedElasticPools/databases/read|Dient zum Abrufen von Metriken für empfohlene Pools für elastische Datenbanken für einen bestimmten Server.|
|/servers/recommendedElasticPools/read|Dient zum Abrufen von Empfehlungen für Pools für elastische Datenbanken, um Kosten zu senken oder die Leistung zu verbessern. Die Empfehlungen basieren auf der Ressourcenverwendung in der Vergangenheit.|
|/servers/recoverableDatabases/read|Vorgang für die Notfallwiederherstellung der Livedatenbank, um die Datenbank auf der Grundlage des letzten als funktionierend bekannten Sicherungspunkts wiederherzustellen. Gibt Informationen zur letzten als funktionierend bekannten Sicherung zurück, stellt die Datenbank aber nicht wieder her.|
|/servers/restorableDroppedDatabases/read|Dient zum Abrufen einer Liste mit Datenbanken, die auf einem bestimmten Server gelöscht wurden und weiterhin in einer Aufbewahrungsrichtlinie enthalten sind.|
|/servers/securityAlertPolicies/operationResults/read|Dient zum Abrufen von Ergebnissen des Schreibvorgangs der Richtlinie für die Serverbedrohungserkennung.|
|/servers/securityAlertPolicies/read|Dient zum Abrufen von Details zur Richtlinie für die Serverbedrohungserkennung, die für einen bestimmten Server konfiguriert ist.|
|/servers/securityAlertPolicies/write|Dient zum Ändern der Richtlinie für die Serverbedrohungserkennung für einen bestimmten Server.|
|/servers/serviceObjectives/read|Dient zum Abrufen einer Liste mit verfügbaren Servicelevelzielen (auch Leistungsstufen genannt) für einen bestimmten Server.|
|/servers/syncAgents/delete|Löscht einen vorhandenen Synchronisierungs-Agent.|
|/servers/syncAgents/generateKey/action|Generiert einen Registrierungsschlüssel für den Synchronisierungs-Agent.|
|/servers/syncAgents/linkedDatabases/read|Gibt die Liste der mit dem Synchronisierungs-Agent verknüpften Datenbanken zurück.|
|/servers/syncAgents/read|Gibt die Liste der Synchronisierungs-Agents zurück oder ruft die Eigenschaften für den angegebenen Synchronisierungs-Agent ab.|
|/servers/syncAgents/write|Erstellt einen Synchronisierungs-Agent mit den angegebenen Parametern oder aktualisiert die Eigenschaften für den angegebenen Synchronisierungs-Agent.|
|/servers/usages/read|Dient zum Zurückgeben des Server-DTU-Kontingents und des aktuellen DTU-Verbrauchs durch alle Datenbanken auf dem Server.|
|/servers/virtualNetworkRules/delete|Löscht eine vorhandene Regel für virtuelle Netzwerke.|
|/servers/virtualNetworkRules/read|Gibt die Liste der Regeln für virtuelle Netzwerke zurück oder ruft die Eigenschaften für die angegebene Regel für virtuelle Netzwerke ab.|
|/servers/virtualNetworkRules/write|Erstellt eine Regel für virtuelle Netzwerke mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für die angegebene Regel für virtuelle Netzwerke.|
|/servers/write|Erstellt einen Server mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für den angegebenen Server.|
|/unregister/action|Hebt die Registrierung für das Abonnement für den Microsoft SQL-Datenbankressourcenanbieter auf und ermöglicht die Erstellung von Microsoft SQL-Datenbanken.|
|/virtualClusters/read|Gibt die Liste der Regeln für virtuelle Cluster zurück oder ruft die Eigenschaften für das angegebene virtuelle Cluster ab.|
|/virtualClusters/write|Aktualisiert die Markierungen virtueller Cluster.|

## <a name="microsoftstorage"></a>Microsoft.Storage

| Vorgang | BESCHREIBUNG |
|---|---|
|/checknameavailability/read|Prüft, ob der Kontoname gültig ist und noch nicht verwendet wird.|
|/locations/deleteVirtualNetworkOrSubnets/action|Benachrichtigt Microsoft.Storage darüber, dass das virtuelle Netzwerk oder Subnetz gelöscht wird.|
|/operations/read|Fragt den Status eines asynchronen Vorgangs ab.|
|/register/action|Registriert das Abonnement für den Speicherressourcenanbieter und ermöglicht die Erstellung von Speicherkonten.|
|/skus/read|Listet die von Microsoft.Storage unterstützten SKUs auf.|
|/storageAccounts/blobServices/containers/clearLegalHold/action|Löscht die rechtliche Aufbewahrungspflicht für Blobcontainer.|
|/storageAccounts/blobServices/containers/delete|Gibt das Ergebnis beim Löschen eines Containers zurück.|
|/storageAccounts/blobServices/containers/immutabilityPolicies/delete|Löscht die Unveränderlichkeitsrichtlinie für Blobcontainer.|
|/storageAccounts/blobServices/containers/immutabilityPolicies/extend/action|Erweitert die Unveränderlichkeitsrichtlinie für Blobcontainer.|
|/storageAccounts/blobServices/containers/immutabilityPolicies/lock/action|Sperrt die Unveränderlichkeitsrichtlinie für Blobcontainer.|
|/storageAccounts/blobServices/containers/immutabilityPolicies/read|Ruft die Unveränderlichkeitsrichtlinie für Blobcontainer ab.|
|/storageAccounts/blobServices/containers/immutabilityPolicies/write|Legt die Unveränderlichkeitsrichtlinie für Blobcontainer fest.|
|/storageAccounts/blobServices/containers/read|Gibt einen Container oder eine Liste von Containern zurück.|
|/storageAccounts/blobServices/containers/setLegalHold/action|Legt die rechtliche Aufbewahrungspflicht für Blobcontainer fest.|
|/storageAccounts/blobServices/containers/write|Hiermit wird das Ergebnis des Vorgangs zum Festlegen oder Leasen eines Blobcontainers zurückgegeben.|
|/storageAccounts/blobServices/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/storageAccounts/blobServices/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/storageAccounts/blobServices/providers/Microsoft.Insights/metricDefinitions/read|Hiermit wird die Liste der Microsoft Storage-Metrikdefinitionen abgerufen.|
|/storageAccounts/blobServices/read|Gibt Eigenschaften oder Statistiken des Blob-Diensts zurück.|
|/storageAccounts/blobServices/write|Hiermit wird das Ergebnis des Vorgangs zum Festlegen von Eigenschaften des Blob-Diensts zurückgegeben.|
|/storageAccounts/delete|Löscht ein vorhandenes Speicherkonto.|
|/storageAccounts/fileServices/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/storageAccounts/fileServices/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/storageAccounts/fileServices/providers/Microsoft.Insights/metricDefinitions/read|Hiermit wird die Liste der Microsoft Storage-Metrikdefinitionen abgerufen.|
|/storageAccounts/listAccountSas/action|Gibt das Konto-SAS-Token für das angegebene Speicherkonto zurück.|
|/storageAccounts/listkeys/action|Gibt die Zugriffsschlüssel für das angegebene Speicherkonto zurück.|
|/storageAccounts/listServiceSas/action|Gibt das Dienst-SAS-Token für das angegebene Speicherkonto zurück.|
|/storageAccounts/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/storageAccounts/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/storageAccounts/providers/Microsoft.Insights/metricDefinitions/read|Hiermit wird die Liste der Microsoft Storage-Metrikdefinitionen abgerufen.|
|/storageAccounts/queueServices/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/storageAccounts/queueServices/providers/Microsoft.Insights/metricDefinitions/read|Hiermit wird die Liste der Microsoft Storage-Metrikdefinitionen abgerufen.|
|/storageAccounts/queueServices/queues/delete|Gibt das Ergebnis beim Löschen einer Warteschlange zurück.|
|/storageAccounts/queueServices/queues/read|Gibt eine Warteschlange oder eine Liste von Warteschlangen zurück.|
|/storageAccounts/queueServices/queues/write|Gibt das Ergebnis beim Schreiben einer Warteschlange zurück.|
|/storageAccounts/queueServices/read|Gibt Eigenschaften oder Statistiken des Warteschlangendiensts zurück.|
|/storageAccounts/queueServices/write|Gibt das Ergebnis beim Festlegen von Eigenschaften des Warteschlangendiensts zurück.|
|/storageAccounts/read|Gibt die Liste mit Speicherkonten zurück oder ruft die Eigenschaften für das angegebene Speicherkonto ab.|
|/storageAccounts/regeneratekey/action|Generiert die Zugriffsschlüssel für das angegebene Speicherkonto neu.|
|/storageAccounts/services/diagnosticSettings/write|Dient zum Erstellen/Aktualisieren von Speicherkonto-Diagnoseeinstellungen.|
|/storageAccounts/storageAccounts/queueServices/providers/ Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/storageAccounts/tableServices/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/storageAccounts/tableServices/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/storageAccounts/tableServices/providers/Microsoft.Insights/metricDefinitions/read|Hiermit wird die Liste der Microsoft Storage-Metrikdefinitionen abgerufen.|
|/storageAccounts/write|Erstellt ein Speicherkonto mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Tags oder fügt eine benutzerdefinierte Domäne zum angegebenen Speicherkonto hinzu.|
|/usages/read|Gibt den Grenzwert und den aktuellen Verwendungszähler für Ressourcen im angegebenen Abonnement zurück.|

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

| Vorgang | BESCHREIBUNG |
|---|---|
|/storageSyncServices/delete|Löscht Speichersynchronisierungsdienste.|
|/storageSyncServices/providers/Microsoft.Insights/metricDefinitions/read|Ruft die verfügbaren Metriken für Speichersynchronisierungsdienste ab.|
|/storageSyncServices/read|Liest Speichersynchronisierungsdienste.|
|/storageSyncServices/registeredServers/delete|Löscht registrierte Server.|
|/storageSyncServices/registeredServers/read|Liest registrierte Server.|
|/storageSyncServices/registeredServers/write|Erstellt oder aktualisiert registrierte Server.|
|/storageSyncServices/syncGroups/cloudEndpoints/delete|Löscht Cloudendpunkte.|
|/storageSyncServices/syncGroups/cloudEndpoints/operationresults/read|Standort-API für asynchrone Sicherungsaufrufe|
|/storageSyncServices/syncGroups/cloudEndpoints/postbackup/action|Ruft diese Aktion nach der Sicherung auf.|
|/storageSyncServices/syncGroups/cloudEndpoints/postrestore/action|Ruft diese Aktion nach der Wiederherstellung auf.|
|/storageSyncServices/syncGroups/cloudEndpoints/prebackup/action|Ruft diese Aktion vor der Sicherung auf.|
|/storageSyncServices/syncGroups/cloudEndpoints/prerestore/action|Ruft diese Aktion vor der Wiederherstellung auf.|
|/storageSyncServices/syncGroups/cloudEndpoints/read|Liest Cloudendpunkte.|
|/storageSyncServices/syncGroups/cloudEndpoints/restoreheartbeat/action|Stellt den Heartbeat wieder her.|
|/storageSyncServices/syncGroups/cloudEndpoints/write|Hiermit erstellen oder aktualisieren Sie Cloudendpunkte.|
|/storageSyncServices/syncGroups/delete|Löscht Synchronisierungsgruppen.|
|/storageSyncServices/syncGroups/read|Liest Synchronisierungsgruppen.|
|/storageSyncServices/syncGroups/serverEndpoints/delete|Löscht Serverendpunkte.|
|/storageSyncServices/syncGroups/serverEndpoints/read|Liest Serverendpunkte.|
|/storageSyncServices/syncGroups/serverEndpoints/recallAction/action|Ruft diese Aktion auf, um Dateien auf einem Server abzurufen.|
|/storageSyncServices/syncGroups/serverEndpoints/write|Hiermit erstellen oder aktualisieren Sie Serverendpunkte.|
|/storageSyncServices/syncGroups/write|Hiermit erstellen oder aktualisieren Sie Synchronisierungsgruppen.|
|/storageSyncServices/write|Erstellt oder aktualisiert Speichersynchronisierungsdienste.|

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

| Vorgang | BESCHREIBUNG |
|---|---|
|/managers/accessControlRecords/delete|Löscht die Access Control-Datensätze.|
|/managers/accessControlRecords/read|Listet die Access Control-Datensätze auf oder ruft sie ab.|
|/managers/accessControlRecords/write|Dient zum Erstellen oder Aktualisieren der Access Control-Datensätze.|
|/managers/alerts/read|Listet die Warnungen auf oder ruft sie ab.|
|/managers/bandwidthSettings/delete|Löscht vorhandene Bandbreiteneinstellungen (nur 8000er Serie).|
|/managers/bandwidthSettings/read|Listet die Bandbreiteneinstellungen auf (nur 8000er Serie).|
|/managers/bandwidthSettings/write|Erstellt eine neue Bandbreiteneinstellung oder aktualisiert Bandbreiteneinstellungen (nur 8000er Serie).|
|/Managers/certificates/write|Der Vorgang „Ressourcenzertifikat aktualisieren“ aktualisiert das Zertifikat für die Ressourcen-/Tresoranmeldeinformationen.|
|/managers/clearAlerts/action|Dient zum Deaktivieren aller Warnungen im Zusammenhang mit dem Geräte-Manager.|
|/managers/cloudApplianceConfigurations/read|Dient zum Auflisten der vom Cloudgerät unterstützten Konfigurationen.|
|/managers/configureDevice/action|Konfiguriert ein Gerät.|
|/managers/delete|Löscht die Geräte-Manager.|
|/Managers/delete|Der Vorgang „Tresor löschen“ löscht die angegebene Azure-Ressource vom Typ „Tresor“.|
|/managers/devices/alertSettings/read|Listet die Warnungseinstellungen auf oder ruft sie ab.|
|/managers/devices/alertSettings/write|Dient zum Erstellen oder Aktualisieren der Warnungseinstellungen.|
|/managers/devices/backupPolicies/backup/action|Dient zum Erstellen einer manuellen Sicherung, um eine bedarfsgesteuerte Sicherung aller durch die Richtlinie geschützten Volumes zu erstellen.|
|/managers/devices/backupPolicies/delete|Löscht vorhandene Sicherungsrichtlinien (nur 8000er Serie).|
|/managers/devices/backupPolicies/read|Dient zum Auflisten der Sicherungsrichtlinien (nur 8000er Serie).|
|/managers/devices/backupPolicies/schedules/delete|Löscht vorhandene Zeitpläne.|
|/managers/devices/backupPolicies/schedules/read|Dient zum Auflisten der Zeitpläne.|
|/managers/devices/backupPolicies/schedules/write|Erstellt einen neuen Zeitplan oder aktualisiert Zeitpläne.|
|/managers/devices/backupPolicies/write|Erstellt eine neue Sicherungsrichtlinie oder aktualisiert vorhandene Sicherungsrichtlinien (nur 8000er Serie).|
|/managers/devices/backups/delete|Löscht den Sicherungssatz.|
|/managers/devices/backups/elements/clone/action|Dient zum Klonen einer Freigabe oder eines Volumes unter Verwendung eines Sicherungselements.|
|/managers/devices/backups/read|Listet den Sicherungssatz auf oder ruft ihn ab.|
|/managers/devices/backups/restore/action|Dient zum Wiederherstellen aller Volumes aus einem Sicherungssatz.|
|/managers/devices/backupScheduleGroups/delete|Löscht die Sicherungszeitplangruppen.|
|/managers/devices/backupScheduleGroups/read|Listet die Sicherungszeitplangruppen auf oder ruft sie ab.|
|/managers/devices/backupScheduleGroups/write|Dient zum Erstellen oder Aktualisieren der Sicherungszeitplangruppen.|
|/managers/devices/chapSettings/delete|Löscht die CHAP-Einstellungen.|
|/managers/devices/chapSettings/read|Listet die CHAP-Einstellungen auf oder ruft sie ab.|
|/managers/devices/chapSettings/write|Dient zum Erstellen oder Aktualisieren der CHAP-Einstellungen.|
|/managers/devices/deactivate/action|Deaktiviert ein Gerät.|
|/managers/devices/delete|Löscht die Geräte.|
|/managers/devices/download/action|Dient zum Herunterladen von Updates für ein Gerät.|
|/managers/devices/failover/action|Dient zum Ausführen eines Failovers für das Gerät.|
|/managers/devices/fileservers/backup/action|Dient zum Erstellen einer Sicherung eines Dateiservers.|
|/managers/devices/fileservers/delete|Löscht die Dateiserver.|
|/managers/devices/fileservers/metrics/read|Listet die Metriken auf oder ruft sie ab.|
|/managers/devices/fileservers/metricsDefinitions/read|Listet die Metrikdefinitionen auf oder ruft sie ab.|
|/managers/devices/fileservers/read|Listet die Dateiserver auf oder ruft sie ab.|
|/managers/devices/fileservers/shares/delete|Löscht die Freigaben.|
|/managers/devices/fileservers/shares/metrics/read|Listet die Metriken auf oder ruft sie ab.|
|/managers/devices/fileservers/shares/metricsDefinitions/read|Listet die Metrikdefinitionen auf oder ruft sie ab.|
|/managers/devices/fileservers/shares/read|Listet die Freigaben auf oder ruft sie ab.|
|/managers/devices/fileservers/shares/write|Dient zum Erstellen oder Aktualisieren der Freigaben.|
|/managers/devices/fileservers/write|Dient zum Erstellen oder Aktualisieren der Dateiserver.|
|/managers/devices/hardwareComponentGroups/changeControllerPowerState/action|Dient zum Ändern des Controllerenergiezustands von Hardwarekomponentengruppen.|
|/managers/devices/hardwareComponentGroups/read|Dient zum Auflisten der Hardwarekomponentengruppen.|
|/managers/devices/install/action|Dient zum Installieren von Updates auf einem Gerät.|
|/managers/devices/installUpdates/action|Installiert Updates auf den Geräten.|
|/managers/devices/iscsiservers/backup/action|Dient zum Erstellen einer Sicherung eines iSCSI-Servers.|
|/managers/devices/iscsiservers/delete|Löscht die iSCSI-Server.|
|/managers/devices/iscsiservers/disks/delete|Löscht die Datenträger.|
|/managers/devices/iscsiservers/disks/metrics/read|Listet die Metriken auf oder ruft sie ab.|
|/managers/devices/iscsiservers/disks/metricsDefinitions/read|Listet die Metrikdefinitionen auf oder ruft sie ab.|
|/managers/devices/iscsiservers/disks/read|Listet die Datenträger auf oder ruft sie ab.|
|/managers/devices/iscsiservers/disks/write|Dient zum Erstellen oder Aktualisieren der Datenträger.|
|/managers/devices/iscsiservers/metrics/read|Listet die Metriken auf oder ruft sie ab.|
|/managers/devices/iscsiservers/metricsDefinitions/read|Listet die Metrikdefinitionen auf oder ruft sie ab.|
|/managers/devices/iscsiservers/read|Listet die iSCSI-Server auf oder ruft sie ab.|
|/managers/devices/iscsiservers/write|Dient zum Erstellen oder Aktualisieren der iSCSI-Server.|
|/managers/devices/jobs/cancel/action|Dient zum Abbrechen eines ausgeführten Auftrags.|
|/managers/devices/jobs/read|Listet die Aufträge auf oder ruft sie ab.|
|/managers/devices/listFailoverSets/action|Dient zum Auflisten der Failovergruppen für ein vorhandenes Gerät.|
|/managers/devices/listFailoverTargets/action|Dient zum Auflisten von Failoverzielen der Geräte.|
|/managers/devices/metrics/read|Listet die Metriken auf oder ruft sie ab.|
|/managers/devices/metricsDefinitions/read|Listet die Metrikdefinitionen auf oder ruft sie ab.|
|/managers/devices/migrationSourceConfigurations/confirmMigration/action|Bestätigt eine erfolgreiche Migration und committet sie.|
|/managers/devices/migrationSourceConfigurations/fetchConfirmMigrationStatus/action|Dient zum Abrufen des Migrationsbestätigungsstatus.|
|/managers/devices/migrationSourceConfigurations/fetchMigrationEstimate/action|Dient zum Abrufen des Status für den Migrationsschätzauftrag.|
|/managers/devices/migrationSourceConfigurations/fetchMigrationStatus/action|Dient zum Abrufen des Status für die Migration.|
|/managers/devices/migrationSourceConfigurations/import/action|Dient zum Importieren von Quellkonfigurationen für die Migration.|
|/managers/devices/migrationSourceConfigurations/startMigration/action|Dient zum Starten der Migration unter Verwendung von Quellkonfigurationen.|
|/managers/devices/migrationSourceConfigurations/startMigrationEstimate/action|Dient zum Starten eines Auftrags zur Ermittlung der voraussichtlichen Dauer des Migrationsvorgangs.|
|/managers/devices/networkSettings/read|Listet die Netzwerkeinstellungen auf oder ruft sie ab.|
|/managers/devices/networkSettings/write|Erstellt eine neue Netzwerkeinstellung oder aktualisiert Netzwerkeinstellungen.|
|/managers/devices/publicEncryptionKey/action|Dient zum Auflisten öffentlicher Verschlüsselungsschlüssel des Geräte-Managers.|
|/managers/devices/publishSupportPackage/action|Dient zum Veröffentlichen des Supportpakets eines Geräts für die Problembehandlung durch den Microsoft-Support.|
|/managers/devices/read|Listet die Geräte auf oder ruft sie ab.|
|/managers/devices/scanForUpdates/action|Dient zum Suchen nach Updates auf einem Gerät.|
|/managers/devices/securitySettings/read|Dient zum Auflisten der Sicherheitseinstellungen.|
|/managers/devices/securitySettings/syncRemoteManagementCertificate/action|Dient zum Synchronisieren des Remoteverwaltungszertifikats für ein Gerät.|
|/managers/devices/securitySettings/update/action|Dient zum Aktualisieren der Sicherheitseinstellungen.|
|/managers/devices/securitySettings/write|Erstellt eine neue Sicherheitseinstellung oder aktualisiert Sicherheitseinstellungen.|
|/managers/devices/sendTestAlertEmail/action|Dient zum Senden einer Testwarnungs-E-Mail an konfigurierte E-Mail-Empfänger.|
|/managers/devices/timeSettings/read|Listet die Uhrzeiteinstellungen auf oder ruft sie ab.|
|/managers/devices/timeSettings/write|Erstellt eine neue Uhrzeiteinstellung oder aktualisiert Uhrzeiteinstellungen.|
|/managers/devices/updateSummary/read|Listet die Updatezusammenfassung auf oder ruft sie ab.|
|/managers/devices/volumeContainers/delete|Löscht vorhandene Volumecontainer (nur 8000er Serie).|
|/managers/devices/volumeContainers/listEncryptionKeys/action|Dient zum Auflisten von Verschlüsselungsschlüsseln von Volumecontainern.|
|/managers/devices/volumeContainers/metrics/read|Dient zum Auflisten der Metriken.|
|/managers/devices/volumeContainers/metricsDefinitions/read|Dient zum Auflisten der Metrikdefinitionen.|
|/managers/devices/volumeContainers/read|Dient zum Auflisten der Volumecontainer (nur 8000er Serie).|
|/managers/devices/volumeContainers/rolloverEncryptionKey/action|Dient zum Ausführen eines Verschlüsselungsschlüssel-Rollovers für Volumecontainer.|
|/managers/devices/volumeContainers/volumes/delete|Löscht vorhandene Volumes.|
|/managers/devices/volumeContainers/volumes/metrics/read|Dient zum Auflisten der Metriken.|
|/managers/devices/volumeContainers/volumes/metricsDefinitions/read|Dient zum Auflisten der Metrikdefinitionen.|
|/managers/devices/volumeContainers/volumes/read|Dient zum Auflisten der Volumes.|
|/managers/devices/volumeContainers/volumes/write|Erstellt ein neues Volume oder aktualisiert Volumes.|
|/managers/devices/volumeContainers/write|Erstellt einen neuen Volumecontainer oder aktualisiert Volumecontainer (nur 8000er Serie).|
|/managers/devices/write|Dient zum Erstellen oder Aktualisieren der Geräte.|
|/managers/encryptionSettings/read|Listet die Verschlüsselungseinstellungen auf oder ruft sie ab.|
|/Managers/extendedInformation/delete|Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt.|
|/Managers/extendedInformation/read|Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt.|
|/Managers/extendedInformation/write|Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt.|
|/managers/getActivationKey/action|Dient zum Abrufen des Aktivierungsschlüssels für den Geräte-Manager.|
|/managers/getEncryptionKey/action|Dient zum Abrufen des Verschlüsselungsschlüssels für den Geräte-Manager.|
|/managers/listActivationKey/action|Ruft den Aktivierungsschlüssel des StorSimple-Geräte-Managers ab.|
|/managers/listPrivateEncryptionKey/action|Ruft den privaten Verschlüsselungsschlüssel für einen StorSimple-Geräte-Manager ab.|
|/managers/listPublicEncryptionKey/action|Dient zum Auflisten öffentlicher Verschlüsselungsschlüssel eines StorSimple-Geräte-Managers.|
|/managers/metrics/read|Listet die Metriken auf oder ruft sie ab.|
|/managers/metricsDefinitions/read|Listet die Metrikdefinitionen auf oder ruft sie ab.|
|/managers/provisionCloudAppliance/action|Dient zum Erstellen eines neuen Cloudgeräts.|
|/managers/read|Listet die Geräte-Manager auf oder ruft sie ab.|
|/Managers/read|Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt.|
|/managers/regenarateRegistationCertificate/action|Dient zum erneuten Generieren des Registrierungszertifikats für den Geräte-Manager.|
|/managers/regenerateActivationKey/action|Dient zum erneuten Generieren des Aktivierungsschlüssels für den Geräte-Manager.|
|/managers/storageAccountCredentials/delete|Löscht die Speicherkonto-Anmeldedaten.|
|/managers/storageAccountCredentials/listAccessKey/action|Dient zum Auflisten von Zugriffsschlüsseln der Speicherkonto-Anmeldedaten.|
|/managers/storageAccountCredentials/read|Listet die Speicherkonto-Anmeldedaten auf oder ruft sie ab.|
|/managers/storageAccountCredentials/write|Dient zum Erstellen oder Aktualisieren der Speicherkonto-Anmeldedaten.|
|/managers/storageDomains/delete|Löscht die Speicherdomänen.|
|/managers/storageDomains/read|Listet die Speicherdomänen auf oder ruft sie ab.|
|/managers/storageDomains/write|Dient zum Erstellen oder Aktualisieren der Speicherdomänen.|
|/managers/write|Dient zum Erstellen oder Aktualisieren der Geräte-Manager.|
|/Managers/write|Der Vorgang „Tresor erstellen“ erstellt eine Azure-Ressource vom Typ „Tresor“.|

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

| Vorgang | BESCHREIBUNG |
|---|---|
|/locations/quotas/Read|Liest das Stream Analytics-Abonnementkontingent.|
|/operations/Read|Liest Stream Analytics-Vorgänge.|
|/Register/action|Registriert das Abonnement beim Stream Analytics-Ressourcenanbieter.|
|/streamingjobs/Delete|Dient zum Löschen von Stream Analytics-Aufträgen.|
|/streamingjobs/functions/Delete|Löscht die Stream Analytics-Auftragsfunktion.|
|/streamingjobs/functions/operationresults/Read|Liest die Vorgangsergebnisse für die Stream Analytics-Auftragsfunktion.|
|/streamingjobs/functions/Read|Liest die Stream Analytics-Auftragsfunktion.|
|/streamingjobs/functions/RetrieveDefaultDefinition/action|Ruft die Standarddefinition einer Stream Analytics-Auftragsfunktion ab.|
|/streamingjobs/functions/Test/action|Testet die Stream Analytics-Auftragsfunktion.|
|/streamingjobs/functions/Write|Schreibt die Stream Analytics-Auftragsfunktion.|
|/streamingjobs/inputs/Delete|Dient zum Löschen von Stream Analytics-Auftragseingaben.|
|/streamingjobs/inputs/operationresults/Read|Liest die Vorgangsergebnisse für die Stream Analytics-Auftragseingabe.|
|/streamingjobs/inputs/Read|Dient zum Lesen von Stream Analytics-Auftragseingaben.|
|/streamingjobs/inputs/Sample/action|Stream Analytics-Auftragseingaben|
|/streamingjobs/inputs/Test/action|Dient zum Testen von Stream Analytics-Auftragseingaben.|
|/streamingjobs/inputs/Write|Dient zum Schreiben von Stream Analytics-Auftragseingaben.|
|/streamingjobs/metricdefinitions/Read|Dient zum Lesen von Metrikdefinitionen.|
|/streamingjobs/operationresults/Read|Liest die Vorgangsergebnisse für den Stream Analytics-Auftrag.|
|/streamingjobs/outputs/Delete|Dient zum Löschen von Stream Analytics-Auftragsausgaben.|
|/streamingjobs/outputs/operationresults/Read|Liest die Vorgangsergebnisse für die Stream Analytics-Auftragsausgabe.|
|/streamingjobs/outputs/Read|Dient zum Lesen von Stream Analytics-Auftragsausgaben.|
|/streamingjobs/outputs/Test/action|Dient zum Testen der Stream Analytics-Auftragsausgabe.|
|/streamingjobs/outputs/Write|Dient zum Schreiben von Stream Analytics-Auftragsausgaben.|
|/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/read|Dient zum Lesen der Diagnoseeinstellung.|
|/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/write|Dient zum Schreiben der Diagnoseeinstellung.|
|/streamingjobs/providers/Microsoft.Insights/logDefinitions/read|Ruft die verfügbaren Protokolle für Streamingaufträge ab.|
|/streamingjobs/providers/Microsoft.Insights/metricDefinitions/read|Ruft die verfügbaren Metriken für Streamingaufträge ab.|
|/streamingjobs/Read|Dient zum Lesen von Stream Analytics-Aufträgen.|
|/streamingjobs/Start/action|Dient zum Starten von Stream Analytics-Aufträgen.|
|/streamingjobs/Stop/action|Dient zum Beenden von Stream Analytics-Aufträgen.|
|/streamingjobs/transformations/Delete|Dient zum Löschen von Stream Analytics-Auftragstransformationen.|
|/streamingjobs/transformations/Read|Dient zum Lesen von Stream Analytics-Auftragstransformationen.|
|/streamingjobs/transformations/Write|Dient zum Schreiben von Stream Analytics-Auftragstransformationen.|
|/streamingjobs/Write|Dient zum Schreiben von Stream Analytics-Aufträgen.|

## <a name="microsoftsubscription"></a>Microsoft.Subscription

| Vorgang | BESCHREIBUNG |
|---|---|
|/SubscriptionDefinitions/read|Ruft die Definition eines Azure-Abonnements in einer Verwaltungsgruppe ab.|
|/SubscriptionDefinitions/write|Erstellt die Definition für ein Azure-Abonnement.|

## <a name="microsoftsupport"></a>Microsoft.Support

| Vorgang | BESCHREIBUNG |
|---|---|
|/register/action|Führt die Registrierung beim Supportressourcenanbieter durch.|
|/supportTickets/read|Ruft Details zu Supporttickets (einschließlich Status, Schweregrad, Kontaktdetails und Kommunikation) oder die Liste mit Supporttickets (abonnementübergreifend) ab.|
|/supportTickets/write|Erstellt oder aktualisiert ein Supportticket. Sie können ein Supportticket zu technischen Problemen bzw. zu Problemen im Zusammenhang mit Abrechnung, Kontingenten oder Abonnementverwaltung erstellen. Der Schweregrad, die Kontaktinformationen und die Kommunikation für vorhandene Supporttickets können aktualisiert werden.|

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

| Vorgang | BESCHREIBUNG |
|---|---|
|/environments/accesspolicies/delete|Löscht die Zugriffsrichtlinie.|
|/environments/accesspolicies/read|Dient zum Abrufen der Eigenschaften einer Zugriffsrichtlinie.|
|/environments/accesspolicies/write|Erstellt eine neue Zugriffsrichtlinie für eine Umgebung oder aktualisiert eine vorhandene Zugriffsrichtlinie.|
|/environments/delete|Löscht die Umgebung.|
|/environments/eventsources/delete|Löscht die Ereignisquelle.|
|/environments/eventsources/eventsources/providers/Microsoft.Insights/ diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/environments/eventsources/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/environments/eventsources/providers/Microsoft.Insights/metricDefinitions/read|Ruft die verfügbaren Metriken für die Ereignisquellen ab.|
|/environments/eventsources/read|Dient zum Abrufen der Eigenschaften einer Ereignisquelle.|
|/environments/eventsources/write|Erstellt eine neue Ereignisquelle für eine Umgebung oder aktualisiert eine vorhandene Ereignisquelle.|
|/environments/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/environments/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/environments/providers/Microsoft.Insights/metricDefinitions/read|Ruft die verfügbaren Metriken für die Umgebungen ab.|
|/environments/read|Dient zum Abrufen der Eigenschaften einer Umgebung.|
|/environments/referencedatasets/delete|Löscht das Verweisdataset.|
|/environments/referencedatasets/read|Dient zum Abrufen der Eigenschaften eines Verweisdatasets.|
|/environments/referencedatasets/write|Erstellt ein neues Verweisdataset für eine Umgebung oder aktualisiert ein vorhandenes Verweisdataset.|
|/environments/status/read|Ruft den Status der Umgebung und den Status der zugehörigen Vorgänge wie eingehende Daten ab.|
|/environments/write|Erstellt eine neue Umgebung oder aktualisiert eine vorhandene Umgebung.|
|/register/action|Registriert das Abonnement für den Time Series Insights-Ressourcenanbieter und ermöglicht die Erstellung von Time Series Insights-Umgebungen.|

## <a name="microsoftweb"></a>microsoft.web

| Vorgang | BESCHREIBUNG |
|---|---|
|/apimanagementaccounts/apiacls/read|Ruft die API-ACLs der API Management-Konten ab.|
|/apimanagementaccounts/apis/apiacls/delete|Löscht die API-ACLs für die APIs der API Management-Konten.|
|/apimanagementaccounts/apis/apiacls/read|Ruft die API-ACLs für die APIs der API Management-Konten ab.|
|/apimanagementaccounts/apis/apiacls/write|Aktualisiert die API-ACLs für die APIs der API Management-Konten.|
|/apimanagementaccounts/apis/connectionacls/read|Ruft die Verbindungs-ACLs für die APIs der API Management-Konten ab.|
|/apimanagementaccounts/apis/connections/confirmconsentcode/action|Bestätigt den Zustimmungscode für die API-Verbindungen der API Management-Konten.|
|/apimanagementaccounts/apis/connections/connectionacls/delete|Löscht die Verbindungs-ACLs für die API-Verbindungen der API Management-Konten.|
|/apimanagementaccounts/apis/connections/connectionacls/read|Ruft die Verbindungs-ACLs für die API-Verbindungen der API Management-Konten ab.|
|/apimanagementaccounts/apis/connections/connectionacls/write|Aktualisiert die Verbindungs-ACLs für die API-Verbindungen der API Management-Konten.|
|/apimanagementaccounts/apis/connections/delete|Löscht die API-Verbindungen der API Management-Konten.|
|/apimanagementaccounts/apis/connections/getconsentlinks/action|Ruft die Zustimmungslinks für die API-Verbindungen der API Management-Konten ab.|
|/apimanagementaccounts/apis/connections/listconnectionkeys/action|Listet die Verbindungsschlüssel für die API-Verbindungen der API Management-Konten auf.|
|/apimanagementaccounts/apis/connections/listsecrets/action|Listet die Geheimnisse für die API-Verbindungen der API Management-Konten auf.|
|/apimanagementaccounts/apis/connections/read|Ruft die API-Verbindungen der API Management-Konten ab.|
|/apimanagementaccounts/apis/connections/write|Aktualisiert die API-Verbindungen der API Management-Konten.|
|/apimanagementaccounts/apis/delete|Löscht die APIs der API Management-Konten.|
|/apimanagementaccounts/apis/localizeddefinitions/delete|Löscht die lokalisierten Definitionen für die APIs der API Management-Konten.|
|/apimanagementaccounts/apis/localizeddefinitions/read|Ruft die lokalisierten Definitionen für die APIs der API Management-Konten ab.|
|/apimanagementaccounts/apis/localizeddefinitions/write|Aktualisiert die lokalisierten Definitionen für die APIs der API Management-Konten.|
|/apimanagementaccounts/apis/read|Ruft die APIs der API Management-Konten ab.|
|/apimanagementaccounts/apis/write|Aktualisiert die APIs der API Management-Konten.|
|/apimanagementaccounts/connectionacls/read|Ruft die Verbindungs-ACLs der API Management-Konten ab.|
|/availablestacks/read|Dient zum Abrufen verfügbarer Stapel.|
|/billingmeters/read|Ruft die Liste der Verbrauchseinheiten für die Abrechnung ab.|
|/certificates/Delete|Dient zum Löschen eines vorhandenen Zertifikats.|
|/certificates/Read|Dient zum Abrufen der Zertifikatliste.|
|/certificates/Write|Dient zum Hinzufügen eines neuen Zertifikats oder zum Aktualisieren eines bereits vorhandenen.|
|/checknameavailability/read|Dient zum Prüfen, ob ein Ressourcenname verfügbar ist.|
|/classicmobileservices/read|Dient zum Abrufen von Mobile Services (klassisch).|
|/connectionGateways/Delete|Löscht ein Verbindungsgateway.|
|/connectionGateways/Join/Action|Verknüpft ein Verbindungsgateway.|
|/connectiongateways/liststatus/action|Listet den Status von Verbindungsgateways auf.|
|/connectionGateways/ListStatus/Action|Listet den Status eines Verbindungsgateways auf.|
|/connectionGateways/Move/Action|Verschiebt ein Verbindungsgateway.|
|/connectionGateways/Read|Dient zum Abrufen der Verbindungsgatewayliste.|
|/connectionGateways/Write|Erstellt oder aktualisiert ein Verbindungsgateway.|
|/connections/confirmconsentcode/action|Dient zum Bestätigen des Genehmigungscodes für Verbindungen.|
|/connections/Delete|Löscht eine Verbindung.|
|/connections/Join/Action|Verknüpft eine Verbindung.|
|/connections/listconsentlinks/action|Dient zum Auflisten von Genehmigungslinks für Verbindungen.|
|/connections/Move/Action|Verschiebt eine Verbindung.|
|/connections/Read|Dient zum Abrufen der Verbindungsliste.|
|/connections/Write|Erstellt oder aktualisiert eine Verbindung.|
|/customApis/Delete|Löscht eine benutzerdefinierte API.|
|/customApis/extractApiDefinitionFromWsdl/Action|Extrahiert eine API-Definition aus einer WSDL-Schnittstelle.|
|/customApis/Join/Action|Verknüpft eine benutzerdefinierte API.|
|/customApis/listWsdlInterfaces/Action|Listet WSDL-Schnittstellen für eine benutzerdefinierte API auf.|
|/customApis/Move/Action|Verschiebt eine benutzerdefinierte API|
|/customApis/Read|Dient zum Abrufen einer benutzerdefinierten API.|
|/customApis/Write|Erstellt oder aktualisiert eine Verbindungs-API.|
|/deploymentlocations/read|Dient zum Abrufen von Bereitstellungsorten.|
|/geoRegions/Read|Ruft die Liste mit geografischen Regionen ab.|
|/hostingenvironments/capacities/read|Dient zum Abrufen von Hostingumgebungskapazitäten.|
|/hostingEnvironments/Delete|Dient zum Löschen einer App Service-Umgebung.|
|/hostingenvironments/diagnostics/read|Dient zum Abrufen von Hostingumgebungsdiagnosen.|
|/hostingenvironments/inboundnetworkdependenciesendpoints/read|Ruft die Netzwerkendpunkte aller eingehenden Abhängigkeiten ab.|
|/hostingenvironments/metricdefinitions/read|Dient zum Abrufen von Metrikdefinitionen für Hostingumgebungen.|
|/hostingenvironments/multirolepools/metricdefinitions/read|Dient zum Abrufen von MultiRole-Poolmetrikdefinitionen für Hostingumgebungen.|
|/hostingenvironments/multirolepools/metrics/read|Dient zum Abrufen von MultiRole-Poolmetriken für Hostingumgebungen.|
|/hostingEnvironments/multiRolePools/providers/Microsoft.Insights/ metricDefinitions/Read|Ruft die verfügbaren MultiRole-Metriken für die App Service-Umgebung ab.|
|/hostingEnvironments/multiRolePools/Read|Dient zum Abrufen der Eigenschaften eines FrontEnd-Pools in einer App Service-Umgebung.|
|/hostingenvironments/multirolepools/skus/read|Dient zum Abrufen von MultiRole-Pool-SKUs für Hostingumgebungen.|
|/hostingenvironments/multirolepools/usages/read|Dient zum Abrufen von MultiRole-Poolverwendungen für Hostingumgebungen.|
|/hostingEnvironments/multiRolePools/Write|Dient zum Erstellen eines neuen FrontEnd-Pools in einer App Service-Umgebung oder zum Aktualisieren eines bereits vorhandenen.|
|/hostingenvironments/operations/read|Dient zum Abrufen von Hostingumgebungsvorgängen.|
|/hostingenvironments/outboundnetworkdependenciesendpoints/read|Ruft die Netzwerkendpunkte aller ausgehenden Abhängigkeiten ab.|
|/hostingenvironments/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/hostingenvironments/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/hostingEnvironments/Read|Dient zum Abrufen der Eigenschaften einer App Service-Umgebung.|
|/hostingEnvironments/reboot/Action|Dient zum Neustarten aller Computer in einer App Service-Umgebung.|
|/hostingenvironments/resume/action|Dient zum Fortsetzen von Hostingumgebungen.|
|/hostingenvironments/serverfarms/read|Dient zum Abrufen von App Service-Plänen für Hostingumgebungen.|
|/hostingenvironments/sites/read|Dient zum Abrufen von Web-Apps für Hostingumgebungen.|
|/hostingenvironments/suspend/action|Dient zum Anhalten von Hostingumgebungen.|
|/hostingenvironments/usages/read|Dient zum Abrufen von Hostingumgebungsverwendungen.|
|/hostingenvironments/workerpools/metricdefinitions/read|Dient zum Abrufen von Workerpool-Metrikdefinitionen für Hostingumgebungen.|
|/hostingenvironments/workerpools/metrics/read|Dient zum Abrufen von Workerpoolmetriken für Hostingumgebungen.|
|/hostingEnvironments/workerPools/providers/Microsoft.Insights/metricDefinitions/Read|Ruft die verfügbaren WorkerPool-Metriken für die App Service-Umgebung ab.|
|/hostingEnvironments/workerPools/Read|Dient zum Abrufen der Eigenschaften eines Workerpools in einer App Service-Umgebung.|
|/hostingenvironments/workerpools/skus/read|Dient zum Abrufen von Workerpool-SKUs für Hostingumgebungen.|
|/hostingenvironments/workerpools/usages/read|Dient zum Abrufen von Workerpoolverwendungen für Hostingumgebungen.|
|/hostingEnvironments/workerPools/Write|Dient zum Erstellen eines neuen Workerpools in einer App Service-Umgebung oder zum Aktualisieren eines bereits vorhandenen.|
|/hostingEnvironments/Write|Dient zum Erstellen einer neuen App Service-Umgebung oder zum Aktualisieren einer bereits vorhandenen.|
|/ishostingenvironmentnameavailable/read|Dient zum Abrufen, ob der Hostingumgebungsname verfügbar ist.|
|/ishostnameavailable/read|Dient zum Prüfen, ob der Hostname verfügbar ist.|
|/isusernameavailable/read|Dient zum Prüfen, ob der Benutzername verfügbar ist.|
|/listSitesAssignedToHostName/Read|Dient zum Abrufen der Namen von Websites, die dem Hostnamen zugewiesen sind.|
|/locations/apioperations/read|Dient zum Abrufen von Standorten für API-Vorgänge.|
|/locations/connectiongatewayinstallations/read|Dient zum Abrufen von Standorten für Verbindungsgatewayinstallationen.|
|/locations/extractapidefinitionfromwsdl/action|Extrahiert eine API-Definition aus einer WSDL-Schnittstelle für Speicherorte.|
|/locations/listwsdlinterfaces/action|Listet WSDL-Schnittstellen für Speicherorte auf.|
|/locations/managedapis/apioperations/read|Dient zum Abrufen verwalteter API-Vorgänge.|
|/locations/managedapis/Join/Action|Verknüpft eine verwaltete API.|
|/locations/managedapis/read|Dient zum Abrufen verwalteter APIs für Standorte.|
|/operations/read|Dient zum Abrufen von Vorgängen.|
|/publishingusers/read|Dient zum Abrufen von Veröffentlichungsbenutzern.|
|/publishingusers/write|Dient zum Aktualisieren von Veröffentlichungsbenutzern.|
|/recommendations/Read|Dient zum Abrufen der Liste mit Empfehlungen für Abonnements.|
|/register/action|Dient zum Registrieren des Microsoft.Web-Ressourcenanbieters für das Abonnement.|
|/resourcehealthmetadata/read|Ruft Metadaten zur Ressourcenintegrität ab.|
|/serverfarms/capabilities/read|Dient zum Abrufen von Funktionen für App Service-Pläne.|
|/serverfarms/Delete|Löschen eines vorhandenen App Service-Plans|
|/serverfarms/firstpartyapps/settings/delete|Dient zum Löschen von Erstanbieter-App-Einstellungen für App Service-Pläne.|
|/serverfarms/firstpartyapps/settings/read|Dient zum Abrufen von Erstanbieter-App-Einstellungen für App Service-Pläne.|
|/serverfarms/firstpartyapps/settings/write|Dient zum Aktualisieren von Erstanbieter-App-Einstellungen für App Service-Pläne.|
|/serverfarms/hybridconnectionnamespaces/relays/delete|Dient zum Löschen von Relaynamespace-Agents für Hybridverbindungen für App Service-Pläne.|
|/serverfarms/hybridconnectionnamespaces/relays/read|Dient zum Abrufen von Relaynamespace-Agents für Hybridverbindungen für App Service-Pläne.|
|/serverfarms/hybridconnectionnamespaces/relays/sites/read|Dient zum Abrufen von Hybridverbindungsnamespace-Relay-Web-Apps für App Service-Pläne.|
|/serverfarms/hybridconnectionplanlimits/read|Dient zum Abrufen von Hybridverbindungsplan-Grenzwerten für App Service-Pläne.|
|/serverfarms/hybridconnectionrelays/read|Dient zum Abrufen von Hybridverbindungsrelays für App Service-Pläne.|
|/serverfarms/metricdefinitions/read|Dient zum Abrufen von Metrikdefinitionen für App Service-Pläne.|
|/serverfarms/metrics/read|Dient zum Abrufen von Metriken für App Service-Pläne.|
|/serverfarms/operationresults/read|Dient zum Abrufen von Vorgangsergebnissen für App Service-Pläne.|
|/serverfarms/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/serverfarms/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/serverfarms/providers/Microsoft.Insights/metricDefinitions/Read|Ruft die verfügbaren Metriken für einen App Service-Plan ab.|
|/serverfarms/Read|Dient zum Abrufen der Eigenschaften für einen App Service-Plan.|
|/serverfarms/restartSites/Action|Dient zum Neustarten aller Web-Apps in einem App Service-Plan.|
|/serverfarms/sites/read|Dient zum Abrufen von Web-Apps für App Service-Pläne.|
|/serverfarms/skus/read|Dient zum Abrufen von SKUs für App Service-Pläne.|
|/serverfarms/usages/read|Dient zum Abrufen von Verwendungen für App Service-Pläne.|
|/serverfarms/virtualnetworkconnections/gateways/write|Dient zum Aktualisieren von Gateways für Verbindungen mit virtuellen Netzwerken für App Service-Pläne.|
|/serverfarms/virtualnetworkconnections/read|Dient zum Abrufen von Verbindungen mit virtuellen Netzwerken für App Service-Pläne.|
|/serverfarms/virtualnetworkconnections/routes/delete|Dient zum Löschen von Routen für Verbindungen mit virtuellen Netzwerken für App Service-Pläne.|
|/serverfarms/virtualnetworkconnections/routes/read|Dient zum Abrufen von Routen für Verbindungen mit virtuellen Netzwerken für App Service-Pläne.|
|/serverfarms/virtualnetworkconnections/routes/write|Dient zum Aktualisieren von Routen für Verbindungen mit virtuellen Netzwerken für App Service-Pläne.|
|/serverfarms/workers/reboot/action|Dient zum Neustarten von Workern für App Service-Pläne.|
|/serverfarms/Write|Dient zum Erstellen eines neuen App Service-Plans oder zum Aktualisieren eines bereits vorhandenen.|
|/sites/analyzecustomhostname/read|Dient zum Analysieren des benutzerdefinierten Hostnamens.|
|/sites/applySlotConfig/Action|Dient zum Anwenden der Web-App-Slotkonfiguration aus dem Zielslot auf die aktuelle Web-App.|
|/sites/backup/Action|Dient zum Erstellen einer neuen Web-App-Sicherung.|
|/sites/backup/read|Dient zum Abrufen der Web-App-Sicherung.|
|/sites/backup/write|Dient zum Aktualisieren der Web-App-Sicherung.|
|/sites/backups/delete|Dient zum Löschen von Web-App-Sicherungen.|
|/sites/backups/list/action|Dient zum Auflisten von Web-App-Sicherungen.|
|/sites/backups/Read|Dient zum Abrufen der Eigenschaften einer Web-App-Sicherung.|
|/sites/backups/restore/action|Dient zum Wiederherstellen von Web-App-Sicherungen.|
|/sites/config/delete|Dient zum Löschen der Web-App-Konfiguration.|
|/sites/config/list/Action|Dient zum Auflisten der sicherheitsrelevanten Einstellungen der Web-App. Hierzu zählen etwa Veröffentlichungsanmeldeinformationen, App-Einstellungen und Verbindungszeichenfolgen.|
|/sites/config/Read|Dient zum Abrufen von Web-App-Konfigurationseinstellungen.|
|/sites/config/Write|Dient zum Aktualisieren der Konfigurationseinstellungen der Web-App.|
|/sites/continuouswebjobs/delete|Dient zum Löschen fortlaufender Webaufträge für Web-Apps.|
|/sites/continuouswebjobs/read|Dient zum Abrufen fortlaufender Webaufträge für Web-Apps.|
|/sites/continuouswebjobs/start/action|Dient zum Starten fortlaufender Webaufträge für Web-Apps.|
|/sites/continuouswebjobs/stop/action|Dient zum Beenden fortlaufender Webaufträge für Web-Apps.|
|/sites/Delete|Dient zum Löschen einer vorhandenen Web-App.|
|/sites/deployments/delete|Dient zum Löschen von Web-App-Bereitstellungen.|
|/sites/deployments/log/read|Dient zum Abrufen des Web-App-Bereitstellungsprotokolls.|
|/sites/deployments/read|Dient zum Abrufen von Web-App-Bereitstellungen.|
|/sites/deployments/write|Dient zum Aktualisieren von Web-App-Bereitstellungen.|
|/sites/diagnostics/analyses/execute/Action|Dient zum Ausführen der Analyse von Web-Apps-Diagnosen.|
|/sites/diagnostics/analyses/read|Dient zum Abrufen der Analyse von Web-Apps-Diagnosen.|
|/sites/diagnostics/aspnetcore/read|Ruft Web-Apps-Diagnosen für die ASP.NET Core-App ab.|
|/sites/diagnostics/autoheal/read|Dient zum Abrufen von AutoHeal für Web-Apps-Diagnosen.|
|/sites/diagnostics/deployment/read|Dient zum Abrufen einer Bereitstellung für Web-Apps-Diagnosen.|
|/sites/diagnostics/deployments/read|Dient zum Abrufen von Bereitstellungen für Web-Apps-Diagnosen.|
|/sites/diagnostics/detectors/execute/Action|Dient zum Ausführen der Erkennung von Web-Apps-Diagnosen.|
|/sites/diagnostics/detectors/read|Dient zum Abrufen der Erkennung von Web-Apps-Diagnosen.|
|/sites/diagnostics/failedrequestsperuri/read|Dient zum Abrufen fehlerhafter Anforderungen bei Web-Apps-Diagnosen pro URI.|
|/sites/diagnostics/frebanalysis/read|Dient zum Abrufen der FREB-Analyse von Web-App-Diagnosen.|
|/sites/diagnostics/loganalyzer/read|Dient zum Abrufen der Protokollanalyse von Web-Apps-Diagnosen.|
|/sites/diagnostics/read|Dient zum Abrufen der Kategorien von Web-Apps-Diagnosen.|
|/sites/diagnostics/runtimeavailability/read|Dient zum Abrufen der Laufzeitverfügbarkeit für Web-App-Diagnosen.|
|/sites/diagnostics/servicehealth/read|Dient zum Abrufen der Dienstintegrität von Web-App-Diagnosen.|
|/sites/diagnostics/sitecpuanalysis/read|Dient zum Abrufen der CPU-Analyse von Websites von Web-Apps-Diagnosen.|
|/sites/diagnostics/sitecrashes/read|Dient zum Abrufen von Websiteabstürzen von Web-Apps-Diagnosen.|
|/sites/diagnostics/sitelatency/read|Dient zum Abrufen von Websitelatenzen von Web-Apps-Diagnosen.|
|/sites/diagnostics/sitememoryanalysis/read|Dient zum Abrufen der Websitespeicheranalyse von Web-Apps-Diagnosen.|
|/sites/diagnostics/siterestartsettingupdate/read|Dient zum Abrufen des Websiteupdates für die Einstellungen zum Neustarten der Web-Apps-Diagnosen.|
|/sites/diagnostics/siterestartuserinitiated/read|Dient zum Abrufen des vom Benutzer initiierten Websiteneustarts der Web-Apps-Diagnosen.|
|/sites/diagnostics/siteswap/read|Dient zum Abrufen des Websiteaustauschs von Web-Apps-Diagnosen.|
|/sites/diagnostics/threadcount/read|Dient zum Abrufen der Threadanzahl von Web-Apps-Diagnosen.|
|/sites/diagnostics/workeravailability/read|Dient zum Abrufen der Workerverfügbarkeit für Web-App-Diagnosen.|
|/sites/diagnostics/workerprocessrecycle/read|Dient zum Abrufen der Workerprozesswiederverwendung für Web-App-Diagnosen.|
|/sites/domainownershipidentifiers/read|Dient zum Abrufen von Domänenbesitz-IDs für Web-Apps.|
|/sites/domainownershipidentifiers/write|Dient zum Aktualisieren von Domänenbesitz-IDs für Web-Apps.|
|/sites/functions/action|Functions-Web-Apps|
|/sites/functions/delete|Dient zum Löschen von Web-App-Funktionen.|
|/sites/functions/listsecrets/action|Dient zum Auflisten der Geheimnisse für Web-App-Funktionen.|
|/sites/functions/masterkey/read|Dient zum Abrufen des Hauptschlüssels von Web-Apps-Funktionen.|
|/sites/functions/read|Dient zum Abrufen von Web-App-Funktionen.|
|/sites/functions/token/read|Dient zum Abrufen des Funktionstokens von Web-Apps.|
|/sites/functions/write|Dient zum Aktualisieren von Web-App-Funktionen.|
|/sites/hostnamebindings/delete|Dient zum Löschen von Hostnamenbindungen für Web-Apps.|
|/sites/hostnamebindings/read|Dient zum Abrufen von Hostnamenbindungen für Web-Apps.|
|/sites/hostnamebindings/write|Dient zum Aktualisieren von Hostnamenbindungen für Web-Apps.|
|/sites/hybridconnection/delete|Dient zum Löschen der Hybridverbindung für Web-Apps.|
|/sites/hybridconnection/read|Dient zum Abrufen der Hybridverbindung für Web-Apps.|
|/sites/hybridconnection/write|Dient zum Aktualisieren der Hybridverbindung für Web-Apps.|
|/sites/hybridconnectionnamespaces/relays/delete|Dient zum Löschen der Relaynamespace-Agents für Hybridverbindungen für Web-Apps.|
|/sites/hybridconnectionnamespaces/relays/listkeys/action|Dient zum Auflisten der Schlüssel von Relaynamespace-Agents für Hybridverbindungen für Web-Apps.|
|/sites/hybridconnectionnamespaces/relays/read|Dient zum Abrufen von Relaynamespace-Agents für Hybridverbindungen für Web-Apps.|
|/sites/hybridconnectionnamespaces/relays/write|Dient zum Aktualisieren des Relaynamespace-Agents für Hybridverbindungen für Web-Apps.|
|/sites/hybridconnectionrelays/read|Dient zum Abrufen von Hybridverbindungsrelays für Web-Apps.|
|/sites/instances/deployments/delete|Dient zum Löschen von Web-Apps-Instanzbereitstellungen.|
|/sites/instances/deployments/read|Dient zum Abrufen von Prozessen für Web-App-Bereitstellungen.|
|/sites/instances/extensions/log/read|Dient zum Abrufen des Web-App-Instanzerweiterungsprotokolls.|
|/sites/instances/extensions/read|Dient zum Abrufen der Erweiterungen von Web-App-Instanzen.|
|/sites/instances/processes/delete|Dient zum Löschen von Prozessen für Web-App-Instanzen.|
|/sites/instances/processes/read|Dient zum Abrufen von Prozessen für Web-App-Instanzen.|
|/sites/instances/read|Dient zum Abrufen von Web-App-Instanzen.|
|/sites/listsyncfunctiontriggerstatus/action|Web-Apps zum Auflisten des Triggerstatus der Synchronisierungsfunktion|
|/sites/metricdefinitions/read|Dient zum Abrufen von Metrikdefinitionen für Web-Apps.|
|/sites/metrics/read|Dient zum Abrufen von Web-App-Metriken.|
|/sites/metricsdefinitions/read|Dient zum Abrufen von Metrikdefinitionen für Web-Apps.|
|/sites/migratemysql/action|Dient zum Durchführen von Migrationen mithilfe von MySql und Web-Apps.|
|/sites/migratemysql/read|Dient zum Abrufen von Web-Apps für die Migration mit MySql.|
|/sites/networktrace/action|Netzwerkablaufverfolgungs-Web-Apps|
|/sites/newpassword/action|NewPassword-Web-Apps|
|/sites/operationresults/read|Dient zum Abrufen der Ergebnisse von Web-App-Vorgängen.|
|/sites/operations/read|Dient zum Abrufen von Web-Apps-Vorgängen.|
|/sites/perfcounters/read|Dient zum Abrufen von Leistungsindikatoren für Web-Apps.|
|/sites/premieraddons/delete|Dient zum Löschen von Premier-Add-Ons für Web-Apps.|
|/sites/premieraddons/read|Dient zum Abrufen von Premier-Add-Ons für Web-Apps.|
|/sites/premieraddons/write|Dient zum Aktualisieren von Premier-Add-Ons für Web-Apps.|
|/sites/processes/read|Dient zum Abrufen von Web-Apps-Prozessen.|
|/sites/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/sites/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/sites/providers/Microsoft.Insights/metricDefinitions/Read|Ruft die verfügbaren Metriken für die Web-App ab.|
|/sites/publiccertificates/delete|Löscht die öffentlichen Zertifikate für Web-Apps.|
|/sites/publiccertificates/read|Ruft die öffentlichen Zertifikate für Web-Apps ab.|
|/sites/publiccertificates/write|Aktualisiert die öffentlichen Zertifikate für Web-Apps.|
|/sites/publish/Action|Dient zum Veröffentlichen einer Web-App.|
|/sites/publishxml/Action|Dient zum Abrufen des Veröffentlichungsprofils (XML) für eine Web-App.|
|/sites/publishxml/read|Dient zum Abrufen des XML-Codes für die Veröffentlichung von Web-Apps.|
|/sites/Read|Dient zum Abrufen der Eigenschaften einer Web-App.|
|/sites/recommendationhistory/read|Dient zum Abrufen des Empfehlungsverlaufs für Web-Apps.|
|/sites/recommendations/disable/action|Dient zum Deaktivieren von Web-App-Empfehlungen.|
|/sites/recommendations/Read|Dient zum Abrufen der Liste mit Empfehlungen für Web-Apps.|
|/sites/recover/action|Stellt Web-Apps wieder her.|
|/sites/resetSlotConfig/Action|Dient zum Zurücksetzen der Web-App-Konfiguration.|
|/sites/resourcehealthmetadata/read|Ruft Metadaten zur Ressourcenintegrität von Web-Apps ab.|
|/sites/restart/Action|Dient zum Neustarten einer Web-App.|
|/sites/restore/read|Dient zum Abrufen der Web-App-Wiederherstellung.|
|/sites/restore/write|Dient zum Wiederherstellen von Web-Apps.|
|/sites/siteextensions/delete|Dient zum Löschen von Web-Apps-Websiteerweiterungen.|
|/sites/siteextensions/read|Dient zum Abrufen von Web-Apps-Websiteerweiterungen.|
|/sites/siteextensions/write|Dient zum Aktualisieren von Web-Apps-Websiteerweiterungen.|
|/sites/slots/analyzecustomhostname/read|Dient zum Abrufen des benutzerdefinierten Hostnamens für die Analyse von Web-Apps-Slots.|
|/sites/slots/applySlotConfig/Action|Dient zum Anwenden der Web-App-Slotkonfiguration aus dem Zielslot auf den aktuellen Slot.|
|/sites/slots/backup/Action|Dient zum Erstellen einer neuen Web-App-Slotsicherung.|
|/sites/slots/backup/read|Dient zum Abrufen von Sicherungen von Web-Apps-Slots.|
|/sites/slots/backup/write|Dient zum Aktualisieren der Web-App-Slotsicherung.|
|/sites/slots/backups/delete|Dient zum Löschen von Sicherungen von Web-Apps-Slots.|
|/sites/slots/backups/list/action|Dient zum Auflisten von Web-App-Slotsicherungen.|
|/sites/slots/backups/Read|Dient zum Abrufen der Eigenschaften einer Sicherung von Web-App-Slots.|
|/sites/slots/backups/restore/action|Dient zum Wiederherstellen von Web-App-Slotsicherungen.|
|/sites/slots/config/delete|Dient zum Löschen der Konfiguration von Web-App-Slots.|
|/sites/slots/config/list/Action|Dient zum Auflisten der sicherheitsrelevanten Einstellungen des Web-App-Slots. Hierzu zählen etwa Veröffentlichungsanmeldeinformationen, App-Einstellungen und Verbindungszeichenfolgen.|
|/sites/slots/config/Read|Dient zum Abrufen der Konfigurationseinstellungen des Web-App-Slots.|
|/sites/slots/config/Write|Dient zum Aktualisieren der Konfigurationseinstellungen des Web-App-Slots.|
|/sites/slots/continuouswebjobs/delete|Dient zum Löschen fortlaufender Webaufträge für Web-App-Slots.|
|/sites/slots/continuouswebjobs/read|Dient zum Abrufen fortlaufender Webaufträge für Web-App-Slots.|
|/sites/slots/continuouswebjobs/start/action|Dient zum Starten fortlaufender Webaufträge für Web-App-Slots.|
|/sites/slots/continuouswebjobs/stop/action|Dient zum Beenden fortlaufender Webaufträge für Web-App-Slots.|
|/sites/slots/Delete|Dient zum Löschen eines vorhandenen Web-App-Slots.|
|/sites/slots/deployments/delete|Dient zum Löschen von Web-App-Slotbereitstellungen.|
|/sites/slots/deployments/log/read|Dient zum Abrufen des Protokolls für Web-App-Slotbereitstellungen.|
|/sites/slots/deployments/read|Dient zum Abrufen von Web-App-Slotbereitstellungen.|
|/sites/slots/deployments/write|Dient zum Aktualisieren von Web-App-Slotbereitstellungen.|
|/sites/slots/diagnostics/analyses/execute/Action|Dient zum Ausführen der Analyse von Web-Apps-Slotdiagnosen.|
|/sites/slots/diagnostics/analyses/read|Dient zum Abrufen der Analyse von Web-Apps-Slotdiagnosen.|
|/sites/slots/diagnostics/aspnetcore/read|Ruft Web-Apps-Slotdiagnosen für die ASP.NET Core-App ab.|
|/sites/slots/diagnostics/autoheal/read|Dient zum Abrufen von AutoHeal für Web-Apps-Slotdiagnosen.|
|/sites/slots/diagnostics/deployment/read|Dient zum Abrufen einer Bereitstellung für Web-Apps-Slotdiagnosen.|
|/sites/slots/diagnostics/deployments/read|Dient zum Abrufen von Bereitstellungen für Web-Apps-Slotdiagnosen.|
|/sites/slots/diagnostics/detectors/execute/Action|Dient zum Ausführen der Erkennung von Web-Apps-Slotdiagnosen.|
|/sites/slots/diagnostics/detectors/read|Dient zum Abrufen der Erkennung von Web-Apps-Slotdiagnosen.|
|/sites/slots/diagnostics/frebanalysis/read|Dient zum Abrufen der FREB-Analyse von Web-Apps-Slotdiagnosen.|
|/sites/slots/diagnostics/loganalyzer/read|Dient zum Abrufen der Protokollanalyse von Web-Apps-Slotdiagnosen.|
|/sites/slots/diagnostics/read|Dient zum Abrufen der Web-Apps-Slotdiagnose.|
|/sites/slots/diagnostics/runtimeavailability/read|Dient zum Abrufen der Laufzeitverfügbarkeit für Web-Apps-Diagnosen.|
|/sites/slots/diagnostics/servicehealth/read|Dient zum Abrufen der Dienstintegrität von Web-Apps-Slotdiagnosen.|
|/sites/slots/diagnostics/sitecpuanalysis/read|Dient zum Abrufen der CPU-Analyse von Websites von Web-Apps-Slotdiagnosen.|
|/sites/slots/diagnostics/sitecrashes/read|Dient zum Abrufen der Siteabstürze von Web-Apps-Slotdiagnosen.|
|/sites/slots/diagnostics/sitelatency/read|Dient zum Abrufen der Websitelatenzen von Web-Apps-Slotdiagnosen.|
|/sites/slots/diagnostics/sitememoryanalysis/read|Dient zum Abrufen der Websitespeicheranalyse von Web-Apps-Slotdiagnosen.|
|/sites/slots/diagnostics/siterestartsettingupdate/read|Dient zum Abrufen des Websiteupdates für die Einstellungen zum Neustarten der Web-Apps-Slotdiagnosen.|
|/sites/slots/diagnostics/siterestartuserinitiated/read|Dient zum Abrufen des vom Benutzer initiierten Websiteneustarts der Web-Apps-Slotdiagnosen.|
|/sites/slots/diagnostics/siteswap/read|Dient zum Abrufen des Websiteaustauschs von Web-Apps-Slotdiagnosen.|
|/sites/slots/diagnostics/threadcount/read|Dient zum Abrufen der Threadanzahl von Web-Apps-Slotdiagnosen.|
|/sites/slots/diagnostics/workeravailability/read|Dient zum Abrufen der Workerverfügbarkeit für Web-App-Slotdiagnosen.|
|/sites/slots/diagnostics/workerprocessrecycle/read|Dient zum Abrufen der Workerprozesswiederverwendung für Web-Apps-Slotdiagnosen.|
|/sites/slots/domainownershipidentifiers/read|Dient zum Abrufen von Domänenbesitz-IDs für Web-Apps-Slots.|
|/sites/slots/hostnamebindings/delete|Dient zum Löschen von Hostnamenbindungen für Web-App-Slots.|
|/sites/slots/hostnamebindings/read|Dient zum Abrufen von Hostnamenbindungen für Web-App-Slots.|
|/sites/slots/hostnamebindings/write|Dient zum Aktualisieren von Hostnamenbindungen für Web-App-Slots.|
|/sites/slots/hybridconnection/delete|Dient zum Löschen der Hybridverbindung für Web-App-Slots.|
|/sites/slots/hybridconnection/read|Dient zum Abrufen der Hybridverbindung für Web-App-Slots.|
|/sites/slots/hybridconnection/write|Dient zum Aktualisieren der Hybridverbindung für Web-App-Slots.|
|/sites/slots/hybridconnectionnamespaces/relays/delete|Dient zum Löschen der Relaynamespace-Agents für Hybridverbindungen für Web-Apps-Slots.|
|/sites/slots/hybridconnectionnamespaces/relays/write|Dient zum Aktualisieren der Relaynamespace-Agents für Hybridverbindungen für Web-Apps-Slots.|
|/sites/slots/hybridconnectionrelays/read|Dient zum Abrufen der Relay-Agents für Hybridverbindungen für Web-Apps-Slots.|
|/sites/slots/instances/deployments/read|Dient zum Abrufen von Bereitstellungen für Web-App-Slotinstanzen.|
|/sites/slots/instances/processes/delete|Dient zum Löschen von Prozessen für Web-Apps-Slotinstanzen.|
|/sites/slots/instances/processes/read|Dient zum Abrufen von Prozessen für Web-App-Slotinstanzen.|
|/sites/slots/instances/read|Dient zum Abrufen von Web-App-Slotinstanzen.|
|/sites/slots/metricdefinitions/read|Dient zum Abrufen von Metrikdefinitionen für Web-App-Slots.|
|/sites/slots/metrics/read|Dient zum Abrufen von Metriken für Web-App-Slots.|
|/sites/slots/migratemysql/read|Dient zum Abrufen von Web-Apps-Slots für die Migration mit MySql.|
|/sites/slots/networktrace/action|Web-Apps-Slots für die Netzwerkablaufverfolgung|
|/sites/slots/newpassword/action|NewPassword-Web-App-Slots|
|/sites/slots/operationresults/read|Dient zum Abrufen der Ergebnisse von Web-App-Slotvorgängen.|
|/sites/slots/operations/read|Dient zum Abrufen von Web-Apps-Slotvorgängen.|
|/sites/slots/perfcounters/read|Dient zum Abrufen von Leistungsindikatoren für Web-Apps-Slots.|
|/sites/slots/phplogging/read|Dient zum Abrufen der PHP-Protokollierung für Web-App-Slots.|
|/sites/slots/premieraddons/delete|Dient zum Löschen von Premier-Add-Ons für Web-App-Slots.|
|/sites/slots/premieraddons/read|Dient zum Abrufen von Premier-Add-Ons für Web-App-Slots.|
|/sites/slots/premieraddons/write|Dient zum Aktualisieren von Premier-Add-Ons für Web-App-Slots.|
|/sites/slots/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/sites/slots/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/sites/slots/providers/Microsoft.Insights/metricDefinitions/Read|Ruft die verfügbaren Metriken für Web-Apps-Slots ab.|
|/sites/slots/publiccertificates/read|Ruft die öffentlichen Zertifikate für Web-Apps-Slots ab.|
|/sites/slots/publiccertificates/write|Erstellt oder aktualisiert die öffentlichen Zertifikate für Web-Apps-Slots.|
|/sites/slots/publish/Action|Dient zum Veröffentlichen eines Web-App-Slots.|
|/sites/slots/publishxml/Action|Dient zum Abrufen des Veröffentlichungsprofils (XML) für einen Web-App-Slot.|
|/sites/slots/Read|Dient zum Abrufen der Eigenschaften eines Web-App-Bereitstellungsslots.|
|/sites/slots/resetSlotConfig/Action|Dient zum Zurücksetzen der Web-App-Slotkonfiguration.|
|/sites/slots/resourcehealthmetadata/read|Ruft Metadaten zur Ressourcenintegrität von Web-Apps-Slots ab.|
|/sites/slots/restart/Action|Dient zum Neustarten eines Web-App-Slots.|
|/sites/slots/restore/read|Dient zum Abrufen der Wiederherstellung für Web-App-Slots.|
|/sites/slots/restore/write|Dient zum Wiederherstellen von Web-Apps-Slots.|
|/sites/slots/siteextensions/delete|Dient zum Löschen von Websiteerweiterungen von Web-Apps-Slots.|
|/sites/slots/siteextensions/read|Dient zum Abrufen von Websiteerweiterungen von Web-Apps-Slots.|
|/sites/slots/siteextensions/write|Dient zum Aktualisieren von Websiteerweiterungen von Web-Apps-Slots.|
|/sites/slots/slotsdiffs/Action|Dient zum Abrufen von Konfigurationsunterschieden zwischen Web-App und Slots.|
|/sites/slots/slotsswap/Action|Dient zum Austauschen von Web-App-Bereitstellungsslots.|
|/sites/slots/snapshots/read|Dient zum Abrufen von Web-Apps-Slotmomentaufnahmen.|
|/sites/slots/sourcecontrols/Delete|Dient zum Löschen der Konfigurationseinstellungen für die Quellcodeverwaltung des Web-App-Slots.|
|/sites/slots/sourcecontrols/Read|Dient zum Abrufen der Konfigurationseinstellungen für die Quellcodeverwaltung des Web-App-Slots.|
|/sites/slots/sourcecontrols/Write|Dient zum Aktualisieren der Konfigurationseinstellungen für die Quellcodeverwaltung des Web-App-Slots.|
|/sites/slots/start/Action|Dient zum Starten eines Web-App-Slots.|
|/sites/slots/stop/Action|Dient zum Beenden eines Web-App-Slots.|
|/sites/slots/sync/action|Dient zum Synchronisieren von Web-App-Slots.|
|/sites/slots/triggeredwebjobs/delete|Dient zum Löschen ausgelöster Webaufträge für Web-App-Slots.|
|/sites/slots/triggeredwebjobs/read|Dient zum Abrufen ausgelöster Webaufträge für Web-App-Slots.|
|/sites/slots/triggeredwebjobs/run/action|Dient zum Ausführen ausgelöster Webaufträge für Web-App-Slots.|
|/sites/slots/usages/read|Dient zum Abrufen von Verwendungen für Web-App-Slots.|
|/sites/slots/virtualnetworkconnections/delete|Dient zum Löschen von Verbindungen mit virtuellen Netzwerken für Web-App-Slots.|
|/sites/slots/virtualnetworkconnections/gateways/write|Dient zum Aktualisieren von Gateways für Verbindungen mit virtuellen Netzwerken für Web-App-Slots.|
|/sites/slots/virtualnetworkconnections/read|Dient zum Abrufen von Verbindungen mit virtuellen Netzwerken für Web-App-Slots.|
|/sites/slots/virtualnetworkconnections/write|Dient zum Aktualisieren von Verbindungen mit virtuellen Netzwerken für Web-App-Slots.|
|/sites/slots/webjobs/read|Dient zum Abrufen von Webaufträgen für Web-App-Slots.|
|/sites/slots/Write|Dient zum Erstellen eines neuen Web-App-Slots oder zum Aktualisieren eines bereits vorhandenen.|
|/sites/slotsdiffs/Action|Dient zum Abrufen von Konfigurationsunterschieden zwischen Web-App und Slots.|
|/sites/slotsswap/Action|Dient zum Austauschen von Web-App-Bereitstellungsslots.|
|/sites/snapshots/read|Dient zum Abrufen von Web-App-Momentaufnahmen.|
|/sites/sourcecontrols/Delete|Dient zum Löschen der Konfigurationseinstellungen für die Quellcodeverwaltung der Web-App.|
|/sites/sourcecontrols/Read|Dient zum Abrufen der Konfigurationseinstellungen für die Quellcodeverwaltung der Web-App.|
|/sites/sourcecontrols/Write|Dient zum Aktualisieren der Konfigurationseinstellungen für die Quellcodeverwaltung der Web-App.|
|/sites/start/Action|Dient zum Starten einer Web-App.|
|/sites/stop/Action|Dient zum Beenden einer Web-App.|
|/sites/sync/action|Synchronisierungs-Web-Apps|
|/sites/syncfunctiontriggers/action|Synchronisierungsfunktionstrigger für Web-Apps.|
|/sites/triggeredwebjobs/delete|Dient zum Löschen ausgelöster Webaufträge für Web-Apps.|
|/sites/triggeredwebjobs/history/read|Dient zum Abrufen des ausgelösten WebJobs-Verlaufs für Web-Apps.|
|/sites/triggeredwebjobs/read|Dient zum Abrufen ausgelöster Webaufträge für Web-Apps.|
|/sites/triggeredwebjobs/run/action|Dient zum Ausführen ausgelöster Webaufträge für Web-Apps.|
|/sites/usages/read|Dient zum Abrufen von Web-App-Verwendungen.|
|/sites/virtualnetworkconnections/delete|Dient zum Löschen von Verbindungen mit virtuellen Netzwerken für Web-Apps.|
|/sites/virtualnetworkconnections/gateways/read|Dient zum Abrufen von Gateways für Verbindungen mit virtuellen Netzwerken für Web-Apps.|
|/sites/virtualnetworkconnections/gateways/write|Dient zum Aktualisieren von Gateways für Verbindungen mit virtuellen Netzwerken für Web-Apps.|
|/sites/virtualnetworkconnections/read|Dient zum Abrufen von Verbindungen mit virtuellen Netzwerken für Web-Apps.|
|/sites/virtualnetworkconnections/write|Dient zum Aktualisieren von Verbindungen mit virtuellen Netzwerken für Web-Apps.|
|/sites/webjobs/read|Dient zum Abrufen von Webaufträgen für Web-Apps.|
|/sites/Write|Dient zum Erstellen einer neuen Web-App oder zum Aktualisieren einer bereits vorhandenen.|
|/skus/read|Dient zum Abrufen von SKUs.|
|/sourcecontrols/read|Dient zum Abrufen von Quellcodeverwaltungen.|
|/sourcecontrols/write|Dient zum Aktualisieren von Quellcodeverwaltungen.|
|/unregister/action|Dient zum Aufheben der Registrierung des Microsoft.Web-Ressourcenanbieters für das Abonnement.|
|/validate/action|Dient zur Überprüfung.|
|/verifyhostingenvironmentvnet/action|Überprüft das VNET der Hostingumgebung.|

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

| Vorgang | BESCHREIBUNG |
|---|---|
|/components/read|Liest Betriebsressourcen.|
|/healthInstances/read|Liest Betriebsressourcen.|
|/Operations/read|Liest Betriebsressourcen.|
|/workloads/delete|Löscht eine Workloadressource.|
|/workloads/read|Liest eine Workloadressource.|
|/workloads/write|Schreibt eine Workloadressource.|

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über das [Erstellen einer benutzerdefinierten Rolle](role-based-access-control-custom-roles.md).
- Machen Sie sich mit den [integrierten RBAC-Rollen](role-based-access-built-in-roles.md) vertraut.
- Informieren Sie sich über das Verwalten von Zugriffszuweisungen [nach Benutzer](role-based-access-control-manage-assignments.md) oder [nach Ressource](role-based-access-control-configure.md). 
- Erfahren Sie mehr über das [Anzeigen von Aktivitätsprotokollen, um Aktionen mit Ressourcen zu überwachen](~/articles/azure-resource-manager/resource-group-audit.md)
