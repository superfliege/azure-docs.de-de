---
title: Konfigurieren von Serverparametern in Azure Database for PostgreSQL mit dem Azure-Portal
description: In diesem Artikel wird beschrieben, wie Sie die Serverparameter in Azure Database for PostgreSQL mithilfe des Azure-Portals konfigurieren.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 6d43cac79c19e117385549b1678a464dc5731bd7
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="configure-server-parameters-in-azure-portal"></a>Konfigurieren von Serverparametern im Azure-Portal
Sie können Konfigurationsparameter für einen Azure Database for PostgreSQL-Server mit dem Azure-Portal auflisten, anzeigen und aktualisieren.

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:
- [Azure Database for PostgreSQL-Server](quickstart-create-server-database-portal.md)

## <a name="viewing-and-editing-parameters"></a>Anzeigen und Bearbeiten von Parametern
1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).

2. Wählen Sie Ihre Azure Database for PostgreSQL-Server aus.

3. Wählen Sie im Abschnitt **EINSTELLUNGEN** die Option **Serverparameter**. Auf der Seite werden eine Liste mit Parametern und die dazugehörigen Werte und Beschreibungen angezeigt.
![Übersichtsseite für Parameter](./media/howto-configure-server-parameters-in-portal/3-overview-of-parameters.png)

4. Wählen Sie die **Dropdownschaltfläche** , um die möglichen Werte für Enumerierungsparameter wie „client_min_messages“ anzuzeigen.
![Dropdownliste für Enumerierung](./media/howto-configure-server-parameters-in-portal/4-enum-drop-down.png)

5. Wählen Sie die Schaltfläche **i** (Informationen), oder zeigen Sie mit der Maus darauf, um den Bereich mit den möglichen Werten für numerische Parameter wie „cpu_index_tuple_cost“ anzuzeigen.
![Schaltfläche „Informationen“](./media/howto-configure-server-parameters-in-portal/4-information-button.png)

6. Verwenden Sie bei Bedarf das **Suchfeld**, um nach einem bestimmten Parameter zu suchen. Die Suche wird für den Namen und die Beschreibung der Parameter durchgeführt.
![Suchergebnisse](./media/howto-configure-server-parameters-in-portal/5-search.png)

7. Ändern Sie die Parameterwerte, die Sie anpassen möchten. Alle in einer Sitzung vorgenommenen Änderungen werden violett hervorgehoben. Nachdem Sie die Werte geändert haben, können Sie **Speichern** wählen. Sie können Ihre Änderungen auch **verwerfen**.
![Änderungen speichern oder verwerfen](./media/howto-configure-server-parameters-in-portal/6-save-and-discard-buttons.png)

8. Wenn Sie neue Werte für die Parameter gespeichert haben, können Sie jederzeit alles zurück auf die Standardwerte setzen, indem Sie die Option **Alle auf Standard zurücksetzen** wählen.
![Alle auf Standard zurücksetzen](./media/howto-configure-server-parameters-in-portal/7-reset-to-default-button.png)

## <a name="next-steps"></a>Nächste Schritte
Sie erhalten Informationen zu folgenden Themen:
- [Azure Database for PostgreSQL-Server](concepts-servers.md)
- [Anpassen der Serverkonfigurationsparameter mithilfe der Azure CLI](howto-configure-server-parameters-using-cli.md)
