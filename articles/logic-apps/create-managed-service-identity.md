---
title: Authentifizieren mit verwalteten Identitäten – Azure Logic Apps | Microsoft-Dokumentation
description: Zum Authentifizieren ohne Anmeldung können Sie eine verwaltete Identität (früher als verwaltete Dienstidentität (Managed Service Identity, MSI) bezeichnet) erstellen, sodass Ihre Logik-App ohne Anmeldeinformationen oder Geheimnisse auf Ressourcen in anderen Azure AD-Mandanten (Azure Active Directory) zugreifen kann.
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
services: logic-apps
ms.service: logic-apps
ms.suite: integration
ms.topic: article
ms.date: 10/05/2018
ms.openlocfilehash: 2964869933dd096b96e892cf08b8d4b66a8f210c
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48817057"
---
# <a name="authenticate-and-access-resources-with-managed-identities-in-azure-logic-apps"></a>Authentifizieren und Zugreifen auf Ressourcen mit verwalteten Identitäten in Azure Logic Apps

Zum Zugreifen auf Ressourcen in anderen Azure AD-Mandanten (Azure Active Directory) und Authentifizieren Ihrer Identität ohne Anmeldung kann Ihre Logik-App eine [verwaltete Identität](../active-directory/managed-identities-azure-resources/overview.md) (früher als verwaltete Dienstidentität (Managed Service Identity, MSI) bezeichnet) erstellen, die anstelle von Anmeldeinformationen oder Geheimnissen verwendet wird. Azure verwaltet diese Identität für Sie und dient als Hilfe beim Schützen Ihrer Anmeldeinformationen, da Sie keine Geheimnisse angeben oder eine Rotation dafür durchführen müssen. In diesem Artikel wird veranschaulicht, wie Sie für Ihre Logik-App eine vom System zugewiesene verwaltete Identität erstellen und verwenden können. Weitere Informationen zu verwalteten Identitäten finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../active-directory/managed-identities-azure-resources/overview.md).

> [!NOTE]
> Sie können derzeit bis zu 10 Logik-App-Workflows mit vom System zugewiesenen verwalteten Identitäten in jedem Azure-Abonnement haben.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich <a href="https://azure.microsoft.com/free/" target="_blank">für ein kostenloses Azure-Konto registrieren</a>.

* Die Logik-App, für die Sie die vom System zugewiesene verwaltete Identität verwenden möchten. Wenn Sie keine Logik-App haben, helfen Ihnen die Informationen unter [Schnellstart: Erstellen Ihres ersten automatisierten Workflows mit Azure Logic Apps – Azure-Portal](../logic-apps/quickstart-create-first-logic-app-workflow.md) weiter.

<a name="create-identity"></a>

## <a name="create-managed-identity"></a>Erstellen einer verwalteten Identität

Sie können eine vom System zugewiesene verwaltete Identität für Ihre Logik-App erstellen oder aktivieren, indem Sie das Azure-Portal, Azure Resource Manager-Vorlagen oder Azure PowerShell verwenden. 

### <a name="azure-portal"></a>Azure-Portal

Aktivieren Sie die Einstellung **Registrierung bei Azure Active Directory** in den Workfloweinstellungen Ihrer Logik-App, um über das Azure-Portal eine vom System zugewiesene verwaltete Identität für die Logik-App zu aktivieren.

1. Öffnen Sie Ihre Logik-App im [Azure-Portal](https://portal.azure.com) im Logik-App-Designer.

1. Folgen Sie diesen Schritten: 

   1. Wählen Sie im Menü der Logik-App unter **Einstellungen** die Option **Workfloweinstellungen**. 

   1. Wählen Sie unter **Verwaltete Dienstidentität** > 
   **Registrierung bei Azure Active Directory** die Option **Ein**.

   1. Wählen Sie in der Symbolleiste **Speichern**, wenn Sie fertig sind.

      ![Aktivieren der Einstellung für die verwaltete Identität](./media/create-managed-service-identity/turn-on-managed-service-identity.png)

      Ihre Logik-App verfügt jetzt über eine in Azure Active Directory registrierte, vom System zugewiesene verwaltete Identität mit diesen Eigenschaften und Werten:

      ![GUIDs für Prinzipal-ID und Mandanten-ID](./media/create-managed-service-identity/principal-tenant-id.png)

      | Eigenschaft | Wert | BESCHREIBUNG | 
      |----------|-------|-------------| 
      | **Prinzipal-ID** | <*principal-ID*> | Eine GUID (Globally Unique Identifier), mit der die Logik-App in einem Azure AD-Mandanten identifiziert wird. | 
      | **Tenant ID** | <*Azure-AD-tenant-ID*> | Eine GUID (Globally Unique Identifier), die für den Azure AD-Mandanten steht, auf dem die Logik-App jetzt Mitglied ist. Unter dem Azure AD-Mandanten hat der Dienstprinzipal den gleichen Namen wie die Logik-App-Instanz. | 
      ||| 

### <a name="deployment-template"></a>Bereitstellungsvorlage

Sie können [Azure Resource Manager-Vorlagen](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md) verwenden, wenn Sie das Erstellen und Bereitstellen von Azure-Ressourcen, z.B. Logik-Apps, automatisieren möchten. Um für Ihre Logik-App mit einer Vorlage eine vom System zugewiesene verwaltete Identität zu erstellen, fügen Sie das `"identity"`-Element und die `"type"`-Eigenschaft der Workflowdefinition Ihrer Logik-App in Ihrer Bereitstellungsvorlage hinzu: 

```json
"identity": {
   "type": "SystemAssigned"
}
```

Beispiel: 

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
   "principalId": "<principal-ID>",
   "tenantId": "<Azure-AD-tenant-ID>"
}
```

| Eigenschaft | Wert | BESCHREIBUNG | 
|----------|-------|-------------|
| **principalId** | <*principal-ID*> | Eine GUID (Globally Unique Identifier), mit der die Logik-App im Azure AD-Mandanten identifiziert wird. | 
| **tenantId** | <*Azure-AD-tenant-ID*> | Eine GUID (Globally Unique Identifier), die für den Azure AD-Mandanten steht, auf dem die Logik-App jetzt Mitglied ist. Unter dem Azure AD-Mandanten hat der Dienstprinzipal den gleichen Namen wie die Logik-App-Instanz. | 
||| 

<a name="access-other-resources"></a>

## <a name="access-resources-with-managed-identity"></a>Zugreifen auf Ressourcen per verwalteter Identität

Nachdem Sie eine vom System zugewiesene verwaltete Identität für Ihre Logik-App erstellt haben, können Sie [dieser Identität Zugriff auf andere Azure-Ressourcen gewähren](../active-directory/managed-identities-azure-resources/howto-assign-access-portal.md). Anschließend können Sie diese Identität genauso wie jeden anderen [Dienstprinzipal](../active-directory/develop/app-objects-and-service-principals.md) zur Authentifizierung nutzen. 

> [!NOTE]
> Die vom System zugewiesene verwaltete Identität und die Ressource, der Sie den Zugriff zuweisen möchten, müssen zu demselben Azure-Abonnement gehören.

### <a name="assign-access-to-managed-identity"></a>Zuweisen des Zugriffs zur verwalteten Identität

Um der vom System zugewiesenen verwalteten Identität Ihrer Logik-App den Zugriff auf eine andere Azure-Ressource zu gewähren, gehen Sie folgendermaßen vor:

1. Öffnen Sie im Azure-Portal die Azure-Ressource, wo Sie den Zugriff für Ihre verwaltete Identität zuweisen möchten. 

1. Wählen Sie im Ressourcenmenü **Zugriffssteuerung (IAM)** und dann **Hinzufügen** aus. 

   ![Hinzufügen von Berechtigungen](./media/create-managed-service-identity/add-permissions-logic-app.png)

1. Wählen Sie unter **Berechtigungen hinzufügen** die **Rolle** aus, die Sie für die Identität wünschen. 

1. Wählen Sie in der Eigenschaft **Zugriff zuweisen zu** die Option **Azure AD-Benutzer, -Gruppe oder -Anwendung** aus, falls sie nicht bereits ausgewählt ist.

1. Geben Sie im Feld **Auswählen** beginnend mit dem ersten Zeichen des Namens Ihrer Logik-App den Namen Ihrer Logik-App ein. Wenn Ihre Logik-App angezeigt wird, wählen Sie die Logik-App aus.

   ![Auswählen der Logik-App mit verwalteter Identität](./media/create-managed-service-identity/add-permissions-select-logic-app.png)

1. Wenn Sie fertig sind, wählen Sie **Speichern** aus.

### <a name="authenticate-with-managed-identity-in-logic-app"></a>Authentifizieren mit einer verwalteten Identität in der Logik-App

Nach dem Einrichten Ihrer Logik-App mit einer vom System zugewiesenen verwalteten Identität und Zuweisen des Zugriffs für die Ressource, die Sie für diese Identität verwenden möchten, können Sie diese Identität jetzt zur Authentifizierung verwenden. Sie können z.B. eine HTTP-Aktion verwenden, damit Ihre Logik-App eine HTTP-Anforderung oder einen Aufruf an diese Ressource senden kann. 

1. Fügen Sie in Ihrer Logik-App die **HTTP**-Aktion hinzu. 

1. Geben Sie die erforderlichen Details für diese Aktion an, z.B. die **Methode** für die Anforderung und den **URI**-Speicherort für die aufzurufende Ressource.

1. Wählen Sie in der HTTP-Aktion die Option **Erweiterte Optionen anzeigen**. 

1. Wählen Sie in der Liste **Authentifizierung** die Option **Verwaltete Dienstidentität**, um die Eigenschaft **Audience** (Zielgruppe) anzuzeigen, die Sie festlegen können:

   ![Auswählen von „Verwaltete Dienstidentität“](./media/create-managed-service-identity/select-managed-service-identity.png)

1. Fahren Sie damit fort, die Logik-App wie gewünscht zu erstellen.

<a name="remove-identity"></a>

## <a name="remove-managed-identity"></a>Entfernen der verwalteten Identität

Zum Deaktivieren einer vom System zugewiesenen verwalteten Identität in Ihrer Logik-App können Sie ähnliche Schritte wie beim Erstellen der Identität über das Azure-Portal, mit Azure Resource Manager-Bereitstellungsvorlagen oder mit Azure PowerShell ausführen. 

Wenn Sie Ihre Logik-App löschen, entfernt Azure die vom System zugewiesene Identität Ihrer Logik-App automatisch aus Azure AD.

### <a name="azure-portal"></a>Azure-Portal

1. Öffnen Sie Ihre Logik-App im Logik-App-Designer.

1. Folgen Sie diesen Schritten: 

   1. Wählen Sie im Menü der Logik-App unter **Einstellungen** die Option **Workfloweinstellungen**. 
   
   1. Wählen Sie unter **Verwaltete Dienstidentität** für die Eigenschaft **Registrierung bei Azure Active Directory** die Option **Aus**.

   1. Wählen Sie in der Symbolleiste **Speichern**, wenn Sie fertig sind.

      ![Deaktivieren der Einstellung für die verwaltete Identität](./media/create-managed-service-identity/turn-off-managed-service-identity.png)

### <a name="deployment-template"></a>Bereitstellungsvorlage

Wenn Sie die vom System zugewiesene verwaltete Identität der Logik-App mit einer Azure Resource Manager-Bereitstellungsvorlage erstellt haben, können Sie die `"type"`-Eigenschaft des `"identity"`-Elements auf `"None"` festlegen. Mit dieser Aktion wird die Prinzipal-ID außerdem aus Azure AD gelöscht. 

```json
"identity": {
   "type": "None"
}
```

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Wenn Sie Features vorschlagen oder für Vorschläge abstimmen möchten, besuchen Sie die [Website für Logic Apps-Benutzerfeedback](http://aka.ms/logicapps-wish).

