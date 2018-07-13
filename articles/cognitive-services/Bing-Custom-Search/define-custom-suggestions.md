---
title: 'Benutzerdefinierte Bing-Suche: Definieren von Vorschlägen für die benutzerdefinierte Vorschlagssuche | Microsoft-Dokumentation'
description: Konfigurieren der benutzerdefinierten Vorschlagssuche mit benutzerdefinierten Vorschlägen
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.technology: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: a41b4e5b6c268ec68488c6764d4192cf8d2345a4
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35374691"
---
# <a name="configure-your-custom-autosuggest-experience"></a>Konfigurieren der benutzerdefinierten Vorschlagssuche
Wenn Sie das entsprechende Abonnement für die benutzerdefinierte Bing-Suche haben (siehe [Preise](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)), können Sie die Suchvorschläge anpassen, die Ihnen in der benutzerdefinierten Bing-Suche angezeigt werden. Die benutzerdefinierte Vorschlagssuche gibt basierend auf einem Teil der Abfragezeichenfolge, die der Benutzer bereitstellt, eine Liste von vorgeschlagenen Abfragen zurück. Mit der benutzerdefinierten Bing-Vorschlagssuche erhalten Sie benutzerdefinierte Suchvorschläge, die für Ihre Suche relevant sind. Sie bestimmen, ob nur benutzerdefinierte Vorschläge zurückgegeben oder auch Bing-Vorschläge eingeschlossen werden. Wenn Sie Bing-Vorschläge einschließen, werden benutzerdefinierte Vorschläge vor den Bing-Vorschlägen angezeigt. Bing-Vorschläge sind auf Ihre benutzerdefinierte Suchinstanz beschränkt.

## <a name="configure-custom-autosuggest"></a>Konfigurieren der benutzerdefinierten Vorschlagssuche
Befolgen Sie die Anleitung, um die benutzerdefinierte Vorschlagssuche für Ihre benutzerdefinierte Suchinstanz zu konfigurieren.

1.  Melden Sie sich in der [benutzerdefinierten Bing-Suche](https://customsearch.ai) an.
2.  Klicken Sie auf eine benutzerdefinierte Suchinstanz. Weitere Informationen dazu finden Sie unter [Erstellen der ersten Instanz der benutzerdefinierten Bing-Suche](quick-start.md).
3.  Klicken Sie auf die Registerkarte **Vorschlagssuche**.

## <a name="enable-bing-suggestions"></a>Aktivieren der Bing-Vorschläge
Schalten Sie den Schieberegler für **Automatische Bing-Vorschläge** auf die Position EIN um, um Bing-Vorschläge zu aktivieren. Der Schieberegler wird Blau angezeigt.

## <a name="add-suggestions"></a>Hinzufügen von Vorschlägen
Geben Sie einen Vorschlag in das Textfeld ein, um ihn hinzuzufügen. Drücken Sie die Eingabetaste, oder klicken Sie auf das Symbol **+**. Benutzerdefinierte Vorschläge können in jeder beliebigen Sprache eingegeben werden und erscheinen vor den Bing- Vorschlägen.

## <a name="upload-suggestions"></a>Hochladen von Vorschlägen
Sie können eine Liste mit Vorschlägen aus einer Datei hochladen. Platzieren Sie jeden Vorschlag in einer separaten Zeile. Klicken Sie auf das Symbol zum Hochladen, und wählen Sie Ihre Datei aus.

## <a name="remove-suggestions"></a>Entfernen von Vorschlägen
Wenn Sie einen Vorschlag entfernen möchten, klicken Sie neben diesem auf das Symbol zum Entfernen.

[!INCLUDE[publish or revert](./includes/publish-revert.md)]

  >[!NOTE]  
  >Es kann bis zu 24 Stunden dauern, bis Änderungen an der Konfiguration der benutzerdefinierten Vorschlagssuche wirksam werden.

## <a name="next-steps"></a>Nächste Schritte

- [Abrufen von benutzerdefinierten Vorschlägen](./get-custom-suggestions.md)
- [Suchen der benutzerdefinierten Instanz](./search-your-custom-view.md)
- [Konfigurieren und Verwenden der benutzerdefinierten gehosteten Benutzeroberfläche](./hosted-ui.md)