---
title: Bring Your Own Key für Apache Kafka in Azure HDInsight (Vorschauversion)
description: In diesem Artikel wird beschrieben, wie Sie mit Ihrem eigenen Schlüssel aus Azure Key Vault Daten verschlüsseln, die in Apache Kafka in Azure HDInsight gespeichert sind.
services: hdinsight
ms.service: hdinsight
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 85fea195b05bea8a1db70f8b5b81cabdfe7c6c72
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2018
ms.locfileid: "48041508"
---
# <a name="bring-your-own-key-for-apache-kafka-on-azure-hdinsight-preview"></a>Bring Your Own Key für Apache Kafka in Azure HDInsight (Vorschauversion)

Azure HDInsight beinhaltet BYOK-Unterstützung (Bring Your Own Key) für Apache Kafka. Dank dieser Funktion können Sie die Verantwortung und die Verwaltung für die Schlüssel übernehmen, die zum Verschlüsseln ruhender Daten verwendet werden. 

Alle verwalteten Datenträger in HDInsight werden mit der Speicherdienstverschlüsselung (Storage Service Encryption, SSE) von Azure geschützt. Die Daten auf diesen Datenträgern werden standardmäßig mit von Microsoft verwalteten Schlüsseln verschlüsselt. Bei der Aktivierung von BYOK geben Sie den Verschlüsselungsschlüssel für HDInsight für die Verwendung und Verwaltung mit Azure Key Vault an. 

Die BYOK-Verschlüsselung besteht aus einem Schritt, der bei der Clustererstellung ohne zusätzliche Kosten ausgeführt wird. Sie müssen bei der Erstellung Ihres Clusters lediglich HDInsight als verwaltete Identität bei Azure Key Vault registrieren und den Verschlüsselungsschlüssel hinzufügen.

Alle Nachrichten an den Kafka-Cluster (einschließlich von Kafka verwaltete Replikate) werden mit einem symmetrischen Datenverschlüsselungsschlüssel (Data Encryption Key, DEK) verschlüsselt. Der DEK wird mit dem Schlüssel für die Schlüsselverschlüsselung (Key Encryption Key, KEK) aus Ihrem Schlüsseltresor geschützt. Die Ver- und Entschlüsselung wird vollständig von Azure HDInsight übernommen. 

Sie können für das sichere Rotieren der Schlüssel im Schlüsseltresor das Azure-Portal oder die Azure CLI verwenden. Beim Rotieren eines Schlüssels beginnt der HDInsight Kafka-Cluster innerhalb weniger Minuten mit der Verwendung des neuen Schlüssels. Aktivieren Sie die Schlüsselschutzfunktionen „Do Not Purge“ (Nicht bereinigen) und „Soft Delete“ (Vorläufig löschen) zum Schutz vor Ransomware und versehentlichem Löschen. Schlüssel ohne diese Schutzfunktionen werden nicht unterstützt.

## <a name="get-started-with-byok"></a>Erste Schritte mit BYOK

1. Erstellen Sie verwaltete Identitäten für Azure-Ressourcen.

   Erstellen Sie für die Authentifizierung für Key Vault mit dem [Azure-Portal](../../active-directory/managed-service-identity/how-to-manage-ua-identity-portal.md), [Azure PowerShell](../../active-directory/managed-service-identity/how-to-manage-ua-identity-powershell.md), [Azure Resource Manager](../../active-directory/managed-service-identity/how-to-manage-ua-identity-arm.md) oder der [Azure CLI](../../active-directory/managed-service-identity/how-to-manage-ua-identity-cli.md) eine benutzerseitig zugewiesene verwaltete Identität. Azure Active Directory ist für verwaltete Identitäten und BYOK in Kafka erforderlich, das Enterprise-Sicherheitspaket (Enterprise Security Package, ESP) jedoch nicht. Achten Sie darauf, die Ressourcen-ID der verwalteten Identität zu speichern, da Sie sie später zur Key Vault-Zugriffsrichtlinie hinzufügen.

   ![Erstellen einer benutzerseitig zugewiesenen verwalteten Identität im Azure-Portal](./media/apache-kafka-byok/user-managed-identity-portal.png)

2. Importieren Sie einen vorhandenen Schlüsseltresor, oder erstellen Sie einen neuen.

   HDInsight unterstützt nur Azure Key Vault. Falls Sie einen eigenen Schlüsseltresor besitzen, können Sie Ihre Schlüssel in Azure Key Vault importieren. Denken Sie daran, dass für die Schlüssel „Soft Delete“ (Vorläufig löschen) und „Do Not Purge“ (Nicht bereinigen) aktiviert sein muss. Die Funktionen „Soft Delete“ (Vorläufig löschen) und „Do Not Purge“ (Nicht bereinigen) sind über die REST-, .NET-/C#-, PowerShell- und Azure CLI-Schnittstelle verfügbar.

   Befolgen Sie zum Erstellen eines neuen Schlüsseltresors die Schnellstartanleitung für [Azure Key Vault](../../key-vault/key-vault-get-started.md). Weitere Informationen zum Importieren vorhandener Schlüsseln finden Sie unter [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](../../key-vault/about-keys-secrets-and-certificates.md).

   Wählen Sie zum Erstellen eines neuen Schlüssels **Generieren/Importieren** im Menü **Schlüssel** unter **Einstellungen** aus.

   ![Generieren eines neuen Schlüssels in Azure Key Vault](./media/apache-kafka-byok/kafka-create-new-key.png)

   Legen Sie **Optionen** auf **Generieren** fest, und benennen Sie den Schlüssel.

   ![Generieren eines neuen Schlüssels in Azure Key Vault](./media/apache-kafka-byok/kafka-create-a-key.png)

   Wählen Sie den Schlüssel, den Sie erstellt haben, aus der Liste der Schlüssel aus.

   ![Azure Key Vault-Schlüsselliste](./media/apache-kafka-byok/kafka-key-vault-key-list.png)

   Wenn Sie Ihren eigenen Schlüssel für die Verschlüsselung des Kafka-Clusters verwenden, müssen Sie die Schlüssel-URI angeben. Kopieren Sie den **Schlüsselbezeichner**, und speichern sie ihn, bis Sie zum Erstellen des Clusters bereit sind.

   ![Kopieren des Schlüsselbezeichners](./media/apache-kafka-byok/kafka-get-key-identifier.png)
   
3. Fügen Sie eine verwaltete Identität zur Key Vault-Zugriffsrichtlinie hinzu.

   Erstellen Sie eine neue Azure Key Vault-Zugriffsrichtlinie.

   ![Erstellen einer neuen Azure Key Vault-Zugriffsrichtlinie](./media/apache-kafka-byok/add-key-vault-access-policy.png)

   Wählen Sie unter **Prinzipal auswählen** die von Ihnen erstellte benutzerseitig zugewiesene verwaltete Identität aus.

   ![Festlegen der Option „Prinzipal auswählen“ für die Azure Key Vault-Zugriffsrichtlinie](./media/apache-kafka-byok/add-key-vault-access-policy-select-principal.png)

   Aktivieren Sie unter **Schlüsselberechtigungen** die Optionen **Abrufen**, **Schlüssel entpacken** und **Schlüssel packen**.

   ![Festlegen der Schlüsselberechtigungen für die Azure Key Vault-Zugriffsrichtlinie](./media/apache-kafka-byok/add-key-vault-access-policy-keys.png)

   Aktivieren Sie unter **Berechtigungen für Geheimnis** die Optionen **Abrufen**, **Festlegen** und **Löschen**.

   ![Festlegen der Schlüsselberechtigungen für die Azure Key Vault-Zugriffsrichtlinie](./media/apache-kafka-byok/add-key-vault-access-policy-secrets.png)

4. Erstellen des HDInsight-Clusters

   Sie können nun einen neuen HDInsight-Cluster erstellen. BYOK kann während der Clustererstellung nur auf neue Cluster angewendet werden. Die Verschlüsselung kann für BYOK-Cluster nicht entfernt werden, und BYOK kann nicht zu vorhandenen Clustern hinzugefügt werden.

   ![Kafka-Datenträgerverschlüsselung im Azure-Portal](./media/apache-kafka-byok/apache-kafka-byok-portal.png)

   Geben Sie während der Clustererstellung die vollständige Schlüssel-URL (einschließlich Schlüsselversion) an. Beispiel: `https://contoso-kv.vault.azure.net/keys/kafkaClusterKey/46ab702136bc4b229f8b10e8c2997fa4`. Darüber hinaus müssen Sie die verwaltete Identität dem Cluster zuweisen und den Schlüssel-URI angeben.

## <a name="faq-for-byok-to-kafka"></a>Häufig gestellte Fragen zu BYOK in Kafka

**Wie greift der Kafka-Cluster auf meinen Schlüsseltresor zu?**

   Ordnen Sie eine verwaltete Identität während der Clustererstellung dem HDInsight Kafka-Cluster zu. Diese verwaltete Identität kann vor oder während der Clustererstellung erstellt werden. Außerdem müssen Sie der verwalteten Identität Zugriff auf den Schlüsseltresor mit dem gespeicherten Schlüssel gewähren.

**Ist dieses Feature für alle Kafka-Cluster in HDInsight verfügbar?**

   Die BYOK-Verschlüsselung ist nur für Kafka-Cluster mit Version 1.1 und höheren Versionen möglich.

**Kann ich verschiedene Schlüssel für verschiedene Themen/Partitionen verwenden?**

   Nein, alle verwalteten Datenträger im Cluster werden mit dem gleichen Schlüssel verschlüsselt.

**Wie kann ich den Cluster wiederherstellen, wenn die Schlüssel gelöscht werden?**

   Da nur Schlüssel unterstützt werden, für die „Soft Delete“ (Vorläufig löschen) aktiviert ist, sollte der Cluster wieder Zugriff auf die Schlüssel erlangen, wenn die Schlüssel im Schlüsseltresor wiederhergestellt werden. Informationen zum Wiederherstellen eines Azure Key Vault-Schlüssels finden Sie unter [Restore-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey).

**Können Producer-/Consumeranwendungen gleichzeitig einen BYOK-Cluster und einen Cluster ohne BYOK verwenden?**

   Ja. Die Verwendung von BYOK ist für Producer-/Consumeranwendungen transparent. Die Verschlüsselung erfolgt auf Betriebssystemebene. An vorhandenen Producer-/Consumeranwendungen mit Kafka müssen keine Änderungen vorgenommen werden.

**Werden Betriebssystemdatenträger/Ressourcendatenträger ebenfalls verschlüsselt?**

   Nein. Betriebssystemdatenträger und Ressourcendatenträger werden nicht verschlüsselt.

**Unterstützen die neuen Broker BYOK einwandfrei, wenn ein Cluster zentral hochskaliert wird?**

   Ja. Der Cluster benötigt während der zentralen Hochskalierung Zugriff auf den Schlüssel im Schlüsseltresor. Alle verwalteten Datenträger im Cluster werden mit dem gleichen Schlüssel verschlüsselt.

**Ist BYOK an meinem Standort verfügbar?**

   Kafka BYOK ist in allen öffentlichen Clouds verfügbar.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Azure Key Vault finden Sie unter [Was ist der Azure-Schlüsseltresor?](../../key-vault/key-vault-whatis.md).
* Weitere Informationen zu den ersten Schritten mit Azure Key Vault finden Sie unter [Erste Schritte mit dem Azure-Schlüsseltresor](../../key-vault/key-vault-get-started.md).
