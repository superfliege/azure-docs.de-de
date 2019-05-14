---
title: Einrichten der Azure Red Hat OpenShift-Entwicklungsumgebung | Microsoft-Dokumentation
description: Hier finden Sie die Voraussetzungen für die Arbeit mit Microsoft Azure Red Hat OpenShift.
services: openshift
keywords: Red Hat OpenShift, Einrichtung, einrichten
author: TylerMSFT
ms.author: twhitney
ms.date: 05/06/2019
ms.topic: conceptual
ms.service: openshift
manager: jeconnoc
ms.openlocfilehash: 7c5fbf135c02abf04e90865e20e902a95174598c
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65080530"
---
# <a name="set-up-your-azure-red-hat-openshift-dev-environment"></a>Einrichten Ihrer Azure Red Hat OpenShift-Entwicklungsumgebung

Für die Erstellung und Ausführung von Microsoft Azure Red Hat OpenShift-Anwendungen müssen Sie folgende Vorbereitungen treffen:

* Erwerben Sie reservierte Azure-VM-Instanzen.
* Installieren Sie Version 2.0.64 (oder höher) der Azure CLI (oder verwenden Sie die Azure Cloud Shell).
* Registrieren Sie sich für das `openshiftmanagedcluster`-Feature und die zugehörigen Ressourcenanbieter.
* Erstellen Sie einen Azure Active Directory-Mandanten (Azure AD).
* Erstellen Sie ein Azure AD-Anwendungsobjekt.
* Erstellen Sie einen Azure AD-Benutzer.

Die folgenden Anweisungen führen Sie durch all diese Voraussetzungen.

## <a name="purchase-azure-virtual-machine-reserved-instances"></a>Erwerben von reservierten Azure-VM-Instanzen

Bevor Sie Azure Red Hat OpenShift verwenden können, müssen Sie reservierte Azure-VM-Instanzen erwerben.

Wenn Sie Azure-Kunde sind, informieren Sie sich, wie Sie [reservierte Azure-VM-Instanzen erwerben](https://aka.ms/openshift/buy). Eine Reservierung senkt Ihre Ausgaben, da Sie im Voraus für vollständig verwaltete Azure-Datendienste zahlen. Unter [*Was sind Azure-Reservierungen*](https://docs.microsoft.com/azure/billing/billing-save-compute-costs-reservations) erhalten Sie weitere Informationen zu Reservierungen und erfahren, wie Sie damit Geld sparen können.

Wenn Sie kein Azure-Kunde sind, [wenden Sie sich an das Vertriebsteam](https://aka.ms/openshift/contact-sales), und füllen Sie das Formular am Ende der Seite aus, um den Prozess zu starten.

## <a name="install-the-azure-cli"></a>Installieren der Azure CLI

Azure Red Hat OpenShift erfordert Version 2.0.64 oder höher der Azure CLI. Wenn Sie die Azure CLI bereits installiert haben, können Sie überprüfen, um welche Version es sich handelt, indem Sie folgenden Befehl ausführen:

```bash
az --version
```

Die erste Zeile der Ausgabe zeigt die Version der Befehlszeilenschnittstelle, z.B. `azure-cli (2.0.64)`.

Wenn Sie eine Neuinstallation oder ein Upgrade benötigen, sehen Sie sich die Anweisungen zum [Installieren der Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) an.

Alternativ können Sie auch die [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) verwenden. Wenn Sie die Azure Cloud Shell verwenden, stellen Sie sicher, dass die **Bash**-Umgebung ausgewählt ist, wenn Sie der Tutorialreihe [Erstellen und Verwalten eines Azure Red Hat OpenShift-Clusters](tutorial-create-cluster.md) folgen möchten.

## <a name="register-providers-and-features"></a>Registrieren von Anbietern und Features

Das `Microsoft.ContainerService openshiftmanagedcluster`-Feature sowie die Anbieter `Microsoft.Solutions` und `Microsoft.Network` müssen manuell in Ihrem Abonnement registriert werden, bevor Sie Ihren ersten Azure Red Hat OpenShift-Cluster bereitstellen können.

Um diese Anbieter und Features manuell zu registrieren, führen Sie die folgenden Anweisungen an einer Bash-Shell aus, wenn Sie die CLI installiert haben. Alternativ dazu können Sie auch eine Azure Cloud Shell-Sitzung (Bash) im Azure-Portal verwenden:
1. Wenn Sie über mehrere Azure-Abonnements verfügen, geben Sie die gewünschte Abonnement-ID an:

    ```bash
    az account set --subscription <SUBSCRIPTION ID>
    ```

2. Registrieren Sie das openshiftmanagedcluster-Feature von Microsoft.ContainerService:

    ```bash
    az feature register --namespace Microsoft.ContainerService -n openshiftmanagedcluster
    ```

3. Registrieren Sie den Microsoft.Solutions-Anbieter:

    ```bash
    az provider register -n Microsoft.Solutions --wait
    ```

4. Registrieren Sie den Microsoft.Network-Anbieter:

    ```bash
    az provider register -n Microsoft.Network --wait
    ```

5. Aktualisieren Sie die Registrierung des Microsoft.ContainerService-Ressourcenanbieters:

    ```bash
    az provider register -n Microsoft.ContainerService --wait
    ```

## <a name="create-an-azure-active-directory-azure-ad-tenant"></a>Erstellen eines Azure Active Directory-Mandanten (Azure AD)

Der Azure Red Hat OpenShift-Dienst erfordert einen zugeordneten Azure AD-Mandanten, der Ihre Organisation und deren Beziehung zu Microsoft repräsentiert. Ihr Azure AD-Mandant ermöglicht es Ihnen, Apps zu registrieren, zu erstellen und zu verwalten sowie weitere Azure-Dienste zu nutzen.

Wenn Sie nicht über ein Azure AD-Verzeichnis verfügen, das Sie als Mandanten für Ihren Azure Red Hat OpenShift-Cluster verwenden können, oder einen Mandanten zu Testzwecken erstellen möchten, befolgen Sie die Anweisungen unter [Erstellen eines Azure AD-Mandanten für Ihren Azure Red Hat OpenShift-Cluster](howto-create-tenant.md), bevor Sie mit diesem Leitfaden fortfahren.

## <a name="create-an-azure-ad-application-object-and-user"></a>Erstellen eines Azure AD-Anwendungsobjekts und -Benutzers

Azure Red Hat OpenShift erfordert Berechtigungen zum Ausführen von Aufgaben in Ihrem Cluster, z.B. zum Konfigurieren von Speicher. Diese Berechtigungen werden durch einen [Dienstprinzipal](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals#service-principal-object) dargestellt und erstellt, wenn Sie eine Azure AD-Anwendung registrieren, die die Workload repräsentiert, die Sie in Azure Red Hat OpenShift hosten möchten. Sie sollten auch einen neuen Active Directory-Benutzer erstellen, um Apps zu testen, die in Ihrem Azure Red Hat OpenShift-Cluster ausgeführt werden sollen.

Befolgen Sie die Anweisungen unter [Erstellen eines Azure AD-App-Objekts und -Benutzers](howto-aad-app-configuration.md), um zu erfahren, wie Sie einen Dienstprinzipal erstellen, einen geheimen Clientschlüssel und eine Rückruf-URL für die Authentifizierung generieren sowie einen neuen Active Directory-Benutzer zum Testen erstellen.

## <a name="next-steps"></a>Nächste Schritte

Sie können Azure Red Hat OpenShift jetzt verwenden!

Lesen Sie dieses Tutorial:
> [!div class="nextstepaction"]
> [Erstellen eines Azure Red Hat OpenShift-Clusters](tutorial-create-cluster.md)

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli