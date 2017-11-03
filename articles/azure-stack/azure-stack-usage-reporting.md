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
ms.openlocfilehash: 5abc325a6e7c019dc3cb84f7f6ff63c3eb2ff76c
ms.sourcegitcommit: 1131386137462a8a959abb0f8822d1b329a4e474
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2017
---
# <a name="report-azure-stack-usage-data-to-azure"></a>Melden von Azure Stack-Nutzungsdaten an Azure 

Nutzungsdaten, auch Verbrauchsdaten genannt, stellen die Menge der verwendeten Ressourcen dar. 

Azure Stack-Systeme mit mehreren Knoten, die das nutzungsbasierte Abrechnungsmodell verwenden, sollten die Nutzungsdaten aus Abrechnungsgründen an Azure melden.  Azure Stack-Betreiber sollten ihre Azure Stack-Instanz so konfigurieren, dass diese Nutzungsdaten an Azure meldet.

> [!NOTE]
> Die Nutzungsdaten-Berichterstellung ist für die Benutzer der Azure Stack-Systeme mit mehreren Knoten erforderlich, die im Rahmen des nutzungsbasierten Modells lizenziert sind. Sie ist optional für Kunden, die im Rahmen des Kapazitätsmodells lizenziert sind (siehe Seite mit [Informationen zum Kaufvorgang](https://azure.microsoft.com/overview/azure-stack/how-to-buy/ to learn more about pricing in Azure Stack)). Für Azure Stack Development Kit-Benutzer können Azure Stack-Betreiber Nutzungsdaten melden und die Funktion testen. Allerdings werden Benutzern keinerlei Kosten berechnet, die für die Nutzung anfallen. 


![Abrechnungsablauf](media/azure-stack-usage-reporting/billing-flow.png)

Nutzungsdaten werden von Azure Stack über Azure Bridge an Azure gesendet. In Azure verarbeitet das Commerce-System die Nutzungsdaten und erstellt die Rechnung. Nachdem die Rechnung erstellt wurde, kann der Besitzer des Azure-Abonnements diese im [Azure-Kontocenter](https://account.windowsazure.com/Subscriptions) anzeigen und von dort herunterladen. Informationen zur Lizenzierung von Azure Stack finden Sie im Dokument [Azure Stack packaging and pricing (Paketerstellung und Preise bei Azure Stack)](https://go.microsoft.com/fwlink/?LinkId=842847&clcid=0x409).

## <a name="set-up-usage-data-reporting"></a>Einrichten der Berichterstellung zu Nutzungsdaten

Um die Berichterstellung zu Nutzungsdaten einzurichten, müssen Sie [Ihre Azure Stack-Instanz bei Azure registrieren](azure-stack-register.md). Im Rahmen des Registrierungsprozesses wird die Azure Bridge-Komponente von Azure Stack, die eine Verbindung zwischen Azure Stack und Azure herstellt und die Nutzungsdaten sendet, konfiguriert. Die folgenden Nutzungsdaten werden von Azure Stack an Azure gesendet:

- **ID der Verbrauchseinheit**: Eine eindeutige ID für die verwendete Ressource
- **Menge**: Menge der Ressourcennutzung.
- **Speicherort**: Der Speicherort, an dem die aktuelle Azure Stack-Ressource bereitgestellt wurde
- **Ressourcen-URI**: Ein vollqualifizierter URI der Ressource, deren Nutzung gemeldet wird
- **Abonnement-ID**: Die Abonnement-ID des Azure Stack-Benutzers Dies ist das lokale (Azure Stack-)Abonnement.
- **Zeit**: Die Start- und Endzeit der Nutzungsdaten Es besteht eine Verzögerung zwischen der Zeit, zu der die Ressourcen in Azure Stack verwendet werden, und der Zeit, zu der die Nutzungsdaten an das Commerce-System gemeldet werden. Azure Stack aggregiert Nutzungsdaten für jeweils 24 Stunden, und der Vorgang des Meldens von Nutzungsdaten an das Commerce-System in Azure dauert zusätzlich einige Stunden. Daher kann es vorkommen, dass eine Nutzung kurz vor Mitternacht am nächsten Tag in Azure angezeigt wird.

## <a name="generate-usage-data-reporting"></a>Generieren der Berichterstellung zu Nutzungsdaten

1. Um die Berichterstellung zu Nutzungsdaten zu testen, erstellen Sie einige Ressourcen in Azure Stack. Beispielsweise können Sie ein [Speicherkonto](azure-stack-provision-storage-account.md), eine [Windows Server-VM](azure-stack-provision-vm.md) und eine Linux-VM mit Basic- und Standard-SKUs erstellen, um zu veranschaulichen, wie die Kernnutzung gemeldet wird. Die Nutzungsdaten für verschiedene Ressourcentypen werden unter verschiedenen Verbrauchseinheiten gemeldet.

2. Führen Sie Ihre Ressourcen einige Stunden lang aus. Nutzungsinformationen werden etwa einmal pro Stunde erfasst. Nachdem die Informationen erfasst wurden, werden diese Daten an Azure übertragen und im Commerce-System von Azure verarbeitet. Dieser Prozess kann einige Stunden dauern.

## <a name="view-usage---csp-subscriptions"></a>Anzeigen der Nutzung – CSP-Abonnements

Wenn Sie Ihre Azure Stack-Instanz mit einem CSP-Abonnement registriert haben, können Sie Ihre Nutzung und Gebühren auf die gleiche Weise anzeigen, wie Sie die Azure-Nutzungsdaten sehen. Die Nutzung von Azure Stack wird in Ihrer Rechnung und in der Abstimmungsdatei aufgeführt, die über [Partner Center](https://partnercenter.microsoft.com/en-us/partner/home) erhältlich ist. Die Abstimmungsdatei wird monatlich aktualisiert. Wenn Sie auf die aktuellen Informationen zur Nutzung von Azure Stack zugreifen müssen, können Sie die Partner Center APIs verwenden.

   ![Partner Center](media/azure-stack-usage-reporting/partner-center.png)


## <a name="view-usage--enterprise-agreement-subscriptions"></a>Anzeigen der Nutzung – Enterprise Agreement-Abonnements

Wenn Sie Ihre Azure Stack-Instanz mit einem Enterprise Agreement-Abonnement registriert haben, können Sie Ihre Nutzung und Gebühren im [EA-Portal](https://ea.azure.com/) sehen. Neben den Azure-Nutzungsdaten werden im EA-Portal im Abschnitt zu den Berichten unter den erweiterten Downloads die Azure Stack-Nutzungsdaten aufgeführt. 

## <a name="view-usage--other-subscriptions"></a>Anzeigen der Nutzung – Andere Abonnements

Wenn Sie Ihre Azure Stack-Instanz mit einem anderen Abonnementtyp, z.B. einem nutzungsbasierten Abonnement, registriert haben, können Sie die Nutzung und die Gebühren im Azure-Kontocenter sehen. Melden Sie sich als Azure-Kontoadministrator beim [Azure-Kontocenter](https://account.windowsazure.com/Subscriptions) an, und wählen Sie das Azure-Abonnement aus, das Sie zum Registrieren von Azure Stack verwendet haben. Ihnen werden wie in der folgenden Abbildung dargestellt die Azure Stack-Nutzungsdaten und der berechnete Betrag für jede der verwendeten Ressourcen angezeigt:

   ![Abrechnungsablauf](media/azure-stack-usage-reporting/pricing-details.png)

Für das Azure Stack Development Kit fallen für Azure-Ressourcen keine Gebühren an. Daher wird als Preis $0,00 angezeigt. Wenn Azure Stack mit mehreren Knoten allgemein verfügbar ist, werden die tatsächlichen Kosten für jede dieser Ressourcen angezeigt.

## <a name="which-azure-stack-deployments-are-charged"></a>Welche Azure Stack-Bereitstellungen werden in Rechnung gestellt?

Die Ressourcennutzung für Azure Stack Development Kit ist kostenlos. Bei Azure Stack-Systemen mit mehreren Knoten hingegen werden Workload-VMs, Speicherdienste und App-Dienste in Rechnung gestellt.

## <a name="are-users-charged-for-the-infrastructure-vms"></a>Müssen Benutzer für die Infrastruktur-VMs bezahlen?

Nein. Nutzungsdaten für einige VMs von Azure Stack-Ressourcenanbietern werden Azure gemeldet, aber es fallen keine Gebühren für diese VMs oder die während der Bereitstellung erstellten VMs an, um die Azure Stack-Infrastruktur zu aktivieren.  

Benutzern werden nur VMs in Rechnung gestellt, die unter den Mandantenabonnements ausgeführt werden. Alle Workloads müssen unter Mandantenabonnements bereitgestellt werden, um die Lizenzbedingungen von Azure Stack einzuhalten.

## <a name="i-have-a-windows-server-license-i-want-to-use-on-azure-stack-how-do-i-do-it"></a>Wie kann ich eine Windows Server-Lizenz für Azure Stack verwenden?

Durch die Verwendung der vorhandenen Lizenzen wird die Inanspruchnahme von Nutzungsverbrauchseinheiten vermieden. Vorhandene Windows Server-Lizenzen können in Azure Stack verwendet werden, wie im [Azure Stack-Lizenzierungshandbuch](https://go.microsoft.com/fwlink/?LinkId=851536&clcid=0x409) im Abschnitt „Verwendung der vorhandenen Software mit Azure Stack“ beschrieben wird. Kunden müssen ihre virtuellen Windows Server-Computer wie im Thema [Azure-Hybridvorteil für Windows Server](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/hybrid-use-benefit-licensing) beschrieben bereitstellen, um Ihre vorhandenen Lizenzen verwenden zu können.

## <a name="which-subscription-is-charged-for-the-resources-consumed"></a>Welchem Abonnement wird die Nutzung der Ressourcen in Rechnung gestellt?
Die Nutzung wird dem bei der [Registrierung von Azure Stack bei Azure](azure-stack-register.md) angegebene Abonnement in Rechnung gestellt.

## <a name="what-types-of-subscriptions-are-supported-for-usage-data-reporting"></a>Welche Arten von Abonnements werden für das Melden von Nutzungsdaten unterstützt?

Für Azure Stack-Systeme mit mehreren Knoten werden das Enterprise Agreement-Abonnement (EA) und CSP-Abonnement unterstützt. Beim Azure Stack Development Kit unterstützen Enterprise Agreement-Abonnements, Abonnements mit nutzungsbasierter Zahlung, CSP- und MSDN-Abonnements das Melden von Nutzungsdaten.

## <a name="does-usage-data-reporting-work-in-sovereign-clouds"></a>Funktioniert das Melden von Nutzungsdaten in unabhängigen Clouds?

Beim Azure Stack Development Kit werden für das Melden von Nutzungsdaten Abonnements benötigt, die im globalen Azure-System erstellt wurden. Abonnements, die in einer der unabhängigen Clouds (die Clouds „Azure Government“, „Azure Deutschland“ und „Azure China“) erstellt wurden, können nicht bei Azure registriert werden und unterstützen daher das Melden von Nutzungsdaten nicht.

## <a name="how-can-users-identify-azure-stack-usage-data-in-the-azure-billing-portal"></a>Wie können Benutzer Azure Stack-Nutzungsdaten auf dem Azure-Abrechnungsportal erkennen?

Die Azure Stack-Nutzungsdaten werden Benutzern in der Datei mit Nutzungsdetails angezeigt. Informationen zum Abrufen der Datei mit Nutzungsdetails finden Sie unter [Herunterladen der Nutzungsdaten aus dem Kontocenter (CSV)](https://docs.microsoft.com/en-us/azure/billing/billing-download-azure-invoice-daily-usage-date#download-usage-from-the-account-center-csv). Die Datei mit Nutzungsdetails enthält die Azure Stack-Verbrauchseinheiten, die Azure Stack-Speicher und -VMs identifizieren. Alle in Azure Stack verwendeten Ressourcen werden im Abschnitt „Azure Stack“ gemeldet.

## <a name="why-doesnt-the-usage-reported-in-azure-stack-match-the-report-generated-from-azure-account-center"></a>Wieso stimmen die in Azure Stack gemeldeten Nutzungsdaten nicht mit dem vom Azure-Kontocenter erstellten Bericht überein?

Zwischen den von den Azure Stack-Nutzungs-APIs und den vom Azure-Kontocenter gemeldeten Nutzungsdaten tritt immer eine Verzögerung auf. Die Verzögerung ergibt sich aus der Zeit, die zum Hochladen der Nutzungsdaten von Azure Stack in das Commerce-System von Azure benötigt wird. Aufgrund dieser Verzögerung kann es vorkommen, dass eine Nutzung kurz vor Mitternacht am nächsten Tag in Azure angezeigt wird. Wenn Sie die [Azure Stack-Nutzungs-APIs](azure-stack-provider-resource-api.md) verwenden und die Ergebnisse mit den auf dem Azure-Abrechnungsportal angezeigten Nutzungsdaten vergleichen, können Sie eine Abweichung feststellen.

## <a name="next-steps"></a>Nächste Schritte

* [Anbieternutzungs-API](azure-stack-provider-resource-api.md)  
* [Mandantennutzungs-API](azure-stack-tenant-resource-usage-api.md)
* [FAQ zur Nutzung](azure-stack-usage-related-faq.md)
