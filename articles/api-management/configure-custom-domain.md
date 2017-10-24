---
title: "Konfigurieren eines benutzerdefinierten Domänennamens für Ihre Azure API Management-Instanz | Microsoft-Dokumentation"
description: "In diesem Thema erfahren Sie, wie Sie einen benutzerdefinierten Domänennamen für Ihre Azure API Management-Instanz konfigurieren."
services: api-management
documentationcenter: 
author: vladvino
manager: anneta
editor: 
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 08/17/2017
ms.author: apimpm
ms.openlocfilehash: a7abba87035016eee05b4ab663404f10127bba1a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-custom-domain-name"></a>Konfigurieren eines benutzerdefinierten Domänennamens 

Wenn Sie eine APIM-Instanz (API Management) erstellen, wird sie von Azure einer Unterdomäne von azure-api.net (beispielsweise `apim-service-name.azure-api.net`) zugewiesen. Sie können Ihre APIM-Endpunkte jedoch unter Ihrem eigenen Domänennamen (beispielsweise **contoso.com**) verfügbar machen. In diesem Tutorial erfahren Sie, wie Sie Endpunkten, die durch eine Azure API Management-Instanz verfügbar gemacht werden, einen vorhandenen benutzerdefinierten DNS-Namen zuordnen.


## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen der in diesem Artikel beschriebenen Schritte benötigen Sie Folgendes:

+ Ein aktives Azure-Abonnement.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ Eine APIM-Instanz. Weitere Informationen finden Sie unter [Erstellen einer neuen Azure API Management-Dienstinstanz](get-started-create-service-instance.md).
+ Einen benutzerdefinierten Domänennamen, der sich in Ihrem Besitz befindet. Der benutzerdefinierte Domänenname, den Sie verwenden möchten, muss separat bezogen und auf einem DNS-Server gehostet werden. Dieses Thema enthält keine Anleitung zum Hosten eines benutzerdefinierten Domänennamens.
+ Sie benötigen ein gültiges Zertifikat mit einem öffentlichen und privaten Schlüssel (.pfx). Der Antragstellername oder der alternative Antragstellername (Subject Alternative Name, SAN) muss dem Domänennamen entsprechen, damit APIM sicher URLs über SSL verfügbar machen kann.

## <a name="use-the-azure-portal-to-set-a-custom-domain-name"></a>Festlegen eines benutzerdefinierten Domänennamens über das Azure-Portal

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) zu Ihrer APIM-Instanz.
2. Klicken Sie auf **Benutzerdefinierte Domänen und SSL**.
    
    Es stehen mehrere Endpunkte zur Verfügung, denen Sie einen benutzerdefinierten Domänennamen zuweisen können. Derzeit sind folgende Endpunkte verfügbar: 
    + **Proxy** (Standardwert: `<apim-service-name>.azure-api.net`) 
    + **Portal** (Standardwert: `<apim-service-name>.portal.azure-api.net`)     
    + **Verwaltung** (Standardwert: `<apim-service-name>.management.azure-api.net`) 
    + **SCM** (Standardwert: `<apim-service-name>.scm.azure-api.net`).

    >[!NOTE]
    > Sie können alle oder nur bestimmte Endpunkte aktualisieren. Kunden aktualisieren üblicherweise **Proxy** (URL zum Aufrufen der über API Management verfügbar gemachten API) und **Portal** (URL des Entwicklerportals). Die Endpunkte **Verwaltung** und **SCM** werden intern von APIM-Kunden verwendet, weshalb ihnen seltener ein benutzerdefinierter Domänenname zugewiesen wird.
3. Wählen Sie den Endpunkt aus, den Sie aktualisieren möchten. 
4. Klicken Sie im Fenster auf der rechten Seite auf **Benutzerdefiniert**.

    + Geben Sie unter **Benutzerdefinierter Domänenname** den gewünschten Namen an. Beispiel: `api.contoso.com`. <br/>Platzhalterdomänennamen wie etwa „*.domain.com“ werden ebenfalls unterstützt.
    + Geben Sie unter **Zertifikat** eine gültige .PFX-Datei an, die Sie hochladen möchten. 
    + Falls das Zertifikat über ein Kennwort verfügt, geben Sie dieses in das Feld **Kennwort** ein.
1. Klicken Sie auf „Übernehmen“.

    >[!NOTE]
    >Die Zuweisung des Zertifikats kann etwa 15 Minuten dauern.

## <a name="next-steps"></a>Nächste Schritte

[Upgrade and scale an API Management instance](upgrade-and-scale.md) (Upgraden und Skalieren einer API Management-Instanz)