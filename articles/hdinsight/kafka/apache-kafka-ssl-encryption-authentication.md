---
title: Einrichten von SSL-Verschlüsselung und -Authentifizierung für Apache Kafka in Azure HDInsight
description: Einrichten von SSL-Verschlüsselung für die Kommunikation zwischen Kafka-Clients und Kafka-Brokern sowie zwischen Kafka-Brokern. Einrichten von SSL-Authentifizierung von Clients.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/15/2019
ms.author: hrasheed
ms.openlocfilehash: decaf892d40de75638d05dc16e999a673dce21e5
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55477028"
---
# <a name="setup-secure-sockets-layer-ssl-encryption-and-authentication-for-apache-kafka-in-azure-hdinsight"></a>Einrichten von Secure Sockets Layer-Verschlüsselung (SSL) und -Authentifizierung für Apache Kafka in Azure HDInsight

In diesem Artikel wird beschrieben, wie Sie SSL-Verschlüsselung zwischen Apache Kafka-Clients und Apache Kafka-Brokern einrichten. Außerdem enthält der Artikel die Schritte, die erforderlich sind, um die Authentifizierung von Clients einzurichten (auch als bidirektionaler SSL bezeichnet).

## <a name="server-setup"></a>Einrichten des Servers

Der erste Schritt besteht im Erstellen eines Keystore und eines Truststore auf jedem Kafka-Broker. Nachdem diese erstellt wurden, importieren Sie die Zertifizierungsstellen- und die Brokerzertifikate in diese Speicher.

> [!Note] 
> In dieser Anleitung werden selbstsignierte Zertifikate verwendet, aber die sicherste Lösung ist die Verwendung von Zertifikaten, die von vertrauenswürdigen Zertifizierungsstellen ausgestellt wurden.

Gehen Sie folgendermaßen vor, um das Serversetup abzuschließen:

1. Erstellen Sie einen Ordner namens „ssl“, und exportieren Sie das Serverkennwort als Umgebungsvariable. Ersetzen Sie `<server_password>` für den Rest dieser Anleitung durch das tatsächliche Administratorkennwort für den Server.
1. Erstellen Sie danach einen Java-Keystore (kafka.server.keystore.jks) und ein Zertifizierungsstellenzertifikat.
1. Erstellen Sie anschließend eine Signieranforderung, damit das Zertifikat, das Sie im vorherigen Schritt erstellt haben, von der Zertifizierungsstelle signiert wird.
1. Senden Sie nun die Signieranforderung an die Zertifizierungsstelle, damit dieses Zertifikat signiert wird. Da ein selbstsigniertes Zertifikat verwendet wird, wird das Zertifikat mit der eigenen Zertifizierungsstelle mit dem Befehl `openssl` signiert.
1. Erstellen Sie einen Vertrauensspeicher, und importieren Sie das Zertifikat der Zertifizierungsstelle.
1. Importieren Sie das öffentliche Zertifikat der Zertifizierungsstelle in den Keystore.
1. Importieren Sie das signierte Zertifikat in den Keystore.

Die Befehle zum Ausführen dieser Schritte werden im folgenden Codeausschnitt gezeigt.

```bash
export SRVPASS=<server_password>
mkdir ssl
cd ssl

# Create a java keystore (kafka.server.keystore.jks) and a CA certificate.

keytool -genkey -keystore kafka.server.keystore.jks -validity 365 -storepass $SRVPASS -keypass $SRVPASS -dname "CN=wn0-umakaf.xvbseke35rbuddm4fyvhm2vz2h.cx.internal.cloudapp.net" -storetype pkcs12

# Create a signing request to get the certificate created in the previous step signed by the CA.

keytool -keystore kafka.server.keystore.jks -certreq -file cert-file -storepass $SRVPASS -keypass $SRVPASS

# Send the signing request to the CA and get this certificate signed.

openssl x509 -req -CA ca-cert -CAkey ca-key -in cert-file -out cert-signed -days 365 -CAcreateserial -passin pass:$SRVPASS

# Create a trust store and import the certificate of the CA.

keytool -keystore kafka.server.truststore.jks -alias CARoot -import -file ca-cert -storepass $SRVPASS -keypass $SRVPASS -noprompt

# Import the public CA certificate into the keystore.

keytool -keystore kafka.server.keystore.jks -alias CARoot -import -file ca-cert -storepass $SRVPASS -keypass $SRVPASS -noprompt

# Import the signed certificate into the keystore.

keytool -keystore kafka.server.keystore.jks -alias CARoot -import -file ca-cert -storepass $SRVPASS -keypass $SRVPASS -noprompt

# The output should say "Certificate reply was added to keystore"
```

Importieren des signierten Zertifikats in den Keystore ist der letzte Schritt, der dazu erforderlich ist, den Truststore und den Keystore für einen Kafka-Broker zu konfigurieren.

## <a name="update-kafka-configuration-to-use-ssl-and-restart-brokers"></a>Aktualisieren der Kafka-Konfiguration zur Verwendung von SSL und Neustarten der Broker

Sie haben nun jeden Kafka-Broker mit einem Keystore und einem Truststore erstellt und die richtigen Zertifikate importiert.  Ändern Sie nun die zugehörigen Kafka-Konfigurationseigenschaften mit Ambari, und starten Sie die Kafka-Broker dann neu. 

Führen Sie die folgenden Schritte aus, um die Konfigurationsänderung abzuschließen:

1. Melden Sie sich im Azure-Portal an, und wählen Sie Ihren Azure HDInsight Apache Kafka-Cluster aus.
1. Wechseln Sie in die Ambari-Benutzeroberfläche, indem Sie unter **Clusterdashboards** auf **Ambari Home** klicken.
1. Legen Sie unter **Kafka-Broker** die Eigenschaft **Listener** auf `PLAINTEXT://localhost:9092,SSL://localhost:9093` fest.
1. Legen Sie unter **Erweiterter Kafka-Broker** die Eigenschaft **security.inter.broker.protocol** auf `SSL` fest.

    ![Bearbeiten von Kafka-SSL-Konfigurationseigenschaften in Ambari](./media/apache-kafka-ssl-encryption-authentication/editing-configuration-ambari.png)

1. Legen Sie unter **Benutzerdefinierter Kafka-Broker** die Eigenschaft **ssl.client.auth** auf `required` fest. Dieser Schritt ist nur erforderlich, wenn Sie sowohl die Authentifizierung als auch die Verschlüsselung einrichten.

    ![Bearbeiten von Kafka-SSL-Konfigurationseigenschaften in Ambari](./media/apache-kafka-ssl-encryption-authentication/editing-configuration-ambari2.png)

1. Fügen Sie Konfigurationseigenschaften zur Kafka-Datei `server.properties` hinzu, um IP-Adressen anstelle des vollständig qualifizierten Domänennamens (FQDN) anzukündigen.

    ```bash
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092,SSL://$IP_ADDRESS:9093" >> /usr/hdp/current/kafka-broker/conf/server.properties
    echo "ssl.keystore.location=/home/sshuser/ssl/kafka.server.keystore.jks" >> /usr/hdp/current/kafka-broker/conf/server.properties
    echo "ssl.keystore.password=<server_password>" >> /usr/hdp/current/kafka-broker/conf/server.properties
    echo "ssl.key.password=<server_password>" >> /usr/hdp/current/kafka-broker/conf/server.properties
    echo "ssl.truststore.location=/home/sshuser/ssl/kafka.server.truststore.jks" >> /usr/hdp/current/kafka-broker/conf/server.properties
    echo "ssl.truststore.password=<server_password>" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

1. Um zu überprüfen, ob die vorherigen Änderungen ordnungsgemäß vorgenommen wurden, können Sie feststellen, ob die folgenden Zeilen in der Kafka-Datei `server.properties` vorhanden sind.

    ```bash
    advertised.listeners=PLAINTEXT://10.0.0.11:9092,SSL://10.0.0.11:9093
    ssl.keystore.location=/home/sshuser/ssl/kafka.server.keystore.jks
    ssl.keystore.password=<server_password>
    ssl.key.password=<server_password>
    ssl.truststore.location=/home/sshuser/ssl/kafka.server.truststore.jks
    ssl.truststore.password=<server_password>
    ```

1. Starten Sie alle Kafka-Broker neu.

## <a name="client-setup-with-authentication"></a>Einrichten des Clients (mit Authentifizierung)

> [!Note]
> Die folgenden Schritte sind nur erforderlich, wenn Sie SSL-Verschlüsselung **und** Authentifizierung einrichten. Wenn Sie nur Verschlüsselung einrichten, wechseln Sie zu [Einrichten des Clients (ohne Authentifizierung)](apache-kafka-ssl-encryption-authentication.md#client-setup-without-authentication).

Führen Sie zum Abschließen des Clientsetups die folgenden Schritte aus:

1. Melden Sie sich beim Clientcomputer (hn1) an.
1. Exportieren Sie das Clientkennwort. Ersetzen Sie `<client_password>` durch das tatsächliche Administratorkennwort auf dem Kafka-Clientcomputer.
1. Erstellen Sie einen Java-Keystore, und rufen Sie ein signiertes Zertifikat für den Broker ab. Kopieren Sie dann das Zertifikat auf den virtuellen Computer, auf dem die Zertifizierungsstelle ausgeführt wird.
1. Wechseln Sie zum Zertifizierungsstellencomputer (wn0), um das Clientzertifikat zu signieren.
1. Wechseln Sie zum Clientcomputer (hn1), und navigieren Sie zum Ordner `~/ssl`. Kopieren Sie das signierte Zertifikat auf den Clientcomputer.

```bash
export CLIPASS=<client_password>
cd ssl

# Create a java keystore and get a signed certificate for the broker. Then copy the certificate to the VM where the CA is running.

keytool -genkey -keystore kafka.client.keystore.jks -validity 365 -storepass $CLIPASS -keypass $CLIPASS -dname "CN=mylaptop1" -alias my-local-pc1 -storetype pkcs12

keytool -keystore kafka.client.keystore.jks -certreq -file client-cert-sign-request -alias my-local-pc1 -storepass $CLIPASS -keypass $CLIPASS

# Copy the cert to the vm where the CA is
scp client-cert-sign-request3 sshuser@wn0-umakaf:~/tmp1/client-cert-sign-request

# Switch to the CA machine (wn0) to sign the client certificate.
cd ssl
openssl x509 -req -CA ca-cert -CAkey ca-key -in /tmp1/client-cert-sign-request -out /tmp1/client-cert-signed -days 365 -CAcreateserial -passin pass:<server_password>

# Return to the client machine (hn1), navigate to ~/ssl folder and copy signed cert to client machine
scp -i ~/kafka-security.pem sshuser@wn0-umakaf:/tmp1/client-cert-signed

# Import CA cert to trust store
keytool -keystore kafka.client.truststore.jks -alias CARoot -import -file ca-cert -storepass $CLIPASS -keypass $CLIPASS -noprompt

# Import CA cert to key store
keytool -keystore kafka.client.keystore.jks -alias CARoot -import -file ca-cert -storepass $CLIPASS -keypass $CLIPASS -noprompt

# Import signed client (cert client-cert-signed1) to keystore
keytool -keystore kafka.client.keystore.jks -import -file client-cert-signed -alias my-local-pc1 -storepass $CLIPASS -keypass $CLIPASS -noprompt
```

Zeigen Sie schließlich die Datei `client-ssl-auth.properties` mit dem Befehl `cat client-ssl-auth.properties` an. Die Datei sollte folgende Zeilen enthalten:

```bash
security.protocol=SSL
ssl.truststore.location=/home/sshuser/ssl/kafka.client.truststore.jks
ssl.truststore.password=<client_password>
ssl.keystore.location=/home/sshuser/ssl/kafka.client.keystore.jks
ssl.keystore.password=<client_password>
ssl.key.password=<client_password>
```

## <a name="client-setup-without-authentication"></a>Einrichten des Clients (ohne Authentifizierung)

Wenn Sie keine Authentifizierung benötigen, dienen folgende Schritte nur zum Einrichten von SSL-Verschlüsselung:

1. Melden Sie sich beim Clientcomputer (hn1) an, und navigieren Sie zum Ordner `~/ssl`.
1. Exportieren Sie das Clientkennwort. Ersetzen Sie `<client_password>` durch das tatsächliche Administratorkennwort auf dem Kafka-Clientcomputer.
1. Kopieren Sie das signierte Zertifikat vom Computer der Zertifizierungsstelle (wn0) auf den Clientcomputer.
1. Importieren Sie das Zertifizierungsstellenzertifikat in den Truststore.
1. Importieren Sie das Zertifizierungsstellenzertifikat in den Keystore.

Diese Schritte werden im folgenden Codeausschnitt gezeigt.

```bash
export CLIPASS=<client_password>
cd ssl

# Copy signed cert to client machine
scp -i ~/kafka-security.pem sshuser@wn0-umakaf:/home/sshuser/ssl/ca-cert .

# Import CA cert to truststore
keytool -keystore kafka.client.truststore.jks -alias CARoot -import -file ca-cert -storepass $CLIPASS -keypass $CLIPASS -noprompt

# Import CA cert to keystore
keytool -keystore kafka.client.keystore.jks -alias CARoot -import -file ca-cert -storepass $CLIPASS -keypass $CLIPASS -noprompt
```

Zeigen Sie schließlich die Datei `client-ssl-auth.properties` mit dem Befehl `cat client-ssl-auth.properties` an. Die Datei sollte folgende Zeilen enthalten:

```bash
security.protocol=SSL
ssl.truststore.location=/home/sshuser/ssl/kafka.client.truststore.jks
ssl.truststore.password=<client_password>
```

## <a name="next-steps"></a>Nächste Schritte

* [Was ist Apache Kafka in HDInsight?](apache-kafka-introduction.md)