---
title: Includedatei
description: Includedatei
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 04/13/2018
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: acb9bdf294dd66005df203f957c155540b658698
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
ms.locfileid: "33830101"
---
## <a name="access-the-media-services-api"></a>Zugreifen auf die Media Services-API

Verwenden Sie die Authentifizierung per Azure AD-Dienstprinzipal, um eine Verbindung mit den Azure Media Services-APIs herzustellen. Der folgende Befehl erstellt eine Azure AD-Anwendung und fügt einen Dienstprinzipal an das Konto an. Sie verwenden die zurückgegebenen Werte zum Konfigurieren Ihrer .NET-App. Dies wird im folgenden Schritt gezeigt.

Vor der Ausführung des Skripts können Sie `amsaccount` und `amsResourceGroup` durch die Namen ersetzen, die Sie beim Erstellen dieser Ressourcen ausgewählt haben. `amsaccount` ist der Name des Azure Media Services-Kontos, an das der Dienstprinzipal angefügt wird. <br/>Der nachfolgende Befehl verwendet die Option `xml`. Mit dieser Option wird eine XML-Datei zurückgegeben, die sie in die Datei „app.config“ einfügen können. Wenn Sie die Option `xml` weglassen, wird die Antwort im `json`-Format zurückgegeben.

```azurecli-interactive
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup --xml
```

Dieser Befehl erzeugt etwa folgende Antwort:

```xml
<add key="Region" value="West US 2" />
<add key="ResourceGroup" value="amsResourceGroup" />
<add key="AadEndpoint" value="https://login.microsoftonline.com" />
<add key="AccountName" value="amsaccount" />
<add key="SubscriptionId" value="111111111-0000-2222-3333-55555555555" />
<add key="ArmAadAudience" value="https://management.core.windows.net/" />
<add key="AadTenantId" value="2222222222-0000-2222-3333-6666666666666" />
<add key="AadSecret" value="33333333-0000-2222-3333-55555555555" />
<add key="AadClientId" value="44444444-0000-2222-3333-55555555555" />
<add key="ArmEndpoint" value="https://management.azure.com/" />
```

### <a name="configure-the-sample-app"></a>Konfigurieren der Beispiel-App

Sie müssen die richtigen Zugriffswerte in „App.config“ angeben, um die App ausführen und auf die Media Services-APIs zugreifen zu können. 

1. Öffnen Sie Visual Studio.
2. Navigieren Sie zu der von Ihnen geklonten Projektmappe.
3. Erweitern Sie im Projektmappen-Explorer das Projekt *EncodeAndStreamFiles*.
4. Legen Sie dieses Projekt als Startprojekt fest.
5. Öffnen Sie „App.config“.
6. Ersetzen Sie die appSettings-Werte durch die Werte, die Sie im vorherigen Schritt abgerufen haben.

 ```xml
 <add key="Region" value="value" />
 <add key="ResourceGroup" value="value" />
 <add key="AadEndpoint" value="value" />
 <add key="AccountName" value="value" />
 <add key="SubscriptionId" value="value" />
 <add key="ArmAadAudience" value="value" />
 <add key="AadTenantId" value="value" />
 <add key="AadSecret" value="value" />
 <add key="AadClientId" value="value" />
 <add key="ArmEndpoint" value="value" />
 ```    
 
7. Drücken Sie STRG+UMSCHALT+B, um die Projektmappe zu erstellen.
