---
title: Verwenden von Azure CDN mit SAS | Microsoft-Dokumentation
description: Azure CDN unterstützt die Verwendung von Shared Access Signature (SAS), um einen eingeschränkten Zugriff auf private Speichercontainer zu gewähren.
services: cdn
documentationcenter: ''
author: dksimpson
manager: ''
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: v-deasim
ms.openlocfilehash: 44c28f45b7be8fbaa47a16d8ab07892ab146c39e
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
# <a name="using-azure-cdn-with-sas"></a>Verwenden von Azure CDN mit SAS

Wenn Sie ein Speicherkonto für Azure CDN (Content Delivery Network) zum Zwischenspeichern von Inhalten einrichten, kann standardmäßig jeder Benutzer, der die URLs für Ihre Speichercontainer kennt, auf die hochgeladenen Dateien zugreifen. Um die Dateien in Ihrem Speicherkonto zu schützen, könnten Sie den Zugriff auf Ihre Speichercontainer von öffentlich auf privat ändern. Allerdings könnte in diesem Fall niemand auf Ihre Dateien zugreifen. 

Wenn Sie einen eingeschränkten Zugriff auf private Speichercontainer gewähren möchten, können Sie die Shared Access Signature (SAS)-Funktion Ihres Azure-Speicherkontos verwenden. Eine SAS ist ein URI, der eingeschränkte Zugriffsrechte auf Ihre Azure Storage-Ressourcen erteilt, ohne Ihren Kontoschlüssel verfügbar zu machen. Sie können eine SAS für Clients bereitstellen, denen Sie nicht Ihren Speicherkontoschlüssel anvertrauen möchten, aber denen Sie Zugriff auf bestimmte Speicherkontoressourcen gewähren möchten. Durch das Bereitstellen eines SAS-URI für diese Clients wird ihnen für einen bestimmten Zeitraum Zugriff auf eine Ressource gewährt.
 
Mit SAS können Sie verschiedene Parameter für den Zugriff auf ein Blob definieren, z.B. Start- und Ablaufzeiten, Berechtigungen (Lesen/Schreiben) und IP-Bereiche. In diesem Artikel wird beschrieben, wie Sie SAS in Kombination mit Azure CDN verwenden. Weitere Informationen zu SAS – einschließlich Erstellung und Parameteroptionen – finden Sie unter [Verwenden von Shared Access Signatures (SAS)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).

## <a name="setting-up-azure-cdn-to-work-with-storage-sas"></a>Einrichten von Azure CDN zur Verwendung mit Speicher-SAS
Für die Verwendung von SAS mit Azure CDN werden die folgenden drei Optionen empfohlen. Bei allen Optionen wird vorausgesetzt, dass Sie bereits eine funktionierende SAS erstellt haben (siehe Voraussetzungen). 
 
### <a name="prerequisites"></a>Voraussetzungen
Erstellen Sie zunächst ein Speicherkonto, und generieren Sie dann eine SAS für Ihr Asset. Sie können zwei Arten von SAS erstellen: eine Dienst-SAS oder eine Konto-SAS. Weitere Informationen finden Sie unter [Arten von Shared Access Signatures](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#types-of-shared-access-signatures).

Nachdem Sie ein SAS-Token generiert haben, können Sie auf die BLOB-Speicherdatei zugreifen, indem Sie `?sv=<SAS token>` an die URL anfügen. Diese URL weist das folgende Format auf: 

`https://<account name>.blob.core.windows.net/<container>/<file>?sv=<SAS token>`
 
Beispiel: 
 ```
https://democdnstorage1.blob.core.windows.net/container1/demo.jpg?sv=2017-04-17&ss=b&srt=co&sp=r&se=2038-01-02T21:30:49Z&st=2018-01-02T13:30:49Z&spr=https&sig=QehoetQFWUEd1lhU5iOMGrHBmE727xYAbKJl5ohSiWI%3D
```

Weitere Informationen zum Festlegen von Parametern finden Sie unter [Überlegungen zu SAS-Parametern](#sas-parameter-considerations) und [Shared Access Signature-Parameter](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#shared-access-signature-parameters).

![CDN-SAS-Einstellungen](./media/cdn-sas-storage-support/cdn-sas-settings.png)

### <a name="option-1-using-sas-with-pass-through-to-blob-storage-from-azure-cdn"></a>Option 1: Verwenden von SAS mit Pass-Through zum BLOB-Speicher aus dem Azure CDN

Diese Option ist die einfachste und verwendet nur ein einziges SAS-Token, das vom Azure CDN an den Ursprungsserver übergeben wird. Unterstützt wird diese Option vom **Azure CDN von Verizon** und vom **Azure CDN von Akamai**. 
 
1. Wählen Sie einen Endpunkt aus, wählen Sie **Cacheregeln** aus, und wählen Sie dann in der Liste **Zwischenspeichern von Abfragezeichenfolgen** die Einstellung **Jede eindeutige URL zwischenspeichern** aus.

    ![CDN-Cacheregeln](./media/cdn-sas-storage-support/cdn-caching-rules.png)

2. Nachdem Sie SAS für Ihr Speicherkonto eingerichtet haben, verwenden Sie das SAS-Token mit der Azure CDN-URL zum Zugriff auf die Datei. 
   
   Die Ergebnis-URL weist das folgende Format auf: `https://<endpoint hostname>.azureedge.net/<container>/<file>?sv=<SAS token>`

   Beispiel:    
   ```
   https://demoendpoint.azureedge.net/container1/demo.jpg/?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
   
3. Optimieren Sie die Dauer der Zwischenspeicherung entweder mithilfe von Cacheregeln oder durch das Hinzufügen von `Cache-Control`-Headern am Ursprungsserver. Da das Azure CDN das SAS-Token als einfache Abfragezeichenfolge betrachtet, sollten Sie als bewährte Methode eine Cachedauer festlegen, die mit oder vor dem SAS-Ablauf endet. Wenn eine Datei länger zwischengespeichert wird, als die SAS aktiv ist, ist die Datei vom Azure CDN-Ursprungsserver möglicherweise auch nach dem SAS-Ablauf weiter zugänglich. Wenn dieser Fall eintritt und Sie den Zugriff auf die Datei unterbinden möchten, müssen Sie einen Bereinigungsvorgang für die Datei durchführen, um sie aus dem Cache zu löschen. Informationen zum Festlegen der Aufbewahrungsdauer im Cache im Azure CDN finden Sie unter [Steuern des Verhaltens beim Zwischenspeichern im Azure CDN mit Cacheregeln](cdn-caching-rules.md).

### <a name="option-2-hidden-cdn-security-token-using-a-rewrite-rule"></a>Option 2: Ausgeblendetes CDN-Sicherheitstoken mit Verwendung einer Rewriteregel
 
Bei dieser Option können Sie den ursprünglichen BLOB-Speicher schützen, ohne dass Azure CDN-Benutzer ein SAS-Token in der URL verwenden müssen. Diese Option kann nützlich sein, wenn Sie keine besonderen Zugriffsbeschränkungen für die Datei benötigen, aber Benutzer am direkten Zugriff auf den Speicherursprung hindern möchten, um die Azure CDN-Auslagerungszeiten zu verbessern. Diese Option ist nur für **Azure CDN Premium von Verizon**-Profile verfügbar. 
 
1. Verwenden Sie die [Regel-Engine](cdn-rules-engine.md), um eine URL-Rewriteregel zu erstellen. Die Verteilung neuer Regeln dauert etwa 90 Minuten.

   ![Schaltfläche zur CDN-Verwaltung](./media/cdn-sas-storage-support/cdn-manage-btn.png)

   ![Schaltfläche für CDN-Regel-Engine](./media/cdn-sas-storage-support/cdn-rules-engine-btn.png)

   Im folgenden Beispiel für eine URL-Rewriteregel wird ein reguläres Ausdrucksmuster mit einer Erfassungsgruppe und einem Endpunkt namens *Storagedemo* verwendet:
   
   Quelle:   
   `(/test/*.)`
   
   Ziel:   
   ```
   $1sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```

   ![CDN-URL-Rewriteregel](./media/cdn-sas-storage-support/cdn-url-rewrite-rule-option-2.png)

2. Wenn die neue Regel aktiv ist, können Sie ohne SAS-Token in der URL auf die Datei im Azure CDN zugreifen. Dazu wird folgendes Format verwendet: `https://<endpoint hostname>.azureedge.net/<container>/<file>`
 
   Beispiel:    
   `https://demoendpoint.azureedge.net/container1/demo.jpg`
       
   Beachten Sie, dass jetzt jeder Benutzer – unabhängig von der Verwendung eines SAS-Tokens – auf Dateien am CDN-Endpunkt zugreifen kann. 

3. Optimieren Sie die Dauer der Zwischenspeicherung entweder mithilfe von Cacheregeln oder durch das Hinzufügen von `Cache-Control`-Headern am Ursprungsserver. Da das Azure CDN das SAS-Token als einfache Abfragezeichenfolge betrachtet, sollten Sie als bewährte Methode eine Cachedauer festlegen, die mit oder vor dem SAS-Ablauf endet. Wenn eine Datei länger zwischengespeichert wird, als die SAS aktiv ist, ist die Datei vom Azure CDN-Ursprungsserver möglicherweise auch nach dem SAS-Ablauf weiter zugänglich. Wenn dieser Fall eintritt und Sie den Zugriff auf die Datei unterbinden möchten, müssen Sie einen Bereinigungsvorgang für die Datei durchführen, um sie aus dem Cache zu löschen. Informationen zum Festlegen der Aufbewahrungsdauer im Cache im Azure CDN finden Sie unter [Steuern des Verhaltens beim Zwischenspeichern im Azure CDN mit Cacheregeln](cdn-caching-rules.md).

### <a name="option-3-using-cdn-security-token-authentication-with-a-rewrite-rule"></a>Option 3: Verwenden der CDN-Sicherheitstokenauthentifizierung mit einer Rewriteregel

Diese Option bietet die höchste Sicherheit und die meisten Anpassungsmöglichkeiten. Um die Azure CDN-Sicherheitstokenauthentifizierung zu verwenden, müssen Sie über das Profil **Azure CDN Premium von Verizon** verfügen. Der Clientzugriff basiert auf den Sicherheitsparametern, die Sie für das CDN-Sicherheitstoken festlegen. Wenn die SAS später jedoch nicht mehr gültig ist, kann das Azure CDN den Inhalt vom Ursprungsserver nicht erneut validieren.

1. [Erstellen Sie ein Azure CDN-Sicherheitstoken](https://docs.microsoft.com/azure/cdn/cdn-token-auth#setting-up-token-authentication), und aktivieren Sie es mithilfe der Regel-Engine für den CDN-Endpunkt und -Pfad, über den Ihre Benutzer auf die Datei zugreifen können.

   Die URL eines Sicherheitstokens weist das folgende Format auf:   
   `https://<endpoint hostname>.azureedge.net/<container>/<file>?<security_token>`
 
   Beispiel:    
   ```
   https://demoendpoint.azureedge.net/container1/demo.jpg?a4fbc3710fd3449a7c99986bkquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
       
   Die Parameteroptionen für eine Sicherheitstokenauthentifizierung unterscheiden sich von den Parameteroptionen für ein SAS-Token. Wenn Sie beim Erstellen eines Sicherheitstokens eine Ablaufzeit festlegen, verwenden Sie denselben Ablaufzeitpunkt wie für das SAS-Token. So ist sichergestellt, dass der Ablaufzeitpunkt vorhersehbar ist. 
 
2. Verwenden Sie die [Regel-Engine](cdn-rules-engine.md), um eine URL-Rewriteregel zu erstellen, mit welcher der Tokenzugriff auf alle Blobs im Container aktiviert wird. Die Verteilung neuer Regeln dauert etwa 90 Minuten.

   Im folgenden Beispiel für eine URL-Rewriteregel wird ein reguläres Ausdrucksmuster mit einer Erfassungsgruppe und einem Endpunkt namens *Storagedemo* verwendet:
   
   Quelle:   
   `(/test/*.)`
   
   Ziel:   
   ```
   $1&sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```

   ![CDN-URL-Rewriteregel](./media/cdn-sas-storage-support/cdn-url-rewrite-rule-option-3.png)

3. Wenn Sie die SAS erneuern, aktualisieren Sie die URL-Rewriteregel mit dem neuen SAS-Token. 

## <a name="sas-parameter-considerations"></a>Überlegungen zu SAS-Parametern

Da SAS-Parameter für das Azure CDN nicht sichtbar sind, kann das Azure CDN sein Übermittlungsverhalten nicht auf Grundlage der Parameter ändern. Die definierten Parametereinschränkungen gelten nur für Anforderungen, die das Azure CDN an den Ursprungsserver sendet, nicht für Anforderungen vom Client an das Azure CDN. Diese Unterscheidung muss beim Festlegen von SAS-Parametern berücksichtigt werden. Wenn diese erweiterten Funktionen benötigt werden und Sie [Option 3](#option-3-using-cdn-security-token-authentication-with-a-rewrite-rule) verwenden, legen Sie die geeigneten Einschränkungen für das Azure CDN-Sicherheitstoken fest.

| Name des SAS-Parameters | BESCHREIBUNG |
| --- | --- |
| Start | Zeitpunkt, ab dem das Azure CDN auf die Blobdatei zugreifen kann. Wählen Sie aufgrund von Uhrabweichungen (Signaleingang für verschiedene Komponenten zu unterschiedlichen Zeiten) einen 15 Minuten früheren Zeitpunkt, wenn Sie möchten, dass das Asset sofort verfügbar ist. |
| End | Der Zeitpunkt, ab dem das Azure CDN nicht mehr auf die Blobdatei zugreifen kann. Zuvor zwischengespeicherte Dateien im Azure CDN sind weiterhin zugänglich. Um die Dateiablaufzeit zu steuern, legen Sie entweder die geeignete Ablaufzeit im Azure CDN-Sicherheitstoken fest, oder bereinigen Sie das Asset. |
| Zulässige IP-Adressen | Optional. Wenn Sie **Azure CDN von Verizon** verwenden, können Sie diesen Parameter auf die Bereiche festlegen, die in [Edgeserver-IP-Adressen für Azure CDN von Verizon](https://msdn.microsoft.com/library/mt757330.aspx) definiert sind. Wenn Sie **Azure CDN von Akamai** verwenden, können Sie den Parameter für IP-Bereiche nicht festlegen, weil die IP-Adressen nicht statisch sind.|
| Zulässige Protokolle | Die zugelassenen Protokolle für eine Anforderung mit dem Konto-SAS. Als Einstellung wird HTTPS empfohlen.|

## <a name="see-also"></a>Weitere Informationen
- [Verwenden von Shared Access Signatures (SAS)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)
- [Shared Access Signatures, Teil 2: Erstellen und Verwenden einer SAS mit Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2)
- [Schützen von Azure Content Delivery Network-Assets mit Tokenauthentifizierung](https://docs.microsoft.com/azure/cdn/cdn-token-auth)
