---
title: Angeben von DNS-Einstellungen in einer Dienstkonfigurationsdatei | Microsoft Docs
description: Angeben benutzerdefinierter DNS-Einstellungen über die Dienstkonfigurationsdatei für ein virtuelles Netzwerk
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: tysonn
ms.assetid: 467a4b99-8691-40b3-b640-e25e49675c71
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/24/2016
ms.author: genli
ms.openlocfilehash: 009206f1e0ba848538ed2c666032a63051d062e4
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2018
ms.locfileid: "31790745"
---
# <a name="specifying-dns-settings-in-a-service-configuration-file"></a>Angeben von DNS-Einstellungen in einer Dienstkonfigurationsdatei
## <a name="dns-elements"></a>DNS-Elemente
Eine Dienstkonfigurationsdatei kann ein DnsServers-Element mit einer Liste von IPv4-Adressen für die DNS-Server (Domain Name System) enthalten, die der Dienst verwendet. Einstellungen in der Dienstkonfigurationsdatei überschreiben die Einstellungen in der Netzwerkkonfigurationsdatei. Weitere Informationen finden Sie unter [Azure-Dienstkonfigurationsschema (.cscfg-Datei)](https://msdn.microsoft.com/library/azure/ee758710.aspx).

**NetworkConfiguration-Element**

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

> [!WARNING]
> Das **name**-Attribut im **DnsServer**-Element wird nur als Verweisname verwendet. Es stellt nicht den Hostnamen für den DNS-Server dar. Jeder **DnsServer** -Attributwert muss im gesamten Microsoft Azure-Abonnement eindeutig sein.
> 
> 

## <a name="see-also"></a>Siehe auch
[Azure-Dienstkonfigurationsschema (CSCFG)](https://msdn.microsoft.com/library/windowsazure/ee758710)

[Konfigurationsschema für virtuelle Azure-Netzwerke](http://go.microsoft.com/fwlink/?LinkId=248093)

[Konfigurieren eines virtuellen Netzwerks mithilfe einer Netzwerkkonfigurationsdatei](http://go.microsoft.com/fwlink/?LinkId=248094)

[Einstellungen für virtuelle Netzwerke im Verwaltungsportal](http://go.microsoft.com/fwlink/?LinkId=248092)

