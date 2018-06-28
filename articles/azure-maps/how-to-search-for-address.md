---
title: Suchen nach einer Adresse mit dem Suchdienst von Azure Maps | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit dem Suchdienst von Azure Maps nach einer Adresse suchen.
author: kgremban
ms.author: kgremban
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 1acb95af7b62641c371627d6250067f9c2eac99c
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2018
ms.locfileid: "36323635"
---
# <a name="how-to-find-an-address-using-the-azure-maps-search-service"></a>Suchen nach einer Adresse mit dem Suchdienst von Azure Maps

Bei dem Maps-Suchdienst handelt es sich um eine für Entwickler konzipierte Gruppe von RESTful-APIs für die Suche nach Adressen, Orten, interessanten Orten, Brancheneinträgen und anderen geografischen Informationen. Der Dienst weist einer bestimmten Adresse, einer Querstraße, einem geografischen Objekt oder einem interessanten Ort (POI) einen Breiten- und Längengrad zu. Die von der Suche zurückgegebenen Werte für den Breiten- und Längengrad können als Parameter in anderen Maps-Diensten verwendet werden, z.B. für Routen oder Verkehrsfluss.

## <a name="prerequisites"></a>Voraussetzungen

Um die Maps-Dienst-APIs aufrufen zu können, benötigen Sie ein Maps-Konto mit zugehörigem Schlüssel. Informationen zum Erstellen eines Kontos und Abrufen eines Schlüssels finden Sie unter [Verwalten Ihres Azure Maps-Kontos und der dazugehörigen Schlüssel](how-to-manage-account-keys.md).

In diesem Artikel wird die [Postman-App](https://www.getpostman.com/apps) zum Erstellen von REST-Aufrufen verwendet. Sie können jedoch auch Ihre bevorzugte API-Entwicklungsumgebung verwenden. 


## <a name="using-fuzzy-search"></a>Verwenden der Fuzzysuche

Die Standard-API für den Suchdienst ist die [Fuzzysuche](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy), die Eingaben jeder beliebigen Kombination aus Adressen oder POI-Token verarbeitet. Bei dieser Such-API handelt es sich um die kanonische „einzeilige Suche“. Sie empfiehlt sich, wenn Sie nicht wissen, welche Suchabfragen Benutzer eingeben. Die API für die Fuzzysuche ist eine Kombination aus POI-Suche und Geocodierung. Die API kann auch mit einer kontextabhängigen Position (Breitengrad-Längengrad-Paar) gewichtet werden, die durch eine Koordinate oder einen Radius vollständig eingeschränkt ist. Oder sie kann allgemeiner ohne jeglichen geografiebezogenen Ankerpunkt ausgeführt werden.

Die meisten Suchabfragen sind zur Leistungssteigerung und Verringerung ungewöhnlicher Ergebnisse standardmäßig auf „maxFuzzyLevel=1“ festgelegt. Diese Standardeinstellung kann bei Bedarf pro Anforderung durch Übergeben des Abfrageparameters „maxFuzzyLevel=2“ oder „maxFuzzyLevel=3“ überschrieben werden.

### <a name="search-for-an-address-using-fuzzy-search"></a>Suchen nach einer Adresse mithilfe der Fuzzysuche

1. Öffnen Sie die Postman-App, klicken Sie auf „New“ (Neu) und „Create New“ (Neu erstellen), und wählen Sie **GET request** (GET-Anforderung) aus. Geben Sie **Fuzzy search** (Fuzzysuche) als Name der Anforderung ein, wählen Sie eine Sammlung oder einen Ordner aus, in der bzw. dem die Anforderung gespeichert wird, und klicken Sie auf **Save** (Speichern).

2. Wählen Sie auf der Registerkarte „Builder“ die HTTP-Methode **GET** aus, und geben Sie die Anforderungs-URL für Ihren API-Endpunkt ein.

    ![Fuzzysuche ](./media/how-to-search-for-address/fuzzy_search_url.png)

    | Parameter | Empfohlener Wert |
    |---------------|------------------------------------------------|
    | HTTP-Methode | GET |
    | Anfrage-URL | https://atlas.microsoft.com/search/fuzzy/json? |
    | Autorisierung | No Auth |

    Das Attribut **json** im URL-Pfad legt das Antwortformat fest. In diesem Artikel wird zur einfachen Handhabung und besseren Lesbarkeit durchgängig JSON verwendet. Sie finden die verfügbaren Antwortformate in der Definition für **Get Search Fuzzy** der [Referenz zur funktionalen API von Maps] (https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy).

3. Klicken Sie auf **Params** (Parameter), und geben Sie die folgenden Schlüssel-Wert-Paare zur Verwendung als Abfrage- oder Pfadparameter in der Anforderungs-URL ein:

    ![Fuzzysuche ](./media/how-to-search-for-address/fuzzy_search_params.png)

    | Schlüssel | Wert |
    |------------------|-------------------------|
    | api-version | 1.0 |
    | subscription-key | \<Ihr Azure Maps-Schlüssel\> |
    | query | pizza |

4. Klicken Sie auf **Send** (Senden), und sehen Sie sich den Antworttext an. 

    Die mehrdeutige Abfragezeichenfolge „pizza“ hat 10 POI-Ergebnisse mit Kategorien zurückgegeben, die unter „Pizza“ und „Restaurant“ fallen. In jedem Ergebnis werden eine Adresse, Breitengrad-Längengrad-Werte, ein Bildausschnitt und Einstiegspunkte für den Standort zurückgegeben.
    
    Die Ergebnisse für diese Abfrage variieren und sind mit keinem bestimmten Referenzstandort verbunden. Mit dem **countrySet**-Parameter können Sie nur die Länder angeben, die in der Anwendung abgedeckt werden sollen. Beim Standardverhalten wird auf der ganzen Welt gesucht, sodass potenziell unnötige Ergebnisse zurückgegeben werden.

5. Fügen Sie das folgende Schlüssel-Wert-Paar zum Abschnitt **Params** (Parameter) hinzu, und klicken Sie auf **Senden**:

    | Schlüssel | Wert |
    |------------------|-------------------------|
    | countrySet | US |
    
    Die Ergebnisse werden nun durch den Ländercode begrenzt, und die Abfrage gibt Pizzerien in den USA zurück.
    
    Für Ergebnisse, die sich auf einen bestimmten Standort beziehen, können Sie einen interessanten Ort abfragen und die zurückgegebenen Werte für den Breiten- und Längengrad im Aufruf des Diensts für die Fuzzysuche verwenden. In diesem Fall verwenden Sie den Suchdienst für die Rückgabe des Standorts der Space Needle in Seattle sowie die Werte für den Breiten- und Längengrad zur Eingrenzung der Suche.
    
4. Geben Sie unter „Params“ (Parameter) die folgenden Schlüssel-Wert-Paare ein, und klicken Sie auf **Send** (Senden):

    ![Fuzzysuche ](./media/how-to-search-for-address/fuzzy_search_latlon.png)
    
    | Schlüssel | Wert |
    |-----|------------|
    | lat | 47.62039 |
    | lon | -122.34928 |

## <a name="search-for-address-properties-and-coordinates"></a>Suchen nach Adresseigenschaften und -koordinaten 

Sie können eine vollständige oder unvollständige Adresse an die API für die Adresssuche übergeben. Sie erhalten dann eine Antwort, die detaillierte Adresseigenschaften, z.B. die Gemeinde oder Wohnsiedlung, sowie Positionswerte mit Breiten- und Längengraden umfasst.

1. Klicken Sie in Postman auf **New Request (Neue Anforderung)** | **GET request (GET-Anforderung)**, und nennen Sie sie **Address Search** (Adresssuche).
2. Wählen Sie auf der Registerkarte „Builder“ die HTTP-Methode **GET** aus, geben Sie die Anforderungs-URL für Ihren API-Endpunkt ein, und wählen Sie ggf. ein Autorisierungsprotokoll aus.

    ![Adresssuche ](./media/how-to-search-for-address/address_search_url.png)
    
    | Parameter | Empfohlener Wert |
    |---------------|------------------------------------------------|
    | HTTP-Methode | GET |
    | Anfrage-URL | https://atlas.microsoft.com/search/address/json? |
    | Autorisierung | No Auth |

2. Klicken Sie auf **Params** (Parameter), und geben Sie die folgenden Schlüssel-Wert-Paare zur Verwendung als Abfrage- oder Pfadparameter in der Anforderungs-URL ein:
    
    ![Adresssuche ](./media/how-to-search-for-address/address_search_params.png)
    
    | Schlüssel | Wert |
    |------------------|-------------------------|
    | api-version | 1.0 |
    | subscription-key | \<Ihr Azure Maps-Schlüssel\> |
    | query | 400 Broad St, Seattle, WA 98109 |
    
3. Klicken Sie auf **Send** (Senden), und sehen Sie sich den Antworttext an. 
    
    In diesem Fall haben Sie eine Abfrage für eine vollständige Adresse angegeben und erhalten im Antworttext ein einzelnes Ergebnis. 
    
4. Ändern Sie in „Params“ (Parameter) die Abfragezeichenfolge in den folgenden Wert:
    ```
        400 Broad, Seattle
    ```

5. Fügen Sie das folgende Schlüssel-Wert-Paar zum Abschnitt **Params** (Parameter) hinzu, und klicken Sie auf **Senden**:

    | Schlüssel | Wert |
    |-----|------------|
    | typeahead | true |

    Das Flag **typeahead** weist die API für die Adresssuche an, die Abfrage als unvollständige Eingabe zu verarbeiten und ein Array von Vorhersagewerten zurückzugeben.

## <a name="search-for-a-street-address-using-reverse-address-search"></a>Suchen nach einer Adresse mithilfe der inversen Adresssuche
1. Klicken Sie in Postman auf **New Request (Neue Anforderung)** | **GET request (GET-Anforderung)**, und nennen Sie sie **Reverse Address Search** (Inverse Adresssuche).

2. Wählen Sie auf der Registerkarte „Builder“ die HTTP-Methode **GET** aus, und geben Sie die Anforderungs-URL für Ihren API-Endpunkt ein.
    
    ![URL für inverse Adresssuche ](./media/how-to-search-for-address/reverse_address_search_url.png)
    
    | Parameter | Empfohlener Wert |
    |---------------|------------------------------------------------|
    | HTTP-Methode | GET |
    | Anfrage-URL | https://atlas.microsoft.com/search/address/reverse/json? |
    | Autorisierung | No Auth |
    
2. Klicken Sie auf **Params** (Parameter), und geben Sie die folgenden Schlüssel-Wert-Paare zur Verwendung als Abfrage- oder Pfadparameter in der Anforderungs-URL ein:
    
    ![Parameter für inverse Adresssuche ](./media/how-to-search-for-address/reverse_address_search_params.png)
    
    | Schlüssel | Wert |
    |------------------|-------------------------|
    | api-version | 1.0 |
    | subscription-key | \<Ihr Azure Maps-Schlüssel\> |
    | query | 47.59093,-122.33263 |
    
3. Klicken Sie auf **Send** (Senden), und sehen Sie sich den Antworttext an. 
    
    Die Antwort enthält den POI-Eintrag für Safeco Field mit der POI-Kategorie „Stadion“. 
    
4. Fügen Sie das folgende Schlüssel-Wert-Paar zum Abschnitt **Params** (Parameter) hinzu, und klicken Sie auf **Senden**:

    | Schlüssel | Wert |
    |-----|------------|
    | number | true |

    Wenn der Abfrageparameter [number](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) mit der Anforderung gesendet wird, enthält die Antwort möglicherweise die Straßenseite (links/rechts) und auch eine versetzte Position für diese Hausnummer.
    
5. Fügen Sie das folgende Schlüssel-Wert-Paar zum Abschnitt **Params** (Parameter) hinzu, und klicken Sie auf **Senden**:

    | Schlüssel | Wert |
    |-----|------------|
    | spatialKeys | true |

    Wenn der Abfrageparameter [spatialKeys](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) festgelegt wird, enthält die Antwort proprietäre geografische Schlüsselinformationen für einen angegebenen Standort.

6. Fügen Sie das folgende Schlüssel-Wert-Paar zum Abschnitt **Params** (Parameter) hinzu, und klicken Sie auf **Senden**:

    | Schlüssel | Wert |
    |-----|------------|
    | returnSpeedLimit | true |
    
    Wenn der Abfrageparameter [returnSpeedLimit](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) festgelegt wird, wird in der Antwort die angegebene Geschwindigkeitsbegrenzung zurückgegeben.

7. Fügen Sie das folgende Schlüssel-Wert-Paar zum Abschnitt **Params** (Parameter) hinzu, und klicken Sie auf **Senden**:

    | Schlüssel | Wert |
    |-----|------------|
    | returnRoadUse | true |

    Wenn der Abfrageparameter [returnRoadUse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) festgelegt wird, wird in der Antwort die Straßennutzung für die inverse Geocodierung von Straßen zurückgegeben.

8. Fügen Sie das folgende Schlüssel-Wert-Paar zum Abschnitt **Params** (Parameter) hinzu, und klicken Sie auf **Senden**:

    | Schlüssel | Wert |
    |-----|------------|
    | roadUse | true |

    Mithilfe des Abfrageparameters [roadUse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) können Sie die Abfrage der inversen Geocodierung auf eine bestimmte Art der Straßennutzung beschränken.
    
## <a name="search-for-the-cross-street-using-reverse-address-cross-street-search"></a>Suchen nach der Querstraße mithilfe der inversen Adresssuche für Querstraßen

1. Klicken Sie in Postman auf **New Request (Neue Anforderung)** | **GET request (GET-Anforderung)**, und nennen Sie sie **Reverse Address Cross Street Search** (Inverse Adresssuche für Querstraßen).

2. Wählen Sie auf der Registerkarte „Builder“ die HTTP-Methode **GET** aus, und geben Sie die Anforderungs-URL für Ihren API-Endpunkt ein.
    
    ![Inverse Adresssuche für Querstraßen ](./media/how-to-search-for-address/reverse_address_search_url.png)
    
    | Parameter | Empfohlener Wert |
    |---------------|------------------------------------------------|
    | HTTP-Methode | GET |
    | Anfrage-URL | https://atlas.microsoft.com/search/address/reverse/crossstreet/json? |
    | Autorisierung | No Auth |
    
3. Klicken Sie auf **Params** (Parameter), und geben Sie die folgenden Schlüssel-Wert-Paare zur Verwendung als Abfrage- oder Pfadparameter in der Anforderungs-URL ein:
    
    | Schlüssel | Wert |
    |------------------|-------------------------|
    | api-version | 1.0 |
    | subscription-key | \<Ihr Azure Maps-Schlüssel\> |
    | query | 47.59093,-122.33263 |
    
4. Klicken Sie auf **Send** (Senden), und sehen Sie sich den Antworttext an. 

## <a name="next-steps"></a>Nächste Schritte
- Machen Sie sich mit der API-Dokumentation zum [Suchdienst von Azure Maps](https://docs.microsoft.com/rest/api/maps/search) vertraut. 
