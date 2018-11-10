---
title: Verwenden des Administratorportals in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie als Azure Stack-Betreiber, wie Sie das Administratorportal verwenden.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 02c7ff03-874e-4951-b591-28166b7a7a79
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/23/2018
ms.author: mabrigg
ms.openlocfilehash: 4453516626c39f73deea8fce0d744fcecbd5c3fc
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50085813"
---
# <a name="using-the-administrator-portal-in-azure-stack"></a>Verwenden des Administratorportals in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

In Azure Stack stehen zwei Portale zur Verfügung: das Administratorportal und das Benutzerportal (manchmal auch als *Mandantenportal* bezeichnet). Als Azure Stack-Betreiber können Sie mit dem Administratorportal tägliche Verwaltungsaufgaben und Vorgänge in Azure Stack ausführen.

## <a name="access-the-administrator-portal"></a>Zugreifen auf das Administratorportal

Bei einer Development Kit-Umgebung müssen Sie zunächst sicherstellen, dass Sie über Remotedesktopverbindung oder VPN [eine Verbindung mit dem Development Kit-Host](azure-stack-connect-azure-stack.md) herstellen können.

Rufen Sie zum Zugreifen auf das Administratorportal die Portal-URL auf, und melden Sie sich mit den Anmeldeinformationen eines Azure Stack-Betreibers an. Bei einem integrierten System variiert die Portal-URL basierend auf dem Regionsnamen und dem externen vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) der Azure Stack-Bereitstellung.

| Environment | URL des Administratorportals |   
| -- | -- | 
| Development Kit| https://adminportal.local.azurestack.external  |
| Integrierte Systeme | https://adminportal.&lt;*Region*&gt;.&lt;*FQDN*&gt; | 
| | |

 ![Das Administratorportal](media/azure-stack-manage-portals/admin-portal.png)

Beachten Sie, dass die Standardzeitzone für alle Azure Stack-Bereitstellungen auf „Koordinierte Weltzeit (UTC)“ festgelegt ist. Sie können bei der Installation von Azure Stack eine Zeitzone auswählen, diese wird während der Installation jedoch automatisch auf die Standardeinstellung „UTC“ zurückgesetzt.

Im Administratorportal können Sie etwa folgende Aktionen ausführen:

* Verwalten der Infrastruktur (Systemintegrität, Updates, Kapazität usw.)
* Auffüllen des Marketplace
* Erstellen von Abonnements für Benutzer
* Erstellen von Plänen und Angeboten

Auf der Kachel **Schnellstart-Lernprogramme** stehen Links zur Onlinedokumentation für die häufigsten Aufgaben zur Verfügung.

Ein Betreiber kann zwar im Administratorportal Ressourcen wie virtuelle Computer, virtuelle Netzwerke und Speicherkonten erstellen, Sie sollten sich jedoch [beim Benutzerportal anmelden](user/azure-stack-use-portal.md), um Ressourcen zu erstellen und zu testen.

>[!NOTE]
>Der Link **Virtuellen Computer erstellen** auf der Kachel „Schnellstart-Lernprogramme“ ermöglicht die Erstellung eines virtuellen Computers im Administratorportal, dies dient jedoch nur zum Überprüfen von Azure Stack nach der ersten Bereitstellung.

## <a name="understand-subscription-behavior"></a>Grundlegendes zum Abonnementverhalten

Im Administratorportal ist nur ein einzelnes Abonnement verfügbar. Bei diesem Abonnement handelt es sich um das *Standardabonnement des Anbieters*. Sie können im Administratorportal keine weiteren Abonnements hinzufügen und verwenden.

Als Azure Stack-Betreiber können Sie über das Administratorportal Abonnements für Ihre Benutzer (einschließlich Sie selbst) hinzufügen. Benutzer (einschließlich Sie selbst) können auf diese Abonnements über das **Benutzerportal** zugreifen und die Abonnements verwenden. Über das Benutzerportal kann jedoch nicht auf die administrativen oder operativen Funktionen des Administratorportals zugegriffen werden.

Das Administratorportal und das Benutzerportal basieren auf separaten Instanzen von Azure Resource Manager. Aufgrund dieser Trennung von Resource Manager sind Abonnements nicht portalübergreifend. Wenn Sie sich beispielsweise als Azure Stack-Betreiber beim Benutzerportal anmelden, können Sie nicht auf das *Standardabonnement des Anbieters* zugreifen. Obwohl Sie keinen Zugriff auf administrative Funktionen haben, können Sie jedoch auf der Grundlage verfügbarer öffentlicher Angebote Abonnements für sich selbst erstellen. Solange Sie beim Benutzerportal angemeldet sind, gelten Sie als Mandantenbenutzer.

  >[!NOTE]
  >In der Development Kit-Umgebung gilt: Wenn ein Benutzer dem gleichen Mandantenverzeichnis angehört wie der Azure Stack-Betreiber, kann er sich beim Administratorportal anmelden. Er hat jedoch keinen Zugriff auf die administrativen Funktionen. Außerdem kann er über das Administratorportal keine Abonnements hinzufügen und auf keine Angebote zugreifen, die für ihn im Benutzerportal zur Verfügung stehen.

## <a name="administrator-portal-tips"></a>Tipps zum Administratorportal

### <a name="customize-the-dashboard"></a>Anpassen des Dashboards

Das Dashboard enthält eine Reihe von Standardkacheln. Wählen Sie **Dashboard bearbeiten** aus, um das Standarddashboard anzupassen, oder wählen Sie **Neues Dashboard** aus, um ein benutzerdefiniertes Dashboard hinzuzufügen. Sie können einem Dashboard einfach Kacheln hinzufügen. Wählen Sie beispielsweise **+Ressource erstellen** aus, klicken Sie mit der rechten Maustaste auf **Angebote und Pläne**, und wählen Sie dann **An Dashboard anheften** aus.

Es kann sein, dass im Portal manchmal ein leeres Dashboard angezeigt wird. Klicken Sie zum Wiederherstellen des Dashboards auf **Dashboard bearbeiten**, klicken Sie dann mit der rechten Maustaste, und wählen Sie **Auf Standardzustand zurücksetzen** aus.

### <a name="quick-access-to-online-documentation"></a>Schneller Zugriff auf Onlinedokumentation

Verwenden Sie das Hilfe- und Supportsymbol (Fragezeichen) oben rechts im Administratorportal, um auf die Dokumentation für Azure Stack-Betreiber zuzugreifen. Bewegen Sie den Cursor über das Symbol, und wählen Sie dann **Hilfe und Support** aus.

### <a name="quick-access-to-help-and-support"></a>Schneller Zugriff auf Hilfe und Support

Wenn Sie oben rechts im Administratorportal das Hilfe- und Supportsymbol (Fragezeichen) und dann **Neue Supportanfrage** auswählen, geschieht Folgendes:

- Bei Verwendung eines integrierten Systems wird mit dieser Aktion eine Website geöffnet, auf der Sie direkt ein Supportticket beim Microsoft-Kundensupport (Microsoft Customer Support Services, CSS) öffnen können. Informationen dazu, wann Sie den Microsoft-Support und wann den Support des OEM-Hardwareanbieters kontaktieren sollten, finden Sie unter [Supportquellen](azure-stack-manage-basics.md#where-to-get-support).
- Bei Verwendung des Development Kits wird mit dieser Aktion direkt die Website mit den Azure Stack-Foren geöffnet. Diese Foren werden regelmäßig überprüft. Da das Development Kit eine Evaluierungsumgebung ist, wird über den Microsoft CSS kein offizieller Support angeboten.

## <a name="next-steps"></a>Nächste Schritte

- [Region management in Azure Stack](azure-stack-region-management.md) (Regionsverwaltung in Azure Stack)
