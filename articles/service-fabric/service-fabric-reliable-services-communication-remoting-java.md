---
title: In Azure Service Fabric-Dienst-Remoting | Microsoft Docs
description: Service Fabric-Remoting kann Clients und Dienste zur Kommunikation mit Services mithilfe des Remoteprozeduraufrufs.
services: service-fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: 
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 06/30/2017
ms.author: pakunapa
ms.openlocfilehash: dc4a362b5737bb424ca2c196c85f4c51b6ee5e30
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="service-remoting-with-reliable-services"></a>Dienst-Remoting mit zuverlässige Dienste
> [!div class="op_single_selector"]
> * [C#-unter Windows](service-fabric-reliable-services-communication-remoting.md)
> * [Java unter Linux](service-fabric-reliable-services-communication-remoting-java.md)
>
>

Das zuverlässige Dienste-Framework bietet einen remotingmechanismus Remoteprozeduraufruf für Dienste schnell und einfach einrichten.

## <a name="set-up-remoting-on-a-service"></a>Remoting auf einen Dienst einrichten
Einrichten von Remoting für einen Dienst erfolgt in zwei Schritten:

1. Erstellen Sie eine Schnittstelle für den Dienst zu implementieren. Diese Schnittstelle definiert die Methoden, die für einen Remoteprozeduraufruf für Ihren Dienst verfügbar sind. Die Methoden muss die Aufgabe zurückgibt asynchrone Methoden. Die Schnittstelle implementieren muss `microsoft.serviceFabric.services.remoting.Service` signalisiert, dass der Dienst eine Remotingschnittstelle.
2. Verwenden Sie einen Listener für Remoting in Ihrem Dienst an. Dies ist ein `CommunicationListener` Implementierung, die Remotefunktionen bereitstellt. `FabricTransportServiceRemotingListener`kann verwendet werden, um einen Remoting-Listener, die unter Verwendung des Standardprotokolls für Remoting Transport zu erstellen.

Die folgenden zustandslosen Diensts macht z. B. eine einzelne Methode, um "Hello World" über einen Remoteprozeduraufruf zu erhalten.

```java
import java.util.ArrayList;
import java.util.concurrent.CompletableFuture;
import java.util.List;
import microsoft.servicefabric.services.communication.runtime.ServiceInstanceListener;
import microsoft.servicefabric.services.remoting.Service;
import microsoft.servicefabric.services.runtime.StatelessService;

public interface MyService extends Service {
    CompletableFuture<String> helloWorldAsync();
}

class MyServiceImpl extends StatelessService implements MyService {
    public MyServiceImpl(StatelessServiceContext context) {
       super(context);
    }

    public CompletableFuture<String> helloWorldAsync() {
        return CompletableFuture.completedFuture("Hello!");
    }

    @Override
    protected List<ServiceInstanceListener> createServiceInstanceListeners() {
        ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
        listeners.add(new ServiceInstanceListener((context) -> {
            return new FabricTransportServiceRemotingListener(context,this);
        }));
        return listeners;
    }
}
```

> [!NOTE]
> Die Argumente und die Rückgabetypen in der Dienstschnittstelle einfache, komplexen oder benutzerdefinierten Typen möglich, aber sie müssen serialisierbar sein.
>
>

## <a name="call-remote-service-methods"></a>Remotedienst Methoden aufrufen
Aufrufen von Methoden für einen Dienst mithilfe des Stapels Remoting erfolgt über einen lokalen Proxy für den Dienst über die `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` Klasse. Die `ServiceProxyBase` Methode erstellt einen lokalen Proxy mithilfe der gleichen Benutzeroberfläche, die der Dienst implementiert. Mit diesen Proxy verwenden können Sie einfach Methoden für die Schnittstelle Remote aufrufen.

```java

MyService helloWorldClient = ServiceProxyBase.create(MyService.class, new URI("fabric:/MyApplication/MyHelloWorldService"));

CompletableFuture<String> message = helloWorldClient.helloWorldAsync();

```

Das Remotingframework gibt den Dienst an dem Client ausgelöste Ausnahmen weiter. Daher Ausnahmebehandlung Logik auf dem Client mithilfe von `ServiceProxyBase` können direkt behandeln von Ausnahmen, die der Dienst auslöst.

## <a name="service-proxy-lifetime"></a>Proxy-Lebensdauer von Diensten
ServiceProxy Erstellung ist ein einfacher Vorgang, sodass Benutzer erstellen, kann so viele, wie sie es benötigen. Dienstproxy können erneut verwendet werden, solange der Benutzer ihn benötigen. Benutzer kann den gleichen Proxy im Fall einer Ausnahme erneut verwenden. Jede ServiceProxy enthält Communication-Clients zum Senden von Nachrichten über die Verbindung verwendet. Beim Aufrufen der API, haben wir die interne überprüfen, um festzustellen, ob die Kommunikation von Clients verwendet, gültig ist. Basierend auf, die entstehen, erstellen neu wir den Kommunikationsclient. Daher Benutzer müssen nicht im Fall einer Ausnahme Serviceproxy neu zu erstellen.

### <a name="serviceproxyfactory-lifetime"></a>ServiceProxyFactory Lebensdauer
[FabricServiceProxyFactory](https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.client._fabric_service_proxy_factory) ist eine Factory, erstellt der Proxy für verschiedene Remoting-Schnittstellen. Wenn Sie die API verwenden `ServiceProxyBase.create` zum Erstellen von Proxys, klicken Sie dann Framework erstellt eine `FabricServiceProxyFactory`.
Es ist sinnvoll, manuell erstellen, wenn Sie außer Kraft setzen müssen [ServiceRemotingClientFactory](https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.client._service_remoting_client_factory) Eigenschaften.
Kanalfactory ist ein teurer Vorgang. `FabricServiceProxyFactory`verwaltet Cache der Kommunikation zwischen Clients freigegeben.
Bewährte Methode besteht darin, den Cache `FabricServiceProxyFactory` so lange als möglich.

## <a name="remoting-exception-handling"></a>Remoting-Ausnahmebehandlung
Alle remote von der dienstverwaltungs-API ausgelöste Ausnahme gesendet werden zurück an den Client als RuntimeException oder FabricException.

ServiceProxy werden alle Failover-Ausnahme für die Dienstpartition, die für die Erstellung durchgeführt. Erneut wird die Endpunkte aufgelöst, wenn Failover Exceptions(Non-Transient Exceptions) vorhanden ist und den Aufruf mit dem richtigen Endpunkt wiederholt. Anzahl von Wiederholungen für die Failover-Ausnahme ist unbegrenzt.
Im Falle TransientExceptions wiederholt er nur den Aufruf.

Wiederholen Sie den Standardparameter sind Ressourcenanbieter durch [OperationRetrySettings]. (https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.communication.client._operation_retry_settings) Benutzer kann diese Werte übergibt OperationRetrySettings Objekt ServiceProxyFactory Konstruktor konfigurieren.

## <a name="next-steps"></a>Nächste Schritte
* [Sichern der Kommunikation für zuverlässige Dienste](service-fabric-reliable-services-secure-communication.md)
