---
title: Einrichten einer Benutzerprofilfreigabe für einen Windows Virtual Desktop-Hostpool (Vorschauversion) – Azure
description: Es wird beschrieben, wie Sie einen FSLogix-Profilcontainer für einen Windows Virtual Desktop-Hostpool (Vorschauversion) einrichten.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: c9c2ca2cc27c5fa757b8ff6846e0a6a8f7087875
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/25/2019
ms.locfileid: "58403713"
---
# <a name="set-up-a-user-profile-share-for-a-host-pool"></a>Einrichten einer Benutzerprofilfreigabe für einen Hostpool

Der Windows Virtual Desktop-Dienst (Vorschauversion) stellt FSLogix-Profilcontainer als empfohlene Lösung für Benutzerprofile bereit. Wir raten davon ab, die Lösung „Benutzerprofil-Datenträger“ (User Profile Disk, UPD) zu nutzen. Sie wird in den zukünftigen Versionen von Windows Virtual Desktop als veraltet eingestuft.

In diesem Abschnitt wird beschrieben, wie Sie eine FSLogix-Profilcontainerfreigabe für einen Hostpool einrichten.

## <a name="create-a-new-virtual-machine-that-will-act-as-a-file-share"></a>Erstellen eines neuen virtuellen Computers als Dateifreigabe

Achten Sie beim Erstellen des virtuellen Computers darauf, dass Sie ihn entweder in demselben virtuellen Netzwerk wie die virtuellen Computer des Hostpools oder in einem virtuellen Netzwerk anordnen, das über eine Verbindung mit den virtuellen Computern des Hostpools verfügt. Sie können einen virtuellen Computer auf unterschiedliche Arten erstellen:

- [Erstellen eines virtuellen Computers aus einem Azure-Katalogimage](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#create-virtual-machine)
- [Erstellen eines virtuellen Computers aus einem verwalteten Image](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-generalized-managed)
- [Erstellen eines virtuellen Computers aus einem nicht verwalteten Image](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

Nachdem Sie den virtuellen Computer erstellt haben, können Sie ihn in die Domäne einbinden, indem Sie die folgenden Schritte ausführen:

1. [Stellen Sie eine Verbindung mit dem virtuellen Computer her](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine), indem Sie die Anmeldeinformationen verwenden, die Sie beim Erstellen des virtuellen Computers angegeben haben.
2. Starten Sie auf dem virtuellen Computer die **Systemsteuerung**, und wählen Sie **System**.
3. Wählen Sie **Computername**, **Einstellungen ändern** und dann **Ändern...**.
4. Wählen Sie **Domäne**, und geben Sie dann die Active Directory-Domäne im virtuellen Netzwerk ein.
5. Authentifizieren Sie sich mit einem Domänenkonto, das über Berechtigungen für in die Domäne eingebundene Computer verfügt.

## <a name="prepare-the-virtual-machine-to-act-as-a-file-share-for-user-profiles"></a>Vorbereiten des virtuellen Computers für Verwendung als Dateifreigabe für Benutzerprofile

Hier ist eine allgemeine Anleitung zum Vorbereiten eines virtuellen Computers als Dateifreigabe für Benutzerprofile angegeben:

1. Binden Sie die virtuellen Computer des Sitzungshosts in eine [Active Directory-Sicherheitsgruppe](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-security-groups) ein. Diese Sicherheitsgruppe wird zum Authentifizieren der virtuellen Computer des Sitzungshosts gegenüber dem virtuellen Dateifreigabencomputer verwendet, den Sie gerade erstellt haben.
2. [Stellen Sie die Verbindung mit dem virtuellen Dateifreigabencomputer her](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine).
3. Erstellen Sie auf dem virtuellen Dateifreigabencomputer einen Ordner auf **Laufwerk C**, der als Profilfreigabe verwendet wird.
4. Klicken Sie mit der rechten Maustaste auf den neuen Ordner, und wählen Sie dann nacheinander die Optionen **Eigenschaften**, **Freigabe** und **Erweiterte Freigabe...**.
5. Wählen Sie **Diesen Ordner freigeben**, **Berechtigungen...** und dann **Hinzufügen...**.
6. Suchen Sie nach der Sicherheitsgruppe, der Sie die virtuellen Computer des Sitzungshosts hinzugefügt haben, und stellen Sie dann sicher, dass für die Gruppe **Vollzugriff** festgelegt ist.
7. Klicken Sie nach dem Hinzufügen der Sicherheitsgruppe mit der rechten Maustaste auf den Ordner, wählen Sie **Eigenschaften** und **Freigabe**, und kopieren Sie anschließend den **Netzwerkpfad** zur späteren Verwendung.

Bewährte Methoden zu Berechtigungen finden Sie in der folgenden [FSLogix-Dokumentation](https://support.fslogix.com/index.php/forum-main/faqs/84-best-practices#120).

## <a name="configure-the-fslogix-profile-container"></a>Konfigurieren des FSLogix-Profilcontainers

Führen Sie auf allen Computern, die für den Hostpool registriert sind, Folgendes durch, um die virtuellen Computer mit der FSLogix-Software zu konfigurieren:

1. [Stellen Sie eine Verbindung mit dem virtuellen Computer her](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine), indem Sie die Anmeldeinformationen verwenden, die Sie beim Erstellen des virtuellen Computers angegeben haben.
2. Starten Sie einen Internetbrowser, und verwenden Sie [diesen Link](https://go.microsoft.com/fwlink/?linkid=2084562), um den FSLogix-Agent herunterzuladen. Im Rahmen der öffentlichen Vorschauversion von Windows Virtual Desktop erhalten Sie einen Lizenzschlüssel zum Aktivieren der FSLogix-Software. Der Schlüssel ist die Datei „LicenseKey.txt“, die in der ZIP-Datei des FSLogix-Agents enthalten ist.
3. Installieren Sie den FSLogix-Agent.
4. Navigieren Sie zu **Programme** > **FSLogix** > **Apps**, um zu überprüfen, ob der Agent installiert wurde.
5. Führen Sie im Startmenü **RegEdit** als Administrator aus. Navigieren Sie zu **Computer\\HKEY_LOCAL_MACHINE\\software\\FSLogix\\Profiles**.
6. Erstellen Sie die folgenden Werte:

| NAME                | Type               | Daten/Wert                        |
|---------------------|--------------------|-----------------------------------|
| Aktiviert             | DWORD              | 1                                 |
| VHDLocations        | Mehrteiliger Zeichenfolgenwert | „Netzwerkpfad für Dateifreigabe“ |
| VolumeType          | Zeichenfolge             | VHDX                              |
| SizeInMBs           | DWORD              | „Ganze Zahl für Größe des Profils“     |
| IsDynamic           | DWORD              | 1                                 |
| LockedRetryCount    | DWORD              | 1                                 |
| LockedRetryInterval | DWORD              | 0                                 |
