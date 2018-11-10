---
title: Löschen eines Azure Active Directory-Mandantenverzeichnisses | Microsoft-Dokumentation
description: Erläuterungen zum Vorbereiten eines Azure Active Directory-Mandantenverzeichnisses auf das Löschen
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 06/13/2018
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.openlocfilehash: ab752799c9e64e47abe0200a26215f9e01c489f6
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50139685"
---
# <a name="delete-an-azure-active-directory-tenant"></a>Löschen eines Azure Active Directory-Mandanten
Beim Löschen eines Mandanten werden auch alle darin enthaltenen Ressourcen gelöscht. Sie müssen den Mandanten vorbereiten, indem Sie die mit ihm verbundenen Ressourcen minimieren, ehe Sie ihn löschen. Nur ein globaler Administrator von Azure Active Directory (Azure AD) kann einen Azure AD-Mandanten im Portal löschen.

## <a name="prepare-the-tenant-for-deletion"></a>Vorbereiten des Mandanten auf das Löschen

Sie können einen Mandanten in Azure AD erst löschen, nachdem er mehrere Prüfungen bestanden hat. Diese Prüfungen verringern das Risiko, dass das Löschen eines Mandanten den Benutzerzugriff beeinträchtigt, wie z.B. die Möglichkeit, sich bei Office 365 anzumelden oder auf Ressourcen in Azure zuzugreifen. Wenn beispielsweise der Mandant, der einem Abonnement zugeordnet ist, versehentlich gelöscht wird, können Benutzer nicht mehr auf die Azure-Ressourcen dieses Abonnements zugreifen. Im nachfolgenden Abschnitt werden die Bedingungen erläutert, die überprüft werden:

* Es darf keine Benutzer im Mandanten geben, außer einem globalen Administrator, der den Mandanten löschen soll. Alle anderen Benutzer müssen gelöscht werden, ehe der Mandant gelöscht werden kann. Wenn Benutzer aus der lokalen Umgebung synchronisiert werden, muss die Synchronisierung deaktiviert werden, und die Benutzer müssen im Cloudmandanten über das Azure-Portal oder mithilfe von Azure PowerShell-Cmdlets gelöscht werden. 
* Es dürfen keine Anwendungen im Mandanten vorhanden sein. Alle Anwendungen müssen entfernt werden, ehe der Mandant gelöscht werden kann.
* Mit dem Mandanten dürfen keine Anbieter einer mehrstufigen Authentifizierung verknüpft sein.
* Dem Mandanten dürfen keine Abonnements für Microsoft Online Services, z.B. Microsoft Azure, Office 365 oder Azure AD Premium, zugeordnet sein. Falls für Sie in Azure beispielsweise ein Standardmandant erstellt wurde, dürfen Sie diesen Mandaten nicht löschen, wenn er vom Azure-Abonnement noch für die Authentifizierung benötigt wird. Analog dazu dürfen Sie auch keinen Mandanten löschen, wenn ein anderer Benutzer diesem ein Abonnement zugeordnet hat. 

## <a name="delete-an-azure-ad-tenant"></a>Löschen eines Azure AD-Mandanten

1. Melden Sie sich beim [Azure AD Admin Center](https://aad.portal.azure.com) mit einem Konto an, das globale Administratorberechtigungen für den Mandanten hat.

2. Wählen Sie **Azure Active Directory**.

3. Wechseln Sie zu dem Mandanten, den Sie löschen möchten.
  
  ![Schaltfläche „Verzeichnis löschen“](./media/directory-delete-howto/delete-directory-command.png)

4. Klicken Sie auf **Verzeichnis löschen**.
  
  ![Schaltfläche „Verzeichnis löschen“](./media/directory-delete-howto/delete-directory-list.png)

5. Wenn Ihr Mandant eine oder mehrere Prüfungen nicht bestanden hat, erhalten Sie einen Link zu weiteren Informationen, wie die Prüfung bestanden werden kann. Sobald Sie alle Prüfungen bestanden haben, klicken Sie auf **Löschen**, um den Vorgang abzuschließen.

## <a name="i-have-an-expired-subscription-but-i-cant-delete-the-tenant"></a>Ich habe ein abgelaufenes Abonnement, kann aber den Mandanten nicht löschen

Wenn Sie Ihren Azure Active Directory-Mandanten konfiguriert haben, haben Sie möglicherweise auch lizenzbasierte Abonnements für Ihr Unternehmen wie Azure Active Directory Premium P2, Office 365 Business Premium oder Enterprise Mobility + Security E5 aktiviert. Diese Abonnements blockieren das Löschen von Verzeichnissen, bis sie vollständig gelöscht sind, um versehentlichen Datenverlust zu vermeiden. Die Abonnements müssen den Zustand **Bereitstellung aufgehoben** haben, um das Löschen des Mandanten zu ermöglichen. Ein Abonnement im Zustand **Abgelaufen** oder **Gekündigt** wechselt in den Zustand **Deaktiviert**. Die letzte Stufe ist der Zustand **Bereitstellung aufgehoben**. 

Informationen dazu, was zu erwarten ist, wenn ein Testabonnement für Office 365 ausläuft (ohne bezahlte Partner/CSP-, Enterprise Agreement- oder Volumenlizenzen), finden Sie in der folgenden Tabelle. Weitere Informationen zum Datenaufbewahrungs- und Abonnementlebenszyklus von Office 365 finden Sie unter [Was geschieht mit meinen Daten und dem Zugriff darauf, wenn mein Office 365 Business-Abonnement endet?](https://support.office.com/article/what-happens-to-my-data-and-access-when-my-office-365-for-business-subscription-ends-4436582f-211a-45ec-b72e-33647f97d8a3). 

Abonnementzustand | Daten | Zugriff auf Daten
----- | ----- | -----
Aktiv (30 Tage für die Testversion)  | Daten für alle zugänglich    | <li>Benutzer haben normalen Zugriff auf Office 365-Dateien oder -Apps<li>Administratoren haben normalen Zugriff auf das Office 365 Admin Center und -Ressourcen 
Abgelaufen (30 Tage)   | Daten für alle zugänglich    | <li>Benutzer haben normalen Zugriff auf Office 365-Dateien oder -Apps<li>Administratoren haben normalen Zugriff auf das Office 365 Admin Center und -Ressourcen
Deaktiviert (30 Tage) | Daten nur für Administrator zugänglich  | <li>Benutzer können nicht auf Office 365-Dateien oder -Apps zugreifen<li>Administratoren können auf das Office 365 Admin Center zugreifen, aber keine Lizenzen zuweisen oder Benutzer aktualisieren
Bereitstellung aufgehoben (30 Tage nach Deaktivierung) | Daten gelöscht (werden automatisch gelöscht, wenn keine anderen Dienste verwendet werden) | <li>Benutzer können nicht auf Office 365-Dateien oder -Apps zugreifen<li>Administratoren können auf das Office 365 Admin Center zugreifen, um andere Abonnements zu erwerben und zu verwalten 

Sie können ein Abonnement in den Zustand **Bereitstellung aufgehoben** versetzen, um es innerhalb von 3 Tagen im Admin Center von Microsoft Store für Unternehmen zu löschen. Diese Funktion wird in Kürze im Office 365 Admin Center verfügbar sein.

1. Melden Sie sich beim [Admin Center von Microsoft Store für Unternehmen](https://businessstore.microsoft.com/manage/) mit einem Konto mit Rechten eines globalen Administrators im Mandanten an. Wenn Sie versuchen, den Mandanten „Contoso“ zu löschen, der die anfängliche Domäne contoso.onmicrosoft.com hat, melden Sie sich mit einem Benutzerprinzipalnamen wie admin@contoso.onmicrosoft.com an.

2. Wechseln Sie zur Registerkarte **Verwalten**, wählen Sie **Produkte und Dienste** und dann das Abonnement aus, das Sie kündigen möchten. Nachdem Sie auf **Kündigen** geklickt haben, aktualisieren Sie die Seite.
  
  ![Link „Löschen“ zum Löschen des Abonnements](./media/directory-delete-howto/delete-command.png)
  
3. Klicken Sie auf **Löschen**, um das Abonnement zu löschen, und akzeptieren Sie die Geschäftsbedingungen. Alle Daten werden innerhalb von drei Tagen dauerhaft gelöscht. Sie können das Abonnement innerhalb der dreitägigen Frist reaktivieren, sollten Sie Ihre Meinung ändern.
  
  ![Geschäftsbedingungen](./media/directory-delete-howto/delete-terms.png)

4. Nachdem sich der Abonnementzustand geändert hat, wird das Abonnement zum Löschen gekennzeichnet. Das Abonnement wechselt 72 Stunden später in den Zustand **Bereitstellung aufgehoben**.

5. Sobald Sie ein Abonnement für Ihren Mandanten gelöscht haben und 72 Stunden verstrichen sind, können Sie sich wieder im Azure AD Admin Center anmelden. Dort sollten keine Aktionen erforderlich sein und keine Abonnements das Löschen Ihres Mandanten blockieren. Sie sollten Ihren Azure AD-Mandanten nun erfolgreich löschen können.
  
  ![Bildschirm mit bestandener Abonnementprüfung bei Löschung](./media/directory-delete-howto/delete-checks-passed.png)

## <a name="next-steps"></a>Nächste Schritte
[Dokumentation zu Azure Active Directory](https://docs.microsoft.com/azure/active-directory/)
