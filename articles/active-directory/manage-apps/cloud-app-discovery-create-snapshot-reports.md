---
title: Erstellen von Cloud App Discovery-Momentaufnahmeberichten in Azure Active Directory | Microsoft-Dokumentation
description: Bietet Informationen zum Suchen und Verwalten von Anwendungen mit Cloud App Discovery, zu den Vorteilen und zu der Funktionsweise.
services: active-directory
keywords: Cloud App Discovery, Verwalten von Anwendungen
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.topic: article
ms.date: 09/22/2017
ms.author: barbkess
ms.reviewer: nigu
ms.custom: it-pro
ms.openlocfilehash: 68585edad6e7d1b6b21a48f1cf4242875cea538a
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2018
ms.locfileid: "34058189"
---
# <a name="create-cloud-app-discovery-snapshot-reports"></a>Erstellen von Cloud App Discovery-Momentaufnahmeberichten

Laden Sie vor dem Einrichten des automatischen Protokollsammlers ein Protokoll manuell hoch, und lassen Sie es von Cloud App Security analysieren. Wenn Sie noch kein Protokoll besitzen und gern ein Beispielprotokoll hätten, können Sie mit dem folgenden Verfahren eine Beispielprotokolldatei herunterladen, um zu erfahren, wie Ihre Protokolldatei aussehen sollte.

## <a name="to-create-a-snapshot-report"></a>So erstellen Sie einen Momentaufnahmebericht

1. Sammeln Sie Protokolldateien von Ihrer Firewall und Ihrem Proxyserver, über die Benutzer in Ihrer Organisation auf das Internet zugreifen. Sammeln Sie Protokolle zu Spitzenverkehrszeiten, die für die Aktivitäten der Benutzern in Ihrer Organisation repräsentativ sind.
2. Klicken Sie in der [Menüleiste von Cloud App Security](https://portal.cloudappsecurity.com) auf die Option **Discover** (Ermitteln) und dann auf **Momentaufnahmebericht erstellen**.
  
  ![Erstellen eines neuen Momentaufnahmeberichts](./media/cloud-app-discovery-create-snapshot-reports/create-snapshot-command.png)
3. Geben Sie einen **Berichtsnamen** und eine **Beschreibung** ein.
    
  ![Neuer Momentaufnahmebericht](./media/cloud-app-discovery-create-snapshot-reports/create-snapshot-form.png)
4. Wählen Sie die **Datenquelle** aus, aus der die Protokolldateien hochgeladen werden sollen.
5. Überprüfen Sie das Protokollformat, um sicherzustellen, dass das Protokoll ordnungsgemäß anhand des Beispiels, das Sie herunterladen können, formatiert wurde. Klicken Sie auf **Anzeigen und überprüfen** und dann auf **Beispielprotokoll herunterladen**. Vergleichen Sie dann Ihr Protokoll mit dem bereitgestellten Beispiel, um sicherzustellen, dass es kompatibel ist.
  
  ![Protokollformat überprüfen](./media/cloud-app-discovery-create-snapshot-reports/create-snapshot-verify.png)
  >  [!NOTE]
  > Das FTP-Beispielformat wird in Momentaufnahmen und automatisierten Upload unterstützt, während Syslog nur in automatisierten Uploads unterstützt wird. Durch das Herunterladen eines Beispielprotokolls wird ein Beispiel-FTP-Protokoll heruntergeladen.
6. **Wählen Sie die Datenverkehrsprotokolle aus**, die Sie hochladen möchten. Sie können bis zu 20 Dateien gleichzeitig hochladen. Komprimierte und ZIP-Dateien werden ebenfalls unterstützt.
  
7. Klicken Sie auf **Create**. Nach Abschluss des Uploads kann es einige Zeit dauern, bis die Analyse abgeschlossen ist. Wenn dies der Fall ist, sendet Cloud App Discovery eine E-Mail-Benachrichtigung.

8. Klicken Sie auf **Momentaufnahmeberichte verwalten**, und wählen Sie Ihren Momentaufnahmebericht aus.
  
  ![Verwaltung von Momentaufnahmeberichten](./media/cloud-app-discovery-create-snapshot-reports/create-snapshot-manage.png)

## <a name="next-steps"></a>Nächste Schritte

* [Erste Schritte mit Cloud App Discovery in Azure AD](cloud-app-discovery.md)
* [Konfigurieren des automatischen Uploads von Protokollen für kontinuierliche Berichte](https://docs.microsoft.com/cloud-app-security/discovery-docker)
* [Use a custom log parser](https://docs.microsoft.com/cloud-app-security/custom-log-parser) (Verwenden eines benutzerdefinierten Protokollparsers)
