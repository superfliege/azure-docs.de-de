.NET-Anwendungen können verwenden die **StackExchange.Redis** Cacheclient, die in Visual Studio mithilfe von NuGet-Paket, das die Konfiguration der cacheclientanwendungen vereinfachen konfiguriert werden können. 

> [!NOTE]
> Weitere Informationen finden Sie unter der [StackExchange.Redis](http://github.com/StackExchange/StackExchange.Redis) Github-Seite und die [Dokumentation zum StackExchange.Redis Cache](http://github.com/StackExchange/StackExchange.Redis#documentation).
> 
> 

Um eine Clientanwendung in Visual Studio mithilfe des StackExchange.Redis-NuGet-Pakets zu konfigurieren, Maustaste das Projekt im **Projektmappen-Explorer** , und wählen Sie **NuGet-Pakete verwalten**. 

![NuGet-Pakete verwalten](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-manage-nuget-menu.png)

Typ **StackExchange.Redis** oder **StackExchange.Redis.StrongName** in das Suchfeld ein, wählen Sie die gewünschte Version aus den Ergebnissen, und klicken Sie auf **installieren**.

> [!NOTE]
> Wenn Sie eine Version mit starkem Namen verwenden möchten die **StackExchange.Redis** -Clientbibliothek verwenden, wählen Sie **StackExchange.Redis.StrongName**; andernfalls wählen Sie **StackExchange.Redis**.
> 
> 

![StackExchange.Redis-NuGet-Paket](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-stackexchange-redis.png)

Das NuGet-Paket wird heruntergeladen und fügt die erforderlichen Assemblyverweise für Ihre Clientanwendung zum Zugriff auf Azure-Redis-Cache mit dem StackExchange.Redis-Cacheclient.

> [!NOTE]
> Wenn Sie das Projekt zur Verwendung des StackExchange.Redis zuvor konfiguriert haben, sehen Sie sich nach Updates für das Paket aus der **NuGet Package Manager**. Um zu überprüfen und aktualisierte Versionen der StackExchange.Redis-NuGet-Paket installiert haben, klicken Sie auf **Updates** in der die **NuGet Package Manager** Fenster. Wenn ein Update für den StackExchange.Redis-NuGet-Paket verfügbar ist, können Sie das Projekt zur Verwendung der aktualisierten Version aktualisieren.
> 
> 

Sie können auch das StackExchange.Redis NuGet-Paket installieren, indem Sie auf **NuGet Package Manager**, **Package Manager Console** aus der **Tools** mit dem folgenden Befehl aus, und im Menü der **Package Manager Console** Fenster.
    
```
Install-Package StackExchange.Redis
```
