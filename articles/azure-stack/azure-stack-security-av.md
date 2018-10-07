---
title: Aktualisieren von Windows Defender Antivirus in Azure Stack
description: Enthält Details dazu, wie das Antivirenprogramm in Azure Stack auf dem neuesten Stand gehalten wird.
services: azure-stack
author: PatAltimore
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 09/26/2018
ms.author: patricka
ms.reviewer: fiseraci
ms.openlocfilehash: 59c59705e840d3cdd0d3c5310d84d711cd00c96a
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47394011"
---
# <a name="update-windows-defender-antivirus-on-azure-stack"></a>Aktualisieren von Windows Defender Antivirus in Azure Stack

[Windows Defender Antivirus](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10) ist eine Antischadsoftwarelösung, die Sicherheit und Virenschutz bereitstellt. Jede Azure Stack-Infrastrukturkomponente (Hyper-V-Hosts und virtuelle Computer) wird mit Windows Defender Antivirus geschützt. Für einen aktuellen Schutz benötigen Windows Defender Antivirus-Definitionen, die -Engine und die -Plattform regelmäßige Updates. Wie Updates angewendet werden, hängt von Ihrer Konfiguration ab.

## <a name="connected-scenario"></a>Verbundenes Szenario

Für Updates der Antischadsoftwaredefinitionen und der -engine lädt der Azure Stack-[Updateressourcenanbieter](azure-stack-updates.md#the-update-resource-provider) mehrmals täglich Antischadsoftwaredefinitionen und Engineupdates herunter. Jede Azure Stack-Infrastrukturkomponente erhält das Update vom Updateressourcenanbieter und wendet das Update automatisch an.

Für Updates der Antischadsoftwareplattform wenden Sie das [monatliche Azure Stack-Update](azure-stack-apply-updates.md) an. Die monatliche Azure Stack-Update umfasst Windows Defender Antivirus-Plattformupdates für den Monat.

## <a name="disconnected-scenario"></a>Nicht verbundenes Szenario

 Wenden Sie das [monatliche Azure Stack-Update](azure-stack-apply-updates.md) an. Die monatliche Azure Stack-Update umfasst Updates der Windows Defender Antivirus-Definitionen, der -Engine und der -Plattform für den Monat.

## <a name="next-steps"></a>Nächste Schritte

[Informieren Sie sich ausführlicher über die Sicherheit von Azure Stack.](azure-stack-security-foundations.md)
