---
title: Integrierte Rollen für die rollenbasierte Zugriffssteuerung (RBAC) in Azure | Microsoft-Dokumentation
description: In diesem Artikel werden die integrierten Rollen der rollenbasierten Zugriffssteuerung (RBAC) beschrieben. Zudem werden actions- und notActions-Abschnitte aufgeführt.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 05/11/2018
ms.author: rolyon
ms.reviewer: rqureshi
ms.custom: it-pro
ms.openlocfilehash: 91f721f5508191c7530e57b6dd96cad3301542a7
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34203504"
---
# <a name="built-in-roles-for-azure-role-based-access-control"></a>Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure
Die [rollenbasierte Zugriffssteuerung (RBAC)](overview.md) verfügt über mehrere integrierte Rollendefinitionen, die Sie Benutzern, Gruppen und Dienstprinzipalen zuweisen können. Durch Rollenzuweisungen wird die Art und Weise gesteuert, wie Sie auf Ressourcen in Azure zugreifen. Die integrierten Rollen können nicht geändert werden, es können jedoch eigene [benutzerdefinierte Rollen](custom-roles.md) je nach den spezifischen Anforderungen Ihrer Organisation erstellt werden.

Die integrierten Rollen werden stetig weiterentwickelt. Verwenden Sie zum Abrufen der neuesten Rollendefinitionen [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) oder [az role definition list](/cli/azure/role/definition#az-role-definition-list).

## <a name="built-in-role-descriptions"></a>Beschreibungen der integrierten Rollen
Die folgende Tabelle enthält kurze Beschreibungen der integrierten Rollen. Klicken Sie auf den Rollennamen, um die Liste von `actions` und `notActions` für jede Rolle anzuzeigen.


| Integrierte Rolle | BESCHREIBUNG |
| --- | --- |
| [Besitzer](#owner) | Ermöglicht Ihnen das Verwalten aller Komponenten einschließlich des Zugriffs auf Ressourcen. |
| [Mitwirkender](#contributor) | Ermöglicht Ihnen das Verwalten aller Komponenten außer des Zugriffs auf Ressourcen. |
| [Leser](#reader) | Sie können alles anzeigen, aber keine Änderungen vornehmen. |
| [AcrImageSigner](#acrimagesigner) | ACR-Imagesignaturgeber |
| [AcrQuarantineReader](#acrquarantinereader) | ACR-Quarantänedatenleser |
| [AcrQuarantineWriter](#acrquarantinewriter) | ACR-Quarantänedatenschreiber |
| [Mitwirkender des API-Verwaltungsdienstes](#api-management-service-contributor) | Kann Dienst und APIs verwalten. |
| [Operatorrolle des API Management-Diensts](#api-management-service-operator-role) | Kann den Dienst, aber nicht die APIs verwalten. |
| [Leserrolle des API Management-Diensts](#api-management-service-reader-role) | Schreibgeschützter Zugriff auf Dienst und APIs |
| [Mitwirkender der Application Insights-Komponente](#application-insights-component-contributor) | Kann Application Insights-Komponenten verwalten |
| [Application Insights-Momentaufnahmedebugger](#application-insights-snapshot-debugger) | Erteilt dem Benutzer die Berechtigung zum Verwenden von Features des Momentaufnahmedebuggers in Application Insights. |
| [Automation-Auftragsoperator](#automation-job-operator) | Hiermit werden Aufträge mithilfe von Automation-Runbooks erstellt und verwaltet. |
| [Operator für Automation](#automation-operator) | Automatisierungsoperatoren können Aufträge starten, beenden, anhalten und fortsetzen. |
| [Automation-Runbookoperator](#automation-runbook-operator) | Runbookeigenschaften lesen: Ermöglicht das Erstellen von Runbookaufträgen. |
| [Besitzer der Azure Stack-Registrierung](#azure-stack-registration-owner) | Ermöglicht Ihnen die Verwaltung von Azure Stack-Registrierungen. |
| [Mitwirkender für Sicherungen](#backup-contributor) | Ermöglicht Ihnen das Verwalten des Sicherungsdiensts, jedoch nicht das Erstellen von Tresoren und das Erteilen des Zugriffs an andere Benutzer. |
| [Sicherungsoperator](#backup-operator) | Ermöglicht Ihnen das Verwalten von Sicherungsdiensten, jedoch nicht das Entfernen der Sicherung, die Tresorerstellung und das Erteilen von Zugriff an andere Benutzer. |
| [Sicherungsleser](#backup-reader) | Kann Sicherungsdienste anzeigen, aber keine Änderungen vornehmen. |
| [Abrechnungsleser](#billing-reader) | Hiermit wird Lesezugriff auf Abrechnungsdaten ermöglicht. |
| [Mitwirkender von BizTalk](#biztalk-contributor) | Ermöglicht Ihnen das Verwalten von BizTalk-Diensten, nicht aber den Zugriff darauf. |
| [Mitwirkender für den CDN-Endpunkt](#cdn-endpoint-contributor) | Kann CDN-Endpunkte verwalten, aber anderen Benutzern keinen Zugriff erteilen. |
| [CDN-Endpunktleser](#cdn-endpoint-reader) | Kann CDN-Endpunkte anzeigen, aber keine Änderungen vornehmen. |
| [Mitwirkender für das CDN-Profil](#cdn-profile-contributor) | Kann CDN-Profile und deren Endpunkte verwalten, aber anderen Benutzern keinen Zugriff erteilen. |
| [CDN-Profilleser](#cdn-profile-reader) | Kann CDN-Profile und deren Endpunkte anzeigen, aber keine Änderungen vornehmen. |
| [Mitwirkender von klassischem Netzwerk](#classic-network-contributor) | Ermöglicht Ihnen das Verwalten von klassischen Netzwerken, nicht aber den Zugriff darauf. |
| [Mitwirkender von klassischem Speicherkonto](#classic-storage-account-contributor) | Ermöglicht Ihnen das Verwalten klassischer Speicherkonten, nicht aber den Zugriff darauf. |
| [Klassische Dienstrolle „Speicherkonto-Schlüsseloperator“](#classic-storage-account-key-operator-service-role) | Klassische Speicherkonto-Schlüsseloperatoren dürfen Schlüssel für klassische Speicherkonten auflisten und neu generieren. |
| [Mitwirkender von klassischen virtuellen Computern](#classic-virtual-machine-contributor) | Ermöglicht Ihnen das Verwalten klassischer virtueller Computer, aber weder den Zugriff darauf noch auf die mit ihnen verbundenen virtuellen Netzwerke oder Speicherkonten. |
| [Mitwirkender von ClearDB-MySQL-DB](#cleardb-mysql-db-contributor) | Ermöglicht Ihnen das Verwalten von ClearDB MySQL-Datenbanken, nicht aber den Zugriff darauf. |
| [Cosmos DB-Rolle „Kontoleser“](#cosmos-db-account-reader-role) | Kann Azure Cosmos DB-Kontodaten lesen. Informationen zum Verwalten von Azure Cosmos DB-Konten finden Sie unter [Mitwirkender von DocumentDB-Konto](#documentdb-account-contributor). |
| [Mitwirkender von Data Factory](#data-factory-contributor) | Erstellen und verwalten Sie Data Factorys sowie die darin enthaltenen untergeordneten Ressourcen. |
| [Data Lake Analytics-Entwickler](#data-lake-analytics-developer) | Ermöglicht Ihnen das Übermitteln, Überwachen und Verwalten Ihrer eigenen Aufträge, aber nicht das Erstellen oder Löschen von Data Lake Analytics-Konten. |
| [Datenpurger](#data-purger) | Kann Analysedaten endgültig löschen. |
| [DevTest Labs-Benutzer](#devtest-labs-user) | Ermöglicht Ihnen das Verbinden, Starten, Neustarten und Herunterfahren Ihrer virtuellen Computer in Ihren Azure DevTest Labs. |
| [DNS Zone Contributor](#dns-zone-contributor) | Ermöglicht Ihnen die Verwaltung von DNS-Zonen und Ressourceneintragssätzen in Azure DNS, aber nicht zu steuern, wer darauf Zugriff hat. |
| [Mitwirkender von DocumentDB-Konto](#documentdb-account-contributor) | Kann Azure Cosmos DB-Konten verwalten. Azure Cosmos DB wurde früher als DocumentDB bezeichnet. |
| [Mitwirkender von Intelligent Systems-Konto](#intelligent-systems-account-contributor) | Ermöglicht Ihnen das Verwalten von Intelligent Systems-Konten, nicht aber den Zugriff darauf. |
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
| [Mitwirkender von virtuellem Netzwerk](#network-contributor) | Ermöglicht Ihnen das Verwalten von Netzwerken, nicht aber den Zugriff darauf. |
| [Mitwirkender von New Relic APM-Konto](#new-relic-apm-account-contributor) | Ermöglicht Ihnen das Verwalten von New Relic Application Performance Management-Konten und -Anwendungen, nicht aber den Zugriff auf diese. |
| [Lese- und Datenzugriff](#reader-and-data-access) | Ermöglicht Ihnen das Anzeigen sämtlicher Aspekte, jedoch nicht das Löschen oder Erstellen eines Speicherkontos oder einer darin enthaltenen Ressource. Sie können auch Lese-/Schreibzugriff auf alle Daten in einem Speicherkonto durch Zugriff auf Speicherkontoschlüssel gewähren. |
| [Mitwirkender von Redis-Cache](#redis-cache-contributor) | Ermöglicht Ihnen das Verwalten von Redis Caches, nicht aber den Zugriff darauf. |
| [Mitwirkender von Zeitplanungsauftragssammlung](#scheduler-job-collections-contributor) | Ermöglicht Ihnen das Verwalten von Scheduler-Auftragssammlungen, nicht aber den Zugriff darauf. |
| [Mitwirkender von Suchdienst](#search-service-contributor) | Ermöglicht Ihnen das Verwalten von Search-Diensten, nicht aber den Zugriff darauf. |
| [Sicherheitsadministrator](#security-admin) | Nur in Security Center: Kann Sicherheitsrichtlinien und -zustände anzeigen, Sicherheitsrichtlinien bearbeiten sowie Warnungen und Empfehlungen anzeigen und verwerfen |
| [Sicherheits-Manager (Legacy)](#security-manager-legacy) | Dies ist eine Legacyrolle. Verwenden Sie stattdessen „Sicherheitsadministrator“. |
| [Benutzer mit Leseberechtigung für Sicherheitsfunktionen](#security-reader) | Nur in Security Center: Kann Empfehlungen und Warnungen sowie Sicherheitsrichtlinien und -zustände anzeigen, aber keine Änderungen vornehmen |
| [Site Recovery-Mitwirkender](#site-recovery-contributor) | Ermöglicht Ihnen die Verwaltung des Site Recovery-Diensts mit Ausnahme der Tresorerstellung und der Rollenzuweisung. |
| [Site Recovery-Operator](#site-recovery-operator) | Ermöglicht Ihnen ein Failover und ein Failback, aber nicht das Durchführen weiterer Site Recovery-Verwaltungsvorgänge. |
| [Site Recovery-Leser](#site-recovery-reader) | Ermöglicht Ihnen die Anzeige des Site Recovery-Status, aber nicht die Durchführung weiterer Verwaltungsvorgänge. |
| [Mitwirkender von SQL DB](#sql-db-contributor) | Ermöglicht Ihnen das Verwalten von SQL-Datenbanken, nicht aber den Zugriff darauf. Darüber hinaus können Sie deren sicherheitsbezogenen Richtlinien oder übergeordneten SQL-Server nicht verwalten. |
| [SQL-Sicherheits-Manager](#sql-security-manager) | Ermöglicht Ihnen das Verwalten von sicherheitsbezogenen Richtlinien von SQL-Server und Datenbanken, jedoch nicht den Zugriff darauf. |
| [Mitwirkender von SQL Server](#sql-server-contributor) | Ermöglicht Ihnen, SQL-Server und -Datenbanken zu verwalten, gewährt Ihnen jedoch keinen Zugriff darauf und auch nicht auf deren sicherheitsbezogenen Richtlinien. |
| [Mitwirkender von Speicherkonto](#storage-account-contributor) | Ermöglicht Ihnen das Verwalten von Speicherkonten, nicht aber den Zugriff darauf. |
| [Dienstrolle „Speicherkonto-Schlüsseloperator“](#storage-account-key-operator-service-role) | Speicherkonto-Schlüsseloperatoren dürfen Schlüssel für Speicherkonten auflisten und neu generieren. |
| [Mitwirkender für Supportanfragen](#support-request-contributor) | Ermöglicht Ihnen die Erstellung und Verwaltung von Supportanfragen. |
| [Traffic Manager-Mitwirkender](#traffic-manager-contributor) | Ermöglicht Ihnen die Verwaltung von Traffic Manager-Profilen, aber nicht die Steuerung des Zugriffs darauf. |
| [Benutzerzugriffsadministrator](#user-access-administrator) | Ermöglicht Ihnen die Verwaltung von Benutzerzugriffen auf Azure-Ressourcen. |
| [VM-Administratoranmeldung](#virtual-machine-administrator-login) | Benutzer mit dieser Rolle haben die Möglichkeit, sich bei einem virtuellen Computer mit Windows-Administrator- oder Linux-Root-Benutzerrechten anzumelden. |
| [Mitwirkender von virtuellen Computern](#virtual-machine-contributor) | Ermöglicht Ihnen das Verwalten virtueller Computer, aber weder den Zugriff darauf, noch auf deren verbundenen virtuellen Netzwerke oder Speicherkonten. |
| [VM-Benutzeranmeldung](#virtual-machine-user-login) | Benutzer mit dieser Rolle haben die Möglichkeit, sich als normaler Benutzer an einem virtuellen Computer anzumelden. |
| [Mitwirkender von Webplan](#web-plan-contributor) | Ermöglicht Ihnen das Verwalten der Webpläne für Websites, nicht aber den Zugriff darauf. |
| [Mitwirkender von Website](#website-contributor) | Ermöglicht Ihnen das Verwalten von Websites (nicht der Webpläne), nicht aber den Zugriff darauf. |


## <a name="owner"></a>Owner (Besitzer)
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen das Verwalten aller Komponenten einschließlich des Zugriffs auf Ressourcen. |
> | **Id** | 8e3af657-a8ff-443c-a75c-2fe8c4bcb635 |
> | **Aktionen** |  |
> | * | Erstellen und Verwalten von Ressourcen aller Typen |

## <a name="contributor"></a>Mitwirkender
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen das Verwalten aller Komponenten außer des Zugriffs auf Ressourcen. |
> | **Id** | b24988ac-6180-42a0-ab88-20f7382dd24c |
> | **Aktionen** |  |
> | * | Erstellen und Verwalten von Ressourcen aller Typen |
> | **NotActions** |  |
> | Microsoft.Authorization/*/Delete | Rollen und Rollenzuweisungen können nicht gelöscht werden. |
> | Microsoft.Authorization/*/Write | Rollen und Rollenzuweisungen können nicht erstellt werden. |
> | Microsoft.Authorization/elevateAccess/Action | Gewährt dem Aufrufer Zugriff vom Typ „Benutzerzugriffsadministrator“ auf der Mandantenebene. |

## <a name="reader"></a>Leser
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Sie können alles anzeigen, aber keine Änderungen vornehmen. |
> | **Id** | acdd72a7-3385-48ef-bd42-f606fba81ae7 |
> | **Aktionen** |  |
> | */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |

## <a name="acrimagesigner"></a>AcrImageSigner
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | ACR-Imagesignaturgeber |
> | **Id** | 6cef56e8-d556-48e5-a04f-b8e64114680f |
> | **Aktionen** |  |
> | Microsoft.ContainerRegistry/registries/*/read |  |
> | Microsoft.ContainerRegistry/registries/*/write |  |

## <a name="acrquarantinereader"></a>AcrQuarantineReader
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | ACR-Quarantänedatenleser |
> | **Id** | cdda3590-29a3-44f6-95f2-9f980659eb04 |
> | **Aktionen** |  |
> | Microsoft.ContainerRegistry/registries/*/read |  |

## <a name="acrquarantinewriter"></a>AcrQuarantineWriter
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | ACR-Quarantänedatenschreiber |
> | **Id** | c8d4ff99-41c3-41a8-9f60-21dfdad59608 |
> | **Aktionen** |  |
> | Microsoft.ContainerRegistry/registries/*/write |  |
> | Microsoft.ContainerRegistry/registries/*/read |  |

## <a name="api-management-service-contributor"></a>Mitwirkender des API-Verwaltungsdienstes
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Kann Dienst und APIs verwalten. |
> | **Id** | 312a565d-c81f-4fd8-895a-4e21e48d571c |
> | **Aktionen** |  |
> | Microsoft.ApiManagement/service/* | Erstellen und Verwalten des API Management-Diensts |
> | Microsoft.Authorization/*/read | Lesen von Autorisierungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="api-management-service-operator-role"></a>Operatorrolle des API Management-Diensts
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Kann den Dienst, aber nicht die APIs verwalten. |
> | **Id** | e022efe7-f5ba-4159-bbe4-b44f577e9b61 |
> | **Aktionen** |  |
> | Microsoft.ApiManagement/service/*/read | Dient zum Lesen von API Management-Dienstinstanzen. |
> | Microsoft.ApiManagement/service/backup/action | Dient zum Sichern des API Management-Diensts im angegebenen Container in einem vom Benutzer bereitgestellten Speicherkonto. |
> | Microsoft.ApiManagement/service/delete | Dient zum Löschen einer API Management-Dienstinstanz. |
> | Microsoft.ApiManagement/service/managedeployments/action | Dient zum Ändern der SKU/Einheiten sowie zum Hinzufügen/Entfernen regionaler Bereitstellungen des API Management-Diensts. |
> | Microsoft.ApiManagement/service/read | Dient zum Lesen der Metadaten für eine API Management-Dienstinstanz. |
> | Microsoft.ApiManagement/service/restore/action | Dient zum Wiederherstellen des API Management-Diensts aus dem angegebenen Container in einem vom Benutzer bereitgestellten Speicherkonto. |
> | Microsoft.ApiManagement/service/updatecertificate/action | Dient zum Hochladen eines SSL-Zertifikats für einen API Management-Dienst. |
> | Microsoft.ApiManagement/service/updatehostname/action | Dient zum Einrichten, Aktualisieren oder Entfernen benutzerdefinierter Domänennamen für einen API Management-Dienst. |
> | Microsoft.ApiManagement/service/write | Dient zum Erstellen einer neuen Instanz des API Management-Diensts. |
> | Microsoft.Authorization/*/read | Lesen von Autorisierungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |
> | **NotActions** |  |
> | Microsoft.ApiManagement/service/users/keys/read | Dient zum Abrufen einer Liste mit Benutzerschlüsseln. |

## <a name="api-management-service-reader-role"></a>Leserrolle des API Management-Diensts
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Schreibgeschützter Zugriff auf Dienst und APIs |
> | **Id** | 71522526-b88f-4d52-b57f-d31fc3546d0d |
> | **Aktionen** |  |
> | Microsoft.ApiManagement/service/*/read | Dient zum Lesen von API Management-Dienstinstanzen. |
> | Microsoft.ApiManagement/service/read | Dient zum Lesen der Metadaten für eine API Management-Dienstinstanz. |
> | Microsoft.Authorization/*/read | Lesen von Autorisierungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |
> | **NotActions** |  |
> | Microsoft.ApiManagement/service/users/keys/read | Dient zum Abrufen einer Liste mit Benutzerschlüsseln. |

## <a name="application-insights-component-contributor"></a>Mitwirkender der Application Insights-Komponente
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Kann Application Insights-Komponenten verwalten |
> | **Id** | ae349356-3a1b-4a5e-921d-050484c6347e |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
> | Microsoft.Insights/components/* | Erstellen und Verwalten von Insights-Komponenten |
> | Microsoft.Insights/webtests/* | Erstellen und Verwalten von Webtests |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="application-insights-snapshot-debugger"></a>Application Insights-Momentaufnahmedebugger
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Erteilt dem Benutzer die Berechtigung zum Verwenden von Features des Momentaufnahmedebuggers in Application Insights. |
> | **Id** | 08954f03-6346-4c2e-81c0-ec3a5cfae23b |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
> | Microsoft.Insights/components/*/read |  |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="automation-job-operator"></a>Automation-Auftragsoperator
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Hiermit werden Aufträge mithilfe von Automation-Runbooks erstellt und verwaltet. |
> | **Id** | 4fe576fe-1146-4730-92eb-48519fa6bf9f |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Automation/automationAccounts/jobs/read | Ruft einen Azure Automation-Auftrag ab. |
> | Microsoft.Automation/automationAccounts/jobs/resume/action | Setzt einen Azure Automation-Auftrag fort. |
> | Microsoft.Automation/automationAccounts/jobs/stop/action | Beendet einen Azure Automation-Auftrag. |
> | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Liest Hybrid Runbook Worker-Ressourcen. |
> | Microsoft.Automation/automationAccounts/jobs/streams/read | Ruft einen Azure Automation-Auftragsdatenstrom ab. |
> | Microsoft.Automation/automationAccounts/jobs/suspend/action | Hält einen Azure Automation-Auftrag an. |
> | Microsoft.Automation/automationAccounts/jobs/write | Erstellt einen Azure Automation-Auftrag. |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="automation-operator"></a>Operator für Automation
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Automatisierungsoperatoren können Aufträge starten, beenden, anhalten und fortsetzen. |
> | **Id** | d3881f73-407a-4167-8283-e981cbba0404 |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Liest Hybrid Runbook Worker-Ressourcen. |
> | Microsoft.Automation/automationAccounts/jobs/read | Ruft einen Azure Automation-Auftrag ab. |
> | Microsoft.Automation/automationAccounts/jobs/resume/action | Setzt einen Azure Automation-Auftrag fort. |
> | Microsoft.Automation/automationAccounts/jobs/stop/action | Beendet einen Azure Automation-Auftrag. |
> | Microsoft.Automation/automationAccounts/jobs/streams/read | Ruft einen Azure Automation-Auftragsdatenstrom ab. |
> | Microsoft.Automation/automationAccounts/jobs/suspend/action | Hält einen Azure Automation-Auftrag an. |
> | Microsoft.Automation/automationAccounts/jobs/write | Erstellt einen Azure Automation-Auftrag. |
> | Microsoft.Automation/automationAccounts/jobSchedules/read | Ruft einen Azure Automation-Auftragszeitplan ab. |
> | Microsoft.Automation/automationAccounts/jobSchedules/write | Erstellt einen Azure Automation-Auftragszeitplan. |
> | Microsoft.Automation/automationAccounts/linkedWorkspace/read | Ruft den Arbeitsbereich ab, der mit dem Automation-Konto verknüpft ist. |
> | Microsoft.Automation/automationAccounts/read | Ruft ein Azure Automation-Konto ab. |
> | Microsoft.Automation/automationAccounts/runbooks/read | Ruft ein Azure Automation-Runbook ab. |
> | Microsoft.Automation/automationAccounts/schedules/read | Ruft ein Azure Automation-Zeitplanasset ab. |
> | Microsoft.Automation/automationAccounts/schedules/write | Erstellt oder aktualisiert ein Azure Automation-Zeitplanasset. |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Automation/automationAccounts/jobs/output/read | Ruft die Ausgabe eines Auftrags ab. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="automation-runbook-operator"></a>Automation-Runbookoperator
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Runbookeigenschaften lesen: Ermöglicht das Erstellen von Runbookaufträgen. |
> | **Id** | 5fb5aef8-1081-4b8e-bb16-9d5d0385bab5 |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Automation/automationAccounts/runbooks/read | Ruft ein Azure Automation-Runbook ab. |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="azure-stack-registration-owner"></a>Besitzer der Azure Stack-Registrierung
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen die Verwaltung von Azure Stack-Registrierungen. |
> | **Id** | 6f12a6df-dd06-4f3e-bcb1-ce8be600526a |
> | **Aktionen** |  |
> | Microsoft.AzureStack/registrations/products/listDetails/action | Ruft erweiterte Details für ein Azure Stack-Marketplace-Produkt ab. |
> | Microsoft.AzureStack/registrations/products/read | Ruft die Eigenschaften eines Azure Stack-Marketplace-Produkts ab. |
> | Microsoft.AzureStack/registrations/read | Ruft die Eigenschaften einer Azure Stack-Registrierung ab. |

## <a name="backup-contributor"></a>Mitwirkender für Sicherungen
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen das Verwalten des Sicherungsdiensts, jedoch nicht das Erstellen von Tresoren und das Erteilen des Zugriffs an andere Benutzer. |
> | **Id** | 5e467623-bb1f-42f4-a55d-6e525e11384b |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Network/virtualNetworks/read | Dient zum Abrufen der Definition des virtuellen Netzwerks. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/* | Verwalten der Ergebnisse eines Vorgangs in der Sicherungsverwaltung |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/* | Erstellen und Verwalten von Sicherungscontainern in Sicherungsfabrics des Recovery Services-Tresors |
> | Microsoft.RecoveryServices/Vaults/backupJobs/* | Erstellen und Verwalten von Sicherungsaufträgen |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Dient zum Exportieren von Aufträgen. |
> | Microsoft.RecoveryServices/Vaults/backupManagementMetaData/* | Erstellen und Verwalten von Metadaten in Zusammenhang mit der Sicherungsverwaltung |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Erstellen und Verwalten der Ergebnisse von Sicherungsverwaltungsvorgängen |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/* | Erstellen und Verwalten von Sicherungsrichtlinien |
> | Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Erstellen und Verwalten von Elementen, die gesichert werden können |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/* | Erstellen und Verwalten von gesicherten Elementen |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/* | Erstellen und Verwalten von Containern mit Sicherungselementen |
> | Microsoft.RecoveryServices/Vaults/certificates/* | Erstellen und Verwalten von Zertifikaten in Zusammenhang mit Sicherungen in einem Recovery Services-Tresor |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/* | Erstellen und Verwalten erweiterter Informationen in Zusammenhang mit einem Tresor |
> | Microsoft.RecoveryServices/Vaults/read | Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/* | Verwalten von Ermittlungsvorgängen zum Abrufen neu erstellter Container |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Erstellen und Verwalten von registrierten Identitäten |
> | Microsoft.RecoveryServices/Vaults/usages/* | Erstellen und Verwalten der Nutzung des Recovery Services-Tresors |
> | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Gibt Zusammenfassungen für geschützte Elemente und geschützte Server für einen Recovery Services-Tresor zurück. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Storage/storageAccounts/read | Gibt die Liste mit Speicherkonten zurück oder ruft die Eigenschaften für das angegebene Speicherkonto ab. |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | „GetAllocatedStamp“ ist ein interner Vorgang des Diensts. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Ruft die Warnungen für den Recovery Services-Tresor ab. |
> | Microsoft.RecoveryServices/Vaults/storageConfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Gibt das Ergebnis des Vorgangs „Auftrag exportieren“ zurück. |
> | Microsoft.RecoveryServices/Vaults/backupSecurityPIN/* |  |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="backup-operator"></a>Sicherungsoperator
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen das Verwalten von Sicherungsdiensten, jedoch nicht das Entfernen der Sicherung, die Tresorerstellung und das Erteilen von Zugriff an andere Benutzer. |
> | **Id** | 00c29273-979b-4161-815c-10b084fb9324 |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Network/virtualNetworks/read | Dient zum Abrufen der Definition des virtuellen Netzwerks. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Gibt den Status des Vorgangs zurück. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Ruft das Ergebnis eines Vorgangs ab, der für den Schutzcontainer ausgeführt wurde. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action | Führt eine Sicherung für geschützte Elemente aus. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Ruft das Ergebnis eines Vorgangs ab, der für geschützte Elemente ausgeführt wurde. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Gibt den Status eines Vorgangs zurück, der für geschützte Elemente ausgeführt wurde. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Gibt Objektdetails des geschützten Elements zurück. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Dient zum Abrufen von Wiederherstellungspunkten für geschützte Elemente. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action | Dient zum Wiederherstellen von Wiederherstellungspunkten für geschützte Elemente. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Dient zum Erstellen eines geschützten Elements für die Sicherung. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Gibt alle registrierten Container zurück. |
> | Microsoft.RecoveryServices/Vaults/backupJobs/* | Erstellen und Verwalten von Sicherungsaufträgen |
> | Microsoft.RecoveryServices/Vaults/backupJobs/cancel/action | Dient zum Abbrechen des Auftrags. |
> | Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | Gibt das Ergebnis von Auftragsvorgängen zurück. |
> | Microsoft.RecoveryServices/Vaults/backupJobs/read | Gibt alle Auftragsobjekte zurück. |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Dient zum Exportieren von Aufträgen. |
> | Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Gibt Sicherungsverwaltungs-Metadaten für Recovery Services-Tresore zurück. |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Erstellen und Verwalten der Ergebnisse von Sicherungsverwaltungsvorgängen |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Dient zum Abrufen der Ergebnisse von Richtlinienvorgängen. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Gibt alle Schutzrichtlinien zurück. |
> | Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Erstellen und Verwalten von Elementen, die gesichert werden können |
> | Microsoft.RecoveryServices/Vaults/backupProtectableItems/read | Gibt die Liste mit allen schützbaren Elementen zurück. |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Gibt die Liste mit allen geschützten Elementen zurück. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Gibt alle zum Abonnement gehörenden Container zurück. |
> | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Gibt Zusammenfassungen für geschützte Elemente und geschützte Server für einen Recovery Services-Tresor zurück. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/write | Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Microsoft.RecoveryServices/Vaults/read | Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/* | Verwalten von Ermittlungsvorgängen zum Abrufen neu erstellter Container |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Mit dem Vorgang „Vorgangsergebnisse abrufen“ können der Vorgangsstatus und das Ergebnis für den asynchron übermittelten Vorgang abgerufen werden. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Der Vorgang „Container abrufen“ kann zum Abrufen der für eine Ressource registrierten Container verwendet werden. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/write | Der Vorgang „Dienstcontainer registrieren“ kann zum Registrieren eines Containers beim Wiederherstellungsdienst verwendet werden. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Gibt Nutzungsdetails für einen Recovery Services-Tresor zurück. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Storage/storageAccounts/read | Gibt die Liste mit Speicherkonten zurück oder ruft die Eigenschaften für das angegebene Speicherkonto ab. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/provisionInstantItemRecovery/action | Dient zum Bereitstellen der sofortigen Elementwiederherstellung für geschützte Elemente. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/revokeInstantItemRecovery/action | Dient zum Widerrufen der sofortigen Elementwiederherstellung für geschützte Elemente. |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | „GetAllocatedStamp“ ist ein interner Vorgang des Diensts. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Ruft die Warnungen für den Recovery Services-Tresor ab. |
> | Microsoft.RecoveryServices/Vaults/storageConfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Gibt das Ergebnis des Vorgangs „Auftrag exportieren“ zurück. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operationStatus/read |  |
> | Microsoft.RecoveryServices/Vaults/certificates/write | Der Vorgang „Ressourcenzertifikat aktualisieren“ aktualisiert das Zertifikat für die Ressourcen-/Tresoranmeldeinformationen. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="backup-reader"></a>Sicherungsleser
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Kann Sicherungsdienste anzeigen, aber keine Änderungen vornehmen. |
> | **Id** | a795c7a0-d4a2-40c1-ae25-d81f01202912 |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Gibt den Status des Vorgangs zurück. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Ruft das Ergebnis eines Vorgangs ab, der für den Schutzcontainer ausgeführt wurde. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Ruft das Ergebnis eines Vorgangs ab, der für geschützte Elemente ausgeführt wurde. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Gibt den Status eines Vorgangs zurück, der für geschützte Elemente ausgeführt wurde. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Gibt Objektdetails des geschützten Elements zurück. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Gibt alle registrierten Container zurück. |
> | Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | Gibt das Ergebnis von Auftragsvorgängen zurück. |
> | Microsoft.RecoveryServices/Vaults/backupJobs/read | Gibt alle Auftragsobjekte zurück. |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Dient zum Exportieren von Aufträgen. |
> | Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Gibt Sicherungsverwaltungs-Metadaten für Recovery Services-Tresore zurück. |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/read | Gibt das Ergebnis eines Sicherungsvorgangs für Recovery Services-Tresore zurück. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Dient zum Abrufen der Ergebnisse von Richtlinienvorgängen. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Gibt alle Schutzrichtlinien zurück. |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Gibt die Liste mit allen geschützten Elementen zurück. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Gibt alle zum Abonnement gehörenden Container zurück. |
> | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Gibt Zusammenfassungen für geschützte Elemente und geschützte Server für einen Recovery Services-Tresor zurück. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Microsoft.RecoveryServices/Vaults/read | Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Mit dem Vorgang „Vorgangsergebnisse abrufen“ können der Vorgangsstatus und das Ergebnis für den asynchron übermittelten Vorgang abgerufen werden. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Der Vorgang „Container abrufen“ kann zum Abrufen der für eine Ressource registrierten Container verwendet werden. |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | „GetAllocatedStamp“ ist ein interner Vorgang des Diensts. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Ruft die Warnungen für den Recovery Services-Tresor ab. |
> | Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
> | Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/read |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Dient zum Abrufen von Wiederherstellungspunkten für geschützte Elemente. |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Gibt das Ergebnis des Vorgangs „Auftrag exportieren“ zurück. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Gibt Nutzungsdetails für einen Recovery Services-Tresor zurück. |

## <a name="billing-reader"></a>Abrechnungsleser
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Hiermit wird Lesezugriff auf Abrechnungsdaten ermöglicht. |
> | **Id** | fa23ad8b-c56e-40d8-ac0c-ce449e1d2c64 |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Billing/*/read | Lesen von Abrechnungsinformationen |
> | Microsoft.Consumption/*/read |  |
> | Microsoft.Commerce/*/read |  |
> | Microsoft.Management/managementGroups/read | Listet die Verwaltungsgruppen für den authentifizierten Benutzer auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="biztalk-contributor"></a>Mitwirkender von BizTalk
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen das Verwalten von BizTalk-Diensten, nicht aber den Zugriff darauf. |
> | **Id** | 5e3c6656-6cfa-4708-81fe-0de47ac73342 |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.BizTalkServices/BizTalk/* | Erstellen und Verwalten von BizTalk-Diensten |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="cdn-endpoint-contributor"></a>Mitwirkender für den CDN-Endpunkt
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Kann CDN-Endpunkte verwalten, aber anderen Benutzern keinen Zugriff erteilen. |
> | **Id** | 426e0c7f-0c7e-4658-b36f-ff54d6c29b45 |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/endpoints/* |  |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="cdn-endpoint-reader"></a>CDN-Endpunktleser
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Kann CDN-Endpunkte anzeigen, aber keine Änderungen vornehmen. |
> | **Id** | 871e35f6-b5c1-49cc-a043-bde969a0f2cd |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/endpoints/*/read |  |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="cdn-profile-contributor"></a>Mitwirkender für das CDN-Profil
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Kann CDN-Profile und deren Endpunkte verwalten, aber anderen Benutzern keinen Zugriff erteilen. |
> | **Id** | ec156ff8-a8d1-4d15-830c-5b80698ca432 |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/* |  |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="cdn-profile-reader"></a>CDN-Profilleser
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Kann CDN-Profile und deren Endpunkte anzeigen, aber keine Änderungen vornehmen. |
> | **Id** | 8f96442b-4075-438f-813d-ad51ab4019af |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/*/read |  |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="classic-network-contributor"></a>Mitwirkender von klassischem Netzwerk
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen das Verwalten von klassischen Netzwerken, nicht aber den Zugriff darauf. |
> | **Id** | b34d265f-36f7-4a0d-a4d4-e158ca92e90f |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Autorisierungen |
> | Microsoft.ClassicNetwork/* | Erstellen und Verwalten von klassischen Netzwerken |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="classic-storage-account-contributor"></a>Mitwirkender von klassischem Speicherkonto
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen das Verwalten klassischer Speicherkonten, nicht aber den Zugriff darauf. |
> | **Id** | 86e8f5dc-a6e9-4c67-9d15-de283e8eac25 |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Autorisierungen |
> | Microsoft.ClassicStorage/storageAccounts/* | Erstellen und Verwalten von Speicherkonten |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="classic-storage-account-key-operator-service-role"></a>Klassische Dienstrolle „Speicherkonto-Schlüsseloperator“
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Klassische Speicherkonto-Schlüsseloperatoren dürfen Schlüssel für klassische Speicherkonten auflisten und neu generieren. |
> | **Id** | 985d6b00-f706-48f5-a6fe-d0ca12fb668d |
> | **Aktionen** |  |
> | Microsoft.ClassicStorage/storageAccounts/listkeys/action | Listet die Zugriffsschlüssel für die Speicherkonten auf. |
> | Microsoft.ClassicStorage/storageAccounts/regeneratekey/action | Generiert die vorhandenen Zugriffsschlüssel für das Speicherkonto neu. |

## <a name="classic-virtual-machine-contributor"></a>Mitwirkender von klassischen virtuellen Computern
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen das Verwalten klassischer virtueller Computer, aber weder den Zugriff darauf noch auf die mit ihnen verbundenen virtuellen Netzwerke oder Speicherkonten. |
> | **Id** | d73bb868-a0df-4d4d-bd69-98a00b01fccb |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Autorisierungen |
> | Microsoft.ClassicCompute/domainNames/* | Erstellen und Verwalten von klassischen Compute-Domänennamen |
> | Microsoft.ClassicCompute/virtualMachines/* | Erstellen und Verwalten von virtuellen Computern |
> | Microsoft.ClassicNetwork/networkSecurityGroups/join/action |  |
> | Microsoft.ClassicNetwork/reservedIps/link/action | Dient zum Verknüpfen einer reservierten IP-Adresse. |
> | Microsoft.ClassicNetwork/reservedIps/read | Ruft die reservierten IP-Adressen ab. |
> | Microsoft.ClassicNetwork/virtualNetworks/join/action | Führt zum Beitritt zum virtuellen Netzwerk. |
> | Microsoft.ClassicNetwork/virtualNetworks/read | Dient zum Abrufen des virtuellen Netzwerks. |
> | Microsoft.ClassicStorage/storageAccounts/disks/read | Gibt den Speicherkontodatenträger zurück. |
> | Microsoft.ClassicStorage/storageAccounts/images/read | Gibt das Speicherkontoimage zurück. |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Listet die Zugriffsschlüssel für die Speicherkonten auf. |
> | Microsoft.ClassicStorage/storageAccounts/read | Dient zum Zurückgeben des Speicherkontos mit dem angegebenen Konto. |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="cleardb-mysql-db-contributor"></a>Mitwirkender von ClearDB-MySQL-DB
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen das Verwalten von ClearDB MySQL-Datenbanken, nicht aber den Zugriff darauf. |
> | **Id** | 9106cda0-8a86-4e81-b686-29a22c54effe |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |
> | successbricks.cleardb/databases/* | Erstellen und Verwalten von ClearDB MySQL-Datenbanken |

## <a name="cosmos-db-account-reader-role"></a>Cosmos DB-Rolle „Kontoleser“
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Kann Azure Cosmos DB-Kontodaten lesen. Informationen zum Verwalten von Azure Cosmos DB-Konten finden Sie unter [Mitwirkender von DocumentDB-Konto](#documentdb-account-contributor). |
> | **Id** | fbdf93bf-df7d-467e-a4d2-9458aa1360c8 |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen, kann die jedem Benutzer erteilten Berechtigungen lesen |
> | Microsoft.DocumentDB/*/read | Lesen einer beliebigen Sammlung |
> | Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | Liest die schreibgeschützten Schlüssel für Datenbankkonten. |
> | Microsoft.Insights/MetricDefinitions/read | Dient zum Lesen von Metrikdefinitionen. |
> | Microsoft.Insights/Metrics/read | Dient zum Lesen von Metriken. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="data-factory-contributor"></a>Mitwirkender von Data Factory
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Erstellen und verwalten Sie Data Factorys sowie die darin enthaltenen untergeordneten Ressourcen. |
> | **Id** | 673868aa-7521-48a0-acc6-0f60742d39f5 |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.DataFactory/dataFactories/* | Erstellt und verwaltet Data Factorys und darin enthaltene untergeordnete Ressourcen. |
> | Microsoft.DataFactory/factories/* | Erstellt und verwaltet Data Factorys und darin enthaltene untergeordnete Ressourcen. |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="data-lake-analytics-developer"></a>Data Lake Analytics-Entwickler
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen das Übermitteln, Überwachen und Verwalten Ihrer eigenen Aufträge, aber nicht das Erstellen oder Löschen von Data Lake Analytics-Konten. |
> | **Id** | 47b7735b-770e-4598-a7da-8b91488b4c88 |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.BigAnalytics/accounts/* |  |
> | Microsoft.DataLakeAnalytics/accounts/* |  |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |
> | **NotActions** |  |
> | Microsoft.BigAnalytics/accounts/Delete |  |
> | Microsoft.BigAnalytics/accounts/TakeOwnership/action |  |
> | Microsoft.BigAnalytics/accounts/Write |  |
> | Microsoft.DataLakeAnalytics/accounts/Delete | Löscht ein DataLakeAnalytics-Konto. |
> | Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action | Erteilt Berechtigungen zum Abbrechen von Aufträgen, die von anderen Benutzern übermittelt wurden. |
> | Microsoft.DataLakeAnalytics/accounts/Write | Erstellt oder aktualisiert ein DataLakeAnalytics-Konto. |
> | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Write | Erstellt oder aktualisiert ein verknüpftes DataLakeStore-Konto eines DataLakeAnalytics-Kontos. |
> | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Delete | Hebt die Verknüpfung eines DataLakeStore-Kontos mit einem DataLakeAnalytics-Konto auf. |
> | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Write | Erstellt oder aktualisiert ein verknüpftes Speicherkonto eines DataLakeAnalytics-Kontos. |
> | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Delete | Hebt die Verknüpfung eines Speicherkontos mit einem DataLakeAnalytics-Konto auf. |
> | Microsoft.DataLakeAnalytics/accounts/firewallRules/Write | Dient zum Erstellen oder Aktualisieren einer Firewallregel. |
> | Microsoft.DataLakeAnalytics/accounts/firewallRules/Delete | Dient zum Löschen einer Firewallregel. |
> | Microsoft.DataLakeAnalytics/accounts/computePolicies/Write | Erstellt oder aktualisiert eine Computerichtlinie. |
> | Microsoft.DataLakeAnalytics/accounts/computePolicies/Delete | Löscht eine Computerichtlinie. |

## <a name="data-purger"></a>Datenpurger
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Kann Analysedaten endgültig löschen. |
> | **Id** | 150f5e0c-0603-4f03-8c7f-cf70034c4e90 |
> | **Aktionen** |  |
> | Microsoft.Insights/components/*/read |  |
> | Microsoft.Insights/components/purge/action |  |
> | Microsoft.OperationalInsights/workspaces/*/read |  |
> | Microsoft.OperationalInsights/workspaces/purge/action | Löscht die angegebenen Daten aus dem Arbeitsbereich. |

## <a name="devtest-labs-user"></a>DevTest Labs-Benutzer
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen das Verbinden, Starten, Neustarten und Herunterfahren Ihrer virtuellen Computer in Ihren Azure DevTest Labs. |
> | **Id** | 76283e04-6283-4c54-8f91-bcf1374a3c64 |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Compute/availabilitySets/read | Dient zum Abrufen der Eigenschaften einer Verfügbarkeitsgruppe. |
> | Microsoft.Compute/virtualMachines/*/read | Lesen der Eigenschaften eines virtuellen Computers (VM-Größen, Laufzeitstatus, VM-Erweiterungen usw.) |
> | Microsoft.Compute/virtualMachines/deallocate/action | Schaltet den virtuellen Computer aus und gibt die Computeressourcen frei. |
> | Microsoft.Compute/virtualMachines/read | Dient zum Abrufen der Eigenschaften eines virtuellen Computers. |
> | Microsoft.Compute/virtualMachines/restart/action | Startet den virtuellen Computer neu. |
> | Microsoft.Compute/virtualMachines/start/action | Startet den virtuellen Computer. |
> | Microsoft.DevTestLab/*/read | Lesen der Eigenschaften eines Labs |
> | Microsoft.DevTestLab/labs/createEnvironment/action | Dient zum Erstellen von virtuellen Computern in einem Lab. |
> | Microsoft.DevTestLab/labs/claimAnyVm/action | Dient zum Anfordern eines beliebigen anforderbaren virtuellen Computers im Lab. |
> | Microsoft.DevTestLab/labs/formulas/delete | Dient zum Löschen von Formeln. |
> | Microsoft.DevTestLab/labs/formulas/read | Dient zum Lesen von Formeln. |
> | Microsoft.DevTestLab/labs/formulas/write | Dient zum Hinzufügen oder Ändern von Formeln. |
> | Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | Wertet Labrichtlinien aus. |
> | Microsoft.DevTestLab/labs/virtualMachines/claim/action | Dient dazu, einen vorhandenen virtuellen Computer in Besitz zu nehmen. |
> | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Verknüpft einen Back-End-Adresspool für den Lastenausgleich. |
> | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Verknüpft eine eingehende NAT-Regel für den Lastenausgleich. |
> | Microsoft.Network/networkInterfaces/*/read | Lesen der Eigenschaften einer Netzwerkschnittstelle (z.B. alle Load Balancer, zu denen die Netzwerkschnittstelle gehört) |
> | Microsoft.Network/networkInterfaces/join/action | Verknüpft einen virtuellen Computer mit einer Netzwerkschnittstelle. |
> | Microsoft.Network/networkInterfaces/read | Ruft eine Netzwerkschnittstellendefinition ab.  |
> | Microsoft.Network/networkInterfaces/write | Erstellt eine Netzwerkschnittstelle oder aktualisiert eine vorhandene Netzwerkschnittstelle.  |
> | Microsoft.Network/publicIPAddresses/*/read | Lesen der Eigenschaften einer öffentlichen IP-Adresse |
> | Microsoft.Network/publicIPAddresses/join/action | Verknüpft eine öffentliche IP-Adresse. |
> | Microsoft.Network/publicIPAddresses/read | Ruft eine Definition für eine öffentliche IP-Adresse ab. |
> | Microsoft.Network/virtualNetworks/subnets/join/action | Verknüpft ein virtuelles Netzwerk. |
> | Microsoft.Resources/deployments/operations/read | Ruft Bereitstellungsvorgänge ab oder listet sie auf. |
> | Microsoft.Resources/deployments/read | Ruft Bereitstellungen ab oder listet sie auf. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Storage/storageAccounts/listKeys/action | Gibt die Zugriffsschlüssel für das angegebene Speicherkonto zurück. |
> | **NotActions** |  |
> | Microsoft.Compute/virtualMachines/vmSizes/read | Listet die verfügbaren Größen auf, auf die der virtuelle Computer aktualisiert werden kann. |

## <a name="dns-zone-contributor"></a>DNS Zone Contributor
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen die Verwaltung von DNS-Zonen und Ressourceneintragssätzen in Azure DNS, aber nicht zu steuern, wer darauf Zugriff hat. |
> | **Id** | befefa01-2a29-4197-83a8-272ff33ce314 |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
> | Microsoft.Network/dnsZones/* | Erstellen und Verwalten von DNS-Zonen und -Einträgen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Supporttickets |

## <a name="documentdb-account-contributor"></a>Mitwirkender von DocumentDB-Konto
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Kann Azure Cosmos DB-Konten verwalten. Azure Cosmos DB wurde früher als DocumentDB bezeichnet. |
> | **Id** | 5bd9cd88-fe45-4216-938b-f97437e15450 |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.DocumentDb/databaseAccounts/* | Kann Azure Cosmos DB-Konten erstellen und verwalten |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="intelligent-systems-account-contributor"></a>Mitwirkender von Intelligent Systems-Konto
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen das Verwalten von Intelligent Systems-Konten, nicht aber den Zugriff darauf. |
> | **Id** | 03a6d094-3444-4b3d-88af-7477090a9e5e |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
> | Microsoft.IntelligentSystems/accounts/* | Erstellen und Verwalten von Intelligent Systems-Konten |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="key-vault-contributor"></a>Key Vault-Mitwirkender
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen die Verwaltung von Schlüsseltresoren, aber nicht den Zugriff darauf. |
> | **Id** | f25e0fa2-a7c8-4377-a976-54943a77a395 |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
> | Microsoft.KeyVault/* |  |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |
> | **NotActions** |  |
> | Microsoft.KeyVault/locations/deletedVaults/purge/action | Dient zum endgültigen Löschen eines vorläufig gelöschten Schlüsseltresors. |
> | Microsoft.KeyVault/hsmPools/* |  |

## <a name="lab-creator"></a>Lab-Ersteller
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen das Erstellen, Verwalten und Löschen verwalteter Labs unter Ihren Azure Lab-Konten. |
> | **Id** | b97fb8bc-a8b2-4522-a38b-dd33c7e65ead |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.LabServices/labAccounts/*/read |  |
> | Microsoft.LabServices/labAccounts/createLab/action | Hiermit wird ein Lab in einem Lab-Konto erstellt. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="log-analytics-contributor"></a>Log Analytics-Mitwirkender
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ein Log Analytics-Mitwirkender kann alle Überwachungsdaten lesen und Überwachungseinstellungen bearbeiten. Das Bearbeiten von Überwachungseinstellungen schließt folgende Aufgaben ein: Hinzufügen der VM-Erweiterung zu VMs, Lesen von Speicherkontoschlüsseln zum Konfigurieren von Protokollsammlungen aus Azure Storage, Erstellen und Konfigurieren von Automation-Konten, Hinzufügen von Lösungen, Konfigurieren der Azure-Diagnose für alle Azure-Ressourcen. |
> | **Id** | 92aaf0da-9dab-42b6-94a3-d43ce8d16293 |
> | **Aktionen** |  |
> | */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
> | Microsoft.Automation/automationAccounts/* |  |
> | Microsoft.ClassicCompute/virtualMachines/extensions/* |  |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Listet die Zugriffsschlüssel für die Speicherkonten auf. |
> | Microsoft.Compute/virtualMachines/extensions/* |  |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
> | Microsoft.Insights/diagnosticSettings/* | Erstellt, aktualisiert oder liest die Diagnoseeinstellung für den Analysis-Server. |
> | Microsoft.OperationalInsights/* |  |
> | Microsoft.OperationsManagement/* |  |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourcegroups/deployments/* |  |
> | Microsoft.Storage/storageAccounts/listKeys/action | Gibt die Zugriffsschlüssel für das angegebene Speicherkonto zurück. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="log-analytics-reader"></a>Log Analytics-Leser
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ein Log Analytics-Leser kann alle Überwachungsdaten anzeigen und durchsuchen sowie Überwachungseinstellungen anzeigen. Hierzu zählt auch die Anzeige der Konfiguration von Azure-Diagnosen für alle Azure-Ressourcen. |
> | **Id** | 73c42c96-874c-492b-b04d-ab87d138a893 |
> | **Aktionen** |  |
> | */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
> | Microsoft.OperationalInsights/workspaces/analytics/query/action | Führt eine Suche mit der neuen Engine aus. |
> | Microsoft.OperationalInsights/workspaces/search/action | Führt eine Suchabfrage aus. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |
> | **NotActions** |  |
> | Microsoft.OperationalInsights/workspaces/sharedKeys/read | Ruft die gemeinsam verwendeten Schlüssel für den Arbeitsbereich ab. Diese Schlüssel werden verwendet, um Microsoft Operational Insights-Agents mit dem Arbeitsbereich zu verbinden. |

## <a name="logic-app-contributor"></a>Mitwirkender für Logik-Apps
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen das Verwalten von Logik-Apps, aber nicht den Zugriff darauf. |
> | **Id** | 87a39d53-fc1b-424a-814c-f7e04687dc9e |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Listet die Zugriffsschlüssel für die Speicherkonten auf. |
> | Microsoft.ClassicStorage/storageAccounts/read | Dient zum Zurückgeben des Speicherkontos mit dem angegebenen Konto. |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
> | Microsoft.Insights/diagnosticSettings/* | Erstellt, aktualisiert oder liest die Diagnoseeinstellung für den Analysis-Server. |
> | Microsoft.Insights/logdefinitions/* | Diese Berechtigung ist für Benutzer notwendig, die über das Portal auf Aktivitätsprotokolle zugreifen müssen. Auflisten der Protokollkategorien im Aktivitätsprotokoll. |
> | Microsoft.Insights/metricDefinitions/* | Lesen von Metrikdefinitionen (Liste der verfügbaren Metriktypen für eine Ressource). |
> | Microsoft.Logic/* | Verwaltet Logic Apps-Ressourcen. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/operationresults/read | Dient zum Abrufen der Ergebnisse des Abonnementvorgangs. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Storage/storageAccounts/listkeys/action | Gibt die Zugriffsschlüssel für das angegebene Speicherkonto zurück. |
> | Microsoft.Storage/storageAccounts/read | Gibt die Liste mit Speicherkonten zurück oder ruft die Eigenschaften für das angegebene Speicherkonto ab. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |
> | Microsoft.Web/connectionGateways/* | Erstellt und verwaltet ein Verbindungsgateway. |
> | Microsoft.Web/connections/* | Erstellt und verwaltet eine Verbindung. |
> | Microsoft.Web/customApis/* | Erstellt und verwaltet eine benutzerdefinierte API. |
> | Microsoft.Web/serverFarms/join/action |  |
> | Microsoft.Web/serverFarms/read | Dient zum Abrufen der Eigenschaften für einen App Service-Plan. |
> | Microsoft.Web/sites/functions/listSecrets/action | Dient zum Auflisten der Geheimnisse für Web-App-Funktionen. |

## <a name="logic-app-operator"></a>Logik-App-Operator
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen das Lesen, Aktivieren und Deaktivieren von Logik-Apps. |
> | **Id** | 515c2055-d9d4-4321-b1b9-bd0c9a0f79fe |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/*/read | Lesen von Insights-Warnungsregeln |
> | Microsoft.Insights/diagnosticSettings/*/read | Ruft die Diagnoseeinstellungen für Logic Apps ab. |
> | Microsoft.Insights/metricDefinitions/*/read | Ruft die verfügbaren Metriken für Logic Apps ab. |
> | Microsoft.Logic/*/read | Liest Logic Apps-Ressourcen. |
> | Microsoft.Logic/workflows/disable/action | Deaktiviert den Workflow. |
> | Microsoft.Logic/workflows/enable/action | Aktiviert den Workflow. |
> | Microsoft.Logic/workflows/validate/action | Überprüft den Workflow. |
> | Microsoft.Resources/deployments/operations/read | Ruft Bereitstellungsvorgänge ab oder listet sie auf. |
> | Microsoft.Resources/subscriptions/operationresults/read | Dient zum Abrufen der Ergebnisse des Abonnementvorgangs. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |
> | Microsoft.Web/connectionGateways/*/read | Liest Verbindungsgateways. |
> | Microsoft.Web/connections/*/read | Liest Verbindungen. |
> | Microsoft.Web/customApis/*/read | Liest benutzerdefinierte API. |
> | Microsoft.Web/serverFarms/read | Dient zum Abrufen der Eigenschaften für einen App Service-Plan. |

## <a name="managed-identity-contributor"></a>Mitwirkender für verwaltete Identität
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Dem Benutzer zugewiesene Identität erstellen, lesen, aktualisieren und löschen. |
> | **Id** | e40ec5ca-96e0-45a2-b4ff-59039f2c2b59 |
> | **Aktionen** |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/read |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/write |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/delete |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="managed-identity-operator"></a>Operator für verwaltete Identität
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Dem Benutzer zugewiesene Identität lesen und zuweisen. |
> | **Id** | f1a07417-d97a-45cb-824c-7a7467783830 |
> | **Aktionen** |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/read |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/assign/action |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="monitoring-contributor"></a>Überwachungsmitwirkender
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Kann sämtliche Überwachungsdaten lesen und Überwachungseinstellungen bearbeiten. Siehe auch [Erste Schritte mit Rollen, Berechtigungen und Sicherheit in Azure Monitor](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles). |
> | **Id** | 749f88d5-cbae-40b8-bcfc-e573ddc772fa |
> | **Aktionen** |  |
> | */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
> | Microsoft.AlertsManagement/alerts/* |  |
> | Microsoft.AlertsManagement/alertsSummary/* |  |
> | Microsoft.Insights/AlertRules/* | Lesen/Schreiben/Löschen von Warnungsregeln. |
> | Microsoft.Insights/components/* | Kann Application Insights-Komponenten lesen/schreiben/delegieren. |
> | Microsoft.Insights/DiagnosticSettings/* | Lesen/Schreiben/Löschen von Diagnoseeinstellungen. |
> | Microsoft.Insights/eventtypes/* | Auflisten von Aktivitätsprotokollereignissen (Verwaltungsereignissen) in einem Abonnement. Diese Berechtigung gilt sowohl für den programmgesteuerten als auch für den Portalzugriff auf das Aktivitätsprotokoll. |
> | Microsoft.Insights/LogDefinitions/* | Diese Berechtigung ist für Benutzer notwendig, die über das Portal auf Aktivitätsprotokolle zugreifen müssen. Auflisten der Protokollkategorien im Aktivitätsprotokoll. |
> | Microsoft.Insights/MetricDefinitions/* | Lesen von Metrikdefinitionen (Liste der verfügbaren Metriktypen für eine Ressource). |
> | Microsoft.Insights/Metrics/* | Lesen von Metriken für eine Ressource. |
> | Microsoft.Insights/Register/Action | Dient zum Registrieren des Microsoft Insights-Anbieters. |
> | Microsoft.Insights/webtests/* | Lesen/Schreiben/Löschen von Application Insights-Webtests. |
> | Microsoft.Insights/actiongroups/* |  |
> | Microsoft.Insights/metricalerts/* |  |
> | Microsoft.Insights/scheduledqueryrules/* |  |
> | Microsoft.OperationalInsights/workspaces/intelligencepacks/* | Lesen/Schreiben/Löschen von Log Analytics-Lösungspaketen. |
> | Microsoft.OperationalInsights/workspaces/savedSearches/* | Lesen/Schreiben/Löschen von gespeicherten Log Analytics-Suchvorgängen. |
> | Microsoft.OperationalInsights/workspaces/search/action | Führt eine Suchabfrage aus. |
> | Microsoft.OperationalInsights/workspaces/sharedKeys/action | Ruft die gemeinsam verwendeten Schlüssel für den Arbeitsbereich ab. Diese Schlüssel werden verwendet, um Microsoft Operational Insights-Agents mit dem Arbeitsbereich zu verbinden. |
> | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/* | Lesen/Schreiben/Löschen von Log Analytics-Speicherdetailinformationen. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |
> | Microsoft.WorkloadMonitor/workloads/* |  |

## <a name="monitoring-reader"></a>Überwachungsleser
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Kann alle Überwachungsdaten (Metriken, Protokolle usw.) lesen. Siehe auch [Erste Schritte mit Rollen, Berechtigungen und Sicherheit in Azure Monitor](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles). |
> | **Id** | 43d0d8ad-25c7-4714-9337-8ba259a9fe05 |
> | **Aktionen** |  |
> | */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
> | Microsoft.OperationalInsights/workspaces/search/action | Führt eine Suchabfrage aus. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="network-contributor"></a>Mitwirkender von virtuellem Netzwerk
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen das Verwalten von Netzwerken, nicht aber den Zugriff darauf. |
> | **Id** | 4d97b98b-1d4f-4787-a291-c67834d212e7 |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
> | Microsoft.Network/* | Erstellen und Verwalten von Netzwerken |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="new-relic-apm-account-contributor"></a>Mitwirkender von New Relic APM-Konto
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen das Verwalten von New Relic Application Performance Management-Konten und -Anwendungen, nicht aber den Zugriff auf diese. |
> | **Id** | 5d28c62d-5b37-4476-8438-e587778df237 |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |
> | NewRelic.APM/accounts/* |  |

## <a name="reader-and-data-access"></a>Lese- und Datenzugriff
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen das Anzeigen sämtlicher Aspekte, jedoch nicht das Löschen oder Erstellen eines Speicherkontos oder einer darin enthaltenen Ressource. Sie können auch Lese-/Schreibzugriff auf alle Daten in einem Speicherkonto durch Zugriff auf Speicherkontoschlüssel gewähren. |
> | **Id** | c12c1c16-33a1-487b-954d-41c89c60f349 |
> | **Aktionen** |  |
> | Microsoft.Storage/storageAccounts/listKeys/action | Gibt die Zugriffsschlüssel für das angegebene Speicherkonto zurück. |
> | Microsoft.Storage/storageAccounts/read | Gibt die Liste mit Speicherkonten zurück oder ruft die Eigenschaften für das angegebene Speicherkonto ab. |

## <a name="redis-cache-contributor"></a>Mitwirkender von Redis-Cache
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen das Verwalten von Redis Caches, nicht aber den Zugriff darauf. |
> | **Id** | e0f68234-74aa-48ed-b826-c38b57376e17 |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Cache/redis/* | Erstellen und Verwalten von Redis-Caches |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="scheduler-job-collections-contributor"></a>Mitwirkender von Zeitplanungsauftragssammlung
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen das Verwalten von Scheduler-Auftragssammlungen, nicht aber den Zugriff darauf. |
> | **Id** | 188a0f2f-5c9e-469b-ae67-2aa5ce574b94 |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Scheduler/jobcollections/* | Erstellen und Verwalten von Auftragssammlungen |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="search-service-contributor"></a>Mitwirkender von Suchdienst
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen das Verwalten von Search-Diensten, nicht aber den Zugriff darauf. |
> | **Id** | 7ca78c08-252a-4471-8644-bb5ff32d4ba0 |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Search/searchServices/* | Erstellen und Verwalten von Suchdiensten |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="security-admin"></a>Sicherheitsadministrator
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Nur in Security Center: Kann Sicherheitsrichtlinien und -zustände anzeigen, Sicherheitsrichtlinien bearbeiten sowie Warnungen und Empfehlungen anzeigen und verwerfen |
> | **Id** | fb1c8493-542b-48eb-b624-b4c8fea62acd |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Authorization/policyAssignments/* | Erstellen und Verwalten von Richtlinienzuweisungen |
> | Microsoft.Authorization/policyDefinitions/* | Erstellen und Verwalten von Richtliniendefinitionen |
> | Microsoft.Authorization/policySetDefinitions/* | Erstellen und Verwalten von Richtliniensätzen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
> | Microsoft.operationalInsights/workspaces/*/read | Anzeigen von Log Analytics-Daten |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Security/*/read | Lesen von Sicherheitskomponenten und -richtlinien |
> | Microsoft.Security/locations/alerts/dismiss/action | Schließt eine Sicherheitswarnung. |
> | Microsoft.Security/locations/alerts/activate/action | Aktiviert eine Sicherheitswarnung. |
> | Microsoft.Security/locations/tasks/dismiss/action | Dient zum Verwerfen einer Sicherheitsempfehlung. |
> | Microsoft.Security/locations/tasks/activate/action | Dient zum Aktivieren einer Sicherheitsempfehlung. |
> | Microsoft.Security/policies/write | Aktualisiert die Sicherheitsrichtlinie. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="security-manager-legacy"></a>Sicherheits-Manager (Legacy)
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Dies ist eine Legacyrolle. Verwenden Sie stattdessen „Sicherheitsadministrator“. |
> | **Id** | e3d13bf0-dd5a-482e-ba6b-9b8433878d10 |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.ClassicCompute/*/read | Lesen von Konfigurationsinformationen zu klassischen virtuellen Computern |
> | Microsoft.ClassicCompute/virtualMachines/*/write | Schreiben der Konfiguration für klassische virtuelle Computer |
> | Microsoft.ClassicNetwork/*/read | Lesen von Konfigurationsinformationen zu klassischem Netzwerk |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Security/* | Erstellen und Verwalten von Sicherheitskomponenten und -richtlinien |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="security-reader"></a>Sicherheit lesen
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Nur in Security Center: Kann Empfehlungen und Warnungen sowie Sicherheitsrichtlinien und -zustände anzeigen, aber keine Änderungen vornehmen |
> | **Id** | 39bc4728-0917-49c7-9d2c-d95423bc2eb4 |
> | **Aktionen** |  |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.operationalInsights/workspaces/*/read | Anzeigen von Log Analytics-Daten |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Security/*/read | Lesen von Sicherheitskomponenten und -richtlinien |

## <a name="site-recovery-contributor"></a>Site Recovery-Mitwirkender
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen die Verwaltung des Site Recovery-Diensts mit Ausnahme der Tresorerstellung und der Rollenzuweisung. |
> | **Id** | 6670b86e-a3f7-4917-ac9b-5d6ab1be4567 |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
> | Microsoft.Network/virtualNetworks/read | Dient zum Abrufen der Definition des virtuellen Netzwerks. |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | „GetAllocatedStamp“ ist ein interner Vorgang des Diensts. |
> | Microsoft.RecoveryServices/locations/allocateStamp/action | „AllocateStamp“ ist ein interner Vorgang des Diensts. |
> | Microsoft.RecoveryServices/Vaults/certificates/write | Der Vorgang „Ressourcenzertifikat aktualisieren“ aktualisiert das Zertifikat für die Ressourcen-/Tresoranmeldeinformationen. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/* | Erstellen und Verwalten erweiterter Informationen in Zusammenhang mit einem Tresor |
> | Microsoft.RecoveryServices/Vaults/read | Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Erstellen und Verwalten von registrierten Identitäten |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/* | Erstellen oder Aktualisieren von Einstellungen für Replikationswarnungen |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | Dient zum Lesen beliebiger Ereignisse. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/* | Erstellen und Verwalten von Replikationsfabrics |
> | Microsoft.RecoveryServices/vaults/replicationJobs/* | Erstellen und Verwalten von Replikationsaufträgen |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/* | Erstellen und Verwalten von Replikationsrichtlinien |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/* | Erstellen und Verwalten von Wiederherstellungsplänen |
> | Microsoft.RecoveryServices/Vaults/storageConfig/* | Erstellen und Verwalten der Speicherkonfiguration des Recovery Services-Tresors |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read | Gibt Tokeninformationen für Recovery Services-Tresore zurück. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Gibt Nutzungsdetails für einen Recovery Services-Tresor zurück. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | Der Vorgang „Tresortoken“ kann zum Abrufen des Tresortokens für Back-End-Vorgänge auf Tresorebene verwendet werden. |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Lesen von Warnungen für den Recovery Services-Tresor |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Storage/storageAccounts/read | Gibt die Liste mit Speicherkonten zurück oder ruft die Eigenschaften für das angegebene Speicherkonto ab. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="site-recovery-operator"></a>Site Recovery-Operator
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen ein Failover und ein Failback, aber nicht das Durchführen weiterer Site Recovery-Verwaltungsvorgänge. |
> | **Id** | 494ae006-db33-4328-bf46-533a6560a3ca |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
> | Microsoft.Network/virtualNetworks/read | Dient zum Abrufen der Definition des virtuellen Netzwerks. |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | „GetAllocatedStamp“ ist ein interner Vorgang des Diensts. |
> | Microsoft.RecoveryServices/locations/allocateStamp/action | „AllocateStamp“ ist ein interner Vorgang des Diensts. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Microsoft.RecoveryServices/Vaults/read | Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Mit dem Vorgang „Vorgangsergebnisse abrufen“ können der Vorgangsstatus und das Ergebnis für den asynchron übermittelten Vorgang abgerufen werden. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Der Vorgang „Container abrufen“ kann zum Abrufen der für eine Ressource registrierten Container verwendet werden. |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Dient zum Lesen beliebiger Warnungseinstellungen. |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | Dient zum Lesen beliebiger Ereignisse. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Prüft die Konsistenz des Fabrics. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Dient zum Lesen beliebiger Fabrics. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | Dient zum Neuzuordnen des Gateways. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Verlängert das Zertifikat für Fabrics. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Dient zum Lesen beliebiger Netzwerke. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Dient zum Lesen beliebiger Netzwerkzuordnungen. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Dient zum Lesen beliebiger Schutzcontainer. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Dient zum Lesen beliebiger schützbarer Elemente. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Dient zum Anwenden des Wiederherstellungspunkts. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | Failovercommit |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | Geplantes Failover |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Dient zum Lesen beliebiger geschützter Elemente. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Dient zum Lesen beliebiger Wiederherstellungspunkte für die Replikation. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/repairReplication/action | Dient zum Reparieren der Replikation. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/reProtect/action | Dient zum erneuten Schützen geschützter Elemente. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailover/action | Testfailover |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | Testfailoverbereinigung |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action | Failover |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Dient zum Aktualisieren von Mobility Service. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Dient zum Lesen beliebiger Schutzcontainerzuordnungen. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Dient zum Lesen beliebiger Recovery Services-Anbieter. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/action | Dient zum Aktualisieren des Anbieters. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | Dient zum Lesen beliebiger Speicherklassifizierungen. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Dient zum Lesen beliebiger Speicherklassifizierungszuordnungen. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Dient zum Lesen beliebiger Aufträge. |
> | Microsoft.RecoveryServices/vaults/replicationJobs/* | Erstellen und Verwalten von Replikationsaufträgen |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Dient zum Lesen beliebiger Richtlinien. |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | Wiederherstellungsplan für Failovercommit |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | Wiederherstellungsplan für geplantes Failover |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Dient zum Lesen beliebiger Wiederherstellungspläne. |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Wiederherstellungsplan für erneutes Schützen |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Wiederherstellungsplan für Testfailover |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Wiederherstellungsplan für Testfailoverbereinigung |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action | Wiederherstellungsplan für Failover |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Lesen von Warnungen für den Recovery Services-Tresor |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read | Gibt Tokeninformationen für Recovery Services-Tresore zurück. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Gibt Nutzungsdetails für einen Recovery Services-Tresor zurück. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | Der Vorgang „Tresortoken“ kann zum Abrufen des Tresortokens für Back-End-Vorgänge auf Tresorebene verwendet werden. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Storage/storageAccounts/read | Gibt die Liste mit Speicherkonten zurück oder ruft die Eigenschaften für das angegebene Speicherkonto ab. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="site-recovery-reader"></a>Site Recovery-Leser
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen die Anzeige des Site Recovery-Status, aber nicht die Durchführung weiterer Verwaltungsvorgänge. |
> | **Id** | dbaa88c4-0c30-4179-9fb3-46319faa6149 |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | „GetAllocatedStamp“ ist ein interner Vorgang des Diensts. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Ruft die Warnungen für den Recovery Services-Tresor ab. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.RecoveryServices/Vaults/read | Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Mit dem Vorgang „Vorgangsergebnisse abrufen“ können der Vorgangsstatus und das Ergebnis für den asynchron übermittelten Vorgang abgerufen werden. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Der Vorgang „Container abrufen“ kann zum Abrufen der für eine Ressource registrierten Container verwendet werden. |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Dient zum Lesen beliebiger Warnungseinstellungen. |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | Dient zum Lesen beliebiger Ereignisse. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Dient zum Lesen beliebiger Fabrics. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Dient zum Lesen beliebiger Netzwerke. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Dient zum Lesen beliebiger Netzwerkzuordnungen. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Dient zum Lesen beliebiger Schutzcontainer. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Dient zum Lesen beliebiger schützbarer Elemente. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Dient zum Lesen beliebiger geschützter Elemente. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Dient zum Lesen beliebiger Wiederherstellungspunkte für die Replikation. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Dient zum Lesen beliebiger Schutzcontainerzuordnungen. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Dient zum Lesen beliebiger Recovery Services-Anbieter. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | Dient zum Lesen beliebiger Speicherklassifizierungen. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Dient zum Lesen beliebiger Speicherklassifizierungszuordnungen. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Dient zum Lesen beliebiger Aufträge. |
> | Microsoft.RecoveryServices/vaults/replicationJobs/read | Dient zum Lesen beliebiger Aufträge. |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Dient zum Lesen beliebiger Richtlinien. |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Dient zum Lesen beliebiger Wiederherstellungspläne. |
> | Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read | Gibt Tokeninformationen für Recovery Services-Tresore zurück. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Gibt Nutzungsdetails für einen Recovery Services-Tresor zurück. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | Der Vorgang „Tresortoken“ kann zum Abrufen des Tresortokens für Back-End-Vorgänge auf Tresorebene verwendet werden. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="sql-db-contributor"></a>Mitwirkender von SQL DB
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen das Verwalten von SQL-Datenbanken, nicht aber den Zugriff darauf. Darüber hinaus können Sie deren sicherheitsbezogenen Richtlinien oder übergeordneten SQL-Server nicht verwalten. |
> | **Id** | 9b7fa17d-e63e-47b0-bb0a-15c516ac86ec |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Sql/locations/*/read |  |
> | Microsoft.Sql/servers/databases/* | Erstellen und Verwalten von SQL-Datenbanken |
> | Microsoft.Sql/servers/read | Gibt die Liste der Server zurück oder ruft die Eigenschaften für den angegebenen Server ab. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |
> | **NotActions** |  |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | Kann keine Überwachungsrichtlinien bearbeiten |
> | Microsoft.Sql/servers/databases/auditingSettings/* | Überwachungsrichtlinien können nicht bearbeiten werden |
> | Microsoft.Sql/servers/databases/auditRecords/read | Dient zum Abrufen der Datensätze für die Datenbankblobüberwachung. |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | Kann keine Verbindungsrichtlinien bearbeiten |
> | Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Kann keine Datenmaskierungsrichtlinien bearbeiten |
> | Microsoft.Sql/servers/databases/extendedAuditingSettings/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/securityAlertPolicies/* | Richtlinien für Sicherheitswarnungen können nicht bearbeitet werden |
> | Microsoft.Sql/servers/databases/securityMetrics/* | Kann keine Sicherheitsmetriken bearbeiten |
> | Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |

## <a name="sql-security-manager"></a>SQL-Sicherheits-Manager
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen das Verwalten von sicherheitsbezogenen Richtlinien von SQL-Server und Datenbanken, jedoch nicht den Zugriff darauf. |
> | **Id** | 056cd41c-7e88-42e1-933e-88ba6a50c9c3 |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen der Microsoft-Autorisierung |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Verknüpft Ressourcen wie etwa ein Speicherkonto oder eine SQL-Datenbank mit einem Subnetz. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Sql/servers/auditingPolicies/* | Erstellen und Verwalten von SQL Server-Überwachungsrichtlinien |
> | Microsoft.Sql/servers/auditingSettings/* | Erstellen und Verwalten von SQL Server-Überwachungseinstellungen |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | Erstellen und Verwalten von Überwachungsrichtlinien von SQL Server-Datenbanken |
> | Microsoft.Sql/servers/databases/auditingSettings/* | Erstellen und Verwalten von Überwachungseinstellungen von SQL Server-Datenbanken |
> | Microsoft.Sql/servers/databases/auditRecords/read | Lesen von Überwachungsdatensätzen |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | Erstellen und Verwalten von Verbindungsrichtlinien von SQL Server-Datenbanken |
> | Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Erstellen und Verwalten von Datenmaskierungsrichtlinien von SQL Server-Datenbanken |
> | Microsoft.Sql/servers/databases/read | Gibt die Liste der Datenbanken zurück oder ruft die Eigenschaften für die angegebene Datenbank ab. |
> | Microsoft.Sql/servers/databases/schemas/read | Ruft die Liste der Schemas einer Datenbank ab. |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/read | Dient zum Abrufen einer Liste mit Tabellenspalten. |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/read | Ruft die Liste der Tabellen einer Datenbank ab. |
> | Microsoft.Sql/servers/databases/securityAlertPolicies/* | Erstellen und Verwalten von Richtlinien für Sicherheitswarnungen von SQL Server-Datenbanken |
> | Microsoft.Sql/servers/databases/securityMetrics/* | Erstellen und Verwalten von Sicherheitsmetriken von SQL Server-Datenbanken |
> | Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | Microsoft.Sql/servers/firewallRules/* |  |
> | Microsoft.Sql/servers/read | Gibt die Liste der Server zurück oder ruft die Eigenschaften für den angegebenen Server ab. |
> | Microsoft.Sql/servers/securityAlertPolicies/* | Erstellen und Verwalten von Richtlinien für Sicherheitswarnungen von SQL Server |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="sql-server-contributor"></a>Mitwirkender von SQL Server
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen, SQL-Server und -Datenbanken zu verwalten, gewährt Ihnen jedoch keinen Zugriff darauf und auch nicht auf deren sicherheitsbezogenen Richtlinien. |
> | **Id** | 6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437 |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Sql/locations/*/read |  |
> | Microsoft.Sql/servers/* | Erstellen und Verwalten von SQL-Servern |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |
> | **NotActions** |  |
> | Microsoft.Sql/servers/auditingPolicies/* | Kann keine SQL Server-Überwachungsrichtlinien bearbeiten |
> | Microsoft.Sql/servers/auditingSettings/* | SQL Server-Überwachungseinstellungen können nicht bearbeitet werden |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | Kann keine Überwachungsrichtlinien von SQL Server-Datenbanken bearbeiten |
> | Microsoft.Sql/servers/databases/auditingSettings/* | Überwachungseinstellungen von SQL Server-Datenbanken können nicht bearbeitet werden |
> | Microsoft.Sql/servers/databases/auditRecords/read | Kann keine Überwachungsdatensätze lesen |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | Kann keine Verbindungsrichtlinien von SQL Server-Datenbanken bearbeiten |
> | Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Kann keine Datenmaskierungsrichtlinien von SQL Server-Datenbanken bearbeiten |
> | Microsoft.Sql/servers/databases/extendedAuditingSettings/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/securityAlertPolicies/* | Richtlinien für Sicherheitswarnungen von SQL Server-Datenbanken können nicht bearbeitet werden |
> | Microsoft.Sql/servers/databases/securityMetrics/* | Kann keine Sicherheitsmetriken von SQL Server-Datenbanken bearbeiten |
> | Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | Microsoft.Sql/servers/extendedAuditingSettings/* |  |
> | Microsoft.Sql/servers/securityAlertPolicies/* | Richtlinien für Sicherheitswarnungen von SQL Server können nicht bearbeitet werden |

## <a name="storage-account-contributor"></a>Mitwirkender von Speicherkonto
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen das Verwalten von Speicherkonten, nicht aber den Zugriff darauf. |
> | **Id** | 17d1049b-9a84-46fb-8f53-869881c3d3ab |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen aller Autorisierungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
> | Microsoft.Insights/diagnosticSettings/* | Verwalten der Diagnoseeinstellungen |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Verknüpft Ressourcen wie etwa ein Speicherkonto oder eine SQL-Datenbank mit einem Subnetz. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Storage/storageAccounts/* | Erstellen und Verwalten von Speicherkonten |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="storage-account-key-operator-service-role"></a>Dienstrolle „Speicherkonto-Schlüsseloperator“
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Speicherkonto-Schlüsseloperatoren dürfen Schlüssel für Speicherkonten auflisten und neu generieren. |
> | **Id** | 81a9662b-bebf-436f-a333-f67b29880f12 |
> | **Aktionen** |  |
> | Microsoft.Storage/storageAccounts/listkeys/action | Gibt die Zugriffsschlüssel für das angegebene Speicherkonto zurück. |
> | Microsoft.Storage/storageAccounts/regeneratekey/action | Generiert die Zugriffsschlüssel für das angegebene Speicherkonto neu. |

## <a name="support-request-contributor"></a>Mitwirkender für Supportanfragen
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen die Erstellung und Verwaltung von Supportanfragen. |
> | **Id** | cfd33db0-3dd1-45e3-aa9d-cdbdf3b6f24e |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Autorisierungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="traffic-manager-contributor"></a>Traffic Manager-Mitwirkender
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen die Verwaltung von Traffic Manager-Profilen, aber nicht die Steuerung des Zugriffs darauf. |
> | **Id** | a4b10055-b0c7-44c2-b00f-c7b5b3550cf7 |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
> | Microsoft.Network/trafficManagerProfiles/* |  |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="user-access-administrator"></a>Benutzerzugriffsadministrator
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen die Verwaltung von Benutzerzugriffen auf Azure-Ressourcen. |
> | **Id** | 18d7d88d-d35e-4fb5-a5c3-7773c20a72d9 |
> | **Aktionen** |  |
> | */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
> | Microsoft.Authorization/* | Verwalten der Autorisierung |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="virtual-machine-administrator-login"></a>VM-Administratoranmeldung
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Benutzer mit dieser Rolle haben die Möglichkeit, sich bei einem virtuellen Computer mit Windows-Administrator- oder Linux-Root-Benutzerrechten anzumelden. |
> | **Id** | 1c0163c0-47e6-4577-8991-ea5c82e286e4 |
> | **Aktionen** |  |
> | Microsoft.Network/publicIPAddresses/read | Ruft eine Definition für eine öffentliche IP-Adresse ab. |
> | Microsoft.Network/virtualNetworks/read | Dient zum Abrufen der Definition des virtuellen Netzwerks. |
> | Microsoft.Network/loadBalancers/read | Ruft eine Lastenausgleichsdefinition ab. |
> | Microsoft.Network/networkInterfaces/read | Ruft eine Netzwerkschnittstellendefinition ab.  |
> | Microsoft.Compute/virtualMachines/*/read |  |

## <a name="virtual-machine-contributor"></a>Mitwirkender von virtuellen Computern
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen das Verwalten virtueller Computer, aber weder den Zugriff darauf, noch auf deren verbundenen virtuellen Netzwerke oder Speicherkonten. |
> | **Id** | 9980e02c-c2be-4d73-94e8-173b1dc7cf3c |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Autorisierungen |
> | Microsoft.Compute/availabilitySets/* | Erstellen und Verwalten von Compute-Verfügbarkeitsgruppen |
> | Microsoft.Compute/locations/* | Erstellen und Verwalten von Compute-Speicherorten |
> | Microsoft.Compute/virtualMachines/* | Erstellen und Verwalten von virtuellen Computern |
> | Microsoft.Compute/virtualMachineScaleSets | Erstellen und Verwalten von Skalierungsgruppen für virtuelle Computer |
> | Microsoft.DevTestLab/schedules/* |  |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
> | Microsoft.Network/applicationGateways/backendAddressPools/join/action | Verknüpft einen Back-End-Adresspool für ein Anwendungsgateway. |
> | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Verknüpft einen Back-End-Adresspool für den Lastenausgleich. |
> | Microsoft.Network/loadBalancers/inboundNatPools/join/action | Verknüpft einen eingehenden NAT-Pool für den Lastenausgleich. |
> | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Verknüpft eine eingehende NAT-Regel für den Lastenausgleich. |
> | Microsoft.Network/loadBalancers/probes/join/action | Ermöglicht die Verwendung von Prüfpunkten eines Lastenausgleichs. Beispielsweise kann mit dieser Berechtigung die healthProbe-Eigenschaft einer VM-Skalierungsgruppe auf den Prüfpunkt verweisen. |
> | Microsoft.Network/loadBalancers/read | Ruft eine Lastenausgleichsdefinition ab. |
> | Microsoft.Network/locations/* | Erstellen und Verwalten von Netzwerkspeicherorten |
> | Microsoft.Network/networkInterfaces/* | Erstellen und Verwalten von Netzwerkschnittstellen |
> | Microsoft.Network/networkSecurityGroups/join/action | Verknüpft eine Netzwerksicherheitsgruppe. |
> | Microsoft.Network/networkSecurityGroups/read | Ruft eine Netzwerksicherheitsgruppen-Definition ab. |
> | Microsoft.Network/publicIPAddresses/join/action | Verknüpft eine öffentliche IP-Adresse. |
> | Microsoft.Network/publicIPAddresses/read | Ruft eine Definition für eine öffentliche IP-Adresse ab. |
> | Microsoft.Network/virtualNetworks/read | Dient zum Abrufen der Definition des virtuellen Netzwerks. |
> | Microsoft.Network/virtualNetworks/subnets/join/action | Verknüpft ein virtuelles Netzwerk. |
> | Microsoft.RecoveryServices/locations/* |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Gibt Objektdetails des geschützten Elements zurück. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Dient zum Erstellen eines geschützten Elements für die Sicherung. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | Erstellt einen beabsichtigten Sicherungsschutz. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Gibt alle Schutzrichtlinien zurück. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/write | Erstellt Schutzrichtlinien. |
> | Microsoft.RecoveryServices/Vaults/read | Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Gibt Nutzungsdetails für einen Recovery Services-Tresor zurück. |
> | Microsoft.RecoveryServices/Vaults/write | Der Vorgang „Tresor erstellen“ erstellt eine Azure-Ressource vom Typ „Tresor“. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Storage/storageAccounts/listKeys/action | Gibt die Zugriffsschlüssel für das angegebene Speicherkonto zurück. |
> | Microsoft.Storage/storageAccounts/read | Gibt die Liste mit Speicherkonten zurück oder ruft die Eigenschaften für das angegebene Speicherkonto ab. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

## <a name="virtual-machine-user-login"></a>VM-Benutzeranmeldung
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Benutzer mit dieser Rolle haben die Möglichkeit, sich als normaler Benutzer an einem virtuellen Computer anzumelden. |
> | **Id** | fb879df8-f326-4884-b1cf-06f3ad86be52 |
> | **Aktionen** |  |
> | Microsoft.Network/publicIPAddresses/read | Ruft eine Definition für eine öffentliche IP-Adresse ab. |
> | Microsoft.Network/virtualNetworks/read | Dient zum Abrufen der Definition des virtuellen Netzwerks. |
> | Microsoft.Network/loadBalancers/read | Ruft eine Lastenausgleichsdefinition ab. |
> | Microsoft.Network/networkInterfaces/read | Ruft eine Netzwerkschnittstellendefinition ab.  |
> | Microsoft.Compute/virtualMachines/*/read |  |

## <a name="web-plan-contributor"></a>Mitwirkender von Webplan
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen das Verwalten der Webpläne für Websites, nicht aber den Zugriff darauf. |
> | **Id** | 2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Autorisierungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |
> | Microsoft.Web/serverFarms/* | Erstellen und Verwalten von Serverfarmen |

## <a name="website-contributor"></a>Mitwirkender von Website
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschreibung** | Ermöglicht Ihnen das Verwalten von Websites (nicht der Webpläne), nicht aber den Zugriff darauf. |
> | **Id** | de139f84-1756-47ae-9be6-808fbbe84772 |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Autorisierungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
> | Microsoft.Insights/components/* | Erstellen und Verwalten von Insights-Komponenten |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |
> | Microsoft.Web/certificates/* | Erstellen und Verwalten von Websitezertifikaten |
> | Microsoft.Web/listSitesAssignedToHostName/read | Dient zum Abrufen der Namen von Websites, die dem Hostnamen zugewiesen sind. |
> | Microsoft.Web/serverFarms/join/action |  |
> | Microsoft.Web/serverFarms/read | Dient zum Abrufen der Eigenschaften für einen App Service-Plan. |
> | Microsoft.Web/sites/* | Erstellen und Verwalten von Websites (die Erstellung von Websites erfordert auch Schreibberechtigungen für den zugehörigen App Service-Plan) |

## <a name="next-steps"></a>Nächste Schritte

- [Benutzerdefinierte Rollen](custom-roles.md)
- [Verwenden der rollenbasierten Zugriffssteuerung zum Verwalten des Zugriffs auf Ihre Azure-Abonnementressourcen](role-assignments-portal.md)
- [Berechtigungen in Azure Security Center](../security-center/security-center-permissions.md)
