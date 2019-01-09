---
title: Sichern einer Einzeldatenbank in einer Azure SQL-Datenbank | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Methoden und Funktionen zum Sichern einer Einzeldatenbank in Ihrer Azure SQL-Datenbank.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: tutorial
author: VanMSFT
ms.author: vanto
ms.reviewer: carlrab
manager: craigg
ms.date: 12/17/2018
ms.openlocfilehash: aea95c245b86905b7bef0a35ffaa6c5e00567111
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2018
ms.locfileid: "53558633"
---
# <a name="tutorial-secure-a-single-database-in-azure-sql-database"></a>Tutorial: Sichern einer Einzeldatenbank in einer Azure SQL-Datenbank

So sichert eine SQ-Datenbank Daten in einer einzelnen Azure SQL-Datenbank:

- Beschränkung des Zugriffs auf Ihre Datenbank mithilfe von Firewallregeln
- Verwendung von Authentifizierungsmechanismen mit Identitätsnachweis
- Autorisierung für Daten durch rollenbasierte Mitgliedschaften und Berechtigungen
- Sicherheit auf Zeilenebene
- Dynamische Datenmaskierung

Darüber hinaus verfügt SQL-Datenbank über ausgeklügelte Funktionen zur Überwachung, Überprüfung und Bedrohungserkennung.

> [!IMPORTANT]
> Die Azure SQL-Datenbank sichert eine Datenbank in einer verwalteten Instanz, indem sie Netzwerksicherheitsregeln und private Endpunkte verwendet. Weitere Informationen finden Sie unter [Verwaltete Azure SQL-Datenbank-Instanz](sql-database-managed-instance-index.yml) und [Verbindungsarchitektur der verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-connectivity-architecture.md).

Sie können den Schutz Ihrer Datenbank vor schädlichen Benutzern oder nicht autorisiertem Zugriff mit wenigen einfachen Schritten verbessern. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> - Einrichten einer Firewallregel auf Serverebene für Ihren Server im Azure-Portal
> - Einrichten von Firewallregeln auf Datenbankebene für Ihre Datenbank mithilfe von SSMS
> - Herstellen einer Verbindung mit Ihrer Datenbank mithilfe einer sicheren Verbindungszeichenfolge
> - Konfigurieren eines Azure Active Directory-Administrators für Azure SQL
> - Verwalten des Benutzerzugriffs
> - Schützen von Daten durch Verschlüsselung
> - Aktivieren der SQL-Datenbanküberwachung
> - Aktivieren der Bedrohungserkennung der SQL-Datenbank

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie dieses Tutorial durcharbeiten können, müssen folgende Voraussetzungen erfüllt sein:

- Die neueste Version von [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS) muss installiert sein.
- Microsoft Excel muss installiert sein.
- Ein Azure SQL Server und eine Azure SQL-Datenbank müssen erstellt sein. Siehe [Erstellen einer Azure SQL-Datenbank im Azure-Portal](sql-database-get-started-portal.md), [Erstellen einer einzelnen Azure SQL-Datenbank mithilfe der Azure CLI](sql-database-cli-samples.md) und [Erstellen einer einzelnen Azure SQL-Datenbank mithilfe von PowerShell](sql-database-powershell-samples.md).

> [!NOTE]
> Dieses Tutorial geht davon aus, dass Sie entweder Azure Active Directory bereits konfiguriert haben, oder dass Sie die anfänglich von Azure Active Directory verwaltete Domäne verwenden. Weitere Informationen zum Verwalten von Azure Active Directory für verschiedene Szenarien finden Sie unter [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](../active-directory/hybrid/whatis-hybrid-identity.md), [Hinzufügen eines benutzerdefinierten Domänennamens zu Azure AD](../active-directory/active-directory-domains-add-azure-portal.md), [Microsoft Azure unterstützt jetzt den Verbund mit Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Verwalten Ihres Azure AD-Verzeichnisses](../active-directory/fundamentals/active-directory-administer.md)[ sowie unter Verwalten von Azure AD mit Windows PowerShell](/powershell/azure/overview?view=azureadps-2.0) und [erforderliche Ports und Protokolle für die Hybrid-Identität](../active-directory/hybrid/reference-connect-ports.md).

## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Erstellen einer Firewallregel auf Serverebene im Azure-Portal

SQL-Datenbanken werden in Azure durch eine Firewall geschützt. Standardmäßig werden alle Verbindungen zum Server und den Datenbanken im Server abgelehnt, es sei denn, es handelt sich um Verbindungen von anderen Azure-Diensten. Weitere Informationen finden Sie unter [Firewallregeln auf Serverebene und Datenbankebene für Azure SQL-Datenbank](sql-database-firewall-configure.md).

Die sicherste Konfiguration besteht darin, „Zugriff auf Azure-Dienste erlauben“ auf „AUS“ festzulegen. Wenn Sie eine Verbindung zwischen der Datenbank und einer Azure-VM oder einem Clouddienst herstellen müssen, sollten Sie eine [reservierte IP (klassische Bereitstellung)](../virtual-network/virtual-networks-reserved-public-ip.md) erstellen und nur den Zugriff auf die reservierte IP-Adresse durch die Firewall zulassen. Bei Verwendung des [Resource Manager](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm)-Bereitstellungsmodells wird der Ressource eine dedizierte öffentliche IP-Adresse zugewiesen, für die Sie den Zugriff durch die Firewall zulassen sollten.

Führen Sie die folgenden Schritte aus, um für den Server eine [SQL-Datenbank-Firewallregel auf Serverebene](sql-database-firewall-configure.md) zu erstellen, damit Verbindungen von einer bestimmten IP-Adresse zugelassen werden.

> [!NOTE]
> Wenn Sie mithilfe eines früheren Tutorials oder Schnellstarts eine Beispieldatenbank in Azure erstellt haben und dieses Tutorial auf einem Computer mit der gleichen IP-Adresse wie beim Durcharbeiten dieser Tutorials ausführen, können Sie diesen Schritt überspringen, da Sie bereits eine Firewallregel auf Serverebene erstellt haben.

1. Klicken Sie im linken Menü auf **SQL-Datenbanken** und anschließend auf die Datenbank, für die Sie die Firewallregel auf der Seite **SQL-Datenbanken** konfigurieren möchten. Die Übersichtsseite für Ihre Datenbank wird geöffnet, die den vollqualifizierten Servernamen (z.B. **mynewserver20170313.database.windows.net**) und Optionen für die weitere Konfiguration enthält.

      ![Serverfirewallregel](./media/sql-database-security-tutorial/server-firewall-rule.png)

2. Klicken Sie auf der Symbolleiste wie in der obigen Abbildung dargestellt auf **Serverfirewall festlegen**. Die Seite **Firewalleinstellungen** für den SQL-Datenbankserver wird geöffnet.

3. Klicken Sie auf der Symbolleiste auf **Client-IP hinzufügen**, um die öffentliche IP-Adresse des mit dem Portal verbundenen Computers hinzuzufügen, oder geben Sie die Firewallregel manuell ein. Klicken Sie anschließend auf **Speichern**.

      ![Festlegen der Serverfirewallregel](./media/sql-database-security-tutorial/server-firewall-rule-set.png)

4. Klicken Sie auf **OK** und dann auf das **X**, um die Seite mit den **Firewalleinstellungen** zu schließen.

Sie können nun eine Verbindung zu einer beliebigen Datenbank auf dem Server mit der angegebenen IP-Adresse oder dem angegebenen IP-Adressbereich herstellen.

> [!NOTE]
> SQL-Datenbank kommuniziert über Port 1433. Wenn Sie versuchen, eine Verbindung aus einem Unternehmensnetzwerk heraus herzustellen, wird der ausgehende Datenverkehr über Port 1433 von der Firewall Ihres Netzwerks unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Azure SQL-Datenbankserver herstellen, wenn Ihre IT-Abteilung Port 1433 öffnet.
>

## <a name="create-a-database-level-firewall-rule-using-ssms"></a>Erstellen einer Firewallregel auf Datenbankebene mithilfe von SSMS

Mit Firewallregeln auf Datenbankebene können Sie unterschiedliche Einstellungen für verschiedene Datenbanken auf demselben logischen Server erstellen, und Sie können portable Firewallregeln erstellen, d.h., sie folgen der Datenbank während eines Failovers, und werden nicht auf dem SQL Server gespeichert. Firewallregeln auf Datenbankebene können nur mithilfe von Transact-SQL-Anweisungen und erst dann konfiguriert werden, nachdem Sie die erste Firewallregel auf Serverebene konfiguriert haben. Weitere Informationen finden Sie unter [Firewallregeln auf Serverebene und Datenbankebene für Azure SQL-Datenbank](sql-database-firewall-configure.md).

Führen Sie folgende Schritte aus, um eine datenbankspezifische Firewallregel zu erstellen.

1. Stellen Sie eine Verbindung mit Ihrer Datenbank her, z.B. mithilfe von [SQL Server Management Studio](./sql-database-connect-query-ssms.md).

2. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf die Datenbank, der eine Firewallregel hinzugefügt werden soll. Klicken Sie dann auf **Neue Abfrage**. Ein leeres Abfragefenster mit einer Verbindung mit Ihrer Datenbank wird geöffnet.

3. Ändern Sie im Abfragefenster die IP-Adresse in Ihre öffentliche IP-Adresse, und führen Sie anschließend die folgende Abfrage aus:

    ```sql
    EXECUTE sp_set_database_firewall_rule N'Example DB Rule','0.0.0.4','0.0.0.4';
    ```

4. Klicken Sie auf der Symbolleiste auf **Ausführen**, um die Firewallregel zu erstellen.

## <a name="view-how-to-connect-an-application-to-your-database-using-a-secure-connection-string"></a>Herstellen der Verbindung einer Anwendung mit Ihrer Datenbank mithilfe einer sicheren Verbindungszeichenfolge

Um eine sichere, verschlüsselte Verbindung zwischen einer Clientanwendung und einer SQL-Datenbank zu gewährleisten, muss die Verbindungszeichenfolge wie folgt konfiguriert werden:

- Es muss eine verschlüsselte Verbindung angefordert werden, und
- das Serverzertifikat wird als nicht vertrauenswürdig angesehen.

Hierdurch wird eine Verbindung mithilfe von Transport Layer Security (TLS) hergestellt und das Risiko von Man-in-the-Middle-Angriffen verringert. Wie für ADO.NET in diesem Screenshot dargestellt, können Sie korrekt konfigurierte Verbindungszeichenfolgen für Ihre SQL-Datenbank für unterstützte Clienttreiber aus dem Azure-Portal abrufen. Informationen zu TLS und zur Konnektivität finden Sie unter [Überlegungen zu TLS](sql-database-connect-query.md#tls-considerations-for-sql-database-connectivity).

1. Wählen Sie im Menü auf der linken Seite die Option **SQL-Datenbanken**, und klicken Sie auf der Seite **SQL-Datenbanken** auf Ihre Datenbank.

2. Klicken Sie auf der Seite **Überblick** Ihrer Datenbank auf **Datenbank-Verbindungszeichenfolgen anzeigen**.

3. Überprüfen Sie die vollständige **ADO.NET**-Verbindungszeichenfolge.

    ![ADO.NET-Verbindungszeichenfolge](./media/sql-database-security-tutorial/adonet-connection-string.png)

## <a name="provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server"></a>Bereitstellen eines Azure Active Directory-Administrators für Ihren Azure SQL-Datenbank-Server

Stellen Sie einen Azure Active Directory-Administrator für Ihre Azure SQL Server-Instanz unter Verwendung des Azure-Portals her.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) in der oberen rechten Ecke Ihre Verbindung aus, um eine Dropdownliste mit möglichen Active Directory-Verzeichnissen zu öffnen. Wählen Sie das richtige Active Directory-Verzeichnis als Standardeinstellung für Azure AD aus. Durch diesen Schritt wird die mit dem Abonnement verbundene Active Directory-Instanz mit der Azure SQL Server-Instanz verknüpft. So wird sichergestellt, dass das gleiche Abonnement sowohl für Azure AD als auch für SQL Server verwendet wird. (Die Azure SQL Server-Instanz kann entweder Azure SQL-Datenbank oder Azure SQL Data Warehouse hosten.)

    ![Administrator auswählen](./media/sql-database-aad-authentication/8choose-ad.png)

2. Wählen Sie auf der Seite **SQL Server** **Active Directory-Administrator**, und wählen Sie auf der Seite **Active Directory-Administrator** **Administrator festlegen**.  ![Active Directory auswählen](./media/sql-database-aad-authentication/select-active-directory.png)  

   > [!IMPORTANT]
   > Sie müssen „Unternehmensadministrator“ oder „Globaler Administrator“ sein, um diese Aufgabe ausführen zu können.

3. Suchen Sie auf der Seite **Administrator hinzufügen** nach einem Benutzer, wählen Sie den Benutzer oder die Gruppe aus, den bzw. die Sie als Administrator festlegen möchten, und wählen Sie dann **Auswählen** aus. Auf der Seite „Active Directory-Administrator“ werden alle Mitglieder und Gruppen in Ihrem Active Directory-Verzeichnis angezeigt. (Auf dem Blatt „Active Directory-Administrator“ werden alle Mitglieder und Gruppen in Ihrem Active Directory-Verzeichnis angezeigt. (Siehe Liste mit unterstützten Administratoren im Abschnitt **Funktionen und Einschränkungen von Azure AD** von [Verwenden der Azure Active Directory-Authentifizierung für die Authentifizierung mit SQL-Datenbank oder SQL Data Warehouse](sql-database-aad-authentication.md).) Die rollenbasierte Zugriffskontrolle (Role-based Access Control, RBAC) gilt nur für das Portal und wird nicht an SQL Server weitergegeben.
    ![Administrator auswählen](./media/sql-database-aad-authentication/select-admin.png)  

4. Wählen Sie oben auf der Seite **Active Directory-Administrator** **SPEICHERN** aus.
    ![Administrator speichern](./media/sql-database-aad-authentication/save-admin.png)

Der Vorgang zum Ändern des Administrators kann einige Minuten in Anspruch nehmen. Anschließend wird der neue Administrator im Feld **Active Directory-Administrator** angezeigt.

   > [!NOTE]
   > Beim Einrichten des Azure AD-Administrators darf der Name des neuen Administrators (Benutzer oder Gruppe) in der virtuellen Masterdatenbank noch nicht als SQL Server-Authentifizierungsbenutzer vorhanden sein. Wenn er vorhanden ist, schlägt die Einrichtung des Azure AD-Administrators fehl, wobei die Erstellung rückgängig gemacht und angegeben wird, dass ein solcher Administrator (Name) bereits vorhanden ist. Da dieser SQL Server-Authentifizierungsbenutzer nicht Teil von Azure AD ist, sind alle Versuche erfolglos, unter der Azure AD-Authentifizierung eine Verbindung mit dem Server herzustellen.

## <a name="creating-database-users"></a>Erstellen von Datenbankbenutzern

Vor der Erstellung von Benutzern müssen Sie zuerst einen der beiden Authentifizierungstypen wählen, die von der Azure SQL-Datenbank unterstützt werden:

**SQL-Authentifizierung**, bei der Benutzername und Kennwort für Anmeldungen und Benutzer verwendet werden, die nur im Kontext einer bestimmten Datenbank auf einem logischen Server gültig sind.

**Azure Active Directory-Authentifizierung**, bei der von Azure Active Directory verwaltete Identitäten verwendet werden.

### <a name="create-a-user-using-sql-authentication"></a>Erstellen eines Benutzers mit SQL-Authentifizierung

Führen Sie die folgenden Schritte aus, um einen Benutzer mit SQL-Authentifizierung zu erstellen:

1. Stellen Sie eine Verbindung mit Ihrer Datenbank her, z.B. mit [SQL Server Management Studio](./sql-database-connect-query-ssms.md), indem Sie Ihre Anmeldeinformationen für die Serveranmeldung verwenden.

2. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf die Datenbank, der ein neuer Benutzer hinzugefügt werden soll. Klicken Sie dann auf **Neue Abfrage**. Ein leeres Abfragefenster, das mit der ausgewählten Datenbank verbunden ist, wird geöffnet.

3. Geben Sie im Abfragefenster die folgende Abfrage ein:

    ```sql
    CREATE USER ApplicationUser WITH PASSWORD = 'YourStrongPassword1';
    ```

4. Klicken Sie auf der Symbolleiste auf **Ausführen**, um den Benutzer zu erstellen.

5. Standardmäßig kann der Benutzer eine Verbindung zur Datenbank herstellen, besitzt jedoch keine Berechtigungen zum Lesen oder Schreiben von Daten. Um dem neu erstellten Benutzer diese Berechtigungen zu gewähren, führen Sie die folgenden zwei Befehle in einem neuen Abfragefenster aus.

    ```sql
    ALTER ROLE db_datareader ADD MEMBER ApplicationUser;
    ALTER ROLE db_datawriter ADD MEMBER ApplicationUser;
    ```

Es wird empfohlen, diese Nichtadministratorkonten für die Verbindung mit Ihrer Datenbank auf Datenbankebene zu erstellen, es sei denn, Sie müssen Administratoraufgaben wie die Erstellung von neuen Benutzern ausführen. Informationen zur Authentifizierung mit Azure Active Directory finden Sie im [Azure Active Directory-Tutorial](./sql-database-aad-authentication-configure.md).

### <a name="create-a-user-using-azure-active-directory-authentication"></a>Erstellen eines Benutzers mit Azure Active Directory-Authentifizierung

Für die Azure Active Directory-Authentifizierung ist es erforderlich, dass Datenbankbenutzer als eigenständige Datenbankbenutzer erstellt werden. Ein eigenständiger Datenbankbenutzer basierend auf einer Azure AD-Identität ist ein Datenbankbenutzer, der über keine Anmeldung für die Masterdatenbank verfügt, und der einer Identität im Azure AD-Verzeichnis zugeordnet ist, das mit der Datenbank verknüpft ist. Bei der Azure AD-Identität kann es sich entweder um ein einzelnes Benutzerkonto oder um eine Gruppe handeln. Weitere Informationen zu eigenständigen Datenbankbenutzern finden Sie unter [Eigenständige Datenbankbenutzer – machen Sie Ihre Datenbank portabel](https://msdn.microsoft.com/library/ff929188.aspx).

> [!NOTE]
> Datenbankbenutzer (mit Ausnahme von Administratoren) können nicht im Azure-Portal erstellt werden. RBAC-Rollen werden nicht auf SQL Server, SQL-Datenbank oder SQL Data Warehouse übertragen. Azure RBAC-Rollen dienen zum Verwalten von Azure-Ressourcen und gelten nicht für Datenbankberechtigungen. Beispielsweise gewährt die Rolle **SQL Server-Mitwirkender** keinen Zugriff zum Herstellen einer Verbindung mit der SQL-Datenbank oder SQL Data Warehouse. Die Zugriffsberechtigung muss direkt in der Datenbank mithilfe von Transact-SQL-Anweisungen erteilt werden.
> [!WARNING]
> Sonderzeichen wie Doppelpunkt `:` oder das Und-Zeichen `&`, die als Benutzernamen in den Anweisungen „T-SQL CREATE LOGIN“ und „CREATE USER“ enthalten sind, werden nicht unterstützt.

1. Verbinden Sie sich mit Ihrem Azure SQL Server über ein Azure Active Directory-Konto mit mindestens der Berechtigung **ALTER ANY USER**.
2. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf die Datenbank, der ein neuer Benutzer hinzugefügt werden soll. Klicken Sie dann auf **Neue Abfrage**. Ein leeres Abfragefenster, das mit der ausgewählten Datenbank verbunden ist, wird geöffnet.

3. Geben Sie im Abfragefenster die folgende Abfrage ein und ändern Sie `<Azure_AD_principal_name>` auf den gewünschten Benutzerprinzipalnamen eines Azure AD-Benutzers oder den Anzeigenamen für eine Azure AD-Gruppe:

   ```sql
   CREATE USER <Azure_AD_principal_name> FROM EXTERNAL PROVIDER;
   ```

   > [!NOTE]
   > Azure AD-Benutzer werden in den Datenbankmetadaten mit dem Typ E (EXTERNAL_USER) gekennzeichnet, Gruppen mit dem Typ X (EXTERNAL_GROUPS). Weitere Informationen finden Sie unter [sys.database_principals](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-principals-transact-sql).

## <a name="protect-your-data-with-encryption"></a>Schützen von Daten durch Verschlüsselung

Die TDE-Funktion (Transparent Data Encryption) der Azure SQL-Datenbank verschlüsselt automatisch Ihre ruhenden Daten, ohne Änderungen an der Anwendung, die auf die verschlüsselte Datenbank zugreift, vorzunehmen. Für neu erstellte Datenbanken ist TDE standardmäßig aktiviert. Führen Sie die folgenden Schritte aus, um TDE für Ihre Datenbank zu aktivieren bzw. um sich zu vergewissern, dass TDE aktiviert ist:

1. Wählen Sie im Menü auf der linken Seite die Option **SQL-Datenbanken**, und klicken Sie auf der Seite **SQL-Datenbanken** auf Ihre Datenbank.

2. Klicken Sie auf **Transparent Data Encryption**, um die Konfigurationsseite für TDE zu öffnen.

    ![Transparent Data Encryption](./media/sql-database-security-tutorial/transparent-data-encryption-enabled.png)

3. Legen Sie **Datenverschlüsselung** auf EIN fest, und klicken Sie auf **Speichern**.

Der Verschlüsselungsvorgang wird im Hintergrund gestartet. Sie können den Fortschritt überwachen, indem Sie beispielsweise unter Verwendung von [SQL Server Management Studio](./sql-database-connect-query-ssms.md) eine Verbindung mit SQL-Datenbank herstellen und die Spalte „encryption_state“ der Ansicht [sys.dm_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql?view=sql-server-2017) abfragen. Der Zustand „3“ gibt an, dass die Datenbank verschlüsselt ist.

## <a name="enable-sql-database-auditing-if-necessary"></a>Aktivieren der SQL-Datenbanküberwachung (falls erforderlich)

Die SQL-Datenbank-Überprüfung in Azure verfolgt Datenbankereignisse und schreibt diese in ein Überwachungsprotokoll in Ihrem Azure-Speicherkonto. Die Überwachung kann Ihnen dabei helfen, die gesetzlichen Bestimmungen einzuhalten, die Datenbankaktivität zu verstehen und Einblicke in Abweichungen und Anomalien zu erhalten, die Hinweise auf potenzielle Sicherheitsverstöße darstellen können. Führen Sie folgende Schritte aus, um eine standardmäßige Überwachungsrichtlinie für Ihre SQL-Datenbank zu erstellen:

1. Wählen Sie im Menü auf der linken Seite die Option **SQL-Datenbanken**, und klicken Sie auf der Seite **SQL-Datenbanken** auf Ihre Datenbank.

2. Wählen Sie auf dem Blatt „Einstellungen“ die Option **Überwachung und Bedrohungserkennung**. Beachten Sie Folgendes: Die Überwachung auf Serverebene ist deaktiviert, und ein Link **Servereinstellungen anzeigen** ist vorhanden, über den Sie die Überwachungseinstellungen des Servers für diesen Kontext anzeigen oder ändern können.

    ![Blatt „Überwachung“](./media/sql-database-security-tutorial/auditing-get-started-settings.png)

3. Wenn Sie lieber einen Überwachungstyp (oder ggf. Überwachungsort) aktivieren möchten, der sich von der Angabe auf Serverebene unterscheidet, gehen Sie wie folgt vor: Legen Sie die Überwachung auf **EIN** fest, und wählen Sie den Überwachungstyp **Blob** aus. Wenn die Serverblobüberwachung aktiviert ist, existiert die konfigurierte Datenbanküberwachung parallel zur Serverblobüberwachung.

    ![Aktivieren der Überwachung](./media/sql-database-security-tutorial/auditing-get-started-turn-on.png)

4. Wählen Sie **Speicherdetails** , um das Blatt „Speicherung von Überwachungsprotokollen“ zu öffnen. Wählen Sie das Azure-Speicherkonto aus, in dem die Protokolle gespeichert werden sollen, sowie die Aufbewahrungsdauer, nach der die alten Protokolle gelöscht werden. Klicken Sie anschließend unten auf **OK**.

   > [!TIP]
   > Verwenden Sie für alle überwachten Datenbanken dasselbe Speicherkonto, um die Berichtvorlagen für die Überwachung optimal einzusetzen.

5. Klicken Sie auf **Speichern**.

> [!IMPORTANT]
> Wenn Sie die überwachten Ereignisse anpassen möchten, können Sie hierfür PowerShell oder die REST-API verwenden. Weitere Informationen finden Sie unter [Erste Schritte bei der Überwachung von SQL-Datenbank](sql-database-auditing.md).
>

## <a name="enable-sql-database-threat-detection"></a>Aktivieren der Bedrohungserkennung der SQL-Datenbank

Die Bedrohungserkennung bietet eine neue Sicherheitsebene und ermöglicht es den Kunden, auf erkannte potenzielle Bedrohungen zu reagieren. Zu diesem Zweck werden Sicherheitshinweise zu anormalen Aktivitäten bereitgestellt. Benutzer können verdächtige Ereignisse mithilfe der SQL-Datenbanküberwachung untersuchen, um zu ermitteln, ob sie auf einen Zugriffsversuch, die Verletzung der Datensicherheit oder den Missbrauch von Daten in der Datenbank zurückzuführen sind. Die Bedrohungserkennung vereinfacht den Umgang mit potenziellen Bedrohungen für die Datenbank, ohne das Fachwissen eines Sicherheitsexperten besitzen oder komplexe Sicherheitsüberwachungssysteme verwalten zu müssen.
So erkennt die Bedrohungserkennung beispielsweise bestimmte anormale Datenbankaktivitäten, die auf eine potenzielle Einschleusung von SQL-Befehlen hindeuten. Die Einschleusung von SQL-Befehlen ist ein verbreitetes Sicherheitsproblem für Webanwendungen im Internet und wird für Angriffe auf datengesteuerte Anwendungen verwendet. Die Angreifer nutzen Sicherheitslücken der Anwendung, um böswillige SQL-Anweisungen in Eingabefelder der Anwendung einzuschleusen und so an die in der Datenbank enthaltenen Daten zu gelangen oder diese zu verändern.

1. Navigieren Sie zum Konfigurationsblatt der SQL-Datenbank, die Sie überwachen möchten. Wählen Sie auf dem Blatt „Einstellungen“ die Option **Überwachung und Bedrohungserkennung**.

    ![Navigationsbereich](./media/sql-database-security-tutorial/auditing-get-started-settings.png)
2. Aktivieren Sie die **Überwachung auf dem Blatt für die Konfiguration** der **Überwachung und Bedrohungserkennung**. Daraufhin werden die Einstellungen für die Bedrohungserkennung angezeigt.

3. Aktivieren **Sie** die Bedrohungserkennung.

4. Konfigurieren Sie die Liste der E-Mail-Empfänger, die bei Erkennung anormaler Datenbankaktivitäten einen Sicherheitshinweis erhalten sollen.

5. Klicken Sie auf dem Blatt **Überwachung und Bedrohungserkennung** auf **Speichern**, um die neue oder aktualisierte Richtlinie für die Überwachung und Bedrohungserkennung zu speichern.

    ![Navigationsbereich](./media/sql-database-security-tutorial/td-turn-on-threat-detection.png)

    Bei Erkennung anormaler Datenbankaktivitäten erhalten Sie eine E-Mail-Benachrichtigung.  Die E-Mail enthält Informationen zum verdächtigen Sicherheitsereignis (wie etwa Art der anomalen Aktivitäten, Datenbankname, Servername und Zeit des Ereignisses). Darüber hinaus enthält sie Angaben zu möglichen Ursachen und empfohlenen Maßnahmen zur Untersuchung und Abwehr der potenziellen Bedrohung für die Datenbank. In den nächsten Schritten erfahren Sie, was Sie tun sollten, falls Sie eine solche E-Mail erhalten sollten:

    ![Bedrohungserkennungs-E-Mail](./media/sql-database-threat-detection-get-started/4_td_email.png)

6. Klicken Sie in der E-Mail auf den Link für das **Azure SQL-Überwachungsprotokoll**, um das Azure-Portal aufzurufen und die relevanten Überwachungsdatensätze für die Zeit des verdächtigen Ereignisses anzuzeigen.

    ![Überwachungsdatensätze](./media/sql-database-threat-detection-get-started/5_td_audit_records.png)

7. Klicken Sie auf die Überwachungsdatensätze, um weitere Informationen zu den verdächtigen Datenbankaktivitäten (z.B. SQL-Anweisung, Fehlerursache und Client-IP) anzuzeigen.

    ![Datensatzdetails](./media/sql-database-security-tutorial/6_td_audit_record_details.png)

8. Klicken Sie auf dem Blatt mit den Überwachungsdatensätzen auf **In Excel öffnen**, um eine vorkonfigurierte Excel-Vorlage zu öffnen, das Überwachungsprotokoll zu importieren und eine ausführlichere Analyse für die Zeit des verdächtigen Ereignisses durchzuführen.

    > [!NOTE]
    > Ab Excel 2010 werden Power Query und die Einstellung **Schnelles Kombinieren** benötigt.

    ![Öffnen von Datensätzen in Excel](./media/sql-database-threat-detection-get-started/7_td_audit_records_open_excel.png)

9. So konfigurieren Sie die Einstellung **Schnelles Kombinieren**: Klicken Sie auf der Registerkarte **POWER QUERY** des Menübands auf **Optionen**, um das gleichnamige Dialogfeld anzuzeigen. Navigieren Sie zum Datenschutzabschnitt, und wählen Sie die zweite Option zum Ignorieren der Sicherheitsstufen und zur potenziellen Verbesserung der Leistung:

    ![Excel-Einstellung „Schnelles Kombinieren“](./media/sql-database-threat-detection-get-started/8_td_excel_fast_combine.png)

10. Wenn Sie SQL-Überwachungsprotokolle laden möchten, vergewissern Sie sich, dass die Parameter auf der Einstellungsregisterkarte ordnungsgemäß festgelegt sind. Klicken Sie dann im Menüband auf „Daten“ und anschließend auf die Schaltfläche „Alle aktualisieren“.

    ![Excel-Parameter](./media/sql-database-threat-detection-get-started/9_td_excel_parameters.png)

11. Die Ergebnisse werden auf dem Blatt mit den **SQL-Überwachungsprotokollen** angezeigt. Dadurch können Sie die erkannten anomalen Aktivitäten eingehender untersuchen und die Auswirkungen des Sicherheitsereignisses in Ihrer Anwendung begrenzen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie den Schutz Ihrer Datenbank vor schädlichen Benutzern oder nicht autorisiertem Zugriff mit wenigen einfachen Schritten verbessern.  Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> - Einrichten von Firewallregeln für den Server und/oder die Datenbank
> - Herstellen einer Verbindung mit Ihrer Datenbank mithilfe einer sicheren Verbindungszeichenfolge
> - Konfigurieren eines Azure Active Directory-Administrators für Azure SQL
> - Verwalten des Benutzerzugriffs
> - Schützen von Daten durch Verschlüsselung
> - Aktivieren der SQL-Datenbanküberwachung
> - Aktivieren der Bedrohungserkennung der SQL-Datenbank

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie eine geografisch verteilte Datenbank schützen.

> [!div class="nextstepaction"]
>[Implementieren einer geografisch verteilten Datenbank](sql-database-implement-geo-distributed-database.md)
