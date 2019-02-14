---
title: Behandeln von Problemen bei der Azure-Zugriffsbereichserweiterung für IE | Microsoft-Dokumentation
description: So stellen Sie das Internet Explorer-Add-On für das Portal "Meine Apps" mithilfe von Gruppenrichtlinien bereit
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/11/2018
ms.author: celested
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: ace2482e51454458977452f0aa610dd43a94e8a7
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56211188"
---
# <a name="troubleshooting-the-access-panel-extension-for-internet-explorer"></a>Problembehandlung in der Zugriffsbereichserweiterung für Internet Explorer
Dieser Artikel hilft Ihnen bei der Behandlung der folgenden Probleme:

* Während der Verwendung von Internet Explorer ist nicht möglich, über das Portal "Meine Apps" auf Ihre Apps zuzugreifen.
* Die Meldung „Software installieren“ wird angezeigt, obwohl Sie die Software bereits installiert haben.

Wenn Sie Administrator sind, lesen Sie auch: [Bereitstellen der Zugriffsbereichserweiterung für Internet Explorer mit der Gruppenrichtlinie; in englischer Sprache](deploy-access-panel-browser-extension.md)

## <a name="run-the-diagnostic-tool"></a>Ausführen des Diagnoseprogramms
Sie können Probleme bei der Installation mithilfe der Zugriffsbereichserweiterung diagnostizieren, indem Sie das Diagnoseprogramm "Zugriffsbereich" herunterladen und ausführen:

1. [Klicken Sie hier, um das Diagnoseprogramm herunterzuladen.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)
2. Öffnen Sie die Datei, und klicken Sie auf die Schaltfläche **Alle extrahieren** .
   
    ![Auf "Alle extrahieren" klicken](./media/manage-access-panel-browser-extension/extract1.png)
3. Klicken Sie dann zum Fortfahren auf die Schaltfläche **Extrahieren** .
   
    ![Auf "Extrahieren" klicken](./media/manage-access-panel-browser-extension/extract2.png)
4. Um das Programm auszuführen, klicken Sie mit der rechten Maustaste auf die Datei mit dem Namen **AccessPanelExtensionDiagnosticTool**, und wählen Sie dann **Öffnen mit > Microsoft Windows Script Host** aus.
   
    ![Öffnen mit > Microsoft Windows Script Host](./media/manage-access-panel-browser-extension/open_tool.png)
5. Anschließend wird das folgende Diagnosefenster angezeigt, in dem die möglichen Probleme bei der Installation angezeigt werden.
   
    ![Ein Beispiel für das Diagnosefenster](./media/manage-access-panel-browser-extension/tool_preview.png)
6. Klicken Sie auf "**Ja**", wenn das Programm die gefundenen Probleme beheben soll.
7. Um diese Änderungen zu speichern, schließen Sie alle Internet Explorer-Fenster, und öffnen Sie Internet Explorer anschließend erneut.<br />Wenn Sie weiterhin nicht auf Ihre Apps zugreifen können, führen Sie die folgenden Schritte aus.

## <a name="check-that-the-access-panel-extension-is-enabled"></a>Überprüfen, ob die Zugriffsbereichserweiterung aktiviert ist
So überprüfen Sie, ob die Zugriffsbereichserweiterung in Internet Explorer aktiviert ist:

1. Klicken Sie in Internet Explorer auf das **Zahnradsymbol** in der oberen rechten Ecke des Fensters. Wählen Sie dann **Internetoptionen** aus.<br />(In älteren Versionen von Internet Explorer finden Sie diese Option unter **Extras > Internetoptionen**.
   
    ![Auf "Extras" > "Internetoptionen" klicken](./media/manage-access-panel-browser-extension/internetoptions.png)
2. Klicken Sie auf die Registerkarte **Programme** und dann auf die Schaltfläche **Add-Ons verwalten**.
   
    ![Auf "Add-Ons verwalten" klicken](./media/manage-access-panel-browser-extension/internetoptions_programs.png)
3. Wählen Sie in diesem Dialogfeld die **Zugriffsbereichserweiterung** aus, und klicken Sie dann auf die Schaltfläche **Aktivieren**.
   
    ![Auf "Aktivieren" klicken](./media/manage-access-panel-browser-extension/enableaddon.png)
4. Um diese Änderungen zu speichern, schließen Sie alle Internet Explorer-Fenster, und öffnen Sie Internet Explorer dann erneut.

## <a name="enable-extensions-for-inprivate-browsing"></a>Aktivieren von Erweiterungen für InPrivate-Browsen
Gehen Sie wie folgt vor, wenn Sie den Modus "InPrivate-Browsen" verwenden:

1. Klicken Sie in Internet Explorer auf das **Zahnradsymbol** in der oberen rechten Ecke des Fensters. Wählen Sie dann **Internetoptionen** aus.<br />(In älteren Versionen von Internet Explorer finden Sie diese Option unter **Extras > Internetoptionen**.
   
    ![Ein Beispiel für das Diagnosefenster](./media/manage-access-panel-browser-extension/inprivateoptions.png)
2. Klicken Sie auf die Registerkarte **Datenschutz**, und **deaktivieren** Sie dann das Kontrollkästchen **Symbolleisten und Erweiterungen beim Starten des InPrivate-Browsens deaktivieren**.</p>
   
    !["Symbolleisten und Erweiterungen beim Starten des InPrivate-Browsens deaktivieren" deaktivieren](./media/manage-access-panel-browser-extension/enabletoolbars.png)
3. Um diese Änderungen zu speichern, schließen Sie alle Internet Explorer-Fenster, und öffnen Sie Internet Explorer dann erneut.

## <a name="uninstall-the-access-panel-extension"></a>Deinstallieren der Zugriffsbereichserweiterung
So deinstallieren Sie die Zugriffsbereichserweiterung auf Ihrem Computer:

1. Drücken Sie auf der Tastatur die **Windows-Taste** , um das Startmenü zu öffnen. Wenn das Menü geöffnet ist, können Sie beliebige Begriffe für die Suche eingeben. Geben Sie "Systemsteuerung" ein, und öffnen Sie dann die **Systemsteuerung** , wenn diese in den Suchergebnissen angezeigt wird.
   
    ![Systemsteuerung suchen](./media/manage-access-panel-browser-extension/search_sm.png)
2. Ändern Sie in der rechten oberen Ecke der Systemsteuerung die Option **Anzeige** in **Große Symbole**. Suchen Sie dann die Schaltfläche **Programme und Funktionen**, und klicken Sie darauf.
   
    ![Ansicht in große Symbole ändern](./media/manage-access-panel-browser-extension/control_panel.png)
3. Wählen Sie in der Liste den Eintrag **Zugriffsbereichserweiterung** aus, und klicken Sie dann auf die Schaltfläche **Deinstallieren**.
   
    ![Auf "Deinstallieren" klicken](./media/manage-access-panel-browser-extension/uninstall.png)
4. Sie können dann versuchen, die Erweiterung erneut zu installieren, um festzustellen, ob das Problem behoben wurde.

Wenn beim Deinstallieren der Erweiterung Probleme auftreten, können Sie diese auch mithilfe des Tools [Microsoft Fix it](https://go.microsoft.com/?linkid=9779673) entfernen.

## <a name="related-articles"></a>Verwandte Artikel
* [Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory](what-is-single-sign-on.md)
* [How to Deploy the Access Panel Extension for Internet Explorer using Group Policy (auf Englisch)](deploy-access-panel-browser-extension.md)

