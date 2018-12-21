---
title: 'Tutorial: Aktivieren einer benutzerdefinierten Domäne mit SSL für eine statische Website mit Azure CDN: Azure Storage'
description: Erfahren Sie, wie Sie eine benutzerdefinierte Domäne für das Hosten von statischen Websites konfigurieren.
services: storage
author: tamram
ms.service: storage
ms.topic: tutorial
ms.date: 12/07/2018
ms.author: tamram
ms.custom: seodec18
ms.openlocfilehash: 6ccd33805fe4b62d3456121321edc4eec3bff2e5
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53110315"
---
# <a name="tutorial-use-azure-cdn-to-enable-a-custom-domain-with-ssl-for-a-static-website"></a>Tutorial: Verwenden von Azure CDN zum Aktivieren einer benutzerdefinierten Domäne mit SSL für eine statische Website

Dieses Tutorial ist der zweite Teil einer Reihe. In diesem Teil erfahren Sie, wie Sie einen benutzerdefinierten Domänenendpunkt mit SSL für Ihre statische Website aktivieren. 

Das Tutorial veranschaulicht, wie Sie mit [Azure CDN](../../cdn/cdn-overview.md) den benutzerdefinierten Domänenendpunkt für Ihre statische Website konfigurieren. Mit Azure CDN können Sie gleichzeitig benutzerdefinierte SSL-Zertifikate bereitstellen, eine benutzerdefinierte Domäne verwenden und benutzerdefinierte Regeln für das erneute Generieren konfigurieren. Durch die Konfiguration von Azure CDN fallen zusätzliche Gebühren an, der Dienst bietet jedoch auf der ganzen Welt durchgängig kurze Wartezeiten für Ihre Website. Azure CDN bietet auch eine SSL-Verschlüsselung mit Ihrem eigenen Zertifikat. Weitere Informationen zu den Preisen von Azure CDN finden Sie unter [Azure Content Delivery Network – Preise](https://azure.microsoft.com/pricing/details/cdn/).

Im zweiten Teil der Reihe lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines CDN-Endpunkts für den Endpunkt der statischen Website
> * Aktivieren der benutzerdefinierten Domäne und von SSL

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie Teil 1 [Tutorial: Host a static website on Blob Storage](storage-blob-static-website-host.md) (Tutorial: Hosten einer statischen Website in Blob Storage) aus, bevor Sie mit diesem Tutorial beginnen. 

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich zunächst beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="create-a-cdn-endpoint-on-the-static-website-endpoint"></a>Erstellen eines CDN-Endpunkts für den Endpunkt der statischen Website

1. Wechseln Sie in Ihrem Webbrowser zum [Azure-Portal](https://portal.azure.com/). 
1. Suchen Sie nach Ihrem Speicherkonto, und zeigen Sie die Kontoübersicht an.
1. Wählen Sie im Menü **Blob-Dienst** die Option **Azure CDN** aus, um Azure CDN zu konfigurieren.
1. Füllen Sie im Abschnitt **Neuer Endpunkt** die Felder aus, um einen neuen CDN-Endpunkt zu erstellen.
1. Geben Sie einen Endpunktnamen ein, z. B. *mystaticwebsiteCDN*.
1. Geben Sie die Domäne Ihrer Website als Hostnamen für den CDN-Endpunkt ein.
1. Geben Sie für den Hostnamen des Ursprungs den Endpunkt Ihrer statischen Website ein. Um den Endpunkt Ihrer statischen Website zu ermitteln, navigieren Sie zum Abschnitt **Statische Website** Ihres Speicherkontos, und kopieren Sie den Endpunkt. 
1. Testen Sie Ihren CDN-Endpunkt, indem Sie in Ihrem Browser zu *mywebsitecdn.azureedge.net* navigieren.

## <a name="enable-custom-domain-and-ssl"></a>Aktivieren der benutzerdefinierten Domäne und von SSL

1. Erstellen Sie einen CNAME-Eintrag beim Anbieter Ihres Domänennamens, um Ihre benutzerdefinierte Domäne zum CDN-Endpunkt umzuleiten. Der CNAME-Eintrag für die *www*-Unterdomäne sollte in etwa wie folgt aussehen:

    ![Angeben des CNAME-Eintrags für die www-Unterdomäne](media/storage-blob-static-website-custom-domain/subdomain-cname-record.png)

1. Klicken Sie im Azure-Portal auf den neu erstellten Endpunkt, um die benutzerdefinierte Domäne und das SSL-Zertifikat zu konfigurieren.
1. Wählen Sie **Benutzerdefinierte Domäne hinzufügen** aus, geben Sie Ihren Domänennamen ein, und klicken Sie dann auf **Hinzufügen**.
1. Wählen Sie die neu erstellte benutzerdefinierte Domänenzuordnung aus, um ein SSL-Zertifikat bereitzustellen.
1. Legen Sie **HTTPS für benutzerdefinierte Domänen** auf **EIN** fest. Wählen Sie **CDN verwaltet** aus, um Ihr SSL-Zertifikat von Azure CDN verwalten zu lassen. Klicken Sie auf **Speichern**.
1. Testen Sie Ihre Website, indem Sie auf die URL der Website zugreifen.

## <a name="next-steps"></a>Nächste Schritte

Im zweiten Teil dieses Tutorials haben Sie erfahren, wie Sie eine benutzerdefinierte Domäne mit SSL in Azure CDN für Ihre statische Website konfigurieren.

Unter dem folgenden Link erfahren Sie mehr über das Hosten statischer Websites in Azure Storage.

> [!div class="nextstepaction"]
> [Weitere Informationen zu statischen Websites](storage-blob-static-website.md)
