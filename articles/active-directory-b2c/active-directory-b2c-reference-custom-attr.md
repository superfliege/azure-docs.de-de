---
title: Definieren benutzerdefinierter Attribute in Azure Active Directory B2C | Microsoft-Dokumentation
description: Definieren benutzerdefinierter Attribute für Ihre Anwendung in Azure Active Directory B2C zum Erfassen von Informationen zu Ihren Kunden.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 277a1d5df0e6eb828c4d3c6faed91d55defb4108
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2019
ms.locfileid: "58894029"
---
# <a name="define-custom-attributes-in-azure-active-directory-b2c"></a>Definieren benutzerdefinierter Attribute in Azure Active Directory B2C

 Für jede kundenorientierte Anwendung gelten spezifische Anforderungen im Hinblick auf die Informationen, die erfasst werden sollen. Der Azure Active Directory (Azure AD) B2C-Mandant umfasst einen integrierten Satz von in Attributen gespeicherten Informationen, z.B. Vorname, Nachname, Ort und Postleitzahl. Mit Azure AD B2C haben Sie die Möglichkeit, den für die einzelnen Kundenkonten gespeicherten Satz von Attributen zu erweitern. 
 
 Im [Azure-Portal](https://portal.azure.com/) können Sie benutzerdefinierte Attribute erstellen und sie in den Benutzerflows für die Registrierung, den Benutzerflows für Registrierung und Anmeldung und den Benutzerflows für die Profilbearbeitung verwenden. Außerdem können Sie diese Attribute mit der [Azure AD Graph-API](active-directory-b2c-devquickstarts-graph-dotnet.md)lesen und schreiben. Benutzerdefinierte Attribute in Azure AD B2C verwenden die [Verzeichnisschemaerweiterungen der Azure AD Graph-API](/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-directory-schema-extensions).

## <a name="create-a-custom-attribute"></a>Erstellen eines benutzerdefinierten Attributs

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.
2. Vergewissern Sie sich, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie oben rechts im Azure-Portal zu diesem Verzeichnis wechseln. Wählen Sie die Abonnementinformationen aus, und klicken Sie dann auf **Verzeichnis wechseln**. 

    ![Wechseln zu Ihrem Azure AD B2C-Mandanten](./media/active-directory-b2c-reference-custom-attr/switch-directories.png)

    Wählen Sie das Verzeichnis aus, das den Mandanten enthält.

    ![Auswählen des Verzeichnisses](./media/active-directory-b2c-reference-custom-attr/select-directory.png)

3. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **Azure AD B2C**, und klicken Sie darauf.
4. Wählen Sie **Benutzerattribute** und dann **Hinzufügen** aus.
5. Geben Sie einen **Namen** für das benutzerdefinierte Attribut an (z.B. „ShoeSize“).
6. Wählen Sie einen **Datentyp** aus. Es stehen nur **Zeichenfolge**, **Boolescher Wert** und **Int** zur Verfügung.
7. Geben Sie optional eine **Beschreibung** zu Informationszwecken ein. 
8. Klicken Sie auf **Create**.

Das benutzerdefinierte Attribut steht jetzt in der Liste der **Benutzerattribute** zur Verfügung und kann in den Benutzerflows verwendet werden. Ein benutzerdefiniertes Attribut wird nur erstellt, wenn es zum ersten Mal in einem Benutzerflow verwendet wird, und nicht, wenn Sie es der Liste der **Benutzerattribute** hinzufügen.

## <a name="use-a-custom-attribute-in-your-user-flow"></a>Verwenden eines benutzerdefinierten Attributs in einem Benutzerflow

1. Wählen Sie in Ihrem Azure AD B2C-Mandanten die Option **Benutzerflows** aus.
2. Wählen Sie eine Richtlinie aus (z.B. „B2C_1_SignupSignin“), um sie zu öffnen. 
4. Wählen Sie **Benutzerattribute** und dann das benutzerdefinierte Attribut aus (z.B. „ShoeSize“). Klicken Sie auf **Speichern**.
5. Wählen Sie **Anwendungsansprüche** und dann das benutzerdefinierte Attribut aus. 
6. Klicken Sie auf **Speichern**.

Mit der Funktion **Benutzerflow ausführen** für den Benutzerflow können Sie die Benutzerfreundlichkeit überprüfen. **ShoeSize** sollte jetzt in der Liste der Attribute, die während der Registrierung erfasst werden, sowie in dem Token angezeigt werden, das zurück an die Anwendung gesendet wird.

