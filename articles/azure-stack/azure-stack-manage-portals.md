---
title: Verwenden des Administratorportals in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie als Azure Stack-Betreiber, wie Sie das Administratorportal verwenden.
services: azure-stack
documentationcenter: 
author: twooley
manager: byronr
editor: 
ms.assetid: 02c7ff03-874e-4951-b591-28166b7a7a79
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: twooley
ms.openlocfilehash: 3a1be7a08fab8ad0253f26e6a0683617bff4b7c9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="using-the-administrator-portal-in-azure-stack"></a>Verwenden des Administratorportals in Azure Stack

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

In Azure Stack stehen zwei Portale zur Verfügung: das Administratorportal und das Benutzerportal (manchmal auch als *Mandantenportal* bezeichnet). Als Azure Stack-Betreiber können Sie mit dem Administratorportal tägliche Verwaltungsaufgaben und Vorgänge in Azure Stack ausführen. 

## <a name="access-the-administrator-portal"></a>Zugreifen auf das Administratorportal

Bei einer Development Kit-Umgebung müssen Sie zunächst sicherstellen, dass Sie über Remotedesktopverbindung oder VPN [eine Verbindung mit dem Development Kit-Host](azure-stack-connect-azure-stack.md) herstellen können.

Rufen Sie zum Zugreifen auf das Administratorportal die Portal-URL auf, und melden Sie sich mit den Anmeldeinformationen eines Azure Stack-Betreibers an. Bei einem integrierten System variiert die Portal-URL basierend auf dem Regionsnamen und dem externen vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) der Azure Stack-Bereitstellung.

| Environment | URL des Administratorportals |   
| -- | -- | 
| Development Kit| https://adminportal.local.azurestack.external  |
| Integrierte Systeme | https://adminportal.&lt;*Region*&gt;.&lt;*FQDN*&gt; | 
| | |

 ![Das Administratorportal](media/azure-stack-manage-portals/image1.png)

Im Administratorportal können Sie etwa folgende Aktionen ausführen:

* Verwalten der Infrastruktur (Systemintegrität, Updates, Kapazität usw.)
* Auffüllen des Marketplace
* Erstellen von Plänen und Angeboten
* Erstellen von Abonnements für Benutzer

Auf der Kachel **Quickstart tutorial** (Schnellstarttutorial) stehen Links zur Onlinedokumentation für die häufigsten Aufgaben zur Verfügung.
 
Ein Betreiber kann zwar im Administratorportal Ressourcen wie virtuelle Computer, virtuelle Netzwerke und Speicherkonten erstellen, Sie sollten sich jedoch [beim Benutzerportal anmelden](user/azure-stack-use-portal.md), um Ressourcen zu erstellen und zu testen. (Der Link **Virtuellen Computer erstellen** auf der Kachel für das Schnellstarttutorial ermöglicht die Erstellung eines virtuellen Computers im Administratorportal, diese Methode dient jedoch nur zum Überprüfen von Azure Stack nach der ersten Bereitstellung.)

## <a name="subscription-behavior"></a>Abonnementverhalten
 
Im Administratorportal ist nur ein einzelnes Abonnement verfügbar. Bei diesem Abonnement handelt es sich um das *Standardabonnement des Anbieters*. Sie können keine weiteren Abonnements für die Verwendung im Administratorportal hinzufügen.

Als Azure Stack-Betreiber können Sie über das Administratorportal Abonnements für Ihre Benutzer (einschließlich Sie selbst) hinzufügen. Benutzer (einschließlich Sie selbst) können auf diese Abonnements über das Benutzerportal zugreifen und die Abonnements verwenden. Über das Benutzerportal kann nicht auf die administrativen oder operativen Funktionen des Administratorportals zugegriffen werden.

Das Administratorportal und das Benutzerportal basieren auf separaten Instanzen von Azure Resource Manager. Aufgrund der Trennung von Resource Manager sind Abonnements nicht portalübergreifend. Ein Beispiel: Wenn Sie sich als Azure Stack-Betreiber beim Benutzerportal anmelden, können Sie nicht auf das Standardabonnement des Anbieters zugreifen. Somit haben Sie auch keinen Zugriff auf administrative Funktionen. Sie können zwar auf der Grundlage öffentlicher Angebote Abonnements für sich selbst erstellen, werden aber als Mandantenbenutzer betrachtet.

  >[!NOTE]
  In der Development Kit-Umgebung gilt: Wenn ein Benutzer dem gleichen Mandantenverzeichnis angehört wie der Azure Stack-Betreiber, kann er sich beim Administratorportal anmelden. Er hat jedoch keinen Zugriff auf die administrativen Funktionen. Außerdem kann er über das Administratorportal keine Abonnements hinzufügen oder auf Angebote zugreifen, die für ihn im Benutzerportal verfügbar gemacht werden.

## <a name="administrator-portal-tips"></a>Tipps zum Administratorportal

### <a name="customize-the-dashboard"></a>Anpassen des Dashboards

Das Dashboard enthält eine Reihe von Standardkacheln. Klicken Sie auf **Dashboard bearbeiten**, um das Standarddashboard anzupassen, oder auf **Neues Dashboard**, um benutzerdefinierte Dashboards hinzuzufügen. Sie können dem Dashboard einfach Kacheln hinzufügen. Klicken Sie beispielsweise auf **Neu**, klicken Sie mit der rechten Maustaste auf **Offers + Plans** (Angebote und Pläne), und klicken Sie dann auf **An Dashboard anheften**.

### <a name="quick-access-to-online-documentation"></a>Schneller Zugriff auf Onlinedokumentation

Klicken Sie oben rechts im Administratorportal auf das Hilfe- und Supportsymbol (Fragezeichen) und dann auf **Hilfe und Support**, um auf die Dokumentation für Azure Stack-Betreiber zuzugreifen.

### <a name="quick-access-to-help-and-support"></a>Schneller Zugriff auf Hilfe und Support

Wenn Sie oben rechts im Administratorportal auf das Hilfe- und Supportsymbol (Fragezeichen) und dann auf **Neue Supportanfrage** klicken, geschieht Folgendes:

- Bei Verwendung eines integrierten Systems wird mit dieser Aktion eine Website geöffnet, auf der Sie direkt ein Supportticket beim Microsoft-Kundensupport (Microsoft Customer Support Services, CSS) öffnen können. Informationen dazu, wann Sie den Microsoft-Support und wann den Support des OEM-Hardwareanbieters kontaktieren sollten, finden Sie unter [Azure Stack administration basics](azure-stack-manage-basics.md) (Grundlagen zur Verwaltung von Azure Stack) im Abschnitt „Where to get support“ (Supportquellen).
- Bei Verwendung des Development Kits wird mit dieser Aktion direkt die Website mit den Azure Stack-Foren geöffnet. Diese Foren werden regelmäßig überprüft. Da das Development Kit eine Evaluierungsumgebung ist, wird über den Microsoft CSS kein offizieller Support angeboten.

## <a name="next-steps"></a>Nächste Schritte

- [Regionsverwaltung in Azure Stack](azure-stack-region-management.md)
