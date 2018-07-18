---
title: Sprachanpassung in benutzerdefinierten Azure Active Directory B2C-Richtlinien | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Inhalte in benutzerdefinierten Richtlinien für mehrere Sprachen lokalisieren.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/13/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 6269ac65e5db20521346d5312bcbadd0905c36e2
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37440563"
---
# <a name="language-customization-in-custom-policies"></a>Sprachanpassung in benutzerdefinierten Richtlinien

> [!NOTE]
> Dieses Feature befindet sich in der Phase der öffentlichen Vorschau.
> 

In benutzerdefinierten Richtlinien funktioniert die Sprachanpassung genau so wie bei den integrierten Richtlinien.  Lesen Sie in der integrierten [Dokumentation](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-language-customization) nach, die das Verhalten bei der Auswahl der Sprache basierend auf Parametern und Browsereinstellungen beschreibt.

## <a name="enable-supported-languages"></a>Aktivieren unterstützter Sprachen
Wenn „ui-locales“ nicht angegeben wurde und der Browser des Benutzers nach einer dieser Sprachen fragt, werden die unterstützten Sprachen dem Benutzer angezeigt.  

Unterstützte Sprachen sind in `<BuildingBlocks>` im folgenden Format definiert:

```XML
<BuildingBlocks>
  <Localization>
    <SupportedLanguages DefaultLanguage="en">
      <SupportedLanguage>qps-ploc</SupportedLanguage>
      <SupportedLanguage>en</SupportedLanguage>
    </SupportedLanguages>
  </Localization>
</BuildingBlocks>
```

Standardsprache und unterstützte Sprachen verhalten sich ebenso wie in den integrierten Richtlinien.

## <a name="enable-custom-language-strings"></a>Aktivieren von Zeichenfolgen in einer benutzerdefinierten Sprache

Für das Erstellen von Zeichenfolgen in einer benutzerdefinierten Sprache sind zwei Schritte erforderlich:
1. Bearbeiten der `<ContentDefinition>` für die Seite, um eine Ressourcen-ID für die gewünschten Sprachen anzugeben
2. Erstellen der `<LocalizedResources>` mit entsprechenden IDs in Ihren `<BuildingBlocks>`

Beachten Sie, dass Sie abhängig davon, ob die Änderungen für alle Ihre erbenden Richtlinien übernommen werden sollen oder nicht, eine `<ContentDefinition>` und einen `<BuildingBlock>` sowohl in Ihrer Erweiterungsdatei als auch in der zugrundeliegenden Richtliniendatei ablegen können.

### <a name="edit-the-contentdefinition-for-the-page"></a>Bearbeiten der ContentDefinition für die Seite

Für jede Seite, die Sie lokalisieren möchten, können Sie in der `<ContentDefinition>` angeben, welche Sprachressourcen für die einzelnen Sprachcodes gesucht werden sollen.

```XML
<ContentDefinition Id="api.signuporsignin">
      <LocalizedResourcesReferences>
        <LocalizedResourcesReference Language="fr" LocalizedResourcesReferenceId="fr" />
        <LocalizedResourcesReference Language="en" LocalizedResourcesReferenceId="en" />
      </LocalizedResourcesReferences>
</ContentDefinition>
```

In diesem Beispiel werden benutzerdefinierte Zeichenfolgen für Französisch („fr“) und Englisch („en“) zur Unified-Registrierungs- oder -Anmeldeseite hinzugefügt.  Die `LocalizedResourcesReferenceId` für jede `LocalizedResourcesReference` ist identisch mit ihrem Gebietsschema, Sie können aber eine beliebige Zeichenfolge als ID verwenden.  Für jede Sprach- und Seitenkombination müssen Sie wie nachfolgend gezeigt entsprechende `<LocalizedResources>` erstellen.


### <a name="create-the-localizedresources"></a>Erstellen von LocalizedResources

Ihre Außerkraftsetzungen sind in Ihren `<BuildingBlocks>` enthalten, und es gibt `<LocalizedResources>` für jede Seite und die in der `<ContentDefinition>` der jeweiligen Seite angegebene Sprache.  Jede Außerkraftsetzung wird wie im folgenden Beispiel gezeigt als `<LocalizedString>` angegeben:

```XML
<BuildingBlocks>
  <Localization>
    <LocalizedResources Id="en">
      <LocalizedStrings>
        <LocalizedString ElementType="ClaimsProvider" StringId="SignInWithLogonNameExchange">Local Account Sign-in</LocalizedString>
        <LocalizedString ElementType="ClaimType" ElementId="UserId" StringId="DisplayName">Username</LocalizedString>
        <LocalizedString ElementType="ClaimType" ElementId="UserId" StringId="UserHelpText">Username used for signing in.</LocalizedString>
        <LocalizedString ElementType="ClaimType" ElementId="UserId" StringId="PatternHelpText">The username you provided is not valid.</LocalizedString>
        <LocalizedString ElementType="UxElement" StringId="button_signin">Sign In Now</LocalizedString>
        <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfInvalidPassword">Your password is incorrect.</LocalizedString>
      </LocalizedStrings>
    </LocalizedResources>
  </Localization>
</BuildingBlocks>
```

Es gibt vier Arten von Zeichenfolgenelementen auf der Seite:

**ClaimsProvider:** Bezeichnungen für Ihre Identitätsanbieter (Facebook, Google, Azure AD usw.). **ClaimType:** Bezeichnungen für Ihre Attribute und deren entsprechende Hilfetexte, oder Feldprüfungs-Fehlermeldungen **UxElement:** Sonstige Zeichenfolgenelemente auf der Seite, die standardmäßig vorhanden sind, also etwa Schaltflächen, Links oder Text**ErrorMessage:** Formularprüfungs-Fehlermeldungen

Stellen Sie sicher, dass die `StringId`s für die Seite übereinstimmen, auf der Sie diese Außerkraftsetzungen verwenden. Andernfalls nämlich wird sie von der Richtlinienprüfung beim Hochladen blockiert.  