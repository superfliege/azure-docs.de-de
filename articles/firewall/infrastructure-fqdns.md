---
title: Infrastruktur-FQDN für Azure Firewall
description: Erfahren Sie mehr über die Infrastruktur-FQDNs in Azure Firewall
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 9/24/2018
ms.author: victorh
ms.openlocfilehash: 1369a82829b2c80768d746ba92daf2482c1fd7f8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46994138"
---
# <a name="infrastructure-fqdns"></a>Infrastruktur-FQDNs

Azure Firewall enthält eine integrierte Regelsammlung für Infrastruktur-FQDNs, die standardmäßig zulässig sind. Diese FQDNs sind plattformspezifisch und können nicht für andere Zwecke verwendet werden. 

Die folgenden Dienste sind in der integrierten Regelsammlung enthalten:

- Computezugriff auf das Speicher-PIR (Platform Image Repository, PIR)
- Speicherzugriff auf den Status verwalteter Datenträger
- Azure-Diagnose und -Protokollierung (MDS)
- Azure Active Directory

## <a name="overriding"></a>Überschreiben 

Diese integrierte Infrastrukturregelsammlung kann außer Kraft gesetzt werden. Erstellen Sie hierzu eine Anwendungsregelsammlung vom Typ „Alle ablehnen“, die ganz zum Schluss verarbeitet wird. Sie wird immer vor der Infrastrukturregelsammlung verarbeitet. Alles, was sich nicht in der Infrastrukturregelsammlung befindet, wird standardmäßig abgelehnt.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie eine [Azure Firewall bereitstellen und konfigurieren](tutorial-firewall-deploy-portal.md).