---
title: "Verwalten der Domäne HDInsight-Cluster - Azure | Microsoft Docs"
description: "Informationen Sie zum Verwalten der Domäne HDInsight-Cluster"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 6ebc4d2f-2f6a-4e1e-ab6d-af4db6b4c87c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/25/2016
ms.author: saurinsh
ms.openlocfilehash: 9b56ce6cc5bdd3b8d48d047751e978cad08598e1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-domain-joined-hdinsight-clusters-preview"></a>Verwalten der Domäne HDInsight-Cluster (Vorschau)
Erfahren Sie, die Benutzer und Rollen im HDInsight Domäne und zum Verwalten von HDInsight-Cluster Domäne angehören.

## <a name="users-of-domain-joined-hdinsight-clusters"></a>Benutzer der Domäne HDInsight-Cluster
Ein HDInsight-Cluster, der keine Domäne eingebunden ist, hat zwei Benutzerkonten, die während der Erstellung des Clusters erstellt werden:

* **Ambari Admin**: dieses Konto wird auch bezeichnet als *Hadoop-Benutzer* oder *HTTP-Benutzer*. Dieses Konto verwendet werden kann, am https:// Ambari anmelden&lt;Clustername >. azurehdinsight.net. Sie können auch verwendet werden, Ambari Ansichten Abfragen ausführen, führen Sie Aufträge über externe Tools (z. B. PowerShell, Templeton, Visual Studio) und mit dem Hive-ODBC-Treiber und BI-Tools (z. B. Excel, Power BI oder Tableau) zu authentifizieren.
* **SSH-Benutzer**: dieses Konto mit SSH verwendet werden kann, und führen Sie die Befehle "sudo". Es wurde die Stamm-Berechtigungen, um die virtuelle Linux-Computer.

Ein Domäne HDInsight-Cluster verfügt über drei neue Benutzer zusätzlich zu den Ambari Admin und SSH.

* **Rangers Admin**: dieses Konto ist das lokale Administratorkonto für die Apache Rangers. Es ist nicht die Domäne active Directory-Benutzer. Dieses Konto dient zum Einrichten von Richtlinien und stellen andere Benutzer Administratoren oder delegierten Administratoren (sodass die Benutzer Richtlinien verwalten können). Standardmäßig ist der Benutzername *Admin* und das Kennwort ist identisch mit Ambari-Administratorkennwort. Das Kennwort kann mithilfe der Seite "Einstellungen" in Rangers aktualisiert werden.
* **Cluster-Admin-Domänenbenutzer**: dieses Konto ist ein active Directory-Domänenbenutzer als Administrator des Hadoop-Cluster einschließlich Ambari und Rangers festgelegt. Sie müssen die Anmeldeinformationen des Benutzers während der Clustererstellung angeben. Dieser Benutzer hat die folgenden Berechtigungen:

  * Hinzufügen von Computern zur Domäne, und setzen Sie sie in der Organisationseinheit, die Sie bei der Erstellung des Clusters angeben.
  * Erstellen Sie die Dienstprinzipale innerhalb der Organisationseinheit, die Sie während der Clustererstellung angeben.
  * Reverse-DNS-Einträge zu erstellen.

    Beachten Sie, dass der AD-Benutzer auch dieser Berechtigungen verfügen.

    Es gibt einige Endpunkte innerhalb des Clusters (z. B. Templeton), die nicht von Rangers verwaltet werden und sind daher nicht sicher. Diese Endpunkte sind für alle Benutzer außer den Cluster Admin Domänenbenutzer gesperrt.
* **Reguläre**: während der Clustererstellung, Sie können mehrere active Directory-Gruppen bereitstellen. Die Benutzer in diesen Gruppen werden Rangers und Ambari synchronisiert werden. Diese Benutzer Domänenbenutzer sind und haben Zugriff auf nur Rangers verwalteten Endpunkte (z. B. Hiveserver2). Die RBAC-Richtlinien und die Überwachung wird auf diesen Benutzer angewendet werden.

## <a name="roles-of-domain-joined-hdinsight-clusters"></a>Rollen von Domäne HDInsight-Cluster
HDInsight Domäne haben die folgenden Rollen:

* Clusterverwaltung
* Cluster-Operator
* Dienstadministrator
* Dienst-Operator
* Clusterbenutzer

**Um die Berechtigungen dieser Rollen finden Sie unter**

1. Öffnen Sie die Ambari-Benutzeroberfläche.  Finden Sie unter [öffnen Sie die Ambari-Benutzeroberfläche](#open-the-ambari-management-ui).
2. Klicken Sie im linken Menü auf **Rollen**.
3. Klicken Sie auf das blaue Fragezeichen klicken, um die Berechtigungen finden Sie unter:

    ![Domäne HDInsight Rollen und Berechtigungen](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-roles-permissions.png)

## <a name="open-the-ambari-management-ui"></a>Öffnen Sie die Ambari-Benutzeroberfläche
1. Melden Sie sich auf die [Azure-Portal](https://portal.azure.com).
2. Öffnen Sie Ihren HDInsight-Cluster in einem Blatt. Finden Sie unter [Liste handelt, und zeigt Cluster](hdinsight-administer-use-management-portal.md#list-and-show-clusters).
3. Klicken Sie auf **Dashboard** in der oberen Menüleiste Ambari zu öffnen.
4. Melden Sie sich auf den Cluster Administrator Domänenbenutzernamen und das Kennwort mit Ambari an.
5. Klicken Sie auf die **Admin** Dropdown-Menü aus der oberen rechten Ecke, und klicken Sie dann auf **Ambari verwalten**.

    ![Ambari zum Verwalten von HDInsight Domäne](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-manage-ambari.png)

    Die Benutzeroberfläche sieht wie folgt:

    ![Domäne HDInsight Ambari-Benutzeroberfläche](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui.png)

## <a name="list-the-domain-users-synchronized-from-your-active-directory"></a>Liste der Domänen-Benutzer aus Ihrem Active Directory synchronisiert werden
1. Öffnen Sie die Ambari-Benutzeroberfläche.  Finden Sie unter [öffnen Sie die Ambari-Benutzeroberfläche](#open-the-ambari-management-ui).
2. Klicken Sie im linken Menü auf **Benutzer**. Sie sehen, dass alle Benutzer, die mit dem HDInsight-Cluster aus dem Active Directory synchronisiert.

    ![Domäne HDInsight Ambari Management UI Benutzer auflisten](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-users.png)

## <a name="list-the-domain-groups-synchronized-from-your-active-directory"></a>Liste der Gruppen der Domäne aus Ihrem Active Directory synchronisiert werden
1. Öffnen Sie die Ambari-Benutzeroberfläche.  Finden Sie unter [öffnen Sie die Ambari-Benutzeroberfläche](#open-the-ambari-management-ui).
2. Klicken Sie im linken Menü auf **Gruppen**. Sie sehen, dass alle Gruppen, die mit dem HDInsight-Cluster aus dem Active Directory synchronisiert.

    ![Domäne HDInsight Ambari-UI Liste Verwaltungsgruppen](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-groups.png)

## <a name="configure-hive-views-permissions"></a>Konfigurieren von Berechtigungen für Hive-Ansichten
1. Öffnen Sie die Ambari-Benutzeroberfläche.  Finden Sie unter [öffnen Sie die Ambari-Benutzeroberfläche](#open-the-ambari-management-ui).
2. Klicken Sie im linken Menü auf **Ansichten**.
3. Klicken Sie auf **HIVE** um die Details anzuzeigen.

    ![Domäne HDInsight Ambari Management UI Hive-Ansichten](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views.png)
4. Klicken Sie auf die **Hive-Ansicht** Link zur Konfiguration von Hive-Ansichten.
5. Führen Sie einen Bildlauf nach unten, um die **Berechtigungen** Abschnitt.

    ![Domäne HDInsight Ambari Management UI Hive-Ansichten Konfigurieren von Berechtigungen](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views-permissions.png)
6. Klicken Sie auf **Add User** oder **Gruppe hinzufügen**, und geben Sie dann die Benutzer oder Gruppen, die Hive-Ansichten verwenden können.

## <a name="configure-users-for-the-roles"></a>Konfigurieren von Benutzern für die Rollen
 Eine Liste der Rollen und deren Berechtigungen finden Sie unter [Rollen der Domäne HDInsight-Cluster](#roles-of-domain---joined-hdinsight-clusters).

1. Öffnen Sie die Ambari-Benutzeroberfläche.  Finden Sie unter [öffnen Sie die Ambari-Benutzeroberfläche](#open-the-ambari-management-ui).
2. Klicken Sie im linken Menü auf **Rollen**.
3. Klicken Sie auf **Add User** oder **Gruppe hinzufügen** , Benutzer und Gruppen unterschiedliche Rollen zuzuweisen.

## <a name="next-steps"></a>Nächste Schritte
* Konfigurieren einen Domäne HDInsight-Cluster, finden Sie unter [konfigurieren Domäne HDInsight-Cluster](hdinsight-domain-joined-configure.md).
* Konfigurieren die Hive-Richtlinien und Hive-Abfragen ausführen, finden Sie unter [Hive-Konfigurieren von Richtlinien für die Domäne HDInsight-Cluster](hdinsight-domain-joined-run-hive.md).
* Zum Ausführen von Hive-Abfragen mithilfe von SSH auf Domäne HDInsight-Clustern finden Sie unter [Verwenden von SSH mit HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
