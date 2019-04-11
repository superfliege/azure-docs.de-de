---
title: Anmelden bei Azure Notebooks
description: Melden Sie sich schnell bei Azure Notebooks an, und legen Sie eine Benutzer-ID fest. So haben Sie die Möglichkeit, auf gespeicherte Projekte zuzugreifen und Notebooks für andere Benutzer freizugeben.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: fb8c94b1-6d0a-4b77-8d14-ae6efcdd99f4
ms.service: azure-notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/25/2019
ms.author: kraigb
ms.openlocfilehash: f3effc900b79ddb7beac6a3aaf2eee0a264f7b4d
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59280858"
---
# <a name="quickstart-sign-in-and-set-a-user-id"></a>Schnellstart: Anmelden und Festlegen einer Benutzer-ID

Zwar können Sie Azure-Notebooks jederzeit anzeigen, ohne sich anzumelden, um Notebooks auszuführen, auf gespeicherte Projekte und Notebooks zuzugreifen und Ihre Notebooks mit anderen zu teilen, müssen Sie sich aber anmelden.

## <a name="sign-in"></a>Anmelden

1. Wählen Sie **Sign in**  (Anmelden) oben rechts in [notebooks.azure.com](https://notebooks.azure.com/) aus.

    ![Position des Anmeldebefehls auf Azure Notebooks](media/accounts/sign-in-command.png)

1. Wenn Sie dazu aufgefordert werden, geben Sie die E-Mail-Adresse eines Microsoft-Kontos oder eines Geschäfts-, Schul- oder Unikontos ein, und wählen Sie **Next** (Weiter) aus. Die Kontotypen sind in [Ihr Benutzerkonto für Azure Notebooks](azure-notebooks-user-account.md) beschrieben. Wenn Sie nicht über ein Microsoft-Konto verfügen oder eins speziell für Azure Notebooks einrichten möchten, wählen Sie **Create one** (Konto erstellen) aus:

    ![Anmeldeaufforderung für den Befehl „Create new Microsoft account“ (Neues Microsoft-Konto erstellen)](media/accounts/create-new-microsoft-account.png)

1. Geben Sie Ihr Kennwort ein, wenn Sie dazu aufgefordert werden.

1. Wenn Sie sich zum ersten Mal anmelden, bittet Azure Notebooks Sie um die Berechtigung, auf Ihr Konto zuzugreifen. Wählen Sie **Yes** (Ja) aus, um fortzufahren:

    ![Kontoberechtigungsaufforderung](media/accounts/account-permission-prompt.png)

## <a name="set-a-user-id"></a>Festlegen einer Benutzer-ID

1. Bei der erstmaligen Anmeldung wird Ihnen eine temporäre Benutzer-ID wie „anon-idrca3“ zugewiesen. Wenn Sie über eine Benutzer-ID verfügen, die mit „anon-“ beginnt, fordert Azure Notebooks Sie auf, eine eigene ID zu erstellen. Ihre Benutzer-ID wird in jeder URL verwendet, die Sie abrufen, um Ihre Projekte und Notebooks zu teilen. Wählen Sie daher eine Zeichenfolge, die eindeutig und für Sie aussagekräftig ist.

    ![Aufforderung zur Eingabe einer Benutzer-ID für Azure Notebooks](media/accounts/create-user-id.png)

    Wenn Sie **No Thanks** (Nein danke) auswählen, fährt Azure Notebooks fort, Sie bei jeder Anmeldung zur Erstellung einer Benutzer-ID aufzufordern. Ihre Benutzer-ID kann außerdem jederzeit in Ihrem [Benutzerprofil](azure-notebooks-user-profile.md) festgelegt werden.

1. Nach erfolgreicher Anmeldung navigiert Azure Notebooks zu Ihrer öffentlichen Profilseite, auf der Sie **Edit Profile Information** (Profilinformationen bearbeiten) auswählen können, um Ihre restlichen Informationen einzusetzen (mehr dazu erfahren Sie unter [Ihr Profil und Ihre Benutzer-ID](azure-notebooks-user-profile.md)):

    ![Anfängliche Ansicht einer Azure Notebooks-Profilseite](media/accounts/profile-page-new.png)

> [!NOTE]
> Wenn die Meldung „Benutzer-ID wird bereits verwendet“ angezeigt wird, probieren Sie eine andere ID aus. Benutzer-IDs sind für alle Azure Notebooks-Konten eindeutig, und Azure Notebooks reserviert auch bestimmte Benutzer-IDs, z.B. Markennamen von Microsoft.

## <a name="sign-out"></a>Abmelden

Um sich abzumelden, wählen Sie Ihren Benutzernamen oben rechts auf der Seite und dann **Sign out** (Abmelden) aus:

![Position des Abmeldebefehls auf Azure Notebooks](media/accounts/sign-out-command.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Schnellstart: Erstellen und Freigeben eines Notebooks](quickstart-create-share-jupyter-notebook.md)
