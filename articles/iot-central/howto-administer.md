---
title: Verwalten einer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie als Administrator eine Azure IoT Central-Anwendung verwalten.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 1bb0bc0aa7ad6bbbad502832ba8e0a96f36de428
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48268299"
---
# <a name="administer-your-iot-central-application"></a>Verwalten Ihrer Azure IoT Central-Anwendung

Nachdem Sie eine Microsoft Azure IoT Central-Anwendung erstellt haben, können Sie diese über den Abschnitt **Verwaltung** auf der Azure IoT Central-Benutzeroberfläche verwalten. Wählen Sie im linken Navigationsmenü **Verwaltung** aus, um zum Abschnitt **Verwaltung** zu navigieren.

Im Abschnitt **Verwaltung** können Sie Folgendes tun:

- Verwalten von Benutzern

- Verwalten von Rollen

- Anzeigen von Abrechnungsinformationen

- Verwalten von Anwendungseinstellungen

- Anbieten einer kostenlosen Testversion

Im Abschnitt **Verwaltung** gibt es ein sekundäres Navigationsmenü mit Links zu verschiedenen Verwaltungsaufgaben.

Um auf den Abschnitt **Verwaltung** zugreifen und diesen verwenden zu können, muss Ihnen die Rolle **Administrator** für eine Azure IoT Central-Anwendung zugewiesen sein. Wenn Sie eine Azure IoT Central-Anwendung erstellen, wird Ihnen automatisch die Rolle **Administrator** für die jeweilige Anwendung zugewiesen. Im Abschnitt *Verwalten von Benutzern* in diesem Artikel wird erläutert, wie Sie anderen Benutzern die Rolle **Administrator** zuweisen.

## <a name="change-application-name"></a>Ändern des Anwendungsnamens

Um den Namen Ihrer Anwendung zu ändern, navigieren Sie über das sekundäre Navigationsmenü zur Seite **Anwendungseinstellungen** im Abschnitt **Verwaltung**.

Geben Sie auf der Seite **Anwendungseinstellungen** einen Namen Ihrer Wahl in das Feld **Anwendungsname** ein. Klicken Sie dann auf **Speichern**.

## <a name="change-the-application-url"></a>Ändern der Anwendungs-URL

Um die URL Ihrer Anwendung zu ändern, navigieren Sie über das sekundäre Navigationsmenü im Abschnitt **Verwaltung** zur Seite **Anwendungseinstellungen**.

![Seite „Anwendungseinstellungen“](media\howto-administer\image0-a.png)

Geben Sie auf der Seite **Anwendungseinstellungen** die URL Ihrer Wahl in das Feld **URL** ein, und wählen Sie dann **Speichern** aus. Ihre URL darf höchstens 200 Zeichen lang sein. Wenn die URL nicht verfügbar ist, wird ein Validierungsfehler angezeigt.

> [!Note]
> Wenn Sie Ihre URL ändern, kann Ihre alte URL von einem anderen Azure IoT Central-Kunden verwendet werden. In diesem Fall kann diese nicht mehr von Ihnen verwendet werden. Wenn Sie Ihre URL ändern, funktioniert die alte URL nicht mehr, und Sie müssen Ihre Benutzer über die neue zu verwendende URL benachrichtigen.

## <a name="change-the-application-image"></a>Ändern des Anwendungsbilds

Weitere Informationen zur Verwendung von Bildern in einer Azure IoT Central-Anwendung finden Sie unter [Vorbereiten und Hochladen von Bildern in Ihre Azure IoT Central-Anwendung](howto-prepare-images.md).

## <a name="copy-an-application"></a>Kopieren einer Anwendung

Sie können eine Kopie jeder Anwendung erstellen, mit Ausnahme von Geräteinstanzen, Gerätedatenverlauf und Benutzerdaten. Die Kopie ist eine kostenpflichtigen Anwendung, die Ihnen in Rechnung gestellt wird. Eine Testversion kann nicht durch das Kopieren einer anderen Anwendung erstellt werden.

Navigieren Sie zur Seite **Anwendungseinstellungen**, um eine Anwendung zu kopieren. Klicken Sie anschließend auf die Schaltfläche **Kopieren**.

![Seite „Anwendungseinstellungen“](media\howto-administer\appCopy1.png)

Durch Klicken auf die Schaltfläche **Kopieren** wird ein Dialogfeld geöffnet, in dem Sie Name, URL, Azure AD-Verzeichnis, Abonnement und Azure-Region für die neue Anwendung auswählen können, die durch Kopieren Ihrer Anwendung erstellt wird. Wählen Sie Werte für diese Felder aus. Wählen Sie dann die Schaltfläche **Kopieren** aus, um zu bestätigen, dass Sie den Vorgang fortsetzen möchten. Weitere Informationen zur Eingabe für diese Werte finden Sie im Artikel zum [Erstellen einer Anwendung](howto-create-application.md).

![Seite „Anwendungseinstellungen“](media\howto-administer\appCopy2.png)

War der Vorgang zum Kopieren der App erfolgreich, können Sie zur neuen Anwendung navigieren, die durch Kopieren Ihrer Anwendung erstellt wurde. Klicken Sie auf den auf der Seite **Anwendungseinstellungen** angezeigten Link, um zur App zu navigieren.

![Seite „Anwendungseinstellungen“](media\howto-administer\appCopy3.png)

> [!Note]
> Beim Kopieren eine Anwendung wird auch die Definition von Regeln oder Aktionen kopiert. Da Benutzer mit Zugriff auf Ihre ursprüngliche App jedoch nicht in die kopierte App übernommen werden, müssen Sie Benutzer manuell zu Aktionen wie E-Mail hinzufügen, für die Benutzer eine erforderliche Komponente sind.

## <a name="delete-an-application"></a>Löschen einer Anwendung

Um Ihre Anwendung zu löschen, navigieren Sie über das sekundäre Navigationsmenü im Abschnitt **Verwaltung** zu der Seite **Anwendungseinstellungen**.

Wählen Sie **Löschen** aus.

> [!Note]
> Bei einer dauerhaften Löschung einer Anwendung werden alle Daten im Zusammenhang mit der Anwendung endgültig gelöscht.  Zum Löschen einer Anwendung benötigen Sie auch Berechtigungen zum Löschen von Ressourcen im Azure-Abonnement, das Sie beim Erstellen der Anwendung gewählt haben. Weitere Informationen finden Sie unter [Verwalten des Zugriffs mithilfe der RBAC und des Azure-Portals](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure).

## <a name="roles-in-azure-iot-central"></a>Rollen in Azure IoT Central

Mit Rollen können Sie steuern, wer in Ihrer Organisation verschiedene Azure IoT Central-Aufgaben ausführen kann. Azure IoT Central stellt drei Rollen zur Verfügung, die Sie Benutzern Ihrer Anwendung zuweisen können. Es werden Rollen von jeder Anwendung zugewiesen. Ein Benutzer kann in verschiedenen Anwendungen unterschiedliche Rollen haben. Auch innerhalb einer Anwendung können Sie einem Benutzer mehrere Rollen zuweisen.

### <a name="administrator"></a>Administrator

Benutzer mit der Rolle **Administrator** haben Zugriff auf alle Funktionen in einer Azure IoT Central-Anwendung.

Dem Benutzer, der eine Anwendung erstellt, wird automatisch die Rolle **Administrator** zugewiesen. Es muss immer mindestens ein Benutzer mit der Rolle **Administrator** vorhanden sein.

### <a name="application-builder"></a>Anwendungsersteller

Benutzer mit der Rolle **Anwendungsersteller** können sämtliche Aktionen in einer Azure IoT Central-Anwendung ausführen, ausgenommen der Verwaltung der Anwendung.

### <a name="application-operator"></a>Anwendungsoperator

Benutzer mit der Rolle **Anwendungsoperator** haben keinen Zugriff auf die Seite **Anwendungsersteller**. Sie können die Anwendung nicht verwalten.

## <a name="manage-users"></a>Verwalten von Benutzern

Anwendungsadministratoren können Benutzern die Rollen in der Anwendung zuweisen.

### <a name="add-users"></a>Hinzufügen von Benutzern

Jeder Benutzer muss ein Benutzerkonto besitzen, um sich bei einer Azure IoT Central-Anwendung anmelden und auf diese zugreifen zu können. Azure IoT Central unterstützt Microsoft-Konten (MSAs) und Azure Active Directory-Konten (Azure AD). Azure Active Directory-Gruppen werden in Azure IoT Central derzeit nicht unterstützt.

Weitere Informationen finden Sie unter [Hilfe zum Microsoft-Konto](https://support.microsoft.com/products/microsoft-account?category=manage-account) und [Vorgehensweise: Hinzufügen oder Löschen von Benutzern in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory).

1. Um ein Benutzerkonto Ihrer Azure IoT Central-Anwendung hinzuzufügen, navigieren Sie über das sekundäre Navigationsmenü im Abschnitt **Verwaltung** zur Seite **Benutzer**.

    ![Liste der Benutzer](media\howto-administer\image1.png)

1. Klicken Sie auf der Seite **Benutzer** auf **+ Benutzer hinzufügen**, um einen Benutzer hinzuzufügen.

    ![Benutzer hinzufügen](media\howto-administer\image2.png)

1. Wählen Sie aus der Dropdownliste **Rolle** eine Rolle für den Benutzer aus. Weitere Informationen zu Rollen finden Sie im Abschnitt *Rollen in Azure IoT Central* dieses Artikels.

    ![Rollenauswahl](media\howto-administer\image3.png)

    > [!NOTE]
    >  Geben Sie zum Massenhinzufügen von Benutzern die Benutzer-IDs aller hinzuzufügenden Benutzer durch Semikolons getrennt ein. Wählen Sie eine Rolle aus der Dropdownliste **Rolle** aus. Klicken Sie dann auf **Speichern**.

1. Nachdem Sie einen Benutzer hinzugefügt haben, wird auf der Seite **Benutzer** ein Eintrag für diesen Benutzer angezeigt.

    ![Benutzerliste](media\howto-administer\image4.png)

### <a name="edit-the-roles-that-are-assigned-to-users"></a>Bearbeiten der den Benutzern zugewiesenen Rollen

Rollen können nach ihrer Zuweisung nicht geändert werden. Um die einem Benutzer zugewiesene Rolle zu ändern, löschen Sie den Benutzer und fügen den Benutzer dann erneut mit einer anderen Rolle hinzu.

### <a name="delete-users"></a>Löschen von Benutzern

Aktivieren Sie zum Löschen von Benutzern auf der Seite **Benutzer** die entsprechenden Kontrollkästchen. Wählen Sie anschließend die Option **Löschen**.

## <a name="view-your-bill"></a>Anzeigen Ihrer Rechnung

Um Ihre Rechnung anzuzeigen, navigieren Sie im Abschnitt **Verwaltung** zur Seite **Abrechnung**. Wählen Sie **Abrechnung** aus. Die Seite mit der Azure-Abrechnung wird auf einer neuen Registerkarte geöffnet. Auf dieser wird für jede Ihrer Azure IoT Central-Anwendung die jeweilige Rechnung angezeigt.

## <a name="convert-your-trial-to-a-paid-application"></a>Umwandeln Ihrer Testversion in eine kostenpflichtige Anwendungsversion

Sobald Sie IoT Central ausgewertet haben, können Sie Ihre Testversion in eine kostenpflichtige Anwendungsversion umwandeln. Um diesen Self-Service-Prozess durchzuführen, gehen Sie folgendermaßen vor:

1. Navigieren Sie über das sekundäre Navigationsmenü im Abschnitt **Verwaltung** zur Seite **Abrechnung**. Wenn Sie Ihre Testversion noch nicht verlängert haben, sieht die Seite wie folgt aus:

    ![Status der kostenlosen Testversion](media/howto-administer/freetrial.png)

2. Wählen Sie **In kostenpflichtige Version umwandeln** aus. Wenn Sie Ihre Testversion noch nicht verlängert haben, sieht das Popupfenster wie folgt aus:

    ![Verlängern einer kostenlosen Testversion](media/howto-administer/extend.png)

3. Wählen Sie im Popupfenster den entsprechenden Azure Active Directory-Mandanten und dann das Azure-Abonnement aus, das für Ihre IoT Central-Anwendung verwendet werden soll.

3. Durch Auswählen von **Umwandeln** wird Ihre Testversion in eine kostenpflichtige Anwendungsversion umgewandelt. Von diesem Zeitpunkt an wird Ihre Nutzung in Rechnung gestellt.

## <a name="extend-your-free-trial"></a>Verlängern Ihrer kostenlosen Testversion

Standardmäßig werden alle kostenlosen Testversionen sieben Tage lang zur Verfügung gestellt. Wenn Sie Ihre Testversion auf 30 Tage verlängern möchten, gehen Sie folgendermaßen vor:

1. Navigieren Sie über das sekundäre Navigationsmenü im Abschnitt **Verwaltung** zur Seite **Abrechnung**.

1. Wählen Sie **Testversion verlängern** aus. Wählen Sie im Popupfenster den entsprechenden Azure Active Directory-Mandanten und dann das Azure-Abonnement aus, das für Ihre IoT Central-Anwendung verwendet werden soll.

1. Wählen Sie dann **Extend** (Verlängern) aus. Ihre Testversion ist jetzt 30 Tage lang gültig.

## <a name="use-the-azure-sdks-for-control-plane-operations"></a>Verwenden der Azure SDKs für Vorgänge auf Steuerungsebene

Azure Resource Manager SDK-Pakete für IoT Central sind für Node, Python, C#, Ruby, Java und Go verfügbar. Diese Bibliotheken unterstützen Vorgänge auf Steuerungsebene für IoT Central, mit denen Sie IoT Central-Anwendungen erstellen, auflisten, aktualisieren oder löschen können. Sie stellen darüber hinaus Hilfsprogramme für die sprachspezifische Authentifizierung und Fehlerbehandlung zur Verfügung. 

Beispiele zur Verwendung der Azure Resource Manager SDKs finden Sie unter [https://github.com/emgarten/iotcentral-arm-sdk-examples](https://github.com/emgarten/iotcentral-arm-sdk-examples).

Sehen Sie sich die folgenden Pakete auf GitHub an, um weitere Informationen zu erhalten:

| Sprache | Repository | Paket |
| ---------| ---------- | ------- |
| Knoten | [https://github.com/Azure/azure-sdk-for-node](https://github.com/Azure/azure-sdk-for-node) | [https://www.npmjs.com/package/azure-arm-iotcentral](https://www.npmjs.com/package/azure-arm-iotcentral)
| Python |[https://github.com/Azure/azure-sdk-for-python](https://github.com/Azure/azure-sdk-for-python) | [https://pypi.org/project/azure-mgmt-iotcentral](https://pypi.org/project/azure-mgmt-iotcentral)
| C# | [https://github.com/Azure/azure-sdk-for-net](https://github.com/Azure/azure-sdk-for-net) | [https://www.nuget.org/packages/Microsoft.Azure.Management.IotCentral](https://www.nuget.org/packages/Microsoft.Azure.Management.IotCentral)
| Ruby | [https://github.com/Azure/azure-sdk-for-ruby](https://github.com/Azure/azure-sdk-for-ruby) | [https://rubygems.org/gems/azure_mgmt_iot_central](https://rubygems.org/gems/azure_mgmt_iot_central)
| Java | [https://github.com/Azure/azure-sdk-for-java](https://github.com/Azure/azure-sdk-for-java) | [https://search.maven.org/search?q=a:azure-mgmt-iotcentral](https://search.maven.org/search?q=a:azure-mgmt-iotcentral)
| Go | [https://github.com/Azure/azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go) | [https://github.com/Azure/azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Ihre Azure IoT Central-Anwendung verwaltet wird, wird als Nächstes der folgende Schritt empfohlen:

> [!div class="nextstepaction"]
> [Einrichten der Gerätevorlage](howto-set-up-template.md)
