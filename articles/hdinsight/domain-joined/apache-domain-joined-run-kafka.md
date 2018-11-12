---
title: Konfigurieren von Kafka-Richtlinien in HDInsight mit dem Enterprise-Sicherheitspaket – Azure
description: Erfahren Sie, wie Sie Apache Ranger-Richtlinien für Kafka in Azure HDInsight mit dem Enterprise-Sicherheitspaket konfigurieren.
services: hdinsight
ms.service: hdinsight
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.topic: tutorial
ms.date: 09/24/2018
ms.openlocfilehash: 48cfba6f62d75470efd27e3a4cdcb995e716798b
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2018
ms.locfileid: "51037140"
---
# <a name="tutorial-configure-kafka-policies-in-hdinsight-with-enterprise-security-package-preview"></a>Tutorial: Konfigurieren von Kafka-Richtlinien in HDInsight mit dem Enterprise-Sicherheitspaket (Vorschauversion)

Erfahren Sie, wie Sie Apache Ranger-Richtlinien für Enterprise-Sicherheitspaket-Kafka-Cluster (ESP) konfigurieren. Es werden Verbindungen von ESP-Clustern mit einer Domäne hergestellt, sodass Benutzer sich mit Anmeldeinformationen für die Domäne authentifizieren können. In diesem Tutorial erstellen Sie zwei Ranger-Richtlinien, um den Zugriff auf die Themen `sales*` und `marketingspend` einzuschränken.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen von Domänenbenutzern
> * Erstellen von Ranger-Richtlinien
> * Erstellen von Themen in einem Kafka-Cluster
> * Testen von Ranger-Richtlinien

## <a name="before-you-begin"></a>Voraussetzungen

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen.

* Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

* Erstellen Sie einen [HDInsight-Kafka-Cluster mit dem Enterprise-Sicherheitspaket](apache-domain-joined-configure-using-azure-adds.md).

## <a name="connect-to-apache-ranger-admin-ui"></a>Herstellen einer Verbindung mit der Apache Ranger-Administratoroberfläche

1. Stellen Sie in einem Browser mit der URL `https://<ClusterName>.azurehdinsight.net/Ranger/` eine Verbindung mit der Ranger-Administrator-Benutzeroberfläche her. Vergessen Sie nicht, `<ClusterName>` in den Namen des Kafka-Clusters zu ändern.

    > [!NOTE] 
    > Ranger-Anmeldeinformationen sind nicht identisch mit Hadoop-Clusteranmeldeinformationen. Stellen Sie die Verbindung mit der Ranger-Administratoroberfläche in einem neuen InPrivate-Browserfenster her, um zu verhindern, dass der Browser zwischengespeicherte Hadoop-Anmeldeinformationen verwendet.

2. Melden Sie sich mit Ihren Administratoranmeldeinformationen für Azure Active Directory (AD) an. Die Azure AD-Administratoranmeldeinformationen sind nicht identisch mit HDInsight-Clusteranmeldeinformationen oder Linux-HDInsight-Knoten-SSH-Anmeldeinformationen.

   ![Apache Ranger-Administratoroberfläche](./media/apache-domain-joined-run-kafka/apache-ranger-admin-login.png)

## <a name="create-domain-users"></a>Erstellen von Domänenbenutzern

Besuchen Sie [Erstellen eines in die Domäne eingebundenen HDInsight-Clusters](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds#create-a-domain-joined-hdinsight-cluster), um Informationen zum Erstellen der Domänenbenutzer **sales_user** und **marketing_user** zu erhalten. In einem Produktionsszenario stammen Domänenbenutzer aus Ihrem Active Directory-Mandanten.

## <a name="create-ranger-policy"></a>Erstellen der Ranger-Richtlinie 

Erstellen Sie eine Ranger-Richtlinie für **sales_user** und **marketing_user**.

1. Öffnen Sie die **Ranger-Administratoroberfläche**.

2. Klicken Sie unter **Kafka** auf **\<ClusterName>_kafka**. Möglicherweise ist eine vorkonfigurierte Richtlinie aufgelistet.

3. Klicken Sie auf **Neue Richtlinie hinzufügen**, und geben Sie die folgenden Werte ein:

   |**Einstellung**  |**Empfohlener Wert**  |
   |---------|---------|
   |Richtlinienname  |  hdi sales*-Richtlinie   |
   |Thema   |  sales* |
   |Benutzer auswählen  |  sales_user1 |
   |Berechtigungen  | Veröffentlichen, Nutzen, Erstellen |

   Die folgenden Platzhalter können im Themennamen enthalten sein:

   * „*“ weist auf null (0) oder mehr Vorkommen von Zeichen hin.
   * „?“ weist auf ein einzelnes Zeichen hin.

   ![Apache Ranger-Administratoroberfläche – Richtlinie erstellen](./media/apache-domain-joined-run-kafka/apache-ranger-admin-create-policy.png)   

   >[!NOTE] 
   >Warten Sie kurz, bis Ranger mit AAD synchronisiert ist, wenn unter **Benutzer auswählen** nicht automatisch ein Domänenbenutzer eingetragen wird.

4. Klicken Sie auf **Hinzufügen**, um die Richtlinie zu speichern.

5. Klicken Sie auf **Neue Richtlinie hinzufügen**, und geben Sie die folgenden Werte ein:

   |**Einstellung**  |**Empfohlener Wert**  |
   |---------|---------|
   |Richtlinienname  |  hdi marketing-Richtlinie   |
   |Thema   |  marketingspend |
   |Benutzer auswählen  |  marketing_user1 |
   |Berechtigungen  | Veröffentlichen, Nutzen, Erstellen |

   ![Apache Ranger-Administratoroberfläche – Richtlinie erstellen](./media/apache-domain-joined-run-kafka/apache-ranger-admin-create-policy-2.png)  

6. Klicken Sie auf **Hinzufügen**, um die Richtlinie zu speichern.

## <a name="create-topics-in-a-kafka-cluster-with-esp"></a>Erstellen von Themen in einem Kafka-Cluster mit ESP

So erstellen Sie zwei Themen, **salesevents** und **marketingspend**:

1. Führen Sie folgenden Befehl aus, um eine SSH-Verbindung mit dem Cluster zu öffnen:

   ```bash
   ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net
   ```

   Ersetzen Sie `SSHUSER` durch den SSH-Benutzer für Ihren Cluster und `CLUSTERNAME` durch den Namen Ihres Clusters. Geben Sie bei entsprechender Aufforderung das Kennwort für das SSH-Benutzerkonto ein. Weitere Informationen zum Verwenden von `scp` mit HDInsight finden Sie unter [Verwenden von SSH mit HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix).

   In einem Produktionsszenario können während der Clustererstellung konfigurierte Domänenbenutzer eine SSH-Verbindung mit dem Cluster herstellen.

2. Um den Clusternamen in einer Variablen zu speichern und ein JSON-Analysehilfsprogramm (`jq`) zu installieren, verwenden Sie die folgenden Befehle. Geben Sie bei entsprechender Aufforderung den Namen des Kafka-Clusters ein.

   ```bash
   sudo apt -y install jq
   read -p 'Enter your Kafka cluster name:' CLUSTERNAME
   ```

3. Um die Kafka-Brokerhosts und die Zookeeper-Hosts abzurufen, verwenden Sie die folgenden Befehle. Geben Sie bei entsprechender Aufforderung das Kennwort des Administratorkontos für den Cluster ein.

   ```bash
   export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`; \
   export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`; \
   ```
> [!Note]
> Vor dem Fortfahren müssen Sie unter Umständen Ihre Entwicklungsumgebung einrichten, falls Sie dies nicht bereits getan haben. Sie benötigen Komponenten wie Java JDK, Apache Maven und einen SSH-Client mit scp. Ausführlichere Informationen finden Sie in der [Setupanleitung](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/tree/master/DomainJoined-Producer-Consumer).
1. Laden Sie die [Apache Kafka-Producer/Consumer-Beispiele mit Domäneneinbindung](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/tree/master/DomainJoined-Producer-Consumer) herunter.

1. Führen Sie unter [Tutorial: Verwenden der Apache Kafka Producer- und Consumer-APIs](https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-producer-consumer-api#build-and-deploy-the-example) die Schritte 2 und 3 im Abschnitt **Erstellen und Bereitstellen des Beispiels** aus.

1. Führen Sie die folgenden Befehle aus:

   ```bash
   java -jar -Djava.security.auth.login.config=/usr/hdp/current/kafka-broker/config/kafka_client_jaas.conf kafka-producer-consumer.jar create salesevents $KAFKABROKERS
   java -jar -Djava.security.auth.login.config=/usr/hdp/current/kafka-broker/config/kafka_client_jaas.conf kafka-producer-consumer.jar create marketingspend $KAFKABROKERS
   ```

   >[!NOTE] 
   >Nur der Besitzer des Prozesses des Kafka-Diensts, z.B. „root“, kann in Zookeeper-ZNodes `/config/topics` schreiben. Ranger-Richtlinien werden nicht erzwungen, wenn ein Benutzer ohne Berechtigungen ein Thema erstellt. Der Grund hierfür ist, dass das `kafka-topics.sh`-Skript zum Erstellen des Themas direkt mit Zookeeper kommuniziert. Den Zookeeper-Knoten werden Einträge hinzugefügt, während die Watcher auf der Brokerseite Themen entsprechend überwachen und erstellen. Die Autorisierung kann nicht über das Ranger-Plug-In ausgeführt werden, und der obige Befehl wird mithilfe von `sudo` über den Kafka-Broker ausgeführt.


## <a name="test-the-ranger-policies"></a>Testen der Ranger-Richtlinien

Basierend auf den konfigurierten Ranger-Richtlinien kann **sales_user** für das Thema **salesevents** produzieren/es nutzen, aber dies gilt nicht für Thema **marketingspend**. Umgekehrt kann **marketing_user** für das Thema **marketingspend** produzieren/es nutzen, aber dies gilt nicht für Thema **salesevents**.

1. Stellen Sie eine neue SSH-Verbindung mit dem Cluster her. Führen Sie den folgenden Befehl aus, um sich als **sales_user1** anzumelden:

   ```bash
   ssh sales_user1@CLUSTERNAME-ssh.azurehdinsight.net
   ```

2. Führen Sie den folgenden Befehl aus:

   ```bash
   export KAFKA_OPTS="-Djava.security.auth.login.config=/usr/hdp/current/kafka-broker/config/kafka_client_jaas.conf"
   ```

3. Verwenden Sie die Broker- und Zookeeper-Namen aus dem vorherigen Abschnitt, um die folgenden Umgebungsvariablen festzulegen:

   ```bash
   export KAFKABROKERS=<brokerlist>:9092 
   ```

   Beispiel: `export KAFKABROKERS=wn0-khdicl.contoso.com:9092,wn1-khdicl.contoso.com:9092`

   ```bash
   export KAFKAZKHOSTS=<zklist>:2181
   ```

   Beispiel: `export KAFKAZKHOSTS=zk1-khdicl.contoso.com:2181,zk2-khdicl.contoso.com:2181`

4. Überprüfen Sie, ob **sales_user1** für das Thema **salesevents** produzieren kann.
   
   Führen Sie den folgenden Befehl zum Starten des Console-Producers für Thema **salesevents** aus:

   ```bash
   /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic salesevents --security-protocol SASL_PLAINTEXT
   ```

   Geben Sie dann einige Nachrichten in die Konsole ein. Drücken Sie **STRG+C**, um den Console-Producer zu beenden.

5. Führen Sie den folgenden Befehl zum Nutzen des Themas **salesevents** aus:

   ```bash
   /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --topic salesevents --security-protocol PLAINTEXTSASL --from-beginning
   ```
 
6. Überprüfen Sie, ob die Nachrichten, die Sie im vorherigen Schritt eingegeben haben, angezeigt werden, und ob **sales_user1** nicht für Thema **marketingspend** produzieren kann.

   Führen Sie im gleichen SSH-Fenster wie oben den folgenden Befehl aus, um für das Thema **marketingspend** zu produzieren:

   ```bash
   /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic marketingspend --security-protocol SASL_PLAINTEXT
   ```

   Ein Autorisierungsfehler tritt auf und kann ignoriert werden. 

7. Beachten Sie, dass **marketing_user1** Thema **salesevents** nicht nutzen kann.

   Wiederholen Sie die obigen Schritte 1-3, aber dieses Mal als **marketing_user1**.

   Führen Sie den folgenden Befehl zum Nutzen des Themas **salesevents** aus:

   ```bash
   /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --topic marketingspend --security-protocol PLAINTEXTSASL --from-beginning
   ```

   Vorherige Meldungen werden nicht angezeigt.

8. Zeigen Sie die Überwachungszugriffsereignisse in der Ranger-Benutzeroberfläche an.

   ![Ranger-Benutzeroberfläche – Richtlinienüberwachung](./media/apache-domain-joined-run-kafka/apache-ranger-admin-audit.png)

## <a name="next-steps"></a>Nächste Schritte

* [Bring Your Own Key für Apache Kafka auf Azure HDInsight (Vorschau)](https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-byok)
* [Einführung in die Hadoop-Sicherheit mit dem Enterprise-Sicherheitspaket](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-introduction)
