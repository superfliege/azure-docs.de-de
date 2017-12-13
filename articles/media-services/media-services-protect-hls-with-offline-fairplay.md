---
title: "Schützen von HLS-Inhalten mit Apple FairPlay im Offlinemodus – Azure | Microsoft-Dokumentation"
description: "Dieses Thema bietet einen Überblick und veranschaulicht, wie Sie Azure Media Services verwenden, um Ihre HLS-Inhalte (HTTP Live Streaming) dynamisch mit Apple FairPlay im Offlinemodus verschlüsseln."
services: media-services
keywords: HLS, DRM, FairPlay Streaming (FPS), Offline, iOS 10
documentationcenter: 
author: willzhan
manager: steveng
editor: 
ms.assetid: 7c3b35d9-1269-4c83-8c91-490ae65b0817
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: willzhan, dwgeo
ms.openlocfilehash: bf5828ecd6b6bd2e862c4d7709014ecac47c6be0
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2017
---
# <a name="offline-fairplay-streaming"></a>FairPlay Streaming im Offlinemodus
Microsoft Azure Media Services bietet eine Reihe von ausgereiften [Content Protection-Diensten](https://azure.microsoft.com/services/media-services/content-protection/), die Folgendes abdecken:
- Microsoft PlayReady
- Google Widevine
- Apple FairPlay
- AES-128-Verschlüsselung

Die DRM/AES-Verschlüsselung des Inhalts erfolgt auf Anforderung für verschiedene Streamingprotokolle dynamisch. Bereitstellungsdienste für DRM-Lizenzen/AES-Entschlüsselungsschlüssel werden auch von Azure Media Services bereitgestellt.

Neben dem Schutz von Inhalten für das Onlinestreaming über verschiedene Streamingprotokolle ist auch der Offlinemodus für geschützte Inhalte ein häufig gefordertes Feature. Für die folgenden Szenarien ist die Unterstützung des Offlinemodus erforderlich:
1. Wiedergabe, wenn keine Internetverbindung verfügbar ist, z. B. auf Reisen.
2. Einige Inhaltsanbieter verbieten möglicherweise die Bereitstellung von DRM-Lizenzen außerhalb bestimmter Landesgrenzen. Wenn ein Benutzer Inhalte ansehen möchte, während er ins Ausland reist, ist ein Offline-Download erforderlich.
3. In einigen Ländern ist die Verfügbarkeit und/oder Bandbreite des Internets noch begrenzt. Benutzer können sich für den vorherigen Download entscheiden, um Inhalte in ausreichender Auflösung für ein zufriedenstellendes Anzeigeerlebnis ansehen zu können. In diesem Fall geht es häufig nicht um die Verfügbarkeit des Netzwerks, sondern um die begrenzte Bandbreite des Netzwerks. OTT/OVP-Anbieter fordern eine Unterstützung des Offlinemodus.

Dieser Artikel befasst sich mit der Unterstützung von FairPlay Streaming (FPS) im Offlinemodus für Zielgeräte mit iOS 10 oder höher. Dieses Feature wird für andere Apple-Plattformen wie watchOS, tvOS oder Safari unter macOS nicht unterstützt.

## <a name="preliminary-steps"></a>Vorbereitende Schritte
Bevor Sie Offline-DRM für FairPlay auf einem iOS 10+-Gerät implementieren, sollten Sie zunächst Folgendes durchführen:
1. Machen Sie sich mit dem Schutz von Onlineinhalten für FairPlay vertraut. Dies wird in den folgenden Artikeln/Beispielen ausführlich behandelt:
- [Allgemeine Verfügbarkeit von Apple FairPlay Streaming für Azure Media Services](https://azure.microsoft.com/blog/apple-FairPlay-streaming-for-azure-media-services-generally-available/)
- [Schützen von HLS-Inhalten mit Apple FairPlay oder Microsoft PlayReady](https://docs.microsoft.com/azure/media-services/media-services-protect-hls-with-FairPlay)
- [Beispiel für FPS Streaming im Onlinemodus](https://azure.microsoft.com/resources/samples/media-services-dotnet-dynamic-encryption-with-FairPlay/)
2. Rufen Sie das FPS SDK über das Apple Developer Network ab. Das FPS SDK enthält zwei Komponenten:
- FPS Server SDK, das KSM (Key Security Module), Clientbeispiele, eine Spezifikation und eine Reihe von Testvektoren enthält.
- FPS Deployment Pack, das die D-Funktion, die Spezifikation sowie Anweisungen zur Generierung des FPS-Zertifikats, des kundenspezifischen privaten Schlüssels und des geheimen Anwendungsschlüssels enthält. Apple gibt das FPS Deployment Pack nur an lizenzierte Inhaltsanbieter aus.

## <a name="configuration-in-azure-media-services"></a>Konfiguration in Azure Media Services
Für die Konfiguration des FPS-Offlinemodus über das [Azure Media Services .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices) müssen Sie das Azure Media Services .NET SDK, Version  4.0.0.0.4 oder höher, verwenden, das die erforderliche API zur Konfiguration des FPS-Offlinemodus bereitstellte.
Wie in den obigen Annahmen angedeutet, gehen wir davon aus, dass Sie über den funktionierenden Code zur Konfiguration des Onlinemodus für FPS Content Protection verfügen. Sobald Sie über den Code für die Konfiguration von Content Protection im Onlinemodus für FPS verfügen, müssen Sie nur noch die folgenden beiden Änderungen vornehmen.

## <a name="code-change-in-fairplay-configuration"></a>Codeänderung für die FairPlay-Konfiguration
Lassen Sie uns einen booleschen Wert für „Offlinemodus aktiviert“ namens „objDRMSettings.EnableOfflineMode“ definieren, der „true“ ist, wenn das DRM-Offlineszenario aktiviert ist. In Abhängigkeit von diesem Indikator nehmen wir die folgende Änderung an der FairPlay-Konfiguration vor:

```csharp
if (objDRMSettings.EnableOfflineMode)
    {
        FairPlayConfiguration = Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
            objX509Certificate2,
            pfxPassword,
            pfxPasswordId,
            askId,
            iv, 
            RentalAndLeaseKeyType.PersistentUnlimited,
            0x9999);
    }
    else
    {
        FairPlayConfiguration = Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
            objX509Certificate2,
            pfxPassword,
            pfxPasswordId,
            askId,
            iv);
    }
```

## <a name="code-change-in-asset-delivery-policy-configuration"></a>Codeänderung für die Konfiguration der Übermittlungsrichtlinie für Objekte
Die zweite Änderung besteht darin, den dritten Schlüssel zum Wörterbuch „Dictionary<AssetDeliveryPolicyConfigurationKey, string>“ hinzuzufügen.
Der dritte „AssetDeliveryPolicyConfigurationKey“ muss wie folgt hinzugefügt werden: 
```csharp
// FPS offline mode
    if (drmSettings.EnableOfflineMode)
    {
        objDictionary_AssetDeliveryPolicyConfigurationKey.Add(AssetDeliveryPolicyConfigurationKey.AllowPersistentLicense, "true");
        Console.WriteLine("FPS OFFLINE MODE: AssetDeliveryPolicyConfigurationKey.AllowPersistentLicense added into asset delivery policy configuration.");
    }

    // for IAssetDelivery for FPS
    IAssetDeliveryPolicy objIAssetDeliveryPolicy = objCloudMediaContext.AssetDeliveryPolicies.Create(
            drmSettings.AssetDeliveryPolicyName,
            AssetDeliveryPolicyType.DynamicCommonEncryptionCbcs,
            AssetDeliveryProtocol.HLS,
            objDictionary_AssetDeliveryPolicyConfigurationKey);
```

Nach diesem Schritt enthält das „Dictionary<AssetDeliveryPolicyConfigurationKey, string>“ in der FPS-Übermittlungrichtlinie für Objekte die folgenden drei Einträge:
1. „AssetDeliveryPolicyConfigurationKey.FairPlayBaseLicenseAcquisitionUrl“ oder „AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl“, je nach Faktoren wie dem FPS KSM-/Schlüsselserver und der Frage, ob dieselbe Übermittlungsrichtlinie für Objekte über mehrere Objekte hinweg wiederverwendet werden soll.
2. AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs
3. AssetDeliveryPolicyConfigurationKey.AllowPersistentLicense

Jetzt ist Ihr Media Services-Konto für die Übermittlung von FairPlay-Lizenzen im Offlinemodus konfiguriert.

## <a name="sample-ios-player"></a>Beispiel für iOS-Player
Zunächst ist zu beachten, dass die Unterstützung des FPS-Offlinemodus nur unter iOS 10 und höher verfügbar ist. Wir sollten das FPS Server SDK (Version 3.0 oder höher) erhalten, das das Dokument und Beispiel für den FPS-Offlinemodus enthält. Insbesondere enthält das FPS Server SDK (Version 3.0 oder höher) die folgenden zwei Elemente, die sich auf den Offlinemodus beziehen:
1. Dokument: Offlinewiedergabe mit FairPlay Streaming und HTTP Live Streaming. Apple, 14.09.2016. Im FPS Server SDK, Version 4.0, wurde dieses Dokument in das FPS-Streamingdokument integriert.
2. Beispielcode: HLSCatalog-Beispiel für den FPS-Offlinemodus in „\FairPlay Streaming Server SDK v3.1\Development\Client\HLSCatalog_With_FPS\HLSCatalog\“. In der HLSCatalog-Beispiel-App sind die folgenden Codedateien speziell für die Implementierung von Features des Offlinemodus vorgesehen:
- Die Codedatei „AssetPersistenceManager.swift“: „AssetPersistenceManager“ ist die Hauptklasse in diesem Beispiel, die Folgendes veranschaulicht:
    - Verwaltung des Downloads von HLS-Datenströmen, z. B. APIs zum Starten und Abbrechen des Downloads oder Löschen vorhandener Objekte vom Gerät des Benutzers
    - Überwachung des Downloadstatus
- Die Codedateien „AssetListTableViewController.swift“ und „AssetListTableViewCell.swift“: „AssetListTableViewController“ ist die Hauptschnittstelle in diesem Beispiel. Sie enthält eine Liste der Objekte, die das Beispiel wiedergeben, herunterladen oder löschen bzw. mit ihnen einen Download abbrechen kann. 

Nachfolgend finden Sie die detaillierten Schritte zum Einrichten eines funktionierenden iOS-Players. Nehmen wir an, Sie beginnen mit dem HLSCatalog-Beispiel im FPS Server SDK, Version 4.0.1.  Folgende Codeänderungen müssen vorgenommen werden:

Implementieren Sie die Methode `requestContentKeyFromKeySecurityModule(spcData: Data, assetID: String)` in „HLSCatalog\Shared\Managers\ContentKeyDelegate.swift“ unter Verwendung des folgenden Codes: Lassen Sie „drmUr“ eine Variable sein, die der URL für das HLS-Streaming zugewiesen ist.

```swift
    var ckcData: Data? = nil
    
    let semaphore = DispatchSemaphore(value: 0)
    let postString = "spc=\(spcData.base64EncodedString())&assetId=\(assetIDString)"
    
    if let postData = postString.data(using: .ascii, allowLossyConversion: true), let drmServerUrl = URL(string: self.drmUrl) {
        var request = URLRequest(url: drmServerUrl)
        request.httpMethod = "POST"
        request.setValue(String(postData.count), forHTTPHeaderField: "Content-Length")
        request.setValue("application/x-www-form-urlencoded", forHTTPHeaderField: "Content-Type")
        request.httpBody = postData
        
        URLSession.shared.dataTask(with: request) { (data, _, error) in
            if let data = data, var responseString = String(data: data, encoding: .utf8) {
                responseString = responseString.replacingOccurrences(of: "<ckc>", with: "").replacingOccurrences(of: "</ckc>", with: "")
                ckcData = Data(base64Encoded: responseString)
            } else {
                print("Error encountered while fetching FairPlay license for URL: \(self.drmUrl), \(error?.localizedDescription ?? "Unknown error")")
            }
            
            semaphore.signal()
            }.resume()
    } else {
        fatalError("Invalid post data")
    }
    
    semaphore.wait()
    return ckcData
```

Implementieren Sie in „HLSCatalog\Shared\Managers\ContentKeyDelegate.swift“ die Methode `requestApplicationCertificate()`. Diese Implementierung hängt davon ab, ob Sie das Zertifikat (nur öffentlicher Schlüssel) mit dem Gerät einbetten oder das Zertifikat im Web hosten. Nachfolgend finden Sie eine Implementierung unter Verwendung des gehosteten Anwendungszertifikats, das in unseren Testbeispielen verwendet wird. Verwenden Sie für „certUrl“ eine Variable, die die URL des Anwendungszertifikats enthält.

```swift
func requestApplicationCertificate() throws -> Data {

        var applicationCertificate: Data? = nil
        do {
            applicationCertificate = try Data(contentsOf: URL(string: certUrl)!)
        } catch {
            print("Error loading FairPlay application certificate: \(error)")
        }
        
        return applicationCertificate
    }
```

Für den abschließenden integrierten Test werden sowohl die Video-URL als auch die URL des Anwendungszertifikats im Abschnitt „Integrierter Test“ zur Verfügung gestellt.

Fügen Sie in „HLSCatalog\Shared\Resources\Streams.plist“ Ihre Testvideo-URL hinzu. Für die ID des symmetrischen Schlüssels können wir einfach die URL für den FairPlay-Lizenzerwerb mit dem skd-Protokoll als eindeutigen Wert verwenden.

![Datenströme von FairPlay iOS-Apps im Offlinemodus](media/media-services-protect-hls-with-offline-FairPlay/media-services-offline-FairPlay-ios-app-streams.png)

Für die Testvideo-URL, die URL für den FairPlay-Lizenzerwerb und die Anwendungszertifikat-URL können Sie eigene URLs verwenden, wenn Sie diese eingerichtet haben. Sie können aber auch mit dem nächsten Abschnitt fortfahren, der Testbeispiele enthält.

## <a name="integrated-test"></a>Integrierter Test
In Azure Media Services wurden drei Testbeispiele eingerichtet, die die folgenden drei Szenarien abdecken:
1.  FPS-geschützt, mit Video, Audio und alternativen Audiotitel
2.  FPS-geschützt, mit Video, Audio, aber ohne alternativen Audiotitel
3.  FPS-geschützt, nur mit Video, ohne Audio

Diese Beispiele sind auf dieser [Demoseite](http://aka.ms/poc#22) zu finden, wobei das entsprechende Anwendungszertifikat in einer Azure-Web-App gehostet wird.
Wir haben festgestellt, dass nur Audio wiedergegeben wird, wenn ein Beispiel der Version 3 oder 4 des FPS Server SDK verwendet wird und die Hauptwiedergabeliste alternative Audiotitel enthält. Aus diesem Grund müssen wir alternative Audiotitel entfernen. Von den drei obigen Beispielen funktionieren somit (2) und (3) sowohl im Online- als auch im Offlinemodus. Dafür gibt (1) Audio nur im Offlinemodus wieder, während das Onlinestreaming gut funktioniert.

## <a name="faq"></a>Häufig gestellte Fragen
Einige häufig gestellte Fragen zur Problembehandlung:
- **Warum wird im Offlinemodus nur Audio, aber kein Video wiedergegeben?** Dieses Verhalten scheint durch den Entwurf der Beispielanwendung bedingt zu sein. Wenn ein alternativer Audiotitel vorhanden ist (was bei HLS der Fall ist), werden sowohl iOS 10 als auch iOS 11 im Offlinemodus standardmäßig den alternativen Audiotitel verwenden. Um dieses Verhalten für den FPS-Offlinemodus zu kompensieren, müssen wir den alternativen Audiotitel aus dem Datenstrom entfernen. Um dies auf der Seite von Azure Media Services zu erreichen, können wir einfach den dynamischen Manifestfilter „audio-only=false“ hinzufügen. Eine HLS-URL würde somit mit „.ism/manifest(format=m3u8-aapl,audio-only=false)“ enden. 
- **Warum wird im Offlinemodus nach dem Hinzufügen von „audio-only=false“ weiterhin nur Audio ohne Video abgespielt?** In Abhängigkeit vom Design des CDN-Cacheschlüssels kann der Inhalt möglicherweise zwischengespeichert werden. Sie müssen den Cacheinhalt löschen.
- **Wird der FPS-Offlinemodus zusätzlich zu iOS 10 auch unter iOS 11 unterstützt?** Ja, der FPS-Offlinemodus wird sowohl für iOS 10 als auch für iOS 11 unterstützt.
- **Warum kann ich das Dokument zur Offlinewiedergabe mit FairPlay Streaming und HTTP Live Streaming nicht im FPS Server SDK finden?** Seit FPS Server SDK Version 4 wurde dieses Dokument in das Dokument für den FairPlay Streaming-Programmierleitfaden integriert.
- **Wofür steht der letzte Parameter in der folgenden API für den FPS-Offlinemodus?**
`Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(objX509Certificate2, pfxPassword, pfxPasswordId, askId, iv, RentalAndLeaseKeyType.PersistentUnlimited, 0x9999);`

Die Dokumentation für diese API finden Sie [hier](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.mediaservices.client.FairPlay.FairPlayconfiguration.createserializedFairPlayoptionconfiguration?view=azure-dotnet). Der Parameter repräsentiert die Dauer der Offlinemiete mit der Einheit „Stunde“.
- **Wie sieht die Struktur der heruntergeladenen bzw. Offlinedateien auf iOS-Geräten aus?** Die heruntergeladene Dateistruktur auf einem iOS-Gerät sieht wie folgt aus (Screenshot). Der Ordner `_keys` speichert heruntergeladene FPS-Lizenzen, d. h. eine Speicherdatei für jeden Lizenzdiensthost. Der Ordner `.movpkg` speichert Audio- und Videoinhalte. Der erste Ordner mit dem Namen, der mit einem Bindestrich endet, auf den eine Zahl folgt, enthält Videoinhalte. Der numerische Wert ist die „Spitzenbandbreite“ (PeakBandwidth) der Videowiedergabe. Der zweite Ordner mit dem Namen, der mit einem Bindestrich endet, auf den „0“ folgt, enthält Audioinhalte. Der dritte Ordner namens „Data“ enthält die Hauptwiedergabeliste des FPS-Inhalts. Die Datei „boot.xml“ bietet eine vollständige Beschreibung des Inhalts des Ordners `.movpkg` (nachfolgend finden Sie eine Beispieldatei für „boot.xml“).

![Dateistruktur der iOS-Beispielanwendung für FairPlay im Offlinemodus](media/media-services-protect-hls-with-offline-FairPlay/media-services-offline-FairPlay-file-structure.png)

Beispieldatei für „boot.xml“:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<HLSMoviePackage xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://apple.com/IMG/Schemas/HLSMoviePackage" xsi:schemaLocation="http://apple.com/IMG/Schemas/HLSMoviePackage /System/Library/Schemas/HLSMoviePackage.xsd">
  <Version>1.0</Version>
  <HLSMoviePackageType>PersistedStore</HLSMoviePackageType>
  <Streams>
    <Stream ID="1-4DTFY3A3VDRCNZ53YZ3RJ2NPG2AJHNBD-0" Path="1-4DTFY3A3VDRCNZ53YZ3RJ2NPG2AJHNBD-0" NetworkURL="https://willzhanmswest.streaming.mediaservices.windows.net/e7c76dbb-8e38-44b3-be8c-5c78890c4bb4/MicrosoftElite01.ism/QualityLevels(127000)/Manifest(aac_eng_2_127,format=m3u8-aapl)">
      <Complete>YES</Complete>
    </Stream>
    <Stream ID="0-HC6H5GWC5IU62P4VHE7NWNGO2SZGPKUJ-310656" Path="0-HC6H5GWC5IU62P4VHE7NWNGO2SZGPKUJ-310656" NetworkURL="https://willzhanmswest.streaming.mediaservices.windows.net/e7c76dbb-8e38-44b3-be8c-5c78890c4bb4/MicrosoftElite01.ism/QualityLevels(161000)/Manifest(video,format=m3u8-aapl)">
      <Complete>YES</Complete>
    </Stream>
  </Streams>
  <MasterPlaylist>
    <NetworkURL>https://willzhanmswest.streaming.mediaservices.windows.net/e7c76dbb-8e38-44b3-be8c-5c78890c4bb4/MicrosoftElite01.ism/manifest(format=m3u8-aapl,audio-only=false)</NetworkURL>
  </MasterPlaylist>
  <DataItems Directory="Data">
    <DataItem>
      <ID>CB50F631-8227-477A-BCEC-365BBF12BCC0</ID>
      <Category>Playlist</Category>
      <Name>master.m3u8</Name>
      <DataPath>Playlist-master.m3u8-CB50F631-8227-477A-BCEC-365BBF12BCC0.data</DataPath>
      <Role>Master</Role>
    </DataItem>
  </DataItems>
</HLSMoviePackage>
```

## <a name="summary"></a>Zusammenfassung
In diesem Dokument finden Sie die detaillierten Schritte und Informationen zur Implementierung des FPS-Offlinemodus, einschließlich:
1. Azure Media Services Content Protection-Konfiguration über AMS NET API. Hiermit wird die dynamische FairPlay-Verschlüsselung und die FairPlay-Lizenzübermittlung in AMS konfiguriert.
2. Auf dem Beispiel des Apple FPS Server SDK basierter iOS-Player. Dadurch würde ein iOS-Player eingerichtet werden, der FPS-Inhalte entweder im Onlinestreamingmodus oder im Offlinemodus wiedergeben kann.
3. Beispielvideos für FPS zum Testen von Offlinemodus und Onlinestreaming.
4. Häufig gestellte Fragen zum FPS-Offlinemodus.
