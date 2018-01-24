---
title: Verwenden von Azure CDN mit SAS | Microsoft-Dokumentation
description: 
services: cdn
documentationcenter: 
author: dksimpson
manager: 
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2017
ms.author: rli v-deasim
ms.openlocfilehash: de30f4319be75362131f8c8ad71aad57b0528f05
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2018
---
# <a name="using-azure-cdn-with-sas"></a>Verwenden von Azure CDN mit SAS

Wenn Sie Inhalte aus dem Speichercontainer Ihres Speicherkontos bereitstellen, sollten Sie den Benutzerzugriff auf Ihre Dateien schützen, indem Sie privaten Zugriff auf den Speichercontainer gewähren. Andernfalls ist ein Speichercontainer für alle Personen zugänglich, denen öffentlicher Zugriff erteilt wurde, und denen die zugehörige URL bekannt ist. Um ein Speicherkonto zu schützen, für das Sie CDN (Content Delivery Network) Zugriff gewährt haben, können Sie mithilfe der SAS-Funktion (Shared Access Signature) von Azure Storage den Zugriff auf private Speichercontainer einschränken.

Eine SAS ist ein URI, der eingeschränkte Zugriffsrechte auf Ihre Azure Storage-Ressourcen erteilt, ohne Ihren Kontoschlüssel verfügbar zu machen. Sie können eine SAS für Clients bereitstellen, denen Sie nicht Ihren Speicherkontoschlüssel anvertrauen möchten, aber denen Sie Zugriff auf bestimmte Speicherkontoressourcen gewähren möchten. Durch das Bereitstellen eines SAS-URI für diese Clients wird ihnen für einen bestimmten Zeitraum Zugriff auf eine Ressource gewährt.
 
SAS ermöglicht das Definieren verschiedener Parameter für den Zugriff auf ein Blob, z.B. Start- und Ablaufzeiten, Berechtigungen (Lesen/Schreiben) und IP-Bereiche. In diesem Artikel wird beschrieben, wie Sie SAS in Kombination mit Azure CDN verwenden. Weitere Informationen zu SAS – einschließlich Erstellung und Parameteroptionen – finden Sie unter [Verwenden von Shared Access Signatures (SAS)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).

## <a name="setting-up-azure-cdn-to-work-with-storage-sas"></a>Einrichten von Azure CDN zur Verwendung mit Speicher-SAS
Für die Verwendung von SAS mit Azure CDN werden die folgenden drei Optionen empfohlen. Bei allen Optionen wird vorausgesetzt, dass Sie bereits eine funktionierende SAS erstellt haben (siehe Voraussetzungen). 
 
### <a name="prerequisites"></a>Voraussetzungen
Erstellen Sie zunächst ein Speicherkonto, und generieren Sie dann eine SAS für Ihr Asset. Sie können zwei Arten von SAS erstellen: eine Dienst-SAS oder eine Konto-SAS. Weitere Informationen finden Sie unter [Arten von Shared Access Signatures](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#types-of-shared-access-signatures).

Nachdem Sie eine SAS generiert haben, können Sie auf Ihre Blob Storage-Datei mithilfe einer URL zugreifen, die das folgende Format aufweist: `https://<account>.blob.core.windows.net/<folder>/<file>?sv=<SAS_TOKEN>`
 
Beispiel: 
 ```
https://democdnstorage1.blob.core.windows.net/container1/sasblob.txt?sv=2017-04-17&ss=b&srt=co&sp=r&se=2038-01-02T21:30:49Z&st=2018-01-02T13:30:49Z&spr=https&sig=QehoetQFWUEd1lhU5iOMGrHBmE727xYAbKJl5ohSiWI%3D
```

Weitere Informationen zum Festlegen von Parametern finden Sie unter [Überlegungen zu SAS-Parametern](#sas-parameter-considerations) und [Shared Access Signature-Parameter](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#shared-access-signature-parameters).

![CDN-SAS-Einstellungen](./media/cdn-sas-storage-support/cdn-sas-settings.png)

### <a name="option-1-using-sas-with-pass-through-to-blob-storage-from-the-cdn"></a>Option 1: Verwenden von SAS mit Passthrough zum Blob Storage aus dem CDN

Diese Option ist die einfachste und verwendet nur ein einziges SAS-Token, das vom CDN an den Ursprungsserver übergeben wird. Diese Option wird unterstützt durch **Azure CDN von Verizon** (Standard- und Premium-Profile) und durch **Azure CDN von Akamai**. 
 
1. Wählen Sie einen Endpunkt aus, klicken Sie auf **Cacheregeln**, und wählen Sie dann in der Liste  **Zwischenspeichern von Abfragezeichenfolgen** die Einstellung **Jede eindeutige URL zwischenspeichern** aus.

    ![CDN-Cacheregeln](./media/cdn-sas-storage-support/cdn-caching-rules.png)

2. Nachdem Sie SAS für Ihr Speicherkonto eingerichtet haben, verwenden Sie das SAS-Token mit der CDN-URL zum Zugriff auf die Datei. 
   
   Die Ergebnis-URL weist das folgende Format auf: `https://<endpoint>.azureedge.net/<folder>/<file>?sv=<SAS_TOKEN>`

   Beispiel:    
   ```
   https://demoendpoint.azureedge.net/test/demo.jpg/?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
   
3. Optimieren Sie die Dauer der Zwischenspeicherung entweder mithilfe von Cacheregeln oder durch das Hinzufügen von `Cache-Control`-Headern am Ursprung. Da das CDN das SAS-Token als einfache Abfragezeichenfolge betrachtet, sollten Sie als Best Practice eine Cachedauer festlegen, die zum Zeitpunkt oder vor dem SAS-Ablauf endet. Wenn eine Datei länger zwischengespeichert wird, als die SAS aktiv ist, ist die Datei vom CDN-Ursprungsserver aus möglicherweise auch nach dem SAS-Ablauf weiter zugänglich. Wenn dieser Fall eintritt und Sie den Zugriff auf die Datei unterbinden möchten, müssen Sie eine Bereinigung für die Datei durchführen, um sie aus dem Cache zu löschen. Informationen zum Festlegen der Cachedauer im CDN finden Sie unter [Steuern des Verhaltens beim Zwischenspeichern im Azure Content Delivery Network mit Cacheregeln](cdn-caching-rules.md).

### <a name="option-2-hidden-cdn-security-token-using-rewrite-rule"></a>Option 2: Ausgeblendetes CDN-Sicherheitstoken mit Verwendung einer Rewriteregel
 
Bei dieser Option können Sie den ursprünglichen Blob Storage ohne ein SAS-Token für den CDN-Benutzer schützen. Diese Option kann nützlich sein, wenn Sie keine besonderen Zugriffsbeschränkungen für die Datei benötigen, aber Benutzer am direkten Zugriff auf den Speicherursprung hindern möchten, um die CDN-Auslagerungszeiten zu verbessern. Diese Option ist nur für **Azure CDN Premium von Verizon**-Profile verfügbar. 
 
1. Verwenden Sie die [Regel-Engine](cdn-rules-engine.md), um eine URL-Rewriteregel zu erstellen. Die Verteilung neuer Regeln dauert etwa 90 Minuten.

   ![Schaltfläche zur CDN-Verwaltung](./media/cdn-sas-storage-support/cdn-manage-btn.png)

   ![Schaltfläche für CDN-Regel-Engine](./media/cdn-sas-storage-support/cdn-rules-engine-btn.png)

   Diese beispielhafte URL-Rewriteregel weist das folgende Muster auf:
   
   Quelle:   
   `/test/demo.jpg`
   
   Ziel:   
   `/test/demo.jpg?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D`

   ![CDN-URL-Rewriteregel](./media/cdn-sas-storage-support/cdn-url-rewrite-rule.png)
 
2. Greifen Sie im folgenden Format ohne SAS-Token auf die Datei in Ihrem CDN zu: `https://<endpoint>.azureedge.net/<folder>/<file>`
 
   Beispiel:    
   `https://demoendpoint.azureedge.net/test/demo.jpg`
       
   Beachten Sie, dass beliebige Benutzer – unabhängig von der Verwendung eines SAS-Tokens – auf einen CDN-Endpunkt zugreifen können. 

3. Optimieren Sie die Dauer der Zwischenspeicherung entweder mithilfe von Cacheregeln oder durch das Hinzufügen von `Cache-Control`-Headern am Ursprung. Da das CDN das SAS-Token als einfache Abfragezeichenfolge betrachtet, sollten Sie als Best Practice eine Cachedauer festlegen, die zum Zeitpunkt oder vor dem SAS-Ablauf endet. Wenn eine Datei länger zwischengespeichert wird, als die SAS aktiv ist, ist die Datei vom CDN-Ursprungsserver aus möglicherweise auch nach dem SAS-Ablauf weiter zugänglich. Wenn dieser Fall eintritt und Sie den Zugriff auf die Datei unterbinden möchten, müssen Sie eine Bereinigung für die Datei durchführen, um sie aus dem Cache zu löschen. Informationen zum Festlegen der Cachedauer im CDN finden Sie unter [Steuern des Verhaltens beim Zwischenspeichern im Azure Content Delivery Network mit Cacheregeln](cdn-caching-rules.md).

### <a name="option-3-using-cdn-security-token-authentication-with-a-rewrite-rule"></a>Option 3: Verwenden der CDN-Sicherheitstokenauthentifizierung mit einer Rewriteregel

Diese Option bietet die höchste Sicherheit und die meisten Anpassungsmöglichkeiten. Um die CDN-Sicherheitstokenauthentifizierung zu verwenden, müssen Sie über ein **Azure CDN Premium von Verizon**-Profil verfügen. Der Clientzugriff basiert auf den Sicherheitsparametern, die für das CDN-Sicherheitstoken festgelegt wurden. Wenn die SAS jedoch nicht mehr gültig ist, kann das CDN den Inhalt vom Ursprungsserver nicht erneut validieren.

1. [Erstellen Sie ein CDN-Sicherheitstoken](https://docs.microsoft.com/azure/cdn/cdn-token-auth#setting-up-token-authentication), und aktivieren Sie es mithilfe der Regel-Engine für den CDN-Endpunkt und -Pfad, über den Ihre Benutzer auf die Datei zugreifen können.

   Eine SAS-URL weist das folgende Format auf:   
   `https://<endpoint>.azureedge.net/<folder>/<file>?sv=<SAS_TOKEN>`
 
   Beispiel:    
   ```
   https://demoendpoint.azureedge.net/test/demo.jpg?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
       
   Die Parameteroptionen für eine CDN-Sicherheitstokenauthentifizierung unterscheiden sich von den Parameteroptionen für ein SAS-Token. Wenn Sie beim Erstellen eines CDN-Sicherheitstokens eine Ablaufzeit festlegen, verwenden Sie denselben Ablaufzeitpunkt wie für das SAS-Token. So ist sichergestellt, dass der Ablaufzeitpunkt vorhersehbar ist. 
 
2. Verwenden Sie die [Regel-Engine](cdn-rules-engine.md), um eine URL-Rewriteregel zu erstellen, mit der der Tokenzugriff auf alle Blobs im Container aktiviert wird. Die Verteilung neuer Regeln dauert etwa 90 Minuten.

   Diese beispielhafte URL-Rewriteregel weist das folgende Muster auf:
   
   Quelle:   
   `/test/demo.jpg`
   
   Ziel:   
   `/test/demo.jpg?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D`

   ![CDN-URL-Rewriteregel](./media/cdn-sas-storage-support/cdn-url-rewrite-rule.png)

3. Wenn Sie die SAS erneuern, aktualisieren Sie die URL-Rewriteregel zur Verwendung des neuen SAS-Tokens. 

## <a name="sas-parameter-considerations"></a>Überlegungen zu SAS-Parametern

SAS-Parameter sind für das CDN nicht sichtbar, deshalb kann das CDN sein Übermittlungsverhalten nicht basierend auf den Parametern ändern. Die definierten Parametereinschränkungen gelten nur für Anforderungen, die das CDN an den Ursprungsserver sendet, nicht für Anforderungen vom Client an das CDN. Diese Unterscheidung muss beim Festlegen der SAS-Parameter berücksichtigt werden. Wenn diese erweiterten Funktionen benötigt werden und Sie [Option 3](#option-3-using-cdn-security-token-authentication-with-a-rewrite-rule) verwenden, legen Sie die geeigneten Einschränkungen für das CDN-Sicherheitstoken fest.

| Name des SAS-Parameters | BESCHREIBUNG |
| --- | --- |
| Start | Zeitpunkt, ab dem das CDN auf die Blobdatei zugreifen kann. Wählen Sie aufgrund von Uhrzeitabweichungen (unterschiedlicher Signaleingang für verschiedene Komponenten) einen 15 Minuten früheren Zeitpunkt, wenn Sie möchten, dass das Asset sofort verfügbar ist. |
| Ende | Der Zeitpunkt, ab dem das CDN nicht mehr auf die Blobdatei zugreifen kann. Zuvor zwischengespeicherte Dateien im CDN sind weiterhin zugänglich. Um die Dateiablaufzeit zu steuern, legen Sie entweder die geeignete Ablaufzeit im CDN-Sicherheitstoken fest, oder bereinigen Sie das Asset. |
| Zulässige IP-Adressen | Optional. Wenn Sie **Azure CDN von Verizon** verwenden, können Sie diesen Parameter auf die Bereiche festlegen, die in [Edgeserver-IP-Adressen für Azure CDN von Verizon](https://msdn.microsoft.com/library/mt757330.aspx) definiert sind. Wenn Sie **Azure CDN von Akamai** verwenden, können Sie den Parameter für IP-Bereiche nicht festlegen, weil die IP-Adressen nicht statisch sind.|
| Zulässige Protokolle | Die zugelassenen Protokolle für eine Anforderung mit dem Konto-SAS. Als Einstellung wird HTTPS empfohlen.|

## <a name="see-also"></a>Weitere Informationen
- [Verwenden von Shared Access Signatures (SAS)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)
- [Shared Access Signatures, Teil 2: Erstellen und Verwenden einer SAS mit Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2)
- [Schützen von Azure Content Delivery Network-Assets mit Tokenauthentifizierung](https://docs.microsoft.com/azure/cdn/cdn-token-auth)
