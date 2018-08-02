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
ms.topic: conceptual
ms.date: 07/23/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: seohack1
ms.openlocfilehash: d1a0e46fe348bbc60a4d02a4727a9bb27cb26742
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223295"
---
# <a name="troubleshoot-rbac-in-azure"></a>Beheben von RBAC-Fehlern in Azure

In diesem Artikel werden häufig gestellte Fragen über die rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) beantwortet. Sie erfahren also, was Sie erwarten können, wenn Sie die Rollen im Azure-Portal verwenden und wie Sie Zugriffsprobleme lösen können.

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

## <a name="rbac-changes-are-not-being-detected"></a>Keine Erkennung von RBAC-Änderungen

Azure Resource Manager speichert Konfigurationen und Daten manchmal zwischen, um die Leistung zu verbessern. Beim Erstellen oder Löschen von Rollenzuweisungen kann es bis zu 30 Minuten dauern, bis Änderungen wirksam werden. Wenn Sie das Azure-Portal, Azure PowerShell oder die Azure CLI verwenden, können Sie eine Aktualisierung der Rollenzuweisungsänderungen erzwingen, indem Sie sich abmelden und wieder anmelden. Wenn Sie Rollenzuweisungsänderungen mit REST-API-Aufrufen vornehmen, können Sie eine Aktualisierung erzwingen, indem Sie das Zugriffstoken aktualisieren.

## <a name="next-steps"></a>Nächste Schritte
* [Verwalten des Zugriffs mithilfe der RBAC und des Azure-Portals](role-assignments-portal.md)
* [Anzeigen von Aktivitätsprotokollen für RBAC-Änderungen](change-history-report.md)

