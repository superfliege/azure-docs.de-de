---
title: Erstellen von Cloud App Discovery-Momentaufnahmeberichten in Azure Active Directory | Microsoft-Dokumentation
description: Bietet Informationen zum Suchen und Verwalten von Anwendungen mit Cloud App Discovery, zu den Vorteilen und zu der Funktionsweise.
services: active-directory
keywords: Cloud App Discovery, Verwalten von Anwendungen
documentationcenter: 
author: curtand
manager: femila
ms.assetid: db968bf5-22ae-489f-9c3e-14df6e1fef0a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: curtand
ms.reviewer: nigu
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: 7dceb7bb38b1dac778151e197db3b5be49dd568a
ms.openlocfilehash: 38e3f71fb4817de7aa95b99e662856f586e72530
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="create-cloud-app-discovery-snapshot-reports"></a>Erstellen von Cloud App Discovery-Momentaufnahmeberichten

Laden Sie vor dem Einrichten des automatischen Protokollsammlers ein Protokoll manuell hoch, und lassen Sie es von Cloud App Security analysieren. Wenn Sie noch kein Protokoll besitzen und gern ein Beispielprotokoll hätten, können Sie mit dem folgenden Verfahren eine Beispielprotokolldatei herunterladen, um zu erfahren, wie Ihre Protokolldatei aussehen sollte.

## <a name="to-create-a-snapshot-report"></a>So erstellen Sie einen Momentaufnahmebericht

1. Sammeln Sie Protokolldateien von Ihrer Firewall und Ihrem Proxyserver, über die Benutzer in Ihrer Organisation auf das Internet zugreifen. Sammeln Sie Protokolle zu Spitzenverkehrszeiten, die für die Aktivitäten der Benutzern in Ihrer Organisation repräsentativ sind.
2. Klicken Sie in der [Menüleiste von Cloud App Security](https://portal.cloudappsecurity.com) auf die Option **Discover** (Ermitteln) und dann auf **Momentaufnahmebericht erstellen**.
  
  ![Erstellen eines neuen Momentaufnahmeberichts](./media/cloudappdiscovery-set-up-snapshots/create-snapshot-command.png)
3. Geben Sie einen **Berichtsnamen** und eine **Beschreibung** ein.
    
  ![Neuer Momentaufnahmebericht](./media/cloudappdiscovery-set-up-snapshots/create-snapshot-form.png)
4. Wählen Sie die **Datenquelle** aus, aus der die Protokolldateien hochgeladen werden sollen.
5. Überprüfen Sie das Protokollformat, um sicherzustellen, dass das Protokoll ordnungsgemäß anhand des Beispiels, das Sie herunterladen können, formatiert wurde. Klicken Sie auf **Anzeigen und überprüfen** und dann auf **Beispielprotokoll herunterladen**. Vergleichen Sie dann Ihr Protokoll mit dem bereitgestellten Beispiel, um sicherzustellen, dass es kompatibel ist.
  
  ![Protokollformat überprüfen](./media/cloudappdiscovery-set-up-snapshots/create-snapshot-verify.png)
  >  [!NOTE]
  > Das FTP-Beispielformat wird in Momentaufnahmen und automatisierten Upload unterstützt, während Syslog nur in automatisierten Uploads unterstützt wird. Durch das Herunterladen eines Beispielprotokolls wird ein Beispiel-FTP-Protokoll heruntergeladen.
6. **Wählen Sie die Datenverkehrsprotokolle aus**, die Sie hochladen möchten. Sie können bis zu 20 Dateien gleichzeitig hochladen. Komprimierte und ZIP-Dateien werden ebenfalls unterstützt.
  
7. Klicken Sie auf **Erstellen**. Nach Abschluss des Uploads kann es einige Zeit dauern, bis die Analyse abgeschlossen ist. Wenn dies der Fall ist, sendet Cloud App Discovery eine E-Mail-Benachrichtigung.

8. Klicken Sie auf **Momentaufnahmeberichte verwalten**, und wählen Sie Ihren Momentaufnahmebericht aus.
  
  ![Verwaltung von Momentaufnahmeberichten](./media/cloudappdiscovery-set-up-snapshots/create-snapshot-manage.png)

## <a name="next-steps"></a>Nächste Schritte

* [Erste Schritte mit Cloud App Discovery in Azure AD](cloudappdiscovery-get-started.md)
* [Konfigurieren des automatischen Uploads von Protokollen für kontinuierliche Berichte](https://docs.microsoft.com/cloud-app-security/discovery-docker)
* [Use a custom log parser](https://docs.microsoft.com/cloud-app-security/custom-log-parser) (Verwenden eines benutzerdefinierten Protokollparsers)

