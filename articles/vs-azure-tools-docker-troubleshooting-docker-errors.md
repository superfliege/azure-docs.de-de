---
title: Problembehandlung bei Docker-Clientfehlern unter Windows mit Visual Studio | Microsoft-Dokumentation
description: "Beheben Sie Probleme, die bei der Verwendung von Visual Studio 2017 unter Windows zum Erstellen und Bereitstellen von Web-Apps in Docker auftreten können."
services: azure-container-service
documentationcenter: na
author: devinb
manager: douge
editor: 
ms.assetid: 346f70b9-7b52-4688-a8e8-8f53869618d3
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 10/13/2017
ms.author: devinb
ms.openlocfilehash: 90dd5df4a607568e2f3a60791da2948af7ce4e50
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2017
---
# <a name="troubleshoot-visual-studio-2017-development-with-docker"></a>Problembehandlung bei der Entwicklung mit Docker in Visual Studio 2017

Wenn Sie mit Visual Studio-Tools für Docker arbeiten, können Probleme beim Erstellen oder Debuggen der Anwendung auftreten. Im Folgenden sind einige allgemeine Schritte zur Problembehandlung aufgelistet.

## <a name="volume-sharing-is-not-enabled-enable-volume-sharing-in-the-docker-ce-for-windows-settings--linux-containers-only"></a>Die Freigabe von Volumes ist nicht aktiviert. Aktivieren Sie die Freigabe von Volumes in den Docker CE für Windows-Einstellungen (gilt nur für Linux-Container)

So lösen Sie dieses Problem:

1. Klicken Sie im Benachrichtigungsbereich mit der rechten Maustaste auf **Docker für Windows**, und wählen Sie dann **Einstellungen**.
1. Wählen Sie **Freigegebene Laufwerke**, und geben Sie das Laufwerk an, auf dem sich das Projekt befindet.

> [!NOTE]
> Wenn Dateien als freigegeben angezeigt werden, müssen Sie möglicherweise noch auf den Link „Anmeldeinformationen zurücksetzen...“ im unteren Bereich des Dialogfelds klicken, um die Freigabe des Volumes erneut zu aktivieren.

![Freigegebene Laufwerke](./media/vs-azure-tools-docker-troubleshooting-docker-errors/shareddrives.png)

## <a name="unable-to-start-debugging"></a>Starten des Debuggens nicht möglich

Ein Grund könnte das Vorhandensein von veralteten Debugging-Komponenten in Ihrem Benutzerprofilordner sein. Führen Sie die folgenden Befehle aus, um diese Ordner zu entfernen, sodass die neuesten Debugging-Komponenten von der nächsten Debugsitzung heruntergeladen werden.

- del %userprofile%\vsdbg
- del %userprofile%\onecoremsvsmon

## <a name="errors-specific-to-networking-when-debugging-your-application"></a>Netzwerkspezifische Fehler beim Debuggen Ihrer Anwendung

Versuchen Sie das Skript auszuführen, das unter[Cleanup Container Host Networking](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/master/windows-server-container-tools/CleanupContainerHostNetworking) heruntergeladen werden kann und das die netzwerkbezogenen Komponenten auf Ihrem Hostcomputer aktualisiert.


## <a name="microsoftdockertools-github-repo"></a>Microsoft/DockerTools GitHub-Repository

Für alle anderen Probleme, die auftreten, wechseln Sie zu [Microsoft/DockerTools](https://github.com/microsoft/dockertools/issues)-Probleme.