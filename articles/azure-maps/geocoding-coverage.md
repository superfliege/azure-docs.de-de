---
title: Geocodierungsabdeckung in Azure Maps | Microsoft-Dokumentation
description: Erfahren Sie mehr über Geocodierungsabdeckung in Azure Maps
author: dsk-2015
ms.author: dkshir
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 664d52a96f1e724502f82f5f72567f87c74e716b
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52444255"
---
# <a name="azure-maps-geocoding-coverage"></a>Geocodierungsabdeckung in Azure Maps

Bei der Suche nach einem Ort mit Azure Maps nimmt der Suchdienst Ihre Suchbegriffe entgegen und gibt die Koordinaten (Breiten- und Längengrad) zurück – dieser Prozess wird als „Geocodierung“ bezeichnet. Maps bietet allerdings nicht für alle Regionen und Länder das gleiche Maß an Informationen und Genauigkeit. Bestimmen Sie anhand dieses Artikels, nach welcher Art von Orten Sie zuverlässig in den einzelnen Regionen suchen können. 

Die Möglichkeit der Geocodierung in einem Land hängt von der Abdeckung der Straßendaten und der Geocodierungsgenauigkeit des Geocodierungsdiensts ab. Die jeweilige Unterstützung der Geocodierung in den einzelnen Ländern wird mit den folgenden Kategorisierungen angegeben.
* **Adresspunkte:** Adressendaten können innerhalb des Adresspakets (Eigenschaftsgrenze) in eine Längen- und Breitenkoordinate aufgelöst werden. Dies wird manchmal als „hausgenau“ bezeichnet. Dies ist die höchste verfügbare Genauigkeit für Adressen. 
* **Hausnummern:** Adressen werden in eine Längen- und Breitenkoordinate in der Straße interpoliert.
* **Straße:** Adressen werden in die Längen- und Breitenkoordinate der Straße mit der Adresse aufgelöst. Die Hausnummer wird möglicherweise nicht verarbeitet.
* **Ort:** Ortsnamen werden nicht unterstützt.

## <a name="americas"></a>Amerika

| Region             | Adresspunkte | Hausnummern | Straße | Ort | POIs |
|-----------------------------------------------------|:---------------:|:--------------:|:------------:|:----------:|:------------------:|
| Anguilla                                            |                 |                |              |      ✓     |          ✓         |
| Antarktis                                          |                 |                |              |      ✓     |          ✓         |
| Antigua und Barbuda                                 |                 |                |       ✓      |      ✓     |          ✓         |
| Argentinien                                           |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Aruba                                               |                 |                |              |      ✓     |          ✓         |
| Bahamas                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Barbados                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Belize                                              |                 |                |              |      ✓     |          ✓         |
| Bermudas                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Bolivien                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Bonaire, Sint Eustatius und Saba|                 |                |              |      ✓     |          ✓         |
| Brasilien                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Kanada                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Kaimaninseln                                      |                 |                |       ✓      |      ✓     |          ✓         |
| Chile                                               |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Clippertoninsel                                   |                 |                |              |      ✓     |                    |
| Kolumbien                                            |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Costa Rica                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Kuba                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Dominica                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Dominikanische Republik                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Ecuador                                             |                 |                |       ✓      |      ✓     |          ✓         |
| El Salvador                                         |                 |                |       ✓      |      ✓     |          ✓         |
| Falklandinseln                                    |                 |                |              |      ✓     |          ✓         |
| Französisch-Guayana                                       |                 |                |       ✓      |      ✓     |          ✓         |
| Grenada                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Guadeloupe|                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Guam                                                |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Guatemala                                           |                 |                |       ✓      |      ✓     |          ✓         |
| Guayana        |                |             |           |      ✓     |                 |
| Haiti                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Honduras                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Jamaika                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Martinique                                          |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Mexiko                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Montserrat                                          |                 |                |              |      ✓     |          ✓         |
| Nicaragua                                           |                 |                |       ✓      |      ✓     |          ✓         |
| Panama                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Paraguay                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Peru                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Puerto Rico                                         |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| St. Barthélemy                                    |                 |                |       ✓      |      ✓     |          ✓         |
| St. Kitts und Nevis                               |                 |                |       ✓      |      ✓     |          ✓         |
| St. Lucia                                         |                 |                |              |      ✓     |          ✓         |
| St. Martin                                        |                 |                |       ✓      |      ✓     |          ✓         |
| St. Pierre und Miquelon|                 |                |       ✓      |      ✓     |          ✓         |
| St. Vincent und die Grenadinen                    |                 |                |              |      ✓     |          ✓         |
| Sint Maarten                                        |                 |                |       ✓      |      ✓     |          ✓         |
| Südgeorgien und Südliche Sandwichinseln        |                 |                |              |      ✓     |          ✓         |
| Surinam                                            |                 |                |              |      ✓     |          ✓         |
| Trinidad und Tobago                                 |                 |                |       ✓      |      ✓     |          ✓         |
| Kleinere Amerikanische Überseeinseln                |                 |                |              |      ✓     |          ✓         |
| Vereinigte Staaten von Amerika                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Uruguay                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Venezuela                                           |                 |                |       ✓      |      ✓     |          ✓         |
| Britische Jungferninseln                            |                 |                |              |      ✓     |          ✓         |
| Amerikanische Jungferninseln                      |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |

## <a name="asia-pacific"></a>Asien-Pazifik

| Region             | Adresspunkte |Hausnummern | Straße | Ort | POIs |
|-----------------------------------------------------|:---------------:|:--------------:|:------------:|:----------:|:------------------:|
| Amerikanisch-Samoa                                      |                 |                |       ✓      |      ✓     |          ✓         |
| Australien                                           |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Bangladesch                                          |                 |                |              |      ✓     |          ✓         |
| Bhutan                                              |                 |                |              |      ✓     |          ✓         |
| Britisches Territorium im Indischen Ozean                      |                 |                |              |      ✓     |          ✓         |
| Brunei                                              |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Kambodscha                                            |                 |                |              |      ✓     |          ✓         |
| China |                 |                |              |      ✓     |          ✓         |
| Weihnachtsinsel                                    |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Kokosinseln|                 |                |              |      ✓     |          ✓         |
| Komoren|                 |                |              |      ✓     |          ✓         |
| Cookinseln                                        |                 |                |              |      ✓     |          ✓         |
| Fidschi |                 |                |              |      ✓     |          ✓         |
| Französisch-Polynesien                                    |                 |                |              |      ✓     |          ✓         |
| Heard und McDonaldinseln                   |                 |                |              |      ✓     |          ✓         |
| Hongkong                                           |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Indonesien                                           |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Indien       |        ✓        |        ✓       |       ✓      |      ✓     |                   |
| Japan                                               |                 |                |              |      ✓     |          ✓         |
| Kiribati                                            |                 |                |              |      ✓     |          ✓         |
| Laos                                                |                 |                |              |      ✓     |          ✓         |
| Macau (SAR)                                               |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Malaysia                                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Mikronesien |                 |                |              |      ✓     |          ✓         |
| Mongolei                                            |                 |                |              |      ✓     |          ✓         |
| Nauru                                               |                 |                |              |      ✓     |          ✓         |
| Nepal|                 |                |              |      ✓     |          ✓         |
| Neukaledonien                                       |                 |                |              |      ✓     |          ✓         |
| Neuseeland                                         |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Niue                                                |                 |                |              |      ✓     |          ✓         |
| Norfolkinsel                                      |                 |                |              |      ✓     |          ✓         |
| Nordkorea                                         |                 |                |              |      ✓     |          ✓         |
| Nördliche Marianen                            |                 |                |       ✓      |      ✓     |          ✓         |
| Pakistan                                            |                 |                |              |      ✓     |          ✓         |
| Palau |                 |                |              |      ✓     |          ✓         |
| Papua-Neuguinea                                    |                 |                |              |      ✓     |          ✓         |
| Paracel-Inseln                                     |                 |                |              |      ✓     |                    |
| Philippinen                                         |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Pitcairninseln                                            |                 |                |              |      ✓     |          ✓         |
| Samoa                                               |                 |                |              |      ✓     |          ✓         |
| Senkaku-Inseln/Diaoyutai-Inseln                                     |        ✓        |                |              |      ✓     |          ✓         |
| Singapur                                           |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Salomonen                                     |                 |                |              |      ✓     |          ✓         |
| Südkorea                                         |                 |                |              |      ✓     |          ✓         |
| Südliche Kurilen                                     |        ✓        |                |              |      ✓     |          ✓         |
| Spratly-Inseln                                     |                 |                |              |      ✓     |                    |
| Sri Lanka                                           |                 |                |              |      ✓     |          ✓         |
| Taiwan                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Thailand                                            |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Tokelau                                             |                 |                |              |      ✓     |          ✓         |
| Tonga                                               |                 |                |              |      ✓     |          ✓         |
| Turks- und Caicosinseln                            |                 |                |              |      ✓     |          ✓         |
| Tuwalu                                              |                 |                |              |      ✓     |          ✓         |
| Vanuatu                                             |                 |                |              |      ✓     |          ✓         |
| Vietnam                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Wallis und Futuna|                 |                |              |      ✓     |          ✓         |

## <a name="europe"></a>Europa

| Region             | Adresspunkte |Hausnummern | Straße | Ort | POIs |
|-----------------------------------------------------|:---------------:|:--------------:|:------------:|:----------:|:------------------:|
| Albanien                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Andorra                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Armenien                                             |                 |                |              |      ✓     |          ✓         |
| Österreich                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Aserbaidschan                                          |                 |                |              |      ✓     |          ✓         |
| Belgien                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Bosnien und Herzegowina                              |                 |                |       ✓      |      ✓     |          ✓         |
| Bulgarien                                            |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Belarus|                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Kroatien                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Zypern                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Tschechische Republik                                      |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Dänemark                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Estland                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Färöer                                       |                 |                |              |      ✓     |          ✓         |
| Finnland                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Frankreich                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Georgien                                             |                 |                |              |      ✓     |          ✓         |
| Deutschland                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Gibraltar                                           |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Griechenland                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Grönland                                           |                 |                |              |      ✓     |          ✓         |
| Guernsey                                            |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Ungarn                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Island                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Irland                               |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Isle of Man                                         |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Italien                                               |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Jan Mayen                                           |        ✓        |                |              |      ✓     |          ✓         |
| Jersey                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Kasachstan                                          |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Kosovo                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Kirgisistan                                          |                 |                |              |      ✓     |          ✓         |
| Lettland                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Liechtenstein                                       |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Litauen                                           |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Luxemburg                                          |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Mazedonien (ehemalige jugoslawische Republik)                                           |                 |                |       ✓      |      ✓     |          ✓         |
| Malta                                               |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Moldau                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Monaco                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Montenegro                                          |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Niederlande                                         |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Norwegen                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Polen                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Portugal                                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Rumänien                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Russische Föderation                                  |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| San Marino                                          |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Serbien                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Slowakei                                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Slowenien                                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Spanien                                               |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Spitzbergen                                            |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Schweden                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Schweiz                                         |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Tadschikistan                                          |                 |                |              |      ✓     |          ✓         |
| Türkei                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Turkmenistan                                        |                 |                |              |      ✓     |          ✓         |
| Ukraine                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Vereinigtes Königreich                                      |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Usbekistan                                          |                 |                |              |      ✓     |          ✓         |
| Vatikanstadt                                        |                 |                |       ✓      |      ✓     |          ✓         |


## <a name="middle-east-and-africa"></a>Naher Osten und Afrika

| Region             | Adresspunkte |Hausnummern | Straße | Ort | POIs |
|-----------------------------------------------------|:---------------:|:--------------:|:------------:|:----------:|:------------------:|
| Afghanistan                                         |                 |                |              |      ✓     |          ✓         |
| Algerien                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Angola                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Bahrain                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Benin                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Botsuana                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Bouvetinsel                                       |                 |                |              |      ✓     |          ✓         |
| Burkina Faso                                        |                 |                |       ✓      |      ✓     |          ✓         |
| Burundi                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Kamerun                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Cabo Verde|                 |                |       ✓      |      ✓     |          ✓         |
| Zentralafrikanische Republik                            |                 |                |       ✓      |      ✓     |          ✓         |
| Tschad                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Kongo (Demokratische Republik)                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Côte d'Ivoire                                       |                 |                |       ✓      |      ✓     |          ✓         |
| Demokratische Republik Kongo                        |                 |                |       ✓      |      ✓     |          ✓         |
| Dschibuti                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Ägypten                                               |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Republik Äquatorialguinea                      |                 |                |       ✓      |      ✓     |          ✓         |
| Eritrea                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Äthiopien                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Französische Gebiete im südlichen Indischen Ozean|                 |                |              |      ✓     |          ✓         |
| Gabun                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Gambia                                              |                 |                |              |      ✓     |          ✓         |
| Ghana                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Guinea                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Guinea-Bissau                                       |                 |                |       ✓      |      ✓     |          ✓         |
| Iran                                                |                 |                |              |      ✓     |          ✓         |
| Irak                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Israel                                              |                 |                |              |      ✓     |          ✓         |
| Jordan                                              |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Kenia                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Kuwait                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Libanon                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Lesotho                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Liberia                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Libyen|                 |                |       ✓      |      ✓     |          ✓         |
| Madagaskar                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Malawi                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Malediven |                 |                |              |      ✓     |          ✓         |
| Mali                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Marshallinseln |                 |                |              |      ✓     |          ✓         |
| Mauretanien                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Mauritius                                           |                 |                |       ✓      |      ✓     |          ✓         |
| Mayotte                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Marokko                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Mosambik                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Myanmar                                             |                 |                |              |      ✓     |          ✓         |
| Namibia                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Niger                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Nigeria                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Oman                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Katar                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Réunion|                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Ruanda                                              |                 |                |       ✓      |      ✓     |          ✓         |
| St. Helena                                        |                 |                |              |      ✓     |          ✓         |
| Saudi-Arabien                                        |                 |                |       ✓      |      ✓     |          ✓         |
| Senegal                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Seychellen                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Sierra Leone                                        |                 |                |       ✓      |      ✓     |          ✓         |
| Somalia                                             |                 |                |              |      ✓     |          ✓         |
| Südafrika                                        |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Südsudan                                         |                 |                |       ✓      |      ✓     |          ✓         |
| Sudan                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Swasiland                                           |                 |                |       ✓      |      ✓     |          ✓         |
| Syrien                                               |                 |                |              |      ✓     |          ✓         |
| Demokratische Republik São Tomé und Príncipe       |                 |                |       ✓      |      ✓     |          ✓         |
| Tansania                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Togo                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Tunesien                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Uganda                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Vereinigte Arabische Emirate                                |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Westjordanland                                           |                 |                |              |      ✓     |          ✓         |
| Jemen                                               |                 |                |              |      ✓     |          ✓         |
| Sambia                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Simbabwe                                            |                 |                |       ✓      |      ✓     |          ✓         |



## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure Maps-Geocodierung finden Sie auf den [Suchen](https://docs.microsoft.com/rest/api/maps/search)-Referenzseiten.

Erfahren Sie mehr über die [Abdeckungsbereiche des Verkehrsinfodiensts von Maps](traffic-coverage.md). 

