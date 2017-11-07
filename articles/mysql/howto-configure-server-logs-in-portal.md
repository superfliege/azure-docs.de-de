---
title: "Konfigurieren der und Zugreifen auf die Serverprotokolle für Azure Database for MySQL im Azure-Portal | Microsoft-Dokumentation"
description: In diesem Artikel wird beschrieben, wie Sie aus dem Azure-Portal die Serverprotokolle in Azure Database for MySQL konfigurieren und auf diese zugreifen.
services: mysql
author: rachel-msft
ms.author: raagyema
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 10/19/2017
ms.openlocfilehash: 89674c133b458c16fbdacd771be24830624dde7c
ms.sourcegitcommit: 2d1153d625a7318d7b12a6493f5a2122a16052e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2017
---
# <a name="configure-and-access-server-logs-in-the-azure-portal"></a>Konfigurieren der und Zugreifen auf die Serverprotokolle im Azure-Portal

Sie können die [Azure Database for MySQL-Serverprotokolle](concepts-server-logs.md) im Azure-Portal konfigurieren und auflisten sowie aus dem Portal herunterladen.

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:
- [Azure Database for MySQL-Server](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="configure-logging"></a>Konfigurieren der Protokollierung
Konfigurieren Sie den Zugriff auf das MySQL-Protokoll für langsame Abfragen. 

1. Melden Sie sich auf dem [Azure-Portal](http://portal.azure.com/)an.

2. Wählen Sie Ihren Server für Azure Database for MySQL aus.

3. Wählen Sie im Abschnitt **Überwachung** in der Randleiste die Option **Serverprotokolle** aus. 
   ![„Serverprotokolle“ auswählen und zum Konfigurieren klicken](./media/howto-configure-server-logs-in-portal/1-select-server-logs-configure.png)

4. Wählen Sie die Überschrift **Klicken Sie hier, um Protokolle zu aktivieren und Protokollparameter zu konfigurieren** aus, um die Serverparameter anzuzeigen.

5. Wählen Sie die Erweiterung **Mehr anzeigen** aus, um eine erweiterte Liste der verfügbaren Parameter anzuzeigen. 

   Weitere Informationen zu den Definitionen von Parametern finden Sie in der MySQL-Dokumentation unter [Protokolle](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html).

   ![Auf „Mehr anzeigen“ klicken, um die vollständige Liste anzuzeigen](./media/howto-configure-server-logs-in-portal/2-show-more.png)

6. Ändern Sie die Parameter, die Sie anpassen müssen. Alle in dieser Sitzung vorgenommenen Änderungen werden in violett hervorgehoben. 

   Nachdem Sie die Parameter geändert haben, klicken Sie auf **Speichern**. Sie können Ihre Änderungen auch **verwerfen**.

   ![Auf „Speichern“ oder „Verwerfen“ klicken](./media/howto-configure-server-logs-in-portal/3-save-discard.png)

7. Kehren Sie zur Liste der Protokolle zurück, indem Sie auf der Seite **Serverparameter** auf die **Schaltfläche „Schließen“** (X-Symbol) klicken.

## <a name="view-list-and-download-logs"></a>Anzeigen der Liste und Herunterladen von Protokollen
Sobald die Protokollierung beginnt, können Sie eine Liste der verfügbaren Protokolle anzeigen und einzelne Protokolldateien im Bereich „Serverprotokolle“ herunterladen. 

1. Öffnen Sie das Azure-Portal.

2. Wählen Sie Ihren Server für Azure Database for MySQL aus.

3. Wählen Sie im Abschnitt **Überwachung** in der Randleiste die Option **Serverprotokolle** aus. Die Seite zeigt wie im folgenden Beispiel eine Liste der Protokolldateien an:

   ![Liste der Protokolle](./media/howto-configure-server-logs-in-portal/4-server-logs-list.png)

   > [!TIP]
   > Die Namenskonvention des Protokolls ist **mysql-slow-<Name_Ihres_Servers>-jjjjmmtthh.log**. Das im Dateinamen verwendete Datum und die Uhrzeit geben den Zeitpunkt an, zu dem das Protokoll ausgestellt wurde. Die Protokolldateien werden alle 24 Stunden oder bei Erreichen einer Größe von 7,5 GB rotiert.

4. Verwenden Sie bei Bedarf das **Suchfeld**, um schnell ein spezifisches Protokoll basierend auf Datum/Uhrzeit einzugrenzen. Die Suche erfolgt anhand des Namens des Protokolls.

5. Laden Sie einzelne Protokolldateien wie gezeigt über die Schaltfläche **Download** (nach unten gerichtetes Pfeilsymbol) neben jeder Protokolldatei in der Tabellenzeile herunter:

   ![Klicken Sie auf das Symbol „Download“.](./media/howto-configure-server-logs-in-portal/5-download.png)


## <a name="next-steps"></a>Nächste Schritte
- Lesen Sie [Zugriff auf Serverprotokolle in der CLI](howto-configure-server-logs-in-cli.md), um weitere Informationen zum programmgesteuerten Herunterladen von Protokollen zu erhalten.
- Erhalten Sie weitere Informationen zu [Serverprotokollen](concepts-server-logs.md) in Azure Database for MySQL. 
- Weitere Informationen zu den Parameterdefinitionen und der MySQL-Protokollierung finden Sie in der MySQL-Dokumentation unter [Protokolle](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html).

