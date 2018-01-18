---
title: Suchen nach nicht verwalteten Cloudanwendungen per Cloud App Discovery in Azure Active Directory| Microsoft-Dokumentation
description: Bietet Informationen zum Suchen und Verwalten von Anwendungen mit Cloud App Discovery, zu den Vorteilen und zu der Funktionsweise.
services: active-directory
keywords: Cloud App Discovery, Verwalten von Anwendungen
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: db968bf5-22ae-489f-9c3e-14df6e1fef0a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 1d0ad06fc7eec07f8e1e0ba47121b6eec01c87df
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2018
---
# <a name="find-unmanaged-cloud-applications-with-cloud-app-discovery"></a>Suchen nach nicht verwalteten Cloudanwendungen per Cloud App Discovery
## <a name="summary"></a>Zusammenfassung

Cloud App Discovery in Azure Active Directory bietet jetzt eine verbesserte Ermittlungsoberfläche ohne Agents, die von Microsoft Cloud App Security unterstützt wird. Um Cloud App Discovery zu verwenden, melden Sie sich einfach mit Ihren Azure AD Premium P1-Anmeldeinformationen an. Dieses Update steht allen Azure AD Premium P1-Kunden ohne zusätzliche Kosten zur Verfügung. Beginnen Sie mit dem Artikel [Einrichten von Cloud App Discovery in Azure AD](https://docs.microsoft.com/azure/active-directory/cloudappdiscovery-get-started), und probieren Sie [Microsoft Cloud App Security](https://portal.cloudappsecurity.com/) dann aus.

> [!IMPORTANT] 
> Die aktuelle Azure AD Cloud App Discovery-Benutzeroberfläche mit auf Agents basierender Ermittlung wird am 5. März 2018 eingestellt. Nach diesem Datum werden die Agents deaktiviert und die Daten gelöscht. Bitte werden Sie in dieser Sache vor dem 5. März aktiv, um die neue Benutzeroberfläche betriebsbereit zu haben und eine Dienstunterbrechung zu vermeiden.  
 
**Cloud App Discovery ermöglicht Ihnen Folgendes:**

* Suchen nach den verwendeten Cloudanwendungen und Messen der Nutzung anhand der Benutzeranzahl, dem Datenverkehrsvolumen oder der Anzahl von Webanforderungen in der Anwendung.
* Identifizieren der Benutzer, die eine Anwendung verwenden.
* Exportieren von Daten für eine Offlineanalyse.
* Unterstellen dieser Anwendungen unter IT-Kontrolle und Aktivieren des einmaligen Anmeldens für die Benutzerverwaltung.

## <a name="how-it-works"></a>So funktioniert's
1. Agents für die Anwendungsnutzung werden auf den Computern des Benutzers installiert.
2. Die von den Agents erfassten Daten zur Anwendungsnutzung werden über einen sicheren, verschlüsselten Kanal an den Cloud App Discovery-Dienst gesendet.
3. Der Cloud App Discovery-Dienst wertet die Daten aus und erstellt Berichte.

![Cloud App Discovery-Diagramm](./media/active-directory-cloudappdiscovery/cad01.png)


## <a name="next-steps"></a>Nächste Schritte
* [Überlegungen zu Sicherheit und Datenschutz in Cloud App Discovery](active-directory-cloudappdiscovery-security-and-privacy-considerations.md)  
* [Cloud App Discovery-Registrierungseinstellungen für Proxyserver mit benutzerdefinierten Ports](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md)
* [Cloud App Discovery – Agent-Änderungsprotokoll ](http://social.technet.microsoft.com/wiki/contents/articles/24616.cloud-app-discovery-agent-changelog.aspx)
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)

