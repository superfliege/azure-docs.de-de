---
title: 'Tutorial: Entwerfen Ihrer ersten Einzeldatenbank in Azure SQL-Datenbank mit SSMS | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie Ihre erste Azure SQL-Datenbank mit SQL Server Management Studio entwerfen.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.topic: tutorial
author: CarlRabeler
ms.author: carlrab
ms.reviewer: v-masebo
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: e7229a0816cf74fed08397a68dd34e305bf8c0ea
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55459535"
---
# <a name="tutorial-design-your-first-azure-sql-database-using-ssms"></a>Tutorial: Entwurf Ihrer ersten Azure SQL-Datenbank mithilfe von SSMS

Azure SQL-Datenbank ist eine relationale DBaaS-Lösung (Database-as-a-Service) in Microsoft Cloud (Azure). In diesem Tutorial erfahren Sie, wie Sie das Azure-Portal und [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS) für folgende Zwecke verwenden:

> [!div class="checklist"]
> * Erstellen einer Datenbank im Azure-Portal*
> * Einrichten einer Firewallregel auf Serverebene im Azure-Portal
> * Herstellen einer Verbindung für die Datenbank mit SSMS
> * Erstellen von Tabellen mit SSMS
> * Massenladen von Daten mit BCP
> * Abfragen von Daten mit SSMS

*Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/), bevor Sie beginnen.

> [!NOTE]
> In diesem Tutorial verwenden wir das [DTU-basierte Kaufmodell](sql-database-service-tiers-dtu.md). Sie haben jedoch auch die Möglichkeit, das [V-Kern-basierte Kaufmodell](sql-database-service-tiers-vcore.md) auszuwählen.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie dieses Tutorial ausführen können, müssen folgende Komponenten installiert sein:

- [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (aktuelle Version)
- [BCP und sqlcmd](https://www.microsoft.com/download/details.aspx?id=36433) (aktuelle Version)

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="create-a-blank-database"></a>Leere Datenbank erstellen

Eine Azure SQL-Datenbank wird mit einer definierten Gruppe von [Compute- und Speicherressourcen](sql-database-service-tiers-dtu.md) erstellt. Die Datenbank wird in einer [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) und auf einem [Azure SQL-Datenbankserver](sql-database-features.md) erstellt.

Führen Sie die folgenden Schritte aus, um eine leere SQL-­Datenbank zu erstellen:

1. Klicken Sie im Azure-Portal links oben auf **Ressource erstellen**.

1. Wählen Sie auf der Seite **Neu** im Abschnitt „Azure Marketplace“ die Option **Datenbanken** aus, und klicken Sie dann im Abschnitt **Empfohlen** auf **SQL-Datenbank**.

   ![Leere Datenbank erstellen](./media/sql-database-design-first-database/create-empty-database.png)

   1. Geben Sie die folgenden Informationen in das **SQL-Datenbank**-Formular ein, wie in der obigen Abbildung dargestellt:

      | Einstellung       | Empfohlener Wert | BESCHREIBUNG |
      | ------------ | ------------------ | ------------------------------------------------- |
      | **Datenbankname** | *yourDatabase* | Gültige Datenbanknamen finden Sie unter [Datenbankbezeichner](/sql/relational-databases/databases/database-identifiers). |
      | **Abonnement** | *yourSubscription*  | Ausführliche Informationen zu Ihren Abonnements finden Sie unter [Abonnements](https://account.windowsazure.com/Subscriptions). |
      | **Ressourcengruppe** | *yourResourceGroup* | Gültige Ressourcengruppennamen finden Sie unter [Naming rules and restrictions](/azure/architecture/best-practices/naming-conventions) (Benennungsregeln und Einschränkungen). |
      | **Quelle auswählen** | Leere Datenbank | Gibt an, dass eine leere Datenbank erstellt werden soll. |

   1. Klicken Sie auf **Server**, um einen vorhandenen Server zu verwenden oder um einen neuen Server für Ihre neue Datenbank zu erstellen und zu konfigurieren. Wählen Sie den Server aus, oder klicken Sie auf **Neuen Server erstellen**, und geben Sie im Formular **Neuer Server** die folgenden Informationen ein:

      | Einstellung       | Empfohlener Wert | BESCHREIBUNG |
      | ------------ | ------------------ | ------------------------------------------------- |
      | **Servername** | Ein global eindeutiger Name | Gültige Servernamen finden Sie unter [Naming rules and restrictions](/azure/architecture/best-practices/naming-conventions) (Benennungsregeln und Einschränkungen). |
      | **Serveradministratoranmeldung** | Ein gültiger Name | Gültige Anmeldenamen finden Sie unter [Datenbankbezeichner](/sql/relational-databases/databases/database-identifiers). |
      | **Kennwort** | Ein gültiges Kennwort | Ihr Kennwort muss mindestens acht Zeichen umfassen und Zeichen aus drei der folgenden Kategorien enthalten: Großbuchstaben, Kleinbuchstaben, Zahlen und nicht alphanumerische Zeichen. |
      | **Location** | Gültiger Standort | Informationen zu Regionen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/). |

      ![Erstellung des Datenbankservers](./media/sql-database-design-first-database/create-database-server.png)

      Klicken Sie auf **Auswählen**.

   1. Klicken Sie auf **Tarif**, um die Dienstebene, die Anzahl von DTUs oder virtuellen Kernen und die Speichermenge anzugeben. Sie können sich mit den Optionen für die Anzahl von DTUs/virtuellen Kernen sowie mit den Speicheroptionen vertraut machen, die für die einzelnen Dienstebenen verfügbar sind. Standardmäßig ist das [DTU-basierte Kaufmodell](sql-database-service-tiers-dtu.md) vom Typ **Standard** ausgewählt. Sie haben jedoch auch die Möglichkeit, das [V-Kern-basierte Kaufmodell](sql-database-service-tiers-vcore.md) auszuwählen.

      > [!IMPORTANT]
      > In allen Regionen außer den folgenden ist im Premium-Tarif derzeit mehr als 1 TB Speicher verfügbar: „Vereinigtes Königreich, Norden“, „USA, Westen-Mitte“, „Vereinigtes Königreich, Süden2“, „China, Osten“, „US DoD, Mitte“, „Deutschland, Mitte“, „US DoD, Osten“, „US Gov, Südwesten“, „US Gov, Süden-Mitte“, „Deutschland, Nordosten“, „China, Norden“, „US Gov, Osten“. In anderen Regionen ist der Speicher im Premium-Tarif auf 1 TB begrenzt. Siehe [Aktuelle Einschränkungen für P11–P15]( sql-database-dtu-resource-limits-single-databases.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).

      Klicken Sie auf **Übernehmen**, wenn Sie die Dienstebene, die Anzahl von DTUs und die Menge an Speicherplatz ausgewählt haben.

   1. Geben Sie eine **Sortierung** für die leere Datenbank ein (verwenden Sie in diesem Tutorial den Standardwert). Weitere Informationen über Sortierungen finden Sie unter [Sortierungen](/sql/t-sql/statements/collations).

1. Nachdem Sie das **SQL-Datenbank**-Formular ausgefüllt haben, können Sie auf **Erstellen** klicken, um die Datenbank bereitzustellen. Dieser Schritt kann einige Minuten dauern.

1. Klicken Sie in der Symbolleiste auf **Benachrichtigungen**, um den Bereitstellungsprozess zu überwachen.

     ![Benachrichtigung](./media/sql-database-design-first-database/notification.png)

## <a name="create-a-firewall-rule"></a>Erstellen einer Firewallregel

Der SQL-Datenbank-Dienst erstellt eine Firewall auf Serverebene. Die Firewall verhindert, dass externe Anwendungen und Tools eine Verbindung mit dem Server und mit Datenbanken auf dem Server herstellen. Um externe Verbindungen mit Ihrer Datenbank zuzulassen, müssen Sie der Firewall zunächst eine Regel für Ihre IP-Adresse hinzufügen. Führen Sie die folgenden Schritte aus, um eine [SQL-Datenbank-Firewallregel auf Serverebene](sql-database-firewall-configure.md) zu erstellen.

> [!NOTE]
> SQL-Datenbank kommuniziert über Port 1433. Wenn Sie versuchen, eine Verbindung aus einem Unternehmensnetzwerk heraus herzustellen, wird der ausgehende Datenverkehr über Port 1433 von der Firewall Ihres Netzwerks unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Azure SQL-Datenbankserver herstellen, wenn Ihr Administrator Port 1433 öffnet.

1. Klicken Sie nach Abschluss der Bereitstellung im Menü auf der linken Seite auf **SQL-Datenbanken**, und klicken Sie dann auf der Seite **SQL-Datenbanken** auf *yourDatabase*. Die Übersichtsseite für Ihre Datenbank wird geöffnet, die den vollqualifizierten **Servernamen** (z.B. *yourserver.database.windows.net*) und Optionen für die weitere Konfiguration enthält.

1. Kopieren Sie diesen vollqualifizierten Servernamen, um in SQL Server Management Studio eine Verbindung mit Ihrem Server und den Datenbanken herzustellen.

   ![Servername](./media/sql-database-design-first-database/server-name.png)

1. Klicken Sie auf der Symbolleiste auf **Serverfirewall festlegen**. Die Seite **Firewalleinstellungen** für den SQL-Datenbankserver wird geöffnet.

   ![Serverfirewallregel](./media/sql-database-design-first-database/server-firewall-rule.png)

   1. Klicken Sie in der Symbolleiste auf **Client-IP-Adresse hinzufügen**, um Ihre aktuelle IP-Adresse einer neuen Firewallregel hinzuzufügen. Eine Firewallregel kann Port 1433 für eine einzelne IP-Adresse oder einen Bereich von IP-Adressen öffnen.

   1. Klicken Sie auf **Speichern**. Für Ihre aktuelle IP-Adresse wird eine Firewallregel auf Serverebene erstellt, und auf dem SQL-Datenbankserver wird Port 1433 geöffnet.

   1. Klicken Sie auf **OK**, und schließen Sie anschließend die Seite **Firewalleinstellungen**.

Die IP-Adresse kann nun die Firewall passieren. Nun können Sie eine Verbindung mit dem SQL-Datenbankserver und den zugehörigen Datenbanken herstellen, indem Sie SQL Server Management Studio oder ein anderes Tool Ihrer Wahl verwenden. Verwenden Sie das Serveradministratorkonto, das Sie zuvor erstellt haben.

> [!IMPORTANT]
> Standardmäßig ist der Zugriff über die SQL-Datenbank-Firewall für alle Azure-Dienste aktiviert. Klicken Sie auf dieser Seite auf **AUS**, um dies für alle Azure-Dienste zu deaktivieren.

## <a name="connect-to-the-database"></a>Herstellen einer Verbindung mit der Datenbank

Verwenden Sie [SQL Server Management Studio](/sql/ssms/sql-server-management-studio-ssms), um eine Verbindung mit Ihrem Azure SQL-Datenbankserver herzustellen.

1. Öffnen Sie SQL Server Management Studio.

1. Geben Sie im Dialogfeld **Mit Server verbinden** die folgenden Informationen ein:

   | Einstellung       | Empfohlener Wert | BESCHREIBUNG |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Servertyp** | Datenbank-Engine | Dieser Wert ist erforderlich. |
   | **Servername** | Der vollqualifizierte Servername | Beispiel: *yourserver.database.windows.net* |
   | **Authentifizierung** | SQL Server-Authentifizierung | In diesem Tutorial haben Sie als einzigen Authentifizierungstyp die SQL-Authentifizierung konfiguriert. |
   | **Anmeldung** | Das Serveradministratorkonto | Hierbei handelt es sich um das Konto, das Sie bei der Servererstellung angegeben haben. |
   | **Kennwort** | Das Kennwort für das Serveradministratorkonto | Das Kennwort, das Sie beim Erstellen des Servers angegeben haben |

   ![Verbindung mit dem Server herstellen](./media/sql-database-design-first-database/connect.png)

   1. Klicken Sie im Dialogfeld **Mit Server verbinden** auf **Optionen**. Geben Sie im Abschnitt **Verbindung mit Datenbank herstellen** den Namen *yourDatabase* ein, um eine Verbindung mit dieser Datenbank herzustellen.

      ![Herstellen einer Verbindung mit der Datenbank auf dem Server](./media/sql-database-design-first-database/options-connect-to-db.png)  

   1. Klicken Sie auf **Verbinden**. Das **Objekt-Explorer**-Fenster wird in SSMS geöffnet.

1. Erweitern Sie im **Objekt-Explorer** die Option **Datenbanken** und anschließend die Option *yourDatabase*, um die Objekte in der Beispieldatenbank anzuzeigen.

   ![Datenbankobjekte](./media/sql-database-design-first-database/connected.png)  

## <a name="create-tables-in-the-database"></a>Erstellen von Tabellen in der Datenbank

Erstellen Sie mit [Transact-SQL-](/sql/t-sql/language-reference) ein Datenbankschema mit vier Tabellen, die ein Studentenverwaltungssystem für Universitäten modellieren:

- Person
- Course (Lehrveranstaltung)
- Student
- Quelle

Die folgende Abbildung zeigt, wie diese Tabellen miteinander verknüpft sind. Aus einigen dieser Tabellen wird auf Spalten in anderen Tabellen verwiesen. Beispielsweise wird aus der Tabelle *Student* auf die Spalte *PersonId* der Tabelle *Person* verwiesen. Sehen Sie sich die Abbildung an, um zu verstehen, wie die Tabellen in diesem Tutorial miteinander verknüpft sind. Eine ausführliche Beschreibung, wie effiziente Datenbanktabellen erstellt werden, finden Sie unter [Erstellen von effizienten Datenbanktabellen](https://msdn.microsoft.com/library/cc505842.aspx). Informationen zum Auswählen von Datentypen finden Sie unter [Datentypen](/sql/t-sql/data-types/data-types-transact-sql).

> [!NOTE]
> Sie können auch den [Tabellen-Designer in SQL Server Management Studio](/sql/ssms/visual-db-tools/design-database-diagrams-visual-database-tools) verwenden, um diese Tabellen zu entwerfen und zu erstellen.

![Tabellenbeziehungen](./media/sql-database-design-first-database/tutorial-database-tables.png)

1. Klicken Sie mit der rechten Maustaste im **Objekt-Explorer** auf *yourDatabase*, und wählen Sie **Neue Abfrage** aus. Ein leeres Abfragefenster mit einer Verbindung mit Ihrer Datenbank wird geöffnet.

1. Führen Sie im Abfragefenster die folgende Abfrage aus, um vier Tabellen in der Datenbank zu erstellen:

   ```sql
   -- Create Person table
   CREATE TABLE Person
   (
       PersonId INT IDENTITY PRIMARY KEY,
       FirstName NVARCHAR(128) NOT NULL,
       MiddelInitial NVARCHAR(10),
       LastName NVARCHAR(128) NOT NULL,
       DateOfBirth DATE NOT NULL
   )

   -- Create Student table
   CREATE TABLE Student
   (
       StudentId INT IDENTITY PRIMARY KEY,
       PersonId INT REFERENCES Person (PersonId),
       Email NVARCHAR(256)
   )

   -- Create Course table
   CREATE TABLE Course
   (
       CourseId INT IDENTITY PRIMARY KEY,
       Name NVARCHAR(50) NOT NULL,
       Teacher NVARCHAR(256) NOT NULL
   )

   -- Create Credit table
   CREATE TABLE Credit
   (
       StudentId INT REFERENCES Student (StudentId),
       CourseId INT REFERENCES Course (CourseId),
       Grade DECIMAL(5,2) CHECK (Grade <= 100.00),
       Attempt TINYINT,
       CONSTRAINT [UQ_studentgrades] UNIQUE CLUSTERED
       (
           StudentId, CourseId, Grade, Attempt
       )
   )
   ```

   ![Erstellen von Tabellen.](./media/sql-database-design-first-database/create-tables.png)

1. Erweitern Sie im **Objekt-Explorer** unter *yourDatabase* den Knoten **Tabellen**, um die von Ihnen erstellten Tabellen anzuzeigen.

   ![Erstellte Tabellen in SMS](./media/sql-database-design-first-database/ssms-tables-created.png)

## <a name="load-data-into-the-tables"></a>Laden von Daten in die Tabellen

1. Erstellen Sie in Ihrem Ordner „Downloads“ einen Ordner namens *sampleData*, in dem die Beispieldaten für Ihre Datenbank gespeichert werden.

1. Klicken Sie mit der rechten Maustaste auf die folgenden Links, und speichern Sie sie im Ordner *sampleData*.

   - [SampleCourseData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleCourseData)
   - [SamplePersonData](https://sqldbtutorial.blob.core.windows.net/tutorials/SamplePersonData)
   - [SampleStudentData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleStudentData)
   - [SampleCreditData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleCreditData)

1. Öffnen Sie ein Eingabeaufforderungsfenster, und navigieren Sie zum Ordner *sampleData*.

1. Führen Sie die folgenden Befehle aus, um Beispieldaten in die Tabellen einzufügen. Ersetzen Sie dabei die Werte für *server*, *database*, *user* und *password* durch die Werte für Ihre Umgebung.
  
   ```cmd
   bcp Course in SampleCourseData -S <server>.database.windows.net -d <database> -U <user> -P <password> -q -c -t ","
   bcp Person in SamplePersonData -S <server>.database.windows.net -d <database> -U <user> -P <password> -q -c -t ","
   bcp Student in SampleStudentData -S <server>.database.windows.net -d <database> -U <user> -P <password> -q -c -t ","
   bcp Credit in SampleCreditData -S <server>.database.windows.net -d <database> -U <user> -P <password> -q -c -t ","
   ```

Sie haben jetzt Beispieldaten in die Tabellen geladen, die Sie zuvor erstellt haben.

## <a name="query-data"></a>Abfragen von Daten

Führen Sie die folgenden Abfragen aus, um Informationen aus den Datenbanktabellen abzurufen. Weitere Informationen zum Schreiben von SQL-Abfragen finden Sie unter [Writing SQL Queries](https://technet.microsoft.com/library/bb264565.aspx) (Schreiben von SQL-Abfragen). In der ersten Abfrage werden alle vier Tabellen verknüpft, um alle Kursteilnehmer zu finden, die von „Dominick Pope“ unterrichtet werden und ein Ergebnis (Grade) haben, das über 75 Prozent liegt. In der zweiten Abfrage werden alle vier Tabellen verknüpft und die Lehrveranstaltungen gesucht, für die „Noe Coleman“ jemals eingeschrieben war.

1. Führen Sie in einem Abfragefenster von SQL Server Management Studio die folgende Abfrage aus:

   ```sql
   -- Find the students taught by Dominick Pope who have a grade higher than 75%
   SELECT  person.FirstName, person.LastName, course.Name, credit.Grade
   FROM  Person AS person
       INNER JOIN Student AS student ON person.PersonId = student.PersonId
       INNER JOIN Credit AS credit ON student.StudentId = credit.StudentId
       INNER JOIN Course AS course ON credit.CourseId = course.courseId
   WHERE course.Teacher = 'Dominick Pope'
       AND Grade > 75
   ```

1. Führen Sie in einem Abfragefenster die folgende Abfrage aus:

   ```sql
   -- Find all the courses in which Noe Coleman has ever enrolled
   SELECT  course.Name, course.Teacher, credit.Grade
   FROM  Course AS course
       INNER JOIN Credit AS credit ON credit.CourseId = course.CourseId
       INNER JOIN Student AS student ON student.StudentId = credit.StudentId
       INNER JOIN Person AS person ON person.PersonId = student.PersonId
   WHERE person.FirstName = 'Noe'
       AND person.LastName = 'Coleman'
   ```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie viele grundlegende Datenbankaufgaben kennengelernt. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen einer Datenbank
> * Einrichten einer Firewallregel
> * Herstellen einer Verbindung mit der Datenbank mit [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS)
> * Erstellen von Tabellen.
> * Massenladen von Daten
> * Abfragen der Daten

Im nächsten Tutorial erfahren Sie, wie Sie eine Datenbank mit Visual Studio und C# entwerfen.

> [!div class="nextstepaction"]
> [Entwerfen einer Azure SQL-Datenbank und Herstellen einer Verbindung mit C# und ADO.NET](sql-database-design-first-database-csharp.md)
