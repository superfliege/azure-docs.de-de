---
title: Dienstremoting mit C# in Service Fabric | Microsoft-Dokumentation
description: Service Fabric-Remoting ermöglicht Clients und Diensten die Kommunikation mit C#-Diensten über einen Remoteprozeduraufruf.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: BharatNarasimman
ms.assetid: abfaf430-fea0-4974-afba-cfc9f9f2354b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 09/20/2017
ms.author: vturecek
ms.openlocfilehash: e7652fe1b211e6811a4a3aa61bc2aa9d2f529dda
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2018
ms.locfileid: "39205815"
---
# <a name="service-remoting-in-c-with-reliable-services"></a>Dienstremoting in C# mit Reliable Services
> [!div class="op_single_selector"]
> * [C# unter Windows](service-fabric-reliable-services-communication-remoting.md)
> * [Java unter Linux](service-fabric-reliable-services-communication-remoting-java.md)
>
>

Für WebAPI, WCF (Windows Communication Foundation) und andere Dienste, die nicht an ein bestimmtes Kommunikationsprotokoll oder einen bestimmten Kommunikationsstapel gebunden sind, stellt das Reliable Services-Framework einen Remotingmechanismus für das schnelle, einfache Einrichten von Remoteprozeduraufrufen für Dienste bereit. Dieser Artikel beschreibt das Einrichten von Remoteprozeduraufrufen für Dienste, die in C# geschrieben wurden.

## <a name="set-up-remoting-on-a-service"></a>Einrichten von Remoting für einen Dienst
Die Einrichtung von Remoting für einen Dienst erfolgt in zwei einfachen Schritten:

1. Erstellen Sie eine Schnittstelle, die vom Dienst implementiert werden soll. Diese Schnittstelle definiert die Methoden, die für den Remoteprozeduraufruf für Ihren Dienst verfügbar sind. Bei den Methoden muss es sich um asynchrone Methoden handeln, die einen Task zurückgeben. Die Schnittstelle muss `Microsoft.ServiceFabric.Services.Remoting.IService` implementieren, um zu signalisieren, dass der Dienst über eine Remotingschnittstelle verfügt.
2. Verwenden Sie einen Remoting-Listener in Ihrem Dienst. Ein Remoting-Listener ist eine `ICommunicationListener` -Implementierung, die Remotingfunktionen bereitstellt. Der `Microsoft.ServiceFabric.Services.Remoting.Runtime`-Namespace enthält die Erweiterungsmethode `CreateServiceRemotingListener` sowohl für zustandslose als auch für zustandsbehaftete Dienste, die zum Erstellen eines Remotinglisteners mit dem standardmäßigen Remotingtransportprotokoll verwendet werden kann.

>[!NOTE]
>Der `Remoting`-Namespace steht als separates NuGet-Paket mit dem Namen `Microsoft.ServiceFabric.Services.Remoting` zur Verfügung.

Der folgende zustandslose Dienst macht beispielsweise eine einzelne Methode verfügbar, um „Hello World“ per Remoteprozeduraufruf abzurufen:

```csharp
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Remoting;
using Microsoft.ServiceFabric.Services.Remoting.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

public interface IMyService : IService
{
    Task<string> HelloWorldAsync();
}

class MyService : StatelessService, IMyService
{
    public MyService(StatelessServiceContext context)
        : base (context)
    {
    }

    public Task<string> HelloWorldAsync()
    {
        return Task.FromResult("Hello!");
    }

    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
     return this.CreateServiceRemotingInstanceListeners();
    }
}
```
> [!NOTE]
> Bei den Argumenten und Rückgabetypen in der Dienstschnittstelle kann es sich um einfache, komplexe oder benutzerdefinierte Typen handeln. Sie müssen jedoch durch [DataContractSerializer](https://msdn.microsoft.com/library/ms731923.aspx) von .NET serialisierbar sein.
>
>

## <a name="call-remote-service-methods"></a>Aufrufen von Remotedienstmethoden
Methoden für einen Dienst, der den Remotingstapel nutzt, werden mithilfe eines lokalen Proxys für den Dienst über die `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` -Klasse abgerufen. Die `ServiceProxy` -Methode erstellt einen lokalen Proxy unter Verwendung der gleichen Schnittstelle, die auch der Dienst implementiert. Mit diesem Proxy können Sie Methoden für die Schnittstelle remote aufrufen.

```csharp

IMyService helloWorldClient = ServiceProxy.Create<IMyService>(new Uri("fabric:/MyApplication/MyHelloWorldService"));

string message = await helloWorldClient.HelloWorldAsync();

```

Das Remotingframework gibt vom Dienst ausgelöste Ausnahmen an den Client weiter. Folglich ist der Client bei der Verwendung von `ServiceProxy` für die Behandlung der vom Dienst ausgelösten Ausnahmen verantwortlich.

## <a name="service-proxy-lifetime"></a>Gültigkeitsdauer von Dienstproxys
Die Erstellung von Dienstproxys ist ein einfacher Vorgang, der sehr wenige Ressourcen verbraucht, sodass Sie so viele erstellen können, wie Sie benötigen. Dienstproxyinstanzen können erneut verwendet werden, solange sie benötigt werden. Wenn ein Remoteprozeduraufruf eine Ausnahme auslöst, können Sie weiterhin dieselbe Proxyinstanz wiederverwenden. Jeder Dienstproxy enthält einen Kommunikationsclient zum Senden von Nachrichten im Netzwerk. Beim Aufruf von Remoteaufrufen wird intern geprüft, ob der Kommunikationsclient gültig ist. Basierend auf den Ergebnissen dieser Überprüfungen wird der Kommunikationsclient bei Bedarf neu erstellt. Darum müssen Sie im Falle einer Ausnahme `ServiceProxy` nicht neu erstellen.

### <a name="serviceproxyfactory-lifetime"></a>Gültigkeitsdauer von „ServiceProxyFactory“
[ServiceProxyFactory](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.client.serviceproxyfactory) ist eine Factory, die Proxyinstanzen für verschiedene Remotingschnittstellen erstellt. Wenn Sie die API `ServiceProxy.Create` zum Erstellen von Proxys verwenden, dann erstellt das Framework einen Singleton-Dienstproxy.
Es ist sinnvoll, eine manuell zu erstellen, wenn Sie [IServiceRemotingClientFactory](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v1.client.iserviceremotingclientfactory)-Eigenschaften überschreiben müssen.
Die Factoryerstellung ist ein kostenintensiver Vorgang. „ServiceProxyFactory“ verwaltet einen internen Cache des Kommunikationsclients.
Es empfiehlt sich, „ServiceProxyFactory“ so lange wie möglich im Cache zwischenzuspeichern.

## <a name="remoting-exception-handling"></a>Behandlung von Remotingausnahmen
Alle von der Dienst-API ausgelösten Remoteausnahmen werden als „AggregateException“ an den Client zurückgesendet. „RemoteExceptions“ muss „DataContract Serializable“ sein. Andernfalls löst die Proxy-API [ServiceException](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.communication.serviceexception) mit enthaltenem Serialisierungsfehler aus.

„ServiceProxy“ verarbeitet sämtliche Failoverausnahmen für die Dienstpartition, für die seine Erstellung erfolgt ist. Dieser Proxy löst die Endpunkte erneut auf, falls Failoverausnahmen (nicht vorübergehende Ausnahmen) vorliegen, und wiederholt den Aufruf mit dem richtigen Endpunkt. Die Anzahl der Wiederholungen bei Failoverausnahmen ist unbegrenzt.
Wenn vorübergehende Ausnahmen auftreten, versucht der Proxy erneut, den Aufruf auszuführen.

Standardparameter für die Wiederholung werden von [OperationRetrySettings](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.communication.client.operationretrysettings) angegeben.

Der Benutzer kann diese Werte konfigurieren, indem das „OperationRetrySettings“-Objekt an den „ServiceProxyFactory“-Konstruktor übergeben wird.

## <a name="how-to-use-the-remoting-v2-stack"></a>Verwenden des Remoting V2-Stapels

Ab dem NuGet Remoting-Paket Version 2.8 haben Sie die Möglichkeit, den Remoting V2-Stapel zu verwenden. Der Remoting V2-Stapel ist leistungsfähiger und stellt Funktionen wie eine benutzerdefinierte Serialisierung und besser einsetzbare APIs zur Verfügung.
Der Vorlagencode verwendet weiterhin den Remoting V1-Stapel.
Remoting V2 ist nicht mit V1 (vorheriger Remotingstapel) kompatibel. Befolgen Sie daher die Anweisungen weiter unten in diesem Artikel, um ein [Upgrade von V1 auf V2](#how-to-upgrade-from-remoting-v1-to-remoting-v2) durchzuführen, ohne die Verfügbarkeit des Diensts zu beeinträchtigen.

Zum Aktivieren des V2-Stapels stehen folgende Methoden zur Verfügung.

### <a name="using-an-assembly-attribute-to-use-the-v2-stack"></a>Verwenden eines assembly-Attributs, um den V2-Stapel zu verwenden

Diese Schritte ändern den Vorlagencode so, dass der V2-Stapel mit einem assembly-Attribut verwendet wird.

1. Ändern Sie die Endpunktressource im Dienstmanifest von `"ServiceEndpoint"` in `"ServiceEndpointV2"`.

  ```xml
  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpointV2" />
    </Endpoints>
  </Resources>
  ```

2. Verwenden Sie die Erweiterungsmethode `Microsoft.ServiceFabric.Services.Remoting.Runtime.CreateServiceRemotingInstanceListeners`, um Remotinglistener zu erstellen (diese sind für V1 und V2 gleich).

  ```csharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return this.CreateServiceRemotingInstanceListeners();
    }
  ```

3. Markieren Sie die Assembly, die die Remotingschnittstellen enthält, mit einem `FabricTransportServiceRemotingProvider`-Attribut.

  ```csharp
  [assembly: FabricTransportServiceRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2, RemotingClientVersion = RemotingClientVersion.V2)]
  ```

Im Clientprojekt sind keine Codeänderungen erforderlich.
Erstellen Sie die Clientassembly mit der Schnittstellenassembly, um sicherzustellen, dass das oben gezeigte assembly-Attribut verwendet wird.

### <a name="using-explicit-v2-classes-to-use-the-v2-stack"></a>Verwenden von expliziten V2-Klassen für die Verwendung des V2-Stapels

Alternativ zur Verwendung eines assembly-Attributs kann der V2-Stapel auch mithilfe von expliziten V2-Klassen aktiviert werden.

Diese Schritte ändern den Vorlagencode so, dass der V2-Stapel mit expliziten V2-Klassen verwendet wird.

1. Ändern Sie die Endpunktressource im Dienstmanifest von `"ServiceEndpoint"` in `"ServiceEndpointV2"`.

  ```xml
  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpointV2" />
    </Endpoints>
  </Resources>
  ```

2. Verwenden Sie den [FabricTransportServiceRemotingListener](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v2.fabrictransport.runtime.fabrictransportserviceremotingListener?view=azure-dotnet) aus dem `Microsoft.ServiceFabric.Services.Remoting.V2.FabricTransport.Runtime`-Namespace.

  ```csharp
  protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[]
        {
            new ServiceInstanceListener((c) =>
            {
                return new FabricTransportServiceRemotingListener(c, this);

            })
        };
    }
  ```

3. Verwenden Sie die [FabricTransportServiceRemotingClientFactory](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v2.fabrictransport.client.fabrictransportserviceremotingclientfactory?view=azure-dotnet) aus dem `Microsoft.ServiceFabric.Services.Remoting.V2.FabricTransport.Client`-Namespace, um Clients zu erstellen.

  ```csharp
  var proxyFactory = new ServiceProxyFactory((c) =>
          {
              return new FabricTransportServiceRemotingClientFactory();
          });
  ```

## <a name="how-to-upgrade-from-remoting-v1-to-remoting-v2"></a>So aktualisieren Sie von Remoting V1 auf Remoting V2
Zum Aktualisieren von V1 auf V2 sind 2 Aktualisierungsschritte erforderlich. Die folgenden Schritte müssen in der aufgeführten Reihenfolge ausgeführt werden.

1. Führen Sie ein Upgrade für den V1-Dienst auf den V2-Dienst durch, indem Sie das nachfolgende Attribut verwenden.
Mit dieser Änderung wird sichergestellt, dass der Dienst den V1- und den V2-Listener abhört.

    a) Fügen Sie im Dienstmanifest eine Endpunktressource namens „ServiceEndpointV2“ hinzu.
      ```xml
      <Resources>
        <Endpoints>
          <Endpoint Name="ServiceEndpointV2" />  
        </Endpoints>
      </Resources>
      ```

    b) Verwenden Sie die folgende Remoting-Erweiterungsmethode, um einen Remoting-Listener zu erstellen.

    ```csharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return this.CreateServiceRemotingInstanceListeners();
    }
    ```

    c) Fügen Sie das Assembly-Attribut für Remotingschnittstellen hinzu, damit der V1- und V2-Listener und der V2-Client verwendet werden.
    ```csharp
    [assembly: FabricTransportServiceRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2|RemotingListenerVersion.V1, RemotingClientVersion = RemotingClientVersion.V2)]

      ```
2. Aktualisieren Sie den V1-Client auf den V2-Client, indem Sie das Attribut für den V2-Client verwenden.
Mit diesem Schritt wird sichergestellt, dass der Client den V2-Stapel verwendet.
Im Clientprojekt/-dienst ist keine Änderung erforderlich. Es genügt, Clientprojekte mit der aktualisierten Schnittstellenassembly zu erstellen.

3. Dieser Schritt ist optional. Verwenden Sie das V2Listener-Attribut, und aktualisieren Sie dann den V2-Dienst.
Mit diesem Schritt wird sichergestellt, dass der Dienst nur den V2-Listener abhört.

```csharp
[assembly: FabricTransportServiceRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2, RemotingClientVersion = RemotingClientVersion.V2)]
```


## <a name="how-to-use-remoting-v2interfacecompatible-stack"></a>Gewusst wie: Verwenden des Remoting V2(InterfaceCompatible)-Stapels
 Der Remoting V2(InterfaceCompatible)-Stapel (auch als „V2_1“ bezeichnet) verfügt über alle Features des Remoting V2-Stapels und ist darüber hinaus ein Stapel, dessen Schnittstelle mit dem Remoting V1-Stapel kompatibel ist, der allerdings nicht mit V2 und V1 abwärtskompatibel ist. Um ohne Beeinträchtigung der Dienstverfügbarkeit ein Upgrade von V1 auf V2_1 durchzuführen, führen Sie die Schritte im nachfolgenden Artikel [Gewusst wie: Durchführen eines Upgrades von V1 auf V2(InterfaceCompatible)](#how-to-upgrade-from-remoting-v1-to-remoting-v2interfacecompatible) durch.


### <a name="using-assembly-attribute-to-use-remoting-v2interfacecompatible-stack"></a>Verwenden des Remoting V2(InterfaceCompatible)-Stapels mit dem Assembly-Attribut

Im Folgenden werden die Schritte beschrieben, die für einen Wechsel zum V2_1-Stapel auszuführen sind.

1. Fügen Sie im Dienstmanifest eine Endpunktressource namens „ServiceEndpointV2_1“ hinzu.

  ```xml
  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpointV2_1" />  
    </Endpoints>
  </Resources>
  ```

2.  Verwenden Sie die Remoting-Erweiterungsmethode, um einen Remoting-Listener zu erstellen.

  ```csharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return this.CreateServiceRemotingInstanceListeners();
    }
  ```

3.  Fügen Sie das [Assembly-Attribut](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.fabrictransport.fabrictransportserviceremotingproviderattribute?view=azure-dotnet) für Remotingschnittstellen hinzu.

  ```csharp
    [assembly:  FabricTransportServiceRemotingProvider(RemotingListenerVersion=  RemotingListenerVersion.V2_1, RemotingClientVersion= RemotingClientVersion.V2_1)]

  ```
Im Clientprojekt sind keine Änderungen erforderlich.
Erstellen Sie die Client-Assembly mit der Schnittstellenassembly, um sicherzustellen, dass das obige Assembly Attribut verwendet wird.

### <a name="using-explicit-remoting-classes-to-create-listener-clientfactory-for-v2interfacecompatible-version"></a>Erstellen von Listener bzw. ClientFactory für die V2(InterfaceCompatible)-Version mithilfe von expliziten Remotingklassen
Es folgen die Schritte, die auszuführen sind.
1.  Fügen Sie im Dienstmanifest eine Endpunktressource namens „ServiceEndpointV2_1“ hinzu.

  ```xml
  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpointV2_1" />  
    </Endpoints>
  </Resources>
  ```

2. Verwenden Sie [RemotingV2Listener](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v2.fabrictransport.runtime.fabrictransportserviceremotinglistener?view=azure-dotnet). Der Standardname für die Dienstendpunktressource lautet „ServiceEndpointV2_1“, und dieser muss im Dienstmanifest definiert sein.

  ```csharp
  protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[]
        {
            new ServiceInstanceListener((c) =>
            {
                var settings = new FabricTransportRemotingListenerSettings();
                settings.UseWrappedMessage = true;
                return new FabricTransportServiceRemotingListener(c, this,settings);

            })
        };
    }
  ```

3. Verwenden Sie V2-[ClientFactory](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v2.fabrictransport.client.fabrictransportserviceremotingclientfactory?view=azure-dotnet).
  ```csharp
  var proxyFactory = new ServiceProxyFactory((c) =>
          {
            var settings = new FabricTransportRemotingSettings();
            settings.UseWrappedMessage = true;
            return new FabricTransportServiceRemotingClientFactory(settings);
          });
  ```

## <a name="how-to-upgrade-from-remoting-v1-to-remoting-v2interfacecompatible"></a>Gewusst wie: Durchführen eines Upgrades von Remoting V1 auf Remoting V2(InterfaceCompatible)
Zum Durchführen eines Upgrades von V1 auf V2(InterfaceCompatible) (auch als „V2_1“ bezeichnet) sind zwei Upgradeschritte erforderlich. Die folgenden Schritte müssen in der aufgeführten Reihenfolge ausgeführt werden.

1. Führen Sie ein Upgrade für den V1-Dienst auf den V2_1-Dienst durch, indem Sie das folgende Attribut verwenden.
Mit dieser Änderung wird sichergestellt, dass der Dienst auf den V1- und den V2_1-Listener lauscht.

    a) Fügen Sie im Dienstmanifest eine Endpunktressource namens „ServiceEndpointV2_1“ hinzu.
      ```xml
      <Resources>
        <Endpoints>
          <Endpoint Name="ServiceEndpointV2_1" />  
        </Endpoints>
      </Resources>
      ```

    b) Verwenden Sie die folgende Remoting-Erweiterungsmethode, um einen Remoting-Listener zu erstellen.

    ```csharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return this.CreateServiceRemotingInstanceListeners();
    }
    ```

    c) Fügen Sie das Assembly-Attribut für Remotingschnittstellen hinzu, damit der V1- und der V2_1-Listener und der V2_1-Client verwendet werden.
    ```csharp
   [assembly: FabricTransportServiceRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2_1 | RemotingListenerVersion.V1, RemotingClientVersion = RemotingClientVersion.V2_1)]

      ```
2. Führen Sie ein Upgrade für den V1-Client auf den V2_1-Client durch, indem Sie das Attribut für den V2_1-Client verwenden.
Mit diesem Schritt wird sichergestellt, dass der Client den V2_1-Stapel verwendet.
Im Clientprojekt/-dienst ist keine Änderung erforderlich. Es genügt, Clientprojekte mit der aktualisierten Schnittstellenassembly zu erstellen.

3. Dieser Schritt ist optional. Entfernen Sie den Listener mit der Version V1 vom Attribut, und führen Sie dann ein Upgrade für den V2-Dienst durch.
Mit diesem Schritt wird sichergestellt, dass der Dienst nur den V2-Listener abhört.

```csharp
[assembly: FabricTransportServiceRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2_1, RemotingClientVersion = RemotingClientVersion.V2_1)]
```

### <a name="how-to-use-custom-serialization-with-remoting-wrapped-message"></a>Gewusst wie: Verwenden der benutzerdefinierten Serialisierung mit umschlossener Remotingnachricht
In umschlossenen Remotingnachrichten wird ein einzelnes umschlossenes Objekt mit allen Parametern als darin enthaltene Felder erstellt.
Gehen Sie wie folgt vor:

1. Implementieren Sie die IServiceRemotingMessageSerializationProvider-Schnittstelle, um die Implementierung für die benutzerdefinierte Serialisierung bereitzustellen.
    Der Codeausschnitt für die Implementierung sieht wie folgt aus.

      ```csharp
      public class ServiceRemotingJsonSerializationProvider : IServiceRemotingMessageSerializationProvider
      {
        public IServiceRemotingMessageBodyFactory CreateMessageBodyFactory()
        {
          return new JsonMessageFactory();
        }

        public IServiceRemotingRequestMessageBodySerializer CreateRequestMessageSerializer(Type serviceInterfaceType, IEnumerable<Type> requestWrappedType, IEnumerable<Type> requestBodyTypes = null)
        {
          return new ServiceRemotingRequestJsonMessageBodySerializer();
        }

        public IServiceRemotingResponseMessageBodySerializer CreateResponseMessageSerializer(Type serviceInterfaceType, IEnumerable<Type> responseWrappedType, IEnumerable<Type> responseBodyTypes = null)
        {
          return new ServiceRemotingResponseJsonMessageBodySerializer();
        }
      }
      ```
      ```csharp
        class JsonMessageFactory : IServiceRemotingMessageBodyFactory
            {

              public IServiceRemotingRequestMessageBody CreateRequest(string interfaceName, string methodName, int numberOfParameters, object wrappedRequestObject)
              {
                return new JsonBody(wrappedRequestObject);
              }

              public IServiceRemotingResponseMessageBody CreateResponse(string interfaceName, string methodName, object wrappedRequestObject)
              {
                return new JsonBody(wrappedRequestObject);
              }
            }
      ```
      ```csharp
      class ServiceRemotingRequestJsonMessageBodySerializer : IServiceRemotingRequestMessageBodySerializer
        {
            private JsonSerializer serializer;

            public ServiceRemotingRequestJsonMessageBodySerializer()
            {
              serializer = JsonSerializer.Create(new JsonSerializerSettings()
              {
                TypeNameHandling = TypeNameHandling.All
                });
              }

              public IOutgoingMessageBody Serialize(IServiceRemotingRequestMessageBody serviceRemotingRequestMessageBody)
             {
               if (serviceRemotingRequestMessageBody == null)
               {
                 return null;
               }          
               using (var writeStream = new MemoryStream())
               {
                 using (var jsonWriter = new JsonTextWriter(new StreamWriter(writeStream)))
                 {
                   serializer.Serialize(jsonWriter, serviceRemotingRequestMessageBody);
                   jsonWriter.Flush();
                   var bytes = writeStream.ToArray();
                   var segment = new ArraySegment<byte>(bytes);
                   var segments = new List<ArraySegment<byte>> { segment };
                   return new OutgoingMessageBody(segments);
                 }
               }
              }

              public IServiceRemotingRequestMessageBody Deserialize(IIncomingMessageBody messageBody)
             {
               using (var sr = new StreamReader(messageBody.GetReceivedBuffer()))
               {
                 using (JsonReader reader = new JsonTextReader(sr))
                 {
                   var ob = serializer.Deserialize<JsonBody>(reader);
                   return ob;
                 }
               }
             }
            }
      ```
      ```csharp
      class ServiceRemotingResponseJsonMessageBodySerializer : IServiceRemotingResponseMessageBodySerializer
       {
         private JsonSerializer serializer;

        public ServiceRemotingResponseJsonMessageBodySerializer()
        {
          serializer = JsonSerializer.Create(new JsonSerializerSettings()
          {
              TypeNameHandling = TypeNameHandling.All
            });
          }

          public IOutgoingMessageBody Serialize(IServiceRemotingResponseMessageBody responseMessageBody)
          {
            if (responseMessageBody == null)
            {
              return null;
            }

            using (var writeStream = new MemoryStream())
            {
              using (var jsonWriter = new JsonTextWriter(new StreamWriter(writeStream)))
              {
                serializer.Serialize(jsonWriter, responseMessageBody);
                jsonWriter.Flush();
                var bytes = writeStream.ToArray();
                var segment = new ArraySegment<byte>(bytes);
                var segments = new List<ArraySegment<byte>> { segment };
                return new OutgoingMessageBody(segments);
              }
            }
          }

          public IServiceRemotingResponseMessageBody Deserialize(IIncomingMessageBody messageBody)
          {

             using (var sr = new StreamReader(messageBody.GetReceivedBuffer()))
             {
               using (var reader = new JsonTextReader(sr))
               {
                 var obj = serializer.Deserialize<JsonBody>(reader);
                 return obj;
               }
             }
           }
       }
    ```
    ```csharp
    class JsonBody : WrappedMessage, IServiceRemotingRequestMessageBody, IServiceRemotingResponseMessageBody
    {
          public JsonBody(object wrapped)
          {
            this.Value = wrapped;
          }

          public void SetParameter(int position, string parameName, object parameter)
          {  //Not Needed if you are using WrappedMessage
            throw new NotImplementedException();
          }

          public object GetParameter(int position, string parameName, Type paramType)
          {
            //Not Needed if you are using WrappedMessage
            throw new NotImplementedException();
          }

          public void Set(object response)
          { //Not Needed if you are using WrappedMessage
            throw new NotImplementedException();
          }

          public object Get(Type paramType)
          {  //Not Needed if you are using WrappedMessage
            throw new NotImplementedException();
          }
    }
    ```

2.    Überschreiben Sie den Standardserialisierungsanbieter mit „JsonSerializationProvider“ für den Remoting-Listener.

  ```csharp
  protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
   {
       return new[]
       {
           new ServiceInstanceListener((c) =>
           {
               return new FabricTransportServiceRemotingListener(c, this,
                   new ServiceRemotingJsonSerializationProvider());
           })
       };
   }
  ```

3.    Überschreiben Sie den Standardserialisierungsanbieter mit „JsonSerializationProvider“ für „RemotingClientFactory“.

```csharp
var proxyFactory = new ServiceProxyFactory((c) =>
{
    return new FabricTransportServiceRemotingClientFactory(
    serializationProvider: new ServiceRemotingJsonSerializationProvider());
  });
  ```
## <a name="next-steps"></a>Nächste Schritte
* [Web-API mit OWIN in Reliable Services](service-fabric-reliable-services-communication-webapi.md)
* [WCF-Kommunikation mit Reliable Services](service-fabric-reliable-services-communication-wcf.md)
* [Absichern der Kommunikation für Reliable Services](service-fabric-reliable-services-secure-communication.md)
