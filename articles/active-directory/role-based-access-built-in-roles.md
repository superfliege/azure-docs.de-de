---
title: Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure (RBAC) | Microsoft-Dokumentation
description: Dieses Thema beschreibt die integrierten Rollen der rollenbasierten Zugriffssteuerung (RBAC). Da die Rollen kontinuierlich hinzugefügt werden, ist es ratsam, die Dokumentation häufiger auf Aktualisierungen zu prüfen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 03/06/2018
ms.author: rolyon
ms.reviewer: rqureshi
ms.custom: it-pro
ms.openlocfilehash: 0e8f1d97f51fbfedbca1619ef5e7f28a3a0570b9
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="built-in-roles-for-azure-role-based-access-control"></a>Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure
Die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) von Azure umfasst die folgenden integrierten Rollen, die Benutzern, Gruppen und Diensten zugewiesen werden können. Die Definitionen integrierter Rollen können nicht geändert werden. Sie können jedoch [benutzerdefinierte Rollen in Azure RBAC](role-based-access-control-custom-roles.md) zur Anpassung an die spezifischen Anforderungen Ihrer Organisation erstellen.

## <a name="built-in-role-descriptions"></a>Beschreibungen der integrierten Rollen
Die folgende Tabelle enthält kurze Beschreibungen der integrierten Rollen. Klicken Sie auf den Rollennamen, um eine ausführliche Liste der **actions** und **notactions** für die Rolle anzuzeigen. Die **Aktions** -Eigenschaft gibt die zulässigen Aktionen für Azure-Ressourcen an. Für Aktionszeichenfolgen dürfen Platzhalter verwendet werden. Die **notactions** -Eigenschaft gibt die Aktionen an, die von den zulässigen Aktionen ausgeschlossen sind.

Die Aktion definiert, welche Art von Vorgängen für einen bestimmten Ressourcentyp ausgeführt werden können. Beispiel: 
- **Schreiben** ermöglicht Ihnen das Ausführen von PUT-, POST-, PATCH- und DELETE-Vorgängen.
- **Lesen** ermöglicht Ihnen das Ausführen von GET-Vorgängen.

In diesem Artikel werden nur die heute vorhandenen verschiedenen Rollen behandelt. Wenn Sie einem Benutzer eine Rolle zuweisen, können Sie jedoch die zulässigen Aktionen weiter einschränken, indem Sie einen Gültigkeitsbereich definieren. Dies ist hilfreich, wenn Sie einem Benutzer die Rolle „Mitwirkender von Website“ zuweisen möchten, jedoch nur für eine Ressourcengruppe.

> [!NOTE]
> Die Definitionen von Azure-Rollen werden ständig weiterentwickelt. Dieser Artikel wird so aktuell wie möglich gehalten. Die aktuellsten Definitionen von Rollen finden Sie jedoch immer in Azure PowerShell. Verwenden Sie das Cmdlet [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition), um alle aktuellen Rollen aufzulisten. Mithilfe von `(get-azurermroledefinition "<role name>").actions` oder `(get-azurermroledefinition "<role name>").notactions` können Sie in eine bestimmte Rolle weiter eintauchen. Verwenden Sie [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation), um die Vorgänge von bestimmten Azure-Ressourcenanbietern aufzulisten.


| Integrierte Rolle | BESCHREIBUNG |
| --- | --- |
| [Besitzer](#owner) | Kann alles verwalten, einschließlich des Zugriffs |
| [Mitwirkender](#contributor) | Kann alles außer den Zugriff verwalten |
| [Leser](#reader) | Kann alles anzeigen, jedoch keine Änderungen vornehmen |
| [Mitwirkender des API-Verwaltungsdienstes](#api-management-service-contributor) | Kann API-Verwaltungsdienste verwalten |
| [Operatorrolle des API Management-Diensts](#api-management-service-operator-role) | Kann API-Verwaltungsdienste verwalten |
| [Leserrolle des API Management-Diensts](#api-management-service-reader-role) | Kann API-Verwaltungsdienste verwalten |
| [Mitwirkender der Application Insights-Komponente](#application-insights-component-contributor) | Kann Application Insights-Komponenten verwalten |
| [Application Insights-Momentaufnahmedebugger](#application-insights-snapshot-debugger) | Erteilt dem Benutzer die Berechtigung zum Verwenden von Features des Momentaufnahmedebuggers in Application Insights. |
| [Automation-Auftragsoperator](#automation-job-operator) | Hiermit werden Aufträge mithilfe von Automation-Runbooks erstellt und verwaltet. |
| [Operator für Automation](#automation-operator) | Kann Aufträge starten, unterbrechen und fortsetzen |
| [Automation-Runbookoperator](#automation-runbook-operator) | Runbookeigenschaften lesen: Ermöglicht das Erstellen von Runbookaufträgen. |
| [Besitzer der Azure Stack-Registrierung](#azure-stack-registration-owner) | Ermöglicht Ihnen die Verwaltung von Azure Stack-Registrierungen. |
| [Mitwirkender für Sicherungen](#backup-contributor) | Kann alle Aktionen für die Sicherungsverwaltung durchführen, außer dem Erstellen eines Recovery Services-Tresors und dem Erteilen von Zugriff für andere |
| [Sicherungsoperator](#backup-operator) | Kann alle Aktionen für die Sicherungsverwaltung durchführen, außer dem Erstellen von Tresoren, dem Entfernen von Sicherungen und dem Erteilen von Zugriff für andere |
| [Sicherungsleser](#backup-reader) | Kann die Sicherungsverwaltung im Recovery Services-Tresor überwachen |
| [Abrechnungsleser](#billing-reader) | Kann sämtliche Abrechnungsinformationen anzeigen |
| [Mitwirkender von BizTalk](#biztalk-contributor) | Kann BizTalk-Dienste verwalten |
| [Mitwirkender für den CDN-Endpunkt](#cdn-endpoint-contributor) | Kann CDN-Endpunkte verwalten, aber anderen Benutzern keinen Zugriff erteilen. |
| [CDN-Endpunktleser](#cdn-endpoint-reader) | Kann CDN-Endpunkte anzeigen, aber keine Änderungen vornehmen. |
| [Mitwirkender für das CDN-Profil](#cdn-profile-contributor) | Kann CDN-Profile und deren Endpunkte verwalten, aber anderen Benutzern keinen Zugriff erteilen. |
| [CDN-Profilleser](#cdn-profile-reader) | Kann CDN-Profile und deren Endpunkte anzeigen, aber keine Änderungen vornehmen. |
| [Mitwirkender von klassischem Netzwerk](#classic-network-contributor) | Kann klassische virtuelle Netzwerke und reservierte IP-Adressen verwalten |
| [Mitwirkender von klassischem Speicherkonto](#classic-storage-account-contributor) | Kann klassische Speicherkonten verwalten |
| [Klassische Dienstrolle „Speicherkonto-Schlüsseloperator“](#classic-storage-account-key-operator-service-role) | Klassische Speicherkonto-Schlüsseloperatoren dürfen Schlüssel für klassische Speicherkonten auflisten und neu generieren. |
| [Mitwirkender von klassischen virtuellen Computern](#classic-virtual-machine-contributor) | Kann klassische virtuelle Computer verwalten, jedoch nicht das virtuelle Netzwerk oder Speicherkonto, mit dem sie verbunden sind |
| [Mitwirkender von ClearDB-MySQL-DB](#cleardb-mysql-db-contributor) | Kann ClearDB MySQL-Datenbanken verwalten |
| [Cosmos DB-Rolle „Kontoleser“](#cosmos-db-account-reader-role) | Kann Azure Cosmos DB-Kontodaten lesen. Informationen zum Verwalten von Azure Cosmos DB-Konten finden Sie unter [Mitwirkender von DocumentDB-Konto](#documentdb-account-contributor). |
| [Mitwirkender von Data Factory](#data-factory-contributor) | Erstellt und verwaltet Data Factorys und darin enthaltene untergeordnete Ressourcen. |
| [Data Lake Analytics-Entwickler](#data-lake-analytics-developer) | Ermöglicht Ihnen das Übermitteln, Überwachen und Verwalten Ihrer eigenen Aufträge, aber nicht das Erstellen oder Löschen von Data Lake Analytics-Konten. |
| [DevTest Labs-Benutzer](#devtest-labs-user) | Kann alles anzeigen sowie virtuelle Maschinen verbinden, starten, neu starten und herunterfahren |
| [DNS Zone Contributor](#dns-zone-contributor) | Kann DNS-Zonen und -Einträge verwalten. |
| [Mitwirkender von DocumentDB-Konto](#documentdb-account-contributor) | Kann Azure Cosmos DB-Konten verwalten. Azure Cosmos DB wurde früher als DocumentDB bezeichnet. |
| [Mitwirkender von Intelligent Systems-Konto](#intelligent-systems-account-contributor) | Kann Intelligent Systems-Konten verwalten |
| [Key Vault-Mitwirkender](#key-vault-contributor) | Ermöglicht Ihnen die Verwaltung von Schlüsseltresoren, aber nicht den Zugriff darauf. |
| [Lab-Ersteller](#lab-creator) | Ermöglicht Ihnen das Erstellen, Verwalten und Löschen verwalteter Labs unter Ihren Azure Lab-Konten. |
| [Log Analytics-Mitwirkender](#log-analytics-contributor) | Ein Log Analytics-Mitwirkender kann alle Überwachungsdaten lesen und Überwachungseinstellungen bearbeiten. Das Bearbeiten von Überwachungseinstellungen schließt folgende Aufgaben ein: Hinzufügen der VM-Erweiterung zu VMs, Lesen von Speicherkontoschlüsseln zum Konfigurieren von Protokollsammlungen aus Azure Storage, Erstellen und Konfigurieren von Automation-Konten, Hinzufügen von Lösungen, Konfigurieren der Azure-Diagnose für alle Azure-Ressourcen. |
| [Log Analytics-Leser](#log-analytics-reader) | Ein Log Analytics-Leser kann alle Überwachungsdaten anzeigen und durchsuchen sowie Überwachungseinstellungen anzeigen. Hierzu zählt auch die Anzeige der Konfiguration von Azure-Diagnosen für alle Azure-Ressourcen. |
| [Logik-App-Mitwirkender](#logic-app-contributor) | Ermöglicht Ihnen das Verwalten von Logik-Apps, aber nicht den Zugriff darauf. |
| [Logik-App-Operator](#logic-app-operator) | Ermöglicht Ihnen das Lesen, Aktivieren und Deaktivieren von Logik-Apps. |
| [Mitwirkender für verwaltete Identität](#managed-identity-contributor) | Dem Benutzer zugewiesene Identität erstellen, lesen, aktualisieren und löschen. |
| [Operator für verwaltete Identität](#managed-identity-operator) | Dem Benutzer zugewiesene Identität lesen und zuweisen. |
| [Überwachungsmitwirkender](#monitoring-contributor) | Kann sämtliche Überwachungsdaten lesen und Überwachungseinstellungen bearbeiten. Siehe auch [Erste Schritte mit Rollen, Berechtigungen und Sicherheit in Azure Monitor](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles). |
| [Überwachungsleser](#monitoring-reader) | Kann alle Überwachungsdaten (Metriken, Protokolle usw.) lesen. Siehe auch [Erste Schritte mit Rollen, Berechtigungen und Sicherheit in Azure Monitor](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles). |
| [Mitwirkender von virtuellem Netzwerk](#network-contributor) | Kann alle Netzwerkressourcen verwalten |
| [Mitwirkender von New Relic APM-Konto](#new-relic-apm-account-contributor) | Ermöglicht Ihnen das Verwalten von New Relic Application Performance Management-Konten und -Anwendungen, nicht aber den Zugriff auf diese. |
| [Mitwirkender von Redis-Cache](#redis-cache-contributor) | Kann Redis-Caches verwalten |
| [Mitwirkender von Zeitplanungsauftragssammlung](#scheduler-job-collections-contributor) | Kann Zeitplanungsauftragssammlungen verwalten |
| [Mitwirkender von Suchdienst](#search-service-contributor) | Kann Suchdienste verwalten |
| [Sicherheitsadministrator](#security-admin) | Nur in Security Center: Kann Sicherheitsrichtlinien und -zustände anzeigen, Sicherheitsrichtlinien bearbeiten sowie Warnungen und Empfehlungen anzeigen und verwerfen |
| [Sicherheits-Manager](#security-manager) | Kann Sicherheitskomponenten, Sicherheitsrichtlinien und virtuelle Maschinen verwalten |
| [Benutzer mit Leseberechtigung für Sicherheitsfunktionen](#security-reader) | Nur in Security Center: Kann Empfehlungen und Warnungen sowie Sicherheitsrichtlinien und -zustände anzeigen, aber keine Änderungen vornehmen |
| [Site Recovery-Mitwirkender](#site-recovery-contributor) | Kann alle Site Recovery-Verwaltungsaktionen verwalten, mit Ausnahme der Erstellung des Recovery Services-Tresors und der Zuweisung von Zugriffsrechten für andere Benutzer |
| [Site Recovery-Operator](#site-recovery-operator) | Kann Failover und Failbacks durchführen, aber keine anderen Site Recovery-Verwaltungsaktionen, oder den Zugriff für andere Benutzer zuweisen |
| [Site Recovery-Leser](#site-recovery-reader) | Kann den Site Recovery-Status im Recovery Services-Tresor überwachen und Supporttickets ausstellen |
| [Mitwirkender von SQL DB](#sql-db-contributor) | Kann SQL-Datenbanken verwalten, jedoch nicht die zugehörigen sicherheitsbezogenen Richtlinien |
| [SQL-Sicherheits-Manager](#sql-security-manager) | Kann die sicherheitsbezogenen Richtlinien von SQL-Server-Instanzen und SQL-Datenbanken verwalten |
| [Mitwirkender von SQL Server](#sql-server-contributor) | Kann SQL-Server und SQL-Datenbanken verwalten, jedoch nicht die zugehörigen sicherheitsbezogenen Richtlinien |
| [Mitwirkender von Speicherkonto](#storage-account-contributor) | Kann Speicherkonten verwalten, aber nicht den Zugriff darauf |
| [Dienstrolle „Speicherkonto-Schlüsseloperator“](#storage-account-key-operator-service-role) | Speicherkonto-Schlüsseloperatoren dürfen Schlüssel für Speicherkonten auflisten und neu generieren. |
| [Mitwirkender für Supportanfragen](#support-request-contributor) | Kann Supporttickets im Abonnementbereich erstellen und verwalten |
| [Traffic Manager-Mitwirkender](#traffic-manager-contributor) | Ermöglicht Ihnen die Verwaltung von Traffic Manager-Profilen, aber nicht die Steuerung des Zugriffs darauf. |
| [Benutzerzugriffsadministrator](#user-access-administrator) | Kann den Benutzerzugriff auf Azure-Ressourcen verwalten |
| [VM-Administratoranmeldung](#virtual-machine-administrator-login) | Benutzer mit dieser Rolle haben die Möglichkeit, sich bei einem virtuellen Computer mit Windows-Administrator- oder Linux-Root-Benutzerrechten anzumelden.
 |
| [Mitwirkender von virtuellen Computern](#virtual-machine-contributor) | Kann virtuelle Computer verwalten, jedoch nicht das virtuelle Netzwerk oder Speicherkonto, mit dem sie verbunden sind |
| [VM-Benutzeranmeldung](#virtual-machine-user-login) | Benutzer mit dieser Rolle haben die Möglichkeit, sich als normaler Benutzer an einem virtuellen Computer anzumelden. |
| [Mitwirkender von Webplan](#web-plan-contributor) | Kann Webpläne verwalten |
| [Mitwirkender von Website](#website-contributor) | Kann Websites verwalten, jedoch nicht die Webpläne, mit denen sie verbunden sind |

Die folgenden Tabellen beschreiben die spezifischen Berechtigungen der einzelnen Rollen. Dazu gehören **Actions**, die Berechtigungen erteilen, und **NotActions**, die sie einschränken.

## <a name="owner"></a>Owner (Besitzer)
Kann alles verwalten, einschließlich des Zugriffs

| **Aktionen** |  |
| --- | --- |
| * | Erstellen und Verwalten von Ressourcen aller Typen |

## <a name="contributor"></a>Mitwirkender
Kann alles außer den Zugriff verwalten

| **Aktionen** |  |
| --- | --- |
| * | Erstellen und Verwalten von Ressourcen aller Typen |

| **NotActions** |  |
| --- | --- |
| Microsoft.Authorization/*/Delete | Rollen und Rollenzuweisungen können nicht gelöscht werden. |
| Microsoft.Authorization/*/Write | Rollen und Rollenzuweisungen können nicht erstellt werden. |
| Microsoft.Authorization/elevateAccess/Action | Gewährt dem Aufrufer Zugriff vom Typ „Benutzerzugriffsadministrator“ auf der Mandantenebene. |

## <a name="reader"></a>Leser
Kann alles anzeigen, jedoch keine Änderungen vornehmen

| **Aktionen** |  |
| --- | --- |
| */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |

## <a name="api-management-service-contributor"></a>Mitwirkender des API-Verwaltungsdienstes
Kann API-Verwaltungsdienste verwalten

| **Aktionen** |  |
| --- | --- |
| Microsoft.ApiManagement/service/* | Erstellen und Verwalten des API Management-Diensts |
| Microsoft.Authorization/*/read | Lesen von Autorisierungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="api-management-service-operator-role"></a>Operatorrolle des API Management-Diensts
Kann API-Verwaltungsdienste verwalten

| **Aktionen** |  |
| --- | --- |
| Microsoft.ApiManagement/service/*/read | Dient zum Lesen von API Management-Dienstinstanzen. |
| Microsoft.ApiManagement/service/backup/action | Dient zum Sichern des API Management-Diensts im angegebenen Container in einem vom Benutzer bereitgestellten Speicherkonto. |
| Microsoft.ApiManagement/service/delete | Löschen einer API Management-Dienstinstanz |
| Microsoft.ApiManagement/service/managedeployments/action | Dient zum Ändern der SKU/Einheiten sowie zum Hinzufügen oder Entfernen regionaler Bereitstellungen des API Management-Diensts. |
| Microsoft.ApiManagement/service/read | Dient zum Lesen der Metadaten für eine API Management-Dienstinstanz. |
| Microsoft.ApiManagement/service/restore/action | Dient zum Wiederherstellen des API Management-Diensts aus dem angegebenen Container in einem vom Benutzer bereitgestellten Speicherkonto. |
| Microsoft.ApiManagement/service/updatecertificate/action | Dient zum Hochladen eines SSL-Zertifikats für einen API Management-Dienst. |
| Microsoft.ApiManagement/service/updatehostname/action | Dient zum Einrichten, Aktualisieren oder Entfernen benutzerdefinierter Domänennamen für einen API Management-Dienst. |
| Microsoft.ApiManagement/service/write | Dient zum Erstellen einer neuen Instanz des API Management-Diensts. |
| Microsoft.Authorization/*/read | Lesen von Autorisierungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

| **NotActions** |  |
| --- | --- |
| Microsoft.ApiManagement/service/users/keys/read | Dient zum Abrufen einer Liste mit Benutzerschlüsseln. |

## <a name="api-management-service-reader-role"></a>Leserrolle des API Management-Diensts
Kann API-Verwaltungsdienste verwalten

| **Aktionen** |  |
| --- | --- |
| Microsoft.ApiManagement/service/*/read | Dient zum Lesen von API Management-Dienstinstanzen. |
| Microsoft.ApiManagement/service/read | Dient zum Lesen der Metadaten für eine API Management-Dienstinstanz. |
| Microsoft.Authorization/*/read | Lesen von Autorisierungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

| **NotActions** |  |
| --- | --- |
| Microsoft.ApiManagement/service/users/keys/read | Dient zum Abrufen einer Liste mit Benutzerschlüsseln. |

## <a name="application-insights-component-contributor"></a>Mitwirkender der Application Insights-Komponente
Kann Application Insights-Komponenten verwalten

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.Insights/components/* | Erstellen und Verwalten von Insights-Komponenten |
| Microsoft.Insights/webtests/* | Erstellen und Verwalten von Webtests |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="application-insights-snapshot-debugger"></a>Application Insights-Momentaufnahmedebugger
Erteilt dem Benutzer die Berechtigung zum Verwenden von Features des Momentaufnahmedebuggers in Application Insights.

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.Insights/components/*/read |  |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="automation-job-operator"></a>Automation-Auftragsoperator
Hiermit werden Aufträge mithilfe von Automation-Runbooks erstellt und verwaltet.

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Automation/automationAccounts/jobs/read | Ruft einen Azure Automation-Auftrag ab. |
| Microsoft.Automation/automationAccounts/jobs/resume/action | Setzt einen Azure Automation-Auftrag fort. |
| Microsoft.Automation/automationAccounts/jobs/stop/action | Beendet einen Azure Automation-Auftrag. |
| Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Liest Hybrid Runbook Worker-Ressourcen. |
| Microsoft.Automation/automationAccounts/jobs/streams/read | Ruft einen Azure Automation-Auftragsdatenstrom ab. |
| Microsoft.Automation/automationAccounts/jobs/suspend/action | Hält einen Azure Automation-Auftrag an. |
| Microsoft.Automation/automationAccounts/jobs/write | Erstellt einen Azure Automation-Auftrag. |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="automation-operator"></a>Operator für Automation
Kann Aufträge starten, unterbrechen und fortsetzen

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Automation/automationAccounts/jobs/read | Lesen von Aufträgen für Automation-Konten |
| Microsoft.Automation/automationAccounts/jobs/resume/action | Fortsetzen eines Auftrags für Automation-Konten |
| Microsoft.Automation/automationAccounts/jobs/stop/action | Beenden eines Auftrags für Automation-Konten |
| Microsoft.Automation/automationAccounts/jobs/streams/read | Lesen von Auftragsdatenströmen für Automation-Konten |
| Microsoft.Automation/automationAccounts/jobs/suspend/action | Unterbrechen eines Auftrags für Automation-Konten |
| Microsoft.Automation/automationAccounts/jobs/write | Schreiben von Aufträgen für Automation-Konten |
| Microsoft.Automation/automationAccounts/jobSchedules/read | Lesen eines Auftragszeitplans für Automation-Konten |
| Microsoft.Automation/automationAccounts/jobSchedules/write | Lesen eines Auftragszeitplans für Automation-Konten |
| Microsoft.Automation/automationAccounts/linkedWorkspace/read | Ruft den Arbeitsbereich ab, der mit dem Automatisierungskonto verknüpft ist. |
| Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Liest Hybrid Runbook Worker-Ressourcen. |
| Microsoft.Automation/automationAccounts/read | Lesen von Automation-Konten |
| Microsoft.Automation/automationAccounts/runbooks/read | Lesen von Automation-Runbooks |
| Microsoft.Automation/automationAccounts/schedules/read | Lesen von Zeitplänen für Automation-Konten |
| Microsoft.Automation/automationAccounts/schedules/write | Schreiben von Zeitplänen für Automation-Konten |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="automation-runbook-operator"></a>Automation-Runbookoperator
Runbookeigenschaften lesen: Ermöglicht das Erstellen von Runbookaufträgen.

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Automation/automationAccounts/runbooks/read | Ruft ein Azure Automation-Runbook ab. |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="azure-stack-registration-owner"></a>Besitzer der Azure Stack-Registrierung
Ermöglicht Ihnen die Verwaltung von Azure Stack-Registrierungen.

| **Aktionen** |  |
| --- | --- |
| Microsoft.AzureStack/registrations/products/listDetails/action | Ruft erweiterte Details für ein Azure Stack-Marketplace-Produkt ab. |
| Microsoft.AzureStack/registrations/products/read | Ruft die Eigenschaften eines Azure Stack-Marketplace-Produkts ab. |
| Microsoft.AzureStack/registrations/read | Ruft die Eigenschaften einer Azure Stack-Registrierung ab. |

## <a name="backup-contributor"></a>Mitwirkender für Sicherungen
Kann alle Aktionen für die Sicherungsverwaltung durchführen, außer dem Erstellen eines Recovery Services-Tresors und dem Erteilen von Zugriff für andere

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Network/virtualNetworks/read | Lesen von virtuellen Netzwerken |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/* | Verwalten der Ergebnisse eines Vorgangs in der Sicherungsverwaltung |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/* | Erstellen und Verwalten von Sicherungscontainern in Sicherungsfabrics des Recovery Services-Tresors |
| Microsoft.RecoveryServices/Vaults/backupJobs/* | Erstellen und Verwalten von Sicherungsaufträgen |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Exportieren von Sicherungsaufträgen in Excel-Tabellen |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/* | Erstellen und Verwalten von Metadaten in Zusammenhang mit der Sicherungsverwaltung |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Erstellen und Verwalten der Ergebnisse von Sicherungsverwaltungsvorgängen |
| Microsoft.RecoveryServices/Vaults/backupPolicies/* | Erstellen und Verwalten von Sicherungsrichtlinien |
| Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Erstellen und Verwalten von Elementen, die gesichert werden können |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/* | Erstellen und Verwalten von gesicherten Elementen |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/* | Erstellen und Verwalten von Containern mit Sicherungselementen |
| Microsoft.RecoveryServices/Vaults/certificates/* | Erstellen und Verwalten von Zertifikaten in Zusammenhang mit Sicherungen in einem Recovery Services-Tresor |
| Microsoft.RecoveryServices/Vaults/extendedInformation/* | Erstellen und Verwalten erweiterter Informationen in Zusammenhang mit einem Tresor |
| Microsoft.RecoveryServices/Vaults/read | Lesen von Recovery Services-Tresoren |
| Microsoft.RecoveryServices/Vaults/refreshContainers/* | Verwalten von Ermittlungsvorgängen zum Abrufen neu erstellter Container |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Erstellen und Verwalten von registrierten Identitäten |
| Microsoft.RecoveryServices/Vaults/usages/* | Erstellen und Verwalten der Nutzung des Recovery Services-Tresors |
| Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Gibt Zusammenfassungen für geschützte Elemente und geschützte Server für einen Recovery Services-Tresor zurück. |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Storage/storageAccounts/read | Lesen von Speicherkonten |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | „GetAllocatedStamp“ ist ein interner Vorgang des Diensts. |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Ruft die Warnungen für den Recovery Services-Tresor ab. |
| Microsoft.RecoveryServices/Vaults/storageConfig/* |  |
| Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/* |  |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Gibt das Ergebnis des Vorgangs „Auftrag exportieren“ zurück. |
| Microsoft.RecoveryServices/Vaults/backupSecurityPIN/* |  |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="backup-operator"></a>Sicherungsoperator
Kann alle Aktionen für die Sicherungsverwaltung durchführen, außer dem Erstellen von Tresoren, dem Entfernen von Sicherungen und dem Erteilen von Zugriff für andere

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Network/virtualNetworks/read | Lesen von virtuellen Netzwerken |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Lesen der Ergebnisse eines Vorgangs in der Sicherungsverwaltung |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ operationResults/read | Einlesen von Vorgangsergebnissen in Schutzcontainer |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/backup/action | Durchführen von Sicherungsvorgängen in einem gesicherten Element nach Bedarf |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/operationResults/read | Lesen der Ergebnisse eines für ein gesichertes Element ausgeführten Vorgangs |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/operationsStatus/read | Gibt den Status eines Vorgangs zurück, der für geschützte Elemente ausgeführt wurde. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/read | Lesen von gesicherten Elementen |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/recoveryPoints/read | Lesen des Wiederherstellungspunkts eines gesicherten Elements |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/recoveryPoints/restore/action | Durchführen eines Wiederherstellungsvorgangs unter Verwendung eines Wiederherstellungspunkts eines gesicherten Elements |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/write | Erstellen eines Sicherungselements |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Lesen von Containern mit Sicherungselementen |
| Microsoft.RecoveryServices/Vaults/backupJobs/* | Erstellen und Verwalten von Sicherungsaufträgen |
| Microsoft.RecoveryServices/Vaults/backupJobs/cancel/action | Dient zum Abbrechen des Auftrags. |
| Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | Gibt das Ergebnis von Auftragsvorgängen zurück. |
| Microsoft.RecoveryServices/Vaults/backupJobs/read | Gibt alle Auftragsobjekte zurück. |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Exportieren von Sicherungsaufträgen in Excel-Tabellen |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Lesen von Metadaten in Zusammenhang mit der Sicherungsverwaltung |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Erstellen und Verwalten der Ergebnisse von Sicherungsverwaltungsvorgängen |
| Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Lesen der Ergebnisse von in Sicherungsrichtlinien durchgeführten Vorgängen |
| Microsoft.RecoveryServices/Vaults/backupPolicies/read | Lesen von Sicherungsrichtlinien |
| Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Erstellen und Verwalten von Elementen, die gesichert werden können |
| Microsoft.RecoveryServices/Vaults/backupProtectableItems/read | Gibt die Liste mit allen schützbaren Elementen zurück. |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Lesen von gesicherten Elementen |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Lesen von gesicherten Containern mit Sicherungselementen |
| Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Gibt Zusammenfassungen für geschützte Elemente und geschützte Server für einen Recovery Services-Tresor zurück. |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Lesen von erweiterten Informationen in Zusammenhang mit einem Tresor |
| Microsoft.RecoveryServices/Vaults/extendedInformation/write | Schreiben von erweiterten Informationen in Zusammenhang mit einem Tresor |
| Microsoft.RecoveryServices/Vaults/read | Lesen von Recovery Services-Tresoren |
| Microsoft.RecoveryServices/Vaults/refreshContainers/* | Verwalten von Ermittlungsvorgängen zum Abrufen neu erstellter Container |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Lesen der Ergebnisse eines mit registrierten Elementen des Tresors durchgeführten Vorgangs |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Lesen von registrierten Elementen des Tresors |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/write | Schreiben von registrierten Elementen in den Tresor |
| Microsoft.RecoveryServices/Vaults/usages/read | Lesen der Nutzung des Recovery Services-Tresors |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Storage/storageAccounts/read | Lesen von Speicherkonten |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/recoveryPoints/provisionInstantItemRecovery/action | Dient zum Bereitstellen der sofortigen Elementwiederherstellung für geschützte Elemente. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/recoveryPoints/revokeInstantItemRecovery/action | Dient zum Widerrufen der sofortigen Elementwiederherstellung für geschützte Elemente. |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | „GetAllocatedStamp“ ist ein interner Vorgang des Diensts. |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Ruft die Warnungen für den Recovery Services-Tresor ab. |
| Microsoft.RecoveryServices/Vaults/storageConfig/* |  |
| Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/* |  |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Gibt das Ergebnis des Vorgangs „Auftrag exportieren“ zurück. |
| Microsoft.RecoveryServices/Vaults/backupPolicies/operationStatus/read |  |
| Microsoft.RecoveryServices/Vaults/certificates/write | Der Vorgang „Ressourcenzertifikat aktualisieren“ aktualisiert das Zertifikat für die Ressourcen-/Tresoranmeldeinformationen. |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="backup-reader"></a>Sicherungsleser
Kann die Sicherungsverwaltung im Recovery Services-Tresor überwachen

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Lesen der Ergebnisse eines Vorgangs in der Sicherungsverwaltung |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ operationResults/read | Einlesen von Vorgangsergebnissen in Schutzcontainer |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/operationResults/read | Lesen der Ergebnisse eines für ein gesichertes Element ausgeführten Vorgangs |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/operationsStatus/read | Gibt den Status eines Vorgangs zurück, der für geschützte Elemente ausgeführt wurde. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/read | Lesen von gesicherten Elementen |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Lesen von Containern mit Sicherungselementen |
| Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | Lesen der Ergebnisse von Sicherungsaufträgen |
| Microsoft.RecoveryServices/Vaults/backupJobs/read | Lesen von Sicherungsaufträgen |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Exportieren von Sicherungsaufträgen in Excel-Tabellen |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Lesen von Metadaten in Zusammenhang mit der Sicherungsverwaltung |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/read | Lesen der Ergebnisse von Vorgängen der Sicherungsverwaltung |
| Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Lesen der Ergebnisse von in Sicherungsrichtlinien durchgeführten Vorgängen |
| Microsoft.RecoveryServices/Vaults/backupPolicies/read | Lesen von Sicherungsrichtlinien |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Lesen von gesicherten Elementen |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Lesen von gesicherten Containern mit Sicherungselementen |
| Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Gibt Zusammenfassungen für geschützte Elemente und geschützte Server für einen Recovery Services-Tresor zurück. |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Lesen von erweiterten Informationen in Zusammenhang mit einem Tresor |
| Microsoft.RecoveryServices/Vaults/read | Lesen von Recovery Services-Tresoren |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read | Lesen der Ergebnisse von Ermittlungsvorgängen zum Abrufen neu erstellter Container |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Lesen der Ergebnisse eines mit registrierten Elementen des Tresors durchgeführten Vorgangs |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Lesen von registrierten Elementen des Tresors |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | „GetAllocatedStamp“ ist ein interner Vorgang des Diensts. |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Ruft die Warnungen für den Recovery Services-Tresor ab. |
| Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
| Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/read |  |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/recoveryPoints/read | Dient zum Abrufen von Wiederherstellungspunkten für geschützte Elemente. |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Gibt das Ergebnis des Vorgangs „Auftrag exportieren“ zurück. |
| Microsoft.RecoveryServices/Vaults/usages/read | Lesen der Nutzung des Recovery Services-Tresors |

## <a name="billing-reader"></a>Abrechnungsleser
Kann sämtliche Abrechnungsinformationen anzeigen

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Billing/*/read | Lesen von Abrechnungsinformationen |
| Microsoft.Consumption/*/read |  |
| Microsoft.Commerce/*/read |  |
| Microsoft.Management/managementGroups/read | Listet die Verwaltungsgruppen für den authentifizierten Benutzer auf. |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="biztalk-contributor"></a>Mitwirkender von BizTalk
Kann BizTalk-Dienste verwalten

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.BizTalkServices/BizTalk/* | Erstellen und Verwalten von BizTalk-Diensten |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="cdn-endpoint-contributor"></a>Mitwirkender für den CDN-Endpunkt
Kann CDN-Endpunkte verwalten, aber anderen Benutzern keinen Zugriff erteilen.

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Cdn/edgenodes/read |  |
| Microsoft.Cdn/operationresults/* |  |
| Microsoft.Cdn/profiles/endpoints/* |  |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="cdn-endpoint-reader"></a>CDN-Endpunktleser
Kann CDN-Endpunkte anzeigen, aber keine Änderungen vornehmen.

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Cdn/edgenodes/read |  |
| Microsoft.Cdn/operationresults/* |  |
| Microsoft.Cdn/profiles/endpoints/*/read |  |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="cdn-profile-contributor"></a>Mitwirkender für das CDN-Profil
Kann CDN-Profile und deren Endpunkte verwalten, aber anderen Benutzern keinen Zugriff erteilen.

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Cdn/edgenodes/read |  |
| Microsoft.Cdn/operationresults/* |  |
| Microsoft.Cdn/profiles/* |  |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="cdn-profile-reader"></a>CDN-Profilleser
Kann CDN-Profile und deren Endpunkte anzeigen, aber keine Änderungen vornehmen.

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Cdn/edgenodes/read |  |
| Microsoft.Cdn/operationresults/* |  |
| Microsoft.Cdn/profiles/*/read |  |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="classic-network-contributor"></a>Mitwirkender von klassischem Netzwerk
Kann klassische virtuelle Netzwerke und reservierte IP-Adressen verwalten

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Autorisierungen |
| Microsoft.ClassicNetwork/* | Erstellen und Verwalten von klassischen Netzwerken |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="classic-storage-account-contributor"></a>Mitwirkender von klassischem Speicherkonto
Kann klassische Speicherkonten verwalten

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Autorisierungen |
| Microsoft.ClassicStorage/storageAccounts/* | Erstellen und Verwalten von Speicherkonten |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="classic-storage-account-key-operator-service-role"></a>Klassische Dienstrolle „Speicherkonto-Schlüsseloperator“
Klassische Speicherkonto-Schlüsseloperatoren dürfen Schlüssel für klassische Speicherkonten auflisten und neu generieren.

| **Aktionen** |  |
| --- | --- |
| Microsoft.ClassicStorage/storageAccounts/listkeys/action | Listet die Zugriffsschlüssel für die Speicherkonten auf. |
| Microsoft.ClassicStorage/storageAccounts/regeneratekey/action | Generiert die vorhandenen Zugriffsschlüssel für das Speicherkonto neu. |

## <a name="classic-virtual-machine-contributor"></a>Mitwirkender von klassischen virtuellen Computern
Kann klassische virtuelle Computer verwalten, jedoch nicht das virtuelle Netzwerk oder Speicherkonto, mit dem sie verbunden sind

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Autorisierungen |
| Microsoft.ClassicCompute/domainNames/* | Erstellen und Verwalten von klassischen Compute-Domänennamen |
| Microsoft.ClassicCompute/virtualMachines/* | Erstellen und Verwalten von virtuellen Computern |
| Microsoft.ClassicNetwork/networkSecurityGroups/join/action | Verknüpfen von Netzwerksicherheitsgruppen |
| Microsoft.ClassicNetwork/reservedIps/link/action | Verknüpfen von reservierten IP-Adressen |
| Microsoft.ClassicNetwork/reservedIps/read | Lesen von reservierten IP-Adressen |
| Microsoft.ClassicNetwork/virtualNetworks/join/action | Verknüpfen von virtuellen Netzwerken |
| Microsoft.ClassicNetwork/virtualNetworks/read | Lesen von virtuellen Netzwerken |
| Microsoft.ClassicStorage/storageAccounts/disks/read | Lesen von Speicherkontodatenträgern |
| Microsoft.ClassicStorage/storageAccounts/images/read | Lesen von Speicherkontoimages |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action | Auflisten von Speicherkontoschlüsseln |
| Microsoft.ClassicStorage/storageAccounts/read | Lesen von klassischen Speicherkonten |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="cleardb-mysql-db-contributor"></a>Mitwirkender von ClearDB-MySQL-DB
Kann ClearDB MySQL-Datenbanken verwalten

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |
| successbricks.cleardb/databases/* | Erstellen und Verwalten von ClearDB MySQL-Datenbanken |

## <a name="cosmos-db-account-reader-role"></a>Cosmos DB-Rolle „Kontoleser“
Kann Azure Cosmos DB-Kontodaten lesen. Informationen zum Verwalten von Azure Cosmos DB-Konten finden Sie unter [Mitwirkender von DocumentDB-Konto](#documentdb-account-contributor).

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen, kann die jedem Benutzer erteilten Berechtigungen lesen |
| Microsoft.DocumentDB/*/read | Lesen einer beliebigen Sammlung |
| Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | Lesen des Bereichs mit schreibgeschützten Schlüsseln |
| Microsoft.Insights/MetricDefinitions/read | Lesen von Metrikdefinitionen |
| Microsoft.Insights/Metrics/read | Lesen von Speicherkontometriken |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="data-factory-contributor"></a>Mitwirkender von Data Factory
Erstellt und verwaltet Data Factorys und darin enthaltene untergeordnete Ressourcen.

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.DataFactory/dataFactories/* | Erstellt und verwaltet Data Factorys und darin enthaltene untergeordnete Ressourcen. |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="data-lake-analytics-developer"></a>Data Lake Analytics-Entwickler
Ermöglicht Ihnen das Übermitteln, Überwachen und Verwalten Ihrer eigenen Aufträge, aber nicht das Erstellen oder Löschen von Data Lake Analytics-Konten.

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.BigAnalytics/accounts/* |  |
| Microsoft.DataLakeAnalytics/accounts/* |  |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

| **NotActions** |  |
| --- | --- |
| Microsoft.BigAnalytics/accounts/Delete |  |
| Microsoft.BigAnalytics/accounts/TakeOwnership/action |  |
| Microsoft.BigAnalytics/accounts/Write |  |
| Microsoft.DataLakeAnalytics/accounts/Delete | Löscht ein DataLakeAnalytics-Konto. |
| Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action | Erteilt Berechtigungen zum Abbrechen von Aufträgen, die von anderen Benutzern übermittelt wurden. |
| Microsoft.DataLakeAnalytics/accounts/Write | Erstellt oder aktualisiert ein DataLakeAnalytics-Konto. |
| Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Write | Erstellt oder aktualisiert ein verknüpftes DataLakeStore-Konto eines DataLakeAnalytics-Kontos. |
| Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Delete | Hebt die Verknüpfung eines DataLakeStore-Kontos mit einem DataLakeAnalytics-Konto auf. |
| Microsoft.DataLakeAnalytics/accounts/storageAccounts/Write | Erstellt oder aktualisiert ein verknüpftes Speicherkonto eines DataLakeAnalytics-Kontos. |
| Microsoft.DataLakeAnalytics/accounts/storageAccounts/Delete | Hebt die Verknüpfung eines Speicherkontos mit einem DataLakeAnalytics-Konto auf. |
| Microsoft.DataLakeAnalytics/accounts/firewallRules/Write | Dient zum Erstellen oder Aktualisieren einer Firewallregel. |
| Microsoft.DataLakeAnalytics/accounts/firewallRules/Delete | Dient zum Löschen einer Firewallregel. |
| Microsoft.DataLakeAnalytics/accounts/computePolicies/Write | Erstellt oder aktualisiert eine Computerichtlinie. |
| Microsoft.DataLakeAnalytics/accounts/computePolicies/Delete | Löscht eine Computerichtlinie. |

## <a name="devtest-labs-user"></a>DevTest Labs-Benutzer
Kann alles anzeigen sowie virtuelle Maschinen verbinden, starten, neu starten und herunterfahren

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Compute/availabilitySets/read | Lesen der Eigenschaften von Verfügbarkeitsgruppen |
| Microsoft.Compute/virtualMachines/*/read | Lesen der Eigenschaften eines virtuellen Computers (VM-Größen, Laufzeitstatus, VM-Erweiterungen usw.) |
| Microsoft.Compute/virtualMachines/deallocate/action | Aufhebung der Zuordnung virtueller Computer |
| Microsoft.Compute/virtualMachines/read | Lesen der Eigenschaften eines virtuellen Computers |
| Microsoft.Compute/virtualMachines/restart/action | Neustarten virtueller Computer |
| Microsoft.Compute/virtualMachines/start/action | Starten virtueller Computer |
| Microsoft.DevTestLab/*/read | Lesen der Eigenschaften eines Labs |
| Microsoft.DevTestLab/labs/createEnvironment/action | Erstellen einer Lab-Umgebung |
| Microsoft.DevTestLab/labs/claimAnyVm/action | Dient zum Anfordern eines beliebigen anforderbaren virtuellen Computers im Lab. |
| Microsoft.DevTestLab/labs/formulas/delete | Löschen von Formeln |
| Microsoft.DevTestLab/labs/formulas/read | Lesen von Formeln |
| Microsoft.DevTestLab/labs/formulas/write | Hinzufügen oder Ändern von Formeln |
| Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | Auswerten von Labrichtlinien |
| Microsoft.DevTestLab/labs/virtualMachines/claim/action | Dient dazu, einen vorhandenen virtuellen Computer in Besitz zu nehmen. |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Verknüpfen eines Back-End-Adresspools für Load Balancer |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Verknüpfen einer eingehenden NAT-Regel für Load Balancer |
| Microsoft.Network/networkInterfaces/*/read | Lesen der Eigenschaften einer Netzwerkschnittstelle (z.B. alle Load Balancer, zu denen die Netzwerkschnittstelle gehört) |
| Microsoft.Network/networkInterfaces/join/action | Verknüpfen eines virtuellen Computers mit einer Netzwerkschnittstelle |
| Microsoft.Network/networkInterfaces/read | Lesen von Netzwerkschnittstellen |
| Microsoft.Network/networkInterfaces/write | Schreiben in Netzwerkschnittstellen |
| Microsoft.Network/publicIPAddresses/*/read | Lesen der Eigenschaften einer öffentlichen IP-Adresse |
| Microsoft.Network/publicIPAddresses/join/action | Verknüpfen einer öffentlichen IP-Adresse |
| Microsoft.Network/publicIPAddresses/read | Lesen von öffentlichen IP-Adressen im Netzwerk |
| Microsoft.Network/virtualNetworks/subnets/join/action | Verknüpfen eines virtuellen Netzwerks |
| Microsoft.Resources/deployments/operations/read | Lesen von Bereitstellungsvorgängen |
| Microsoft.Resources/deployments/read | Lesen von Bereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Storage/storageAccounts/listKeys/action | Auflisten von Speicherkontoschlüsseln |

| **NotActions** |  |
| --- | --- |
| Microsoft.Compute/virtualMachines/vmSizes/read | Listet die verfügbaren Größen auf, auf die der virtuelle Computer aktualisiert werden kann. |

## <a name="dns-zone-contributor"></a>DNS Zone Contributor
Kann DNS-Zonen und -Einträge verwalten.

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.Network/dnsZones/* | Erstellen und Verwalten von DNS-Zonen und -Einträgen |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Supporttickets |

## <a name="documentdb-account-contributor"></a>Mitwirkender von DocumentDB-Konto
Kann Azure Cosmos DB-Konten verwalten. Azure Cosmos DB wurde früher als DocumentDB bezeichnet.

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.DocumentDb/databaseAccounts/* | Kann Azure Cosmos DB-Konten erstellen und verwalten |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="intelligent-systems-account-contributor"></a>Mitwirkender von Intelligent Systems-Konto
Kann Intelligent Systems-Konten verwalten

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.IntelligentSystems/accounts/* | Erstellen und Verwalten von Intelligent Systems-Konten |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="key-vault-contributor"></a>Key Vault-Mitwirkender
Ermöglicht Ihnen die Verwaltung von Schlüsseltresoren, aber nicht den Zugriff darauf.

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.KeyVault/* |  |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

| **NotActions** |  |
| --- | --- |
| Microsoft.KeyVault/locations/deletedVaults/purge/action | Dient zum endgültigen Löschen eines vorläufig gelöschten Schlüsseltresors. |
| Microsoft.KeyVault/hsmPools/* |  |

## <a name="lab-creator"></a>Lab-Ersteller
Ermöglicht Ihnen das Erstellen, Verwalten und Löschen verwalteter Labs unter Ihren Azure Lab-Konten.

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.ManagedLab/labAccounts/createLab/action | Hiermit wird ein Lab in einem Lab-Konto erstellt. |
| Microsoft.ManagedLab/labAccounts/*/read |  |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="log-analytics-contributor"></a>Log Analytics-Mitwirkender
Ein Log Analytics-Mitwirkender kann alle Überwachungsdaten lesen und Überwachungseinstellungen bearbeiten. Das Bearbeiten von Überwachungseinstellungen schließt folgende Aufgaben ein: Hinzufügen der VM-Erweiterung zu VMs, Lesen von Speicherkontoschlüsseln zum Konfigurieren von Protokollsammlungen aus Azure Storage, Erstellen und Konfigurieren von Automation-Konten, Hinzufügen von Lösungen, Konfigurieren der Azure-Diagnose für alle Azure-Ressourcen.

| **Aktionen** |  |
| --- | --- |
| */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
| Microsoft.Automation/automationAccounts/* |  |
| Microsoft.ClassicCompute/virtualMachines/extensions/* |  |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action | Listet die Zugriffsschlüssel für die Speicherkonten auf. |
| Microsoft.Compute/virtualMachines/extensions/* |  |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.Insights/diagnosticSettings/* | Erstellt, aktualisiert oder liest die Diagnoseeinstellung für den Analysis-Server. |
| Microsoft.OperationalInsights/* |  |
| Microsoft.OperationsManagement/* |  |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourcegroups/deployments/* |  |
| Microsoft.Storage/storageAccounts/listKeys/action | Gibt die Zugriffsschlüssel für das angegebene Speicherkonto zurück. |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="log-analytics-reader"></a>Log Analytics-Leser
Ein Log Analytics-Leser kann alle Überwachungsdaten anzeigen und durchsuchen sowie Überwachungseinstellungen anzeigen. Hierzu zählt auch die Anzeige der Konfiguration von Azure-Diagnosen für alle Azure-Ressourcen.

| **Aktionen** |  |
| --- | --- |
| */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
| Microsoft.OperationalInsights/workspaces/analytics/query/action | Führt eine Suche mit der neuen Engine aus. |
| Microsoft.OperationalInsights/workspaces/search/action | Führt eine Suchabfrage aus. |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

| **NotActions** |  |
| --- | --- |
| Microsoft.OperationalInsights/workspaces/sharedKeys/read | Ruft die gemeinsam verwendeten Schlüssel für den Arbeitsbereich ab. Diese Schlüssel werden verwendet, um Microsoft Operational Insights-Agents mit dem Arbeitsbereich zu verbinden. |

## <a name="logic-app-contributor"></a>Mitwirkender für Logik-Apps
Ermöglicht Ihnen das Verwalten von Logik-Apps, aber nicht den Zugriff darauf.

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action | Listet die Zugriffsschlüssel für die Speicherkonten auf. |
| Microsoft.ClassicStorage/storageAccounts/read | Dient zum Zurückgeben des Speicherkontos mit dem angegebenen Konto. |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.Insights/diagnosticSettings/* | Erstellt, aktualisiert oder liest die Diagnoseeinstellung für den Analysis-Server. |
| Microsoft.Insights/logdefinitions/* | Diese Berechtigung ist für Benutzer notwendig, die über das Portal auf Aktivitätsprotokolle zugreifen müssen. Auflisten der Protokollkategorien im Aktivitätsprotokoll. |
| Microsoft.Insights/metricDefinitions/* | Lesen von Metrikdefinitionen (Liste der verfügbaren Metriktypen für eine Ressource). |
| Microsoft.Logic/* | Verwaltet Logic Apps-Ressourcen. |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/operationresults/read | Dient zum Abrufen der Ergebnisse des Abonnementvorgangs. |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
| Microsoft.Storage/storageAccounts/listkeys/action | Gibt die Zugriffsschlüssel für das angegebene Speicherkonto zurück. |
| Microsoft.Storage/storageAccounts/read | Gibt die Liste mit Speicherkonten zurück oder ruft die Eigenschaften für das angegebene Speicherkonto ab. |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |
| Microsoft.Web/connectionGateways/* | Erstellt und verwaltet ein Verbindungsgateway. |
| Microsoft.Web/connections/* | Erstellt und verwaltet eine Verbindung. |
| Microsoft.Web/customApis/* | Erstellt und verwaltet eine benutzerdefinierte API. |
| Microsoft.Web/serverFarms/join/action |  |
| Microsoft.Web/serverFarms/read | Dient zum Abrufen der Eigenschaften für einen App Service-Plan. |
| Microsoft.Web/sites/functions/listSecrets/action | Dient zum Auflisten der Geheimnisse für Web-App-Funktionen. |

## <a name="logic-app-operator"></a>Logik-App-Operator
Ermöglicht Ihnen das Lesen, Aktivieren und Deaktivieren von Logik-Apps.

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Insights/alertRules/*/read | Lesen von Insights-Warnungsregeln |
| Microsoft.Insights/diagnosticSettings/*/read | Ruft die Diagnoseeinstellungen für Logic Apps ab. |
| Microsoft.Insights/metricDefinitions/*/read | Ruft die verfügbaren Metriken für Logic Apps ab. |
| Microsoft.Logic/*/read | Liest Logic Apps-Ressourcen. |
| Microsoft.Logic/workflows/disable/action | Deaktiviert den Workflow. |
| Microsoft.Logic/workflows/enable/action | Aktiviert den Workflow. |
| Microsoft.Logic/workflows/validate/action | Überprüft den Workflow. |
| Microsoft.Resources/deployments/operations/read | Ruft Bereitstellungsvorgänge ab oder listet sie auf. |
| Microsoft.Resources/subscriptions/operationresults/read | Dient zum Abrufen der Ergebnisse des Abonnementvorgangs. |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |
| Microsoft.Web/connectionGateways/*/read | Liest Verbindungsgateways. |
| Microsoft.Web/connections/*/read | Liest Verbindungen. |
| Microsoft.Web/customApis/*/read | Liest benutzerdefinierte API. |
| Microsoft.Web/serverFarms/read | Dient zum Abrufen der Eigenschaften für einen App Service-Plan. |

## <a name="managed-identity-contributor"></a>Mitwirkender für verwaltete Identität
Dem Benutzer zugewiesene Identität erstellen, lesen, aktualisieren und löschen.

| **Aktionen** |  |
| --- | --- |
| Microsoft.ManagedIdentity/userAssignedIdentities/*/read |  |
| Microsoft.ManagedIdentity/userAssignedIdentities/*/write |  |
| Microsoft.ManagedIdentity/userAssignedIdentities/*/delete |  |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="managed-identity-operator"></a>Operator für verwaltete Identität
Dem Benutzer zugewiesene Identität lesen und zuweisen.

| **Aktionen** |  |
| --- | --- |
| Microsoft.ManagedIdentity/userAssignedIdentities/*/read |  |
| Microsoft.ManagedIdentity/userAssignedIdentities/*/assign/action |  |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="monitoring-contributor"></a>Überwachungsmitwirkender
Kann sämtliche Überwachungsdaten lesen und Überwachungseinstellungen bearbeiten. Siehe auch [Erste Schritte mit Rollen, Berechtigungen und Sicherheit in Azure Monitor](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles).

| **Aktionen** |  |
| --- | --- |
| */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
| Microsoft.AlertsManagement/alerts/* |  |
| Microsoft.AlertsManagement/alertsSummary/* |  |
| Microsoft.Insights/AlertRules/* | Lesen/Schreiben/Löschen von Warnungsregeln. |
| Microsoft.Insights/components/* | Kann Application Insights-Komponenten lesen/schreiben/delegieren. |
| Microsoft.Insights/DiagnosticSettings/* | Lesen/Schreiben/Löschen von Diagnoseeinstellungen. |
| Microsoft.Insights/eventtypes/* | Auflisten von Aktivitätsprotokollereignissen (Verwaltungsereignissen) in einem Abonnement. Diese Berechtigung gilt sowohl für den programmgesteuerten als auch für den Portalzugriff auf das Aktivitätsprotokoll. |
| Microsoft.Insights/LogDefinitions/* | Diese Berechtigung ist für Benutzer notwendig, die über das Portal auf Aktivitätsprotokolle zugreifen müssen. Auflisten der Protokollkategorien im Aktivitätsprotokoll. |
| Microsoft.Insights/MetricDefinitions/* | Lesen von Metrikdefinitionen (Liste der verfügbaren Metriktypen für eine Ressource). |
| Microsoft.Insights/Metrics/* | Lesen von Metriken für eine Ressource. |
| Microsoft.Insights/Register/Action | Registriert den Microsoft.Insights-Anbieter. |
| Microsoft.Insights/webtests/* | Lesen/Schreiben/Löschen von Application Insights-Webtests. |
| Microsoft.OperationalInsights/workspaces/intelligencepacks/* | Lesen/Schreiben/Löschen von Log Analytics-Lösungspaketen. |
| Microsoft.OperationalInsights/workspaces/savedSearches/* | Lesen/Schreiben/Löschen von gespeicherten Log Analytics-Suchvorgängen. |
| Microsoft.OperationalInsights/workspaces/search/action | Durchsuchen von Log Analytics-Arbeitsbereichen. |
| Microsoft.OperationalInsights/workspaces/sharedKeys/action | Auflisten der Schlüssel für einen Log Analytics-Arbeitsbereich. |
| Microsoft.OperationalInsights/workspaces/storageinsightconfigs/* | Lesen/Schreiben/Löschen von Log Analytics-Speicherdetailinformationen. |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |
| Microsoft.WorkloadMonitor/workloads/* |  |

## <a name="monitoring-reader"></a>Überwachungsleser
Kann alle Überwachungsdaten (Metriken, Protokolle usw.) lesen. Siehe auch [Erste Schritte mit Rollen, Berechtigungen und Sicherheit in Azure Monitor](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles).

| **Aktionen** |  |
| --- | --- |
| */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
| Microsoft.OperationalInsights/workspaces/search/action | Durchsuchen von Log Analytics-Daten |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="network-contributor"></a>Mitwirkender von virtuellem Netzwerk
Kann alle Netzwerkressourcen verwalten

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.Network/* | Erstellen und Verwalten von Netzwerken |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="new-relic-apm-account-contributor"></a>Mitwirkender von New Relic APM-Konto
Ermöglicht Ihnen das Verwalten von New Relic Application Performance Management-Konten und -Anwendungen, nicht aber den Zugriff auf diese.

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |
| NewRelic.APM/accounts/* |  |

## <a name="redis-cache-contributor"></a>Mitwirkender von Redis-Cache
Kann Redis-Caches verwalten

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Cache/redis/* | Erstellen und Verwalten von Redis-Caches |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="scheduler-job-collections-contributor"></a>Mitwirkender von Zeitplanungsauftragssammlung
Kann Zeitplanungsauftragssammlungen verwalten

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Scheduler/jobcollections/* | Erstellen und Verwalten von Auftragssammlungen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="search-service-contributor"></a>Mitwirkender von Suchdienst
Kann Suchdienste verwalten

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Search/searchServices/* | Erstellen und Verwalten von Suchdiensten |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="security-admin"></a>Sicherheitsadministrator
Nur in Security Center: Kann Sicherheitsrichtlinien und -zustände anzeigen, Sicherheitsrichtlinien bearbeiten sowie Warnungen und Empfehlungen anzeigen und verwerfen

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Authorization/policyAssignments/* | Erstellen und Verwalten von Richtlinienzuweisungen |
| Microsoft.Authorization/policyDefinitions/* | Erstellen und Verwalten von Richtliniendefinitionen |
| Microsoft.Authorization/policySetDefinitions/* | Erstellen und Verwalten von Richtliniensätzen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.operationalInsights/workspaces/*/read | Anzeigen von Log Analytics-Daten |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Security/*/read | Lesen von Sicherheitskomponenten und -richtlinien |
| Microsoft.Security/locations/alerts/dismiss/action | Schließt eine Sicherheitswarnung. |
| Microsoft.Security/locations/tasks/dismiss/action | Dient zum Verwerfen einer Sicherheitsempfehlung. |
| Microsoft.Security/policies/write | Aktualisiert die Sicherheitsrichtlinie. |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="security-manager"></a>Sicherheits-Manager
Kann Sicherheitskomponenten, Sicherheitsrichtlinien und virtuelle Maschinen verwalten

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.ClassicCompute/*/read | Lesen von Konfigurationsinformationen zu klassischen virtuellen Computern |
| Microsoft.ClassicCompute/virtualMachines/*/write | Schreiben der Konfiguration für klassische virtuelle Computer |
| Microsoft.ClassicNetwork/*/read | Lesen von Konfigurationsinformationen zu klassischem Netzwerk |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Security/* | Erstellen und Verwalten von Sicherheitskomponenten und -richtlinien |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="security-reader"></a>Sicherheit lesen
Nur in Security Center: Kann Empfehlungen und Warnungen sowie Sicherheitsrichtlinien und -zustände anzeigen, aber keine Änderungen vornehmen

| **Aktionen** |  |
| --- | --- |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.operationalInsights/workspaces/*/read | Anzeigen von Log Analytics-Daten |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Security/*/read | Lesen von Sicherheitskomponenten und -richtlinien |

## <a name="site-recovery-contributor"></a>Site Recovery-Mitwirkender
Kann alle Site Recovery-Verwaltungsaktionen verwalten, mit Ausnahme der Erstellung des Recovery Services-Tresors und der Zuweisung von Zugriffsrechten für andere Benutzer

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.Network/virtualNetworks/read | Lesen von virtuellen Netzwerken |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | „GetAllocatedStamp“ ist ein interner Vorgang des Diensts. |
| Microsoft.RecoveryServices/locations/allocateStamp/action | „AllocateStamp“ ist ein interner Vorgang des Diensts. |
| Microsoft.RecoveryServices/Vaults/certificates/write | Aktualisieren des Tresoranmeldeinformationen-Zertifikats |
| Microsoft.RecoveryServices/Vaults/extendedInformation/* | Erstellen und Verwalten erweiterter Informationen in Zusammenhang mit einem Tresor |
| Microsoft.RecoveryServices/Vaults/read | Lesen von Recovery Services-Tresoren |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read | Verwalten von Ermittlungsvorgängen zum Abrufen neu erstellter Container |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Erstellen und Verwalten von registrierten Identitäten |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/* | Erstellen oder Aktualisieren von Einstellungen für Replikationswarnungen |
| Microsoft.RecoveryServices/vaults/replicationEvents/read | Lesen von Replikationsereignissen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/* | Erstellen und Verwalten von Replikationsfabrics |
| Microsoft.RecoveryServices/vaults/replicationJobs/* | Erstellen und Verwalten von Replikationsaufträgen |
| Microsoft.RecoveryServices/vaults/replicationPolicies/* | Erstellen und Verwalten von Replikationsrichtlinien |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/* | Erstellen und Verwalten von Wiederherstellungsplänen |
| Microsoft.RecoveryServices/Vaults/storageConfig/* | Erstellen und Verwalten der Speicherkonfiguration des Recovery Services-Tresors |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read | Lesen der Tokeninformationen für den Recovery Services-Tresor |
| Microsoft.RecoveryServices/Vaults/usages/read | Lesen der Nutzungsdetails eines Recovery Services-Tresors |
| Microsoft.RecoveryServices/Vaults/vaultTokens/read | Der Vorgang „Tresortoken“ kann zum Abrufen des Tresortokens für Back-End-Vorgänge auf Tresorebene verwendet werden. |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Lesen von Warnungen für den Recovery Services-Tresor |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read | Lesen der Konfiguration für Recovery Services-Tresorbenachrichtigungen |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Storage/storageAccounts/read | Lesen von Speicherkonten |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="site-recovery-operator"></a>Site Recovery-Operator
Kann Failover und Failbacks durchführen, aber keine anderen Site Recovery-Verwaltungsaktionen, oder den Zugriff für andere Benutzer zuweisen

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.Network/virtualNetworks/read | Lesen von virtuellen Netzwerken |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | „GetAllocatedStamp“ ist ein interner Vorgang des Diensts. |
| Microsoft.RecoveryServices/locations/allocateStamp/action | „AllocateStamp“ ist ein interner Vorgang des Diensts. |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Lesen von erweiterten Informationen in Zusammenhang mit einem Tresor |
| Microsoft.RecoveryServices/Vaults/read | Lesen von Recovery Services-Tresoren |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read | Verwalten von Ermittlungsvorgängen zum Abrufen neu erstellter Container |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Lesen des Vorgangsstatus und -ergebnisses für einen übermittelten Vorgang |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Lesen von Containern, die für eine Ressource registriert sind |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Lesen von Einstellungen für Replikationswarnungen |
| Microsoft.RecoveryServices/vaults/replicationEvents/read | Lesen von Replikationsereignissen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Prüfen der Konsistenz von Fabrics |
| Microsoft.RecoveryServices/vaults/replicationFabrics/read | Lesen von Replikationsfabrics |
| Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | Erneutes Zuordnen des Replikationsgateways |
| Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Erneuern des Replikationsfabric-Zertifikats |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Lesen von Replikationsfabric-Netzwerken |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationNetworks/replicationNetworkMappings/read | Lesen der Replikationsfabric-Netzwerkzuordnung |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/read | Lesen von Schutzcontainern |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectableItems/read | Abrufen der Liste mit allen schützbaren Elementen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Dient zum Anwenden des Wiederherstellungspunkts. |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | Failovercommit |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | Geplantes Failover |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/read | Abrufen einer Liste mit allen geschützten Elementen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Abrufen einer Liste mit verfügbaren Wiederherstellungspunkten |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/repairReplication/action | Dient zum Reparieren der Replikation. |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/reProtect/action | Starten des erneuten Schutzes für ein geschütztes Element |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/testFailover/action | Starten des Testfailovers eines geschützten Elements |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | Testfailoverbereinigung |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action | Failover |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Dient zum Aktualisieren von Mobility Service. |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectionContainerMappings/read | Lesen von Schutzcontainerzuordnungen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders/read | Lesen von Recovery Services-Anbietern |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders/refreshProvider/action | Aktualisieren des Recovery Services-Anbieters |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/read | Lesen von Speicherklassifizierungen für Replikationsfabrics |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/replicationStorageClassificationMappings/read | Lesen von Speicherklassifizierungszuordnungen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Lesen von registrierten vCenter-Informationen |
| Microsoft.RecoveryServices/vaults/replicationJobs/* | Erstellen und Verwalten von Replikationsaufträgen |
| Microsoft.RecoveryServices/vaults/replicationPolicies/read | Lesen von Replikationsrichtlinien |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | Committen des Failovers für ein Wiederherstellungsplan-Failover |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | Starten des Failovers eines Wiederherstellungsplans |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Lesen von Wiederherstellungsplänen |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Starten des erneuten Schutzes eines Wiederherstellungsplans |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Starten des Testfailovers eines Wiederherstellungsplans |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Starten der Bereinigung eines Wiederherstellungsplan-Testfailovers |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action | Starten des ungeplanten Failovers eines Wiederherstellungsplans |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Lesen von Warnungen für den Recovery Services-Tresor |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read | Lesen der Konfiguration für Recovery Services-Tresorbenachrichtigungen |
| Microsoft.RecoveryServices/Vaults/storageConfig/read | Lesen der Speicherkonfiguration eines Recovery Services-Tresors |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read | Lesen der Tokeninformationen für den Recovery Services-Tresor |
| Microsoft.RecoveryServices/Vaults/usages/read | Lesen der Nutzungsdetails eines Recovery Services-Tresors |
| Microsoft.RecoveryServices/Vaults/vaultTokens/read | Der Vorgang „Tresortoken“ kann zum Abrufen des Tresortokens für Back-End-Vorgänge auf Tresorebene verwendet werden. |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Storage/storageAccounts/read | Lesen von Speicherkonten |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="site-recovery-reader"></a>Site Recovery-Leser
Kann den Site Recovery-Status im Recovery Services-Tresor überwachen und Supporttickets ausstellen

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | „GetAllocatedStamp“ ist ein interner Vorgang des Diensts. |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Lesen von erweiterten Informationen in Zusammenhang mit einem Tresor |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Lesen von Warnungen für den Recovery Services-Tresor |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read | Lesen der Konfiguration für Recovery Services-Tresorbenachrichtigungen |
| Microsoft.RecoveryServices/Vaults/read | Lesen von Recovery Services-Tresoren |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read | Verwalten von Ermittlungsvorgängen zum Abrufen neu erstellter Container |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Lesen des Vorgangsstatus und -ergebnisses für einen übermittelten Vorgang |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Lesen von Containern, die für eine Ressource registriert sind |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Lesen von Einstellungen für Replikationswarnungen |
| Microsoft.RecoveryServices/vaults/replicationEvents/read | Lesen von Replikationsereignissen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/read | Lesen von Replikationsfabrics |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Lesen von Replikationsfabric-Netzwerken |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationNetworks/replicationNetworkMappings/read | Lesen der Replikationsfabric-Netzwerkzuordnung |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/read | Lesen von Schutzcontainern |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectableItems/read | Abrufen der Liste mit allen schützbaren Elementen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/read | Abrufen einer Liste mit allen geschützten Elementen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Abrufen einer Liste mit verfügbaren Wiederherstellungspunkten |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectionContainerMappings/read | Lesen von Schutzcontainerzuordnungen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders/read | Lesen von Recovery Services-Anbietern |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/read | Lesen von Speicherklassifizierungen für Replikationsfabrics |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/replicationStorageClassificationMappings/read | Lesen von Speicherklassifizierungszuordnungen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Lesen von registrierten vCenter-Informationen |
| Microsoft.RecoveryServices/vaults/replicationJobs/read | Lesen des Status von Replikationsaufträgen |
| Microsoft.RecoveryServices/vaults/replicationPolicies/read | Lesen von Replikationsrichtlinien |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Lesen von Wiederherstellungsplänen |
| Microsoft.RecoveryServices/Vaults/storageConfig/read | Lesen der Speicherkonfiguration eines Recovery Services-Tresors |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read | Lesen der Tokeninformationen für den Recovery Services-Tresor |
| Microsoft.RecoveryServices/Vaults/usages/read | Lesen der Nutzungsdetails eines Recovery Services-Tresors |
| Microsoft.RecoveryServices/Vaults/vaultTokens/read | Der Vorgang „Tresortoken“ kann zum Abrufen des Tresortokens für Back-End-Vorgänge auf Tresorebene verwendet werden. |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="sql-db-contributor"></a>Mitwirkender von SQL DB
Kann SQL-Datenbanken verwalten, jedoch nicht die zugehörigen sicherheitsbezogenen Richtlinien

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Sql/locations/*/read |  |
| Microsoft.Sql/servers/databases/* | Erstellen und Verwalten von SQL-Datenbanken |
| Microsoft.Sql/servers/read | Lesen von SQL Server |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

| **NotActions** |  |
| --- | --- |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Kann keine Überwachungsrichtlinien bearbeiten |
| Microsoft.Sql/servers/databases/auditingSettings/* | Überwachungsrichtlinien können nicht bearbeiten werden |
| Microsoft.Sql/servers/databases/auditRecords/read | Kann keine Überwachungsdatensätze lesen |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Kann keine Verbindungsrichtlinien bearbeiten |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Kann keine Datenmaskierungsrichtlinien bearbeiten |
| Microsoft.Sql/servers/databases/extendedAuditingSettings/* |  |
| Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Richtlinien für Sicherheitswarnungen können nicht bearbeitet werden |
| Microsoft.Sql/servers/databases/securityMetrics/* | Kann keine Sicherheitsmetriken bearbeiten |
| Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |

## <a name="sql-security-manager"></a>SQL-Sicherheits-Manager
Kann die sicherheitsbezogenen Richtlinien von SQL-Server-Instanzen und SQL-Datenbanken verwalten

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen der Microsoft-Autorisierung |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Verknüpft Ressourcen wie etwa ein Speicherkonto oder eine SQL-Datenbank mit einem Subnetz. |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Sql/servers/auditingPolicies/* | Erstellen und Verwalten von SQL Server-Überwachungsrichtlinien |
| Microsoft.Sql/servers/auditingSettings/* | Erstellen und Verwalten von SQL Server-Überwachungseinstellungen |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Erstellen und Verwalten von Überwachungsrichtlinien von SQL Server-Datenbanken |
| Microsoft.Sql/servers/databases/auditingSettings/* | Erstellen und Verwalten von Überwachungseinstellungen von SQL Server-Datenbanken |
| Microsoft.Sql/servers/databases/auditRecords/read | Lesen von Überwachungsdatensätzen |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Erstellen und Verwalten von Verbindungsrichtlinien von SQL Server-Datenbanken |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Erstellen und Verwalten von Datenmaskierungsrichtlinien von SQL Server-Datenbanken |
| Microsoft.Sql/servers/databases/read | Lesen von SQL-Datenbanken |
| Microsoft.Sql/servers/databases/schemas/read | Lesen von Datenbankschemas von SQL-Servern |
| Microsoft.Sql/servers/databases/schemas/tables/columns/read | Lesen von Spalten der Datenbanktabellen von SQL-Servern |
| Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/schemas/tables/read | Lesen von Datenbanktabellen von SQL-Servern |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Erstellen und Verwalten von Richtlinien für Sicherheitswarnungen von SQL Server-Datenbanken |
| Microsoft.Sql/servers/databases/securityMetrics/* | Erstellen und Verwalten von Sicherheitsmetriken von SQL Server-Datenbanken |
| Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
| Microsoft.Sql/servers/firewallRules/* |  |
| Microsoft.Sql/servers/read | Lesen von SQL Server |
| Microsoft.Sql/servers/securityAlertPolicies/* | Erstellen und Verwalten von Richtlinien für Sicherheitswarnungen von SQL Server |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="sql-server-contributor"></a>Mitwirkender von SQL Server
Kann SQL-Server und SQL-Datenbanken verwalten, jedoch nicht die zugehörigen sicherheitsbezogenen Richtlinien

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Sql/locations/*/read |  |
| Microsoft.Sql/servers/* | Erstellen und Verwalten von SQL-Servern |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

| **NotActions** |  |
| --- | --- |
| Microsoft.Sql/servers/auditingPolicies/* | Kann keine SQL Server-Überwachungsrichtlinien bearbeiten |
| Microsoft.Sql/servers/auditingSettings/* | SQL Server-Überwachungseinstellungen können nicht bearbeitet werden |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Kann keine Überwachungsrichtlinien von SQL Server-Datenbanken bearbeiten |
| Microsoft.Sql/servers/databases/auditingSettings/* | Überwachungseinstellungen von SQL Server-Datenbanken können nicht bearbeitet werden |
| Microsoft.Sql/servers/databases/auditRecords/read | Kann keine Überwachungsdatensätze lesen |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Kann keine Verbindungsrichtlinien von SQL Server-Datenbanken bearbeiten |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Kann keine Datenmaskierungsrichtlinien von SQL Server-Datenbanken bearbeiten |
| Microsoft.Sql/servers/databases/extendedAuditingSettings/* |  |
| Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Richtlinien für Sicherheitswarnungen von SQL Server-Datenbanken können nicht bearbeitet werden |
| Microsoft.Sql/servers/databases/securityMetrics/* | Kann keine Sicherheitsmetriken von SQL Server-Datenbanken bearbeiten |
| Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
| Microsoft.Sql/servers/extendedAuditingSettings/* |  |
| Microsoft.Sql/servers/securityAlertPolicies/* | Richtlinien für Sicherheitswarnungen von SQL Server können nicht bearbeitet werden |

## <a name="storage-account-contributor"></a>Mitwirkender von Speicherkonto
Kann Speicherkonten verwalten, aber nicht den Zugriff darauf

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen aller Autorisierungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.Insights/diagnosticSettings/* | Verwalten der Diagnoseeinstellungen |
| Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Verknüpft Ressourcen wie etwa ein Speicherkonto oder eine SQL-Datenbank mit einem Subnetz. |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Storage/storageAccounts/* | Erstellen und Verwalten von Speicherkonten |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="storage-account-key-operator-service-role"></a>Dienstrolle „Speicherkonto-Schlüsseloperator“
Speicherkonto-Schlüsseloperatoren dürfen Schlüssel für Speicherkonten auflisten und neu generieren.

| **Aktionen** |  |
| --- | --- |
| Microsoft.Storage/storageAccounts/listkeys/action | Gibt die Zugriffsschlüssel für das angegebene Speicherkonto zurück. |
| Microsoft.Storage/storageAccounts/regeneratekey/action | Generiert die Zugriffsschlüssel für das angegebene Speicherkonto neu. |

## <a name="support-request-contributor"></a>Mitwirkender für Supportanfragen
Kann Supporttickets im Abonnementbereich erstellen und verwalten

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Autorisierungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="traffic-manager-contributor"></a>Traffic Manager-Mitwirkender
Ermöglicht Ihnen die Verwaltung von Traffic Manager-Profilen, aber nicht die Steuerung des Zugriffs darauf.

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.Network/trafficManagerProfiles/* |  |
| Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="user-access-administrator"></a>Benutzerzugriffsadministrator
Kann den Benutzerzugriff auf Azure-Ressourcen verwalten

| **Aktionen** |  |
| --- | --- |
| */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
| Microsoft.Authorization/* | Verwalten der Autorisierung |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="virtual-machine-administrator-login"></a>VM-Administratoranmeldung
-   Benutzer mit dieser Rolle haben die Möglichkeit, sich bei einem virtuellen Computer mit Windows-Administrator- oder Linux-Root-Benutzerrechten anzumelden.

| **Aktionen** |  |
| --- | --- |
| Microsoft.Compute/virtualMachines/loginAsAdmin/action |  |
| Microsoft.Compute/virtualMachines/login/action |  |
| Microsoft.Compute/virtualMachine/loginAsAdmin/action |  |
| Microsoft.Compute/virtualMachine/logon/action |  |

## <a name="virtual-machine-contributor"></a>Mitwirkender von virtuellen Computern
Kann virtuelle Computer verwalten, jedoch nicht das virtuelle Netzwerk oder Speicherkonto, mit dem sie verbunden sind

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Autorisierungen |
| Microsoft.Compute/availabilitySets/* | Erstellen und Verwalten von Compute-Verfügbarkeitsgruppen |
| Microsoft.Compute/locations/* | Erstellen und Verwalten von Compute-Speicherorten |
| Microsoft.Compute/virtualMachines/* | Erstellen und Verwalten von virtuellen Computern |
| Microsoft.Compute/virtualMachineScaleSets | Erstellen und Verwalten von Skalierungsgruppen für virtuelle Computer |
| Microsoft.DevTestLab/schedules/* |  |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.Network/applicationGateways/backendAddressPools/join/action | Verknüpfen von Back-End-Adresspools für Netzwerkanwendungsgateway |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Verknüpfen von Back-End-Adresspools für Load Balancer |
| Microsoft.Network/loadBalancers/inboundNatPools/join/action | Verknüpfen von eingehenden NAT-Pools für Load Balancer |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Verknüpfen von eingehenden NAT-Regeln für Load Balancer |
| Microsoft.Network/loadBalancers/probes/join/action | Ermöglicht die Verwendung von Prüfpunkten eines Lastenausgleichs. Beispielsweise kann mit dieser Berechtigung die healthProbe-Eigenschaft einer VM-Skalierungsgruppe auf den Prüfpunkt verweisen. |
| Microsoft.Network/loadBalancers/read | Lesen von Load Balancern |
| Microsoft.Network/locations/* | Erstellen und Verwalten von Netzwerkspeicherorten |
| Microsoft.Network/networkInterfaces/* | Erstellen und Verwalten von Netzwerkschnittstellen |
| Microsoft.Network/networkSecurityGroups/join/action | Verknüpfen von Netzwerksicherheitsgruppen |
| Microsoft.Network/networkSecurityGroups/read | Lesen von Netzwerksicherheitsgruppen |
| Microsoft.Network/publicIPAddresses/join/action | Verknüpfen von öffentlichen IP-Adressen im Netzwerk |
| Microsoft.Network/publicIPAddresses/read | Lesen von öffentlichen IP-Adressen im Netzwerk |
| Microsoft.Network/virtualNetworks/read | Lesen von virtuellen Netzwerken |
| Microsoft.Network/virtualNetworks/subnets/join/action | Verknüpfen von Subnetzen in virtuellen Netzwerken |
| Microsoft.RecoveryServices/locations/* |  |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/*/read |  |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/read | Gibt Objektdetails des geschützten Elements zurück. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/write | Dient zum Erstellen eines geschützten Elements für die Sicherung. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | Erstellt einen beabsichtigten Sicherungsschutz. |
| Microsoft.RecoveryServices/Vaults/backupPolicies/read | Gibt alle Schutzrichtlinien zurück. |
| Microsoft.RecoveryServices/Vaults/backupPolicies/write | Erstellt Schutzrichtlinien. |
| Microsoft.RecoveryServices/Vaults/read | Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
| Microsoft.RecoveryServices/Vaults/usages/read | Gibt Nutzungsdetails für einen Recovery Services-Tresor zurück. |
| Microsoft.RecoveryServices/Vaults/write | Der Vorgang „Tresor erstellen“ erstellt eine Azure-Ressource vom Typ „Tresor“. |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Storage/storageAccounts/listKeys/action | Auflisten von Speicherkontoschlüsseln |
| Microsoft.Storage/storageAccounts/read | Lesen von Speicherkonten |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="virtual-machine-user-login"></a>VM-Benutzeranmeldung
Benutzer mit dieser Rolle haben die Möglichkeit, sich als normaler Benutzer an einem virtuellen Computer anzumelden.

| **Aktionen** |  |
| --- | --- |
| Microsoft.Compute/virtualMachines/login/action |  |
| Microsoft.Compute/virtualMachine/logon/action |  |

## <a name="web-plan-contributor"></a>Mitwirkender von Webplan
Kann Webpläne verwalten

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Autorisierungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |
| Microsoft.Web/serverFarms/* | Erstellen und Verwalten von Serverfarmen |

## <a name="website-contributor"></a>Mitwirkender von Website
Kann Websites verwalten, jedoch nicht die Webpläne, mit denen sie verbunden sind

| **Aktionen** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lesen von Autorisierungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.Insights/components/* | Erstellen und Verwalten von Insights-Komponenten |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |
| Microsoft.Web/certificates/* | Erstellen und Verwalten von Websitezertifikaten |
| Microsoft.Web/listSitesAssignedToHostName/read | Lesen von Websites, die einem Hostnamen zugewiesen sind |
| Microsoft.Web/serverFarms/join/action | Verknüpfen von Serverfarmen |
| Microsoft.Web/serverFarms/read | Lesen von Serverfarmen |
| Microsoft.Web/sites/* | Erstellen und Verwalten von Websites (die Erstellung von Websites erfordert auch Schreibberechtigungen für den zugehörigen App Service-Plan) |

## <a name="see-also"></a>Weitere Informationen
* [Rollenbasierte Zugriffssteuerung](role-based-access-control-configure.md): Erste Schritte mit RBAC im Azure-Portal.
* [Benutzerdefinierte Rollen in Azure RBAC](role-based-access-control-custom-roles.md): Erfahren Sie, wie Sie benutzerdefinierte Rollen entsprechend Ihren Zugriffsanforderungen erstellen.
* [Erstellen eines Verlaufsberichts zu Zugriffsänderungen:](role-based-access-control-access-change-history-report.md)Verfolgen Sie Änderungen an Rollenzuweisungen in RBAC.
* [Problembehandlung bei rollenbasierter Zugriffssteuerung:](role-based-access-control-troubleshooting.md)Sehen Sie sich Vorschläge zur Behebung häufig auftretender Probleme an.
* [Berechtigungen in Azure Security Center](../security-center/security-center-permissions.md)
