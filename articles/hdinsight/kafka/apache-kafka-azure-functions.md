---
title: Verwenden von Azure Functions zum Senden von Daten an Kafka in HDInsight | Microsoft Docs
description: Erfahren Sie, wie eine Azure-Funktion verwendet wird, um Daten in Kafka in HDInsight zu schreiben.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/09/2018
ms.author: larryfr
ms.openlocfilehash: 6c462f9fe5e152c82be1a2b8643ee35d260a90f6
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="use-kafka-on-hdinsight-from-an-azure-function-app"></a>Verwenden von Kafka in HDInsight aus einer Azure Functions-App

Erfahren Sie, wie eine Azure Functions-App erstellt wird, die Daten an Kafka in HDInsight sendet.

[Azure Functions](https://docs.microsoft.com/azure/azure-functions/) ist ein serverloser Computedienst. Mit ihm können Sie Code bedarfsgesteuert ausführen, ohne eine Infrastruktur explizit bereitstellen oder verwalten zu müssen.

[Apache Kafka](https://kafka.apache.org) ist eine verteilte Open Source-Streamingplattform, die zum Erstellen von Datenpipelines und Anwendungen mit Echtzeitstreaming verwendet werden kann. Kafka verfügt auch über Nachrichtenbrokerfunktionen, die einer Nachrichtenwarteschlange ähneln, über die Sie benannte Datenströme veröffentlichen und diese abonnieren können. Bei Kafka in HDInsight erhalten Sie einen verwalteten, hoch skalierbaren und hoch verfügbaren Dienst in der Microsoft Azure-Cloud.

Kafka in HDInsight stellt keine API im öffentlichen Internet bereit. Zum Veröffentlichen oder Nutzen von Daten aus Kafka müssen Sie eine Verbindung mit dem Kafka-Cluster mit einem Microsoft Azure Virtual Network herstellen. Azure Functions bietet eine einfache Möglichkeit, einen öffentlichen Endpunkt zu erstellen, der Daten über ein Gateway eines virtuellen Netzwerks in Kafka in HDInsight pusht.

> [!NOTE]
> Der Schwerpunkt dieses Dokuments liegt auf den Schritten, die erforderlich sind, um die Kommunikation von Azure Functions mit Kafka in HDInsight zu ermöglichen. Als eigentliches Beispiel dient nur ein Kafka-Basisproducer, um die Funktionsweise der Konfiguration zu veranschaulichen.

## <a name="prerequisites"></a>Voraussetzungen

* Eine Azure Functions-App. Weitere Informationen finden Sie im Dokument [Erstellen Ihrer ersten Funktion](../../azure-functions/functions-create-first-azure-function.md).

* Ein Azure Virtual Network. Das virtuelle Netzwerk muss einen der folgenden IP-Adressbereiche verwenden, um mit Azure Functions zusammenarbeiten zu können:

    * 10.0.0.0-10.255.255.255
    * 172.16.0.0-172.31.255.255
    * 192.168.0.0-192.168.255.255

    Weitere Informationen finden Sie im Dokument [Integrieren einer App in ein Azure Virtual Network](../../app-service/web-sites-integrate-with-vnet.md).

* Ein Kafka-Cluster in HDInsight. Weitere Informationen zum Erstellen eines Kafka-Clusters in HDInsight finden Sie im Dokument [Erstellen eines Kafka-Clusters](apache-kafka-get-started.md).

    > [!IMPORTANT]
    > Während der Clusterkonfiguration müssen Sie den Schritt __Erweiterte Einstellungen__ zum Auswählen des Azure Virtual Network und des Subnetzes verwenden, das HDInsight verwendet. Wählen Sie das im vorherigen Schritt erstellte virtuelle Netzwerk und Subnetz aus.

    Weitere Informationen zu Kafka und virtuellen Netzwerken finden Sie im Dokument [Herstellen einer Verbindung mit Kafka über ein virtuelles Netzwerk](apache-kafka-connect-vpn-gateway.md).

## <a name="architecture"></a>Architecture

Kafka in HDInsight ist in einem Azure Virtual Network enthalten. Azure Functions kann mit dem virtuellen Netzwerk kommunizieren, indem ein Point-to-Site-Gateway verwendet wird. Die folgende Abbildung zeigt ein Diagramm dieser Netzwerktopologie:

![Architektur der Verbindung von Azure Functions mit HDInsight](./media/apache-kafka-azure-functions/kafka-azure-functions.png)

## <a name="configure-kafka"></a>Konfigurieren von Kafka

Die Informationen in diesem Abschnitt bereiten den Kafka-Cluster darauf vor, Daten aus der Azure-Funktion zu akzeptieren. Die folgenden Konfigurationsaktionen werden behandelt:

* IP-Ankündigung
* Erfassen von Kafka-IP-Brokeradressen
* Erstellen eines Kafka-Themas

### <a name="configure-kafka-for-ip-advertising"></a>Konfigurieren von Kafka zum Ankündigen der IP-Adresse

Standardmäßig gibt Zookeeper den Domänennamen der Kafka-Broker an Clients zurück. Diese Konfiguration funktioniert nicht ohne einen DNS-Server, weil der Client (Azure Functions) keine Namen für das virtuelle Netzwerk auflösen kann. Verwenden Sie für diese Konfiguration die folgenden Schritte, um Kafka zum Ankündigen von IP-Adressen anstelle von Domänennamen zu konfigurieren:

1. Wechseln Sie in Ihrem Webbrowsers zu https://CLUSTERNAME.azurehdinsight.net. Ersetzen Sie __CLUSTERNAME__ durch den Namen von Kafka im HDInsight-Cluster.

    Geben Sie bei entsprechender Aufforderung den HTTPS-Benutzernamen und das Kennwort für den Cluster ein. Die Ambari-Webbenutzeroberfläche für den Cluster wird angezeigt.

2. Wählen Sie zum Anzeigen von Informationen über Kafka aus der Liste auf der linken Seite __Kafka__ aus.

    ![Dienstliste, in der Kafka hervorgehoben ist](./media/apache-kafka-azure-functions/select-kafka-service.png)

3. Wählen Sie zum Anzeigen der Kafka-Konfiguration oben in der Mitte __Konfigurationen__ aus.

    ![Konfigurationslinks für Kafka](./media/apache-kafka-azure-functions/select-kafka-config.png)

4. Geben Sie zum Suchen der Konfiguration __kafka-env__ oben rechts in das Feld __Filter__ die Zeichenfolge `kafka-env` ein.

    ![Kafka-Konfiguration für kafka-env](./media/apache-kafka-azure-functions/search-for-kafka-env.png)

5. Um Kafka zum Ankündigen von IP-Adressen zu konfigurieren, fügen Sie den folgenden Text am Ende des Felds __Vorlage für kafka-env__ hinzu:

    ```
    # Configure Kafka to advertise IP addresses instead of FQDN
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

6. Geben Sie zum Konfigurieren der Schnittstelle, an der Kafka lauscht, oben rechts in das Feld __Filter__ die Zeichenfolge `listeners` ein.

7. Um Kafka zum Lauschen an allen Netzwerkschnittstellen zu konfigurieren, ändern Sie den Wert im Feld __Listener__ in `PLAINTEXT://0.0.0.0:9092`.

8. Klicken Sie auf die Schaltfläche __Speichern__, um die Konfigurationsänderungen zu speichern. Geben Sie eine Textnachricht ein, die die Änderungen beschreibt. Wählen Sie __OK__ aus, nachdem die Änderungen gespeichert wurden.

    ![Schaltfläche zum Speichern der Konfiguration](./media/apache-kafka-azure-functions/save-button.png)

9. Um Fehler beim Neustart von Kafka zu vermeiden, verwenden Sie die Schaltfläche __Dienstaktionen__, und wählen Sie __Wartungsmodus aktivieren__ aus. Wählen Sie „OK“ aus, um diesen Vorgang abzuschließen.

    ![Dienstaktionen, „Wartungsmodus aktivieren“ hervorgehoben](./media/apache-kafka-azure-functions/turn-on-maintenance-mode.png)

10. Verwenden Sie zum Neustarten von Kafka die Schaltfläche __Neu starten__, und wählen Sie __Alle betroffenen Instanzen neu starten__ aus. Bestätigen Sie den Neustart, und verwenden Sie dann die Schaltfläche __OK__, wenn der Vorgang abgeschlossen ist.

    ![Schaltfläche „Neu starten“, Funktion zum Neustarten aller betroffenen Instanzen hervorgehoben](./media/apache-kafka-azure-functions/restart-button.png)

11. Um den Wartungsmodus zu deaktivieren, verwenden die Schaltfläche __Dienstaktionen__, und wählen Sie __Wartungsmodus deaktivieren__ aus. Wählen Sie **OK** aus, um diesen Vorgang abzuschließen.

### <a name="get-the-kafka-broker-ip-address"></a>Abrufen der Kafka-IP-Brokeradresse

Verwenden Sie eine der folgenden Methoden, um den vollqualifizierten Domänennamen (FQDN) und die IP-Adressen der Knoten im Kafka-Cluster abzurufen:

```powershell
$resourceGroupName = "The resource group that contains the virtual network used with HDInsight"

$clusterNICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName | where-object {$_.Name -like "*node*"}

$nodes = @()
foreach($nic in $clusterNICs) {
    $node = new-object System.Object
    $node | add-member -MemberType NoteProperty -name "Type" -value $nic.Name.Split('-')[1]
    $node | add-member -MemberType NoteProperty -name "InternalIP" -value $nic.IpConfigurations.PrivateIpAddress
    $node | add-member -MemberType NoteProperty -name "InternalFQDN" -value $nic.DnsSettings.InternalFqdn
    $nodes += $node
}
$nodes | sort-object Type
```

```azurecli
az network nic list --resource-group <resourcegroupname> --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
```

Dieser Befehl setzt voraus, dass `<resourcegroupname>` der Name der Azure-Ressourcengruppe ist, die das virtuelle Netzwerk enthält.

Wählen Sie aus der Liste der Namen, die zurückgegeben wird, die IP-Adresse eines Workerknotens aus. Der intern vollqualifizierte Domänenname des Knotens beginnt mit den Buchstaben `wn`. Diese IP-Adresse wird von der Funktion für die Kommunikation mit Kafka verwendet.

### <a name="create-a-kafka-topic"></a>Erstellen eines Kafka-Themas

Kafka speichert Daten in __Themen__. Vor dem Senden von Daten aus einer Azure-Funktion an Kafka müssen Sie die Funktion erstellen.

Um ein Thema zu erstellen, verwenden Sie die Schritte im Dokument [Erstellen eines Kafka-Clusters](apache-kafka-get-started.md).

## <a name="create-a-function-that-sends-to-kafka"></a>Erstellen einer Funktion, die an Kafka sendet

> [!NOTE]
> Die Schritte in diesem Abschnitt erstellen eine JavaScript-Funktion, die das Paket [kafka-node](https://www.npmjs.com/package/kafka-node) zum Veröffentlichen von Daten für Kafka verwendet:

1. Erstellen Sie eine neue Funktion __WebHook und API__, und wählen Sie __JavaScript__ als Sprache aus. Weitere Informationen zum Erstellen neuer Funktionen finden Sie im Dokument [Erstellen Ihrer ersten Funktion](../../azure-functions/functions-create-first-azure-function.md#create-function).

2. Verwenden Sie den folgenden Code als Textkörper der Funktion:

    ```javascript
    var kafka = require('kafka-node');

    // The topic and a Kafka broker host for your HDInsight cluster
    var topic = 'test';
    var brokerHost = '10.1.0.7:9092';
    // Create the client
    var client= new kafka.KafkaClient({kafkaHost: brokerHost});

    module.exports = function (context, req) {
        // Create the producer
        var producer = new kafka.Producer(client, {requireAcks: 1});

        context.log('JavaScript HTTP trigger function processed a request.');

        if (req.query.name || (req.body && req.body.name)) {
            var name = req.query.name || req.body.name
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: "Hello " + name
            };
            // Create the payload, using the name as the body
            var payloads = [
                    { topic: topic, messages: [name]}
            ];
            context.log("calling producer.send");
            // Send the data to Kafka
            producer.send(payloads, function(err, data) {
                if(err) {
                    context.log(err);
                } else {
                    context.log(data);
                }
            });
        }
        else {
            context.res = {
                status: 400,
                body: "Please pass a name on the query string or in the request body"
            };
        }
        context.done();
    };
    ```

    Ersetzen Sie `'test'` durch den Namen des Kafka-Themas, das Sie in Ihrem HDInsight-Cluster erstellt haben.

    Ersetzen Sie `10.1.0.7` durch die IP-Adresse, die Sie zuvor abgerufen haben. Lassen Sie den Wert `:9092` unverändert. 9092 ist der Port, an dem Kafka lauscht.

    Verwenden Sie die Schaltfläche __Save__ , um die Änderungen zu speichern.

    ![Editor mit Schaltfläche „Speichern“](./media/apache-kafka-azure-functions/function-editor.png)

3. Wählen Sie auf der rechten Seite des Funktionen-Editors __Dateien anzeigen__ aus. Wählen Sie __+ Hinzufügen__ aus, und fügen Sie eine neue Datei mit dem Namen `package.json` hinzu. Diese Datei wird verwendet, um die Abhängigkeit vom `kafka-node`-Paket anzugeben.

    ![Hinzufügen einer Datei](./media/apache-kafka-azure-functions/add-files.png)

4. Wählen Sie zum Bearbeiten der neuen Datei `package.json` aus der Liste __Dateien anzeigen__ aus. Verwenden Sie als Inhalt der Datei den folgenden Text:

    ```json
    {
    "name": "kafkatest",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "author": "",
    "license": "ISC",
    "dependencies": {
            "kafka-node": "^2.4.1"
        }
    }
    ```

    Verwenden Sie die Schaltfläche __Save__ , um die Änderungen zu speichern.

5. Verwenden Sie zum Installieren des `kafka-node`-Pakets den Abschnitt _Knotenversion und Paketverwaltung_ des [Azure Functions JavaScript-Entwicklerhandbuchs](../../azure-functions/functions-reference-node.md#node-version-and-package-management).

    > [!NOTE]
    > Möglicherweise erhalten Sie mehrere Fehler, während das kafka-node-Paket installiert wird. Sie können diese Fehler ignorieren.

## <a name="run-the-function"></a>Ausführen der Funktion

Wählen Sie auf der rechten Seite des Funktionen-Editors __Testen__ aus. Übernehmen Sie die Standardeinstellungen für den Test, und wählen Sie dann __Ausführen__ aus. Während der Test ausgeführt wird, übergibt er einen `name`-Parameter an die Funktion. Dieser Parameter enthält den Wert `Azure`, den die Funktion in Kafka einfügt.

![Screenshot des Testdialogfelds](./media/apache-kafka-azure-functions/function-test-dialog.png)

Wählen Sie zum Anzeigen von Informationen, die von der Funktion während der Testausführung protokolliert wurden, __Protokolle__ am unteren Rand der Seite aus. Führen Sie den Test erneut aus, um Protokollinformationen zu generieren.

![Beispiel für die Ausgabe des Funktionsprotokolls](./media/apache-kafka-azure-functions/function-log.png)

## <a name="verify-the-data-is-in-kafka"></a>Überprüfen der Daten in Kafka

Um sicherzustellen, dass die Daten im Kafka-Thema empfangen wurden, verwenden Sie die Informationen im Abschnitt _Generieren und Nutzen von Datensätzen_ des Dokuments [Erstellen eines Kafka-Clusters](apache-kafka-get-started.md#produce-and-consume-records). Der `kafka-console-consumer` liest Daten aus dem Thema und zeigt eine Liste der im Thema gespeicherten Nachrichten an.

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie die folgenden Links, um Informationen zur Verwendung von Apache Kafka unter HDInsight zu erhalten:

* [Erste Schritte mit Apache Kafka (Vorschau) in HDInsight](apache-kafka-get-started.md)

* [Verwenden von MirrorMaker zum Erstellen eines Replikats von Kafka in HDInsight](apache-kafka-mirroring.md)

* [Verwenden von Apache Storm mit Kafka in HDInsight](../hdinsight-apache-storm-with-kafka.md)

* [Verwenden von Apache Spark mit Kafka in HDInsight](../hdinsight-apache-spark-with-kafka.md)

* [Herstellen einer Verbindung mit Kafka über eine Azure Virtual Network-Instanz](apache-kafka-connect-vpn-gateway.md)
