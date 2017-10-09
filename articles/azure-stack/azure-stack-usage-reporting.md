---
title: Melden von Azure Stack-Nutzungsdaten an Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie das Melden von Nutzungsdaten in Azure Stack einrichten.
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: sngun;AlfredoPizzirani
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 695b93a818d3c0e615bb79e0a2861e134b2f5c89
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="report-azure-stack-usage-data-to-azure"></a>Melden von Azure Stack-Nutzungsdaten an Azure 

Nutzungsdaten, auch Verbrauchsdaten genannt, stellen die Menge der verwendeten Ressourcen dar. Die integrierten Azure Stack-Systeme, die das nutzungsbasierte Abrechnungsmodell verwenden, sollten die Nutzungsdaten an Azure melden. Diese Nutzungsdaten werden für Abrechnungszwecke verwendet. Der Azure Stack-Operator ist für die Konfiguration der Nutzungsdaten-Berichterstellung in seiner Azure Stack-Umgebung verantwortlich.


> [!NOTE]
> Nutzungsdaten-Berichterstellung ist für die Benutzer der integrierten Azure Stack-Systeme erforderlich, die im Rahmen des nutzungsbasierten Modells lizenziert sind. Sie ist hingegen optional für Kunden, die im Rahmen des Kapazitätsmodells lizenziert sind. Für das Azure Stack Development Kit können Cloudoperatoren Nutzungsdaten berichten und die Funktion testen, aber Benutzern wird keinerlei Nutzung in Rechnung gestellt. Auf der Seite [Azure Stack erwerben](https://azure.microsoft.com/overview/azure-stack/how-to-buy/) erhalten Sie weitere Informationen zu Preisen in Azure Stack.

## <a name="usage-data-reporting-flow"></a>Ablauf der Nutzungsdaten-Berichterstellung

Nutzungsdaten werden von Azure Stack über Azure Bridge an Azure gesendet. In Azure verarbeitet das Commerce-System die Nutzungsdaten und erstellt die Rechnung. Nachdem die Rechnung erstellt wurde, kann der Besitzer des Azure-Abonnements diese im [Azure-Kontocenter](https://account.windowsazure.com/Subscriptions) anzeigen und von dort herunterladen. Informationen zur Lizenzierung von Azure Stack finden Sie im Dokument [Azure Stack packaging and pricing (Paketerstellung und Preise bei Azure Stack)](https://go.microsoft.com/fwlink/?LinkId=842847&clcid=0x409). 

Die folgende Abbildung zeigt eine grafische Darstellung des Nutzungsdatenflusses in Azure Stack:

![Abrechnungsablauf](media/azure-stack-usage-reporting/billing-flow.png)

## <a name="set-up-usage-data-reporting"></a>Einrichten der Berichterstellung zu Nutzungsdaten

Um die Berichterstellung zu Nutzungsdaten in Azure Stack einzurichten, müssen Sie [Ihre Azure Stack-Instanz bei Azure registrieren](azure-stack-register.md). Im Rahmen des Registrierungsprozesses wird die Komponente Azure Bridge von Azure Stack konfiguriert, um eine Verbindung von Azure Stack mit Azure herzustellen. Azure Bridge sendet nach der Konfiguration folgende Nutzungsdaten an Azure: 

* **ID der Verbrauchseinheit**: Eine eindeutige ID für die verwendete Ressource
* **Menge**: Menge an Ressourcennutzungsdaten.
* **Speicherort**: Der Speicherort, an dem die aktuelle Azure Stack-Ressource bereitgestellt wurde
* **Ressourcen-URI**: Ein vollqualifizierter URI der Ressource, deren Nutzung gemeldet wird 
* **Abonnement-ID**: Die Abonnement-ID des Azure Stack-Benutzers.
* **Zeit**: Die Start- und Endzeit der Nutzungsdaten Es besteht eine Verzögerung zwischen dem Zeitpunkt, zu dem die Ressourcen in Azure Stack verwendet werden, und dem Zeitpunkt, zu dem die Nutzungsdaten an das Commerce-System gemeldet werden. Azure Stack aggregiert Nutzungsdaten für jeweils 24 Stunden, und der Vorgang des Meldens von Nutzungsdaten an das Commerce-System in Azure dauert zusätzlich einige Stunden. Daher kann es vorkommen, dass eine Nutzung kurz vor Mitternacht am nächsten Tag in Azure angezeigt wird.

## <a name="test-usage-data-reporting"></a>Testen der Berichterstellung zu Nutzungsdaten 

1. Um die Berichterstellung zu Nutzungsdaten zu testen, erstellen Sie einige Ressourcen in Azure Stack. Beispielsweise können Sie ein [Speicherkonto](azure-stack-provision-storage-account.md), eine [Windows Server-VM](azure-stack-provision-vm.md) und eine Linux-VM mit Basic- und Standard-SKUs erstellen, um zu veranschaulichen, wie die Kernnutzung gemeldet wird. Die Nutzungsdaten für verschiedene Ressourcentypen werden unter verschiedenen Verbrauchseinheiten gemeldet.  

2. Führen Sie Ihre Ressourcen einige Stunden lang aus. Nutzungsinformationen werden etwa einmal pro Stunde erfasst. Nachdem die Informationen erfasst wurden, werden diese Daten an Azure übertragen und im Commerce-System von Azure verarbeitet. Dieser Prozess kann einige Stunden dauern.  

3. Melden Sie sich als Azure-Kontoadministrator beim [Azure-Kontocenter](https://account.windowsazure.com/Subscriptions) an, und wählen Sie das Azure-Abonnement aus, das Sie zum Registrieren von Azure Stack verwendet haben. Ihnen werden wie in der folgenden Abbildung dargestellt die Azure Stack-Nutzungsdaten und der berechnete Betrag für jede der verwendeten Ressourcen angezeigt:  

   ![Abrechnungsablauf](media/azure-stack-usage-reporting/pricing-details.png)

Für das Azure Stack Development Kit fallen keine Gebühren für Ressourcen an. Daher wird als Preis $0,00 angezeigt. Für die integrierten Azure Stack-Systeme werden die tatsächlichen Kosten der einzelnen Ressourcen angezeigt.

## <a name="are-users-charged-for-the-infrastructure-virtual-machines"></a>Müssen Benutzer für die Infrastruktur-VMs bezahlen?
Nein, Nutzungsdaten für einige virtuelle Computer von Azure Stack-Ressourcenanbietern und VMs der Infrastruktur, die während der Bereitstellung erstellt werden, werden an Azure gemeldet, aber es fallen keine Gebühren dafür an. 

Benutzern werden nur virtuelle Computer in Rechnung gestellt, die unter Benutzerabonnements erstellt werden. Daher müssen alle Workloads unter Benutzerabonnements bereitgestellt werden, um die Azure Stack-Lizenzbestimmungen einzuhalten.

## <a name="how-do-i-use-my-existing-windows-server-licenses-in-azure-stack"></a>Gewusst wie: Wie verwende ich meine vorhandenen Windows Server-Lizenzen in Azure Stack? 
Vorhandene Windows Server-Lizenzen können in Azure Stack verwendet werden. Dieses Modell wird als BYOL (Bring-Your-Own-License) bezeichnet. Mit dem Verwenden die Lizenzen, die Sie bereits besitzen, vermeiden Sie das Generieren zusätzlicher Nutzungsmesser. Um Ihre vorhandenen Lizenzen verwenden zu können, müssen Sie die virtuellen Windows Server-Computer wie im Thema [Azure-Vorteil bei Hybridnutzung für Windows Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md) beschrieben bereitstellen. 

## <a name="what-azure-meters-are-used-when-reporting-usage-data-in-integrated-systems"></a>Welche Azure-Verbrauchseinheiten werden beim Melden von Nutzungsdaten in integrierten Systemen verwendet?
* **Vollpreis-Verbrauchseinheiten**: werden bei Ressourcen verwendet, die Benutzerworkloads zugeordnet sind  
* **Administrator-Verbrauchseinheiten**: werden bei Infrastrukturressourcen verwendet Diese Verbrauchseinheiten haben einen Preis von 0 Dollar.

## <a name="which-subscription-is-charged-for-the-resources-consumed"></a>Welchem Abonnement wird die Nutzung der Ressourcen in Rechnung gestellt?
Die Nutzung wird dem bei der [Registrierung von Azure Stack bei Azure](azure-stack-register.md) angegebene Abonnement in Rechnung gestellt.

## <a name="what-types-of-subscriptions-are-supported-for-usage-data-reporting"></a>Welche Arten von Abonnements werden für das Melden von Nutzungsdaten unterstützt?
Für integrierte Azure Stack-Systeme werden die Enterprise Agreement- und CSP-Abonnements (EA) unterstützt. 

Beim Azure Stack Development Kit unterstützen Enterprise Agreement-Abonnements, Abonnements mit nutzungsbasierter Zahlung, CSP- und MSDN-Abonnements das Melden von Nutzungsdaten.

## <a name="which-sovereign-clouds-support-usage-data-reporting"></a>Welche unabhängigen Clouds unterstützen das Melden von Nutzungsdaten?
Beim Azure Stack Development Kit werden für das Melden von Nutzungsdaten Abonnements benötigt, die im globalen Azure-System erstellt wurden. Abonnements, die in einer der unabhängigen Clouds (die Clouds „Azure Government“, „Azure Deutschland“ und „Azure China“) erstellt wurden, können nicht bei Azure registriert werden und unterstützen daher das Melden von Nutzungsdaten nicht. 

## <a name="how-can-users-identify-azure-stack-usage-data-in-the-azure-billing-portal"></a>Wie können Benutzer Azure Stack-Nutzungsdaten auf dem Azure-Abrechnungsportal erkennen?
Die Azure Stack-Nutzungsdaten werden Benutzern in der Datei mit Nutzungsdetails angezeigt. Informationen zum Abrufen der Datei mit Nutzungsdetails finden Sie unter [Herunterladen der Nutzungsdaten aus dem Kontocenter (CSV)](../billing/billing-download-azure-invoice-daily-usage-date.md#download-usage-from-the-account-center-csv). Die Datei mit Nutzungsdetails enthält die Azure Stack-Verbrauchseinheiten, die Azure Stack-Speicher und -VMs identifizieren. Alle in Azure Stack verwendeten Ressourcen werden im Abschnitt „Azure Stack“ gemeldet.

## <a name="why-doesnt-the-usage-reported-in-azure-stack-match-the-report-generated-from-azure-account-center"></a>Wieso stimmen die in Azure Stack gemeldeten Nutzungsdaten nicht mit dem vom Azure-Kontocenter erstellten Bericht überein?
Zwischen den von den Azure Stack-Nutzungs-APIs und den vom Azure-Kontocenter gemeldeten Nutzungsdaten tritt eine Verzögerung auf. Die Verzögerung ergibt sich aus der Zeit, die zum Hochladen der Nutzungsdaten von Azure Stack in das Commerce-System von Azure benötigt wird. Aufgrund dieser Verzögerung kann es vorkommen, dass eine Nutzung kurz vor Mitternacht am nächsten Tag in Azure angezeigt wird. Wenn Sie die [Azure Stack-Nutzungs-APIs](azure-stack-provider-resource-api.md) verwenden und die Ergebnisse mit den auf dem Azure-Abrechnungsportal angezeigten Nutzungsdaten vergleichen, können Sie eine Abweichung feststellen.

## <a name="next-steps"></a>Nächste Schritte

* [Anbieternutzungs-API](azure-stack-provider-resource-api.md)  
* [Mandantennutzungs-API](azure-stack-tenant-resource-usage-api.md)
* [FAQ zur Nutzung](azure-stack-usage-related-faq.md)
