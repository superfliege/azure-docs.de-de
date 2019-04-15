---
title: Herstellen einer Verbindung mit Windows Virtual Desktop (Vorschauversion) mit Windows 7 und Windows 10 – Azure
description: Es wird beschrieben, wie Sie mit Windows 7 oder Windows 10 eine Verbindung mit dem Windows Virtual Desktop-Dienst (Vorschauversion) herstellen.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 03/27/2019
ms.author: helohr
ms.openlocfilehash: 8b5a649d767dae9ae9c0fb077f550451a7f83601
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2019
ms.locfileid: "58630384"
---
# <a name="connect-with-windows-7-and-windows-10"></a>Herstellen einer Verbindung mit Windows 7 und Windows 10

> Gilt für: Windows 7 und Windows 10.

Es ist ein Client als Download verfügbar, der den Zugriff auf Windows Virtual Desktop-Ressourcen (Vorschauversion) über Geräte ermöglicht, auf denen Windows 7 und Windows 10 ausgeführt wird.

> [!IMPORTANT]
> Verwenden Sie nicht **RemoteApp- und Desktopverbindungen (RADC)** oder **Remotedesktopverbindung (MSTSC)** für den Zugriff auf Windows Virtual Desktop-Ressourcen, da Windows Virtual Desktop keinen von beiden Clients unterstützt.

## <a name="install-the-client"></a>Installieren des Clients

Führen Sie den [Download](https://go.microsoft.com/fwlink/?linkid=2068602) des Clients und dann die Installation auf Ihrem lokalen PC durch. Hierfür sind Administratorrechte erforderlich.

## <a name="subscribe-to-a-feed"></a>Abonnieren eines Feeds

Rufen Sie die Liste mit den verfügbaren Ressourcen über Ihren lokalen PC ab, indem Sie den von Ihrem Administrator bereitgestellten Feed abonnieren.

Abonnieren Sie einen Feed wie folgt:

1. Starten Sie den Client aus der Liste „Alle Apps“, suchen Sie nach **Remotedesktop**.
1. Wählen Sie auf der Hauptseite **Abonnieren** aus, um eine Verbindung mit dem Dienst herzustellen und Ihre Ressourcen abzurufen.
1. **Melden Sie sich**  mit Ihrem Benutzerkonto an, wenn Sie dazu aufgefordert werden.

Nach der erfolgreichen Authentifizierung sollte nun eine Liste mit den Ressourcen angezeigt werden, die Ihnen zur Verfügung stehen.

Sie können zwei Methoden nutzen, um Ressourcen zu starten.

- Doppelklicken Sie auf der Hauptseite des Clients auf eine Ressource, um sie zu starten.
- Starten Sie eine Ressource über das Startmenü, wie Sie dies sonst auch für andere Apps tun.
  - Sie können auch in der Suchleiste nach Apps suchen.

## <a name="update-the-client"></a>Aktualisieren des Clients

Wenn eine neue Version des Clients verfügbar ist, werden Sie vom Client und über das Info-Center von Windows benachrichtigt. Wählen Sie die Benachrichtigung aus, um den Updateprozess zu starten.
