---
title: Erste Schritte mit Azure CDN | Microsoft-Dokumentation
description: "In diesem Thema wird die Aktivierung des Azure Content Delivery Network (CDN) veranschaulicht. Das Tutorial enthält ausführliche Informationen zur Erstellung eines neuen CDN-Profils und -Endpunkts."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 4ca51224-5423-419b-98cf-89860ef516d2
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/25/2018
ms.author: mazha
ms.openlocfilehash: 81a88f6495ca9092ca3b55b8ffb3e41def3b4623
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2018
---
# <a name="getting-started-with-azure-cdn"></a>Erste Schritte mit Azure CDN
In diesem Artikel erfahren Sie, wie Sie Azure CDN (Content Delivery Network) durch das Erstellen eines neuen CDN-Profils und -Endpunkts aktivieren.

> [!IMPORTANT]
> Eine Einführung in CDN sowie eine Featureliste finden Sie in der [CDN-Übersicht](cdn-overview.md).
> 
> 

## <a name="create-a-new-cdn-profile"></a>Erstellen eines neuen CDN-Profils
Ein CDN-Profil ist eine Sammlung von CDN-Endpunkten. Jedes Profil kann mindestens einen CDN-Endpunkt enthalten. Sie können mehrere Profile verwenden, um Ihre CDN-Endpunkte nach Internetdomäne, Webanwendung oder anderen Kriterien zu organisieren.

> [!NOTE]
> Ein Azure-Abonnement verfügt für die folgenden Ressourcen über Standardgrenzwerte:
> - Anzahl von CDN-Profilen, die erstellt werden können
> - Anzahl von Endpunkten, die in einem CDN-Profil erstellt werden können 
> - Anzahl von benutzerdefinierten Domänen, die einem Endpunkt zugeordnet werden können
>
> Informationen zu Grenzwerten für CDN-Abonnements finden Sie unter [CDN-Grenzwerte](https://docs.microsoft.com/azure/azure-subscription-service-limits#cdn-limits).
>
> Die Preise für Azure CDN gelten auf der Ebene von CDN-Profilen. Um mehrere Azure CDN-Tarife zu kombinieren, müssen Sie mehrere CDN-Profile erstellen.
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Erstellen eines neuen CDN-Endpunkts
**So erstellen Sie einen neuen CDN-Endpunkt**

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem CDN-Profil. Eventuell haben Sie es im vorherigen Schritt an das Dashboard angeheftet. Falls nicht, können Sie darauf zugreifen, indem Sie **Alle Dienste** und dann **CDN-Profile** wählen. Wählen Sie im Bereich **CDN-Profile** das Profil aus, dem Sie Ihren Endpunkt hinzufügen möchten. 
   
    Der Bereich „CDN-Profil“ wird angezeigt.
   
    ![CDN-Profil][cdn-profile-settings]

2. Wählen Sie **Endpunkt**.
   
    ![Schaltfläche „Endpunkt hinzufügen“][cdn-new-endpoint-button]
   
    Der Bereich **Endpunkt hinzufügen** wird angezeigt.
   
    ![Bereich „Endpunkt hinzufügen“][cdn-add-endpoint]

3. Geben Sie unter **Name** einen eindeutigen Namen für den neuen CDN-Endpunkt ein. Dieser Name wird für den Zugriff auf Ihre zwischengespeicherten Ressourcen in der Domäne `<endpointname>.azureedge.net` verwendet.

4. Wählen Sie unter **Ursprungstyp** einen Ursprungstyp aus. Wählen Sie **Storage** für ein Azure-Speicherkonto, **Clouddienst** für einen Azure-Clouddienst, **Web-App** für eine Azure-Web-App oder **Benutzerdefinierter Ursprung** für einen öffentlich zugänglichen Webserver (in Azure oder anderswo gehostet).
   
    ![CDN-Ursprung](./media/cdn-create-new-endpoint/cdn-origin-type.png)

5. Wählen Sie unter **Hostname des Ursprungs** Ihre Ursprungsdomäne aus, oder geben Sie sie ein. In der Dropdownliste werden alle verfügbaren Ursprungstypen aufgelistet, die Sie in Schritt 4 angegeben haben. Geben Sie die Domäne Ihres benutzerdefinierten Ursprungs ein, wenn Sie **Benutzerdefinierter Ursprung** als Ursprungstyp auswählen.
    
6. Geben Sie unter **Ursprünglicher Pfad** den Pfad zu den Ressourcen ein, die Sie zwischenspeichern möchten, oder lassen Sie das Feld leer, um das Zwischenspeichern für alle Ressourcen aus Schritt 5 in der Domäne zuzulassen.
    
7. Geben Sie unter **Header des Ursprungshosts** den Hostheader ein, den das Azure CDN bei jeder Anforderung senden soll, oder übernehmen Sie den Standardwert.
   
   > [!WARNING]
   > Bei einigen Ursprungstypen wie etwa Azure Storage und Web-Apps muss der Hostheader mit der Domäne des Ursprungs übereinstimmen. Verwenden Sie den Standardwert, es sei denn, Sie nutzen einen Ursprung, bei dem der Hostheader von der Domäne abweichen muss.
   > 
    
8. Geben Sie unter **Protokoll** und **Ursprungsport** die Protokolle und Ports an, über die auf die Ressourcen am Ursprung zugegriffen werden soll. Sie müssen mindestens ein Protokoll (HTTP oder HTTPS) auswählen. Verwenden Sie die vom CDN bereitgestellte Domäne (`<endpointname>.azureedge.net`), um auf HTTPS-Inhalte zuzugreifen. 
   
   > [!NOTE]
   > Mit dem Wert unter **Ursprungsport** wird nur der Port bestimmt, der vom Endpunkt zum Abrufen von Informationen vom Ursprung verwendet wird. Der Endpunkt selbst ist nur für Endclients auf den HTTP- und HTTPS-Standardports (80 und 443) verfügbar, und zwar unabhängig vom Wert unter **Ursprungsport**.  
   > 
   > Für Endpunkte in Profilen vom Typ **Azure CDN von Akamai** wird nicht der vollständige TCP-Portbereich für Ursprungsports zugelassen. Eine Liste der nicht zulässigen Ursprungsports finden Sie unter [Azure CDN from Akamai Allowed Origin Ports](https://msdn.microsoft.com/library/mt757337.aspx)(Azure CDN von Akamai – Zulässige Ursprungsports).  
   > 
   > Beim Zugreifen auf CDN-Inhalt per HTTPS gelten die folgenden Einschränkungen:
   > 
   > * Verwenden Sie das vom CDN bereitgestellte SSL-Zertifikat. Zertifikate von Drittanbietern werden nicht unterstützt.
   > * HTTPS-Unterstützung für benutzerdefinierte Azure CDN-Domänen ist nur für Produkte vom Typ **Azure CDN von Verizon** (Standard und Premium) verfügbar. Für **Azure CDN von Akamai** wird es nicht unterstützt. Weitere Informationen finden Sie unter [Aktivieren oder Deaktivieren von HTTPS in einer benutzerdefinierten Azure Content Delivery Network-Domäne](cdn-custom-ssl.md).
    
9. Wählen Sie **Hinzufügen**, um den neuen Endpunkt zu erstellen.
   
   Der erstellte Endpunkt wird in der Liste mit den Endpunkten für das Profil angezeigt.
    
   ![CDN-Endpunkt][cdn-endpoint-success]
    
   > [!IMPORTANT]
   > Der Endpunkt kann nicht sofort verwendet werden, da die Verteilung der Registrierung über das CDN eine Weile dauern kann. Bei **Azure CDN von Akamai** -Profilen ist die Weitergabe in der Regel in einer Minute abgeschlossen. Bei Profilen vom Typ **Azure CDN von Verizon** ist die Weitergabe in der Regel in 90 Minuten abgeschlossen, in manchen Fällen kann es aber länger dauern.
    > 
    > Wenn Sie versuchen, den CDN-Domänennamen zu verwenden, bevor die Endpunktkonfiguration an die POPs verteilt wurde, wird unter Umständen der Antwortstatus „HTTP 404“ angezeigt. Falls Sie auch mehrere Stunden nach Erstellung des Endpunkts noch den Antwortstatus 404 erhalten, helfen Ihnen die Informationen zur [Problembehandlung bei CDN-Endpunkten mit Status 404](cdn-troubleshoot-endpoint.md) weiter.
    > 
    > 

## <a name="see-also"></a>Siehe auch
* [Steuern des Zwischenspeicherverhaltens von Anforderungen mit Abfragezeichenfolgen](cdn-query-string.md)
* [Zuordnen von CDN-Inhalt (Content Delivery Network) zu einer benutzerdefinierten Domäne](cdn-map-content-to-custom-domain.md)
* [Vorabladen von Assets auf einen Azure CDN-Endpunkt](cdn-preload-endpoint.md)
* [Löschen eines Azure CDN-Endpunkts](cdn-purge-endpoint.md)
* [Troubleshooting CDN endpoints returning 404 statuses (Problembehandlung bei CDN-Endpunkten mit Status 404)](cdn-troubleshoot-endpoint.md)

[cdn-profile-settings]: ./media/cdn-create-new-endpoint/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-new-endpoint/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-new-endpoint/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-new-endpoint/cdn-endpoint-success.png
