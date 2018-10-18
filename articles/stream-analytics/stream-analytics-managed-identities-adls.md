---
title: Verwenden von verwalteten Identitäten zum Authentifizieren von Azure Stream Analytics-Aufträgen für die Azure Data Lake Storage Gen1-Ausgabe (Vorschau)
description: ''
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 09/27/2018
ms.openlocfilehash: 41b3dcc03f7cfbfee11798738a3b2daaf5e96741
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49365287"
---
# <a name="use-managed-identities-to-authenticate-azure-stream-analytics-jobs-to-azure-data-lake-storage-gen1-output-preview"></a>Verwenden von verwalteten Identitäten zum Authentifizieren von Azure Stream Analytics-Aufträgen für die Azure Data Lake Storage Gen1-Ausgabe (Vorschau)

Azure Stream Analytics unterstützt die Authentifizierung über verwaltete Identitäten für die Azure Data Lake Storage Gen1-Ausgabe. Bei einer Identität handelt es sich um eine in Azure Active Directory registrierte verwaltete Anwendung, die einen bestimmten Stream Analytics-Auftrag repräsentiert und für die Authentifizierung bei einer Zielressource verwendet werden kann. Verwaltete Identitäten beseitigen die Einschränkungen benutzerbasierter Authentifizierungsmethoden, wie etwa die Notwendigkeit einer erneuten Authentifizierung aufgrund von Kennwortänderungen oder Token, die alle 90 Tage ablaufen. Darüber hinaus helfen verwaltete Identitäten bei der Automatisierung von Stream Analytics-Auftragsbereitstellungen, deren Ausgabe in Azure Data Lake Storage Gen1 erfolgt.

Lesen Sie den Blogbeitrag [Eight new features in Azure Stream Analytics](https://azure.microsoft.com/blog/eight-new-features-in-azure-stream-analytics/) (Acht neue Features in Azure Stream Analytics), um sich für diese Vorschau zu registrieren und mehr über neue Features zu erfahren.

Dieser Artikel veranschaulicht zwei Methoden, um eine verwaltete Identität für einen Azure Stream Analytics-Auftrag zu aktivieren, dessen Ausgabe an Azure Data Lake Storage Gen1 erfolgt: das Azure-Portal und die Bereitstellung mithilfe von Azure Resource Manager-Vorlagen.

## <a name="enable-managed-identity-with-azure-portal"></a>Aktivieren einer verwalteten Identität über das Azure-Portal

1. Erstellen Sie zunächst einen neuen Stream Analytics-Auftrag, oder öffnen Sie einen vorhandenen Auftrag im Azure-Portal. Wählen Sie in der Menüleiste auf der linken Bildschirmseite unter **Konfigurieren** die Option **Verwaltete Identität (Vorschau)**.

   ![Konfigurieren einer verwalteten Identität für Stream Analytics (Vorschau)](./media/stream-analytics-managed-identities-adls/stream-analytics-managed-identity-preview.png)

2. Wählen Sie im rechts angezeigten Fenster die Option **Vom System zugewiesene verwaltete Identität verwenden (Vorschau)**. Klicken Sie auf **Speichern**, um einen Dienstprinzipal für die Identität des Stream Analytics-Auftrags in Azure Active Directory zu erstellen. Der Lebenszyklus der neu erstellten Identität wird von Azure verwaltet. Wenn der Stream Analytics-Auftrag gelöscht wird, wird die zugeordnete Identität (also der Dienstprinzipal) von Azure automatisch ebenfalls gelöscht.

   Nach dem Speichern der Konfiguration wird die Objekt-ID (OIS) des Dienstprinzipals als Prinzipal-ID aufgeführt, wie hier gezeigt:

   ![Prinzipal-ID von Stream Analytics](./media/stream-analytics-managed-identities-adls/stream-analytics-principal-id.png)
 
   Der Dienstprinzipal weist den gleichen Namen auf wie der Stream Analytics-Auftrag. Wenn der Name des Auftrags z.B. **MyASAJob** lautet, erhält auch der erstellte Dienstprinzipal den Namen **MyASAJob**.

3. Klicken Sie im Fenster mit den Ausgabeeigenschaften der ADLS Gen1-Ausgabesenke auf die Dropdownliste mit Authentifizierungsmodi, und wählen Sie **Verwaltete Identität (Vorschau)** aus.

4. Geben Sie die restlichen Eigenschaften an. Weitere Informationen zum Erstellen einer ADLS-Ausgabe finden Sie unter [Erstellen einer Data Lake Store-Ausgabe mit Stream Analytics](../data-lake-store/data-lake-store-stream-analytics.md). Wenn Sie fertig sind, klicken Sie auf **Speichern**.

   ![Konfigurieren von Azure Data Lake Storage](./media/stream-analytics-managed-identities-adls/stream-analytics-configure-adls.png)
 
5. Navigieren Sie zur Übersichtsseite für ADLS Gen1, und klicken Sie auf **Daten-Explorer**.

   ![Konfigurieren von Data Lake Storage – Übersicht](./media/stream-analytics-managed-identities-adls/stream-analytics-adls-overview.png)

6. Klicken Sie im Bereich „Daten-Explorer“ auf **Zugriff**, und klicken Sie im Bereich „Zugriff“ auf **Hinzufügen**.

   ![Konfigurieren des Data Lake Storage-Zugriffs](./media/stream-analytics-managed-identities-adls/stream-analytics-adls-access.png)

7. Geben Sie im Textfeld im Bereich **Benutzer oder Gruppe auswählen** den Namen des Dienstprinzipals ein. Denken Sie daran, dass der Name des Dienstprinzipals auch der Name des zugehörigen Stream Analytics-Auftrags ist. Wenn Sie anfangen, den Namen einzugeben, wird er unterhalb des Textfelds angezeigt. Wählen Sie den Namen des gewünschten Dienstprinzipals aus, und klicken Sie auf **Auswählen**.

   ![Auswählen eines Dienstprinzipalnamens](./media/stream-analytics-managed-identities-adls/stream-analytics-service-principal-name.png)
 
8. Aktivieren Sie im Bereich **Berechtigungen** die Berechtigungen **Schreiben** und **Ausführen**, und weisen Sie diese der Option **Diesen Ordner und alle untergeordneten Ordner** zu. Klicken Sie dann auf **OK**.

   ![Auswählen einer Berechtigung](./media/stream-analytics-managed-identities-adls/stream-analytics-select-permissions.png)
 
9. Der Dienstprinzipal wird im Bereich **Zugriff** unter **Zugewiesene Berechtigungen** aufgeführt, wie unten gezeigt. Sie können jetzt Ihren Stream Analytics-Auftrag starten.

   ![Zugriffsliste](./media/stream-analytics-managed-identities-adls/stream-analytics-access-list.png)

   Weitere Informationen zu den Dateisystemberechtigungen von Data Lake Storage Gen1 finden Sie unter [Zugriffssteuerung in Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-access-control.md).

## <a name="resource-manager-template-deployment"></a>Resource Manager-Vorlagenbereitstellung

1. Sie können eine *Microsoft.StreamAnalytics/streamingjobs*-Ressource mit einer verwalteten Identität erstellen, indem Sie die folgende Eigenschaft in den Ressourcenabschnitt Ihrer Resource Manager-Vorlage einfügen:

   ```json
   "Identity": {
   "Type": "SystemAssigned",
   },
   ```

   Diese Eigenschaft weist Azure Resource Manager an, die Identität für Ihren Azure Stream Analytics-Auftrag zu erstellen und zu verwalten.

   **Beispielauftrag**

   ```json
   { 
   "Name": "AsaJobWithIdentity", 
   "Type": "Microsoft.StreamAnalytics/streamingjobs", 
   "Location": "West US",
   "Identity": {
     "Type": "SystemAssigned", 
     }, 
   "properties": {
      "sku": {
       "name": "standard"
       },
   "outputs": [
         {
           "name": "string",
           "properties":{
             "datasource": {        
               "type": "Microsoft.DataLake/Accounts",
               "properties": {
                 "accountName": “myDataLakeAccountName",
                 "filePathPrefix": “cluster1/logs/{date}/{time}",
                 "dateFormat": "YYYY/MM/DD",
                 "timeFormat": "HH",
                 "authenticationMode": "Msi"
                 }
                 
   }
   ```
  
   **Antwort des Beispielauftrags**

   ```json
   { 
   "Name": "mySAJob", 
   "Type": "Microsoft.StreamAnalytics/streamingjobs", 
   "Location": "West US",
   "Identity": {
   "Type": "SystemAssigned",
    "principalId": "GUID", 
    "tenantId": "GUID", 
   }, 
   "properties": {
           "sku": {
             "name": "standard"
           },
   }
   ```

   Notieren Sie sich die Prinzipal-ID der Auftragsantwort, um Zugriff auf die angeforderte ADLS-Ressource zu gewähren.

   Die **Mandanten-ID** ist die ID des Azure Active Directory-Mandanten, in dem der Dienstprinzipal erstellt wurde. Der Dienstprinzipal wird in dem Azure-Mandanten erstellt, der von diesem Abonnement als vertrauenswürdig eingestuft wird.

   Der **Typ** gibt den Typ der verwalteten Identität an, wie im Abschnitt mit den Typen verwalteter Identitäten erläutert. Es wird nur der vom System zugewiesene Typ unterstützt.

2. Gewähren Sie Zugriff auf den Dienstprinzipal über PowerShell. Um über PowerShell Zugriff auf den Dienstprinzipal zu gewähren, führen Sie folgenden Befehl aus:

   ```powershell
   Set-AzureRmDataLakeStoreItemAclEntry -AccountName <accountName> -Path <Path> -AceType User -Id <PrinicpalId> -Permissions <Permissions>
   ```

   Die **PrincipalId** ist die Objekt-ID des Dienstprinzipals und wird im Portal aufgeführt, sobald der Dienstprinzipal erstellt wurde. Wenn Sie den Auftrag mithilfe einer Resource Manager-Vorlagenbereitstellung erstellt haben, wird die Objekt-ID in der Identity-Eigenschaft der Auftragsantwort aufgeführt.

   **Beispiel**

   ```powershell
   PS > Set-AzureRmDataLakeStoreItemAclEntry -AccountName "adlsmsidemo" -Path / -AceType
   User -Id 14c6fd67-d9f5-4680-a394-cd7df1f9bacf -Permissions WriteExecute
   ```

   Weitere Informationen zum oben gezeigten PowerShell-Befehl finden Sie in der Dokumentation zu [Set-AzureRmDataLakeStoreItemAclEntry](https://docs.microsoft.com/powershell/module/azurerm.datalakestore/set-azurermdatalakestoreitemaclentry?view=azurermps-6.8.1&viewFallbackFrom=azurermps-4.2.0#optional-parameters).

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen einer Data Lake Store-Ausgabe mit Stream Analytics](../data-lake-store/data-lake-store-stream-analytics.md)
