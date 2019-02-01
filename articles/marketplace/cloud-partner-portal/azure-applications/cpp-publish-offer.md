---
title: Veröffentlichen eines Azure-Anwendungsangebots – Azure Marketplace | Microsoft-Dokumentation
description: Beschreibt den Prozess und die Schritte zum Veröffentlichen eines Azure-Anwendungsangebots im Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: pbutlerm
ms.openlocfilehash: 2adf07cf2337611b9136af47ce6a35b617e2e9ff
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55177031"
---
# <a name="publish-azure-application-offer"></a>Veröffentlichen eines Azure-Anwendungsangebots

Nachdem Sie die Informationen auf der Seite **Neues Angebot** angegeben haben, um ein Angebot zu erstellen, können Sie das Angebot veröffentlichen. Klicken Sie auf **Veröffentlichen**, um den Veröffentlichungsprozess zu starten.

Das folgende Diagramm zeigt die wichtigsten Schritte des Veröffentlichungsprozesses, die ausgeführt werden müssen, um ein Angebot live zu schalten.

![Angebotsveröffentlichungsschritte](./media/offer-publishing-steps.png)


## <a name="detailed-description-of-publishing-steps"></a>Ausführliche Beschreibung der Veröffentlichungsschritte

In der folgenden Tabelle sind die einzelnen Veröffentlichungsschritte sowie deren voraussichtliche Dauer aufgeführt und beschrieben.  Zeitschätzungen in „Tagen“ beziehen sich auf Werktage, ohne Wochenenden und Feiertage.

|  **Veröffentlichungsschritt**           | **Time**    | **Beschreibung**                                                            |
|  -------------------           | --------    | ---------------                                                            |
| Überprüfen der Voraussetzungen         | < 15 Minuten    | Die Angebotsinformationen und -einstellungen werden überprüft.                        |
| Überprüfen der beeinflussten Umsatzeinstellungen | < 15 Minuten  | Die Zuordnung der Azure-Ressourcennutzung wird für das Angebot überprüft.             |
| Zertifizierung                  | < 1 Tag     | Das Angebot wird vom Azure-Zertifizierungsteam analysiert. Das Angebot wird auf Viren, Schadsoftware, Sicherheitskonformität und Sicherheitsprobleme untersucht. Das Angebot wird überprüft, um festzustellen, ob es die Eignungskriterien erfüllt. Weitere Informationen finden Sie unter [Voraussetzungen](./cpp-prerequisites.md). Sollte ein Problem gefunden werden, erhalten Sie entsprechendes Feedback. |
| Überprüfung der Testversion          | < 2 Stunden   | (Optional) Wenn ein Testlaufwerk vorhanden ist, überprüft Microsoft, ob dieses bereitgestellt und repliziert werden kann.  |
| Paketerstellung und Registrierung von generierten Leads | < 1 Stunde  | Die technischen Ressourcen des Angebots werden für die Verwendung durch die Kunden gepackt, und die Leadsysteme werden konfiguriert und bereitgestellt. |
|  Abzeichnung des Verlegers             |  manual    | Letzte Überprüfung und Bestätigung durch den Herausgeber, bevor das Angebot live geschaltet wird. Das Angebot ist jetzt als Vorschau verfügbar.  Sie können Ihr Angebot in den ausgewählten Abonnements (in den Schritten für die Angebotsinformationen) bereitstellen, um sicherzugehen, dass es alle Ihre Anforderungen erfüllt.  Nachdem Sie Ihr Angebot überprüft haben, wählen Sie **Freischalten** aus, damit der nächste Schritt für Ihr Angebot ausgeführt werden kann. |
| Prüfung durch Microsoft                | 7-14 Tage | Microsoft überprüft Ihre Azure-Anwendung ganzheitlich und sendet Ihnen E-Mails, wenn Probleme entdeckt werden.  Die Dauer dieses Schrittes hängt von der Komplexität der Anwendung, den aufgedeckten Problemen ab und davon, wie schnell Sie darauf reagieren.  |
| Live                           | < 1 Tag | Das Angebot wird veröffentlicht, in den angegebenen Regionen repliziert und für die Öffentlichkeit verfügbar gemacht. |
|   |&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|   |

 
Sie können den Veröffentlichungsvorgang auf der Registerkarte **Status** für Ihr Angebot im Cloud-Partnerportal überwachen.

![Registerkarte „Status“ für ein Azure-App-Angebot](./media/offer-status-tab.png)

Nach Abschluss des Veröffentlichungsvorgangs wird Ihr Angebot in der [Microsoft Azure Marketplace-Anwendungskategorie](https://azuremarketplace.microsoft.com/marketplace/apps/) aufgelistet.



## <a name="errors-and-review-feedback"></a>Fehler und Prüfungsfeedback

Zusätzlich zum Veröffentlichungsstatus Ihres Angebots werden auf der Registerkarte **Status** auch Fehlermeldungen und Feedback aus dem Schritt **Review durch Microsoft** angezeigt.  In der Regel werden bei der Prüfung festgestellte Probleme als Pull-Requests (PR) bezeichnet.  Jeder PR ist mit einem Onlineelement von Visual Studio Team Services (VSTS, umbenannt in [Azure DevOps](https://azure.microsoft.com/services/devops/)) verknüpft, das Details zu dem Problem enthält.  Die folgende Abbildung zeigt ein Beispiel für einen PR-Verweis einer Prüfung.  In komplexeren Situationen erhalten Sie E-Mails von den Prüfungs- und Supportteams. 

![Registerkarte „Status“ mit Prüfungsfeedback](./media/status-tab-ms-review.png)

Sie müssen jedes gemeldete Problem beheben, bevor der Veröffentlichungsvorgang für das Angebot fortgesetzt wird.  Das folgende Diagramm veranschaulicht, wie sich dieser Feedbackvorgang auf den Veröffentlichungsvorgang auswirkt.

![Veröffentlichungsschritte mit VSTS-Feedback](./media/pub-flow-vsts-access.png)


### <a name="vsts-access"></a>VSTS-Zugriff

Zum Anzeigen der VSTS-Elemente, auf die im Prüfungsfeedback verwiesen wird, müssen Herausgeber über eine entsprechende Autorisierung verfügen.  Andernfalls wird neuen Herausgebern die Antwortseite `401 - Not Authorized` angezeigt.  Um den Zugriff auf das VSTS-System zur Angebotsprüfung anzufordern, führen Sie die folgenden Schritte aus:

1. Stellen Sie die folgenden Informationen zusammen:
    - Name und ID des Herausgebers
    - Angebotstyp (Azure-App), Angebotsnamen und SKU-ID
    - Pull-Request-Link, z. B.: `https://solutiontemplates.visualstudio.com/marketplacesolutions/_git/contoso/pullrequest/<number>` Diese URL kann aus der Benachrichtigungsnachricht oder der Adresse der Antwortseite 401 abgerufen werden.
    - Die E-Mail-Adresse(n) der Personen aus Ihrer Veröffentlichungsorganisation, für die Sie Zugriff erhalten möchten.  Dies sollten die Eigentümeradresse(n) sein, die Sie bei der Registrierung als Herausgeber im Cloud-Partnerportal angegeben haben.
2. Erstellen Sie einen Supportvorfall.  Wählen Sie in der Titelleiste des Cloud-Partnerportals die Schaltfläche **Hilfe** und dann **Support** aus dem Menü aus.  Ihr Standardwebbrowser sollte gestartet werden und zur Microsoft-Seite für den neuen Supportvorfall navigieren.  (Möglicherweise müssen Sie sich zunächst anmelden.)
3. Geben Sie **Marketplace-Onboarding** als **Problemtyp** und **Zugriffsproblem** als **Kategorie** an, und wählen Sie dann **Anforderung starten** aus.

    ![Supportticketkategorie](./media/support-incident1.png)

4. Geben Sie auf der Seite **Schritt 1 von 2** Ihre Kontaktinformationen ein, und wählen Sie **Weiter** aus.
5. Geben Sie auf der Seite **Schritt 2 von 2** einen Vorfalltitel (z. B. `Request VSTS access`) sowie die Informationen an, die Sie im ersten Schritt (oben) gesammelt haben.  Lesen und akzeptieren Sie die Vereinbarung, und wählen Sie dann **Senden** aus.

Wenn die Erstellung des Vorfalls erfolgreich war, wird eine Bestätigungsseite angezeigt.  Speichern Sie die Bestätigungsinformationen zur künftigen Bezugnahme.  Der Microsoft-Support sollte innerhalb weniger Werktage auf Ihre Zugriffsanforderung antworten.


## <a name="next-steps"></a>Nächste Schritte

Sobald eine Azure-App veröffentlicht wurde, können Sie [das bestehende Angebot aktualisieren](./cpp-update-existing-offer.md), um den sich ändernden geschäftlichen oder technischen Anforderungen gerecht zu werden. 
