---
title: Problembehandlung von RBAC für Azure-Ressourcen | Microsoft-Dokumentation
description: Beheben von Problemen mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) für Azure-Ressourcen.
services: azure-portal
documentationcenter: na
author: rolyon
manager: mtillman
ms.assetid: df42cca2-02d6-4f3c-9d56-260e1eb7dc44
ms.service: role-based-access-control
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/18/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: seohack1
ms.openlocfilehash: 7b27c811214def7f5646f886b955d035a50c0725
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/18/2019
ms.locfileid: "56342472"
---
# <a name="troubleshoot-rbac-for-azure-resources"></a>Problembehandlung von RBAC für Azure-Ressourcen

In diesem Artikel werden häufig gestellte Fragen über die rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) für Azure-Ressourcen beantwortet. Sie erfahren also, was Sie erwarten können, wenn Sie die Rollen im Azure-Portal verwenden und wie Sie Zugriffsprobleme lösen können.

## <a name="problems-with-rbac-role-assignments"></a>Probleme mit RBAC-Rollenzuweisungen

- Falls Sie keine Rollenzuweisung hinzufügen können, da die Option **Rollenzuweisung hinzufügen** deaktiviert ist oder ein Berechtigungsfehler auftritt, vergewissern Sie sich, dass Sie eine Rolle mit der Berechtigung `Microsoft.Authorization/roleAssignments/*` für den Bereich verwenden, dem Sie die Rolle zuweisen möchten. Sollten Sie nicht über diese Berechtigung verfügen, wenden Sie sich an den zuständigen Abonnementadministrator.
- Falls beim Erstellen einer Ressource ein Berechtigungsfehler auftritt, vergewissern Sie sich, dass Sie eine Rolle verwenden, die über die Berechtigung zum Erstellen von Ressourcen im ausgewählten Bereich verfügt. Beispielsweise müssen Sie unter Umständen Mitwirkender sein. Sollten Sie nicht über die Berechtigung verfügen, wenden Sie sich an den zuständigen Abonnementadministrator.
- Falls beim Erstellen oder Aktualisieren eines Supporttickets ein Berechtigungsfehler auftritt, vergewissern Sie sich, dass Sie eine Rolle verwenden, die über die Berechtigung `Microsoft.Support/*` verfügt (beispielsweise [Mitwirkender bei Supportanfragen](built-in-roles.md#support-request-contributor)).
- Tritt beim Zuweisen einer Rolle ein Fehler mit dem Hinweis auf, dass die Anzahl von Rollenzuweisungen überschritten wurde, weisen Sie Rollen stattdessen Gruppen zu, um die Anzahl von Rollenzuweisungen zu verringern. Azure unterstützt pro Abonnement bis zu **2.000** Rollenzuweisungen.

## <a name="problems-with-custom-roles"></a>Probleme mit benutzerdefinierten Rollen

- Falls Sie eine vorhandene benutzerdefinierte Rollen nicht aktualisieren können, vergewissern Sie sich, dass Sie über die Berechtigung `Microsoft.Authorization/roleDefinition/write` verfügen.
- Sollten Sie eine vorhandene benutzerdefinierte Rolle nicht aktualisieren können, überprüfen Sie, ob im Mandanten zuweisbare Bereiche gelöscht wurden. Die Eigenschaft `AssignableScopes` für eine benutzerdefinierte Rolle steuert, [wer zum Erstellen, Löschen, Aktualisieren oder Anzeigen der benutzerdefinierten Rolle berechtigt ist](custom-roles.md#who-can-create-delete-update-or-view-a-custom-role).
- Sollte beim Erstellen einer neuen Rolle ein Fehler mit dem Hinweis auftreten, dass das Limit für Rollendefinitionen überschritten wurde, löschen Sie alle benutzerdefinierten Rollen, die nicht verwendet werden. Sie können auch versuchen, vorhandene benutzerdefinierte Rollen zu konsolidieren oder wiederzuverwenden. Azure unterstützt bis zu **2.000** benutzerdefinierte Rollen in einem Mandanten.
- Falls Sie eine benutzerdefinierte Rolle nicht löschen können, überprüfen Sie, ob die benutzerdefinierte Rolle noch von mindestens einer Rollenzuweisung verwendet wird.

## <a name="recover-rbac-when-subscriptions-are-moved-across-tenants"></a>Wiederherstellen von RBAC beim übergreifenden Verschieben von Abonnements auf Mandanten

- Schritte zum Übertragen eines Abonnements auf einen anderen Mandanten finden Sie unter [Übertragen des Besitzes eines Azure-Abonnements auf ein anderes Konto](../billing/billing-subscription-transfer.md).
- Wenn Sie ein Abonnement auf einen anderen Mandanten übertragen, werden alle Rollenzuweisungen dauerhaft aus dem Quellmandanten gelöscht und nicht zum Zielmandanten migriert. Sie müssen Ihre Rollenzuweisungen auf dem Zielmandanten neu erstellen.
- Globale Administratoren ohne Zugriff auf ein Abonnement können mithilfe der **Zugriffsverwaltung für Azure-Ressourcen** vorübergehend ihren [Zugriff erhöhen](elevate-access-global-admin.md), um wieder Zugriff auf das Abonnement zu erlangen.

## <a name="rbac-changes-are-not-being-detected"></a>Keine Erkennung von RBAC-Änderungen

Azure Resource Manager speichert Konfigurationen und Daten manchmal zwischen, um die Leistung zu verbessern. Beim Erstellen oder Löschen von Rollenzuweisungen kann es bis zu 30 Minuten dauern, bis Änderungen wirksam werden. Wenn Sie das Azure-Portal, Azure PowerShell oder die Azure CLI verwenden, können Sie eine Aktualisierung der Rollenzuweisungsänderungen erzwingen, indem Sie sich abmelden und wieder anmelden. Wenn Sie Rollenzuweisungsänderungen mit REST-API-Aufrufen vornehmen, können Sie eine Aktualisierung erzwingen, indem Sie das Zugriffstoken aktualisieren.

## <a name="web-app-features-that-require-write-access"></a>Web-App-Features, für die Schreibzugriff erforderlich ist

Wenn Sie einem Benutzer schreibgeschützten Zugriff für eine einzelne Web-App gewähren, sind einige Features deaktiviert, von denen Sie das unter Umständen nicht erwartet haben. Die folgenden Verwaltungsfunktionen erfordern **Schreibzugriff** auf eine Web-App (entweder als Mitwirkender oder Besitzer) und stehen nicht zur Verfügung, wenn der Benutzer nur über Lesezugriff für die Web-App verfügt.

* Befehle (z.B. starten, anhalten usw.)
* Änderung von Einstellungen wie allgemeine Konfigurationen, Skalierungseinstellungen, Sicherungseinstellungen und Überwachungseinstellungen
* Zugriff auf Anmeldedaten für die Veröffentlichung oder andere geheime Schlüssel wie App- und Verbindungseinstellungen
* Streamingprotokolle
* Konfiguration von Diagnoseprotokollen
* Konsole (Eingabeaufforderung)
* Aktive und kürzlich vorgenommene Bereitstellungen (für fortlaufende Bereitstellungen lokaler Gits)
* Geschätzte Ausgaben
* Webtests
* Virtuelles Netzwerk (für Leser nur sichtbar, wenn ein virtuelles Netzwerk zuvor von einem Benutzer mit Schreibzugriff konfiguriert wurde)

Wenn Sie auf keine dieser Kacheln zugreifen können, fragen Sie den Administrator nach Zugriff als Mitwirkender auf die Web-App.

## <a name="web-app-resources-that-require-write-access"></a>Web-App-Ressourcen, für die Schreibzugriff erforderlich ist

Web-Apps können aufgrund verschiedener miteinander verknüpfter Ressourcen kompliziert sein. Im Folgenden ist eine typische Ressourcengruppe mit einigen Websites dargestellt:

![Web-App-Ressourcengruppe](./media/troubleshooting/website-resource-model.png)

Wenn Sie daher lediglich auf die Web-App Zugriff gewähren, sind viele Funktionen des Blatts „Website“ im Azure-Portal deaktiviert.

Die folgenden Elemente erfordern **Schreibzugriff** auf den **App Service-Plan** für Ihre Website:  

* Anzeigen des Tarifs für die Web-App (Free oder Standard)  
* Skalierungskonfiguration (Anzahl der Instanzen, Größe des virtuellen Computers, Einstellungen für automatische Skalierung)  
* Kontingente (Speicher, Bandbreite, CPU)  

Die folgenden Elemente erfordern **Schreibzugriff** auf die gesamte **Ressourcengruppe**, die Ihre Website umfasst:  

* SSL-Zertifikate und -Bindungen (SSL-Zertifikate können von Websites derselben Ressourcengruppe und desselben geografischen Standorts gemeinsam genutzt werden)  
* Warnregeln  
* Einstellungen für automatische Skalierung  
* Application Insights-Komponenten  
* Webtests  

## <a name="virtual-machine-features-that-require-write-access"></a>Features virtueller Computer, für die Schreibzugriff erforderlich ist

Ähnlich wie bei Web-Apps erfordern einige Funktionen auf dem Blatt „Virtueller Computer“ Schreibzugriff auf den virtuellen Computer oder auf andere Ressourcen in der Ressourcengruppe.

Virtuelle Computer stehen in Verbindung mit Domänennamen, virtuellen Netzwerken, Speicherkonten und Warnungsregeln.

Die folgenden Elemente erfordern **Schreibzugriff** auf den **virtuellen Computer**:

* Endpunkte  
* IP-Adressen  
* Datenträger  
* Erweiterungen  

Die folgenden Elemente erfordern **Schreibzugriff** auf den **virtuellen Computer** und die **Ressourcengruppe** (zusammen mit dem Domänennamen), in der sich der virtuelle Computer befindet:  

* Verfügbarkeitsgruppe  
* Satz mit Lastenausgleich  
* Warnregeln  

Wenn Sie auf keine dieser Kacheln zugreifen können, fragen Sie den Administrator nach Zugriff als Mitwirkender auf diese Ressourcengruppe.

## <a name="azure-functions-and-write-access"></a>Azure Functions und Schreibzugriff

Einige Funktionen von [Azure Functions](../azure-functions/functions-overview.md) erfordern Schreibzugriff. Wenn einem Benutzer beispielsweise die Leserolle zugewiesen ist, können Sie nicht die Funktionen in einer Funktions-App anzeigen. Im Portal wird **(Kein Zugriff)** angezeigt.

![Funktions-Apps ohne Zugriff](./media/troubleshooting/functionapps-noaccess.png)

Ein Leser kann auf die Registerkarte **Plattformfeatures** und dann auf **Alle Einstellungen** klicken, um einige Einstellungen im Zusammenhang mit einer Funktions-App (ähnlich wie bei einer Web-App) anzuzeigen, kann diese Einstellungen jedoch nicht ändern.

## <a name="next-steps"></a>Nächste Schritte
* [Verwalten des Zugriffs auf Azure-Ressourcen mit RBAC und dem Azure-Portal](role-assignments-portal.md)
* [Anzeigen von Aktivitätsprotokollen für RBAC-Änderungen an Azure-Ressourcen](change-history-report.md)

