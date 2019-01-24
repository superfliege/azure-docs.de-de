---
title: Archivieren des Azure-Aktivitätsprotokolls
description: Archivieren Ihres Azure-Aktivitätsprotokolls zur langfristigen Aufbewahrung in einem Speicherkonto.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/07/2018
ms.author: johnkem
ms.subservice: logs
ms.openlocfilehash: d9abfe90296b27918594c41a207befe2b59027b9
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54461595"
---
# <a name="archive-the-azure-activity-log"></a>Archivieren des Azure-Aktivitätsprotokolls
In diesem Artikel erfahren Sie, wie Sie Ihr [**Azure-Aktivitätsprotokoll**](../../azure-monitor/platform/activity-logs-overview.md) über das Azure-Portal, mithilfe von PowerShell-Cmdlets oder mithilfe der plattformübergreifenden Befehlszeilenschnittstelle in einem Speicherkonto archivieren. Dies ist hilfreich, wenn Sie Ihr Aktivitätsprotokoll (bei vollständiger Kontrolle über die Aufbewahrungsrichtlinie) zur Überwachung, statischen Analyse oder als Sicherungskopie länger als 90 Tage aufbewahren möchten. Falls Sie Ihre Ereignisse nur maximal 90 Tage lang aufbewahren möchten, müssen Sie keine Archivierung in einem Speicherkonto einrichten, da Aktivitätsprotokollereignisse in der Azure-Plattform auch ohne aktivierte Archivierung 90 Tage lang aufbewahrt werden.

> [!WARNING]
> Das Format der Protokolldaten im Speicherkonto wird am 1. November 2018 in JSON Lines geändert. [Dieser Artikel enthält eine Beschreibung der Auswirkungen und der Aktualisierung Ihrer Tools zur Verarbeitung des neuen Formats.](./../../azure-monitor/platform/diagnostic-logs-append-blobs.md) 
>
> 

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie beginnen, müssen Sie [ein Speicherkonto erstellen](../../storage/common/storage-quickstart-create-account.md) , in dem Sie Ihr Aktivitätsprotokoll archivieren können. Um den Zugriff auf Überwachungsdaten besser steuern zu können, wird dringend davon abgeraten, ein bereits vorhandenes Speicherkonto mit anderen, nicht überwachungsbezogenen Daten zu verwenden. Wenn Sie jedoch auch Diagnoseprotokolle und Metriken in einem Speicherkonto archivieren, ist es unter Umständen sinnvoll, dieses Speicherkonto auch für Ihr Aktivitätsprotokoll zu verwenden, damit sich alle Überwachungsdaten an einem zentralen Ort befinden. Das Speicherkonto muss sich nicht unter demselben Abonnement befinden, das Protokolle ausgibt, sofern der Benutzer, der die Einstellung konfiguriert, den entsprechenden RBAC-Zugriff auf beide Abonnements hat.

> [!NOTE]
>  Sie können derzeit keine Daten in einem Speicherkonto archivieren, das hinter einem geschützten virtuellen Netzwerk erstellt wird.

## <a name="log-profile"></a>Protokollprofil
Legen Sie das **Protokollprofil** für ein Abonnement fest, um das Aktivitätsprotokoll mit einer der weiter unten angegebenen Methoden zu archivieren. Das Protokollprofil definiert die Art der Ereignisse, die gespeichert oder gestreamt werden, sowie die Ausgaben (Speicherkonto und/oder Event Hub). Außerdem definiert es die Aufbewahrungsrichtlinie (Anzahl von Tagen für die Aufbewahrung) für Ereignisse, die in einem Speicherkonto gespeichert werden. Wird die Aufbewahrungsrichtlinie auf Null festgelegt, werden Ereignisse dauerhaft gespeichert. Andernfalls kann sie auf einen beliebigen Wert zwischen 1 und 2.147.483.647 festgelegt werden. Aufbewahrungsrichtlinien werden pro Tag angewendet, sodass Protokolle am Ende eines Tages (UTC) ab dem Tag, der nun außerhalb der Aufbewahrungsrichtlinie liegt, gelöscht werden. Beispiel: Wenn Sie eine Aufbewahrungsrichtlinie für einen Tag verwenden, werden heute am Anfang des Tages die Protokolle von vorgestern gelöscht. Der Löschvorgang beginnt um Mitternacht (UTC), jedoch kann es bis zu 24 Stunden dauern, bis die Protokolle aus Ihrem Speicherkonto gelöscht werden. Weitere Informationen zu Protokollprofilen finden Sie [hier](../../azure-monitor/platform/activity-logs-overview.md#export-the-activity-log-with-a-log-profile). 

## <a name="archive-the-activity-log-using-the-portal"></a>Archivieren des Aktivitätsprotokolls über das Portal
1. Klicken Sie im linken Navigationsbereich des Portals auf den Link **Aktivitätsprotokoll** . Sollte kein Link für das Aktivitätsprotokoll angezeigt werden, klicken Sie zuerst auf den Link **Alle Dienste**.
   
    ![Zum Blatt „Aktivitätsprotokoll“ navigieren](media/archive-activity-log/activity-logs-portal-navigate-v2.png)
2. Klicken Sie oben auf dem Blatt auf **In Event Hub exportieren**.
   
    ![Auf die Schaltfläche „Exportieren“ klicken](media/archive-activity-log/activity-logs-portal-export-v2.png)
3. Aktivieren Sie auf dem daraufhin angezeigten Blatt das Kontrollkästchen **Export to a storage account** (In ein Speicherkonto exportieren), und wählen Sie ein Speicherkonto aus.
   
    ![Speicherkonto festlegen](media/archive-activity-log/act-log-portal-export-blade.png)
4. Geben Sie mithilfe des Schiebereglers oder des Textfelds an, für wie viele Tage (0 bis 365) Aktivitätsprotokollereignisse in Ihrem Speicherkonto aufbewahrt werden sollen. Falls die Daten dauerhaft im Speicherkonto gespeichert werden sollen, legen Sie den Wert auf „0“ fest. Wenn Sie für die Anzahl Tage eine größere Zahl als 365 eingeben müssen, verwenden Sie die unten beschriebenen Verfahren in PowerShell oder auf der Befehlszeilenschnittstelle.
5. Klicken Sie auf **Speichern**.

## <a name="archive-the-activity-log-via-powershell"></a>Archivieren des Aktivitätsprotokolls mithilfe von PowerShell

   ```powershell
   # Settings needed for the new log profile
   $logProfileName = "default"
   $locations = (Get-AzureRmLocation).Location
   $locations += "global"
   $subscriptionId = "<your Azure subscription Id>"
   $resourceGroupName = "<resource group name your storage account belongs to>"
   $storageAccountName = "<your storage account name>"

   # Build the storage account Id from the settings above
   $storageAccountId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Storage/storageAccounts/$storageAccountName"

   Add-AzureRmLogProfile -Name $logProfileName -Location $locations -StorageAccountId $storageAccountId
   ```

| Eigenschaft | Erforderlich | BESCHREIBUNG |
| --- | --- | --- |
| StorageAccountId |JA |Ressourcen-ID des Speicherkontos, in dem Aktivitätsprotokolle gespeichert werden sollen. |
| Standorte |JA |Kommagetrennte Liste mit den Regionen, für die Sie Aktivitätsprotokollereignisse erfassen möchten. Mit `(Get-AzureRmLocation).Location` können Sie eine Liste aller Regionen für Ihr Abonnement anzeigen. |
| RetentionInDays |Nein  |Anzahl von Tagen für die Aufbewahrung von Ereignissen (1 bis 2.147.483.647). Bei einem Wert von 0 werden die Protokolle dauerhaft (d.h. für immer) gespeichert. |
| Categories |Nein  |Kommagetrennte Liste mit den Ereigniskategorien, die erfasst werden sollen. Mögliche Werte sind „Write“, „Delete“ und „Action“.  Wird kein Wert angegeben, werden alle möglichen Werte angenommen. |

## <a name="archive-the-activity-log-via-cli"></a>Archivieren des Aktivitätsprotokolls mithilfe der Befehlszeilenschnittstelle

   ```azurecli-interactive
   az monitor log-profiles create --name "default" --location null --locations "global" "eastus" "westus" --categories "Delete" "Write" "Action"  --enabled false --days 0 --storage-account-id "/subscriptions/<YOUR SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>"
   ```

| Eigenschaft | Erforderlich | BESCHREIBUNG |
| --- | --- | --- |
| name |JA |Name des Protokollprofils. |
| storage-account-id |JA |Ressourcen-ID des Speicherkontos, in dem Aktivitätsprotokolle gespeichert werden sollen. |
| Locations |JA |Durch Leerzeichen getrennte Liste mit den Regionen, für die Sie Aktivitätsprotokollereignisse erfassen möchten. Mit `az account list-locations --query [].name` können Sie eine Liste aller Regionen für Ihr Abonnement anzeigen. |
| Tage |JA |Anzahl von Tagen für die Aufbewahrung von Ereignissen (1 bis 2.147.483.647). Bei einem Wert von 0 werden die Protokolle dauerhaft (d.h. für immer) gespeichert.  Wenn der Wert 0 ist, muss der aktivierte Parameter auf „true“ festgelegt werden. |
|Aktiviert | JA |„True“ oder „False“.  Wird zum Aktivieren bzw. Deaktivieren der Aufbewahrungsrichtlinie verwendet.  Ist „True“ festgelegt, muss für den Parameter „days“ ein Wert größer 0 angegeben werden.
| categories |JA |Durch Leerzeichen getrennte Liste mit den Ereigniskategorien, die erfasst werden sollen. Mögliche Werte sind „Write“, „Delete“ und „Action“. |

## <a name="storage-schema-of-the-activity-log"></a>Speicherschema des Aktivitätsprotokolls
Nach dem Einrichten der Archivierung wird in dem Speicherkonto ein Speichercontainer erstellt, sobald ein Aktivitätsprotokollereignis auftritt. Die Blobs innerhalb des Containers befolgen in Aktivitätsprotokollen und Diagnoseprotokollen übergreifend die gleiche Namenskonvention, wie hier zu sehen:

```
insights-operational-logs/name=default/resourceId=/SUBSCRIPTIONS/{subscription ID}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Beispiel für einen Blobnamen:

```
insights-operational-logs/name=default/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

Jedes Blob vom Typ „PT1H.json“ enthält ein JSON-Blob mit Ereignissen, die innerhalb der in der Blob-URL angegebenen Stunde (Beispiel: h=12) aufgetreten sind. Während der aktuellen Stunde werden Ereignisse an die Datei „PT1H.json“ angefügt, sobald sie auftreten. Der Minutenwert (m=00) ist immer „00“, da Aktivitätsprotokollereignisse stundenweise in einzelne Blobs unterteilt werden.

Die einzelnen Ereignisse werden innerhalb der Datei „PT1H.json“ im folgenden Format im Array „records“ gespeichert:

``` JSON
{
    "records": [
        {
            "time": "2015-01-21T22:14:26.9792776Z",
            "resourceId": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
            "operationName": "microsoft.support/supporttickets/write",
            "category": "Write",
            "resultType": "Success",
            "resultSignature": "Succeeded.Created",
            "durationMs": 2826,
            "callerIpAddress": "111.111.111.11",
            "correlationId": "c776f9f4-36e5-4e0e-809b-c9b3c3fb62a8",
            "identity": {
                "authorization": {
                    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
                    "action": "microsoft.support/supporttickets/write",
                    "evidence": {
                        "role": "Subscription Admin"
                    }
                },
                "claims": {
                    "aud": "https://management.core.windows.net/",
                    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
                    "iat": "1421876371",
                    "nbf": "1421876371",
                    "exp": "1421880271",
                    "ver": "1.0",
                    "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
                    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
                    "puid": "20030000801A118C",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
                    "name": "John Smith",
                    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
                    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
                    "appidacr": "2",
                    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
                    "http://schemas.microsoft.com/claims/authnclassreference": "1"
                }
            },
            "level": "Information",
            "location": "global",
            "properties": {
                "statusCode": "Created",
                "serviceRequestId": "50d5cddb-8ca0-47ad-9b80-6cde2207f97c"
            }
        }
    ]
}
```


| Elementname | BESCHREIBUNG |
| --- | --- |
| time |Zeitstempel der Ereignisgenerierung durch den Azure-Dienst, der die zum Ereignis gehörende Anforderung verarbeitet hat. |
| ResourceId |Ressourcen-ID der betroffenen Ressource. |
| operationName |Name des Vorgangs. |
| category |Kategorie der Aktion, z.B. Schreiben, Lesen, Aktion. |
| resultType |Der Typ des Ergebnisses, z.B. Erfolg, Fehler, Start |
| resultSignature |Abhängig vom Ressourcentyp. |
| durationMs |Dauer des Vorgangs in Millisekunden. |
| callerIpAddress |IP-Adresse des Benutzers, der den Vorgang, UPN-Anspruch oder SPN-Anspruch ausgeführt hat (sofern verfügbar). |
| correlationId |Normalerweise eine GUID im Zeichenfolgenformat. Ereignisse, die über die gleiche correlationId verfügen, gehören zu derselben übergeordneten Aktion. |
| identity |JSON-Blob zur Beschreibung der Autorisierung und Ansprüche. |
| authorization |Blob mit RBAC-Eigenschaften des Ereignisses. Enthält normalerweise die Eigenschaften „action“, „role“ und „scope“. |
| level |Ebene des Ereignisses. Einer der folgenden Werte: Critical, Error, Warning, Informational oder Verbose |
| location |Region des Speicherorts (oder „global“). |
| Eigenschaften |Satz mit `<Key, Value>` -Paaren (Wörterbuch), die die Details des Ereignisses beschreiben. |

> [!NOTE]
> Die Eigenschaften und deren Verwendung können je nach Ressource variieren.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
* [Herunterladen von Blobs für die Analyse](../../storage/blobs/storage-quickstart-blobs-dotnet.md)
* [Streamen des Aktivitätsprotokolls an Event Hubs](../../azure-monitor/platform/activity-logs-stream-event-hubs.md)
* [Weitere Informationen zum Aktivitätsprotokoll](../../azure-monitor/platform/activity-logs-overview.md)


