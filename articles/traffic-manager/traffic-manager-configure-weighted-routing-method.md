---
title: Gewichteter Roundrobin-verkehrsroutingmethode mit Azure Traffic Manager konfigurieren | Microsoft Docs
description: "In diesem Artikel wird erläutert, wie Lastenausgleich des Datenverkehrs über eine Roundrobin-Methode in Traffic Manager"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 6dca6de1-18f7-4962-bd98-6055771fab22
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: kumud
ms.openlocfilehash: 7aa4c9120d44ff1b3e59a57090ea04e3f8021fc4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-the-weighted-traffic-routing-method-in-traffic-manager"></a>Konfigurieren Sie die gewichtete verkehrsroutingmethode in Traffic Manager

Ein allgemeines Datenverkehr routing-Methode-Muster ist stehen eine Reihe identischer Endpunkte, z. B. Cloud-Dienste und Websites, und Datenverkehr im Roundrobin-Verfahren an die einzelnen gesendet. Die folgenden Schritte beschreiben, wie diese Art von verkehrsroutingmethode konfigurieren.

> [!NOTE]
> Azure-Websites bieten bereits Roundrobin-Lastenausgleichsfunktionen für Websites in einem Rechenzentrum (auch bekannt als "Region"). Traffic Manager ermöglicht Ihnen die Angabe von Roundrobin-Routingmethode für Websites in verschiedenen Datencentern.

## <a name="to-configure-the-weighted-traffic-routing-method"></a>So konfigurieren Sie die gewichtete datenverkehrrouting-Methode

1. Über einen Browser und melden Sie sich bei der [Azure-Portal](http://portal.azure.com). Wenn Sie ein Konto noch nicht haben, registrieren Sie sich für eine [kostenlose einmonatige Testversion](https://azure.microsoft.com/free/). 
2. Suchen Sie in das Portal Suchleiste den **Traffic Manager-Profile** , und klicken Sie dann auf den Profilnamen, die die Routingmethode für konfiguriert werden soll.
3. In der **Traffic Manager-Profil** Blatt, stellen Sie sicher, dass die Cloud-Dienste und Websites, die Sie in Ihre Konfiguration einschließen möchten, vorhanden sind.
4. In der **Einstellungen** auf **Konfiguration**, und klicken Sie in der **Konfiguration** Blatt vollständige wie folgt:
    1. Für **Datenverkehr Routingeinstellungen Methode**, stellen Sie sicher, dass die datenverkehrrouting-Methode ist **Weighted**. Wenn sie nicht der Fall ist, klicken Sie auf **Weighted** aus der Dropdownliste aus.
    2. Legen Sie die **Monitor endpunkteinstellungen** identisch ist, damit alle jeden Endpunkt in diesem Profil wie folgt:
        1. Wählen Sie das entsprechende **Protokoll**, und geben Sie die **Port** Anzahl. 
        2. Für **Pfad** Geben Sie einen Schrägstrich  */* . Um Endpunkte zu überwachen, müssen Sie einen Pfad und Dateinamen angeben. Ein Schrägstrich "/" ein gültiger Eintrag für den relativen Pfad und bedeutet, dass die Datei im Stammverzeichnis (Standardwert) ist.
        3. Klicken Sie am oberen Rand der Seite, auf **speichern**.
5. Testen Sie die Änderungen in Ihrer Konfiguration wie folgt:
    1.  Klicken Sie in das Portal Suchleiste den Traffic Manager-Profilnamen suchen, und klicken Sie auf die Traffic Manager-Profil in den Ergebnissen, angezeigt.
    2.  In der **Traffic Manager** Blatt ein Profil erstellen, klicken Sie auf **Übersicht**.
    3.  Die **Traffic Manager-Profil** Blatt zeigt der DNS-Name des neu erstellten Traffic Manager-Profil an. Dies kann von Clients (z. B. durch Navigieren Sie mithilfe eines Webbrowsers) verwendet werden, an dem richtigen Endpunkt weitergeleitet werden, die durch den routing Typ bestimmt. In diesem Fall werden alle Anforderungen weitergeleitet jeder Endpunkt im Round-Robin.
6. Sobald Traffic Manager-Profil arbeitet, bearbeiten Sie den DNS-Eintrag auf dem autoritativen DNS-Server so, dass Ihre Unternehmensdomäne dem Traffic Manager-Domänennamen verweist.

![Konfigurieren von gewichteten verkehrsroutingmethode mit Traffic Manager][1]

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Priorität Datenverkehr Routingmethode](traffic-manager-configure-priority-routing-method.md).
- Erfahren Sie mehr über [Leistung Datenverkehr Routingmethode](traffic-manager-configure-performance-routing-method.md).
- Erfahren Sie mehr über [geografischen Routingmethode](traffic-manager-configure-geographic-routing-method.md).
- Erfahren Sie, wie [Traffic Manager-testeinstellungen](traffic-manager-testing-settings.md).

<!--Image references-->
[1]: ./media/traffic-manager-weighted-routing-method/traffic-manager-weighted-routing-method.png
