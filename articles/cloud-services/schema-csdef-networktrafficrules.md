---
title: "Azure Cloud Services – Definition NetworkTrafficRules-Schema | Microsoft-Dokumentation"
ms.custom: 
ms.date: 04/14/2015
ms.prod: azure
ms.reviewer: 
ms.service: cloud-services
ms.suite: 
ms.tgt_pltfrm: 
ms.topic: reference
ms.assetid: 351b369f-365e-46c1-82ce-03fc3655cc88
caps.latest.revision: "17"
author: thraka
ms.author: adegeo
manager: timlt
ms.openlocfilehash: 37ec6c771ad83be680d95e9b5597d8da3cace9ce
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-cloud-services-definition-networktrafficrules-schema"></a>Azure Cloud Services – Definition NetworkTrafficRules-Schema
Der `NetworkTrafficRules`-Knoten ist ein optionales Element in der Dienstdefinitionsdatei, das angibt, wie Rollen miteinander kommunizieren. Er begrenzt, welche Rollen auf die internen Endpunkte der bestimmten Rolle zugreifen können. Der `NetworkTrafficRules`-Knoten ist kein eigenständiges Element; er wird mit mindestens zwei Rollen in einer Dienstdefinitionsdatei kombiniert.

Die Standarderweiterung für die Dienstdefinitionsdatei lautet „.csdef“.

> [!NOTE]
>  Der `NetworkTrafficRules`-Knoten ist nur mit Azure SDK Version 1.3 und höher verfügbar.

## <a name="basic-service-definition-schema-for-the-network-traffic-rules"></a>Basisdienstdefinitions-Schema für die Netzwerkverkehrs-Regeln
Das Standardformat einer Dienstdefinitionsdatei, die Netzwerkverkehrs-Definitionen enthält, lautet wie folgt.

```xml
<ServiceDefinition …>
   <NetworkTrafficRules>
      <OnlyAllowTrafficTo>
         <Destinations>
            <RoleEndpoint endpointName="<name-of-the-endpoint>" roleName="<name-of-the-role-containing-the-endpoint>"/>
         </Destinations>
         <AllowAllTraffic/>
         <WhenSource matches="[AnyRule]">
            <FromRole roleName="<name-of-the-role-to-allow-traffic-from>"/>
         </WhenSource>
      </OnlyAllowTrafficTo>
   </NetworkTrafficRules>
</ServiceDefinition>
```

## <a name="schema-elements"></a>Schema-Elemente
Der `NetworkTrafficRules`-Knoten der Dienstdefinitionsdatei enthält die folgenden Elemente, die in den folgenden Abschnitten in diesem Thema ausführlich beschrieben werden:

[NetworkTrafficRules-Element](#NetworkTrafficRules)

[OnlyAllowTrafficTo-Element](#OnlyAllowTrafficTo)

[Destinations-Element](#Destinations)

[RoleEndpoint-Element](#RoleEndpoint)

[AllowAllTraffic-Element](#AllowAllTraffic)

[WhenSource-Element](#WhenSource)

[FromRole-Element](#FromRole)

##  <a name="NetworkTrafficRules"></a>NetworkTrafficRules-Element
Das `NetworkTrafficRules`-Element gibt an, welche Rollen mit welchem Endpunkt einer anderen Rolle kommunizieren können. Ein Dienst kann eine einzelne `NetworkTrafficRules`-Definition enthalten.

##  <a name="OnlyAllowTrafficTo"></a>OnlyAllowTrafficTo-Element
Das `OnlyAllowTrafficTo`-Element beschreibt eine Sammlung der Zielendpunkte und der Rollen, die mit ihnen kommunizieren können. Sie können mehrere Feldzuordnungen `OnlyAllowTrafficTo`-Knoten angeben.

##  <a name="Destinations"></a>Destinations-Element
Das `Destinations`-Element beschreibt eine Sammlung von RoleEndpoints, mit denen kommuniziert werden kann.

##  <a name="RoleEndpoint"></a> RoleEndpoint-Element
Das `RoleEndpoint`-Element beschreibt einen Endpunkt einer Rolle, um die Kommunikation mit ihm zuzulassen. Sie können mehrere `RoleEndpoint`-Elemente angeben, wenn die Rolle mehrere Endpunkte aufweist.

| Attribut      | Typ     | Beschreibung |
| -------------- | -------- | ----------- |
| `endpointName` | `string` | Erforderlich. Der Name des Endpunkts, mit dem Datenverkehr zugelassen ist.|
| `roleName`     | `string` | Erforderlich. Der Name der Webrolle, mit der Kommunikation zugelassen ist.|

## <a name="allowalltraffic-element"></a>AllowAllTraffic-Element
Das `AllowAllTraffic`-Element ist eine Regel, die allen Rollen die Kommunikation mit den im `Destinations`-Knoten definierten Endpunkten ermöglicht.

##  <a name="WhenSource"></a> WhenSource-Element
Das `WhenSource`-Element beschreibt eine Sammlung von Rollen, die mit den im `Destinations`-Knoten definierten Endpunkten kommunizieren können.

| Attribut | Typ     | Beschreibung |
| --------- | -------- | ----------- |
| `matches` | `string` | Erforderlich. Gibt die Regel an, die beim Zulassen von Kommunikation angewendet werden soll. Derzeit ist `AnyRule` der einzige gültige Wert.|
  
##  <a name="FromRole"></a> FromRole-Element
Das `FromRole`-Element gibt die Rollen an, die mit den im `Destinations`-Knoten definierten Endpunkten kommunizieren können. Sie können mehrere `FromRole`-Elemente angeben, wenn mehrere Rollen mit den Endpunkten kommunizieren können.

| Attribut  | Typ     | Beschreibung |
| ---------- | -------- | ----------- |
| `roleName` | `string` | Erforderlich. Der Name für die Rolle, von der aus Kommunikation zugelassen ist.|

## <a name="see-also"></a>Weitere Informationen
[Clouddienst-Definitionsschema (klassisch)](schema-csdef-file.md)