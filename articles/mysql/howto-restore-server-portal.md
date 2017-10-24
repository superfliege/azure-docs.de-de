---
title: "Wiederherstellen eines Servers in Azure-Datenbank für MySQL | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie mit dem Azure-Portal einen Server in Azure-Datenbank für MySQL wiederherstellen."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 09/15/2017
ms.openlocfilehash: 6c1c0f8a0c0e59661b70b787b551b8cfdb024cda
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-back-up-and-restore-a-server-in-azure-database-for-mysql-by-using-the-azure-portal"></a>Sichern und Wiederherstellen eines Servers in Azure Database for MySQL mit dem Azure-Portal

## <a name="backup-happens-automatically"></a>Automatische Sicherung
Bei der Verwendung von Azure Database for MySQL erstellt der Datenbankdienst automatisch alle fünf Minuten eine Sicherung des Diensts. 

Die Sicherungen sind im Basic-Tarif sieben Tage und im Standard-Tarif 35 Tage lang verfügbar. Weitere Informationen finden Sie im Artikel zu den [Dienstebenen für Azure-Datenbank für MySQL](concepts-service-tiers.md).

Mithilfe dieses automatischen Sicherungsfeatures können Sie einen früheren Zustand des Servers und aller seiner Datenbanken wiederherstellen.

## <a name="restore-in-the-azure-portal"></a>Wiederherstellen im Azure-Portal
Mit Azure Database for MySQL können Sie den Zustand des Servers zu einem bestimmten Zeitpunkt und in einer neuen Kopie des Servers wiederherstellen. Sie können diesen neuen Server zur Wiederherstellung Ihrer Daten verwenden. 

Beispiel: Wenn eine Tabelle heute um 12 Uhr versehentlich gelöscht wurde, können Sie den Zustand von kurz vor 12 Uhr wiederherstellen und die fehlende Tabelle und die Daten aus der neuen Kopie des Servers abrufen.

Mithilfe der folgenden Schritte wird der Status des Beispielservers zu einem bestimmten Zeitpunkt wiederhergestellt:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Suchen Sie den Server für Azure-Datenbank für MySQL. Wählen Sie im linken Bereich **Alle Ressourcen** und anschließend den Server in der Liste aus.

3.  Klicken Sie oben auf dem Serverübersichtsblatt auf der Symbolleiste auf **Wiederherstellen**. Das Blatt „Wiederherstellen“ wird geöffnet.
![Klicken auf die Schaltfläche „Wiederherstellen“](./media/howto-restore-server-portal/click-restore-button.png)

4. Geben Sie im Formular „Wiederherstellen“ die erforderlichen Informationen ein:

- **Wiederherstellungspunkt (UTC)**: Wählen Sie mithilfe der Datums- und Zeitauswahl einen Zeitpunkt aus, dessen Zustand wiederhergestellt werden soll. Die Zeit wird im UTC-Format angegeben, daher müssen Sie die Ortszeit wahrscheinlich in UTC umrechnen.
- **Auf neuem Server wiederherstellen**: Geben Sie den Namen eines neuen Servers an, auf dem der vorhandene Server wiederhergestellt werden soll.
- **Standort**: Die Region wird automatisch mit der Region des Quellservers ausgefüllt und kann nicht geändert werden.
- **Tarif**: Als Tarif wird automatisch der gleiche Tarif wie für den Quellserver festgelegt, der hier nicht geändert werden kann. 
![PITR-Wiederherstellung](./media/howto-restore-server-portal/pitr-restore.png)

5. Klicken Sie auf **OK**, um den Zustand eines Servers zu einem bestimmten Zeitpunkt wiederherzustellen. 

6. Suchen Sie nach Abschluss der Wiederherstellung den neuen erstellten Server, um zu überprüfen, ob die Datenbanken wie erwartet wiederhergestellt wurden.

## <a name="next-steps"></a>Nächste Schritte
- [Datenverbindungsbibliotheken für Azure Database for MySQL](concepts-connection-libraries.md)