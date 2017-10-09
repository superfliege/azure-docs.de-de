---
title: Kundenabrechnung und verbrauchsbasierte Kostenzuteilung in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Informationen zum Ressourcenverbrauch aus Azure Stack abrufen.
services: azure-stack
documentationcenter: 
author: AlfredoPizzirani
manager: byronr
editor: 
ms.assetid: a9afc7b6-43da-437b-a853-aab73ff14113
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: alfredop
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: ea7510c239ee07a9a27f3e682e61a6b08eb5694d
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="usage-and-billing-in-azure-stack"></a>Verbrauch und Abrechnung in Azure Stack

Der Verbrauch stellt die Menge an Ressourcen dar, die von einem Benutzer genutzt wurden. Azure Stack sammelt Nutzungsinformationen für jeden Benutzer und verwendet diese für die Abrechnung. Dieser Artikel erläutert, wie die Abrechnung des Ressourcenverbrauchs für Azure Stack-Benutzer erfolgt und wie Sie zur Analyse und verbrauchsbasierten Kostenzuteilung auf die Abrechnungsinformationen zugreifen können.

Azure Stack enthält die Infrastruktur zum Sammeln und Aggregieren der Nutzungsdaten für alle Ressourcen sowie zum Weiterleiten dieser Daten an Azure Commerce. Sie können einen Abrechnungsadapter verwenden, um auf diese Daten zuzugreifen und sie in ein Abrechnungssystem zu exportieren. Alternativ dazu können Sie die Daten auch in ein Business Intelligence-Tool wie z.B. Microsoft Power BI exportieren. Nach dem Export werden diese Abrechnungsinformationen zu Analysezwecken verwendet oder in ein System für die verbrauchsbasierte Kostenzuteilung übertragen.

![Konzeptionelles Modell eines Abrechnungsadapters zur Verknüpfung von Azure Stack mit einer Abrechnungsanwendung](media/azure-stack-billing-and-chargeback/image1.png)

## <a name="usage-pipeline"></a>Nutzungspipeline

Jeder Ressourcenanbieter in Azure Stack gibt Nutzungsdaten gemäß Ressourcennutzung aus. Der Nutzungsdienst aggregiert diese Nutzungsdaten in regelmäßigen Abständen (stündlich oder täglich) und speichert sie in der Nutzungsdatenbank. Auf die gespeicherten Nutzungsdaten kann von Azure Stack-Betreibern und -Benutzern lokal mithilfe von Nutzungs-APIs zugegriffen werden. 

Wenn Sie [Ihre Azure Stack-Instanz bei Azure registriert haben](azure-stack-register.md), wird die Nutzungsbrücke zum Senden der Nutzungsdaten an Azure Commerce konfiguriert. Wenn die Daten in Azure verfügbar sind, können Sie darauf über das Abrechnungsportal oder mithilfe der Nutzungs-APIs von Azure zugreifen. Weitere Informationen dazu, welche Nutzungsdaten an Azure gemeldet werden, finden Sie im Thema [Report Azure Stack usage data to Azure](azure-stack-usage-reporting.md) (Melden von Azure Stack-Nutzungsdaten an Azure). 

Die folgende Abbildung zeigt die wichtigsten Komponenten in der Nutzungspipeline:

![Nutzungspipeline](media/azure-stack-billing-and-chargeback/usagepipeline.png)

## <a name="what-usage-information-can-i-find-and-how"></a>Welche Nutzungsinformationen kann ich suchen, und wo finde ich sie?

Azure Stack-Ressourcenanbieter wie Compute-, Speicher- und Netzwerkressourcen generieren stündlich Nutzungsdaten für jedes Abonnement. Die Nutzungsdaten enthalten Informationen zur verwendeten Ressource, z.B. den Ressourcennamen, das verwendete Abonnement, die genutzte Menge usw. Weitere Informationen zu den Verbrauchseinheiten finden Sie im Artikel [Häufig gestellte Fragen zu Nutzungs-APIs](azure-stack-usage-related-faq.md). 

Die gesammelten Nutzungsdaten werden [an Azure gemeldet](azure-stack-usage-reporting.md), um eine Rechnung zu generieren, die im Azure-Abrechnungsportal angezeigt werden kann. 

> [!NOTE]
> Das Melden von Nutzungsdaten ist für Azure Stack Development Kit-Benutzer und Benutzer von integrierten Azure Stack-Systemen, die unter dem Kapazitätsmodell lizenziert sind, nicht erforderlich. Weitere Informationen zur Lizenzierung in Azure Stack finden Sie im Datenblatt zu [Paketerstellung und Preisen](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf).

Das Azure-Abrechnungsportal zeigt nur die Nutzungsdaten für kostenpflichtige Ressourcen. Zusätzlich zu den kostenpflichtigen Ressourcen erfasst Azure Stack Nutzungsdaten für eine umfassendere Palette an Ressourcen. Auf diese Daten können Sie in Ihrer Azure Stack-Umgebung über REST-APIs oder PowerShell zugreifen. Azure Stack-Betreiber können die Nutzungsdaten für alle Benutzerabonnements abrufen, Benutzer können jedoch nur ihre eigenen Verbrauchsdaten abrufen.

## <a name="retrieve-usage-information"></a>Abrufen von Nutzungsinformationen

Um Nutzungsdaten zu generieren, benötigen Sie Ressourcen, die ausgeführt werden und das System aktiv verwenden, beispielsweise einen aktiven virtuellen Computer oder ein Speicherkonto mit Daten. Wenn Sie nicht sicher sind, ob Sie über Ressourcen verfügen, die im Azure Stack Marketplace ausgeführt werden, stellen Sie einen virtuellen Computer bereit, und überprüfen Sie das Überwachungsblatt für diesen Computer, um sicherzustellen, dass er ausgeführt wird. Verwenden Sie die folgenden PowerShell-Cmdlets, um die Nutzungsdaten anzuzeigen:

1. [Installieren Sie PowerShell für Azure Stack-](azure-stack-powershell-install.md)
2. Konfigurieren Sie die PowerShell-Umgebung des [Azure Stack-Benutzers](user/azure-stack-powershell-configure-user.md) oder des [Azure Stack-Betreibers](azure-stack-powershell-configure-admin.md). 

3. Um die Nutzungsdaten abzurufen, verwenden Sie das PowerShell-Cmdlet [Get-UsageAggregates](/powershell/module/azurerm.usageaggregates/get-usageaggregates):

   ```powershell
   Get-UsageAggregates -ReportedStartTime "<Start time for usage reporting>" -ReportedEndTime "<end time for usage reporting>" -AggregationGranularity <Hourly or Daily>
   ```

## <a name="next-steps"></a>Nächste Schritte

[Melden von Azure Stack-Nutzungsdaten an Azure](azure-stack-usage-reporting.md)

[Ressourcennutzungs-API für Anbieter](azure-stack-provider-resource-api.md)

[Ressourcennutzungs-API für Mandanten](azure-stack-tenant-resource-usage-api.md)

[Häufig gestellte Fragen zur Nutzung](azure-stack-usage-related-faq.md)


