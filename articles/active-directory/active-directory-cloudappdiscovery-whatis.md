---
title: Suchen nach nicht verwalteten Cloudanwendungen per Cloud App Discovery in Azure Active Directory| Microsoft-Dokumentation
description: Bietet Informationen zum Suchen und Verwalten von Anwendungen mit Cloud App Discovery, zu den Vorteilen und zu der Funktionsweise.
services: active-directory
keywords: Cloud App Discovery, Verwalten von Anwendungen
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: db968bf5-22ae-489f-9c3e-14df6e1fef0a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: markvi
ms.reviewer: nigu
ms.translationtype: HT
ms.sourcegitcommit: 7dceb7bb38b1dac778151e197db3b5be49dd568a
ms.openlocfilehash: 35b898aa3c03aeef914a7df574ac65a22a6c7bec
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="find-unmanaged-cloud-applications-with-cloud-app-discovery"></a>Suchen nach nicht verwalteten Cloudanwendungen per Cloud App Discovery
## <a name="summary"></a>Zusammenfassung

Cloud App Discovery ist eine Funktion von Azure Active Directory Premium, mit dem Sie nicht verwaltete Cloudanwendungen ermitteln können, die von den Personen in Ihrer Organisation verwendet werden. In modernen Unternehmen sind den IT-Abteilungen häufig nicht alle Cloudanwendungen bekannt, die die Mitarbeiter des Unternehmens für ihre Arbeit verwenden. Es ist verständlich, warum Administratoren sich Sorgen um den unberechtigten Zugriff auf Unternehmensdaten, mögliche Datenlecks und andere Sicherheitsrisiken machen. Dieses fehlende Bewusstsein kann die Erstellung eines Plans für den Umgang mit diesen Sicherheitsrisiken als sehr anspruchsvolle Aufgabe erscheinen lassen.

> [!TIP] 
> Es wurden Verbesserungen an Cloud App Discovery in Azure Active Directory (Azure AD) vorgenommen. Diese Verbesserungen werden durch die [Integration in Microsoft Cloud App Security](https://portal.cloudappsecurity.com) verstärkt.

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


