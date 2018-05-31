---
title: Vorgänge für Azure Resource Manager-Ressourcenanbieter | Microsoft-Dokumentation
description: Hier werden die verfügbaren Vorgängen für Microsoft Azure Resource Manager-Ressourcenanbieter aufgeführt.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/11/2018
ms.author: rolyon
ms.openlocfilehash: 08059da95baa9e70b8dba2dd847d0b28669778b7
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
ms.locfileid: "34161208"
---
# <a name="azure-resource-manager-resource-provider-operations"></a>Vorgänge für Azure Resource Manager-Ressourcenanbieter

In diesem Artikel werden die Vorgänge aufgeführt, die jedem Azure Resource Manager-Ressourcenanbieter zur Verfügung stehen. Die Vorgänge können in [benutzerdefinierten Rollen](custom-roles.md) verwendet werden, um eine präzise [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)](overview.md) für Ressourcen in Azure bereitzustellen. Die Vorgangszeichenfolgen weisen folgendes Format auf: `Microsoft.{ProviderName}/{ChildResourceType}/{action}`

Die Vorgänge für Ressourcenanbieter werden ständig weiterentwickelt. Verwenden Sie [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) oder [az provider operation list](/cli/azure/provider/operation#az-provider-operation-list), um die neuesten Vorgänge abzurufen.

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.AAD/domainServices/delete | Löscht Domänendienste. |
> | Aktion | Microsoft.AAD/domainServices/read | Liest Domänendienste. |
> | Aktion | Microsoft.AAD/domainServices/write | Schreibt Domänendienste. |
> | Aktion | Microsoft.AAD/locations/operationresults/read | Liest den Status eines asynchronen Vorgangs. |
> | Aktion | Microsoft.AAD/Operations/read | Die lokalisierte Beschreibung für den Vorgang in der für den Benutzer bestimmten Darstellung. |

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | microsoft.aadiam/diagnosticsettings/delete | Löscht eine Diagnoseeinstellung. |
> | Aktion | microsoft.aadiam/diagnosticsettings/read | Liest eine Diagnoseeinstellung. |
> | Aktion | microsoft.aadiam/diagnosticsettings/write | Schreibt eine Diagnoseeinstellung. |
> | Aktion | microsoft.aadiam/diagnosticsettingscategories/read | Liest die Kategorien einer Diagnoseeinstellung. |
> | Aktion | microsoft.aadiam/tenants/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | microsoft.aadiam/tenants/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | microsoft.aadiam/tenants/providers/Microsoft.Insights/logDefinitions/read | Ruft die verfügbaren Protokolle für Mandanten ab. |

## <a name="microsoftaddons"></a>Microsoft.Addons

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Addons/operations/read | Ruft unterstützte Vorgänge für Ressourcenanbieter ab. |
> | Aktion | Microsoft.Addons/register/action | Registriert das angegebene Abonnement in Microsoft.Addons. |
> | Aktion | Microsoft.Addons/supportProviders/listsupportplaninfo/action | Listet die aktuellen Informationen für den Supportplan für das angegebene Abonnement auf. |
> | Aktion | Microsoft.Addons/supportProviders/supportPlanTypes/delete | Entfernt den angegebenen kanonischen Supportplan. |
> | Aktion | Microsoft.Addons/supportProviders/supportPlanTypes/read | Ruft den Status des angegebenen kanonischen Supportplans ab. |
> | Aktion | Microsoft.Addons/supportProviders/supportPlanTypes/write | Fügt den angegebenen Typ des kanonischen Supportplans hinzu. |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.ADHybridHealthService/addsservices/action | Erstellt eine neue Gesamtstruktur für den Mandanten. |
> | Aktion | Microsoft.ADHybridHealthService/addsservices/addomainservicemembers/read | Ruft alle Server für den angegebenen Dienstnamen ab. |
> | Aktion | Microsoft.ADHybridHealthService/addsservices/alerts/read | Ruft Warnungsdetails für die Gesamtstruktur ab, z.B. die Warnungs-ID, das Datum der Warnung, die letzte erkannte Warnung, die Beschreibung der Warnung, den Zeitpunkt des letzten Updates, den Warnungsstatus und Links zur Problembehandlung der Warnung. |
> | Aktion | Microsoft.ADHybridHealthService/addsservices/configuration/read | Ruft die Dienstkonfiguration für die Gesamtstruktur ab. Beispiel: Name der Gesamtstruktur, Funktionsebene, FSMO-Rolle des Domänennamenmasters, FSMO-Rolle des Schemamasters usw. |
> | Aktion | Microsoft.ADHybridHealthService/addsservices/delete | Löscht einen Dienst und dessen Server zusammen mit den Integritätsdaten. |
> | Aktion | Microsoft.ADHybridHealthService/addsservices/dimensions/read | Ruft die Informationen zu Domänen und Standorten für die Gesamtstruktur ab. Beispiel: Integritätsstatus, aktive Warnungen, behobene Warnungen, Eigenschaften wie die Funktionsebene der Domäne, die Gesamtstruktur, der Infrastrukturmaster, PDC, RID-Master usw.  |
> | Aktion | Microsoft.ADHybridHealthService/addsservices/features/userpreference/read | Ruft die benutzerdefinierte Einstellung für die Gesamtstruktur ab.<br>Beispiel: Namen von metrischen Zählern wie ldapsuccessfulbinds, ntlmauthentications, kerberosauthentications, addsinsightsagentprivatebytes und ldapsearches.<br>Einstellungen für die Diagramme der Benutzeroberfläche (unter anderem). |
> | Aktion | Microsoft.ADHybridHealthService/addsservices/forestsummary/read | Ruft die Zusammenfassung für die angegebene Gesamtstruktur ab, z.B. den Namen der Gesamtstruktur, die Anzahl der Domänen in dieser Gesamtstruktur, die Anzahl der Standorte, Informationen zu Standorten usw. |
> | Aktion | Microsoft.ADHybridHealthService/addsservices/metricmetadata/read | Ruft die Liste der unterstützten Metriken für einen bestimmten Dienst ab.<br>Für den AD-Verbunddienst können dies z.B. Sperrungen von Extranet-Konten, die Gesamtanzahl der fehlgeschlagenen Anforderungen, ausstehende Tokenanforderungen (Proxy) oder Tokenanforderungen/Sekunde sein.<br>Für den AD-Domänendienst können dies z.B. NTLM-Authentifizierungen/Sekunde, erfolgreiche LDAP-Bindungen/Sekunde, die Bindungszeit für LDAPs, aktive LDAP-Threads, Kerberos-Authentifizierungen/Sekunde oder die Gesamtanzahl der ATQ-Threads sein.<br>Für den ADSync-Dienst können dies z.B. die Latenz des Ausführungsprofil, hergestellte TCP-Verbindungen, private Bytes des Insights-Agents oder Statistiken über Exporte in Azure AD sein. |
> | Aktion | Microsoft.ADHybridHealthService/addsservices/metrics/groups/read | Diese API ruft die Metrikinformationen für einen bestimmten Dienst ab.<br>Für den AD-Verbunddienst kann die API beispielsweise verwendet werden, um Informationen zu Folgendem abzurufen: Sperrungen von Extranet-Konten, die Gesamtanzahl der fehlgeschlagenen Anforderungen, ausstehende Tokenanforderungen (Proxy), Tokenanforderungen/Sekunde.<br>Für den AD-Domänendienst können dies z.B. NTLM-Authentifizierungen/Sekunde, erfolgreiche LDAP-Bindungen/Sekunde, die Bindungszeit für LDAPs, aktive LDAP-Threads, Kerberos-Authentifizierungen/Sekunde oder die Gesamtanzahl der ATQ-Threads sein.<br>Für den Sync-Dienst können dies z.B. die Latenz des Ausführungsprofil, hergestellte TCP-Verbindungen, private Bytes des Insights-Agents oder Statistiken über Exporte in Azure AD sein. |
> | Aktion | Microsoft.ADHybridHealthService/addsservices/premiumcheck/read | Diese API ruft die Liste aller eingebundenen AD-Domänendienste für einen Premium-Mandanten ab. |
> | Aktion | Microsoft.ADHybridHealthService/addsservices/read | Ruft Dienstdetails für den angegebenen Dienstnamen ab. |
> | Aktion | Microsoft.ADHybridHealthService/addsservices/replicationdetails/read | Ruft Replikationsdetails für alle Server für den angegebenen Dienstnamen ab. |
> | Aktion | Microsoft.ADHybridHealthService/addsservices/replicationstatus/read | Ruft die Anzahl der Domänencontroller und deren Replikationsfehler (falls vorhanden) ab. |
> | Aktion | Microsoft.ADHybridHealthService/addsservices/replicationsummary/read | Ruft die vollständige Liste der Domänencontroller und die Replikationsdetails für die angegebene Gesamtstruktur ab. |
> | Aktion | Microsoft.ADHybridHealthService/addsservices/servicemembers/action | Fügt eine Serverinstanz zum Dienst hinzu. |
> | Aktion | Microsoft.ADHybridHealthService/addsservices/servicemembers/credentials/read | Während der Serverregistrierung des AD-Domänendiensts wird diese API aufgerufen, um die Anmeldeinformationen für das Onboarding neuer Server abzurufen. |
> | Aktion | Microsoft.ADHybridHealthService/addsservices/servicemembers/delete | Löscht einen Server für einen bestimmten Dienst und Mandanten. |
> | Aktion | Microsoft.ADHybridHealthService/addsservices/write | Erstellt oder aktualisiert die Instanz des AD-Domänendiensts für den Mandanten. |
> | Aktion | Microsoft.ADHybridHealthService/configuration/action | Aktualisiert die Mandantenkonfiguration. |
> | Aktion | Microsoft.ADHybridHealthService/configuration/read | Liest die Mandantenkonfiguration. |
> | Aktion | Microsoft.ADHybridHealthService/configuration/write | Erstellt eine Mandantenkonfiguration. |
> | Aktion | Microsoft.ADHybridHealthService/logs/contents/read | Ruft den Inhalt der Agent-Installation und die im Blob gespeicherten Registrierungsprotokolle ab. |
> | Aktion | Microsoft.ADHybridHealthService/logs/read | Ruft Protokolle für die Agent-Installation und die Registrierung für den Mandanten ab. |
> | Aktion | Microsoft.ADHybridHealthService/operations/read | Ruft die Liste der vom System unterstützten Vorgänge ab. |
> | Aktion | Microsoft.ADHybridHealthService/register/action | Registriert den Ressourcenanbieter für AD Hybrid Health Service und aktiviert die Erstellung von AD Hybrid Health Service-Ressourcen. |
> | Aktion | Microsoft.ADHybridHealthService/reports/availabledeployments/read | Ruft die Liste der verfügbaren Regionen ab, die von DevOps verwendet werden, um Incidents von Benutzern zu unterstützen. |
> | Aktion | Microsoft.ADHybridHealthService/reports/badpassword/read | Ruft die Liste ungültiger Kennworteingabeversuche für alle Benutzer des Active Directory-Verbunddiensts ab. |
> | Aktion | Microsoft.ADHybridHealthService/reports/badpassworduseridipfrequency/read | Ruft den SAS-URI von Blobs ab, der den Zustand und das mögliche Ergebnis für neu in die Warteschlange eingereihte Berichtsaufträge für die Häufigkeit von ungültigen Eingabeversuchen für Benutzername oder Kennwort pro Benutzer-ID, IP-Adresse und Tag für einen bestimmten Mandanten enthält. |
> | Aktion | Microsoft.ADHybridHealthService/reports/consentedtodevopstenants/read | Ruft die Liste der für DevOps genehmigten Mandanten ab. Diese wird üblicherweise für den Kundendienst verwendet. |
> | Aktion | Microsoft.ADHybridHealthService/reports/isdevops/read | Ruft einen Wert ab, der angibt, ob der Mandant für DevOps genehmigt ist. |
> | Aktion | Microsoft.ADHybridHealthService/reports/selectdevopstenant/read | Aktualisiert die Benutzer-ID (Objekt-ID) für den ausgewählten DevOps-Mandanten. |
> | Aktion | Microsoft.ADHybridHealthService/reports/selecteddeployment/read | Ruft die ausgewählte Bereitstellung für den angegebenen Mandanten ab. |
> | Aktion | Microsoft.ADHybridHealthService/reports/tenantassigneddeployment/read | Ruft den Speicherort eines Mandanten anhand der Mandanten-ID ab. |
> | Aktion | Microsoft.ADHybridHealthService/reports/updateselecteddeployment/read | Ruft den geografischen Standort ab, von dem aus auf die Daten zugegriffen wird. |
> | Aktion | Microsoft.ADHybridHealthService/services/action | Aktualisiert eine Dienstinstanz im Mandanten. |
> | Aktion | Microsoft.ADHybridHealthService/services/alerts/read | Liest die Warnungen für einen Dienst. |
> | Aktion | Microsoft.ADHybridHealthService/services/alerts/read | Liest die Warnungen für einen Dienst. |
> | Aktion | Microsoft.ADHybridHealthService/services/checkservicefeatureavailibility/read | Überprüft anhand eines Featurenamens, ob ein Dienst die Voraussetzungen für die Verwendung dieses Features erfüllt. |
> | Aktion | Microsoft.ADHybridHealthService/services/delete | Löscht eine Dienstinstanz im Mandanten. |
> | Aktion | Microsoft.ADHybridHealthService/services/exporterrors/read | Ruft die Exportfehler für einen bestimmten Synchronisierungsdienst ab. |
> | Aktion | Microsoft.ADHybridHealthService/services/exportstatus/read | Ruft den Exportstatus für einen bestimmten Dienst ab. |
> | Aktion | Microsoft.ADHybridHealthService/services/feedbacktype/feedback/read | Ruft Feedback zu Warnungen für einen bestimmten Dienst und Server ab. |
> | Aktion | Microsoft.ADHybridHealthService/services/metricmetadata/read | Ruft die Liste der unterstützten Metriken für einen bestimmten Dienst ab.<br>Für den AD-Verbunddienst können dies z.B. Sperrungen von Extranet-Konten, die Gesamtanzahl der fehlgeschlagenen Anforderungen, ausstehende Tokenanforderungen (Proxy) oder Tokenanforderungen/Sekunde sein.<br>Für den AD-Domänendienst können dies z.B. NTLM-Authentifizierungen/Sekunde, erfolgreiche LDAP-Bindungen/Sekunde, die Bindungszeit für LDAPs, aktive LDAP-Threads, Kerberos-Authentifizierungen/Sekunde oder die Gesamtanzahl der ATQ-Threads sein.<br>Für den ADSync-Dienst können dies z.B. die Latenz des Ausführungsprofil, hergestellte TCP-Verbindungen, private Bytes des Insights-Agents oder Statistiken über Exporte in Azure AD sein. |
> | Aktion | Microsoft.ADHybridHealthService/services/metrics/groups/average/read | Diese API ruft anhand eines Diensts den Durchschnitt der Metriken für einen bestimmten Dienst ab.<br>Für den AD-Verbunddienst kann die API beispielsweise verwendet werden, um Informationen zu Folgendem abzurufen: Sperrungen von Extranet-Konten, die Gesamtanzahl der fehlgeschlagenen Anforderungen, ausstehende Tokenanforderungen (Proxy), Tokenanforderungen/Sekunde.<br>Für den AD-Domänendienst können dies z.B. NTLM-Authentifizierungen/Sekunde, erfolgreiche LDAP-Bindungen/Sekunde, die Bindungszeit für LDAPs, aktive LDAP-Threads, Kerberos-Authentifizierungen/Sekunde oder die Gesamtanzahl der ATQ-Threads sein.<br>Für den Sync-Dienst können dies z.B. die Latenz des Ausführungsprofil, hergestellte TCP-Verbindungen, private Bytes des Insights-Agents oder Statistiken über Exporte in Azure AD sein. |
> | Aktion | Microsoft.ADHybridHealthService/services/metrics/groups/read | Diese API ruft die Metrikinformationen für einen bestimmten Dienst ab.<br>Für den AD-Verbunddienst kann die API beispielsweise verwendet werden, um Informationen zu Folgendem abzurufen: Sperrungen von Extranet-Konten, die Gesamtanzahl der fehlgeschlagenen Anforderungen, ausstehende Tokenanforderungen (Proxy), Tokenanforderungen/Sekunde.<br>Für den AD-Domänendienst können dies z.B. NTLM-Authentifizierungen/Sekunde, erfolgreiche LDAP-Bindungen/Sekunde, die Bindungszeit für LDAPs, aktive LDAP-Threads, Kerberos-Authentifizierungen/Sekunde oder die Gesamtanzahl der ATQ-Threads sein.<br>Für den Sync-Dienst können dies z.B. die Latenz des Ausführungsprofil, hergestellte TCP-Verbindungen, private Bytes des Insights-Agents oder Statistiken über Exporte in Azure AD sein. |
> | Aktion | Microsoft.ADHybridHealthService/services/metrics/groups/sum/read | Diese API ruft anhand eines Diensts die aggregierte Ansicht der Metriken für einen bestimmten Dienst ab.<br>Für den AD-Verbunddienst kann die API beispielsweise verwendet werden, um Informationen zu Folgendem abzurufen: Sperrungen von Extranet-Konten, die Gesamtanzahl der fehlgeschlagenen Anforderungen, ausstehende Tokenanforderungen (Proxy), Tokenanforderungen/Sekunde.<br>Für den AD-Domänendienst können dies z.B. NTLM-Authentifizierungen/Sekunde, erfolgreiche LDAP-Bindungen/Sekunde, die Bindungszeit für LDAPs, aktive LDAP-Threads, Kerberos-Authentifizierungen/Sekunde oder die Gesamtanzahl der ATQ-Threads sein.<br>Für den Sync-Dienst können dies z.B. die Latenz des Ausführungsprofil, hergestellte TCP-Verbindungen, private Bytes des Insights-Agents oder Statistiken über Exporte in Azure AD sein. |
> | Aktion | Microsoft.ADHybridHealthService/services/monitoringconfiguration/write | Fügt die Überwachungskonfiguration für einen Dienst hinzu oder aktualisiert diese. |
> | Aktion | Microsoft.ADHybridHealthService/services/monitoringconfigurations/read | Ruft Überwachungskonfigurationen für einen bestimmten Dienst ab. |
> | Aktion | Microsoft.ADHybridHealthService/services/monitoringconfigurations/write | Fügt Überwachungskonfigurationen zu einem Dienst hinzu. |
> | Aktion | Microsoft.ADHybridHealthService/services/premiumcheck/read | Diese API ruft die Liste aller eingebundenen Dienste für einen Premium-Mandanten ab. |
> | Aktion | Microsoft.ADHybridHealthService/services/read | Liest die Dienstinstanz im Mandanten. |
> | Aktion | Microsoft.ADHybridHealthService/services/reports/details/read | Ruft einen Bericht mit den obersten 50 Benutzern mit dem Fehler „Falsches Kennwort“ der letzten sieben Tage ab. |
> | Aktion | Microsoft.ADHybridHealthService/services/servicemembers/action | Erstellt eine Dienstinstanz im Dienst. |
> | Aktion | Microsoft.ADHybridHealthService/services/servicemembers/alerts/read | Liest die Warnungen für einen Server. |
> | Aktion | Microsoft.ADHybridHealthService/services/servicemembers/credentials/read | Während der Serverregistrierung wird diese API aufgerufen, um die Anmeldeinformationen für das Onboarding neuer Server abzurufen. |
> | Aktion | Microsoft.ADHybridHealthService/services/servicemembers/datafreshness/read | Diese API ruft für einen bestimmten Server die Liste der Datentypen, die von den Servern hochgeladen werden, sowie die letzte Uhrzeit für jeden Upload ab. |
> | Aktion | Microsoft.ADHybridHealthService/services/servicemembers/delete | Löscht eine Dienstinstanz aus dem Dienst. |
> | Aktion | Microsoft.ADHybridHealthService/services/servicemembers/exportstatus/read | Ruft Informationen zu Exportfehlern bei der Synchronisierung für einen bestimmten Synchronisierungsdienst ab. |
> | Aktion | Microsoft.ADHybridHealthService/services/servicemembers/metrics/groups/read | Diese API ruft die Metrikinformationen für einen bestimmten Dienst ab.<br>Für den AD-Verbunddienst kann die API beispielsweise verwendet werden, um Informationen zu Folgendem abzurufen: Sperrungen von Extranet-Konten, die Gesamtanzahl der fehlgeschlagenen Anforderungen, ausstehende Tokenanforderungen (Proxy), Tokenanforderungen/Sekunde.<br>Für den AD-Domänendienst können dies z.B. NTLM-Authentifizierungen/Sekunde, erfolgreiche LDAP-Bindungen/Sekunde, die Bindungszeit für LDAPs, aktive LDAP-Threads, Kerberos-Authentifizierungen/Sekunde oder die Gesamtanzahl der ATQ-Threads sein.<br>Für den Sync-Dienst können dies z.B. die Latenz des Ausführungsprofil, hergestellte TCP-Verbindungen, private Bytes des Insights-Agents oder Statistiken über Exporte in Azure AD sein. |
> | Aktion | Microsoft.ADHybridHealthService/services/servicemembers/read | Liest die Dienstinstanz im Dienst. |
> | Aktion | Microsoft.ADHybridHealthService/services/servicemembers/serviceconfiguration/read | Ruft die Dienstkonfiguration für einen bestimmten Mandanten ab. |
> | Aktion | Microsoft.ADHybridHealthService/services/tenantwhitelisting/read | Ruft den Status eines Features für die Aufnahme in die Whitelist für einen bestimmten Mandanten ab. |
> | Aktion | Microsoft.ADHybridHealthService/services/write | Erstellt eine Dienstinstanz im Mandanten. |
> | Aktion | Microsoft.ADHybridHealthService/unregister/action | Hebt die Registrierung des Abonnements für den AD Hybrid Health Service-Ressourcenanbieter ab. |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Advisor/configurations/read | Konfigurationen abrufen |
> | Aktion | Microsoft.Advisor/configurations/write | Erstellt oder aktualisiert Konfigurationen. |
> | Aktion | Microsoft.Advisor/generateRecommendations/action | Generiert Empfehlungen. |
> | Aktion | Microsoft.Advisor/generateRecommendations/read | Ruft den Status der Empfehlungsgenerierung ab. |
> | Aktion | Microsoft.Advisor/operations/read | Ruft die Vorgänge für den Microsoft Advisor ab. |
> | Aktion | Microsoft.Advisor/recommendations/read | Liest Empfehlungen. |
> | Aktion | Microsoft.Advisor/recommendations/suppressions/delete | Löscht Unterdrückungen. |
> | Aktion | Microsoft.Advisor/recommendations/suppressions/read | Ruft Unterdrückungen ab. |
> | Aktion | Microsoft.Advisor/recommendations/suppressions/write | Erstellt/Aktualisiert Unterdrückungen. |
> | Aktion | Microsoft.Advisor/register/action | Registriert das Abonnement für den Microsoft Advisor. |
> | Aktion | Microsoft.Advisor/suppressions/delete | Löscht Unterdrückungen. |
> | Aktion | Microsoft.Advisor/suppressions/read | Ruft Unterdrückungen ab. |
> | Aktion | Microsoft.Advisor/suppressions/write | Erstellt/Aktualisiert Unterdrückungen. |
> | Aktion | Microsoft.Advisor/unregister/action | Hebt die Registrierung für das Abonnement für den Microsoft Advisor auf. |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.AlertsManagement/alerts/changestate/action | Ändert den Status der Warnung. |
> | Aktion | Microsoft.AlertsManagement/alerts/read | Ruft alle Benachrichtigungen für die Eingabefilter ab. |
> | Aktion | Microsoft.AlertsManagement/alertsSummary/read | Ruft die Zusammenfassung der Benachrichtigungen ab. |
> | Aktion | Microsoft.AlertsManagement/Operations/read | Liest die angegebenen Vorgänge. |
> | Aktion | Microsoft.AlertsManagement/smartGroups/changestate/action | Ändert den Status der intelligenten Gruppe. |
> | Aktion | Microsoft.AlertsManagement/smartGroups/read | Ruft alle intelligenten Gruppen für die Eingabefilter ab. |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.AnalysisServices/locations/checkNameAvailability/action | Prüft, ob der angegebene Analysis-Servername gültig ist oder bereits verwendet wird. |
> | Aktion | Microsoft.AnalysisServices/locations/operationresults/read | Ruft die Informationen des angegebenen Vorgangsergebnisses ab. |
> | Aktion | Microsoft.AnalysisServices/locations/operationstatuses/read | Ruft die Informationen des angegebenen Vorgangsstatus ab. |
> | Aktion | Microsoft.AnalysisServices/operations/read | Ruft die Informationen von Vorgängen ab. |
> | Aktion | Microsoft.AnalysisServices/register/action | Registriert den Analysis Services-Ressourcenanbieter. |
> | Aktion | Microsoft.AnalysisServices/servers/delete | Löscht den Analysis-Server. |
> | Aktion | Microsoft.AnalysisServices/servers/listGatewayStatus/action | Listet den Status des Gateways auf, der dem Server zugeordnet ist. |
> | Aktion | Microsoft.AnalysisServices/servers/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für den Analysis-Server ab. |
> | Aktion | Microsoft.AnalysisServices/servers/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für den Analysis-Server. |
> | Aktion | Microsoft.AnalysisServices/servers/providers/Microsoft.Insights/logDefinitions/read | Ruft die verfügbaren Protokolle für Server ab. |
> | Aktion | Microsoft.AnalysisServices/servers/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für den Analysis-Server ab. |
> | Aktion | Microsoft.AnalysisServices/servers/read | Ruft die Informationen des angegebenen Analysis-Servers ab. |
> | Aktion | Microsoft.AnalysisServices/servers/resume/action | Setzt den Analysis-Server fort. |
> | Aktion | Microsoft.AnalysisServices/servers/skus/read | Ruft die verfügbaren SKU-Informationen für den Server ab. |
> | Aktion | Microsoft.AnalysisServices/servers/suspend/action | Hält den Analysis-Server an. |
> | Aktion | Microsoft.AnalysisServices/servers/write | Erstellt oder aktualisiert den angegebenen Analysis-Server. |
> | Aktion | Microsoft.AnalysisServices/skus/read | Ruft die Informationen der SKUs ab. |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.ApiManagement/checkNameAvailability/read | Prüft, ob der angegebene Dienstname verfügbar ist. |
> | Aktion | Microsoft.ApiManagement/operations/read | Liest alle API-Vorgänge, die für die Microsoft.ApiManagement-Ressource verfügbar sind. |
> | Aktion | Microsoft.ApiManagement/register/action | Dient zum Registrieren des Microsoft.ApiManagement-Ressourcenanbieters. |
> | Aktion | Microsoft.ApiManagement/reports/read | Ruft Berichte ab, die nach Zeiträumen, geografischer Region, Entwicklern, Produkten, APIs, Vorgängen, Abonnement und byRequest aggregiert sind. |
> | Aktion | Microsoft.ApiManagement/service/apis/delete | Dient zum Entfernen vorhandener APIs. |
> | Aktion | Microsoft.ApiManagement/service/apis/diagnostics/delete | Entfernt eine vorhandene Diagnose. |
> | Aktion | Microsoft.ApiManagement/service/apis/diagnostics/loggers/delete | Entfernt die Zuordnung zwischen einer Protokollierung und einer Diagnoseeinstellung. |
> | Aktion | Microsoft.ApiManagement/service/apis/diagnostics/loggers/read | Ruft die Liste der vorhandenen Diagnoseprotokollierungen ab. |
> | Aktion | Microsoft.ApiManagement/service/apis/diagnostics/loggers/write | Ordnet eine Protokollierung einer Diagnoseeinstellung zu. |
> | Aktion | Microsoft.ApiManagement/service/apis/diagnostics/read | Ruft die Liste der Diagnosen oder Details einer Diagnose ab. |
> | Aktion | Microsoft.ApiManagement/service/apis/diagnostics/write | Fügt eine neue Diagnose hinzu oder aktualisiert die Details vorhandener Diagnosen. |
> | Aktion | Microsoft.ApiManagement/service/apis/issues/attachments/delete | Entfernt eine vorhandene Anlage. |
> | Aktion | Microsoft.ApiManagement/service/apis/issues/attachments/read | Ruft Anlagen für Probleme oder Informationen zu Anlagen für Problemen mit API Management ab. |
> | Aktion | Microsoft.ApiManagement/service/apis/issues/attachments/write | Fügt eine Anlage für API-Probleme hinzu. |
> | Aktion | Microsoft.ApiManagement/service/apis/issues/comments/delete | Entfernt einen vorhandenen Kommentar. |
> | Aktion | Microsoft.ApiManagement/service/apis/issues/comments/read | Ruft Kommentare zu Problemen oder Informationen zu Kommentaren zu Problemen mit API Management ab. |
> | Aktion | Microsoft.ApiManagement/service/apis/issues/comments/write | Fügt einen Kommentar zu einem API-Problem hinzu. |
> | Aktion | Microsoft.ApiManagement/service/apis/issues/delete | Entfernt ein vorhandenes Problem. |
> | Aktion | Microsoft.ApiManagement/service/apis/issues/read | Ruft Probleme mit der API oder Informationen zu Problemen mit API Management ab. |
> | Aktion | Microsoft.ApiManagement/service/apis/issues/write | Fügt API-Probleme hinzu oder aktualisiert diese. |
> | Aktion | Microsoft.ApiManagement/service/apis/operations/delete | Dient zum Entfernen vorhandener API-Vorgänge. |
> | Aktion | Microsoft.ApiManagement/service/apis/operations/policies/delete | Entfernt die Richtlinienkonfiguration von Richtlinien für API-Vorgänge. |
> | Aktion | Microsoft.ApiManagement/service/apis/operations/policies/read | Ruft Richtlinien für API-Vorgänge oder Richtlinienkonfigurationsdetails für API-Vorgänge ab. |
> | Aktion | Microsoft.ApiManagement/service/apis/operations/policies/write | Dient zum Festlegen von Richtlinienkonfigurationsdetails für API-Vorgänge. |
> | Aktion | Microsoft.ApiManagement/service/apis/operations/policy/delete | Dient zum Entfernen der Richtlinienkonfiguration von Vorgängen. |
> | Aktion | Microsoft.ApiManagement/service/apis/operations/policy/read | Dient zum Abrufen von Richtlinienkonfigurationsdetails für Vorgänge. |
> | Aktion | Microsoft.ApiManagement/service/apis/operations/policy/write | Dient zum Festlegen von Richtlinienkonfigurationsdetails für Vorgänge. |
> | Aktion | Microsoft.ApiManagement/service/apis/operations/read | Dient zum Abrufen einer Liste mit vorhandenen API-Vorgängen oder zum Abrufen von Details zu API-Vorgängen. |
> | Aktion | Microsoft.ApiManagement/service/apis/operations/tags/delete | Löscht die Zuordnung zwischen einer vorhandenen Markierung und einem vorhandenen Vorgang. |
> | Aktion | Microsoft.ApiManagement/service/apis/operations/tags/read | Ruft die dem Vorgang zugeordneten Markierungen oder Markierungsdetails ab. |
> | Aktion | Microsoft.ApiManagement/service/apis/operations/tags/write | Ordnet eine vorhandene Markierung einem vorhandenen Vorgang zu. |
> | Aktion | Microsoft.ApiManagement/service/apis/operations/write | Dient zum Erstellen eines neuen API-Vorgangs oder zum Aktualisieren vorhandener API-Vorgänge. |
> | Aktion | Microsoft.ApiManagement/service/apis/operationsByTags/read | Ruft die Liste der Zuordnungen zwischen Vorgängen und Markierungen ab. |
> | Aktion | Microsoft.ApiManagement/service/apis/policies/delete | Dient zum Entfernen der Richtlinienkonfiguration aus API-Richtlinien. |
> | Aktion | Microsoft.ApiManagement/service/apis/policies/read | Ruft Richtlinien für APIs oder Richtlinienkonfigurationsdetails für APIs ab. |
> | Aktion | Microsoft.ApiManagement/service/apis/policies/write | Dient zum Festlegen von Richtlinienkonfigurationsdetails für APIs. |
> | Aktion | Microsoft.ApiManagement/service/apis/policy/delete | Dient zum Entfernen der Richtlinienkonfiguration von APIs. |
> | Aktion | Microsoft.ApiManagement/service/apis/policy/read | Dient zum Abrufen von Richtlinienkonfigurationsdetails für APIs. |
> | Aktion | Microsoft.ApiManagement/service/apis/policy/write | Dient zum Festlegen von Richtlinienkonfigurationsdetails für APIs. |
> | Aktion | Microsoft.ApiManagement/service/apis/products/read | Ruft alle Produkte ab, denen die API zugehörig ist. |
> | Aktion | Microsoft.ApiManagement/service/apis/read | Dient zum Abrufen einer Liste mit allen registrierten APIs oder zum Abrufen von API-Details. |
> | Aktion | Microsoft.ApiManagement/service/apis/releases/delete | Entfernt ein Release oder alle Releases der API. |
> | Aktion | Microsoft.ApiManagement/service/apis/releases/read | Ruft Releases für eine API oder Details von einem API-Release ab. |
> | Aktion | Microsoft.ApiManagement/service/apis/releases/write | Dient zum Erstellen eines neuen API-Release oder zum Aktualisieren eines bereits vorhandenen API-Release. |
> | Aktion | Microsoft.ApiManagement/service/apis/revisions/delete | Entfernt alle Revisionen einer API. |
> | Aktion | Microsoft.ApiManagement/service/apis/revisions/read | Ruft Revisionen ab, die einer API zugehörig sind. |
> | Aktion | Microsoft.ApiManagement/service/apis/schemas/delete | Entfernt ein vorhandenes Schema. |
> | Aktion | Microsoft.ApiManagement/service/apis/schemas/document/read | Ruft das Dokument ab, das das Schema beschreibt. |
> | Aktion | Microsoft.ApiManagement/service/apis/schemas/document/write | Aktualisiert das Dokument, das das Schema beschreibt. |
> | Aktion | Microsoft.ApiManagement/service/apis/schemas/read | Ruft alle Schemas für eine bestimmte API oder die von der API verwendeten Schemas ab. |
> | Aktion | Microsoft.ApiManagement/service/apis/schemas/write | Legt die von der API verwendeten Schemas fest. |
> | Aktion | Microsoft.ApiManagement/service/apis/tagDescriptions/delete | Entfernt die Markierungsbeschreibung aus der API. |
> | Aktion | Microsoft.ApiManagement/service/apis/tagDescriptions/read | Ruft eine oder mehrere Markierungsbeschreibungen im Bereich der API ab. |
> | Aktion | Microsoft.ApiManagement/service/apis/tagDescriptions/write | Erstellt oder ändert eine Markierungsbeschreibung im Bereich der API. |
> | Aktion | Microsoft.ApiManagement/service/apis/tags/delete | Entfernt vorhandene Zuordnungen zwischen einer API und einer Markierung. |
> | Aktion | Microsoft.ApiManagement/service/apis/tags/read | Ruft alle Zuordnungen zwischen einer API und einer Markierung für die API oder die zugehörigen Details ab. |
> | Aktion | Microsoft.ApiManagement/service/apis/tags/write | Fügt eine neue Zuordnung zwischen einer API und einer Markierung hinzu. |
> | Aktion | Microsoft.ApiManagement/service/apis/write | Dient zum Erstellen einer neuen API oder zum Aktualisieren bereits vorhandener API-Details. |
> | Aktion | Microsoft.ApiManagement/service/apisByTags/read | Ruft die Liste der Zuordnungen zwischen APIs und Markierungen ab. |
> | Aktion | Microsoft.ApiManagement/service/api-version-sets/delete | Dient zum Entfernen vorhandener VersionSet-Eigenschaften. |
> | Aktion | Microsoft.ApiManagement/service/api-version-sets/read | Ruft die Liste der Versionsgruppenentitäten oder Details zu einer VersionSet-Eigenschaft ab. |
> | Aktion | Microsoft.ApiManagement/service/api-version-sets/versions/read | Ruft die Liste der Versionsentitäten ab. |
> | Aktion | Microsoft.ApiManagement/service/api-version-sets/write | Erstellt neue VersionSet-Details oder aktualisiert vorhandene VersionSet-Details. |
> | Aktion | Microsoft.ApiManagement/service/applynetworkconfigurationupdates/action | Aktualisiert die in Virtual Network ausgeführten Microsoft.ApiManagement-Ressourcen, um aktualisierte Netzwerkeinstellungen zu verwenden. |
> | Aktion | Microsoft.ApiManagement/service/authorizationServers/delete | Dient zum Entfernen vorhandener Autorisierungsserver. |
> | Aktion | Microsoft.ApiManagement/service/authorizationServers/read | Dient zum Abrufen einer Liste mit Autorisierungsservern oder zum Abrufen von Autorisierungsserverdetails. |
> | Aktion | Microsoft.ApiManagement/service/authorizationServers/write | Dient zum Erstellen eines neuen Autorisierungsservers oder zum Aktualisieren der Details eines bereits vorhandenen Autorisierungsservers. |
> | Aktion | Microsoft.ApiManagement/service/backends/delete | Dient zum Entfernen vorhandener Back-Ends. |
> | Aktion | Microsoft.ApiManagement/service/backends/read | Dient zum Abrufen einer Liste mit Back-Ends oder zum Abrufen von Back-End-Details. |
> | Aktion | Microsoft.ApiManagement/service/backends/reconnect/action | Erstellt eine Anforderung zum Herstellen einer erneuten Verbindung. |
> | Aktion | Microsoft.ApiManagement/service/backends/write | Dient zum Hinzufügen eines neuen Back-Ends oder zum Aktualisieren bereits vorhandener Back-End-Details. |
> | Aktion | Microsoft.ApiManagement/service/backup/action | Dient zum Sichern des API Management-Diensts im angegebenen Container in einem vom Benutzer bereitgestellten Speicherkonto. |
> | Aktion | Microsoft.ApiManagement/service/certificates/delete | Dient zum Entfernen vorhandener Zertifikate. |
> | Aktion | Microsoft.ApiManagement/service/certificates/read | Dient zum Abrufen einer Liste mit Zertifikaten oder zum Abrufen von Zertifikatdetails. |
> | Aktion | Microsoft.ApiManagement/service/certificates/write | Dient zum Hinzufügen eines neuen Zertifikats. |
> | Aktion | Microsoft.ApiManagement/service/delete | Dient zum Löschen einer API Management-Dienstinstanz. |
> | Aktion | Microsoft.ApiManagement/service/diagnostics/delete | Entfernt eine vorhandene Diagnose. |
> | Aktion | Microsoft.ApiManagement/service/diagnostics/loggers/delete | Entfernt die Zuordnung zwischen einer Protokollierung und einer Diagnoseeinstellung. |
> | Aktion | Microsoft.ApiManagement/service/diagnostics/loggers/read | Ruft die Liste der vorhandenen Diagnoseprotokollierungen ab. |
> | Aktion | Microsoft.ApiManagement/service/diagnostics/loggers/write | Ordnet eine Protokollierung einer Diagnoseeinstellung zu. |
> | Aktion | Microsoft.ApiManagement/service/diagnostics/read | Ruft die Liste der Diagnosen oder Details einer Diagnose ab. |
> | Aktion | Microsoft.ApiManagement/service/diagnostics/write | Fügt eine neue Diagnose hinzu oder aktualisiert die Details vorhandener Diagnosen. |
> | Aktion | Microsoft.ApiManagement/service/getssotoken/action | Ruft ein SSO-Token ab, das verwendet werden kann, um sich als Administrator beim Legacyportal des API Management-Diensts anzumelden. |
> | Aktion | Microsoft.ApiManagement/service/groups/delete | Dient zum Entfernen vorhandener Gruppen. |
> | Aktion | Microsoft.ApiManagement/service/groups/read | Dient zum Abrufen einer Liste mit Gruppen oder zum Abrufen von Details einer Gruppe. |
> | Aktion | Microsoft.ApiManagement/service/groups/users/delete | Dient zum Entfernen von vorhandenen Benutzern aus vorhandenen Gruppen. |
> | Aktion | Microsoft.ApiManagement/service/groups/users/read | Dient zum Abrufen einer Liste mit Gruppenbenutzern. |
> | Aktion | Microsoft.ApiManagement/service/groups/users/write | Dient zum Hinzufügen von vorhandenen Benutzern zu vorhandenen Gruppen. |
> | Aktion | Microsoft.ApiManagement/service/groups/write | Dient zum Erstellen einer neuen Gruppe oder zum Aktualisieren vorhandener Gruppendetails. |
> | Aktion | Microsoft.ApiManagement/service/identityProviders/delete | Dient zum Entfernen bereits vorhandener Identitätsanbieter. |
> | Aktion | Microsoft.ApiManagement/service/identityProviders/read | Dient zum Abrufen der Identitätsanbieterliste oder zum Abrufen von Details zum Identitätsanbieter. |
> | Aktion | Microsoft.ApiManagement/service/identityProviders/write | Dient zum Erstellen eines neuen Identitätsanbieters oder zum Aktualisieren der Details eines bereits vorhandenen Identitätsanbieters. |
> | Aktion | Microsoft.ApiManagement/service/locations/networkstatus/read | Ruft den Netzwerkzugriffsstatus von Ressourcen ab, von denen der Dienst am jeweiligen Standort abhängt. |
> | Aktion | Microsoft.ApiManagement/service/loggers/delete | Dient zum Entfernen vorhandener Protokollierungen. |
> | Aktion | Microsoft.ApiManagement/service/loggers/read | Dient zum Abrufen einer Liste mit Protokollierungen oder zum Abrufen von Protokollierungsdetails. |
> | Aktion | Microsoft.ApiManagement/service/loggers/write | Dient zum Hinzufügen einer neuen Protokollierung oder zum Aktualisieren der Details einer bereits vorhandenen Protokollierung. |
> | Aktion | Microsoft.ApiManagement/service/managedeployments/action | Dient zum Ändern der SKU/Einheiten sowie zum Hinzufügen/Entfernen regionaler Bereitstellungen des API Management-Diensts. |
> | Aktion | Microsoft.ApiManagement/service/networkstatus/read | Ruft den Netzwerkzugriffsstatus von Ressourcen ab, von denen der Dienst abhängt. |
> | Aktion | Microsoft.ApiManagement/service/notifications/action | Sendet Benachrichtigungen an einen angegebenen Benutzer. |
> | Aktion | Microsoft.ApiManagement/service/notifications/read | Ruft alle API Management-Benachrichtigungen für Herausgeber oder die entsprechenden Details ab. |
> | Aktion | Microsoft.ApiManagement/service/notifications/recipientEmails/delete | Entfernt eine vorhandenen E-Mail-Adresse, die einer Benachrichtigung zugeordnet ist. |
> | Aktion | Microsoft.ApiManagement/service/notifications/recipientEmails/read | Ruft die E-Mail-Empfänger ab, die einer API Management-Benachrichtigung für Herausgeber zugeordnet sind. |
> | Aktion | Microsoft.ApiManagement/service/notifications/recipientEmails/write | Erstellt einen neuen E-Mail-Empfänger der Benachrichtigung. |
> | Aktion | Microsoft.ApiManagement/service/notifications/recipientUsers/delete | Entfernt Benutzer, die den Empfängern der Benachrichtigung zugeordnet sind. |
> | Aktion | Microsoft.ApiManagement/service/notifications/recipientUsers/read | Ruft die Empfängerbenutzer ab, die der Benachrichtigung zugeordnet sind. |
> | Aktion | Microsoft.ApiManagement/service/notifications/recipientUsers/write | Fügt einen Benutzer zu den Benachrichtigungsempfängern hinzu. |
> | Aktion | Microsoft.ApiManagement/service/notifications/write | Erstellt oder aktualisiert eine API Management-Benachrichtigung für Herausgeber. |
> | Aktion | Microsoft.ApiManagement/service/openidConnectProviders/delete | Dient zum Entfernen vorhandener OpenID Connect-Anbieter. |
> | Aktion | Microsoft.ApiManagement/service/openidConnectProviders/read | Dient zum Abrufen einer Liste mit OpenID Connect-Anbietern oder zum Abrufen von Details zu OpenID Connect-Anbietern. |
> | Aktion | Microsoft.ApiManagement/service/openidConnectProviders/write | Dient zum Erstellen eines neuen OpenID Connect-Anbieters oder zum Aktualisieren der Details eines vorhandenen OpenID Connect-Anbieters. |
> | Aktion | Microsoft.ApiManagement/service/operationresults/read | Ruft den aktuellen Status eines Vorgangs mit langer Ausführungsdauer ab. |
> | Aktion | Microsoft.ApiManagement/service/policies/delete | Dient zum Entfernen der Richtlinienkonfiguration von Mandantenrichtlinien. |
> | Aktion | Microsoft.ApiManagement/service/policies/read | Ruft Richtlinien für Mandanten oder die Richtlinienkonfigurationsdetails für Mandanten ab. |
> | Aktion | Microsoft.ApiManagement/service/policies/write | Dient zum Festlegen von Richtlinienkonfigurationsdetails für Mandanten. |
> | Aktion | Microsoft.ApiManagement/service/policySnippets/read | Ruft alle Richtlinienausschnitte ab. |
> | Aktion | Microsoft.ApiManagement/service/portalsettings/read | Ruft die Registrierungs-, Anmelde- oder Delegierungseinstellungen für das Portal ab. |
> | Aktion | Microsoft.ApiManagement/service/portalsettings/write | Aktualisiert Anmelde-, Registrierungs- oder Delegierungseinstellungen. |
> | Aktion | Microsoft.ApiManagement/service/products/apis/delete | Dient dazu, vorhandene APIs von vorhandenen Produkten zu entfernen. |
> | Aktion | Microsoft.ApiManagement/service/products/apis/read | Dient zum Abrufen einer Liste mit APIs, die einem vorhandenen Produkt hinzugefügt wurden. |
> | Aktion | Microsoft.ApiManagement/service/products/apis/write | Dient zum Hinzufügen von vorhandenen APIs zu vorhandenen Produkten. |
> | Aktion | Microsoft.ApiManagement/service/products/delete | Dient zum Entfernen vorhandener Produkt. |
> | Aktion | Microsoft.ApiManagement/service/products/groups/delete | Dient zum Löschen der Zuordnung von vorhandenen Entwicklergruppen zu vorhandenen Produkten. |
> | Aktion | Microsoft.ApiManagement/service/products/groups/read | Dient zum Abrufen einer Liste mit Entwicklergruppen, die einem Produkt zugeordnet sind. |
> | Aktion | Microsoft.ApiManagement/service/products/groups/write | Dient zum Zuordnen von vorhandenen Entwicklergruppen zu vorhandenen Produkten. |
> | Aktion | Microsoft.ApiManagement/service/products/policies/delete | Dient zum Entfernen der Richtlinienkonfiguration von Produktrichtlinien. |
> | Aktion | Microsoft.ApiManagement/service/products/policies/read | Ruft Richtlinien für Produkte oder Richtlinienkonfigurationsdetails für Produkte ab. |
> | Aktion | Microsoft.ApiManagement/service/products/policies/write | Dient zum Festlegen von Richtlinienkonfigurationsdetails für Produkte. |
> | Aktion | Microsoft.ApiManagement/service/products/policy/delete | Dient zum Entfernen der Richtlinienkonfiguration von vorhandenen Produkten. |
> | Aktion | Microsoft.ApiManagement/service/products/policy/read | Dient zum Abrufen der Richtlinienkonfiguration vorhandener Produkte. |
> | Aktion | Microsoft.ApiManagement/service/products/policy/write | Dient zum Festlegen der Richtlinienkonfiguration für vorhandene Produkte. |
> | Aktion | Microsoft.ApiManagement/service/products/read | Dient zum Abrufen einer Liste mit Produkten oder zum Abrufen von Produktdetails. |
> | Aktion | Microsoft.ApiManagement/service/products/subscriptions/read | Dient zum Abrufen einer Liste mit Produktabonnements. |
> | Aktion | Microsoft.ApiManagement/service/products/tags/delete | Löscht die Zuordnung zwischen einer vorhandenen Markierung und einem vorhandenen Produkt. |
> | Aktion | Microsoft.ApiManagement/service/products/tags/read | Ruft die dem Produkt zugeordneten Markierungen oder Markierungsdetails ab. |
> | Aktion | Microsoft.ApiManagement/service/products/tags/write | Ordnet eine vorhandene Markierung einem vorhandenen Produkt zu. |
> | Aktion | Microsoft.ApiManagement/service/products/write | Dient zum Erstellen eines neuen Produkts oder zum Aktualisieren vorhandener Produktdetails. |
> | Aktion | Microsoft.ApiManagement/service/properties/delete | Entfernt vorhandene Eigenschaften. |
> | Aktion | Microsoft.ApiManagement/service/properties/read | Dient zum Abrufen einer Liste mit allen Eigenschaften oder zum Abrufen von Details zur angegebenen Eigenschaft. |
> | Aktion | Microsoft.ApiManagement/service/properties/write | Erstellt eine neue Eigenschaft oder aktualisiert den Wert für die angegebene Eigenschaft. |
> | Aktion | Microsoft.ApiManagement/service/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für den API Management-Dienst ab. |
> | Aktion | Microsoft.ApiManagement/service/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für den API Management-Dienst. |
> | Aktion | Microsoft.ApiManagement/service/providers/Microsoft.Insights/logDefinitions/read | Ruft die verfügbaren Protokolle für den API Management-Dienst ab. |
> | Aktion | Microsoft.ApiManagement/service/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für den API Management-Dienst ab. |
> | Aktion | Microsoft.ApiManagement/service/quotas/periods/read | Ruft den Wert des Kontingentzählers für einen bestimmten Zeitraum ab. |
> | Aktion | Microsoft.ApiManagement/service/quotas/periods/write | Legt den aktuellen Wert für den Kontingentzähler fest. |
> | Aktion | Microsoft.ApiManagement/service/quotas/read | Ruft Kontingentwerte ab. |
> | Aktion | Microsoft.ApiManagement/service/quotas/write | Legt den aktuellen Wert für den Kontingentzähler fest. |
> | Aktion | Microsoft.ApiManagement/service/read | Dient zum Lesen der Metadaten für eine API Management-Dienstinstanz. |
> | Aktion | Microsoft.ApiManagement/service/reports/read | Ruft Berichte ab, die nach Zeiträumen, geografischer Region oder Entwicklern aggregiert sind.<br>Dient alternativ zum Abrufen von nach Produkten aggregierten Berichten.<br>Dient alternativ zum Abrufen von Berichten, die nach APIs, Vorgängen oder Abonnement aggregiert sind.<br>Dient alternativ zum Abrufen von Berichtsdaten für Anforderungen. |
> | Aktion | Microsoft.ApiManagement/service/restore/action | Dient zum Wiederherstellen des API Management-Diensts aus dem angegebenen Container in einem vom Benutzer bereitgestellten Speicherkonto. |
> | Aktion | Microsoft.ApiManagement/service/subscriptions/delete | Dient zum Löschen von Abonnements. Dieser Vorgang kann zum Löschen von Abonnements verwendet werden. |
> | Aktion | Microsoft.ApiManagement/service/subscriptions/read | Dient zum Abrufen einer Liste mit Produktabonnements oder zum Abrufen von Produktabonnementdetails. |
> | Aktion | Microsoft.ApiManagement/service/subscriptions/regeneratePrimaryKey/action | Dient zum erneuten Generieren des primären Abonnementschlüssels. |
> | Aktion | Microsoft.ApiManagement/service/subscriptions/regenerateSecondaryKey/action | Dient zum erneuten Generieren des sekundären Abonnementschlüssels. |
> | Aktion | Microsoft.ApiManagement/service/subscriptions/write | Dient dazu, für einen vorhandenen Benutzer ein Abonnement für ein vorhandenes Produkt zu erstellen oder die Details eines vorhandenen Abonnements zu aktualisieren. Dieser Vorgang kann zum Verlängern von Abonnements verwendet werden. |
> | Aktion | Microsoft.ApiManagement/service/tagResources/read | Ruft die Liste der Markierungen ab, die den Ressourcen zugeordnet sind. |
> | Aktion | Microsoft.ApiManagement/service/tags/delete | Dient zum Entfernen vorhandener Markierungen. |
> | Aktion | Microsoft.ApiManagement/service/tags/read | Dient zum Abrufen einer Liste mit Tags oder zum Abrufen von Markierungsdetails. |
> | Aktion | Microsoft.ApiManagement/service/tags/write | Fügt eine neue Markierung hinzu oder aktualisiert die Details vorhandener Markierungen. |
> | Aktion | Microsoft.ApiManagement/service/templates/delete | Setzt die standardmäßige E-Mail-Vorlage des API Management zurück. |
> | Aktion | Microsoft.ApiManagement/service/templates/read | Ruft alle E-Mail-Vorlagen des API Management oder die entsprechenden Details ab. |
> | Aktion | Microsoft.ApiManagement/service/templates/write | Erstellt oder aktualisiert die E-Mail-Vorlage des API Management. |
> | Aktion | Microsoft.ApiManagement/service/tenant/delete | Dient zum Entfernen der Richtlinienkonfiguration für den Mandanten. |
> | Aktion | Microsoft.ApiManagement/service/tenant/deploy/action | Führt eine Bereitstellungsaufgabe aus, um Änderungen aus der angegebenen Git-Verzweigung auf die Konfiguration in der Datenbank anzuwenden. |
> | Aktion | Microsoft.ApiManagement/service/tenant/operationResults/read | Dient zum Abrufen einer Liste mit Vorgangsergebnissen oder zum Abrufen des Ergebnisses eines bestimmten Vorgangs. |
> | Aktion | Microsoft.ApiManagement/service/tenant/read | Ruft die Richtlinienkonfiguration für den Mandanten oder Zugriffsdetails des Mandanten ab. |
> | Aktion | Microsoft.ApiManagement/service/tenant/regeneratePrimaryKey/action | Dient zum erneuten Generieren des primären Zugriffsschlüssels. |
> | Aktion | Microsoft.ApiManagement/service/tenant/regenerateSecondaryKey/action | Dient zum erneuten Generieren des sekundären Zugriffsschlüssels. |
> | Aktion | Microsoft.ApiManagement/service/tenant/save/action | Erstellt Commit mit Konfigurationsmomentaufnahme in der angegebenen Verzweigung im Repository. |
> | Aktion | Microsoft.ApiManagement/service/tenant/syncState/read | Dient zum Abrufen des Status der letzten Git-Synchronisierung. |
> | Aktion | Microsoft.ApiManagement/service/tenant/validate/action | Überprüft Änderungen aus der angegebenen Git-Verzweigung. |
> | Aktion | Microsoft.ApiManagement/service/tenant/write | Legt die Richtlinienkonfiguration für den Mandanten ab oder aktualisiert die Zugriffsdetails des Mandanten. |
> | Aktion | Microsoft.ApiManagement/service/updatecertificate/action | Dient zum Hochladen eines SSL-Zertifikats für einen API Management-Dienst. |
> | Aktion | Microsoft.ApiManagement/service/updatehostname/action | Dient zum Einrichten, Aktualisieren oder Entfernen benutzerdefinierter Domänennamen für einen API Management-Dienst. |
> | Aktion | Microsoft.ApiManagement/service/users/action | Registriert einen neuen Benutzer. |
> | Aktion | Microsoft.ApiManagement/service/users/applications/attachments/delete | Entfernt eine Anlage. |
> | Aktion | Microsoft.ApiManagement/service/users/applications/attachments/read | Ruft eine oder mehrere Anwendungsanlagen ab. |
> | Aktion | Microsoft.ApiManagement/service/users/applications/attachments/write | Fügt eine Anlage zu einer Anwendung hinzu. |
> | Aktion | Microsoft.ApiManagement/service/users/applications/delete | Entfernt eine vorhandene Anwendung. |
> | Aktion | Microsoft.ApiManagement/service/users/applications/read | Ruft die Liste aller Benutzeranwendungen oder die entsprechenden Details der API Management-Anwendungen ab. |
> | Aktion | Microsoft.ApiManagement/service/users/applications/write | Registriert eine Anwendung beim API Management oder aktualisiert die jeweiligen Anwendungsdetails. |
> | Aktion | Microsoft.ApiManagement/service/users/delete | Dient zum Entfernen von Benutzerkonten. |
> | Aktion | Microsoft.ApiManagement/service/users/generateSsoUrl/action | Dient zum Generieren der SSO-URL. Die URL kann für den Zugriff auf das Verwaltungsportal verwendet werden. |
> | Aktion | Microsoft.ApiManagement/service/users/groups/read | Dient zum Abrufen einer Liste mit Benutzergruppen. |
> | Aktion | Microsoft.ApiManagement/service/users/keys/read | Dient zum Abrufen einer Liste mit Benutzerschlüsseln. |
> | Aktion | Microsoft.ApiManagement/service/users/read | Dient zum Abrufen einer Liste mit registrierten Benutzern oder zum Abrufen der Kontodetails eines Benutzers. |
> | Aktion | Microsoft.ApiManagement/service/users/subscriptions/read | Dient zum Abrufen einer Liste mit Benutzerabonnements. |
> | Aktion | Microsoft.ApiManagement/service/users/token/action | Rufen das Zugriffstoken für einen Benutzer ab. |
> | Aktion | Microsoft.ApiManagement/service/users/write | Dient zum Registrieren eines neuen Benutzers oder zum Aktualisieren der Kontodetails eines vorhandenen Benutzers. |
> | Aktion | Microsoft.ApiManagement/service/write | Dient zum Erstellen einer neuen Instanz des API Management-Diensts. |
> | Aktion | Microsoft.ApiManagement/unregister/action | Dient zum Aufheben der Registrierung des Microsoft.ApiManagement-Ressourcenanbieters. |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Authorization/checkAccess/action | Überprüft, ob der Aufrufer zum Ausführen einer bestimmten Aktion berechtigt ist. |
> | Aktion | Microsoft.Authorization/classicAdministrators/delete | Entfernt den Administrator aus dem Abonnement. |
> | Aktion | Microsoft.Authorization/classicAdministrators/read | Liest die Administratoren für das Abonnement. |
> | Aktion | Microsoft.Authorization/classicAdministrators/write | Dient zum Hinzufügen oder Ändern von Administratoren für ein Abonnement. |
> | Aktion | Microsoft.Authorization/elevateAccess/action | Gewährt dem Aufrufer Zugriff vom Typ „Benutzerzugriffsadministrator“ auf der Mandantenebene. |
> | Aktion | Microsoft.Authorization/locks/delete | Dient zum Löschen von Sperren im angegebenen Bereich. |
> | Aktion | Microsoft.Authorization/locks/read | Ruft Sperren im angegebenen Bereich ab. |
> | Aktion | Microsoft.Authorization/locks/write | Dient zum Hinzufügen von Sperren im angegebenen Bereich. |
> | Aktion | Microsoft.Authorization/permissions/read | Listet alle Berechtigungen auf, über die der Aufrufer in einem bestimmten Bereich verfügt. |
> | Aktion | Microsoft.Authorization/policyAssignments/delete | Dient zum Löschen einer Richtlinienzuweisung im angegebenen Bereich. |
> | Aktion | Microsoft.Authorization/policyAssignments/read | Dient zum Abrufen von Informationen zu einer Richtlinienzuweisung. |
> | Aktion | Microsoft.Authorization/policyAssignments/write | Dient zum Erstellen einer Richtlinienzuweisung im angegebenen Bereich. |
> | Aktion | Microsoft.Authorization/policyDefinitions/delete | Dient zum Löschen einer Richtliniendefinition. |
> | Aktion | Microsoft.Authorization/policyDefinitions/read | Dient zum Abrufen von Informationen zu einer Richtliniendefinition. |
> | Aktion | Microsoft.Authorization/policyDefinitions/write | Dient zum Erstellen einer benutzerdefinierten Richtliniendefinition. |
> | Aktion | Microsoft.Authorization/policySetDefinitions/delete | Löscht eine Richtliniengruppendefinition. |
> | Aktion | Microsoft.Authorization/policySetDefinitions/read | Ruft Informationen zu einer Richtliniengruppendefinition ab. |
> | Aktion | Microsoft.Authorization/policySetDefinitions/write | Erstellt eine benutzerdefinierte Richtliniengruppendefinition. |
> | Aktion | Microsoft.Authorization/providerOperations/read | Dient zum Abrufen von Vorgängen für alle Ressourcenanbieter, die in Rollendefinitionen verwendet werden können. |
> | Aktion | Microsoft.Authorization/roleAssignments/delete | Dient zum Löschen einer Rollenzuweisung im angegebenen Bereich. |
> | Aktion | Microsoft.Authorization/roleAssignments/read | Dient zum Abrufen von Informationen zu einer Rollenzuweisung. |
> | Aktion | Microsoft.Authorization/roleAssignments/write | Dient zum Erstellen einer Rollenzuweisung im angegebenen Bereich. |
> | Aktion | Microsoft.Authorization/roleDefinitions/delete | Dient zum Löschen der angegebenen benutzerdefinierten Rollendefinition. |
> | Aktion | Microsoft.Authorization/roleDefinitions/read | Dient zum Abrufen von Informationen zu einer Rollendefinition. |
> | Aktion | Microsoft.Authorization/roleDefinitions/write | Dient zum Erstellen oder Aktualisieren einer benutzerdefinierten Rollendefinition mit angegebenen Berechtigungen und zuweisbaren Bereichen. |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Automation/automationAccounts/agentRegistrationInformation/read | Liest die Registrierungsinformationen einer Azure Automation DSC-Instanz. |
> | Aktion | Microsoft.Automation/automationAccounts/agentRegistrationInformation/regenerateKey/action | Schreibt eine Anforderung zum erneuten Generieren von Azure Automation DSC-Schlüsseln. |
> | Aktion | Microsoft.Automation/automationAccounts/certificates/delete | Löscht ein Azure Automation-Zertifikatasset. |
> | Aktion | Microsoft.Automation/automationAccounts/certificates/getCount/action | Liest die Anzahl der Zertifikate. |
> | Aktion | Microsoft.Automation/automationAccounts/certificates/read | Ruft ein Azure Automation-Zertifikatasset ab. |
> | Aktion | Microsoft.Automation/automationAccounts/certificates/write | Erstellt oder aktualisiert ein Azure Automation-Zertifikatasset. |
> | Aktion | Microsoft.Automation/automationAccounts/compilationjobs/read | Liest die Kompilierung einer Azure Automation DSC-Instanz. |
> | Aktion | Microsoft.Automation/automationAccounts/compilationjobs/read | Liest die Kompilierung einer Azure Automation DSC-Instanz. |
> | Aktion | Microsoft.Automation/automationAccounts/compilationjobs/write | Schreibt die Kompilierung einer Azure Automation DSC-Instanz. |
> | Aktion | Microsoft.Automation/automationAccounts/compilationjobs/write | Schreibt die Kompilierung einer Azure Automation DSC-Instanz. |
> | Aktion | Microsoft.Automation/automationAccounts/configurations/content/read | Liest den Inhalt der Konfigurationsmedien. |
> | Aktion | Microsoft.Automation/automationAccounts/configurations/delete | Löscht den Inhalt einer Azure Automation DSC-Instanz. |
> | Aktion | Microsoft.Automation/automationAccounts/configurations/getCount/action | Liest den Inhalt einer Azure Automation DSC-Instanz. |
> | Aktion | Microsoft.Automation/automationAccounts/configurations/read | Ruft den Inhalt einer Azure Automation DSC-Instanz ab. |
> | Aktion | Microsoft.Automation/automationAccounts/configurations/write | Schreibt den Inhalt einer Azure Automation DSC-Instanz. |
> | Aktion | Microsoft.Automation/automationAccounts/connections/delete | Löscht ein Azure Automation-Verbindungsasset. |
> | Aktion | Microsoft.Automation/automationAccounts/connections/getCount/action | Liest die Anzahl der Verbindungen. |
> | Aktion | Microsoft.Automation/automationAccounts/connections/read | Ruft ein Azure Automation-Verbindungsasset ab. |
> | Aktion | Microsoft.Automation/automationAccounts/connections/write | Erstellt oder aktualisiert ein Azure Automation-Verbindungsasset. |
> | Aktion | Microsoft.Automation/automationAccounts/connectionTypes/delete | Löscht ein Azure Automation-Verbindungstypasset. |
> | Aktion | Microsoft.Automation/automationAccounts/connectionTypes/read | Ruft ein Azure Automation-Verbindungstypasset ab. |
> | Aktion | Microsoft.Automation/automationAccounts/connectionTypes/write | Erstellt ein Azure Automation-Verbindungstypasset. |
> | Aktion | Microsoft.Automation/automationAccounts/credentials/delete | Löscht ein Azure Automation-Anmeldeinformationsasset. |
> | Aktion | Microsoft.Automation/automationAccounts/credentials/getCount/action | Liest die Anzahl der Anmeldeinformationen. |
> | Aktion | Microsoft.Automation/automationAccounts/credentials/read | Ruft ein Azure Automation-Anmeldeinformationsasset ab. |
> | Aktion | Microsoft.Automation/automationAccounts/credentials/write | Erstellt oder aktualisiert ein Azure Automation-Anmeldeinformationsasset. |
> | Aktion | Microsoft.Automation/automationAccounts/delete | Löscht ein Azure Automation-Konto. |
> | Aktion | Microsoft.Automation/automationAccounts/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | Microsoft.Automation/automationAccounts/diagnosticSettings/write | Legt die Diagnoseeinstellung für die Ressource fest. |
> | Aktion | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/delete | Löscht Hybrid Runbook Worker-Ressourcen. |
> | Aktion | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Liest Hybrid Runbook Worker-Ressourcen. |
> | Aktion | Microsoft.Automation/automationAccounts/jobs/output/read | Ruft die Ausgabe eines Auftrags ab. |
> | Aktion | Microsoft.Automation/automationAccounts/jobs/read | Ruft einen Azure Automation-Auftrag ab. |
> | Aktion | Microsoft.Automation/automationAccounts/jobs/resume/action | Setzt einen Azure Automation-Auftrag fort. |
> | Aktion | Microsoft.Automation/automationAccounts/jobs/runbookContent/action | Ruft den Inhalt des Azure Automation-Runbooks zum Zeitpunkt der Auftragsausführung ab. |
> | Aktion | Microsoft.Automation/automationAccounts/jobs/stop/action | Beendet einen Azure Automation-Auftrag. |
> | Aktion | Microsoft.Automation/automationAccounts/jobs/streams/read | Ruft einen Azure Automation-Auftragsdatenstrom ab. |
> | Aktion | Microsoft.Automation/automationAccounts/jobs/streams/read | Ruft einen Azure Automation-Auftragsdatenstrom ab. |
> | Aktion | Microsoft.Automation/automationAccounts/jobs/suspend/action | Hält einen Azure Automation-Auftrag an. |
> | Aktion | Microsoft.Automation/automationAccounts/jobs/write | Erstellt einen Azure Automation-Auftrag. |
> | Aktion | Microsoft.Automation/automationAccounts/jobSchedules/delete | Löscht einen Azure Automation-Auftragszeitplan. |
> | Aktion | Microsoft.Automation/automationAccounts/jobSchedules/read | Ruft einen Azure Automation-Auftragszeitplan ab. |
> | Aktion | Microsoft.Automation/automationAccounts/jobSchedules/write | Erstellt einen Azure Automation-Auftragszeitplan. |
> | Aktion | Microsoft.Automation/automationAccounts/linkedWorkspace/read | Ruft den Arbeitsbereich ab, der mit dem Automation-Konto verknüpft ist. |
> | Aktion | Microsoft.Automation/automationAccounts/listKeys/action | Liest die Schlüssel für das Automation-Konto. |
> | Aktion | Microsoft.Automation/automationAccounts/logDefinitions/read | Ruft die verfügbaren Protokolle für das Automation-Konto ab. |
> | Aktion | Microsoft.Automation/automationAccounts/modules/activities/read | Ruft Azure Automation-Aktivitäten ab. |
> | Aktion | Microsoft.Automation/automationAccounts/modules/delete | Löscht ein Azure Automation-Modul. |
> | Aktion | Microsoft.Automation/automationAccounts/modules/getCount/action | Ruft die Anzahl der Module innerhalb des Automation-Kontos ab. |
> | Aktion | Microsoft.Automation/automationAccounts/modules/read | Ruft ein Azure Automation-Modul ab. |
> | Aktion | Microsoft.Automation/automationAccounts/modules/write | Erstellt oder aktualisiert ein Azure Automation-Modul. |
> | Aktion | Microsoft.Automation/automationAccounts/nodeConfigurations/delete | Löscht die Knotenkonfiguration einer Azure Automation DSC-Instanz. |
> | Aktion | Microsoft.Automation/automationAccounts/nodeConfigurations/rawContent/action | Liest den Inhalt der Knotenkonfiguration einer Azure Automation DSC-Instanz. |
> | Aktion | Microsoft.Automation/automationAccounts/nodeConfigurations/read | Liest die Knotenkonfiguration einer Azure Automation DSC-Instanz. |
> | Aktion | Microsoft.Automation/automationAccounts/nodeConfigurations/write | Schreibt die Knotenkonfiguration einer Azure Automation DSC-Instanz. |
> | Aktion | Microsoft.Automation/automationAccounts/nodecounts/read | Liest die zusammengefasste Anzahl der Knoten für den angegebenen Typ. |
> | Aktion | Microsoft.Automation/automationAccounts/nodes/delete | Löscht Azure Automation DSC-Knoten. |
> | Aktion | Microsoft.Automation/automationAccounts/nodes/read | Liest Azure Automation DSC-Knoten. |
> | Aktion | Microsoft.Automation/automationAccounts/nodes/reports/content/read | Liest Azure Automation DSC-Berichtsinhalte. |
> | Aktion | Microsoft.Automation/automationAccounts/nodes/reports/read | Liest Azure Automation DSC-Berichte. |
> | Aktion | Microsoft.Automation/automationAccounts/nodes/write | Erstellt oder aktualisiert Azure Automation DSC-Knoten. |
> | Aktion | Microsoft.Automation/automationAccounts/objectDataTypes/fields/read | Ruft TypeFields-Elemente für Azure Automation ab. |
> | Aktion | Microsoft.Automation/automationAccounts/providers/Microsoft.Insights/metricDefinitions/read | Ruft Automation-Metrikdefinitionen ab. |
> | Aktion | Microsoft.Automation/automationAccounts/read | Ruft ein Azure Automation-Konto ab. |
> | Aktion | Microsoft.Automation/automationAccounts/runbooks/content/read | Ruft den Inhalt eines Azure Automation-Runbooks ab. |
> | Aktion | Microsoft.Automation/automationAccounts/runbooks/delete | Löscht ein Azure Automation-Runbook. |
> | Aktion | Microsoft.Automation/automationAccounts/runbooks/draft/content/write | Erstellt den Inhalt eines Azure Automation-Runbookentwurfs. |
> | Aktion | Microsoft.Automation/automationAccounts/runbooks/draft/operationResults/read | Ruft die Vorgangsergebnisse des Azure Automation-Runbookentwurfs ab. |
> | Aktion | Microsoft.Automation/automationAccounts/runbooks/draft/publish/action | Veröffentlicht einen Azure Automation-Runbookentwurf. |
> | Aktion | Microsoft.Automation/automationAccounts/runbooks/draft/read | Ruft einen Azure Automation-Runbookentwurf ab. |
> | Aktion | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/read | Ruft einen Testauftrag für einen Azure Automation-Runbookentwurf ab. |
> | Aktion | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/resume/action | Setzt einen Testauftrag für einen Azure Automation-Runbookentwurf fort. |
> | Aktion | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/stop/action | Beendet einen Testauftrag für einen Azure Automation-Runbookentwurf. |
> | Aktion | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/suspend/action | Hält einen Testauftrag für einen Azure Automation-Runbookentwurf an. |
> | Aktion | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/write | Erstellt einen Testauftrag für einen Azure Automation-Runbookentwurf. |
> | Aktion | Microsoft.Automation/automationAccounts/runbooks/draft/undoEdit/action | Macht die Bearbeitung eines Azure Automation-Runbookentwurfs rückgängig. |
> | Aktion | Microsoft.Automation/automationAccounts/runbooks/getCount/action | Ruft die Anzahl der Azure Automation-Runbooks ab. |
> | Aktion | Microsoft.Automation/automationAccounts/runbooks/read | Ruft ein Azure Automation-Runbook ab. |
> | Aktion | Microsoft.Automation/automationAccounts/runbooks/write | Erstellt oder aktualisiert ein Azure Automation-Runbook. |
> | Aktion | Microsoft.Automation/automationAccounts/schedules/delete | Löscht ein Azure Automation-Zeitplanasset. |
> | Aktion | Microsoft.Automation/automationAccounts/schedules/getCount/action | Ruft die Anzahl der Azure Automation-Zeitpläne ab. |
> | Aktion | Microsoft.Automation/automationAccounts/schedules/read | Ruft ein Azure Automation-Zeitplanasset ab. |
> | Aktion | Microsoft.Automation/automationAccounts/schedules/write | Erstellt oder aktualisiert ein Azure Automation-Zeitplanasset. |
> | Aktion | Microsoft.Automation/automationAccounts/statistics/read | Ruft die Statistik für Azure Automation ab. |
> | Aktion | Microsoft.Automation/automationAccounts/updateDeploymentMachineRuns/read | Ruft einen Bereitstellungscomputer für Azure Automation-Updates ab. |
> | Aktion | Microsoft.Automation/automationAccounts/updateManagementPatchJob/read | Ruft einen Patchauftrag für die Azure Automation-Updateverwaltung ab. |
> | Aktion | Microsoft.Automation/automationAccounts/usages/read | Ruft die Azure Automation-Nutzung ab. |
> | Aktion | Microsoft.Automation/automationAccounts/variables/delete | Löscht ein Azure Automation-Variablenasset. |
> | Aktion | Microsoft.Automation/automationAccounts/variables/read | Liest ein Azure Automation-Variablenasset. |
> | Aktion | Microsoft.Automation/automationAccounts/variables/write | Erstellt oder aktualisiert ein Azure Automation-Variablenasset. |
> | Aktion | Microsoft.Automation/automationAccounts/watchers/delete | Löscht einen Azure Automation Watcher-Auftrag. |
> | Aktion | Microsoft.Automation/automationAccounts/watchers/read | Ruft einen Azure Automation Watcher-Auftrag ab. |
> | Aktion | Microsoft.Automation/automationAccounts/watchers/start/action | Startet einen Azure Automation Watcher-Auftrag. |
> | Aktion | Microsoft.Automation/automationAccounts/watchers/stop/action | Beendet einen Azure Automation Watcher-Auftrag. |
> | Aktion | Microsoft.Automation/automationAccounts/watchers/streams/read | Ruft einen Azure Automation Watcher-Auftragsdatenstrom ab. |
> | Aktion | Microsoft.Automation/automationAccounts/watchers/watcherActions/delete | Löscht Aktionen eines Azure Automation Watcher-Auftrags. |
> | Aktion | Microsoft.Automation/automationAccounts/watchers/watcherActions/read | Ruft Aktionen eines Azure Automation Watcher-Auftrags ab. |
> | Aktion | Microsoft.Automation/automationAccounts/watchers/watcherActions/write | Erstellt Aktionen eines Azure Automation Watcher-Auftrags. |
> | Aktion | Microsoft.Automation/automationAccounts/watchers/write | Erstellt einen Azure Automation Watcher-Auftrag. |
> | Aktion | Microsoft.Automation/automationAccounts/webhooks/action | Generiert einen URI für einen Azure Automation-Webhook. |
> | Aktion | Microsoft.Automation/automationAccounts/webhooks/delete | Löscht einen Azure Automation-Webhook.  |
> | Aktion | Microsoft.Automation/automationAccounts/webhooks/read | Liest einen Azure Automation-Webhook. |
> | Aktion | Microsoft.Automation/automationAccounts/webhooks/write | Erstellt oder aktualisiert einen Azure Automation-Webhook. |
> | Aktion | Microsoft.Automation/automationAccounts/write | Erstellt oder aktualisiert ein Azure Automation-Konto. |
> | Aktion | Microsoft.Automation/operations/read | Ruft verfügbare Vorgänge für Azure Automation-Ressourcen ab. |
> | Aktion | Microsoft.Automation/register/action | Registriert das Abonnement für Azure Automation. |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.AzureActiveDirectory/b2cDirectories/delete | Löscht eine B2C-Verzeichnisressource. |
> | Aktion | Microsoft.AzureActiveDirectory/b2cDirectories/read | Zeigt eine B2C-Verzeichnisressource an. |
> | Aktion | Microsoft.AzureActiveDirectory/b2cDirectories/write | Erstellt oder aktualisiert eine B2C-Verzeichnisressource. |
> | Aktion | Microsoft.AzureActiveDirectory/operations/read | Hiermit lesen Sie alle API-Vorgänge, die für den Microsoft.AzureActiveDirectory-Ressourcenanbieter verfügbar sind. |
> | Aktion | Microsoft.AzureActiveDirectory/register/action | Hiermit registrieren Sie das Abonnement für den Microsoft.AzureActiveDirectory-Ressourcenanbieter. |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.AzureStack/Operations/read | Ruft die Eigenschaften für einen Vorgang eines Ressourcenanbieters ab. |
> | Aktion | Microsoft.AzureStack/register/action | Registriert ein Abonnement beim Microsoft.AzureStack-Ressourcenanbieter. |
> | Aktion | Microsoft.AzureStack/registrations/customerSubscriptions/delete | Löscht ein Azure Stack-Kundenabonnement. |
> | Aktion | Microsoft.AzureStack/registrations/customerSubscriptions/read | Ruft die Eigenschaften eines Azure Stack-Kundenabonnements ab. |
> | Aktion | Microsoft.AzureStack/registrations/customerSubscriptions/write | Erstellt oder aktualisiert ein Azure Stack-Kundenabonnement. |
> | Aktion | Microsoft.AzureStack/registrations/delete | Löscht eine Azure Stack-Registrierung. |
> | Aktion | Microsoft.AzureStack/registrations/getActivationKey/action | Ruft den neuesten Azure Stack-Aktivierungsschlüssel ab. |
> | Aktion | Microsoft.AzureStack/registrations/products/listDetails/action | Ruft erweiterte Details für ein Azure Stack-Marketplace-Produkt ab. |
> | Aktion | Microsoft.AzureStack/registrations/products/read | Ruft die Eigenschaften eines Azure Stack-Marketplace-Produkts ab. |
> | Aktion | Microsoft.AzureStack/registrations/read | Ruft die Eigenschaften einer Azure Stack-Registrierung ab. |
> | Aktion | Microsoft.AzureStack/registrations/write | Erstellt oder aktualisiert eine Azure Stack-Registrierung. |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Batch/batchAccounts/applications/delete | Löscht eine Anwendung. |
> | Aktion | Microsoft.Batch/batchAccounts/applications/read | Listet Anwendungen auf oder ruft die Eigenschaften einer Anwendung ab. |
> | Aktion | Microsoft.Batch/batchAccounts/applications/versions/activate/action | Aktiviert ein Anwendungspaket. |
> | Aktion | Microsoft.Batch/batchAccounts/applications/versions/delete | Löscht ein Anwendungspaket. |
> | Aktion | Microsoft.Batch/batchAccounts/applications/versions/read | Ruft die Eigenschaften eines Anwendungspakets ab. |
> | Aktion | Microsoft.Batch/batchAccounts/applications/versions/write | Erstellt ein neues Anwendungspaket oder aktualisiert ein vorhandenes Anwendungspaket. |
> | Aktion | Microsoft.Batch/batchAccounts/applications/write | Erstellt eine neue Anwendung oder aktualisiert eine vorhandene Anwendung. |
> | Aktion | Microsoft.Batch/batchAccounts/certificateOperationResults/read | Ruft die Ergebnisse eines lange andauernden Zertifikatvorgangs für ein Batch-Konto ab. |
> | Aktion | Microsoft.Batch/batchAccounts/certificates/cancelDelete/action | Bricht eine fehlerhafte Löschung eines Zertifikats für ein Batch-Konto ab. |
> | Aktion | Microsoft.Batch/batchAccounts/certificates/delete | Löscht ein Zertifikat aus einem Batch-Konto. |
> | Aktion | Microsoft.Batch/batchAccounts/certificates/read | Listet die Zertifikate für ein Batch-Konto auf oder ruft die Eigenschaften eines Zertifikats ab. |
> | Aktion | Microsoft.Batch/batchAccounts/certificates/write | Erstellt ein neues Zertifikat für ein Batch-Konto oder aktualisiert ein vorhandenes Zertifikat. |
> | Aktion | Microsoft.Batch/batchAccounts/delete | Löscht ein Batch-Konto. |
> | Aktion | Microsoft.Batch/batchAccounts/listkeys/action | Listet Zugriffsschlüssel für ein Batch-Konto auf. |
> | Aktion | Microsoft.Batch/batchAccounts/operationResults/read | Ruft die Ergebnisse eines lange andauernden Vorgangs im Batch-Konto ab. |
> | Aktion | Microsoft.Batch/batchAccounts/poolOperationResults/read | Ruft die Ergebnisse eines lange andauernden Poolvorgangs für ein Batch-Konto ab. |
> | Aktion | Microsoft.Batch/batchAccounts/pools/delete | Löscht einen Pool aus einem Batch-Konto. |
> | Aktion | Microsoft.Batch/batchAccounts/pools/disableAutoscale/action | Deaktiviert die automatische Skalierung für einen Pool des Batch-Kontos. |
> | Aktion | Microsoft.Batch/batchAccounts/pools/read | Listet Pools für ein Batch-Konto auf oder ruft die Eigenschaften eines Pools ab. |
> | Aktion | Microsoft.Batch/batchAccounts/pools/stopResize/action | Beendet einen aktiven Größenänderungsvorgang für einen Pool des Batch-Kontos. |
> | Aktion | Microsoft.Batch/batchAccounts/pools/upgradeOs/action | Führt ein Upgrade für das Betriebssystem eines Batch-Kontopools durch. |
> | Aktion | Microsoft.Batch/batchAccounts/pools/write | Erstellt einen neuen Pool für ein Batch-Konto oder aktualisiert einen vorhandenen Pool. |
> | Aktion | Microsoft.Batch/batchAccounts/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | Microsoft.Batch/batchAccounts/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.Batch/batchAccounts/providers/Microsoft.Insights/logDefinitions/read | Ruft die verfügbaren Protokolle für den Batch-Dienst ab. |
> | Aktion | Microsoft.Batch/batchAccounts/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für den Batch-Dienst ab. |
> | Aktion | Microsoft.Batch/batchAccounts/read | Listet Batch-Konten auf oder ruft die Eigenschaften eines Batch-Kontos ab. |
> | Aktion | Microsoft.Batch/batchAccounts/regeneratekeys/action | Generiert Zugriffsschlüssel für ein Batch-Konto neu. |
> | Aktion | Microsoft.Batch/batchAccounts/syncAutoStorageKeys/action | Synchronisiert Zugriffsschlüssel für das automatische Speicherkonto, das für ein Batch-Konto konfiguriert ist. |
> | Aktion | Microsoft.Batch/batchAccounts/write | Erstellt ein neues Batch-Konto oder aktualisiert ein vorhandenes Batch-Konto. |
> | Aktion | Microsoft.Batch/locations/checkNameAvailability/action | Prüft, ob der Kontoname gültig ist und noch nicht verwendet wird. |
> | Aktion | Microsoft.Batch/locations/quotas/read | Ruft Batch-Kontingente des angegebenen Abonnements in der angegebenen Azure-Region ab. |
> | Aktion | Microsoft.Batch/register/action | Registriert das Abonnement für den Batch-Ressourcenanbieter und ermöglicht die Erstellung von Batch-Konten. |
> | Aktion | Microsoft.Batch/unregister/action | Hebt die Registrierung für das Abonnement für den Batch-Ressourcenanbieter auf und verhindert so die Erstellung von Batch-Konten. |

## <a name="microsoftbatchai"></a>Microsoft.BatchAI

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.BatchAI/clusters/delete | Löscht einen Batch AI-Cluster. |
> | Aktion | Microsoft.BatchAI/clusters/read | Listet Batch AI-Cluster auf oder ruft die Eigenschaften eines Batch AI-Clusters ab. |
> | Aktion | Microsoft.BatchAI/clusters/remoteLoginInformation/action | Listet Informationen zur Remoteanmeldung für einen Batch AI-Cluster auf. |
> | Aktion | Microsoft.BatchAI/clusters/write | Erstellt einen neuen Batch AI-Cluster oder aktualisiert einen vorhandenen Batch AI-Cluster. |
> | Aktion | Microsoft.BatchAI/fileservers/delete | Löscht einen Batch AI-Dateiserver. |
> | Aktion | Microsoft.BatchAI/fileservers/read | Listet Batch AI-Dateiserver auf oder ruft die Eigenschaften eines Batch AI-Dateiservers ab. |
> | Aktion | Microsoft.BatchAI/fileservers/resume/action | Setzt einen Batch AI-Dateiserver fort. |
> | Aktion | Microsoft.BatchAI/fileservers/suspend/action | Hält einen Batch AI-Dateiserver an. |
> | Aktion | Microsoft.BatchAI/fileservers/write | Erstellt einen neuen Batch AI-Dateiserver oder aktualisiert einen vorhandenen Batch AI-Dateiserver. |
> | Aktion | Microsoft.BatchAI/jobs/delete | Löscht einen Batch AI-Auftrag. |
> | Aktion | Microsoft.BatchAI/jobs/read | Listet Batch AI-Aufträge auf oder ruft die Eigenschaften eines Batch AI-Auftrags ab. |
> | Aktion | Microsoft.BatchAI/jobs/remoteLoginInformation/action | Listet Informationen zur Remoteanmeldung für einen Batch AI-Auftrag auf. |
> | Aktion | Microsoft.BatchAI/jobs/terminate/action | Beendet einen Batch AI-Auftrag. |
> | Aktion | Microsoft.BatchAI/jobs/write | Erstellt einen neues Batch AI-Auftrag oder aktualisiert einen vorhandenen Batch AI-Auftrag. |
> | Aktion | Microsoft.BatchAI/register/action | Registriert das Abonnement für den Batch AI-Ressourcenanbieter und ermöglicht die Erstellung von Batch AI-Ressourcen. |

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Billing/billingPeriods/read | Listet die verfügbaren Abrechnungszeiträume auf. |
> | Aktion | Microsoft.Billing/invoices/read | Listet verfügbare Rechnungen auf. |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.BingMaps/mapApis/Delete | Löschvorgang |
> | Aktion | Microsoft.BingMaps/mapApis/listSecrets/action | Dient zum Auflisten der Geheimnisse. |
> | Aktion | Microsoft.BingMaps/mapApis/listSingleSignOnToken/action | Dient zum Lesen des SSO-Autorisierungstokens für die Ressource. |
> | Aktion | Microsoft.BingMaps/mapApis/Read | Lesevorgang |
> | Aktion | Microsoft.BingMaps/mapApis/regenerateKey/action | Generiert den Schlüssel neu. |
> | Aktion | Microsoft.BingMaps/mapApis/Write | Schreibvorgang |
> | Aktion | Microsoft.BingMaps/Operations/read | Beschreibung des Vorgangs. |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Cache/checknameavailability/action | Prüft, ob ein Name für die Verwendung mit einer neuen Redis Cache-Instanz verfügbar ist. |
> | Aktion | Microsoft.Cache/locations/operationresults/read | Ruft das Ergebnis eines lange andauernden Vorgangs ab, dessen Location-Header zuvor an den Client zurückgegeben wurde. |
> | Aktion | Microsoft.Cache/operations/read | Listet die Vorgänge auf, die der Microsoft.Cache-Anbieter unterstützt. |
> | Aktion | Microsoft.Cache/redis/delete | Dient zum Löschen der gesamten Redis Cache-Instanz. |
> | Aktion | Microsoft.Cache/redis/export/action | Dient dazu, Redis-Daten im angegebenen Format in Speicherblobs mit Präfix zu exportieren. |
> | Aktion | Microsoft.Cache/redis/firewallRules/delete | Dient zum Löschen der IP-Firewallregeln einer Redis Cache-Instanz. |
> | Aktion | Microsoft.Cache/redis/firewallRules/read | Dient zum Abrufen der IP-Firewallregeln einer Redis Cache-Instanz. |
> | Aktion | Microsoft.Cache/redis/firewallRules/write | Dient zum Bearbeiten der IP-Firewallregeln einer Redis Cache-Instanz. |
> | Aktion | Microsoft.Cache/redis/forceReboot/action | Dient zum Erzwingen des Neustarts einer Cache-Instanz (ggf. mit Datenverlust). |
> | Aktion | Microsoft.Cache/redis/import/action | Dient zum Importieren von Daten eines angegebenen Formats aus mehreren Blobs in Redis. |
> | Aktion | Microsoft.Cache/redis/linkedservers/delete | Dient zum Löschen von Verbindungsservern aus einer Redis Cache-Instanz. |
> | Aktion | Microsoft.Cache/redis/linkedservers/read | Dient zum Abrufen von Verbindungsservern, die einer Redis Cache-Instanz zugeordnet sind. |
> | Aktion | Microsoft.Cache/redis/linkedservers/write | Dient zum Hinzufügen von Verbindungsservern zu einer Redis Cache-Instanz. |
> | Aktion | Microsoft.Cache/redis/listKeys/action | Dient zum Anzeigen des Werts von Redis Cache-Zugriffsschlüsseln im Verwaltungsportal. |
> | Aktion | Microsoft.Cache/redis/listUpgradeNotifications/read | Dient zum Auflisten der neuesten Upgradebenachrichtigungen für den Cache-Mandanten. |
> | Aktion | Microsoft.Cache/redis/metricDefinitions/read | Ruft die verfügbaren Metriken für eine Redis Cache-Instanz ab. |
> | Aktion | Microsoft.Cache/redis/patchSchedules/delete | Dient zum Löschen des Patchzeitplans einer Redis Cache-Instanz. |
> | Aktion | Microsoft.Cache/redis/patchSchedules/read | Ruft den Patchzeitplan einer Redis Cache-Instanz ab. |
> | Aktion | Microsoft.Cache/redis/patchSchedules/write | Dient zum Ändern des Patchzeitplans einer Redis Cache-Instanz. |
> | Aktion | Microsoft.Cache/redis/read | Dient zum Anzeigen der Redis Cache-Einstellungen und -Konfiguration im Verwaltungsportal. |
> | Aktion | Microsoft.Cache/redis/regenerateKey/action | Dient zum Ändern des Werts von Redis Cache-Zugriffsschlüsseln im Verwaltungsportal. |
> | Aktion | Microsoft.Cache/redis/start/action | Dient zum Starten einer Cache-Instanz. |
> | Aktion | Microsoft.Cache/redis/stop/action | Dient zum Beenden einer Cache-Instanz. |
> | Aktion | Microsoft.Cache/redis/write | Dient zum Ändern der Redis Cache-Einstellungen und -Konfiguration im Verwaltungsportal. |
> | Aktion | Microsoft.Cache/register/action | Registriert den Ressourcenanbieter „Microsoft.Cache“ bei einem Abonnement. |
> | Aktion | Microsoft.Cache/unregister/action | Hebt die Registrierung des Ressourcenanbieters „Microsoft.Cache“ bei einem Abonnement auf. |

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Capacity/register/action | Registriert den Kapazitätsressourcenanbieter und ermöglicht die Erstellung von Kapazitätsressourcen. |
> | Aktion | Microsoft.Capacity/reservationorders/action | Aktualisiert eine Reservierung. |
> | Aktion | Microsoft.Capacity/reservationorders/delete | Löscht eine Reservierung. |
> | Aktion | Microsoft.Capacity/reservationorders/read | Liest alle Reservierungen. |
> | Aktion | Microsoft.Capacity/reservationorders/reservations/action | Aktualisiert eine Reservierung. |
> | Aktion | Microsoft.Capacity/reservationorders/reservations/delete | Löscht eine Reservierung. |
> | Aktion | Microsoft.Capacity/reservationorders/reservations/read | Liest alle Reservierungen. |
> | Aktion | Microsoft.Capacity/reservationorders/reservations/revisions/read | Liest alle Reservierungen. |
> | Aktion | Microsoft.Capacity/reservationorders/reservations/write | Erstellt eine Reservierung. |
> | Aktion | Microsoft.Capacity/reservationorders/write | Erstellt eine Reservierung. |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Cdn/CheckNameAvailability/action |  |
> | Aktion | Microsoft.Cdn/CheckResourceUsage/action |  |
> | Aktion | Microsoft.Cdn/edgenodes/delete |  |
> | Aktion | Microsoft.Cdn/edgenodes/read |  |
> | Aktion | Microsoft.Cdn/edgenodes/write |  |
> | Aktion | Microsoft.Cdn/operationresults/delete |  |
> | Aktion | Microsoft.Cdn/operationresults/profileresults/CheckResourceUsage/action |  |
> | Aktion | Microsoft.Cdn/operationresults/profileresults/delete |  |
> | Aktion | Microsoft.Cdn/operationresults/profileresults/endpointresults/CheckResourceUsage/action |  |
> | Aktion | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/delete |  |
> | Aktion | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/DisableCustomHttps/action |  |
> | Aktion | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/EnableCustomHttps/action |  |
> | Aktion | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/read |  |
> | Aktion | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/write |  |
> | Aktion | Microsoft.Cdn/operationresults/profileresults/endpointresults/delete |  |
> | Aktion | Microsoft.Cdn/operationresults/profileresults/endpointresults/Load/action |  |
> | Aktion | Microsoft.Cdn/operationresults/profileresults/endpointresults/originresults/delete |  |
> | Aktion | Microsoft.Cdn/operationresults/profileresults/endpointresults/originresults/read |  |
> | Aktion | Microsoft.Cdn/operationresults/profileresults/endpointresults/originresults/write |  |
> | Aktion | Microsoft.Cdn/operationresults/profileresults/endpointresults/Purge/action |  |
> | Aktion | Microsoft.Cdn/operationresults/profileresults/endpointresults/read |  |
> | Aktion | Microsoft.Cdn/operationresults/profileresults/endpointresults/Start/action |  |
> | Aktion | Microsoft.Cdn/operationresults/profileresults/endpointresults/Stop/action |  |
> | Aktion | Microsoft.Cdn/operationresults/profileresults/endpointresults/ValidateCustomDomain/action |  |
> | Aktion | Microsoft.Cdn/operationresults/profileresults/endpointresults/write |  |
> | Aktion | Microsoft.Cdn/operationresults/profileresults/GenerateSsoUri/action |  |
> | Aktion | Microsoft.Cdn/operationresults/profileresults/GetSupportedOptimizationTypes/action |  |
> | Aktion | Microsoft.Cdn/operationresults/profileresults/read |  |
> | Aktion | Microsoft.Cdn/operationresults/profileresults/write |  |
> | Aktion | Microsoft.Cdn/operationresults/read |  |
> | Aktion | Microsoft.Cdn/operationresults/write |  |
> | Aktion | Microsoft.Cdn/operations/read |  |
> | Aktion | Microsoft.Cdn/profiles/CheckResourceUsage/action |  |
> | Aktion | Microsoft.Cdn/profiles/delete |  |
> | Aktion | Microsoft.Cdn/profiles/endpoints/CheckResourceUsage/action |  |
> | Aktion | Microsoft.Cdn/profiles/endpoints/customdomains/delete |  |
> | Aktion | Microsoft.Cdn/profiles/endpoints/customdomains/DisableCustomHttps/action |  |
> | Aktion | Microsoft.Cdn/profiles/endpoints/customdomains/EnableCustomHttps/action |  |
> | Aktion | Microsoft.Cdn/profiles/endpoints/customdomains/read |  |
> | Aktion | Microsoft.Cdn/profiles/endpoints/customdomains/write |  |
> | Aktion | Microsoft.Cdn/profiles/endpoints/delete |  |
> | Aktion | Microsoft.Cdn/profiles/endpoints/Load/action |  |
> | Aktion | Microsoft.Cdn/profiles/endpoints/origins/delete |  |
> | Aktion | Microsoft.Cdn/profiles/endpoints/origins/read |  |
> | Aktion | Microsoft.Cdn/profiles/endpoints/origins/write |  |
> | Aktion | Microsoft.Cdn/profiles/endpoints/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellungen für die Ressource ab. |
> | Aktion | Microsoft.Cdn/profiles/endpoints/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellungen für die Ressource. |
> | Aktion | Microsoft.Cdn/profiles/endpoints/providers/Microsoft.Insights/logDefinitions/read | Ruft die verfügbaren Protokolle für Microsoft.Cdn ab. |
> | Aktion | Microsoft.Cdn/profiles/endpoints/Purge/action |  |
> | Aktion | Microsoft.Cdn/profiles/endpoints/read |  |
> | Aktion | Microsoft.Cdn/profiles/endpoints/Start/action |  |
> | Aktion | Microsoft.Cdn/profiles/endpoints/Stop/action |  |
> | Aktion | Microsoft.Cdn/profiles/endpoints/ValidateCustomDomain/action |  |
> | Aktion | Microsoft.Cdn/profiles/endpoints/write |  |
> | Aktion | Microsoft.Cdn/profiles/GenerateSsoUri/action |  |
> | Aktion | Microsoft.Cdn/profiles/GetSupportedOptimizationTypes/action |  |
> | Aktion | Microsoft.Cdn/profiles/read |  |
> | Aktion | Microsoft.Cdn/profiles/write |  |
> | Aktion | Microsoft.Cdn/register/action | Hiermit wird das Abonnement für den CDN-Ressourcenanbieter registriert und die Erstellung von CDN-Profilen aktiviert. |
> | Aktion | Microsoft.Cdn/ValidateProbe/action |  |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.CertificateRegistration/certificateOrders/certificates/Delete | Dient zum Löschen eines vorhandenen Zertifikats. |
> | Aktion | Microsoft.CertificateRegistration/certificateOrders/certificates/Read | Dient zum Abrufen der Zertifikatliste. |
> | Aktion | Microsoft.CertificateRegistration/certificateOrders/certificates/Write | Dient zum Hinzufügen eines neuen Zertifikats oder zum Aktualisieren eines bereits vorhandenen. |
> | Aktion | Microsoft.CertificateRegistration/certificateOrders/Delete | Dient zum Löschen eines vorhandenen AppServiceCertificate-Elements. |
> | Aktion | Microsoft.CertificateRegistration/certificateOrders/Read | Dient zum Abrufen der CertificateOrders-Liste. |
> | Aktion | Microsoft.CertificateRegistration/certificateOrders/reissue/Action | Dient zum erneuten Ausstellen einer vorhandenen Zertifikatbestellung. |
> | Aktion | Microsoft.CertificateRegistration/certificateOrders/renew/Action | Dient zum Verlängern einer vorhandenen Zertifikatbestellung. |
> | Aktion | Microsoft.CertificateRegistration/certificateOrders/resendEmail/Action | Dient zum erneuten Senden von Zertifikat-E-Mails. |
> | Aktion | Microsoft.CertificateRegistration/certificateOrders/resendRequestEmails/Action | Dient zum erneuten Senden von Anforderungs-E-Mails an eine andere E-Mail-Adresse. |
> | Aktion | Microsoft.CertificateRegistration/certificateOrders/resendRequestEmails/Action | Dient zum Abrufen eines Websitesiegels für ein ausgestelltes App Service-Zertifikat. |
> | Aktion | Microsoft.CertificateRegistration/certificateOrders/retrieveCertificateActions/Action | Dient zum Abrufen der Liste mit Zertifikataktionen. |
> | Aktion | Microsoft.CertificateRegistration/certificateOrders/retrieveEmailHistory/Action | Dient zum Abrufen des Zertifikat-E-Mail-Verlaufs. |
> | Aktion | Microsoft.CertificateRegistration/certificateOrders/verifyDomainOwnership/Action | Überprüfen des Domänenbesitzes |
> | Aktion | Microsoft.CertificateRegistration/certificateOrders/Write | Dient zum Hinzufügen eines neuen certificateOrder-Elements oder zum Aktualisieren eines bereits vorhandenen. |
> | Aktion | Microsoft.CertificateRegistration/operations/Read | Dient zum Auflisten aller Vorgänge über die App Service-Zertifikatregistrierung. |
> | Aktion | Microsoft.CertificateRegistration/provisionGlobalAppServicePrincipalInUserTenant/Action | Dient zum Bereitstellen eines Dienstprinzipals für einen Dienst-App-Prinzipal. |
> | Aktion | Microsoft.CertificateRegistration/register/action | Dient zum Registrieren des Microsoft-Zertifikatressourcenanbieters für das Abonnement. |
> | Aktion | Microsoft.CertificateRegistration/validateCertificateRegistrationInformation/Action | Dient zum Überprüfen eines Zertifikaterwerbsobjekts, ohne es zu übermitteln. |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.ClassicCompute/capabilities/read | Zeigt die Funktionen an. |
> | Aktion | Microsoft.ClassicCompute/checkDomainNameAvailability/action | Prüft die Verfügbarkeit eines angegebenen Domänennamens. |
> | Aktion | Microsoft.ClassicCompute/domainNames/active/write | Legt den aktiven Domänennamen fest. |
> | Aktion | Microsoft.ClassicCompute/domainNames/availabilitySets/read | Dient zum Anzeigen der Verfügbarkeitsgruppe für die Ressource. |
> | Aktion | Microsoft.ClassicCompute/domainNames/capabilities/read | Zeigt die Domänennamenfunktionen an. |
> | Aktion | Microsoft.ClassicCompute/domainNames/delete | Dient zum Entfernen der Domänennamen für Ressourcen. |
> | Aktion | Microsoft.ClassicCompute/domainNames/extensions/delete | Dient zum Entfernen der Domänennamenerweiterungen. |
> | Aktion | Microsoft.ClassicCompute/domainNames/extensions/operationStatuses/read | Liest den Vorgangsstatus für die Domänennamenerweiterungen. |
> | Aktion | Microsoft.ClassicCompute/domainNames/extensions/read | Gibt die Domänennamenerweiterungen zurück. |
> | Aktion | Microsoft.ClassicCompute/domainNames/extensions/write | Dient zum Hinzufügen der Domänennamenerweiterungen. |
> | Aktion | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/delete | Dient zum Entfernen eines neuen internen Lastenausgleichsmoduls. |
> | Aktion | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/operationStatuses/read | Liest den Vorgangsstatus für die internen Lastenausgleichsmodule der Domänennamen. |
> | Aktion | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/read | Ruft die internen Lastenausgleichsmodule ab. |
> | Aktion | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/write | Erstellt ein neues internes Lastenausgleichsmodul. |
> | Aktion | Microsoft.ClassicCompute/domainNames/loadBalancedEndpointSets/operationStatuses/read | Liest den Vorgangsstatus für die Endpunktgruppen mit Lastenausgleich der Domänennamen. |
> | Aktion | Microsoft.ClassicCompute/domainNames/loadBalancedEndpointSets/read | Zeigt die Endpunktgruppen mit Lastenausgleich an. |
> | Aktion | Microsoft.ClassicCompute/domainNames/read | Dient zum Zurückgeben der Domänennamen für Ressourcen. |
> | Aktion | Microsoft.ClassicCompute/domainNames/serviceCertificates/delete | Dient zum Löschen der verwendeten Dienstzertifikate. |
> | Aktion | Microsoft.ClassicCompute/domainNames/serviceCertificates/operationStatuses/read | Liest den Vorgangsstatus für die Dienstzertifikate der Domänennamen. |
> | Aktion | Microsoft.ClassicCompute/domainNames/serviceCertificates/read | Gibt die verwendeten Dienstzertifikate zurück. |
> | Aktion | Microsoft.ClassicCompute/domainNames/serviceCertificates/write | Dient zum Hinzufügen oder Ändern der verwendeten Dienstzertifikate. |
> | Aktion | Microsoft.ClassicCompute/domainNames/slots/delete | Löscht einen angegebenen Bereitstellungsslot. |
> | Aktion | Microsoft.ClassicCompute/domainNames/slots/operationStatuses/read | Liest den Vorgangsstatus für die Domänennamenslots. |
> | Aktion | Microsoft.ClassicCompute/domainNames/slots/read | Zeigt die Bereitstellungsslots an. |
> | Aktion | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/delete | Dient zum Entfernen des Erweiterungsverweises für die Rolle „Bereitstellungsslot“. |
> | Aktion | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/operationStatuses/read | Liest den Vorgangsstatus für die Slotrollen-Erweiterungsverweise der Domänennamen. |
> | Aktion | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/read | Gibt den Erweiterungsverweis für die Rolle „Bereitstellungsslot“ zurück. |
> | Aktion | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/write | Dient zum Hinzufügen oder Ändern des Erweiterungsverweises für die Rolle „Bereitstellungsslot“. |
> | Aktion | Microsoft.ClassicCompute/domainNames/slots/roles/providers/Microsoft.Insights/diagnosticSettings/read | Dient zum Abrufen der Diagnoseeinstellungen. |
> | Aktion | Microsoft.ClassicCompute/domainNames/slots/roles/providers/Microsoft.Insights/diagnosticSettings/write | Dient zum Hinzufügen oder Ändern der Diagnoseeinstellungen. |
> | Aktion | Microsoft.ClassicCompute/domainNames/slots/roles/providers/Microsoft.Insights/metricDefinitions/read | Ruft die Metrikdefinitionen ab. |
> | Aktion | Microsoft.ClassicCompute/domainNames/slots/roles/read | Dient zum Abrufen der Rolle für den Bereitstellungsslot. |
> | Aktion | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/operationStatuses/read | Liest den Vorgangsstatus für die Slotrollen-Rolleninstanzen der Domänennamen. |
> | Aktion | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/read | Dient zum Abrufen der Rolleninstanz. |
> | Aktion | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/rebuild/action | Hiermit wird die Rolleninstanz neu erstellt. |
> | Aktion | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/reimage/action | Führt ein Reimaging der Rolleninstanz aus. |
> | Aktion | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/restart/action | Startet Rolleninstanzen neu. |
> | Aktion | Microsoft.ClassicCompute/domainNames/slots/start/action | Startet einen Bereitstellungsslot. |
> | Aktion | Microsoft.ClassicCompute/domainNames/slots/state/start/write | Ändert den Zustand des Bereitstellungsslots in „Beendet“. |
> | Aktion | Microsoft.ClassicCompute/domainNames/slots/state/stop/write | Ändert den Zustand des Bereitstellungsslots in „Gestartet“. |
> | Aktion | Microsoft.ClassicCompute/domainNames/slots/stop/action | Hält den Bereitstellungsslot an. |
> | Aktion | Microsoft.ClassicCompute/domainNames/slots/upgradeDomain/write | Dient zum Aufrüsten der Domäne. |
> | Aktion | Microsoft.ClassicCompute/domainNames/slots/write | Erstellt oder aktualisiert die Bereitstellung. |
> | Aktion | Microsoft.ClassicCompute/domainNames/swap/action | Vertauscht Stagingslot und Produktionsslot. |
> | Aktion | Microsoft.ClassicCompute/domainNames/write | Dient zum Hinzufügen oder Ändern der Domänennamen für Ressourcen. |
> | Aktion | Microsoft.ClassicCompute/moveSubscriptionResources/action | Dient zum Verschieben aller klassischen Ressourcen in ein anderes Abonnement. |
> | Aktion | Microsoft.ClassicCompute/operatingSystemFamilies/read | Listet die in Microsoft Azure verfügbaren Gastbetriebssystemfamilien sowie die für jede Familie verfügbaren Betriebssystemversionen auf. |
> | Aktion | Microsoft.ClassicCompute/operatingSystems/read | Listet die Versionen des Gastbetriebssystems auf, die derzeit in Microsoft Azure verfügbar sind. |
> | Aktion | Microsoft.ClassicCompute/quotas/read | Dient zum Abrufen des Kontingents für das Abonnement. |
> | Aktion | Microsoft.ClassicCompute/register/action | Dient zum Registrieren für klassische Computeressourcen. |
> | Aktion | Microsoft.ClassicCompute/resourceTypes/skus/read | Ruft die SKU-Liste für unterstützte Ressourcentypen ab. |
> | Aktion | Microsoft.ClassicCompute/validateSubscriptionMoveAvailability/action | Dient zum Überprüfen der Verfügbarkeit des Abonnements für einen klassischen Verschiebevorgang. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/delete | Löscht die Netzwerksicherheitsgruppe, die dem virtuellen Computer zugeordnet ist. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/operationStatuses/read | Liest den Vorgangsstatus für die den virtuellen Computern zugeordneten Netzwerksicherheitsgruppen. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/read | Ruft die Netzwerksicherheitsgruppe ab, die dem virtuellen Computer zugeordnet ist. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/write | Fügt eine Netzwerksicherheitsgruppe hinzu, die dem virtuellen Computer zugeordnet ist. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/asyncOperations/read | Ruft die möglichen asynchronen Vorgänge ab. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/attachDisk/action | Fügt einem virtuellen Computer einen Datenträger an. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/delete | Entfernt virtuelle Computer. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/detachDisk/action | Trennt einen Datenträger von einem virtuellen Computer. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/disks/read | Ruft die Liste mit den Datenträgern ab. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/downloadRemoteDesktopConnectionFile/action | Lädt die RDP-Datei für einen virtuellen Computer herunter. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/extensions/operationStatuses/read | Liest den Vorgangsstatus für die VM-Erweiterungen. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/extensions/read | Ruft die VM-Erweiterung ab. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/extensions/write | Legt die VM-Erweiterung fest. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/metrics/read | Ruft die Metriken ab. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/delete | Löscht die Netzwerksicherheitsgruppe, die der Netzwerkschnittstelle zugeordnet ist. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/operationStatuses/read | Liest den Vorgangsstatus für die den virtuellen Computern zugeordneten Netzwerksicherheitsgruppen. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/read | Ruft die Netzwerksicherheitsgruppe ab, die der Netzwerkschnittstelle zugeordnet ist. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/write | Fügt eine Netzwerksicherheitsgruppe hinzu, die der Netzwerkschnittstelle zugeordnet ist. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/operationStatuses/read | Liest den Vorgangsstatus für die virtuellen Computer. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/performMaintenance/action | Hiermit wird eine Wartung für den virtuellen Computer durchgeführt. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/read | Dient zum Abrufen der Diagnoseeinstellungen. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/write | Dient zum Hinzufügen oder Ändern der Diagnoseeinstellungen. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/metricDefinitions/read | Ruft die Metrikdefinitionen ab. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/read | Ruft eine Liste mit virtuellen Computern ab. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/redeploy/action | Stellt den virtuellen Computer erneut bereit. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/restart/action | Startet virtuelle Computer neu. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/shutdown/action | Fährt den virtuellen Computer herunter. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/start/action | Starten Sie den virtuellen Computer. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/stop/action | Beendet den virtuellen Computer. |
> | Aktion | Microsoft.ClassicCompute/virtualMachines/write | Dient zum Hinzufügen oder Ändern virtueller Computer. |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.ClassicNetwork/gatewaySupportedDevices/read | Ruft die Liste mit unterstützten Geräten ab. |
> | Aktion | Microsoft.ClassicNetwork/networkSecurityGroups/delete | Löscht die Netzwerksicherheitsgruppe. |
> | Aktion | Microsoft.ClassicNetwork/networkSecurityGroups/operationStatuses/read | Liest den Vorgangsstatus für die Netzwerksicherheitsgruppe. |
> | Aktion | Microsoft.ClassicNetwork/networkSecurityGroups/read | Ruft die Netzwerksicherheitsgruppe ab. |
> | Aktion | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/delete | Löscht die Sicherheitsregel. |
> | Aktion | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/operationStatuses/read | Liest den Vorgangsstatus für die Sicherheitsregeln der Netzwerksicherheitsgruppe. |
> | Aktion | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/read | Ruft die Sicherheitsregel ab. |
> | Aktion | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/write | Fügt eine Sicherheitsregel hinzu oder aktualisiert sie. |
> | Aktion | Microsoft.ClassicNetwork/networkSecurityGroups/write | Fügt eine neue Netzwerksicherheitsgruppe hinzu. |
> | Aktion | Microsoft.ClassicNetwork/quotas/read | Dient zum Abrufen des Kontingents für das Abonnement. |
> | Aktion | Microsoft.ClassicNetwork/register/action | Dient zum Registrieren für klassische Netzwerkressourcen. |
> | Aktion | Microsoft.ClassicNetwork/reservedIps/delete | Dient zum Löschen einer reservierten IP-Adresse. |
> | Aktion | Microsoft.ClassicNetwork/reservedIps/join/action | Dient zum Einbinden einer reservierten IP-Adresse. |
> | Aktion | Microsoft.ClassicNetwork/reservedIps/link/action | Dient zum Verknüpfen einer reservierten IP-Adresse. |
> | Aktion | Microsoft.ClassicNetwork/reservedIps/operationStatuses/read | Liest den Vorgangsstatus für die reservierten IP-Adressen. |
> | Aktion | Microsoft.ClassicNetwork/reservedIps/read | Ruft die reservierten IP-Adressen ab. |
> | Aktion | Microsoft.ClassicNetwork/reservedIps/write | Dient zum Hinzufügen einer neuen reservierten IP-Adresse. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/capabilities/read | Zeigt die Funktionen an. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/checkIPAddressAvailability/action | Prüft die Verfügbarkeit einer bestimmten IP-Adresse in einem virtuellen Netzwerk. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/delete | Löscht das virtuelle Netzwerk. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/delete | Hebt die Sperrung eines Clientzertifikats auf. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/read | Dient zum Lesen der gesperrten Clientzertifikate. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/write | Sperrt ein Clientzertifikat. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/delete | Löscht das Clientzertifikat des virtuellen Netzwerkgateways. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/download/action | Lädt ein Zertifikat nach Fingerabdruck herunter. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/listPackage/action | Listet das Zertifikatpaket für das virtuelle Netzwerkgateway auf. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/read | Dient zum Suchen nach den Clientstammzertifikaten. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/write | Lädt ein neues Clientstammzertifikat hoch. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/connect/action | Stellt eine Site-to-Site-Gatewayverbindung her. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/disconnect/action | Trennt eine Site-to-Site-Gatewayverbindung. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/read | Ruft die Verbindungsliste ab. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/test/action | Testet eine Site-to-Site-Gatewayverbindung. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/gateways/delete | Löscht das virtuelle Netzwerkgateway. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/gateways/downloadDeviceConfigurationScript/action | Lädt das Gerätekonfigurationsskript herunter. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/gateways/downloadDiagnostics/action | Lädt die Gatewaydiagnose herunter. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/gateways/listCircuitServiceKey/action | Ruft den Schlüssel des Verbindungsdiensts ab. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/gateways/listPackage/action | Listet das Paket für das virtuelle Netzwerkgateway auf. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/gateways/operationStatuses/read | Liest den Vorgangsstatus für die virtuellen Netzwerkgateways. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/gateways/packages/read | Ruft das Paket für das virtuelle Netzwerkgateway ab. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/gateways/read | Ruft die virtuellen Netzwerkgateways ab. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/gateways/startDiagnostics/action | Startet die Diagnose für das virtuelle Netzwerkgateway. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/gateways/stopDiagnostics/action | Beendet die Diagnose für das virtuelle Netzwerkgateway. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/gateways/write | Fügt ein virtuelles Netzwerkgateway hinzu. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/join/action | Führt zum Beitritt zum virtuellen Netzwerk. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/operationStatuses/read | Liest den Vorgangsstatus für die virtuellen Netzwerke. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/peer/action | Richtet ein virtuelles Netzwerk als Peer für ein anderes virtuelles Netzwerk ein. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/read | Dient zum Abrufen des virtuellen Netzwerks. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/delete | Löscht die Netzwerksicherheitsgruppe, die dem Subnetz zugeordnet ist. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/operationStatuses/read | Liest den Vorgangsstatus für die dem Subnetz des virtuellen Netzwerks zugeordnete Netzwerksicherheitsgruppe. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/read | Ruft die Netzwerksicherheitsgruppe ab, die dem Subnetz zugeordnet ist. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/write | Fügt die Netzwerksicherheitsgruppe hinzu, die dem Subnetz zugeordnet ist. |
> | Aktion | Microsoft.ClassicNetwork/virtualNetworks/write | Dient zum Hinzufügen eines neuen virtuellen Netzwerks. |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.ClassicStorage/capabilities/read | Zeigt die Funktionen an. |
> | Aktion | Microsoft.ClassicStorage/checkStorageAccountAvailability/action | Prüft die Verfügbarkeit eines Speicherkontos. |
> | Aktion | Microsoft.ClassicStorage/disks/read | Gibt den Speicherkontodatenträger zurück. |
> | Aktion | Microsoft.ClassicStorage/images/read | Gibt das Image zurück. |
> | Aktion | Microsoft.ClassicStorage/osImages/read | Gibt das Betriebssystemimage zurück. |
> | Aktion | Microsoft.ClassicStorage/publicImages/read | Ruft das öffentliche VM-Image ab. |
> | Aktion | Microsoft.ClassicStorage/quotas/read | Dient zum Abrufen des Kontingents für das Abonnement. |
> | Aktion | Microsoft.ClassicStorage/register/action | Dient zum Registrieren für klassischen Speicher. |
> | Aktion | Microsoft.ClassicStorage/storageAccounts/delete | Löschen Sie das Speicherkonto. |
> | Aktion | Microsoft.ClassicStorage/storageAccounts/disks/delete | Löscht den angegebenen Speicherkontodatenträger. |
> | Aktion | Microsoft.ClassicStorage/storageAccounts/disks/operationStatuses/read | Liest den Vorgangsstatus für die Ressource. |
> | Aktion | Microsoft.ClassicStorage/storageAccounts/disks/read | Gibt den Speicherkontodatenträger zurück. |
> | Aktion | Microsoft.ClassicStorage/storageAccounts/disks/write | Fügt einen Speicherkontodatenträger hinzu. |
> | Aktion | Microsoft.ClassicStorage/storageAccounts/images/delete | Löscht das angegebene Speicherkontoimage. |
> | Aktion | Microsoft.ClassicStorage/storageAccounts/images/read | Gibt das Speicherkontoimage zurück. |
> | Aktion | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Listet die Zugriffsschlüssel für die Speicherkonten auf. |
> | Aktion | Microsoft.ClassicStorage/storageAccounts/operationStatuses/read | Liest den Vorgangsstatus für die Ressource. |
> | Aktion | Microsoft.ClassicStorage/storageAccounts/osImages/delete | Löscht das angegebene Betriebssystemimage für das Speicherkonto. |
> | Aktion | Microsoft.ClassicStorage/storageAccounts/osImages/read | Gibt das Betriebssystemimage für das Speicherkonto zurück. |
> | Aktion | Microsoft.ClassicStorage/storageAccounts/read | Dient zum Zurückgeben des Speicherkontos mit dem angegebenen Konto. |
> | Aktion | Microsoft.ClassicStorage/storageAccounts/regenerateKey/action | Generiert die vorhandenen Zugriffsschlüssel für das Speicherkonto neu. |
> | Aktion | Microsoft.ClassicStorage/storageAccounts/services/diagnosticSettings/read | Dient zum Abrufen der Diagnoseeinstellungen. |
> | Aktion | Microsoft.ClassicStorage/storageAccounts/services/diagnosticSettings/write | Dient zum Hinzufügen oder Ändern der Diagnoseeinstellungen. |
> | Aktion | Microsoft.ClassicStorage/storageAccounts/services/metricDefinitions/read | Ruft die Metrikdefinitionen ab. |
> | Aktion | Microsoft.ClassicStorage/storageAccounts/services/metrics/read | Ruft die Metriken ab. |
> | Aktion | Microsoft.ClassicStorage/storageAccounts/services/read | Dient zum Abrufen der verfügbaren Dienste. |
> | Aktion | Microsoft.ClassicStorage/storageAccounts/write | Fügt ein neues Speicherkonto hinzu. |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.CognitiveServices/accounts/delete | Löscht API-Konten. |
> | Aktion | Microsoft.CognitiveServices/accounts/listKeys/action | Dient zum Auflisten von Schlüsseln. |
> | Aktion | Microsoft.CognitiveServices/accounts/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | Microsoft.CognitiveServices/accounts/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.CognitiveServices/accounts/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für Cognitive Services ab. |
> | Aktion | Microsoft.CognitiveServices/accounts/read | Liest API-Konten. |
> | Aktion | Microsoft.CognitiveServices/accounts/regenerateKey/action | Dient zum erneuten Generieren eines Schlüssels. |
> | Aktion | Microsoft.CognitiveServices/accounts/skus/read | Liest verfügbare SKUs für eine vorhandene Ressource. |
> | Aktion | Microsoft.CognitiveServices/accounts/usages/read | Dient zum Abrufen der Kontingentnutzung für eine vorhandene Ressource. |
> | Aktion | Microsoft.CognitiveServices/accounts/write | Schreibt API-Konten. |
> | Aktion | Microsoft.CognitiveServices/locations/checkSkuAvailability/action | Liest verfügbare SKUs für ein Abonnement. |
> | Aktion | Microsoft.CognitiveServices/Operations/read | Listet alle verfügbaren Vorgänge auf. |
> | Aktion | Microsoft.CognitiveServices/register/action | Registriert ein Abonnement für Cognitive Services. |
> | Aktion | Microsoft.CognitiveServices/skus/read | Liest die verfügbaren SKUs für Cognitive Services. |

## <a name="microsoftcommerce"></a>Microsoft.Commerce

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Commerce/RateCard/read | Gibt Angebotsdaten, Metadaten für Ressourcen/Verbrauchseinheiten und Preise für das angegebene Abonnement zurück. |
> | Aktion | Microsoft.Commerce/UsageAggregates/read | Ruft die Nutzung von Microsoft Azure für ein Abonnement ab. Das Ergebnis enthält aggregierte Nutzungsdaten sowie abonnement- und ressourcenbezogene Informationen für einen bestimmten Zeitbereich. |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Compute/availabilitySets/delete | Löscht die Verfügbarkeitsgruppe. |
> | Aktion | Microsoft.Compute/availabilitySets/read | Dient zum Abrufen der Eigenschaften einer Verfügbarkeitsgruppe. |
> | Aktion | Microsoft.Compute/availabilitySets/vmSizes/read | Dient zum Auflisten der Größen, die zum Erstellen oder Aktualisieren eines virtuellen Computers in der Verfügbarkeitsgruppe zur Verfügung stehen. |
> | Aktion | Microsoft.Compute/availabilitySets/write | Erstellt eine neue Verfügbarkeitsgruppe oder aktualisiert eine bereits vorhandene. |
> | Aktion | Microsoft.Compute/disks/beginGetAccess/action | Dient zum Abrufen des SAS-URIs des Datenträgers für den Blobzugriff. |
> | Aktion | Microsoft.Compute/disks/delete | Löscht den Datenträger. |
> | Aktion | Microsoft.Compute/disks/endGetAccess/action | Dient zum Sperren des SAS-URIs des Datenträgers. |
> | Aktion | Microsoft.Compute/disks/read | Dient zum Abrufen der Eigenschaften eines Datenträgers. |
> | Aktion | Microsoft.Compute/disks/write | Erstellt einen neuen Datenträger oder aktualisiert einen bereits vorhandenen. |
> | Aktion | Microsoft.Compute/images/delete | Löscht das Image. |
> | Aktion | Microsoft.Compute/images/read | Dient zum Abrufen der Eigenschaften des Images. |
> | Aktion | Microsoft.Compute/images/write | Erstellt ein neues Image oder aktualisiert ein bereits vorhandenes. |
> | Aktion | Microsoft.Compute/locations/capsOperations/read | Ruft den Status eines asynchronen Vorgangs zum Abrufen von Obergrenzen ab. |
> | Aktion | Microsoft.Compute/locations/diskOperations/read | Ruft den Status eines asynchronen Vorgangs zum Abrufen von Datenträgern ab. |
> | Aktion | Microsoft.Compute/locations/operations/read | Ruft den Status eines asynchronen Vorgangs ab. |
> | Aktion | Microsoft.Compute/locations/publishers/artifacttypes/offers/read | Ruft die Eigenschaften eines Plattformimageangebots ab. |
> | Aktion | Microsoft.Compute/locations/publishers/artifacttypes/offers/skus/read | Ruft die Eigenschaften einer Plattformimage-SKU ab. |
> | Aktion | Microsoft.Compute/locations/publishers/artifacttypes/offers/skus/versions/read | Ruft die Eigenschaften einer Plattformimageversion ab. |
> | Aktion | Microsoft.Compute/locations/publishers/artifacttypes/types/read | Dient zum Abrufen der Eigenschaften eines VMExtension-Typs. |
> | Aktion | Microsoft.Compute/locations/publishers/artifacttypes/types/versions/read | Dient zum Abrufen der Eigenschaften einer VMExtension-Version. |
> | Aktion | Microsoft.Compute/locations/publishers/read | Dient zum Abrufen der Eigenschaften eines Herausgebers. |
> | Aktion | Microsoft.Compute/locations/runCommands/read | Hiermit werden die am Standort verfügbaren Ausführungsbefehle aufgelistet. |
> | Aktion | Microsoft.Compute/locations/usages/read | Ruft Dienstlimits und aktuelle Nutzungsmengen für die Computeressourcen des Abonnements an einem Standort ab. |
> | Aktion | Microsoft.Compute/locations/vmSizes/read | Listet die verfügbaren VM-Größen für einen Standort auf. |
> | Aktion | Microsoft.Compute/operations/read | Listet verfügbare Vorgänge für den Microsoft.Compute-Ressourcenanbieter auf. |
> | Aktion | Microsoft.Compute/register/action | Registriert ein Abonnement beim Microsoft.Compute-Ressourcenanbieter. |
> | Aktion | Microsoft.Compute/restorePointCollections/delete | Löscht die Wiederherstellungspunktsammlung und die darin enthaltenen Wiederherstellungspunkte. |
> | Aktion | Microsoft.Compute/restorePointCollections/read | Dient zum Abrufen der Eigenschaften einer Wiederherstellungspunktsammlung. |
> | Aktion | Microsoft.Compute/restorePointCollections/restorePoints/delete | Löscht den Wiederherstellungspunkt. |
> | Aktion | Microsoft.Compute/restorePointCollections/restorePoints/read | Dient zum Abrufen der Eigenschaften eines Wiederherstellungspunkts. |
> | Aktion | Microsoft.Compute/restorePointCollections/restorePoints/retrieveSasUris/action | Dient zum Abrufen der Eigenschaften eines Wiederherstellungspunkts sowie der Blob-SAS-URIs. |
> | Aktion | Microsoft.Compute/restorePointCollections/restorePoints/write | Erstellt einen neuen Wiederherstellungspunkt. |
> | Aktion | Microsoft.Compute/restorePointCollections/write | Erstellt eine neue Wiederherstellungspunktsammlung oder aktualisiert eine bereits vorhandene. |
> | Aktion | Microsoft.Compute/sharedVMImages/delete | Löscht SharedVMImage. |
> | Aktion | Microsoft.Compute/sharedVMImages/read | Dient zum Abrufen der Eigenschaften von SharedVMImage. |
> | Aktion | Microsoft.Compute/sharedVMImages/versions/delete | Dient zum Löschen von SharedVMImageVersion. |
> | Aktion | Microsoft.Compute/sharedVMImages/versions/read | Ruft die Eigenschaften von SharedVMImageVersion ab. |
> | Aktion | Microsoft.Compute/sharedVMImages/versions/replicate/action | Repliziert SharedVMImageVersion in Zielregionen. |
> | Aktion | Microsoft.Compute/sharedVMImages/versions/write | Erstellt eine neue SharedVMImageVersion oder aktualisiert eine vorhandene. |
> | Aktion | Microsoft.Compute/sharedVMImages/write | Erstellt ein neues SharedVMImage oder aktualisiert ein vorhandenes. |
> | Aktion | Microsoft.Compute/skus/read | Ruft die Liste der verfügbaren Microsoft.Compute-SKUs für Ihr Abonnement ab. |
> | Aktion | Microsoft.Compute/snapshots/beginGetAccess/action | Dient zum Abrufen des SAS-URI der Momentaufnahme für den Blobzugriff. |
> | Aktion | Microsoft.Compute/snapshots/delete | Dient zum Löschen einer Momentaufnahme. |
> | Aktion | Microsoft.Compute/snapshots/endGetAccess/action | Dient zum Sperren des SAS-URI der Momentaufnahme. |
> | Aktion | Microsoft.Compute/snapshots/read | Dient zum Abrufen der Eigenschaften einer Momentaufnahme. |
> | Aktion | Microsoft.Compute/snapshots/write | Dient zum Erstellen einer neuen Momentaufnahme oder zum Aktualisieren einer bereits vorhandenen. |
> | Aktion | Microsoft.Compute/virtualMachines/capture/action | Erfasst den virtuellen Computer, indem die virtuellen Datenträger kopiert werden und eine Vorlage generiert wird, die dann zum Erstellen ähnlicher virtueller Computer verwendet werden kann. |
> | Aktion | Microsoft.Compute/virtualMachines/convertToManagedDisks/action | Konvertiert blobbasierte Datenträger des virtuellen Computers in verwaltete Datenträger. |
> | Aktion | Microsoft.Compute/virtualMachines/deallocate/action | Schaltet den virtuellen Computer aus und gibt die Computeressourcen frei. |
> | Aktion | Microsoft.Compute/virtualMachines/delete | Löscht den virtuellen Computer. |
> | Aktion | Microsoft.Compute/virtualMachines/extensions/delete | Löscht die VM-Erweiterung. |
> | Aktion | Microsoft.Compute/virtualMachines/extensions/read | Dient zum Abrufen der Eigenschaften einer VM-Erweiterung. |
> | Aktion | Microsoft.Compute/virtualMachines/extensions/write | Erstellt eine neue VM-Erweiterung oder aktualisiert eine bereits vorhandene. |
> | Aktion | Microsoft.Compute/virtualMachines/generalize/action | Legt den Zustand des virtuellen Computers auf „Generalisiert“ fest und bereitet den virtuellen Computer auf die Erfassung vor. |
> | Aktion | Microsoft.Compute/virtualMachines/instanceView/read | Ruft den detaillierten Laufzeitstatus des virtuellen Computers und seiner Ressourcen ab. |
> | Aktion | Microsoft.Compute/virtualMachines/performMaintenance/action | Führt den Wartungsvorgang für die VM durch. |
> | Aktion | Microsoft.Compute/virtualMachines/powerOff/action | Schaltet den virtuellen Computer aus. Der virtuelle Computer wird weiterhin in Rechnung gestellt. |
> | Aktion | Microsoft.Compute/virtualMachines/providers/Microsoft.Insights/metricDefinitions/read | Hiermit werden VM-Metrikdefinitionen gelesen. |
> | Aktion | Microsoft.Compute/virtualMachines/read | Dient zum Abrufen der Eigenschaften eines virtuellen Computers. |
> | Aktion | Microsoft.Compute/virtualMachines/redeploy/action | Stellt den virtuellen Computer erneut bereit. |
> | Aktion | Microsoft.Compute/virtualMachines/reimage/action | Hiermit wird ein Reimaging für einen virtuellen Computer durchgeführt, der einen differenzierenden Datenträger verwendet. |
> | Aktion | Microsoft.Compute/virtualMachines/restart/action | Startet den virtuellen Computer neu. |
> | Aktion | Microsoft.Compute/virtualMachines/runCommand/action | Hiermit wird ein vordefiniertes Skript für den virtuellen Computer ausgeführt. |
> | Aktion | Microsoft.Compute/virtualMachines/start/action | Startet den virtuellen Computer. |
> | Aktion | Microsoft.Compute/virtualMachines/vmSizes/read | Listet die verfügbaren Größen auf, auf die der virtuelle Computer aktualisiert werden kann. |
> | Aktion | Microsoft.Compute/virtualMachines/write | Erstellt einen neuen virtuellen Computer oder aktualisiert einen vorhandenen virtuellen Computer. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/deallocate/action | Schaltet die Computeressourcen für die Instanzen der VM-Skalierungsgruppe aus und gibt sie frei.  |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/delete | Löscht die VM-Skalierungsgruppe. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/delete/action | Löscht die Instanzen der VM-Skalierungsgruppe. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/extensions/delete | Hiermit wird die VM-Skalierungsgruppenerweiterung gelöscht. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/extensions/read | Hiermit werden die Eigenschaften einer VM-Skalierungsgruppenerweiterung abgerufen. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/extensions/write | Hiermit wird eine neue VM-Skalierungsgruppenerweiterung erstellt oder eine vorhandene aktualisiert. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/forceRecoveryServiceFabricPlatformUpdateDomainWalk/action | Hiermit werden Plattformupdatedomänen einer Service Fabric-VM-Skalierungsgruppe manuell durchgeführt, um eine ausstehende Aktualisierung fertigzustellen. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/instanceView/read | Ruft die Instanzansicht der VM-Skalierungsgruppe ab. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/manualUpgrade/action | Aktualisiert Instanzen manuell auf das neueste Modell der VM-Skalierungsgruppe. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/networkInterfaces/read | Ruft die Eigenschaften aller Netzwerkschnittstellen einer VM-Skalierungsgruppe ab. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/osUpgradeHistory/read | Ruft den Verlauf von Betriebssystemupgrades für eine VM-Skalierungsgruppe ab. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/performMaintenance/action | Führt ungeplante Wartungsaufgaben für die Instanzen der VM-Skalierungsgruppe durch. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/powerOff/action | Schaltet die Instanzen der VM-Skalierungsgruppe aus. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/providers/Microsoft.Insights/metricDefinitions/read | Hiermit werden Metrikdefinitionen für VM-Skalierungsgruppen gelesen. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/publicIPAddresses/read | Ruft die Eigenschaften aller öffentlichen IP-Adressen einer VM-Skalierungsgruppe ab. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/read | Dient zum Abrufen der Eigenschaften einer VM-Skalierungsgruppe. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/redeploy/action | Stellt erneut die Instanzen der VM-Skalierungsgruppe bereit. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/reimage/action | Führt ein Reimaging für die Instanzen der VM-Skalierungsgruppe durch. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/restart/action | Startet die Instanzen der VM-Skalierungsgruppe neu. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades/cancel/action | Bricht das parallele Upgrade einer VM-Skalierungsgruppe ab. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades/read | Hiermit wird der aktuelle Status des parallelen Upgrades für eine VM-Skalierungsgruppe abgerufen. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/scale/action | Hiermit wird überprüft, ob eine vorhandene VM-Skalierungsgruppe horizontal auf die angegebene Anzahl von Instanzen hoch- oder herunterskaliert werden kann. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/skus/read | Listet die gültigen SKUs für eine vorhandene VM-Skalierungsgruppe auf. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/start/action | Startet die Instanzen der VM-Skalierungsgruppe. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/deallocate/action | Schaltet die Computeressourcen für einen virtuellen Computer in einer VM-Skalierungsgruppe aus und gibt sie frei. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/delete | Dient zum Löschen eines bestimmten virtuellen Computers in einer VM-Skalierungsgruppe. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/instanceView/read | Ruft die Instanzansicht eines virtuellen Computers in einer VM-Skalierungsgruppe ab. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/ipConfigurations/publicIPAddresses/read | Ruft die Eigenschaften einer öffentlichen IP-Adresse ab, die mithilfe einer VM-Skalierungsgruppe erstellt wurde. Eine VM-Skalierungsgruppe kann höchstens eine öffentliche IP pro IP-Konfiguration (private IP) erstellen. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/ipConfigurations/read | Ruft die Eigenschaften einer oder aller IP-Konfigurationen einer Netzwerkschnittstelle ab, die mit der VM-Skalierungsgruppe erstellt wurde. IP-Konfigurationen stellen private IP-Adressen dar. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/read | Ruft die Eigenschaften einer oder aller Netzwerkschnittstelle eines virtuellen Computers ab, der mithilfe der VM-Skalierungsgruppe erstellt wurde. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/performMaintenance/action | Führt ungeplante Wartungsaufgaben für eine VM-Instanz in einer VM-Skalierungsgruppe durch. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/powerOff/action | Schaltet eine Instanz eines virtuellen Computers in einer VM-Skalierungsgruppe aus. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/providers/Microsoft.Insights/metricDefinitions/read | Hiermit werden die Metrikdefinitionen virtueller Computer in Skalierungsgruppen gelesen. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/read | Ruft die Eigenschaften eines virtuellen Computers in einer VM-Skalierungsgruppe ab. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/redeploy/action | Hiermit wird eine erneute Bereitstellung für eine VM-Instanz in einer VM-Skalierungsgruppe durchgeführt. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/reimage/action | Hiermit wird ein Reimaging für eine VM-Instanz in einer VM-Skalierungsgruppe durchgeführt. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/restart/action | Startet eine Instanz eines virtuellen Computers in einer VM-Skalierungsgruppe neu. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/start/action | Startet eine Instanz eines virtuellen Computers in einer VM-Skalierungsgruppe. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/write | Aktualisiert die Eigenschaften eines virtuellen Computers in einer VM-Skalierungsgruppe. |
> | Aktion | Microsoft.Compute/virtualMachineScaleSets/write | Erstellt eine neue VM-Skalierungsgruppe oder aktualisiert eine bereits vorhandene. |

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Consumption/balances/read | Listet die Auslastungszusammenfassung für einen Abrechnungszeitraum für eine Verwaltungsgruppe auf. |
> | Aktion | Microsoft.Consumption/budgets/read | Liste die Budgets nach einem Abonnement oder einer Verwaltungsgruppe auf. |
> | Aktion | Microsoft.Consumption/budgets/write | Erstellt, aktualisiert und löscht die Budgets nach einem Abonnement oder einer Verwaltungsgruppe. |
> | Aktion | Microsoft.Consumption/marketplaces/read | Listet die Nutzungsdetails von Marketplace-Ressourcen für einen Bereich für EA- und WebDirect-Abonnements auf. |
> | Aktion | Microsoft.Consumption/operations/read | Listet alle vom Microsoft.Consumption-Ressourcenanbieter unterstützte Vorgänge auf. |
> | Aktion | Microsoft.Consumption/pricesheets/read | Listet die Daten zu Preisblättern nach einem Abonnement oder einer Verwaltungsgruppe auf. |
> | Aktion | Microsoft.Consumption/reservationDetails/read | Listet die Auslastungsdetails für reservierte Instanzen nach Reservierungsauftrag oder Verwaltungsgruppe auf. Die Detaildaten gelten pro Instanz und Tag. |
> | Aktion | Microsoft.Consumption/reservationRecommendations/read | Führt einzelne oder freigegebene Empfehlungen für reservierte Instanzen für ein Abonnement ab. |
> | Aktion | Microsoft.Consumption/reservationSummaries/read | Listet die Auslastungszusammenfassung für reservierte Instanzen nach Reservierungsauftrag oder Verwaltungsgruppe auf. Die Zusammenfassungsdaten gelten entweder pro Monat oder Tag. |
> | Aktion | Microsoft.Consumption/reservationTransactions/read | Listet den Transaktionsverlauf für reservierte Instanzen nach Verwaltungsgruppe auf. |
> | Aktion | Microsoft.Consumption/terms/read | Liste die Bedingungen für ein Abonnement oder eine Verwaltungsgruppe auf. |
> | Aktion | Microsoft.Consumption/usageDetails/read | Listet die Nutzungsdetails für einen Bereich für EA- und WebDirect-Abonnements auf. |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.ContainerInstance/containerGroups/containers/logs/read | Hiermit rufen Sie Protokolle für einen bestimmten Container ab. |
> | Aktion | Microsoft.ContainerInstance/containerGroups/delete | Hiermit löschen Sie eine bestimmte Containergruppe. |
> | Aktion | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Containergruppe ab. |
> | Aktion | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Containergruppe. |
> | Aktion | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für die Containergruppe ab. |
> | Aktion | Microsoft.ContainerInstance/containerGroups/read | Hiermit rufen Sie alle Containergruppen ab. |
> | Aktion | Microsoft.ContainerInstance/containerGroups/write | Hiermit erstellen oder aktualisieren Sie eine bestimmte Containergruppe. |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.ContainerRegistry/checkNameAvailability/read | Überprüft, ob der Name der Containerregistrierung für die Verwendung verfügbar ist. |
> | Aktion | Microsoft.ContainerRegistry/locations/operationResults/read | Ruft das Ergebnis eines asynchronen Vorgangs ab. |
> | Aktion | Microsoft.ContainerRegistry/operations/read | Listet alle verfügbaren REST-API-Vorgänge von Azure Container Registry auf. |
> | Aktion | Microsoft.ContainerRegistry/register/action | Registriert das Abonnement für den Containerregistrierungs-Ressourcenanbieter und ermöglicht die Erstellung von Containerregistrierungen. |
> | Aktion | Microsoft.ContainerRegistry/registries/delete | Löscht eine Containerregistrierung. |
> | Aktion | Microsoft.ContainerRegistry/registries/eventGridFilters/delete | Löscht einen Event Grid-Filter aus einer Containerregistrierung. |
> | Aktion | Microsoft.ContainerRegistry/registries/eventGridFilters/read | Ruft die Eigenschaften des angegebenen Event Grid-Filters ab oder listet alle Event Grid-Filter für die angegebene Containerregistrierung auf. |
> | Aktion | Microsoft.ContainerRegistry/registries/eventGridFilters/write | Erstellt oder aktualisiert einen Event Grid-Filter für eine Containerregistrierung mit den angegebenen Parametern. |
> | Aktion | Microsoft.ContainerRegistry/registries/importImage/action | Importiert ein Image in die Containerregistrierung mit den angegebenen Parametern. |
> | Aktion | Microsoft.ContainerRegistry/registries/listCredentials/action | Listet die Anmeldeinformationen für die angegebene Containerregistrierung auf. |
> | Aktion | Microsoft.ContainerRegistry/registries/listUsages/read | Listet den Kontingentbedarf für die angegebene Containerregistrierung auf. |
> | Aktion | Microsoft.ContainerRegistry/registries/operationStatuses/read | Ruft den Status eines asynchronen Registrierungsvorgangs ab. |
> | Aktion | Microsoft.ContainerRegistry/registries/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | Microsoft.ContainerRegistry/registries/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.ContainerRegistry/registries/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für die Microsoft-Containerregistrierung ab. |
> | Aktion | Microsoft.ContainerRegistry/registries/read | Ruft die Eigenschaften der angegebenen Containerregistrierung ab oder listet alle Containerregistrierungen unter der angegebenen Ressourcengruppe oder dem angegebenen Abonnement auf. |
> | Aktion | Microsoft.ContainerRegistry/registries/regenerateCredential/action | Generiert die Anmeldeinformationen für die angegebene Containerregistrierung neu. |
> | Aktion | Microsoft.ContainerRegistry/registries/replications/delete | Löscht eine Replikation aus einer Containerregistrierung. |
> | Aktion | Microsoft.ContainerRegistry/registries/replications/operationStatuses/read | Ruft den Status eines asynchronen Replikationsvorgangs ab. |
> | Aktion | Microsoft.ContainerRegistry/registries/replications/read | Ruft die Eigenschaften der angegebenen Replikation ab oder listet alle Replikationen für die angegebene Containerregistrierung auf. |
> | Aktion | Microsoft.ContainerRegistry/registries/replications/write | Erstellt oder aktualisiert eine Replikation für eine Containerregistrierung mit den angegebenen Parametern. |
> | Aktion | Microsoft.ContainerRegistry/registries/webhooks/delete | Löscht einen Webhook aus einer Containerregistrierung. |
> | Aktion | Microsoft.ContainerRegistry/registries/webhooks/getCallbackConfig/action | Ruft die Konfiguration des Dienst-URI und benutzerdefinierte Header für den Webhook ab. |
> | Aktion | Microsoft.ContainerRegistry/registries/webhooks/listEvents/action | Listet die aktuellen Ereignisse für den angegebenen Webhook auf. |
> | Aktion | Microsoft.ContainerRegistry/registries/webhooks/operationStatuses/read | Ruft den Status eines asynchronen Webhookvorgangs ab. |
> | Aktion | Microsoft.ContainerRegistry/registries/webhooks/ping/action | Löst ein Pingereignis aus, das an den Webhook gesendet werden muss. |
> | Aktion | Microsoft.ContainerRegistry/registries/webhooks/read | Ruft die Eigenschaften des angegebenen Webhooks ab oder listet alle Webhooks für die angegebene Containerregistrierung auf. |
> | Aktion | Microsoft.ContainerRegistry/registries/webhooks/write | Erstellt oder aktualisiert einen Webhook für eine Containerregistrierung mit den angegebenen Parametern. |
> | Aktion | Microsoft.ContainerRegistry/registries/write | Erstellt oder aktualisiert eine Containerregistrierung mit den angegebenen Parametern. |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.ContainerService/containerServices/delete | Erstellt einen Containerdienst. |
> | Aktion | Microsoft.ContainerService/containerServices/read | Ruft einen Containerdienst ab. |
> | Aktion | Microsoft.ContainerService/containerServices/write | Erstellt einen neuen Containerdienst oder aktualisiert einen vorhandenen. |
> | Aktion | Microsoft.ContainerService/managedClusters/accessProfiles/listCredential/action | Ruft ein Zugriffsprofil für verwaltete Cluster anhand des Rollennamens mithilfe der Liste der Anmeldeinformationen ab. |
> | Aktion | Microsoft.ContainerService/managedClusters/accessProfiles/read | Ruft ein Zugriffsprofil für verwaltete Cluster anhand des Rollennamens ab. |
> | Aktion | Microsoft.ContainerService/managedClusters/delete | Löscht einen verwalteten Cluster. |
> | Aktion | Microsoft.ContainerService/managedClusters/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für eine verwaltete Clusterressource ab. |
> | Aktion | Microsoft.ContainerService/managedClusters/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für eine verwaltete Clusterressource. |
> | Aktion | Microsoft.ContainerService/managedClusters/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für verwaltete Cluster ab. |
> | Aktion | Microsoft.ContainerService/managedClusters/read | Ruft einen verwalteten Cluster ab. |
> | Aktion | Microsoft.ContainerService/managedClusters/write | Erstellt einen neuen verwalteten Cluster oder aktualisiert einen vorhandenen. |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.ContentModerator/applications/delete | Löschvorgang |
> | Aktion | Microsoft.ContentModerator/applications/listSecrets/action | Dient zum Auflisten von Geheimnissen. |
> | Aktion | Microsoft.ContentModerator/applications/listSingleSignOnToken/action | Dient zum Lesen von Token für einmaliges Anmelden. |
> | Aktion | Microsoft.ContentModerator/applications/read | Lesevorgang |
> | Aktion | Microsoft.ContentModerator/applications/write | Schreibvorgang |
> | Aktion | Microsoft.ContentModerator/applications/write | Schreibvorgang |
> | Aktion | Microsoft.ContentModerator/listCommunicationPreference/action | Dient zum Auflisten der Kommunikationseinstellungen. |
> | Aktion | Microsoft.ContentModerator/operations/read | Dient zum Lesen von Vorgängen. |
> | Aktion | Microsoft.ContentModerator/updateCommunicationPreference/action | Dient zum Aktualisieren der Kommunikationseinstellungen. |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.CustomerInsights/hubs/adobemetadata/action | Dient zum Erstellen oder Aktualisieren von Adobe-Metadaten in Azure Customer Insights. |
> | Aktion | Microsoft.CustomerInsights/hubs/adobemetadata/read | Dient zum Lesen von Adobe-Metadaten in Azure Customer Insights. |
> | Aktion | Microsoft.CustomerInsights/hubs/authorizationPolicies/delete | Dient zum Löschen einer beliebigen SAS-Richtlinie für Azure Customer Insights. |
> | Aktion | Microsoft.CustomerInsights/hubs/authorizationPolicies/read | Dient zum Lesen einer beliebigen SAS-Richtlinie für Azure Customer Insights. |
> | Aktion | Microsoft.CustomerInsights/hubs/authorizationPolicies/regeneratePrimaryKey/action | Dient zum erneuten Generieren des primären Schlüssels einer SAS-Richtlinie für Azure Customer Insights. |
> | Aktion | Microsoft.CustomerInsights/hubs/authorizationPolicies/regenerateSecondaryKey/action | Dient zum erneuten Generieren des sekundären Schlüssels einer SAS-Richtlinie für Azure Customer Insights. |
> | Aktion | Microsoft.CustomerInsights/hubs/authorizationPolicies/write | Dient zum Erstellen oder Aktualisieren einer beliebigen SAS-Richtlinie für Azure Customer Insights. |
> | Aktion | Microsoft.CustomerInsights/hubs/connectors/activate/action | Dient zum Aktivieren eines beliebigen Azure Customer Insights-Connectors. |
> | Aktion | Microsoft.CustomerInsights/hubs/connectors/activate/action | Dient zum Aktivieren eines beliebigen Azure Customer Insights-Connectors. |
> | Aktion | Microsoft.CustomerInsights/hubs/connectors/delete | Dient zum Löschen eines beliebigen Azure Customer Insights-Connectors. |
> | Aktion | Microsoft.CustomerInsights/hubs/connectors/getruntimestatus/action | Dient zum Abrufen eines beliebigen Laufzeitstatus des Azure Customer Insights-Connectors. |
> | Aktion | Microsoft.CustomerInsights/hubs/connectors/mappings/activate/action | Dient zum Aktivieren einer beliebigen Azure Customer Insights-Connectorzuordnung. |
> | Aktion | Microsoft.CustomerInsights/hubs/connectors/mappings/delete | Dient zum Löschen einer beliebigen Azure Customer Insights-Connectorzuordnung. |
> | Aktion | Microsoft.CustomerInsights/hubs/connectors/mappings/operations/read | Dient zum Lesen eines beliebigen Vorgangsergebnisses einer Azure Customer Insights-Connectorzuordnung. |
> | Aktion | Microsoft.CustomerInsights/hubs/connectors/mappings/read | Dient zum Lesen einer beliebigen Azure Customer Insights-Connectorzuordnung. |
> | Aktion | Microsoft.CustomerInsights/hubs/connectors/mappings/write | Dient zum Erstellen oder Aktualisieren einer beliebigen Azure Customer Insights-Connectorzuordnung. |
> | Aktion | Microsoft.CustomerInsights/hubs/connectors/operations/read | Dient zum Lesen eines beliebigen Vorgangsergebnisses eines Azure Customer Insights-Connectors. |
> | Aktion | Microsoft.CustomerInsights/hubs/connectors/read | Dient zum Lesen eines beliebigen Azure Customer Insights-Connectors. |
> | Aktion | Microsoft.CustomerInsights/hubs/connectors/saveauthinfo/action | Dient zum Erstellen oder Aktualisieren von Informationen zur Authentifizierung eines beliebigen Azure Customer Insights-Connectors. |
> | Aktion | Microsoft.CustomerInsights/hubs/connectors/update/action | Dient zum Aktualisieren eines beliebigen Azure Customer Insights-Connectors. |
> | Aktion | Microsoft.CustomerInsights/hubs/connectors/write | Dient zum Erstellen oder Aktualisieren eines beliebigen Azure Customer Insights-Connectors. |
> | Aktion | Microsoft.CustomerInsights/hubs/crmmetadata/action | Dient zum Erstellen oder Aktualisieren von CRM-Metadaten in Azure Customer Insights. |
> | Aktion | Microsoft.CustomerInsights/hubs/crmmetadata/read | Dient zum Lesen von CRM-Metadaten in Azure Customer Insights. |
> | Aktion | Microsoft.CustomerInsights/hubs/delete | Dient zum Löschen eines beliebigen Azure Customer Insights-Hubs. |
> | Aktion | Microsoft.CustomerInsights/hubs/gdpr/delete | Dient zum Löschen einer beliebigen DSGVO für Azure Customer Insights. |
> | Aktion | Microsoft.CustomerInsights/hubs/gdpr/read | Dient zum Lesen einer beliebigen Azure Customer Insights-DSGVO. |
> | Aktion | Microsoft.CustomerInsights/hubs/gdpr/write | Dient zum Erstellen oder Aktualisieren einer beliebigen Azure Customer Insights-DSGVO. |
> | Aktion | Microsoft.CustomerInsights/hubs/getbillingcredits/read | Dient zum Abrufen des Abrechnungsguthaben im Azure Customer Insights-Hub. |
> | Aktion | Microsoft.CustomerInsights/hubs/getbillinghistory/read | Dient zum Abrufen des Abrechnungsverlaufs im Azure Customer Insights-Hub. |
> | Aktion | Microsoft.CustomerInsights/hubs/images/delete | Dient zum Löschen eines beliebigen Azure Customer Insights-Image. |
> | Aktion | Microsoft.CustomerInsights/hubs/images/read | Dient zum Lesen eines beliebigen Azure Customer Insights-Image. |
> | Aktion | Microsoft.CustomerInsights/hubs/images/write | Dient zum Erstellen oder Aktualisieren eines beliebigen Azure Customer Insights-Image. |
> | Aktion | Microsoft.CustomerInsights/hubs/interactions/delete | Dient zum Löschen beliebiger Azure Customer Insights-Interaktionen. |
> | Aktion | Microsoft.CustomerInsights/hubs/interactions/operations/read | Dient zum Lesen eines beliebigen Vorgangsergebnisses von Azure Customer Insights-Interaktionen. |
> | Aktion | Microsoft.CustomerInsights/hubs/interactions/read | Dient zum Lesen einer beliebigen Azure Customer Insights-Interaktion. |
> | Aktion | Microsoft.CustomerInsights/hubs/interactions/suggestrelationshiplinks/action | Empfiehlt RelationShip-Links für beliebige Azure Insights Customer Insights-Interaktionen. |
> | Aktion | Microsoft.CustomerInsights/hubs/interactions/write | Dient zum Erstellen oder Aktualisieren einer beliebigen Azure Customer Insights-Interaktion. |
> | Aktion | Microsoft.CustomerInsights/hubs/kpi/delete | Dient zum Löschen eines beliebigen Key Performance Indicators für Azure Customer Insights. |
> | Aktion | Microsoft.CustomerInsights/hubs/kpi/operations/read | Dient zum Lesen eines Vorgangsergebnisses für einen beliebigen Key Performance Indicator für Azure Customer Insights. |
> | Aktion | Microsoft.CustomerInsights/hubs/kpi/read | Dient zum Lesen eines beliebigen Key Performance Indicators für Azure Customer Insights. |
> | Aktion | Microsoft.CustomerInsights/hubs/kpi/reprocess/action | Dient zum erneuten Verarbeiten beliebiger Key Performance Indicators für Azure Customer Insights. |
> | Aktion | Microsoft.CustomerInsights/hubs/kpi/write | Dient zum Erstellen oder Aktualisieren eines beliebigen Key Performance Indicators für Azure Customer Insights. |
> | Aktion | Microsoft.CustomerInsights/hubs/links/delete | Dient zum Löschen beliebiger Azure Customer Insights-Verknüpfungen. |
> | Aktion | Microsoft.CustomerInsights/hubs/links/operations/read | Dient zum Lesen eines beliebigen Vorgangsergebnisses für Azure Customer Insights-Verknüpfungen. |
> | Aktion | Microsoft.CustomerInsights/hubs/links/read | Dient zum Lesen beliebiger Azure Customer Insights-Verknüpfungen. |
> | Aktion | Microsoft.CustomerInsights/hubs/links/write | Dient zum Erstellen oder Aktualisieren beliebiger Azure Customer Insights-Verknüpfungen. |
> | Aktion | Microsoft.CustomerInsights/hubs/msemetadata/action | Dient zum Erstellen oder Aktualisieren von MSE-Metadaten in Azure Customer Insights. |
> | Aktion | Microsoft.CustomerInsights/hubs/msemetadata/read | Dient zum Lesen von MSE-Metadaten in Azure Customer Insights. |
> | Aktion | Microsoft.CustomerInsights/hubs/operationresults/read | Dient zum Abrufen von Abrechnungsergebnissen im Azure Customer Insights-Hub. |
> | Aktion | Microsoft.CustomerInsights/hubs/predictions/delete | Dient zum Löschen beliebiger Azure Customer Insights-Vorhersagen. |
> | Aktion | Microsoft.CustomerInsights/hubs/predictions/operations/read | Dient zum Lesen eines beliebigen Vorgangsergebnisses für Azure Customer Insights-Vorhersagen. |
> | Aktion | Microsoft.CustomerInsights/hubs/predictions/read | Dient zum Lesen beliebiger Azure Customer Insights-Vorhersagen. |
> | Aktion | Microsoft.CustomerInsights/hubs/predictions/write | Dient zum Erstellen oder Aktualisieren beliebiger Azure Customer Insights-Vorhersagen. |
> | Aktion | Microsoft.CustomerInsights/hubs/predictivematchpolicies/delete | Dient zum Löschen aller Azure Customer Insights-Richtlinien für den Vorhersageabgleich. |
> | Aktion | Microsoft.CustomerInsights/hubs/predictivematchpolicies/operations/read | Dient zum Lesen des Vorgangsergebnisses aller Azure Customer Insights-Richtlinien für den Vorhersageabgleich. |
> | Aktion | Microsoft.CustomerInsights/hubs/predictivematchpolicies/read | Dient zum Lesen aller Azure Customer Insights-Richtlinien für den Vorhersageabgleich. |
> | Aktion | Microsoft.CustomerInsights/hubs/predictivematchpolicies/write | Dient zum Erstellen oder Aktualisieren aller Azure Customer Insights-Richtlinien für den Vorhersageabgleich. |
> | Aktion | Microsoft.CustomerInsights/hubs/profiles/delete | Dient zum Löschen eines beliebigen Azure Customer Insights-Profils. |
> | Aktion | Microsoft.CustomerInsights/hubs/profiles/operations/read | Dient zum Lesen eines beliebigen Vorgangsergebnisses für das Azure Customer Insights-Profil. |
> | Aktion | Microsoft.CustomerInsights/hubs/profiles/read | Dient zum Lesen eines beliebigen Azure Customer Insights-Profils. |
> | Aktion | Microsoft.CustomerInsights/hubs/profiles/write | Dient zum Schreiben eines beliebigen Azure Customer Insights-Profils. |
> | Aktion | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/logDefinitions/read | Ruft die verfügbaren Protokolle für die Ressource ab. |
> | Aktion | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für die Ressource ab. |
> | Aktion | Microsoft.CustomerInsights/hubs/read | Dient zum Lesen eines beliebigen Azure Customer Insights-Hubs. |
> | Aktion | Microsoft.CustomerInsights/hubs/relationshiplinks/delete | Dient zum Löschen beliebiger Azure Customer Insights-Beziehungsverknüpfungen. |
> | Aktion | Microsoft.CustomerInsights/hubs/relationshiplinks/operations/read | Dient zum Lesen eines beliebigen Vorgangsergebnisses für Azure Customer Insights-Beziehungsverknüpfungen. |
> | Aktion | Microsoft.CustomerInsights/hubs/relationshiplinks/read | Dient zum Lesen beliebiger Azure Customer Insights-Beziehungsverknüpfungen. |
> | Aktion | Microsoft.CustomerInsights/hubs/relationshiplinks/write | Dient zum Erstellen oder Aktualisieren beliebiger Azure Customer Insights-Beziehungsverknüpfungen. |
> | Aktion | Microsoft.CustomerInsights/hubs/relationships/delete | Dient zum Löschen beliebiger Azure Customer Insights-Beziehungen. |
> | Aktion | Microsoft.CustomerInsights/hubs/relationships/operations/read | Dient zum Lesen eines beliebigen Vorgangsergebnisses für Azure Customer Insights-Beziehungen. |
> | Aktion | Microsoft.CustomerInsights/hubs/relationships/read | Dient zum Lesen beliebiger Azure Customer Insights-Beziehungen. |
> | Aktion | Microsoft.CustomerInsights/hubs/relationships/write | Dient zum Erstellen oder Aktualisieren beliebiger Azure Customer Insights-Beziehungen. |
> | Aktion | Microsoft.CustomerInsights/hubs/roleAssignments/delete | Dient zum Löschen einer beliebigen Azure Customer Insights-RBAC-Zuweisung. |
> | Aktion | Microsoft.CustomerInsights/hubs/roleAssignments/operations/read | Dient zum Lesen eines beliebigen Vorgangsergebnisses für eine Azure Customer Insights-RBAC-Zuweisung. |
> | Aktion | Microsoft.CustomerInsights/hubs/roleAssignments/read | Dient zum Lesen einer beliebigen Azure Customer Insights-RBAC-Zuweisung. |
> | Aktion | Microsoft.CustomerInsights/hubs/roleAssignments/write | Dient zum Erstellen oder Aktualisieren einer beliebigen Azure Customer Insights-RBAC-Zuweisung. |
> | Aktion | Microsoft.CustomerInsights/hubs/roles/read | Dient zum Lesen beliebiger Azure Customer Insights-RBAC-Rollen. |
> | Aktion | Microsoft.CustomerInsights/hubs/salesforcemetadata/action | Dient zum Erstellen oder Aktualisieren von Salesforce-Metadaten in Azure Customer Insights. |
> | Aktion | Microsoft.CustomerInsights/hubs/salesforcemetadata/read | Dient zum Lesen von Salesforce-Metadaten in Azure Customer Insights. |
> | Aktion | Microsoft.CustomerInsights/hubs/segments/delete | Dient zum Löschen beliebiger Azure Customer Insights-Segmente. |
> | Aktion | Microsoft.CustomerInsights/hubs/segments/dynamic/action | Dient zur Verwaltung beliebiger Azure Customer Insights-Segmente. |
> | Aktion | Microsoft.CustomerInsights/hubs/segments/read | Dient zum Lesen beliebiger Azure Customer Insights-Segmente. |
> | Aktion | Microsoft.CustomerInsights/hubs/segments/static/action | Dient zur Verwaltung beliebiger statischer Azure Customer Insights-Segmente. |
> | Aktion | Microsoft.CustomerInsights/hubs/segments/write | Dient zum Erstellen oder Aktualisieren beliebiger Azure Customer Insights-Segmente. |
> | Aktion | Microsoft.CustomerInsights/hubs/sqlconnectionstrings/delete | Dient zum Löschen beliebiger SqlConnectionStrings-Elemente in Azure Customer Insights. |
> | Aktion | Microsoft.CustomerInsights/hubs/sqlconnectionstrings/read | Dient zum Lesen beliebiger SqlConnectionStrings-Elemente in Azure Customer Insights. |
> | Aktion | Microsoft.CustomerInsights/hubs/sqlconnectionstrings/write | Dient zum Erstellen oder Aktualisieren beliebiger SqlConnectionStrings-Elemente in Azure Customer Insights. |
> | Aktion | Microsoft.CustomerInsights/hubs/suggesttypeschema/action | Generiert ein Empfehlungstypschema von Beispieldaten. |
> | Aktion | Microsoft.CustomerInsights/hubs/tenantmanagement/read | Verwaltet alle Einstellungen für Azure Customer Insights-Hubs. |
> | Aktion | Microsoft.CustomerInsights/hubs/views/delete | Dient zum Löschen einer beliebigen Azure Customer Insights-App-Ansicht. |
> | Aktion | Microsoft.CustomerInsights/hubs/views/read | Dient zum Lesen einer beliebigen Azure Customer Insights-App-Ansicht. |
> | Aktion | Microsoft.CustomerInsights/hubs/views/write | Dient zum Erstellen oder Aktualisieren einer beliebigen Azure Customer Insights-App-Ansicht. |
> | Aktion | Microsoft.CustomerInsights/hubs/widgettypes/read | Liest Widgettypen der Azure Customer Insights-App. |
> | Aktion | Microsoft.CustomerInsights/hubs/write | Dient zum Erstellen oder Aktualisieren eines beliebigen Azure Customer Insights-Hubs. |
> | Aktion | Microsoft.CustomerInsights/operations/read | Liest Metadaten der Azure Customer Insights-API. |
> | Aktion | Microsoft.CustomerInsights/register/action | Registriert das Abonnement für den Customer Insights-Ressourcenanbieter und ermöglicht die Erstellung von Customer Insights-Ressourcen. |
> | Aktion | Microsoft.CustomerInsights/unregister/action | Hebt die Registrierung des Abonnements für den Customer Insights-Ressourcenanbieter auf. |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Databricks/workspaces/delete | Entfernt einen Arbeitsbereich. |
> | Aktion | Microsoft.Databricks/workspaces/read | Ruft eine Liste der Arbeitsbereiche ab. |
> | Aktion | Microsoft.Databricks/workspaces/write | Erstellt einen Arbeitsbereich. |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.DataCatalog/catalogs/delete | Löscht den Katalog. |
> | Aktion | Microsoft.DataCatalog/catalogs/read | Dient zum Abrufen von Eigenschaften für einen Katalog oder für mehrere Kataloge unter einem Abonnement oder einer Ressourcengruppe. |
> | Aktion | Microsoft.DataCatalog/catalogs/write | Erstellt einen Katalog oder aktualisiert die Tags und Eigenschaften für den Katalog. |
> | Aktion | Microsoft.DataCatalog/checkNameAvailability/action | Prüft die Verfügbarkeit von Katalognamen für Mandanten. |
> | Aktion | Microsoft.DataCatalog/operations/read | Listet verfügbare Vorgänge für den Microsoft.DataCatalog-Ressourcenanbieter auf. |
> | Aktion | Microsoft.DataCatalog/register/action | Registriert ein Abonnement beim Microsoft.DataCatalog-Ressourcenanbieter. |
> | Aktion | Microsoft.DataCatalog/unregister/action | Hebt die Registrierung eines Abonnements beim Microsoft.DataCatalog-Ressourcenanbieter auf. |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.DataFactory/datafactories/activitywindows/read | Liest die Aktivitätsfenster in der Data Factory mit den angegebenen Parametern. |
> | Aktion | Microsoft.DataFactory/datafactories/datapipelines/activities/activitywindows/read | Liest die Aktivitätsfenster für die Pipelineaktivität mit den angegebenen Parametern. |
> | Aktion | Microsoft.DataFactory/datafactories/datapipelines/activitywindows/read | Liest die Aktivitätsfenster für die Pipeline mit den angegebenen Parametern. |
> | Aktion | Microsoft.DataFactory/datafactories/datapipelines/delete | Löscht alle Pipelines. |
> | Aktion | Microsoft.DataFactory/datafactories/datapipelines/pause/action | Hält alle Pipelines an. |
> | Aktion | Microsoft.DataFactory/datafactories/datapipelines/read | Liest alle Pipelines. |
> | Aktion | Microsoft.DataFactory/datafactories/datapipelines/resume/action | Setzt alle Pipelines fort. |
> | Aktion | Microsoft.DataFactory/datafactories/datapipelines/update/action | Aktualisiert alle Pipelines. |
> | Aktion | Microsoft.DataFactory/datafactories/datapipelines/write | Erstellt oder aktualisiert alle Pipelines. |
> | Aktion | Microsoft.DataFactory/datafactories/datasets/activitywindows/read | Liest die Aktivitätsfenster für das Dataset mit den angegebenen Parametern. |
> | Aktion | Microsoft.DataFactory/datafactories/datasets/delete | Löscht alle Datasets. |
> | Aktion | Microsoft.DataFactory/datafactories/datasets/read | Liest alle Datasets. |
> | Aktion | Microsoft.DataFactory/datafactories/datasets/sliceruns/read | Liest die Ausführung des Datenslices für ein angegebenes Dataset mit der angegebenen Startzeit. |
> | Aktion | Microsoft.DataFactory/datafactories/datasets/slices/read | Ruft die Datenslices im angegebenen Zeitraum ab. |
> | Aktion | Microsoft.DataFactory/datafactories/datasets/slices/write | Aktualisiert den Status des Datenslices. |
> | Aktion | Microsoft.DataFactory/datafactories/datasets/write | Erstellt oder aktualisiert alle Datasets. |
> | Aktion | Microsoft.DataFactory/datafactories/delete | Löscht die Data Factory. |
> | Aktion | Microsoft.DataFactory/datafactories/gateways/connectioninfo/action | Liest die Verbindungsinformationen für alle Gateways. |
> | Aktion | Microsoft.DataFactory/datafactories/gateways/delete | Löscht alle Gateways. |
> | Aktion | Microsoft.DataFactory/datafactories/gateways/listauthkeys/action | Führt die Authentifizierungsschlüssel für alle Gateways auf. |
> | Aktion | Microsoft.DataFactory/datafactories/gateways/read | Liest alle Gateways. |
> | Aktion | Microsoft.DataFactory/datafactories/gateways/regenerateauthkey/action | Generiert die Authentifizierungsschlüssel für alle Gateways neu. |
> | Aktion | Microsoft.DataFactory/datafactories/gateways/write | Erstellt oder aktualisiert alle Gateways. |
> | Aktion | Microsoft.DataFactory/datafactories/linkedServices/delete | Löscht alle verknüpften Dienste. |
> | Aktion | Microsoft.DataFactory/datafactories/linkedServices/read | Liest alle verknüpften Dienste. |
> | Aktion | Microsoft.DataFactory/datafactories/linkedServices/write | Erstellt oder aktualisiert alle verknüpften Dienste. |
> | Aktion | Microsoft.DataFactory/datafactories/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | Microsoft.DataFactory/datafactories/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.DataFactory/datafactories/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für Data Factorys ab. |
> | Aktion | Microsoft.DataFactory/datafactories/read | Liest die Data Factory. |
> | Aktion | Microsoft.DataFactory/datafactories/runs/loginfo/read | Liest den SAS-URI für einen Blobcontainer, der die Protokolle enthält. |
> | Aktion | Microsoft.DataFactory/datafactories/tables/delete | Löscht alle Datasets. |
> | Aktion | Microsoft.DataFactory/datafactories/tables/read | Liest alle Datasets. |
> | Aktion | Microsoft.DataFactory/datafactories/tables/write | Erstellt oder aktualisiert alle Datasets. |
> | Aktion | Microsoft.DataFactory/datafactories/write | Erstellt oder aktualisiert die Data Factory. |
> | Aktion | Microsoft.DataFactory/factories/cancelpipelinerun/action | Bricht die Ausführung der Pipeline ab, die von der Ausführungs-ID angegeben wurde. |
> | Aktion | Microsoft.DataFactory/factories/datasets/delete | Löscht alle Datasets. |
> | Aktion | Microsoft.DataFactory/factories/datasets/read | Liest alle Datasets. |
> | Aktion | Microsoft.DataFactory/factories/datasets/write | Erstellt oder aktualisiert alle Datasets. |
> | Aktion | Microsoft.DataFactory/factories/delete | Löscht Data Factorys. |
> | Aktion | Microsoft.DataFactory/factories/integrationruntimes/delete | Löscht alle Integration Runtimes. |
> | Aktion | Microsoft.DataFactory/factories/integrationruntimes/getconnectioninfo/read | Liest Verbindungsinformationen für die Integration Runtime. |
> | Aktion | Microsoft.DataFactory/factories/integrationruntimes/getstatus/read | Liest den Status der Integration Runtime. |
> | Aktion | Microsoft.DataFactory/factories/integrationruntimes/listauthkeys/read | Führt die Authentifizierungsschlüssel für alle Integration Runtimes auf. |
> | Aktion | Microsoft.DataFactory/factories/integrationruntimes/monitoringdata/read | Ruft die Überwachungsdaten für alle Integration Runtimes ab. |
> | Aktion | Microsoft.DataFactory/factories/integrationruntimes/nodes/delete | Löscht den Knoten für die angegebene Integration Runtime. |
> | Aktion | Microsoft.DataFactory/factories/integrationruntimes/nodes/ipAddress/action | Gibt die IP-Adresse für den angegebenen Knoten der Integration Runtime zurück. |
> | Aktion | Microsoft.DataFactory/factories/integrationruntimes/nodes/write | Aktualisiert einen selbstgehosteten Integration Runtime-Knoten. |
> | Aktion | Microsoft.DataFactory/factories/integrationruntimes/read | Liest alle Integration Runtimes. |
> | Aktion | Microsoft.DataFactory/factories/integrationruntimes/regenerateauthkey/action | Generiert die Authentifizierungsschlüssel für die angegebene Integration Runtime neu. |
> | Aktion | Microsoft.DataFactory/factories/integrationruntimes/start/action | Startet alle Integration Runtimes. |
> | Aktion | Microsoft.DataFactory/factories/integrationruntimes/stop/action | Beendet alle Integration Runtimes. |
> | Aktion | Microsoft.DataFactory/factories/integrationruntimes/synccredentials/action | Synchronisiert die Anmeldeinformationen für die angegebene Integration Runtime. |
> | Aktion | Microsoft.DataFactory/factories/integrationruntimes/upgrade/action | Führt ein Upgrade für die angegebene Integration Runtime durch. |
> | Aktion | Microsoft.DataFactory/factories/integrationruntimes/write | Erstellt oder aktualisiert alle Integration Runtimes. |
> | Aktion | Microsoft.DataFactory/factories/linkedServices/delete | Löscht verknüpfte Dienste. |
> | Aktion | Microsoft.DataFactory/factories/linkedServices/read | Liest verknüpfte Dienste. |
> | Aktion | Microsoft.DataFactory/factories/linkedServices/write | Erstellt oder aktualisiert verknüpfte Dienste. |
> | Aktion | Microsoft.DataFactory/factories/pipelineruns/activityruns/read | Liest die Ausführung von Aktivitäten für die angegebene Ausführungs-ID der Pipeline. |
> | Aktion | Microsoft.DataFactory/factories/pipelineruns/read | Liest die Pipelineausführungen. |
> | Aktion | Microsoft.DataFactory/factories/pipelines/createrun/action | Erstellt eine Ausführung für die Pipeline. |
> | Aktion | Microsoft.DataFactory/factories/pipelines/delete | Löscht Pipelines. |
> | Aktion | Microsoft.DataFactory/factories/pipelines/read | Liest Pipelines. |
> | Aktion | Microsoft.DataFactory/factories/pipelines/write | Dient zum Erstellen oder Aktualisieren von Pipelines. |
> | Aktion | Microsoft.DataFactory/factories/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | Microsoft.DataFactory/factories/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.DataFactory/factories/providers/Microsoft.Insights/logDefinitions/read | Ruft die verfügbaren Protokolle für Data Factorys ab. |
> | Aktion | Microsoft.DataFactory/factories/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für Data Factorys ab. |
> | Aktion | Microsoft.DataFactory/factories/read | Liest Data Factorys. |
> | Aktion | Microsoft.DataFactory/factories/triggers/delete | Löscht alle Trigger. |
> | Aktion | Microsoft.DataFactory/factories/triggers/read | Liest alle Trigger. |
> | Aktion | Microsoft.DataFactory/factories/triggers/start/action | Startet alle Trigger. |
> | Aktion | Microsoft.DataFactory/factories/triggers/stop/action | Beendet alle Trigger. |
> | Aktion | Microsoft.DataFactory/factories/triggers/triggerruns/read | Liest die Triggerausführungen. |
> | Aktion | Microsoft.DataFactory/factories/triggers/write | Erstellt oder aktualisiert alle Trigger. |
> | Aktion | Microsoft.DataFactory/factories/write | Dient zum Erstellen oder Aktualisieren von Data Factorys. |
> | Aktion | Microsoft.DataFactory/register/action | Registriert das Abonnement für den Data Factory-Ressourcenanbieter. |
> | Aktion | Microsoft.DataFactory/unregister/action | Hebt die Registrierung des Abonnements für den Data Factory-Ressourcenanbieter auf. |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.DataLakeAnalytics/accounts/computePolicies/delete | Löscht eine Computerichtlinie. |
> | Aktion | Microsoft.DataLakeAnalytics/accounts/computePolicies/read | Ruft Informationen zu einer Computerichtlinie ab. |
> | Aktion | Microsoft.DataLakeAnalytics/accounts/computePolicies/write | Erstellt oder aktualisiert eine Computerichtlinie. |
> | Aktion | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/delete | Hebt die Verknüpfung eines DataLakeStore-Kontos mit einem DataLakeAnalytics-Konto auf. |
> | Aktion | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/read | Ruft Informationen zu einem verknüpften DataLakeStore-Konto eines DataLakeAnalytics-Kontos ab. |
> | Aktion | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/write | Erstellt oder aktualisiert ein verknüpftes DataLakeStore-Konto eines DataLakeAnalytics-Kontos. |
> | Aktion | Microsoft.DataLakeAnalytics/accounts/delete | Löscht ein DataLakeAnalytics-Konto. |
> | Aktion | Microsoft.DataLakeAnalytics/accounts/firewallRules/delete | Dient zum Löschen einer Firewallregel. |
> | Aktion | Microsoft.DataLakeAnalytics/accounts/firewallRules/read | Dient zum Abrufen von Informationen zu einer Firewallregel. |
> | Aktion | Microsoft.DataLakeAnalytics/accounts/firewallRules/write | Dient zum Erstellen oder Aktualisieren einer Firewallregel. |
> | Aktion | Microsoft.DataLakeAnalytics/accounts/operationResults/read | Ruft das Ergebnis eines Vorgangs des DataLakeAnalytics-Kontos ab. |
> | Aktion | Microsoft.DataLakeAnalytics/accounts/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellungen für das DataLakeAnalytics-Konto ab. |
> | Aktion | Microsoft.DataLakeAnalytics/accounts/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellungen für das DataLakeAnalytics-Konto. |
> | Aktion | Microsoft.DataLakeAnalytics/accounts/providers/Microsoft.Insights/logDefinitions/read | Ruft die verfügbaren Protokolle für das DataLakeAnalytics-Konto ab. |
> | Aktion | Microsoft.DataLakeAnalytics/accounts/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für das DataLakeAnalytics-Konto ab. |
> | Aktion | Microsoft.DataLakeAnalytics/accounts/read | Ruft Informationen zu einem vorhandenen DataLakeAnalytics-Konto ab. |
> | Aktion | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Containers/listSasTokens/action | Listet SAS-Tokens für Speichercontainer eines verknüpften Speicherkontos eines DataLakeAnalytics-Kontos auf. |
> | Aktion | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Containers/read | Ruft Container eines verknüpften Speicherkontos eines DataLakeAnalytics-Kontos ab. |
> | Aktion | Microsoft.DataLakeAnalytics/accounts/storageAccounts/delete | Hebt die Verknüpfung eines Speicherkontos mit einem DataLakeAnalytics-Konto auf. |
> | Aktion | Microsoft.DataLakeAnalytics/accounts/storageAccounts/read | Ruft Informationen zu einem verknüpften Speicherkonto eines DataLakeAnalytics-Kontos ab. |
> | Aktion | Microsoft.DataLakeAnalytics/accounts/storageAccounts/write | Erstellt oder aktualisiert ein verknüpftes Speicherkonto eines DataLakeAnalytics-Kontos. |
> | Aktion | Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action | Erteilt Berechtigungen zum Abbrechen von Aufträgen, die von anderen Benutzern übermittelt wurden. |
> | Aktion | Microsoft.DataLakeAnalytics/accounts/write | Erstellt oder aktualisiert ein DataLakeAnalytics-Konto. |
> | Aktion | Microsoft.DataLakeAnalytics/locations/capability/read | Ruft Funktionsinformationen eines Abonnements bezüglich der Verwendung von DataLakeAnalytics ab. |
> | Aktion | Microsoft.DataLakeAnalytics/locations/checkNameAvailability/action | Überprüft die Verfügbarkeit eines DataLakeAnalytics-Kontonamens. |
> | Aktion | Microsoft.DataLakeAnalytics/locations/operationResults/read | Ruft das Ergebnis eines Vorgangs des DataLakeAnalytics-Kontos ab. |
> | Aktion | Microsoft.DataLakeAnalytics/operations/read | Ruft verfügbare Vorgänge von DataLakeAnalytics ab. |
> | Aktion | Microsoft.DataLakeAnalytics/register/action | Registriert ein Abonnement für DataLakeAnalytics. |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.DataLakeStore/accounts/delete | Löscht ein DataLakeStore-Konto. |
> | Aktion | Microsoft.DataLakeStore/accounts/enableKeyVault/action | Aktiviert KeyVault für ein DataLakeStore-Konto. |
> | Aktion | Microsoft.DataLakeStore/accounts/firewallRules/delete | Dient zum Löschen einer Firewallregel. |
> | Aktion | Microsoft.DataLakeStore/accounts/firewallRules/read | Dient zum Abrufen von Informationen zu einer Firewallregel. |
> | Aktion | Microsoft.DataLakeStore/accounts/firewallRules/write | Dient zum Erstellen oder Aktualisieren einer Firewallregel. |
> | Aktion | Microsoft.DataLakeStore/accounts/operationResults/read | Ruft das Ergebnis eines Vorgangs des DataLakeStore-Kontos ab. |
> | Aktion | Microsoft.DataLakeStore/accounts/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellungen für das DataLakeStore-Konto ab. |
> | Aktion | Microsoft.DataLakeStore/accounts/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellungen für das DataLakeStore-Konto. |
> | Aktion | Microsoft.DataLakeStore/accounts/providers/Microsoft.Insights/logDefinitions/read | Ruft die verfügbaren Protokolle für das DataLakeStore-Konto ab. |
> | Aktion | Microsoft.DataLakeStore/accounts/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für das DataLakeStore-Konto ab. |
> | Aktion | Microsoft.DataLakeStore/accounts/read | Ruft Informationen zu einem vorhandenen DataLakeStore-Konto ab. |
> | Aktion | Microsoft.DataLakeStore/accounts/Superuser/action | Erteilt Superuser-Berechtigungen für Data Lake Store, sofern diese über „Microsoft.Authorization/roleAssignments/write“ erteilt werden. |
> | Aktion | Microsoft.DataLakeStore/accounts/trustedIdProviders/delete | Dient zum Löschen eines vertrauenswürdigen Identitätsanbieters. |
> | Aktion | Microsoft.DataLakeStore/accounts/trustedIdProviders/read | Dient zum Abrufen von Informationen zu einem vertrauenswürdigen Identitätsanbieter. |
> | Aktion | Microsoft.DataLakeStore/accounts/trustedIdProviders/write | Dient zum Erstellen oder Aktualisieren eines vertrauenswürdigen Identitätsanbieters. |
> | Aktion | Microsoft.DataLakeStore/accounts/write | Erstellt oder aktualisiert ein DataLakeStore-Konto. |
> | Aktion | Microsoft.DataLakeStore/locations/capability/read | Ruft Funktionsinformationen eines Abonnements bezüglich der Verwendung von DataLakeStore ab. |
> | Aktion | Microsoft.DataLakeStore/locations/checkNameAvailability/action | Überprüft die Verfügbarkeit eines DataLakeStore-Kontonamens. |
> | Aktion | Microsoft.DataLakeStore/locations/operationResults/read | Ruft das Ergebnis eines Vorgangs des DataLakeStore-Kontos ab. |
> | Aktion | Microsoft.DataLakeStore/operations/read | Ruft verfügbare Vorgänge von DataLakeStore ab. |
> | Aktion | Microsoft.DataLakeStore/register/action | Registriert ein Abonnement für DataLakeStore. |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.DBforMySQL/locations/performanceTiers/read | Gibt die Liste der verfügbaren Leistungsstufen zurück. |
> | Aktion | Microsoft.DBforMySQL/performanceTiers/read | Gibt die Liste der verfügbaren Leistungsstufen zurück. |
> | Aktion | Microsoft.DBforMySQL/servers/delete | Löscht einen vorhandenen Server. |
> | Aktion | Microsoft.DBforMySQL/servers/firewallRules/delete | Löscht eine vorhandene Firewallregel. |
> | Aktion | Microsoft.DBforMySQL/servers/firewallRules/read | Gibt die Liste der Firewallregeln für einen Server zurück oder ruft die Eigenschaften für die angegebene Firewallregel ab. |
> | Aktion | Microsoft.DBforMySQL/servers/firewallRules/write | Erstellt eine Firewallregel mit den angegebenen Parametern oder aktualisiert eine vorhandene Regel. |
> | Aktion | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/metricDefinitions/read | Dient zum Zurückgeben verfügbarer Metriktypen für Datenbanken. |
> | Aktion | Microsoft.DBforMySQL/servers/read | Gibt die Liste der Server zurück oder ruft die Eigenschaften für den angegebenen Server ab. |
> | Aktion | Microsoft.DBforMySQL/servers/recoverableServers/read | Gibt die wiederherstellbaren MySQL Server-Informationen zurück. |
> | Aktion | Microsoft.DBforMySQL/servers/virtualNetworkRules/delete | Löscht eine vorhandene Regel für virtuelle Netzwerke. |
> | Aktion | Microsoft.DBforMySQL/servers/virtualNetworkRules/read | Gibt die Liste der Regeln für virtuelle Netzwerke zurück oder ruft die Eigenschaften für die angegebene Regel für virtuelle Netzwerke ab. |
> | Aktion | Microsoft.DBforMySQL/servers/virtualNetworkRules/write | Erstellt eine Regel für virtuelle Netzwerke mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für die angegebene Regel für virtuelle Netzwerke. |
> | Aktion | Microsoft.DBforMySQL/servers/write | Erstellt einen Server mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für den angegebenen Server. |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.DBforPostgreSQL/locations/performanceTiers/read | Gibt die Liste der verfügbaren Leistungsstufen zurück. |
> | Aktion | Microsoft.DBforPostgreSQL/performanceTiers/read | Gibt die Liste der verfügbaren Leistungsstufen zurück. |
> | Aktion | Microsoft.DBforPostgreSQL/servers/delete | Löscht einen vorhandenen Server. |
> | Aktion | Microsoft.DBforPostgreSQL/servers/firewallRules/delete | Löscht eine vorhandene Firewallregel. |
> | Aktion | Microsoft.DBforPostgreSQL/servers/firewallRules/read | Gibt die Liste der Firewallregeln für einen Server zurück oder ruft die Eigenschaften für die angegebene Firewallregel ab. |
> | Aktion | Microsoft.DBforPostgreSQL/servers/firewallRules/write | Erstellt eine Firewallregel mit den angegebenen Parametern oder aktualisiert eine vorhandene Regel. |
> | Aktion | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/logDefinitions/read | Ruft die verfügbaren Protokolle für Postgres-Server ab. |
> | Aktion | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/metricDefinitions/read | Dient zum Zurückgeben verfügbarer Metriktypen für Datenbanken. |
> | Aktion | Microsoft.DBforPostgreSQL/servers/read | Gibt die Liste der Server zurück oder ruft die Eigenschaften für den angegebenen Server ab. |
> | Aktion | Microsoft.DBforPostgreSQL/servers/recoverableServers/read | Gibt die wiederherstellbaren PostgreSQL Server-Informationen zurück. |
> | Aktion | Microsoft.DBforPostgreSQL/servers/securityAlertPolicies/read | Dient zum Abrufen von Details zur Richtlinie für die Serverbedrohungserkennung, die für einen bestimmten Server konfiguriert ist. |
> | Aktion | Microsoft.DBforPostgreSQL/servers/securityAlertPolicies/write | Dient zum Ändern der Richtlinie für die Serverbedrohungserkennung für einen bestimmten Server. |
> | Aktion | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/delete | Löscht eine vorhandene Regel für virtuelle Netzwerke. |
> | Aktion | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/read | Gibt die Liste der Regeln für virtuelle Netzwerke zurück oder ruft die Eigenschaften für die angegebene Regel für virtuelle Netzwerke ab. |
> | Aktion | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/write | Erstellt eine Regel für virtuelle Netzwerke mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für die angegebene Regel für virtuelle Netzwerke. |
> | Aktion | Microsoft.DBforPostgreSQL/servers/write | Erstellt einen Server mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für den angegebenen Server. |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Devices/checkNameAvailability/Action | Dient zum Prüfen, ob ein IotHub-Name verfügbar ist. |
> | Aktion | Microsoft.Devices/checkProvisioningServiceNameAvailability/Action | Dient zum Prüfen, ob ein IotHub-Name verfügbar ist. |
> | Aktion | Microsoft.Devices/ElasticPools/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | Microsoft.Devices/ElasticPools/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.Devices/elasticPools/iotHubTenants/Delete | Löscht die IotHub-Mandantenressource. |
> | Aktion | Microsoft.Devices/ElasticPools/IotHubTenants/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | Microsoft.Devices/ElasticPools/IotHubTenants/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Delete | Dient zum Löschen von EventHub-Consumergruppen. |
> | Aktion | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Read | Dient zum Abrufen von EventHub-Consumergruppen. |
> | Aktion | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Write | Dient zum Erstellen von EventHub-Consumergruppen. |
> | Aktion | Microsoft.Devices/elasticPools/iotHubTenants/exportDevices/Action | Dient zum Exportieren von Geräten. |
> | Aktion | Microsoft.Devices/elasticPools/iotHubTenants/getStats/Read | Ruft die IotHub-Mandantenstatistikressource ab. |
> | Aktion | Microsoft.Devices/elasticPools/iotHubTenants/importDevices/Action | Dient zum Importieren von Geräten. |
> | Aktion | Microsoft.Devices/elasticPools/iotHubTenants/iotHubKeys/listkeys/Action | Ruft den IotHub-Mandantenschlüssel ab. |
> | Aktion | Microsoft.Devices/elasticPools/iotHubTenants/jobs/Read | Dient zum Abrufen von Auftragsdetails, die für eine bestimmte IotHub-Instanz übermittelt wurden. |
> | Aktion | Microsoft.Devices/elasticPools/iotHubTenants/listKeys/Action | Ruft die IotHub-Mandantenschlüssel ab. |
> | Aktion | Microsoft.Devices/ElasticPools/IotHubTenants/logDefinitions/read | Ruft die verfügbaren Protokolldefinitionen für den IotHub-Dienst ab. |
> | Aktion | Microsoft.Devices/ElasticPools/IotHubTenants/metricDefinitions/read | Ruft die verfügbaren Metriken für den IotHub-Dienst ab. |
> | Aktion | Microsoft.Devices/elasticPools/iotHubTenants/quotaMetrics/Read | Dient zum Abrufen von Kontingentmetriken. |
> | Aktion | Microsoft.Devices/elasticPools/iotHubTenants/Read | Ruft die IotHub-Mandantenressource ab. |
> | Aktion | Microsoft.Devices/elasticPools/iotHubTenants/routing/routes/$testall/Action | Dient zum Testen einer Nachricht mit allen vorhandenen Routen. |
> | Aktion | Microsoft.Devices/elasticPools/iotHubTenants/routing/routes/$testnew/Action | Dient zum Testen einer Nachricht mit einer angegebenen Testroute. |
> | Aktion | Microsoft.Devices/elasticPools/iotHubTenants/routingEndpointsHealth/Read | Ruft die Integrität aller Routingendpunkte für eine IotHub-Instanz ab. |
> | Aktion | Microsoft.Devices/elasticPools/iotHubTenants/Write | Erstellt oder aktualisiert die IotHub-Mandantenressource. |
> | Aktion | Microsoft.Devices/ElasticPools/metricDefinitions/read | Ruft die verfügbaren Metriken für den IotHub-Dienst ab. |
> | Aktion | Microsoft.Devices/iotHubs/certificates/Delete | Löscht Zertifikate. |
> | Aktion | Microsoft.Devices/iotHubs/certificates/generateVerificationCode/Action | Generiert einen Prüfcode. |
> | Aktion | Microsoft.Devices/iotHubs/certificates/Read | Ruft das Zertifikat ab. |
> | Aktion | Microsoft.Devices/iotHubs/certificates/verify/Action | Überprüft die Zertifikatsressource. |
> | Aktion | Microsoft.Devices/iotHubs/certificates/Write | Erstellt oder aktualisiert das Zertifikat. |
> | Aktion | Microsoft.Devices/iotHubs/Delete | Dient zum Löschen von IotHub-Ressourcen. |
> | Aktion | Microsoft.Devices/IotHubs/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | Microsoft.Devices/IotHubs/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.Devices/iotHubs/eventGridFilters/Delete | Löscht den Event Grid-Filter. |
> | Aktion | Microsoft.Devices/iotHubs/eventGridFilters/Read | Ruft den Event Grid-Filter ab. |
> | Aktion | Microsoft.Devices/iotHubs/eventGridFilters/Write | Erstellt einen neuen Event Grid-Filter oder aktualisiert einen vorhandenen Event Grid-Filter. |
> | Aktion | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Delete | Dient zum Löschen von EventHub-Consumergruppen. |
> | Aktion | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Read | Dient zum Abrufen von EventHub-Consumergruppen. |
> | Aktion | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Write | Dient zum Erstellen von EventHub-Consumergruppen. |
> | Aktion | Microsoft.Devices/iotHubs/exportDevices/Action | Dient zum Exportieren von Geräten. |
> | Aktion | Microsoft.Devices/iotHubs/importDevices/Action | Dient zum Importieren von Geräten. |
> | Aktion | Microsoft.Devices/iotHubs/iotHubKeys/listkeys/Action | Dient zum Abrufen des IotHub-Schlüssels für den angegebenen Namen. |
> | Aktion | Microsoft.Devices/iotHubs/iotHubStats/Read | Dient zum Abrufen der IotHub-Statistik. |
> | Aktion | Microsoft.Devices/iotHubs/jobs/Read | Dient zum Abrufen von Auftragsdetails, die für eine bestimmte IotHub-Instanz übermittelt wurden. |
> | Aktion | Microsoft.Devices/iotHubs/listkeys/Action | Dient zum Abrufen aller IotHub-Schlüssel. |
> | Aktion | Microsoft.Devices/IotHubs/logDefinitions/read | Ruft die verfügbaren Protokolldefinitionen für den IotHub-Dienst ab. |
> | Aktion | Microsoft.Devices/IotHubs/metricDefinitions/read | Ruft die verfügbaren Metriken für den IotHub-Dienst ab. |
> | Aktion | Microsoft.Devices/iotHubs/quotaMetrics/Read | Dient zum Abrufen von Kontingentmetriken. |
> | Aktion | Microsoft.Devices/iotHubs/Read | Ruft die IotHub-Ressourcen ab. |
> | Aktion | Microsoft.Devices/iotHubs/routing/$testall/Action | Dient zum Testen einer Nachricht mit allen vorhandenen Routen. |
> | Aktion | Microsoft.Devices/iotHubs/routing/$testnew/Action | Dient zum Testen einer Nachricht mit einer angegebenen Testroute. |
> | Aktion | Microsoft.Devices/iotHubs/routingEndpointsHealth/Read | Ruft die Integrität aller Routingendpunkte für eine IotHub-Instanz ab. |
> | Aktion | Microsoft.Devices/iotHubs/skus/Read | Dient zum Abrufen gültiger IotHub-SKUs. |
> | Aktion | Microsoft.Devices/iotHubs/Write | Dient zum Erstellen oder Aktualisieren von IotHub-Ressourcen. |
> | Aktion | Microsoft.Devices/operations/Read | Dient zum Abrufen aller ResourceProvider-Vorgänge. |
> | Aktion | Microsoft.Devices/provisioningServices/certificates/Delete | Löscht Zertifikate. |
> | Aktion | Microsoft.Devices/provisioningServices/certificates/generateVerificationCode/Action | Generiert einen Prüfcode. |
> | Aktion | Microsoft.Devices/provisioningServices/certificates/Read | Ruft das Zertifikat ab. |
> | Aktion | Microsoft.Devices/provisioningServices/certificates/verify/Action | Überprüft die Zertifikatsressource. |
> | Aktion | Microsoft.Devices/provisioningServices/certificates/Write | Erstellt oder aktualisiert das Zertifikat. |
> | Aktion | Microsoft.Devices/provisioningServices/Delete | Löscht die IotDps-Ressource. |
> | Aktion | Microsoft.Devices/provisioningServices/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | Microsoft.Devices/provisioningServices/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.Devices/provisioningServices/listkeys/Action | Ruft alle IotDps-Schlüssel ab. |
> | Aktion | Microsoft.Devices/provisioningServices/logDefinitions/read | Ruft die verfügbaren Protokolldefinitionen für den Bereitstellungsdienst ab. |
> | Aktion | Microsoft.Devices/provisioningServices/metricDefinitions/read | Ruft die verfügbaren Metriken für den Bereitstellungsdienst ab. |
> | Aktion | Microsoft.Devices/provisioningServices/ProvisioningServiceKeys/listkeys/Action | Ruft IotDps-Schlüssel für Schlüsselnamen ab. |
> | Aktion | Microsoft.Devices/provisioningServices/Read | Ruft die IotDps-Ressource ab. |
> | Aktion | Microsoft.Devices/provisioningServices/skus/Read | Ruft gültige IotDps-SKUs ab. |
> | Aktion | Microsoft.Devices/provisioningServices/Write | Erstellt die IotDps-Ressource. |
> | Aktion | Microsoft.Devices/register/action | Dient zum Registrieren des Abonnements für den IotHub-Ressourcenanbieter und ermöglicht die Erstellung von IotHub-Ressourcen. |
> | Aktion | Microsoft.Devices/register/action | Dient zum Registrieren des Abonnements für den IotHub-Ressourcenanbieter und ermöglicht die Erstellung von IotHub-Ressourcen. |
> | Aktion | Microsoft.Devices/usages/Read | Dient zum Abrufen von Details zur Abonnementnutzung für diesen Anbieter. |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.DevTestLab/labCenters/delete | Löscht die Lab-Center. |
> | Aktion | Microsoft.DevTestLab/labCenters/read | Liest die Lab-Center. |
> | Aktion | Microsoft.DevTestLab/labCenters/write | Dient zum Hinzufügen oder Ändern von Lab-Centern. |
> | Aktion | Microsoft.DevTestLab/labs/artifactSources/armTemplates/read | Dient zum Lesen von Azure Resource Manager-Vorlagen. |
> | Aktion | Microsoft.DevTestLab/labs/artifactSources/artifacts/GenerateArmTemplate/action | Generiert eine ARM-Vorlage für das angegebene Artefakt, lädt die erforderlichen Dateien in ein Speicherkonto hoch und überprüft das generierte Artefakt. |
> | Aktion | Microsoft.DevTestLab/labs/artifactSources/artifacts/read | Dient zum Lesen von Artefakten. |
> | Aktion | Microsoft.DevTestLab/labs/artifactSources/delete | Dient zum Löschen von Artefaktquellen. |
> | Aktion | Microsoft.DevTestLab/labs/artifactSources/read | Dient zum Lesen von Artefaktquellen. |
> | Aktion | Microsoft.DevTestLab/labs/artifactSources/write | Dient zum Hinzufügen oder Ändern von Artefaktquellen. |
> | Aktion | Microsoft.DevTestLab/labs/ClaimAnyVm/action | Dient zum Anfordern eines beliebigen anforderbaren virtuellen Computers im Lab. |
> | Aktion | Microsoft.DevTestLab/labs/costs/read | Dient zum Lesen von Kosten. |
> | Aktion | Microsoft.DevTestLab/labs/costs/write | Dient zum Hinzufügen oder Ändern von Kosten. |
> | Aktion | Microsoft.DevTestLab/labs/CreateEnvironment/action | Dient zum Erstellen von virtuellen Computern in einem Lab. |
> | Aktion | Microsoft.DevTestLab/labs/customImages/delete | Dient zum Löschen benutzerdefinierter Images. |
> | Aktion | Microsoft.DevTestLab/labs/customImages/read | Dient zum Lesen benutzerdefinierter Images. |
> | Aktion | Microsoft.DevTestLab/labs/customImages/write | Dient zum Hinzufügen oder Ändern benutzerdefinierter Images. |
> | Aktion | Microsoft.DevTestLab/labs/delete | Dient zum Löschen von Labs. |
> | Aktion | Microsoft.DevTestLab/labs/ExportResourceUsage/action | Exportiert die Labressourcenverwendung in ein Speicherkonto. |
> | Aktion | Microsoft.DevTestLab/labs/formulas/delete | Dient zum Löschen von Formeln. |
> | Aktion | Microsoft.DevTestLab/labs/formulas/read | Dient zum Lesen von Formeln. |
> | Aktion | Microsoft.DevTestLab/labs/formulas/write | Dient zum Hinzufügen oder Ändern von Formeln. |
> | Aktion | Microsoft.DevTestLab/labs/galleryImages/read | Dient zum Lesen von Katalogimages. |
> | Aktion | Microsoft.DevTestLab/labs/GenerateUploadUri/action | Dient zum Generieren eines URI für das Hochladen benutzerdefinierter Datenträgerimages in ein Lab. |
> | Aktion | Microsoft.DevTestLab/labs/ImportVirtualMachine/action | Importiert einen virtuellen Computer in ein anderes Lab. |
> | Aktion | Microsoft.DevTestLab/labs/ListVhds/action | Dient zum Auflisten von Datenträgerimages, die für die Erstellung benutzerdefinierter Images verfügbar sind. |
> | Aktion | Microsoft.DevTestLab/labs/notificationChannels/delete | Dient zum Löschen von Benachrichtigungskanälen. |
> | Aktion | Microsoft.DevTestLab/labs/notificationChannels/Notify/action | Dient zum Senden einer Benachrichtigung an einen angegebenen Kanal. |
> | Aktion | Microsoft.DevTestLab/labs/notificationChannels/read | Dient zum Lesen von Benachrichtigungskanälen. |
> | Aktion | Microsoft.DevTestLab/labs/notificationChannels/write | Dient zum Hinzufügen oder Ändern von Benachrichtigungskanälen. |
> | Aktion | Microsoft.DevTestLab/labs/policySets/EvaluatePolicies/action | Wertet Labrichtlinien aus. |
> | Aktion | Microsoft.DevTestLab/labs/policySets/policies/delete | Dient zum Löschen von Richtlinien. |
> | Aktion | Microsoft.DevTestLab/labs/policySets/policies/read | Dient zum Lesen von Richtlinien. |
> | Aktion | Microsoft.DevTestLab/labs/policySets/policies/write | Dient zum Hinzufügen oder Ändern von Richtlinien. |
> | Aktion | Microsoft.DevTestLab/labs/read | Dient zum Lesen von Labs. |
> | Aktion | Microsoft.DevTestLab/labs/schedules/delete | Dient zum Löschen von Zeitplänen. |
> | Aktion | Microsoft.DevTestLab/labs/schedules/Execute/action | Dient zum Ausführen eines Zeitplans. |
> | Aktion | Microsoft.DevTestLab/labs/schedules/ListApplicable/action | Listet alle anwendbaren Zeitpläne auf. |
> | Aktion | Microsoft.DevTestLab/labs/schedules/read | Dient zum Lesen von Zeitplänen. |
> | Aktion | Microsoft.DevTestLab/labs/schedules/write | Dient zum Hinzufügen oder Ändern von Zeitplänen. |
> | Aktion | Microsoft.DevTestLab/labs/serviceRunners/delete | Dient zum Löschen von Dienstausführern. |
> | Aktion | Microsoft.DevTestLab/labs/serviceRunners/read | Dient zum Lesen von Dienstausführern. |
> | Aktion | Microsoft.DevTestLab/labs/serviceRunners/write | Dient zum Hinzufügen oder Ändern von Dienstausführern. |
> | Aktion | Microsoft.DevTestLab/labs/users/delete | Dient zum Löschen von Benutzerprofilen. |
> | Aktion | Microsoft.DevTestLab/labs/users/disks/Attach/action | Dient zum Anfügen des Datenträgers an den virtuellen Computer sowie zum Erstellen der Datenträgerlease für den virtuellen Computer. |
> | Aktion | Microsoft.DevTestLab/labs/users/disks/delete | Dient zum Löschen von Datenträgern. |
> | Aktion | Microsoft.DevTestLab/labs/users/disks/Detach/action | Dient zum Trennen des an den virtuellen Computer angefügten Datenträgers sowie zum Beenden der Datenträgerlease für den virtuellen Computer. |
> | Aktion | Microsoft.DevTestLab/labs/users/disks/read | Dient zum Lesen von Datenträgern. |
> | Aktion | Microsoft.DevTestLab/labs/users/disks/write | Dient zum Hinzufügen oder Ändern von Datenträgern. |
> | Aktion | Microsoft.DevTestLab/labs/users/environments/delete | Dient zum Löschen von Umgebungen. |
> | Aktion | Microsoft.DevTestLab/labs/users/environments/read | Dient zum Lesen von Umgebungen. |
> | Aktion | Microsoft.DevTestLab/labs/users/environments/write | Dient zum Hinzufügen oder Ändern von Umgebungen. |
> | Aktion | Microsoft.DevTestLab/labs/users/read | Dient zum Lesen von Benutzerprofilen. |
> | Aktion | Microsoft.DevTestLab/labs/users/secrets/delete | Dient zum Löschen von Geheimnissen. |
> | Aktion | Microsoft.DevTestLab/labs/users/secrets/read | Dient zum Lesen von Geheimnissen. |
> | Aktion | Microsoft.DevTestLab/labs/users/secrets/write | Dient zum Hinzufügen oder Ändern von Geheimnissen. |
> | Aktion | Microsoft.DevTestLab/labs/users/serviceFabrics/delete | Löscht Service Fabrics. |
> | Aktion | Microsoft.DevTestLab/labs/users/serviceFabrics/ListApplicableSchedules/action | Listet alle anwendbaren Zeitpläne auf. |
> | Aktion | Microsoft.DevTestLab/labs/users/serviceFabrics/read | Liest Service Fabrics. |
> | Aktion | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/delete | Dient zum Löschen von Zeitplänen. |
> | Aktion | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/Execute/action | Dient zum Ausführen eines Zeitplans. |
> | Aktion | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/read | Dient zum Lesen von Zeitplänen. |
> | Aktion | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/write | Dient zum Hinzufügen oder Ändern von Zeitplänen. |
> | Aktion | Microsoft.DevTestLab/labs/users/serviceFabrics/Start/action | Startet ein Service Fabric. |
> | Aktion | Microsoft.DevTestLab/labs/users/serviceFabrics/Stop/action | Beendet ein Service Fabric. |
> | Aktion | Microsoft.DevTestLab/labs/users/serviceFabrics/write | Dient zum Hinzufügen oder Ändern von Service Fabrics. |
> | Aktion | Microsoft.DevTestLab/labs/users/write | Dient zum Hinzufügen oder Ändern von Benutzerprofilen. |
> | Aktion | Microsoft.DevTestLab/labs/virtualMachines/AddDataDisk/action | Dient zum Anfügen eines neuen oder vorhandenen Datenträgers an virtuelle Computer. |
> | Aktion | Microsoft.DevTestLab/labs/virtualMachines/ApplyArtifacts/action | Dient zum Anwenden von Artefakten auf virtuelle Computer. |
> | Aktion | Microsoft.DevTestLab/labs/virtualMachines/Claim/action | Dient dazu, einen vorhandenen virtuellen Computer in Besitz zu nehmen. |
> | Aktion | Microsoft.DevTestLab/labs/virtualMachines/delete | Dient zum Löschen virtueller Computer. |
> | Aktion | Microsoft.DevTestLab/labs/virtualMachines/DetachDataDisk/action | Dient zum Trennen des angegebenen Datenträgers vom virtuellen Computer. |
> | Aktion | Microsoft.DevTestLab/labs/virtualMachines/ListApplicableSchedules/action | Listet alle anwendbaren Zeitpläne auf. |
> | Aktion | Microsoft.DevTestLab/labs/virtualMachines/read | Dient zum Lesen virtueller Computer. |
> | Aktion | Microsoft.DevTestLab/labs/virtualMachines/Redeploy/action | Stellt einen virtuellen Computer erneut bereit. |
> | Aktion | Microsoft.DevTestLab/labs/virtualMachines/Resize/action | Ändert die Größe eines virtuellen Computers. |
> | Aktion | Microsoft.DevTestLab/labs/virtualMachines/Restart/action | Startet einen virtuellen Computer. |
> | Aktion | Microsoft.DevTestLab/labs/virtualMachines/schedules/delete | Dient zum Löschen von Zeitplänen. |
> | Aktion | Microsoft.DevTestLab/labs/virtualMachines/schedules/Execute/action | Dient zum Ausführen eines Zeitplans. |
> | Aktion | Microsoft.DevTestLab/labs/virtualMachines/schedules/read | Dient zum Lesen von Zeitplänen. |
> | Aktion | Microsoft.DevTestLab/labs/virtualMachines/schedules/write | Dient zum Hinzufügen oder Ändern von Zeitplänen. |
> | Aktion | Microsoft.DevTestLab/labs/virtualMachines/Start/action | Dient zum Starten eines virtuellen Computers. |
> | Aktion | Microsoft.DevTestLab/labs/virtualMachines/Stop/action | Dient zum Beenden eines virtuellen Computers. |
> | Aktion | Microsoft.DevTestLab/labs/virtualMachines/TransferDisks/action | Hiermit übertragen Sie den Besitz der VM-Datenträger an sich selbst. |
> | Aktion | Microsoft.DevTestLab/labs/virtualMachines/UnClaim/action | Hiermit geben Sie den Besitz eines vorhandenen virtuellen Computers frei. |
> | Aktion | Microsoft.DevTestLab/labs/virtualMachines/write | Dient zum Hinzufügen oder Ändern virtueller Computer. |
> | Aktion | Microsoft.DevTestLab/labs/virtualNetworks/delete | Dient zum Löschen virtueller Netzwerke. |
> | Aktion | Microsoft.DevTestLab/labs/virtualNetworks/read | Dient zum Lesen virtueller Netzwerke. |
> | Aktion | Microsoft.DevTestLab/labs/virtualNetworks/write | Dient zum Hinzufügen oder Ändern virtueller Netzwerke. |
> | Aktion | Microsoft.DevTestLab/labs/vmPools/delete | Löscht Pools für virtuelle Computer. |
> | Aktion | Microsoft.DevTestLab/labs/vmPools/read | Liest Pools für virtuelle Computer. |
> | Aktion | Microsoft.DevTestLab/labs/vmPools/write | Fügt Pools für virtuelle Computer hinzu oder ändert diese. |
> | Aktion | Microsoft.DevTestLab/labs/write | Dient zum Hinzufügen oder Ändern von Labs. |
> | Aktion | Microsoft.DevTestLab/locations/operations/read | Dient zum Lesen von Vorgängen. |
> | Aktion | Microsoft.DevTestLab/register/action | Registriert das Abonnement. |
> | Aktion | Microsoft.DevTestLab/schedules/delete | Dient zum Löschen von Zeitplänen. |
> | Aktion | Microsoft.DevTestLab/schedules/Execute/action | Dient zum Ausführen eines Zeitplans. |
> | Aktion | Microsoft.DevTestLab/schedules/read | Dient zum Lesen von Zeitplänen. |
> | Aktion | Microsoft.DevTestLab/schedules/Retarget/action | Aktualisiert die Zielressourcen-ID eines Zeitplans. |
> | Aktion | Microsoft.DevTestLab/schedules/write | Dient zum Hinzufügen oder Ändern von Zeitplänen. |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.DocumentDB/databaseAccountNames/read | Prüft die Verfügbarkeit des Namens. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/changeResourceGroup/action | Dient zum Ändern der Ressourcengruppe eines Datenbankkontos. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/databases/collections/metricDefinitions/read | Liest die Sammlungsmetrikdefinitionen. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/databases/collections/metrics/read | Liest die Sammlungsmetriken. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitionKeyRangeId/metrics/read | Liest Metriken auf Partitionsschlüsselebene eines Datenbankkontos. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/metrics/read | Liest Metriken auf Partitionsebene eines Datenbankkontos. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/read | Liest die Partitionen eines Datenbankkontos in einer Collection. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/usages/read | Liest Verwendungen auf Partitionsebene eines Datenbankkontos. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/databases/collections/usages/read | Liest Informationen zur Sammlungsverwendung. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/databases/metricDefinitions/read | Liest die Datenbankmetrikdefinitionen. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/databases/metrics/read | Liest die Datenbankmetriken. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/databases/usages/read | Liest Informationen zur Datenbankverwendung. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/delete | Löscht die Datenbankkonten. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/failoverPriorityChange/action | Dient zum Ändern der Failoverprioritäten von Regionen eines Datenbankkontos. Wird für manuelle Failovervorgänge verwendet. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/listConnectionStrings/action | Dient zum Abrufen der Verbindungszeichenfolgen für ein Datenbankkonto. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/listKeys/action | Dient zum Auflisten von Schlüsseln eines Datenbankkontos. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/metricDefinitions/read | Liest die Metrikdefinitionen für Datenbankkonten. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/metrics/read | Liest die Datenbankkontometriken. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/operationResults/read | Liest den Status eines asynchronen Vorgangs. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/percentile/metrics/read | Liest Latenzmetriken. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/percentile/read | Liest Perzentile von Replikationslatenzen. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/percentile/sourceRegion/targetRegion/metrics/read | Liest die Latenzmetriken für eine bestimmte Quell- und Zielregion. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/percentile/targetRegion/metrics/read | Liest die Latenzmetriken für eine bestimmte Zielregion. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/providers/Microsoft.Insights/logDefinitions/read | Ruft die verfügbaren Protokollkategorien für ein Datenbankkonto ab. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für ein Datenbankkonto ab. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/read | Liest ein Datenbankkonto. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | Liest die schreibgeschützten Schlüssel für Datenbankkonten. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/readonlykeys/read | Liest die schreibgeschützten Schlüssel für Datenbankkonten. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/regenerateKey/action | Dient zum Rotieren von Schlüsseln eines Datenbankkontos. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/metrics/read | Liest die Metriken von regionalen Collections. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitionKeyRangeId/metrics/read | Liest regionale Metriken auf Partitionsschlüsselebene eines Datenbankkontos. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitions/metrics/read | Liest regionale Metriken auf Partitionsebene eines Datenbankkontos. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitions/read | Liest die Partitionen eines regionalen Datenbankkontos in einer Collection. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/region/metrics/read | Liest die Region und die Datenbankkontometriken. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/usages/read | Liest Informationen zur Datenbankkontoverwendung. |
> | Aktion | Microsoft.DocumentDB/databaseAccounts/write | Dient zum Aktualisieren von Datenbankkonten. |
> | Aktion | Microsoft.DocumentDB/locations/deleteVirtualNetworkOrSubnets/action | Benachrichtigt Microsoft.DocumentDB darüber, dass das virtuelle Netzwerk oder Subnetz gelöscht wird. |
> | Aktion | Microsoft.DocumentDB/locations/deleteVirtualNetworkOrSubnets/operationResults/read | Liest den Status des asynchronen Vorgangs „deleteVirtualNetworkOrSubnets“. |
> | Aktion | Microsoft.DocumentDB/operationResults/read | Liest den Status eines asynchronen Vorgangs. |
> | Aktion | Microsoft.DocumentDB/operations/read | Liest verfügbare Vorgänge für Microsoft DocumentDB  |
> | Aktion | Microsoft.DocumentDB/register/action |  Dient zum Registrieren des Microsoft DocumentDB-Ressourcenanbieters für das Abonnement. |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.DomainRegistration/checkDomainAvailability/Action | Dient zum Prüfen, ob eine Domäne erworben werden kann. |
> | Aktion | Microsoft.DomainRegistration/domains/Delete | Dient zum Löschen einer vorhandenen Domäne. |
> | Aktion | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Delete | Löscht die Besitz-ID. |
> | Aktion | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Read | Listet die Besitz-IDs auf. |
> | Aktion | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Read | Ruft die Besitz-ID ab. |
> | Aktion | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Write | Erstellt oder aktualisiert eine ID. |
> | Aktion | Microsoft.DomainRegistration/domains/operationresults/Read | Dient zum Abrufen eines Domänenvorgangs. |
> | Aktion | Microsoft.DomainRegistration/domains/Read | Dient zum Abrufen der Domänenliste. |
> | Aktion | Microsoft.DomainRegistration/domains/Read | Ruft die Domäne ab. |
> | Aktion | Microsoft.DomainRegistration/domains/renew/Action | Dient zum Verlängern einer vorhandenen Domäne. |
> | Aktion | Microsoft.DomainRegistration/domains/Write | Dient zum Hinzufügen einer neuen Domäne oder zum Aktualisieren einer bereits vorhandenen. |
> | Aktion | Microsoft.DomainRegistration/generateSsoRequest/Action | Dient zum Generieren einer Anforderung für die Anmeldung beim Domänensteuerungszentrum. |
> | Aktion | Microsoft.DomainRegistration/listDomainRecommendations/Action | Dient zum Abrufen der Liste mit Domänenempfehlungen auf der Grundlage von Schlüsselwörtern. |
> | Aktion | Microsoft.DomainRegistration/operations/Read | Dient zum Auflisten aller Vorgänge über die App Service-Domänenregistrierung. |
> | Aktion | Microsoft.DomainRegistration/register/action | Dient zum Registrieren des Microsoft-Domänenressourcenanbieters für das Abonnement. |
> | Aktion | Microsoft.DomainRegistration/topLevelDomains/listAgreements/Action | Listet die Vereinbarungsaktion auf. |
> | Aktion | Microsoft.DomainRegistration/topLevelDomains/Read | Ruft die Domänen der obersten Domäne ab. |
> | Aktion | Microsoft.DomainRegistration/topLevelDomains/Read | Ruft die Domäne der obersten Domäne ab. |
> | Aktion | Microsoft.DomainRegistration/validateDomainRegistrationInformation/Action | Dient zum Überprüfen eines Domänenerwerbsobjekts, ohne es zu übermitteln. |

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.DynamicsLcs/lcsprojects/clouddeployments/read | Dient zum Anzeigen von Microsoft Dynamics AX 2012 R3 Evaluation-Bereitstellungen in einem Microsoft Dynamics Lifecycle Services-Projekt, die zu einem Benutzer gehören. |
> | Aktion | Microsoft.DynamicsLcs/lcsprojects/clouddeployments/write | Dient zum Erstellen von Microsoft Dynamics AX 2012 R3 Evaluation-Bereitstellungen in einem Microsoft Dynamics Lifecycle Services-Projekt, die zu einem Benutzer gehören. Bereitstellungen können über das Azure-Verwaltungsportal verwaltet werden. |
> | Aktion | Microsoft.DynamicsLcs/lcsprojects/connectors/read | Dient zum Lesen von Connectors, die zu einem Microsoft Dynamics Lifecycle Services-Projekt gehören. |
> | Aktion | Microsoft.DynamicsLcs/lcsprojects/connectors/write | Dient zum Erstellen und Aktualisieren von Connectors, die zu einem Microsoft Dynamics Lifecycle Services-Projekt gehören. |
> | Aktion | Microsoft.DynamicsLcs/lcsprojects/delete | Dient zum Löschen von Microsoft Dynamics Lifecycle Services-Projekten, die zum Benutzer gehören. |
> | Aktion | Microsoft.DynamicsLcs/lcsprojects/read | Dient zum Anzeigen von Microsoft Dynamics Lifecycle Services-Projekten, die zu einem Benutzer gehören. |
> | Aktion | Microsoft.DynamicsLcs/lcsprojects/write | Dient zum Erstellen oder Aktualisieren von Microsoft Dynamics Lifecycle Services-Projekten, die zum Benutzer gehören. Nur die Eigenschaften für Name und Beschreibung können aktualisiert werden. Abonnement und Standort für das Projekt können nach der Erstellung nicht mehr aktualisiert werden. |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.EventGrid/eventSubscriptions/delete | Löscht eventSubscription. |
> | Aktion | Microsoft.EventGrid/eventSubscriptions/getFullUrl/action | Ruft die vollständige URL für das Ereignisabonnement ab. |
> | Aktion | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für Ereignisabonnements ab. |
> | Aktion | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für Ereignisabonnements. |
> | Aktion | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für eventSubscriptions ab. |
> | Aktion | Microsoft.EventGrid/eventSubscriptions/read | Liest eventSubscription. |
> | Aktion | Microsoft.EventGrid/eventSubscriptions/write | Erstellt oder aktualisiert eventSubscription. |
> | Aktion | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Themen ab. |
> | Aktion | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für Themen. |
> | Aktion | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für Themen ab. |
> | Aktion | Microsoft.EventGrid/register/action | Registriert das eventSubscription für den EventGrid-Ressourcenanbieter und ermöglicht die Erstellung von Event Grid-Ressourcen. |
> | Aktion | Microsoft.EventGrid/topics/delete | Löschen eines Themas |
> | Aktion | Microsoft.EventGrid/topics/listKeys/action | Listet Schlüssel für das Thema auf. |
> | Aktion | Microsoft.EventGrid/topics/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Themen ab. |
> | Aktion | Microsoft.EventGrid/topics/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für Themen. |
> | Aktion | Microsoft.EventGrid/topics/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für Themen ab. |
> | Aktion | Microsoft.EventGrid/topics/read | Liest ein Thema. |
> | Aktion | Microsoft.EventGrid/topics/regenerateKey/action | Generiert erneut einen Schlüssel für das Thema. |
> | Aktion | Microsoft.EventGrid/topics/write | Erstellt oder aktualisiert ein Thema. |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.EventHub/checkNameAvailability/action | Prüft die Verfügbarkeit des Namespaces unter dem angegebenen Abonnement. |
> | Aktion | Microsoft.EventHub/checkNamespaceAvailability/action | Prüft die Verfügbarkeit des Namespaces unter dem angegebenen Abonnement. Diese API ist veraltet. Verwenden Sie stattdessen CheckNameAvailabiltiy. |
> | Aktion | Microsoft.EventHub/clusters/providers/Microsoft.Insights/metricDefinitions/read | Ruft die Liste der Ressourcenbeschreibungen für Clustermetriken ab. |
> | Aktion | Microsoft.EventHub/clusters/read | Ruft die Beschreibung der Clusterressource ab. |
> | Aktion | Microsoft.EventHub/clusters/write | Ruft die Beschreibung der Clusterressource ab. |
> | Aktion | Microsoft.EventHub/namespaces/authorizationRules/action | Dient zum Aktualisieren von Namespace-Autorisierungsregeln. Diese API ist veraltet. Verwenden Sie stattdessen einen PUT-Aufruf, um die Namespace-Autorisierungsregel zu aktualisieren. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. |
> | Aktion | Microsoft.EventHub/namespaces/authorizationRules/delete | Dient zum Löschen von Namespace-Autorisierungsregeln. Die standardmäßige Namespace-Autorisierungsregel kann nicht gelöscht werden.  |
> | Aktion | Microsoft.EventHub/namespaces/authorizationRules/listkeys/action | Dient zum Abrufen der Verbindungszeichenfolge für den Namespace. |
> | Aktion | Microsoft.EventHub/namespaces/authorizationRules/read | Dient zum Abrufen der Liste mit Beschreibungen für Namespace-Autorisierungsregeln. |
> | Aktion | Microsoft.EventHub/namespaces/authorizationRules/regenerateKeys/action | Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource. |
> | Aktion | Microsoft.EventHub/namespaces/authorizationRules/write | Dient zum Erstellen von Autorisierungsregeln auf Namespaceebene und zum Aktualisieren der dazugehörigen Eigenschaften. Aktualisiert werden können die Zugriffsrechte für Autorisierungsregeln sowie der primäre und der sekundäre Schlüssel. |
> | Aktion | Microsoft.EventHub/namespaces/Delete | Dient zum Löschen von Namespaceressourcen. |
> | Aktion | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Ruft die Schlüssel der Autorisierungsregeln für den primären Namespace für die Notfallwiederherstellung. |
> | Aktion | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/authorizationRules/read | Ruft die Autorisierungsregeln des primären Namespace für die Notfallwiederherstellung ab. |
> | Aktion | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/breakPairing/action | Hiermit wird die Notfallwiederherstellung deaktiviert und die Replikation von Änderungen vom primären Namespace zu sekundären Namespaces beendet. |
> | Aktion | Microsoft.EventHub/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Prüft die Verfügbarkeit des Namespacealias unter dem angegebenen Abonnement. |
> | Aktion | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/delete | Hiermit wird die dem Namespace zugeordnete Notfallwiederherstellungskonfiguration gelöscht. Dieser Vorgang kann nur über den primären Namespace aufgerufen werden. |
> | Aktion | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/failover/action | Hiermit wird ein Failover bei der georedundanten Notfallwiederherstellung ausgelöst und der Namespacealias neu konfiguriert, um auf den sekundären Namespace zu zeigen. |
> | Aktion | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/read | Hiermit wird die dem Namespace zugeordnete Notfallwiederherstellungskonfiguration abgerufen. |
> | Aktion | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/write | Hiermit wird die dem Namespace zugeordnete Notfallwiederherstellungskonfiguration erstellt oder aktualisiert. |
> | Aktion | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/action | Vorgang zum Aktualisieren von EventHub. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. Autorisierungsregeln. Verwenden Sie einen PUT-Aufruf, um die Autorisierungsregel zu aktualisieren. |
> | Aktion | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/delete | Vorgang zum Löschen von EventHub-Autorisierungsregeln. |
> | Aktion | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/listkeys/action | Dient zum Abrufen der EventHub-Verbindungszeichenfolge. |
> | Aktion | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/read |  Dient zum Abrufen der Liste mit EventHub-Autorisierungsregeln. |
> | Aktion | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/regenerateKeys/action | Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource. |
> | Aktion | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/write | Dient zum Erstellen von EventHub-Autorisierungsregeln und zum Aktualisieren der dazugehörigen Eigenschaften. Die Zugriffsrechte für Autorisierungsregeln können aktualisiert werden. |
> | Aktion | Microsoft.EventHub/namespaces/eventHubs/consumergroups/Delete | Vorgang zum Löschen von ConsumerGroup-Ressourcen. |
> | Aktion | Microsoft.EventHub/namespaces/eventHubs/consumergroups/read | Dient zum Abrufen einer Liste mit ConsumerGroup-Ressourcenbeschreibungen. |
> | Aktion | Microsoft.EventHub/namespaces/eventHubs/consumergroups/write | Dient zum Erstellen oder Aktualisieren von ConsumerGroup-Eigenschaften. |
> | Aktion | Microsoft.EventHub/namespaces/eventhubs/Delete | Vorgang zum Löschen von EventHub-Ressourcen. |
> | Aktion | Microsoft.EventHub/namespaces/eventhubs/read | Dient zum Abrufen einer Liste mit EventHub-Ressourcenbeschreibungen. |
> | Aktion | Microsoft.EventHub/namespaces/eventhubs/write | Dient zum Erstellen oder Aktualisieren von EventHub-Eigenschaften. |
> | Aktion | Microsoft.EventHub/namespaces/messagingPlan/read | Hiermit wird der Messagingplan für einen Namespace abgerufen.<br>Diese API ist veraltet.<br>Über die MessagingPlan-Ressource verfügbar gemachte Eigenschaften werden in höheren API-Versionen in die (übergeordnete) Namespaceressource verschoben.<br>Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. |
> | Aktion | Microsoft.EventHub/namespaces/messagingPlan/write | Hiermit wird der Messagingplan für einen Namespace aktualisiert.<br>Diese API ist veraltet.<br>Über die MessagingPlan-Ressource verfügbar gemachte Eigenschaften werden in höheren API-Versionen in die (übergeordnete) Namespaceressource verschoben.<br>Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. |
> | Aktion | Microsoft.EventHub/namespaces/operationresults/read | Hiermit wird der Status des Namespacevorgangs abgerufen. |
> | Aktion | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/diagnosticSettings/read | Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacediagnoseeinstellungen. |
> | Aktion | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/diagnosticSettings/write | Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacediagnoseeinstellungen. |
> | Aktion | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/logDefinitions/read | Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespaceprotokolle. |
> | Aktion | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacemetriken. |
> | Aktion | Microsoft.EventHub/namespaces/read | Dient zum Abrufen der Liste mit Beschreibungen für Namespaceressourcen. |
> | Aktion | Microsoft.EventHub/namespaces/removeAcsNamepsace/action | Entfernt den ACS-Namespace. |
> | Aktion | Microsoft.EventHub/namespaces/write | Dient zum Erstellen einer Namespaceressource und zum Aktualisieren ihrer Eigenschaften. Die Eigenschaften für Markierungen und Kapazität des Namespace können aktualisiert werden. |
> | Aktion | Microsoft.EventHub/operations/read | Dient zum Abrufen von Vorgängen. |
> | Aktion | Microsoft.EventHub/register/action | Registriert das Abonnement für den EventHub-Ressourcenanbieter und ermöglicht die Erstellung von EventHub-Ressourcen. |
> | Aktion | Microsoft.EventHub/sku/read | Dient zum Abrufen einer Liste mit SKU-Ressourcenbeschreibungen. |
> | Aktion | Microsoft.EventHub/sku/regions/read | Dient zum Abrufen einer Liste mit SkuRegions-Ressourcenbeschreibungen. |
> | Aktion | Microsoft.EventHub/unregister/action | Registriert den EventHub-Ressourcenanbieter. |

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Features/features/read | Ruft die Features eines Abonnements ab. |
> | Aktion | Microsoft.Features/operations/read | Ruft die Liste der Vorgänge ab. |
> | Aktion | Microsoft.Features/providers/features/read | Ruft das Feature eines Abonnements in einem angegebenen Ressourcenanbieter ab. |
> | Aktion | Microsoft.Features/providers/features/register/action | Registriert das Feature für ein Abonnement in einem angegebenen Ressourcenanbieter. |
> | Aktion | Microsoft.Features/providers/features/unregister/action | Hiermit wird die Registrierung des Features für ein Abonnement in einem vorgegebenen Ressourcenanbieter aufgehoben. |
> | Aktion | Microsoft.Features/register/action | Registriert das Feature eines Abonnements. |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.HDInsight/clusters/changerdpsetting/action | Dient zum Ändern der RDP-Einstellung für HDInsight-Cluster. |
> | Aktion | Microsoft.HDInsight/clusters/configurations/action | Dient zum Aktualisieren der Konfiguration von HDInsight-Clustern. |
> | Aktion | Microsoft.HDInsight/clusters/configurations/read | Dient zum Abrufen der Konfigurationen von HDInsight-Clustern. |
> | Aktion | Microsoft.HDInsight/clusters/delete | Dient zum Löschen eines HDInsight-Clusters. |
> | Aktion | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für den HDInsight-Cluster der Ressource ab. |
> | Aktion | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für den HDInsight-Cluster der Ressource. |
> | Aktion | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für den HDInsight-Cluster ab. |
> | Aktion | Microsoft.HDInsight/clusters/read | Dient zum Abrufen von Details zu HDInsight-Clustern. |
> | Aktion | Microsoft.HDInsight/clusters/roles/resize/action | Dient zum Anpassen der Größe eines HDInsight-Clusters. |
> | Aktion | Microsoft.HDInsight/clusters/write | Dient zum Erstellen oder Aktualisieren von HDInsight-Clustern. |
> | Aktion | Microsoft.HDInsight/locations/capabilities/read | Dient zum Abrufen von Abonnementfunktionen. |
> | Aktion | Microsoft.HDInsight/locations/checkNameAvailability/read | Dient zum Prüfen der Verfügbarkeit des Namens. |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.ImportExport/jobs/delete | Löscht einen vorhandenen Auftrag. |
> | Aktion | Microsoft.ImportExport/jobs/listBitLockerKeys/action | Ruft die BitLocker-Schlüssel für den angegebenen Auftrag ab. |
> | Aktion | Microsoft.ImportExport/jobs/read | Ruft die Eigenschaften für den angegebenen Auftrag ab oder gibt die Auftragsliste zurück. |
> | Aktion | Microsoft.ImportExport/jobs/write | Erstellt einen Auftrag mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Tags für den angegebenen Auftrag. |
> | Aktion | Microsoft.ImportExport/locations/read | Ruft die Eigenschaften für den angegebenen Standort ab oder gibt die Standortliste zurück. |
> | Aktion | Microsoft.ImportExport/register/action | Registriert das Abonnement für den Import/Export-Ressourcenanbieter und ermöglicht die Erstellung von Import/Exportaufträgen. |

## <a name="microsoftinsights"></a>Microsoft.Insights

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Insights/ActionGroups/Delete | Hiermit wird eine Aktionsgruppe gelöscht. |
> | Aktion | Microsoft.Insights/ActionGroups/Read | Hiermit wird eine Aktionsgruppe gelesen. |
> | Aktion | Microsoft.Insights/ActionGroups/Write | Hiermit wird eine Aktionsgruppe geschrieben. |
> | Aktion | Microsoft.Insights/ActivityLogAlerts/Activated/Action | Aktivitätsprotokollwarnung ausgelöst |
> | Aktion | Microsoft.Insights/ActivityLogAlerts/Delete | Hiermit wird eine Aktivitätsprotokollwarnung gelöscht. |
> | Aktion | Microsoft.Insights/ActivityLogAlerts/Read | Hiermit wird eine Aktivitätsprotokollwarnung gelesen. |
> | Aktion | Microsoft.Insights/ActivityLogAlerts/Write | Hiermit wird eine Aktivitätsprotokollwarnung gelesen. |
> | Aktion | Microsoft.Insights/AlertRules/Activated/Action | Warnungsregel aktiviert |
> | Aktion | Microsoft.Insights/AlertRules/Delete | Dient zum Löschen einer Warnungsregelkonfiguration. |
> | Aktion | Microsoft.Insights/AlertRules/Incidents/Read | Dient zum Lesen einer Warnungsregel-Incidentkonfiguration. |
> | Aktion | Microsoft.Insights/AlertRules/Read | Dient zum Lesen einer Warnungsregelkonfiguration. |
> | Aktion | Microsoft.Insights/AlertRules/Resolved/Action | Warnungsregel aufgelöst |
> | Aktion | Microsoft.Insights/AlertRules/Throttled/Action | Warnungsregel gedrosselt |
> | Aktion | Microsoft.Insights/AlertRules/Write | Dient zum Schreiben in eine Warnungsregelkonfiguration. |
> | Aktion | Microsoft.Insights/AutoscaleSettings/Delete | Dient zum Löschen einer Autoskalierungseinstellungskonfiguration. |
> | Aktion | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/MetricDefinitions/Read | Dient zum Lesen von Metrikdefinitionen. |
> | Aktion | Microsoft.Insights/AutoscaleSettings/Read | Dient zum Lesen einer Autoskalierungseinstellungskonfiguration. |
> | Aktion | Microsoft.Insights/AutoscaleSettings/Scaledown/Action | Automatisches zentrales Herunterskalieren |
> | Aktion | Microsoft.Insights/AutoscaleSettings/Scaleup/Action | Automatisches zentrales Hochskalieren |
> | Aktion | Microsoft.Insights/AutoscaleSettings/Write | Dient zum Schreiben in eine Autoskalierungseinstellungskonfiguration. |
> | Aktion | Microsoft.Insights/Components/AnalyticsItems/Delete | Hiermit wird ein Application Insights-Analyseelement gelöscht. |
> | Aktion | Microsoft.Insights/Components/AnalyticsItems/Read | Hiermit wird ein Application Insights-Analyseelement gelesen. |
> | Aktion | Microsoft.Insights/Components/AnalyticsItems/Write | Hiermit wird ein Application Insights-Analyseelement geschrieben. |
> | Aktion | Microsoft.Insights/Components/AnalyticsTables/Action | Application Insights-Analysetabellenaktion |
> | Aktion | Microsoft.Insights/Components/AnalyticsTables/Delete | Hiermit wird ein Application Insights-Analysetabellenschema gelöscht. |
> | Aktion | Microsoft.Insights/Components/AnalyticsTables/Read | Hiermit wird ein Application Insights-Analysetabellenschema gelesen. |
> | Aktion | Microsoft.Insights/Components/AnalyticsTables/Write | Hiermit wird ein Application Insights-Analysetabellenschema geschrieben. |
> | Aktion | Microsoft.Insights/Components/Annotations/Delete | Hiermit wird eine Application Insights-Anmerkung gelöscht. |
> | Aktion | Microsoft.Insights/Components/Annotations/Read | Hiermit wird eine Application Insights-Anmerkung gelesen. |
> | Aktion | Microsoft.Insights/Components/Annotations/Write | Hiermit wird eine Application Insights-Anmerkung geschrieben. |
> | Aktion | Microsoft.Insights/Components/Api/Read | Hiermit wird die Daten-API für Application Insights-Komponenten gelesen. |
> | Aktion | Microsoft.Insights/Components/ApiKeys/Action | Hiermit wird ein Application Insights-API-Schlüssel generiert. |
> | Aktion | Microsoft.Insights/Components/ApiKeys/Delete | Hiermit wird ein Application Insights-API-Schlüssel gelöscht. |
> | Aktion | Microsoft.Insights/Components/ApiKeys/Read | Hiermit wird ein Application Insights-API-Schlüssel gelesen. |
> | Aktion | Microsoft.Insights/Components/BillingPlanForComponent/Read | Hiermit wird ein Abrechnungsplan für eine Application Insights-Komponente gelesen. |
> | Aktion | Microsoft.Insights/Components/CurrentBillingFeatures/Read | Hiermit werden die aktuellen Abrechnungsfeatures für eine Application Insights-Komponente gelesen. |
> | Aktion | Microsoft.Insights/Components/CurrentBillingFeatures/Write | Hiermit werden die aktuellen Abrechnungsfeatures für eine Application Insights-Komponente geschrieben. |
> | Aktion | Microsoft.Insights/Components/DefaultWorkItemConfig/Read | Hiermit wird die standardmäßige ALM-Integrationskonfiguration von Application Insights gelesen. |
> | Aktion | Microsoft.Insights/Components/Delete | Hiermit wird die Konfiguration der Application Insights-Komponente gelöscht. |
> | Aktion | Microsoft.Insights/Components/ExportConfiguration/Action | Application Insights-Exporteinstellungsaktion |
> | Aktion | Microsoft.Insights/Components/ExportConfiguration/Delete | Hiermit werden die Application Insights-Exporteinstellungen gelöscht. |
> | Aktion | Microsoft.Insights/Components/ExportConfiguration/Read | Hiermit werden die Application Insights-Exporteinstellungen gelesen. |
> | Aktion | Microsoft.Insights/Components/ExportConfiguration/Write | Hiermit werden die Application Insights-Exporteinstellungen geschrieben. |
> | Aktion | Microsoft.Insights/Components/ExtendQueries/Read | Hiermit werden die erweiterten Abfrageergebnisse der Application Insights-Komponenten gelesen. |
> | Aktion | Microsoft.Insights/Components/Favorites/Delete | Hiermit wird ein Application Insights-Favorit gelöscht. |
> | Aktion | Microsoft.Insights/Components/Favorites/Read | Hiermit wird ein Application Insights-Favorit gelesen. |
> | Aktion | Microsoft.Insights/Components/Favorites/Write | Hiermit wird ein Application Insights-Favorit geschrieben. |
> | Aktion | Microsoft.Insights/Components/FeatureCapabilities/Read | Hiermit werden die Features der Application Insights-Komponente gelesen. |
> | Aktion | Microsoft.Insights/Components/GetAvailableBillingFeatures/Read | Hiermit werden die verfügbaren Abrechnungsfeatures für eine Application Insights-Komponente gelesen. |
> | Aktion | Microsoft.Insights/Components/GetToken/Read | Hiermit wird ein Token einer Application Insights-Komponente gelesen. |
> | Aktion | Microsoft.Insights/Components/MetricDefinitions/Read | Hiermit werden die Metrikdefinitionen für Application Insights-Komponenten gelesen. |
> | Aktion | Microsoft.Insights/Components/Metrics/Read | Hiermit werden die Metriken für Application Insights-Komponenten gelesen. |
> | Aktion | Microsoft.Insights/Components/Move/Action | Hiermit wird eine Application Insights-Komponente auf eine andere Ressourcengruppe oder ein anderes Abonnement migriert. |
> | Aktion | Microsoft.Insights/Components/MyAnalyticsItems/Delete | Hiermit wird ein persönliches Application Insights-Analyseelement gelöscht. |
> | Aktion | Microsoft.Insights/Components/MyAnalyticsItems/Read | Hiermit wird ein persönliches Application Insights-Analyseelement gelesen. |
> | Aktion | Microsoft.Insights/Components/MyAnalyticsItems/Write | Hiermit wird ein persönliches Application Insights-Analyseelement geschrieben. |
> | Aktion | Microsoft.Insights/Components/MyFavorites/Read | Hiermit wird ein persönlicher Application Insights-Favorit gelesen. |
> | Aktion | Microsoft.Insights/Components/PricingPlans/Read | Hiermit wird ein Tarif für eine Application Insights-Komponente gelesen. |
> | Aktion | Microsoft.Insights/Components/PricingPlans/Write | Hiermit wird ein Tarif für eine Application Insights-Komponente geschrieben. |
> | Aktion | Microsoft.Insights/Components/ProactiveDetectionConfigs/Read | Hiermit wird die Konfiguration von Proactive Detection in Application Insights gelesen. |
> | Aktion | Microsoft.Insights/Components/ProactiveDetectionConfigs/Write | Hiermit wird die Konfiguration von Proactive Detection in Application Insights geschrieben. |
> | Aktion | Microsoft.Insights/Components/providers/Microsoft.Insights/MetricDefinitions/Read | Dient zum Lesen von Metrikdefinitionen. |
> | Aktion | Microsoft.Insights/Components/QuotaStatus/Read | Hiermit wird der Kontingentstatus der Application Insights-Komponente gelesen. |
> | Aktion | Microsoft.Insights/Components/Read | Hiermit wird die Konfiguration der Application Insights-Komponente gelesen. |
> | Aktion | Microsoft.Insights/Components/SyntheticMonitorLocations/Read | Hiermit werden die Webteststandorte von Application Insights gelesen. |
> | Aktion | Microsoft.Insights/Components/WorkItemConfigs/Delete | Hiermit wird die standardmäßige ALM-Integrationskonfiguration von Application Insights gelöscht. |
> | Aktion | Microsoft.Insights/Components/WorkItemConfigs/Read | Hiermit wird die ALM-Integrationskonfiguration von Application Insights gelesen. |
> | Aktion | Microsoft.Insights/Components/WorkItemConfigs/Write | Hiermit wird die ALM-Integrationskonfiguration von Application Insights geschrieben. |
> | Aktion | Microsoft.Insights/Components/Write | Hiermit wird in eine Application Insights-Komponentenkonfiguration geschrieben. |
> | Aktion | Microsoft.Insights/DiagnosticSettings/Delete | Dient zum Löschen der Diagnoseeinstellungskonfiguration. |
> | Aktion | Microsoft.Insights/DiagnosticSettings/Read | Dient zum Lesen einer Diagnoseeinstellungskonfiguration. |
> | Aktion | Microsoft.Insights/DiagnosticSettings/Write | Dient zum Schreiben in die Diagnoseeinstellungskonfiguration. |
> | Aktion | Microsoft.Insights/EventCategories/Read | Hiermit wird eine Ereigniskategorie gelesen. |
> | Aktion | Microsoft.Insights/eventtypes/digestevents/Read | Dient zum Lesen des Verwaltungsereignistyp-Digests. |
> | Aktion | Microsoft.Insights/eventtypes/values/Read | Dient zum Lesen von Verwaltungsereignistyp-Werten. |
> | Aktion | Microsoft.Insights/ExtendedDiagnosticSettings/Delete | Dient zum Löschen der erweiterten Diagnoseeinstellungskonfiguration. |
> | Aktion | Microsoft.Insights/ExtendedDiagnosticSettings/Read | Dient zum Lesen einer erweiterten Diagnoseeinstellungskonfiguration. |
> | Aktion | Microsoft.Insights/ExtendedDiagnosticSettings/Write | Dient zum Schreiben in die erweiterte Diagnoseeinstellungskonfiguration. |
> | Aktion | Microsoft.Insights/ListMigrationDate/Action | Hiermit wird das Migrationsdatum für das Abonnement zurückgegeben. |
> | Aktion | Microsoft.Insights/ListMigrationDate/Read | Hiermit wird das Migrationsdatum für das Abonnement zurückgegeben. |
> | Aktion | Microsoft.Insights/LogDefinitions/Read | Dient zum Lesen von Protokolldefinitionen. |
> | Aktion | Microsoft.Insights/LogProfiles/Delete | Dient zum Löschen von Protokollprofilkonfigurationen. |
> | Aktion | Microsoft.Insights/LogProfiles/Read | Dient zum Lesen von Protokollprofilen. |
> | Aktion | Microsoft.Insights/LogProfiles/Write | Dient zum Schreiben in eine Protokollprofilkonfiguration. |
> | Aktion | Microsoft.Insights/MetricAlerts/Delete | Hiermit wird eine Metrikwarnung gelöscht. |
> | Aktion | Microsoft.Insights/MetricAlerts/Read | Hiermit wird eine Metrikwarnung gelesen. |
> | Aktion | Microsoft.Insights/MetricAlerts/Status/Read | Liest den Status der Metrikwarnung. |
> | Aktion | Microsoft.Insights/MetricAlerts/Write | Hiermit wird eine Metrikwarnung geschrieben. |
> | Aktion | Microsoft.Insights/MetricDefinitions/Microsoft.Insights/Read | Dient zum Lesen von Metrikdefinitionen. |
> | Aktion | Microsoft.Insights/MetricDefinitions/providers/Microsoft.Insights/Read | Dient zum Lesen von Metrikdefinitionen. |
> | Aktion | Microsoft.Insights/MetricDefinitions/Read | Dient zum Lesen von Metrikdefinitionen. |
> | Aktion | Microsoft.Insights/Metrics/providers/Metrics/Read | Dient zum Lesen von Metriken. |
> | Aktion | Microsoft.Insights/Metrics/Read | Dient zum Lesen von Metriken. |
> | Aktion | Microsoft.Insights/Metrics/Write | Hiermit werden Metriken geschrieben. |
> | Aktion | Microsoft.Insights/MigrateToNewpricingModel/Action | Hiermit wird ein Abonnement auf das neue Preismodell migriert. |
> | Aktion | Microsoft.Insights/Operations/Read | Hiermit werden Vorgänge gelesen. |
> | Aktion | Microsoft.Insights/Register/Action | Dient zum Registrieren des Microsoft Insights-Anbieters. |
> | Aktion | Microsoft.Insights/RollbackToLegacyPricingModel/Action | Hiermit wird ein Rollback für das Abonnement auf das Legacypreismodell durchgeführt. |
> | Aktion | Microsoft.Insights/ScheduledQueryRules/Delete | Löscht eine geplante Abfrageregel. |
> | Aktion | Microsoft.Insights/ScheduledQueryRules/Read | Liest eine geplante Abfrageregel. |
> | Aktion | Microsoft.Insights/ScheduledQueryRules/Write | Schreibt eine geplante Abfrageregel. |
> | Aktion | Microsoft.Insights/Tenants/Register/Action | Hiermit wird der Microsoft Insights-Anbieter initialisiert. |
> | Aktion | Microsoft.Insights/Unregister/Action | Dient zum Registrieren des Microsoft Insights-Anbieters. |
> | Aktion | Microsoft.Insights/Webtests/Delete | Hiermit wird eine Webtestkonfiguration gelöscht. |
> | Aktion | Microsoft.Insights/Webtests/GetToken/Read | Hiermit wird ein Webtesttoken gelesen. |
> | Aktion | Microsoft.Insights/Webtests/MetricDefinitions/Read | Hiermit werden Webtestmetrikdefinitionen gelesen. |
> | Aktion | Microsoft.Insights/Webtests/Metrics/Read | Hiermit werden Webtestmetriken gelesen. |
> | Aktion | Microsoft.Insights/Webtests/Read | Hiermit wird eine Webtestkonfiguration gelesen. |
> | Aktion | Microsoft.Insights/Webtests/Write | Hiermit wird in eine Webtestkonfiguration geschrieben. |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.KeyVault/checkNameAvailability/read | Prüft, ob ein Schlüsseltresorname gültig ist und noch nicht verwendet wird. |
> | Aktion | Microsoft.KeyVault/deletedVaults/read | Dient zum Anzeigen der Eigenschaften vorläufig gelöschter Schlüsseltresore. |
> | Aktion | Microsoft.KeyVault/hsmPools/delete | Hiermit wird ein HSM-Pool gelöscht. |
> | Aktion | Microsoft.KeyVault/hsmPools/joinVault/action | Hiermit wird ein Schlüsseltresor zu einem HSM-Pool hinzugefügt. |
> | Aktion | Microsoft.KeyVault/hsmPools/read | Hiermit werden die Eigenschaften eines HSM-Pools angezeigt. |
> | Aktion | Microsoft.KeyVault/hsmPools/write | Hiermit wird durch das Aktualisieren der Eigenschaften eines vorhandenen HSM-Pools ein neuer HSM-Pool erstellt. |
> | Aktion | Microsoft.KeyVault/locations/deletedVaults/purge/action | Dient zum endgültigen Löschen eines vorläufig gelöschten Schlüsseltresors. |
> | Aktion | Microsoft.KeyVault/locations/deletedVaults/read | Dient zum Anzeigen der Eigenschaften eines vorläufig gelöschten Schlüsseltresors. |
> | Aktion | Microsoft.KeyVault/locations/deleteVirtualNetworkOrSubnets/action | Hiermit wird Microsoft.KeyVault darüber benachrichtigt, dass ein virtuelles Netzwerk oder Subnetz gelöscht wird. |
> | Aktion | Microsoft.KeyVault/locations/operationResults/read | Dient zum Überprüfen des Ergebnisses eines Vorgangs mit langer Ausführungszeit. |
> | Aktion | Microsoft.KeyVault/operations/read | Listet die verfügbaren Vorgänge für den Microsoft.KeyVault-Ressourcenanbieter auf. |
> | Aktion | Microsoft.KeyVault/register/action | Registriert ein Abonnement. |
> | Aktion | Microsoft.KeyVault/unregister/action | Hiermit wird die Registrierung für ein Abonnement aufgehoben. |
> | Aktion | Microsoft.KeyVault/vaults/accessPolicies/write | Dient zum Aktualisieren einer vorhandenen Zugriffsrichtlinie durch Zusammenführen oder Ersetzen oder zum Hinzufügen einer neuen Zugriffsrichtlinie zu einem Tresor. |
> | Aktion | Microsoft.KeyVault/vaults/delete | Dient zum Löschen eines Schlüsseltresors. |
> | Aktion | Microsoft.KeyVault/vaults/deploy/action | Ermöglicht den Zugriff auf Geheimnisse in einem Schlüsseltresor beim Bereitstellen von Azure-Ressourcen. |
> | Aktion | Microsoft.KeyVault/vaults/providers/Microsoft.Insights/diagnosticSettings/Read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | Microsoft.KeyVault/vaults/providers/Microsoft.Insights/diagnosticSettings/Write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.KeyVault/vaults/providers/Microsoft.Insights/logDefinitions/read | Hiermit werden die verfügbaren Protokolle für einen Schlüsseltresor abgerufen. |
> | Aktion | Microsoft.KeyVault/vaults/providers/Microsoft.Insights/metricDefinitions/read | Hiermit werden die verfügbaren Metriken für einen Schlüsseltresor abgerufen. |
> | Aktion | Microsoft.KeyVault/vaults/read | Dient zum Anzeigen der Eigenschaften eines Schlüsseltresors. |
> | Aktion | Microsoft.KeyVault/vaults/secrets/read | Dient zum Anzeigen der Eigenschaften (aber nicht des Werts) eines Geheimnisses. |
> | Aktion | Microsoft.KeyVault/vaults/secrets/write | Dient zum Erstellen eines neuen Geheimnisses oder zum Aktualisieren des Werts eines vorhandenen Geheimnisses. |
> | Aktion | Microsoft.KeyVault/vaults/write | Dient zum Erstellen eines neuen Schlüsseltresors oder zum Aktualisieren der Eigenschaften einer vorhandenen Schlüsseltresors. |

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.LabServices/labAccounts/CreateLab/action | Hiermit wird ein Lab in einem Lab-Konto erstellt. |
> | Aktion | Microsoft.LabServices/labAccounts/delete | Hiermit werden Lab-Konten gelöscht. |
> | Aktion | Microsoft.LabServices/labAccounts/labs/delete | Dient zum Löschen von Labs. |
> | Aktion | Microsoft.LabServices/labAccounts/labs/environmentSettings/ClaimAny/action | Beansprucht eine zufällige Umgebung für einen Benutzer in den Umgebungseinstellungen. |
> | Aktion | Microsoft.LabServices/labAccounts/labs/environmentSettings/delete | Hiermit wird eine Umgebungseinstellung gelöscht. |
> | Aktion | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Claim/action | Beansprucht die Umgebung und weist diese dem Benutzer zu. |
> | Aktion | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/delete | Dient zum Löschen von Umgebungen. |
> | Aktion | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/read | Dient zum Lesen von Umgebungen. |
> | Aktion | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Start/action | Startet eine Umgebung, indem alle Ressourcen innerhalb der Umgebung gestartet werden. |
> | Aktion | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Stop/action | Beendet eine Umgebung, indem alle Ressourcen innerhalb der Umgebung beendet werden. |
> | Aktion | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/write | Dient zum Hinzufügen oder Ändern von Umgebungen. |
> | Aktion | Microsoft.LabServices/labAccounts/labs/environmentSettings/Publish/action | Stellt die erforderlichen Ressourcen für eine Umgebungseinstellung anhand des aktuellen Status der Lab- bzw. Umgebungseinstellung bereit oder hebt deren Bereitstellung auf. |
> | Aktion | Microsoft.LabServices/labAccounts/labs/environmentSettings/read | Hiermit wird eine Umgebungseinstellung gelesen. |
> | Aktion | Microsoft.LabServices/labAccounts/labs/environmentSettings/Start/action | Startet eine Vorlage, indem alle Ressourcen innerhalb der Vorlage gestartet werden. |
> | Aktion | Microsoft.LabServices/labAccounts/labs/environmentSettings/Stop/action | Startet eine Vorlage, indem alle Ressourcen innerhalb der Vorlage gestartet werden. |
> | Aktion | Microsoft.LabServices/labAccounts/labs/environmentSettings/write | Dient zum Hinzufügen oder Ändern einer Umgebungseinstellung. |
> | Aktion | Microsoft.LabServices/labAccounts/labs/read | Dient zum Lesen von Labs. |
> | Aktion | Microsoft.LabServices/labAccounts/labs/Register/action | Führt eine Registrierung im verwalteten Lab durch. |
> | Aktion | Microsoft.LabServices/labAccounts/labs/users/delete | Hiermit werden Benutzer gelöscht. |
> | Aktion | Microsoft.LabServices/labAccounts/labs/users/read | Hiermit werden Benutzer gelesen. |
> | Aktion | Microsoft.LabServices/labAccounts/labs/users/write | Dient zum Hinzufügen oder Ändern von Benutzern. |
> | Aktion | Microsoft.LabServices/labAccounts/labs/write | Dient zum Hinzufügen oder Ändern von Labs. |
> | Aktion | Microsoft.LabServices/labAccounts/read | Hiermit werden Lab-Konten gelesen. |
> | Aktion | Microsoft.LabServices/labAccounts/sizes/read | Liest die Größe. |
> | Aktion | Microsoft.LabServices/labAccounts/write | Dient zum Hinzufügen oder Ändern von Lab-Konten. |
> | Aktion | Microsoft.LabServices/locations/operations/read | Dient zum Lesen von Vorgängen. |
> | Aktion | Microsoft.LabServices/register/action | Registriert das Abonnement. |
> | Aktion | Microsoft.LabServices/users/GetEnvironment/action | Ruft Details zu virtuellen Computern ab. |
> | Aktion | Microsoft.LabServices/users/ListEnvironments/action | Führt die Umgebungen für den Benutzer auf. |
> | Aktion | Microsoft.LabServices/users/ListLabs/action | Führt die Labs für den Benutzer auf. |
> | Aktion | Microsoft.LabServices/users/Register/action | Registriert einen Benutzer für ein verwaltetes Lab. |
> | Aktion | Microsoft.LabServices/users/StartEnvironment/action | Startet eine Umgebung, indem alle Ressourcen innerhalb der Umgebung gestartet werden. |
> | Aktion | Microsoft.LabServices/users/StopEnvironment/action | Beendet eine Umgebung, indem alle Ressourcen innerhalb der Umgebung beendet werden. |

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.LocationBasedServices/accounts/delete | (Veraltet: Verwenden Sie /providers/Microsoft.Maps) Löscht ein standortbasiertes Dienstkonto. |
> | Aktion | Microsoft.LocationBasedServices/accounts/listKeys/action | (Veraltet: Verwenden Sie /providers/Microsoft.Maps) Führt standortbasierte Schlüssel für Dienstkonten auf. |
> | Aktion | Microsoft.LocationBasedServices/accounts/providers/Microsoft.Insights/diagnosticSettings/read | (Veraltet: Verwenden Sie /providers/Microsoft.Maps) Ruft die Diagnoseeinstellung der Ressource ab. |
> | Aktion | Microsoft.LocationBasedServices/accounts/providers/Microsoft.Insights/diagnosticSettings/write | (Veraltet: Verwenden Sie /providers/Microsoft.Maps) Erstellt oder aktualisiert die Diagnoseeinstellung der Ressource. |
> | Aktion | Microsoft.LocationBasedServices/accounts/providers/Microsoft.Insights/metricDefinitions/read | (Veraltet: Verwenden Sie /providers/Microsoft.Maps) Ruft die verfügbaren Metriken für die standortbasierten Dienstkonten ab. |
> | Aktion | Microsoft.LocationBasedServices/accounts/read | (Veraltet: Verwenden Sie /providers/Microsoft.Maps) Ruft ein standortbasiertes Dienstkonto ab. |
> | Aktion | Microsoft.LocationBasedServices/accounts/regenerateKey/action | (Veraltet: Verwenden Sie /providers/Microsoft.Maps) Generiert einen neuen primären oder sekundären Schlüssel für ein standortbasiertes Dienstkonto. |
> | Aktion | Microsoft.LocationBasedServices/accounts/write | (Veraltet: Verwenden Sie /providers/Microsoft.Maps) Erstellt oder aktualisiert ein standortbasiertes Dienstkonto. |
> | Aktion | Microsoft.LocationBasedServices/register/action | (Veraltet: Verwenden Sie /providers/Microsoft.Maps) Registriert den Anbieter. |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Logic/integrationAccounts/agreements/delete | Löscht die Vereinbarung im Integrationskonto. |
> | Aktion | Microsoft.Logic/integrationAccounts/agreements/listContentCallbackUrl/action | Ruft die Rückruf-URL für den Vereinbarungsinhalt im Integrationskonto ab. |
> | Aktion | Microsoft.Logic/integrationAccounts/agreements/read | Liest die Vereinbarung im Integrationskonto. |
> | Aktion | Microsoft.Logic/integrationAccounts/agreements/write | Erstellt oder aktualisiert die Vereinbarung im Integrationskonto. |
> | Aktion | Microsoft.Logic/integrationAccounts/assemblies/delete | Löscht die Assembly im Integrationskonto. |
> | Aktion | Microsoft.Logic/integrationAccounts/assemblies/listContentCallbackUrl/action | Ruft die Rückruf-URL für den Inhalt der Assembly im Integrationskonto ab. |
> | Aktion | Microsoft.Logic/integrationAccounts/assemblies/read | Liest die Assembly im Integrationskonto. |
> | Aktion | Microsoft.Logic/integrationAccounts/assemblies/write | Erstellt oder aktualisiert die Assembly im Integrationskonto. |
> | Aktion | Microsoft.Logic/integrationAccounts/batchConfigurations/delete | Löscht die Batchkonfiguration im Integrationskonto. |
> | Aktion | Microsoft.Logic/integrationAccounts/batchConfigurations/read | Liest die Batchkonfiguration im Integrationskonto. |
> | Aktion | Microsoft.Logic/integrationAccounts/batchConfigurations/write | Erstellt oder aktualisiert die Batchkonfiguration im Integrationskonto. |
> | Aktion | Microsoft.Logic/integrationAccounts/certificates/delete | Löscht das Zertifikat im Integrationskonto. |
> | Aktion | Microsoft.Logic/integrationAccounts/certificates/read | Liest das Zertifikat im Integrationskonto. |
> | Aktion | Microsoft.Logic/integrationAccounts/certificates/write | Erstellt oder aktualisiert das Zertifikat im Integrationskonto. |
> | Aktion | Microsoft.Logic/integrationAccounts/delete | Löscht das Integrationskonto. |
> | Aktion | Microsoft.Logic/integrationAccounts/join/action | Verknüpft das Integrationskonto. |
> | Aktion | Microsoft.Logic/integrationAccounts/listCallbackUrl/action | Ruft die Rückruf-URL für das Integrationskonto ab. |
> | Aktion | Microsoft.Logic/integrationAccounts/listKeyVaultKeys/action | Ruft die Schlüssel im Schlüsseltresor ab. |
> | Aktion | Microsoft.Logic/integrationAccounts/logTrackingEvents/action | Protokolliert die Überwachungsereignisse im Integrationskonto. |
> | Aktion | Microsoft.Logic/integrationAccounts/maps/delete | Löscht die Zuordnung im Integrationskonto. |
> | Aktion | Microsoft.Logic/integrationAccounts/maps/listContentCallbackUrl/action | Ruft die Rückruf-URL für den Inhalt der Zuordnung im Integrationskonto ab. |
> | Aktion | Microsoft.Logic/integrationAccounts/maps/read | Liest die Zuordnung im Integrationskonto. |
> | Aktion | Microsoft.Logic/integrationAccounts/maps/write | Erstellt oder aktualisiert die Zuordnung im Integrationskonto. |
> | Aktion | Microsoft.Logic/integrationAccounts/partners/delete | Löscht den Partner im Integrationskonto. |
> | Aktion | Microsoft.Logic/integrationAccounts/partners/listContentCallbackUrl/action | Ruft die Rückruf-URL für den Partnerinhalt im Integrationskonto ab. |
> | Aktion | Microsoft.Logic/integrationAccounts/partners/read | Liest den Partner im Integrationskonto. |
> | Aktion | Microsoft.Logic/integrationAccounts/partners/write | Erstellt oder aktualisiert den Partner im Integrationskonto. |
> | Aktion | Microsoft.Logic/integrationAccounts/providers/Microsoft.Insights/logDefinitions/read | Liest die Protokolldefinitionen des Integrationskontos. |
> | Aktion | Microsoft.Logic/integrationAccounts/read | Liest das Integrationskonto. |
> | Aktion | Microsoft.Logic/integrationAccounts/regenerateAccessKey/action | Generiert die Zugriffsschlüsselgeheimnisse neu. |
> | Aktion | Microsoft.Logic/integrationAccounts/schemas/delete | Löscht das Schema im Integrationskonto. |
> | Aktion | Microsoft.Logic/integrationAccounts/schemas/listContentCallbackUrl/action | Ruft die Rückruf-URL für den Schemainhalt im Integrationskonto ab. |
> | Aktion | Microsoft.Logic/integrationAccounts/schemas/read | Liest das Schema im Integrationskonto. |
> | Aktion | Microsoft.Logic/integrationAccounts/schemas/write | Erstellt oder aktualisiert das Schema im Integrationskonto. |
> | Aktion | Microsoft.Logic/integrationAccounts/sessions/delete | Löscht die Sitzung im Integrationskonto. |
> | Aktion | Microsoft.Logic/integrationAccounts/sessions/read | Liest die Batchkonfiguration im Integrationskonto. |
> | Aktion | Microsoft.Logic/integrationAccounts/sessions/write | Erstellt oder aktualisiert die Sitzung im Integrationskonto. |
> | Aktion | Microsoft.Logic/integrationAccounts/write | Erstellt oder aktualisiert das Integrationskonto. |
> | Aktion | Microsoft.Logic/locations/workflows/validate/action | Überprüft den Workflow. |
> | Aktion | Microsoft.Logic/operations/read | Ruft den Vorgang ab. |
> | Aktion | Microsoft.Logic/register/action | Registriert den Microsoft.Logic-Ressourcenanbieter für ein angegebenes Abonnement. |
> | Aktion | Microsoft.Logic/workflows/accessKeys/delete | Löscht den Zugriffsschlüssel. |
> | Aktion | Microsoft.Logic/workflows/accessKeys/list/action | Listet die Zugriffsschlüsselgeheimnisse auf. |
> | Aktion | Microsoft.Logic/workflows/accessKeys/read | Liest den Zugriffsschlüssel. |
> | Aktion | Microsoft.Logic/workflows/accessKeys/regenerate/action | Generiert die Zugriffsschlüsselgeheimnisse neu. |
> | Aktion | Microsoft.Logic/workflows/accessKeys/write | Erstellt oder aktualisiert den Zugriffsschlüssel. |
> | Aktion | Microsoft.Logic/workflows/delete | Löscht den Workflow. |
> | Aktion | Microsoft.Logic/workflows/disable/action | Deaktiviert den Workflow. |
> | Aktion | Microsoft.Logic/workflows/enable/action | Aktiviert den Workflow. |
> | Aktion | Microsoft.Logic/workflows/listCallbackUrl/action | Ruft die Rückruf-URL für Workflows ab. |
> | Aktion | Microsoft.Logic/workflows/listSwagger/action | Ruft die Swagger-Definitionen für den Workflow ab. |
> | Aktion | Microsoft.Logic/workflows/move/action | Verschiebt den Workflow von der vorhandenen Abonnement-ID, der Ressourcengruppe und/oder dem Namen in eine andere Abonnement-ID, eine andere Ressourcengruppe und/oder einen anderen Namen. |
> | Aktion | Microsoft.Logic/workflows/providers/Microsoft.Insights/diagnosticSettings/read | Liest die Diagnoseeinstellungen des Workflows. |
> | Aktion | Microsoft.Logic/workflows/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung des Workflows. |
> | Aktion | Microsoft.Logic/workflows/providers/Microsoft.Insights/logDefinitions/read | Liest die Protokolldefinitionen des Workflows. |
> | Aktion | Microsoft.Logic/workflows/providers/Microsoft.Insights/metricDefinitions/read | Liest die Metrikdefinitionen des Workflows. |
> | Aktion | Microsoft.Logic/workflows/read | Liest den Workflow. |
> | Aktion | Microsoft.Logic/workflows/regenerateAccessKey/action | Generiert die Zugriffsschlüsselgeheimnisse neu. |
> | Aktion | Microsoft.Logic/workflows/run/action | Startet eine Ausführung des Workflows. |
> | Aktion | Microsoft.Logic/workflows/runs/actions/listExpressionTraces/action | Ruft die Ausdrucksablaufverfolgungen für Workflowausführungsaktionen ab. |
> | Aktion | Microsoft.Logic/workflows/runs/actions/read | Liest die Workflowausführungsaktion. |
> | Aktion | Microsoft.Logic/workflows/runs/actions/repetitions/listExpressionTraces/action | Ruft die Ausdrucksablaufverfolgungen für Wiederholungen von Workflowausführungsaktionen ab. |
> | Aktion | Microsoft.Logic/workflows/runs/actions/repetitions/read | Liest die Wiederholung von Workflowausführungsaktionen. |
> | Aktion | Microsoft.Logic/workflows/runs/actions/scoperepetitions/read | Liest die Bereichswiederholung von Workflowausführungsaktionen. |
> | Aktion | Microsoft.Logic/workflows/runs/cancel/action | Bricht die Ausführung eines Workflows ab. |
> | Aktion | Microsoft.Logic/workflows/runs/operations/read | Liest den Status von Workflowausführungsaktionen. |
> | Aktion | Microsoft.Logic/workflows/runs/read | Liest die Workflowausführung. |
> | Aktion | Microsoft.Logic/workflows/suspend/action | Hält den Workflow an. |
> | Aktion | Microsoft.Logic/workflows/triggers/histories/read | Liest die Triggerverläufe. |
> | Aktion | Microsoft.Logic/workflows/triggers/histories/resubmit/action | Übermittelt den Workflowtrigger erneut. |
> | Aktion | Microsoft.Logic/workflows/triggers/listCallbackUrl/action | Ruft die Rückruf-URL für Trigger ab. |
> | Aktion | Microsoft.Logic/workflows/triggers/read | Liest den Trigger. |
> | Aktion | Microsoft.Logic/workflows/triggers/reset/action | Setzt den Trigger zurück. |
> | Aktion | Microsoft.Logic/workflows/triggers/run/action | Führt den Trigger aus. |
> | Aktion | Microsoft.Logic/workflows/triggers/setState/action | Legt den Triggerzustand fest. |
> | Aktion | Microsoft.Logic/workflows/validate/action | Überprüft den Workflow. |
> | Aktion | Microsoft.Logic/workflows/versions/read | Liest die Workflowversion. |
> | Aktion | Microsoft.Logic/workflows/versions/triggers/listCallbackUrl/action | Ruft die Rückruf-URL für Trigger ab. |
> | Aktion | Microsoft.Logic/workflows/write | Erstellt oder aktualisiert den Workflow. |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.MachineLearning/commitmentPlans/commitmentAssociations/move/action | Dient zum Verschieben beliebiger Machine Learning-Vertragsplanzuordnungen. |
> | Aktion | Microsoft.MachineLearning/commitmentPlans/commitmentAssociations/read | Dient zum Lesen beliebiger Machine Learning-Vertragsplanzuordnungen. |
> | Aktion | Microsoft.MachineLearning/commitmentPlans/delete | Dient zum Löschen beliebiger Machine Learning-Vertragspläne. |
> | Aktion | Microsoft.MachineLearning/commitmentPlans/join/action | Dient zum Beitreten zu beliebigen Machine Learning-Vertragsplänen. |
> | Aktion | Microsoft.MachineLearning/commitmentPlans/read | Dient zum Lesen beliebiger Machine Learning-Vertragspläne. |
> | Aktion | Microsoft.MachineLearning/commitmentPlans/write | Dient zum Erstellen oder Aktualisieren beliebiger Machine Learning-Vertragspläne. |
> | Aktion | Microsoft.MachineLearning/locations/operationresults/read | Ruft das Ergebnis eines Machine Learning-Vorgangs ab. |
> | Aktion | Microsoft.MachineLearning/locations/operationsstatus/read | Ruft den Status eines laufenden Machine Learning-Vorgangs ab. |
> | Aktion | Microsoft.MachineLearning/operations/read | Ruft Machine Learning-Vorgänge ab. |
> | Aktion | Microsoft.MachineLearning/register/action | Registriert das Abonnement für den Machine Learning-Webdienst-Ressourcenanbieter und ermöglicht die Erstellung von Webdiensten. |
> | Aktion | Microsoft.MachineLearning/skus/read | Dient zum Abrufen von SKUs für Machine Learning-Verpflichtungspläne. |
> | Aktion | Microsoft.MachineLearning/webServices/action | Dient zum Erstellen regionaler Webdiensteigenschaften für unterstützte Regionen. |
> | Aktion | Microsoft.MachineLearning/webServices/delete | Dient zum Löschen beliebiger Machine Learning-Webdienste. |
> | Aktion | Microsoft.MachineLearning/webServices/listkeys/read | Ruft Schlüssel für einen Machine Learning-Webdienst ab. |
> | Aktion | Microsoft.MachineLearning/webServices/read | Dient zum Lesen beliebiger Machine Learning-Webdienste. |
> | Aktion | Microsoft.MachineLearning/webServices/write | Dient zum Erstellen oder Aktualisieren beliebiger Machine Learning-Webdienste. |
> | Aktion | Microsoft.MachineLearning/Workspaces/delete | Dient zum Löschen beliebiger Machine Learning-Arbeitsbereiche. |
> | Aktion | Microsoft.MachineLearning/Workspaces/listworkspacekeys/action | Dient zum Auflisten von Schlüsseln für einen Machine Learning-Arbeitsbereich. |
> | Aktion | Microsoft.MachineLearning/Workspaces/read | Dient zum Lesen beliebiger Machine Learning-Arbeitsbereiche. |
> | Aktion | Microsoft.MachineLearning/Workspaces/resyncstoragekeys/action | Dient zum erneuten Synchronisieren von Schlüsseln eines für einen Machine Learning-Arbeitsbereich konfigurierten Speicherkontos. |
> | Aktion | Microsoft.MachineLearning/Workspaces/write | Dient zum Erstellen oder Aktualisieren beliebiger Machine Learning-Arbeitsbereiche. |

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.MachineLearningCompute/operationalizationClusters/checkUpdate/action | Hiermit wird überprüft, ob Updates für Systemdienste für den Operationalisierungscluster verfügbar sind. |
> | Aktion | Microsoft.MachineLearningCompute/operationalizationClusters/delete | Hiermit werden alle Hostingkonten gelöscht. |
> | Aktion | Microsoft.MachineLearningCompute/operationalizationClusters/listKeys/action | Hiermit werden die dem Operationalisierungscluster zugeordneten Schlüssel aufgelistet. |
> | Aktion | Microsoft.MachineLearningCompute/operationalizationClusters/read | Hiermit werden alle Hostingkonten gelesen. |
> | Aktion | Microsoft.MachineLearningCompute/operationalizationClusters/updateSystem/action | Hiermit werden die Systemdienste in einem Operationalisierungscluster aktualisiert. |
> | Aktion | Microsoft.MachineLearningCompute/operationalizationClusters/write | Hiermit werden alle Hostingkonten erstellt oder aktualisiert. |
> | Aktion | Microsoft.MachineLearningCompute/register/action | Registriert die Abonnement-ID beim Ressourcenanbieter und ermöglicht die Erstellung von Machine Learning-Computeressourcen. |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.MachineLearningModelManagement/accounts/delete | Hiermit werden alle Hostingkonten gelöscht. |
> | Aktion | Microsoft.MachineLearningModelManagement/accounts/read | Hiermit werden alle Hostingkonten gelesen. |
> | Aktion | Microsoft.MachineLearningModelManagement/accounts/write | Hiermit werden alle Hostingkonten erstellt oder aktualisiert. |
> | Aktion | Microsoft.MachineLearningModelManagement/register/action | Registriert die Abonnement-ID beim Ressourcenanbieter und ermöglicht die Erstellung eines Hostingkontos. |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.MachineLearningServices/register/action | Registriert das Abonnement für den Machine Learning Services-Ressourcenanbieter. |
> | Aktion | Microsoft.MachineLearningServices/workspaces/computes/delete | Löscht die Computeressourcen in Machine Learning Services-Arbeitsbereichen. |
> | Aktion | Microsoft.MachineLearningServices/workspaces/computes/listKeys/action | Führt geheime Schlüssel für die Computeressourcen in Machine Learning Services-Arbeitsbereichen auf. |
> | Aktion | Microsoft.MachineLearningServices/workspaces/computes/read | Ruft die Computeressourcen in Machine Learning Services-Arbeitsbereichen ab. |
> | Aktion | Microsoft.MachineLearningServices/workspaces/computes/write | Erstellt oder aktualisiert die Computeressourcen in Machine Learning Services-Arbeitsbereichen. |
> | Aktion | Microsoft.MachineLearningServices/workspaces/delete | Löscht die Machine Learning Services-Arbeitsbereiche. |
> | Aktion | Microsoft.MachineLearningServices/workspaces/listKeys/action | Führt die geheimen Schlüssel für einen Machine Learning Services-Arbeitsbereich auf. |
> | Aktion | Microsoft.MachineLearningServices/workspaces/read | Ruft die Machine Learning Services-Arbeitsbereiche ab. |
> | Aktion | Microsoft.MachineLearningServices/workspaces/write | Erstellt oder aktualisiert die Machine Learning Services-Arbeitsbereiche. |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.ManagedIdentity/userAssignedIdentities/assign/action | RBAC-Aktion für das Zuweisen einer vorhandenen Identität, die einem Benutzer zugewiesen ist, zu einer Ressource |
> | Aktion | Microsoft.ManagedIdentity/userAssignedIdentities/delete | Löscht eine vorhandene zugewiesene Benutzeridentität. |
> | Aktion | Microsoft.ManagedIdentity/userAssignedIdentities/read | Ruft eine vorhandene zugewiesene Benutzeridentität ab. |
> | Aktion | Microsoft.ManagedIdentity/userAssignedIdentities/write | Erstellt eine neue zugewiesene Benutzeridentität oder aktualisiert die Markierungen, die einer vorhandenen zugewiesenen Benutzeridentität zugeordnet sind. |

## <a name="microsoftmanagedlab"></a>Microsoft.ManagedLab

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.ManagedLab/labAccounts/CreateLab/action | Hiermit wird ein Lab in einem Lab-Konto erstellt. |
> | Aktion | Microsoft.ManagedLab/labAccounts/delete | Hiermit werden Lab-Konten gelöscht. |
> | Aktion | Microsoft.ManagedLab/labAccounts/labs/delete | Dient zum Löschen von Labs. |
> | Aktion | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/delete | Hiermit wird eine Umgebungseinstellung gelöscht. |
> | Aktion | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/environments/delete | Dient zum Löschen von Umgebungen. |
> | Aktion | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/environments/read | Dient zum Lesen von Umgebungen. |
> | Aktion | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/environments/write | Dient zum Hinzufügen oder Ändern von Umgebungen. |
> | Aktion | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/read | Hiermit wird eine Umgebungseinstellung gelesen. |
> | Aktion | Microsoft.ManagedLab/labAccounts/labs/environmentSettings/write | Dient zum Hinzufügen oder Ändern einer Umgebungseinstellung. |
> | Aktion | Microsoft.ManagedLab/labAccounts/labs/labVms/delete | Löscht virtuelle Lab-Computer. |
> | Aktion | Microsoft.ManagedLab/labAccounts/labs/labVms/read | Liest virtuelle Lab-Computer. |
> | Aktion | Microsoft.ManagedLab/labAccounts/labs/labVms/write | Dient zum Hinzufügen oder Ändern virtueller Lab-Computer. |
> | Aktion | Microsoft.ManagedLab/labAccounts/labs/read | Dient zum Lesen von Labs. |
> | Aktion | Microsoft.ManagedLab/labAccounts/labs/write | Dient zum Hinzufügen oder Ändern von Labs. |
> | Aktion | Microsoft.ManagedLab/labAccounts/read | Hiermit werden Lab-Konten gelesen. |
> | Aktion | Microsoft.ManagedLab/labAccounts/write | Dient zum Hinzufügen oder Ändern von Lab-Konten. |
> | Aktion | Microsoft.ManagedLab/locations/operations/read | Dient zum Lesen von Vorgängen. |
> | Aktion | Microsoft.ManagedLab/register/action | Registriert das Abonnement. |

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Management/checkNameAvailability/action | Überprüft, ob der Name der angegebenen Verwaltungsgruppe gültig und eindeutig ist. |
> | Aktion | Microsoft.Management/getEntities/action | Listet alle Entitäten (Verwaltungsgruppen, Abonnements usw.) für den authentifizierten Benutzer auf. |
> | Aktion | Microsoft.Management/managementGroups/delete | Löscht eine Verwaltungsgruppe. |
> | Aktion | Microsoft.Management/managementGroups/read | Listet die Verwaltungsgruppen für den authentifizierten Benutzer auf. |
> | Aktion | Microsoft.Management/managementGroups/subscriptions/delete | Hebt die Zuordnung des Abonnements mit der Verwaltungsgruppe auf. |
> | Aktion | Microsoft.Management/managementGroups/subscriptions/write | Ordnet ein vorhandenes Abonnement der Verwaltungsgruppe zu. |
> | Aktion | Microsoft.Management/managementGroups/write | Erstellt oder aktualisiert eine Verwaltungsgruppe. |
> | Aktion | Microsoft.Management/register/action | Registriert das angegebene Abonnement mit Microsoft.Management. |

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Maps/accounts/delete | Löscht ein Maps-Konto. |
> | Aktion | Microsoft.Maps/accounts/listKeys/action | Führt Schlüssel für ein Maps-Konto auf. |
> | Aktion | Microsoft.Maps/accounts/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | Microsoft.Maps/accounts/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.Maps/accounts/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für Maps-Konten ab. |
> | Aktion | Microsoft.Maps/accounts/read | Ruft ein Maps-Konto ab. |
> | Aktion | Microsoft.Maps/accounts/regenerateKey/action | Generiert einen neuen primären oder sekundären Schlüssel für ein Maps-Konto. |
> | Aktion | Microsoft.Maps/accounts/write | Erstellt oder aktualisiert ein Maps-Konto. |
> | Aktion | Microsoft.Maps/register/action | Hiermit wird der Anbieter registriert. |

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Marketplace/offerTypes/publishers/offers/plans/agreements/read | Gibt eine Vereinbarung zurück. |
> | Aktion | Microsoft.Marketplace/offerTypes/publishers/offers/plans/agreements/write | Akzeptiert eine unterzeichnete Vereinbarung. |
> | Aktion | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/importImage/action | Importiert ein Image in den Access Control-Datensatz des Endbenutzers. |
> | Aktion | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/read | Gibt eine Konfiguration zurück. |
> | Aktion | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/write | Speichert eine Konfiguration. |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.MarketplaceApps/ClassicDevServices/delete | Führt einen DELETE-Vorgang für eine Ressource eines klassischen Entwicklerdiensts durch. |
> | Aktion | Microsoft.MarketplaceApps/ClassicDevServices/listSecrets/action | Ruft die Ressourcenverwaltungsschlüssel eines klassischen Entwicklerdiensts ab. |
> | Aktion | Microsoft.MarketplaceApps/ClassicDevServices/listSingleSignOnToken/action | Ruft die URL für einmaliges Anmelden für einen klassischen Entwicklerdienst ab. |
> | Aktion | Microsoft.MarketplaceApps/ClassicDevServices/read | Führt einen GET-Vorgang für einen klassischen Entwicklerdienst durch. |
> | Aktion | Microsoft.MarketplaceApps/ClassicDevServices/regenerateKey/action | Generiert Ressourcenverwaltungsschlüssel eines klassischen Entwicklerdiensts. |
> | Aktion | Microsoft.MarketplaceApps/Operations/read | Liest die Vorgänge für alle Ressourcentypen. |

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.MarketplaceOrdering/agreements/offers/plans/cancel/action | Dient zum Stornieren einer Vereinbarung für ein angegebenes Marketplace-Element. |
> | Aktion | Microsoft.MarketplaceOrdering/agreements/offers/plans/read | Dient zum Zurückgeben einer Vereinbarung für ein angegebenes Marketplace-Element. |
> | Aktion | Microsoft.MarketplaceOrdering/agreements/offers/plans/sign/action | Dient zum Unterzeichnen einer Vereinbarung für ein angegebenes Marketplace-Element. |
> | Aktion | Microsoft.MarketplaceOrdering/agreements/read | Gibt alle Vereinbarungen eines bestimmten Abonnements zurück. |
> | Aktion | Microsoft.MarketplaceOrdering/offertypes/publishers/offers/plans/agreements/read | Dient zum Abrufen einer Vereinbarung für ein angegebenes Marketplace-VM-Element. |
> | Aktion | Microsoft.MarketplaceOrdering/offertypes/publishers/offers/plans/agreements/write | Dient zum Registrieren oder Stornieren einer Vereinbarung für ein angegebenes Marketplace-VM-Element. |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Media/checknameavailability/action | Überprüft, ob ein Media Services-Kontoname verfügbar ist. |
> | Aktion | Microsoft.Media/locations/checkNameAvailability/action | Überprüft, ob ein Media Services-Kontoname verfügbar ist. |
> | Aktion | Microsoft.Media/mediaservices/assets/delete | Löscht alle Objekte. |
> | Aktion | Microsoft.Media/mediaservices/assets/getEncryptionKey/action | Ruft Verschlüsselungsschlüssel für ein Objekt ab. |
> | Aktion | Microsoft.Media/mediaservices/assets/listContainerSas/action | Führt die Container-SAS-URLs für ein Objekt auf. |
> | Aktion | Microsoft.Media/mediaservices/assets/read | Liest alle Objekte. |
> | Aktion | Microsoft.Media/mediaservices/assets/write | Erstellt oder aktualisiert alle Objekte. |
> | Aktion | Microsoft.Media/mediaservices/contentKeyPolicies/delete | Löscht alle Richtlinien für Inhaltsschlüssel. |
> | Aktion | Microsoft.Media/mediaservices/contentKeyPolicies/getPolicyPropertiesWithSecrets/action | Ruft Richtlinieneigenschaften mit geheimen Schlüsseln ab. |
> | Aktion | Microsoft.Media/mediaservices/contentKeyPolicies/read | Liest alle Richtlinien für Inhaltsschlüssel. |
> | Aktion | Microsoft.Media/mediaservices/contentKeyPolicies/write | Erstellt oder aktualisiert alle Richtlinien für Inhaltsschlüssel. |
> | Aktion | Microsoft.Media/mediaservices/delete | Löscht ein Media Services-Konto. |
> | Aktion | Microsoft.Media/mediaservices/eventGridFilters/delete | Löscht alle Event Grid-Filter. |
> | Aktion | Microsoft.Media/mediaservices/eventGridFilters/read | Liest alle Event Grid-Filter. |
> | Aktion | Microsoft.Media/mediaservices/eventGridFilters/write | Erstellt oder aktualisiert alle Event Grid-Filter. |
> | Aktion | Microsoft.Media/mediaservices/liveEventOperations/read | Liest alle Vorgänge für Liveereignisse. |
> | Aktion | Microsoft.Media/mediaservices/liveEvents/delete | Löscht alle Liveereignisse. |
> | Aktion | Microsoft.Media/mediaservices/liveEvents/liveOutputs/delete | Löscht alle Liveausgaben. |
> | Aktion | Microsoft.Media/mediaservices/liveEvents/liveOutputs/read | Liest alle Liveausgaben. |
> | Aktion | Microsoft.Media/mediaservices/liveEvents/liveOutputs/write | Erstellt oder aktualisiert alle Liveausgaben. |
> | Aktion | Microsoft.Media/mediaservices/liveEvents/read | Liest alle Liveereignisse. |
> | Aktion | Microsoft.Media/mediaservices/liveEvents/reset/action | Setzt alle Vorgänge für Liveereignisse zurück. |
> | Aktion | Microsoft.Media/mediaservices/liveEvents/start/action | Startet alle Vorgänge für Liveereignisse. |
> | Aktion | Microsoft.Media/mediaservices/liveEvents/stop/action | Beendet alle Vorgänge für Liveereignisse. |
> | Aktion | Microsoft.Media/mediaservices/liveEvents/write | Erstellt oder aktualisiert alle Liveereignisse. |
> | Aktion | Microsoft.Media/mediaservices/liveOutputOperations/read | Liest alle Vorgänge für Liveausgaben. |
> | Aktion | Microsoft.Media/mediaservices/read | Liest ein Media Services-Konto. |
> | Aktion | Microsoft.Media/mediaservices/streamingEndpointOperations/read | Liest alle Vorgänge des Streamingendpunkts. |
> | Aktion | Microsoft.Media/mediaservices/streamingEndpoints/delete | Löscht alle Streamingendpunkte. |
> | Aktion | Microsoft.Media/mediaservices/streamingEndpoints/read | Liest alle Streamingendpunkte. |
> | Aktion | Microsoft.Media/mediaservices/streamingEndpoints/scale/action | Skaliert alle Vorgänge des Streamingendpunkts. |
> | Aktion | Microsoft.Media/mediaservices/streamingEndpoints/start/action | Startet alle Vorgänge des Streamingendpunkts. |
> | Aktion | Microsoft.Media/mediaservices/streamingEndpoints/stop/action | Beendet alle Vorgänge des Streamingendpunkts. |
> | Aktion | Microsoft.Media/mediaservices/streamingEndpoints/write | Erstellt oder aktualisiert alle Streamingendpunkte. |
> | Aktion | Microsoft.Media/mediaservices/streamingLocators/delete | Löscht alle Streaminglocators. |
> | Aktion | Microsoft.Media/mediaservices/streamingLocators/listContentKeys/action | Führt Inhaltsschlüssel auf. |
> | Aktion | Microsoft.Media/mediaservices/streamingLocators/listPaths/action | Führt Pfade auf. |
> | Aktion | Microsoft.Media/mediaservices/streamingLocators/read | Liest alle Streaminglocators. |
> | Aktion | Microsoft.Media/mediaservices/streamingLocators/write | Erstellt oder aktualisiert alle Streaminglocators. |
> | Aktion | Microsoft.Media/mediaservices/streamingPolicies/delete | Löscht alle Streamingrichtlinien. |
> | Aktion | Microsoft.Media/mediaservices/streamingPolicies/read | Liest alle Streamingrichtlinien. |
> | Aktion | Microsoft.Media/mediaservices/streamingPolicies/write | Erstellt oder aktualisiert alle Streamingrichtlinien. |
> | Aktion | Microsoft.Media/mediaservices/syncStorageKeys/action | Synchronisiert die Speicherschlüssel für ein angefügtes Azure Storage-Konto. |
> | Aktion | Microsoft.Media/mediaservices/transforms/delete | Löscht alle Transformationen. |
> | Aktion | Microsoft.Media/mediaservices/transforms/jobs/cancelJob/action | Dient zum Abbrechen von Aufträgen. |
> | Aktion | Microsoft.Media/mediaservices/transforms/jobs/delete | Löscht alle Aufträge. |
> | Aktion | Microsoft.Media/mediaservices/transforms/jobs/read | Liest alle Aufträge. |
> | Aktion | Microsoft.Media/mediaservices/transforms/jobs/write | Erstellt oder aktualisiert alle Aufträge. |
> | Aktion | Microsoft.Media/mediaservices/transforms/read | Liest alle Transformationen. |
> | Aktion | Microsoft.Media/mediaservices/transforms/write | Erstellt oder aktualisiert alle Transformationen. |
> | Aktion | Microsoft.Media/mediaservices/write | Erstellt oder aktualisiert ein Media Services-Konto. |
> | Aktion | Microsoft.Media/operations/read | Dient zum Abrufen verfügbarer Vorgänge. |
> | Aktion | Microsoft.Media/register/action | Registriert das Abonnement für den Media Services-Ressourcenanbieter und ermöglicht die Erstellung von Media Services-Konten. |
> | Aktion | Microsoft.Media/unregister/action | Hebt die Registrierung des Abonnements für den Media Services-Ressourcenanbieter auf. |

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Migrate/Operations/read | Liest die verfügbar gemachten Vorgänge. |

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Network/applicationGatewayAvailableSslOptions/predefinedPolicies/read | Vordefinierte SSL-Richtlinie für Application Gateway |
> | Aktion | Microsoft.Network/applicationGatewayAvailableSslOptions/read | Verfügbare SSL-Optionen für Application Gateway |
> | Aktion | Microsoft.Network/applicationGatewayAvailableWafRuleSets/read | Ruft verfügbare WAF-Regelsätze für Application Gateway ab. |
> | Aktion | Microsoft.Network/applicationGateways/backendAddressPools/join/action | Verknüpft einen Back-End-Adresspool für ein Anwendungsgateway. |
> | Aktion | Microsoft.Network/applicationGateways/backendhealth/action | Ruft die Back-End-Integrität eines Anwendungsgateways ab. |
> | Aktion | Microsoft.Network/applicationGateways/delete | Löscht ein Anwendungsgateway. |
> | Aktion | Microsoft.Network/applicationGateways/effectiveNetworkSecurityGroups/action | Ruft die für Application Gateway konfigurierte Routentabelle ab. |
> | Aktion | Microsoft.Network/applicationGateways/effectiveRouteTable/action | Ruft die für Application Gateway konfigurierte Routentabelle ab. |
> | Aktion | Microsoft.Network/applicationGateways/providers/Microsoft.Insights/logDefinitions/read | Ruft die Ereignisse für Application Gateway ab. |
> | Aktion | Microsoft.Network/applicationGateways/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für Application Gateway ab. |
> | Aktion | Microsoft.Network/applicationGateways/read | Ruft ein Anwendungsgateway ab. |
> | Aktion | Microsoft.Network/applicationGateways/setSecurityCenterConfiguration/action | Legt die Security Center-Konfiguration für Application Gateway fest. |
> | Aktion | Microsoft.Network/applicationGateways/start/action | Startet ein Anwendungsgateway. |
> | Aktion | Microsoft.Network/applicationGateways/stop/action | Beendet ein Anwendungsgateway. |
> | Aktion | Microsoft.Network/applicationGateways/write | Erstellt ein Anwendungsgateway oder aktualisiert ein vorhandenes Anwendungsgateway. |
> | Aktion | Microsoft.Network/applicationSecurityGroups/delete | Löscht eine Anwendungssicherheitsgruppe. |
> | Aktion | Microsoft.Network/applicationSecurityGroups/joinIpConfiguration/action | Dient zum Verknüpfen der IP-Konfiguration mit Anwendungssicherheitsgruppen. |
> | Aktion | Microsoft.Network/applicationSecurityGroups/joinNetworkSecurityRule/action | Dient zum Verknüpfen der Sicherheitsregel mit Anwendungssicherheitsgruppen. |
> | Aktion | Microsoft.Network/applicationSecurityGroups/read | Ruft eine Anwendungssicherheitsgruppen-ID ab. |
> | Aktion | Microsoft.Network/applicationSecurityGroups/write | Erstellt eine Anwendungssicherheitsgruppe oder aktualisiert eine vorhandene Anwendungssicherheitsgruppe. |
> | Aktion | Microsoft.Network/bgpServiceCommunities/read | Dient zum Abrufen von BGP Service Communities. |
> | Aktion | Microsoft.Network/checkTrafficManagerNameAvailability/action | Prüft die Verfügbarkeit eines relativen Traffic Manager-DNS-Namens. |
> | Aktion | Microsoft.Network/connections/delete | Löscht VirtualNetworkGatewayConnection-Elemente. |
> | Aktion | Microsoft.Network/connections/read | Ruft VirtualNetworkGatewayConnection-Elemente ab. |
> | Aktion | Microsoft.Network/connections/sharedkey/action | Ruft SharedKey-Elemente für VirtualNetworkGatewayConnection ab. |
> | Aktion | Microsoft.Network/connections/sharedKey/read | Ruft SharedKey-Elemente für „VirtualNetworkGatewayConnection“ ab. |
> | Aktion | Microsoft.Network/connections/sharedKey/write | Erstellt ein SharedKey-Element für „VirtualNetworkGatewayConnection“ oder aktualisiert ein vorhandenes SharedKey-Element für „VirtualNetworkGatewayConnection“. |
> | Aktion | Microsoft.Network/connections/vpndeviceconfigurationscript/action | Ruft die VPN-Gerätekonfiguration von VirtualNetworkGatewayConnection ab. |
> | Aktion | Microsoft.Network/connections/write | Erstellt ein VirtualNetworkGatewayConnection-Element oder aktualisiert ein vorhandenes VirtualNetworkGatewayConnection-Element. |
> | Aktion | Microsoft.Network/ddosProtectionPlans/ddosProtectionPlanProxies/delete | Löscht den Proxy eines DDoS Protection-Plans. |
> | Aktion | Microsoft.Network/ddosProtectionPlans/ddosProtectionPlanProxies/read | Ruft die Proxydefinition eines DDoS Protection-Plans ab. |
> | Aktion | Microsoft.Network/ddosProtectionPlans/ddosProtectionPlanProxies/write | Erstellt einen Proxy für den DDoS Protection-Plan oder aktualisiert einen vorhandenen Proxy für den DDoS Protection-Plan. |
> | Aktion | Microsoft.Network/ddosProtectionPlans/delete | Löscht einen DDoS Protection-Plan. |
> | Aktion | Microsoft.Network/ddosProtectionPlans/join/action | Dient zum Verknüpfen eines DDoS Protection-Plans. |
> | Aktion | Microsoft.Network/ddosProtectionPlans/read | Dient zum Aufrufen eines DDoS Protection-Plans. |
> | Aktion | Microsoft.Network/ddosProtectionPlans/write | Erstellt oder aktualisiert einen DDoS Protection-Plan.  |
> | Aktion | Microsoft.Network/dnsoperationresults/read | Ruft Ergebnisse eines DNS-Vorgangs ab. |
> | Aktion | Microsoft.Network/dnsoperationstatuses/read | Ruft den Status eines DNS-Vorgangs ab.  |
> | Aktion | Microsoft.Network/dnszones/A/delete | Dient zum Entfernen des Ressourceneintragssatzes mit einem bestimmten Namen und dem Typ „A“ aus einer DNS-Zone. |
> | Aktion | Microsoft.Network/dnszones/A/read | Dient zum Abrufen des Ressourceneintragssatzes vom Typ „A“ im JSON-Format. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag. |
> | Aktion | Microsoft.Network/dnszones/A/write | Dient zum Erstellen oder Aktualisieren eines Ressourceneintragssatzes vom Typ „A“ innerhalb einer DNS-Zone. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt. |
> | Aktion | Microsoft.Network/dnszones/AAAA/delete | Dient zum Entfernen des Ressourceneintragssatzes mit einem bestimmten Namen und dem Typ „AAAA“ aus einer DNS-Zone. |
> | Aktion | Microsoft.Network/dnszones/AAAA/read | Dient zum Abrufen des Ressourceneintragssatzes vom Typ „AAAA“ im JSON-Format. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag. |
> | Aktion | Microsoft.Network/dnszones/AAAA/write | Dient zum Erstellen oder Aktualisieren eines Ressourceneintragssatzes vom Typ „AAAA“ innerhalb einer DNS-Zone. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt. |
> | Aktion | Microsoft.Network/dnszones/all/read | Ruft DNS-Ressourceneintragssätze typenübergreifend ab. |
> | Aktion | Microsoft.Network/dnszones/CAA/delete | Dient zum Entfernen der Datensatzgruppe mit einem bestimmten Namen und dem Typ „CAA“ aus einer DNS-Zone. |
> | Aktion | Microsoft.Network/dnszones/CAA/read | Dient zum Abrufen der Datensatzgruppe vom Typ „CAA“ im JSON-Format. Der Ressourceneintragssatz enthält Gültigkeitsdauer, Tags und ETag. |
> | Aktion | Microsoft.Network/dnszones/CAA/write | Dient zum Erstellen oder Aktualisieren einer Datensatzgruppe vom Typ „CAA“ innerhalb einer DNS-Zone. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt. |
> | Aktion | Microsoft.Network/dnszones/CNAME/delete | Dient zum Entfernen des Ressourceneintragssatzes mit einem bestimmten Namen und dem Typ „CNAME“ aus einer DNS-Zone. |
> | Aktion | Microsoft.Network/dnszones/CNAME/read | Dient zum Abrufen des Ressourceneintragssatzes vom Typ „CNAME“ im JSON-Format. Der Ressourceneintragssatz enthält Gültigkeitsdauer, Tags und ETag. |
> | Aktion | Microsoft.Network/dnszones/CNAME/write | Dient zum Erstellen oder Aktualisieren eines Ressourceneintragssatzes vom Typ „CNAME“ innerhalb einer DNS-Zone. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt. |
> | Aktion | Microsoft.Network/dnszones/delete | Dient zum Löschen der DNS-Zone im JSON-Format. Zu den Zoneneigenschaften zählen „tags“, „etag“, „numberOfRecordSets“ und „maxNumberOfRecordSets“. |
> | Aktion | Microsoft.Network/dnszones/MX/delete | Dient zum Entfernen des Ressourceneintragssatzes mit einem bestimmten Namen und dem Typ „MX“ aus einer DNS-Zone. |
> | Aktion | Microsoft.Network/dnszones/MX/read | Dient zum Abrufen des Ressourceneintragssatzes vom Typ „MX“ im JSON-Format. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag. |
> | Aktion | Microsoft.Network/dnszones/MX/write | Dient zum Erstellen oder Aktualisieren eines Ressourceneintragssatzes vom Typ „MX“ innerhalb einer DNS-Zone. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt. |
> | Aktion | Microsoft.Network/dnszones/NS/delete | Löscht den DNS-Ressourceneintragssatz vom Typ „NS“. |
> | Aktion | Microsoft.Network/dnszones/NS/read | Ruft DNS-Ressourceneintragssätze vom Typ „NS“ ab. |
> | Aktion | Microsoft.Network/dnszones/NS/write | Erstellt oder aktualisiert DNS-Ressourceneintragssätze vom Typ „NS“. |
> | Aktion | Microsoft.Network/dnszones/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellungen für eine DNS-Zone ab. |
> | Aktion | Microsoft.Network/dnszones/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellungen für eine DNS-Zone. |
> | Aktion | Microsoft.Network/dnszones/providers/Microsoft.Insights/metricDefinitions/read | Ruft die Metrikdefinitionen der DNS-Zone ab. |
> | Aktion | Microsoft.Network/dnszones/PTR/delete | Dient zum Entfernen des Ressourceneintragssatzes mit einem bestimmten Namen und dem Typ „PTR“ aus einer DNS-Zone. |
> | Aktion | Microsoft.Network/dnszones/PTR/read | Dient zum Abrufen des Ressourceneintragssatzes vom Typ „PTR“ im JSON-Format. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag. |
> | Aktion | Microsoft.Network/dnszones/PTR/write | Dient zum Erstellen oder Aktualisieren eines Ressourceneintragssatzes vom Typ „PTR“ innerhalb einer DNS-Zone. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt. |
> | Aktion | Microsoft.Network/dnszones/read | Dient zum Abrufen der DNS-Zone im JSON-Format. Zu den Zoneneigenschaften zählen „tags“, „etag“, „numberOfRecordSets“ und „maxNumberOfRecordSets“. Die in der Zone enthaltenen Ressourceneintragssätze werden durch diesen Befehl nicht abgerufen. |
> | Aktion | Microsoft.Network/dnszones/recordsets/read | Ruft DNS-Ressourceneintragssätze typenübergreifend ab. |
> | Aktion | Microsoft.Network/dnszones/SOA/read | Ruft DNS-Ressourceneintragssätze vom Typ „SOA“ ab. |
> | Aktion | Microsoft.Network/dnszones/SOA/write | Erstellt oder aktualisiert DNS-Ressourceneintragssätze vom Typ „SOA“. |
> | Aktion | Microsoft.Network/dnszones/SRV/delete | Dient zum Entfernen des Ressourceneintragssatzes mit einem bestimmten Namen und dem Typ „SRV“ aus einer DNS-Zone. |
> | Aktion | Microsoft.Network/dnszones/SRV/read | Dient zum Abrufen des Ressourceneintragssatzes vom Typ „SRV“ im JSON-Format. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag. |
> | Aktion | Microsoft.Network/dnszones/SRV/write | Dient zum Erstellen oder Aktualisieren von Ressourceneintragssätzen vom Typ „SRV“. |
> | Aktion | Microsoft.Network/dnszones/TXT/delete | Dient zum Entfernen des Ressourceneintragssatzes mit einem bestimmten Namen und dem Typ „TXT“ aus einer DNS-Zone. |
> | Aktion | Microsoft.Network/dnszones/TXT/read | Dient zum Abrufen des Ressourceneintragssatzes vom Typ „TXT“ im JSON-Format. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag. |
> | Aktion | Microsoft.Network/dnszones/TXT/write | Dient zum Erstellen oder Aktualisieren eines Ressourceneintragssatzes vom Typ „TXT“ innerhalb einer DNS-Zone. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt. |
> | Aktion | Microsoft.Network/dnszones/write | Dient zum Erstellen oder Aktualisieren einer DNS-Zone innerhalb einer Ressourcengruppe.  Wird zum Aktualisieren der Tags für eine DNS-Zonenressource verwendet. Mit diesem Befehl können keine Ressourceneintragssätze innerhalb der Zone erstellt oder aktualisiert werden. |
> | Aktion | Microsoft.Network/expressRouteCircuits/authorizations/delete | Löscht eine ExpressRouteCircuit-Autorisierung. |
> | Aktion | Microsoft.Network/expressRouteCircuits/authorizations/read | Ruft eine ExpressRouteCircuit-Autorisierung ab. |
> | Aktion | Microsoft.Network/expressRouteCircuits/authorizations/write | Erstellt eine ExpressRouteCircuit-Autorisierung oder aktualisiert eine vorhandene ExpressRouteCircuit-Autorisierung. |
> | Aktion | Microsoft.Network/expressRouteCircuits/delete | Löscht ein ExpressRouteCircuit-Element. |
> | Aktion | Microsoft.Network/expressRouteCircuits/join/action | Verknüpft eine ExpressRoute-Verbindung. |
> | Aktion | Microsoft.Network/expressRouteCircuits/peerings/arpTables/action | Ruft ein ArpTable-Element für ein ExpressRouteCircuit-Peering ab. |
> | Aktion | Microsoft.Network/expressRouteCircuits/peerings/connections/delete | Löscht eine ExpressRouteCircuit-Verbindung. |
> | Aktion | Microsoft.Network/expressRouteCircuits/peerings/connections/read | Ruft eine ExpressRouteCircuit-Verbindung ab. |
> | Aktion | Microsoft.Network/expressRouteCircuits/peerings/connections/write | Erstellt eine ExpressRouteCircuit-Verbindungsressource oder aktualisiert eine vorhandene ExpressRouteCircuit-Verbindungsressource. |
> | Aktion | Microsoft.Network/expressRouteCircuits/peerings/delete | Löscht ein ExpressRouteCircuit-Peering. |
> | Aktion | Microsoft.Network/expressRouteCircuits/peerings/read | Ruft ein ExpressRouteCircuit-Peering ab. |
> | Aktion | Microsoft.Network/expressRouteCircuits/peerings/routeTables/action | Ruft ein RouteTable-Element für ein ExpressRouteCircuit-Peering ab. |
> | Aktion | Microsoft.Network/expressRouteCircuits/peerings/routeTablesSummary/action | Ruft eine RouteTable-Zusammenfassung für ein ExpressRouteCircuit-Peering ab. |
> | Aktion | Microsoft.Network/expressRouteCircuits/peerings/stats/read | Ruft eine ExpressRouteCircuit-Peeringstatistik ab. |
> | Aktion | Microsoft.Network/expressRouteCircuits/peerings/write | Erstellt ein ExpressRouteCircuit-Peering oder aktualisiert ein vorhandenes ExpressRouteCircuit-Peering. |
> | Aktion | Microsoft.Network/expressRouteCircuits/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellungen für ExpressRoute-Verbindungen ab. |
> | Aktion | Microsoft.Network/expressRouteCircuits/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellungen für ExpressRoute-Verbindungen. |
> | Aktion | Microsoft.Network/expressRouteCircuits/providers/Microsoft.Insights/logDefinitions/read | Ruft die Ereignisse für ExpressRoute-Verbindungen ab. |
> | Aktion | Microsoft.Network/expressRouteCircuits/providers/Microsoft.Insights/metricDefinitions/read | Ruft die Metrikdefinitionen für ExpressRoute-Verbindungen ab. |
> | Aktion | Microsoft.Network/expressRouteCircuits/read | Dient zum Abrufen eines ExpressRouteCircuit-Elements. |
> | Aktion | Microsoft.Network/expressRouteCircuits/stats/read | Ruft eine ExpressRouteCircuit-Statistik ab. |
> | Aktion | Microsoft.Network/expressRouteCircuits/write | Erstellt ein ExpressRouteCircuit-Element oder aktualisiert ein vorhandenes ExpressRouteCircuit-Element. |
> | Aktion | Microsoft.Network/expressRouteCrossConnections/delete | Löscht eine ExpressRoute-Querverbindung |
> | Aktion | Microsoft.Network/expressRouteCrossConnections/join/action | Verknüpft eine ExpressRoute-Querverbindung |
> | Aktion | Microsoft.Network/expressRouteCrossConnections/peerings/arpTables/action | Ruft eine ARP-Tabelle zum Peering einer ExpressRoute-Querverbindung ab. |
> | Aktion | Microsoft.Network/expressRouteCrossConnections/peerings/delete | Löscht ein Peering einer ExpressRoute-Querverbindung. |
> | Aktion | Microsoft.Network/expressRouteCrossConnections/peerings/read | Ruft ein Peering einer ExpressRoute-Querverbindung ab. |
> | Aktion | Microsoft.Network/expressRouteCrossConnections/peerings/routeTables/action | Ruft eine Routentabelle zum Peering einer ExpressRoute-Querverbindung ab. |
> | Aktion | Microsoft.Network/expressRouteCrossConnections/peerings/routeTableSummary/action | Ruft eine Routentabellenzusammenfassung zum Peering einer ExpressRoute-Querverbindung ab. |
> | Aktion | Microsoft.Network/expressRouteCrossConnections/peerings/write | Erstellt ein Peering für eine ExpressRoute-Querverbindung oder aktualisiert ein Peering für eine vorhandene ExpressRoute-Querverbindung. |
> | Aktion | Microsoft.Network/expressRouteCrossConnections/read | Ruft eine ExpressRoute-Querverbindung ab. |
> | Aktion | Microsoft.Network/expressRouteCrossConnections/write | Erstellt oder aktualisiert eine ExpressRoute-Querverbindung. |
> | Aktion | Microsoft.Network/expressRouteServiceProviders/read | Ruft ExpressRoute-Dienstanbieter ab. |
> | Aktion | Microsoft.Network/intendedPolicies/delete | Löscht eine gewünschte Richtlinie. |
> | Aktion | Microsoft.Network/intendedPolicies/read | Ruft eine Beschreibung für eine gewünschte Richtlinie ab. |
> | Aktion | Microsoft.Network/intendedPolicies/write | Erstellt eine gewünschte Richtlinie oder aktualisiert eine vorhandene. |
> | Aktion | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Verknüpft einen Back-End-Adresspool für den Lastenausgleich. |
> | Aktion | Microsoft.Network/loadBalancers/backendAddressPools/read | Ruft eine Back-End-Adresspooldefinition für den Lastenausgleich ab. |
> | Aktion | Microsoft.Network/loadBalancers/delete | Löscht einen Lastenausgleich. |
> | Aktion | Microsoft.Network/loadBalancers/frontendIPConfigurations/read | Ruft eine Front-End-IP-Konfigurationsdefinition für den Lastenausgleich ab. |
> | Aktion | Microsoft.Network/loadBalancers/inboundNatPools/join/action | Verknüpft einen eingehenden NAT-Pool für den Lastenausgleich. |
> | Aktion | Microsoft.Network/loadBalancers/inboundNatPools/read | Ruft eine eingehende NAT-Pooldefinition für den Lastenausgleich ab. |
> | Aktion | Microsoft.Network/loadBalancers/inboundNatRules/delete | Löscht eine eingehende NAT-Regel für den Lastenausgleich. |
> | Aktion | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Verknüpft eine eingehende NAT-Regel für den Lastenausgleich. |
> | Aktion | Microsoft.Network/loadBalancers/inboundNatRules/read | Ruft eine eingehende NAT-Regeldefinition für den Lastenausgleich ab. |
> | Aktion | Microsoft.Network/loadBalancers/inboundNatRules/write | Erstellt eine eingehende NAT-Regel für den Lastenausgleich oder aktualisiert eine vorhandene eingehende NAT-Regel für den Lastenausgleich. |
> | Aktion | Microsoft.Network/loadBalancers/loadBalancingRules/read | Ruft eine Lastenausgleichsregel-Definition für den Lastenausgleich ab. |
> | Aktion | Microsoft.Network/loadBalancers/networkInterfaces/read | Ruft Verweise auf alle Netzwerkschnittstellen unter einem Lastenausgleich ab. |
> | Aktion | Microsoft.Network/loadBalancers/outboundNatRules/read | Ruft eine ausgehende NAT-Regeldefinition für den Lastenausgleich ab. |
> | Aktion | Microsoft.Network/loadBalancers/probes/join/action | Ermöglicht die Verwendung von Prüfpunkten eines Lastenausgleichs. Beispielsweise kann mit dieser Berechtigung die healthProbe-Eigenschaft einer VM-Skalierungsgruppe auf den Prüfpunkt verweisen. |
> | Aktion | Microsoft.Network/loadBalancers/probes/read | Ruft einen Lastenausgleichstest ab. |
> | Aktion | Microsoft.Network/loadBalancers/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellungen für den Lastenausgleich ab. |
> | Aktion | Microsoft.Network/loadBalancers/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellungen für den Lastenausgleich. |
> | Aktion | Microsoft.Network/loadBalancers/providers/Microsoft.Insights/logDefinitions/read | Ruft die Ereignisse für den Lastenausgleich ab. |
> | Aktion | Microsoft.Network/loadBalancers/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für den Lastenausgleich ab. |
> | Aktion | Microsoft.Network/loadBalancers/read | Ruft eine Lastenausgleichsdefinition ab. |
> | Aktion | Microsoft.Network/loadBalancers/virtualMachines/read | Ruft Verweise auf alle virtuellen Computer unter einem Lastenausgleich ab. |
> | Aktion | Microsoft.Network/loadBalancers/write | Erstellt einen Lastenausgleich oder aktualisiert einen vorhandenen Lastenausgleich. |
> | Aktion | Microsoft.Network/localnetworkgateways/delete | Löscht lokale Netzwerkgateways. |
> | Aktion | Microsoft.Network/localnetworkgateways/read | Ruft lokale Netzwerkgateways ab. |
> | Aktion | Microsoft.Network/localnetworkgateways/write | Erstellt ein lokales Netzwerkgateway oder aktualisiert ein vorhandenes lokales Netzwerkgateway. |
> | Aktion | Microsoft.Network/locations/availableDelegations/read | Ruft verfügbare Delegierungen ab. |
> | Aktion | Microsoft.Network/locations/checkAcceleratedNetworkingSupport/action | Überprüft die Unterstützung für beschleunigte Netzwerke. |
> | Aktion | Microsoft.Network/locations/checkDnsNameAvailability/read | Prüft, ob eine DNS-Bezeichnung am angegebenen Standort verfügbar ist. |
> | Aktion | Microsoft.Network/locations/operationResults/read | Ruft das Ergebnis eines asynchronen POST- oder DELETE-Vorgangs ab. |
> | Aktion | Microsoft.Network/locations/operations/read | Ruft die Vorgangsressource ab, die den Status eines asynchronen Vorgangs darstellt. |
> | Aktion | Microsoft.Network/locations/supportedVirtualMachineSizes/read | Ruft die unterstützten Größen für virtuelle Computer ab. |
> | Aktion | Microsoft.Network/locations/usages/read | Ruft die Ressourcenverwendungsmetriken ab. |
> | Aktion | Microsoft.Network/locations/virtualNetworkAvailableEndpointServices/read | Ruft eine Liste der verfügbaren Dienstendpunkte für virtuelle Netzwerke ab. |
> | Aktion | Microsoft.Network/networkInterfaces/delete | Löscht eine Netzwerkschnittstelle. |
> | Aktion | Microsoft.Network/networkInterfaces/diagnosticIdentity/read | Ruft die Diagnoseidentität der Ressource ab. |
> | Aktion | Microsoft.Network/networkInterfaces/effectiveNetworkSecurityGroups/action | Dient zum Abrufen der Netzwerksicherheitsgruppen, die für die Netzwerkschnittstelle des virtuellen Computers konfiguriert sind. |
> | Aktion | Microsoft.Network/networkInterfaces/effectiveRouteTable/action | Dient zum Abrufen der Routingtabelle, die für die Netzwerkschnittstelle des virtuellen Computers konfiguriert ist. |
> | Aktion | Microsoft.Network/networkInterfaces/ipconfigurations/read | Ruft eine IP-Konfigurationsdefinition für Netzwerkschnittstellen ab.  |
> | Aktion | Microsoft.Network/networkInterfaces/join/action | Verknüpft einen virtuellen Computer mit einer Netzwerkschnittstelle. |
> | Aktion | Microsoft.Network/networkInterfaces/joinViaPrivateIp/action | Verknüpft eine Ressource mit einer Netzwerkschnittstelle über eine Dienstzuordnung. |
> | Aktion | Microsoft.Network/networkInterfaces/loadBalancers/read | Ruft alle Lastenausgleichsmodule ab, denen die Netzwerkschnittstelle angehört. |
> | Aktion | Microsoft.Network/networkInterfaces/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für die Netzwerkschnittstelle ab. |
> | Aktion | Microsoft.Network/networkInterfaces/read | Ruft eine Netzwerkschnittstellendefinition ab.  |
> | Aktion | Microsoft.Network/networkInterfaces/serviceAssociations/delete | Löscht eine Dienstzuordnung. |
> | Aktion | Microsoft.Network/networkInterfaces/serviceAssociations/read | Ruft die Definition für eine Dienstzuordnung ab. |
> | Aktion | Microsoft.Network/networkInterfaces/serviceAssociations/validate/action | Überprüft eine Dienstzuordnung. |
> | Aktion | Microsoft.Network/networkInterfaces/serviceAssociations/write | Erstellt eine neue Dienstzuordnung oder bearbeitet eine vorhandene. |
> | Aktion | Microsoft.Network/networkInterfaces/write | Erstellt eine Netzwerkschnittstelle oder aktualisiert eine vorhandene Netzwerkschnittstelle.  |
> | Aktion | Microsoft.Network/networkSecurityGroups/defaultSecurityRules/read | Ruft eine Standardsicherheitsregel-Definition ab. |
> | Aktion | Microsoft.Network/networkSecurityGroups/delete | Löscht eine Netzwerksicherheitsgruppe. |
> | Aktion | Microsoft.Network/networkSecurityGroups/join/action | Verknüpft eine Netzwerksicherheitsgruppe. |
> | Aktion | Microsoft.Network/networksecuritygroups/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellungen für Netzwerksicherheitsgruppen ab. |
> | Aktion | Microsoft.Network/networksecuritygroups/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellungen für Netzwerksicherheitsgruppen. Dieser Vorgang wird durch den Insights-Ressourcenanbieter unterstützt. |
> | Aktion | Microsoft.Network/networksecuritygroups/providers/Microsoft.Insights/logDefinitions/read | Ruft die Ereignisse für Netzwerksicherheitsgruppen ab. |
> | Aktion | Microsoft.Network/networkSecurityGroups/read | Ruft eine Netzwerksicherheitsgruppen-Definition ab. |
> | Aktion | Microsoft.Network/networkSecurityGroups/securityRules/delete | Löscht eine Sicherheitsregel. |
> | Aktion | Microsoft.Network/networkSecurityGroups/securityRules/read | Ruft eine Sicherheitsregeldefinition ab. |
> | Aktion | Microsoft.Network/networkSecurityGroups/securityRules/write | Erstellt eine Sicherheitsregel oder aktualisiert eine vorhandene Sicherheitsregel. |
> | Aktion | Microsoft.Network/networkSecurityGroups/write | Erstellt eine Netzwerksicherheitsgruppe oder aktualisiert eine vorhandene Netzwerksicherheitsgruppe. |
> | Aktion | Microsoft.Network/networkWatchers/availableProvidersList/action | Gibt alle verfügbaren Internetdienstanbieter für eine bestimmte Azure-Region zurück. |
> | Aktion | Microsoft.Network/networkWatchers/azureReachabilityReport/action | Gibt das relative Latenzergebnis für Internetdienstanbieter zwischen einem bestimmten Standort und Azure-Regionen zurück. |
> | Aktion | Microsoft.Network/networkWatchers/configureFlowLog/action | Konfiguriert die Datenflussprotokollierung für eine Zielressource. |
> | Aktion | Microsoft.Network/networkWatchers/connectionMonitors/delete | Löscht einen Verbindungsmonitor. |
> | Aktion | Microsoft.Network/networkWatchers/connectionMonitors/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellungen des Verbindungsmonitors ab. |
> | Aktion | Microsoft.Network/networkWatchers/connectionMonitors/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellungen des Verbindungsmonitors. |
> | Aktion | Microsoft.Network/networkWatchers/connectionMonitors/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für den Verbindungsmonitor ab. |
> | Aktion | Microsoft.Network/networkWatchers/connectionMonitors/query/action | Fragt die Überwachungskonnektivität zwischen angegebenen Endpunkten ab. |
> | Aktion | Microsoft.Network/networkWatchers/connectionMonitors/read | Ruft Details zum Verbindungsmonitor ab. |
> | Aktion | Microsoft.Network/networkWatchers/connectionMonitors/start/action | Startet die Überwachungskonnektivität zwischen angegebenen Endpunkten. |
> | Aktion | Microsoft.Network/networkWatchers/connectionMonitors/stop/action | Startet oder beendet die Überwachungskonnektivität zwischen angegebenen Endpunkten. |
> | Aktion | Microsoft.Network/networkWatchers/connectionMonitors/write | Erstellt einen Verbindungsmonitor. |
> | Aktion | Microsoft.Network/networkWatchers/connectivityCheck/action | Überprüft die Möglichkeit, eine direkte TCP-Verbindung zwischen einem virtuellen Computer und einem angegebenen Endpunkt herzustellen, einschließlich mit anderen VMs oder einem freien Remoteserver. |
> | Aktion | Microsoft.Network/networkWatchers/delete | Löscht eine Network Watcher-Instanz. |
> | Aktion | Microsoft.Network/networkWatchers/ipFlowVerify/action | Gibt zurück, ob das Paket an oder von einem bestimmten Ziel zugelassen oder abgelehnt wird. |
> | Aktion | Microsoft.Network/networkWatchers/lenses/delete | Löscht einen Bereich. |
> | Aktion | Microsoft.Network/networkWatchers/lenses/query/action | Fragt die Überwachung des Netzwerkdatenverkehrs für einen angegebenen Endpunkt ab. |
> | Aktion | Microsoft.Network/networkWatchers/lenses/read | Ruft Bereichsdetails ab. |
> | Aktion | Microsoft.Network/networkWatchers/lenses/start/action | Startet die Überwachung des Netzwerkdatenverkehrs für einen angegebenen Endpunkt. |
> | Aktion | Microsoft.Network/networkWatchers/lenses/stop/action | Beendet die Überwachung des Netzwerkdatenverkehrs für einen angegebenen Endpunkt oder hält diese an. |
> | Aktion | Microsoft.Network/networkWatchers/lenses/write | Erstellt einen Bereich. |
> | Aktion | Microsoft.Network/networkWatchers/nextHop/action | Gibt für ein angegebenes Ziel und eine Ziel-IP-Adresse die Art des nächsten Hops und die IP-Adresse des nächsten Hops zurück. |
> | Aktion | Microsoft.Network/networkWatchers/packetCaptures/delete | Löscht eine Paketerfassung. |
> | Aktion | Microsoft.Network/networkWatchers/packetCaptures/queryStatus/action | Ruft Informationen zu Eigenschaften und zum Status einer Paketerfassungsressource ab. |
> | Aktion | Microsoft.Network/networkWatchers/packetCaptures/read | Dient zum Abrufen der Paketerfassungsdefinition. |
> | Aktion | Microsoft.Network/networkWatchers/packetCaptures/stop/action | Dient zum Beenden der laufenden Paketerfassungssitzung. |
> | Aktion | Microsoft.Network/networkWatchers/packetCaptures/write | Erstellt eine Paketerfassung. |
> | Aktion | Microsoft.Network/networkWatchers/queryFlowLogStatus/action | Ruft den Status der Datenflussprotokollierung für eine Ressource ab. |
> | Aktion | Microsoft.Network/networkWatchers/queryTroubleshootResult/action | Ruft das Ergebnis des zuvor oder momentan ausgeführten Problembehandlungsvorgangs ab. |
> | Aktion | Microsoft.Network/networkWatchers/read | Dient zum Abrufen der Network Watcher-Definition. |
> | Aktion | Microsoft.Network/networkWatchers/securityGroupView/action | Dient zum Anzeigen der konfigurierten und effektiven Netzwerksicherheitsgruppen-Regeln, die auf einen virtuellen Computer angewendet wurden. |
> | Aktion | Microsoft.Network/networkWatchers/topology/action | Ruft eine Netzwerkebenenansicht mit Ressourcen und deren Beziehungen in einer Ressourcengruppe ab. |
> | Aktion | Microsoft.Network/networkWatchers/troubleshoot/action | Startet die Problembehandlung für eine Netzwerkressource in Azure. |
> | Aktion | Microsoft.Network/networkWatchers/write | Erstellt eine Network Watcher-Instanz oder aktualisiert eine vorhandene Network Watcher-Instanz. |
> | Aktion | Microsoft.Network/operations/read | Dient zum Abrufen verfügbarer Vorgänge. |
> | Aktion | Microsoft.Network/publicIPAddresses/delete | Löscht eine öffentliche IP-Adresse. |
> | Aktion | Microsoft.Network/publicIPAddresses/join/action | Verknüpft eine öffentliche IP-Adresse. |
> | Aktion | Microsoft.Network/publicIPAddresses/loadBalancerPools/delete | Löscht einen Back-End-Pool für den Lastenausgleich von öffentlichen IP-Adressen. |
> | Aktion | Microsoft.Network/publicIPAddresses/loadBalancerPools/join/action | Verknüpft einen Back-End-Pool für den Lastenausgleich von öffentlichen IP-Adressen. |
> | Aktion | Microsoft.Network/publicIPAddresses/loadBalancerPools/read | Ruft die Definition eines Back-End-Pools für den Lastenausgleich von öffentlichen IP-Adressen ab. |
> | Aktion | Microsoft.Network/publicIPAddresses/loadBalancerPools/write | Erstellt einen Back-End-Pool für den Lastenausgleich von öffentlichen IP-Adressen oder aktualisiert einen vorhandenen. |
> | Aktion | Microsoft.Network/publicIPAddresses/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellungen der öffentlichen IP-Adresse ab. |
> | Aktion | Microsoft.Network/publicIPAddresses/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellungen der öffentlichen IP-Adresse. |
> | Aktion | Microsoft.Network/publicIPAddresses/providers/Microsoft.Insights/logDefinitions/read | Ruft die Protokolldefinitionen der öffentlichen IP-Adresse ab. |
> | Aktion | Microsoft.Network/publicIPAddresses/providers/Microsoft.Insights/metricDefinitions/read | Ruft die Metrikdefinitionen der öffentlichen IP-Adresse ab. |
> | Aktion | Microsoft.Network/publicIPAddresses/read | Ruft eine Definition für eine öffentliche IP-Adresse ab. |
> | Aktion | Microsoft.Network/publicIPAddresses/write | Erstellt eine öffentliche IP-Adresse oder aktualisiert eine vorhandene öffentliche IP-Adresse.  |
> | Aktion | Microsoft.Network/register/action | Registriert das Abonnement. |
> | Aktion | Microsoft.Network/routeFilters/delete | Löscht eine Routenfilterdefinition. |
> | Aktion | Microsoft.Network/routeFilters/join/action | Verknüpft einen Routenfilter. |
> | Aktion | Microsoft.Network/routeFilters/read | Ruft eine Routenfilterdefinition ab. |
> | Aktion | Microsoft.Network/routeFilters/routeFilterRules/delete | Löscht eine Routenfilterregel-Definition. |
> | Aktion | Microsoft.Network/routeFilters/routeFilterRules/read | Ruft eine Routenfilterregel-Definition ab. |
> | Aktion | Microsoft.Network/routeFilters/routeFilterRules/write | Erstellt eine Routenfilterregel oder aktualisiert eine vorhandene Routenfilterregel. |
> | Aktion | Microsoft.Network/routeFilters/write | Erstellt einen Routenfilter oder aktualisiert einen vorhandenen Routenfilter. |
> | Aktion | Microsoft.Network/routeTables/delete | Löscht eine Routingtabellendefinition. |
> | Aktion | Microsoft.Network/routeTables/join/action | Verknüpft eine Routingtabelle. |
> | Aktion | Microsoft.Network/routeTables/read | Ruft eine Routingtabellendefinition ab. |
> | Aktion | Microsoft.Network/routeTables/routes/delete | Löscht eine Routendefinition. |
> | Aktion | Microsoft.Network/routeTables/routes/read | Ruft eine Routendefinition ab. |
> | Aktion | Microsoft.Network/routeTables/routes/write | Erstellt eine Route oder aktualisiert eine vorhandene Route. |
> | Aktion | Microsoft.Network/routeTables/write | Erstellt eine Routingtabelle oder aktualisiert eine vorhandene Routingtabelle. |
> | Aktion | Microsoft.Network/securegateways/applicationRuleCollections/delete | Löscht eine Collection mit Anwendungsregeln für ein sicheres Gateway. |
> | Aktion | Microsoft.Network/securegateways/applicationRuleCollections/read | Ruft eine Collection mit Anwendungsregeln für ein sicheres Gateway ab. |
> | Aktion | Microsoft.Network/securegateways/applicationRuleCollections/write | Erstellt oder aktualisiert eine Collection mit Anwendungsregeln für ein sicheres Gateway. |
> | Aktion | Microsoft.Network/securegateways/delete | Löscht ein sicheres Gateway. |
> | Aktion | Microsoft.Network/securegateways/networkRuleCollections/delete | Löscht eine Collection mit Netzwerkregeln für ein sicheres Gateway. |
> | Aktion | Microsoft.Network/securegateways/networkRuleCollections/read | Ruft eine Collection mit Netzwerkregeln für ein angegebenes sicheres Gateway ab. |
> | Aktion | Microsoft.Network/securegateways/networkRuleCollections/write | Erstellt oder aktualisiert eine Collection mit Netzwerkregeln für ein sicheres Gateway. |
> | Aktion | Microsoft.Network/securegateways/read | Ruft ein sicheres Gateway ab. |
> | Aktion | Microsoft.Network/securegateways/write | Erstellt oder aktualisiert ein sicheres Gateway. |
> | Aktion | Microsoft.Network/serviceEndpointPolicies/delete | Löscht eine Dienstendpunktrichtlinie. |
> | Aktion | Microsoft.Network/serviceEndpointPolicies/join/action | Verknüpft eine Dienstendpunktrichtlinie. |
> | Aktion | Microsoft.Network/serviceEndpointPolicies/joinSubnet/action | Verknüpft ein Subnetz mit Dienstendpunktrichtlinien. |
> | Aktion | Microsoft.Network/serviceEndpointPolicies/read | Ruft die Beschreibung einer Dienstendpunktrichtlinie ab. |
> | Aktion | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/delete | Löscht die Definition einer Dienstendpunktrichtlinie. |
> | Aktion | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/read | Ruft die Beschreibung für die Definition einer Dienstendpunktrichtlinie ab. |
> | Aktion | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/write | Erstellt eine Definition für eine Dienstendpunktrichtlinie oder aktualisiert eine vorhandene Definition für eine Dienstendpunktrichtlinie. |
> | Aktion | Microsoft.Network/serviceEndpointPolicies/write | Erstellt eine Dienstendpunktrichtlinie oder aktualisiert eine vorhandene Dienstendpunktrichtlinie. |
> | Aktion | Microsoft.Network/trafficManagerGeographicHierarchies/read | Ruft die geografische Traffic Manager-Hierarchie mit Regionen ab, die mit der geografischen Routingmethode für Datenverkehr verwendet werden können. |
> | Aktion | Microsoft.Network/trafficManagerProfiles/azureEndpoints/delete | Löscht einen Azure-Endpunkt aus einem vorhandenen Traffic Manager-Profil an. Der Traffic Manager beendet die Weiterleitung des Datenverkehrs an den gelöschten Azure-Endpunkt. |
> | Aktion | Microsoft.Network/trafficManagerProfiles/azureEndpoints/read | Ruft einen Azure-Endpunkt ab, der einem Traffic Manager-Profil zugehörig ist, einschließlich aller Eigenschaften dieses Azure-Endpunkts. |
> | Aktion | Microsoft.Network/trafficManagerProfiles/azureEndpoints/write | Fügt einen neuen Azure-Endpunkt in einem vorhandenen Traffic Manager-Profil hinzu oder aktualisiert die Eigenschaften eines vorhandenen Azure-Endpunkts in diesem Traffic Manager-Profil. |
> | Aktion | Microsoft.Network/trafficManagerProfiles/delete | Dient zum Löschen des Traffic Manager-Profils. Alle dem Traffic Manager-Profil zugeordneten Einstellungen gehen verloren, und das Profil kann nicht mehr zum Routen von Datenverkehr verwendet werden. |
> | Aktion | Microsoft.Network/trafficManagerProfiles/externalEndpoints/delete | Löscht einen externen Endpunkt aus einem vorhandenen Traffic Manager-Profil. Der Traffic Manager beendet die Weiterleitung des Datenverkehrs an den gelöschten externen Endpunkt. |
> | Aktion | Microsoft.Network/trafficManagerProfiles/externalEndpoints/read | Ruft einen externen Endpunkt ab, der einem Traffic Manager-Profil zugehörig ist, einschließlich aller Eigenschaften dieses externen Endpunkts. |
> | Aktion | Microsoft.Network/trafficManagerProfiles/externalEndpoints/write | Fügt einen neuen externen Endpunkt in einem vorhandenen Traffic Manager-Profil hinzu oder aktualisiert die Eigenschaften eines vorhandenen externen Endpunkts in diesem Traffic Manager-Profil. |
> | Aktion | Microsoft.Network/trafficManagerProfiles/heatMaps/read | Ruft die Traffic Manager-Heatmap für das angegebene Traffic Manager-Profil ab, das die Anzahl der Abfragen und Latenzdaten nach Standort und Quell-IP enthält. |
> | Aktion | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/delete | Löscht einen geschachtelten Endpunkt aus einem vorhandenen Traffic Manager-Profil. Der Traffic Manager beendet die Weiterleitung des Datenverkehrs an den gelöschten geschachtelten Endpunkt. |
> | Aktion | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/read | Ruft einen geschachtelten Endpunkt ab, der einem Traffic Manager-Profil zugehörig ist, einschließlich aller Eigenschaften dieses geschachtelten Endpunkts. |
> | Aktion | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/write | Fügt einen neuen geschachtelten Endpunkt in einem vorhandenen Traffic Manager-Profil hinzu oder aktualisiert die Eigenschaften eines vorhandenen geschachtelten Endpunkts in diesem Traffic Manager-Profil. |
> | Aktion | Microsoft.Network/trafficManagerProfiles/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellungen für den Traffic Manager ab. |
> | Aktion | Microsoft.Network/trafficManagerProfiles/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellungen für den Traffic Manager. Dieser Vorgang wird durch den Insights-Ressourcenanbieter unterstützt. |
> | Aktion | Microsoft.Network/trafficManagerProfiles/providers/Microsoft.Insights/logDefinitions/read | Ruft die Ereignisse für den Traffic Manager ab. |
> | Aktion | Microsoft.Network/trafficManagerProfiles/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für den Traffic Manager ab. |
> | Aktion | Microsoft.Network/trafficManagerProfiles/read | Dient zum Abrufen der Traffic Manager-Profilkonfiguration. Dazu zählen DNS-Einstellungen, Routingeinstellungen für Datenverkehr, Endpunktüberwachungseinstellungen und die Liste mit den Endpunkten, die durch dieses Traffic Manager-Profil geroutet werden. |
> | Aktion | Microsoft.Network/trafficManagerProfiles/write | Dient zum Erstellen eines Traffic Manager-Profils oder zum Ändern der Konfiguration eines vorhandenen Traffic Manager-Profils.<br>Dies schließt das Aktivieren oder Deaktivieren eines Profils sowie das Ändern von DNS-Einstellungen, Routingeinstellungen für Datenverkehr oder Endpunktüberwachungseinstellungen ein.<br>Durch das Traffic Manager-Profil geroutete Endpunkte können hinzugefügt, entfernt, aktiviert oder deaktiviert werden. |
> | Aktion | Microsoft.Network/trafficManagerUserMetricsKeys/delete | Löscht die Schlüssel auf Abonnementebene, die für die Collection für Echtzeitbenutzermetriken verwendet wird. |
> | Aktion | Microsoft.Network/trafficManagerUserMetricsKeys/read | Ruft die Schlüssel auf Abonnementebene für die Collection mit Echtzeitbenutzermetriken ab. |
> | Aktion | Microsoft.Network/trafficManagerUserMetricsKeys/write | Erstellt einen neuen Schlüssel auf Abonnementebene, der für die Collection mit Echtzeitbenutzermetriken verwendet werden soll. |
> | Aktion | Microsoft.Network/unregister/action | Hebt die Registrierung des Abonnements auf. |
> | Aktion | Microsoft.Network/virtualHubs/delete | Löscht einen virtuellen Hub. |
> | Aktion | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/delete | Löscht HubVirtualNetworkConnection. |
> | Aktion | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/read | Ruft HubVirtualNetworkConnection ab. |
> | Aktion | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/write | Erstellt oder aktualisiert HubVirtualNetworkConnection. |
> | Aktion | Microsoft.Network/virtualHubs/read | Ruft einen virtuellen Hub ab. |
> | Aktion | Microsoft.Network/virtualHubs/write | Erstellt oder aktualisiert einen virtuellen Hub. |
> | Aktion | microsoft.network/virtualnetworkgateways/connections/read | Ruft VirtualNetworkGatewayConnection ab. |
> | Aktion | Microsoft.Network/virtualNetworkGateways/delete | Löscht virtualNetworkGateway. |
> | Aktion | microsoft.network/virtualnetworkgateways/generatevpnclientpackage/action | Generiert ein VpnClient-Paket für virtualNetworkGateway. |
> | Aktion | microsoft.network/virtualnetworkgateways/generatevpnprofile/action | Generiert ein VpnProfile-Paket für VirtualNetworkGateway. |
> | Aktion | microsoft.network/virtualnetworkgateways/getadvertisedroutes/action | Ruft angekündigte virtualNetworkGateway-Routen ab. |
> | Aktion | microsoft.network/virtualnetworkgateways/getbgppeerstatus/action | Ruft den Status des BGP-Peerings für virtualNetworkGateway ab. |
> | Aktion | microsoft.network/virtualnetworkgateways/getlearnedroutes/action | Ruft die erlernten virtualNetworkGateway-Routen ab. |
> | Aktion | microsoft.network/virtualnetworkgateways/getvpnclientipsecparameters/action | Ruft die Vpnclient-IPsec-Parameter für den VirtualNetworkGateway P2S-Client ab. |
> | Aktion | microsoft.network/virtualnetworkgateways/getvpnprofilepackageurl/action | Ruft die URL eines vorab generierten VPN-Clientprofilpakets ab. |
> | Aktion | Microsoft.Network/virtualNetworkGateways/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellungen für das Gateway des virtuellen Netzwerks ab. |
> | Aktion | Microsoft.Network/virtualNetworkGateways/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellungen für das Gateway des virtuellen Netzwerks. Dieser Vorgang wird durch den Insights-Ressourcenanbieter unterstützt. |
> | Aktion | Microsoft.Network/virtualNetworkGateways/providers/Microsoft.Insights/logDefinitions/read | Ruft die Ereignisse für das Gateway des virtuellen Netzwerks ab. |
> | Aktion | Microsoft.Network/virtualNetworkGateways/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für das Gateway des virtuellen Netzwerks ab. |
> | Aktion | Microsoft.Network/virtualNetworkGateways/read | Ruft VirtualNetworkGateway ab. |
> | Aktion | microsoft.network/virtualnetworkgateways/reset/action | Setzt virtualNetworkGateway zurück. |
> | Aktion | microsoft.network/virtualnetworkgateways/setvpnclientipsecparameters/action | Legt die Vpnclient-IPsec-Parameter für den VirtualNetworkGateway P2S-Client fest. |
> | Aktion | Microsoft.Network/virtualnetworkgateways/supportedvpndevices/action | Listet die unterstützten VPN-Geräte auf. |
> | Aktion | Microsoft.Network/virtualNetworkGateways/write | Erstellt oder aktualisiert VirtualNetworkGateway. |
> | Aktion | Microsoft.Network/virtualNetworks/checkIpAddressAvailability/read | Prüft, ob eine IP-Adresse im angegebenen virtuellen Netzwerk verfügbar ist. |
> | Aktion | Microsoft.Network/virtualNetworks/customViews/get/action | Ruft den Inhalt einer benutzerdefinierten Ansicht eines virtuellen Netzwerks ab. |
> | Aktion | Microsoft.Network/virtualNetworks/customViews/read | Ruft die Definition einer benutzerdefinierten Ansicht des virtuellen Netzwerks ab. |
> | Aktion | Microsoft.Network/virtualNetworks/delete | Löscht ein virtuelles Netzwerk. |
> | Aktion | Microsoft.Network/virtualNetworks/peer/action | Richtet ein virtuelles Netzwerk als Peer für ein anderes virtuelles Netzwerk ein. |
> | Aktion | Microsoft.Network/virtualNetworks/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellungen des virtuellen Netzwerks ab. |
> | Aktion | Microsoft.Network/virtualNetworks/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellungen des virtuellen Netzwerks. |
> | Aktion | Microsoft.Network/virtualNetworks/providers/Microsoft.Insights/logDefinitions/read | Ruft die Protokolldefinitionen des virtuellen Netzwerks ab. |
> | Aktion | Microsoft.Network/virtualNetworks/read | Dient zum Abrufen der Definition des virtuellen Netzwerks. |
> | Aktion | Microsoft.Network/virtualNetworks/remoteVirtualNetworkPeeringProxies/delete | Löscht den Proxy für das Peering virtueller Netzwerke. |
> | Aktion | Microsoft.Network/virtualNetworks/remoteVirtualNetworkPeeringProxies/read | Ruft eine Proxydefinition für das Peering virtueller Netzwerke ab. |
> | Aktion | Microsoft.Network/virtualNetworks/remoteVirtualNetworkPeeringProxies/write | Erstellt ein Proxy für das Peering virtueller Netzwerke oder aktualisiert ein vorhandenes Proxy für das Peering virtueller Netzwerke. |
> | Aktion | Microsoft.Network/virtualNetworks/subnets/delete | Löscht ein Subnetz für virtuelle Netzwerke. |
> | Aktion | Microsoft.Network/virtualNetworks/subnets/join/action | Verknüpft ein virtuelles Netzwerk. |
> | Aktion | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Verknüpft Ressourcen wie etwa ein Speicherkonto oder eine SQL-Datenbank mit einem Subnetz. |
> | Aktion | Microsoft.Network/virtualNetworks/subnets/read | Ruft eine Subnetzdefinition für virtuelle Netzwerke ab. |
> | Aktion | Microsoft.Network/virtualNetworks/subnets/resourceNavigationLinks/delete | Löscht einen Navigationslink für die Ressource. |
> | Aktion | Microsoft.Network/virtualNetworks/subnets/resourceNavigationLinks/read | Ruft die Definition des Ressourcennavigationslinks ab. |
> | Aktion | Microsoft.Network/virtualNetworks/subnets/resourceNavigationLinks/write | Erstellt einen Ressourcennavigationslink oder aktualisiert einen vorhandenen Ressourcennavigationslink. |
> | Aktion | Microsoft.Network/virtualNetworks/subnets/serviceAssociationLinks/delete | Löscht den Link zu einer Dienstzuordnung. |
> | Aktion | Microsoft.Network/virtualNetworks/subnets/serviceAssociationLinks/details/read | Ruft die Details zum Link für eine Dienstzuordnung ab. |
> | Aktion | Microsoft.Network/virtualNetworks/subnets/serviceAssociationLinks/read | Ruft die Definition zum Link für eine Dienstzuordnung ab. |
> | Aktion | Microsoft.Network/virtualNetworks/subnets/serviceAssociationLinks/validate/action | Überprüft den Link für eine Dienstzuordnung. |
> | Aktion | Microsoft.Network/virtualNetworks/subnets/serviceAssociationLinks/write | Erstellt einen Link für eine Dienstzuordnung oder aktualisiert einen vorhandenen. |
> | Aktion | Microsoft.Network/virtualNetworks/subnets/virtualMachines/read | Ruft Verweise auf alle virtuellen Computer im Subnetz eines virtuellen Netzwerks ab. |
> | Aktion | Microsoft.Network/virtualNetworks/subnets/write | Erstellt ein Subnetz für virtuelle Netzwerke oder aktualisiert ein vorhandenes Subnetz für virtuelle Netzwerke. |
> | Aktion | Microsoft.Network/virtualNetworks/taggedTrafficConsumers/delete | Löscht einen markierten Datenverkehrsconsumer. |
> | Aktion | Microsoft.Network/virtualNetworks/taggedTrafficConsumers/read | Ruft die Definition eines markierten Datenverkehrsconsumers ab. |
> | Aktion | Microsoft.Network/virtualNetworks/taggedTrafficConsumers/validate/action | Überprüft einen markierten Datenverkehrsconsumer. |
> | Aktion | Microsoft.Network/virtualNetworks/taggedTrafficConsumers/write | Erstellt einen markierten Datenverkehrsconsumer oder aktualisiert einen vorhandenen markierten Datenverkehrsconsumer. |
> | Aktion | Microsoft.Network/virtualNetworks/usages/read | Ruft die IP-Adressenbelegungen für jedes Subnetz des virtuellen Netzwerks ab. |
> | Aktion | Microsoft.Network/virtualNetworks/virtualMachines/read | Ruft Verweise auf alle virtuellen Computer in einem virtuellen Netzwerk ab. |
> | Aktion | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/delete | Löscht ein Peering virtueller Netzwerke. |
> | Aktion | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/read | Ruft eine Definition für das Peering virtueller Netzwerke ab. |
> | Aktion | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write | Erstellt ein Peering virtueller Netzwerke oder aktualisiert ein vorhandenes Peering virtueller Netzwerke. |
> | Aktion | Microsoft.Network/virtualNetworks/write | Erstellt ein virtuelles Netzwerk oder aktualisiert ein vorhandenes virtuelles Netzwerk. |
> | Aktion | Microsoft.Network/virtualNetworkTaps/delete | Löscht einen TAP eines virtuellen Netzwerks. |
> | Aktion | Microsoft.Network/virtualNetworkTaps/join/action | Verknüpft einen TAP eines virtuellen Netzwerks. |
> | Aktion | Microsoft.Network/virtualNetworkTaps/read | Ruft einen TAP eines virtuellen Netzwerks ab. |
> | Aktion | Microsoft.Network/virtualNetworkTaps/write | Erstellt oder aktualisiert einen TAP eines virtuellen Netzwerks. |
> | Aktion | Microsoft.Network/virtualWans/delete | Löscht ein virtuelles WAN. |
> | Aktion | Microsoft.Network/virtualWans/read | Ruft ein virtuelles WAN ab. |
> | Aktion | Microsoft.Network/virtualWans/virtualHubProxies/delete | Löscht einen Proxy für einen virtuellen Hub. |
> | Aktion | Microsoft.Network/virtualWans/virtualHubProxies/read | Ruft die Definition für den Proxy eines virtuellen Hubs ab. |
> | Aktion | Microsoft.Network/virtualWans/virtualHubProxies/write | Erstellt oder aktualisiert einen Proxy für einen virtuellen Hub. |
> | Aktion | Microsoft.Network/virtualWans/virtualHubs/read | Ruft alle virtuellen Hubs ab, die auf ein virtuelles WAN verweisen. |
> | Aktion | Microsoft.Network/virtualwans/vpnconfiguration/action | Ruft eine VPN-Konfiguration ab. |
> | Aktion | Microsoft.Network/virtualWans/vpnSiteProxies/delete | Löscht einen Proxy für einen VPN-Standort. |
> | Aktion | Microsoft.Network/virtualWans/vpnSiteProxies/read | Ruft eine Proxydefinition für einen VPN-Standort-ab. |
> | Aktion | Microsoft.Network/virtualWans/vpnSiteProxies/write | Erstellt einen Proxy für einen VPN-Standort oder aktualisiert einen Proxy für einen VPN-Standort. |
> | Aktion | Microsoft.Network/virtualWans/vpnSites/read | Ruft alle VPN-Standorte ab, die auf ein virtuelles WAN verweisen. |
> | Aktion | Microsoft.Network/virtualWans/write | Erstellt oder aktualisiert ein virtuelles WAN. |
> | Aktion | Microsoft.Network/vpnGateways/read | Ruft VpnGateway ab. |
> | Aktion | microsoft.network/vpnGateways/vpnConnections/read | Ruft VpnConnection ab. |
> | Aktion | microsoft.network/vpnGateways/vpnConnections/write | Legt VpnConnection fest. |
> | Aktion | Microsoft.Network/vpnGateways/write | Legt VpnGateway fest. |
> | Aktion | Microsoft.Network/vpnsites/delete | Löscht eine Ressource für einen VPN-Standort. |
> | Aktion | Microsoft.Network/vpnsites/read | Ruft eine Ressource für einen VPN-Standort ab. |
> | Aktion | Microsoft.Network/vpnsites/write | Erstellt oder aktualisiert eine Ressource für einen VPN-Standort. |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.NotificationHubs/CheckNamespaceAvailability/action | Prüft, ob ein bestimmter Name einer Namespaceressource innerhalb des NotificationHub-Diensts verfügbar ist. |
> | Aktion | Microsoft.NotificationHubs/Namespaces/authorizationRules/action | Dient zum Abrufen der Liste mit Beschreibungen für Namespace-Autorisierungsregeln. |
> | Aktion | Microsoft.NotificationHubs/Namespaces/authorizationRules/delete | Dient zum Löschen von Namespace-Autorisierungsregeln. Die standardmäßige Namespace-Autorisierungsregel kann nicht gelöscht werden.  |
> | Aktion | Microsoft.NotificationHubs/Namespaces/authorizationRules/listkeys/action | Dient zum Abrufen der Verbindungszeichenfolge für den Namespace. |
> | Aktion | Microsoft.NotificationHubs/Namespaces/authorizationRules/read | Dient zum Abrufen der Liste mit Beschreibungen für Namespace-Autorisierungsregeln. |
> | Aktion | Microsoft.NotificationHubs/Namespaces/authorizationRules/regenerateKeys/action | Namespace-Autorisierungsregel zum erneuten Generieren des primären/sekundären Schlüssels. Geben Sie den Schlüssel an, der neu generiert werden soll. |
> | Aktion | Microsoft.NotificationHubs/Namespaces/authorizationRules/write | Dient zum Erstellen von Autorisierungsregeln auf Namespaceebene und zum Aktualisieren der dazugehörigen Eigenschaften. Aktualisiert werden können die Zugriffsrechte für Autorisierungsregeln sowie der primäre und der sekundäre Schlüssel. |
> | Aktion | Microsoft.NotificationHubs/Namespaces/CheckNotificationHubAvailability/action | Prüft, ob ein bestimmter NotificationHub-Name innerhalb eines Namespaces verfügbar ist. |
> | Aktion | Microsoft.NotificationHubs/Namespaces/Delete | Dient zum Löschen von Namespaceressourcen. |
> | Aktion | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/action | Dient zum Abrufen der Liste mit Notification Hub-Autorisierungsregeln. |
> | Aktion | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/delete | Dient zum Löschen von Notification Hub-Autorisierungsregeln. |
> | Aktion | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/listkeys/action | Dient zum Abrufen der Notification Hub-Verbindungszeichenfolge. |
> | Aktion | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/read | Dient zum Abrufen der Liste mit Notification Hub-Autorisierungsregeln. |
> | Aktion | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/regenerateKeys/action | Notification Hub-Autorisierungsregel zum erneuten Generieren des primären/sekundären Schlüssels. Geben Sie den Schlüssel an, der neu generiert werden soll. |
> | Aktion | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/write | Dient zum Erstellen von Notification Hub-Autorisierungsregeln und zum Aktualisieren der dazugehörigen Eigenschaften. Aktualisiert werden können die Zugriffsrechte für Autorisierungsregeln sowie der primäre und der sekundäre Schlüssel. |
> | Aktion | Microsoft.NotificationHubs/Namespaces/NotificationHubs/debugSend/action | Dient zum Senden einer Test-Pushbenachrichtigung. |
> | Aktion | Microsoft.NotificationHubs/Namespaces/NotificationHubs/Delete | Dient zum Löschen von Notification Hub-Ressourcen. |
> | Aktion | Microsoft.NotificationHubs/Namespaces/NotificationHubs/metricDefinitions/read | Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacemetriken. |
> | Aktion | Microsoft.NotificationHubs/Namespaces/NotificationHubs/pnsCredentials/action | Dient zum Abrufen aller Notification Hub-PNS-Anmeldeinformationen. Hierzu zählen Anmeldeinformationen für WNS, MPNS, APNS, GCM und Baidu. |
> | Aktion | Microsoft.NotificationHubs/Namespaces/NotificationHubs/read | Dient zum Abrufen einer Liste mit Notification Hub-Ressourcenbeschreibungen. |
> | Aktion | Microsoft.NotificationHubs/Namespaces/NotificationHubs/write | Dient zum Erstellen eines Notification Hubs und zum Aktualisieren seiner Eigenschaften. Zu diesen Einstellungen zählen in erster Linie PNS-Anmeldeinformationen, Autorisierungsregeln und Gültigkeitsdauer. |
> | Aktion | Microsoft.NotificationHubs/Namespaces/read | Dient zum Abrufen der Liste mit Beschreibungen für Namespaceressourcen. |
> | Aktion | Microsoft.NotificationHubs/Namespaces/write | Dient zum Erstellen einer Namespaceressource und zum Aktualisieren ihrer Eigenschaften. Die Eigenschaften für Markierungen und Kapazität des Namespace können aktualisiert werden. |
> | Aktion | Microsoft.NotificationHubs/register/action | Registriert das Abonnement für den NotificiationHubs-Ressourcenanbieter und ermöglicht die Erstellung von Namespaces und Notification Hubs. |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.OperationalInsights/linkTargets/read | Listet vorhandene Konten auf, die keinem Azure-Abonnement zugeordnet sind. Verwenden Sie eine Kunden-ID, die von diesem Vorgang in der Eigenschaft für die Kunden-ID des Vorgangs „Arbeitsbereich erstellen“ zurückgegeben wird, um dieses Azure-Abonnement mit einem Arbeitsbereich zu verknüpfen. |
> | Aktion | Microsoft.OperationalInsights/register/action | Dient zum Registrieren eines Abonnements bei einem Ressourcenanbieter. |
> | Aktion | Microsoft.OperationalInsights/workspaces/analytics/query/action | Führt eine Suche mit der neuen Engine aus. |
> | Aktion | Microsoft.OperationalInsights/workspaces/analytics/query/schema/read | Rufen Sie das Suchschema V2 ab. |
> | Aktion | Microsoft.OperationalInsights/workspaces/api/query/action | Führt eine Suche mit der neuen Engine aus. |
> | Aktion | Microsoft.OperationalInsights/workspaces/api/query/schema/read | Rufen Sie das Suchschema V2 ab. |
> | Aktion | Microsoft.OperationalInsights/workspaces/configurationScopes/delete | Dient zum Löschen von Konfigurationsbereichen. |
> | Aktion | Microsoft.OperationalInsights/workspaces/configurationScopes/read | Dient zum Abrufen von Konfigurationsbereichen. |
> | Aktion | Microsoft.OperationalInsights/workspaces/configurationScopes/write | Dient zum Festlegen von Konfigurationsbereichen. |
> | Aktion | Microsoft.OperationalInsights/workspaces/datasources/delete | Dient zum Löschen von Datenquellen unter einem Arbeitsbereich. |
> | Aktion | Microsoft.OperationalInsights/workspaces/datasources/read | Dient zum Abrufen von Datenquellen unter einem Arbeitsbereich. |
> | Aktion | Microsoft.OperationalInsights/workspaces/datasources/write | Dient zum Erstellen/Aktualisieren von Datenquellen unter einem Arbeitsbereich. |
> | Aktion | Microsoft.OperationalInsights/workspaces/delete | Löscht einen Arbeitsbereich. Wenn der Arbeitsbereich bei seiner Erstellung mit einem vorhandenen Arbeitsbereich verknüpft war, wird der Arbeitsbereich, mit dem er verknüpft war, nicht gelöscht. |
> | Aktion | Microsoft.OperationalInsights/workspaces/generateregistrationcertificate/action | Generiert ein Registrierungszertifikat für den Arbeitsbereich. Dieses Zertifikat wird verwendet, um Microsoft System Center Operation Manager mit dem Arbeitsbereich zu verbinden. |
> | Aktion | Microsoft.OperationalInsights/workspaces/intelligencepacks/disable/action | Deaktiviert ein Intelligence Pack für einen bestimmten Arbeitsbereich. |
> | Aktion | Microsoft.OperationalInsights/workspaces/intelligencepacks/enable/action | Aktiviert ein Intelligence Pack für einen bestimmten Arbeitsbereich. |
> | Aktion | Microsoft.OperationalInsights/workspaces/intelligencepacks/read | Listet alle für einen angegebenen Arbeitsbereich sichtbaren Intelligence Packs auf und gibt an, ob das Pack für diesen Arbeitsbereich aktiviert oder deaktiviert ist. |
> | Aktion | Microsoft.OperationalInsights/workspaces/linkedServices/delete | Löscht verknüpfte Dienste im angegebenen Arbeitsbereich. |
> | Aktion | Microsoft.OperationalInsights/workspaces/linkedServices/read | Ruft verknüpfte Dienste im angegebenen Arbeitsbereich ab. |
> | Aktion | Microsoft.OperationalInsights/workspaces/linkedServices/write | Erstellt oder aktualisiert verknüpfte Dienste im angegebenen Arbeitsbereich. |
> | Aktion | Microsoft.OperationalInsights/workspaces/listKeys/action | Ruft die Listenschlüssel für den Arbeitsbereich ab. Diese Schlüssel werden verwendet, um Microsoft Operational Insights-Agents mit dem Arbeitsbereich zu verbinden. |
> | Aktion | Microsoft.OperationalInsights/workspaces/listKeys/read | Ruft die Listenschlüssel für den Arbeitsbereich ab. Diese Schlüssel werden verwendet, um Microsoft Operational Insights-Agents mit dem Arbeitsbereich zu verbinden. |
> | Aktion | Microsoft.OperationalInsights/workspaces/managementGroups/read | Ruft die Namen und Metadaten für über System Center Operations Manager verwaltete Gruppen ab, die mit diesem Arbeitsbereich verbunden sind. |
> | Aktion | Microsoft.OperationalInsights/workspaces/metricDefinitions/read | Ruft Metrikdefinitionen im Arbeitsbereich ab. |
> | Aktion | Microsoft.OperationalInsights/workspaces/notificationSettings/delete | Löscht die Benachrichtigungseinstellungen für den Arbeitsbereich des Benutzers. |
> | Aktion | Microsoft.OperationalInsights/workspaces/notificationSettings/read | Ruft die Benachrichtigungseinstellungen für den Arbeitsbereich des Benutzers ab. |
> | Aktion | Microsoft.OperationalInsights/workspaces/notificationSettings/write | Legt die Benachrichtigungseinstellungen für den Arbeitsbereich des Benutzers fest. |
> | Aktion | Microsoft.OperationalInsights/workspaces/purge/action | Löscht die angegebenen Daten aus dem Arbeitsbereich. |
> | Aktion | Microsoft.OperationalInsights/workspaces/read | Ruft einen vorhandenen Arbeitsbereich ab. |
> | Aktion | Microsoft.OperationalInsights/workspaces/savedSearches/delete | Löscht eine gespeicherte Suchabfrage. |
> | Aktion | Microsoft.OperationalInsights/workspaces/savedSearches/read | Ruft eine gespeicherte Suchabfrage ab. |
> | Aktion | Microsoft.OperationalInsights/workspaces/savedSearches/write | Erstellt eine gespeicherte Suchabfrage. |
> | Aktion | Microsoft.OperationalInsights/workspaces/schema/read | Ruft das Suchschema für den Arbeitsbereich ab.  Ein Suchschema umfasst die verfügbar gemachten Felder und die dazugehörigen Typen. |
> | Aktion | Microsoft.OperationalInsights/workspaces/search/action | Führt eine Suchabfrage aus. |
> | Aktion | Microsoft.OperationalInsights/workspaces/sharedKeys/action | Ruft die gemeinsam verwendeten Schlüssel für den Arbeitsbereich ab. Diese Schlüssel werden verwendet, um Microsoft Operational Insights-Agents mit dem Arbeitsbereich zu verbinden. |
> | Aktion | Microsoft.OperationalInsights/workspaces/sharedKeys/read | Ruft die gemeinsam verwendeten Schlüssel für den Arbeitsbereich ab. Diese Schlüssel werden verwendet, um Microsoft Operational Insights-Agents mit dem Arbeitsbereich zu verbinden. |
> | Aktion | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/delete | Löscht eine Speicherkonfiguration. Daraufhin werden von Microsoft Operational Insights keine Daten mehr aus dem Speicherkonto gelesen. |
> | Aktion | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/read | Ruft eine Speicherkonfiguration ab. |
> | Aktion | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/write | Erstellt eine neue Speicherkonfiguration. Diese Konfigurationen werden dazu verwendet, Daten von einem Speicherort in einem vorhandenen Speicherkonto abzurufen. |
> | Aktion | Microsoft.OperationalInsights/workspaces/usages/read | Ruft Nutzungsdaten für einen Arbeitsbereich ab, einschließlich der durch den Arbeitsbereich gelesenen Datenmenge. |
> | Aktion | Microsoft.OperationalInsights/workspaces/write | Erstellt einen neuen Arbeitsbereich oder stellt eine Verknüpfung mit einem vorhandenen Arbeitsbereich her, indem die Kunden-ID für den vorhandenen Arbeitsbereich bereitgestellt wird. |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.OperationsManagement/managementAssociations/delete | Löscht eine vorhandene Verwaltungszuordnung. |
> | Aktion | Microsoft.OperationsManagement/managementAssociations/read | Ruft eine vorhandene Verwaltungszuordnung ab. |
> | Aktion | Microsoft.OperationsManagement/managementAssociations/write | Erstellt eine neue Verwaltungszuordnung. |
> | Aktion | Microsoft.OperationsManagement/managementConfigurations/delete | Löscht eine vorhandene Verwaltungskonfiguration. |
> | Aktion | Microsoft.OperationsManagement/managementConfigurations/read | Ruft eine vorhandene Verwaltungskonfiguration ab. |
> | Aktion | Microsoft.OperationsManagement/managementConfigurations/write | Erstellt eine vorhandene Verwaltungskonfiguration. |
> | Aktion | Microsoft.OperationsManagement/register/action | Dient zum Registrieren eines Abonnements bei einem Ressourcenanbieter. |
> | Aktion | Microsoft.OperationsManagement/solutions/delete | Dient zum Löschen vorhandener OMS-Lösungen. |
> | Aktion | Microsoft.OperationsManagement/solutions/read | Dient zum Abrufen vorhandener OMS-Lösungen. |
> | Aktion | Microsoft.OperationsManagement/solutions/write | Dient zum Erstellen neuer OMS-Lösungen. |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.PolicyInsights/policyEvents/queryResults/action | Fragt Informationen zu Richtlinienereignissen ab. |
> | Aktion | Microsoft.PolicyInsights/policyStates/queryResults/action | Fragt Informationen zu Richtlinienzuständen ab. |
> | Aktion | Microsoft.PolicyInsights/policyStates/summarize/action | Fragt zusammenfassende Informationen zu den letzten Richtlinienzuständen ab. |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Portal/dashboards/delete | Entfernt das Dashboard aus dem Abonnement. |
> | Aktion | Microsoft.Portal/dashboards/read | Liest die Dashboards für das Abonnement. |
> | Aktion | Microsoft.Portal/dashboards/write | Fügt ein Dashboard zu einem Abonnement hinzu oder ändert es. |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.PowerBIDedicated/capacities/checkNameAvailability/action | Überprüft, ob der Name für die angegebene Power BI-Kapazität gültig ist und nicht verwendet wird. |
> | Aktion | Microsoft.PowerBIDedicated/capacities/delete | Löscht die dedizierte Power BI-Kapazität. |
> | Aktion | Microsoft.PowerBIDedicated/capacities/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | Microsoft.PowerBIDedicated/capacities/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.PowerBIDedicated/capacities/providers/Microsoft.Insights/logDefinitions/read | Ruft die verfügbaren Protokolle für dedizierte Power BI-Kapazitäten ab. |
> | Aktion | Microsoft.PowerBIDedicated/capacities/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für die dedizierte Power BI-Kapazität ab. |
> | Aktion | Microsoft.PowerBIDedicated/capacities/read | Ruft die Informationen der angegebenen dedizierten Power BI-Kapazität ab. |
> | Aktion | Microsoft.PowerBIDedicated/capacities/resume/action | Setzt die Kapazität fort. |
> | Aktion | Microsoft.PowerBIDedicated/capacities/skus/read | Ruft die verfügbaren SKU-Informationen für die Kapazität ab. |
> | Aktion | Microsoft.PowerBIDedicated/capacities/suspend/action | Hält die Kapazität an. |
> | Aktion | Microsoft.PowerBIDedicated/capacities/write | Erstellt oder aktualisiert die angegebene dedizierte Power BI-Kapazität. |
> | Aktion | Microsoft.PowerBIDedicated/locations/operationresults/read | Ruft die Informationen des angegebenen Vorgangsergebnisses ab. |
> | Aktion | Microsoft.PowerBIDedicated/locations/operationstatuses/read | Ruft die Informationen des angegebenen Vorgangsstatus ab. |
> | Aktion | Microsoft.PowerBIDedicated/operations/read | Ruft die Informationen von Vorgängen ab. |
> | Aktion | Microsoft.PowerBIDedicated/register/action | Registriert den dedizierten Power BI-Ressourcenanbieter. |
> | Aktion | Microsoft.PowerBIDedicated/skus/read | Ruft die Informationen der SKUs ab. |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.RecoveryServices/locations/allocatedStamp/read | „GetAllocatedStamp“ ist ein interner Vorgang des Diensts. |
> | Aktion | Microsoft.RecoveryServices/locations/allocateStamp/action | „AllocateStamp“ ist ein interner Vorgang des Diensts. |
> | Aktion | Microsoft.RecoveryServices/locations/backupPreValidateProtection/action |  |
> | Aktion | Microsoft.RecoveryServices/locations/backupStatus/action | Überprüft den Sicherungsstatus für Recovery Services-Tresore. |
> | Aktion | Microsoft.RecoveryServices/locations/backupValidateFeatures/action | Überprüft Features. |
> | Aktion | Microsoft.RecoveryServices/operations/read | Der Vorgang gibt die Liste der Vorgänge für einen Ressourcenanbieter zurück. |
> | Aktion | Microsoft.RecoveryServices/register/action | Registriert das Abonnement für den angegebenen Ressourcenanbieter. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupconfig/read | Gibt die Konfiguration für Recovery Services-Tresore zurück. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupconfig/write | Aktualisiert die Konfiguration für Recovery Services-Tresore. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupEngines/read | Gibt alle Sicherungsverwaltungsserver zurück, die beim Tresor registriert sind. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | Erstellt einen beabsichtigten Sicherungsschutz. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Gibt den Status des Vorgangs zurück. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read | Ruft alle schützbaren Container ab. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/delete | Löscht den registrierten Container. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/inquire/action | Führt die Abfrage für Workloads innerhalb eines Containers durch. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read | Ruft alle Elemente in einem Container ab. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Ruft das Ergebnis eines Vorgangs ab, der für den Schutzcontainer ausgeführt wurde. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action | Führt eine Sicherung für geschützte Elemente aus. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/delete | Löscht geschützte Elemente. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Ruft das Ergebnis eines Vorgangs ab, der für geschützte Elemente ausgeführt wurde. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Gibt den Status eines Vorgangs zurück, der für geschützte Elemente ausgeführt wurde. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Gibt Objektdetails des geschützten Elements zurück. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/provisionInstantItemRecovery/action | Dient zum Bereitstellen der sofortigen Elementwiederherstellung für geschützte Elemente. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Dient zum Abrufen von Wiederherstellungspunkten für geschützte Elemente. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action | Dient zum Wiederherstellen von Wiederherstellungspunkten für geschützte Elemente. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/revokeInstantItemRecovery/action | Dient zum Widerrufen der sofortigen Elementwiederherstellung für geschützte Elemente. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Dient zum Erstellen eines geschützten Elements für die Sicherung. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Gibt alle registrierten Container zurück. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/write | Erstellt einen registrierten Container. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action | Aktualisiert die Containerliste. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupJobs/cancel/action | Dient zum Abbrechen des Auftrags. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | Gibt das Ergebnis von Auftragsvorgängen zurück. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupJobs/read | Gibt alle Auftragsobjekte zurück. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Dient zum Exportieren von Aufträgen. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Gibt das Ergebnis des Vorgangs „Auftrag exportieren“ zurück. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Gibt Sicherungsverwaltungs-Metadaten für Recovery Services-Tresore zurück. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupOperationResults/read | Gibt das Ergebnis eines Sicherungsvorgangs für Recovery Services-Tresore zurück. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupOperations/read | Gibt den Status eines Sicherungsvorgangs für Recovery Services-Tresore zurück. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupPolicies/delete | Dient zum Löschen einer Schutzrichtlinie. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Dient zum Abrufen der Ergebnisse von Richtlinienvorgängen. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read | Dient zum Abrufen des Status von Richtlinienvorgängen. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Gibt alle Schutzrichtlinien zurück. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupPolicies/write | Erstellt Schutzrichtlinien. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupProtectableItems/read | Gibt die Liste mit allen schützbaren Elementen zurück. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Gibt die Liste mit allen geschützten Elementen zurück. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Gibt alle zum Abonnement gehörenden Container zurück. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupSecurityPIN/action | Gibt Informationen zur Sicherheits-PIN für Recovery Services-Tresore zurück. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupstorageconfig/read | Gibt die Speicherkonfiguration für Recovery Services-Tresore zurück. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupstorageconfig/write | Aktualisiert Speicherkonfiguration für Recovery Services-Tresore. |
> | Aktion | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Gibt Zusammenfassungen für geschützte Elemente und geschützte Server für einen Recovery Services-Tresor zurück. |
> | Aktion | Microsoft.RecoveryServices/Vaults/certificates/write | Der Vorgang „Ressourcenzertifikat aktualisieren“ aktualisiert das Zertifikat für die Ressourcen-/Tresoranmeldeinformationen. |
> | Aktion | Microsoft.RecoveryServices/Vaults/delete | Der Vorgang „Tresor löschen“ löscht die angegebene Azure-Ressource vom Typ „Tresor“. |
> | Aktion | Microsoft.RecoveryServices/Vaults/extendedInformation/delete | Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Aktion | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Aktion | Microsoft.RecoveryServices/Vaults/extendedInformation/write | Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Aktion | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Ruft die Warnungen für den Recovery Services-Tresor ab. |
> | Aktion | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | Löst die Warnung auf. |
> | Aktion | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/read | Ruft die Konfiguration von Recovery Services-Tresorbenachrichtigungen ab. |
> | Aktion | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/write | Konfiguriert E-Mail-Benachrichtigungen für den Recovery Services-Tresor. |
> | Aktion | Microsoft.RecoveryServices/Vaults/providers/Microsoft.Insights/diagnosticSettings/read | Azure Backup-Diagnose |
> | Aktion | Microsoft.RecoveryServices/Vaults/providers/Microsoft.Insights/diagnosticSettings/write | Azure Backup-Diagnose |
> | Aktion | Microsoft.RecoveryServices/Vaults/providers/Microsoft.Insights/logDefinitions/read | Azure Backup-Protokolle |
> | Aktion | Microsoft.RecoveryServices/Vaults/providers/Microsoft.Insights/metricDefinitions/read | Azure Backup-Metriken |
> | Aktion | Microsoft.RecoveryServices/Vaults/read | Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Aktion | Microsoft.RecoveryServices/Vaults/registeredIdentities/delete | Der Vorgang „Registrierung des Containers aufheben“ kann zum Aufheben der Registrierung eines Containers verwendet werden. |
> | Aktion | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Mit dem Vorgang „Vorgangsergebnisse abrufen“ können der Vorgangsstatus und das Ergebnis für den asynchron übermittelten Vorgang abgerufen werden. |
> | Aktion | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Der Vorgang „Container abrufen“ kann zum Abrufen der für eine Ressource registrierten Container verwendet werden. |
> | Aktion | Microsoft.RecoveryServices/Vaults/registeredIdentities/write | Der Vorgang „Dienstcontainer registrieren“ kann zum Registrieren eines Containers beim Wiederherstellungsdienst verwendet werden. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Dient zum Lesen beliebiger Warnungseinstellungen. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationAlertSettings/write | Dient zum Erstellen oder Aktualisieren beliebiger Warnungseinstellungen. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationEvents/read | Dient zum Lesen beliebiger Ereignisse. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Prüft die Konsistenz des Fabrics. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/delete | Dient zum Löschen beliebiger Fabrics. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/deployProcessServerImage/action | Dient zum Bereitstellen von Prozessserverimages. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Dient zum Lesen beliebiger Fabrics. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | Dient zum Neuzuordnen des Gateways. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/remove/action | Dient zum Entfernen von Fabrics. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Verlängert das Zertifikat für Fabrics. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Dient zum Lesen beliebiger Netzwerke. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/delete | Dient zum Löschen beliebiger Netzwerkzuordnungen. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Dient zum Lesen beliebiger Netzwerkzuordnungen. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/write | Dient zum Erstellen oder Aktualisieren beliebiger Netzwerkzuordnungen. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/discoverProtectableItem/action | Dient zum Ermitteln des schützbaren Elements. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Dient zum Lesen beliebiger Schutzcontainer. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/remove/action | Dient zum Entfernen von Schutzcontainern. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Dient zum Lesen beliebiger schützbarer Elemente. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Dient zum Anwenden des Wiederherstellungspunkts. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/delete | Dient zum Löschen beliebiger geschützter Elemente. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | Failovercommit |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | Geplantes Failover |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Dient zum Lesen beliebiger geschützter Elemente. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Dient zum Lesen beliebiger Wiederherstellungspunkte für die Replikation. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/remove/action | Dient zum Entfernen geschützter Elemente. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/repairReplication/action | Dient zum Reparieren der Replikation. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/reProtect/action | Dient zum erneuten Schützen geschützter Elemente. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailover/action | Testfailover |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | Testfailoverbereinigung |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action | Failover |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Dient zum Aktualisieren von Mobility Service. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/write | Dient zum Erstellen oder Aktualisieren beliebiger geschützter Elemente. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/delete | Dient zum Löschen beliebiger Schutzcontainerzuordnungen. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Dient zum Lesen beliebiger Schutzcontainerzuordnungen. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/remove/action | Dient zum Entfernen von Schutzcontainerzuordnungen. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/write | Dient zum Erstellen oder Aktualisieren beliebiger Schutzcontainerzuordnungen. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/switchprotection/action | Dient zum Wechseln von Schutzcontainern. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/write | Dient zum Erstellen oder Aktualisieren beliebiger Schutzcontainer. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/delete | Dient zum Löschen beliebiger Recovery Services-Anbieter. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Dient zum Lesen beliebiger Recovery Services-Anbieter. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/action | Dient zum Aktualisieren des Anbieters. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/remove/action | Dient zum Entfernen von Recovery Services-Anbietern. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/write | Dient zum Erstellen oder Aktualisieren beliebiger Recovery Services-Anbieter. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | Dient zum Lesen beliebiger Speicherklassifizierungen. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/delete | Dient zum Löschen beliebiger Speicherklassifizierungszuordnungen. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Dient zum Lesen beliebiger Speicherklassifizierungszuordnungen. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/write | Dient zum Erstellen oder Aktualisieren beliebiger Speicherklassifizierungszuordnungen. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/delete | Dient zum Löschen beliebiger Aufträge. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Dient zum Lesen beliebiger Aufträge. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/write | Dient zum Erstellen oder Aktualisieren beliebiger Aufträge. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationFabrics/write | Dient zum Erstellen oder Aktualisieren beliebiger Fabrics. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationJobs/cancel/action | Dient zum Abbrechen von Aufträgen. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationJobs/read | Dient zum Lesen beliebiger Aufträge. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationJobs/restart/action | Dient zum Neustarten von Aufträgen. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationJobs/resume/action | Dient zum Fortsetzen von Aufträgen. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationPolicies/delete | Dient zum Löschen beliebiger Richtlinien. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Dient zum Lesen beliebiger Richtlinien. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationPolicies/write | Dient zum Erstellen oder Aktualisieren beliebiger Richtlinien. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/delete | Dient zum Löschen beliebiger Wiederherstellungspläne. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | Wiederherstellungsplan für Failovercommit |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | Wiederherstellungsplan für geplantes Failover |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Dient zum Lesen beliebiger Wiederherstellungspläne. |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Wiederherstellungsplan für erneutes Schützen |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Wiederherstellungsplan für Testfailover |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Wiederherstellungsplan für Testfailoverbereinigung |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action | Wiederherstellungsplan für Failover |
> | Aktion | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/write | Dient zum Erstellen oder Aktualisieren beliebiger Wiederherstellungspläne. |
> | Aktion | Microsoft.RecoveryServices/Vaults/tokenInfo/read | Gibt Tokeninformationen für Recovery Services-Tresore zurück. |
> | Aktion | Microsoft.RecoveryServices/vaults/usages/read | Dient zum Lesen beliebiger Tresorverwendungen. |
> | Aktion | Microsoft.RecoveryServices/Vaults/usages/read | Gibt Nutzungsdetails für einen Recovery Services-Tresor zurück. |
> | Aktion | Microsoft.RecoveryServices/Vaults/vaultTokens/read | Der Vorgang „Tresortoken“ kann zum Abrufen des Tresortokens für Back-End-Vorgänge auf Tresorebene verwendet werden. |
> | Aktion | Microsoft.RecoveryServices/Vaults/write | Der Vorgang „Tresor erstellen“ erstellt eine Azure-Ressource vom Typ „Tresor“. |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Relay/checkNameAvailability/action | Prüft die Verfügbarkeit des Namespaces unter dem angegebenen Abonnement. |
> | Aktion | Microsoft.Relay/checkNamespaceAvailability/action | Prüft die Verfügbarkeit des Namespaces unter dem angegebenen Abonnement. Diese API ist veraltet. Verwenden Sie stattdessen CheckNameAvailabiltiy. |
> | Aktion | Microsoft.Relay/namespaces/authorizationRules/action | Dient zum Aktualisieren von Namespace-Autorisierungsregeln. Diese API ist veraltet. Verwenden Sie stattdessen einen PUT-Aufruf, um die Namespace-Autorisierungsregel zu aktualisieren. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. |
> | Aktion | Microsoft.Relay/namespaces/authorizationRules/delete | Dient zum Löschen von Namespace-Autorisierungsregeln. Die standardmäßige Namespace-Autorisierungsregel kann nicht gelöscht werden.  |
> | Aktion | Microsoft.Relay/namespaces/authorizationRules/listkeys/action | Dient zum Abrufen der Verbindungszeichenfolge für den Namespace. |
> | Aktion | Microsoft.Relay/namespaces/authorizationRules/read | Dient zum Abrufen der Liste mit Beschreibungen für Namespace-Autorisierungsregeln. |
> | Aktion | Microsoft.Relay/namespaces/authorizationRules/regenerateKeys/action | Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource. |
> | Aktion | Microsoft.Relay/namespaces/authorizationRules/write | Dient zum Erstellen von Autorisierungsregeln auf Namespaceebene und zum Aktualisieren der dazugehörigen Eigenschaften. Aktualisiert werden können die Zugriffsrechte für Autorisierungsregeln sowie der primäre und der sekundäre Schlüssel. |
> | Aktion | Microsoft.Relay/namespaces/Delete | Dient zum Löschen von Namespaceressourcen. |
> | Aktion | Microsoft.Relay/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Ruft die Schlüssel der Autorisierungsregeln für den primären Namespace für die Notfallwiederherstellung. |
> | Aktion | Microsoft.Relay/namespaces/disasterRecoveryConfigs/authorizationRules/read | Ruft die Autorisierungsregeln des primären Namespace für die Notfallwiederherstellung ab. |
> | Aktion | Microsoft.Relay/namespaces/disasterRecoveryConfigs/breakPairing/action | Hiermit wird die Notfallwiederherstellung deaktiviert und die Replikation von Änderungen vom primären Namespace zu sekundären Namespaces beendet. |
> | Aktion | Microsoft.Relay/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Prüft die Verfügbarkeit des Namespacealias unter dem angegebenen Abonnement. |
> | Aktion | Microsoft.Relay/namespaces/disasterRecoveryConfigs/delete | Hiermit wird die dem Namespace zugeordnete Notfallwiederherstellungskonfiguration gelöscht. Dieser Vorgang kann nur über den primären Namespace aufgerufen werden. |
> | Aktion | Microsoft.Relay/namespaces/disasterRecoveryConfigs/failover/action | Hiermit wird ein Failover bei der georedundanten Notfallwiederherstellung ausgelöst und der Namespacealias neu konfiguriert, um auf den sekundären Namespace zu zeigen. |
> | Aktion | Microsoft.Relay/namespaces/disasterRecoveryConfigs/read | Hiermit wird die dem Namespace zugeordnete Notfallwiederherstellungskonfiguration abgerufen. |
> | Aktion | Microsoft.Relay/namespaces/disasterRecoveryConfigs/write | Hiermit wird die dem Namespace zugeordnete Notfallwiederherstellungskonfiguration erstellt oder aktualisiert. |
> | Aktion | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/action | Vorgang zum Aktualisieren von HybridConnection. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. Autorisierungsregeln. Verwenden Sie einen PUT-Aufruf, um die Autorisierungsregel zu aktualisieren. |
> | Aktion | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/delete | Vorgang zum Löschen von HybridConnection-Autorisierungsregeln. |
> | Aktion | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/listkeys/action | Dient zum Abrufen der HybridConnection-Verbindungszeichenfolge. |
> | Aktion | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/read |  Ruft die Liste der HybridConnection-Autorisierungsregeln ab. |
> | Aktion | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/regeneratekeys/action | Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource. |
> | Aktion | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/write | Dient zum Erstellen von HybridConnection-Autorisierungsregeln und zum Aktualisieren der dazugehörigen Eigenschaften. Die Zugriffsrechte für Autorisierungsregeln können aktualisiert werden. |
> | Aktion | Microsoft.Relay/namespaces/HybridConnections/Delete | Vorgang zum Löschen von HybridConnection-Ressourcen. |
> | Aktion | Microsoft.Relay/namespaces/HybridConnections/read | Dient zum Abrufen einer Liste mit HybridConnection-Ressourcenbeschreibungen. |
> | Aktion | Microsoft.Relay/namespaces/HybridConnections/write | Dient zum Erstellen oder Aktualisieren von HybridConnection-Eigenschaften. |
> | Aktion | Microsoft.Relay/namespaces/messagingPlan/read | Hiermit wird der Messagingplan für einen Namespace abgerufen.<br>Diese API ist veraltet.<br>Über die MessagingPlan-Ressource verfügbar gemachte Eigenschaften werden in höheren API-Versionen in die (übergeordnete) Namespaceressource verschoben.<br>Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. |
> | Aktion | Microsoft.Relay/namespaces/messagingPlan/write | Hiermit wird der Messagingplan für einen Namespace aktualisiert.<br>Diese API ist veraltet.<br>Über die MessagingPlan-Ressource verfügbar gemachte Eigenschaften werden in höheren API-Versionen in die (übergeordnete) Namespaceressource verschoben.<br>Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. |
> | Aktion | Microsoft.Relay/namespaces/operationresults/read | Hiermit wird der Status des Namespacevorgangs abgerufen. |
> | Aktion | Microsoft.Relay/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacemetriken. |
> | Aktion | Microsoft.Relay/namespaces/read | Dient zum Abrufen der Liste mit Beschreibungen für Namespaceressourcen. |
> | Aktion | Microsoft.Relay/namespaces/removeAcsNamepsace/action | Entfernt den ACS-Namespace. |
> | Aktion | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/action | Vorgang zum Aktualisieren von WcfRelay. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. Autorisierungsregeln. Verwenden Sie einen PUT-Aufruf, um die Autorisierungsregel zu aktualisieren. |
> | Aktion | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/delete | Vorgang zum Löschen von WcfRelay-Autorisierungsregeln. |
> | Aktion | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/listkeys/action | Dient zum Abrufen der WcfRelay-Verbindungszeichenfolge. |
> | Aktion | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/read |  Dient zum Abrufen der WcfRelay-Autorisierungsregeln. |
> | Aktion | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/regeneratekeys/action | Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource. |
> | Aktion | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/write | Dient zum Erstellen von WcfRelay-Autorisierungsregeln und zum Aktualisieren der dazugehörigen Eigenschaften. Die Zugriffsrechte für Autorisierungsregeln können aktualisiert werden. |
> | Aktion | Microsoft.Relay/namespaces/WcfRelays/Delete | Vorgang zum Löschen von WcfRelay-Ressourcen. |
> | Aktion | Microsoft.Relay/namespaces/WcfRelays/read | Dient zum Abrufen einer Liste mit WcfRelay-Ressourcenbeschreibungen. |
> | Aktion | Microsoft.Relay/namespaces/WcfRelays/write | Dient zum Erstellen oder Aktualisieren von WcfRelay-Eigenschaften. |
> | Aktion | Microsoft.Relay/namespaces/write | Dient zum Erstellen einer Namespaceressource und zum Aktualisieren ihrer Eigenschaften. Die Eigenschaften für Markierungen und Kapazität des Namespace können aktualisiert werden. |
> | Aktion | Microsoft.Relay/operations/read | Dient zum Abrufen von Vorgängen. |
> | Aktion | Microsoft.Relay/register/action | Registriert das Abonnement für den Relayressourcenanbieter und ermöglicht die Erstellung von Relayressourcen. |
> | Aktion | Microsoft.Relay/unregister/action | Registriert das Abonnement für den Relayressourcenanbieter und ermöglicht die Erstellung von Relayressourcen. |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.ResourceHealth/AvailabilityStatuses/current/read | Ruft den Verfügbarkeitsstatus für die angegebene Ressource ab. |
> | Aktion | Microsoft.ResourceHealth/AvailabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Aktion | Microsoft.ResourceHealth/events/read | Ruft Service Health-Ereignisse für ein angegebenes Abonnement ab. |
> | Aktion | Microsoft.Resourcehealth/healthevent/action | Zeigt die Änderung des Integritätszustands für die angegebene Ressource an. |
> | Aktion | Microsoft.Resourcehealth/healthevent/Activated/action | Zeigt die Änderung des Integritätszustands für die angegebene Ressource an. |
> | Aktion | Microsoft.Resourcehealth/healthevent/InProgress/action | Zeigt die Änderung des Integritätszustands für die angegebene Ressource an. |
> | Aktion | Microsoft.Resourcehealth/healthevent/Pending/action | Zeigt die Änderung des Integritätszustands für die angegebene Ressource an. |
> | Aktion | Microsoft.Resourcehealth/healthevent/Resolved/action | Zeigt die Änderung des Integritätszustands für die angegebene Ressource an. |
> | Aktion | Microsoft.Resourcehealth/healthevent/Updated/action | Zeigt die Änderung des Integritätszustands für die angegebene Ressource an. |
> | Aktion | Microsoft.ResourceHealth/impactedResources/read | Ruft betroffene Ressourcen für das angegebene Abonnement ab. |
> | Aktion | Microsoft.ResourceHealth/register/action | Hiermit wird das Abonnement für Microsoft ResourceHealth registriert. |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Resources/checkPolicyCompliance/action | Überprüft den Konformitätsstatus einer angegebenen Ressource anhand von Ressourcenrichtlinien. |
> | Aktion | Microsoft.Resources/checkResourceName/action | Dient zum Überprüfen der Gültigkeit des Ressourcennamens. |
> | Aktion | Microsoft.Resources/deployments/cancel/action | Bricht eine Bereitstellung ab. |
> | Aktion | Microsoft.Resources/deployments/delete | Löscht eine Bereitstellung. |
> | Aktion | Microsoft.Resources/deployments/operations/read | Ruft Bereitstellungsvorgänge ab oder listet sie auf. |
> | Aktion | Microsoft.Resources/deployments/read | Ruft Bereitstellungen ab oder listet sie auf. |
> | Aktion | Microsoft.Resources/deployments/validate/action | Überprüft eine Bereitstellung. |
> | Aktion | Microsoft.Resources/deployments/write | Erstellt oder aktualisiert eine Bereitstellung. |
> | Aktion | Microsoft.Resources/links/delete | Löscht einen Ressourcenlink. |
> | Aktion | Microsoft.Resources/links/read | Ruft Ressourcenlinks ab oder listet sie auf. |
> | Aktion | Microsoft.Resources/links/write | Erstellt oder aktualisiert einen Ressourcenlink. |
> | Aktion | Microsoft.Resources/marketplace/purchase/action | Führt zum Erwerb einer Ressource im Marketplace. |
> | Aktion | Microsoft.Resources/providers/read | Dient zum Abrufen der Anbieterliste. |
> | Aktion | Microsoft.Resources/resources/read | Dient zum Abrufen einer gefilterten Ressourcenliste. |
> | Aktion | Microsoft.Resources/subscriptions/locations/read | Ruft die Liste mit den unterstützten Standorten ab. |
> | Aktion | Microsoft.Resources/subscriptions/operationresults/read | Dient zum Abrufen der Ergebnisse des Abonnementvorgangs. |
> | Aktion | Microsoft.Resources/subscriptions/providers/read | Ruft Ressourcenanbieter ab oder listet sie auf. |
> | Aktion | Microsoft.Resources/subscriptions/read | Ruft die Abonnementliste ab. |
> | Aktion | Microsoft.Resources/subscriptions/resourceGroups/delete | Löscht eine Ressourcengruppe und alle dazugehörigen Ressourcen. |
> | Aktion | Microsoft.Resources/subscriptions/resourcegroups/deployments/operations/read | Ruft Bereitstellungsvorgänge ab oder listet sie auf. |
> | Aktion | Microsoft.Resources/subscriptions/resourcegroups/deployments/operationstatuses/read | Ruft den Status von Bereitstellungsvorgängen ab oder listet ihn auf. |
> | Aktion | Microsoft.Resources/subscriptions/resourcegroups/deployments/read | Ruft Bereitstellungen ab oder listet sie auf. |
> | Aktion | Microsoft.Resources/subscriptions/resourcegroups/deployments/write | Erstellt oder aktualisiert eine Bereitstellung. |
> | Aktion | Microsoft.Resources/subscriptions/resourceGroups/moveResources/action | Verschiebt Ressourcen aus einer Ressourcengruppe in eine andere. |
> | Aktion | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Aktion | Microsoft.Resources/subscriptions/resourcegroups/resources/read | Ruft die Ressourcen für die Ressourcengruppe ab. |
> | Aktion | Microsoft.Resources/subscriptions/resourceGroups/validateMoveResources/action | Überprüft das Verschieben von Ressourcen aus einer Ressourcengruppe in eine andere. |
> | Aktion | Microsoft.Resources/subscriptions/resourceGroups/write | Erstellt oder aktualisiert eine Ressourcengruppe. |
> | Aktion | Microsoft.Resources/subscriptions/resources/read | Ruft die Ressourcen eines Abonnements ab. |
> | Aktion | Microsoft.Resources/subscriptions/tagNames/delete | Löscht ein Abonnementtag. |
> | Aktion | Microsoft.Resources/subscriptions/tagNames/read | Ruft Abonnementtags ab oder listet sie auf. |
> | Aktion | Microsoft.Resources/subscriptions/tagNames/tagValues/delete | Löscht einen Abonnementtagwert. |
> | Aktion | Microsoft.Resources/subscriptions/tagNames/tagValues/read | Ruft Abonnementtagwerte ab oder listet sie auf. |
> | Aktion | Microsoft.Resources/subscriptions/tagNames/tagValues/write | Fügt einen Abonnementtagwert hinzu. |
> | Aktion | Microsoft.Resources/subscriptions/tagNames/write | Fügt ein Abonnementtag hinzu. |
> | Aktion | Microsoft.Resources/tenants/read | Ruft die Mandantenliste ab. |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Scheduler/jobcollections/delete | Löscht Auftragssammlungen. |
> | Aktion | Microsoft.Scheduler/jobcollections/disable/action | Deaktiviert Auftragssammlungen. |
> | Aktion | Microsoft.Scheduler/jobcollections/enable/action | Aktiviert Auftragssammlungen. |
> | Aktion | Microsoft.Scheduler/jobcollections/jobs/delete | Löscht Aufträge. |
> | Aktion | Microsoft.Scheduler/jobcollections/jobs/generateLogicAppDefinition/action | Generiert Logik-App-Definitionen auf der Grundlage eines Scheduler-Auftrags. |
> | Aktion | Microsoft.Scheduler/jobcollections/jobs/jobhistories/read | Ruft den Auftragsverlauf ab. |
> | Aktion | Microsoft.Scheduler/jobcollections/jobs/read | Ruft Aufträge ab. |
> | Aktion | Microsoft.Scheduler/jobcollections/jobs/run/action | Führt Aufträge aus. |
> | Aktion | Microsoft.Scheduler/jobcollections/jobs/write | Erstellt oder aktualisiert Aufträge. |
> | Aktion | Microsoft.Scheduler/jobcollections/read | Dient zum Abrufen von Auftragssammlungen. |
> | Aktion | Microsoft.Scheduler/jobcollections/write | Erstellt oder aktualisiert Auftragssammlungen. |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Search/checkNameAvailability/action | Prüft die Verfügbarkeit des Dienstnamens. |
> | Aktion | Microsoft.Search/operations/read | Führt alle verfügbaren Vorgänge des Anbieters Microsoft.Search auf. |
> | Aktion | Microsoft.Search/register/action | Registriert das Abonnement für den Search-Ressourcenanbieter und ermöglicht die Erstellung von Suchdiensten. |
> | Aktion | Microsoft.Search/searchServices/createQueryKey/action | Erstellt den Abfrageschlüssel. |
> | Aktion | Microsoft.Search/searchServices/delete | Löscht den Suchdienst. |
> | Aktion | Microsoft.Search/searchServices/deleteQueryKey/delete | Löscht den Abfrageschlüssel. |
> | Aktion | Microsoft.Search/searchServices/diagnosticSettings/read | Ruft die gelesene Diagnoseeinstellung für die Ressource ab. |
> | Aktion | Microsoft.Search/searchServices/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.Search/searchServices/listAdminKeys/action | Liest die Administratorschlüssel. |
> | Aktion | Microsoft.Search/searchServices/listQueryKeys/read | Gibt die Liste der API-Abfrageschlüssel für den angegebenen Azure Search-Dienst zurück. |
> | Aktion | Microsoft.Search/searchServices/logDefinitions/read | Ruft die verfügbaren Protokolle für den Suchdienst ab. |
> | Aktion | Microsoft.Search/searchServices/metricDefinitions/read | Ruft die verfügbaren Metriken für den Suchdienst ab. |
> | Aktion | Microsoft.Search/searchServices/read | Liest den Suchdienst. |
> | Aktion | Microsoft.Search/searchServices/regenerateAdminKey/action | Generiert die Administratorschlüssel neu. |
> | Aktion | Microsoft.Search/searchServices/start/action | Startet den Suchdienst. |
> | Aktion | Microsoft.Search/searchServices/stop/action | Beendet den Suchdienst. |
> | Aktion | Microsoft.Search/searchServices/write | Erstellt oder aktualisiert den Suchdienst. |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Security/alerts/read | Ruft alle verfügbaren Sicherheitswarnungen ab. |
> | Aktion | Microsoft.Security/applicationWhitelistings/read | Ruft die Anwendungswhitelists ab. |
> | Aktion | Microsoft.Security/applicationWhitelistings/write | Erstellt eine neue Anwendungswhitelist oder aktualisiert eine bereits vorhandene. |
> | Aktion | Microsoft.Security/complianceResults/read | Ruft die Konformitätsergebnisse für die Ressource ab. |
> | Aktion | Microsoft.Security/locations/alerts/activate/action | Aktiviert eine Sicherheitswarnung. |
> | Aktion | Microsoft.Security/locations/alerts/dismiss/action | Schließt eine Sicherheitswarnung. |
> | Aktion | Microsoft.Security/locations/alerts/read | Ruft alle verfügbaren Sicherheitswarnungen ab. |
> | Aktion | Microsoft.Security/locations/jitNetworkAccessPolicies/initiate/action | Initiiert eine JIT-Netzwerkzugriffsrichtlinie. |
> | Aktion | Microsoft.Security/locations/jitNetworkAccessPolicies/read | Ruft die JIT-Netzwerkzugriffsrichtlinien ab. |
> | Aktion | Microsoft.Security/locations/jitNetworkAccessPolicies/write | Erstellt eine neue JIT-Netzwerkzugriffsrichtlinie oder aktualisiert eine bereits vorhandene. |
> | Aktion | Microsoft.Security/locations/read | Ruft den Speicherort der Sicherheitsdaten ab. |
> | Aktion | Microsoft.Security/locations/tasks/activate/action | Dient zum Aktivieren einer Sicherheitsempfehlung. |
> | Aktion | Microsoft.Security/locations/tasks/dismiss/action | Dient zum Verwerfen einer Sicherheitsempfehlung. |
> | Aktion | Microsoft.Security/locations/tasks/read | Ruft alle verfügbaren Sicherheitsempfehlungen ab. |
> | Aktion | Microsoft.Security/locations/tasks/resolve/action | Dient zum Auflösen einer Sicherheitsempfehlung. |
> | Aktion | Microsoft.Security/locations/tasks/start/action | Dient zum Starten einer Sicherheitsempfehlung. |
> | Aktion | Microsoft.Security/policies/read | Ruft die Sicherheitsrichtlinie ab. |
> | Aktion | Microsoft.Security/policies/write | Aktualisiert die Sicherheitsrichtlinie. |
> | Aktion | Microsoft.Security/pricings/delete | Löscht die Tarifeinstellungen für den Bereich. |
> | Aktion | Microsoft.Security/pricings/read | Ruft die Tarifeinstellungen für den Bereich ab. |
> | Aktion | Microsoft.Security/pricings/write | Aktualisiert die Tarifeinstellungen für den Bereich. |
> | Aktion | Microsoft.Security/register/action | Registriert das Abonnement für das Azure Security Center. |
> | Aktion | Microsoft.Security/securityContacts/delete | Löscht den Sicherheitskontakt. |
> | Aktion | Microsoft.Security/securityContacts/read | Ruft den Sicherheitskontakt ab. |
> | Aktion | Microsoft.Security/securityContacts/write | Aktualisiert den Sicherheitskontakt. |
> | Aktion | Microsoft.Security/securitySolutions/delete | Löscht eine Sicherheitslösung. |
> | Aktion | Microsoft.Security/securitySolutions/read | Ruft die Sicherheitslösungen ab. |
> | Aktion | Microsoft.Security/securitySolutions/write | Erstellt eine neue Sicherheitslösung oder aktualisiert eine bereits vorhandene. |
> | Aktion | Microsoft.Security/securitySolutionsReferenceData/read | Ruft die Verweisdaten für Sicherheitslösungen ab. |
> | Aktion | Microsoft.Security/securityStatuses/read | Ruft den Sicherheitsintegritätsstatus für Azure-Ressourcen ab. |
> | Aktion | Microsoft.Security/securityStatusesSummaries/read | Ruft die Zusammenfassungen der Sicherheitsstatus für den Bereich ab. |
> | Aktion | Microsoft.Security/tasks/read | Ruft alle verfügbaren Sicherheitsempfehlungen ab. |
> | Aktion | Microsoft.Security/webApplicationFirewalls/delete | Löscht eine Web Application Firewall. |
> | Aktion | Microsoft.Security/webApplicationFirewalls/read | Ruft die Web Application Firewalls ab. |
> | Aktion | Microsoft.Security/webApplicationFirewalls/write | Erstellt eine neue Web Application Firewall oder aktualisiert eine bereits vorhandene. |
> | Aktion | Microsoft.Security/workspaceSettings/connect/action | Ändert die Einstellungen zum erneuten Herstellen einer Verbindung in den Arbeitsbereichseinstellungen. |
> | Aktion | Microsoft.Security/workspaceSettings/delete | Löscht die Arbeitsbereichseinstellungen. |
> | Aktion | Microsoft.Security/workspaceSettings/read | Ruft die Arbeitsbereichseinstellungen ab. |
> | Aktion | Microsoft.Security/workspaceSettings/write | Aktualisiert die Arbeitsbereichseinstellungen. |

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.SecurityGraph/diagnosticsettings/delete | Löscht eine Diagnoseeinstellung. |
> | Aktion | Microsoft.SecurityGraph/diagnosticsettings/read | Liest eine Diagnoseeinstellung. |
> | Aktion | Microsoft.SecurityGraph/diagnosticsettings/write | Schreibt eine Diagnoseeinstellung. |
> | Aktion | Microsoft.SecurityGraph/diagnosticsettingscategories/read | Liest die Kategorien einer Diagnoseeinstellung. |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.ServiceBus/checkNameAvailability/action | Prüft die Verfügbarkeit des Namespaces unter dem angegebenen Abonnement. |
> | Aktion | Microsoft.ServiceBus/checkNamespaceAvailability/action | Prüft die Verfügbarkeit des Namespaces unter dem angegebenen Abonnement. Diese API ist veraltet. Verwenden Sie stattdessen CheckNameAvailabiltiy. |
> | Aktion | Microsoft.ServiceBus/namespaces/authorizationRules/action | Dient zum Aktualisieren von Namespace-Autorisierungsregeln. Diese API ist veraltet. Verwenden Sie stattdessen einen PUT-Aufruf, um die Namespace-Autorisierungsregel zu aktualisieren. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. |
> | Aktion | Microsoft.ServiceBus/namespaces/authorizationRules/delete | Dient zum Löschen von Namespace-Autorisierungsregeln. Die standardmäßige Namespace-Autorisierungsregel kann nicht gelöscht werden.  |
> | Aktion | Microsoft.ServiceBus/namespaces/authorizationRules/listkeys/action | Dient zum Abrufen der Verbindungszeichenfolge für den Namespace. |
> | Aktion | Microsoft.ServiceBus/namespaces/authorizationRules/read | Dient zum Abrufen der Liste mit Beschreibungen für Namespace-Autorisierungsregeln. |
> | Aktion | Microsoft.ServiceBus/namespaces/authorizationRules/regenerateKeys/action | Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource. |
> | Aktion | Microsoft.ServiceBus/namespaces/authorizationRules/write | Dient zum Erstellen von Autorisierungsregeln auf Namespaceebene und zum Aktualisieren der dazugehörigen Eigenschaften. Aktualisiert werden können die Zugriffsrechte für Autorisierungsregeln sowie der primäre und der sekundäre Schlüssel. |
> | Aktion | Microsoft.ServiceBus/namespaces/Delete | Dient zum Löschen von Namespaceressourcen. |
> | Aktion | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Ruft die Schlüssel der Autorisierungsregeln für den primären Namespace für die Notfallwiederherstellung. |
> | Aktion | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/authorizationRules/read | Ruft die Autorisierungsregeln des primären Namespace für die Notfallwiederherstellung ab. |
> | Aktion | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/breakPairing/action | Hiermit wird die Notfallwiederherstellung deaktiviert und die Replikation von Änderungen vom primären Namespace zu sekundären Namespaces beendet. |
> | Aktion | Microsoft.ServiceBus/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Prüft die Verfügbarkeit des Namespacealias unter dem angegebenen Abonnement. |
> | Aktion | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/delete | Hiermit wird die dem Namespace zugeordnete Notfallwiederherstellungskonfiguration gelöscht. Dieser Vorgang kann nur über den primären Namespace aufgerufen werden. |
> | Aktion | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/failover/action | Hiermit wird ein Failover bei der georedundanten Notfallwiederherstellung ausgelöst und der Namespacealias neu konfiguriert, um auf den sekundären Namespace zu zeigen. |
> | Aktion | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/read | Hiermit wird die dem Namespace zugeordnete Notfallwiederherstellungskonfiguration abgerufen. |
> | Aktion | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/write | Hiermit wird die dem Namespace zugeordnete Notfallwiederherstellungskonfiguration erstellt oder aktualisiert. |
> | Aktion | Microsoft.ServiceBus/namespaces/eventGridFilters/delete | Löscht den Event Grid-Filter, der dem Namespace zugeordnet ist. |
> | Aktion | Microsoft.ServiceBus/namespaces/eventGridFilters/read | Ruft den Event Grid-Filter ab, der dem Namespace zugeordnet ist. |
> | Aktion | Microsoft.ServiceBus/namespaces/eventGridFilters/write | Erstellt oder aktualisiert den Event Grid-Filter, der dem Namespace zugeordnet ist. |
> | Aktion | Microsoft.ServiceBus/namespaces/eventhubs/read | Dient zum Abrufen einer Liste mit EventHub-Ressourcenbeschreibungen. |
> | Aktion | Microsoft.ServiceBus/namespaces/messagingPlan/read | Hiermit wird der Messagingplan für einen Namespace abgerufen.<br>Diese API ist veraltet.<br>Über die MessagingPlan-Ressource verfügbar gemachte Eigenschaften werden in höheren API-Versionen in die (übergeordnete) Namespaceressource verschoben.<br>Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. |
> | Aktion | Microsoft.ServiceBus/namespaces/messagingPlan/write | Hiermit wird der Messagingplan für einen Namespace aktualisiert.<br>Diese API ist veraltet.<br>Über die MessagingPlan-Ressource verfügbar gemachte Eigenschaften werden in höheren API-Versionen in die (übergeordnete) Namespaceressource verschoben.<br>Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. |
> | Aktion | Microsoft.ServiceBus/namespaces/migrate/action | Migriert den Namespacevorgang. |
> | Aktion | Microsoft.ServiceBus/namespaces/operationresults/read | Hiermit wird der Status des Namespacevorgangs abgerufen. |
> | Aktion | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/diagnosticSettings/read | Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacediagnoseeinstellungen. |
> | Aktion | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/diagnosticSettings/write | Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacediagnoseeinstellungen. |
> | Aktion | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/logDefinitions/read | Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespaceprotokolle. |
> | Aktion | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacemetriken. |
> | Aktion | Microsoft.ServiceBus/namespaces/queues/authorizationRules/action | Vorgang zum Aktualisieren von Queue. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. Autorisierungsregeln. Verwenden Sie einen PUT-Aufruf, um die Autorisierungsregel zu aktualisieren. |
> | Aktion | Microsoft.ServiceBus/namespaces/queues/authorizationRules/delete | Vorgang zum Löschen von Warteschlangen-Autorisierungsregeln. |
> | Aktion | Microsoft.ServiceBus/namespaces/queues/authorizationRules/listkeys/action | Dient zum Abrufen der Warteschlangen-Verbindungszeichenfolge. |
> | Aktion | Microsoft.ServiceBus/namespaces/queues/authorizationRules/read |  Dient zum Abrufen der Liste mit Warteschlangen-Autorisierungsregeln. |
> | Aktion | Microsoft.ServiceBus/namespaces/queues/authorizationRules/regenerateKeys/action | Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource. |
> | Aktion | Microsoft.ServiceBus/namespaces/queues/authorizationRules/write | Dient zum Erstellen von Warteschlangen-Autorisierungsregeln und zum Aktualisieren der dazugehörigen Eigenschaften. Die Zugriffsrechte für Autorisierungsregeln können aktualisiert werden. |
> | Aktion | Microsoft.ServiceBus/namespaces/queues/Delete | Vorgang zum Löschen von Warteschlangenressourcen. |
> | Aktion | Microsoft.ServiceBus/namespaces/queues/read | Dient zum Abrufen einer Liste mit Warteschlangen-Ressourcenbeschreibungen. |
> | Aktion | Microsoft.ServiceBus/namespaces/queues/write | Dient zum Erstellen oder Aktualisieren von Warteschlangeneigenschaften. |
> | Aktion | Microsoft.ServiceBus/namespaces/read | Dient zum Abrufen der Liste mit Beschreibungen für Namespaceressourcen. |
> | Aktion | Microsoft.ServiceBus/namespaces/removeAcsNamepsace/action | Entfernt den ACS-Namespace. |
> | Aktion | Microsoft.ServiceBus/namespaces/topics/authorizationRules/action | Vorgang zum Aktualisieren von Topic. Dieser Vorgang wird nicht für die API-Version 2017-04-01 unterstützt. Autorisierungsregeln. Verwenden Sie einen PUT-Aufruf, um die Autorisierungsregel zu aktualisieren. |
> | Aktion | Microsoft.ServiceBus/namespaces/topics/authorizationRules/delete | Vorgang zum Löschen von Themaautorisierungsregeln. |
> | Aktion | Microsoft.ServiceBus/namespaces/topics/authorizationRules/listkeys/action | Dient zum Abrufen der Themaverbindungszeichenfolge. |
> | Aktion | Microsoft.ServiceBus/namespaces/topics/authorizationRules/read |  Dient zum Abrufen der Liste mit Themaautorisierungsregeln. |
> | Aktion | Microsoft.ServiceBus/namespaces/topics/authorizationRules/regenerateKeys/action | Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource. |
> | Aktion | Microsoft.ServiceBus/namespaces/topics/authorizationRules/write | Dient zum Erstellen von Themaautorisierungsregeln und zum Aktualisieren der dazugehörigen Eigenschaften. Die Zugriffsrechte für Autorisierungsregeln können aktualisiert werden. |
> | Aktion | Microsoft.ServiceBus/namespaces/topics/Delete | Vorgang zum Löschen von Themaressourcen. |
> | Aktion | Microsoft.ServiceBus/namespaces/topics/read | Dient zum Abrufen einer Liste mit Themaressourcenbeschreibungen. |
> | Aktion | Microsoft.ServiceBus/namespaces/topics/subscriptions/Delete | Vorgang zum Löschen von TopicSubscription-Ressourcen. |
> | Aktion | Microsoft.ServiceBus/namespaces/topics/subscriptions/read | Dient zum Abrufen einer Liste mit TopicSubscription-Ressourcenbeschreibungen. |
> | Aktion | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/Delete | Vorgang zum Löschen von Regelressourcen. |
> | Aktion | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/read | Dient zum Abrufen einer Liste mit Regelressourcenbeschreibungen. |
> | Aktion | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/write | Dient zum Erstellen oder Aktualisieren von Aktualisierungsregeleigenschaften. |
> | Aktion | Microsoft.ServiceBus/namespaces/topics/subscriptions/write | Dient zum Erstellen oder Aktualisieren von TopicSubscription-Eigenschaften. |
> | Aktion | Microsoft.ServiceBus/namespaces/topics/write | Dient zum Erstellen oder Aktualisieren von Themaeigenschaften. |
> | Aktion | Microsoft.ServiceBus/namespaces/write | Dient zum Erstellen einer Namespaceressource und zum Aktualisieren ihrer Eigenschaften. Die Eigenschaften für Markierungen und Kapazität des Namespace können aktualisiert werden. |
> | Aktion | Microsoft.ServiceBus/operations/read | Dient zum Abrufen von Vorgängen. |
> | Aktion | Microsoft.ServiceBus/register/action | Registriert das Abonnement für den ServiceBus-Ressourcenanbieter und ermöglicht die Erstellung von ServiceBus-Ressourcen. |
> | Aktion | Microsoft.ServiceBus/sku/read | Dient zum Abrufen einer Liste mit SKU-Ressourcenbeschreibungen. |
> | Aktion | Microsoft.ServiceBus/sku/regions/read | Dient zum Abrufen einer Liste mit SkuRegions-Ressourcenbeschreibungen. |
> | Aktion | Microsoft.ServiceBus/unregister/action | Registriert das Abonnement für den ServiceBus-Ressourcenanbieter und ermöglicht die Erstellung von ServiceBus-Ressourcen. |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.ServiceFabric/clusters/applications/delete | Hiermit löschen Sie eine Anwendung. |
> | Aktion | Microsoft.ServiceFabric/clusters/applications/read | Liest eine Anwendung. |
> | Aktion | Microsoft.ServiceFabric/clusters/applications/services/delete | Hiermit löschen Sie einen Dienst. |
> | Aktion | Microsoft.ServiceFabric/clusters/applications/services/partitions/read | Liest eine Partition. |
> | Aktion | Microsoft.ServiceFabric/clusters/applications/services/partitions/replicas/read | Liest ein Replikat. |
> | Aktion | Microsoft.ServiceFabric/clusters/applications/services/read | Liest einen Dienst. |
> | Aktion | Microsoft.ServiceFabric/clusters/applications/services/statuses/read | Liest einen Dienststatus. |
> | Aktion | Microsoft.ServiceFabric/clusters/applications/services/write | Hiermit erstellen oder aktualisieren Sie einen Dienst. |
> | Aktion | Microsoft.ServiceFabric/clusters/applications/write | Hiermit erstellen oder aktualisieren Sie eine Anwendung. |
> | Aktion | Microsoft.ServiceFabric/clusters/applicationTypes/delete | Hiermit löschen Sie einen Anwendungstyp. |
> | Aktion | Microsoft.ServiceFabric/clusters/applicationTypes/read | Hiermit lesen Sie einen Anwendungstyp. |
> | Aktion | Microsoft.ServiceFabric/clusters/applicationTypes/versions/delete | Hiermit löschen Sie eine Anwendungstypversion. |
> | Aktion | Microsoft.ServiceFabric/clusters/applicationTypes/versions/read | Hiermit lesen Sie eine Anwendungstypversion. |
> | Aktion | Microsoft.ServiceFabric/clusters/applicationTypes/versions/write | Hiermit erstellen oder aktualisieren Sie eine Anwendungstypversion. |
> | Aktion | Microsoft.ServiceFabric/clusters/applicationTypes/write | Hiermit erstellen oder aktualisieren Sie einen Anwendungstyp. |
> | Aktion | Microsoft.ServiceFabric/clusters/delete | Löscht einen Cluster. |
> | Aktion | Microsoft.ServiceFabric/clusters/nodes/read | Liest einen Knoten. |
> | Aktion | Microsoft.ServiceFabric/clusters/read | Liest einen Cluster. |
> | Aktion | Microsoft.ServiceFabric/clusters/statuses/read | Liest einen Clusterstatus. |
> | Aktion | Microsoft.ServiceFabric/clusters/write | Erstellt oder aktualisiert einen Cluster. |
> | Aktion | Microsoft.ServiceFabric/locations/clusterVersions/read | Liest Clusterversionen. |
> | Aktion | Microsoft.ServiceFabric/locations/environments/clusterVersions/read | Hiermit lesen Sie eine Clusterversion für eine bestimmte Umgebung. |
> | Aktion | Microsoft.ServiceFabric/locations/operationresults/read | Hiermit lesen Sie Vorgangsergebnisse. |
> | Aktion | Microsoft.ServiceFabric/locations/operations/read | Hiermit lesen Sie Vorgänge nach Standort. |
> | Aktion | Microsoft.ServiceFabric/operations/read | Hiermit lesen Sie verfügbare Vorgänge. |
> | Aktion | Microsoft.ServiceFabric/register/action | Hiermit registrieren Sie eine Aktion. |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.SignalRService/checknameavailability/action | Überprüft, ob ein Name für die Verwendung mit einem neuen SignalR-Dienst verfügbar ist. |
> | Aktion | Microsoft.SignalRService/register/action | Registriert den Ressourcenanbieter „Microsoft.SignalRService“ bei einem Abonnement. |
> | Aktion | Microsoft.SignalRService/SignalR/delete | Löscht den gesamten SignalR-Dienst. |
> | Aktion | Microsoft.SignalRService/SignalR/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | Microsoft.SignalRService/SignalR/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.SignalRService/SignalR/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für SignalR ab. |
> | Aktion | Microsoft.SignalRService/SignalR/read | Zeigt die SignalR-Einstellungen und Konfigurationen im Verwaltungsportal oder über die API an. |
> | Aktion | Microsoft.SignalRService/SignalR/write | Bearbeitet die SignalR-Einstellungen und Konfigurationen im Verwaltungsportal oder über die API. |
> | Aktion | Microsoft.SignalRService/unregister/action | Hebt die Registrierung des Ressourcenanbieters „Microsoft.SignalRService“ bei einem Abonnement auf. |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Solutions/applicationDefinitions/delete | Hiermit wird eine Anwendungsdefinition entfernt. |
> | Aktion | Microsoft.Solutions/applicationDefinitions/read | Hiermit wird eine Liste mit Anwendungsdefinitionen abgerufen. |
> | Aktion | Microsoft.Solutions/applicationDefinitions/write | Hiermit wird eine Anwendungsdefinition hinzugefügt oder geändert. |
> | Aktion | Microsoft.Solutions/applications/delete | Hiermit wird eine Anwendung entfernt. |
> | Aktion | Microsoft.Solutions/applications/read | Hiermit wird eine Liste mit Anwendungen abgerufen. |
> | Aktion | Microsoft.Solutions/applications/write | Erstellt eine Anwendung |
> | Aktion | Microsoft.Solutions/locations/operationStatuses/read | Liest den Vorgangsstatus für die Ressource. |
> | Aktion | Microsoft.Solutions/register/action | Hiermit registrieren Sie sich bei Lösungen. |

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Sql/checkNameAvailability/action | Überprüfen Sie, ob der angegebene Servername für die weltweite Bereitstellung bei einem bestimmten Abonnement verfügbar ist. |
> | Aktion | Microsoft.Sql/locations/auditingSettingsAzureAsyncOperation/read | Dient zum Abrufen des Ergebnisses des Set-Vorgangs der Richtlinie für die erweiterte Serverblobüberwachung. |
> | Aktion | Microsoft.Sql/locations/auditingSettingsOperationResults/read | Dient zum Abrufen des Ergebnisses des Set-Vorgangs der Richtlinie für die Serverblobüberwachung. |
> | Aktion | Microsoft.Sql/locations/capabilities/read | Ruft die Funktionen für dieses Abonnement in einem bestimmten Standort ab. |
> | Aktion | Microsoft.Sql/locations/databaseAzureAsyncOperation/read | Ruft den Status eines Datenbankvorgangs ab. |
> | Aktion | Microsoft.Sql/locations/databaseOperationResults/read | Ruft den Status eines Datenbankvorgangs ab. |
> | Aktion | Microsoft.Sql/locations/deletedServerAsyncOperation/read | Ruft gegenwärtig ausgeführte Vorgänge für gelöschte Server ab. |
> | Aktion | Microsoft.Sql/locations/deletedServerOperationResults/read | Ruft gegenwärtig ausgeführte Vorgänge für gelöschte Server ab. |
> | Aktion | Microsoft.Sql/locations/deletedServers/read | Gibt die Liste der gelöschten Server zurück oder ruft die Eigenschaften für den angegebenen gelöschten Server ab. |
> | Aktion | Microsoft.Sql/locations/deletedServers/recover/action | Stellt einen gelöschten Server wieder her. |
> | Aktion | Microsoft.Sql/locations/deleteVirtualNetworkOrSubnets/action | Löscht Regeln für virtuelle Netzwerke, die einem virtuellen Netzwerk oder Subnetz zugeordnet sind. |
> | Aktion | Microsoft.Sql/locations/elasticPoolAzureAsyncOperation/read | Ruft den asynchronen Vorgang in Azure für einen asynchronen Vorgang in einem Pool für elastische Datenbank ab. |
> | Aktion | Microsoft.Sql/locations/elasticPoolOperationResults/read | Ruft das Ergebnis eines Vorgangs eines Pools für elastische Datenbanken ab. |
> | Aktion | Microsoft.Sql/locations/extendedAuditingSettingsAzureAsyncOperation/read | Dient zum Abrufen des Ergebnisses des Set-Vorgangs der Richtlinie für die erweiterte Serverblobüberwachung. |
> | Aktion | Microsoft.Sql/locations/extendedAuditingSettingsOperationResults/read | Dient zum Abrufen des Ergebnisses des Set-Vorgangs der Richtlinie für die erweiterte Serverblobüberwachung. |
> | Aktion | Microsoft.Sql/locations/instanceFailoverGroups/delete | Löscht eine vorhandene Failovergruppe einer Instanz. |
> | Aktion | Microsoft.Sql/locations/instanceFailoverGroups/failover/action | Führt das geplante Failover in einer vorhandenen Failovergruppe einer Instanz durch. |
> | Aktion | Microsoft.Sql/locations/instanceFailoverGroups/forceFailoverAllowDataLoss/action | Führt das erzwungene Failover in einer vorhandenen Failovergruppe einer Instanz durch. |
> | Aktion | Microsoft.Sql/locations/instanceFailoverGroups/read | Gibt die Liste der Failovergruppen einer Instanz zurück oder ruft die Eigenschaften für die angegebene Failovergruppe einer Instanz ab. |
> | Aktion | Microsoft.Sql/locations/instanceFailoverGroups/write | Erstellt eine Failovergruppe einer Instanz mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Tags für die angegebene Failovergruppe einer Instanz. |
> | Aktion | Microsoft.Sql/locations/managedDatabaseRestoreAzureAsyncOperation/completeRestore/action | Führt den Vorgang zur Wiederherstellung einer verwalteten Datenbank durch. |
> | Aktion | Microsoft.Sql/locations/managedTransparentDataEncryptionAzureAsyncOperation/read | Ruft gegenwärtig ausgeführte Vorgänge für Transparent Data Encryption einer verwalteten Datenbank ab. |
> | Aktion | Microsoft.Sql/locations/managedTransparentDataEncryptionOperationResults/read | Ruft gegenwärtig ausgeführte Vorgänge für Transparent Data Encryption einer verwalteten Datenbank ab. |
> | Aktion | Microsoft.Sql/locations/networkInterfaceAzureAsyncOperation/read | Gibt die Details eines asynchronen Azure-Vorgangs für eine bestimmte Netzwerkschnittstelle zurück. |
> | Aktion | Microsoft.Sql/locations/networkInterfaceOperationResults/read | Gibt die Details des Vorgangs der bestimmten Netzwerkschnittstelle zurück. |
> | Aktion | Microsoft.Sql/locations/read | Ruft die verfügbaren Standorte für ein bestimmtes Abonnement ab. |
> | Aktion | Microsoft.Sql/locations/syncAgentOperationResults/read | Ruft das Ergebnis des Ressourcenvorgangs des Synchronisierungs-Agents ab. |
> | Aktion | Microsoft.Sql/locations/syncDatabaseIds/read | Ruft die Synchronisierungsdatenbank-IDs für eine bestimmte Region und ein bestimmtes Abonnement ab. |
> | Aktion | Microsoft.Sql/locations/syncGroupOperationResults/read | Ruft das Ergebnis des Ressourcenvorgangs der Synchronisierungsgruppe ab. |
> | Aktion | Microsoft.Sql/locations/syncMemberOperationResults/read | Ruft das Ergebnis des Ressourcenvorgangs des Synchronisierungsmitglieds ab. |
> | Aktion | Microsoft.Sql/locations/usages/read | Ruft eine Collection von Nutzungsmetriken für dieses Abonnement an einem Standort ab. |
> | Aktion | Microsoft.Sql/locations/virtualNetworkRulesAzureAsyncOperation/read | Gibt die Details des asynchronen Vorgangs in Azure für die Regeln für das angegebene virtuelle Netzwerk zurück.  |
> | Aktion | Microsoft.Sql/locations/virtualNetworkRulesOperationResults/read | Gibt die Details der Regeln für das angegebene virtuelle Netzwerk zurück.  |
> | Aktion | Microsoft.Sql/managedInstances/administrators/delete | Löscht einen vorhandenen Administrator einer verwalteten Instanz. |
> | Aktion | Microsoft.Sql/managedInstances/administrators/read | Ruft eine Liste der Administratoren für verwaltete Instanzen ab. |
> | Aktion | Microsoft.Sql/managedInstances/administrators/write | Erstellt oder aktualisiert den Administrator für verwaltete Instanzen mit den angegebenen Parametern. |
> | Aktion | Microsoft.Sql/managedInstances/databases/delete | Löscht eine vorhandene verwaltete Datenbank. |
> | Aktion | Microsoft.Sql/managedInstances/databases/read | Ruft eine vorhandene verwaltete Datenbank ab. |
> | Aktion | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/read | Dient zum Abrufen von Details zur Bedrohungserkennungsrichtlinie von Datenbanken, die für eine bestimmte verwaltete Datenbank konfiguriert ist. |
> | Aktion | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/write | Dient zum Ändern der Bedrohungserkennungsrichtlinie für eine bestimmte verwaltete Datenbank. |
> | Aktion | Microsoft.Sql/managedInstances/databases/securityEvents/read | Ruft die Sicherheitsereignisse für die verwaltete Datenbank ab. |
> | Aktion | Microsoft.Sql/managedInstances/databases/transparentDataEncryption/read | Ruft Details zu Datenbank-TDE (Transparent Data Encryption) für eine bestimmte verwaltete Datenbank ab. |
> | Aktion | Microsoft.Sql/managedInstances/databases/transparentDataEncryption/write | Ändert Transparent Data Encryption für Datenbanken für eine bestimmte verwaltete Datenbank. |
> | Aktion | Microsoft.Sql/managedInstances/databases/write | Erstellt eine neue Datenbank oder aktualisiert eine bereits vorhandene Datenbank |
> | Aktion | Microsoft.Sql/managedInstances/delete | Löscht eine vorhandene verwaltete Instanz. |
> | Aktion | Microsoft.Sql/managedInstances/metricDefinitions/read | Ruft Metrikdefinitionen der verwalteten Instanz ab. |
> | Aktion | Microsoft.Sql/managedInstances/metrics/read | Ruft Metriken der verwalteten Instanz ab. |
> | Aktion | Microsoft.Sql/managedInstances/read | Gibt die Liste der verwalteten Instanzen zurück oder ruft die Eigenschaften für die angegebene verwaltete Instanz ab. |
> | Aktion | Microsoft.Sql/managedInstances/securityAlertPolicies/read | Dient zum Abrufen von Details zur Richtlinie zur Bedrohungserkennung bei verwalteten Servern, die für einen bestimmten verwalteten Server konfiguriert ist. |
> | Aktion | Microsoft.Sql/managedInstances/securityAlertPolicies/write | Dient zum Ändern der Richtlinie zur Bedrohungserkennung bei verwalteten Servern für einen bestimmten verwalteten Server. |
> | Aktion | Microsoft.Sql/managedInstances/write | Erstellt eine verwaltete Instanz mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für die verwaltete Instanz. |
> | Aktion | Microsoft.Sql/operations/read | Ruft die verfügbaren REST-Vorgänge ab. |
> | Aktion | Microsoft.Sql/register/action | Registriert das Abonnement für den Microsoft SQL-Datenbankressourcenanbieter und ermöglicht die Erstellung von Microsoft SQL-Datenbanken. |
> | Aktion | Microsoft.Sql/servers/administratorOperationResults/read | Ruft gegenwärtig ausgeführte Vorgänge für Serveradministratoren ab. |
> | Aktion | Microsoft.Sql/servers/administrators/delete | Löscht Serveradministratoren. |
> | Aktion | Microsoft.Sql/servers/administrators/read | Dient zum Abrufen von Serveradministratordetails. |
> | Aktion | Microsoft.Sql/servers/administrators/write | Dient zum Erstellen oder Aktualisieren von Serveradministratoren. |
> | Aktion | Microsoft.Sql/servers/advisors/read | Gibt eine Liste mit verfügbaren Ratgebern für den Server zurück. |
> | Aktion | Microsoft.Sql/servers/advisors/recommendedActions/read | Gibt eine Liste mit empfohlenen Aktionen des angegebenen Ratgebers für den Server zurück. |
> | Aktion | Microsoft.Sql/servers/advisors/recommendedActions/write | Dient zum Anwenden der empfohlenen Aktion auf den Server. |
> | Aktion | Microsoft.Sql/servers/advisors/write | Aktualisiert den Status der automatischen Ausführung eines Ratgebers auf der Serverebene. |
> | Aktion | Microsoft.Sql/servers/auditingPolicies/read | Dient zum Abrufen von Details zur Richtlinie für die Standardservertabellen-Überwachung, die für einen bestimmten Server konfiguriert ist. |
> | Aktion | Microsoft.Sql/servers/auditingPolicies/write | Dient zum Ändern der Standardservertabellen-Überwachung für einen bestimmten Server. |
> | Aktion | Microsoft.Sql/servers/auditingSettings/operationResults/read | Dient zum Abrufen des Ergebnisses des Set-Vorgangs der Richtlinie für die Serverblobüberwachung. |
> | Aktion | Microsoft.Sql/servers/auditingSettings/read | Dient zum Abrufen von Details zur Richtlinie für die Serverblobüberwachung, die für einen bestimmten Server konfiguriert ist. |
> | Aktion | Microsoft.Sql/servers/auditingSettings/write | Dient zum Ändern der Serverblobüberwachung für einen bestimmten Server. |
> | Aktion | Microsoft.Sql/servers/automaticTuning/read | Gibt die Einstellungen zur automatischen Optimierung für den Server zurück. |
> | Aktion | Microsoft.Sql/servers/automaticTuning/write | Aktualisiert die Einstellungen zur automatischen Optimierung für den Server und gibt die aktualisierten Einstellungen zurück. |
> | Aktion | Microsoft.Sql/servers/backupLongTermRetentionVaults/delete | Löscht einen vorhandenen Sicherungsarchivtresor. |
> | Aktion | Microsoft.Sql/servers/backupLongTermRetentionVaults/read | Vorgang zum Abrufen eines Sicherungstresors für die langfristige Aufbewahrung. Gibt Informationen zu dem Tresor zurück, der bei diesem Server registriert ist. |
> | Aktion | Microsoft.Sql/servers/backupLongTermRetentionVaults/write | Dieser Vorgang wird zum Registrieren eines Sicherungstresors für die langfristige Aufbewahrung bei einem Server verwendet. |
> | Aktion | Microsoft.Sql/servers/communicationLinks/delete | Löscht eine vorhandene Serverkommunikationsverbindung. |
> | Aktion | Microsoft.Sql/servers/communicationLinks/read | Gibt die Liste der Kommunikationsverbindungen eines bestimmten Servers zurück. |
> | Aktion | Microsoft.Sql/servers/communicationLinks/write | Erstellt oder aktualisiert eine Serverkommunikationsverbindung. |
> | Aktion | Microsoft.Sql/servers/connectionPolicies/read | Gibt die Liste der Serververbindungsrichtlinien eines angegebenen Servers zurück. |
> | Aktion | Microsoft.Sql/servers/connectionPolicies/write | Erstellt oder aktualisiert eine Serververbindungsrichtlinie. |
> | Aktion | Microsoft.Sql/servers/databases/advisors/read | Gibt eine Liste mit verfügbaren Ratgebern für die Datenbank zurück. |
> | Aktion | Microsoft.Sql/servers/databases/advisors/recommendedActions/read | Gibt eine Liste mit empfohlenen Aktionen des angegebenen Ratgebers für die Datenbank zurück. |
> | Aktion | Microsoft.Sql/servers/databases/advisors/recommendedActions/write | Dient zum Anwenden der empfohlenen Aktion auf die Datenbank. |
> | Aktion | Microsoft.Sql/servers/databases/advisors/write | Dient zum Aktualisieren des Status der automatischen Ausführung eines Ratgebers auf der Datenbankebene. |
> | Aktion | Microsoft.Sql/servers/databases/auditingPolicies/read | Dient zum Abrufen von Details zur Tabellenüberwachungsrichtlinie, die für eine bestimmte Datenbank konfiguriert ist. |
> | Aktion | Microsoft.Sql/servers/databases/auditingPolicies/write | Dient zum Ändern der Tabellenüberwachungsrichtlinie für eine bestimmte Datenbank. |
> | Aktion | Microsoft.Sql/servers/databases/auditingSettings/read | Dient zum Abrufen von Details zur Blobüberwachungsrichtlinie, die für eine bestimmte Datenbank konfiguriert ist. |
> | Aktion | Microsoft.Sql/servers/databases/auditingSettings/write | Dient zum Ändern der Blobüberwachungsrichtlinie für eine bestimmte Datenbank. |
> | Aktion | Microsoft.Sql/servers/databases/auditRecords/read | Dient zum Abrufen der Datensätze für die Datenbankblobüberwachung. |
> | Aktion | Microsoft.Sql/servers/databases/automaticTuning/read | Gibt die Einstellungen zur automatischen Optimierung für eine Datenbank zurück. |
> | Aktion | Microsoft.Sql/servers/databases/automaticTuning/write | Aktualisiert die Einstellungen zur automatischen Optimierung für eine Datenbank und gibt die aktualisierten Einstellungen zurück. |
> | Aktion | Microsoft.Sql/servers/databases/azureAsyncOperation/read | Ruft den Status eines Datenbankvorgangs ab. |
> | Aktion | Microsoft.Sql/servers/databases/backupLongTermRetentionPolicies/read | Gibt die Liste der Sicherungsarchivierungsrichtlinien einer angegebenen Datenbank zurück. |
> | Aktion | Microsoft.Sql/servers/databases/backupLongTermRetentionPolicies/write | Erstellt oder aktualisiert eine Datenbanksicherungs-Archivierungsrichtlinie. |
> | Aktion | Microsoft.Sql/servers/databases/connectionPolicies/read | Dient zum Abrufen von Details zur Verbindungsrichtlinie, die für eine bestimmte Datenbank konfiguriert ist. |
> | Aktion | Microsoft.Sql/servers/databases/connectionPolicies/write | Dient zum Ändern der Verbindungsrichtlinie für eine bestimmte Datenbank. |
> | Aktion | Microsoft.Sql/servers/databases/dataMaskingPolicies/read | Gibt die Liste der Richtlinien zur Maskierung von Datenbankdaten zurück. |
> | Aktion | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/delete | Dient zum Löschen von Datenmaskierungsrichtlinien-Regeln für eine bestimmte Datenbank. |
> | Aktion | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/read | Dient zum Abrufen von Details zur Datenmaskierungsrichtlinien-Regel, die für eine bestimmte Datenbank konfiguriert ist. |
> | Aktion | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/write | Dient zum Ändern von Datenmaskierungsrichtlinen-Regeln für eine bestimmte Datenbank. |
> | Aktion | Microsoft.Sql/servers/databases/dataMaskingPolicies/write | Dient zum Ändern der Datenmaskierungsrichtlinie für eine bestimmte Datenbank. |
> | Aktion | Microsoft.Sql/servers/databases/dataWarehouseQueries/dataWarehouseQuerySteps/read | Gibt die verteilten Abfrageschrittinformationen von Data Warehouse-Abfragen für die ausgewählte Schritt-ID zurück. |
> | Aktion | Microsoft.Sql/servers/databases/dataWarehouseQueries/read | Gibt die Data Warehouse-Verteilungsabfrageinformationen für die ausgewählte Abfrage-ID zurück. |
> | Aktion | Microsoft.Sql/servers/databases/dataWarehouseUserActivities/read | Ruft die Benutzeraktivitäten einer SQL Data Warehouse-Instanz ab, einschließlich ausgeführte und angehaltene Abfragen. |
> | Aktion | Microsoft.Sql/servers/databases/delete | Löscht eine vorhandene Datenbank. |
> | Aktion | Microsoft.Sql/servers/databases/export/action | Exportiert Azure SQL-Datenbank. |
> | Aktion | Microsoft.Sql/servers/databases/extendedAuditingSettings/read | Dient zum Abrufen von Details zur erweiterten Blobüberwachungsrichtlinie, die für eine bestimmte Datenbank konfiguriert ist. |
> | Aktion | Microsoft.Sql/servers/databases/extendedAuditingSettings/write | Dient zum Ändern der erweiterten Blobüberwachungsrichtlinie für eine bestimmte Datenbank. |
> | Aktion | Microsoft.Sql/servers/databases/extensions/read | Ruft eine Collection von Erweiterungen für die Datenbank ab. |
> | Aktion | Microsoft.Sql/servers/databases/extensions/write | Ändert die Erweiterung für eine bestimmte Datenbank. |
> | Aktion | Microsoft.Sql/servers/databases/geoBackupPolicies/read | Ruft Richtlinien für die Geosicherung einer bestimmten Datenbank ab. |
> | Aktion | Microsoft.Sql/servers/databases/geoBackupPolicies/write | Erstellt oder aktualisiert eine Richtlinie für die Geosicherung einer Datenbank. |
> | Aktion | Microsoft.Sql/servers/databases/importExportOperationResults/read | Ruft gegenwärtig ausgeführte Import-/Exportvorgänge ab. |
> | Aktion | Microsoft.Sql/servers/databases/metricDefinitions/read | Dient zum Zurückgeben verfügbarer Metriktypen für Datenbanken. |
> | Aktion | Microsoft.Sql/servers/databases/metrics/read | Gibt Metriken für Datenbanken zurück. |
> | Aktion | Microsoft.Sql/servers/databases/move/action | Benennt eine Azure SQL-Datenbank um. |
> | Aktion | Microsoft.Sql/servers/databases/operationResults/read | Ruft den Status eines Datenbankvorgangs ab. |
> | Aktion | Microsoft.Sql/servers/databases/operations/cancel/action | Bricht einen ausstehenden asynchronen Vorgang in Azure SQL-Datenbank ab, der noch nicht abgeschlossen ist. |
> | Aktion | Microsoft.Sql/servers/databases/operations/read | Gibt die Liste der Vorgänge zurück, die für die Datenbank ausgeführt werden. |
> | Aktion | Microsoft.Sql/servers/databases/pause/action | Hält die Azure SQL Data Warehouse-Datenbank an. |
> | Aktion | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/logDefinitions/read | Ruft die verfügbaren Protokolle für Datenbanken ab. |
> | Aktion | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/metricDefinitions/read | Dient zum Zurückgeben verfügbarer Metriktypen für Datenbanken. |
> | Aktion | Microsoft.Sql/servers/databases/queryStore/queryTexts/read | Gibt die Collection von Abfragetexten zurück, die den angegebenen Parametern entsprechen. |
> | Aktion | Microsoft.Sql/servers/databases/queryStore/read | Gibt die aktuellen Werte von Abfragespeichereinstellungen für die Datenbank zurück. |
> | Aktion | Microsoft.Sql/servers/databases/queryStore/write | Aktualisiert die Abfragespeichereinstellung für die Datenbank. |
> | Aktion | Microsoft.Sql/servers/databases/read | Gibt die Liste der Datenbanken zurück oder ruft die Eigenschaften für die angegebene Datenbank ab. |
> | Aktion | Microsoft.Sql/servers/databases/replicationLinks/delete | Dient zum Erzwingen der Beendigung der Replikationsbeziehung mit potenziellem Datenverlust. |
> | Aktion | Microsoft.Sql/servers/databases/replicationLinks/failover/action | Dient zum Ausführen eines Failovers nach der Synchronisierung aller Änderungen der primären Datenbank. Dadurch wird diese Datenbank zur primären Datenbank der Replikationsbeziehung, und die primäre Remotedatenbank wird zur sekundären Datenbank. |
> | Aktion | Microsoft.Sql/servers/databases/replicationLinks/forceFailoverAllowDataLoss/action | Dient zum Ausführen eines sofortigen Failovers mit potenziellem Datenverlust. Dadurch wird diese Datenbank zur primären Datenbank der Replikationsbeziehung, und die primäre Remotedatenbank wird zur sekundären Datenbank. |
> | Aktion | Microsoft.Sql/servers/databases/replicationLinks/read | Dient zum Zurückgeben von Details zu Replikationslinks, die für eine bestimmte Datenbank eingerichtet wurden. |
> | Aktion | Microsoft.Sql/servers/databases/replicationLinks/unlink/action | Dient zum Beenden der Replikationsbeziehung – entweder erzwungen oder nach der Synchronisierung mit dem Partner. |
> | Aktion | Microsoft.Sql/servers/databases/replicationLinks/updateReplicationMode/action | Dient zum Aktualisieren des Replikationsmodus für die Verknüpfung auf den synchronen oder asynchronen Modus. |
> | Aktion | Microsoft.Sql/servers/databases/restorePoints/action | Erstellt einen neuen Wiederherstellungspunkt. |
> | Aktion | Microsoft.Sql/servers/databases/restorePoints/delete | Löscht den Wiederherstellungspunkt der Datenbank. |
> | Aktion | Microsoft.Sql/servers/databases/restorePoints/read | Gibt Wiederherstellungspunkte für die Datenbank zurück. |
> | Aktion | Microsoft.Sql/servers/databases/resume/action | Setzt die Azure SQL Data Warehouse-Datenbank fort. |
> | Aktion | Microsoft.Sql/servers/databases/schemas/read | Ruft die Liste der Schemas einer Datenbank ab. |
> | Aktion | Microsoft.Sql/servers/databases/schemas/tables/columns/read | Dient zum Abrufen einer Liste mit Tabellenspalten. |
> | Aktion | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/delete | Löscht die Vertraulichkeitsbezeichnung einer bestimmten Spalte. |
> | Aktion | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/read | Ruft die Vertraulichkeitsbezeichnung einer bestimmten Spalte ab. |
> | Aktion | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/write | Erstellt oder aktualisiert die Vertraulichkeitsbezeichnung einer bestimmten Spalte. |
> | Aktion | Microsoft.Sql/servers/databases/schemas/tables/read | Ruft die Liste der Tabellen einer Datenbank ab. |
> | Aktion | Microsoft.Sql/servers/databases/schemas/tables/recommendedIndexes/read | Dient zum Abrufen einer Liste mit Indexempfehlungen für eine Datenbank. |
> | Aktion | Microsoft.Sql/servers/databases/schemas/tables/recommendedIndexes/write | Dient zum Anwenden von Indexempfehlungen. |
> | Aktion | Microsoft.Sql/servers/databases/securityAlertPolicies/read | Dient zum Abrufen von Details zur Bedrohungserkennungsrichtlinie, die für eine bestimmte Datenbank konfiguriert ist. |
> | Aktion | Microsoft.Sql/servers/databases/securityAlertPolicies/write | Dient zum Ändern der Bedrohungserkennungsrichtlinie für eine bestimmte Datenbank. |
> | Aktion | Microsoft.Sql/servers/databases/securityMetrics/read | Ruft eine Collection von Metriken für Datenbanksicherheit ab. |
> | Aktion | Microsoft.Sql/servers/databases/sensitivityLabels/read | Listet die Vertraulichkeitsbezeichnungen einer bestimmten Datenbank auf. |
> | Aktion | Microsoft.Sql/servers/databases/serviceTierAdvisors/read | Gibt Vorschläge zum zentralen Hoch- oder Herunterskalieren von Datenbanken auf der Grundlage von Abfrageausführungsstatistiken zurück, um die Leistung zu verbessern oder die Kosten zu verringern. |
> | Aktion | Microsoft.Sql/servers/databases/skus/read | Ruft eine Collection von SKUs ab, die für eine Datenbank verfügbar ist. |
> | Aktion | Microsoft.Sql/servers/databases/syncGroups/cancelSync/action | Bricht die Synchronisierung von Synchronisierungsgruppen ab. |
> | Aktion | Microsoft.Sql/servers/databases/syncGroups/delete | Löscht eine vorhandene Synchronisierungsgruppe. |
> | Aktion | Microsoft.Sql/servers/databases/syncGroups/hubSchemas/read | Gibt die Liste der Datenbankschemas für Synchronisierungshubs zurück. |
> | Aktion | Microsoft.Sql/servers/databases/syncGroups/logs/read | Gibt die Liste der Synchronisierungsgruppenprotokolle zurück. |
> | Aktion | Microsoft.Sql/servers/databases/syncGroups/read | Gibt die Liste der Synchronisierungsgruppen zurück oder ruft die Eigenschaften für die angegebene Synchronisierungsgruppe ab. |
> | Aktion | Microsoft.Sql/servers/databases/syncGroups/refreshHubSchema/action | Aktualisiert das Datenbankschema des Synchronisierungshubs. |
> | Aktion | Microsoft.Sql/servers/databases/syncGroups/refreshHubSchemaOperationResults/read | Ruft das Ergebnis des Aktualisierungsvorgangs für das Schema des Synchronisierungshubs ab. |
> | Aktion | Microsoft.Sql/servers/databases/syncGroups/syncMembers/delete | Löscht ein vorhandenes Synchronisierungsmitglied. |
> | Aktion | Microsoft.Sql/servers/databases/syncGroups/syncMembers/read | Gibt die Liste der Synchronisierungsmitglieder zurück oder ruft die Eigenschaften für das angegebene Synchronisierungsmitglied ab. |
> | Aktion | Microsoft.Sql/servers/databases/syncGroups/syncMembers/refreshSchema/action | Aktualisiert das Schema für das Synchronisierungsmitglied. |
> | Aktion | Microsoft.Sql/servers/databases/syncGroups/syncMembers/refreshSchemaOperationResults/read | Ruft das Ergebnis des Aktualisierungsvorgangs für das Schema für das Synchronisierungsmitglied ab. |
> | Aktion | Microsoft.Sql/servers/databases/syncGroups/syncMembers/schemas/read | Gibt die Liste von Schemas für Synchronisierungsmitglieder zurück. |
> | Aktion | Microsoft.Sql/servers/databases/syncGroups/syncMembers/write | Erstellt ein Synchronisierungsmitglied mit den angegebenen Parametern oder aktualisiert die Eigenschaften für das angegebene Synchronisierungsmitglied. |
> | Aktion | Microsoft.Sql/servers/databases/syncGroups/triggerSync/action | Löst die Synchronisierung für die Synchronisierungsgruppe aus. |
> | Aktion | Microsoft.Sql/servers/databases/syncGroups/write | Erstellt eine Synchronisierungsgruppe mit den angegebenen Parametern oder aktualisiert die Eigenschaften für die angegebene Synchronisierungsgruppe. |
> | Aktion | Microsoft.Sql/servers/databases/topQueries/queryText/action | Gibt den Transact-SQL-Text für die ausgewählte Abfrage-ID zurück. |
> | Aktion | Microsoft.Sql/servers/databases/topQueries/read | Gibt aggregierte Laufzeitstatistiken für die ausgewählte Abfrage im ausgewählten Zeitraum zurück. |
> | Aktion | Microsoft.Sql/servers/databases/topQueries/statistics/read | Gibt aggregierte Laufzeitstatistiken für die ausgewählte Abfrage im ausgewählten Zeitraum zurück. |
> | Aktion | Microsoft.Sql/servers/databases/transparentDataEncryption/operationResults/read | Ruft gegenwärtig ausgeführte Vorgänge für Transparent Data Encryption ab. |
> | Aktion | Microsoft.Sql/servers/databases/transparentDataEncryption/read | Dient zum Abrufen von Status und Details des Transparent Data Encryption-Sicherheitsfeatures für eine bestimmte Datenbank. |
> | Aktion | Microsoft.Sql/servers/databases/transparentDataEncryption/write | Ändert den Status von Transparent Data Encryption. |
> | Aktion | Microsoft.Sql/servers/databases/upgradeDataWarehouse/action | Führt ein Upgrade für die Azure SQL Data Warehouse-Datenbank durch. |
> | Aktion | Microsoft.Sql/servers/databases/usages/read | Ruft Informationen zur Azure SQL-Datenbanknutzung ab. |
> | Aktion | Microsoft.Sql/servers/databases/vulnerabilityAssessments/delete | Entfernt die Sicherheitsrisikobewertung für eine bestimmte Datenbank. |
> | Aktion | Microsoft.Sql/servers/databases/vulnerabilityAssessments/read | Dient zum Abrufen von Details zur Sicherheitsrisikobewertung, die für eine bestimmte Datenbank konfiguriert ist. |
> | Aktion | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/delete | Entfernt die Baseline für die Regel für die Sicherheitsrisikobewertung für eine bestimmte Datenbank. |
> | Aktion | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/read | Ruft das Baseline für die Regel für die Sicherheitsrisikobewertung für eine bestimmte Datenbank ab. |
> | Aktion | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/write | Ändert die Baseline für die Regel für die Sicherheitsrisikobewertung für eine bestimmte Datenbank. |
> | Aktion | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/export/action | Konvertiert ein vorhandenes Überprüfungsergebnis in ein lesbares Format. Wenn dies bereits der Fall ist, werden keine Vorgänge ausgeführt. |
> | Aktion | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/initiateScan/action | Dient zum Ausführen einer Datenbanküberprüfung zur Bewertung von Sicherheitsrisiken. |
> | Aktion | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/read | Gibt die Liste der Überprüfungsdatensätze für eine Datenbank-Sicherheitsrisikobewertung oder den Überprüfungsdatensatz für die angegebene Überprüfungs-ID zurück. |
> | Aktion | Microsoft.Sql/servers/databases/vulnerabilityAssessments/write | Ändert die Sicherheitsrisikobewertung für eine bestimmte Datenbank. |
> | Aktion | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/action | Dient zum Ausführen einer Datenbanküberprüfung zur Bewertung von Sicherheitsrisiken. |
> | Aktion | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/operationResults/read | Ruft das Ergebnis des Ausführungsvorgangs für die Datenbank-Sicherheitsrisikobewertung ab. |
> | Aktion | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/read | Dient zum Abrufen von Details zur Sicherheitsrisikobewertung, die für eine bestimmte Datenbank konfiguriert ist. |
> | Aktion | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/write | Ändert die Sicherheitsrisikobewertung für eine bestimmte Datenbank. |
> | Aktion | Microsoft.Sql/servers/databases/write | Erstellt eine Datenbank mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für die angegebene Datenbank. |
> | Aktion | Microsoft.Sql/servers/delete | Löscht einen vorhandenen Server. |
> | Aktion | Microsoft.Sql/servers/disasterRecoveryConfiguration/delete | Löscht vorhandene Notfallwiederherstellungskonfigurationen für einen bestimmten Server. |
> | Aktion | Microsoft.Sql/servers/disasterRecoveryConfiguration/failover/action | Führt ein Failover für DisasterRecoveryConfiguration durch. |
> | Aktion | Microsoft.Sql/servers/disasterRecoveryConfiguration/forceFailoverAllowDataLoss/action | Erzwingen Sie ein Failover für DisasterRecoveryConfiguration. |
> | Aktion | Microsoft.Sql/servers/disasterRecoveryConfiguration/read | Ruft eine Collection von Notfallwiederherstellungskonfigurationen ab, die diesen Server umfassen. |
> | Aktion | Microsoft.Sql/servers/disasterRecoveryConfiguration/write | Ändert die Notfallwiederherstellungskonfiguration des Servers. |
> | Aktion | Microsoft.Sql/servers/elasticPoolEstimates/read | Gibt die Liste mit Schätzwerten für den Pool für elastische Datenbanken zurück, die bereits für diesen Server erstellt wurden. |
> | Aktion | Microsoft.Sql/servers/elasticPoolEstimates/write | Erstellt für die Liste der bereitgestellten Datenbanken neue Schätzwerte für den Pool für elastische Datenbanken. |
> | Aktion | Microsoft.Sql/servers/elasticPools/advisors/read | Gibt eine Liste mit verfügbaren Ratgebern für den Pool für elastische Datenbanken zurück. |
> | Aktion | Microsoft.Sql/servers/elasticPools/advisors/recommendedActions/read | Gibt eine Liste mit empfohlenen Aktionen des angegebenen Ratgebers für den Pool für elastische Datenbanken zurück. |
> | Aktion | Microsoft.Sql/servers/elasticPools/advisors/recommendedActions/write | Dient zum Anwenden der empfohlenen Aktion auf den Pool für elastische Datenbanken. |
> | Aktion | Microsoft.Sql/servers/elasticPools/advisors/write | Dient zum Aktualisieren des Status der automatischen Ausführung eines Ratgebers auf der Ebene von Pools für elastische Datenbanken. |
> | Aktion | Microsoft.Sql/servers/elasticPools/databases/read | Ruft eine Liste der Datenbanken für einen Pool für elastische Datenbanken zurück. |
> | Aktion | Microsoft.Sql/servers/elasticPools/delete | Löscht einen vorhandenen Pool für elastische Datenbanken. |
> | Aktion | Microsoft.Sql/servers/elasticPools/elasticPoolActivity/read | Dient zum Abrufen von Aktivitäten und Details zu einem bestimmten Pool für elastische Datenbanken. |
> | Aktion | Microsoft.Sql/servers/elasticPools/elasticPoolDatabaseActivity/read | Dient zum Abrufen von Aktivitäten und Details zu einer bestimmten Datenbank, die einem Pool für elastische Datenbanken angehört. |
> | Aktion | Microsoft.Sql/servers/elasticPools/metricDefinitions/read | Dient zum Zurückgeben verfügbarer Metriktypen für Pools für elastische Datenbanken. |
> | Aktion | Microsoft.Sql/servers/elasticPools/metrics/read | Gibt Metriken für Pools für elastische Datenbanken zurück. |
> | Aktion | Microsoft.Sql/servers/elasticPools/operations/cancel/action | Bricht einen ausstehenden asynchronen Vorgang in einem Pool für elastische Azure SQL-Datenbanken ab, der noch nicht abgeschlossen ist. |
> | Aktion | Microsoft.Sql/servers/elasticPools/operations/read | Gibt die Liste der Vorgänge zurück, die für den Pool für elastische Datenbanken ausgeführt werden. |
> | Aktion | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/metricDefinitions/read | Dient zum Zurückgeben verfügbarer Metriktypen für Pools für elastische Datenbanken. |
> | Aktion | Microsoft.Sql/servers/elasticPools/read | Dient zum Abrufen von Details zu Pools für elastische Datenbanken auf einem bestimmten Server. |
> | Aktion | Microsoft.Sql/servers/elasticPools/skus/read | Ruft eine Collection von SKUs ab, die für einen Pool für elastische Datenbanken verfügbar ist. |
> | Aktion | Microsoft.Sql/servers/elasticPools/write | Dient zum Erstellen eines neuen Pools für elastische Datenbanken oder zum Ändern von Eigenschaften vorhandener Pools für elastische Datenbanken. |
> | Aktion | Microsoft.Sql/servers/encryptionProtector/read | Gibt eine Liste von Schutzvorrichtungen zur Serververschlüsselung zurück oder ruft die Eigenschaften für die angegebene Schutzvorrichtung zur Serververschlüsselung ab. |
> | Aktion | Microsoft.Sql/servers/encryptionProtector/write | Aktualisiert die Eigenschaften für die angegebene Schutzvorrichtung zur Serververschlüsselung. |
> | Aktion | Microsoft.Sql/servers/extendedAuditingSettings/read | Dient zum Abrufen von Details zur Richtlinie für die erweiterte Serverblobüberwachung, die für einen bestimmten Server konfiguriert ist. |
> | Aktion | Microsoft.Sql/servers/extendedAuditingSettings/write | Dient zum Ändern der erweiterten Serverblobüberwachung für einen bestimmten Server. |
> | Aktion | Microsoft.Sql/servers/failoverGroups/delete | Löscht eine vorhandene Failovergruppe. |
> | Aktion | Microsoft.Sql/servers/failoverGroups/failover/action | Führt das geplante Failover in einer vorhandenen Failovergruppe durch. |
> | Aktion | Microsoft.Sql/servers/failoverGroups/forceFailoverAllowDataLoss/action | Führt ein erzwungenes Failover in einer vorhandenen Failovergruppe aus. |
> | Aktion | Microsoft.Sql/servers/failoverGroups/read | Gibt die Liste der Failovergruppen zurück oder ruft die Eigenschaften für die angegebene Failovergruppe ab. |
> | Aktion | Microsoft.Sql/servers/failoverGroups/write | Erstellt eine Failovergruppe mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für die angegebene Failovergruppe. |
> | Aktion | Microsoft.Sql/servers/firewallRules/delete | Löscht eine vorhandene Serverfirewallregel. |
> | Aktion | Microsoft.Sql/servers/firewallRules/read | Gibt die Liste der Serverfirewallregeln zurück oder ruft die Eigenschaften für die angegebene Serverfirewallregel ab. |
> | Aktion | Microsoft.Sql/servers/firewallRules/write | Erstellt eine Serverfirewallregel mit den angegebenen Parametern, aktualisiert die Eigenschaften für die angegebene Regel oder überschreibt alle vorhandenen Regeln durch neue Serverfirewallregel. |
> | Aktion | Microsoft.Sql/servers/import/action | Dient zum Erstellen einer neuen Datenbank auf dem Server und zum Bereitstellen von Schema und Daten aus einem DACPAC-Paket. |
> | Aktion | Microsoft.Sql/servers/importExportOperationResults/read | Ruft gegenwärtig ausgeführte Import-/Exportvorgänge ab. |
> | Aktion | Microsoft.Sql/servers/keys/delete | Löscht einen vorhandenen Serverschlüssel. |
> | Aktion | Microsoft.Sql/servers/keys/read | Gibt die Liste der Serverschlüssel zurück oder ruft die Eigenschaften für den angegebenen Serverschlüssel ab. |
> | Aktion | Microsoft.Sql/servers/keys/write | Erstellt einen Schlüssel mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für den angegebenen Serverschlüssel. |
> | Aktion | Microsoft.Sql/servers/networkInterfaces/read | Gibt die Eigenschaften für die angegebenen Netzwerkschnittstelle zurück. |
> | Aktion | Microsoft.Sql/servers/networkInterfaces/write | Erstellt eine Netzwerkschnittstelle mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Tags für die angegebene Netzwerkschnittstelle. |
> | Aktion | Microsoft.Sql/servers/operationResults/read | Ruft gegenwärtig ausgeführte Servervorgänge ab. |
> | Aktion | Microsoft.Sql/servers/providers/Microsoft.Insights/metricDefinitions/read | Dient zum Zurückgeben verfügbarer Metriken für Server. |
> | Aktion | Microsoft.Sql/servers/read | Gibt die Liste der Server zurück oder ruft die Eigenschaften für den angegebenen Server ab. |
> | Aktion | Microsoft.Sql/servers/recommendedElasticPools/databases/read | Dient zum Abrufen von Metriken für empfohlene Pools für elastische Datenbanken für einen bestimmten Server. |
> | Aktion | Microsoft.Sql/servers/recommendedElasticPools/read | Dient zum Abrufen von Empfehlungen für Pools für elastische Datenbanken, um Kosten zu senken oder die Leistung zu verbessern. Die Empfehlungen basieren auf der Ressourcenverwendung in der Vergangenheit. |
> | Aktion | Microsoft.Sql/servers/recoverableDatabases/read | Vorgang für die Notfallwiederherstellung der Livedatenbank, um die Datenbank auf der Grundlage des letzten als funktionierend bekannten Sicherungspunkts wiederherzustellen. Gibt Informationen zur letzten als funktionierend bekannten Sicherung zurück, stellt die Datenbank aber nicht wieder her. |
> | Aktion | Microsoft.Sql/servers/restorableDroppedDatabases/read | Dient zum Abrufen einer Liste mit Datenbanken, die auf einem bestimmten Server gelöscht wurden und weiterhin in einer Aufbewahrungsrichtlinie enthalten sind. |
> | Aktion | Microsoft.Sql/servers/securityAlertPolicies/operationResults/read | Dient zum Abrufen von Ergebnissen des Schreibvorgangs der Richtlinie für die Serverbedrohungserkennung. |
> | Aktion | Microsoft.Sql/servers/securityAlertPolicies/read | Dient zum Abrufen von Details zur Richtlinie für die Serverbedrohungserkennung, die für einen bestimmten Server konfiguriert ist. |
> | Aktion | Microsoft.Sql/servers/securityAlertPolicies/write | Dient zum Ändern der Richtlinie für die Serverbedrohungserkennung für einen bestimmten Server. |
> | Aktion | Microsoft.Sql/servers/serviceObjectives/read | Dient zum Abrufen einer Liste mit verfügbaren Servicelevelzielen (auch Leistungsstufen genannt) für einen bestimmten Server. |
> | Aktion | Microsoft.Sql/servers/syncAgents/delete | Löscht einen vorhandenen Synchronisierungs-Agent. |
> | Aktion | Microsoft.Sql/servers/syncAgents/generateKey/action | Generiert einen Registrierungsschlüssel für den Synchronisierungs-Agent. |
> | Aktion | Microsoft.Sql/servers/syncAgents/linkedDatabases/read | Gibt die Liste der mit dem Synchronisierungs-Agent verknüpften Datenbanken zurück. |
> | Aktion | Microsoft.Sql/servers/syncAgents/read | Gibt die Liste der Synchronisierungs-Agents zurück oder ruft die Eigenschaften für den angegebenen Synchronisierungs-Agent ab. |
> | Aktion | Microsoft.Sql/servers/syncAgents/write | Erstellt einen Synchronisierungs-Agent mit den angegebenen Parametern oder aktualisiert die Eigenschaften für den angegebenen Synchronisierungs-Agent. |
> | Aktion | Microsoft.Sql/servers/usages/read | Dient zum Zurückgeben des Server-DTU-Kontingents und des aktuellen DTU-Verbrauchs durch alle Datenbanken auf dem Server. |
> | Aktion | Microsoft.Sql/servers/virtualNetworkRules/delete | Löscht eine vorhandene Regel für virtuelle Netzwerke. |
> | Aktion | Microsoft.Sql/servers/virtualNetworkRules/read | Gibt die Liste der Regeln für virtuelle Netzwerke zurück oder ruft die Eigenschaften für die angegebene Regel für virtuelle Netzwerke ab. |
> | Aktion | Microsoft.Sql/servers/virtualNetworkRules/write | Erstellt eine Regel für virtuelle Netzwerke mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für die angegebene Regel für virtuelle Netzwerke. |
> | Aktion | Microsoft.Sql/servers/write | Erstellt einen Server mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Markierungen für den angegebenen Server. |
> | Aktion | Microsoft.Sql/unregister/action | Hebt die Registrierung für das Abonnement für den Microsoft SQL-Datenbankressourcenanbieter auf und ermöglicht die Erstellung von Microsoft SQL-Datenbanken. |
> | Aktion | Microsoft.Sql/virtualClusters/read | Gibt die Liste der Regeln für virtuelle Cluster zurück oder ruft die Eigenschaften für das angegebene virtuelle Cluster ab. |
> | Aktion | Microsoft.Sql/virtualClusters/write | Aktualisiert die Markierungen virtueller Cluster. |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Storage/checknameavailability/read | Prüft, ob der Kontoname gültig ist und noch nicht verwendet wird. |
> | Aktion | Microsoft.Storage/locations/deleteVirtualNetworkOrSubnets/action | Benachrichtigt Microsoft.Storage darüber, dass das virtuelle Netzwerk oder Subnetz gelöscht wird. |
> | Aktion | Microsoft.Storage/operations/read | Fragt den Status eines asynchronen Vorgangs ab. |
> | Aktion | Microsoft.Storage/register/action | Registriert das Abonnement für den Speicherressourcenanbieter und ermöglicht die Erstellung von Speicherkonten. |
> | Aktion | Microsoft.Storage/skus/read | Listet die von Microsoft.Storage unterstützten SKUs auf. |
> | Aktion | Microsoft.Storage/storageAccounts/blobServices/containers/clearLegalHold/action | Löscht die rechtliche Aufbewahrungspflicht für Blobcontainer. |
> | Aktion | Microsoft.Storage/storageAccounts/blobServices/containers/delete | Gibt das Ergebnis beim Löschen eines Containers zurück. |
> | Aktion | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/delete | Löscht die Unveränderlichkeitsrichtlinie für Blobcontainer. |
> | Aktion | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/extend/action | Erweitert die Unveränderlichkeitsrichtlinie für Blobcontainer. |
> | Aktion | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/lock/action | Sperrt die Unveränderlichkeitsrichtlinie für Blobcontainer. |
> | Aktion | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/read | Ruft die Unveränderlichkeitsrichtlinie für Blobcontainer ab. |
> | Aktion | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/write | Legt die Unveränderlichkeitsrichtlinie für Blobcontainer fest. |
> | Aktion | Microsoft.Storage/storageAccounts/blobServices/containers/read | Gibt einen Container oder eine Liste von Containern zurück. |
> | Aktion | Microsoft.Storage/storageAccounts/blobServices/containers/setLegalHold/action | Legt die rechtliche Aufbewahrungspflicht für Blobcontainer fest. |
> | Aktion | Microsoft.Storage/storageAccounts/blobServices/containers/write | Hiermit wird das Ergebnis des Vorgangs zum Festlegen oder Leasen eines Blobcontainers zurückgegeben. |
> | Aktion | Microsoft.Storage/storageAccounts/blobServices/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | Microsoft.Storage/storageAccounts/blobServices/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.Storage/storageAccounts/blobServices/providers/Microsoft.Insights/metricDefinitions/read | Hiermit wird die Liste der Microsoft Storage-Metrikdefinitionen abgerufen. |
> | Aktion | Microsoft.Storage/storageAccounts/blobServices/read | Gibt Eigenschaften oder Statistiken des Blob-Diensts zurück. |
> | Aktion | Microsoft.Storage/storageAccounts/blobServices/write | Hiermit wird das Ergebnis des Vorgangs zum Festlegen von Eigenschaften des Blob-Diensts zurückgegeben. |
> | Aktion | Microsoft.Storage/storageAccounts/delete | Löscht ein vorhandenes Speicherkonto. |
> | Aktion | Microsoft.Storage/storageAccounts/fileServices/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | Microsoft.Storage/storageAccounts/fileServices/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.Storage/storageAccounts/fileServices/providers/Microsoft.Insights/metricDefinitions/read | Hiermit wird die Liste der Microsoft Storage-Metrikdefinitionen abgerufen. |
> | Aktion | Microsoft.Storage/storageAccounts/listAccountSas/action | Gibt das Konto-SAS-Token für das angegebene Speicherkonto zurück. |
> | Aktion | Microsoft.Storage/storageAccounts/listkeys/action | Gibt die Zugriffsschlüssel für das angegebene Speicherkonto zurück. |
> | Aktion | Microsoft.Storage/storageAccounts/listServiceSas/action | Gibt das Dienst-SAS-Token für das angegebene Speicherkonto zurück. |
> | Aktion | Microsoft.Storage/storageAccounts/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | Microsoft.Storage/storageAccounts/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.Storage/storageAccounts/providers/Microsoft.Insights/metricDefinitions/read | Hiermit wird die Liste der Microsoft Storage-Metrikdefinitionen abgerufen. |
> | Aktion | Microsoft.Storage/storageAccounts/queueServices/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.Storage/storageAccounts/queueServices/providers/Microsoft.Insights/metricDefinitions/read | Hiermit wird die Liste der Microsoft Storage-Metrikdefinitionen abgerufen. |
> | Aktion | Microsoft.Storage/storageAccounts/queueServices/queues/delete | Gibt das Ergebnis beim Löschen einer Warteschlange zurück. |
> | Aktion | Microsoft.Storage/storageAccounts/queueServices/queues/read | Gibt eine Warteschlange oder eine Liste von Warteschlangen zurück. |
> | Aktion | Microsoft.Storage/storageAccounts/queueServices/queues/write | Gibt das Ergebnis beim Schreiben einer Warteschlange zurück. |
> | Aktion | Microsoft.Storage/storageAccounts/queueServices/read | Gibt Eigenschaften oder Statistiken des Warteschlangendiensts zurück. |
> | Aktion | Microsoft.Storage/storageAccounts/queueServices/write | Gibt das Ergebnis beim Festlegen von Eigenschaften des Warteschlangendiensts zurück. |
> | Aktion | Microsoft.Storage/storageAccounts/read | Gibt die Liste mit Speicherkonten zurück oder ruft die Eigenschaften für das angegebene Speicherkonto ab. |
> | Aktion | Microsoft.Storage/storageAccounts/regeneratekey/action | Generiert die Zugriffsschlüssel für das angegebene Speicherkonto neu. |
> | Aktion | Microsoft.Storage/storageAccounts/services/diagnosticSettings/write | Dient zum Erstellen/Aktualisieren von Speicherkonto-Diagnoseeinstellungen. |
> | Aktion | Microsoft.Storage/storageAccounts/storageAccounts/queueServices/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | Microsoft.Storage/storageAccounts/tableServices/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | Microsoft.Storage/storageAccounts/tableServices/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.Storage/storageAccounts/tableServices/providers/Microsoft.Insights/metricDefinitions/read | Hiermit wird die Liste der Microsoft Storage-Metrikdefinitionen abgerufen. |
> | Aktion | Microsoft.Storage/storageAccounts/write | Erstellt ein Speicherkonto mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Tags oder fügt eine benutzerdefinierte Domäne zum angegebenen Speicherkonto hinzu. |
> | Aktion | Microsoft.Storage/usages/read | Gibt den Grenzwert und den aktuellen Verwendungszähler für Ressourcen im angegebenen Abonnement zurück. |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | microsoft.storagesync/storageSyncServices/delete | Löscht Speichersynchronisierungsdienste. |
> | Aktion | microsoft.storagesync/storageSyncServices/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für Speichersynchronisierungsdienste ab. |
> | Aktion | microsoft.storagesync/storageSyncServices/read | Liest Speichersynchronisierungsdienste. |
> | Aktion | microsoft.storagesync/storageSyncServices/registeredServers/delete | Löscht registrierte Server. |
> | Aktion | microsoft.storagesync/storageSyncServices/registeredServers/read | Liest registrierte Server. |
> | Aktion | microsoft.storagesync/storageSyncServices/registeredServers/write | Erstellt oder aktualisiert registrierte Server. |
> | Aktion | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/delete | Löscht Cloudendpunkte. |
> | Aktion | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/operationresults/read | Standort-API für asynchrone Sicherungsaufrufe |
> | Aktion | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/postbackup/action | Ruft diese Aktion nach der Sicherung auf. |
> | Aktion | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/postrestore/action | Ruft diese Aktion nach der Wiederherstellung auf. |
> | Aktion | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/prebackup/action | Ruft diese Aktion vor der Sicherung auf. |
> | Aktion | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/prerestore/action | Ruft diese Aktion vor der Wiederherstellung auf. |
> | Aktion | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/read | Liest Cloudendpunkte. |
> | Aktion | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/restoreheartbeat/action | Stellt den Heartbeat wieder her. |
> | Aktion | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/write | Hiermit erstellen oder aktualisieren Sie Cloudendpunkte. |
> | Aktion | microsoft.storagesync/storageSyncServices/syncGroups/delete | Löscht Synchronisierungsgruppen. |
> | Aktion | microsoft.storagesync/storageSyncServices/syncGroups/read | Liest Synchronisierungsgruppen. |
> | Aktion | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/delete | Löscht Serverendpunkte. |
> | Aktion | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/read | Liest Serverendpunkte. |
> | Aktion | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/recallAction/action | Ruft diese Aktion auf, um Dateien auf einem Server abzurufen. |
> | Aktion | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/write | Hiermit erstellen oder aktualisieren Sie Serverendpunkte. |
> | Aktion | microsoft.storagesync/storageSyncServices/syncGroups/write | Hiermit erstellen oder aktualisieren Sie Synchronisierungsgruppen. |
> | Aktion | microsoft.storagesync/storageSyncServices/write | Erstellt oder aktualisiert Speichersynchronisierungsdienste. |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.StorSimple/managers/accessControlRecords/delete | Löscht die Access Control-Datensätze. |
> | Aktion | Microsoft.StorSimple/managers/accessControlRecords/read | Listet die Access Control-Datensätze auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/managers/accessControlRecords/write | Dient zum Erstellen oder Aktualisieren der Access Control-Datensätze. |
> | Aktion | Microsoft.StorSimple/managers/alerts/read | Listet die Warnungen auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/managers/bandwidthSettings/delete | Löscht vorhandene Bandbreiteneinstellungen (nur 8000er Serie). |
> | Aktion | Microsoft.StorSimple/managers/bandwidthSettings/read | Listet die Bandbreiteneinstellungen auf (nur 8000er Serie). |
> | Aktion | Microsoft.StorSimple/managers/bandwidthSettings/write | Erstellt eine neue Bandbreiteneinstellung oder aktualisiert Bandbreiteneinstellungen (nur 8000er Serie). |
> | Aktion | Microsoft.StorSimple/Managers/certificates/write | Der Vorgang „Ressourcenzertifikat aktualisieren“ aktualisiert das Zertifikat für die Ressourcen-/Tresoranmeldeinformationen. |
> | Aktion | Microsoft.StorSimple/managers/clearAlerts/action | Dient zum Deaktivieren aller Warnungen im Zusammenhang mit dem Geräte-Manager. |
> | Aktion | Microsoft.StorSimple/managers/cloudApplianceConfigurations/read | Dient zum Auflisten der vom Cloudgerät unterstützten Konfigurationen. |
> | Aktion | Microsoft.StorSimple/managers/configureDevice/action | Konfiguriert ein Gerät. |
> | Aktion | Microsoft.StorSimple/managers/delete | Löscht die Geräte-Manager. |
> | Aktion | Microsoft.StorSimple/Managers/delete | Der Vorgang „Tresor löschen“ löscht die angegebene Azure-Ressource vom Typ „Tresor“. |
> | Aktion | Microsoft.StorSimple/managers/devices/alertSettings/read | Listet die Warnungseinstellungen auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/managers/devices/alertSettings/write | Dient zum Erstellen oder Aktualisieren der Warnungseinstellungen. |
> | Aktion | Microsoft.StorSimple/managers/devices/backupPolicies/backup/action | Dient zum Erstellen einer manuellen Sicherung, um eine bedarfsgesteuerte Sicherung aller durch die Richtlinie geschützten Volumes zu erstellen. |
> | Aktion | Microsoft.StorSimple/managers/devices/backupPolicies/delete | Löscht vorhandene Sicherungsrichtlinien (nur 8000er Serie). |
> | Aktion | Microsoft.StorSimple/managers/devices/backupPolicies/read | Dient zum Auflisten der Sicherungsrichtlinien (nur 8000er Serie). |
> | Aktion | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/delete | Löscht vorhandene Zeitpläne. |
> | Aktion | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/read | Dient zum Auflisten der Zeitpläne. |
> | Aktion | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/write | Erstellt einen neuen Zeitplan oder aktualisiert Zeitpläne. |
> | Aktion | Microsoft.StorSimple/managers/devices/backupPolicies/write | Erstellt eine neue Sicherungsrichtlinie oder aktualisiert vorhandene Sicherungsrichtlinien (nur 8000er Serie). |
> | Aktion | Microsoft.StorSimple/managers/devices/backups/delete | Löscht den Sicherungssatz. |
> | Aktion | Microsoft.StorSimple/managers/devices/backups/elements/clone/action | Dient zum Klonen einer Freigabe oder eines Volumes unter Verwendung eines Sicherungselements. |
> | Aktion | Microsoft.StorSimple/managers/devices/backups/read | Listet den Sicherungssatz auf oder ruft ihn ab. |
> | Aktion | Microsoft.StorSimple/managers/devices/backups/restore/action | Dient zum Wiederherstellen aller Volumes aus einem Sicherungssatz. |
> | Aktion | Microsoft.StorSimple/managers/devices/backupScheduleGroups/delete | Löscht die Sicherungszeitplangruppen. |
> | Aktion | Microsoft.StorSimple/managers/devices/backupScheduleGroups/read | Listet die Sicherungszeitplangruppen auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/managers/devices/backupScheduleGroups/write | Dient zum Erstellen oder Aktualisieren der Sicherungszeitplangruppen. |
> | Aktion | Microsoft.StorSimple/managers/devices/chapSettings/delete | Löscht die CHAP-Einstellungen. |
> | Aktion | Microsoft.StorSimple/managers/devices/chapSettings/read | Listet die CHAP-Einstellungen auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/managers/devices/chapSettings/write | Dient zum Erstellen oder Aktualisieren der CHAP-Einstellungen. |
> | Aktion | Microsoft.StorSimple/managers/devices/deactivate/action | Deaktiviert ein Gerät. |
> | Aktion | Microsoft.StorSimple/managers/devices/delete | Löscht die Geräte. |
> | Aktion | Microsoft.StorSimple/managers/devices/download/action | Dient zum Herunterladen von Updates für ein Gerät. |
> | Aktion | Microsoft.StorSimple/managers/devices/failover/action | Dient zum Ausführen eines Failovers für das Gerät. |
> | Aktion | Microsoft.StorSimple/managers/devices/fileservers/backup/action | Dient zum Erstellen einer Sicherung eines Dateiservers. |
> | Aktion | Microsoft.StorSimple/managers/devices/fileservers/delete | Löscht die Dateiserver. |
> | Aktion | Microsoft.StorSimple/managers/devices/fileservers/metrics/read | Listet die Metriken auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/managers/devices/fileservers/metricsDefinitions/read | Listet die Metrikdefinitionen auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/managers/devices/fileservers/read | Listet die Dateiserver auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/managers/devices/fileservers/shares/delete | Löscht die Freigaben. |
> | Aktion | Microsoft.StorSimple/managers/devices/fileservers/shares/metrics/read | Listet die Metriken auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/managers/devices/fileservers/shares/metricsDefinitions/read | Listet die Metrikdefinitionen auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/managers/devices/fileservers/shares/read | Listet die Freigaben auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/managers/devices/fileservers/shares/write | Dient zum Erstellen oder Aktualisieren der Freigaben. |
> | Aktion | Microsoft.StorSimple/managers/devices/fileservers/write | Dient zum Erstellen oder Aktualisieren der Dateiserver. |
> | Aktion | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/changeControllerPowerState/action | Dient zum Ändern des Controllerenergiezustands von Hardwarekomponentengruppen. |
> | Aktion | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/read | Dient zum Auflisten der Hardwarekomponentengruppen. |
> | Aktion | Microsoft.StorSimple/managers/devices/install/action | Dient zum Installieren von Updates auf einem Gerät. |
> | Aktion | Microsoft.StorSimple/managers/devices/installUpdates/action | Installiert Updates auf den Geräten. |
> | Aktion | Microsoft.StorSimple/managers/devices/iscsiservers/backup/action | Dient zum Erstellen einer Sicherung eines iSCSI-Servers. |
> | Aktion | Microsoft.StorSimple/managers/devices/iscsiservers/delete | Löscht die iSCSI-Server. |
> | Aktion | Microsoft.StorSimple/managers/devices/iscsiservers/disks/delete | Löscht die Datenträger. |
> | Aktion | Microsoft.StorSimple/managers/devices/iscsiservers/disks/metrics/read | Listet die Metriken auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/managers/devices/iscsiservers/disks/metricsDefinitions/read | Listet die Metrikdefinitionen auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/managers/devices/iscsiservers/disks/read | Listet die Datenträger auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/managers/devices/iscsiservers/disks/write | Dient zum Erstellen oder Aktualisieren der Datenträger. |
> | Aktion | Microsoft.StorSimple/managers/devices/iscsiservers/metrics/read | Listet die Metriken auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/managers/devices/iscsiservers/metricsDefinitions/read | Listet die Metrikdefinitionen auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/managers/devices/iscsiservers/read | Listet die iSCSI-Server auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/managers/devices/iscsiservers/write | Dient zum Erstellen oder Aktualisieren der iSCSI-Server. |
> | Aktion | Microsoft.StorSimple/managers/devices/jobs/cancel/action | Dient zum Abbrechen eines ausgeführten Auftrags. |
> | Aktion | Microsoft.StorSimple/managers/devices/jobs/read | Listet die Aufträge auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/managers/devices/listFailoverSets/action | Dient zum Auflisten der Failovergruppen für ein vorhandenes Gerät. |
> | Aktion | Microsoft.StorSimple/managers/devices/listFailoverTargets/action | Dient zum Auflisten von Failoverzielen der Geräte. |
> | Aktion | Microsoft.StorSimple/managers/devices/metrics/read | Listet die Metriken auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/managers/devices/metricsDefinitions/read | Listet die Metrikdefinitionen auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/confirmMigration/action | Bestätigt eine erfolgreiche Migration und committet sie. |
> | Aktion | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchConfirmMigrationStatus/action | Dient zum Abrufen des Migrationsbestätigungsstatus. |
> | Aktion | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchMigrationEstimate/action | Dient zum Abrufen des Status für den Migrationsschätzauftrag. |
> | Aktion | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchMigrationStatus/action | Dient zum Abrufen des Status für die Migration. |
> | Aktion | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/import/action | Dient zum Importieren von Quellkonfigurationen für die Migration. |
> | Aktion | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/startMigration/action | Dient zum Starten der Migration unter Verwendung von Quellkonfigurationen. |
> | Aktion | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/startMigrationEstimate/action | Dient zum Starten eines Auftrags zur Ermittlung der voraussichtlichen Dauer des Migrationsvorgangs. |
> | Aktion | Microsoft.StorSimple/managers/devices/networkSettings/read | Listet die Netzwerkeinstellungen auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/managers/devices/networkSettings/write | Erstellt eine neue Netzwerkeinstellung oder aktualisiert Netzwerkeinstellungen. |
> | Aktion | Microsoft.StorSimple/managers/devices/publicEncryptionKey/action | Dient zum Auflisten öffentlicher Verschlüsselungsschlüssel des Geräte-Managers. |
> | Aktion | Microsoft.StorSimple/managers/devices/publishSupportPackage/action | Dient zum Veröffentlichen des Supportpakets eines Geräts für die Problembehandlung durch den Microsoft-Support. |
> | Aktion | Microsoft.StorSimple/managers/devices/read | Listet die Geräte auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/managers/devices/scanForUpdates/action | Dient zum Suchen nach Updates auf einem Gerät. |
> | Aktion | Microsoft.StorSimple/managers/devices/securitySettings/read | Dient zum Auflisten der Sicherheitseinstellungen. |
> | Aktion | Microsoft.StorSimple/managers/devices/securitySettings/syncRemoteManagementCertificate/action | Dient zum Synchronisieren des Remoteverwaltungszertifikats für ein Gerät. |
> | Aktion | Microsoft.StorSimple/managers/devices/securitySettings/update/action | Dient zum Aktualisieren der Sicherheitseinstellungen. |
> | Aktion | Microsoft.StorSimple/managers/devices/securitySettings/write | Erstellt eine neue Sicherheitseinstellung oder aktualisiert Sicherheitseinstellungen. |
> | Aktion | Microsoft.StorSimple/managers/devices/sendTestAlertEmail/action | Dient zum Senden einer Testwarnungs-E-Mail an konfigurierte E-Mail-Empfänger. |
> | Aktion | Microsoft.StorSimple/managers/devices/timeSettings/read | Listet die Uhrzeiteinstellungen auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/managers/devices/timeSettings/write | Erstellt eine neue Uhrzeiteinstellung oder aktualisiert Uhrzeiteinstellungen. |
> | Aktion | Microsoft.StorSimple/managers/devices/updateSummary/read | Listet die Updatezusammenfassung auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/managers/devices/volumeContainers/delete | Löscht vorhandene Volumecontainer (nur 8000er Serie). |
> | Aktion | Microsoft.StorSimple/managers/devices/volumeContainers/listEncryptionKeys/action | Dient zum Auflisten von Verschlüsselungsschlüsseln von Volumecontainern. |
> | Aktion | Microsoft.StorSimple/managers/devices/volumeContainers/metrics/read | Dient zum Auflisten der Metriken. |
> | Aktion | Microsoft.StorSimple/managers/devices/volumeContainers/metricsDefinitions/read | Dient zum Auflisten der Metrikdefinitionen. |
> | Aktion | Microsoft.StorSimple/managers/devices/volumeContainers/read | Dient zum Auflisten der Volumecontainer (nur 8000er Serie). |
> | Aktion | Microsoft.StorSimple/managers/devices/volumeContainers/rolloverEncryptionKey/action | Dient zum Ausführen eines Verschlüsselungsschlüssel-Rollovers für Volumecontainer. |
> | Aktion | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/delete | Löscht vorhandene Volumes. |
> | Aktion | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/metrics/read | Dient zum Auflisten der Metriken. |
> | Aktion | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/metricsDefinitions/read | Dient zum Auflisten der Metrikdefinitionen. |
> | Aktion | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/read | Dient zum Auflisten der Volumes. |
> | Aktion | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/write | Erstellt ein neues Volume oder aktualisiert Volumes. |
> | Aktion | Microsoft.StorSimple/managers/devices/volumeContainers/write | Erstellt einen neuen Volumecontainer oder aktualisiert Volumecontainer (nur 8000er Serie). |
> | Aktion | Microsoft.StorSimple/managers/devices/write | Dient zum Erstellen oder Aktualisieren der Geräte. |
> | Aktion | Microsoft.StorSimple/managers/encryptionSettings/read | Listet die Verschlüsselungseinstellungen auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/Managers/extendedInformation/delete | Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Aktion | Microsoft.StorSimple/Managers/extendedInformation/read | Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Aktion | Microsoft.StorSimple/Managers/extendedInformation/write | Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Aktion | Microsoft.StorSimple/managers/getActivationKey/action | Dient zum Abrufen des Aktivierungsschlüssels für den Geräte-Manager. |
> | Aktion | Microsoft.StorSimple/managers/getEncryptionKey/action | Dient zum Abrufen des Verschlüsselungsschlüssels für den Geräte-Manager. |
> | Aktion | Microsoft.StorSimple/managers/listActivationKey/action | Ruft den Aktivierungsschlüssel des StorSimple-Geräte-Managers ab. |
> | Aktion | Microsoft.StorSimple/managers/listPrivateEncryptionKey/action | Ruft den privaten Verschlüsselungsschlüssel für einen StorSimple-Geräte-Manager ab. |
> | Aktion | Microsoft.StorSimple/managers/listPublicEncryptionKey/action | Dient zum Auflisten öffentlicher Verschlüsselungsschlüssel eines StorSimple-Geräte-Managers. |
> | Aktion | Microsoft.StorSimple/managers/metrics/read | Listet die Metriken auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/managers/metricsDefinitions/read | Listet die Metrikdefinitionen auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/managers/provisionCloudAppliance/action | Dient zum Erstellen eines neuen Cloudgeräts. |
> | Aktion | Microsoft.StorSimple/managers/read | Listet die Geräte-Manager auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/Managers/read | Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Aktion | Microsoft.StorSimple/managers/regenarateRegistationCertificate/action | Dient zum erneuten Generieren des Registrierungszertifikats für den Geräte-Manager. |
> | Aktion | Microsoft.StorSimple/managers/regenerateActivationKey/action | Dient zum erneuten Generieren des Aktivierungsschlüssels für den Geräte-Manager. |
> | Aktion | Microsoft.StorSimple/managers/storageAccountCredentials/delete | Löscht die Speicherkonto-Anmeldedaten. |
> | Aktion | Microsoft.StorSimple/managers/storageAccountCredentials/listAccessKey/action | Dient zum Auflisten von Zugriffsschlüsseln der Speicherkonto-Anmeldedaten. |
> | Aktion | Microsoft.StorSimple/managers/storageAccountCredentials/read | Listet die Speicherkonto-Anmeldedaten auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/managers/storageAccountCredentials/write | Dient zum Erstellen oder Aktualisieren der Speicherkonto-Anmeldedaten. |
> | Aktion | Microsoft.StorSimple/managers/storageDomains/delete | Löscht die Speicherdomänen. |
> | Aktion | Microsoft.StorSimple/managers/storageDomains/read | Listet die Speicherdomänen auf oder ruft sie ab. |
> | Aktion | Microsoft.StorSimple/managers/storageDomains/write | Dient zum Erstellen oder Aktualisieren der Speicherdomänen. |
> | Aktion | Microsoft.StorSimple/managers/write | Dient zum Erstellen oder Aktualisieren der Geräte-Manager. |
> | Aktion | Microsoft.StorSimple/Managers/write | Der Vorgang „Tresor erstellen“ erstellt eine Azure-Ressource vom Typ „Tresor“. |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.StreamAnalytics/locations/quotas/Read | Liest das Stream Analytics-Abonnementkontingent. |
> | Aktion | Microsoft.StreamAnalytics/operations/Read | Liest Stream Analytics-Vorgänge. |
> | Aktion | Microsoft.StreamAnalytics/Register/action | Registriert das Abonnement beim Stream Analytics-Ressourcenanbieter. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/Delete | Dient zum Löschen von Stream Analytics-Aufträgen. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/functions/Delete | Löscht die Stream Analytics-Auftragsfunktion. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/functions/operationresults/Read | Liest die Vorgangsergebnisse für die Stream Analytics-Auftragsfunktion. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/functions/Read | Liest die Stream Analytics-Auftragsfunktion. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/functions/RetrieveDefaultDefinition/action | Ruft die Standarddefinition einer Stream Analytics-Auftragsfunktion ab. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/functions/Test/action | Testet die Stream Analytics-Auftragsfunktion. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/functions/Write | Schreibt die Stream Analytics-Auftragsfunktion. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/inputs/Delete | Dient zum Löschen von Stream Analytics-Auftragseingaben. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/inputs/operationresults/Read | Liest die Vorgangsergebnisse für die Stream Analytics-Auftragseingabe. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/inputs/Read | Dient zum Lesen von Stream Analytics-Auftragseingaben. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/inputs/Sample/action | Stream Analytics-Auftragseingaben |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/inputs/Test/action | Dient zum Testen von Stream Analytics-Auftragseingaben. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/inputs/Write | Dient zum Schreiben von Stream Analytics-Auftragseingaben. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/metricdefinitions/Read | Dient zum Lesen von Metrikdefinitionen. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/operationresults/Read | Liest die Vorgangsergebnisse für den Stream Analytics-Auftrag. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/outputs/Delete | Dient zum Löschen von Stream Analytics-Auftragsausgaben. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/outputs/operationresults/Read | Liest die Vorgangsergebnisse für die Stream Analytics-Auftragsausgabe. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/outputs/Read | Dient zum Lesen von Stream Analytics-Auftragsausgaben. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/outputs/Test/action | Dient zum Testen der Stream Analytics-Auftragsausgabe. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/outputs/Write | Dient zum Schreiben von Stream Analytics-Auftragsausgaben. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/read | Dient zum Lesen der Diagnoseeinstellung. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/write | Dient zum Schreiben der Diagnoseeinstellung. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/logDefinitions/read | Ruft die verfügbaren Protokolle für Streamingaufträge ab. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für Streamingaufträge ab. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/Read | Dient zum Lesen von Stream Analytics-Aufträgen. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/Start/action | Dient zum Starten von Stream Analytics-Aufträgen. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/Stop/action | Dient zum Beenden von Stream Analytics-Aufträgen. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/transformations/Delete | Dient zum Löschen von Stream Analytics-Auftragstransformationen. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/transformations/Read | Dient zum Lesen von Stream Analytics-Auftragstransformationen. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/transformations/Write | Dient zum Schreiben von Stream Analytics-Auftragstransformationen. |
> | Aktion | Microsoft.StreamAnalytics/streamingjobs/Write | Dient zum Schreiben von Stream Analytics-Aufträgen. |

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Subscription/CreateSubscription/action | Erstellt ein Azure-Abonnement. |
> | Aktion | Microsoft.Subscription/SubscriptionDefinitions/read | Ruft die Definition eines Azure-Abonnements in einer Verwaltungsgruppe ab. |
> | Aktion | Microsoft.Subscription/SubscriptionDefinitions/write | Erstellt die Definition für ein Azure-Abonnement. |

## <a name="microsoftsupport"></a>Microsoft.Support

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.Support/register/action | Führt die Registrierung beim Supportressourcenanbieter durch. |
> | Aktion | Microsoft.Support/supportTickets/read | Ruft Details zu Supporttickets (einschließlich Status, Schweregrad, Kontaktdetails und Kommunikation) oder die Liste mit Supporttickets (abonnementübergreifend) ab. |
> | Aktion | Microsoft.Support/supportTickets/write | Erstellt oder aktualisiert ein Supportticket. Sie können ein Supportticket zu technischen Problemen bzw. zu Problemen im Zusammenhang mit Abrechnung, Kontingenten oder Abonnementverwaltung erstellen. Der Schweregrad, die Kontaktinformationen und die Kommunikation für vorhandene Supporttickets können aktualisiert werden. |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.TimeSeriesInsights/environments/accesspolicies/delete | Löscht die Zugriffsrichtlinie. |
> | Aktion | Microsoft.TimeSeriesInsights/environments/accesspolicies/read | Dient zum Abrufen der Eigenschaften einer Zugriffsrichtlinie. |
> | Aktion | Microsoft.TimeSeriesInsights/environments/accesspolicies/write | Erstellt eine neue Zugriffsrichtlinie für eine Umgebung oder aktualisiert eine vorhandene Zugriffsrichtlinie. |
> | Aktion | Microsoft.TimeSeriesInsights/environments/delete | Löscht die Umgebung. |
> | Aktion | Microsoft.TimeSeriesInsights/environments/eventsources/delete | Löscht die Ereignisquelle. |
> | Aktion | Microsoft.TimeSeriesInsights/environments/eventsources/eventsources/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.TimeSeriesInsights/environments/eventsources/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | Microsoft.TimeSeriesInsights/environments/eventsources/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für die Ereignisquellen ab. |
> | Aktion | Microsoft.TimeSeriesInsights/environments/eventsources/read | Dient zum Abrufen der Eigenschaften einer Ereignisquelle. |
> | Aktion | Microsoft.TimeSeriesInsights/environments/eventsources/write | Erstellt eine neue Ereignisquelle für eine Umgebung oder aktualisiert eine vorhandene Ereignisquelle. |
> | Aktion | Microsoft.TimeSeriesInsights/environments/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | Microsoft.TimeSeriesInsights/environments/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.TimeSeriesInsights/environments/providers/Microsoft.Insights/metricDefinitions/read | Ruft die verfügbaren Metriken für die Umgebungen ab. |
> | Aktion | Microsoft.TimeSeriesInsights/environments/read | Dient zum Abrufen der Eigenschaften einer Umgebung. |
> | Aktion | Microsoft.TimeSeriesInsights/environments/referencedatasets/delete | Löscht das Verweisdataset. |
> | Aktion | Microsoft.TimeSeriesInsights/environments/referencedatasets/read | Dient zum Abrufen der Eigenschaften eines Verweisdatasets. |
> | Aktion | Microsoft.TimeSeriesInsights/environments/referencedatasets/write | Erstellt ein neues Verweisdataset für eine Umgebung oder aktualisiert ein vorhandenes Verweisdataset. |
> | Aktion | Microsoft.TimeSeriesInsights/environments/status/read | Ruft den Status der Umgebung und den Status der zugehörigen Vorgänge wie eingehende Daten ab. |
> | Aktion | Microsoft.TimeSeriesInsights/environments/write | Erstellt eine neue Umgebung oder aktualisiert eine vorhandene Umgebung. |
> | Aktion | Microsoft.TimeSeriesInsights/register/action | Registriert das Abonnement für den Time Series Insights-Ressourcenanbieter und ermöglicht die Erstellung von Time Series Insights-Umgebungen. |

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.VisualStudio/Account/Delete | Löscht ein Konto. |
> | Aktion | Microsoft.VisualStudio/Account/Read | Liest ein Konto. |
> | Aktion | Microsoft.VisualStudio/Account/Write | Legt ein Konto fest. |
> | Aktion | Microsoft.VisualStudio/Extension/Delete | Löscht eine Erweiterung. |
> | Aktion | Microsoft.VisualStudio/Extension/Read | Liest eine Erweiterung. |
> | Aktion | Microsoft.VisualStudio/Extension/Write | Legt eine Erweiterung fest. |
> | Aktion | Microsoft.VisualStudio/Project/Delete | Löscht ein Projekt. |
> | Aktion | Microsoft.VisualStudio/Project/Read | Liest ein Projekt. |
> | Aktion | Microsoft.VisualStudio/Project/Write | Legt ein Projekt fest. |

## <a name="microsoftweb"></a>microsoft.web

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | microsoft.web/apimanagementaccounts/apiacls/read | Ruft die API-ACLs der API Management-Konten ab. |
> | Aktion | microsoft.web/apimanagementaccounts/apis/apiacls/delete | Löscht die API-ACLs für die APIs der API Management-Konten. |
> | Aktion | microsoft.web/apimanagementaccounts/apis/apiacls/read | Ruft die API-ACLs für die APIs der API Management-Konten ab. |
> | Aktion | microsoft.web/apimanagementaccounts/apis/apiacls/write | Aktualisiert die API-ACLs für die APIs der API Management-Konten. |
> | Aktion | microsoft.web/apimanagementaccounts/apis/connectionacls/read | Ruft die Verbindungs-ACLs für die APIs der API Management-Konten ab. |
> | Aktion | microsoft.web/apimanagementaccounts/apis/connections/confirmconsentcode/action | Bestätigt den Zustimmungscode für die API-Verbindungen der API Management-Konten. |
> | Aktion | microsoft.web/apimanagementaccounts/apis/connections/connectionacls/delete | Löscht die Verbindungs-ACLs für die API-Verbindungen der API Management-Konten. |
> | Aktion | microsoft.web/apimanagementaccounts/apis/connections/connectionacls/read | Ruft die Verbindungs-ACLs für die API-Verbindungen der API Management-Konten ab. |
> | Aktion | microsoft.web/apimanagementaccounts/apis/connections/connectionacls/write | Aktualisiert die Verbindungs-ACLs für die API-Verbindungen der API Management-Konten. |
> | Aktion | microsoft.web/apimanagementaccounts/apis/connections/delete | Löscht die API-Verbindungen der API Management-Konten. |
> | Aktion | microsoft.web/apimanagementaccounts/apis/connections/getconsentlinks/action | Ruft die Zustimmungslinks für die API-Verbindungen der API Management-Konten ab. |
> | Aktion | microsoft.web/apimanagementaccounts/apis/connections/listconnectionkeys/action | Listet die Verbindungsschlüssel für die API-Verbindungen der API Management-Konten auf. |
> | Aktion | microsoft.web/apimanagementaccounts/apis/connections/listsecrets/action | Listet die Geheimnisse für die API-Verbindungen der API Management-Konten auf. |
> | Aktion | microsoft.web/apimanagementaccounts/apis/connections/read | Ruft die API-Verbindungen der API Management-Konten ab. |
> | Aktion | microsoft.web/apimanagementaccounts/apis/connections/write | Aktualisiert die API-Verbindungen der API Management-Konten. |
> | Aktion | microsoft.web/apimanagementaccounts/apis/delete | Löscht die APIs der API Management-Konten. |
> | Aktion | microsoft.web/apimanagementaccounts/apis/localizeddefinitions/delete | Löscht die lokalisierten Definitionen für die APIs der API Management-Konten. |
> | Aktion | microsoft.web/apimanagementaccounts/apis/localizeddefinitions/read | Ruft die lokalisierten Definitionen für die APIs der API Management-Konten ab. |
> | Aktion | microsoft.web/apimanagementaccounts/apis/localizeddefinitions/write | Aktualisiert die lokalisierten Definitionen für die APIs der API Management-Konten. |
> | Aktion | microsoft.web/apimanagementaccounts/apis/read | Ruft die APIs der API Management-Konten ab. |
> | Aktion | microsoft.web/apimanagementaccounts/apis/write | Aktualisiert die APIs der API Management-Konten. |
> | Aktion | microsoft.web/apimanagementaccounts/connectionacls/read | Ruft die Verbindungs-ACLs der API Management-Konten ab. |
> | Aktion | microsoft.web/availablestacks/read | Dient zum Abrufen verfügbarer Stapel. |
> | Aktion | microsoft.web/billingmeters/read | Ruft die Liste der Verbrauchseinheiten für die Abrechnung ab. |
> | Aktion | Microsoft.Web/certificates/Delete | Dient zum Löschen eines vorhandenen Zertifikats. |
> | Aktion | Microsoft.Web/certificates/Read | Dient zum Abrufen der Zertifikatliste. |
> | Aktion | Microsoft.Web/certificates/Write | Dient zum Hinzufügen eines neuen Zertifikats oder zum Aktualisieren eines bereits vorhandenen. |
> | Aktion | microsoft.web/checknameavailability/read | Dient zum Prüfen, ob ein Ressourcenname verfügbar ist. |
> | Aktion | microsoft.web/classicmobileservices/read | Dient zum Abrufen von Mobile Services (klassisch). |
> | Aktion | Microsoft.Web/connectionGateways/Delete | Löscht ein Verbindungsgateway. |
> | Aktion | Microsoft.Web/connectionGateways/Join/Action | Verknüpft ein Verbindungsgateway. |
> | Aktion | microsoft.web/connectiongateways/liststatus/action | Listet den Status von Verbindungsgateways auf. |
> | Aktion | Microsoft.Web/connectionGateways/ListStatus/Action | Listet den Status eines Verbindungsgateways auf. |
> | Aktion | Microsoft.Web/connectionGateways/Move/Action | Verschiebt ein Verbindungsgateway. |
> | Aktion | Microsoft.Web/connectionGateways/Read | Dient zum Abrufen der Verbindungsgatewayliste. |
> | Aktion | Microsoft.Web/connectionGateways/Write | Erstellt oder aktualisiert ein Verbindungsgateway. |
> | Aktion | microsoft.web/connections/confirmconsentcode/action | Dient zum Bestätigen des Genehmigungscodes für Verbindungen. |
> | Aktion | Microsoft.Web/connections/Delete | Löscht eine Verbindung. |
> | Aktion | Microsoft.Web/connections/Join/Action | Verknüpft eine Verbindung. |
> | Aktion | microsoft.web/connections/listconsentlinks/action | Dient zum Auflisten von Genehmigungslinks für Verbindungen. |
> | Aktion | Microsoft.Web/connections/Move/Action | Verschiebt eine Verbindung. |
> | Aktion | Microsoft.Web/connections/Read | Dient zum Abrufen der Verbindungsliste. |
> | Aktion | Microsoft.Web/connections/Write | Erstellt oder aktualisiert eine Verbindung. |
> | Aktion | Microsoft.Web/customApis/Delete | Löscht eine benutzerdefinierte API. |
> | Aktion | Microsoft.Web/customApis/extractApiDefinitionFromWsdl/Action | Extrahiert eine API-Definition aus einer WSDL-Schnittstelle. |
> | Aktion | Microsoft.Web/customApis/Join/Action | Verknüpft eine benutzerdefinierte API. |
> | Aktion | Microsoft.Web/customApis/listWsdlInterfaces/Action | Listet WSDL-Schnittstellen für eine benutzerdefinierte API auf. |
> | Aktion | Microsoft.Web/customApis/Move/Action | Verschiebt eine benutzerdefinierte API |
> | Aktion | Microsoft.Web/customApis/Read | Dient zum Abrufen einer benutzerdefinierten API. |
> | Aktion | Microsoft.Web/customApis/Write | Erstellt oder aktualisiert eine Verbindungs-API. |
> | Aktion | microsoft.web/deploymentlocations/read | Dient zum Abrufen von Bereitstellungsorten. |
> | Aktion | Microsoft.Web/geoRegions/Read | Ruft die Liste mit geografischen Regionen ab. |
> | Aktion | microsoft.web/hostingenvironments/capacities/read | Dient zum Abrufen von Hostingumgebungskapazitäten. |
> | Aktion | Microsoft.Web/hostingEnvironments/Delete | Dient zum Löschen einer App Service-Umgebung. |
> | Aktion | microsoft.web/hostingenvironments/diagnostics/read | Dient zum Abrufen von Hostingumgebungsdiagnosen. |
> | Aktion | microsoft.web/hostingenvironments/inboundnetworkdependenciesendpoints/read | Ruft die Netzwerkendpunkte aller eingehenden Abhängigkeiten ab. |
> | Aktion | microsoft.web/hostingenvironments/metricdefinitions/read | Dient zum Abrufen von Metrikdefinitionen für Hostingumgebungen. |
> | Aktion | microsoft.web/hostingenvironments/multirolepools/metricdefinitions/read | Dient zum Abrufen von MultiRole-Poolmetrikdefinitionen für Hostingumgebungen. |
> | Aktion | microsoft.web/hostingenvironments/multirolepools/metrics/read | Dient zum Abrufen von MultiRole-Poolmetriken für Hostingumgebungen. |
> | Aktion | Microsoft.Web/hostingEnvironments/multiRolePools/providers/Microsoft.Insights/metricDefinitions/Read | Ruft die verfügbaren MultiRole-Metriken für die App Service-Umgebung ab. |
> | Aktion | Microsoft.Web/hostingEnvironments/multiRolePools/Read | Dient zum Abrufen der Eigenschaften eines FrontEnd-Pools in einer App Service-Umgebung. |
> | Aktion | microsoft.web/hostingenvironments/multirolepools/skus/read | Dient zum Abrufen von MultiRole-Pool-SKUs für Hostingumgebungen. |
> | Aktion | microsoft.web/hostingenvironments/multirolepools/usages/read | Dient zum Abrufen von MultiRole-Poolverwendungen für Hostingumgebungen. |
> | Aktion | Microsoft.Web/hostingEnvironments/multiRolePools/Write | Dient zum Erstellen eines neuen FrontEnd-Pools in einer App Service-Umgebung oder zum Aktualisieren eines bereits vorhandenen. |
> | Aktion | microsoft.web/hostingenvironments/operations/read | Dient zum Abrufen von Hostingumgebungsvorgängen. |
> | Aktion | microsoft.web/hostingenvironments/outboundnetworkdependenciesendpoints/read | Ruft die Netzwerkendpunkte aller ausgehenden Abhängigkeiten ab. |
> | Aktion | microsoft.web/hostingenvironments/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | microsoft.web/hostingenvironments/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.Web/hostingEnvironments/Read | Dient zum Abrufen der Eigenschaften einer App Service-Umgebung. |
> | Aktion | Microsoft.Web/hostingEnvironments/reboot/Action | Dient zum Neustarten aller Computer in einer App Service-Umgebung. |
> | Aktion | microsoft.web/hostingenvironments/resume/action | Dient zum Fortsetzen von Hostingumgebungen. |
> | Aktion | microsoft.web/hostingenvironments/serverfarms/read | Dient zum Abrufen von App Service-Plänen für Hostingumgebungen. |
> | Aktion | microsoft.web/hostingenvironments/sites/read | Dient zum Abrufen von Web-Apps für Hostingumgebungen. |
> | Aktion | microsoft.web/hostingenvironments/suspend/action | Dient zum Anhalten von Hostingumgebungen. |
> | Aktion | microsoft.web/hostingenvironments/usages/read | Dient zum Abrufen von Hostingumgebungsverwendungen. |
> | Aktion | microsoft.web/hostingenvironments/workerpools/metricdefinitions/read | Dient zum Abrufen von Workerpool-Metrikdefinitionen für Hostingumgebungen. |
> | Aktion | microsoft.web/hostingenvironments/workerpools/metrics/read | Dient zum Abrufen von Workerpoolmetriken für Hostingumgebungen. |
> | Aktion | Microsoft.Web/hostingEnvironments/workerPools/providers/Microsoft.Insights/metricDefinitions/Read | Ruft die verfügbaren WorkerPool-Metriken für die App Service-Umgebung ab. |
> | Aktion | Microsoft.Web/hostingEnvironments/workerPools/Read | Dient zum Abrufen der Eigenschaften eines Workerpools in einer App Service-Umgebung. |
> | Aktion | microsoft.web/hostingenvironments/workerpools/skus/read | Dient zum Abrufen von Workerpool-SKUs für Hostingumgebungen. |
> | Aktion | microsoft.web/hostingenvironments/workerpools/usages/read | Dient zum Abrufen von Workerpoolverwendungen für Hostingumgebungen. |
> | Aktion | Microsoft.Web/hostingEnvironments/workerPools/Write | Dient zum Erstellen eines neuen Workerpools in einer App Service-Umgebung oder zum Aktualisieren eines bereits vorhandenen. |
> | Aktion | Microsoft.Web/hostingEnvironments/Write | Dient zum Erstellen einer neuen App Service-Umgebung oder zum Aktualisieren einer bereits vorhandenen. |
> | Aktion | microsoft.web/ishostingenvironmentnameavailable/read | Dient zum Abrufen, ob der Hostingumgebungsname verfügbar ist. |
> | Aktion | microsoft.web/ishostnameavailable/read | Dient zum Prüfen, ob der Hostname verfügbar ist. |
> | Aktion | microsoft.web/isusernameavailable/read | Dient zum Prüfen, ob der Benutzername verfügbar ist. |
> | Aktion | Microsoft.Web/listSitesAssignedToHostName/Read | Dient zum Abrufen der Namen von Websites, die dem Hostnamen zugewiesen sind. |
> | Aktion | microsoft.web/locations/apioperations/read | Dient zum Abrufen von Standorten für API-Vorgänge. |
> | Aktion | microsoft.web/locations/connectiongatewayinstallations/read | Dient zum Abrufen von Standorten für Verbindungsgatewayinstallationen. |
> | Aktion | microsoft.web/locations/extractapidefinitionfromwsdl/action | Extrahiert eine API-Definition aus einer WSDL-Schnittstelle für Speicherorte. |
> | Aktion | microsoft.web/locations/listwsdlinterfaces/action | Listet WSDL-Schnittstellen für Speicherorte auf. |
> | Aktion | microsoft.web/locations/managedapis/apioperations/read | Dient zum Abrufen verwalteter API-Vorgänge. |
> | Aktion | Microsoft.Web/locations/managedapis/Join/Action | Verknüpft eine verwaltete API. |
> | Aktion | microsoft.web/locations/managedapis/read | Dient zum Abrufen verwalteter APIs für Standorte. |
> | Aktion | microsoft.web/operations/read | Dient zum Abrufen von Vorgängen. |
> | Aktion | microsoft.web/publishingusers/read | Dient zum Abrufen von Veröffentlichungsbenutzern. |
> | Aktion | microsoft.web/publishingusers/write | Dient zum Aktualisieren von Veröffentlichungsbenutzern. |
> | Aktion | Microsoft.Web/recommendations/Read | Dient zum Abrufen der Liste mit Empfehlungen für Abonnements. |
> | Aktion | microsoft.web/register/action | Dient zum Registrieren des Microsoft.Web-Ressourcenanbieters für das Abonnement. |
> | Aktion | microsoft.web/resourcehealthmetadata/read | Ruft Metadaten zur Ressourcenintegrität ab. |
> | Aktion | microsoft.web/serverfarms/capabilities/read | Dient zum Abrufen von Funktionen für App Service-Pläne. |
> | Aktion | Microsoft.Web/serverfarms/Delete | Löschen eines vorhandenen App Service-Plans |
> | Aktion | microsoft.web/serverfarms/firstpartyapps/settings/delete | Dient zum Löschen von Erstanbieter-App-Einstellungen für App Service-Pläne. |
> | Aktion | microsoft.web/serverfarms/firstpartyapps/settings/read | Dient zum Abrufen von Erstanbieter-App-Einstellungen für App Service-Pläne. |
> | Aktion | microsoft.web/serverfarms/firstpartyapps/settings/write | Dient zum Aktualisieren von Erstanbieter-App-Einstellungen für App Service-Pläne. |
> | Aktion | microsoft.web/serverfarms/hybridconnectionnamespaces/relays/delete | Dient zum Löschen von Relaynamespace-Agents für Hybridverbindungen für App Service-Pläne. |
> | Aktion | microsoft.web/serverfarms/hybridconnectionnamespaces/relays/read | Dient zum Abrufen von Relaynamespace-Agents für Hybridverbindungen für App Service-Pläne. |
> | Aktion | microsoft.web/serverfarms/hybridconnectionnamespaces/relays/sites/read | Dient zum Abrufen von Hybridverbindungsnamespace-Relay-Web-Apps für App Service-Pläne. |
> | Aktion | microsoft.web/serverfarms/hybridconnectionplanlimits/read | Dient zum Abrufen von Hybridverbindungsplan-Grenzwerten für App Service-Pläne. |
> | Aktion | microsoft.web/serverfarms/hybridconnectionrelays/read | Dient zum Abrufen von Hybridverbindungsrelays für App Service-Pläne. |
> | Aktion | microsoft.web/serverfarms/metricdefinitions/read | Dient zum Abrufen von Metrikdefinitionen für App Service-Pläne. |
> | Aktion | microsoft.web/serverfarms/metrics/read | Dient zum Abrufen von Metriken für App Service-Pläne. |
> | Aktion | microsoft.web/serverfarms/operationresults/read | Dient zum Abrufen von Vorgangsergebnissen für App Service-Pläne. |
> | Aktion | microsoft.web/serverfarms/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | microsoft.web/serverfarms/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.Web/serverfarms/providers/Microsoft.Insights/metricDefinitions/Read | Ruft die verfügbaren Metriken für einen App Service-Plan ab. |
> | Aktion | Microsoft.Web/serverfarms/Read | Dient zum Abrufen der Eigenschaften für einen App Service-Plan. |
> | Aktion | Microsoft.Web/serverfarms/restartSites/Action | Dient zum Neustarten aller Web-Apps in einem App Service-Plan. |
> | Aktion | microsoft.web/serverfarms/sites/read | Dient zum Abrufen von Web-Apps für App Service-Pläne. |
> | Aktion | microsoft.web/serverfarms/skus/read | Dient zum Abrufen von SKUs für App Service-Pläne. |
> | Aktion | microsoft.web/serverfarms/usages/read | Dient zum Abrufen von Verwendungen für App Service-Pläne. |
> | Aktion | microsoft.web/serverfarms/virtualnetworkconnections/gateways/write | Dient zum Aktualisieren von Gateways für Verbindungen mit virtuellen Netzwerken für App Service-Pläne. |
> | Aktion | microsoft.web/serverfarms/virtualnetworkconnections/read | Dient zum Abrufen von Verbindungen mit virtuellen Netzwerken für App Service-Pläne. |
> | Aktion | microsoft.web/serverfarms/virtualnetworkconnections/routes/delete | Dient zum Löschen von Routen für Verbindungen mit virtuellen Netzwerken für App Service-Pläne. |
> | Aktion | microsoft.web/serverfarms/virtualnetworkconnections/routes/read | Dient zum Abrufen von Routen für Verbindungen mit virtuellen Netzwerken für App Service-Pläne. |
> | Aktion | microsoft.web/serverfarms/virtualnetworkconnections/routes/write | Dient zum Aktualisieren von Routen für Verbindungen mit virtuellen Netzwerken für App Service-Pläne. |
> | Aktion | microsoft.web/serverfarms/workers/reboot/action | Dient zum Neustarten von Workern für App Service-Pläne. |
> | Aktion | Microsoft.Web/serverfarms/Write | Dient zum Erstellen eines neuen App Service-Plans oder zum Aktualisieren eines bereits vorhandenen. |
> | Aktion | microsoft.web/sites/analyzecustomhostname/read | Dient zum Analysieren des benutzerdefinierten Hostnamens. |
> | Aktion | Microsoft.Web/sites/applySlotConfig/Action | Dient zum Anwenden der Web-App-Slotkonfiguration aus dem Zielslot auf die aktuelle Web-App. |
> | Aktion | Microsoft.Web/sites/backup/Action | Dient zum Erstellen einer neuen Web-App-Sicherung. |
> | Aktion | microsoft.web/sites/backup/read | Dient zum Abrufen der Web-App-Sicherung. |
> | Aktion | microsoft.web/sites/backup/write | Dient zum Aktualisieren der Web-App-Sicherung. |
> | Aktion | microsoft.web/sites/backups/delete | Dient zum Löschen von Web-App-Sicherungen. |
> | Aktion | microsoft.web/sites/backups/list/action | Dient zum Auflisten von Web-App-Sicherungen. |
> | Aktion | Microsoft.Web/sites/backups/Read | Dient zum Abrufen der Eigenschaften einer Web-App-Sicherung. |
> | Aktion | microsoft.web/sites/backups/restore/action | Dient zum Wiederherstellen von Web-App-Sicherungen. |
> | Aktion | microsoft.web/sites/config/delete | Dient zum Löschen der Web-App-Konfiguration. |
> | Aktion | Microsoft.Web/sites/config/list/Action | Dient zum Auflisten der sicherheitsrelevanten Einstellungen der Web-App. Hierzu zählen etwa Veröffentlichungsanmeldeinformationen, App-Einstellungen und Verbindungszeichenfolgen. |
> | Aktion | Microsoft.Web/sites/config/Read | Dient zum Abrufen von Web-App-Konfigurationseinstellungen. |
> | Aktion | Microsoft.Web/sites/config/Write | Dient zum Aktualisieren der Konfigurationseinstellungen der Web-App. |
> | Aktion | microsoft.web/sites/continuouswebjobs/delete | Dient zum Löschen fortlaufender Webaufträge für Web-Apps. |
> | Aktion | microsoft.web/sites/continuouswebjobs/read | Dient zum Abrufen fortlaufender Webaufträge für Web-Apps. |
> | Aktion | microsoft.web/sites/continuouswebjobs/start/action | Dient zum Starten fortlaufender Webaufträge für Web-Apps. |
> | Aktion | microsoft.web/sites/continuouswebjobs/stop/action | Dient zum Beenden fortlaufender Webaufträge für Web-Apps. |
> | Aktion | Microsoft.Web/sites/Delete | Dient zum Löschen einer vorhandenen Web-App. |
> | Aktion | microsoft.web/sites/deployments/delete | Dient zum Löschen von Web-App-Bereitstellungen. |
> | Aktion | microsoft.web/sites/deployments/log/read | Dient zum Abrufen des Web-App-Bereitstellungsprotokolls. |
> | Aktion | microsoft.web/sites/deployments/read | Dient zum Abrufen von Web-App-Bereitstellungen. |
> | Aktion | microsoft.web/sites/deployments/write | Dient zum Aktualisieren von Web-App-Bereitstellungen. |
> | Aktion | microsoft.web/sites/diagnostics/analyses/execute/Action | Dient zum Ausführen der Analyse von Web-Apps-Diagnosen. |
> | Aktion | microsoft.web/sites/diagnostics/analyses/read | Dient zum Abrufen der Analyse von Web-Apps-Diagnosen. |
> | Aktion | microsoft.web/sites/diagnostics/aspnetcore/read | Ruft Web-Apps-Diagnosen für die ASP.NET Core-App ab. |
> | Aktion | microsoft.web/sites/diagnostics/autoheal/read | Dient zum Abrufen von AutoHeal für Web-Apps-Diagnosen. |
> | Aktion | microsoft.web/sites/diagnostics/deployment/read | Dient zum Abrufen einer Bereitstellung für Web-Apps-Diagnosen. |
> | Aktion | microsoft.web/sites/diagnostics/deployments/read | Dient zum Abrufen von Bereitstellungen für Web-Apps-Diagnosen. |
> | Aktion | microsoft.web/sites/diagnostics/detectors/execute/Action | Dient zum Ausführen der Erkennung von Web-Apps-Diagnosen. |
> | Aktion | microsoft.web/sites/diagnostics/detectors/read | Dient zum Abrufen der Erkennung von Web-Apps-Diagnosen. |
> | Aktion | microsoft.web/sites/diagnostics/failedrequestsperuri/read | Dient zum Abrufen fehlerhafter Anforderungen bei Web-Apps-Diagnosen pro URI. |
> | Aktion | microsoft.web/sites/diagnostics/frebanalysis/read | Dient zum Abrufen der FREB-Analyse von Web-App-Diagnosen. |
> | Aktion | microsoft.web/sites/diagnostics/loganalyzer/read | Dient zum Abrufen der Protokollanalyse von Web-Apps-Diagnosen. |
> | Aktion | microsoft.web/sites/diagnostics/read | Dient zum Abrufen der Kategorien von Web-Apps-Diagnosen. |
> | Aktion | microsoft.web/sites/diagnostics/runtimeavailability/read | Dient zum Abrufen der Laufzeitverfügbarkeit für Web-App-Diagnosen. |
> | Aktion | microsoft.web/sites/diagnostics/servicehealth/read | Dient zum Abrufen der Dienstintegrität von Web-App-Diagnosen. |
> | Aktion | microsoft.web/sites/diagnostics/sitecpuanalysis/read | Dient zum Abrufen der CPU-Analyse von Websites von Web-Apps-Diagnosen. |
> | Aktion | microsoft.web/sites/diagnostics/sitecrashes/read | Dient zum Abrufen von Websiteabstürzen von Web-Apps-Diagnosen. |
> | Aktion | microsoft.web/sites/diagnostics/sitelatency/read | Dient zum Abrufen von Websitelatenzen von Web-Apps-Diagnosen. |
> | Aktion | microsoft.web/sites/diagnostics/sitememoryanalysis/read | Dient zum Abrufen der Websitespeicheranalyse von Web-Apps-Diagnosen. |
> | Aktion | microsoft.web/sites/diagnostics/siterestartsettingupdate/read | Dient zum Abrufen des Websiteupdates für die Einstellungen zum Neustarten der Web-Apps-Diagnosen. |
> | Aktion | microsoft.web/sites/diagnostics/siterestartuserinitiated/read | Dient zum Abrufen des vom Benutzer initiierten Websiteneustarts der Web-Apps-Diagnosen. |
> | Aktion | microsoft.web/sites/diagnostics/siteswap/read | Dient zum Abrufen des Websiteaustauschs von Web-Apps-Diagnosen. |
> | Aktion | microsoft.web/sites/diagnostics/threadcount/read | Dient zum Abrufen der Threadanzahl von Web-Apps-Diagnosen. |
> | Aktion | microsoft.web/sites/diagnostics/workeravailability/read | Dient zum Abrufen der Workerverfügbarkeit für Web-App-Diagnosen. |
> | Aktion | microsoft.web/sites/diagnostics/workerprocessrecycle/read | Dient zum Abrufen der Workerprozesswiederverwendung für Web-App-Diagnosen. |
> | Aktion | microsoft.web/sites/domainownershipidentifiers/read | Dient zum Abrufen von Domänenbesitz-IDs für Web-Apps. |
> | Aktion | microsoft.web/sites/domainownershipidentifiers/write | Dient zum Aktualisieren von Domänenbesitz-IDs für Web-Apps. |
> | Aktion | microsoft.web/sites/functions/action | Functions-Web-Apps |
> | Aktion | microsoft.web/sites/functions/delete | Dient zum Löschen von Web-App-Funktionen. |
> | Aktion | microsoft.web/sites/functions/listsecrets/action | Dient zum Auflisten der Geheimnisse für Web-App-Funktionen. |
> | Aktion | microsoft.web/sites/functions/masterkey/read | Dient zum Abrufen des Hauptschlüssels von Web-Apps-Funktionen. |
> | Aktion | microsoft.web/sites/functions/read | Dient zum Abrufen von Web-App-Funktionen. |
> | Aktion | microsoft.web/sites/functions/token/read | Dient zum Abrufen des Funktionstokens von Web-Apps. |
> | Aktion | microsoft.web/sites/functions/write | Dient zum Aktualisieren von Web-App-Funktionen. |
> | Aktion | microsoft.web/sites/hostnamebindings/delete | Dient zum Löschen von Hostnamenbindungen für Web-Apps. |
> | Aktion | microsoft.web/sites/hostnamebindings/read | Dient zum Abrufen von Hostnamenbindungen für Web-Apps. |
> | Aktion | microsoft.web/sites/hostnamebindings/write | Dient zum Aktualisieren von Hostnamenbindungen für Web-Apps. |
> | Aktion | microsoft.web/sites/hybridconnection/delete | Dient zum Löschen der Hybridverbindung für Web-Apps. |
> | Aktion | microsoft.web/sites/hybridconnection/read | Dient zum Abrufen der Hybridverbindung für Web-Apps. |
> | Aktion | microsoft.web/sites/hybridconnection/write | Dient zum Aktualisieren der Hybridverbindung für Web-Apps. |
> | Aktion | microsoft.web/sites/hybridconnectionnamespaces/relays/delete | Dient zum Löschen der Relaynamespace-Agents für Hybridverbindungen für Web-Apps. |
> | Aktion | microsoft.web/sites/hybridconnectionnamespaces/relays/listkeys/action | Dient zum Auflisten der Schlüssel von Relaynamespace-Agents für Hybridverbindungen für Web-Apps. |
> | Aktion | microsoft.web/sites/hybridconnectionnamespaces/relays/read | Dient zum Abrufen von Relaynamespace-Agents für Hybridverbindungen für Web-Apps. |
> | Aktion | microsoft.web/sites/hybridconnectionnamespaces/relays/write | Dient zum Aktualisieren des Relaynamespace-Agents für Hybridverbindungen für Web-Apps. |
> | Aktion | microsoft.web/sites/hybridconnectionrelays/read | Dient zum Abrufen von Hybridverbindungsrelays für Web-Apps. |
> | Aktion | microsoft.web/sites/instances/deployments/delete | Dient zum Löschen von Web-Apps-Instanzbereitstellungen. |
> | Aktion | microsoft.web/sites/instances/deployments/read | Dient zum Abrufen von Prozessen für Web-App-Bereitstellungen. |
> | Aktion | microsoft.web/sites/instances/extensions/log/read | Dient zum Abrufen des Web-App-Instanzerweiterungsprotokolls. |
> | Aktion | microsoft.web/sites/instances/extensions/read | Dient zum Abrufen der Erweiterungen von Web-App-Instanzen. |
> | Aktion | microsoft.web/sites/instances/processes/delete | Dient zum Löschen von Prozessen für Web-App-Instanzen. |
> | Aktion | microsoft.web/sites/instances/processes/read | Dient zum Abrufen von Prozessen für Web-App-Instanzen. |
> | Aktion | microsoft.web/sites/instances/read | Dient zum Abrufen von Web-App-Instanzen. |
> | Aktion | microsoft.web/sites/listsyncfunctiontriggerstatus/action | Web-Apps zum Auflisten des Triggerstatus der Synchronisierungsfunktion |
> | Aktion | microsoft.web/sites/metricdefinitions/read | Dient zum Abrufen von Metrikdefinitionen für Web-Apps. |
> | Aktion | microsoft.web/sites/metrics/read | Dient zum Abrufen von Web-App-Metriken. |
> | Aktion | microsoft.web/sites/metricsdefinitions/read | Dient zum Abrufen von Metrikdefinitionen für Web-Apps. |
> | Aktion | microsoft.web/sites/migratemysql/action | Dient zum Durchführen von Migrationen mithilfe von MySql und Web-Apps. |
> | Aktion | microsoft.web/sites/migratemysql/read | Dient zum Abrufen von Web-Apps für die Migration mit MySql. |
> | Aktion | microsoft.web/sites/networktrace/action | Netzwerkablaufverfolgungs-Web-Apps |
> | Aktion | microsoft.web/sites/newpassword/action | NewPassword-Web-Apps |
> | Aktion | microsoft.web/sites/operationresults/read | Dient zum Abrufen der Ergebnisse von Web-App-Vorgängen. |
> | Aktion | microsoft.web/sites/operations/read | Dient zum Abrufen von Web-Apps-Vorgängen. |
> | Aktion | microsoft.web/sites/perfcounters/read | Dient zum Abrufen von Leistungsindikatoren für Web-Apps. |
> | Aktion | microsoft.web/sites/premieraddons/delete | Dient zum Löschen von Premier-Add-Ons für Web-Apps. |
> | Aktion | microsoft.web/sites/premieraddons/read | Dient zum Abrufen von Premier-Add-Ons für Web-Apps. |
> | Aktion | microsoft.web/sites/premieraddons/write | Dient zum Aktualisieren von Premier-Add-Ons für Web-Apps. |
> | Aktion | microsoft.web/sites/processes/read | Dient zum Abrufen von Web-Apps-Prozessen. |
> | Aktion | microsoft.web/sites/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | microsoft.web/sites/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.Web/sites/providers/Microsoft.Insights/metricDefinitions/Read | Ruft die verfügbaren Metriken für die Web-App ab. |
> | Aktion | microsoft.web/sites/publiccertificates/delete | Löscht die öffentlichen Zertifikate für Web-Apps. |
> | Aktion | microsoft.web/sites/publiccertificates/read | Ruft die öffentlichen Zertifikate für Web-Apps ab. |
> | Aktion | microsoft.web/sites/publiccertificates/write | Aktualisiert die öffentlichen Zertifikate für Web-Apps. |
> | Aktion | Microsoft.Web/sites/publish/Action | Dient zum Veröffentlichen einer Web-App. |
> | Aktion | Microsoft.Web/sites/publishxml/Action | Dient zum Abrufen des Veröffentlichungsprofils (XML) für eine Web-App. |
> | Aktion | microsoft.web/sites/publishxml/read | Dient zum Abrufen des XML-Codes für die Veröffentlichung von Web-Apps. |
> | Aktion | Microsoft.Web/sites/Read | Dient zum Abrufen der Eigenschaften einer Web-App. |
> | Aktion | microsoft.web/sites/recommendationhistory/read | Dient zum Abrufen des Empfehlungsverlaufs für Web-Apps. |
> | Aktion | microsoft.web/sites/recommendations/disable/action | Dient zum Deaktivieren von Web-App-Empfehlungen. |
> | Aktion | Microsoft.Web/sites/recommendations/Read | Dient zum Abrufen der Liste mit Empfehlungen für Web-Apps. |
> | Aktion | microsoft.web/sites/recover/action | Stellt Web-Apps wieder her. |
> | Aktion | Microsoft.Web/sites/resetSlotConfig/Action | Dient zum Zurücksetzen der Web-App-Konfiguration. |
> | Aktion | microsoft.web/sites/resourcehealthmetadata/read | Ruft Metadaten zur Ressourcenintegrität von Web-Apps ab. |
> | Aktion | Microsoft.Web/sites/restart/Action | Dient zum Neustarten einer Web-App. |
> | Aktion | microsoft.web/sites/restore/read | Dient zum Abrufen der Web-App-Wiederherstellung. |
> | Aktion | microsoft.web/sites/restore/write | Dient zum Wiederherstellen von Web-Apps. |
> | Aktion | microsoft.web/sites/restoresnapshot/action | Stellt Momentaufnahmen von Web-Apps wieder her. |
> | Aktion | microsoft.web/sites/siteextensions/delete | Dient zum Löschen von Web-Apps-Websiteerweiterungen. |
> | Aktion | microsoft.web/sites/siteextensions/read | Dient zum Abrufen von Web-Apps-Websiteerweiterungen. |
> | Aktion | microsoft.web/sites/siteextensions/write | Dient zum Aktualisieren von Web-Apps-Websiteerweiterungen. |
> | Aktion | microsoft.web/sites/slots/analyzecustomhostname/read | Dient zum Abrufen des benutzerdefinierten Hostnamens für die Analyse von Web-Apps-Slots. |
> | Aktion | Microsoft.Web/sites/slots/applySlotConfig/Action | Dient zum Anwenden der Web-App-Slotkonfiguration aus dem Zielslot auf den aktuellen Slot. |
> | Aktion | Microsoft.Web/sites/slots/backup/Action | Dient zum Erstellen einer neuen Web-App-Slotsicherung. |
> | Aktion | microsoft.web/sites/slots/backup/read | Dient zum Abrufen von Sicherungen von Web-Apps-Slots. |
> | Aktion | microsoft.web/sites/slots/backup/write | Dient zum Aktualisieren der Web-App-Slotsicherung. |
> | Aktion | microsoft.web/sites/slots/backups/delete | Dient zum Löschen von Sicherungen von Web-Apps-Slots. |
> | Aktion | microsoft.web/sites/slots/backups/list/action | Dient zum Auflisten von Web-App-Slotsicherungen. |
> | Aktion | Microsoft.Web/sites/slots/backups/Read | Dient zum Abrufen der Eigenschaften einer Sicherung von Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/backups/restore/action | Dient zum Wiederherstellen von Web-App-Slotsicherungen. |
> | Aktion | microsoft.web/sites/slots/config/delete | Dient zum Löschen der Konfiguration von Web-App-Slots. |
> | Aktion | Microsoft.Web/sites/slots/config/list/Action | Dient zum Auflisten der sicherheitsrelevanten Einstellungen des Web-App-Slots. Hierzu zählen etwa Veröffentlichungsanmeldeinformationen, App-Einstellungen und Verbindungszeichenfolgen. |
> | Aktion | Microsoft.Web/sites/slots/config/Read | Dient zum Abrufen der Konfigurationseinstellungen des Web-App-Slots. |
> | Aktion | Microsoft.Web/sites/slots/config/Write | Dient zum Aktualisieren der Konfigurationseinstellungen des Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/continuouswebjobs/delete | Dient zum Löschen fortlaufender Webaufträge für Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/continuouswebjobs/read | Dient zum Abrufen fortlaufender Webaufträge für Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/continuouswebjobs/start/action | Dient zum Starten fortlaufender Webaufträge für Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/continuouswebjobs/stop/action | Dient zum Beenden fortlaufender Webaufträge für Web-App-Slots. |
> | Aktion | Microsoft.Web/sites/slots/Delete | Dient zum Löschen eines vorhandenen Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/deployments/delete | Dient zum Löschen von Web-App-Slotbereitstellungen. |
> | Aktion | microsoft.web/sites/slots/deployments/log/read | Dient zum Abrufen des Protokolls für Web-App-Slotbereitstellungen. |
> | Aktion | microsoft.web/sites/slots/deployments/read | Dient zum Abrufen von Web-App-Slotbereitstellungen. |
> | Aktion | microsoft.web/sites/slots/deployments/write | Dient zum Aktualisieren von Web-App-Slotbereitstellungen. |
> | Aktion | microsoft.web/sites/slots/diagnostics/analyses/execute/Action | Dient zum Ausführen der Analyse von Web-Apps-Slotdiagnosen. |
> | Aktion | microsoft.web/sites/slots/diagnostics/analyses/read | Dient zum Abrufen der Analyse von Web-Apps-Slotdiagnosen. |
> | Aktion | microsoft.web/sites/slots/diagnostics/aspnetcore/read | Ruft Web-Apps-Slotdiagnosen für die ASP.NET Core-App ab. |
> | Aktion | microsoft.web/sites/slots/diagnostics/autoheal/read | Dient zum Abrufen von AutoHeal für Web-Apps-Slotdiagnosen. |
> | Aktion | microsoft.web/sites/slots/diagnostics/deployment/read | Dient zum Abrufen einer Bereitstellung für Web-Apps-Slotdiagnosen. |
> | Aktion | microsoft.web/sites/slots/diagnostics/deployments/read | Dient zum Abrufen von Bereitstellungen für Web-Apps-Slotdiagnosen. |
> | Aktion | microsoft.web/sites/slots/diagnostics/detectors/execute/Action | Dient zum Ausführen der Erkennung von Web-Apps-Slotdiagnosen. |
> | Aktion | microsoft.web/sites/slots/diagnostics/detectors/read | Dient zum Abrufen der Erkennung von Web-Apps-Slotdiagnosen. |
> | Aktion | microsoft.web/sites/slots/diagnostics/frebanalysis/read | Dient zum Abrufen der FREB-Analyse von Web-Apps-Slotdiagnosen. |
> | Aktion | microsoft.web/sites/slots/diagnostics/loganalyzer/read | Dient zum Abrufen der Protokollanalyse von Web-Apps-Slotdiagnosen. |
> | Aktion | microsoft.web/sites/slots/diagnostics/read | Dient zum Abrufen der Web-Apps-Slotdiagnose. |
> | Aktion | microsoft.web/sites/slots/diagnostics/runtimeavailability/read | Dient zum Abrufen der Laufzeitverfügbarkeit für Web-Apps-Diagnosen. |
> | Aktion | microsoft.web/sites/slots/diagnostics/servicehealth/read | Dient zum Abrufen der Dienstintegrität von Web-Apps-Slotdiagnosen. |
> | Aktion | microsoft.web/sites/slots/diagnostics/sitecpuanalysis/read | Dient zum Abrufen der CPU-Analyse von Websites von Web-Apps-Slotdiagnosen. |
> | Aktion | microsoft.web/sites/slots/diagnostics/sitecrashes/read | Dient zum Abrufen der Siteabstürze von Web-Apps-Slotdiagnosen. |
> | Aktion | microsoft.web/sites/slots/diagnostics/sitelatency/read | Dient zum Abrufen der Websitelatenzen von Web-Apps-Slotdiagnosen. |
> | Aktion | microsoft.web/sites/slots/diagnostics/sitememoryanalysis/read | Dient zum Abrufen der Websitespeicheranalyse von Web-Apps-Slotdiagnosen. |
> | Aktion | microsoft.web/sites/slots/diagnostics/siterestartsettingupdate/read | Dient zum Abrufen des Websiteupdates für die Einstellungen zum Neustarten der Web-Apps-Slotdiagnosen. |
> | Aktion | microsoft.web/sites/slots/diagnostics/siterestartuserinitiated/read | Dient zum Abrufen des vom Benutzer initiierten Websiteneustarts der Web-Apps-Slotdiagnosen. |
> | Aktion | microsoft.web/sites/slots/diagnostics/siteswap/read | Dient zum Abrufen des Websiteaustauschs von Web-Apps-Slotdiagnosen. |
> | Aktion | microsoft.web/sites/slots/diagnostics/threadcount/read | Dient zum Abrufen der Threadanzahl von Web-Apps-Slotdiagnosen. |
> | Aktion | microsoft.web/sites/slots/diagnostics/workeravailability/read | Dient zum Abrufen der Workerverfügbarkeit für Web-App-Slotdiagnosen. |
> | Aktion | microsoft.web/sites/slots/diagnostics/workerprocessrecycle/read | Dient zum Abrufen der Workerprozesswiederverwendung für Web-Apps-Slotdiagnosen. |
> | Aktion | microsoft.web/sites/slots/domainownershipidentifiers/read | Dient zum Abrufen von Domänenbesitz-IDs für Web-Apps-Slots. |
> | Aktion | microsoft.web/sites/slots/hostnamebindings/delete | Dient zum Löschen von Hostnamenbindungen für Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/hostnamebindings/read | Dient zum Abrufen von Hostnamenbindungen für Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/hostnamebindings/write | Dient zum Aktualisieren von Hostnamenbindungen für Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/hybridconnection/delete | Dient zum Löschen der Hybridverbindung für Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/hybridconnection/read | Dient zum Abrufen der Hybridverbindung für Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/hybridconnection/write | Dient zum Aktualisieren der Hybridverbindung für Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/hybridconnectionnamespaces/relays/delete | Dient zum Löschen der Relaynamespace-Agents für Hybridverbindungen für Web-Apps-Slots. |
> | Aktion | microsoft.web/sites/slots/hybridconnectionnamespaces/relays/write | Dient zum Aktualisieren der Relaynamespace-Agents für Hybridverbindungen für Web-Apps-Slots. |
> | Aktion | microsoft.web/sites/slots/hybridconnectionrelays/read | Dient zum Abrufen der Relay-Agents für Hybridverbindungen für Web-Apps-Slots. |
> | Aktion | microsoft.web/sites/slots/instances/deployments/read | Dient zum Abrufen von Bereitstellungen für Web-App-Slotinstanzen. |
> | Aktion | microsoft.web/sites/slots/instances/processes/delete | Dient zum Löschen von Prozessen für Web-Apps-Slotinstanzen. |
> | Aktion | microsoft.web/sites/slots/instances/processes/read | Dient zum Abrufen von Prozessen für Web-App-Slotinstanzen. |
> | Aktion | microsoft.web/sites/slots/instances/read | Dient zum Abrufen von Web-App-Slotinstanzen. |
> | Aktion | microsoft.web/sites/slots/metricdefinitions/read | Dient zum Abrufen von Metrikdefinitionen für Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/metrics/read | Dient zum Abrufen von Metriken für Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/migratemysql/read | Dient zum Abrufen von Web-Apps-Slots für die Migration mit MySql. |
> | Aktion | microsoft.web/sites/slots/networktrace/action | Web-Apps-Slots für die Netzwerkablaufverfolgung |
> | Aktion | microsoft.web/sites/slots/newpassword/action | NewPassword-Web-App-Slots |
> | Aktion | microsoft.web/sites/slots/operationresults/read | Dient zum Abrufen der Ergebnisse von Web-App-Slotvorgängen. |
> | Aktion | microsoft.web/sites/slots/operations/read | Dient zum Abrufen von Web-Apps-Slotvorgängen. |
> | Aktion | microsoft.web/sites/slots/perfcounters/read | Dient zum Abrufen von Leistungsindikatoren für Web-Apps-Slots. |
> | Aktion | microsoft.web/sites/slots/phplogging/read | Dient zum Abrufen der PHP-Protokollierung für Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/premieraddons/delete | Dient zum Löschen von Premier-Add-Ons für Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/premieraddons/read | Dient zum Abrufen von Premier-Add-Ons für Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/premieraddons/write | Dient zum Aktualisieren von Premier-Add-Ons für Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/providers/Microsoft.Insights/diagnosticSettings/read | Ruft die Diagnoseeinstellung für die Ressource ab. |
> | Aktion | microsoft.web/sites/slots/providers/Microsoft.Insights/diagnosticSettings/write | Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource. |
> | Aktion | Microsoft.Web/sites/slots/providers/Microsoft.Insights/metricDefinitions/Read | Ruft die verfügbaren Metriken für Web-Apps-Slots ab. |
> | Aktion | microsoft.web/sites/slots/publiccertificates/read | Ruft die öffentlichen Zertifikate für Web-Apps-Slots ab. |
> | Aktion | microsoft.web/sites/slots/publiccertificates/write | Erstellt oder aktualisiert die öffentlichen Zertifikate für Web-Apps-Slots. |
> | Aktion | Microsoft.Web/sites/slots/publish/Action | Dient zum Veröffentlichen eines Web-App-Slots. |
> | Aktion | Microsoft.Web/sites/slots/publishxml/Action | Dient zum Abrufen des Veröffentlichungsprofils (XML) für einen Web-App-Slot. |
> | Aktion | Microsoft.Web/sites/slots/Read | Dient zum Abrufen der Eigenschaften eines Web-App-Bereitstellungsslots. |
> | Aktion | microsoft.web/sites/slots/recover/action | Stellt Web-App-Slots wieder her. |
> | Aktion | Microsoft.Web/sites/slots/resetSlotConfig/Action | Dient zum Zurücksetzen der Web-App-Slotkonfiguration. |
> | Aktion | microsoft.web/sites/slots/resourcehealthmetadata/read | Ruft Metadaten zur Ressourcenintegrität von Web-Apps-Slots ab. |
> | Aktion | Microsoft.Web/sites/slots/restart/Action | Dient zum Neustarten eines Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/restore/read | Dient zum Abrufen der Wiederherstellung für Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/restore/write | Dient zum Wiederherstellen von Web-Apps-Slots. |
> | Aktion | microsoft.web/sites/slots/restoresnapshot/action | Stellt Momentaufnahme von Web-App-Slots wieder her. |
> | Aktion | microsoft.web/sites/slots/siteextensions/delete | Dient zum Löschen von Websiteerweiterungen von Web-Apps-Slots. |
> | Aktion | microsoft.web/sites/slots/siteextensions/read | Dient zum Abrufen von Websiteerweiterungen von Web-Apps-Slots. |
> | Aktion | microsoft.web/sites/slots/siteextensions/write | Dient zum Aktualisieren von Websiteerweiterungen von Web-Apps-Slots. |
> | Aktion | Microsoft.Web/sites/slots/slotsdiffs/Action | Dient zum Abrufen von Konfigurationsunterschieden zwischen Web-App und Slots. |
> | Aktion | Microsoft.Web/sites/slots/slotsswap/Action | Dient zum Austauschen von Web-App-Bereitstellungsslots. |
> | Aktion | microsoft.web/sites/slots/snapshots/read | Dient zum Abrufen von Web-Apps-Slotmomentaufnahmen. |
> | Aktion | Microsoft.Web/sites/slots/sourcecontrols/Delete | Dient zum Löschen der Konfigurationseinstellungen für die Quellcodeverwaltung des Web-App-Slots. |
> | Aktion | Microsoft.Web/sites/slots/sourcecontrols/Read | Dient zum Abrufen der Konfigurationseinstellungen für die Quellcodeverwaltung des Web-App-Slots. |
> | Aktion | Microsoft.Web/sites/slots/sourcecontrols/Write | Dient zum Aktualisieren der Konfigurationseinstellungen für die Quellcodeverwaltung des Web-App-Slots. |
> | Aktion | Microsoft.Web/sites/slots/start/Action | Dient zum Starten eines Web-App-Slots. |
> | Aktion | Microsoft.Web/sites/slots/stop/Action | Dient zum Beenden eines Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/sync/action | Dient zum Synchronisieren von Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/triggeredwebjobs/delete | Dient zum Löschen ausgelöster Webaufträge für Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/triggeredwebjobs/read | Dient zum Abrufen ausgelöster Webaufträge für Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/triggeredwebjobs/run/action | Dient zum Ausführen ausgelöster Webaufträge für Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/usages/read | Dient zum Abrufen von Verwendungen für Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/virtualnetworkconnections/delete | Dient zum Löschen von Verbindungen mit virtuellen Netzwerken für Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/virtualnetworkconnections/gateways/write | Dient zum Aktualisieren von Gateways für Verbindungen mit virtuellen Netzwerken für Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/virtualnetworkconnections/read | Dient zum Abrufen von Verbindungen mit virtuellen Netzwerken für Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/virtualnetworkconnections/write | Dient zum Aktualisieren von Verbindungen mit virtuellen Netzwerken für Web-App-Slots. |
> | Aktion | microsoft.web/sites/slots/webjobs/read | Dient zum Abrufen von Webaufträgen für Web-App-Slots. |
> | Aktion | Microsoft.Web/sites/slots/Write | Dient zum Erstellen eines neuen Web-App-Slots oder zum Aktualisieren eines bereits vorhandenen. |
> | Aktion | Microsoft.Web/sites/slotsdiffs/Action | Dient zum Abrufen von Konfigurationsunterschieden zwischen Web-App und Slots. |
> | Aktion | Microsoft.Web/sites/slotsswap/Action | Dient zum Austauschen von Web-App-Bereitstellungsslots. |
> | Aktion | microsoft.web/sites/snapshots/read | Dient zum Abrufen von Web-App-Momentaufnahmen. |
> | Aktion | Microsoft.Web/sites/sourcecontrols/Delete | Dient zum Löschen der Konfigurationseinstellungen für die Quellcodeverwaltung der Web-App. |
> | Aktion | Microsoft.Web/sites/sourcecontrols/Read | Dient zum Abrufen der Konfigurationseinstellungen für die Quellcodeverwaltung der Web-App. |
> | Aktion | Microsoft.Web/sites/sourcecontrols/Write | Dient zum Aktualisieren der Konfigurationseinstellungen für die Quellcodeverwaltung der Web-App. |
> | Aktion | Microsoft.Web/sites/start/Action | Dient zum Starten einer Web-App. |
> | Aktion | Microsoft.Web/sites/stop/Action | Dient zum Beenden einer Web-App. |
> | Aktion | microsoft.web/sites/sync/action | Synchronisierungs-Web-Apps |
> | Aktion | microsoft.web/sites/syncfunctiontriggers/action | Synchronisierungsfunktionstrigger für Web-Apps. |
> | Aktion | microsoft.web/sites/triggeredwebjobs/delete | Dient zum Löschen ausgelöster Webaufträge für Web-Apps. |
> | Aktion | microsoft.web/sites/triggeredwebjobs/history/read | Dient zum Abrufen des ausgelösten WebJobs-Verlaufs für Web-Apps. |
> | Aktion | microsoft.web/sites/triggeredwebjobs/read | Dient zum Abrufen ausgelöster Webaufträge für Web-Apps. |
> | Aktion | microsoft.web/sites/triggeredwebjobs/run/action | Dient zum Ausführen ausgelöster Webaufträge für Web-Apps. |
> | Aktion | microsoft.web/sites/usages/read | Dient zum Abrufen von Web-App-Verwendungen. |
> | Aktion | microsoft.web/sites/virtualnetworkconnections/delete | Dient zum Löschen von Verbindungen mit virtuellen Netzwerken für Web-Apps. |
> | Aktion | microsoft.web/sites/virtualnetworkconnections/gateways/read | Dient zum Abrufen von Gateways für Verbindungen mit virtuellen Netzwerken für Web-Apps. |
> | Aktion | microsoft.web/sites/virtualnetworkconnections/gateways/write | Dient zum Aktualisieren von Gateways für Verbindungen mit virtuellen Netzwerken für Web-Apps. |
> | Aktion | microsoft.web/sites/virtualnetworkconnections/read | Dient zum Abrufen von Verbindungen mit virtuellen Netzwerken für Web-Apps. |
> | Aktion | microsoft.web/sites/virtualnetworkconnections/write | Dient zum Aktualisieren von Verbindungen mit virtuellen Netzwerken für Web-Apps. |
> | Aktion | microsoft.web/sites/webjobs/read | Dient zum Abrufen von Webaufträgen für Web-Apps. |
> | Aktion | Microsoft.Web/sites/Write | Dient zum Erstellen einer neuen Web-App oder zum Aktualisieren einer bereits vorhandenen. |
> | Aktion | microsoft.web/skus/read | Dient zum Abrufen von SKUs. |
> | Aktion | microsoft.web/sourcecontrols/read | Dient zum Abrufen von Quellcodeverwaltungen. |
> | Aktion | microsoft.web/sourcecontrols/write | Dient zum Aktualisieren von Quellcodeverwaltungen. |
> | Aktion | microsoft.web/unregister/action | Dient zum Aufheben der Registrierung des Microsoft.Web-Ressourcenanbieters für das Abonnement. |
> | Aktion | microsoft.web/validate/action | Dient zur Überprüfung. |
> | Aktion | microsoft.web/verifyhostingenvironmentvnet/action | Überprüft das VNET der Hostingumgebung. |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tdCol2BreakAll"]
> | Aktionstyp | Vorgang | Beschreibung |
> | --- | --- | --- |
> | Aktion | Microsoft.WorkloadMonitor/components/read | Liest Betriebsressourcen. |
> | Aktion | Microsoft.WorkloadMonitor/healthInstances/read | Liest Betriebsressourcen. |
> | Aktion | Microsoft.WorkloadMonitor/Operations/read | Liest Betriebsressourcen. |
> | Aktion | Microsoft.WorkloadMonitor/workloads/delete | Löscht eine Workloadressource. |
> | Aktion | Microsoft.WorkloadMonitor/workloads/read | Liest eine Workloadressource. |
> | Aktion | Microsoft.WorkloadMonitor/workloads/write | Schreibt eine Workloadressource. |

## <a name="next-steps"></a>Nächste Schritte

- [Benutzerdefinierte Rollen](custom-roles.md)
- [Integrierte Rollen](built-in-roles.md)
