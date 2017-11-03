---
title: Verschieben einer Azure-Web-App in eine andere Ressourcengruppe | Microsoft-Dokumentation
description: "Beschreibt die Szenarien, in denen Sie Web-Apps und App Services aus einer Ressourcengruppe in eine andere verschieben können."
services: app-service
documentationcenter: 
author: ZainRizvi
manager: erikre
editor: 
ms.assetid: 22f97607-072e-4d1f-a46f-8d500420c33c
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: zarizvi
ms.openlocfilehash: 4d5d6540dd0b51a4f172649a1fd9a2343efeceee
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2017
---
# <a name="move-an-azure-web-app-to-another-resource-group"></a>Verschieben einer Azure-Web-App in eine andere Ressourcengruppe
Sie können eine Web-App und/oder die zugehörigen Ressourcen in eine andere Ressourcengruppe desselben Abonnements oder eine Ressourcengruppe eines anderen Abonnements verschieben. Dies gehört zur standardmäßigen Ressourcenverwaltung in Azure. Weitere Informationen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/resource-group-move-resources.md).

## <a name="limitations-when-moving-within-the-same-subscription"></a>Einschränkungen beim Verschieben innerhalb desselben Abonnements

Beim Verschieben einer Web-App _innerhalb desselben Abonnements_ können Sie die hochgeladenen SSL-Zertifikate nicht verschieben. Allerdings können Sie eine Web-App in die neue Ressourcengruppe verschieben, ohne ihr hochgeladenes SSL-Zertifikat zu verschieben, und die SSL-Funktionalität Ihrer App wird davon nicht beeinträchtigt. 

Wenn Sie das SSL-Zertifikat mit der Web-App verschieben möchten, gehen Sie folgendermaßen vor:

1.  Löschen Sie das hochgeladene Zertifikat aus der Web-App.
2.  Verschieben Sie die Web-App.
3.  Laden Sie das Zertifikat in die verschobene Web-App hoch.

## <a name="limitations-when-moving-across-subscriptions"></a>Einschränkungen beim Verschieben zwischen Abonnements

Beim Verschieben einer Web-App _zwischen Abonnements_ gelten die folgenden Einschränkungen:

- Die Zielressourcengruppe darf keine App Service-Ressourcen besitzen. Zu App Service-Ressourcen zählen:
    - Web-Apps
    - App Service-Pläne
    - Hochgeladene oder importierte SSL-Zertifikate
    - App Service-Umgebungen
- Alle App Service-Ressourcen in der Ressourcengruppe müssen zusammen verschoben werden.
- App Service-Ressourcen können nur aus der Ressourcengruppe verschoben werden, in der sie ursprünglich erstellt wurden. Wenn eine App Service-Ressource sich nicht mehr in ihrer ursprünglichen Ressourcengruppe befindet, muss sie erst zurück in die ursprüngliche Ressourcengruppe verschoben werden, bevor sie zwischen Abonnements verschoben werden kann. 
