---
title: Erstellen einer Identität für die Azure-App im Portal | Microsoft-Dokumentation
description: Beschreibt das Erstellen einer neuen Azure Active Directory-Anwendung und eines Dienstprinzipals, der mit der rollenbasierten Zugriffskontrolle in Azure Resource Manager zum Verwalten des Zugriffs auf Ressourcen verwendet werden kann.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/21/2018
ms.author: tomfitz
ms.openlocfilehash: 264befc6c60b87d41658b4da763e477fbb7e3f8c
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="use-portal-to-create-an-azure-active-directory-application-and-service-principal-that-can-access-resources"></a>Erstellen einer Azure Active Directory-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals

Richten Sie eine Azure Active Directory-Anwendung (AD) ein, wenn Sie über Code verfügen, mit dem auf Ressourcen zugegriffen werden muss oder mit dem Änderungen dafür durchgeführt werden müssen. Sie weisen der AD-Anwendung die erforderlichen Berechtigungen zu. Dieser Ansatz ist dem Ausführen der App mit Ihren eigenen Anmeldeinformationen vorzuziehen, da Sie der App-Identität Berechtigungen zuweisen können, die sich von Ihren eigenen Berechtigungen unterscheiden. In der Regel sind diese Berechtigungen genau auf die Aufgaben der App beschränkt.

In diesem Artikel erfahren Sie, wie diese Schritte über das Portal ausgeführt werden. Es konzentriert sich auf eine Anwendung mit nur einem Mandanten, die nur zur Ausführung in einer einzigen Organisation vorgesehen ist. Anwendungen mit nur einem Mandanten werden in der Regel für innerhalb Ihrer Organisation ausgeführte Branchenanwendungen verwendet.

> [!IMPORTANT]
> Anstatt einen Dienstprinzipal zu erstellen, sollten Sie die Verwendung der verwalteten Dienstidentität für Azure AD für Ihre Anwendungsidentität erwägen. Die verwaltete Dienstidentität für Azure AD (Azure AD MSI) ist ein öffentliches Vorschaufeature von Azure Active Directory, mit dem das Erstellen einer Identität für Code vereinfacht wird. Wenn Ihr Code unter einem Dienst ausgeführt wird, der Azure AD MSI unterstützt, und auf Ressourcen zugreift, die die Azure Active Directory-Authentifizierung unterstützen, ist Azure AD MSI für Sie die besser geeignete Option. Weitere Informationen zu Azure AD MSI, z.B. welche Dienste derzeit unterstützt werden, finden Sie unter [Verwaltete Dienstidentität für Azure-Ressourcen](../active-directory/managed-service-identity/overview.md).

## <a name="required-permissions"></a>Erforderliche Berechtigungen

Um diesen Artikel abzuschließen, müssen Sie über ausreichende Berechtigungen verfügen, um eine Anwendung bei Ihrem Azure AD-Mandanten zu registrieren, und die Anwendung einer Rolle in Ihrem Azure-Abonnement zuzuweisen. Stellen Sie sicher, dass Sie über die richtigen Berechtigungen für diese Schritte verfügen.

### <a name="check-azure-active-directory-permissions"></a>Überprüfen der Azure Active Directory-Berechtigungen

1. Wählen Sie **Azure Active Directory**.

   ![Azure Active Directory auswählen](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Wählen Sie in Azure Active Directory die Option **Benutzereinstellungen** aus.

   ![Benutzereinstellungen auswählen](./media/resource-group-create-service-principal-portal/select-user-settings.png)

1. Überprüfen Sie die Einstellung **App Registrierungen**. Ist diese Option auf **Ja** festgelegt, können Benutzer ohne Administratorrechte AD-Apps registrieren. Diese Einstellung bedeutet, dass jeder Benutzer im Azure AD-Mandanten Apps registrieren kann. Sie können mit [Berechtigungen des Azure-Abonnements überprüfen](#check-azure-subscription-permissions) fortfahren.

   ![App-Registrierungen anzeigen](./media/resource-group-create-service-principal-portal/view-app-registrations.png)

1. Wenn die App-Registrierungseinstellung auf **Nein** festgelegt ist, können nur Administratorbenutzer Apps registrieren. Überprüfen Sie, ob Ihr Konto ein Administrator für den Azure AD-Mandanten ist. Wählen Sie **Übersicht**, und sehen Sie sich Ihre Benutzerinformationen an. Gehen Sie wie folgt vor, wenn Ihrem Konto die Rolle „Benutzer“ zugewiesen wurde, die App-Registrierungseinstellung (aus dem vorherigen Schritt) aber auf Administratoren begrenzt ist: Bitten Sie Ihren Administrator, Ihnen entweder eine Administratorrolle zuzuweisen oder es Benutzern zu erlauben, Apps zu registrieren.

   ![Benutzer suchen](./media/resource-group-create-service-principal-portal/view-user-info.png)

### <a name="check-azure-subscription-permissions"></a>Überprüfen der Berechtigungen des Azure-Abonnements

Ihr Konto muss in Ihrem Azure-Abonnement über `Microsoft.Authorization/*/Write`-Zugriff verfügen, um einer Rolle eine AD-App zuzuweisen. Diese Aktion wird über die Rolle [Besitzer](../active-directory/role-based-access-built-in-roles.md#owner) oder [Benutzerzugriffsadministrator](../active-directory/role-based-access-built-in-roles.md#user-access-administrator) gewährt. Wenn Ihr Konto der Rolle **Mitwirkender** zugewiesen ist, verfügen Sie nicht über die erforderliche Berechtigung. Beim Versuch, einer Rolle den Dienstprinzipal zuzuweisen, erhalten Sie eine Fehlermeldung.

So überprüfen Sie die Berechtigungen Ihres Abonnements

1. Wählen Sie oben rechts Ihr Konto und dann die Option **Meine Berechtigungen**.

   ![Auswählen von Benutzerberechtigungen](./media/resource-group-create-service-principal-portal/select-my-permissions.png)

1. Wählen Sie in der Dropdownliste das Abonnement aus. Wählen Sie **Click here to view complete access details for this subscription** (Klicken Sie hier, um die vollständigen Zugriffsdetails für dieses Abonnement anzuzeigen).

   ![Benutzer suchen](./media/resource-group-create-service-principal-portal/view-details.png)

1. Zeigen Sie Ihre zugewiesenen Rollen an, und ermitteln Sie, ob Sie über die erforderlichen Berechtigungen verfügen, um einer Rolle eine AD-App zuzuweisen. Wenn dies nicht der Fall ist, bitten Sie Ihren Abonnementadministrator, Sie zur Rolle „Benutzerzugriffsadministrator“ hinzuzufügen. In der folgenden Abbildung wurde der Benutzer der Rolle „Besitzer“ zugeordnet. Dies bedeutet, dass dieser Benutzer über die erforderlichen Berechtigungen verfügt.

   ![Berechtigungen anzeigen](./media/resource-group-create-service-principal-portal/view-user-role.png)

## <a name="create-an-azure-active-directory-application"></a>Erstellen einer Azure Active Directory-Anwendung

1. Melden Sie sich über das [Azure-Portal](https://portal.azure.com) bei Ihrem Azure-Konto an.
1. Wählen Sie **Azure Active Directory**.

   ![Azure Active Directory auswählen](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Wählen Sie **App-Registrierungen** aus.

   ![App-Registrierungen auswählen](./media/resource-group-create-service-principal-portal/select-app-registrations.png)

1. Wählen Sie **Registrierung einer neuen Anwendung** aus.

   ![App hinzufügen](./media/resource-group-create-service-principal-portal/select-add-app.png)

1. Geben Sie einen Namen und eine URL für die Anwendung an. Wählen Sie als Typ für die zu erstellende Anwendung **Web-App/API** aus. Sie können keine Anmeldeinformationen für eine [native](../active-directory/active-directory-application-proxy-native-client.md) Anwendung erstellen. Daher funktioniert dieser Typ nicht für eine automatisierte Anwendung. Wählen Sie nach dem Festlegen der Werte **Erstellen** aus.

   ![Anwendung benennen](./media/resource-group-create-service-principal-portal/create-app.png)

Sie haben Ihre Anwendung erstellt.

## <a name="get-application-id-and-authentication-key"></a>Abrufen der Anwendungs-ID und des Authentifizierungsschlüssels

Beim programmgesteuerten Anmelden benötigen Sie die ID für Ihre Anwendung und einen Authentifizierungsschlüssel. Führen Sie die folgenden Schritte aus, um diese Werte abzurufen:

1. Wählen Sie in Azure Active Directory unter **App-Registrierungen** Ihre Anwendung aus.

   ![Anwendung auswählen](./media/resource-group-create-service-principal-portal/select-app.png)

1. Kopieren Sie die **Anwendungs-ID**, und speichern Sie sie in Ihrem Anwendungscode. In einigen [Beispielanwendungen](#log-in-as-the-application) wird dieser Wert als „Client-ID“ bezeichnet.

   ![Client-ID](./media/resource-group-create-service-principal-portal/copy-app-id.png)

1. Wählen Sie zum Generieren eines Authentifizierungsschlüssels die Option **Einstellungen**.

   ![Auswählen von „Einstellungen“](./media/resource-group-create-service-principal-portal/select-settings.png)

1. Wählen Sie zum Generieren eines Authentifizierungsschlüssels die Option **Schlüssel** aus.

   ![Schlüssel auswählen](./media/resource-group-create-service-principal-portal/select-keys.png)

1. Geben Sie eine Beschreibung des Schlüssels und eine Dauer für den Schlüssel ein. Wählen Sie dann die Option **Schließen**.

   ![Schlüssel speichern](./media/resource-group-create-service-principal-portal/save-key.png)

   Nach dem Speichern des Schlüssels wird der Wert des Schlüssels angezeigt. Kopieren Sie diesen Wert jetzt, da Sie ihn später nicht mehr abrufen können. Sie geben den Schlüsselwert zusammen mit der Anwendungs-ID ein, um sich als Anwendung anzumelden. Speichern Sie die Schlüsselwert an einem Ort, von dem Ihre Anwendung ihn abrufen kann.

   ![gespeicherter Schlüssel](./media/resource-group-create-service-principal-portal/copy-key.png)

## <a name="get-tenant-id"></a>Abrufen der Mandanten-ID

Beim programmgesteuerten Anmelden müssen Sie mit Ihrer Authentifizierungsanforderung die Mandanten-ID übergeben.

1. Wählen Sie **Azure Active Directory**.

   ![Azure Active Directory auswählen](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Wählen Sie zum Abrufen der Mandanten-ID die Option **Eigenschaften** für Ihren Azure AD-Mandanten aus.

   ![Auswählen von Azure AD-Eigenschaften](./media/resource-group-create-service-principal-portal/select-ad-properties.png)

1. Kopieren Sie die **-Verzeichnis-ID**. Dieser Wert ist Ihre Mandanten-ID.

   ![tenant ID](./media/resource-group-create-service-principal-portal/copy-directory-id.png)

## <a name="assign-application-to-role"></a>Zuweisen einer Anwendung zur Rolle

Um auf Ressourcen in Ihrem Abonnement zuzugreifen, müssen Sie die Anwendung einer Rolle zuweisen. Entscheiden Sie, welche Rolle die geeigneten Berechtigungen für die Anwendung darstellt. Informationen zu verfügbaren Rollen finden Sie unter [RBAC: Integrierte Rollen](../active-directory/role-based-access-built-in-roles.md).

Sie können den Umfang auf Abonnement-, Ressourcengruppen- oder Ressourcenebene festlegen. Berechtigungen werden von niedrigeren Ebenen mit geringerem Umfang geerbt. Wenn z.B. der Leserolle für eine Ressourcengruppe eine Anwendung hinzugefügt wird, kann diese Rolle die Ressourcengruppe und alle darin enthaltenen Ressourcen lesen.

1. Navigieren Sie zur Bereichsebene, der Sie die Anwendung zuweisen möchten. Um z.B. einer Gruppe im Abonnementkontext eine Rolle zuzuweisen, wählen Sie **Abonnements** aus. Sie können stattdessen auch eine Ressourcengruppe oder Ressource auswählen.

   ![Abonnement auswählen](./media/resource-group-create-service-principal-portal/select-subscription.png)

1. Wählen Sie das entsprechende Abonnement (Ressourcengruppe oder Ressource) aus, dem die Anwendung zugewiesen werden soll.

   ![Abonnement für Zuweisung auswählen](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

1. Wählen Sie **Access Control (IAM)** aus.

   ![„Zugriff“ auswählen](./media/resource-group-create-service-principal-portal/select-access-control.png)

1. Wählen Sie **Hinzufügen**.

   ![„Hinzufügen“ wählen](./media/resource-group-create-service-principal-portal/select-add.png)

1. Wählen Sie die Rolle aus, die Sie der Anwendung zuweisen möchten. In der folgenden Abbildung ist die Rolle **Leser** dargestellt.

   ![Rolle wählen](./media/resource-group-create-service-principal-portal/select-role.png)

1. Azure Active Directory-Anwendungen werden standardmäßig nicht in den verfügbaren Optionen angezeigt. Zum Suchen Ihrer Anwendung müssen Sie ihren Namen ins Suchfeld eingeben. Wählen Sie dies aus.

   ![Nach App suchen](./media/resource-group-create-service-principal-portal/search-app.png)

1. Wählen Sie **Speichern** aus, um das Zuweisen der Rolle abzuschließen. Ihre Anwendung wird in der Liste der Benutzer angezeigt, die einer Rolle für diesen Kontext zugewiesen sind.

## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Einrichten einer Anwendung mit mehreren Mandanten finden Sie im [Entwicklerhandbuch für die Autorisierung mit der Azure Resource Manager-API](resource-manager-api-authentication.md).
* Informationen zum Festlegen von Sicherheitsrichtlinien finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-control-configure.md).  
* Eine Liste der verfügbaren Aktionen, die Benutzern erteilt oder verweigert werden können, finden Sie unter [Vorgänge für Azure Resource Manager-Ressourcenanbieter](../active-directory/role-based-access-control-resource-provider-operations.md).
