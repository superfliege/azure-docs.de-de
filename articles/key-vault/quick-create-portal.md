---
title: 'Azure-Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Key Vault mithilfe des Azure-Portals | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe des Azure-Portals ein Geheimnis in Azure Key Vault festlegen und das Geheimnis daraus abrufen.
services: key-vault
author: barclayn
manager: barbkess
tags: azure-resource-manager
ms.assetid: 98cf8387-34de-468e-ac8f-5c02c9e83e68
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: quickstart
ms.custom: mvc
ms.date: 01/07/2019
ms.author: barclayn
ms.openlocfilehash: 4dd81fd03c39dec3c34e614234a563ec8c2ced38
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2019
ms.locfileid: "56117101"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-using-the-azure-portal"></a>Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe des Azure-Portals

Azure Key Vault ist ein Clouddienst, der als sicherer Geheimnisspeicher fungiert. Dadurch können Schlüssel, Kennwörter, Zertifikate und andere Geheimnisse sicher gespeichert werden. Azure Key Vault-Instanzen können über das Azure-Portal erstellt und verwaltet werden. In dieser Schnellstartanleitung erstellen Sie einen Schlüsseltresor und verwenden diesen dann, um ein Geheimnis zu speichern. Weitere Informationen zu Key Vault finden Sie in der [Übersicht](key-vault-overview.md).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter http://portal.azure.com beim Azure-Portal an.

## <a name="create-a-vault"></a>Erstellen eines Tresors

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Option **Ressource erstellen**.

    ![Ausgabe nach Erstellung der Key Vault-Instanz](./media/quick-create-portal/search-services.png)
2. Geben Sie **Key Vault** in das Suchfeld ein.
3. Wählen Sie in der Ergebnisliste **Key Vault** aus.
4. Klicken Sie im Abschnitt „Key Vault“ auf **Erstellen**.
5. Geben Sie im Abschnitt **Schlüsseltresor erstellen** folgende Informationen ein:
    - **Name**: Es ist ein eindeutiger Name erforderlich. Im Rahmen dieser Schnellstartanleitung verwenden wir **Contoso-vault2**. 
    - **Abonnement**: Wählen Sie ein Abonnement aus.
    - Klicken Sie unter **Ressourcengruppe** auf **Neu erstellen**, und geben Sie einen Ressourcengruppennamen ein.
    - Wählen Sie im Pulldownmenü **Speicherort** einen Speicherort aus.
    - Aktivieren Sie das Kontrollkästchen **An Dashboard anheften**.
    - Behalten Sie bei den anderen Optionen die Standardeinstellungen bei.
6. Klicken Sie nach der Angabe der obigen Informationen auf **Erstellen**.

Beachten Sie die beiden folgenden Eigenschaften:

* **Tresorname**: In diesem Beispiel wird **Contoso-Vault2** verwendet. Dieser Name wird noch für andere Schritte benötigt.
* **Tresor-URI**: In diesem Beispiel https://contoso-vault2.vault.azure.net/. Anwendungen, die Ihren Tresor über die zugehörige REST-API nutzen, müssen diesen URI verwenden.

An diesem Punkt ist nur Ihr Azure-Konto zum Ausführen von Vorgängen für den neuen Tresor autorisiert.

![Ausgabe nach Erstellung der Key Vault-Instanz](./media/quick-create-portal/vault-properties.png)

## <a name="add-a-secret-to-key-vault"></a>Hinzufügen eines Geheimnisses zu Key Vault

Ein Geheimnis kann dem Tresor mit einigen wenigen Zusatzschritten hinzugefügt werden. In diesem Fall fügen wir ein Kennwort hinzu, das von einer Anwendung verwendet werden kann. Das Kennwort heißt **ExamplePassword**, und wir speichern darin den Wert **hVFkk965BuUv**.

1. Klicken Sie auf den Key Vault-Eigenschaftenseiten auf **Geheimnisse**.
2. Klicken Sie auf **Generieren/Importieren**.
3. Wählen Sie auf dem Bildschirm **Geheimnis erstellen** folgende Werte aus:
    - **Uploadoptionen**: Manuell
    - **Name**: ExamplePassword
    - **Wert**: hVFkk965BuUv
    - Behalten Sie bei den anderen Optionen die Standardwerte bei. Klicken Sie auf **Create**.

Nachdem Sie die Meldung erhalten haben, dass das Geheimnis erfolgreich erstellt wurde, können Sie in der Liste auf das Geheimnis klicken. Daraufhin werden einige der Eigenschaften angezeigt. Wenn Sie auf die aktuelle Version klicken, sehen Sie den Wert, den Sie im vorherigen Schritt angegeben haben.

![Geheimniseigenschaften](./media/quick-create-portal/version.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Andere Schnellstartanleitungen und Tutorials für Key Vault bauen auf dieser Schnellstartanleitung auf. Falls Sie mit weiteren Schnellstartanleitungen und Tutorials fortfahren möchten, sollten Sie die Ressourcen nicht bereinigen.
Wenn Sie die Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe. Dadurch werden die Key Vault-Instanz und die dazugehörigen Ressourcen gelöscht. So löschen Sie die Ressourcengruppe über das Portal:

1. Geben Sie den Namen Ihrer Ressourcengruppe in das Suchfeld am oberen Rand des Portals ein. Klicken Sie in den Suchergebnissen auf die Ressourcengruppe aus dieser Schnellstartanleitung.
2. Wählen Sie die Option **Ressourcengruppe löschen**.
3. Geben Sie im Feld **GEBEN SIE DEN RESSOURCENGRUPPENNAMEN EIN:** den Namen der Ressourcengruppe ein, und klicken Sie anschließend auf **Löschen**.


## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine Key Vault-Instanz erstellt und ein Geheimnis gespeichert. Das Tutorial zu Webanwendungen mit Key Vault enthält weitere Informationen zu Key Vault und zur Verwendung mit Ihren Anwendungen.

> [!div class="nextstepaction"]
> Wenn Sie erfahren möchten, wie Sie ein Geheimnis aus Key Vault aus einer Webanwendung mit verwalteten Identitäten für Azure-Ressourcen lesen, fahren Sie mit dem Tutorial [Konfigurieren einer Azure-Webanwendung für das Lesen eines Geheimnisses aus Key Vault](quick-create-net.md) fort.
