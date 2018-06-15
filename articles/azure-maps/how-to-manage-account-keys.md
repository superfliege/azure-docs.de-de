---
title: Verwalten Ihres Azure Maps-Kontos und der dazugehörigen Schlüssel | Microsoft-Dokumentation
description: Sie können das Azure-Portal verwenden, um Ihr Azure Maps-Konto und Ihre Zugriffsschlüssel zu verwalten.
author: kgremban
ms.author: kgremban
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 153fb87b0e2b576fd67a24bb833165f2a1c93c56
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34599659"
---
# <a name="how-to-manage-your-azure-maps-account-and-keys"></a>Verwalten Ihres Azure Maps-Kontos und der dazugehörigen Schlüssel

Sie können Ihr Azure Maps-Konto und die dazugehörigen Schlüssel über das Azure-Portal verwalten. Wenn Sie über ein Konto und einen Schlüssel verfügen, können Sie die APIs auf Ihrer Website oder in Ihrer mobilen Anwendung implementieren.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-a-new-account"></a>Erstellen eines neuen Kontos

1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com)an.

1. Klicken Sie im Azure-Portal links oben auf **Ressource erstellen**.

2. Suchen Sie nach **Maps**, wählen Sie den Dienst aus, und klicken Sie dann auf **Erstellen**.

3. Geben Sie die Informationen für Ihr neues Konto ein. 

![Eingeben von Kontoinformationen im Portal](./media/how-to-manage-account-keys/new-account-portal.png)

## <a name="manage-keys-on-the-account-page"></a>Verwalten von Schlüsseln auf der Kontoseite

Nach der Kontoerstellung erhalten Sie zwei zufällig generierte Schlüssel. Sie verwenden die Schlüssel für die Authentifizierung für die Maps-APIs, wenn Sie Kartendaten abrufen oder eine neue JavaScript-Karteninstanz erstellen möchten. 

Sie finden Ihre Schlüssel im Azure-Portal. Navigieren Sie zu Ihrem Konto, und klicken Sie dann im Menü auf **Schlüssel**.

![Verwalten von Kontoschlüsseln im Portal](./media/how-to-manage-account-keys/account-keys-portal.png)

Auf dieser Seite können Sie Ihre Schlüssel kopieren oder neue Schlüssel generieren. 

## <a name="delete-an-account"></a>Löschen eines Kontos

Sie können ein Konto im Azure-Portal löschen. Navigieren Sie zur Seite mit der Kontoübersicht, und klicken Sie auf **Löschen**.

![Löschen des Kontos im Portal](./media/how-to-manage-account-keys/account-delete-portal.png)

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie, wie Sie mit der [Maps-Verwaltungs-API](https://docs.microsoft.com/rest/api/maps-management/accounts) Maps-Konten erstellen, aktualisieren und löschen. 