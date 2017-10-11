
<a name="cs_0_csharpprogramexample_h2"/>

## <a name="c-program-example"></a>Beispiel für c#-Programm

In den nächsten Abschnitten dieses Artikels vorhanden, ein C#-Programm, das ADO.NET zum Senden von Transact-SQL-Anweisungen an die SQL-Datenbank verwendet wird. Das C#-Programm führt folgenden Aktionen aus:

1. [Stellt eine Verbindung her, um unsere SQL-Datenbank mithilfe von ADO.NET](#cs_1_connect).
2. [Erstellt Tabellen](#cs_2_createtables).
3. [Füllt die Tabellen mit Daten, indem Sie T-SQL-INSERT-Anweisungen ausgeben](#cs_3_insert).
4. [Aktualisiert die Daten mithilfe einer Verknüpfung](#cs_4_updatejoin).
5. [Löscht Daten mithilfe einer Verknüpfung](#cs_5_deletejoin).
6. [Wählt die Datenzeilen mithilfe einer Verknüpfung](#cs_6_selectrows).
7. Schließt die Verbindung (die alle temporären Tabellen in Tempdb löscht).

Das C#-Programm enthält:

- C#-Code für die Verbindung mit der Datenbank.
- Methoden, die den T-SQL-Quellcode zurückgeben.
- Zwei Methoden, die die T-SQL-Datenbank zu übermitteln.

#### <a name="to-compile-and-run"></a>Zum Kompilieren und ausführen

Dieses c#-Programm ist logisch eine CS-Datei. Jedoch hier das Programm physisch in Codeblöcke aufgeteilt werden mehrere, um jeden Block leichter erkennen und zu verstehen. Zum Kompilieren und dieses Programm auszuführen, führen Sie folgende Schritte aus:

1. Erstellen Sie ein C#-Projekt in Visual Studio.
    - Der Projekttyp muss eine *Konsole* -Anwendung einsetzen möchten, von etwas wie die folgende Hierarchie: **Vorlagen** > **Visual C#-** > **klassische Windows-Desktop** > **Konsolen-App ((.NET Framework)**.
3. In der Datei **"Program.cs"**, löschen Sie die kleinen Starter Codezeilen.
3. Klicken Sie in "Program.cs" Kopieren Sie und fügen Sie jeweils die folgenden Datenblöcke in derselben Reihenfolge an, die sie hier dargestellt sind.
4. Bearbeiten Sie in "Program.cs" die folgenden Werte in der **Main** Methode:

   - **Cb. Datenquelle**
   - **CD. Benutzer-ID**
   - **Cb. Kennwort**
   - **"InitialCatalog"**

5. Überprüfen Sie, ob die Assembly **"System.Data.dll"** verwiesen wird. Um zu überprüfen, erweitern Sie die **Verweise** Knoten in der **Projektmappen-Explorer** Bereich.
6. Um das Programm in Visual Studio zu erstellen, klicken Sie auf die **erstellen** Menü.
7. Um das Programm in Visual Studio ausführen möchten, klicken Sie auf die **starten** Schaltfläche. Die Berichtsausgabe wird in einem cmd.exe-Fenster angezeigt.

> [!NOTE]
> Sie haben die Möglichkeit der Bearbeitung des T-SQL, ein führendes hinzuzufügen  **#**  auf den Tabellennamen erstellt die sie als temporäre Tabellen in **Tempdb**. Dies kann zu Demonstrationszwecken nützlich sein, wenn keine Testdatenbank verfügbar ist. Temporäre Tabellen werden automatisch gelöscht, wenn die Verbindung geschlossen wird. Alle Verweise für die Fremdschlüssel sind für temporäre Tabellen nicht erzwungen.
>

<a name="cs_1_connect"/>
### <a name="c-block-1-connect-by-using-adonet"></a>C#-Block 1: Herstellen einer Verbindung mithilfe von ADO.NET

- [Weiter](#cs_2_createtables)


```csharp
using System;
using System.Data.SqlClient;   // System.Data.dll 
//using System.Data;           // For:  SqlDbType , ParameterDirection

namespace csharp_db_test
{
   class Program
   {
      static void Main(string[] args)
      {
         try
         {
            var cb = new SqlConnectionStringBuilder();
            cb.DataSource = "your_server.database.windows.net";
            cb.UserID = "your_user";
            cb.Password = "your_password";
            cb.InitialCatalog = "your_database";

            using (var connection = new SqlConnection(cb.ConnectionString))
            {
               connection.Open();

               Submit_Tsql_NonQuery(connection, "2 - Create-Tables",
                  Build_2_Tsql_CreateTables());

               Submit_Tsql_NonQuery(connection, "3 - Inserts",
                  Build_3_Tsql_Inserts());

               Submit_Tsql_NonQuery(connection, "4 - Update-Join",
                  Build_4_Tsql_UpdateJoin(),
                  "@csharpParmDepartmentName", "Accounting");

               Submit_Tsql_NonQuery(connection, "5 - Delete-Join",
                  Build_5_Tsql_DeleteJoin(),
                  "@csharpParmDepartmentName", "Legal");

               Submit_6_Tsql_SelectEmployees(connection);
            }
         }
         catch (SqlException e)
         {
            Console.WriteLine(e.ToString());
         }
         Console.WriteLine("View the report output here, then press any key to end the program...");
         Console.ReadKey();
      }
```


<a name="cs_2_createtables"/>
### <a name="c-block-2-t-sql-to-create-tables"></a>C#-Block 2: T-SQL zum Erstellen von Tabellen

- [Previous](#cs_1_connect) &nbsp; / &nbsp; [Next](#cs_3_insert)

```csharp
      static string Build_2_Tsql_CreateTables()
      {
         return @"
DROP TABLE IF EXISTS tabEmployee;
DROP TABLE IF EXISTS tabDepartment;  -- Drop parent table last.


CREATE TABLE tabDepartment
(
   DepartmentCode  nchar(4)          not null
      PRIMARY KEY,
   DepartmentName  nvarchar(128)     not null
);

CREATE TABLE tabEmployee
(
   EmployeeGuid    uniqueIdentifier  not null  default NewId()
      PRIMARY KEY,
   EmployeeName    nvarchar(128)     not null,
   EmployeeLevel   int               not null,
   DepartmentCode  nchar(4)              null
      REFERENCES tabDepartment (DepartmentCode)  -- (REFERENCES would be disallowed on temporary tables.)
);
";
      }
```

#### <a name="entity-relationship-diagram-erd"></a>Entitätsbeziehungsdiagramm (ERD)

Der vorherigen CREATE TABLE-Anweisungen umfassen die **Verweise** Schlüsselwort zum Erstellen einer *Fremdschlüssel* (FS) die Beziehung zwischen zwei Tabellen.  Bei Verwendung von Tempdb kommentieren Sie Sie aus der `--REFERENCES` Schlüsselwort mit einem Paar von führenden Bindestriche enthalten.

Als Nächstes wird eine ERD, in dem die Beziehung zwischen den beiden Tabellen angezeigt. Die Werte in der #tabEmployee.DepartmentCode *untergeordneten* Spalte sind beschränkt auf die Werte in der #tabDepartment.Department *übergeordneten* Spalte.

![Fremdschlüssel für ERD anzeigen](./media/sql-database-csharp-adonet-create-query-2/erd-dept-empl-fky-2.png)


<a name="cs_3_insert"/>
### <a name="c-block-3-t-sql-to-insert-data"></a>C#-Block 3: T-SQL zum Einfügen von Daten

- [Previous](#cs_2_createtables) &nbsp; / &nbsp; [Next](#cs_4_updatejoin)


```csharp
      static string Build_3_Tsql_Inserts()
      {
         return @"
-- The company has these departments.
INSERT INTO tabDepartment
   (DepartmentCode, DepartmentName)
      VALUES
   ('acct', 'Accounting'),
   ('hres', 'Human Resources'),
   ('legl', 'Legal');

-- The company has these employees, each in one department.
INSERT INTO tabEmployee
   (EmployeeName, EmployeeLevel, DepartmentCode)
      VALUES
   ('Alison'  , 19, 'acct'),
   ('Barbara' , 17, 'hres'),
   ('Carol'   , 21, 'acct'),
   ('Deborah' , 24, 'legl'),
   ('Elle'    , 15, null);
";
      }
```


<a name="cs_4_updatejoin"/>
### <a name="c-block-4-t-sql-to-update-join"></a>C#-Block 4: T-SQL-Update-Join

- [Previous](#cs_3_insert) &nbsp; / &nbsp; [Next](#cs_5_deletejoin)


```csharp
      static string Build_4_Tsql_UpdateJoin()
      {
         return @"
DECLARE @DName1  nvarchar(128) = @csharpParmDepartmentName;  --'Accounting';


-- Promote everyone in one department (see @parm...).
UPDATE empl
   SET
      empl.EmployeeLevel += 1
   FROM
      tabEmployee   as empl
      INNER JOIN
      tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
   WHERE
      dept.DepartmentName = @DName1;
";
      }
```


<a name="cs_5_deletejoin"/>
### <a name="c-block-5-t-sql-to-delete-join"></a>C#-Block 5: T-SQL-Delete-Join

- [Previous](#cs_4_updatejoin) &nbsp; / &nbsp; [Next](#cs_6_selectrows)


```csharp
      static string Build_5_Tsql_DeleteJoin()
      {
         return @"
DECLARE @DName2  nvarchar(128);
SET @DName2 = @csharpParmDepartmentName;  --'Legal';


-- Right size the Legal department.
DELETE empl
   FROM
      tabEmployee   as empl
      INNER JOIN
      tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
   WHERE
      dept.DepartmentName = @DName2

-- Disband the Legal department.
DELETE tabDepartment
   WHERE DepartmentName = @DName2;
";
      }
```



<a name="cs_6_selectrows"/>
### <a name="c-block-6-t-sql-to-select-rows"></a>C#-Block 6: T-SQL zum Auswählen von Zeilen

- [Previous](#cs_5_deletejoin) &nbsp; / &nbsp; [Next](#cs_6b_datareader)


```csharp
      static string Build_6_Tsql_SelectEmployees()
      {
         return @"
-- Look at all the final Employees.
SELECT
      empl.EmployeeGuid,
      empl.EmployeeName,
      empl.EmployeeLevel,
      empl.DepartmentCode,
      dept.DepartmentName
   FROM
      tabEmployee   as empl
      LEFT OUTER JOIN
      tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
   ORDER BY
      EmployeeName;
";
      }
```


<a name="cs_6b_datareader"/>
### <a name="c-block-6b-executereader"></a>C#-Block 6: ExecuteReader

- [Previous](#cs_6_selectrows) &nbsp; / &nbsp; [Next](#cs_7_executenonquery)

Diese Methode ist dafür entworfen, die T-SQL SELECT-Anweisung ausführen, die erstellt wird die **Build_6_Tsql_SelectEmployees** Methode.


```csharp
      static void Submit_6_Tsql_SelectEmployees(SqlConnection connection)
      {
         Console.WriteLine();
         Console.WriteLine("=================================");
         Console.WriteLine("Now, SelectEmployees (6)...");

         string tsql = Build_6_Tsql_SelectEmployees();

         using (var command = new SqlCommand(tsql, connection))
         {
            using (SqlDataReader reader = command.ExecuteReader())
            {
               while (reader.Read())
               {
                  Console.WriteLine("{0} , {1} , {2} , {3} , {4}",
                     reader.GetGuid(0),
                     reader.GetString(1),
                     reader.GetInt32(2),
                     (reader.IsDBNull(3)) ? "NULL" : reader.GetString(3),
                     (reader.IsDBNull(4)) ? "NULL" : reader.GetString(4));
               }
            }
         }
      }
```


<a name="cs_7_executenonquery"/>
### <a name="c-block-7-executenonquery"></a>C#-Block 7: ExecuteNonQuery

- [Previous](#cs_6b_datareader) &nbsp; / &nbsp; [Next](#cs_8_output)

Diese Methode wird für Vorgänge aufgerufen werden, die den Dateninhalt der Tabellen zu ändern, ohne alle Datenzeilen zurückzugeben.


```csharp
      static void Submit_Tsql_NonQuery(
         SqlConnection connection,
         string tsqlPurpose,
         string tsqlSourceCode,
         string parameterName = null,
         string parameterValue = null
         )
      {
         Console.WriteLine();
         Console.WriteLine("=================================");
         Console.WriteLine("T-SQL to {0}...", tsqlPurpose);

         using (var command = new SqlCommand(tsqlSourceCode, connection))
         {
            if (parameterName != null)
            {
               command.Parameters.AddWithValue(  // Or, use SqlParameter class.
                  parameterName,
                  parameterValue);
            }
            int rowsAffected = command.ExecuteNonQuery();
            Console.WriteLine(rowsAffected + " = rows affected.");
         }
      }
   } // EndOfClass
}
```


<a name="cs_8_output"/>
### <a name="c-block-8-actual-test-output-to-the-console"></a>C#-Block 8: tatsächliche Test-Ausgabe in die Konsole

- [Vorherige](#cs_7_executenonquery)

In diesem Abschnitt zeichnet die Ausgabe, die das Programm an die Konsole gesendet. Der Guid-Werte unterschiedlich Testläufe ab.


```text
[C:\csharp_db_test\csharp_db_test\bin\Debug\]
>> csharp_db_test.exe

=================================
Now, CreateTables (10)...

=================================
Now, Inserts (20)...

=================================
Now, UpdateJoin (30)...
2 rows affected, by UpdateJoin.

=================================
Now, DeleteJoin (40)...

=================================
Now, SelectEmployees (50)...
0199be49-a2ed-4e35-94b7-e936acf1cd75 , Alison , 20 , acct , Accounting
f0d3d147-64cf-4420-b9f9-76e6e0a32567 , Barbara , 17 , hres , Human Resources
cf4caede-e237-42d2-b61d-72114c7e3afa , Carol , 22 , acct , Accounting
cdde7727-bcfd-4f72-a665-87199c415f8b , Elle , 15 , NULL , NULL

[C:\csharp_db_test\csharp_db_test\bin\Debug\]
>>
```
