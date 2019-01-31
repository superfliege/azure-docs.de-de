---
title: Verwenden einer systemseitig zugewiesenen verwalteten Identität einer Linux-VM für den Zugriff auf die Azure AD Graph-API
description: In diesem Tutorial erfahren Sie, wie Sie eine systemseitig zugewiesene verwaltete Identität einer Linux-VM verwenden, um auf die Azure AD Graph-API zuzugreifen.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: daveba
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/20/2018
ms.author: priyamo
ms.openlocfilehash: ad2d20bc54a2de5a049beadac779fff47d39d9b4
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55167953"
---
# <a name="tutorial-use-a-linux-vm-system-assigned-managed-identity-to-access-azure-ad-graph-api"></a>Tutorial: Verwenden einer systemseitig zugewiesenen verwalteten Identität einer Linux-VM für den Zugriff auf die Azure AD Graph-API

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice.md)]

In diesem Tutorial erfahren Sie, wie Sie eine systemseitig zugewiesene verwaltete Identität für eine Linux-VM verwenden, um zum Abrufen ihrer Gruppenmitgliedschaften auf die Azure AD Graph-API zuzugreifen. Verwaltete Identitäten für Azure-Ressourcen werden von Azure automatisch verwaltet und ermöglichen Ihnen die Authentifizierung für Dienste, die die Azure AD-Authentifizierung unterstützen, ohne dass Sie Anmeldeinformationen in Ihren Code einfügen müssen.  

In diesem Tutorial fragen Sie die Mitgliedschaft Ihrer VM-Identität in Azure AD-Gruppen ab. Gruppeninformationen werden häufig für Autorisierungsentscheidungen verwendet. Im Hintergrund wird die verwaltete Identität Ihrer VM durch einen **Dienstprinzipal** in Azure AD dargestellt. 

> [!div class="checklist"]
> * Stellen Sie eine Verbindung mit Azure AD her.
> * Fügen Sie Ihre VM-Identität einer Gruppe in Azure AD hinzu. 
> * Gewähren Sie der Identität der VM Zugriff auf Azure AD Graph. 
> * Rufen Sie mithilfe der VM-Identität ein Zugriffstoken ab, und rufen Sie damit Azure AD Graph auf.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

- [Installieren der aktuellen Version der Azure-Befehlszeilenschnittstelle (CLI)](https://docs.microsoft.com/cli/azure/install-azure-cli)

- Um einer VM-Identität Zugriff auf Azure AD Graph gewähren zu können, muss Ihrem Konto die Rolle **Globaler Administrator** in Azure AD zugewiesen sein.

## <a name="connect-to-azure-ad"></a>Stellen Sie eine Verbindung mit Azure AD her.

Sie müssen eine Verbindung mit Azure AD herstellen, um die VM einer Gruppe zuzuweisen und ihr die Berechtigung zum Abrufen der Gruppenmitgliedschaften zu erteilen.

```cli
az login
```

## <a name="add-your-vms-identity-to-a-group-in-azure-ad"></a>Hinzufügen Ihrer VM-Identität zu einer Gruppe in Azure AD

Wenn Sie die systemseitig zugewiesene verwaltete Identität auf der Linux-VM aktivieren, wird ein Dienstprinzipal in Azure AD erstellt.  Sie müssen die VM einer Gruppe hinzufügen. Im folgenden Artikel finden Sie Anweisungen zum Hinzufügen der VM zu einer Gruppe in Azure AD:

- [Add group members](/cli/azure/ad/group/member?view=azure-cli-latest#az-ad-group-member-add) (Hinzufügen von Gruppenmitgliedern)

## <a name="grant-your-vm-access-to-the-azure-ad-graph-api"></a>Gewähren des Zugriffs auf die Azure AD Graph-API für Ihre VM

Mithilfe von verwalteten Identitäten für Azure-Ressourcen kann der Code Zugriffstoken zur Authentifizierung von Ressourcen abrufen, die die Azure AD-Authentifizierung unterstützen. Die Microsoft Azure AD Graph-API unterstützt die Azure AD-Authentifizierung. In diesem Schritt gewähren Sie dem Dienstprinzipal Ihrer VM-Identität Zugriff auf Azure AD Graph, damit die VM-Identität Gruppenmitgliedschaften abfragen kann. Dienstprinzipalen wird der Zugriff auf Microsoft Graph oder Azure AD Graph über **Anwendungsberechtigungen** gewährt. Der Anwendungsberechtigungstyp, den Sie erteilen müssen, hängt von der Entität ab, auf die Sie in Microsoft oder Azure AD Graph zugreifen möchten.

In diesem Tutorial ermöglichen Sie Ihrer VM-Identität das Abfragen von Gruppenmitgliedschaften mithilfe der Anwendungsberechtigung `Directory.Read.All`. Um diese Berechtigung zu erteilen, benötigen Sie ein Benutzerkonto, dem in Azure AD die Rolle „Globaler Administrator“ zugewiesen ist. Normalerweise erteilen Sie eine Anwendungsberechtigung, indem Sie im Azure-Portal die Registrierung der Anwendung besuchen und die Berechtigung dort hinzufügen. Für verwaltete Identitäten für Azure-Ressourcen werden jedoch keine Anwendungsobjekte in Azure AD registriert, sondern nur Dienstprinzipale. Zum Registrieren der Anwendungsberechtigung verwenden Sie das Azure AD PowerShell-Befehlszeilentool. 

Azure AD Graph:
- appId des Dienstprinzipals (wird zum Erteilen der Anwendungsberechtigung verwendet): 00000002-0000-0000-c000-000000000000
- Ressourcen-ID (wird zum Anfordern des Zugriffstokens von verwalteten Identitäten für Azure-Ressourcen verwendet): https://graph.windows.net
- Referenz zu Berechtigungsbereichen: [Azure AD Graph-Berechtigungsreferenz](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)

### <a name="grant-application-permissions-using-curl"></a>Erteilen von Anwendungsberechtigungen mit CURL

1. Rufen Sie ein Token ab, um CURL-Anforderungen auszuführen:

   ```cli
   az account get-access-token --resource "https://graph.windows.net/"
   ```

2. Sie müssen die `objectId` Ihrer VM abrufen und notieren. Sie wird in den nachfolgenden Schritten verwendet, um der VM Berechtigungen zum Lesen ihrer Gruppenmitgliedschaft zu erteilen. Ersetzen Sie `<ACCESS TOKEN>` durch das Zugriffstoken, das Sie im vorherigen Schritt abgerufen haben.

   ```bash
   curl 'https://graph.windows.net/myorganization/servicePrincipals?$filter=startswith%28displayName%2C%27myVM%27%29&api-version=1.6' -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

3. Verwenden Sie die Azure AD Graph-appID (00000002-0000-0000-c000-000000000000), rufen Sie die `objectId` für `odata.type: Microsoft.DirectoryServices.ServicePrincipal` sowie die `id` für die Anwendungsrollenberechtigung `Directory.Read.All` ab, und notieren Sie diese Werte.  Ersetzen Sie `<ACCESS TOKEN>` durch das Zugriffstoken, das Sie zuvor abgerufen haben.

   ```bash
   curl "https://graph.windows.net/myorganization/servicePrincipals?api-version=1.6&%24filter=appId%20eq%20'00000002-0000-0000-c000-000000000000'" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   Antwort:

   ```json
   "odata.metadata":"https://graph.windows.net/myorganization/$metadata#directoryObjects",
   "value":[
      {
         "odata.type":"Microsoft.DirectoryServices.ServicePrincipal",
         "objectType":"ServicePrincipal",
         "objectId":"81789304-ff96-402b-ae73-07ec0db26721",
         "deletionTimestamp":null,
         "accountEnabled":true,
         "addIns":[
         ],
         "alternativeNames":[
         ],
         "appDisplayName":"Windows Azure Active Directory",
         "appId":"00000002-0000-0000-c000-000000000000",
         "appOwnerTenantId":"f8cdef31-a31e-4b4a-93e4-5f571e91255a",
         "appRoleAssignmentRequired":false,
         "appRoles":[
            {
               "allowedMemberTypes":[
                  "Application"
               ],
               "description":"Allows the app to read data in your company or school directory, such as users, groups, and apps.",
               "displayName":"Read directory data",
               "id":"5778995a-e1bf-45b8-affa-663a9f3f4d04",
               "isEnabled":true,
               "value":"Directory.Read.All"
            },
            {
               //other appRoles values
            }
   ``` 

4. Gewähren Sie dem Dienstprinzipal der VM nun Lesezugriff auf Azure AD-Verzeichnisobjekte über die Azure AD Graph-API.  Der `id`-Wert ist der Wert für die Anwendungsrollenberechtigung `Directory.Read.All`, und die `resourceId` ist die `objectId` für den Dienstprinzipal `odata.type:Microsoft.DirectoryServices.ServicePrincipal` (die im vorherigen Schritt notierten Werte).

   ```bash
   curl "https://graph.windows.net/myorganization/servicePrincipals/<VM Object ID>/appRoleAssignments?api-version=1.6" -X POST -d '{"id":"5778995a-e1bf-45b8-affa-663a9f3f4d04","principalId":"<VM Object ID>","resourceId":"81789304-ff96-402b-ae73-07ec0db26721"}'-H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ``` 
 
## <a name="get-an-access-token-using-the-vms-identity-to-call-azure-ad-graph"></a>Abrufen eines Zugriffstokens mithilfe der VM-Identität zum Aufrufen von Azure AD Graph 

Zum Ausführen dieser Schritte benötigen Sie einen SSH-Client. Wenn Sie Windows verwenden, können Sie den SSH-Client im [Windows-Subsystem für Linux](https://msdn.microsoft.com/commandline/wsl/about) verwenden. Wenn Sie Hilfe beim Konfigurieren der SSH-Clientschlüssel benötigen, lesen Sie die Informationen unter [Vorgehensweise: Verwenden von SSH-Schlüsseln mit Windows in Azure](../../virtual-machines/linux/ssh-from-windows.md) oder [Erstellen und Verwenden eines SSH-Schlüsselpaars (öffentlich und privat) für virtuelle Linux-Computer in Azure](../../virtual-machines/linux/mac-create-ssh-keys.md).

1. Navigieren Sie im Portal zu Ihrem virtuellen Linux-Computer, und klicken Sie in der **Übersicht** auf **Verbinden**.  
2. **Verbinden** Sie den virtuellen Computer mit dem gewünschten SSH-Client. 
3. Senden Sie im Terminalfenster mit CURL eine Anforderung an den lokalen Endpunkt für verwaltete Identitäten für Azure-Ressourcen, um ein Zugriffstoken für Azure AD Graph abzurufen.  
    
   ```bash
   curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://graph.windows.net' -H Metadata:true
   ```    
   Die Antwort enthält das Zugriffstoken, das Sie für den Zugriff auf Azure AD Graph benötigen.

   Antwort:

   ```bash
   {
       "access_token":"eyJ0eXAiOiJKV...",
       "expires_in":"3599",
       "expires_on":"1519622535",
       "not_before":"1519618635",
       "resource":"https://graph.windows.net",
       "token_type":"Bearer"
   }
   ```

4. Mit der Objekt-ID des Dienstprinzipals Ihrer VM (der zuvor abgerufene Wert) können Sie die Azure AD Graph-API zum Abrufen der Gruppenmitgliedschaften abfragen. Ersetzen Sie `<OBJECT-ID>` durch die Objekt-ID des Dienstprinzipals Ihres virtuellen Computers und `<ACCESS-TOKEN>` durch das zuvor abgerufene Zugriffstoken:

   ```bash
   curl 'https://graph.windows.net/myorganization/servicePrincipals/<OBJECT-ID>/getMemberGroups?api-version=1.6' -X POST -d "{\"securityEnabledOnly\": false}" -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS-TOKEN>"
   ```

   Antwort:

   ```bash   
   Content : {"odata.metadata":"https://graph.windows.net/myorganization/$metadata#Collection(Edm.String)","value":["<ObjectID of VM's group membership>"]}
   ```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie eine systemseitig zugewiesene verwaltete Identität einer Linux-VM verwenden, um auf Azure AD Graph zuzugreifen.  Weitere Informationen zu Azure AD Graph finden Sie hier:

>[!div class="nextstepaction"]
>[Azure AD Graph](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api)
