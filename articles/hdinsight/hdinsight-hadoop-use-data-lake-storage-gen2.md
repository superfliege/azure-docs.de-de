---
title: Verwenden von Azure Data Lake Storage Gen2 mit Azure HDInsight-Clustern
description: Erfahren Sie, wie Sie Azure Data Lake Storage Gen2 mit Azure HDInsight-Clustern verwenden.
services: hdinsight
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: howto
ms.date: 01/10/2019
ms.author: hrasheed
ms.openlocfilehash: fae92f8e09cc2ad6b63cb15599e0b1ab72588ed8
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2019
ms.locfileid: "55728842"
---
# <a name="use-azure-data-lake-storage-gen2-with-azure-hdinsight-clusters"></a>Verwenden von Azure Data Lake Storage Gen2 mit Azure HDInsight-Clustern

Azure Data Lake Storage (Data Lake Storage) Gen2 baut auf Azure Blob Storage auf und bietet eine Reihe von Funktionen für die Big Data-Analyse. Data Lake Storage Gen2 ist das Ergebnis der Kombination der Funktionen von Azure Blob Storage und Azure Data Lake Storage Gen1. Daraus resultiert ein Dienst, der Features von Azure Data Lake Storage Gen1 (z.B. Dateisystemsemantik, Sicherheit auf Verzeichnis- und Dateiebene und Skalierbarkeit) mit den kostengünstigen, mehrstufigen Speicherlösungen, Hochverfügbarkeits- und Notfallwiederherstellungsfunktionen von Azure Blob Storage kombiniert.

## <a name="data-lake-storage-gen2-availability"></a>Verfügbarkeit von Data Lake Storage Gen2

Azure Data Lake Storage Gen2 ist als Speicheroption für fast alle Typen von Azure HDInsight-Clustern sowohl als Standard- als auch zusätzliches Speicherkonto verfügbar. HBase kann jedoch nur ein Data Lake Storage Gen2-Konto verwenden.

> [!Note] 
> Nachdem Sie Data Lake Storage Gen2 als **primären Speichertyp** ausgewählt haben, können Sie kein Data Lake Storage Gen1-Konto als zusätzlichen Speicher auswählen.

## <a name="creating-an-hdinsight-cluster-with-data-lake-storage-gen2"></a>Erstellen eines HDInsight-Clusters mit Data Lake Storage Gen2

## <a name="using-the-azure-portal"></a>Verwenden des Azure-Portals

Um einen HDInsight-Cluster zu erstellen, der Data Lake Storage Gen2 als Speicher verwendet, erstellen Sie mithilfe der folgenden Schritte ein ordnungsgemäß konfiguriertes Data Lake Storage Gen2-Konto.

1. Erstellen Sie eine benutzerseitig zugewiesene verwaltete Identität, falls noch keine vorhanden ist. Informationen hierzu finden Sie unter [Erstellen, Auflisten, Löschen oder Zuweisen einer Rolle zu einer benutzerseitig zugewiesenen verwalteten Identität über das Azure-Portal](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity).

    ![Erstellen einer benutzerseitig zugewiesenen verwalteten Identität](./media/hdinsight-hadoop-data-lake-storage-gen2/create-user-assigned-managed-identity-portal.png)

1. Erstellen Sie ein Azure Data Lake Storage Gen2-Speicherkonto. Stellen Sie sicher, dass die Option **Hierarchisches Dateisystem** aktiviert ist. Weitere Informationen finden Sie unter [Schnellstart: Erstellen eines Azure Data Lake Storage Gen2-Speicherkontos](../storage/blobs/data-lake-storage-quickstart-create-account.md).

    ![Screenshot zur Speicherkontoerstellung im Azure-Portal](./media/hdinsight-hadoop-data-lake-storage-gen2/azure-data-lake-storage-account-create-advanced.png)
 
1. Weisen Sie die verwaltete Identität der Rolle **Besitzer von Speicherblobdaten (Vorschau)** im Speicherkonto zu. Informationen finden Sie unter [Verwalten von Zugriffsrechten für Blob- und Warteschlangendaten von Azure mit RBAC (Vorschau)](../storage/common/storage-auth-aad-rbac.md#assign-a-role-scoped-to-the-storage-account-in-the-azure-portal).

    1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Speicherkonto.
    1. Wählen Sie Ihr Speicherkonto und dann **Zugriffssteuerung (IAM)** aus, um Zugriffssteuerungseinstellungen für das Konto anzuzeigen. Wählen Sie die Registerkarte **Rollenzuweisungen** aus, um die Liste mit den Rollenzuweisungen anzuzeigen.
    
        ![Screenshot mit Anzeige der Zugriffssteuerungseinstellungen für Speicher](./media/hdinsight-hadoop-data-lake-storage-gen2/portal-access-control.png)
    
    1. Klicken Sie auf die Schaltfläche **Rollenzuweisung hinzufügen**, um eine neue Rolle hinzuzufügen.
    1. Wählen Sie im Fenster **Rollenzuweisung hinzufügen** die Rolle **Besitzer von Speicherblobdaten (Vorschau)** aus. Wählen Sie dann das Abonnement aus, das über die verwaltete Identität und das Speicherkonto verfügt. Suchen Sie als Nächstes die vom Benutzer zugewiesene verwaltete Identität, die Sie zuvor erstellt haben. Wählen Sie abschließend die verwaltete Identität aus, sodass sie unter **Ausgewählte Mitglieder** aufgelistet wird.
    
        ![Screenshot: Zuweisen einer RBAC-Rolle](./media/hdinsight-hadoop-data-lake-storage-gen2/add-rbac-role3.png)
    
    1. Klicken Sie auf **Speichern**. Die vom Benutzer zugewiesene Identität, die Sie ausgewählt haben, wird jetzt unter der Rolle **Mitwirkender** aufgelistet.

    1. Nachdem das anfängliche Setup abgeschlossen ist, können Sie einen Cluster über das Portal erstellen. Der Cluster muss sich in der gleichen Azure-Region befinden wie das Speicherkonto. Wählen Sie im Abschnitt **Speicher** des Menüs zur Clustererstellung die folgenden Optionen aus:
        
        * Klicken Sie für **Primärer Speichertyp** auf **Azure Data Lake Storage Gen2**.
        * Suchen Sie unter **Speicherkonto auswählen** das neu erstellte Data Lake Storage Gen2-Speicherkonto, und wählen Sie es aus.
        
            ![Speichereinstellungen für das Verwenden von Data Lake Storage Gen2 mit Azure HDInsight](./media/hdinsight-hadoop-data-lake-storage-gen2/primary-storage-type-adls-gen2.png)
        
        * Wählen Sie unter **Identität** das richtige Abonnement und die neu erstellte vom Benutzer zugewiesene verwaltete Identität aus.
        
            ![Identitätsstellungen für das Verwenden von Data Lake Storage Gen2 mit Azure HDInsight](./media/hdinsight-hadoop-data-lake-storage-gen2/managed-identity-cluster-creation.png)

### <a name="using-a-resource-manager-template-deployed-with-azure-cli"></a>Verwenden einer mit der Azure CLI bereitgestellten Resource Manager-Vorlage

Sie können [hier eine Beispielvorlagendatei](https://github.com/Azure-Samples/hdinsight-data-lake-storage-gen2-templates/blob/master/hdinsight-adls-gen2-template.json) und [hier eine Beispieldatei für Parameter](https://github.com/Azure-Samples/hdinsight-data-lake-storage-gen2-templates/blob/master/parameters.json) herunterladen. Bevor Sie die Vorlage verwenden, ersetzen Sie die Zeichenfolge `<SUBSCRIPTION_ID>` durch die tatsächliche ID Ihres Azure-Abonnements. Ersetzen Sie außerdem die Zeichenfolge `<PASSWORD>` durch Ihr ausgewähltes Kennwort, um sowohl das Kennwort für die Anmeldung bei Ihrem Cluster als auch das SSH-Kennwort festzulegen.

Der nachstehende Codeausschnitt zeigt die Ausführung der folgenden ersten Schritte:

1. Melden Sie sich bei Ihrem Azure-Konto an.
1. Legen Sie das aktive Abonnement fest, in dem die Erstellungsvorgänge ausgeführt werden.
1. Erstellen Sie eine neue Ressourcengruppe für die neuen Bereitstellungsaktivitäten `hdinsight-deployment-rg`.
1. Erstellen Sie eine benutzerseitig verwaltete Dienstidentität `test-hdinsight-msi`.
1. Fügen Sie der Azure CLI eine Erweiterung zu, um Features für Data Lake Storage Gen2 zu nutzen.
1. Erstellen Sie ein neues Data Lake Storage Gen2-Konto `hdinsightadlsgen2`, indem Sie das Flag `--hierarchical-namespace true` verwenden.

```azurecli
az login
az account set --subscription <subscription_id>

#create resource group
az group create --name hdinsight-deployment-rg --location eastus

# Create managed identity
az identity create -g hdinsight-deployment-rg -n test-hdinsight-msi

az extension add --name storage-preview

az storage account create --name hdinsightadlsgen2 \
    --resource-group hdinsight-deployment-rg \
    --location eastus --sku Standard_LRS \
    --kind StorageV2 --hierarchical-namespace true
```

Melden Sie sich anschließend beim Portal an, und fügen Sie die neue verwaltete Dienstidentität der Rolle **Mitwirkender an Storage-Blobdaten (Vorschau)** für das Speicherkonto zu, wie in Schritt 3 oben unter [Verwenden des Azure-Portals](hdinsight-hadoop-use-data-lake-storage-gen2.md#using-the-azure-portal) beschrieben.

Nachdem die verwaltete Dienstidentität im Portal hinzugefügt wurde, fahren Sie mit dem Bereitstellen der Vorlage unter Verwendung des nachstehenden Codeausschnitts fort.

```azurecli
az group deployment create --name HDInsightADLSGen2Deployment \
    --resource-group hdinsight-deployment-rg \
    --template-file hdinsight-adls-gen2-template.json \
    --parameters parameters.json
```

## <a name="access-control-for-data-lake-storage-gen2-in-hdinsight"></a>Zugriffssteuerung für Data Lake Storage Gen2 in HDInsight

### <a name="what-kinds-of-permissions-does-data-lake-storage-gen2-support"></a>Welche Arten von Berechtigungen unterstützt Data Lake Storage Gen2?

Azure Data Lake Storage Gen2 implementiert ein Zugriffssteuerungsmodell, das sowohl die rollenbasierte Zugriffssteuerung (Role Based Access Control, RBAC) in Azure als auch POSIX-Zugriffssteuerungslisten (Access Control Lists, ACLs) unterstützt. Data Lake Storage Gen1 unterstützt nur Zugriffssteuerungslisten zum Steuern des Zugriffs auf Daten.

Für die rollenbasierte Zugriffssteuerung in Azure werden Rollenzuweisungen verwendet, um Benutzern, Gruppen und Dienstprinzipalen Berechtigungssätze für Azure-Ressourcen effektiv zuzuweisen. In der Regel sind diese Azure-Ressourcen auf Ressourcen auf oberster Ebene (z.B. Azure Storage-Konten) beschränkt. Für Azure Storage und damit auch Azure Data Lake Storage Gen2 wurde dieser Mechanismus auf die Dateisystemressource ausgeweitet.

 Weitere Informationen zu Dateiberechtigungen mit RBAC finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure (RBAC)](../storage/blobs/data-lake-storage-access-control.md#azure-role-based-access-control-rbac).

Weitere Informationen zu Dateiberechtigungen mit ACLs finden Sie unter [Zugriffssteuerungslisten für Dateien und Verzeichnisse](../storage/blobs/data-lake-storage-access-control.md#access-control-lists-on-files-and-directories).


### <a name="how-do-i-control-access-to-my-data-in-gen2"></a>Wie steuere ich in Gen2 den Zugriff auf meine Daten?

In Data Lake Storage Gen2 steuern verwaltete Identitäten den Zugriff auf Dateien durch Ihren HDInsight-Cluster. Eine verwaltete Identität ist eine in Azure AD registrierte Identität, deren Anmeldeinformationen von Azure verwaltet werden. Sie müssen keine Dienstprinzipale in Azure AD registrieren und keine Anmeldeinformationen wie z.B. Zertifikate verwalten.

Es gibt zwei Arten von verwalteten Identitäten für Azure-Dienste: durch das System zugewiesene und durch den Benutzer zugewiesene Identitäten. Azure HDInsight verwendet durch den Benutzer zugewiesene verwaltete Identitäten, um auf Azure Data Lake Storage Gen2 zuzugreifen. Eine vom Benutzer zugewiesene verwaltete Identität wird als eigenständige Azure-Ressource erstellt. Azure erstellt eine Identität in dem Azure AD-Mandanten, der vom verwendeten Abonnement als vertrauenswürdig eingestuft wird. Nachdem die Identität erstellt wurde, kann sie einer oder mehreren Azure-Dienstinstanzen zugewiesen werden. Der Lebenszyklus einer vom Benutzer zugewiesenen Identität wird getrennt vom Lebenszyklus der Azure-Dienstinstanzen verwaltet, denen sie zugewiesen ist. Weitere Informationen zu verwalteten Identitäten finden Sie unter [Wie funktionieren die verwalteten Identitäten für Azure-Ressourcen?](../active-directory/managed-identities-azure-resources/overview.md#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka).

### <a name="how-do-i-set-permissions-for-azure-ad-users-to-query-data-in-data-lake-storage-gen2-using-hive-or-other-services"></a>Wie richte ich Berechtigungen für Azure AD-Benutzer zum Abfragen von Daten in Data Lake Storage Gen2 mit Hive oder anderen Diensten ein?

Verwenden Sie Azure AD-Sicherheitsgruppen als zugewiesenen Prinzipal in ACLs. Weisen Sie nicht einzelnen Benutzern oder Dienstprinzipalen direkt Dateizugriffsberechtigungen zu. Wenn Sie AD-Sicherheitsgruppen zum Steuern des Flusses von Berechtigungen verwenden, können Sie Benutzer oder Dienstprinzipale hinzufügen und entfernen, ohne erneut ACLs auf eine gesamte Verzeichnisstruktur anzuwenden. Sie müssen die Benutzer einfach der entsprechenden Azure AD-Sicherheitsgruppe hinzufügen oder sie aus ihr entfernen. ACLs werden nicht vererbt, und die erneute Anwendung von ACLs erfordert daher eine Aktualisierung der ACL für jede Datei und jedes Unterverzeichnis.

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden von Azure Data Lake Storage Gen2 Preview mit Azure HDInsight-Clustern](../storage/blobs/data-lake-storage-use-hdi-cluster.md)
* [Azure HDInsight integration with Data Lake Storage Gen2 preview – ACL and security update (Integration von Azure HDInsight in Data Lake Storage Gen2 (Vorschau) – ACL und Sicherheitsupdate)](https://azure.microsoft.com/blog/azure-hdinsight-integration-with-data-lake-storage-gen-2-preview-acl-and-security-update/)
* [Einführung in Azure Data Lake Storage Gen2 Preview](../storage/blobs/data-lake-storage-introduction.md)
