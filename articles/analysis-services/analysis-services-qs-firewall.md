---
title: Schnellstart – Konfigurieren einer Firewall für einen Analysis Services-Server in Azure | Microsoft-Dokumentation
description: Informationen zum Konfigurieren einer Firewall für eine Analysis Services-Serverinstanz in Azure
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: quickstart
ms.date: 10/18/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 0351ab3213ac75559cc025f2ef71e2f098a7504c
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2018
ms.locfileid: "52993197"
---
# <a name="quickstart-configure-server-firewall---portal"></a>Schnellstart: Konfigurieren der Serverfirewall – Portal

In diesem Schnellstart wird beschrieben, wie Sie eine Firewall für Ihren Azure Analysis Services-Server konfigurieren. Das Aktivieren einer Firewall und das Konfigurieren von IP-Adressbereichen nur für jene Computer, die auf Ihren Server zugreifen, sind ein wichtiger Bestandteil der Sicherung Ihres Servers und Ihrer Daten.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Analysis Services-Server in Ihrem Abonnement. Weitere Informationen finden Sie unter [Schnellstart: Erstellen eines Analysis Services-Servers im Azure-Portal](analysis-services-create-server.md) oder unter [Schnellstart: Erstellen eines Servers mit PowerShell](analysis-services-create-powershell.md).
- Mindestens ein IP-Adressbereich für Clientcomputer (falls erforderlich).

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an. 

[Anmelden beim Portal](https://portal.azure.com)

## <a name="configure-a-firewall"></a>Konfigurieren einer Firewall

1. Klicken Sie auf Ihren Server, um die Seite „Übersicht“ zu öffnen. 
2. Klicken Sie unter **EINSTELLUNGEN** > **Firewall** > **Firewall aktivieren** auf **Ein**.
3. Klicken Sie in **Zugriff über Power BI zulassen** auf **Ein**, um DirectQuery-Zugriff über Power BI zuzulassen.  
4. (Optional) Geben Sie einen oder mehrere IP-Adressbereiche an. Geben Sie einen Namen sowie eine Start- und End-IP-Adresse für jeden Bereich ein. 
5. Klicken Sie auf **Speichern**.

     ![Firewalleinstellungen](./media/analysis-services-qs-firewall/aas-qs-firewall.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie IP-Adressbereiche, oder deaktivieren Sie die Firewall, wenn diese nicht mehr benötigt werden.

## <a name="next-steps"></a>Nächste Schritte
In diesem Schnellstart haben Sie erfahren, wie eine Firewall für Ihren Server konfiguriert wird. Nachdem Sie nun über einen Server verfügen und ihn mit einer Firewall gesichert haben, können Sie ihm ein einfaches Beispieldatenmodell aus dem Portal hinzufügen. Ein Beispielmodell ist hilfreich, um sich mit dem Konfigurieren von Modelldatenbankrollen und dem Testen von Clientverbindungen vertraut zu machen. Fahren Sie mit dem Tutorial zum Hinzufügen eines Beispielmodells fort, um mehr zu erfahren.

> [!div class="nextstepaction"]
> [Tutorial: Tutorial: Hinzufügen eines Beispielmodells über das Portal](analysis-services-create-sample-model.md)
