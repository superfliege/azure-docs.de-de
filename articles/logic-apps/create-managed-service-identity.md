---
title: Zugreifen und Authentifizieren ohne Anmeldung – Azure Logic Apps | Microsoft-Dokumentation
description: Erstellen Sie eine verwaltete Identität, damit Ihre Logik-App ohne Ihre Anmeldeinformationen Ressourcen in anderen Azure AD-Mandanten (Azure Active Directory) authentifizieren und darauf zugreifen kann.
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
services: logic-apps
ms.service: logic-apps
ms.suite: integration
ms.topic: article
ms.date: 09/24/2018
ms.openlocfilehash: fb1c31e6e7c075e20191a4e51d7b1a9323f3b979
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46973965"
---
# <a name="access-resources-and-authenticate-as-managed-identities-in-azure-logic-apps"></a>Zugreifen auf Ressourcen und Authentifizieren als verwaltete Identitäten in Azure Logic Apps

Zum Zugreifen auf Ressourcen in anderen Azure AD-Mandanten (Azure Active Directory) und Authentifizieren Ihrer Identität ohne Anmeldung können Sie eine [verwaltete Identität](../active-directory/managed-identities-azure-resources/overview.md) erstellen, die anstelle Ihrer Anmeldeinformationen Ihre Logik-App verwendet. Azure verwaltet diese Identität für Sie und dient als Hilfe beim Schützen Ihrer Anmeldeinformationen, da Sie keine Geheimnisse angeben oder eine Rotation dafür durchführen müssen. In diesem Artikel wird veranschaulicht, wie Sie für Ihre Logik-App eine verwaltete Identität erstellen und verwenden. Weitere Informationen finden Sie unter [Verwenden verwalteter Identitäten für App Service und Azure Functions](../app-service/app-service-managed-service-identity.md).

> [!NOTE]
> „Verwaltete Identitäten für Azure-Ressourcen“ ist der Ersatzname für den Dienst, der früher als „Verwaltete Dienstidentität“ (Managed Service Identity, MSI) bezeichnet wurde.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich <a href="https://azure.microsoft.com/free/" target="_blank">für ein kostenloses Azure-Konto registrieren</a>.

* Die Logik-App, für die Sie die verwaltete Identität verwenden möchten. Wenn Sie keine Logik-App haben, helfen Ihnen die Informationen unter [Schnellstart: Erstellen Ihres ersten automatisierten Workflows mit Azure Logic Apps – Azure-Portal](../logic-apps/quickstart-create-first-logic-app-workflow.md) weiter.

<a name="create-identity"></a>

## <a name="create-managed-identity"></a>Erstellen einer verwalteten Identität

Sie können eine verwaltete Identität für Ihre Logik-App erstellen oder aktivieren, indem Sie das Azure-Portal, Azure Resource Manager-Vorlagen oder Azure PowerShell verwenden. 

### <a name="azure-portal"></a>Azure-Portal

Aktivieren Sie die Einstellung **Registrierung bei Azure Active Directory** in den Workfloweinstellungen Ihrer Logik-App, um über das Azure-Portal eine verwaltete Identität für die Logik-App zu erstellen.

1. Öffnen Sie Ihre Logik-App im [Azure-Portal](https://portal.azure.com) im Logik-App-Designer.

1. Folgen Sie diesen Schritten: 

   1. Wählen Sie im Menü der Logik-App unter **Einstellungen** die Option **Workfloweinstellungen**. 

   1. Wählen Sie unter **Verwaltete Dienstidentität** > 
   **Registrierung bei Azure Active Directory** die Option **Ein**.

   1. Wählen Sie in der Symbolleiste **Speichern**, wenn Sie fertig sind.

      ![Aktivieren der Einstellung für die verwaltete Identität](./media/create-managed-service-identity/turn-on-managed-service-identity.png)

      In Azure werden diese Eigenschaften und Werte für die verwaltete Identität Ihrer Logik-App jetzt angezeigt:

      ![GUIDs für Prinzipal-ID und Mandanten-ID](./media/create-managed-service-identity/principal-tenant-id.png)

      | Eigenschaft | Wert | BESCHREIBUNG | 
      |----------|-------|-------------| 
      | **Prinzipal-ID** | <*principal-ID-GUID*> | Eine GUID (Globally Unique Identifier), mit der die Logik-App in einem Azure AD-Mandanten identifiziert wird. | 
      | **Tenant ID** | <*Azure-AD-tenant--ID-GUID*> | Eine GUID (Globally Unique Identifier), die für den Azure AD-Mandanten steht, auf dem die Logik-App jetzt Mitglied ist. Unter dem Azure AD-Mandanten hat der Dienstprinzipal den gleichen Namen wie die Logik-App-Instanz. | 
      ||| 

### <a name="deployment-template"></a>Bereitstellungsvorlage

Sie können Azure Resource Manager-Vorlagen einrichten, um das Erstellen und Bereitstellen von Azure-Ressourcen, z.B. Logik-Apps, zu automatisieren. Weitere Informationen finden Sie unter [Erstellen und Bereitstellen von Logik-Apps mit Azure Resource Manager-Vorlagen](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md). 

Gehen Sie wie folgt vor, um für Ihre Logik-App mit einer Vorlage eine verwaltete Identität zu erstellen: Fügen Sie das **identity**-Element und die **type**-Eigenschaft der Workflowdefinition Ihrer Logik-App in Ihrer Bereitstellungsvorlage hinzu. Mit diesen Einstellungen wird angegeben, dass Azure diese Identität für Ihre Logik-App erstellt und verwaltet:

```json
"identity": {
    "type": "SystemAssigned"
}
```

Ihre Logik-App kann beispielsweise wie diese Version aussehen:

```json
{
   "apiVersion": "2016-06-01", 
   "type": "Microsoft.logic/workflows", 
   "name": "[variables('logicappName')]", 
   "location": "[resourceGroup().location]", 
   "identity": { 
      "type": "SystemAssigned" 
   }, 
   "properties": { 
      "definition": { 
         "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#", 
          "actions": {}, 
          "parameters": {}, 
          "triggers": {}, 
          "contentVersion": "1.0.0.0", 
          "outputs": {} 
     }, 
     "parameters": {}, 
     "dependsOn": [] 
}
```

Wenn Azure Ihre Logik-App erstellt, enthält die Workflowdefinition der Logik-App diese zusätzlichen Eigenschaften:

```json
"identity": {
    "type": "SystemAssigned",
    "principalId": "<principal-ID-GUID>",
    "tenantId": "<Azure-AD-tenant-ID>-GUID"
}
```

| Eigenschaft | Wert | BESCHREIBUNG | 
|----------|-------|-------------|
| **principalId** | <*principal-ID-GUID*> | Eine GUID (Globally Unique Identifier), mit der die Logik-App im Azure AD-Mandanten identifiziert wird. | 
| **tenantId** | <*Azure-AD-tenant--ID-GUID*> | Eine GUID (Globally Unique Identifier), die für den Azure AD-Mandanten steht, auf dem die Logik-App jetzt Mitglied ist. Unter dem Azure AD-Mandanten hat der Dienstprinzipal den gleichen Namen wie die Logik-App-Instanz. | 
||| 

<a name="access-other-resources"></a>

## <a name="access-resources-with-managed-identity"></a>Zugreifen auf Ressourcen per verwalteter Identität

Nachdem Sie eine verwaltete Identität für Ihre Logik-App erstellt haben, können Sie [dieser Identität Zugriff auf andere Ressourcen gewähren](../active-directory/managed-identities-azure-resources/howto-assign-access-portal.md). Anschließend können Sie diese verwaltete Identität genauso wie jeden anderen [Dienstprinzipal](../active-directory/develop/app-objects-and-service-principals.md) für die Authentifizierung nutzen. 

Angenommen, Sie haben bereits eine Logik-App mit einer verwalteten Identität eingerichtet, die über Zugriff auf eine andere Ressource verfügt. Sie können jetzt eine HTTP-Aktion hinzufügen, damit Ihre Logik-App eine HTTP-Anforderung oder einen Aufruf an diese Ressource senden kann. 

1. Fügen Sie in Ihrer Logik-App die **HTTP**-Aktion hinzu. 

1. Geben Sie die erforderlichen Details für diese Aktion an, z.B. die **Methode** für die Anforderung und den **URI**-Speicherort für die aufzurufende Ressource.

1. Wählen Sie in der HTTP-Aktion die Option **Erweiterte Optionen anzeigen**. 

1. Wählen Sie in der Liste **Authentifizierung** die Option **Verwaltete Dienstidentität**, um die Eigenschaft **Audience** (Zielgruppe) anzuzeigen, die Sie festlegen können:

   ![Auswählen von „Verwaltete Dienstidentität“](./media/create-managed-service-identity/select-managed-service-identity.png)

1. Fahren Sie damit fort, die Logik-App wie gewünscht zu erstellen.

<a name="remove-identity"></a>

## <a name="remove-managed-identity"></a>Entfernen der verwalteten Identität

Zum Deaktivieren einer verwalteten Identität in Ihrer Logik-App können Sie ähnliche Schritte wie beim Erstellen der Identität über das Azure-Portal, mit Azure Resource Manager-Bereitstellungsvorlagen oder mit Azure PowerShell ausführen. 

Wenn Sie Ihre Logik-App löschen, entfernt Azure die vom System zugewiesene Identität Ihrer Logik-App automatisch aus Azure AD.

### <a name="azure-portal"></a>Azure-Portal

1. Öffnen Sie Ihre Logik-App im Logik-App-Designer.

1. Folgen Sie diesen Schritten: 

   1. Wählen Sie im Menü der Logik-App unter **Einstellungen** die Option **Workfloweinstellungen**. 
   
   1. Wählen Sie unter **Verwaltete Dienstidentität** für die Eigenschaft **Registrierung bei Azure Active Directory** die Option **Aus**.

   1. Wählen Sie in der Symbolleiste **Speichern**, wenn Sie fertig sind.

      ![Deaktivieren der Einstellung für die verwaltete Identität](./media/create-managed-service-identity/turn-off-managed-service-identity.png)

### <a name="deployment-template"></a>Bereitstellungsvorlage

Wenn Sie die verwaltete Identität der Logik-App mit einer Azure Resource Manager-Bereitstellungsvorlage erstellt haben, können Sie die `"type"`-Eigenschaft des `"identity"`-Elements auf `"None"` festlegen. Mit dieser Aktion wird die Prinzipal-ID außerdem aus Azure AD gelöscht. 

```json
"identity": {
    "type": "None"
}
```

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Wenn Sie Features vorschlagen oder für Vorschläge abstimmen möchten, besuchen Sie die [Website für Logic Apps-Benutzerfeedback](http://aka.ms/logicapps-wish).
