---
title: Erstellen einer Azure Service Fabric Reliable Actors-Java-Anwendung unter Linux | Microsoft-Dokumentation
description: "Es wird beschrieben, wie Sie in fünf Minuten eine Service Fabric Reliable Actors-Anwendung erstellen und bereitstellen."
services: service-fabric
documentationcenter: java
author: rwike77
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/20/2017
ms.author: ryanwi
ms.openlocfilehash: c7625a5670aca5d105601432fedfd0d7a78bb53c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="create-your-first-java-service-fabric-reliable-actors-application-on-linux"></a>Erstellen Ihrer ersten Service Fabric Reliable Actors-Java-Anwendung unter Linux
> [!div class="op_single_selector"]
> * [C# – Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java – Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# – Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

Diese Schnellstartanleitung hilft Ihnen beim Erstellen Ihrer ersten Azure Service Fabric-Java-Anwendung in einer Linux-Entwicklungsumgebung in nur wenigen Minuten.  Wenn Sie fertig sind, verfügen Sie über eine einfache Java-Einzeldienstanwendung, die im lokalen Entwicklungscluster ausgeführt wird.  

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie beginnen, installieren Sie das Service Fabric SDK und die Service Fabric-Befehlszeilenschnittstelle und richten einen Entwicklungscluster in Ihrer [Linux-Entwicklungsumgebung](service-fabric-get-started-linux.md) ein. Bei Verwendung von Mac OS X können Sie [mithilfe von Vagrant eine Linux-Entwicklungsumgebung auf einem virtuellen Computer einrichten](service-fabric-get-started-mac.md).

Installieren Sie auch die [Service Fabric CLI](service-fabric-cli.md).

### <a name="install-and-set-up-the-generators-for-java"></a>Installieren und Einrichten der Generatoren für Java
Die Gerüstbautools von Service Fabric unterstützen Sie beim Erstellen einer Service Fabric-Java-Anwendung über das Terminal unter Verwendung des Yeoman-Vorlagengenerators. Führen Sie die folgenden Schritte aus, um sicherzustellen, dass der Service Fabric-Yeoman-Vorlagengenerator für Java auf dem Computer verwendet werden kann.
1. Installieren von Node.js und npm auf dem Computer

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. Installieren des [Yeoman](http://yeoman.io/)-Vorlagengenerators auf dem Computer über npm

  ```bash
  sudo npm install -g yo
  ```
3. Installieren des Service Fabric-Yeoman-Java-Anwendungsgenerators über npm

  ```bash
  sudo npm install -g generator-azuresfjava
  ```

## <a name="basic-concepts"></a>Grundlegende Konzepte
Für den Einstieg in Reliable Actors müssen Sie sich nur mit einigen grundlegenden Konzepten vertraut machen:

* **Actor-Dienst**. Reliable Actors sind in Reliable Services gepackt, die in der Service Fabric-Infrastruktur bereitgestellt werden können. Actor-Instanzen werden in einer benannten Dienstinstanz aktiviert.
* **Actor-Registrierung**. Wie bei Reliable Services muss ein Reliable Actor-Dienst bei der Service Fabric-Laufzeit registriert werden. Außerdem muss der Actor-Typ bei der Actor-Laufzeit registriert werden.
* **Actor-Schnittstelle**. Die Actor-Schnittstelle wird zum Definieren einer stark typisierten öffentlichen Schnittstelle eines Actors verwendet. In der Terminologie von Reliable Actor-Modellen definiert die Actor-Schnittstelle die Nachrichtentypen, die der Actor verstehen und verarbeiten kann. Die Actor-Schnittstelle wird von anderen Actors oder von Clientanwendungen zum (asynchronen) „Senden“ von Nachrichten an den Actor verwendet. Reliable Actors können mehrere Schnittstellen implementieren.
* **ActorProxy-Klasse**. Die ActorProxy-Klasse dient Clientanwendungen zum Aufrufen der Methoden, die über die Actor-Schnittstelle verfügbar gemacht werden. Die ActorProxy-Klasse bietet zwei wichtige Funktionen:
  
  * Namensauflösung: Kann zum Lokalisieren des Actors im Cluster verwendet werden (den Knoten des Clusters suchen, auf dem er gehostet wird).
  * Fehlerbehandlung: Kann Methodenaufrufe erneut versuchen und den Actor-Speicherort erneut auflösen, z.B. nach einem Fehler, der erfordert, dass der Actor auf einen anderen Knoten im Cluster verschoben wird.

Die folgenden Regeln zu Actor-Schnittstellen sind erwähnenswert:

* Actor-Schnittstellenmethoden können nicht überladen werden.
* Actor-Schnittstellenmethoden dürfen keine out-, ref- oder optionalen Parameter aufweisen.
* Generische Schnittstellen werden nicht unterstützt.

## <a name="create-the-application"></a>Erstellen der Anwendung
Eine Service Fabric-Anwendung enthält einen oder mehrere Dienste, die jeweils eine bestimmte Rolle bei der Bereitstellung von Funktionen der Anwendung haben. Der im letzten Abschnitt installierte Generator erleichtert die Erstellung des ersten Diensts sowie das spätere Hinzufügen weiterer Dienste.  Sie können auch Service Fabric-Java-Anwendungen mithilfe eines Plug-Ins für Eclipse entwickeln, erstellen und bereitstellen. Weitere Informationen finden Sie unter [Erstellen und Bereitstellen Ihrer ersten Java-Anwendung mithilfe von Eclipse](service-fabric-get-started-eclipse.md). In diesem Schnellstart verwenden Sie Yeoman, um eine Anwendung mit einem einzigen Dienst zu erstellen, der einen Zählerwert speichert und abruft.

1. Geben Sie in einem Terminal ``yo azuresfjava`` ein.
2. Benennen Sie Ihre Anwendung.
3. Wählen Sie die Art Ihres ersten Diensts aus, und benennen Sie ihn. Im Rahmen dieses Tutorials wählen Sie einen Reliable Actor-Dienst aus. Weitere Informationen zu anderen Diensttypen finden Sie unter [Übersicht über die Service Fabric-Programmiermodelle](service-fabric-choose-framework.md).
   ![Service Fabric-Yeoman-Generator für Java][sf-yeoman]

Wenn Sie der Anwendung den Namen „HelloWorldActorApplication“ und dem Actor den Namen „HelloWorldActor“ geben, wird der folgende Gerüstbau erstellt:

```bash
HelloWorldActorApplication/
├── build.gradle
├── HelloWorldActor
│   ├── build.gradle
│   ├── settings.gradle
│   └── src
│       └── reliableactor
│           ├── HelloWorldActorHost.java
│           └── HelloWorldActorImpl.java
├── HelloWorldActorApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldActorPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   ├── _readme.txt
│       │   └── Settings.xml
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── HelloWorldActorInterface
│   ├── build.gradle
│   └── src
│       └── reliableactor
│           └── HelloWorldActor.java
├── HelloWorldActorTestClient
│   ├── build.gradle
│   ├── settings.gradle
│   ├── src
│   │   └── reliableactor
│   │       └── test
│   │           └── HelloWorldActorTestClient.java
│   └── testclient.sh
├── install.sh
├── settings.gradle
└── uninstall.sh
```
## <a name="reliable-actors-basic-building-blocks"></a>Grundlegende Bausteine von Reliable Actors
Die zuvor beschriebenen Konzepte bilden die Grundbausteine eines Reliable Actor-Diensts.

### <a name="actor-interface"></a>Actor-Schnittstelle
Diese enthält die Schnittstellendefinition für den Actor. Diese Schnittstelle definiert den Actor-Vertrag, der von der Actor-Implementierung und den den Actor aufrufenden Clients gemeinsam verwendet wird. In der Regel ist es also sinnvoll, diesen an einem Ort zu definieren, der getrennt ist von der Actor-Implementierung und von mehreren anderen Diensten oder Clientanwendungen gemeinsam verwendet werden kann.

`HelloWorldActorInterface/src/reliableactor/HelloWorldActor.java`:

```java
public interface HelloWorldActor extends Actor {
    @Readonly   
    CompletableFuture<Integer> getCountAsync();

    CompletableFuture<?> setCountAsync(int count);
}
```

### <a name="actor-service"></a>Actordienst
Dieser Dienst enthält Ihre Actor-Implementierung und den Actor-Registrierungscode. Die Actor-Klasse implementiert die Actor-Schnittstelle. Hier führt der Actor seine Aufgaben aus.

`HelloWorldActor/src/reliableactor/HelloWorldActorImpl`:

```java
@ActorServiceAttribute(name = "HelloWorldActor.HelloWorldActorService")
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class HelloWorldActorImpl extends ReliableActor implements HelloWorldActor {
    Logger logger = Logger.getLogger(this.getClass().getName());

    protected CompletableFuture<?> onActivateAsync() {
        logger.log(Level.INFO, "onActivateAsync");

        return this.stateManager().tryAddStateAsync("count", 0);
    }

    @Override
    public CompletableFuture<Integer> getCountAsync() {
        logger.log(Level.INFO, "Getting current count value");
        return this.stateManager().getStateAsync("count");
    }

    @Override
    public CompletableFuture<?> setCountAsync(int count) {
        logger.log(Level.INFO, "Setting current count value {0}", count);
        return this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value);
    }
}
```

### <a name="actor-registration"></a>Actor-Registrierung
Der Actordienst muss mit einem Diensttyp in der Service Fabric-Laufzeit registriert sein. Damit der Actordienst Ihre Actor-Instanzen ausführen kann, muss auch Ihr Actortyp mit dem Actordienst registriert sein. Die `ActorRuntime` -Registrierungsmethode führt dies für Akteure aus.

`HelloWorldActor/src/reliableactor/HelloWorldActorHost`:

```java
public class HelloWorldActorHost {

    public static void main(String[] args) throws Exception {

        try {
            ActorRuntime.registerActorAsync(HelloWorldActorImpl.class, (context, actorType) -> new ActorServiceImpl(context, actorType, ()-> new HelloWorldActorImpl()), Duration.ofSeconds(10));

            Thread.sleep(Long.MAX_VALUE);

        } catch (Exception e) {
            e.printStackTrace();
            throw e;
        }
    }
}
```

## <a name="build-the-application"></a>Erstellen der Anwendung
Die Yeoman-Vorlagen von Service Fabric enthalten ein Buildskript für [Gradle](https://gradle.org/). Damit können Sie die Anwendung über das Terminal erstellen.
Service Fabric-Java-Abhängigkeiten werden aus Maven abgerufen. Damit Sie Service Fabric-Java-Anwendungen erstellen und verwenden können, muss JDK und Gradle installiert sein. Ist dies nicht der Fall, können Sie Folgendes ausführen, um JDK (openjdk-8-jdk) und Gradle zu installieren:

  ```bash
  sudo apt-get install openjdk-8-jdk-headless
  sudo apt-get install gradle
  ```

Führen Sie Folgendes aus, um die Anwendung zu erstellen und zu packen:

  ```bash
  cd myapp
  gradle
  ```

## <a name="deploy-the-application"></a>Bereitstellen der Anwendung
Die erstellte Anwendung kann im lokalen Cluster bereitgestellt werden.

1. Stellen Sie eine Verbindung mit dem lokalen Service Fabric-Cluster her.

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. Führen Sie das in der Vorlage bereitgestellte Installationsskript aus, um das Anwendungspaket in den Imagespeicher des Clusters zu kopieren, den Anwendungstyp zu registrieren und eine Instanz der Anwendung zu erstellen.

    ```bash
    ./install.sh
    ```

Bei der Bereitstellung der erstellten Anwendung wird genau wie bei anderen Service Fabric-Anwendungen vorgegangen. Ausführliche Anweisungen finden Sie in der Dokumentation zum [Verwalten einer Service Fabric-Anwendung mit der Service Fabric-Befehlszeilenschnittstelle](service-fabric-application-lifecycle-sfctl.md).

Parameter für diesen Befehl finden Sie in den erstellten Manifesten im Anwendungspaket.

Öffnen Sie nach der Bereitstellung der Anwendung einen Browser, und navigieren Sie zu [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) unter [http://localhost:19080/Explorer](http://localhost:19080/Explorer).
Erweitern Sie dann den Knoten **Anwendungen**. Hier finden Sie nun einen Eintrag für Ihren Anwendungstyp und einen weiteren für die erste Instanz dieses Typs.

## <a name="start-the-test-client-and-perform-a-failover"></a>Starten des Testclients und Ausführen eines Failovers
Akteure führen selbst keine Aktionen durch. Sie benötigen einen anderen Dienst oder Client, der ihnen Nachrichten sendet. Die Actor-Vorlage enthält ein einfaches Testskript, das Sie für die Interaktion mit dem Actor-Dienst verwenden können.

1. Führen Sie das Skript mithilfe des watch-Hilfsprogramms aus, um die Ausgabe des Actor-Diensts zu erhalten.  Das Testskript ruft die `setCountAsync()`-Methode für den Akteur auf, um einen Zähler zu erhöhen, und die `getCountAsync()`-Methode für den Akteur, um den neuen Zählerwert abzurufen. Anschließend wird der Wert an der Konsole angezeigt.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. Suchen Sie in Service Fabric Explorer den Knoten, der das primäre Replikat für den Akteurdienst hostet. Im folgenden Screenshot ist das „Node_3“: Das primäre Dienstreplikat verarbeitet die Lese- und Schreibvorgänge.  Änderungen am Dienststatus werden dann an die sekundären Replikate, die auf den Knoten 0 und 1 im Screenshot unten ausgeführt werden, repliziert.

    ![Suchen des primären Replikats in Service Fabric Explorer][sfx-primary]

3. Klicken Sie in **Knoten** auf den Knoten, den Sie im vorherigen Schritt ermittelt haben, und wählen Sie im Aktionsmenü die Option **Deaktivieren (neu starten)** aus. Mit dieser Aktion wird der Knoten, auf dem das primäre Dienstreplikat ausgeführt wird, neu gestartet und ein Failover auf eines der sekundären Replikate erzwungen, das auf einem anderen Knoten ausgeführt wird.  Dieses sekundäre Replikat wird zum primären Replikat heraufgestuft. Ein weiteres sekundäres Replikat wird auf einem anderen Knoten erstellt, und das primäre Replikat beginnt damit, Lese- und Schreibvorgänge anzunehmen. Behalten Sie beim Neustart des Knotens die Ausgabe des Testclients im Auge, und beachten Sie, dass sich der Zähler trotz des Failovers weiter erhöht.

## <a name="remove-the-application"></a>Entfernen der Anwendung
Verwenden Sie das in der Vorlage bereitgestellte Deinstallationsskript, um die Anwendungsinstanz zu löschen, die Registrierung des Anwendungspakets aufzuheben und das Anwendungspaket aus dem Imagespeicher des Clusters zu entfernen.

```bash
./uninstall.sh
```

Im Service Fabric Explorer werden die Anwendung und der Anwendungstyp anschließend nicht mehr im Knoten **Anwendungen** angezeigt.

## <a name="service-fabric-java-libraries-on-maven"></a>Service Fabric-Java-Bibliotheken in Maven
Service Fabric-Java-Bibliotheken wurden in Maven gehostet. Sie können die Abhängigkeiten in ``pom.xml`` oder ``build.gradle`` Ihrer Projekte hinzufügen, um Service Fabric-Java-Bibliotheken aus **mavenCentral** zu verwenden. 

### <a name="actors"></a>Akteure

Unterstützung von Service Fabric Reliable Actors für Ihre Anwendung:

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-actors-preview</artifactId>
      <version>0.12.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-actors-preview:0.12.0'
  }
  ```

### <a name="services"></a>Dienste

Unterstützung von Service Fabric Reliable Services für Ihre Anwendung:

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-services-preview</artifactId>
      <version>0.12.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-services-preview:0.12.0'
  }
  ```

### <a name="others"></a>Andere
#### <a name="transport"></a>Transport

Transportschichtunterstützung für die Service Fabric-Java-Anwendung: Diese Abhängigkeit muss Reliable Actors- oder Reliable Services-Anwendungen nur dann explizit hinzugefügt werden, wenn Sie in der Transportschicht programmieren.

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-transport-preview</artifactId>
      <version>0.12.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-transport-preview:0.12.0'
  }
  ```

#### <a name="fabric-support"></a>Fabric-Unterstützung

Unterstützung auf Systemebene für Service Fabric, wobei die Kommunikation mit der nativen Service Fabric-Runtime erfolgt. Diese Abhängigkeit muss Reliable Actors- oder Reliable Services-Anwendungen nicht explizit hinzugefügt werden. Sie wird automatisch von Maven abgerufen, wenn Sie die anderen oben erwähnten Abhängigkeiten hinzufügen.

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-preview</artifactId>
      <version>0.12.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-preview:0.12.0'
  }
  ```

## <a name="migrating-old-service-fabric-java-applications-to-be-used-with-maven"></a>Migrieren alter Service Fabric-Java-Anwendungen für die Verwendung mit Maven
Vor Kurzem wurden Service Fabric-Java-Bibliotheken aus dem Service Fabric Java SDK in das Maven-Repository verschoben. Die neuen mit Yeoman oder Eclipse erstellten Anwendungen generieren aktuelle Projekte (die mit Maven verwendet werden können). Sie können Ihre vorhandenen zustandslosen oder akteurbasierten Service Fabric-Java-Anwendungen, die zuvor das Service Fabric Java SDK verwendet haben, aktualisieren, sodass sie die Service Fabric-Java-Abhängigkeiten von Maven nutzen. Führen Sie die [hier](service-fabric-migrate-old-javaapp-to-use-maven.md) aufgeführten Schritte aus, um sicherzustellen, dass Ihre älteren Anwendung mit Maven verwendet werden können.

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen Ihrer ersten Service Fabric-Java-Anwendung unter Linux mit Eclipse](service-fabric-get-started-eclipse.md)
* [Erfahren Sie mehr über Reliable Actors.](service-fabric-reliable-actors-introduction.md)
* [Interagieren mit Service Fabric-Clustern mithilfe der Service Fabric-Befehlszeilenschnittstelle](service-fabric-cli.md)
* Informieren Sie sich über [Service Fabric-Supportoptionen](service-fabric-support.md).
* [Erste Schritte mit der Service Fabric-Befehlszeilenschnittstelle](service-fabric-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png
