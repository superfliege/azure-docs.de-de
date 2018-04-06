---
title: 'Transformieren von XML mit XSLT-Zuordnungen: Azure-Logik-Apps | Microsoft-Dokumentation'
description: Informationen zum Hinzufügen von XSLT-Zuordnungen zum Transformieren von XML-Daten mit Azure Logic Apps und dem Enterprise Integration Pack
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 90f5cfc4-46b2-4ef7-8ac4-486bb0e3f289
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 4b4d626028eed09e9ce6a45fa8fa69859c082da7
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="add-maps-for-xml-data-transform"></a>Hinzufügen von Zuordnungen für die XML-Datentransformation

Für die Unternehmensintegration werden Zuordnungen verwendet, um XML-Daten in andere Formate zu transformieren. Eine Zuordnung ist ein XML-Dokument, das definiert, welche Daten in einem Dokument in ein anderes Format transformiert werden sollen. 

## <a name="why-use-maps"></a>Gründe für Zuordnungen

Angenommen, Sie erhalten regelmäßig B2B-Aufträge oder -Rechnungen von einem Kunden, der das Format JJMMTT für Datumsangaben verwendet. In Ihrer Organisation wird das Datum allerdings im Format TTMMJJJJ gespeichert. Mithilfe einer Zuordnung können das Datumsformat JJMMTT in das Format TTMMJJJJ *transformieren* , ehe die Auftrags- oder Rechnungsdetails in Ihrer Kundendatenbank gespeichert werden.


## <a name="how-do-i-create-a-map"></a>Wie erstelle ich eine Zuordnung?

Das [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack") für Visual Studio 2015 ermöglicht Ihnen die Erstellung von BizTalk Integration-Projekten. Sie können eine Integration Map-Datei erstellen, mit deren Hilfe Sie Elemente visuell zwischen zwei XML-Schemadateien zuordnen können. Nachdem Sie dieses Projekt erstellt haben, verfügen Sie über ein XSLT-Dokument.

Wenn die Zuordnung einen Verweis auf eine externe Assembly enthält, muss beides in das Integrationskonto hochgeladen werden. Die Elemente müssen in einer bestimmten Reihenfolge hochgeladen werden: Laden Sie zuerst die Assembly und dann die Zuordnung hoch, die auf die Assembly verweist.


## <a name="how-do-i-add-a-map"></a>Wie füge ich eine Zuordnung hinzu?

1. Wählen Sie im Microsoft Azure-Portal **Durchsuchen** aus.

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. Geben Sie im Filtersuchfeld das Wort **Integration** ein, und wählen Sie in der Liste mit den Ergebnissen den Eintrag **Integrationskonten** aus.

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. Wählen Sie das Integrationskonto aus, dem Sie das Zertifikat hinzufügen möchten.

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Wählen Sie die Kachel **Zuordnungen** aus.

    ![](./media/logic-apps-enterprise-integration-maps/map-1.png)

5. Klicken Sie auf der Seite „Zuordnungen“ auf **Hinzufügen**.

    ![](./media/logic-apps-enterprise-integration-maps/map-2.png)  

6. Geben Sie einen **Namen** für Ihre App ein. Wählen Sie dann zum Hochladen der Zuordnungsdatei das Ordnersymbol rechts neben dem Textfeld **Zuordnung** aus. Klicken Sie nach dem Hochladevorgang auf **OK**.

    ![](./media/logic-apps-enterprise-integration-maps/map-3.png)

7. Nachdem Azure die Zuordnung Ihrem Integrationskonto hinzugefügt hat, erhalten Sie auf dem Bildschirm die Meldung, dass Ihre Zuordnungsdatei hinzugefügt wurde oder nicht. Nachdem Sie diese Meldung erhalten haben, wählen Sie die Kachel **Zuordnungen** aus, damit Sie die neu hinzugefügte Zuordnung anzeigen können.

    ![](./media/logic-apps-enterprise-integration-maps/map-4.png)


## <a name="how-do-i-add-an-assembly"></a>Wie füge ich eine Assembly hinzu?
Öffnen Sie das Integrationskonto, in das Sie die Assembly hochladen möchten.

1. Klicken Sie auf die Kachel **Assemblys**.

    ![integrationaccount-assembly-tile](./media/logic-apps-enterprise-integration-maps/assemblytile.png)

2. Klicken Sie auf der Seite „Assemblys“ auf **Hinzufügen**. Geben Sie unter **Name** einen Namen für Ihre Assembly ein. Klicken Sie zum Hochladen der Assemblydatei rechts neben dem Textfeld **Assembly** auf das Ordnersymbol. Klicken Sie nach dem Hochladevorgang auf **OK**.

    ![add-assembly](./media/logic-apps-enterprise-integration-maps/assemblyfile.png)


## <a name="how-do-i-edit-a-map"></a>Wie bearbeite ich eine Zuordnung?

Sie müssen eine neue Zuordnungsdatei mit den gewünschten Änderungen hochladen. Zunächst können Sie die Zuordnung zum Bearbeiten herunterladen.

Gehen Sie wie folgt vor, um eine neue Zuordnung hochzuladen, die eine vorhandene Zuordnung ersetzt.

1. Wählen Sie die Kachel **Zuordnungen** aus.

2. Wählen Sie auf der Seite „Zuordnungen“ die Zuordnung aus, die Sie bearbeiten möchten.

3. Klicken Sie auf der Seite **Zuordnungen** auf **Aktualisieren**.

    ![](./media/logic-apps-enterprise-integration-maps/edit-1.png)

4. Wählen Sie in der Dateiauswahl die Zuordnungsdatei, die Sie hochladen möchten, und dann **Öffnen** aus.

    ![](./media/logic-apps-enterprise-integration-maps/edit-2.png)

## <a name="how-to-delete-a-map"></a>Löschen einer Zuordnung

1. Wählen Sie die Kachel **Zuordnungen** aus.

2. Wählen Sie auf der Seite „Zuordnungen“ die Zuordnung aus, die Sie löschen möchten.

3. Wählen Sie **Löschen** aus.

    ![](./media/logic-apps-enterprise-integration-maps/delete.png)

4. Bestätigen Sie, dass Sie die Zuordnung löschen möchten.

    ![](./media/logic-apps-enterprise-integration-maps/delete-confirmation-1.png)

## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zum Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack")  
* [Weitere Informationen zu Vereinbarungen](../logic-apps/logic-apps-enterprise-integration-agreements.md "Informationen zu Vereinbarungen zur Unternehmensintegration")  
* [Weitere Informationen zu Transformationen](logic-apps-enterprise-integration-transform.md "Informationen zu Unternehmensintegrationstransformationen")  

