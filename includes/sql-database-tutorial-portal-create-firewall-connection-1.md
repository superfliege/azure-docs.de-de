---
author: MightyPen
ms.service: sql-database
ms.topic: include
ms.date: 12/10/2018
ms.author: genemi
ms.openlocfilehash: ab31ee82e8035fe888fa70b5796aef2c2b2939b2
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2018
ms.locfileid: "53728540"
---
## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)

## <a name="create-a-blank-sql-database"></a>Erstellen einer leeren SQL-Datenbank

Eine Azure SQL-Datenbank verfügt über eine definierte Gruppe von [Compute- und Speicherressourcen](../articles/sql-database/sql-database-service-tiers-dtu.md). Die Datenbank befindet sich in einer [Azure-Ressourcengruppe](../articles/azure-resource-manager/resource-group-overview.md) und wird auf einem [logischen Azure SQL-Datenbankserver](../articles/sql-database/sql-database-features.md) ausgeführt.

Führen Sie die folgenden Schritte aus, um eine leere SQL-­Datenbank zu erstellen:

1. Klicken Sie im Azure-Portal links oben auf **Ressource erstellen**.

1. Wählen Sie auf der Seite **Neu** die Optionen **Datenbanken** > **SQL-Datenbank** aus.

   ![Leere Datenbank erstellen](../articles/sql-database/media/sql-database-design-first-database/create-empty-database.png)

1. Geben Sie im Bereich **SQL-Datenbank** die folgenden Werte ein, oder wählen Sie sie aus:

   | Einstellung       | Empfohlener Wert | BESCHREIBUNG |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Datenbankname** | *yourDatabase* | Gültige Datenbanknamen finden Sie unter [Datenbankbezeichner](/sql/relational-databases/databases/database-identifiers). |
   | **Abonnement** | *yourSubscription*  | Ausführliche Informationen zu Ihren Abonnements finden Sie unter [Abonnements](https://account.windowsazure.com/Subscriptions). |
   | **Ressourcengruppe** | *yourResourceGroup* | Gültige Ressourcengruppennamen finden Sie unter [Naming rules and restrictions](/azure/architecture/best-practices/naming-conventions) (Benennungsregeln und Einschränkungen). |
   | **Quelle auswählen** | Leere Datenbank | Gibt an, dass eine leere Datenbank erstellt werden soll. |

   ![Datenbank erstellen](../articles/sql-database/media/sql-database-design-first-database/create-database.png)

   1. Wählen Sie **Server** aus, um einen Server für Ihre neue Datenbank zu konfigurieren. Geben Sie dann die folgenden Werte ein bzw. wählen Sie diese aus:

      | Einstellung       | Empfohlener Wert | BESCHREIBUNG |
      | ------------ | ------------------ | ------------------------------------------------- |
      | **Servername** | Ein global eindeutiger Name | Gültige Servernamen finden Sie unter [Naming rules and restrictions](/azure/architecture/best-practices/naming-conventions) (Benennungsregeln und Einschränkungen). |
      | **Serveradministratoranmeldung** | Ein gültiger Name | Gültige Anmeldenamen finden Sie unter [Datenbankbezeichner](/sql/relational-databases/databases/database-identifiers).|
      | **Kennwort** | Ein gültiges Kennwort | Ihr Kennwort muss aus mindestens acht Zeichen bestehen und Zeichen aus drei der folgenden Kategorien enthalten: Großbuchstaben, Kleinbuchstaben, Zahlen und nicht alphanumerische Zeichen. |
      | **Location** | Gültiger Standort | Informationen zu Regionen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/). |

      Wählen Sie **Auswählen**.

      ![Erstellung des Datenbankservers](../articles/sql-database/media/sql-database-design-first-database/create-database-server.png)

   1. Klicken Sie auf **Tarif**, um den Diensttarif, die Anzahl von DTUs und die Speichermenge anzugeben. Entdecken Sie die Optionen für DTUs und Speicher, die für jeden Diensttarif verfügbar sind.

      Wenn Sie den Diensttarif, die Anzahl von DTUs und die Speichermenge ausgewählt haben, wählen Sie **Übernehmen** aus.

   1. Geben Sie eine **Sortierung** für die leere Datenbank ein (verwenden Sie in diesem Tutorial den Standardwert). Weitere Informationen zu Sortierungen finden Sie unter [Sortierungen](/sql/t-sql/statements/collations).

1. Nachdem Sie das **SQL-Datenbank**-Formular ausgefüllt haben, wählen Sie **Erstellen** aus, um die Datenbank zu erstellen. Dieser Schritt kann bis zu eineinhalb Minuten dauern.

1. Klicken Sie in der Symbolleiste auf **Benachrichtigungen**, um den Bereitstellungsprozess zu überwachen.

     ![Benachrichtigung](../articles/sql-database/media/sql-database-design-first-database/notification.png)

## <a name="create-a-firewall-rule"></a>Erstellen einer Firewallregel

Der SQL-Datenbankdienst erstellt eine Firewall auf Serverebene, um zu verhindern, dass externe Anwendungen und Tools eine Verbindung mit dem Server oder mit Datenbanken auf dem Server herstellen. Führen Sie die folgenden Schritte aus, um eine [Firewallregel auf Ebene des SQL-Datenbankservers](../articles/sql-database/sql-database-firewall-configure.md) für die IP-Adresse Ihres Clients zu erstellen. Dieser Vorgang lässt eine externe Verbindung durch die SQL-Datenbankfirewall nur für Ihre IP-Adresse zu.

> [!NOTE]
> SQL-Datenbank kommuniziert über Port 1433. Wenn Sie versuchen, eine Verbindung aus einem Unternehmensnetzwerk heraus herzustellen, wird der ausgehende Datenverkehr über Port 1433 von der Firewall Ihres Netzwerks unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Azure SQL-Datenbankserver herstellen, wenn Ihr Administrator Port 1433 öffnet.

1. Klicken Sie nach Abschluss der Bereitstellung im Menü auf der linken Seite auf **SQL-Datenbanken**, und wählen Sie dann auf der Seite **SQL-Datenbanken** die Option für *IhreDatenbank* aus. Die **Übersichtsseite** für Ihre Datenbank wird geöffnet, die den vollqualifizierten Servernamen (z.B. *ihrserver.database.windows.net*) und Optionen für die weitere Konfiguration enthält.

1. Kopieren Sie den vollqualifizierten Servernamen, um mit ihm in späteren Schritten eine Verbindung mit Ihrem Server und den Datenbanken herzustellen.

   ![Servername](../articles/sql-database/media/sql-database-design-first-database/server-name.png)

1. Wählen Sie in der Symbolleiste die Option **Serverfirewall festlegen**. Die Seite **Firewalleinstellungen** für den SQL-Datenbankserver wird geöffnet.

   ![Serverfirewallregel](../articles/sql-database/media/sql-database-design-first-database/server-firewall-rule.png)

   1. Wählen Sie in der Symbolleiste die Option **Client-IP-Adresse hinzufügen** aus, um Ihre aktuelle IP-Adresse einer neuen Firewallregel hinzuzufügen. Eine Firewallregel kann Port 1433 für eine einzelne IP-Adresse oder einen Bereich von IP-Adressen öffnen.

   1. Wählen Sie **Speichern** aus. Für Ihre aktuelle IP-Adresse wird eine Firewallregel auf Serverebene erstellt, und auf dem logischen Server wird Port 1433 geöffnet.

   1. Wählen Sie **OK** aus, und schließen Sie anschließend die Seite **Firewalleinstellungen**.

Nun kann Ihre IP-Adresse die Firewall passieren, und Sie können eine Verbindung mit dem SQL-Datenbankserver und den zugehörigen Datenbanken herstellen, indem Sie SQL Server Management Studio oder ein anderes Tool Ihrer Wahl verwenden. Verwenden Sie unbedingt das Serveradministratorkonto, das Sie zuvor erstellt haben.

> [!IMPORTANT]
> Standardmäßig ist der Zugriff über die SQL-Datenbank-Firewall für alle Azure-Dienste aktiviert. Wählen Sie auf dieser Seite die Option **AUS** aus, um diesen für alle Azure-Dienste zu deaktivieren.
