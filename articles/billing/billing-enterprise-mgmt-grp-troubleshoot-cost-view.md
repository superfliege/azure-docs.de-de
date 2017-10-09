---
title: "Problembehandlung bei Unternehmenskostenansichten – Azure | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Probleme beheben, die bei Unternehmenskostenansichten im Azure-Portal auftreten könnten."
author: rthorn17
manager: rithorn
editor: 
ms.assetid: 
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: rithorn
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: eca1ac9ed51e6c2243be451a074792fbec2840d2
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="troubleshoot-enterprise-cost-views"></a>Problembehandlung bei Unternehmenskostenansichten 

Bei Unternehmensanmeldungen gibt es mehrere Einstellungen, die verhindern können, dass Benutzer der Anmeldung Kosten nicht anzeigen können.  Diese Einstellungen werden durch den Registrierungsadministrator verwaltet, oder den Partner, wenn die Registrierung nicht direkt bei Microsoft erworben wird.  In diesem Artikel erfahren Sie, um welche Einstellungen es geht, und wie sie sich auf die Anmeldung auswirken. Diese Einstellungen sind unabhängig von den [Azure-RBAC-Rollen](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-configure). 

> [!Note]
> Dieses Feature ist zurzeit als private Vorschau verfügbar. [Melden Sie sich hier](https://forms.office.com/Pages/DesignPage.aspx#FormId=v4j5cvGGr0GRqy180BHbR0YtfU6ham9OsGsPPYdu2xdUNk1BQUwzTkUyOVc5NUpCTFcwR0pIOVFETS4u) für die Vorschau an.     

## <a name="enabling-access-to-costs"></a>Aktivieren des Zugriffs auf Kosten

Erhalten Sie die Meldung „Nicht autorisiert“ oder *Kostenansichten sind in Ihrer Registrierung deaktiviert*, wenn Sie Kosteninformationen suchen? ![Nicht autorisiert](media/billing-enterprise-mgmt-groups/unauthorized.png)

Dies kann einen der folgenden Gründe haben:

1. Sie haben Azure über einen Enterprise-Partner erworben, und der Partner hat die Preisinformationen noch nicht freigegeben. Bitten Sie den Partner, die Einstellung im [Enterprise Portal](https://ea.azure.com) zu aktualisieren, um die Preisinformationen freizugeben.
2. Wenn Sie alternativ ein EA-Direktkunde sind, haben Sie eine Reihe von Möglichkeiten:
    * Sie sind Kontobesitzer, und der Registrierungsadministrator hat die Einstellung „Gebühren anzeigen“ für Kontobesitzer deaktiviert.  
    * Sie sind Abteilungsadministrator, und der Registrierungsadministrator hat die Einstellung „Gebühren anzeigen“ für Abteilungsadministratoren deaktiviert.
    * Wenden Sie sich an den Registrierungsadministrator, um Zugriff zu erhalten. Der Registrierungsadministrator kann die Einstellung im [Enterprise Portal](https://ea.azure.com/manage/enrollment) wie hier gezeigt aktualisieren:

![Einstellungen im Enterprise Portal](media/billing-enterprise-mgmt-groups/ea-portal-settings.png)


## <a name="asset-is-unavailable"></a>Asset ist nicht verfügbar? 
Wenn Sie beim Versuch, auf ein Abonnement oder eine Verwaltungsgruppe zuzugreifen, die Fehlermeldung „Dieses Asset ist nicht verfügbar“ erhalten, dann besitzen Sie nicht die richtige Rolle, um dieses Element anzuzeigen.  

![Asset nicht gefunden](media/billing-enterprise-mgmt-groups/asset-not-found.png)

Wenden Sie sich an den Administrator des Abonnements oder der Verwaltungsgruppe, um Zugriff zu erhalten.  
* Nutzen Sie für Abonnements das Dokument [Verwenden der rollenbasierten Zugriffssteuerung zum Verwalten des Zugriffs auf Ihre Azure-Abonnementressourcen](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-configure) als Referenz, um zu ermitteln, welche Rolle erforderlich ist.
* Für Verwaltungsgruppen ist RBAC-Zugriff noch nicht verfügbar, wird jedoch bald verfügbar sein. Bitten Sie Ihren Enterprise Portal-Administrator, Ihnen den Zugriff zuzuweisen.   

