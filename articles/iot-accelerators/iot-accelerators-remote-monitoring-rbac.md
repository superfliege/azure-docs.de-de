---
title: Remoteüberwachung-Zugriffssteuerung – Azure | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie die rollenbasierte Zugriffssteuerung im Solution Accelerators der Remoteüberwachung konfigurieren.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 08/06/2018
ms.topic: conceptual
ms.openlocfilehash: 8886c08e88c1dd46d2b325b9ba8e9e0b8ae104f9
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2018
ms.locfileid: "40007382"
---
# <a name="configure-role-based-access-controls-in-the-remote-monitoring-solution-accelerator"></a>Konfigurieren der rollenbasierten Zugriffssteuerung im Solution Accelerator der Remoteüberwachung

In diesem Artikel erfahren Sie, wie Sie die rollenbasierte Zugriffssteuerung im Solution Accelerators der Remoteüberwachung konfigurieren. Mit der rollenbasierten Zugriffssteuerung können Sie den Zugriff einzelner Benutzer auf bestimmte Funktionen der Lösung beschränken.

## <a name="default-settings"></a>Standardeinstellungen

Wenn Sie die Remoteüberwachungslösung zum ersten Mal bereitstellen, stehen zwei Rollen zur Verfügung: **Administrator** und **Lesezugriff**.

Benutzer mit der Rolle **Administrator** verfügen über Vollzugriff auf die Lösung. Benutzer mit der Rolle **Lesezugriff** können die folgenden Aufgaben nicht ausführen:

- Aktualisieren von Alarmen
- Löschen von Alarmen
- Erstellen von Geräten
- Aktualisieren von Geräten
- Löschen von Geräten
- Erstellen von Gerätegruppen
- Aktualisieren von Gerätegruppen
- Löschen von Gerätegruppen
- Erstellen von Regeln
- Aktualisieren von Regeln
- Löschen von Regeln
- Erstellen von Aufträgen
- Aktualisieren der SIM-Verwaltung

Die Person, die die Remoteüberwachungslösung bereitstellt, erhält automatisch die Rolle **Administrator** und ist automatisch ein Azure Active Directory-Anwendungsbesitzer. Als Anwendungsbesitzer können Sie anderen Benutzern im Azure-Portal Rollen zuweisen.

Wenn Sie möchten, dass ein anderer Benutzer Rollen in der Lösung zuweist, müssen Sie diesen im Azure-Portal als Anwendungsbesitzer bestimmen.

## <a name="add-or-remove-users"></a>Hinzufügen oder Entfernen von Benutzern

Im Azure-Portal können Sie Benutzer der Remoteüberwachungslösung hinzufügen oder daraus entfernen. In den folgenden Schritten wird die [Azure Active Directory-Unternehmensanwendung](../active-directory/manage-apps/add-application-portal.md#find-your-azure-ad-tenant-application) verwendet, die für Sie beim Bereitstellen der Remoteüberwachungslösung erstellt wurde.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Überprüfen Sie, ob der [Benutzer sich in dem Verzeichnis befindet](../active-directory/fundamentals/add-users-azure-active-directory.md), das Sie verwenden. Sie haben das zu verwendende Verzeichnis ausgewählt, als Sie sich auf der Seite [Microsoft Azure IoT Solution Accelerators](https://www.azureiotsolutions.com/Accelerators) angemeldet haben. Der Verzeichnisname wird oben rechts auf der [Seite](https://www.azureiotsolutions.com/Accelerators) angezeigt.

1. Suchen Sie nach der **Unternehmensanwendung** für Ihre Lösung im Azure-Portal. Der Anwendungsname ist der Name Ihrer Remoteüberwachungslösung. Im folgenden Screenshot sind der Anzeigename der Lösung und der Anwendung **contoso-rm4**.

    ![Unternehmensanwendung](media/iot-accelerators-remote-monitoring-rbac/appregistration.png)

1. Überprüfen Sie, ob Sie der Besitzer der Anwendung sind, indem Sie auf die Anwendung und dann auf **Besitzer** klicken. Im folgenden Screenshot ist **Contoso Admin** ein Besitzer der Anwendung **contoso-rm4**:

    ![Besitzer](media/iot-accelerators-remote-monitoring-rbac/owners.png)

    Wenn Sie kein Besitzer sind, müssen Sie einen vorhandenen Besitzer darum bitten, Sie der Liste hinzuzufügen. Nur Besitzer können anderen Benutzern Anwendungsrollen wie **Administrator** oder **Lesezugriff** zuweisen.

1. Klicken Sie auf **Benutzer und Gruppen**, um die Liste der Benutzer anzuzeigen, denen Rollen in der Anwendung zugewiesen sind.

1. Klicken Sie auf **+ Benutzer hinzufügen**, um einen Benutzer hinzuzufügen, und klicken Sie dann auf **Benutzer und Gruppen, keine ausgewählt**, um einen Benutzer aus dem Verzeichnis auszuwählen.

1. Um dem Benutzer eine Rolle zuzuweisen, klicken Sie auf **Rolle auswählen, keine ausgewählt**, und wählen Sie für den Benutzer die Rolle **Administrator** oder **Lesezugriff** aus. Klicken Sie auf **Auswählen** und dann auf **Zuweisen**.

    ![Rolle wählen](media/iot-accelerators-remote-monitoring-rbac/selectrole.png)

1. Der Benutzer kann nun auf die Remoteüberwachungslösung mit den rollenspezifischen Berechtigungen zugreifen.

1. Auf der Portalseite **Benutzer und Gruppen** können Sie Benutzer aus der Anwendung löschen.

## <a name="create-a-custom-role"></a>Erstellen einer benutzerdefinierten Rolle

Die Remoteüberwachungslösung umfasst bei der ersten Bereitstellung die beiden Rollen **Administrator** und **Lesezugriff**. Sie können benutzerdefinierte Rollen mit unterschiedlichen Berechtigungen hinzufügen. Um eine benutzerdefinierte Rolle zu definieren, müssen Sie:

- Der Anwendung im Azure-Portal eine neue Rolle hinzufügen
- Eine Richtlinie für die neue Rolle im Microservice „Authentifizierung und Autorisierung“ definieren
- Die Webbenutzeroberfläche der Lösung aktualisieren

### <a name="define-a-custom-role-in-the-azure-portal"></a>Erstellen einer benutzerdefinierten Rolle im Azure-Portal

Die folgenden Schritte beschreiben, wie Sie einer Anwendung in Azure Active Directory eine Rolle hinzufügen. In diesem Beispiel erstellen Sie eine neue Rolle, mit der Benutzer in der Remoteüberwachungslösung Geräte erstellen, aktualisieren und löschen können.

1. Suchen Sie nach der **App-Registrierung** für Ihre Lösung im Azure-Portal. Der Anwendungsname ist der Name Ihrer Remoteüberwachungslösung. Im folgenden Screenshot sind der Anzeigename der Lösung und der Anwendung **contoso-rm4**.

    ![App-Registrierung](media/iot-accelerators-remote-monitoring-rbac/appregistration2.png)

1. Wählen Sie Ihre Anwendung aus, und klicken Sie auf **Manifest**. Es werden die beiden vorhandenen [App-Rollen](https://docs.microsoft.com/azure/architecture/multitenant-identity/app-roles) angezeigt, die für die Anwendung definiert sind:

    ![Anzeigen des Manifests](media/iot-accelerators-remote-monitoring-rbac/viewmanifest.png)

1. Bearbeiten Sie das Manifest, um die Rolle **ManageDevices** hinzuzufügen, wie im folgenden Ausschnitt dargestellt. Sie benötigen eine eindeutige Zeichenfolge wie eine GUID für die neue Rollen-ID. Sie können eine neue GUID mit einem Dienst wie [Online GUID Generator](https://www.guidgenerator.com/) generieren:

    ```json
    "appRoles": [
      {
        "allowedMemberTypes": [
          "User"
        ],
        "displayName": "Admin",
        "id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
        "isEnabled": true,
        "description": "Administrator access to the application",
        "value": "Admin"
      },
      {
        "allowedMemberTypes": [
          "User"
        ],
        "displayName": "Read Only",
        "id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
        "isEnabled": true,
        "description": "Read only access to device information",
        "value": "ReadOnly"
      },
      {
        "allowedMemberTypes": [
          "User"
        ],
        "displayName": "ManageDevices",
        "id": "ADD A NEW GUID HERE",
        "isEnabled": true,
        "description": "A new role for the application.",
        "value": "ManageDevices"
      }
    ],
    ```

    Speichern Sie die Änderungen.

### <a name="define-a-policy-for-the-new-role"></a>Definieren einer Richtlinie für die neue Rolle

Nachdem Sie die Rolle der App im Azure-Portal hinzugefügt haben, müssen Sie für die Rolle eine Richtlinie in [roles.json](https://github.com/Azure/remote-monitoring-services-dotnet/blob/master/pcs-auth/Services/data/policies/roles.json) definieren, die zur Geräteverwaltung erforderliche Berechtigungen zuweist.

1. Klonen Sie das GitHub-Repository des Microservices [Authentifizierung und Autorisierung](https://github.com/Azure/pcs-auth-dotnet) auf Ihren lokalen Computer.

1. Bearbeiten Sie die Datei **Services/data/policies/roles.json**, um die Richtlinie der Rolle **ManageDevices** hinzuzufügen (siehe folgender Ausschnitt). Die Werte für **ID** und **Rolle** müssen der Rollendefinition im App-Manifest aus dem vorherigen Abschnitt entsprechen. Die Liste der zulässigen Aktionen ermöglicht Benutzern mit der Rolle **ManageDevices** mit der Lösung verbundene Geräte zu erstellen, zu aktualisieren und zu löschen:

    ```json
    {
    "Items": [
      {
        "Id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
        "Role": "admin",
        "AllowedActions": [
          "UpdateAlarms",
          "DeleteAlarms",
          "CreateDevices",
          "UpdateDevices",
          "DeleteDevices",
          "CreateDeviceGroups",
          "UpdateDeviceGroups",
          "DeleteDeviceGroups",
          "CreateRules",
          "UpdateRules",
          "DeleteRules",
          "CreateJobs",
          "UpdateSimManagement"
        ]
      },
      {
        "Id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
        "Role": "readOnly",
        "AllowedActions": []
      },
      {
        "Id": "GUID FROM APP MANIFEST",
        "Role": "ManageDevices",
        "AllowedActions": [
          "CreateDevices",
          "UpdateDevices",
          "DeleteDevices"
        ]
      }
    ]
    }
    ```

1. Wenn Sie die Datei **Services/data/policies/roles.json** bearbeitet haben, erstellen Sie den Microservice „Authentifizierung und Autorisierung“ neu, und stellen Sie ihn erneut für Ihren Solution Accelerator bereit.

### <a name="how-the-web-ui-enforces-permissions"></a>Erzwingen von Berechtigungen durch die Webbenutzeroberfläche

Die Webbenutzeroberfläche verwendet den Microservice [Authentifizierung und Autorisierung](https://github.com/Azure/pcs-auth-dotnet) um festzulegen, welche Aktionen ein Benutzer ausführen darf, und welche Steuerelemente in der Benutzeroberfläche angezeigt werden. Wenn Ihre Lösung beispielsweise **contoso-rm4** heißt, ruft die Webbenutzeroberfläche eine Liste der zulässigen Aktionen für den aktuellen Benutzer ab, indem sie die folgende Anforderung sendet:

```http
http://contoso-rm4.azurewebsites.net/v1/users/current
headers:
X-Source: true
Authorization: Bearer <JWT Token from ADAL>
```

Für einen Benutzer namens **Geräte-Manager** mit der Rolle **ManageDevices** enthält die Antwort den folgenden JSON-Code im Text:

```json
{
  "Id": "userIdExample",
  "Email": "devicemanager@contoso.com",
  "Name": "Device Manager",
  "AllowedActions": [
    "CreateDevices",
    "UpdateDevices",
    "DeleteDevices"
  ]
}
```

Der folgende Ausschnitt aus [deviceDelete.js](https://github.com/Azure/pcs-remote-monitoring-webui/blob/master/src/components/pages/devices/flyouts/deviceDelete/deviceDelete.js) in der [Webbenutzeroberfläche](https://github.com/Azure/pcs-remote-monitoring-webui/) zeigt, wie die Berechtigungen deklarativ erzwungen werden:

```json
<FlyoutContent>
  <Protected permission={permissions.deleteDevices}>
    <form className="device-delete-container" onSubmit={this.deleteDevices}>
      ...
    </form>
  </Protected>
</FlyoutContent>
```

Weitere Informationen finden Sie unter [Geschützte Komponenten](https://github.com/Azure/pcs-remote-monitoring-webui/blob/master/src/components/shared/protected/README.md). Sie können zusätzliche Berechtigungen in der Datei [authModel.js](https://github.com/Azure/pcs-remote-monitoring-webui/blob/master/src/services/models/authModels.js) definieren.

### <a name="how-the-microservices-enforce-permissions"></a>Erzwingen von Berechtigungen durch Microservices

Die Microservices überprüfen die Berechtigungen, um Schutz vor nicht autorisierten API-Anforderungen zu bieten. Wenn ein Microservice eine API-Anforderung empfängt, decodiert und überprüft er das JWT-Token, um die Benutzer-ID und die Berechtigungen der Benutzerrolle abzurufen.

Der folgende Ausschnitt aus der Datei [DevicesController.cs](https://github.com/Azure/iothub-manager-dotnet/blob/master/WebService/v1/Controllers/DevicesController.cs) im [IoT Hub-Manager-Microservice](https://github.com/Azure/iothub-manager-dotnet) zeigt, wie Berechtigungen erzwungen werden:

```csharp
[HttpDelete("{id}")]
[Authorize("DeleteDevices")]
public async Task DeleteAsync(string id)
{
    await this.devices.DeleteAsync(id);
}
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie die rollenbasierte Zugriffssteuerung im Solution Accelerator der Remoteüberwachung implementieren.

Weitere konzeptuelle Informationen zum Solution Accelerator für Remoteüberwachung finden Sie unter [Remoteüberwachungsarchitektur](iot-accelerators-remote-monitoring-sample-walkthrough.md).

Weitere Informationen zum Anpassen der Remoteüberwachungslösung finden Sie unter [Anpassen und erneutes Bereitstellen eines Microservice](iot-accelerators-microservices-example.md)
<!-- Next tutorials in the sequence -->.