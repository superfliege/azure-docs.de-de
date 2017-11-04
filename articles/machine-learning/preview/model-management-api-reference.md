---
title: "Erstellen eines Docker-Image für die Modellverwaltung in Azure Machine Learning | Microsoft-Dokumentation"
description: "In diesem Artikel werden die Schritte zum Erstellen eines Docker-Image für Ihren Webdienst beschrieben."
services: machine-learning
author: chhavib
ms.author: chhavib
manager: neerajkh
editor: jasonwhowell
ms.service: machine-learning
ms.workload: data-services
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: 1939a18fbaf0aed0f62ac9e7641b9901ec4762ea
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2017
---
# <a name="azure-machine-learning-model-management-account-api-reference"></a>API zum Azure Machine Learning-Modellverwaltungskonto – Referenz

Informationen zur Einrichtung der Bereitstellungsumgebung finden Sie unter [Model Management account setup](model-management-configuration.md) (Einrichtung des Modellverwaltungskontos).

Mit der API zum Azure Machine Learning-Modellverwaltungskonto werden die folgenden Vorgänge implementiert:

- Modellregistrierung
- Manifesterstellung
- Docker-Imageerstellung
- Webdiensterstellung

Sie können dieses Image verwenden, um einen Webdienst entweder lokal oder in einem Azure Container Service-Remotecluster oder einer anderen Umgebung mit Docker-Unterstützung Ihrer Wahl zu erstellen.

## <a name="prerequisites"></a>Voraussetzungen
Stellen Sie sicher, dass Sie die Installationsschritte im Dokument [Install and create Quickstart](quickstart-installation.md) (Schnellstart zum Installieren und Erstellen) ausgeführt haben.

Folgendes ist erforderlich, bevor Sie fortfahren können:
1. Bereitstellung des Modellverwaltungskontos
2. Erstellung der Umgebung zum Bereitstellen und Verwalten von Modellen
3. Ein Machine Learning-Modell

### <a name="azure-ad-token"></a>Azure AD-Token
Melden Sie sich bei Verwendung der Azure CLI mit `az login` an. Die CLI nutzt Ihr Azure AD-Token (Azure Active Directory) aus der AZURE-Datei. Wenn Sie die APIs nutzen möchten, stehen Ihnen die hier angegebenen Optionen zur Verfügung.

##### <a name="acquire-the-azure-ad-token-manually"></a>Manuelles Abrufen des Azure AD-Tokens
Sie können `az login` verwenden und das aktuelle Token in der AZURE-Datei in Ihrem Stammverzeichnis ermitteln.

##### <a name="acquire-the-azure-ad-token-programmatically"></a>Programmgesteuertes Abrufen des Azure AD-Tokens
```
az ad sp create-for-rbac --scopes /subscriptions/<SubscriptionId>/resourcegroups/<ResourceGroupName> --role Contributor --years <length of time> --name <MyservicePrincipalContributor>
```
Speichern Sie die Ausgabe, nachdem Sie den Dienstprinzipal erstellt haben. Hiermit können Sie das Token jetzt aus Azure AD abrufen:

```cs
 private static async Task<string> AcquireTokenAsync(string clientId, string password, string authority, string resource)
{
        var creds = new ClientCredential(clientId, password);
        var context = new AuthenticationContext(authority);
        var token = await context.AcquireTokenAsync(resource, creds).ConfigureAwait(false);
        return token.AccessToken;
}
```
Das Token befindet sich in einem Autorisierungsheader für API-Aufrufe.


## <a name="register-a-model"></a>Registrieren eines Modells

Im Schritt für die Modellregistrierung wird Ihr Machine Learning-Modell unter dem von Ihnen erstellten Azure-Modellverwaltungskonto registriert. Diese Registrierung ermöglicht das Nachverfolgen der Modelle und ihrer Versionen, die bei der Registrierung zugewiesen werden. Der Benutzer gibt den Namen des Modells an. Bei der nachfolgenden Registrierung von Modellen unter dem gleichen Namen werden eine neue Version und ID generiert.

### <a name="request"></a>Anforderung

| Methode | Anforderungs-URI |
|------------|------------|
| POST |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models 
 |
### <a name="description"></a>Beschreibung
Dient zum Registrieren eines Modells.

### <a name="parameters"></a>Parameter
| Name | Enthalten in | Beschreibung | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | string |
| ResourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | string |
| accountName | path | Name des Modellverwaltungskontos | Ja | string |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | string |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | string |
| model | body | Nutzlast, die zum Registrieren eines Modells verwendet wird | Ja | [Modell](#model) |


### <a name="responses"></a>Antworten
| Code | Beschreibung | Schema |
|--------------------|--------------------|--------------------|
| 200 | OK. Die Modellregistrierung war erfolgreich. | [Modell](#model) |
| default | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-models-in-an-account"></a>Abfragen der Liste mit den Modellen in einem Konto
### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models 
 |
### <a name="description"></a>Beschreibung
Dient zum Abfragen der Liste mit den Modellen in einem Konto. Sie können die Ergebnisliste nach Tag und Name filtern. Wenn kein Filter übergeben wird, werden mit der Abfrage alle Modelle des Kontos aufgelistet. Die zurückgegebene Liste wird paginiert, und die Anzahl von Elementen auf jeder Seite ist ein optionaler Parameter.

### <a name="parameters"></a>Parameter
| Name | Enthalten in | Beschreibung | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | string |
| ResourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | string |
| accountName | path | Name des Modellverwaltungskontos | Ja | string |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | string |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | string |
| Name | query | Objektname | Nein | string |
| tag | query | Modelltag | Nein | string |
| count | query | Anzahl von Elementen, die pro Seite abgerufen werden sollen | Nein | string |
| $skipToken | query | Fortsetzungstoken zum Abrufen der nächsten Seite | Nein | string |

### <a name="responses"></a>Antworten
| Code | Beschreibung | Schema |
|--------------------|--------------------|--------------------|
| 200 | Erfolgreich. | [PaginatedModelList](#paginatedmodellist) |
| default | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse) |

## <a name="get-model-details"></a>Abrufen der Modelldetails
### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models/{id}  
 |

### <a name="description"></a>Beschreibung
Dient zum Abrufen eines Modells nach der ID.

### <a name="parameters"></a>Parameter
| Name | Enthalten in | Beschreibung | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | string |
| ResourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | string |
| accountName | path | Name des Modellverwaltungskontos | Ja | string |
| id | path | Objekt-ID | Ja | string |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | string |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | string |

### <a name="responses"></a>Antworten
| Code | Beschreibung | Schema |
|--------------------|--------------------|--------------------|
| 200 | Erfolgreich. | [Modell](#model) |
| default | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse) |

## <a name="register-a-manifest-with-the-registered-model-and-all-dependencies"></a>Registrieren eines Manifests beim registrierten Modell und allen Abhängigkeiten

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| POST |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests | 

### <a name="description"></a>Beschreibung
Dient zum Registrieren eines Manifests beim registrierten Modell und allen Abhängigkeiten.

### <a name="parameters"></a>Parameter
| Name | Enthalten in | Beschreibung | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | string |
| ResourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | string |
| accountName | path | Name des Modellverwaltungskontos | Ja | string |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | string |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | string |
| manifestRequest | body | Nutzlast, die zum Registrieren eines Manifests verwendet wird | Ja | [Manifest](#manifest) |

### <a name="responses"></a>Antworten
| Code | Beschreibung | Schema |
|--------------------|--------------------|--------------------|
| 200 | Die Registrierung des Manifests war erfolgreich. | [Manifest](#manifest) |
| default | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-manifests-in-an-account"></a>Abfragen der Liste mit den Manifesten in einem Konto

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests | 

### <a name="description"></a>Beschreibung
Dient zum Abfragen der Liste mit den Manifesten in einem Konto. Sie können die Ergebnisliste nach Modell-ID und Manifestname filtern. Wenn kein Filter übergeben wird, werden mit der Abfrage alle Manifeste des Kontos aufgelistet. Die zurückgegebene Liste wird paginiert, und die Anzahl von Elementen auf jeder Seite ist ein optionaler Parameter.

### <a name="parameters"></a>Parameter
| Name | Enthalten in | Beschreibung | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | string |
| ResourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | string |
| accountName | path | Name des Modellverwaltungskontos | Ja | string |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | string |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | string |
| modelId | query | Modell-ID | Nein | string |
| manifestName | query | Manifestname | Nein | string |
| count | query | Anzahl von Elementen, die pro Seite abgerufen werden sollen | Nein | string |
| $skipToken | query | Fortsetzungstoken zum Abrufen der nächsten Seite | Nein | string |

### <a name="responses"></a>Antworten
| Code | Beschreibung | Schema |
|--------------------|--------------------|--------------------|
| 200 | Erfolgreich. | [PaginatedManifestList](#paginatedmanifestlist) |
| default | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse) |

## <a name="get-manifest-details"></a>Abrufen der Manifestdetails

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests/{id} | 

### <a name="description"></a>Beschreibung
Dient zum Abrufen des Manifests nach der ID.

### <a name="parameters"></a>Parameter
| Name | Enthalten in | Beschreibung | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | string |
| ResourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | string |
| accountName | path | Name des Modellverwaltungskontos | Ja | string |
| id | path | Objekt-ID | Ja | string |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | string |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | string |

### <a name="responses"></a>Antworten
| Code | Beschreibung | Schema |
|--------------------|--------------------|--------------------|
| 200 | Erfolgreich. | [Manifest](#manifest) |
| default | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse) |

## <a name="create-an-image"></a>Erstellen eines Images

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| POST |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images | 

### <a name="description"></a>Beschreibung
Dient zum Erstellen eines Image als Docker-Image in Azure Container Registry.

### <a name="parameters"></a>Parameter
| Name | Enthalten in | Beschreibung | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | string |
| ResourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | string |
| accountName | path | Name des Modellverwaltungskontos | Ja | string |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | string |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | string |
| imageRequest | body | Nutzlast, die zum Erstellen eines Image verwendet wird | Ja | [ImageRequest](#imagerequest) |

### <a name="responses"></a>Antworten
| Code | Beschreibung | Header | Schema |
|--------------------|--------------------|--------------------|--------------------|
| 202 | Standort-URL für den asynchronen Vorgang. Mit einem GET-Aufruf können Sie den Status der Aufgabe für die Imageerstellung anzeigen. | Operation-Location |
| default | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-images-in-an-account"></a>Abfragen der Liste mit den Images in einem Konto

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images | 

### <a name="description"></a>Beschreibung
Dient zum Abfragen der Liste mit den Images in einem Konto. Sie können die Ergebnisliste nach Manifest-ID und Name filtern. Wenn kein Filter übergeben wird, werden mit der Abfrage alle Images des Kontos aufgelistet. Die zurückgegebene Liste wird paginiert, und die Anzahl von Elementen auf jeder Seite ist ein optionaler Parameter.

### <a name="parameters"></a>Parameter
| Name | Enthalten in | Beschreibung | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | string |
| ResourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | string |
| accountName | path | Name des Modellverwaltungskontos | Ja | string |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | string |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | string |
| manifestId | query | Manifest-ID | Nein | string |
| manifestName | query | Manifestname | Nein | string |
| count | query | Anzahl von Elementen, die pro Seite abgerufen werden sollen | Nein | string |
| $skipToken | query | Fortsetzungstoken zum Abrufen der nächsten Seite | Nein | string |

### <a name="responses"></a>Antworten
| Code | Beschreibung | Schema |
|--------------------|--------------------|--------------------|
| 200 | Erfolgreich. | [PaginatedImageList](#paginatedimagelist) |
| default | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse) |

## <a name="get-image-details"></a>Abrufen von Imagedetails

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images/{id} | 

### <a name="description"></a>Beschreibung
Dient zum Abrufen eines Image nach der ID.

### <a name="parameters"></a>Parameter
| Name | Enthalten in | Beschreibung | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | string |
| ResourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | string |
| accountName | path | Name des Modellverwaltungskontos | Ja | string |
| id | path | Image-ID | Ja | string |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | string |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | string |

### <a name="responses"></a>Antworten
| Code | Beschreibung | Schema |
|--------------------|--------------------|--------------------|
| 200 | Erfolgreich. | [Image](#image) |
| default | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse) |


## <a name="create-a-service"></a>Erstellen von Diensten

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| POST |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services | 

### <a name="description"></a>Beschreibung
Dient zum Erstellen eines Diensts aus einem Image.

### <a name="parameters"></a>Parameter
| Name | Enthalten in | Beschreibung | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | string |
| ResourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | string |
| accountName | path | Name des Modellverwaltungskontos | Ja | string |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | string |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | string |
| serviceRequest | body | Nutzlast, die zum Erstellen eines Diensts verwendet wird | Ja | [ServiceCreateRequest](#servicecreaterequest) |

### <a name="responses"></a>Antworten
| Code | Beschreibung | Header | Schema |
|--------------------|--------------------|--------------------|--------------------|
| 202 | Standort-URL für den asynchronen Vorgang. Mit einem GET-Aufruf können Sie den Status der Aufgabe für die Diensterstellung anzeigen. | Operation-Location |
| 409 | Ein Dienst mit dem angegebenen Namen ist bereits vorhanden. |
| default | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-services-in-an-account"></a>Abfragen der Liste mit den Diensten in einem Konto

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services | 

### <a name="description"></a>Beschreibung
Dient zum Abfragen der Liste mit den Diensten in einem Konto. Sie können die Ergebnisliste nach Modellname/-ID, Manifestname/-ID, Image-ID, Dienstname oder Machine Learning-Computeressourcen-ID filtern. Wenn kein Filter übergeben wird, werden mit der Abfrage alle Dienste des Kontos aufgeführt. Die zurückgegebene Liste wird paginiert, und die Anzahl von Elementen auf jeder Seite ist ein optionaler Parameter.

### <a name="parameters"></a>Parameter
| Name | Enthalten in | Beschreibung | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | string |
| ResourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | string |
| accountName | path | Name des Modellverwaltungskontos | Ja | string |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | string |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | string |
| serviceName | query | Dienstname | Nein | string |
| modelId | query | Modellname | Nein | string |
| modelName | query | Modell-ID | Nein | string |
| manifestId | query | Manifest-ID | Nein | string |
| manifestName | query | Manifestname | Nein | string |
| imageId | query | Image-ID | Nein | string |
| computeResourceId | query | Machine Learning-Computeressourcen-ID | Nein | string |
| count | query | Anzahl von Elementen, die pro Seite abgerufen werden sollen | Nein | string |
| $skipToken | query | Fortsetzungstoken zum Abrufen der nächsten Seite | Nein | string |

### <a name="responses"></a>Antworten
| Code | Beschreibung | Schema |
|--------------------|--------------------|--------------------|
| 200 | Erfolgreich. | [PaginatedServiceList](#paginatedservicelist) |
| default | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse) |

## <a name="get-service-details"></a>Abrufen von Dienstdetails

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id} | 

### <a name="description"></a>Beschreibung
Dient zum Abrufen eines Diensts nach der ID.

### <a name="parameters"></a>Parameter
| Name | Enthalten in | Beschreibung | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | string |
| ResourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | string |
| accountName | path | Name des Modellverwaltungskontos | Ja | string |
| id | path | Objekt-ID | Ja | string |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | string |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | string |

### <a name="responses"></a>Antworten
| Code | Beschreibung | Schema |
|--------------------|--------------------|--------------------|
| 200 | Erfolgreich. | [ServiceResponse](#serviceresponse) |
| default | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse)

## <a name="update-a-service"></a>Aktualisieren eines Diensts

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| PUT |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id} | 

### <a name="description"></a>Beschreibung
Dient zum Aktualisieren eines vorhandenen Diensts.

### <a name="parameters"></a>Parameter
| Name | Enthalten in | Beschreibung | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | string |
| ResourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | string |
| accountName | path | Name des Modellverwaltungskontos | Ja | string |
| id | path | Objekt-ID | Ja | string |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | string |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | string |
| serviceUpdateRequest | body | Nutzlast, die zum Aktualisieren eines vorhandenen Diensts verwendet wird | Ja |  [ServiceUpdateRequest](#serviceupdaterequest) |

### <a name="responses"></a>Antworten
| Code | Beschreibung | Header | Schema |
|--------------------|--------------------|--------------------|--------------------|
| 202 | Standort-URL für den asynchronen Vorgang. Mit einem GET-Aufruf können Sie den Status der Aufgabe für die Dienstaktualisierung anzeigen. | Operation-Location |
| 404 | Der Dienst mit der angegebenen ID ist nicht vorhanden. |
| default | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse)

## <a name="delete-a-service"></a>Löschen von Diensten

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| DELETE |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id} | 

### <a name="description"></a>Beschreibung
Dient zum Löschen eines Diensts.

### <a name="parameters"></a>Parameter
| Name | Enthalten in | Beschreibung | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | string |
| ResourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | string |
| accountName | path | Name des Modellverwaltungskontos | Ja | string |
| id | path | Objekt-ID | Ja | string |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | string |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | string |

### <a name="responses"></a>Antworten
| Code | Beschreibung | Schema |
|--------------------|--------------------|--------------------|
| 200 | Erfolgreich. |  |
| 204 | Der Dienst mit der angegebenen ID ist nicht vorhanden. |
| default | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse)

## <a name="get-service-keys"></a>Abrufen von Dienstschlüsseln

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id}/keys | 

### <a name="description"></a>Beschreibung
Dient zum Abrufen von Dienstschlüsseln.

### <a name="parameters"></a>Parameter
| Name | Enthalten in | Beschreibung | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | string |
| ResourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | string |
| accountName | path | Name des Modellverwaltungskontos | Ja | string |
| id | path | Dienst-ID | Ja | string |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | string |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | string |

### <a name="responses"></a>Antworten
| Code | Beschreibung | Schema |
|--------------------|--------------------|--------------------|
| 200 | Erfolgreich. | [AuthKeys](#authkeys)
| default | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse)

## <a name="regenerate-service-keys"></a>Erneutes Generieren von Dienstschlüsseln

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| POST |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id}/keys | 

### <a name="description"></a>Beschreibung
Dient zum erneuten Generieren und Zurückgeben von Dienstschlüsseln.

### <a name="parameters"></a>Parameter
| Name | Enthalten in | Beschreibung | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | string |
| ResourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | string |
| accountName | path | Name des Modellverwaltungskontos | Ja | string |
| id | path | Dienst-ID | Ja | string |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | string |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | string |
| regenerateKeyRequest | body | Nutzlast, die zum Aktualisieren eines vorhandenen Diensts verwendet wird | Ja | [ServiceRegenerateKeyRequest](#serviceregeneratekeyrequest) |

### <a name="responses"></a>Antworten
| Code | Beschreibung | Schema |
|--------------------|--------------------|--------------------|
| 200 | Erfolgreich. | [AuthKeys](#authkeys)
| default | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse)

## <a name="query-the-list-of-deployments-in-an-account"></a>Abfragen der Liste mit den Bereitstellungen eines Kontos

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/deployments | 

### <a name="description"></a>Beschreibung
Dient zum Abfragen der Liste mit den Bereitstellungen eines Kontos. Sie können die Ergebnisliste nach der Dienst-ID filtern. Hierbei werden nur die Bereitstellungen zurückgegeben, die für den jeweiligen Dienst erstellt werden. Wenn kein Filter übergeben wird, werden mit der Abfrage alle Bereitstellungen des Kontos aufgelistet.

### <a name="parameters"></a>Parameter
| Name | Enthalten in | Beschreibung | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | string |
| ResourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | string |
| accountName | path | Name des Modellverwaltungskontos | Ja | string |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | string |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | string |
| serviceId | query | Dienst-ID | Nein | string |

### <a name="responses"></a>Antworten
| Code | Beschreibung | Schema |
|--------------------|--------------------|--------------------|
| 200 | Erfolgreich. | [DeploymentList](#deploymentlist) |
| default | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse)

## <a name="get-deployment-details"></a>Abrufen von Bereitstellungsdetails

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/deployments/{id} | 

### <a name="description"></a>Beschreibung
Dient zum Abrufen der Bereitstellung nach der ID.

### <a name="parameters"></a>Parameter
| Name | Enthalten in | Beschreibung | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | string |
| ResourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | string |
| accountName | path | Name des Modellverwaltungskontos | Ja | string |
| id | path | Bereitstellungs-ID | Ja | string |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | string |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | string |

### <a name="responses"></a>Antworten
| Code | Beschreibung | Schema |
|--------------------|--------------------|--------------------|
| 200 | Erfolgreich. | [Bereitstellung](#deployment) |
| default | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse)

## <a name="get-operation-details"></a>Abrufen von Vorgangsdetails

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/operations/{id} | 

### <a name="description"></a>Beschreibung
Dient zum Abrufen des Status von asynchronen Vorgängen nach Vorgangs-ID.

### <a name="parameters"></a>Parameter
| Name | Enthalten in | Beschreibung | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | string |
| ResourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | string |
| accountName | path | Name des Modellverwaltungskontos | Ja | string |
| id | path | Vorgangs-ID | Ja | string |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | string |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | string |

### <a name="responses"></a>Antworten
| Code | Beschreibung | Schema |
|--------------------|--------------------|--------------------|
| 200 | Erfolgreich. | [OperationStatus](#asyncoperationstatus) |
| default | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse)



<a name="definitions"></a>
## <a name="definitions"></a>Definitionen

<a name="asset"></a>
### <a name="asset"></a>Asset
Das Assetobjekt, das für die Erstellung des Docker-Image benötigt wird.


|Name|Beschreibung|Schema|
|---|---|---|
|**id**  <br>*optional*|Asset-ID|string|
|**mimeType**  <br>*optional*|MIME-Typ des Modellinhalts. Weitere Informationen zum MIME-Typ finden Sie in der [Liste mit den IANA-Medientypen](https://www.iana.org/assignments/media-types/media-types.xhtml).|string|
|**unpack**  <br>*optional*|Gibt an, wo der Inhalt während der Erstellung des Docker-Image entpackt werden muss.|Boolescher Wert|
|**url**  <br>*optional*|URL des Asset-Standorts|string|


<a name="asyncoperationstate"></a>
### <a name="asyncoperationstate"></a>AsyncOperationState
Der Status des asynchronen Vorgangs.

*Typ*: enum (NotStarted, Running, Cancelled, Succeeded, Failed)


<a name="asyncoperationstatus"></a>
### <a name="asyncoperationstatus"></a>AsyncOperationStatus
Der Vorgangsstatus.


|Name|Beschreibung|Schema|
|---|---|---|
|**createdTime**  <br>*optional*  <br>*schreibgeschützt*|Erstellungszeitpunkt (UTC) des asynchronen Vorgangs.|string (Datum/Uhrzeit)|
|**endTime**  <br>*optional*  <br>*schreibgeschützt*|Endzeitpunkt (UTC) des asynchronen Vorgangs.|string (Datum/Uhrzeit)|
|**error**  <br>*optional*||[ErrorResponse](#errorresponse)|
|**id**  <br>*optional*|ID des asynchronen Vorgangs.|string|
|**operationType**  <br>*optional*|Typ des asynchronen Vorgangs.|enum (Image, Dienst)|
|**resourceLocation**  <br>*optional*|Per asynchronem Vorgang erstellte oder aktualisierte Ressource.|string|
|**state**  <br>*optional*||[AsyncOperationState](#asyncoperationstate)|


<a name="authkeys"></a>
### <a name="authkeys"></a>AuthKeys
Die Authentifizierungsschlüssel für einen Dienst.


|Name|Beschreibung|Schema|
|---|---|---|
|**primaryKey**  <br>*optional*|Primärschlüssel|string|
|**secondaryKey**  <br>*optional*|Sekundärschlüssel|string|


<a name="autoscaler"></a>
### <a name="autoscaler"></a>AutoScaler
Die Einstellungen für die automatische Skalierung.


|Name|Beschreibung|Schema|
|---|---|---|
|**autoscaleEnabled**  <br>*optional*|Dient zum Aktivieren oder Deaktivieren der automatischen Skalierung.|Boolescher Wert|
|**maxReplicas**  <br>*optional*|Maximale Anzahl von Pod-Replikaten für das zentrale Hochskalieren.  <br>**Mindestwert**: `1`|Ganze Zahl|
|**minReplicas**  <br>*optional*|Minimale Anzahl von Pod-Replikaten für das zentrale Herunterskalieren.  <br>**Mindestwert**: `0`|Ganze Zahl|
|**refreshPeriodInSeconds**  <br>*optional*|Aktualisierungszeitpunkt für den Trigger der automatischen Skalierung.  <br>**Mindestwert**: `1`|Ganze Zahl|
|**targetUtilization**  <br>*optional*|Auslastungsprozentsatz, bei dem die automatische Skalierung ausgelöst wird.  <br>**Mindestwert**: `0`  <br>**Maximalwert**: `100`|Ganze Zahl|


<a name="computeresource"></a>
### <a name="computeresource"></a>ComputeResource
Die Machine Learning-Computeressource.


|Name|Beschreibung|Schema|
|---|---|---|
|**id**  <br>*optional*|Ressourcen-ID|string|
|**type**  <br>*optional*|Ressourcentyp.|enum (Cluster)|


<a name="containerresourcereservation"></a>
### <a name="containerresourcereservation"></a>ContainerResourceReservation
Konfiguration zum Reservieren von Ressourcen für einen Container im Cluster.


|Name|Beschreibung|Schema|
|---|---|---|
|**cpu**  <br>*optional*|Gibt die CPU-Reservierung an. Format für Kubernetes: siehe [Meaning of CPU](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#meaning-of-cpu) (Bedeutung der CPU).|string|
|**memory**  <br>*optional*|Gibt die Speicherreservierung an. Format für Kubernetes: siehe [Meaning of memory](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#meaning-of-memory) (Bedeutung des Arbeitsspeichers).|string|


<a name="deployment"></a>
### <a name="deployment"></a>Bereitstellung
Eine Instanz einer Azure Machine Learning-Bereitstellung.


|Name|Beschreibung|Schema|
|---|---|---|
|**createdAt**  <br>*optional*  <br>*schreibgeschützt*|Erstellungszeitpunkt (UTC) der Bereitstellung|string (Datum/Uhrzeit)|
|**expiredAt**  <br>*optional*  <br>*schreibgeschützt*|Ablaufzeitpunkt (UTC) der Bereitstellung|string (Datum/Uhrzeit)|
|**id**  <br>*optional*|Bereitstellungs-ID|string|
|**imageId**  <br>*optional*|Image-ID, die dieser Bereitstellung zugeordnet ist|string|
|**serviceName**  <br>*optional*|Dienstname|string|
|**status**  <br>*optional*|Aktueller Status der Bereitstellung|string|


<a name="deploymentlist"></a>
### <a name="deploymentlist"></a>DeploymentList
Ein Array mit Bereitstellungsobjekten.

*Typ*: <[Deployment](#deployment)>-Array


<a name="errordetail"></a>
### <a name="errordetail"></a>ErrorDetail
Fehlerdetails zum Modellverwaltungsdienst.


|Name|Beschreibung|Schema|
|---|---|---|
|**code**  <br>*erforderlich*|Fehlercode|string|
|**message**  <br>*erforderlich*|Fehlermeldung.|string|


<a name="errorresponse"></a>
### <a name="errorresponse"></a>ErrorResponse
Ein Fehlerobjekt des Modellverwaltungsdiensts.


|Name|Beschreibung|Schema|
|---|---|---|
|**code**  <br>*erforderlich*|Fehlercode|string|
|**details**  <br>*optional*|Array mit Fehlerdetailobjekten|<[ErrorDetail](#errordetail)>-Array|
|**message**  <br>*erforderlich*|Fehlermeldung|string|
|**statusCode**  <br>*optional*|HTTP-Statuscode|Ganze Zahl|


<a name="image"></a>
### <a name="image"></a>Image
Das Azure Machine Learning-Image.


|Name|Beschreibung|Schema|
|---|---|---|
|**computeResourceId**  <br>*optional*|ID der Umgebung, die in der Machine Learning-Computeressource erstellt wurde|string|
|**createdTime**  <br>*optional*|Erstellungszeitpunkt (UTC) des Image|string (Datum/Uhrzeit)|
|**creationState**  <br>*optional*||[AsyncOperationState](#asyncoperationstate)|
|**description**  <br>*optional*|Image-Beschreibungstext|string|
|**error**  <br>*optional*||[ErrorResponse](#errorresponse)|
|**id**  <br>*optional*|Image-ID|string|
|**imageBuildLogUri**  <br>*optional*|URI der hochgeladenen Protokolle aus dem Image-Build|string|
|**imageLocation**  <br>*optional*|Azure Container Registry-Standortzeichenfolge für das erstellte Image|string|
|**imageType**  <br>*optional*||[ImageType](#imagetype)|
|**manifest**  <br>*optional*||[Manifest](#manifest)|
|**name**  <br>*optional*|Imagename|string|
|**version**  <br>*optional*|Vom Modellverwaltungsdienst festgelegte Imageversion|Ganze Zahl|


<a name="imagerequest"></a>
### <a name="imagerequest"></a>ImageRequest
Eine Anforderung zur Erstellung eines Azure Machine Learning-Image.


|Name|Beschreibung|Schema|
|---|---|---|
|**computeResourceId**  <br>*erforderlich*|ID der Umgebung, die in der Machine Learning-Computeressource erstellt wurde|string|
|**description**  <br>*optional*|Image-Beschreibungstext|string|
|**imageType**  <br>*erforderlich*||[ImageType](#imagetype)|
|**manifestId**  <br>*erforderlich*|ID des Manifests, aus dem das Image erstellt wird|string|
|**name**  <br>*erforderlich*|Imagename|string|


<a name="imagetype"></a>
### <a name="imagetype"></a>ImageType
Gibt den Typ des Image an.

*Typ*: enum (Docker)


<a name="manifest"></a>
### <a name="manifest"></a>Manifest
Das Azure Machine Learning-Manifest.


|Name|Beschreibung|Schema|
|---|---|---|
|**assets**  <br>*erforderlich*|Liste mit Assets|<[Asset](#asset)>-Array|
|**createdTime**  <br>*optional*  <br>*schreibgeschützt*|Erstellungszeitpunkt (UTC) des Manifests|string (Datum/Uhrzeit)|
|**description**  <br>*optional*|Beschreibungstext des Manifests|string|
|**driverProgram**  <br>*erforderlich*|Treiberprogramm des Manifests|string|
|**id**  <br>*optional*|Manifest-ID|string|
|**modelIds**  <br>*optional*|Liste mit den Modell-IDs der registrierten Modelle. Die Anforderung schlägt fehl, wenn keines der enthaltenen Modelle registriert ist.|<string>-Array|
|**modelType**  <br>*optional*|Gibt an, dass die Modelle bereits beim Modellverwaltungsdienst registriert sind.|enum (registriert)|
|**name**  <br>*erforderlich*|Manifestname|string|
|**targetRuntime**  <br>*erforderlich*||[TargetRuntime](#targetruntime)|
|**version**  <br>*optional*  <br>*schreibgeschützt*|Vom Modellverwaltungsdienst zugewiesene Manifestversion|Ganze Zahl|
|**webserviceType**  <br>*optional*|Gibt den gewünschten Typ von Webdienst an, der über das Manifest erstellt wird.|enum (Echtzeit)|


<a name="model"></a>
### <a name="model"></a>Modell
Eine Instanz eines Azure Machine Learning-Modells.


|Name|Beschreibung|Schema|
|---|---|---|
|**createdAt**  <br>*optional*  <br>*schreibgeschützt*|Erstellungszeitpunkt (UTC) des Modells|string (Datum/Uhrzeit)|
|**description**  <br>*optional*|Beschreibungstext des Modells|string|
|**id**  <br>*optional*  <br>*schreibgeschützt*|Modell-ID|string|
|**mimeType**  <br>*erforderlich*|MIME-Typ des Modellinhalts. Weitere Informationen zum MIME-Typ finden Sie in der [Liste mit den IANA-Medientypen](https://www.iana.org/assignments/media-types/media-types.xhtml).|string|
|**name**  <br>*erforderlich*|Modellname|string|
|**Tags**  <br>*optional*|Liste mit Modelltags|<string>-Array|
|**unpack**  <br>*optional*|Gibt an, ob das Modell während der Erstellung des Docker-Image entpackt werden muss.|Boolescher Wert|
|**url**  <br>*erforderlich*|Die URL des Modells. Normalerweise wird hier eine SAS-URL (Shared Access Signature) angegeben.|string|
|**version**  <br>*optional*  <br>*schreibgeschützt*|Vom Modellverwaltungsdienst zugewiesene Modellversion|Ganze Zahl|


<a name="modeldatacollection"></a>
### <a name="modeldatacollection"></a>ModelDataCollection
Die Informationen zur Modelldatensammlung.


|Name|Beschreibung|Schema|
|---|---|---|
|**eventHubEnabled**  <br>*optional*|Dient zum Aktivieren eines Event Hub für einen Dienst.|Boolescher Wert|
|**storageEnabled**  <br>*optional*|Dient zum Aktivieren des Speichers für einen Dienst.|Boolescher Wert|


<a name="paginatedimagelist"></a>
### <a name="paginatedimagelist"></a>PaginatedImageList
Eine paginierte Liste mit Images.


|Name|Beschreibung|Schema|
|---|---|---|
|**nextLink**  <br>*optional*|Fortsetzungslink (absoluter URI) auf die nächste Seite mit Ergebnissen in der Liste|string|
|**value**  <br>*optional*|Array mit Modellobjekten|<[Image](#image)>-Array|


<a name="paginatedmanifestlist"></a>
### <a name="paginatedmanifestlist"></a>PaginatedManifestList
Eine paginierte Liste mit Manifesten.


|Name|Beschreibung|Schema|
|---|---|---|
|**nextLink**  <br>*optional*|Fortsetzungslink (absoluter URI) auf die nächste Seite mit Ergebnissen in der Liste|string|
|**value**  <br>*optional*|Array mit Manifestobjekten|<[Manifest](#manifest)>-Array|


<a name="paginatedmodellist"></a>
### <a name="paginatedmodellist"></a>PaginatedModelList
Eine paginierte Liste mit Modellen.


|Name|Beschreibung|Schema|
|---|---|---|
|**nextLink**  <br>*optional*|Fortsetzungslink (absoluter URI) auf die nächste Seite mit Ergebnissen in der Liste|string|
|**value**  <br>*optional*|Array mit Modellobjekten|<[Model](#model)>-Array|


<a name="paginatedservicelist"></a>
### <a name="paginatedservicelist"></a>PaginatedServiceList
Eine paginierte Liste mit Diensten.


|Name|Beschreibung|Schema|
|---|---|---|
|**nextLink**  <br>*optional*|Fortsetzungslink (absoluter URI) auf die nächste Seite mit Ergebnissen in der Liste|string|
|**value**  <br>*optional*|Array mit Dienstobjekten|<[ServiceResponse](#serviceresponse)>-Array|


<a name="servicecreaterequest"></a>
### <a name="servicecreaterequest"></a>ServiceCreateRequest
Eine Anforderung zum Erstellen eines Diensts.


|Name|Beschreibung|Schema|
|---|---|---|
|**appInsightsEnabled**  <br>*optional*|Dient zum Aktivieren von Application Insights für einen Dienst.|Boolescher Wert|
|**autoScaler**  <br>*optional*||[AutoScaler](#autoscaler)|
|**computeResource**  <br>*erforderlich*||[ComputeResource](#computeresource)|
|**containerResourceReservation**  <br>*optional*||[ContainerResourceReservation](#containerresourcereservation)|
|**dataCollection**  <br>*optional*||[ModelDataCollection](#modeldatacollection)|
|**imageId**  <br>*erforderlich*|Image zum Erstellen des Diensts|string|
|**maxConcurrentRequestsPerContainer**  <br>*optional*|Maximale Anzahl von gleichzeitigen Anforderungen.  <br>**Mindestwert**: `1`|Ganze Zahl|
|**name**  <br>*erforderlich*|Dienstname|string|
|**numReplicas**  <br>*optional*|Anzahl von Pod-Replikaten, die jeweils ausgeführt werden. Kann nicht angegeben werden, wenn die automatische Skalierung aktiviert ist.  <br>**Mindestwert**: `0`|Ganze Zahl|


<a name="serviceregeneratekeyrequest"></a>
### <a name="serviceregeneratekeyrequest"></a>ServiceRegenerateKeyRequest
Eine Anforderung zum erneuten Generieren eines Schlüssels für einen Dienst.


|Name|Beschreibung|Schema|
|---|---|---|
|**keyType**  <br>*optional*|Gibt an, welcher Schlüssel neu generiert werden soll.|enum (Primär, Sekundär)|


<a name="serviceresponse"></a>
### <a name="serviceresponse"></a>ServiceResponse
Der detaillierte Status des Diensts.


|Name|Beschreibung|Schema|
|---|---|---|
|**createdAt**  <br>*optional*|Erstellungszeitpunkt (UTC) des Diensts|string (Datum/Uhrzeit)|
|**ID**  <br>*optional*|Dienst-ID|string|
|**image**  <br>*optional*||[Image](#image)|
|**manifest**  <br>*optional*||[Manifest](#manifest)|
|**models**  <br>*optional*|Liste mit Modellen|<[Model](#model)>-Array|
|**name**  <br>*optional*|Dienstname|string|
|**scoringUri**  <br>*optional*|URI für die Bewertung des Diensts|string|
|**state**  <br>*optional*||[AsyncOperationState](#asyncoperationstate)|
|**updatedAt**  <br>*optional*|Zeitpunkt (UTC) der letzten Aktualisierung|string (Datum/Uhrzeit)|
|**appInsightsEnabled**  <br>*optional*|Dient zum Aktivieren von Application Insights für einen Dienst.|Boolescher Wert|
|**autoScaler**  <br>*optional*||[AutoScaler](#autoscaler)|
|**computeResource**  <br>*erforderlich*||[ComputeResource](#computeresource)|
|**containerResourceReservation**  <br>*optional*||[ContainerResourceReservation](#containerresourcereservation)|
|**dataCollection**  <br>*optional*||[ModelDataCollection](#modeldatacollection)|
|**maxConcurrentRequestsPerContainer**  <br>*optional*|Maximale Anzahl von gleichzeitigen Anforderungen.  <br>**Mindestwert**: `1`|Ganze Zahl|
|**numReplicas**  <br>*optional*|Anzahl von Pod-Replikaten, die jeweils ausgeführt werden. Kann nicht angegeben werden, wenn die automatische Skalierung aktiviert ist.  <br>**Mindestwert**: `0`|Ganze Zahl|
|**error**  <br>*optional*||[ErrorResponse](#errorresponse)|


<a name="serviceupdaterequest"></a>
### <a name="serviceupdaterequest"></a>ServiceUpdateRequest
Eine Anforderung zum Aktualisieren eines Diensts.


|Name|Beschreibung|Schema|
|---|---|---|
|**appInsightsEnabled**  <br>*optional*|Dient zum Aktivieren von Application Insights für einen Dienst.|Boolescher Wert|
|**autoScaler**  <br>*optional*||[AutoScaler](#autoscaler)|
|**containerResourceReservation**  <br>*optional*||[ContainerResourceReservation](#containerresourcereservation)|
|**dataCollection**  <br>*optional*||[ModelDataCollection](#modeldatacollection)|
|**imageId**  <br>*optional*|Image zum Erstellen des Diensts|string|
|**maxConcurrentRequestsPerContainer**  <br>*optional*|Maximale Anzahl von gleichzeitigen Anforderungen.  <br>**Mindestwert**: `1`|Ganze Zahl|
|**numReplicas**  <br>*optional*|Anzahl von Pod-Replikaten, die jeweils ausgeführt werden. Kann nicht angegeben werden, wenn die automatische Skalierung aktiviert ist.  <br>**Mindestwert**: `0`|Ganze Zahl|


<a name="targetruntime"></a>
### <a name="targetruntime"></a>TargetRuntime
Der Typ der Ziellaufzeit.


|Name|Beschreibung|Schema|
|---|---|---|
|**properties**  <br>*erforderlich*||<string, string>-Zuordnung|
|**runtimeType**  <br>*erforderlich*|Gibt die Laufzeit an.|enum (SparkPython, Python)|

