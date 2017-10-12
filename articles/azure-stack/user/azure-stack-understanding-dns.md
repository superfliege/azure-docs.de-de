---
title: Grundlegendes zu DNS in Azure Stack | Microsoft-Dokumentation
description: Grundlegendes zu DNS-Features und -Funktionen in Azure Stack
services: azure-stack
documentationcenter: 
author: ScottNapolitan
manager: darmour
editor: 
ms.assetid: 60f5ac85-be19-49ac-a7c1-f290d682b5de
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: scottnap
ms.openlocfilehash: 8c023eda179ace41a082bf4a4fadc281c14db7ba
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="introducing-idns-for-azure-stack"></a>Einführung in iDNS für Azure Stack

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

iDNS ist ein Feature in Azure Stack, das Ihnen das Auflösen externer DNS-Namen (z.B. „http://www.bing.com“) ermöglicht.
Zudem ermöglicht es Ihnen, interne virtuelle Netzwerknamen zu registrieren. Auf diese Weise können Sie virtuelle Computer im selben virtuellen Netzwerk nach dem Namen statt der IP-Adresse auflösen, ohne benutzerdefinierte DNS-Servereinträge anzugeben.

Dies war in Azure immer vorhanden, steht jedoch auch in Windows Server 2016 und Azure Stack zur Verfügung.

## <a name="what-does-idns-do"></a>Wozu dient iDNS?
Mit iDNS in Azure Stack erhalten Sie die folgenden Funktionen, ohne benutzerdefinierte DNS-Servereinträge angeben zu müssen.

* Gemeinsam genutzte DNS-Namensauflösungsdienste für Mandantenworkloads.
* Autorisierender DNS-Dienst für Namensauflösung und DNS-Registrierung innerhalb des virtuellen Netzwerks des Mandanten.
* Rekursiver DNS-Dienst für die Auflösung von Internetnamen aus den Mandanten-VMs. Mandanten müssen nicht mehr benutzerdefinierte DNS-Einträge zum Auflösen von Internetnamen (z. B. „www.bing.com“) angeben.

Sie können weiterhin Ihr eigenes DNS und benutzerdefinierte DNS-Server verwenden, wenn Sie möchten. Wenn Sie jedoch nur Internet-DNS-Namen auflösen und mit anderen virtuellen Computern im selben virtuellen Netzwerk Verbindungen herstellen möchten, müssen Sie jetzt nichts mehr angeben, aber es funktioniert.

## <a name="what-does-idns-not-do"></a>Wozu dient iDNS nicht?
iDNS ermöglicht Ihnen nicht, einen DNS-Eintrag für einen Namen zu erstellen, der von außerhalb des virtuellen Netzwerks aufgelöst werden kann.

In Azure haben Sie die Option, eine DNS-Namensbezeichnung anzugeben, die einer öffentlichen IP-Adresse zugeordnet werden kann. Sie können die Bezeichnung (das Präfix) auswählen, aber Azure wählt das Suffix, das auf der Region basiert, in der Sie die öffentliche IP-Adresse erstellen.

![Screenshot der DNS-Namensbezeichnung](media/azure-stack-understanding-dns-in-tp2/image3.png)

In der Abbildung oben erstellt Azure einen „A“-Eintrag im DNS für die DNS-Namensbezeichnung, angegeben unter der Zone **westus.cloudapp.azure.com**. Das Präfix und Suffix bilden zusammen einen vollständig qualifizierten Domänennamen (Fully Qualified Domain Name, FQDN), der von überall im öffentlichen Internet aufgelöst werden kann.

Azure Stack unterstützt iDNS nur für die Registrierung interner Namen, sodass das Folgende nicht möglich ist.

* Erstellen eines DNS-Eintrags unter einer vorhandenen gehosteten DNS-Zone (z.B. „local.azurestack.external“).
* Erstellen einer DNS-Zone (z.B. „contoso.com“).
* Erstellen eines Eintrags unter Ihrer eigenen benutzerdefinierten DNS-Zone.
* Unterstützen des Erwerbs von Domänennamen.

