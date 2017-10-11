---
title: Verwendung von Eigenschaften in Azure API Management-Richtlinien
description: Erfahren Sie, wie Sie Eigenschaften in Azure API Management-Richtlinien zu verwenden.
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 6f39b00f-cf6e-4cef-9bf2-1f89202c0bc0
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 3b0fe2a300038e13cc488bdb4f50f8be270ea8f4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-properties-in-azure-api-management-policies"></a>Verwendung von Eigenschaften in Azure API Management-Richtlinien
API Management-Richtlinien sind eine leistungsstarke Funktion des Systems, das den Verleger, um das Verhalten der API über Konfiguration ändern können. Richtlinien sind eine Auflistung von Anweisungen, die sequenziell bei Anfragen ausgeführt werden oder Antworten einer API. Richtlinienanweisungen können mithilfe von literalen Textwerte, Richtlinienausdrücke und Eigenschaften erstellt werden. 

Jede API Management-Dienstinstanz verfügt über eine Properties-Auflistung von Schlüssel/Wert-Paaren, die global für die Dienstinstanz werden. Diese Eigenschaften können Konstantenzeichenfolge Werte in allen-API-Konfiguration und Richtlinien verwalten verwendet werden. Jede Eigenschaft verfügt über die folgenden Attribute.

| Attribut | Typ | Beschreibung |
| --- | --- | --- |
| Name |string |Der Name der Eigenschaft. Er kann nur Buchstaben, Ziffern, Punkt, Strich enthalten und Unterstriche enthalten. |
| Wert |string |Der Wert der Eigenschaft. Es darf nicht leer sein oder nur aus Leerzeichen bestehen. |
| Geheim |boolean |Bestimmt, ob der Wert einen geheimen Schlüssel und verschlüsselt oder nicht verschlüsselt werden soll. |
| Tags |Array von Zeichenfolgen |Optional, tags, die bei der zum Filtern der Eigenschaftenliste verwendet werden können. |

Eigenschaften werden im Portal Verleger konfiguriert, auf die **Eigenschaften** Registerkarte. Im folgenden Beispiel werden drei Eigenschaften konfiguriert.

![Eigenschaften][api-management-properties]

Eigenschaftswerte können Literalzeichenfolgen enthalten und [Richtlinienausdrücke](https://msdn.microsoft.com/library/azure/dn910913.aspx). Die folgende Tabelle zeigt die vorherigen drei Eigenschaften und ihre Attribute. Der Wert des `ExpressionProperty` ist ein Richtlinienausdruck, der eine Zeichenfolge mit dem aktuellen Datum und Uhrzeit zurückgibt. Die Eigenschaft `ContosoHeaderValue` wird als ein geheimer Schlüssel markiert, sodass der Wert nicht angezeigt wird.

| Name | Wert | Geheim | Tags |
| --- | --- | --- | --- |
| ContosoHeader |TrackingId |False |Contoso |
| ContosoHeaderValue |•••••••••••••••••••••• |True |Contoso |
| ExpressionProperty |@(DateTime.Now.ToString()) |False | |

## <a name="to-use-a-property"></a>Um eine Eigenschaft zu verwenden
Um eine Eigenschaft in einer Richtlinie zu verwenden, platzieren Sie den Namen der Eigenschaft innerhalb eines doppelten Paares geschweifter Klammern wie `{{ContosoHeader}}`, wie im folgenden Beispiel gezeigt.

```xml
<set-header name="{{ContosoHeader}}" exists-action="override">
  <value>{{ContosoHeaderValue}}</value>
</set-header>
```

In diesem Beispiel `ContosoHeader` dient als den Namen eines Headers in einer `set-header` Richtlinie und `ContosoHeaderValue` als der Wert dieses Headers verwendet wird. Wenn diese Richtlinie, während einer Anforderung oder Antwort zum API Management-Gateway ausgewertet wird `{{ContosoHeader}}` und `{{ContosoHeaderValue}}` werden durch ihre jeweiligen Eigenschaftswerte ersetzt.

Eigenschaften können als vollständige Attribut- oder Elementwerten wie im vorherigen Beispiel gezeigt verwendet werden, aber sie können auch eingefügt oder in Kombination mit Teil eines Ausdrucks wörtlichen Text wie im folgenden Beispiel gezeigt:`<set-header name = "CustomHeader{{ContosoHeader}}" ...>`

Eigenschaften können auch Richtlinienausdrücke enthalten. Im folgenden Beispiel die `ExpressionProperty` verwendet wird.

```xml
<set-header name="CustomHeader" exists-action="override">
    <value>{{ExpressionProperty}}</value>
</set-header>
```

Wenn diese Richtlinie ausgewertet wird, `{{ExpressionProperty}}` wird mit dem Wert ersetzt: `@(DateTime.Now.ToString())`. Weil der Wert eines Ausdrucks für die Richtlinie ist, der Ausdruck wird ausgewertet, und die Richtlinie die Ausführung wird fortgesetzt.

Sie können diese Out im Entwicklerportal testen, durch Aufrufen eines Vorgangs, das im Gültigkeitsbereich über eine Richtlinie mit Eigenschaften verfügt. Im folgenden Beispiel wird ein Vorgang mit den beiden vorherigen Beispiel aufgerufen `set-header` Richtlinien mit Eigenschaften. Beachten Sie, dass die Antwort zwei benutzerdefinierte Header enthält, die mithilfe von Richtlinien mit Eigenschaften konfiguriert wurden.

![Entwicklerportal][api-management-send-results]

Bei Betrachtung der [Inspektor-API-Ablaufverfolgung](api-management-howto-api-inspector.md) für einen Aufruf, der die beiden vorherigen Beispielrichtlinien mit Eigenschaften enthält, sehen Sie die beiden `set-header` Richtlinien mit den Eigenschaftswerten, die eingefügt werden, sowie die Auswertung von Ausdrücken Richtlinie für die Eigenschaft, die den Richtlinienausdruck enthalten.

![Inspector-API-Ablaufverfolgung][api-management-api-inspector-trace]

Beachten Sie, dass während der Eigenschaftswerte Richtlinienausdrücke enthalten können, Eigenschaftswerte andere Eigenschaften enthalten können. Wenn Text mit Verweis auf eine Eigenschaft für den Wert einer Eigenschaft, z. B. verwendet wird `Property value text {{MyProperty}}`, Referenz zur Eigenschaft wird nicht ersetzt werden und wird als Teil des Eigenschaftswerts.

## <a name="to-create-a-property"></a>So erstellen Sie eine Eigenschaft
Um eine Eigenschaft zu erstellen, klicken Sie auf **Eigenschaft hinzufügen** auf die **Eigenschaften** Registerkarte.

![Eigenschaft hinzufügen][api-management-properties-add-property-menu]

**Namen** und **Wert** Werte erforderlich sind. Wenn dieser Eigenschaftswert einen geheimen Schlüssel ist, überprüfen Sie die **Dies ist ein geheimer Schlüssel** Kontrollkästchen. Geben Sie eine oder mehrere optionale Tags helfen beim Organisieren Ihrer Eigenschaften, und klicken Sie auf **speichern**.

![Eigenschaft hinzufügen][api-management-properties-add-property]

Wenn eine neue Eigenschaft gespeichert wird, die **Sucheigenschaftenliste** Textfeld mit dem Namen der neuen Eigenschaft aufgefüllt wird und die neue Eigenschaft angezeigt wird. Um alle Eigenschaften anzuzeigen, deaktivieren Sie die **Sucheigenschaftenliste** Textfeld, und drücken Sie EINGABETASTE.

![Eigenschaften][api-management-properties-property-saved]

Informationen zum Erstellen einer Eigenschaft mithilfe der REST-API finden Sie unter [erstellen Sie eine Eigenschaft, die mithilfe der REST-API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).

## <a name="to-edit-a-property"></a>Um eine Eigenschaft zu bearbeiten.
Um eine Eigenschaft zu bearbeiten, klicken Sie auf **bearbeiten** neben der Eigenschaft zu bearbeiten.

![Bearbeiten der Eigenschaft][api-management-properties-edit]

Stellen Sie die gewünschten Änderungen vor, und klicken Sie auf **speichern**. Wenn Sie den Namen der Eigenschaft ändern, werden alle Richtlinien, die diese Eigenschaft auf automatisch aktualisiert, um den neuen Namen zu verwenden.

![Bearbeiten der Eigenschaft][api-management-properties-edit-property]

Informationen zum Bearbeiten einer Eigenschaft mithilfe der REST-API finden Sie unter [Bearbeiten einer Eigenschaft mithilfe der REST-API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).

## <a name="to-delete-a-property"></a>So löschen Sie eine Eigenschaft
Um eine Eigenschaft zu löschen, klicken Sie auf **löschen** neben der Eigenschaft zu löschen.

![Löschen Sie die Eigenschaft][api-management-properties-delete]

Klicken Sie auf **Ja, löschen Sie ihn** zu bestätigen.

![Löschen bestätigen][api-management-delete-confirm]

> [!IMPORTANT]
> Wenn die Eigenschaft keine Richtlinien verwiesen wird, werden Sie möglicherweise nicht erfolgreich gelöscht, bis Sie die Eigenschaft aller Richtlinien entfernen, die sie verwenden.
> 
> 

Informationen zum Löschen einer Eigenschaft mithilfe der REST-API finden Sie unter [Löschen einer Eigenschaft mithilfe der REST-API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).

## <a name="to-search-and-filter-properties"></a>Zur Suche und Filterung Eigenschaften
Die **Eigenschaften** Registerkarte enthält Such- und Filterfunktionen helfen Ihnen beim Verwalten Ihrer Eigenschaften. Die Eigenschaftenliste anhand des Namens der Eigenschaft filtern, geben Sie einen Suchbegriff in das **Sucheigenschaftenliste** Textfeld. Um alle Eigenschaften anzuzeigen, deaktivieren Sie die **Sucheigenschaftenliste** Textfeld, und drücken Sie EINGABETASTE.

![Suchen][api-management-properties-search]

Um die Eigenschaftenliste nach Tag-Werte zu filtern, geben Sie ein oder mehrere Tags in der **nach Tags filtern** Textfeld. Um alle Eigenschaften anzuzeigen, deaktivieren Sie die **nach Tags filtern** Textfeld, und drücken Sie EINGABETASTE.

![Filter][api-management-properties-filter]

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Arbeiten mit Richtlinien
  * [Richtlinien in der API-Verwaltung](api-management-howto-policies.md)
  * [Richtlinienreferenz](https://msdn.microsoft.com/library/azure/dn894081.aspx)
  * [Richtlinienausdrücke](https://msdn.microsoft.com/library/azure/dn910913.aspx)

## <a name="watch-a-video-overview"></a>Sehen Sie sich ein video-Überblick
> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Use-Properties-in-Policies/player]
> 
> 

[api-management-properties]: ./media/api-management-howto-properties/api-management-properties.png
[api-management-properties-add-property]: ./media/api-management-howto-properties/api-management-properties-add-property.png
[api-management-properties-edit-property]: ./media/api-management-howto-properties/api-management-properties-edit-property.png
[api-management-properties-add-property-menu]: ./media/api-management-howto-properties/api-management-properties-add-property-menu.png
[api-management-properties-property-saved]: ./media/api-management-howto-properties/api-management-properties-property-saved.png
[api-management-properties-delete]: ./media/api-management-howto-properties/api-management-properties-delete.png
[api-management-properties-edit]: ./media/api-management-howto-properties/api-management-properties-edit.png
[api-management-delete-confirm]: ./media/api-management-howto-properties/api-management-delete-confirm.png
[api-management-properties-search]: ./media/api-management-howto-properties/api-management-properties-search.png
[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png

