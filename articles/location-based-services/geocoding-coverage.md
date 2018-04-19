---
title: Geocodierung in Azure Location Based Services | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über die Geocodierung in Azure Location Based Services.
services: location-based-services
keywords: ''
author: dsk-2015
ms.author: dkshir
ms.date: 11/28/2017
ms.topic: article
ms.service: location-based-services
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: b2ae0f2cb8eba6ca42b3c82458d1fadf4ea93cdf
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2018
---
# <a name="azure-location-based-services---geocoding-coverage"></a>Azure Location Based Services: Umfang der Geocodierung
Azure Location Based Services (LBS) bietet ausführliche Geocodierungsinformationen für die weltweite Suche nach Adressen, Orten und anderen geografischen Einheiten. Dies ist wichtig für Kunden, die wissen möchten, wo sie bei der Adresssuche im Suchdienst unterschiedliche Genauigkeiten erwarten können und wo nicht. Bei der Suche in Regionen mit geringer Genauigkeit können Sie keine Suchergebnisse mit geringerer Zuverlässigkeit erwarten. Die folgende Tabelle enthält Informationen zum Umfang der Geocodierung für die Azure LBS-Suche:

| Region             | Adresspunkte<sup>1</sup>|Hausnummern<sup>2</sup>| Straße | Ort | POIs |
|-----------------------------------------------------|:---------------:|:--------------:|:------------:|:----------:|:------------------:|
| **Amerika**                                            |                 |                |              |            |                    |
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
| |                 |                |              |            |                    |
| **Asien-Pazifik**                                        |                 |                |              |            |                    |
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
| |                 |                |              |            |                    |
| **Europa**                                              |                 |                |              |            |                    |
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
| |                 |                |              |            |                    |
| **Naher Osten/Afrika**                                  |                 |                |              |            |                    |
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


<br>
<br>




|Begriffe|Definitionen|
|--- |--- |
|**<sup>1</sup> – Adresspunkt**|Wird vom Punktadressindex in der Onlinesuche abgeleitet. Dieses Feld wird nur unterstützt, wenn Suche und Geocodierung unterstützt werden.|
|**<sup>2</sup> – Hausnummer**|Wird werden vom Adressinterpolationsindex in der Onlinesuche abgeleitet. Dieses Feld wird nur unterstützt, wenn Suche und Geocodierung unterstützt werden.|
|**<sup>3</sup> – Vom Benutzer zugewiesener Code**|Spezifischer Code für die Onlinesuche, kein offizieller Code vom Typ ISO 3166-1.|
