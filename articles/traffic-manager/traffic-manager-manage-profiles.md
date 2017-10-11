---
title: Verwalten von Azure Traffic Manager-Profilen | Microsoft Docs
description: "In diesem Artikel können Sie die zu erstellen, deaktivieren, aktivieren und Löschen eines Azure Traffic Manager-Profils."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: f06e0365-0a20-4d08-b7e1-e56025e64f66
ms.service: traffic-manager
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/10/2017
ms.author: kumud
ms.openlocfilehash: a5164282264124835692bc72a4ab61891aa7af9d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-an-azure-traffic-manager-profile"></a>Verwalten einer Azure Traffic Manager-Profil

Traffic Manager-Profile verwenden datenverkehrrouting Methoden, um die Verteilung des Datenverkehrs für Ihre Clouddienste oder die Website-Endpunkte steuern. Dieser Artikel beschreibt das Erstellen und verwalten Sie diese Profile.

## <a name="create-a-traffic-manager-profile"></a>Erstellen Sie ein Traffic Manager-Profil

Sie können eine Traffic Manager-Profil erstellen, mit der Azure-Portal. Nach der Erstellung Ihres Profils können Sie Endpunkte, Überwachung und andere Einstellungen im Azure-Portal konfigurieren. Traffic Manager unterstützt bis zu 200 Endpunkte pro Profil. Die meisten Szenarien für die Verwendung erfordern jedoch nur einige der Endpunkte.

### <a name="to-create-a-traffic-manager-profile"></a>Zum Erstellen eines Traffic Manager-Profils

1. Über einen Browser und melden Sie sich bei der [Azure-Portal](http://portal.azure.com). Wenn Sie ein Konto noch nicht haben, registrieren Sie sich für eine [kostenlose einmonatige Testversion](https://azure.microsoft.com/free/). 
2. Auf der **Hub** im Menü klicken Sie auf **neu** > **Networking** > **alle**, klicken Sie auf **Traffic Manager** Profil öffnen die **erstellen Traffic Manager-Profil** Blatt, klicken Sie dann auf **erstellen**.
3. Auf der **erstellen Traffic Manager-Profil** Blatt vollständige wie folgt:
    1. In **Namen**, geben Sie einen Namen für Ihr Profil. Dieser Name muss innerhalb der Zone trafficmanager.net eindeutig sein und führt zu den DNS-Namen <name>, trafficmanager.net, die zum Zugriff auf Ihre Traffic Manager-Profil verwendet wird.
    2. In **Routingmethode**, wählen die **Priorität** Routingmethode.
    3. In **Abonnement**, wählen Sie das Abonnement dieses Profil unter erstellt werden sollen.
    4. In **Ressourcengruppe**, erstellen Sie eine neue Ressourcengruppe, um dieses Profil zu platzieren.
    5. In **Speicherort der Ressourcengruppe**, wählen Sie den Speicherort der Ressourcengruppe. Diese Einstellung bezieht sich auf den Speicherort der Ressourcengruppe und hat keine Auswirkungen auf die Traffic Manager-Profil, das Global bereitgestellt werden.
    6. Klicken Sie auf **Erstellen**.
    7. Wenn die globale Bereitstellung Ihres Traffic Manager-Profils abgeschlossen ist, wird sie als eine der Ressourcen in der jeweiligen Ressourcengruppe aufgeführt.

## <a name="disable-enable-or-delete-a-profile"></a>Deaktivieren, aktivieren oder Löschen eines Profils

Sie können ein vorhandenes Profil deaktivieren, sodass Traffic Manager benutzeranforderungen nicht an die konfigurierten Endpunkte verweist. Wenn Sie ein Traffic Manager-Profil deaktivieren, werden das Profil und die im Profil enthaltenen Informationen bleiben intakt und können in der Oberfläche von Traffic Manager bearbeitet werden.  Verweise auf fort, wenn Sie das Profil erneut aktivieren. Wenn Sie ein Traffic Manager-Profil im Azure-Portal erstellen, wird es automatisch aktiviert. Wenn Sie sich, dass ein Profil nicht mehr erforderlich ist entscheiden, können Sie es löschen.

### <a name="to-disable-a-profile"></a>So deaktivieren Sie ein Profil

1. Wenn Sie einen benutzerdefinierten Domänennamen verwenden, ändern Sie den CNAME-Eintrag auf dem Internet-DNS-Server, damit er nicht mehr auf Ihre Traffic Manager-Profils verweist.
2. Datenverkehr wird beendet, die an die Endpunkte über die Traffic Manager-profileinstellungen weitergeleitet wird.
3. Über einen Browser und melden Sie sich bei der [Azure-Portal](http://portal.azure.com).
2. Suchen Sie in das Portal Suchleiste den **Traffic Manager-Profil** Namen, den Sie ändern möchten, und klicken Sie dann auf die Traffic Manager-Profil in den Ergebnissen, angezeigt.
3. In der **Traffic Manager-Profil** Blatt, klicken Sie auf **Übersicht**, in der Übersicht über die Blatt auf **deaktivieren**, und bestätigen Sie zum Deaktivieren des Traffic Manager-Profils.

### <a name="to-enable-a-profile"></a>So aktivieren Sie ein Profil

1. Über einen Browser und melden Sie sich bei der [Azure-Portal](http://portal.azure.com).
2. Suchen Sie in das Portal Suchleiste den **Traffic Manager-Profil** Namen, den Sie ändern möchten, und klicken Sie dann auf die Traffic Manager-Profil in den Ergebnissen, angezeigt.
3. In der **Traffic Manager-Profil** Blatt, klicken Sie auf **Übersicht über**, und klicken Sie dann in der Übersicht über die Blatt auf **aktivieren**.
5. Wenn Sie einen benutzerdefinierten Domänennamen verwenden, erstellen Sie einen CNAME-Eintrag für Ihre DNS-Internetserver so, dass auf den Domänennamen Ihres Traffic Manager-Profils verweist.
6. Datenverkehr wird erneut an die Endpunkte weitergeleitet.

### <a name="to-delete-a-profile"></a>Löschen eines Profils

1. Stellen Sie sicher, dass der DNS-Ressourceneintrag auf Ihrem DNS-Internetserver nicht mehr einen CNAME-Ressourceneintrag verwendet, der auf den Domänennamen Ihres Traffic Manager-Profils verweist.
2. Suchen Sie in das Portal Suchleiste den **Traffic Manager-Profil** Namen, den Sie ändern möchten, und klicken Sie dann auf die Traffic Manager-Profil in den Ergebnissen, angezeigt.
3. In der **Traffic Manager-Profil** Blatt, klicken Sie auf **Übersicht**, in der Übersicht über die Blatt auf **löschen**, und bestätigen Sie zum Löschen des Traffic Manager-Profil.

## <a name="next-steps"></a>Nächste Schritte

* [Fügen Sie einen Endpunkt](traffic-manager-endpoints.md)
* [Konfigurieren der Priorität datenverkehrrouting-Methode](traffic-manager-configure-priority-routing-method.md)
* [Konfigurieren von geografischen datenverkehrrouting-Methode](traffic-manager-configure-geographic-routing-method.md) 
* [Konfigurieren von gewichteten datenverkehrrouting-Methode](traffic-manager-configure-weighted-routing-method.md)
* [Konfigurieren Sie routing Leistung-Methode](traffic-manager-configure-performance-routing-method.md)
