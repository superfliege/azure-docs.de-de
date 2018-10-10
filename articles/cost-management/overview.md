---
title: Übersicht über Cloudyn in Azure | Microsoft-Dokumentation
description: Cloudyn ist eine Kostenverwaltungslösung für mehrere Clouds, die das Verwenden von Azure und anderen Cloudressourcen vereinfacht.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 09/18/2018
ms.topic: overview
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 9f0eac454d6ac937fc3313e4c50f8872e320c7ca
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46985383"
---
# <a name="what-is-cloudyn"></a>Was ist Cloudyn?

Cloudyn, eine Tochtergesellschaft von Microsoft, ermöglicht Ihnen das Nachverfolgen der Cloudnutzung und der Ausgaben für Ihre Azure-Ressourcen sowie andere Cloudanbieter, einschließlich AWS und Google. Leicht verständliche Dashboardberichte unterstützen Sie bei der Kostenzuteilung sowie bei Showbacks und verbrauchsbasierten Kostenzuteilungen. Cloudyn unterstützt Sie beim Optimieren der Ausgaben für Ihre Cloud, indem nicht ausgelastete Ressourcen ermittelt werden, die Sie dann verwalten und anpassen können.

Ein Einführungsvideo finden Sie unter [Introduction to Azure Cloudyn](https://azure.microsoft.com/resources/videos/azure-cost-management-overview-and-demo) (Einführung in Azure Cloudyn).

Azure Cost Management bietet ähnliche Funktionen wie Cloudyn. Azure Cost Management ist eine native Azure-Kostenverwaltungslösung. Die Lösung unterstützt Sie beim Analysieren von Kosten, beim Erstellen und Verwalten von Budgets, beim Exportieren von Daten sowie beim Prüfen von Optimierungsempfehlungen und beim Reagieren auf diese – und damit beim Sparen von Geld. Weitere Informationen finden Sie unter [Azure Cost Management](overview-cost-mgt.md).

## <a name="monitor-usage-and-spending"></a>Überwachen der Nutzung und Ausgaben

Das Überwachen Ihrer Nutzung und Ausgaben ist für Cloudinfrastrukturen ausgesprochen wichtig, da Organisationen für die Ressourcen zahlen, die Sie im Lauf der Zeit verwenden. Wenn die Nutzung die Schwellenwerte der Vereinbarung übersteigt, kann schnell eine unerwartete Überschreitung der Kosten auftreten. Einige wichtige Faktoren können die Ad-hoc-Überwachung erschweren. Erstens wird beim Projizieren von Kosten basierend auf der durchschnittlichen Nutzung davon ausgegangen, dass Ihre Nutzung über einen bestimmten Abrechnungszeitraum konsistent bleibt. Zweitens ist es wichtig, dass Sie proaktiv Benachrichtigungen erhalten, um Ihre Ausgaben anzupassen, wenn die Kosten sich Ihrem Budget annähern oder dieses überschreiten. Zudem bieten Clouddienstanbieter nicht notwendigerweise Berichte für die Kostenprojektion im Vergleich mit den Schwellenwerten oder für einen Vergleich von Zeiträumen an.

Berichte unterstützen Sie bei der Überwachung Ihrer Ausgaben, um die Cloudnutzung, Kosten und Trends zu analysieren und nachzuverfolgen. Durch das Verwenden von Berichten für den Zeitverlauf können Sie Anomalien erkennen, die von den normalen Trends abweichen. Ineffizienzen in Ihrer Cloudbereitstellung werden in Optimierungsberichten angezeigt. Sie können Ineffizienzen auch in Kostenanalyseberichten feststellen.

## <a name="manage-costs"></a>Verwalten von Kosten

Verlaufsdaten können Sie beim Verwalten der Kosten unterstützen, wenn Sie die Nutzung und die Kosten im Lauf der Zeit analysieren, um Trends zu erkennen. Trends werden dann verwendet, um zukünftige Ausgaben vorherzusagen. Cloudyn enthält auch nützliche Berichte über geplante Kosten.

Die Kostenzuteilung verwaltet Kosten, indem Ihre Kosten basierend auf Ihrer Tagrichtlinie analysiert werden. Sie können Tags für Ihre benutzerdefinierten Konten, Ressourcen und Entitäten verwenden, um die Kostenzuteilung zu optimieren. Der Kategorie-Manager organisiert Ihre Tags, um zusätzliche Kontrolle zu ermöglichen. Sie verwenden die Kostenzuteilung ebenfalls für Showback und die verbrauchsbasierte Kostenzuteilung, um die Ressourcennutzung und die zugehörigen Kosten anzuzeigen und das Nutzungsverhalten zu beeinflussen oder diese Mandantenkunden in Rechnung zu stellen.

Die Zugriffssteuerung unterstützt Sie beim Verwalten der Kosten, indem sichergestellt wird, dass Benutzer und Teams nur auf die Daten der Kostenverwaltung zugreifen können, die sie benötigen. Sie verwenden eine Entitätsstruktur, die Benutzerverwaltung und geplante Berichte mit Empfängerlisten, um den Zugriff zuzuweisen.

Warnungen unterstützen Sie beim Verwalten der Kosten, indem Sie automatisch benachrichtigt werden, wenn ungewöhnliche Ausgaben oder eine Budgetüberschreitung auftreten. Durch Warnungen können auch andere Beteiligte automatisch über Anomalien bei den Ausgaben und das Risiko einer Budgetüberschreitung benachrichtigt werden. Verschiedene Berichte unterstützen Warnungen, die auf dem Budget und auf Kostenschwellenwerten basieren. Für Konten oder Abonnements von CSP-Partnern werden Warnungen jedoch derzeit nicht unterstützt.

## <a name="improve-efficiency"></a>Verbessern der Effizienz

Mit Cloudyn können Sie die optimale Nutzung der VMs bestimmen, VMs im Leerlauf identifizieren und VMs im Leerlauf sowie nicht angefügte Datenträger entfernen. Mithilfe der Informationen in den Berichten zu Größenempfehlungen und Ineffizienzen können Sie einen Plan erstellen, um die Größe von VMs im Leerlauf zu reduzieren oder diese zu entfernen. Für Konten oder Abonnements von CSP-Partnern werden Optimierungsberichte jedoch derzeit nicht unterstützt.

Wenn Sie für AWS reservierte Instanzen bereitgestellt haben, können Sie die Nutzung Ihrer reservierten Instanzen mit Optimierungsberichten verbessern, in denen Sie Kaufempfehlungen einsehen, ungenutzte Reservierungen ändern und Bereitstellungen planen können.

## <a name="next-steps"></a>Nächste Schritte

Da Sie nun mit Cloudyn vertraut sind, bestehen die nächsten Schritte im Registrieren Ihrer Cloudumgebung und dem Untersuchen Ihrer Daten.

- [Register an individual Azure subscription (Registrieren eines einzelnen Azure-Abonnements)](quick-register-azure-sub.md)
