---
title: Diagnose und Problembehandlung des Azure Cosmos DB Java Async SDK | Microsoft-Dokumentation
description: Verwenden Sie Features wie clientseitige Protokollierung und andere Tools von Drittanbietern, um Probleme im Zusammenhang mit Azure Cosmos DB zu erkennen, zu diagnostizieren und zu beheben.
services: cosmos-db
author: moderakh
ms.service: cosmos-db
ms.topic: troubleshooting
ms.date: 10/28/2018
ms.author: moderakh
ms.devlang: java
ms.component: cosmosdb-sql
ms.openlocfilehash: ee92a5dd474cdf4f32ed2c7327d732a2cfbbbf79
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51632933"
---
# <a name="troubleshooting-issues-when-using-java-async-sdk-with-azure-cosmos-db-sql-api-accounts"></a>Behandlung von Problemen bei der Verwendung des Java Async SDK mit Azure Cosmos DB SQL API-Konten
Dieser Artikel behandelt häufige Probleme, Problemumgehungen, Diagnoseschritte und Tools bei der Verwendung des [Java Async SDK](sql-api-sdk-async-java.md) mit Azure Cosmos DB SQL API-Konten.
Das Java Async SDK bietet eine clientseitige logische Darstellung für den Zugriff auf die Azure Cosmos DB SQL-API. Dieser Artikel beschreibt die Tools und Vorgehensweisen, die Ihnen helfen, wenn Sie auf Probleme stoßen.

Beginnen Sie mit dieser Liste:
    * Sehen Sie sich den Abschnitt [Häufig auftretende Probleme und Problemumgehungen] in diesem Artikel an.
    * Unser SDK ist [Open-Source auf GitHub](https://github.com/Azure/azure-cosmosdb-java), und wir stellen einen [Abschnitt zu Problemen](https://github.com/Azure/azure-cosmosdb-java/issues) bereit, den wir aktiv überwachen. Überprüfen Sie, ob Sie ein ähnliches Problem finden können, für das eine Problemumgehung bereits dokumentiert wurde.
    * Überprüfen Sie die [Leistungstipps](performance-tips-async-java.md), und befolgen Sie die empfohlenen Methoden.
    * Lesen Sie den Rest dieses Artikels. Wenn Sie keine Lösung gefunden haben, legen Sie ein [GitHub-Problem](https://github.com/Azure/azure-cosmosdb-java/issues) an.

## <a name="common-issues-workarounds"></a>Häufig auftretende Probleme und Problemumgehungen

### <a name="network-issues-netty-read-timeout-failure-low-throughput-high-latency"></a>Netzwerkprobleme, Netty-Lesetimeoutfehler, niedriger Durchsatz, hohe Latenz

#### <a name="general-suggestions"></a>Allgemeine Empfehlungen
* Stellen Sie sicher, dass die App in der gleichen Region wie Ihr Cosmos DB-Konto ausgeführt wird. 
* Überprüfen Sie die CPU-Auslastung auf dem Host, auf dem die App ausgeführt wird. Wenn die CPU-Auslastung 90 % oder mehr beträgt, sollten Sie Ihre App ggf. auf einem Host mit einer höheren Konfiguration ausführen oder die Last auf mehrere Computer verteilen.

#### <a name="connection-throttling"></a>Verbindungsdrosselung
Verbindungsdrosselung kann aufgrund eines [Verbindungslimits auf dem Hostcomputer] oder aufgrund von [Azure SNAT-Portauslastung (PAT)] erfolgen:

##### <a name="connection-limit-on-host"></a>Verbindungslimit auf dem Hostcomputer
Einige Linux-Systeme (z.B. „Red Hat“) weisen eine Obergrenze für die Gesamtzahl von geöffneten Dateien auf. Sockets werden in Linux als Dateien implementiert, daher schränkt diese Zahl die Gesamtzahl der Verbindungen ebenfalls ein.
Führen Sie den folgenden Befehl aus:

```bash
ulimit -a
```
Die Anzahl von geöffneten Dateien („nofile“) muss groß genug sein (mindestens das Doppelte der Größe des Verbindungspools). Ausführlichere Informationen finden Sie unter [Leistungstipps](performance-tips-async-java.md).

##### <a name="snat"></a>Azure SNAT-Portauslastung (PAT)

Wenn Ihre App auf einem virtuellen Azure-Computer ohne öffentliche IP-Adresse bereitgestellt wird, werden standardmäßig [Azure SNAT-Ports](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports) verwendet, um Verbindungen mit beliebigen Endpunkten außerhalb Ihres virtuellen Computers herzustellen. Die Anzahl der zulässigen Verbindungen des virtuellen Computers mit dem Cosmos DB-Endpunkt wird durch die [Azure SNAT-Konfiguration](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports) eingeschränkt.

Die Azure SNAT-Ports werden nur verwendet, wenn Ihr virtueller Azure-Computer eine private IP-Adresse besitzt und ein Prozess aus dem virtuellen Computer versucht, eine Verbindung mit einer öffentlichen IP-Adresse herzustellen. Es gibt also zwei Problemumgehungen, um die Einschränkung von Azure SNAT zu vermeiden:
    * Fügen Sie Ihren Azure Cosmos DB-Dienstendpunkt dem Subnetz Ihres Azure-VM-VNETs hinzu, wie unter [Aktivieren des VNET-Dienstendpunkts](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) beschrieben. Wenn der Dienstendpunkt aktiviert ist, werden die Anforderungen nicht mehr von einer öffentlichen IP-Adresse an Cosmos DB gesendet, sondern es wird stattdessen die VNET- und Subnetzidentität gesendet. Diese Änderung kann zu Firewallproblemen führen, wenn nur öffentliche IP-Adressen zulässig sind. Wenn Sie eine Firewall verwenden, fügen Sie der Firewall bei der Aktivierung des Dienstendpunkts mit [VNET ACLs](https://docs.microsoft.com/azure/virtual-network/virtual-networks-acl) ein Subnetz hinzu.
    * Weisen Sie Ihrem virtuellen Azure-Computer eine öffentliche IP-Adresse zu.

#### <a name="http-proxy"></a>HTTP-Proxy

Wenn Sie einen HttpProxy verwenden, stellen Sie sicher, dass Ihr HttpProxy die Anzahl der Verbindungen unterstützen kann, die in der `ConnectionPolicy` des SDK konfiguriert sind.
Andernfalls treten Verbindungsprobleme auf.

#### <a name="invalid-coding-pattern-blocking-netty-io-thread"></a>Ungültiges Codierungsmuster: Blockieren des Netty-E/A-Threads

Das SDK verwendet die [Netty](https://netty.io/)-E/A-Bibliothek für die Kommunikation mit dem Azure Cosmos DB-Dienst. Wir verwenden die Async API und nicht blockierende E/A-APIs von Netty. Die E/A-Aufgaben des SDK werden für E/A-Netty-Threads ausgeführt. Die Anzahl der E/A-Netty-Threads ist so konfiguriert, dass sie mit der Anzahl der CPU-Kerne des App-Computers übereinstimmt. Die Netty-E/A-Threads sind nur für die Verwendung für nicht blockierende Netty-E/A-Aufgaben vorgesehen. Das SDK gibt das API-Aufrufergebnis für einen der Netty-E/A-Threads an den Code der App zurück. Wenn die App nach dem Empfangen von Ergebnissen für den Netty-Thread einen Vorgang mit langer Ausführungszeit für den Netty-Thread durchführt, kann dies dazu führen, dass das SDK nicht über genügend E/A-Threads verfügt, um seine internen E/A-Aufgaben auszuführen. Eine solche App-Codierung kann zu niedrigem Durchsatz, hoher Latenz und `io.netty.handler.timeout.ReadTimeoutException`-Fehlern führen. Die Problemumgehung besteht darin, den Thread zu wechseln, wenn Sie wissen, dass der Vorgang längere Zeit in Anspruch nehmen wird.

   Der folgende Codeausschnitt zeigt beispielsweise, dass schließlich ein Zustand eintreten kann, in dem kein Netty-E/A-Thread zur Verarbeitung von E/A-Aufgaben vorhanden ist, wenn Sie eine Aufgabe mit langer Ausführungszeit im Netty-Thread ausführen, die länger als einige Millisekunden dauert. Als Ergebnis erhalten Sie eine ReadTimeoutException:
```java
@Test
public void badCodeWithReadTimeoutException() throws Exception {
    int requestTimeoutInSeconds = 10;

    ConnectionPolicy policy = new ConnectionPolicy();
    policy.setRequestTimeoutInMillis(requestTimeoutInSeconds * 1000);

    AsyncDocumentClient testClient = new AsyncDocumentClient.Builder()
            .withServiceEndpoint(TestConfigurations.HOST)
            .withMasterKeyOrResourceToken(TestConfigurations.MASTER_KEY)
            .withConnectionPolicy(policy)
            .build();

    int numberOfCpuCores = Runtime.getRuntime().availableProcessors();
    int numberOfConcurrentWork = numberOfCpuCores + 1;
    CountDownLatch latch = new CountDownLatch(numberOfConcurrentWork);
    AtomicInteger failureCount = new AtomicInteger();

    for (int i = 0; i < numberOfConcurrentWork; i++) {
        Document docDefinition = getDocumentDefinition();
        Observable<ResourceResponse<Document>> createObservable = testClient
                .createDocument(getCollectionLink(), docDefinition, null, false);
        createObservable.subscribe(r -> {
                    try {
                        // time consuming work. For example:
                        // writing to a file, computationally heavy work, or just sleep
                        // basically anything which takes more than a few milliseconds
                        // doing such operation on the IO netty thread
                        // without a proper scheduler, will cause problems.
                        // The subscriber will get ReadTimeoutException failure.
                        TimeUnit.SECONDS.sleep(2 * requestTimeoutInSeconds);
                    } catch (Exception e) {
                    }
                },

                exception -> {
                    //will be io.netty.handler.timeout.ReadTimeoutException
                    exception.printStackTrace();
                    failureCount.incrementAndGet();
                    latch.countDown();
                },
                () -> {
                    latch.countDown();
                });
    }

    latch.await();
    assertThat(failureCount.get()).isGreaterThan(0);
}
```
   Die Problemumgehung besteht darin, den Thread zu ändern, in dem Sie zeitaufwändige Aufgaben ausführen. Definieren Sie eine Singletoninstanz des Schedulers für Ihre App:
   ```java
// have a singleton instance of executor and scheduler
ExecutorService ex  = Executors.newFixedThreadPool(30);
Scheduler customScheduler = rx.schedulers.Schedulers.from(ex);
   ```
   Wann immer Sie zeitintensive Aufgaben ausführen müssen (z.B. computeintensive Aufgaben, die die E/A blockieren), ändern Sie den Thread in einen Worker, der durch Ihren `customScheduler` mit der `.observeOn(customScheduler)`-API bereitgestellt wird.
```java
Observable<ResourceResponse<Document>> createObservable = client
        .createDocument(getCollectionLink(), docDefinition, null, false);

createObservable
        .observeOn(customScheduler) // switches the thread.
        .subscribe(
            // ...
        );
```
Durch die Verwendung von `observeOn(customScheduler)` geben Sie den Netty-E/A-Thread frei und wechseln zu Ihrem eigenen benutzerdefinierten Thread, der von customScheduler bereitgestellt wird. Diese Änderung behebt das Problem, und Sie erhalten keine weiteren `io.netty.handler.timeout.ReadTimeoutException`-Fehler.

### <a name="connection-pool-exhausted-issue"></a>Problem „Verbindungspool erschöpft“

`PoolExhaustedException` ist ein clientseitiger Fehler. Wenn dieser Fehler häufiger auftritt, ist das ein Hinweis darauf, dass Ihre App-Workload höher ist als das, was der SDK-Verbindungspool bewältigen kann. Das Erhöhen der Größe des Verbindungspools oder das Verteilen der Last auf mehrere Apps kann hilfreich sein.

### <a name="request-rate-too-large"></a>Anforderungsrate zu groß
Dieser Fehler ist ein serverseitiger Fehler, der anzeigt, dass Sie Ihren bereitgestellten Durchsatz verbraucht haben und es später noch einmal versuchen sollten. Wenn Sie diesen Fehler häufig feststellen, sollten Sie eine Erhöhung des Sammlungsdurchsatzes in Betracht ziehen.

### <a name="failure-connecting-to-azure-cosmos-db-emulator"></a>Fehler beim Herstellen einer Verbindung mit dem Azure Cosmos DB-Emulator

Das HTTPS-Zertifikat des COSMOS DB-Emulators ist selbstsigniert. Damit das SDK mit dem Emulator funktioniert, sollten Sie das Emulatorzertifikat in Java TrustStore importieren. Die Vorgehensweise wird [hier](local-emulator-export-ssl-certificates.md) beschrieben.


## <a name="enable-client-sice-logging"></a>Aktivieren von Client-SDK-Protokollierung

Das Java Async SDK verwendet SLF4j als Protokollierungsfassade, die die Anmeldung bei gängigen Protokollierungsframeworks wie log4j und logback unterstützt.

Wenn Sie beispielsweise log4j als Protokollierungsframework verwenden möchten, fügen Sie Ihrem Java-Klassenpfad die folgenden Bibliotheken hinzu:

```xml
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-log4j12</artifactId>
  <version>${slf4j.version}</version>
</dependency>
<dependency>
  <groupId>log4j</groupId>
  <artifactId>log4j</artifactId>
  <version>${log4j.version}</version>
</dependency>
```

Fügen Sie außerdem eine log4j-Konfiguration hinzu:
```
# this is a sample log4j configuration

# Set root logger level to DEBUG and its only appender to A1.
log4j.rootLogger=INFO, A1

log4j.category.com.microsoft.azure.cosmosdb=DEBUG
#log4j.category.io.netty=INFO
#log4j.category.io.reactivex=INFO
# A1 is set to be a ConsoleAppender.
log4j.appender.A1=org.apache.log4j.ConsoleAppender

# A1 uses PatternLayout.
log4j.appender.A1.layout=org.apache.log4j.PatternLayout
log4j.appender.A1.layout.ConversionPattern=%d %5X{pid} [%t] %-5p %c - %m%n
```

Weitere Informationen finden Sie im [Leitfaden zur sfl4j-Protokollierung](https://www.slf4j.org/manual.html).

## <a name="netstats"></a>Netzwerkstatistiken des Betriebssystems
Führen Sie den Befehl netstat aus, um einen Überblick darüber zu erhalten, wie viele Verbindungen sich im Zustand `Established`, `CLOSE_WAIT` usw. befinden.

Unter Linux können Sie den folgenden Befehl ausführen:
```bash
netstat -nap
```
Filtern Sie das Ergebnis so, dass nur Verbindungen mit dem Cosmos DB-Endpunkt angezeigt werden.

Offensichtlich sollte die Anzahl der Verbindungen mit dem Cosmos DB-Endpunkt im Zustand `Established` nicht größer sein als die Größe des konfigurierten Verbindungspools.

Wenn es viele Verbindungen mit dem Cosmos DB-Endpunkt im Zustand `CLOSE_WAIT` gibt (z.B. mehr als 1.000 Verbindungen), ist das ein Hinweis darauf, dass Verbindungen aufgebaut und schnell abgebaut werden, was potenziell Probleme verursachen kann. Weitere Details finden Sie im Abschnitt [Häufig auftretende Probleme und Problemumgehungen].

 <!--Anchors-->
[Häufig auftretende Probleme und Problemumgehungen]: #common-issues-workarounds
[Enable client SDK logging]: #enable-client-sice-logging
[Verbindungslimits auf dem Hostcomputer]: #connection-limit-on-host
[Azure SNAT-Portauslastung (PAT)]: #snat


