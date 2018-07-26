---
title: Implementieren der Notfallwiederherstellung mit Sichern und Wiederherstellen in Azure API Management | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Sichern und Wiederherstellen zur Notfallwiederherstellung in Azure API Management verwenden.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2018
ms.author: apimpm
ms.openlocfilehash: b06a179459a449762555879669d177f811cb9560
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090876"
---
# <a name="how-to-implement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a>So implementieren Sie die Notfallwiederherstellung mit Sichern und Wiederherstellen von Diensten in Azure API Management

Indem Sie Ihre APIs über Azure API Management veröffentlichen und verwalten, profitieren Sie von vielen Fehlertoleranz- und Infrastrukturfunktionen, die Sie sonst erst entwickeln, implementieren und verwalten müssten. Die Azure-Plattform minimiert einen Großteil der potenziellen Ausfälle zu einem Bruchteil der Kosten.

Bei Verfügbarkeitsproblemen in der Region, in der Ihr API Management-Dienst gehostet wird, sollten Sie jederzeit dazu in der Lage sein, Ihren Dienst in einer anderen Region wiederherzustellen. Je nach Verfügbarkeitsziel und Wiederherstellungszeitfenster möchten Sie möglicherweise einen Sicherungsdienst in einer oder mehreren Regionen reservieren, dessen Konfiguration und Inhalt möglichst mit dem aktiven Dienst synchron sind. Das Feature zum Sichern und Wiederherstellen von Diensten ist grundlegend für die Implementierung Ihrer Strategie zur Notfallwiederherstellung.

In dieser Anleitung wird erläutert, wie Anforderungen von Azure Resource Manager authentifiziert und API Management-Dienstinstanzen gesichert und wiederhergestellt werden.

> [!NOTE]
> Der Vorgang zum Sichern und Wiederherstellen einer API Management-Dienstinstanz für die Notfallwiederherstellung kann auch zum Replizieren von API Management-Dienstinstanzen für Szenarios wie z. B. das Staging verwendet werden.
>
> Jede Sicherung läuft nach 30 Tagen ab. Wenn Sie versuchen, eine Sicherung nach dem Ablaufzeitraum von 30 Tagen wiederherzustellen, erhalten Sie die Fehlermeldung `Cannot restore: backup expired`.
>
>

## <a name="authenticating-azure-resource-manager-requests"></a>Authentifizieren von Anforderungen des Azure-Ressourcen-Managers

> [!IMPORTANT]
> Die REST-API für die Sicherung und Wiederherstellung verwendet den Azure-Ressourcen-Manager und weist eine andere Authentifizierungsmethode als die REST-APIs für die Verwaltung von API Management-Entitäten auf. In den Schritten in diesem Abschnitt wird beschrieben, wie Anforderungen des Azure-Ressourcen-Managers authentifiziert werden. Weitere Informationen finden Sie unter [Authentifizieren von Anforderungen des Azure-Ressourcen-Managers](http://msdn.microsoft.com/library/azure/dn790557.aspx).
>
>

Alle Aufgaben, die Sie mithilfe von Azure Resource Manager für Ressourcen ausführen, müssen entsprechend den folgenden Schritten mit Azure Active Directory authentifiziert werden:

* Fügen Sie dem Azure Active Directory-Mandanten eine Anwendung hinzu.
* Legen Sie Berechtigungen für die hinzugefügte Anwendung fest.
* Rufen Sie das Token zum Authentifizieren von Anforderungen an den Azure-Ressourcen-Manager ab.

### <a name="create-an-azure-active-directory-application"></a>Erstellen einer Azure Active Directory-Anwendung

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
2. Verwenden Sie das Abonnement, das Ihre API Management-Dienstinstanz enthält, und navigieren Sie zur Registerkarte **App-Registrierungen** in **Azure Active Directory** (Azure Active Directory > Verwalten > App-Registrierungen).

    > [!NOTE]
    > Wenn das Azure Active Directory-Standardverzeichnis in Ihrem Konto nicht angezeigt wird, bitten Sie den Administrator des Azure-Abonnements, die erforderlichen Berechtigungen für das Konto zu erteilen.
3. Klicken Sie auf **Registrierung einer neuen Anwendung**.

    Das Fenster **Erstellen** wird auf der rechten Seite angezeigt. Dort geben Sie die relevanten Informationen der AAD-App ein.
4. Geben Sie einen Namen für die Anwendung ein.
5. Wählen Sie als Anwendungstyp **Nativ** aus.
6. Geben Sie eine Platzhalter-URL wie z.B. `http://resources` für den **Umleitungs-URI** ein, da es sich um ein Pflichtfeld handelt. Der Wert wird jedoch später nicht verwendet. Klicken Sie auf das Kontrollkästchen, um die Anwendung zu speichern.
7. Klicken Sie auf **Create**.

### <a name="add-an-application"></a>Hinzufügen einer Anwendung

1. Nachdem die Anwendung erstellt wurde, klicken Sie auf **Einstellungen**.
2. Klicken Sie auf **Erforderliche Berechtigungen**.
3. Klicken Sie auf **+Hinzufügen**.
4. Drücken Sie **API auswählen**.
5. Wählen Sie **Windows** **Azure-Dienstverwaltungs-API** aus.
6. Drücken Sie **Auswählen**. 

    ![Hinzufügen von Berechtigungen](./media/api-management-howto-disaster-recovery-backup-restore/add-app.png)

7. Klicken Sie neben der neu hinzugefügten Anwendung auf **Delegierte Berechtigungen**, und aktivieren Sie das Kontrollkästchen **Access Azure Service Management (Vorschau)**.
8. Drücken Sie **Auswählen**.

### <a name="configuring-your-app"></a>Konfigurieren der App

Vor dem Aufrufen der APIs, die die Sicherung erstellen und wiederherstellen, muss ein Token abgerufen werden. Im folgenden Beispiel wird das Token mit dem NuGet-Paket [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) abgerufen.

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System;

namespace GetTokenResourceManagerRequests
{
    class Program
    {
        static void Main(string[] args)
        {
            var authenticationContext = new AuthenticationContext("https://login.microsoftonline.com/{tenant id}");
            var result = authenticationContext.AcquireToken("https://management.azure.com/", {application id}, new Uri({redirect uri});

            if (result == null) {
                throw new InvalidOperationException("Failed to obtain the JWT token");
            }

            Console.WriteLine(result.AccessToken);

            Console.ReadLine();
        }
    }
}
```

Ersetzen Sie `{tentand id}`, `{application id}` und `{redirect uri}` entsprechend den folgenden Anweisungen:

1. Ersetzen Sie `{tenant id}` durch die Mandanten-ID der Azure Active Directory-Anwendung, die Sie erstellt haben. Auf die ID können Sie durch Klicken auf **App-Registrierungen** -> **Endpunkte** zugreifen.

    ![Endpunkte][api-management-endpoint]
2. Ersetzen Sie `{application id}` durch den Wert, den Sie durch Navigieren zur Seite **Einstellungen** erhalten.
3. Ersetzen Sie `{redirect uri}` durch den Wert der Registerkarte **Weiterleitungs-URIs** Ihrer Azure Active Directory-Anwendung.

    Wenn die Werte angegeben wurden, sollte im Codebeispiel ein Token ähnlich dem folgenden Beispiel zurückgegeben werden:

    ![Tokenverschlüsselung][api-management-arm-token]

    > [!NOTE]
    > Das Token kann nach einer bestimmten Zeit ablaufen. Führen Sie das Codebeispiel erneut aus, um ein neues Token zu generieren.

## <a name="calling-the-backup-and-restore-operations"></a>Aufrufen der Sicherungs- und Wiederherstellungsvorgänge

Legen Sie vor dem Aufrufen der in den folgenden Abschnitten beschriebenen Vorgänge zur Sicherung und Wiederherstellung den Autorisierungsanforderungsheader für den REST-Aufruf fest.

```csharp
request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);
```

### <a name="step1"></a>Sichern eines API Management-Diensts
Zum Sichern eines API Management-Diensts führen Sie die folgende HTTP-Anforderung aus:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}`

Hierbei gilt:

* `subscriptionId` – Abonnement-ID des API Management-Diensts, den Sie sichern möchten
* `resourceGroupName` – der Name der Ressourcengruppe Ihres Azure API Management-Diensts
* `serviceName` – Der Name des zu sichernden API Management-Diensts zum Zeitpunkt seiner Erstellung
* `api-version` – ersetzen durch `2014-02-14`

Geben Sie im Hauptteil der Anforderung das Azure-Zielspeicherkonto, den Zugriffsschlüssel, den Blobcontainernamen und den Sicherungsnamen an:

```
'{  
    storageAccount : {storage account name for the backup},  
    accessKey : {access key for the account},  
    containerName : {backup container name},  
    backupName : {backup blob name}  
}'
```

Legen Sie für den `Content-Type`-Anforderungsheader den Wert `application/json` fest.

Die Sicherung ist ein länger anhaltender Vorgang, der bis zum Abschluss mehrere Minuten dauern kann.  Falls die Anforderung erfolgreich war und der Sicherungsvorgang eingeleitet wurde, erhalten Sie den Antwortstatuscode `202 Accepted`, zusammen mit einem `Location`-Header.  Senden Sie GET-Anforderungen der URL im `Location` -Header, um den Status des Vorgangs zu ermitteln. Während der Sicherung erhalten Sie weiterhin den Statuscode „202 Accepted“. Mit dem Antwortcode `200 OK` wird der erfolgreiche Abschluss des Sicherungsvorgangs angezeigt.

Beachten Sie die folgenden Einschränkungen für Sicherungsanforderungen.

* Der im Hauptteil der Anforderung angegebene **Container** **muss vorhanden sein**.
* **Versuchen Sie nicht, bei laufender Sicherung Dienstverwaltungsvorgänge durchzuführen**, z.B. SKU-Upgrade oder -Downgrade, Wechseln des Domänennamens usw.
* Die Wiederherstellung einer Sicherung nach ihrer Erstellung **wird nur 30 Tage lange garantiert**.
* **Nutzungsdaten** zum Erstellen von Analyseberichten sind in der Sicherung **nicht enthalten**. Verwenden Sie die [Azure API Management-REST-API][Azure API Management REST API], um regelmäßig Analyseberichte zur Aufbewahrung abzurufen.
* Die Häufigkeit, mit der Sie Dienstsicherungen durchführen, wirkt sich auf das Ziel Ihres Wiederherstellungspunkts aus. Um die Auswirkungen zu minimieren, wird empfohlen, regelmäßige Sicherungen zu implementieren und bei Bedarf Sicherungen durchzuführen, wenn Sie bedeutende Änderungen an Ihrem API Management-Dienst vorgenommen haben.
* **Änderungen** an der Dienstkonfiguration (z.B. APIs, Richtlinien, Erscheinungsbild des Entwicklerportals), die während des Sicherungsvorgangs vorgenommen werden, sind ggf. **nicht in der Sicherung enthalten und gehen somit verloren**.

### <a name="step2"></a>Wiederherstellen eines API Management-Diensts
Zum Wiederherstellen eines API Management-Diensts aus einer zuvor erstellten Sicherung führen Sie die folgende HTTP-Anforderung aus:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}`

Hierbei gilt:

* `subscriptionId`– Abonnement-ID des API Management-Diensts, den Sie sichern möchten
* `resourceGroupName` – Zeichenfolge in der Form „Api-Default-{service-region}“, wobei `service-region` die Azure-Region angibt, in der der zu wiederherzustellende API Management-Dienst gehostet wird, z.B. `North-Central-US`
* `serviceName` – Der Name des wiederherzustellenden API Management-Diensts zum Zeitpunkt seiner Erstellung
* `api-version` – ersetzen durch `2014-02-14`

Geben Sie im Hauptteil der Anforderung den Speicherort der Sicherungsdatei an, d.h. das Azure Storage-Konto, den Zugriffsschlüssel, den Blobcontainernamen und den Sicherungsnamen:

```
'{  
    storageAccount : {storage account name for the backup},  
    accessKey : {access key for the account},  
    containerName : {backup container name},  
    backupName : {backup blob name}  
}'
```

Legen Sie für den `Content-Type`-Anforderungsheader den Wert `application/json` fest.

Die Wiederherstellung ist ein länger anhaltender Vorgang, der bis zum Abschluss bis zu 30 Minuten dauern kann. Falls die Anforderung erfolgreich war und der Wiederherstellungsvorgang eingeleitet wurde, erhalten Sie den Antwortstatuscode `202 Accepted`, zusammen mit einem `Location`-Header. Senden Sie GET-Anforderungen der URL im `Location` -Header, um den Status des Vorgangs zu ermitteln. Während der Wiederherstellung erhalten Sie weiterhin den Statuscode „202 Accepted“. Mit dem Antwortcode `200 OK` wird der erfolgreiche Abschluss des Wiederherstellungsvorgangs angezeigt.

> [!IMPORTANT]
> **Die SKU** des wiederherzustellenden Diensts **muss** mit der SKU des gesicherten Diensts übereinstimmen.
>
> **Änderungen** an der Dienstkonfiguration (z.B. APIs, Richtlinien, Erscheinungsbild des Entwicklerportals), die während des Wiederherstellungsvorgangs vorgenommen werden, **könnten überschrieben werden**.

> [!NOTE]
> Sicherungs- und Wiederherstellungsvorgänge können ebenfalls mithilfe der PowerShell-Befehle *Backup-AzureRmApiManagement* bzw. *Restore-AzureRmApiManagement* ausgeführt werden.

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich die folgenden Microsoft-Blogs für zwei verschiedene Vorgehensweisen des Sicherungs- und Wiederherstellungsvorgangs an.

* [Replicate Azure API Management Accounts (in englischer Sprache)](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/)
* [Azure API Management: Backing Up and Restoring Configuration (in englischer Sprache)](http://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
  * Der Ansatz von Stuart entspricht nicht der offiziellen Anleitung, ist aber sehr interessant.

[Backup an API Management service]: #step1
[Restore an API Management service]: #step2


[Azure API Management REST API]: http://msdn.microsoft.com/library/azure/dn781421.aspx

[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png

[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png
