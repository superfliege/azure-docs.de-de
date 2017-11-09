---
title: "Verwalten von Benutzerberechtigungen auf Datei- und Ordnerebene – Azure HDInsight | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie Datei- und Ordnerberechtigungen für HDInsight-Cluster verwalten, die einer Domäne beigetreten sind."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: maxluk
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2017
ms.author: maxluk
ms.openlocfilehash: 9ca91721e691eca239478c4ac8b85e2652babdfd
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2017
---
# <a name="manage-user-permissions-at-the-file-and-folder-levels"></a>Verwalten von Benutzerberechtigungen auf Datei- und Ordnerebene

Für [in die Domäne eingebundene HDInsight-Cluster](./domain-joined/apache-domain-joined-introduction.md) werden eine strenge Authentifizierung für Azure AD-Benutzer (Azure Active Directory) und Richtlinien für die *rollenbasierte Zugriffssteuerung* (RBAC) für verschiedene Dienste wie YARN und Hive verwendet. Wenn Sie als Standarddatenspeicher für Ihren Cluster Azure Storage oder Windows Azure Storage Blobs (WASB) nutzen, können Sie auch Berechtigungen auf Datei- und Ordnerebene erzwingen. Sie können Apache Ranger verwenden, um den Zugriff auf die Dateien des Clusters für Ihre synchronisierten Azure AD-Benutzer und -Gruppen zu steuern.
<!-- [synchronized Azure AD users and groups](hdinsight-sync-aad-users-to-cluster.md). -->

Die Apache Ranger-Instanz für in Domänen eingebundene HDInsight-Cluster ist mit dem Ranger-WASB-Dienst vorkonfiguriert. Der Ranger-WASB-Dienst ist ein Richtlinienverwaltungsmodul, das Ranger-HDFS ähnelt, aber über eine andere Erzwingung der Zugriffsrichtlinien von Ranger verfügt. Für den Ranger-WASB-Dienst lautet die Standardantwort DENY, wenn eine eingehende Ressourcenanforderung nicht über eine passende Ranger-Richtlinie verfügt. Der Ranger-Dienst übergibt die Berechtigungsüberprüfung nicht an WASB.

## <a name="permission-and-policy-model"></a>Berechtigungs- und Richtlinienmodell

Zugriffsanforderungen für Ressourcen werden anhand des folgenden Ablaufs ausgewertet:

![Apache Ranger-Ablauf zur Richtlinienauswertung](./media/hdinsight-add-acls-at-file-folder-levels/ranger-policy-evaluation-flow.png)

Zuerst werden DENY-Regeln und anschließend ALLOW-Regeln ausgewertet. Am Ende des Abgleichs wird DENY zurückgegeben, wenn keine Richtlinien übereinstimmen.

### <a name="user-variable"></a>USER-Variable

Sie können die `{USER}`-Variable verwenden, wenn Sie Richtlinien für einen Benutzer für den Zugriff auf das Unterverzeichnis `/{username}` zuweisen, z.B.:

```
resource: path=/app-logs/{USER}, user: {USER}, recursive=true, permissions: all, delegateAdmin=true
```

Die obige Richtlinie gewährt Benutzern Zugriff auf ihren eigenen Unterordner unterhalb des Verzeichnisses `/app-logs/`. Diese Richtlinie sieht auf der Ranger-Benutzeroberfläche wie folgt aus:

![Anwendungsbeispiel für die USER-Variable](./media/hdinsight-add-acls-at-file-folder-levels/user-variable.png)

### <a name="policy-model-examples"></a>Richtlinienmodellbeispiele

Die folgende Tabelle enthält einige Beispiele für die Funktionsweise des Richtlinienmodells:

| Ranger-Richtlinie | Vorhandenes Dateisystem | Benutzeranforderung | Ergebnis |
| -- | -- | -- | -- |
| /data/finance/, bob, WRITE | /data | bob, Datei „/data/finance/mydatafile.txt“ erstellen | ALLOW: Zwischenordner „finance“ wird aufgrund von Vorgängerprüfung erstellt |
| /data/finance/, bob, WRITE | /data | alice, Datei „/data/finance/mydatafile.txt“ erstellen | DENY: Keine übereinstimmende Richtlinie |
| /data/finance*, bob, WRITE | /data | bob, Datei „/data/finance/mydatafile.txt“ erstellen | ALLOW: In diesem Fall ist die optionale rekursive Richtlinie (`*`) vorhanden, siehe [Platzhalter](#wildcards) |
| /data/finance/mydatafile.txt, bob, WRITE | /data | bob, Datei „/data/finance/mydatafile.txt“ erstellen | DENY: Vorgängerprüfung für „/data“ schlägt fehl, da keine Richtlinie vorhanden ist |
| /data/finance/mydatafile.txt, bob, WRITE | /data/finance | bob, Datei „/data/finance/mydatafile.txt“ erstellen | DENY: Keine Richtlinie für Vorgängerprüfung unter „/data/finance“ vorhanden |

Berechtigungen sind je nach Vorgangstyp auf Ordnerebene oder auf Dateiebene erforderlich. Für einen Aufruf vom Typ „read/open“ ist beispielsweise Lesezugriff auf Dateiebene erforderlich, während für einen „create“-Aufruf Berechtigungen auf Vorgängerordnerebene benötigt werden.

### <a name="wildcards-"></a>Platzhalter (*)

Wenn ein Platzhalter (`*`) im Pfad für eine Richtlinie enthalten ist, gilt der Platzhalter für diesen Pfad und die gesamte Teilstruktur. Diese Rekursion entspricht der Verwendung des `recurse-flag`-Elements. In Ranger-WASB gibt der Platzhalter sowohl die Rekursion als auch eine Teilübereinstimmung für den Namen an.

## <a name="manage-file-and-folder-level-permissions-with-apache-ranger"></a>Verwalten von Berechtigungen auf Datei- und Ordnerebene mit Apache Ranger

Führen Sie, falls noch nicht geschehen, [diese Anweisungen](./domain-joined/apache-domain-joined-configure.md) aus, um einen neuen in die Domäne eingebundenen Cluster bereitzustellen.

Öffnen Sie Ranger-WASB, indem Sie zu `https://<YOUR CLUSTER NAME>.azurehdinsight.net/ranger/` navigieren. Geben Sie den Benutzernamen und das Kennwort für den Clusteradministrator ein, die Sie beim Erstellen des Clusters definiert haben.

Nach der Anmeldung wird das Ranger-Dashboard angezeigt:

![Ranger-Dashboard](./media/hdinsight-add-acls-at-file-folder-levels/ranger-dashboard.png)

Klicken Sie zum Anzeigen der aktuellen Datei- und Ordnerberechtigungen für das zugeordnete Azure Storage-Konto Ihres Clusters im WASB-Menüfeld auf den Link ***CLUSTERNAME*_wasb**.

![Ranger-Dashboard](./media/hdinsight-add-acls-at-file-folder-levels/wasb-dashboard-link.png)

Die aktuelle Liste mit den Richtlinien wird angezeigt. Als Starthilfe sind mehrere typische Richtlinien vorhanden. Sehen Sie sich die Details jeder Richtlinie an, um Beispiele für die Nutzung zu erhalten.

Für jede Richtlinie wird angezeigt, ob die Richtlinie aktiviert ist, ob die Überwachungsprotokollierung konfiguriert ist und welche Gruppen und Benutzer zugewiesen sind. Es gibt zwei Aktionsschaltflächen für jede Richtlinie: „Bearbeiten“ und „Löschen“.

![Liste mit Richtlinien](./media/hdinsight-add-acls-at-file-folder-levels/policy-list.png)

### <a name="adding-a-new-policy"></a>Hinzufügen einer neuen Richtlinie

1. Wählen Sie oben rechts auf der Seite mit den WASB-Richtlinien die Option **Neue Richtlinie hinzufügen**.

    ![Neue Richtlinie hinzufügen](./media/hdinsight-add-acls-at-file-folder-levels/add-new.png)

2. Geben Sie unter **Richtlinienname** einen beschreibenden Namen ein. Geben Sie das Azure-**Speicherkonto** für Ihren Cluster (*KONTONAME*.blob.core.windows.net) und den **Speicherkontocontainer** an, den Sie beim Erstellen Ihres Clusters angegeben haben. Geben Sie unter **Relativer Pfad** den relativen Pfad für den Zugriff auf den Ordner oder die Datei ein (relativ zum Cluster).

    ![Formular für neue Richtlinie](./media/hdinsight-add-acls-at-file-folder-levels/new-policy.png)

3. Geben Sie unterhalb des Formulars Ihre **zulässigen Bedingungen** für diese neue Ressource an. Wählen Sie die gewünschten Gruppen und Benutzer aus, und legen Sie die Berechtigungen dafür fest. Im folgenden Beispiel gewähren wir für alle Benutzer in der Gruppe `sales` Lese- und Schreibzugriff.

    ![Zulassen von „sales“](./media/hdinsight-add-acls-at-file-folder-levels/allow-sales.png)

4. Wählen Sie **Speichern** aus.

### <a name="example-policy-conditions"></a>Beispielbedingungen für Richtlinien

Im Apache Ranger-[Ablauf zur Richtlinienauswertung](#permission-and-policy-model) können Sie eine beliebige Kombination von Allow- und Deny-Bedingungen angeben, die Sie für Ihre Anforderungen benötigen. Hier sind einige Beispiele:

1. Alle Vertriebsbenutzer zulassen, aber keine Praktikanten:

    ![Vertrieb zulassen, Praktikanten verweigern](./media/hdinsight-add-acls-at-file-folder-levels/allow-sales-deny-interns.png)

2. Alle Vertriebsbenutzer zulassen und alle Praktikanten verweigern, mit Ausnahme des Praktikanten „hiveuser3“, der über Lesezugriff verfügen soll:

    ![Vertrieb zulassen, Praktikanten mit Ausnahme von „hiveuser3“ verweigern](./media/hdinsight-add-acls-at-file-folder-levels/allow-sales-deny-interns-except-hiveuser3.png)

## <a name="next-steps"></a>Nächste Schritte

* [Konfigurieren von Hive-Richtlinien in HDInsight mit Domänenverknüpfung](./domain-joined/apache-domain-joined-run-hive.md)
* [Verwalten von in die Domäne eingebundenen HDInsight-Clustern](./domain-joined/apache-domain-joined-manage.md)
* [Autorisieren von Benutzern für Ambari-Ansichten](hdinsight-authorize-users-to-ambari.md)

<!-- * [Synchronize Azure AD users and groups](hdinsight-sync-aad-users-to-cluster.md) -->

