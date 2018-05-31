---
title: Azure AD Connect Health – Diagnostizieren von Synchronisierungsfehlern aufgrund doppelter Attribute | Microsoft-Dokumentation
description: In diesem Dokument werden der Diagnoseprozess für Synchronisierungsfehler aufgrund doppelter Attribute und die potenzielle Behebung von Szenarien mit verwaisten Objekten direkt über das Azure-Portal beschrieben.
services: active-directory
documentationcenter: ''
author: zhiweiw
manager: maheshu
editor: billmath
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2018
ms.author: zhiweiw
ms.openlocfilehash: 0a345fd3443fb33d6f5912c8a04677091e20ecc8
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/18/2018
ms.locfileid: "34305596"
---
# <a name="duplicate-attribute-sync-errors-diagnosis-and-remediation"></a>Synchronisierungsfehler aufgrund doppelter Attribute – Diagnose und Lösung 

## <a name="overview"></a>Übersicht
In Bezug auf Synchronisierungsfehler gehen wir bei Azure Active Directory Connect Health noch einen Schritt weiter. Es wird eine Self-Service-Lösungsoberfläche eingeführt, die zur Problembehandlung für Synchronisierungsfehler aufgrund doppelter Attribute und zur Behebung verwaister Objekte für Azure AD verwendet werden kann. Der wichtigste Vorteil des Diagnosefeatures ist:
- Bereitstellung eines Diagnoseverfahrens zum Eingrenzen von Szenarien mit Synchronisierungsfehlern aufgrund doppelter Attribute und Angeben von spezifischen Lösungen
- Anwenden einer Lösung für dedizierte Szenarien aus Azure AD, um den Fehler mit nur einem Klick zu beheben
- Kein Upgrade- oder Konfigurationsaufwand zur Aktivierung dieses Features
(siehe ausführlichere Informationen zu [Resilienz bei doppelten Attributen](https://aka.ms/dupattributeresdocs) in Azure AD)

## <a name="problems"></a>Probleme
### <a name="common-scenario"></a>Allgemeines Szenario
Wenn die Synchronisierungsfehler **QuarantinedAttributeValueMustBeUnique** und **AttributeValueMustBeUnique** auftreten, kommt es in Azure AD häufig zu einem Benutzerprinzipalnamen- oder Proxyadressenkonflikt. Sie können die Synchronisierungsfehler beheben, indem Sie das in Konflikt stehende Quellobjekt auf lokaler Seite aktualisieren. Der Synchronisierungsfehler wird nach der folgenden Synchronisierung behoben. In der Abbildung unten ist beispielsweise angegeben, dass für zwei Benutzer ein Konflikt in Bezug auf den Benutzerprinzipalnamen (UserPrincipalName) *Joe.J@contoso.com* besteht. Die in Konflikt stehenden Objekte werden in Azure AD unter Quarantäne gestellt. 

![Diagnostizieren von Synchronisierungsfehlern – Allgemeines Szenario](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixCommonCase.png)

### <a name="orphaned-object-scenario"></a>Szenario: Verwaistes Objekt
Gelegentlich kann es vorkommen, dass ein vorhandener Benutzer den Quellanker verliert. Das Löschen des Quellobjekts erfolgte in der lokalen AD-Instanz, aber die Änderung des Löschsignals wurde nicht mit Azure AD synchronisiert. Gründe hierfür können ein Problem mit dem Synchronisierungsmodul oder die Migration einer Domäne sein. Wenn dasselbe Objekt wiederhergestellt oder neu erstellt wurde, sollte der logisch vorhandene Benutzer der Benutzer sein, der die Synchronisierung vom Quellanker durchführt. Für einen vorhandenen Benutzer als auf die Cloud beschränktes Objekt kann es auch sein, dass der in Konflikt stehende Benutzer mit Azure AD synchronisiert wird und nicht synchron mit dem vorhandenen Objekt abgeglichen werden kann. Es gibt keine Möglichkeit, den Quellanker direkt neu zuzuordnen. [Hier finden Sie weitere Informationen](https://support.microsoft.com/help/2647098). Beispiel: Das vorhandene Objekt in Azure AD behält die Lizenz von Joe bei. Das neu synchronisierte Objekt mit einem anderen Quellanker ist in einem Zustand mit doppeltem Attribut in Azure AD aufgetreten. Änderungen von Joe in der lokalen AD-Instanz können nicht auf den ursprünglichen Benutzer von Joe (vorhandenes Objekt) in Azure AD angewendet werden.  

![Diagnostizieren von Synchronisierungsfehlern – Szenario mit verwaistem Objekt](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixOrphanedCase.png)

## <a name="diagnostic-and-troubleshooting-steps-in-connect-health"></a>Schritte für die Diagnose und Problembehandlung in Connect Health 
Das Diagnosefeature unterstützt Benutzerobjekte (User) mit den folgenden doppelten Attributen:

| Attributname | Typen von Synchronisierungsfehlern|
| ------------------ | -----------------|
| UserPrincipalName | QuarantinedAttributeValueMustBeUnique oder AttributeValueMustBeUnique | 
| ProxyAddresses | QuarantinedAttributeValueMustBeUnique oder AttributeValueMustBeUnique | 
| SipProxyAddress | AttributeValueMustBeUnique (Wert für Attribut muss eindeutig sein) | 
| OnPremiseSecurityIdentifier |  AttributeValueMustBeUnique (Wert für Attribut muss eindeutig sein) |

>[!IMPORTANT]
> Für den Zugriff auf dieses Feature ist die Berechtigung **Globaler Administrator** oder **Mitwirkender** aus den RBAC-Einstellungen erforderlich. 
>

Indem Sie die Schritte im Azure-Portal befolgen, können Sie die Details zu Synchronisierungsfehlern eingrenzen und spezifischere Lösungen bereitstellen:

![Diagnostizieren von Synchronisierungsfehlern – Diagnoseschritte](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixSteps.png)

Sie können im Azure-Portal einige Schritte ausführen, um bestimmte behebbare Szenarien zu identifizieren:  
1.  In der Spalte mit dem Diagnosestatus wird angezeigt, ob ggf. ein Problembehandlungsablauf verfügbar ist, um die Fehlerbeschreibung einzugrenzen und unter Umständen die Behebung direkt in Azure Active Directory durchzuführen.
| Status | Was bedeutet das? |
| ------------------ | -----------------|
| Nicht gestartet | Sie haben diesen Diagnoseprozess nicht aufgerufen. Je nach Diagnoseergebnis besteht unter Umständen die Möglichkeit, den Synchronisierungsfehler direkt über das Portal zu beheben. |
| Manuelle Korrektur erforderlich | Der Fehler erfüllt nicht die Kriterien für eine verfügbare Korrektur über das Portal. Es kann sein, dass (1) die Typen der in Konflikt stehenden Objekte keine Benutzer sind oder (2) dass Sie die Diagnoseschritte bereits ausgeführt haben und im Portal keine Korrektur verfügbar ist. In diesem Fall ist die Korrektur der lokalen Seite weiterhin eine der Lösungen. [Erfahren Sie mehr zur lokalen Korrektur](https://support.microsoft.com/help/2647098). | 
| Synchronisierung ausstehend | Die Korrektur wurde angewendet. Es wird auf den nächsten Synchronisierungszyklus gewartet, um den Fehler zu beheben. |
>[!IMPORTANT]
> Die Spalte mit dem Diagnosestatus wird nach jedem Synchronisierungszyklus zurückgesetzt. 
>

2.  Wenn Sie auf dem Blatt mit den Fehlerdetails auf die Schaltfläche **Diagnose** klicken, müssen Sie einige Fragen beantworten und die Details zu Synchronisierungsfehlern identifizieren. Das Beantworten der Fragen hilft Ihnen für das Szenario mit verwaisten Objekten beim Identifizieren des Fehlers. 

3.  Wenn am Ende des Diagnosevorgangs die Schaltfläche **Schließen** angezeigt wird, bedeutet dies, dass basierend auf den angegebenen Antworten keine schnelle Korrektur über das Portal verfügbar ist. Weitere Informationen finden Sie unter der Lösung, die im letzten Schritt angezeigt wird. Die lokale Korrektur gilt weiterhin als Lösung. Nach dem Klicken auf die Schaltfläche „Schließen“ sehen Sie, dass der Status des aktuellen Synchronisierungsfehlers in **Manuelle Korrektur erforderlich** geändert wird. Der Status wird während des aktuellen Synchronisierungszyklus beibehalten.

4.  Nachdem als Fehlerbeschreibung „Verwaiste Objekte“ identifiziert wurde, können Sie Synchronisierungsfehler aufgrund von doppelten Attributen direkt über das Portal beheben. Klicken Sie auf die Schaltfläche **Fix anwenden**, um den Prozess auszulösen. Der Status des aktuellen Synchronisierungsfehlers wird in **Synchronisierung ausstehend** aktualisiert.

5.  Nach dem folgenden Synchronisierungszyklus sollte der Fehler aus der Liste entfernt werden.

## <a name="how-to-answer-the-diagnosis-questions"></a>Beantworten von Fragen zur Diagnose 
### <a name="does-the-user-exist-in-your-on-premises-active-directory"></a>Ist der Benutzer in Ihrer lokalen Active Directory-Instanz vorhanden?

Mit dieser Frage wird versucht, das Quellobjekt des vorhandenen Benutzers in der lokalen Active Directory-Instanz zu identifizieren.  
1.  Überprüfen Sie, ob Active Directory ein Objekt mit dem angegebenen UserPrincipalName enthält. Falls nicht, antworten Sie mit „Nein“.
2.  Falls ja, überprüfen Sie, ob das Objekt weiterhin im Bereich für die Synchronisierung enthalten ist.  
  - Suchen Sie im Azure AD-Connectorbereich mit dem DN.
  - Wenn das Objekt mit dem Status **Ausstehender Hinzufügevorgang** gefunden wird, antworten Sie mit „Nein“. Azure AD Connect kann für das Objekt keine Verbindung mit dem richtigen AD-Objekt herstellen.
  - Wenn das Objekt nicht gefunden wird, antworten Sie mit „Ja“.

> Sehen Sie sich beispielsweise das folgende Diagramm an. Mit der Frage soll ermittelt werden, ob *Joe Jackson* in der lokalen Active Directory-Instanz noch vorhanden ist.
Für das **allgemeine Szenario** ist sowohl der Benutzer *Joe Johnson* als auch der Benutzer *Joe Jackson* in Ihrer lokalen Active Directory-Instanz vorhanden. Bei den unter Quarantäne gestellten Objekten handelt es sich um zwei unterschiedliche Benutzer.

![Diagnostizieren von Synchronisierungsfehlern – Allgemeines Szenario](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixCommonCase.png)

> Für das **Szenario mit verwaisten Objekten** ist nur ein Benutzer (*Joe Johnson*) in der lokalen Active Directory-Instanz vorhanden:

![Diagnostizieren von Synchronisierungsfehlern – Szenario mit verwaistem Objekt](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixOrphanedCase.png)

### <a name="do-both-these-accounts-belong-to-the-same-user"></a>Gehören beide Konten zu demselben Benutzer?
Mit der Frage werden in Konflikt stehende eingehende Benutzer und das vorhandene Benutzerobjekt in Azure AD überprüft, um zu ermitteln, ob sie zu demselben Benutzer gehören.  
1.  Das in Konflikt stehende Objekt wird neu mit Azure Active Directory synchronisiert. Vergleichen Sie die Objekte anhand der folgenden Aspekte:  
  - Anzeigename
  - Benutzerprinzipalname
  - Object ID (Objekt-ID)
2.  Wenn ein Vergleich nicht möglich ist, überprüfen Sie, ob in Ihrer Active Directory-Umgebung Objekte mit den angegebenen UserPrincipalNames enthalten sind. Wenn beide gefunden werden, antworten Sie mit „Nein“.  

> Im unten angegebenen Fall gehören die beiden Objekte zu demselben Benutzer *Joe Johnson*.

![Diagnostizieren von Synchronisierungsfehlern – Szenario mit verwaistem Objekt](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixOrphanedCase.png)


## <a name="what-happened-after-fix-is-applied-for-orphaned-object-scenario"></a>Nach der Anwendung der Korrektur auf das Szenario mit verwaistem Objekt
Basierend auf den Antworten auf die gestellten Fragen wird die Schaltfläche **Fix anwenden** angezeigt, wenn über Azure AD eine Korrektur verfügbar ist. In diesem Fall wird das lokale Objekt mit einem unerwarteten Azure AD-Objekt synchronisiert. Die beiden Objekte werden über den „Quellanker“ zugeordnet. Mit der angewendeten Änderung werden beispielsweise folgende Schritte ausgeführt:
- Aktualisieren des Quellankers auf das richtige Objekt in Azure AD
- Löschen des in Konflikt stehenden Objekts in Azure AD, falls vorhanden

![Diagnostizieren von Synchronisierungsfehlern – Nach der Korrektur](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixAfterFix.png)

>[!IMPORTANT]
> Die Änderung „Fix anwenden“ gilt nur für die Fälle mit verwaisten Objekten.
>

Nach den oben beschriebenen Schritten kann der Benutzer auf die ursprüngliche Ressource zugreifen, die mit dem vorhandenen Objekt verknüpft ist. Der Wert zum **Diagnosestatus** in der Listenansicht wird in **Synchronisierung ausstehend** aktualisiert. Der Synchronisierungsfehler wird nach der folgenden Synchronisierung behoben. In Connect Health werden behobene Synchronisierungsfehler nicht mehr in der Listenansicht angezeigt. 


## <a name="faq"></a>Häufig gestellte Fragen
 -  Was passiert, wenn es beim Anwenden zu einem Fehler kommt?  
Bei einer fehlerhaften Ausführung kann es sein, dass Azure AD Connect gerade einen Export durchführt und der Fehler deshalb auftritt. Aktualisieren Sie die Portalseite, und wiederholen Sie den Vorgang nach der folgenden Synchronisierung. Der Synchronisierungszyklus dauert standardmäßig 30 Minuten. 

 -  Was passiert, wenn das **vorhandene Objekt** das zu löschende Objekt ist?  
Wenn das vorhandene Objekt in diesem Fall gelöscht werden soll, umfasst der Prozess nicht die Änderung des Quellankers. Sie sollten die Behebung über Ihre lokale AD-Instanz durchführen können.  

 -  Welche Berechtigung benötigt der Benutzer, um die Behebung anwenden zu können?  
„Globaler Administrator“ oder „Mitwirkender“ aus den RBAC-Einstellungen sind die Berechtigungen, die den Zugriff auf den Diagnose- und Problembehandlungsprozess ermöglichen.

 -  Muss ich AAD Connect konfigurieren oder den Azure AD Connect Health-Agent für dieses Feature aktualisieren?  
Nein. Der Diagnoseprozess ist ein ausschließlich cloudbasiertes Feature.

 -  Wird für das Objekt während des Diagnoseprozesses wieder der aktive Zustand hergestellt, wenn das vorhandene Objekt vorläufig gelöscht wird?  
Nein. Bei der Korrektur wird über den Quellanker hinaus kein Objektattribut aktualisiert. 
