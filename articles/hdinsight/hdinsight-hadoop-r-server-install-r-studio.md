---
title: Installieren Sie RStudio mit R-Server auf HDInsight - Azure | Microsoft Docs
description: Wie RStudio mit R-Server auf HDInsight installiert.
services: hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 918abb0d-8248-4bc5-98dc-089c0e007d49
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: 416420d855505508735ebd8526e93efdb230ad53
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="installing-rstudio-with-r-server-on-hdinsight"></a>Installieren RStudio mit R-Server auf HDInsight

In diesem Artikel wird beschrieben, wie die Community (free) Version von installieren [RStudio Server](https://www.rstudio.com/products/rstudio-server/) auf dem Edge-Knoten eines Clusters mithilfe eines benutzerdefinierten Skripts. RStudio Server stellt eine browserbasierte IDE von Remoteclients auch unter Linux verwendet wird. Es gibt mehrere integrierte entwicklungsumgebungen (IDEs) für R heute, einschließlich:

- Microsofts [R-Tools für Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS) 
- [RStudio-Server](https://www.rstudio.com/products/rstudio-server/) 
- Walware des Eclipse-basierten [StatET](http://www.walware.de/goto/statet).

Der Vorteil RStudio Server installieren, auf dem Edge-Knoten, der einen HDInsight-Cluster ist, eine vollständige IDE-Erfahrung für die Entwicklung und Ausführung von R-Skripts mit R-Server im Cluster bereitgestellt. Diese Konfiguration kann als Standard verwenden der R-Konsole in erheblich produktiver zu sein.

> [!NOTE]
> Das in diesem Artikel beschriebene Verfahren ist nur relevant, wenn Sie nicht zur Installation von RStudio Server Community Edition, bei der Bereitstellung des Clusters ausgewählt haben. Wenn Sie während der Bereitstellung hinzugefügt, und klicken Sie dann für den Zugriff darauf klicken Sie auf die **R Server-Dashboards** Kachel im Azure Portal Eintrag für den Cluster, klicken Sie dann auf die **R Studio Server** Kachel. 

Wenn Sie die kommerziell lizenzierte Pro-Version von RStudio Server verwenden möchten, müssen Sie die installationsanweisungen unter befolgen [RStudio Server](https://www.rstudio.com/products/rstudio/download-server/).

> [!NOTE]
> Bei Verwendung ein HDInsight-Clusters für den R installiert wurde, mit der [Installieren von R-Skriptaktion](hdinsight-hadoop-r-scripts-linux.md), die Schritte in diesem Dokument funktioniert nicht ordnungsgemäß, wie sie ein R-Server auf den HDInsight-Cluster erforderlich.
>
> 

## <a name="prerequisites"></a>Voraussetzungen

* Eine Azure HDInsight-Cluster mit R-Server installiert werden soll. Anweisungen hierzu finden Sie unter [erste Schritte mit R-Server auf HDInsight-Clustern](hdinsight-hadoop-r-server-get-started.md).
* Eine SSH-Client. Für Linux und Unix-Distributionen oder Macintosh OS X die `ssh` Befehl wird mit dem Betriebssystem bereitgestellt. Für Windows, empfehlen wir [Cygwin](http://www.redhat.com/services/custom/cygwin/) mit der [OpenSSH-Option](https://www.youtube.com/watch?v=CwYSvvGaiWU), oder [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).  

## <a name="install-rstudio-on-the-cluster-using-a-custom-script"></a>Installieren Sie RStudio, auf dem Cluster mithilfe eines benutzerdefinierten Skripts

Es folgen die Schritte aus:

1. Identifizieren Sie das Edge-Knoten des Clusters. Unten ist für einen HDInsight-Cluster mit R-Server die Benennungskonvention für den Hauptknoten und Edge-Knoten.
   * Hauptknoten`CLUSTERNAME-ssh.azurehdinsight.net`
   * Edge-Knoten`CLUSTERNAME-ed-ssh.azurehdinsight.net` 

2. SSH in den Edge-Knoten des Clusters mit dem Benennungsschema, die in Schritt 1 angegeben. Weitere Informationen finden Sie unter [Verwenden von SSH mit HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

3. Sobald Sie verbunden sind, werden Sie Root-Benutzer auf dem Cluster. Verwenden Sie in der SSH-Sitzung den folgenden Befehl ein:

        sudo su -

4. Laden Sie das benutzerdefinierte Skript um RStudio zu installieren. Verwenden Sie den folgenden Befehl:

        wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/InstallRStudio.sh

5. Ändern Sie die Berechtigungen für den benutzerdefinierten Skriptdatei an, und führen Sie das Skript. Verwenden Sie die folgenden Befehle:

        chmod 755 InstallRStudio.sh
        ./InstallRStudio.sh

6. Wenn Sie ein SSH-Kennwort beim Erstellen eines HDInsight-Clusters mit R-Server verwendet, können Sie diesen Schritt überspringen und mit der nächsten fortfahren. Hätten Sie einen SSH-Schlüssel zum Erstellen des Clusters, müssen Sie ein Kennwort für den SSH-Benutzer festlegen. Sie benötigen dieses Kennwort bei der Verbindung RStudio. Führen Sie die folgenden Befehle aus:

        passwd USERNAME
        Current Kerberos password:
        New password:
        Retype new password:
        Current Kerberos password:


7. Aufforderung zur Eingabe **aktuellen Kerberos-Kennwort**, drücken Sie die **EINGABETASTE**.  Beachten Sie, die ersetzt werden müssen, `USERNAME` mit einem SSH-Benutzer für den HDInsight-Cluster. Wenn Ihr Kennwort wurde erfolgreich festgelegt ist, sollten Sie die folgende Meldung angezeigt:

        passwd: password updated successfully

    Die SSH-Sitzung zu beenden.

8. Erstellen Sie einen SSH-Tunnel mit dem Cluster durch die Zuordnung `ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` auf den HDInsight-Cluster auf den Clientcomputer. Sie müssen einen SSH-Tunnel erstellen, bevor Sie eine neue Browsersitzung öffnen.

   * Auf einem Linux-Client oder ein Windows-Client mit [Cygwin](http://www.redhat.com/services/custom/cygwin/), öffnen Sie eine terminal-Sitzung, und verwenden Sie den folgenden Befehl:

             ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

       Ersetzen Sie **Benutzername** mit einem SSH-Benutzer für Ihre HDInsight-Cluster, und Ersetzen Sie **CLUSTERNAME** durch den Namen des HDInsight-Cluster.
       Sie können auch einen SSH-Schlüssel anstelle eines Kennworts durch Hinzufügen von `-i id_rsa_key`.        
   * Wenn einen Windows-Client mit PuTTY verwenden

     1. Öffnen Sie PuTTY, und geben Sie die Verbindungsinformationen.
     2. In der **Kategorie** Abschnitt auf der linken Seite des Dialogfelds, erweitern Sie **Verbindung**, erweitern Sie **SSH**, und wählen Sie dann **Tunnel**.
     3. Geben Sie die folgenden Informationen auf der **Optionen zur Steuerung der SSH-portweiterleitung** Form:

        * **Quellport** – der Port auf dem Client, die weitergeleitet werden sollen. Beispielsweise **8787**.
        * **Ziel** -Ziel, die mit dem lokalen Client-Computer zugeordnet werden muss. Beispielsweise **Localhost:8787**.

            ![Erstellen Sie einen SSH-Tunnel](./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "erstellen Sie einen SSH-Tunnel")

     4. Klicken Sie auf **hinzufügen** fügen die Einstellungen, und klicken Sie dann auf **öffnen** SSH-Verbindung zu öffnen.
     5. Wenn Sie aufgefordert werden, melden Sie sich an den Server zur Herstellung einer SSH-Sitzung aktivieren den Tunnel verlassen.

9. Öffnen Sie einen Webbrowser, und geben Sie die folgende URL basierend auf den Port an, den Sie für den Tunnel eingegeben haben:

        http://localhost:8787/ 

10. Sie werden aufgefordert, den SSH-Benutzername und das Kennwort für die Verbindung zum Cluster eingeben. Wenn Sie einen SSH-Schlüssel beim Erstellen des Clusters verwendet haben, müssen Sie das Kennwort eingeben, das Sie in Schritt 5 erstellt haben.

    ![Herstellen einer Verbindung mit R Studio](./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "erstellen Sie einen SSH-Tunnel")

11. Um zu testen, ob die RStudio-Installation erfolgreich war, können Sie ein Testskript ausführen, die R-basierte MapReduce und Spark-Aufträge im Cluster ausgeführt wird. Zum Herunterladen von des Testskripts im RStudio ausgeführt wechseln Sie zurück in die SSH-Konsole, und geben Sie die folgenden Befehle:

    *    Wenn Sie einen Hadoop-Cluster mit R erstellt haben, verwenden Sie folgenden Befehl ein:

            Wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r
    *    Wenn Sie einen Spark-Cluster mit R erstellt haben, verwenden Sie folgenden Befehl ein:

            Wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r

12. In RStudio sehen Sie das Testskript, das Sie heruntergeladen haben. Doppelklicken Sie auf die Datei, um es zu öffnen, wählen Sie den Inhalt der Datei und klicken Sie dann auf **ausführen**. Daraufhin sollte die Ausgabe in die **Konsole** Bereich:

   ![Die Installation testen](./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "die Installation testen")

Eine weitere Möglichkeit wäre die geben `source(testhdi.r)` oder `source(testhdi_spark.r)` beim Ausführen des Skripts.

## <a name="see-also"></a>Weitere Informationen:

* [Berechnen Sie die Kontextoptionen für R-Server auf HDInsight-Cluster](hdinsight-hadoop-r-server-compute-contexts.md)
* [Azure Storage-Optionen für R-Server auf HDInsight](hdinsight-hadoop-r-server-storage.md)

