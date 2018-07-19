---
title: Übersicht über Azure Media Services mit der Widevine-Lizenzvorlage | Microsoft-Dokumentation
description: Dieses Thema bietet einen Überblick über eine Widevine-Lizenzvorlage, mit der Widevine-Lizenzen konfiguriert werden.
author: juliako
manager: cfowler
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: juliako
ms.openlocfilehash: e3af5efd253458401c13f6174d9567f932482eb0
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37133310"
---
# <a name="widevine-license-template-overview"></a>Übersicht über die Widevine-Lizenzvorlage

Widevine-Lizenzen können mithilfe von Azure Media Services konfiguriert und angefordert werden. Wenn der Player versucht, Ihre durch Widevine geschützten Inhalte wiederzugeben, wird eine Anforderung zum Erwerben einer Lizenz an den Lizenzbereitstellungsdienst gesendet. Wenn der Lizenzdienst die Anforderung genehmigt, stellt der Dienst die Lizenz aus. Sie wird an den Client gesendet und dient zum Entschlüsseln und Wiedergeben des angegebenen Inhalts.

Eine Widevine-Lizenzanforderung ist als JSON-Nachricht formatiert.  

>[!NOTE]
> Sie können eine leere Nachricht ohne Werte erstellen, indem Sie einfach „{}“ verwenden. Dann wird eine Lizenzvorlage mit Standardeinstellungen erstellt. Die Standardeinstellungen funktionieren in den meisten Fällen. Microsoft-basierte Lizenzübermittlungsszenarien sollten immer die Standardeinstellungen verwenden. Wenn Sie die Werte für „provider“ und „content_id“ festlegen müssen, muss der Anbieter den Widevine-Anmeldeinformationen entsprechen.

    {  
       "payload":"<license challenge>",
       "content_id": "<content id>"
       "provider": "<provider>"
       "allowed_track_types":"<types>",
       "content_key_specs":[  
          {  
             "track_type":"<track type 1>"
          },
          {  
             "track_type":"<track type 2>"
          },
          …
       ],
       "policy_overrides":{  
          "can_play":<can play>,
          "can persist":<can persist>,
          "can_renew":<can renew>,
          "rental_duration_seconds":<rental duration>,
          "playback_duration_seconds":<playback duration>,
          "license_duration_seconds":<license duration>,
          "renewal_recovery_duration_seconds":<renewal recovery duration>,
          "renewal_server_url":"<renewal server url>",
          "renewal_delay_seconds":<renewal delay>,
          "renewal_retry_interval_seconds":<renewal retry interval>,
          "renew_with_usage”:<renew with usage>
       }
    }

## <a name="json-message"></a>JSON-Nachricht

| NAME | Wert | BESCHREIBUNG |
| --- | --- | --- |
| payload |Base64-codierte Zeichenfolge |Die von einem Client gesendete Lizenzanforderung. |
| content_id |Base64-codierte Zeichenfolge |Bezeichner, der zum Ableiten der Schlüssel-ID und des Inhaltsschlüssels für „content_key_specs.track_type“ verwendet wird. |
| Anbieter |Zeichenfolge |Wird zum Nachschlagen von Inhaltsschlüsseln und Richtlinien verwendet. Wenn für die Widevine-Lizenzbereitstellung die Microsoft-Schlüsselübermittlung verwendet wird, wird dieser Parameter ignoriert. |
| policy_name |Zeichenfolge |Der Name einer zuvor registrierten Richtlinie. Optional. |
| allowed_track_types |enum |SD_ONLY oder SD_HD. Steuert, welche Inhaltsschlüssel in eine Lizenz aufgenommen werden. |
| content_key_specs |Array von JSON-Strukturen, siehe Abschnitt „Spezifikationen für Inhaltsschlüssel“.  |Eine feiner abgestimmte Steuerung der zurückzugebenden Inhaltsschlüssel. Weitere Informationen finden Sie im Abschnitt „Spezifikationen für Inhaltsschlüssel“. Nur einer der Werte („allowed_track_types“ oder „content_key_specs“) kann angegeben werden. |
| use_policy_overrides_exclusively |Boolescher Wert, true oder false |Verwenden Sie Richtlinienattribute, die von „policy_overrides“ angegeben werden, und lassen Sie alle zuvor gespeicherten Richtlinien weg. |
| policy_overrides |JSON-Struktur, siehe Abschnitt „Außerkraftsetzungen von Richtlinien“. |Richtlinieneinstellungen für diese Lizenz.  Falls dieses Medienobjekt über eine vorab definierte Richtlinie verfügt, werden diese angegebenen Werte verwendet. |
| session_init |JSON-Struktur, siehe Abschnitt „Sitzungsinitialisierung“. |An die Lizenz werden optionale Daten übergeben. |
| parse_only |Boolescher Wert, true oder false |Die Lizenzanforderung wird analysiert, jedoch wird keine Lizenz ausgestellt. Allerdings werden Werte aus der Lizenzanforderung in der Antwort zurückgegeben. |

## <a name="content-key-specs"></a>Spezifikationen für Inhaltsschlüssel
Wenn eine Richtlinie bereits vorhanden ist, müssen keine Werte in der Spezifikation für Inhaltsschlüssel angegeben werden. Die mit diesen Inhalten verbundene, bereits bestehende Richtlinie wird zur Bestimmung des Ausgabeschutzes verwendet, z. B. HDCP (High-bandwidth Digital Content Protection) und CGMS (Copy General Management System). Wenn eine vorhandene Richtlinie nicht beim Widevine-Lizenzserver registriert ist, kann der Inhaltsanbieter die Werte in die Lizenzanforderung einfügen.   

Jeder content_key_specs-Wert muss für alle Titel angegeben werden, unabhängig von der Option „use_policy_overrides_exclusively“. 

| NAME | Wert | BESCHREIBUNG |
| --- | --- | --- |
| content_key_specs. track_type |Zeichenfolge |Der Name eines Titeltyps. Wenn „content_key_specs“ in der Lizenzanforderung angegeben ist, sollten Sie unbedingt alle Titeltypen explizit angeben. Andernfalls wird die Wiedergabe nach 10 Sekunden beendet. |
| content_key_specs  <br/> security_level |UInt32 |Definiert die Clientstabilitätsanforderungen für die Wiedergabe. <br/> – Softwarebasierte White-Box-Kryptografie ist erforderlich. <br/> – Softwarekryptografie und ein verborgener Decoder sind erforderlich. <br/> – Die zentralen Vorgänge für Daten und Kryptografie müssen innerhalb einer hardwaregestützten vertrauenswürdigen Ausführungsumgebung ausgeführt werden. <br/> – Kryptografie und Decodierung müssen innerhalb einer hardwaregestützten vertrauenswürdigen Ausführungsumgebung ausgeführt werden.  <br/> – Kryptografie, Decodierung und Verarbeitung von Medien (komprimiert und nicht komprimiert) müssen innerhalb einer hardwaregestützten vertrauenswürdigen Ausführungsumgebung verarbeitet werden. |
| content_key_specs <br/> required_output_protection.hdc |Zeichenfolge, HDCP_NONE, HDCP_V1 oder HDCP_V2 |Gibt an, ob HDCP erforderlich ist. |
| content_key_specs <br/>key |Base64-<br/>codierte Zeichenfolge |Der Inhaltsschlüssel, der für diesen Titel verwendet werden soll. Wenn ein Wert angegeben wird, ist „track_type“ oder „key_id“ erforderlich. Mit dieser Option können Inhaltsanbieter den Inhaltsschlüssel für diesen Titel einfügen, statt einen Schlüssel durch den Widevine-Lizenzserver zu generieren oder zu suchen. |
| content_key_specs.key_id |Base64-codierte binäre Zeichenfolge, 16 Bytes |Eindeutiger Bezeichner für den Schlüssel. |

## <a name="policy-overrides"></a>Außerkraftsetzungen von Richtlinien
| NAME | Wert | BESCHREIBUNG |
| --- | --- | --- |
| policy_overrides&#46;can_play |Boolescher Wert, true oder false |Gibt an, dass die Wiedergabe des Inhalts zulässig ist. Die Standardeinstellung ist "false". |
| policy_overrides&#46;can_persist |Boolescher Wert, true oder false |Gibt an, dass die Lizenz in einem permanenten Speicher für die Offlineverwendung beibehalten werden kann. Die Standardeinstellung ist "false". |
| policy_overrides&#46;can_renew |Boolescher Wert, true oder false |Gibt an, dass die Verlängerung dieser Lizenz zulässig ist. Bei „true“ kann die Dauer der Lizenz über den Takt verlängert werden. Die Standardeinstellung ist "false". |
| policy_overrides&#46;license_duration_seconds |int64 |Gibt das Zeitfenster für diese bestimmte Lizenz an. Der Wert 0 gibt an, dass es keine Beschränkung für die Dauer gibt. Der Standardwert ist 0. |
| policy_overrides&#46;rental_duration_seconds |int64 |Gibt das Zeitfenster an, in dem die Wiedergabe zulässig ist. Der Wert 0 gibt an, dass es keine Beschränkung für die Dauer gibt. Der Standardwert ist 0. |
| policy_overrides&#46;playback_duration_seconds |int64 |Das Anzeigezeitfenster, sobald die Wiedergabe innerhalb der Lizenzdauer beginnt. Der Wert 0 gibt an, dass es keine Beschränkung für die Dauer gibt. Der Standardwert ist 0. |
| policy_overrides&#46;renewal_server_url |Zeichenfolge |Alle Heartbeatanforderungen (Verlängerung) für diese Lizenz werden an die angegebene URL weitergeleitet. Dieses Feld wird nur verwendet, wenn „can_renew“ auf „true“ festgelegt ist. |
| policy_overrides&#46;renewal_delay_seconds |int64 |Die Anzahl der Sekunden nach „license_start_time“, bevor der erste Versuch einer Verlängerung unternommen wird. Dieses Feld wird nur verwendet, wenn „can_renew“ auf „true“ festgelegt ist. Der Standardwert ist 0. |
| policy_overrides&#46;renewal_retry_interval_seconds |int64 |Gibt die Verzögerung in Sekunden zwischen den nachfolgenden Anforderungen zu Lizenzverlängerung an, falls ein Fehler auftritt. Dieses Feld wird nur verwendet, wenn „can_renew“ auf „true“ festgelegt ist. |
| policy_overrides&#46;renewal_recovery_duration_seconds |int64 |Das Zeitfenster, für das die Wiedergabe fortgesetzt werden kann, während der Versuch einer Verlängerung unternommen wird, aber aufgrund von Back-End-Problemen mit dem Lizenzserver noch nicht erfolgreich war. Der Wert 0 gibt an, dass es keine Beschränkung für die Dauer gibt. Dieses Feld wird nur verwendet, wenn „can_renew“ auf „true“ festgelegt ist. |
| policy_overrides&#46;renew_with_usage |Boolescher Wert, true oder false |Gibt an, dass die Lizenz zur Verlängerung gesendet wird, wenn die Verwendung startet. Dieses Feld wird nur verwendet, wenn „can_renew“ auf „true“ festgelegt ist. |

## <a name="session-initialization"></a>Sitzungsinitialisierung
| NAME | Wert | BESCHREIBUNG |
| --- | --- | --- |
| provider_session_token |Base64-codierte Zeichenfolge |Dieses Sitzungstoken wird wieder in der Lizenz übergeben und ist in nachfolgenden Verlängerungen vorhanden. Das Sitzungstoken bleibt nicht über Sitzungen hinaus erhalten. |
| provider_client_token |Base64-codierte Zeichenfolge |Clienttoken, das in der Lizenzantwort zurück gesendet wird. Wenn die Lizenzanforderung ein Clienttoken enthält, wird dieser Wert ignoriert. Das Clienttoken bleibt über Lizenzsitzungen hinaus erhalten. |
| override_provider_client_token |Boolescher Wert, true oder false |Wenn „false“ festgelegt wird und die Lizenzanforderung ein Clienttoken enthält, verwenden Sie das Token aus der Anforderung, selbst wenn ein Clienttoken in dieser Struktur angegeben wurde. Verwenden Sie bei „true“ immer das Token, das in dieser Struktur angegeben ist. |

## <a name="configure-your-widevine-license-with-net"></a>Konfigurieren der Widevine-Lizenz mit .NET 

Media Services umfasst eine Klasse, mit der Sie eine Widevine-Lizenz konfigurieren können. Um die Lizenz zu erstellen, übergeben Sie eine JSON-Zeichenfolge an [WidevineTemplate](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.contentkeypolicywidevineconfiguration.widevinetemplate?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_ContentKeyPolicyWidevineConfiguration_WidevineTemplate).

Zum Konfigurieren der Vorlage haben Sie folgende Möglichkeiten:

1.  Direktes Erstellen/Hartcodieren einer JSON-Zeichenfolge (kann fehleranfällig sein)

    ```csharp
        ContentKeyPolicyWidevineConfiguration objContentKeyPolicyWidevineConfiguration = new ContentKeyPolicyWidevineConfiguration
    {
        WidevineTemplate = @"{""allowed_track_types"":""SD_HD"",""content_key_specs"":[{""track_type"":""SD"",""security_level"":1,""required_output_protection"":{""hdcp"":""HDCP_V2""}}],""policy_overrides"":{""can_play"":true,""can_persist"":true,""can_renew"":false}}"
    };
    ```

2.  Erstellen erforderlicher Klassen mit Eigenschaften, die diesen JSON-Attributen zugeordnet sind, und Instanziieren der Klassen vor dem Serialisieren in der JSON-Zeichenfolge Nachfolgend sehen Sie ein Beispiel für diese Klassen und ihre Instanziierung und Serialisierung.

    ```csharp
    public class policy_overrides
    {
        public bool can_play { get; set; }
        public bool can_persist { get; set; }
        public bool can_renew { get; set; }
        public int rental_duration_seconds { get; set; }    //Indicates the time window while playback is permitted. A value of 0 indicates that there is no limit to the duration. Default is 0.
        public int playback_duration_seconds { get; set; }  //The viewing window of time after playback starts within the license duration. A value of 0 indicates that there is no limit to the duration. Default is 0.
        public int license_duration_seconds { get; set; }   //Indicates the time window for this specific license. A value of 0 indicates that there is no limit to the duration. Default is 0.
    }

    public class content_key_spec
    {
        public string track_type { get; set; }
        public int security_level { get; set; }
        public output_protection required_output_protection { get; set; }
    }

    public class output_protection
    {
        public string hdcp { get; set; }
    }

    public class widevine_template
    {
        public string allowed_track_types { get; set; }
        public content_key_spec[] content_key_specs { get; set; }
        public policy_overrides policy_overrides { get; set; }
    }
    ```

### <a name="configure-the-license"></a>Konfigurieren der Lizenz

Verwenden Sie die im vorherigen Abschnitt definierten Klassen, um eine JSON-Zeichenfolge zu erstellen, mit der [WidevineTemplate](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.contentkeypolicywidevineconfiguration.widevinetemplate?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_ContentKeyPolicyWidevineConfiguration_WidevineTemplate) konfiguriert wird:

```csharp
void ConfigureLicense()
{
    widevine_template objwidevine_template = new widevine_template()
    {
        allowed_track_types = "SD_HD",
        content_key_specs = new content_key_spec[]
        {
            new content_key_spec()
            {
                track_type = "SD",
                security_level = 1,
                required_output_protection = new output_protection()
                {
                hdcp = "HDCP_V2"
                }
            }
        },
        policy_overrides = new policy_overrides()
        {
            can_play = true,
            can_persist = true,
            can_renew = false,
            license_duration_seconds = 2592000,
            playback_duration_seconds = 10800,
            rental_duration_seconds = 604800,
        }
    };

    ContentKeyPolicyWidevineConfiguration objContentKeyPolicyWidevineConfiguration = new ContentKeyPolicyWidevineConfiguration
    {
        WidevineTemplate = Newtonsoft.Json.JsonConvert.SerializeObject(objwidevine_template)
    };
}
```

## <a name="next-steps"></a>Nächste Schritte

[Übersicht](content-protection-overview.md)