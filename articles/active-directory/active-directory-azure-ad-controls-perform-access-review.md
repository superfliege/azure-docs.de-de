---
title: "Überprüfen des Zugriffs mithilfe der Azure AD-Zugriffsüberprüfung | Microsoft-Dokumentation"
description: "Überprüfen des Zugriffs mithilfe der Azure AD-Zugriffsüberprüfung"
services: active-directory
author: markwahl-msft
manager: femila
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: billmath
ms.openlocfilehash: c9115bb5bc3d22c67ea0ac77b8972ae7f919926b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-review-access-with-azure-ad-access-reviews"></a>Überprüfen des Zugriffs mit der Azure AD-Zugriffsüberprüfung

Azure Active Directory vereinfacht Unternehmen das Verwalten des Zugriffs auf Anwendungen und Mitglieder von Gruppen in Azure AD und anderen Microsoft Online Services durch eine Funktion namens „Zugriffsüberprüfungen“.  Sie haben möglicherweise eine E-Mail von Microsoft erhalten, in der Sie aufgefordert werden, den Zugriff von Mitgliedern einer Gruppe oder Benutzern mit Zugriff auf eine Anwendung zu überprüfen. 

## <a name="opening-an-access-review"></a>Öffnen einer Zugriffsüberprüfung

Sie können die ausstehenden Zugriffsüberprüfungen durch Klicken auf den Link in der E-Mail anzeigen.  Wenn Sie nicht über die E-Mail verfügen, können Sie die Zugriffsüberprüfungen ermitteln, indem Sie Folgendes ausführen:

1. Melden Sie sich beim [Azure AD-Zugriffsbereich](https://myapps.microsoft.com) an.
2. Klicken Sie auf das Benutzersymbol in der oberen rechten Ecke der Seite, das Ihren Namen und Ihre Standardorganisation anzeigt. Werden mehrere Organisationen aufgeführt, wählen Sie die Organisation aus, die eine Zugriffsüberprüfung angefordert hat.
3. Wenn rechts auf der Seite eine Kachel mit der Bezeichnung **Zugriffsüberprüfungen** vorhanden ist, klicken Sie darauf. Wenn die Kachel nicht angezeigt wird, sind für diese Organisation keine Zugriffsüberprüfungen auszuführen. Daher ist zurzeit keine Aktion erforderlich.

## <a name="filling-out-an-access-review"></a>Ausfüllen einer Zugriffsüberprüfung

Wählen Sie eine Zugriffsüberprüfung aus der Liste aus, und dann sehen Sie die Namen der Benutzer, die überprüft werden müssen.  Sie sehen möglicherweise nur Ihren eigenen Namen, wenn die Anforderung die Überprüfung Ihres eigenen Zugriffs war.

Sie können für jede Zeile auf der Liste entscheiden, ob Sie den Zugriff des Benutzers genehmigen oder verweigern. Sie können auf die Zeile klicken und auswählen, ob Sie den Zugriff genehmigen oder verweigern. (Wenn Sie den Benutzer nicht kennen, können Sie dies auch mitteilen.)

Der Prüfer kann möglicherweise verlangen, dass Sie eine Begründung für den weiteren Zugriffsbedarf oder die Gruppenmitgliedschaft angeben, bevor der Zugriff genehmigt wird.

## <a name="next-steps"></a>Nächste Schritte

Bitte beachten Sie, dass der verweigerte Zugriff des Benutzers nicht direkt entfernt wird. Er kann entfernt werden, wenn die Überprüfung abgeschlossen ist oder ein Administrator die Überprüfung anhält. Wenn Sie also Ihre Antwort ändern und einen zuvor abgelehnten Benutzer genehmigen oder einen zuvor genehmigten Benutzer ablehnen möchten, klicken Sie auf die Zeile, setzen Sie die Antwort zurück und wählen Sie eine neue Antwort aus.  Dies ist möglich, bis die Zugriffsüberprüfung abgeschlossen ist.



