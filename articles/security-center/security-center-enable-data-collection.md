---
title: Datensammlung in Azure Security Center | Microsoft-Dokumentation
description: " Hier erfahren Sie, wie Sie die Datensammlung in Azure Security Center aktivieren. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/14/2018
ms.author: terrylan
ms.openlocfilehash: 847127c96f23bbeb3cf3a5d1c9768af6e0cc0dc4
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34203970"
---
# <a name="data-collection-in-azure-security-center"></a>Datensammlung in Azure Security Center
Security Center sammelt Daten von Ihren virtuellen Azure-Computern (VMs) und Azure-fremden Computern, um sie hinsichtlich Sicherheitslücken und Bedrohungen zu überwachen. Die Daten werden mithilfe von Microsoft Monitoring Agent gesammelt. Der Agent liest verschiedene sicherheitsrelevante Konfigurationen und Ereignisprotokolle auf dem Computer und kopiert die Daten zur Analyse in Ihren Arbeitsbereich. Beispiele für Daten dieser Art: Betriebssystemtyp und -version, Betriebssystemprotokolle (Windows-Ereignisprotokolle), ausgeführte Prozesse, Computername, IP-Adressen, angemeldeter Benutzer und Mandanten-ID. Darüber hinaus kopiert der Microsoft Monitoring Agent Absturzabbilddateien in Ihren Arbeitsbereich.

## <a name="enable-automatic-provisioning-of-microsoft-monitoring-agent"></a>Aktivieren der automatischen Bereitstellung von Microsoft Monitoring Agent     
Die automatische Bereitstellung ist standardmäßig deaktiviert. Bei aktivierter automatischer Bereitstellung wird Microsoft Monitoring Agent von Security Center auf allen unterstützten virtuellen Azure-Computern sowie auf allen neu erstellten virtuellen Computern bereitgestellt. Die automatische Bereitstellung wird dringend empfohlen, die manuelle Agent-Installation ist jedoch ebenfalls verfügbar. [Erfahren Sie, wie Sie die Microsoft Monitoring Agent-Erweiterung installieren.](../log-analytics/log-analytics-quick-collect-azurevm.md#enable-the-log-analytics-vm-extension)

> [!NOTE]
> Wenn Sie die automatische Bereitstellung deaktivieren, schränkt dies die Sicherheitsüberwachung für Ihre Ressourcen ein. Weitere Informationen finden Sie in diesem Artikel unter [Deaktivieren der automatischen Bereitstellung](security-center-enable-data-collection.md#disable-automatic-provisioning). Momentaufnahmen von VM-Datenträgern sowie die Artefaktsammlung sind auch bei deaktivierter automatischer Bereitstellung aktiviert.
>
>

So aktivieren Sie die automatische Bereitstellung von Microsoft Monitoring Agent:
1. Klicken Sie im Hauptmenü von Security Center auf **Sicherheitsrichtlinie**.
2. Wählen Sie das Abonnement aus.

  ![Wählen Sie das Abonnement aus.][7]

3. Klicken Sie unter **Sicherheitsrichtlinie** auf **Datensammlung**.
4. Wählen Sie unter **Automatische Bereitstellung** die Option **Ein** aus, um die automatische Bereitstellung zu aktivieren.
5. Wählen Sie **Speichern**aus.

  ![Aktivieren der automatischen Bereitstellung][1]

## <a name="default-workspace-configuration"></a>Standardkonfiguration für Arbeitsbereiche
Von Security Center gesammelte Daten werden in Log Analytics-Arbeitsbereichen gespeichert.  Sie können festlegen, ob Daten von virtuellen Azure-Computern in von Security Center erstellten Arbeitsbereichen oder in einem vorhandenen Arbeitsbereich gespeichert werden sollen, den Sie selbst erstellt haben.

Wenn Sie Ihren vorhandenen Log Analytics-Arbeitsbereich verwenden möchten, gilt Folgendes:
- Der Arbeitsbereich muss Ihrem ausgewählten Azure-Abonnement zugeordnet sein.
- Sie müssen mindestens über Leseberechtigungen verfügen, um auf den Arbeitsbereich zugreifen zu können.

So wählen Sie einen vorhandenen Log Analytics-Arbeitsbereich aus:

1. Wählen Sie unter **Standardmäßige Arbeitsbereichskonfiguration** die Option **Use another workspace** (Anderen Arbeitsbereich verwenden) aus.

   ![Auswählen eines vorhandenen Arbeitsbereichs][2]

2. Wählen Sie im Pulldownmenü einen Arbeitsbereich zum Speichern der gesammelten Daten aus.

  > [!NOTE]
  > Im Pulldownmenü sind alle Arbeitsbereiche in allen Ihren Abonnements verfügbar. Weitere Informationen finden Sie unter [Abonnementübergreifende Arbeitsbereichsauswahl](security-center-enable-data-collection.md#cross-subscription-workspace-selection).
  >
  >

3. Wählen Sie **Speichern**aus.
4. Nach dem Klicken auf **Speichern** werden Sie gefragt, ob Sie überwachte virtuelle Computer neu konfigurieren möchten.

   - Klicken Sie auf **Nein**, wenn die neuen Arbeitsbereichseinstellungen nur auf neue virtuelle Computer angewendet werden sollen. Die neuen Arbeitsbereichseinstellungen gelten nur für neue Agent-Installationen (neu ermittelte virtuelle Computer, auf denen Microsoft Monitoring Agent nicht installiert ist).
   - Klicken Sie auf **Ja**, wenn die neuen Arbeitsbereichseinstellungen auf alle virtuellen Computer angewendet werden sollen. Darüber hinaus wird jeder virtuelle Computer, der mit einem von Security Center erstellten Arbeitsbereich verbunden ist, nun mit dem neuen Zielarbeitsbereich verbunden.

   > [!NOTE]
   > Wenn Sie auf „Ja“ klicken, dürfen die von Security Center erstellten Arbeitsbereiche erst gelöscht werden, wenn alle virtuellen Computer mit dem neuen Zielarbeitsbereich verbunden sind. Dieser Vorgang ist nicht erfolgreich, wenn ein Arbeitsbereich zu früh gelöscht wird.
   >
   >

   - Klicken Sie auf **Abbrechen**, um den Vorgang abzubrechen.

     ![Auswählen eines vorhandenen Arbeitsbereichs][3]

## <a name="cross-subscription-workspace-selection"></a>Abonnementübergreifende Arbeitsbereichsauswahl
Wenn Sie einen Arbeitsbereich zum Speichern Ihrer Daten auswählen, sind alle Arbeitsbereiche in allen Ihren Abonnements verfügbar. Über die abonnementübergreifende Arbeitsbereichsauswahl können Sie Daten von virtuellen Computern erfassen, die in verschiedenen Abonnements ausgeführt werden, und im Arbeitsbereich Ihrer Wahl speichern. Diese Funktion eignet sich für virtuelle Computer unter Linux und unter Windows.

> [!NOTE]
> Die abonnementübergreifende Arbeitsbereichsauswahl ist im Free-Tarif von Azure Security Center enthalten. Weitere Informationen zu den Tarifen von Security Center finden Sie unter [Preise](security-center-pricing.md).
>
>

## <a name="data-collection-tier"></a>Datensammlungsebene
Security Center kann die Menge an Ereignissen verringern und dabei gleichzeitig sicherstellen, dass genügend Ereignisse zur Untersuchung, Überwachung und Bedrohungserkennung zur Verfügung stehen. Sie können die passende Filterrichtlinie für Ihre Abonnements und Arbeitsbereiche auswählen. Dabei haben Sie die Wahl zwischen vier Gruppen von Ereignissen, die vom Agent gesammelt werden sollen.

- **Alle Ereignisse:** Für Kunden, die alle Ereignisse sammeln möchten. Dies ist die Standardoption.
- **Allgemein:** eine Gruppe von Ereignissen, die die Anforderungen der meisten Kunden erfüllt und einen vollständigen Überwachungspfad bietet.
- **Minimal:** Eine kleinere Gruppe von Ereignissen für Kunden, die das Ereignisvolumen minimieren möchten.
- **Keine:** Deaktiviert das Sammeln sicherheitsrelevanter Ereignisse aus Sicherheits- und AppLocker-Protokollen. Bei Kunden, die sich für diese Option entscheiden, werden auf den Sicherheitsdashboards nur Protokolle der Windows-Firewall sowie proaktive Bewertungen wie Antischadsoftware, Baseline und Update angezeigt.

> [!NOTE]
> Diese Sicherheitsereignisgruppen sind nur im Standard-Tarif von Security Center verfügbar. Weitere Informationen zu den Tarifen von Security Center finden Sie unter [Preise](security-center-pricing.md).
Diese Gruppen wurden für typische Szenarien konzipiert. Überlegen Sie sich vor der Implementierung, welche Ihre Anforderungen erfüllt.
>
>

Bei der Bestimmung der Ereignisse für die Gruppen **Allgemein** und **Minimal** haben wir uns mit Kunden und Branchenstandards beschäftigt, um uns über die ungefilterte Häufigkeit der einzelnen Ereignisse und deren Verwendung zu informieren. Dabei haben wir uns an den folgenden Richtlinien orientiert:

- **Minimal:** Diese Gruppe deckt nur Ereignisse ab, die auf eine erfolgreiche Sicherheitsverletzung hindeuten, sowie wichtige Ereignisse, die nur sehr selten auftreten. So deckt diese Gruppe beispielsweise erfolgreiche und nicht erfolgreiche Benutzeranmeldungen (Ereignis-IDs 4624 und 4625), aber keine Abmeldungen ab. Diese sind zwar für die Überwachung wichtig, nicht aber für die Erkennung, und sie treten zudem relativ häufig auf. Der Großteil des Datenvolumens dieser Gruppe ist auf Anmeldeereignisse und auf das Prozesserstellungsereignis (Ereignis-ID 4688) zurückzuführen.
- **Allgemein:** Diese Gruppe bietet einen vollständigen Benutzerüberwachungspfad. So enthält die Gruppe beispielsweise sowohl Benutzeranmeldungen als auch Benutzerabmeldungen (Ereignis-ID 4634). Wir schließen Überwachungsaktionen wie Sicherheitsgruppenänderungen, wichtige Domänencontroller-Kerberos-Vorgänge und andere Ereignisse ein, die von Branchenorganisationen empfohlen werden.

Ereignisse mit sehr geringem Volumen wurden in die Gruppe „Allgemein“ aufgenommen, da das Hauptargument für ihre Verwendung (im Vergleich zu allen anderen Ereignissen) in der Verringerung des Volumens besteht (und nicht darin, bestimmte Ereignisse herauszufiltern).

Im Anschluss finden Sie eine vollständige Aufschlüsselung der Sicherheits- und AppLocker-Ereignis-IDs für die einzelnen Gruppen:

| Datenschicht | Indikatoren für gesammelte Ereignisse |
| --- | --- |
| Wenig | 1102,4624,4625,4657,4663,4688,4700,4702,4719,4720,4722,4723,4724,4727,4728,4732,4735,4737,4739,4740,4754,4755, |
| | 4756,4767,4799,4825,4946,4948,4956,5024,5033,8001,8002,8003,8004,8005,8006,8007,8222 |
| Common | 1,299,300,324,340,403,404,410,411,412,413,431,500,501,1100,1102,1107,1108,4608,4610,4611,4614,461,4622, |
| |  4624,4625,4634,4647,4648,4649,4657,4661,4662,4663,4665,4666,4667,4688,4670,4672,4673,4674,4675,4689,4697, |
| | 4700,4702,4704,4705,4716,4717,4718,4719,4720,4722,4723,4724,4725,4726,4727,4728,4729,4733,4732,4735,4737, |
| | 4738,4739,4740,4742,4744,4745,4746,4750,4751,4752,4754,4755,4756,4757,4760,4761,4762,4764,4767,4768,4771, |
| | 4774,4778,4779,4781,4793,4797,4798,4799,4800,4801,4802,4803,4825,4826,4870,4886,4887,4888,4893,4898,4902, |
| | 4904,4905,4907,4931,4932,4933,4946,4948,4956,4985,5024,5033,5059,5136,5137,5140,5145,5632,6144,6145,6272, |
| | 6273,6278,6416,6423,6424,8001,8002,8003,8004,8005,8006,8007,8222,26401,30004 |

> [!NOTE]
> Wenn Sie ein Gruppenrichtlinienobjekt (Group Policy Object, GPO) verwenden, wird empfohlen, das Überwachungsrichtlinien-Prozesserstellungsereignis 4688 und das Feld *CommandLine* im Ereignis 4688 zu aktivieren. Weitere Informationen zum Prozesserstellungsereignis 4688 finden Sie in den [häufig gestellten Fragen](security-center-faq.md#what-happens-when-data-collection-is-enabled) zu Security Center. Weitere Informationen zu diesen Überwachungsrichtlinien finden Sie unter [Empfehlungen zur Überwachungsrichtlinie](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations).
>
>

So wählen Sie Ihre Filterrichtlinie:
1. Wählen Sie auf dem Blatt **Sicherheitsrichtlinie – Datensammlung** unter **Sicherheitsereignisse** Ihre Filterrichtlinie aus.
2. Wählen Sie **Speichern**aus.

   ![Auswählen der Filterrichtlinie][5]

## <a name="disable-automatic-provisioning"></a>Deaktivieren der automatischen Bereitstellung
Sie können die automatische Bereitstellung in Ressourcen jederzeit deaktivieren, indem Sie diese Einstellung in der Sicherheitsrichtlinie deaktivieren. Die automatische Bereitstellung wird dringend empfohlen, um Sicherheitswarnungen und -empfehlungen zu Systemupdates, zu Sicherheitsrisiken für das Betriebssystem und zu Endpoint Protection zu erhalten.

> [!NOTE]
> Wenn Sie die automatische Bereitstellung deaktivieren, wird Microsoft Monitoring Agent nicht von virtuellen Azure-Computern entfernt, auf denen der Agent bereitgestellt wurde.
>
>

1. Kehren Sie zum Hauptmenü von Security Center zurück, und klicken Sie auf die Sicherheitsrichtlinie.
2. Wählen Sie das Abonnement aus, für das Sie die automatische Bereitstellung deaktivieren möchten.
3. Wählen Sie auf dem Blatt **Sicherheitsrichtlinie – Datensammlung** unter **Automatische Bereitstellung** die Option **Aus** aus.
4. Wählen Sie **Speichern**aus.

  ![Deaktivieren der automatischen Bereitstellung][6]

Wenn die automatische Bereitstellung deaktiviert ist, wird der Abschnitt mit der standardmäßigen Arbeitsbereichskonfiguration nicht angezeigt.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurde die Funktionsweise der Datensammlung und der automatischen Bereitstellung in Security Center erläutert. Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Verwalten von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md) : Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md): Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md)Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
- [Azure Security Center-Datensicherheit:](security-center-data-security.md) Hier erfahren Sie, wie Daten in Security Center verwaltet und geschützt werden.
* [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md): Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/)(Blog zur Azure-Sicherheit): Hier finden Sie Neuigkeiten und Informationen zur Azure-Sicherheit.

<!--Image references-->
[1]: ./media/security-center-enable-data-collection/enable-automatic-provisioning.png
[2]: ./media/security-center-enable-data-collection/use-another-workspace.png
[3]: ./media/security-center-enable-data-collection/reconfigure-monitored-vm.png
[5]: ./media/security-center-enable-data-collection/data-collection-tiers.png
[6]: ./media/security-center-enable-data-collection/disable-data-collection.png
[7]: ./media/security-center-enable-data-collection/select-subscription.png
