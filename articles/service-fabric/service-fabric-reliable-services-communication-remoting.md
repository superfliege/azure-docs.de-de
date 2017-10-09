
---
title: Dienstremoting in Service Fabric | Microsoft Docs
description: "Service Fabric-Remoting ermöglicht Clients und Diensten die Kommunikation mit Diensten über einen Remoteprozeduraufruf."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: BharatNarasimman
ms.assetid: abfaf430-fea0-4974-afba-cfc9f9f2354b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 09/20/2017
ms.author: vturecek
ms.translationtype: HT
ms.sourcegitcommit: 469246d6cb64d6aaf995ef3b7c4070f8d24372b1
ms.openlocfilehash: 655bc3dd3735a35fbe7437e8dda92b2adf15f7bf
ms.contentlocale: de-de
ms.lasthandoff: 09/27/2017

---
# <a name="service-remoting-with-reliable-services"></a>Dienstremoting mit Reliable Services
Für WebAPI, WCF (Windows Communication Foundation) und andere Dienste, die nicht an ein bestimmtes Kommunikationsprotokoll oder einen bestimmten Kommunikationsstapel gebunden sind, stellt das Reliable Services-Framework einen Remotingmechanismus für das schnelle, einfache Einrichten von Remoteprozeduraufrufen für Dienste bereit.

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

    public Task HelloWorldAsync()
    {
        return Task.FromResult("Hello!");
    }

    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[] { new ServiceInstanceListener(context =>            this.CreateServiceRemotingListener(context)) };
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

Das Remotingframework gibt beim Dienst aufgetretene Ausnahmen an den Client weiter. Somit kann die Ausnahmebehandlungslogik auf dem Client Ausnahmen, die vom Dienst ausgegeben werden, mithilfe von `ServiceProxy` direkt behandeln.

## <a name="service-proxy-lifetime"></a>Gültigkeitsdauer von Dienstproxys
Die Erstellung von Dienstproxys ist ein einfacher Vorgang, sodass Benutzer so viele erstellen können, wie sie benötigen. Ein Dienstproxy kann erneut verwendet werden, solange Benutzer ihn benötigen. Löst eine Remote-API eine Ausnahme aus, können Benutzer weiterhin denselben Proxy wiederverwenden. Jeder Dienstproxy enthält einen Kommunikationsclient zum Senden von Nachrichten im Netzwerk. Beim Aufrufen der API erfolgt eine interne Prüfung, ob der verwendete Kommunikationsclient gültig ist. Basierend auf diesem Ergebnis erstellen wir den Kommunikationsclient neu. Daher müssen Benutzer, wenn eine Ausnahme auftritt, den Dienstproxy nicht neu erstellen.

### <a name="serviceproxyfactory-lifetime"></a>Gültigkeitsdauer von „ServiceProxyFactory“
[ServiceProxyFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.client.serviceproxyfactory) ist eine Factory, die einen Proxy für verschiedene Remotingschnittstellen erstellt. Bei Verwenden der API „ServiceProxy.Create“ zum Erstellen des Proxys erzeugt das Framework die ServiceProxyFactory als Singleton.
Es ist sinnvoll, eine manuell zu erstellen, wenn Sie [IServiceRemotingClientFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.client.iserviceremotingclientfactory)-Eigenschaften überschreiben müssen.
„Factory“ ist ein aufwendiger Vorgang. „ServiceProxyFactory“ verwaltet den Cache des Kommunikationsclients.
Es empfiehlt sich, „ServiceProxyFactory“ so lange wie möglich im Cache zwischenzuspeichern.

## <a name="remoting-exception-handling"></a>Behandlung von Remotingausnahmen
Alle von der Dienst-API ausgelösten Ausnahmen werden als „AggregateException“ an den Client zurückgesendet. „RemoteExceptions“ muss „DataContract Serializable“ sein, da andernfalls [ServiceException](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.serviceexception) für die Proxy-API mit enthaltenem Serialisierungsfehler ausgelöst wird.

„ServiceProxy“ verarbeitet sämtliche Failoverausnahmen für die Dienstpartition, für die seine Erstellung erfolgt ist. Dieser Proxy löst die Endpunkte erneut auf, falls Failoverausnahmen (nicht vorübergehende Ausnahmen) vorliegen, und wiederholt den Aufruf mit dem richtigen Endpunkt. Die Anzahl der Wiederholungen bei Failoverausnahmen ist unbegrenzt.
Wenn vorübergehende Ausnahmen auftreten, versucht der Proxy erneut, den Aufruf auszuführen.

Standardparameter für die Wiederholung werden von [OperationRetrySettings] angegeben. (https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.client.operationretrysettings) Der Benutzer kann diese Werte konfigurieren, indem er das Objekt „OperationRetrySettings“ an den Konstruktor „ServiceProxyFactory“ übergibt.
## <a name="how-to-use-remoting-v2-stack"></a>So verwenden Sie den Remoting V2-Stapel
Mit dem 2.8 NuGet Remoting-Paket haben Sie die Möglichkeit, den Remoting V2-Stapel zu verwenden. Der Remoting-V2-Stapel ist leistungsfähiger und stellt Funktionalität wie benutzerdefinierte serialisierbare und besser austauschbare APIs zur Verfügung.
Standardmäßig wird, wenn Sie die folgenden Änderungen nicht vornehmen, weiterhin der Remoting V1-Stapel verwendet.
Remoting V2 ist nicht mit V1 (vorheriger Remotingstapel) kompatibel. Lesen Sie daher im weiteren Verlauf des Artikels, wie ein Upgrade von V1 auf V2 erfolgt, ohne dass die Verfügbarkeit des Diensts beeinträchtigt wird.

### <a name="using-assembly-attribute-to-use-v2-stack"></a>Verwenden des Assembly-Attributs, um V2-Stapel zu verwenden

Es folgen die Schritte, die für einen Wechsel zu V2-Stapel auszuführen sind.

1. Fügen Sie im Dienstmanifest eine Endpunktressource namens „ServiceEndpointV2“ hinzu.

  ```xml
  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpointV2" />  
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

3.  Fügen Sie das Assembly-Attribut für Remotingschnittstellen hinzu.

  ```csharp
  [assembly: FabricTransportServiceRemotingProvider(RemotingListener = RemotingListener.V2Listener, RemotingClient = RemotingClient.V2Client)]
  ```
Im Clientprojekt sind keine Änderungen erforderlich.
Erstellen Sie die Client-Assembly mit der Schnittstellenassembly, um sicherzustellen, dass das obige Assembly Attribut verwendet wird.

### <a name="using-explicit-v2-classes-to-create-listener-clientfactory"></a>Verwenden von expliziten V2-Klassen, um Listener/ClientFactory zu erstellen
Es folgen die Schritte, die auszuführen sind.
1.  Fügen Sie im Dienstmanifest eine Endpunktressource namens „ServiceEndpointV2“ hinzu.

  ```xml
  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpointV2" />  
    </Endpoints>
  </Resources>
  ```

2. Verwenden Sie [RemotingV2Listener](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.v2.fabrictransport.runtime.fabrictransportserviceremotingistener?view=azure-dotnet). Der Standardname für die Dienstendpunktressource lautet „ServiceEndpointV2“, und dieser muss im Dienstmanifest definiert sein.

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

3. Verwenden Sie V2-[ClientFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.v2.fabrictransport.client.fabrictransportserviceremotingclientfactory?view=azure-dotnet).
  ```csharp
  var proxyFactory = new ServiceProxyFactory((c) =>
          {
              return new FabricTransportServiceRemotingClientFactory();
          });
  ```

## <a name="how-to-upgrade-from-remoting-v1-to-remoting-v2"></a>So aktualisieren Sie von Remoting V1 auf Remoting V2
Zum Aktualisieren von V1 auf V2 sind 2 Aktualisierungsschritte erforderlich. Die folgenden Schritte müssen in der aufgeführten Reihenfolge ausgeführt werden.

1. Aktualisieren Sie den V1-Dienst auf den V2-Dienst, indem Sie das CompactListener-Attribut verwenden.
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

    c) Fügen Sie das Assembly-Attribut für Remotingschnittstellen hinzu, damit „CompatListener“ und der V2-Client verwendet werden.
    ```csharp
    [assembly: FabricTransportServiceRemotingProvider(RemotingListener = RemotingListener.CompatListener, RemotingClient = RemotingClient.V2Client)]

      ```
2. Aktualisieren Sie den V1-Client auf den V2-Client, indem Sie das Attribut für den V2-Client verwenden.
Mit diesem Schritt wird sichergestellt, dass der Client den V2-Stapel verwendet.
Im Clientprojekt/-dienst ist keine Änderung erforderlich. Es genügt, Clientprojekte mit der aktualisierten Schnittstellenassembly zu erstellen.

3. Dieser Schritt ist optional. Verwenden Sie das V2Listener-Attribut, und aktualisieren Sie dann den V2-Dienst.
Mit diesem Schritt wird sichergestellt, dass der Dienst nur den V2-Listener abhört.

```csharp
[assembly: FabricTransportServiceRemotingProvider(RemotingListener = RemotingListener.V2Listener, RemotingClient = RemotingClient.V2Client)]
```

## <a name="how-to-use-custom-serialization-with-remoting-v2"></a>So wird die benutzerdefinierte Serialisierung mit Remoting V2 verwendet
Im folgenden Beispiel wird die Json-Serialisierung mit Remoting V2 verwendet.
1. Implementieren Sie die IServiceRemotingMessageSerializationProvider-Schnittstelle, um die Implementierung für die benutzerdefinierte Serialisierung bereitzustellen.
    Der Codeausschnitt für die Implementierung sieht wie folgt aus.

    ```csharp
    public class ServiceRemotingJsonSerializationProvider : IServiceRemotingMessageSerializationProvider
    {
      public IServiceRemotingRequestMessageBodySerializer CreateRequestMessageSerializer(Type serviceInterfaceType,
          IEnumerable<Type> requestBodyTypes)
      {
          return new ServiceRemotingRequestJsonMessageBodySerializer(serviceInterfaceType, requestBodyTypes);
      }

      public IServiceRemotingResponseMessageBodySerializer CreateResponseMessageSerializer(Type serviceInterfaceType,
          IEnumerable<Type> responseBodyTypes)
      {
          return new ServiceRemotingResponseJsonMessageBodySerializer(serviceInterfaceType, responseBodyTypes);
      }

      public IServiceRemotingMessageBodyFactory CreateMessageBodyFactory()
      {
          return new JsonMessageFactory();
      }
     }

  class JsonMessageFactory: IServiceRemotingMessageBodyFactory
  {
      public IServiceRemotingRequestMessageBody CreateRequest(string interfaceName, string methodName,
          int numberOfParameters)
      {
          return new JsonRemotingRequestBody(new JObject());
      }

      public IServiceRemotingResponseMessageBody CreateResponse(string interfaceName, string methodName)
      {
          return  new JsonRemotingResponseBody();
      }
   }

  class ServiceRemotingRequestJsonMessageBodySerializer: IServiceRemotingRequestMessageBodySerializer
  {
      public ServiceRemotingRequestJsonMessageBodySerializer(Type serviceInterfaceType,
          IEnumerable<Type> parameterInfo)
      {
      }

      public OutgoingMessageBody Serialize(IServiceRemotingRequestMessageBody serviceRemotingRequestMessageBody)
      {
          if (serviceRemotingRequestMessageBody == null)
          {
              return null;
          }

          var json = serviceRemotingRequestMessageBody.ToString();
          var bytes = Encoding.UTF8.GetBytes(json);
          var segment = new ArraySegment<byte>(bytes);
          var segments = new List<ArraySegment<byte>> {segment};
          return new OutgoingMessageBody(segments);
      }

      public IServiceRemotingRequestMessageBody Deserialize(IncomingMessageBody messageBody)
      {
          using (var sr = new StreamReader(messageBody.GetReceivedBuffer()))

          using (JsonReader reader = new JsonTextReader(sr))
          {
              var serializer = new JsonSerializer();
              var ob = serializer.Deserialize<JObject>(reader);
              var ob2 = new JsonRemotingRequestBody(ob);
              return ob2;
          }
      }
  }

  class ServiceRemotingResponseJsonMessageBodySerializer: IServiceRemotingResponseMessageBodySerializer
  {

      public ServiceRemotingResponseJsonMessageBodySerializer(Type serviceInterfaceType,
          IEnumerable<Type> parameterInfo)
      {
      }

      public OutgoingMessageBody Serialize(IServiceRemotingResponseMessageBody responseMessageBody)
      {
          var json = JsonConvert.SerializeObject(responseMessageBody,new JsonSerializerSettings()
          {
              TypeNameHandling = TypeNameHandling.All
          });
          var bytes = Encoding.UTF8.GetBytes(json);
          var segment = new ArraySegment<byte>(bytes);
          var list = new List<ArraySegment<byte>> {segment};
          return new OutgoingMessageBody(list);
      }

      public IServiceRemotingResponseMessageBody Deserialize(IncomingMessageBody messageBody)
      {
          using (var sr = new StreamReader(messageBody.GetReceivedBuffer()))

          using (var reader = new JsonTextReader(sr))
          {
              var serializer = JsonSerializer.Create(new JsonSerializerSettings()
              {
                  TypeNameHandling = TypeNameHandling.All
              });
              return serializer.Deserialize<JsonRemotingResponseBody>(reader);
          }
      }
  }
  internal class JsonRemotingResponseBody: IServiceRemotingResponseMessageBody
  {
      public object Value;

      public void Set(object response)
      {
          this.Value = response;
      }

      public object Get(Type paramType)
      {
          return this.Value;
      }
  }

  class JsonRemotingRequestBody: IServiceRemotingRequestMessageBody
    {
      private readonly JObject jobject;

        public JsonRemotingRequestBody(JObject ob)
        {
            this.jobject = ob;
        }

        public void SetParameter(int position, string parameName, object parameter)
        {
            this.jobject.Add(parameName, JToken.FromObject(parameter));
        }

        public object GetParameter(int position, string parameName, Type paramType)
       {
           var ob = this.jobject[parameName];
           return ob.ToObject(paramType);
       }

       public override string ToString()
        {
            return this.jobject.ToString();
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

