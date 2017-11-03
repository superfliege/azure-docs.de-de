---
title: Verbinden von Microsoft Advanced Threat Analytics mit Azure Security Center | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure Security Center mit Microsoft Advanced Threat Analytics integrieren.
services: security-center
documentationcenter: na
author: YuriDio
manager: MBaldwin
editor: 
ms.assetid: 5d80bf91-16c3-40b3-82fc-e0805e6708db
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2017
ms.author: yurid
ms.openlocfilehash: e1b9e598af3b55c1d9591e5c1e529a80ae3319ca
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2017
---
# <a name="connecting-microsoft-advanced-threat-analytics-to-azure-security-center"></a>Verbinden von Microsoft Advanced Threat Analytics mit Azure Security Center
Dieses Dokument soll Sie bei der Konfiguration der Integration zwischen Microsoft Advanced Threat Analytics und dem Azure Security Center unterstützen.

## <a name="why-add-advanced-threat-analytics-data"></a>Vorteile des Hinzufügens von Daten aus Advanced Threat Analytics
[Advanced Threat Analytics](https://docs.microsoft.com/advanced-threat-analytics/what-is-ata) ist eine lokale Plattform, die verdächtiges Benutzerverhalten erkennt. Wenn Sie verbunden sind, können Sie im Security Center verdächtige Vorgänge anschauen, die Advanced Threat Analytics erkannt hat. Durch diese Integration können Sie alle Sicherheitswarnungen, die im Zusammenhang mit Ihren hybriden Cloudworkloads im Security Center stehen, ansehen, korrelieren lassen und untersuchen. 

## <a name="how-do-i-configure-this-integration"></a>Vorgehensweise bei der Konfiguration der Integration
Folgen Sie diesen Schritten zur Konfiguration dieser Integration, wenn Sie Advanced Threat Analytics bereits installiert haben und es lokal einwandfrei funktioniert:

1. Melden Sie sich im Advanced Threat Analytics-Center an, und navigieren Sie zum Advanced Threat Analytics-Portal.
2. Klicken Sie im linken Bereich auf **Syslog-Server**.

    ![Syslog-Server](./media/security-center-ata-integration/security-center-ata-integration-fig1.png)

3. Geben Sie in dem Feld **Syslog server endpoint** (Syslog-Server-Endpunkt) 127.0.0.7 (erforderliche Adresse) und anschließend im Port die Ziffern 5114 (empfohlen) ein. Da es sich bei der Portnummer nur um eine Empfehlung handelt, sollte jeder eindeutige Port verwendet werden können. Lassen Sie alle anderen Optionen unverändert, und klicken Sie auf **Speichern**.
4. Klicken Sie im linken Bereich auf **Benachrichtigungen**, und aktivieren Sie, wie in der folgenden Abbildung dargestellt, alle Syslog-Benachrichtigungen (empfohlen):

    ![Benachrichtigungen](./media/security-center-ata-integration/security-center-ata-integration-fig2.png)

5. Klicken Sie auf **Speichern**.
6. Öffnen Sie das Dashboard **Security Center**.
7. Klicken Sie im linken Bereich auf **Sicherheitslösungen**.
8. Klicken Sie unter **Advanced Threat Analytics** auf **HINZUFÜGEN**.

    ![ATA](./media/security-center-ata-integration/security-center-ata-integration-fig3.png)
    
9. Gehen Sie zum letzten Schritt zurück, und klicken Sie auf **Agent herunterladen**.

    ![ATA](./media/security-center-ata-integration/security-center-ata-integration-fig4.png)

10. Wählen Sie auf der Seite **Add new non-Azure computer** (neuen Nicht-Azure-Computer hinzufügen) den Arbeitsbereich aus.

    ![Nicht-Azure](./media/security-center-ata-integration/security-center-ata-integration-fig5.png)

11. Laden Sie auf der Seite **Direkt-Agent** den passenden Windows-Agent herunter, und notieren Sie sich die **Arbeitsbereich-ID** und den **Primärschlüssel**.

    ![Direkt-Agent](./media/security-center-ata-integration/security-center-ata-integration-fig6.png)

12. Installieren Sie diesen Agent im Advanced Threat Analytics-Center. Stellen Sie bei der Installation sicher, dass die Option **Connect the agent to Azure Log Analytics (OMS)** (Den Agent mit der Azure-Protokollanalyse (OMS) verbinden) ausgewählt ist, und geben Sie die *Arbeitsbereich-ID* und den *Primärschlüssel* ein, wenn Sie dazu aufgefordert werden.


Sobald Sie die Installation beendet haben, ist die Integration abgeschlossen, und Ihnen werden neue Warnungen, die von Advanced Threat Analytics an das Security Center gesendet werden, unter **Sicherheitswarnungen** und **Suchen** angezeigt. Die Lösung erscheint auf der Seite **Sicherheitslösungen** unter **Verbundene Lösungen**. 

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument wurde erläutert, wie Sie Microsoft Advanced Threat Analytics mit Security Center verbinden. Weitere Informationen zu Security Center finden Sie in den folgenden Artikeln:

* [Connecting Azure Active Directory Identity Protection to Azure Security Center (Verbinden von Azure Active Directory Identity Protection mit Azure Security Center)](security-center-aadip-integration.md)
* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Verwalten von Sicherheitsempfehlungen in Azure Security Center:](security-center-recommendations.md) Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md) : Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center](security-center-partner-solutions.md) : Erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen überwacht wird.
- [Azure Security Center-Datensicherheit:](security-center-data-security.md) Hier erfahren Sie, wie Daten in Security Center verwaltet und geschützt werden.
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Neuigkeiten und Informationen zur Azure-Sicherheit.


