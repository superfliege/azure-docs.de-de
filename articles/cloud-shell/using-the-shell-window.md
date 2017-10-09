---
title: Verwenden des Azure Cloud Shell-Fensters (Vorschau) | Microsoft-Dokumentation
description: "Übersicht über die Verwendung des Azure Cloud Shell-Fensters."
services: azure
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: juluk
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: fb242abfbea79bc8c242a7a89b3d775cf74a0617
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="using-the-azure-cloud-shell-window"></a>Verwenden des Azure Cloud Shell-Fensters

Dieses Dokument erläutert, wie das Cloud Shell-Fenster verwendet wird.

## <a name="swap-between-bash-and-powershell-environments"></a>Wechsel zwischen Bash- und PowerShell-Umgebungen
![](media/using-the-shell-window/env-selector.png)

Verwenden Sie die Umgebungsauswahl auf der Cloud Shell-Symbolleiste, um zwischen Bash- und PowerShell-Umgebungen zu wechseln.

## <a name="restart-cloud-shell"></a>Neustarten der Cloud Shell
![](media/using-the-shell-window/restart.png)
> [!WARNING]
> Durch Neustart der Cloud Shell wird der Computerstatus zurückgesetzt und alle Dateien, die nicht von Ihrer Dateifreigabe beibehalten werden, gehen verloren.

* Klicken Sie auf das Neustartsymbol auf der Cloud Shell-Symbolleiste, um den Computerzustand zurückzusetzen.

## <a name="minimize--maximize-cloud-shell-window"></a>Minimieren und Maximieren des Cloud Shell-Fensters
![](media/using-the-shell-window/minmax.png)
* Klicken Sie auf das Symbol „Minimieren“ oben rechts im Fenster, um es auszublenden. Klicken Sie erneut auf das Cloud Shell-Symbol, um es einzublenden.
* Klicken Sie auf das Symbol „Maximieren“, um die maximale Höhe des Fensters festzulegen. Klicken Sie auf „Wiederherstellen“, um das Fenster in vorheriger Größe wiederherstellen.

## <a name="concurrent-sessions"></a>Gleichzeitige Sitzungen
Cloud Shell ermöglicht mehrere gleichzeitige Sitzungen auf individuellen Browserregisterkarten, wobei jede Sitzung als separater Bash-Prozess vorhanden sein kann.
Achten Sie beim Beenden einer Sitzung darauf, jedes Sitzungsfenster zu schließen, da die Prozesse unabhängig voneinander ausgeführt werden, obwohl sie auf dem gleichen Computer ausgeführt werden.

## <a name="copy-and-paste"></a>Kopieren und Einfügen
[!include [copy-paste](../../includes/cloud-shell-copy-paste.md)]

## <a name="resize-cloud-shell-window"></a>Ändern der Größe des Cloud Shell-Fensters
* Klicken Sie auf den oberen Rand der Symbolleiste, und ziehen Sie ihn nach oben oder unten, um die Größe des Cloud Shell-Fensters zu ändern.

## <a name="scrolling-text-display"></a>Scrollen der Textanzeige
* Scrollen Sie mit Ihrer Maus oder Ihrem Touchpad, um Terminaltext zu verschieben.

## <a name="changing-the-text-size"></a>Ändern der Textgröße
![](media/using-the-shell-window/text-size.png)
* Klicken Sie oben links im Fenster auf das Einstellungssymbol, und bewegen Sie dann den Mauszeiger über die Option "Textgröße", und wählen Sie die gewünschte Textgröße aus.

## <a name="exit-command"></a>Befehl „Beenden“
Ausführen von `exit` beendet die aktive Sitzung. Dieses Verhalten tritt standardmäßig nach 10 Minuten ohne Interaktion auf.

## <a name="next-steps"></a>Nächste Schritte

[Bash in Cloud Shell – Schnellstart](quickstart.md)
[PowerShell in Cloud Shell – Schnellstart](quickstart-powershell.md)
