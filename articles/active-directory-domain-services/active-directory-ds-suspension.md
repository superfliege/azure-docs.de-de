---
title: 'Azure Active Directory Domain Services: Angehaltene Domänen | Microsoft-Dokumentation'
description: Anhalten und Löschen verwalteter Domänen
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: 95e1d8da-60c7-4fc1-987d-f48fde56a8cb
ms.service: active-directory
ms.component: domains
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2018
ms.author: ergreenl
ms.openlocfilehash: 99896b0f618151cd368a21c4a212b7d8e28b7a6f
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2018
ms.locfileid: "39126092"
---
# <a name="suspended-domains"></a>Angehaltene Domänen
Wenn Azure AD Domain Services eine verwaltete Domäne für einen längeren Zeitraum-nicht bedienen kann, wird die verwaltete Domäne in den Zustand „Angehalten“ versetzt. In diesem Artikel wird erläutert, weshalb verwaltete Domänen angehalten werden und wie Sie eine angehaltene Domäne bereinigen.


## <a name="states-your-managed-domain-can-be-in"></a>Mögliche Zustände für Ihre verwaltete Domäne

![Zeitachse für angehaltene Domänen](media\active-directory-domain-services-suspension\suspension-timeline.PNG)

In der Grafik oben werden die möglichen Zustände einer von Azure AD Domain Services verwalteten Domäne dargestellt.

### <a name="running-state"></a>Zustand „Wird ausgeführt“
Eine verwaltete Domäne, die ordnungsgemäß konfiguriert ist und regulär arbeitet, befindet sich im Zustand **Wird ausgeführt**.

**Ihre Möglichkeiten:**
* Microsoft kann die Integrität Ihrer verwalteten Domäne regelmäßig überwachen.
* Domänencontroller für Ihre verwaltete Domäne werden regelmäßig gepatcht und aktualisiert.
* Änderungen von Azure Active Directory werden regelmäßig mit Ihrer verwalteten Domäne synchronisiert.
* Für Ihre verwaltete Domäne werden regelmäßige Sicherungen durchgeführt.


### <a name="needs-attention-state"></a>Zustand „Eingreifen erforderlich“
Eine verwaltete Domäne befindet sich im Zustand **Eingreifen erforderlich**, wenn mindestens ein Problem erfordert, dass ein Administrator Maßnahmen ergreift. Auf der Integritätsseite Ihrer verwalteten Domäne werden eine oder mehrere Warnungen in diesem Status aufgelistet. Wenn Sie beispielsweise eine restriktive NSG für Ihr virtuelles Netzwerk konfiguriert haben, ist Microsoft möglicherweise nicht in der Lage, Ihre verwaltete Domäne zu aktualisieren und zu überwachen. Aufgrund dieser ungültigen Konfiguration wird eine Warnung generiert, und Ihre verwaltete Domäne wird in den Zustand „Eingreifen erforderlich“ versetzt.

Jede Warnung verfügt über bestimmte Lösungsschritte. Einige Warnungen sind vorübergehend und werden vom Dienst automatisch aufgelöst. Sie können einige andere Warnungen mithilfe der Anweisungen aus den entsprechenden Lösungsschritten für diese Warnung auflösen. Um einige kritische Warnungen aufzulösen, müssen Sie sich an den Microsoft-Support wenden.

Weitere Informationen finden Sie unter [how to troubleshoot alerts on a managed domain (Problembehandlung bei Warnungen für eine verwaltete Domäne)](active-directory-ds-troubleshoot-alerts.md).

**Ihre Möglichkeiten:**

In einigen Fällen (z.B., wenn Ihre Netzwerkkonfiguration ungültig ist) sind die Domänencontroller für Ihre verwaltete Domäne möglicherweise nicht erreichbar. Microsoft kann nicht garantieren, dass Ihre verwaltete Domäne in diesem Zustand regelmäßig überwacht, gepatcht, aktualisiert oder gesichert wird.

* Ihre verwaltete Domäne befindet sich in einem fehlerhaften Zustand und die fortlaufende Systemüberwachung wird möglicherweise beendet, bis die Warnung aufgelöst wurde.
* Domänencontroller für Ihre verwaltete Domäne werden möglicherweise nicht regelmäßig gepatcht und aktualisiert.
* Änderungen von Azure Active Directory werden möglicherweise nicht mit Ihrer verwalteten Domäne synchronisiert.
* Nach Möglichkeit werden Sicherungen für Ihre verwalte Domäne durchgeführt.
* Wenn Sie die Warnungen auflösen, die Auswirkungen auf Ihre verwaltete Domäne haben, wird Ihre verwaltete Domäne möglicherweise wieder in den Zustand „Wird ausgeführt“ versetzt.
* Kritische Warnungen werden für Konfigurationsprobleme ausgelöst, wenn Microsoft nicht auf Ihre Domänencontroller zugreifen kann. Wenn solche Warnungen nicht innerhalb von 15 Tagen aufgelöst werden, wird Ihre verwaltete Domäne in den Zustand „Angehalten“ versetzt.


### <a name="suspended-state"></a>Zustand „Angehalten“
Eine verwaltete Domäne wird aus den folgenden Gründen in den Zustand **Angehalten** versetzt:
* Mindestens eine kritische Warnung wurde nicht innerhalb von 15 Tagen aufgelöst. Kritische Warnungen können von einer Fehlkonfiguration verursacht werden, die den Zugriff auf von Azure AD Domain Services benötigte Ressourcen blockiert.
    * Beispiel: Wenn Warnung [AADDS104: Netzwerkfehler](active-directory-ds-troubleshoot-nsg.md) für die verwaltete Domäne seit mehr als 15 Tagen nicht aufgelöst ist.
* Ein Abrechnungsproblem in Bezug auf Ihr Azure-Abonnement liegt vor, oder das Azure-Abonnement ist abgelaufen.

Verwaltete Domänen werden angehalten, wenn Microsoft nicht in der Lage ist, die Domäne fortlaufend zu verwalten, überwachen, patchen oder sichern.

**Ihre Möglichkeiten:**
* Die Bereitstellung der Domänencontroller für die verwaltete Domäne wird aufgehoben, und die Domänencontroller sind nicht mehr erreichbar im virtuellen Netzwerk.
* Der sichere LDAP-Zugriff auf Ihre verwaltete Domäne über das Internet (sofern aktiviert) funktioniert nicht mehr.
* Sie stellen Fehler beim Authentifizieren in der verwalteten Domäne, bei der Anmeldung bei in die Domäne eingebundenen virtuellen Computern und beim Herstellen einer Verbindung über LDAP/LDAPS fest.
* Sicherungen für Ihre verwaltete Domäne werden nicht mehr erstellt.
* Die Synchronisierung mit Azure AD wird beendet.
* Lösen Sie die Warnung auf, die für den Zustand „Angehalten“ der verwalteten Domäne verantwortlich ist, und wenden Sie sich anschließend an den Support.
* Der Support kann Ihre verwaltete Domäne nur dann wiederherstellen, wenn eine weniger als 30 Tage alte Sicherung vorhanden ist.

Die verwaltete Domäne bleibt nur für die Dauer von 15 Tagen im Zustand „Angehalten“. Zum Wiederherstellen Ihrer verwalteten Domäne empfiehlt Microsoft, dass Sie kritische Warnungen sofort auflösen.


### <a name="deleted-state"></a>Zustand „Gelöscht“
Eine verwaltete Domäne, die 15 Tage im Zustand „Angehalten“ verbleibt, wird **gelöscht**.

**Ihre Möglichkeiten:**
* Alle Ressourcen und Sicherungen für die verwaltete Domäne werden gelöscht.
* Sie können die verwaltete Domäne nicht wiederherstellen und müssen eine neue verwaltete Domäne erstellen, um die Azure AD Domain Services zu verwenden.
* Nach dem Löschen wird Ihnen die verwaltete Domäne nicht in Rechnung gestellt.


## <a name="how-do-you-know-if-your-managed-domain-is-suspended"></a>Wie erkennen Sie, dass Ihre verwaltete Domäne angehalten wurde?
Sie sehen eine [Warnung](active-directory-ds-troubleshoot-alerts.md) auf der Seite „Azure AD Domain Services Health“ (Integrität der Azure AD Domain Services), die angibt, dass die Domäne angehalten wurde. Der Zustand der Domäne wird ebenfalls als „Angehalten“ angezeigt.


## <a name="restore-a-suspended-domain"></a>Wiederherstellen einer angehaltenen Domäne
So stellen Sie eine Domäne im Zustand „Angehalten“ wieder her:

1. Navigieren Sie im Azure-Portal zur [Seite „Azure AD Domain Services“](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) im Azure-Portal
2. Klicken Sie auf die verwaltete Domäne.
3. Klicken Sie im linken Navigationsbereich auf **Integrität**.
4. Klicken Sie auf die Warnung. Die Warnungs-ID lautet je nach Grund für das Anhalten entweder AADDS503 oder AADDS504.
5. Klicken Sie auf den Auflösungslink in der Warnung, und führen Sie die Schritte zum Auflösen der Warnung aus.

Ihre verwaltete Domäne kann nur bis zum Datum der letzten Sicherung wiederhergestellt werden. Das Datum der letzten Sicherung wird auf der Seite „Integrität“ Ihrer verwalteten Domäne angezeigt. Alle nach der letzten Sicherung aufgetretenen Änderungen werden nicht wiederhergestellt. Sicherungen für eine verwaltete Domäne werden bis zu 30 Tage lang gespeichert. Sicherungen, die älter als 30 Tage sind, werden gelöscht.


## <a name="next-steps"></a>Nächste Schritte
- [Azure AD Domain Services – Problembehandlung von Warnungen](active-directory-ds-troubleshoot-alerts.md)
- [Weitere Informationen zu Azure AD Domain Services](active-directory-ds-overview.md)
- [Wenden Sie sich an das Produktteam](active-directory-ds-contact-us.md)

## <a name="contact-us"></a>So erreichen Sie uns
Wenden Sie sich an das Produktteam der Azure Active Directory Domain Services, um [Feedback zu geben oder Support zu erhalten](active-directory-ds-contact-us.md).
