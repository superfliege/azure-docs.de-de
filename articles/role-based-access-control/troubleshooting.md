---
title: Beheben von RBAC-Fehlern in Azure | Microsoft-Dokumentation
description: Beheben von Problemen mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) von Azure.
services: azure-portal
documentationcenter: na
author: rolyon
manager: mtillman
ms.assetid: df42cca2-02d6-4f3c-9d56-260e1eb7dc44
ms.service: role-based-access-control
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: seohack1
ms.openlocfilehash: 557d3330ef155181c050a18b14d31b65ba1f2dcf
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295395"
---
# <a name="troubleshooting-rbac-in-azure"></a>Beheben von RBAC-Fehlern in Azure

In diesem Artikel werden häufig gestellte Fragen über die rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) beantwortet. Sie erfahren also, was Sie erwarten können, wenn Sie die Rollen im Azure-Portal verwenden und wie Sie Zugriffsprobleme lösen können. Diese drei Rollen decken alle Ressourcentypen ab:

* Owner (Besitzer)  
* Mitwirkender  
* Leser  

Sowohl Besitzer als auch Mitwirkende haben Vollzugriff auf alle Verwaltungsfunktionen, Mitwirkende können jedoch anderen Benutzern oder Gruppen keinen Zugriff gewähren. Die Leserolle werden wir aufgrund ihrer umfassenden Eigenschaften ausführlicher beschreiben. Informationen zum Gewähren des Zugriffs finden Sie unter [Verwalten des Zugriffs mithilfe der RBAC und des Azure-Portals](role-assignments-portal.md).

## <a name="app-service"></a>App Service
### <a name="write-access-capabilities"></a>Schreibzugriff
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

### <a name="dealing-with-related-resources"></a>Umgang mit zugehörigen Ressourcen
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

## <a name="azure-functions"></a>Azure-Funktionen
Einige Funktionen von [Azure Functions](../azure-functions/functions-overview.md) erfordern Schreibzugriff. Wenn einem Benutzer beispielsweise die Leserolle zugewiesen ist, können Sie nicht die Funktionen in einer Funktions-App anzeigen. Im Portal wird **(Kein Zugriff)** angezeigt.

![Funktions-Apps ohne Zugriff](./media/troubleshooting/functionapps-noaccess.png)

Ein Leser kann auf die Registerkarte **Plattformfeatures** und dann auf **Alle Einstellungen** klicken, um einige Einstellungen im Zusammenhang mit einer Funktions-App (ähnlich wie bei einer Web-App) anzuzeigen, kann diese Einstellungen jedoch nicht ändern.

## <a name="virtual-machine"></a>Virtueller Computer
Ähnlich wie bei Web-Apps erfordern einige Funktionen auf dem Blatt "Virtueller Computer" Schreibzugriff auf den virtuellen Computer oder auf andere Ressourcen in der Ressourcengruppe.

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

## <a name="next-steps"></a>Nächste Schritte
* [Verwalten des Zugriffs mithilfe der RBAC und des Azure-Portals](role-assignments-portal.md)
* [Anzeigen von Aktivitätsprotokollen für RBAC-Änderungen](change-history-report.md)

