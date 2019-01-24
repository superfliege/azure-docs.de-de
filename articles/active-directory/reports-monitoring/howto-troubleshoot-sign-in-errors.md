---
title: Durchführen der Problembehandlung für Anmeldefehler mit Azure Active Directory-Berichten | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie die Problembehandlung für Anmeldefehler mit Azure Active Directory-Berichten im Azure-Portal durchführen.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: f39557b21e8c54f565ddbb6764d697968d3bbee6
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54823764"
---
# <a name="how-to-troubleshoot-sign-in-errors-using-azure-active-directory-reports"></a>Gewusst wie: Beheben von Anmeldefehlern mit Azure Active Directory-Berichten

Mit dem [Bericht zu Anmeldeaktivitäten](concept-sign-ins.md) in Azure Active Directory (Azure AD) können Sie Antworten auf Fragen zur Verwaltung des Zugriffs auf die Anwendungen in Ihrer Organisation finden, z.B.:

- Wie sieht das Anmeldemuster eines Benutzers aus?
- Wie viele Benutzer sind für Benutzer im Laufe einer Woche angemeldet?
- Wie lautet der Status dieser Anmeldungen?


Darüber hinaus kann Ihnen der Bericht zu Anmeldeaktivitäten auch als Hilfe bei der Problembehandlung von Anmeldefehlern für die Benutzer Ihrer Organisation dienen. In dieser Anleitung wird beschrieben, wie Sie im Bericht zu Anmeldeaktivitäten einen Anmeldefehler isolieren und verwenden, um die Grundursache des Fehlers zu ermitteln.

## <a name="prerequisites"></a>Voraussetzungen

Erforderlich:

* Ein Azure AD-Mandant mit einer Premium-Lizenz (P1/P2). Unter [Erste Schritte mit Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) erfahren Sie, wie Sie ein Upgrade für Ihre Azure Active Directory-Edition durchführen.
* Einen Benutzer, der über die Rolle **Globaler Administrator**, **Sicherheitsadministrator**, **Benutzer mit Leseberechtigung für Sicherheitsfunktionen** oder **Benutzer mit Leseberechtigung für Berichte** für den Mandanten verfügt. Darüber hinaus kann jeder Benutzer auf die eigenen Anmeldungen zugreifen. 

## <a name="troubleshoot-sign-in-errors-using-the-sign-ins-report"></a>Durchführen der Problembehandlung für Anmeldefehler mit dem Bericht zu Anmeldeaktivitäten

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com), und wählen Sie Ihr Verzeichnis aus.
2. Wählen Sie **Azure Active Directory** und dann im Bereich **Überwachung** die Option **Anmeldungen** aus. 
3. Verwenden Sie die bereitgestellten Filter, um den Fehler einzugrenzen, indem Sie entweder den Benutzernamen oder Objektbezeichner, den Anwendungsnamen oder das Datum nutzen. Wählen Sie zusätzlich in der Dropdownliste **Status** die Option **Fehler**, um nur die nicht erfolgreichen Anmeldungen anzuzeigen. 

    ![Ergebnisse filtern](./media/howto-troubleshoot-sign-in-errors/filters.png)
        
4. Identifizieren Sie die fehlerhafte Anmeldung, die Sie untersuchen möchten, und wählen Sie sie aus. Das Fenster mit zusätzlichen Details wird geöffnet, in dem weitere Informationen zur fehlgeschlagenen Anmeldung enthalten sind. Notieren Sie sich den **Code des Anmeldefehlers** und die **Fehlerursache**. 

    ![Auswählen des Datensatzes](./media/howto-troubleshoot-sign-in-errors/sign-in-failures.png)
        
5. Diese Informationen finden Sie auch im Fenster mit den Details auf der Registerkarte **Problembehandlung und Support**.

    ![Problembehandlung und Support](./media/howto-troubleshoot-sign-in-errors/troubleshooting-and-support.png)

6. Die Fehlerursache beschreibt den Fehler. Im obigen Szenario lautet die Fehlerursache beispielsweise **Ungültiger Benutzername bzw. ungültiges Kennwort oder ungültiger lokaler Benutzername bzw. Kennwort**. Dies bedeutet, dass der Benutzer den Benutzernamen oder das Kennwort für die Anmeldung am Azure-Portal falsch eingegeben hat. Die Lösung besteht darin, die Anmeldung einfach mit dem richtigen Benutzernamen und Kennwort erneut durchzuführen.

7. Sie können weitere Informationen abrufen, z.B. Ideen für Lösungen, indem Sie in diesem Beispiel unter [Fehlercodes des Berichts mit den Anmeldeaktivitäten](reference-sign-ins-error-codes.md) nach dem Fehlercode **50126** suchen. 

8. Wenn Sie keinen Erfolg haben oder das Problem trotz der empfohlenen Vorgehensweise weiterhin besteht, können Sie [ein Supportticket erstellen](../fundamentals/active-directory-troubleshooting-support-howto.md), indem Sie die Schritte auf der Registerkarte **Problembehandlung und Support** ausführen. 

## <a name="next-steps"></a>Nächste Schritte

* [Fehlercodes des Berichts mit den Anmeldeaktivitäten](reference-sign-ins-error-codes.md)
* [Berichte zu Anmeldeaktivitäten im Azure Active Directory-Portal](concept-sign-ins.md)
