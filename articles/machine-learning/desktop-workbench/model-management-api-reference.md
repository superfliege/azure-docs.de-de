---
title: Erstellen eines Docker-Image für die Modellverwaltung in Azure Machine Learning | Microsoft-Dokumentation
description: In diesem Artikel werden die Schritte zum Erstellen eines Docker-Image für Ihren Webdienst beschrieben.
services: machine-learning
author: chhavib
ms.author: chhavib
manager: hjerez
editor: jasonwhowell
ms.reviewer: jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: e518799fb4144093e5c3caf0524254a098e4b9cc
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
# <a name="azure-machine-learning-model-management-account-api-reference"></a>API zum Azure Machine Learning-Modellverwaltungskonto – Referenz

Informationen zur Einrichtung der Bereitstellungsumgebung finden Sie unter [Model Management account setup](deployment-setup-configuration.md) (Einrichtung des Modellverwaltungskontos).

Mit der API zum Azure Machine Learning-Modellverwaltungskonto werden die folgenden Vorgänge implementiert:

- Modellregistrierung
- Manifesterstellung
- Docker-Imageerstellung
- Webdiensterstellung

Sie können dieses Image verwenden, um einen Webdienst entweder lokal oder in einem Azure Container Service-Remotecluster oder einer anderen Umgebung mit Docker-Unterstützung Ihrer Wahl zu erstellen.

## <a name="prerequisites"></a>Voraussetzungen
Stellen Sie sicher, dass Sie die Installationsschritte im Dokument [Install and create Quickstart](../service/quickstart-installation.md) (Schnellstart zum Installieren und Erstellen) ausgeführt haben.

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
### <a name="description"></a>BESCHREIBUNG
Dient zum Registrieren eines Modells.

### <a name="parameters"></a>Parameter
| NAME | Enthalten in | BESCHREIBUNG | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | Zeichenfolge |
| resourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | Zeichenfolge |
| .<Name der Region | path | Name des Modellverwaltungskontos | Ja | Zeichenfolge |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | Zeichenfolge |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | Zeichenfolge |
| model | body | Nutzlast, die zum Registrieren eines Modells verwendet wird | Ja | [Modell](#model) |


### <a name="responses"></a>Antworten
| Code | BESCHREIBUNG | Schema |
|--------------------|--------------------|--------------------|
| 200 | OK. Die Modellregistrierung war erfolgreich. | [Modell](#model) |
| die Standardeinstellung | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-models-in-an-account"></a>Abfragen der Liste mit den Modellen in einem Konto
### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models 
 |
### <a name="description"></a>BESCHREIBUNG
Dient zum Abfragen der Liste mit den Modellen in einem Konto. Sie können die Ergebnisliste nach Tag und Name filtern. Wenn kein Filter übergeben wird, werden mit der Abfrage alle Modelle des Kontos aufgelistet. Die zurückgegebene Liste wird paginiert, und die Anzahl von Elementen auf jeder Seite ist ein optionaler Parameter.

### <a name="parameters"></a>Parameter
| NAME | Enthalten in | BESCHREIBUNG | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | Zeichenfolge |
| resourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | Zeichenfolge |
| .<Name der Region | path | Name des Modellverwaltungskontos | Ja | Zeichenfolge |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | Zeichenfolge |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | Zeichenfolge |
| name | query | Objektname | Nein  | Zeichenfolge |
| tag | query | Modelltag | Nein  | Zeichenfolge |
| count | query | Anzahl von Elementen, die pro Seite abgerufen werden sollen | Nein  | Zeichenfolge |
| $skipToken | query | Fortsetzungstoken zum Abrufen der nächsten Seite | Nein  | Zeichenfolge |

### <a name="responses"></a>Antworten
| Code | BESCHREIBUNG | Schema |
|--------------------|--------------------|--------------------|
| 200 | Erfolgreich. | [PaginatedModelList](#paginatedmodellist) |
| die Standardeinstellung | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse) |

## <a name="get-model-details"></a>Abrufen der Modelldetails
### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models/{id}  
 |

### <a name="description"></a>BESCHREIBUNG
Dient zum Abrufen eines Modells nach der ID.

### <a name="parameters"></a>Parameter
| NAME | Enthalten in | BESCHREIBUNG | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | Zeichenfolge |
| resourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | Zeichenfolge |
| .<Name der Region | path | Name des Modellverwaltungskontos | Ja | Zeichenfolge |
| id | path | Objekt-ID | Ja | Zeichenfolge |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | Zeichenfolge |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | Zeichenfolge |

### <a name="responses"></a>Antworten
| Code | BESCHREIBUNG | Schema |
|--------------------|--------------------|--------------------|
| 200 | Erfolgreich. | [Modell](#model) |
| die Standardeinstellung | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse) |

## <a name="register-a-manifest-with-the-registered-model-and-all-dependencies"></a>Registrieren eines Manifests beim registrierten Modell und allen Abhängigkeiten

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| POST |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests | 

### <a name="description"></a>BESCHREIBUNG
Dient zum Registrieren eines Manifests beim registrierten Modell und allen Abhängigkeiten.

### <a name="parameters"></a>Parameter
| NAME | Enthalten in | BESCHREIBUNG | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | Zeichenfolge |
| resourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | Zeichenfolge |
| .<Name der Region | path | Name des Modellverwaltungskontos | Ja | Zeichenfolge |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | Zeichenfolge |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | Zeichenfolge |
| manifestRequest | body | Nutzlast, die zum Registrieren eines Manifests verwendet wird | Ja | [Manifest](#manifest) |

### <a name="responses"></a>Antworten
| Code | BESCHREIBUNG | Schema |
|--------------------|--------------------|--------------------|
| 200 | Die Registrierung des Manifests war erfolgreich. | [Manifest](#manifest) |
| die Standardeinstellung | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-manifests-in-an-account"></a>Abfragen der Liste mit den Manifesten in einem Konto

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests | 

### <a name="description"></a>BESCHREIBUNG
Dient zum Abfragen der Liste mit den Manifesten in einem Konto. Sie können die Ergebnisliste nach Modell-ID und Manifestname filtern. Wenn kein Filter übergeben wird, werden mit der Abfrage alle Manifeste des Kontos aufgelistet. Die zurückgegebene Liste wird paginiert, und die Anzahl von Elementen auf jeder Seite ist ein optionaler Parameter.

### <a name="parameters"></a>Parameter
| NAME | Enthalten in | BESCHREIBUNG | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | Zeichenfolge |
| resourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | Zeichenfolge |
| .<Name der Region | path | Name des Modellverwaltungskontos | Ja | Zeichenfolge |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | Zeichenfolge |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | Zeichenfolge |
| modelId | query | Modell-ID | Nein  | Zeichenfolge |
| manifestName | query | Manifestname | Nein  | Zeichenfolge |
| count | query | Anzahl von Elementen, die pro Seite abgerufen werden sollen | Nein  | Zeichenfolge |
| $skipToken | query | Fortsetzungstoken zum Abrufen der nächsten Seite | Nein  | Zeichenfolge |

### <a name="responses"></a>Antworten
| Code | BESCHREIBUNG | Schema |
|--------------------|--------------------|--------------------|
| 200 | Erfolgreich. | [PaginatedManifestList](#paginatedmanifestlist) |
| die Standardeinstellung | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse) |

## <a name="get-manifest-details"></a>Abrufen der Manifestdetails

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests/{id} | 

### <a name="description"></a>BESCHREIBUNG
Dient zum Abrufen des Manifests nach der ID.

### <a name="parameters"></a>Parameter
| NAME | Enthalten in | BESCHREIBUNG | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | Zeichenfolge |
| resourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | Zeichenfolge |
| .<Name der Region | path | Name des Modellverwaltungskontos | Ja | Zeichenfolge |
| id | path | Objekt-ID | Ja | Zeichenfolge |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | Zeichenfolge |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | Zeichenfolge |

### <a name="responses"></a>Antworten
| Code | BESCHREIBUNG | Schema |
|--------------------|--------------------|--------------------|
| 200 | Erfolgreich. | [Manifest](#manifest) |
| die Standardeinstellung | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse) |

## <a name="create-an-image"></a>Erstellen eines Images

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| POST |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images | 

### <a name="description"></a>BESCHREIBUNG
Dient zum Erstellen eines Image als Docker-Image in Azure Container Registry.

### <a name="parameters"></a>Parameter
| NAME | Enthalten in | BESCHREIBUNG | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | Zeichenfolge |
| resourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | Zeichenfolge |
| .<Name der Region | path | Name des Modellverwaltungskontos | Ja | Zeichenfolge |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | Zeichenfolge |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | Zeichenfolge |
| imageRequest | body | Nutzlast, die zum Erstellen eines Image verwendet wird | Ja | [ImageRequest](#imagerequest) |

### <a name="responses"></a>Antworten
| Code | BESCHREIBUNG | Header | Schema |
|--------------------|--------------------|--------------------|--------------------|
| 202 | Standort-URL für den asynchronen Vorgang. Mit einem GET-Aufruf können Sie den Status der Aufgabe für die Imageerstellung anzeigen. | Operation-Location |
| die Standardeinstellung | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-images-in-an-account"></a>Abfragen der Liste mit den Images in einem Konto

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images | 

### <a name="description"></a>BESCHREIBUNG
Dient zum Abfragen der Liste mit den Images in einem Konto. Sie können die Ergebnisliste nach Manifest-ID und Name filtern. Wenn kein Filter übergeben wird, werden mit der Abfrage alle Images des Kontos aufgelistet. Die zurückgegebene Liste wird paginiert, und die Anzahl von Elementen auf jeder Seite ist ein optionaler Parameter.

### <a name="parameters"></a>Parameter
| NAME | Enthalten in | BESCHREIBUNG | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | Zeichenfolge |
| resourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | Zeichenfolge |
| .<Name der Region | path | Name des Modellverwaltungskontos | Ja | Zeichenfolge |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | Zeichenfolge |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | Zeichenfolge |
| manifestId | query | Manifest-ID | Nein  | Zeichenfolge |
| manifestName | query | Manifestname | Nein  | Zeichenfolge |
| count | query | Anzahl von Elementen, die pro Seite abgerufen werden sollen | Nein  | Zeichenfolge |
| $skipToken | query | Fortsetzungstoken zum Abrufen der nächsten Seite | Nein  | Zeichenfolge |

### <a name="responses"></a>Antworten
| Code | BESCHREIBUNG | Schema |
|--------------------|--------------------|--------------------|
| 200 | Erfolgreich. | [PaginatedImageList](#paginatedimagelist) |
| die Standardeinstellung | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse) |

## <a name="get-image-details"></a>Abrufen von Imagedetails

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images/{id} | 

### <a name="description"></a>BESCHREIBUNG
Dient zum Abrufen eines Image nach der ID.

### <a name="parameters"></a>Parameter
| NAME | Enthalten in | BESCHREIBUNG | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | Zeichenfolge |
| resourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | Zeichenfolge |
| .<Name der Region | path | Name des Modellverwaltungskontos | Ja | Zeichenfolge |
| id | path | Image-ID | Ja | Zeichenfolge |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | Zeichenfolge |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | Zeichenfolge |

### <a name="responses"></a>Antworten
| Code | BESCHREIBUNG | Schema |
|--------------------|--------------------|--------------------|
| 200 | Erfolgreich. | [Image](#image) |
| die Standardeinstellung | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse) |


## <a name="create-a-service"></a>Erstellen von Diensten

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| POST |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services | 

### <a name="description"></a>BESCHREIBUNG
Dient zum Erstellen eines Diensts aus einem Image.

### <a name="parameters"></a>Parameter
| NAME | Enthalten in | BESCHREIBUNG | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | Zeichenfolge |
| resourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | Zeichenfolge |
| .<Name der Region | path | Name des Modellverwaltungskontos | Ja | Zeichenfolge |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | Zeichenfolge |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | Zeichenfolge |
| serviceRequest | body | Nutzlast, die zum Erstellen eines Diensts verwendet wird | Ja | [ServiceCreateRequest](#servicecreaterequest) |

### <a name="responses"></a>Antworten
| Code | BESCHREIBUNG | Header | Schema |
|--------------------|--------------------|--------------------|--------------------|
| 202 | Standort-URL für den asynchronen Vorgang. Mit einem GET-Aufruf können Sie den Status der Aufgabe für die Diensterstellung anzeigen. | Operation-Location |
| 409 | Ein Dienst mit dem angegebenen Namen ist bereits vorhanden. |
| die Standardeinstellung | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-services-in-an-account"></a>Abfragen der Liste mit den Diensten in einem Konto

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services | 

### <a name="description"></a>BESCHREIBUNG
Dient zum Abfragen der Liste mit den Diensten in einem Konto. Sie können die Ergebnisliste nach Modellname/-ID, Manifestname/-ID, Image-ID, Dienstname oder Machine Learning-Computeressourcen-ID filtern. Wenn kein Filter übergeben wird, werden mit der Abfrage alle Dienste des Kontos aufgeführt. Die zurückgegebene Liste wird paginiert, und die Anzahl von Elementen auf jeder Seite ist ein optionaler Parameter.

### <a name="parameters"></a>Parameter
| NAME | Enthalten in | BESCHREIBUNG | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | Zeichenfolge |
| resourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | Zeichenfolge |
| .<Name der Region | path | Name des Modellverwaltungskontos | Ja | Zeichenfolge |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | Zeichenfolge |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | Zeichenfolge |
| serviceName | query | Dienstname | Nein  | Zeichenfolge |
| modelId | query | Modellname | Nein  | Zeichenfolge |
| modelName | query | Modell-ID | Nein  | Zeichenfolge |
| manifestId | query | Manifest-ID | Nein  | Zeichenfolge |
| manifestName | query | Manifestname | Nein  | Zeichenfolge |
| imageId | query | Image-ID | Nein  | Zeichenfolge |
| computeResourceId | query | Machine Learning-Computeressourcen-ID | Nein  | Zeichenfolge |
| count | query | Anzahl von Elementen, die pro Seite abgerufen werden sollen | Nein  | Zeichenfolge |
| $skipToken | query | Fortsetzungstoken zum Abrufen der nächsten Seite | Nein  | Zeichenfolge |

### <a name="responses"></a>Antworten
| Code | BESCHREIBUNG | Schema |
|--------------------|--------------------|--------------------|
| 200 | Erfolgreich. | [PaginatedServiceList](#paginatedservicelist) |
| die Standardeinstellung | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse) |

## <a name="get-service-details"></a>Abrufen von Dienstdetails

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id} | 

### <a name="description"></a>BESCHREIBUNG
Dient zum Abrufen eines Diensts nach der ID.

### <a name="parameters"></a>Parameter
| NAME | Enthalten in | BESCHREIBUNG | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | Zeichenfolge |
| resourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | Zeichenfolge |
| .<Name der Region | path | Name des Modellverwaltungskontos | Ja | Zeichenfolge |
| id | path | Objekt-ID | Ja | Zeichenfolge |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | Zeichenfolge |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | Zeichenfolge |

### <a name="responses"></a>Antworten
| Code | BESCHREIBUNG | Schema |
|--------------------|--------------------|--------------------|
| 200 | Erfolgreich. | [ServiceResponse](#serviceresponse) |
| die Standardeinstellung | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse)

## <a name="update-a-service"></a>Aktualisieren eines Diensts

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| PUT |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id} | 

### <a name="description"></a>BESCHREIBUNG
Dient zum Aktualisieren eines vorhandenen Diensts.

### <a name="parameters"></a>Parameter
| NAME | Enthalten in | BESCHREIBUNG | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | Zeichenfolge |
| resourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | Zeichenfolge |
| .<Name der Region | path | Name des Modellverwaltungskontos | Ja | Zeichenfolge |
| id | path | Objekt-ID | Ja | Zeichenfolge |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | Zeichenfolge |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | Zeichenfolge |
| serviceUpdateRequest | body | Nutzlast, die zum Aktualisieren eines vorhandenen Diensts verwendet wird | Ja |  [ServiceUpdateRequest](#serviceupdaterequest) |

### <a name="responses"></a>Antworten
| Code | BESCHREIBUNG | Header | Schema |
|--------------------|--------------------|--------------------|--------------------|
| 202 | Standort-URL für den asynchronen Vorgang. Mit einem GET-Aufruf können Sie den Status der Aufgabe für die Dienstaktualisierung anzeigen. | Operation-Location |
| 404 | Der Dienst mit der angegebenen ID ist nicht vorhanden. |
| die Standardeinstellung | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse)

## <a name="delete-a-service"></a>Löschen von Diensten

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| DELETE |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id} | 

### <a name="description"></a>BESCHREIBUNG
Dient zum Löschen eines Diensts.

### <a name="parameters"></a>Parameter
| NAME | Enthalten in | BESCHREIBUNG | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | Zeichenfolge |
| resourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | Zeichenfolge |
| .<Name der Region | path | Name des Modellverwaltungskontos | Ja | Zeichenfolge |
| id | path | Objekt-ID | Ja | Zeichenfolge |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | Zeichenfolge |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | Zeichenfolge |

### <a name="responses"></a>Antworten
| Code | BESCHREIBUNG | Schema |
|--------------------|--------------------|--------------------|
| 200 | Erfolgreich. |  |
| 204 | Der Dienst mit der angegebenen ID ist nicht vorhanden. |
| die Standardeinstellung | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse)

## <a name="get-service-keys"></a>Abrufen von Dienstschlüsseln

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id}/keys | 

### <a name="description"></a>BESCHREIBUNG
Dient zum Abrufen von Dienstschlüsseln.

### <a name="parameters"></a>Parameter
| NAME | Enthalten in | BESCHREIBUNG | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | Zeichenfolge |
| resourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | Zeichenfolge |
| .<Name der Region | path | Name des Modellverwaltungskontos | Ja | Zeichenfolge |
| id | path | Dienst-ID | Ja | Zeichenfolge |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | Zeichenfolge |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | Zeichenfolge |

### <a name="responses"></a>Antworten
| Code | BESCHREIBUNG | Schema |
|--------------------|--------------------|--------------------|
| 200 | Erfolgreich. | [AuthKeys](#authkeys)
| die Standardeinstellung | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse)

## <a name="regenerate-service-keys"></a>Erneutes Generieren von Dienstschlüsseln

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| POST |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id}/keys | 

### <a name="description"></a>BESCHREIBUNG
Dient zum erneuten Generieren und Zurückgeben von Dienstschlüsseln.

### <a name="parameters"></a>Parameter
| NAME | Enthalten in | BESCHREIBUNG | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | Zeichenfolge |
| resourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | Zeichenfolge |
| .<Name der Region | path | Name des Modellverwaltungskontos | Ja | Zeichenfolge |
| id | path | Dienst-ID | Ja | Zeichenfolge |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | Zeichenfolge |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | Zeichenfolge |
| regenerateKeyRequest | body | Nutzlast, die zum Aktualisieren eines vorhandenen Diensts verwendet wird | Ja | [ServiceRegenerateKeyRequest](#serviceregeneratekeyrequest) |

### <a name="responses"></a>Antworten
| Code | BESCHREIBUNG | Schema |
|--------------------|--------------------|--------------------|
| 200 | Erfolgreich. | [AuthKeys](#authkeys)
| die Standardeinstellung | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse)

## <a name="query-the-list-of-deployments-in-an-account"></a>Abfragen der Liste mit den Bereitstellungen eines Kontos

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/deployments | 

### <a name="description"></a>BESCHREIBUNG
Dient zum Abfragen der Liste mit den Bereitstellungen eines Kontos. Sie können die Ergebnisliste nach der Dienst-ID filtern. Hierbei werden nur die Bereitstellungen zurückgegeben, die für den jeweiligen Dienst erstellt werden. Wenn kein Filter übergeben wird, werden mit der Abfrage alle Bereitstellungen des Kontos aufgelistet.

### <a name="parameters"></a>Parameter
| NAME | Enthalten in | BESCHREIBUNG | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | Zeichenfolge |
| resourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | Zeichenfolge |
| .<Name der Region | path | Name des Modellverwaltungskontos | Ja | Zeichenfolge |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | Zeichenfolge |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | Zeichenfolge |
| serviceId | query | Dienst-ID | Nein  | Zeichenfolge |

### <a name="responses"></a>Antworten
| Code | BESCHREIBUNG | Schema |
|--------------------|--------------------|--------------------|
| 200 | Erfolgreich. | [DeploymentList](#deploymentlist) |
| die Standardeinstellung | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse)

## <a name="get-deployment-details"></a>Abrufen von Bereitstellungsdetails

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/deployments/{id} | 

### <a name="description"></a>BESCHREIBUNG
Dient zum Abrufen der Bereitstellung nach der ID.

### <a name="parameters"></a>Parameter
| NAME | Enthalten in | BESCHREIBUNG | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | Zeichenfolge |
| resourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | Zeichenfolge |
| .<Name der Region | path | Name des Modellverwaltungskontos | Ja | Zeichenfolge |
| id | path | Bereitstellungs-ID | Ja | Zeichenfolge |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | Zeichenfolge |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | Zeichenfolge |

### <a name="responses"></a>Antworten
| Code | BESCHREIBUNG | Schema |
|--------------------|--------------------|--------------------|
| 200 | Erfolgreich. | [Bereitstellung](#deployment) |
| die Standardeinstellung | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse)

## <a name="get-operation-details"></a>Abrufen von Vorgangsdetails

### <a name="request"></a>Anforderung
| Methode | Anforderungs-URI |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/operations/{id} | 

### <a name="description"></a>BESCHREIBUNG
Dient zum Abrufen des Status von asynchronen Vorgängen nach Vorgangs-ID.

### <a name="parameters"></a>Parameter
| NAME | Enthalten in | BESCHREIBUNG | Erforderlich | Schema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | path | Die Azure-Abonnement-ID. | Ja | Zeichenfolge |
| resourceGroupName | path | Name der Ressourcengruppe, in der sich das Modellverwaltungskonto befindet | Ja | Zeichenfolge |
| .<Name der Region | path | Name des Modellverwaltungskontos | Ja | Zeichenfolge |
| id | path | Vorgangs-ID | Ja | Zeichenfolge |
| api-version | query | Version der Microsoft.Machine.Learning-Ressourcenanbieter-API, die verwendet werden soll | Ja | Zeichenfolge |
| Autorisierung | Header | Autorisierungstoken. Es sollte „Bearer XXXXXX“ oder ähnlich lauten. | Ja | Zeichenfolge |

### <a name="responses"></a>Antworten
| Code | BESCHREIBUNG | Schema |
|--------------------|--------------------|--------------------|
| 200 | Erfolgreich. | [OperationStatus](#asyncoperationstatus) |
| die Standardeinstellung | Fehlerantwort, die beschreibt, warum der Vorgang nicht erfolgreich durchgeführt werden konnte. | [ErrorResponse](#errorresponse)



<a name="definitions"></a>
## <a name="definitions"></a>Definitionen

<a name="asset"></a>
### <a name="asset"></a>Asset
Das Assetobjekt, das für die Erstellung des Docker-Image benötigt wird.


|NAME|BESCHREIBUNG|Schema|
|---|---|---|
|**id**  <br>*optional*|Asset-ID|Zeichenfolge|
|**mimeType**  <br>*optional*|MIME-Typ des Modellinhalts. Weitere Informationen zum MIME-Typ finden Sie in der [Liste mit den IANA-Medientypen](https://www.iana.org/assignments/media-types/media-types.xhtml).|Zeichenfolge|
|**unpack**  <br>*optional*|Gibt an, wo der Inhalt während der Erstellung des Docker-Image entpackt werden muss.|boolean|
|**url**  <br>*optional*|URL des Asset-Standorts|Zeichenfolge|


<a name="asyncoperationstate"></a>
### <a name="asyncoperationstate"></a>AsyncOperationState
Der Status des asynchronen Vorgangs.

*Typ*: enum (NotStarted, Running, Cancelled, Succeeded, Failed)


<a name="asyncoperationstatus"></a>
### <a name="asyncoperationstatus"></a>AsyncOperationStatus
Der Vorgangsstatus.


|NAME|BESCHREIBUNG|Schema|
|---|---|---|
|**createdTime**  <br>*optional*  <br>*schreibgeschützt*|Erstellungszeitpunkt (UTC) des asynchronen Vorgangs.|string (Datum/Uhrzeit)|
|**endTime**  <br>*optional*  <br>*schreibgeschützt*|Endzeitpunkt (UTC) des asynchronen Vorgangs.|string (Datum/Uhrzeit)|
|**error**  <br>*optional*||[ErrorResponse](#errorresponse)|
|**id**  <br>*optional*|ID des asynchronen Vorgangs.|Zeichenfolge|
|**operationType**  <br>*optional*|Typ des asynchronen Vorgangs.|enum (Image, Dienst)|
|**resourceLocation**  <br>*optional*|Per asynchronem Vorgang erstellte oder aktualisierte Ressource.|Zeichenfolge|
|**state**  <br>*optional*||[AsyncOperationState](#asyncoperationstate)|


<a name="authkeys"></a>
### <a name="authkeys"></a>AuthKeys
Die Authentifizierungsschlüssel für einen Dienst.


|NAME|BESCHREIBUNG|Schema|
|---|---|---|
|**primaryKey**  <br>*optional*|Primärschlüssel|Zeichenfolge|
|**secondaryKey**  <br>*optional*|Sekundärschlüssel|Zeichenfolge|


<a name="autoscaler"></a>
### <a name="autoscaler"></a>AutoScaler
Die Einstellungen für die automatische Skalierung.


|NAME|BESCHREIBUNG|Schema|
|---|---|---|
|**autoscaleEnabled**  <br>*optional*|Dient zum Aktivieren oder Deaktivieren der automatischen Skalierung.|boolean|
|**maxReplicas**  <br>*optional*|Maximale Anzahl von Pod-Replikaten für das zentrale Hochskalieren.  <br>**Mindestwert**: `1`|integer|
|**minReplicas**  <br>*optional*|Minimale Anzahl von Pod-Replikaten für das zentrale Herunterskalieren.  <br>**Mindestwert**: `0`|integer|
|**refreshPeriodInSeconds**  <br>*optional*|Aktualisierungszeitpunkt für den Trigger der automatischen Skalierung.  <br>**Mindestwert**: `1`|integer|
|**targetUtilization**  <br>*optional*|Auslastungsprozentsatz, bei dem die automatische Skalierung ausgelöst wird.  <br>**Mindestwert**: `0`  <br>**Maximalwert**: `100`|integer|


<a name="computeresource"></a>
### <a name="computeresource"></a>ComputeResource
Die Machine Learning-Computeressource.


|NAME|BESCHREIBUNG|Schema|
|---|---|---|
|**id**  <br>*optional*|Ressourcen-ID|Zeichenfolge|
|**type**  <br>*optional*|Ressourcentyp.|enum (Cluster)|


<a name="containerresourcereservation"></a>
### <a name="containerresourcereservation"></a>ContainerResourceReservation
Konfiguration zum Reservieren von Ressourcen für einen Container im Cluster.


|NAME|BESCHREIBUNG|Schema|
|---|---|---|
|**cpu**  <br>*optional*|Gibt die CPU-Reservierung an. Format für Kubernetes: siehe [Meaning of CPU](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#meaning-of-cpu) (Bedeutung der CPU).|Zeichenfolge|
|**memory**  <br>*optional*|Gibt die Speicherreservierung an. Format für Kubernetes: siehe [Meaning of memory](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#meaning-of-memory) (Bedeutung des Arbeitsspeichers).|Zeichenfolge|


<a name="deployment"></a>
### <a name="deployment"></a>Bereitstellung
Eine Instanz einer Azure Machine Learning-Bereitstellung.


|NAME|BESCHREIBUNG|Schema|
|---|---|---|
|**createdAt**  <br>*optional*  <br>*schreibgeschützt*|Erstellungszeitpunkt (UTC) der Bereitstellung|string (Datum/Uhrzeit)|
|**expiredAt**  <br>*optional*  <br>*schreibgeschützt*|Ablaufzeitpunkt (UTC) der Bereitstellung|string (Datum/Uhrzeit)|
|**id**  <br>*optional*|Bereitstellungs-ID|Zeichenfolge|
|**imageId**  <br>*optional*|Image-ID, die dieser Bereitstellung zugeordnet ist|Zeichenfolge|
|**serviceName**  <br>*optional*|Dienstname|Zeichenfolge|
|**status**  <br>*optional*|Aktueller Status der Bereitstellung|Zeichenfolge|


<a name="deploymentlist"></a>
### <a name="deploymentlist"></a>DeploymentList
Ein Array mit Bereitstellungsobjekten.

*Typ*: <[Deployment](#deployment)>-Array


<a name="errordetail"></a>
### <a name="errordetail"></a>ErrorDetail
Fehlerdetails zum Modellverwaltungsdienst.


|NAME|BESCHREIBUNG|Schema|
|---|---|---|
|**code**  <br>*erforderlich*|Fehlercode|Zeichenfolge|
|**message**  <br>*erforderlich*|Fehlermeldung.|Zeichenfolge|


<a name="errorresponse"></a>
### <a name="errorresponse"></a>ErrorResponse
Ein Fehlerobjekt des Modellverwaltungsdiensts.


|NAME|BESCHREIBUNG|Schema|
|---|---|---|
|**code**  <br>*erforderlich*|Fehlercode|Zeichenfolge|
|**details**  <br>*optional*|Array mit Fehlerdetailobjekten|<[ErrorDetail](#errordetail)>-Array|
|**message**  <br>*erforderlich*|Fehlermeldung.|Zeichenfolge|
|**statusCode**  <br>*optional*|HTTP-Statuscode.|integer|


<a name="image"></a>
### <a name="image"></a>Image
Das Azure Machine Learning-Image.


|NAME|BESCHREIBUNG|Schema|
|---|---|---|
|**computeResourceId**  <br>*optional*|ID der Umgebung, die in der Machine Learning-Computeressource erstellt wurde|Zeichenfolge|
|**createdTime**  <br>*optional*|Erstellungszeitpunkt (UTC) des Image|string (Datum/Uhrzeit)|
|**creationState**  <br>*optional*||[AsyncOperationState](#asyncoperationstate)|
|**description**  <br>*optional*|Image-Beschreibungstext|Zeichenfolge|
|**error**  <br>*optional*||[ErrorResponse](#errorresponse)|
|**id**  <br>*optional*|Image-ID|Zeichenfolge|
|**imageBuildLogUri**  <br>*optional*|URI der hochgeladenen Protokolle aus dem Image-Build|Zeichenfolge|
|**imageLocation**  <br>*optional*|Azure Container Registry-Standortzeichenfolge für das erstellte Image|Zeichenfolge|
|**imageType**  <br>*optional*||[ImageType](#imagetype)|
|**manifest**  <br>*optional*||[Manifest](#manifest)|
|**name**  <br>*optional*|Imagename|Zeichenfolge|
|**version**  <br>*optional*|Vom Modellverwaltungsdienst festgelegte Imageversion|integer|


<a name="imagerequest"></a>
### <a name="imagerequest"></a>ImageRequest
Eine Anforderung zur Erstellung eines Azure Machine Learning-Image.


|NAME|BESCHREIBUNG|Schema|
|---|---|---|
|**computeResourceId**  <br>*erforderlich*|ID der Umgebung, die in der Machine Learning-Computeressource erstellt wurde|Zeichenfolge|
|**description**  <br>*optional*|Image-Beschreibungstext|Zeichenfolge|
|**imageType**  <br>*erforderlich*||[ImageType](#imagetype)|
|**manifestId**  <br>*erforderlich*|ID des Manifests, aus dem das Image erstellt wird|Zeichenfolge|
|**name**  <br>*erforderlich*|Imagename|Zeichenfolge|


<a name="imagetype"></a>
### <a name="imagetype"></a>ImageType
Gibt den Typ des Image an.

*Typ*: enum (Docker)


<a name="manifest"></a>
### <a name="manifest"></a>Manifest
Das Azure Machine Learning-Manifest.


|NAME|BESCHREIBUNG|Schema|
|---|---|---|
|**assets**  <br>*erforderlich*|Liste mit Assets|<[Asset](#asset)>-Array|
|**createdTime**  <br>*optional*  <br>*schreibgeschützt*|Erstellungszeitpunkt (UTC) des Manifests|string (Datum/Uhrzeit)|
|**description**  <br>*optional*|Beschreibungstext des Manifests|Zeichenfolge|
|**driverProgram**  <br>*erforderlich*|Treiberprogramm des Manifests|Zeichenfolge|
|**id**  <br>*optional*|Manifest-ID|Zeichenfolge|
|**modelIds**  <br>*optional*|Liste mit den Modell-IDs der registrierten Modelle. Die Anforderung schlägt fehl, wenn keines der enthaltenen Modelle registriert ist.|<string>-Array|
|**modelType**  <br>*optional*|Gibt an, dass die Modelle bereits beim Modellverwaltungsdienst registriert sind.|enum (registriert)|
|**name**  <br>*erforderlich*|Manifestname|Zeichenfolge|
|**targetRuntime**  <br>*erforderlich*||[TargetRuntime](#targetruntime)|
|**version**  <br>*optional*  <br>*schreibgeschützt*|Vom Modellverwaltungsdienst zugewiesene Manifestversion|integer|
|**webserviceType**  <br>*optional*|Gibt den gewünschten Typ von Webdienst an, der über das Manifest erstellt wird.|enum (Echtzeit)|


<a name="model"></a>
### <a name="model"></a>Modell
Eine Instanz eines Azure Machine Learning-Modells.


|NAME|BESCHREIBUNG|Schema|
|---|---|---|
|**createdAt**  <br>*optional*  <br>*schreibgeschützt*|Erstellungszeitpunkt (UTC) des Modells|string (Datum/Uhrzeit)|
|**description**  <br>*optional*|Beschreibungstext des Modells|Zeichenfolge|
|**id**  <br>*optional*  <br>*schreibgeschützt*|Modell-ID|Zeichenfolge|
|**mimeType**  <br>*erforderlich*|MIME-Typ des Modellinhalts. Weitere Informationen zum MIME-Typ finden Sie in der [Liste mit den IANA-Medientypen](https://www.iana.org/assignments/media-types/media-types.xhtml).|Zeichenfolge|
|**name**  <br>*erforderlich*|Modellname|Zeichenfolge|
|**Tags**  <br>*optional*|Liste mit Modelltags|<string>-Array|
|**unpack**  <br>*optional*|Gibt an, ob das Modell während der Erstellung des Docker-Image entpackt werden muss.|boolean|
|**url**  <br>*erforderlich*|Die URL des Modells. Normalerweise wird hier eine SAS-URL (Shared Access Signature) angegeben.|Zeichenfolge|
|**version**  <br>*optional*  <br>*schreibgeschützt*|Vom Modellverwaltungsdienst zugewiesene Modellversion|integer|


<a name="modeldatacollection"></a>
### <a name="modeldatacollection"></a>ModelDataCollection
Die Informationen zur Modelldatensammlung.


|NAME|BESCHREIBUNG|Schema|
|---|---|---|
|**eventHubEnabled**  <br>*optional*|Dient zum Aktivieren eines Event Hub für einen Dienst.|boolean|
|**storageEnabled**  <br>*optional*|Dient zum Aktivieren des Speichers für einen Dienst.|boolean|


<a name="paginatedimagelist"></a>
### <a name="paginatedimagelist"></a>PaginatedImageList
Eine paginierte Liste mit Images.


|NAME|BESCHREIBUNG|Schema|
|---|---|---|
|**nextLink**  <br>*optional*|Fortsetzungslink (absoluter URI) auf die nächste Seite mit Ergebnissen in der Liste|Zeichenfolge|
|**value**  <br>*optional*|Array mit Modellobjekten|<[Image](#image)>-Array|


<a name="paginatedmanifestlist"></a>
### <a name="paginatedmanifestlist"></a>PaginatedManifestList
Eine paginierte Liste mit Manifesten.


|NAME|BESCHREIBUNG|Schema|
|---|---|---|
|**nextLink**  <br>*optional*|Fortsetzungslink (absoluter URI) auf die nächste Seite mit Ergebnissen in der Liste|Zeichenfolge|
|**value**  <br>*optional*|Array mit Manifestobjekten|<[Manifest](#manifest)>-Array|


<a name="paginatedmodellist"></a>
### <a name="paginatedmodellist"></a>PaginatedModelList
Eine paginierte Liste mit Modellen.


|NAME|BESCHREIBUNG|Schema|
|---|---|---|
|**nextLink**  <br>*optional*|Fortsetzungslink (absoluter URI) auf die nächste Seite mit Ergebnissen in der Liste|Zeichenfolge|
|**value**  <br>*optional*|Array mit Modellobjekten|<[Model](#model)>-Array|


<a name="paginatedservicelist"></a>
### <a name="paginatedservicelist"></a>PaginatedServiceList
Eine paginierte Liste mit Diensten.


|NAME|BESCHREIBUNG|Schema|
|---|---|---|
|**nextLink**  <br>*optional*|Fortsetzungslink (absoluter URI) auf die nächste Seite mit Ergebnissen in der Liste|Zeichenfolge|
|**value**  <br>*optional*|Array mit Dienstobjekten|<[ServiceResponse](#serviceresponse)>-Array|


<a name="servicecreaterequest"></a>
### <a name="servicecreaterequest"></a>ServiceCreateRequest
Eine Anforderung zum Erstellen eines Diensts.


|NAME|BESCHREIBUNG|Schema|
|---|---|---|
|**appInsightsEnabled**  <br>*optional*|Dient zum Aktivieren von Application Insights für einen Dienst.|boolean|
|**autoScaler**  <br>*optional*||[AutoScaler](#autoscaler)|
|**computeResource**  <br>*erforderlich*||[ComputeResource](#computeresource)|
|**containerResourceReservation**  <br>*optional*||[ContainerResourceReservation](#containerresourcereservation)|
|**dataCollection**  <br>*optional*||[ModelDataCollection](#modeldatacollection)|
|**imageId**  <br>*erforderlich*|Image zum Erstellen des Diensts|Zeichenfolge|
|**maxConcurrentRequestsPerContainer**  <br>*optional*|Maximale Anzahl von gleichzeitigen Anforderungen.  <br>**Mindestwert**: `1`|integer|
|**name**  <br>*erforderlich*|Dienstname|Zeichenfolge|
|**numReplicas**  <br>*optional*|Anzahl von Pod-Replikaten, die jeweils ausgeführt werden. Kann nicht angegeben werden, wenn die automatische Skalierung aktiviert ist.  <br>**Mindestwert**: `0`|integer|


<a name="serviceregeneratekeyrequest"></a>
### <a name="serviceregeneratekeyrequest"></a>ServiceRegenerateKeyRequest
Eine Anforderung zum erneuten Generieren eines Schlüssels für einen Dienst.


|NAME|BESCHREIBUNG|Schema|
|---|---|---|
|**keyType**  <br>*optional*|Gibt an, welcher Schlüssel neu generiert werden soll.|enum (Primär, Sekundär)|


<a name="serviceresponse"></a>
### <a name="serviceresponse"></a>ServiceResponse
Der detaillierte Status des Diensts.


|NAME|BESCHREIBUNG|Schema|
|---|---|---|
|**createdAt**  <br>*optional*|Erstellungszeitpunkt (UTC) des Diensts|string (Datum/Uhrzeit)|
|**ID**  <br>*optional*|Dienst-ID|Zeichenfolge|
|**image**  <br>*optional*||[Image](#image)|
|**manifest**  <br>*optional*||[Manifest](#manifest)|
|**models**  <br>*optional*|Liste mit Modellen|<[Model](#model)>-Array|
|**name**  <br>*optional*|Dienstname|Zeichenfolge|
|**scoringUri**  <br>*optional*|URI für die Bewertung des Diensts|Zeichenfolge|
|**state**  <br>*optional*||[AsyncOperationState](#asyncoperationstate)|
|**updatedAt**  <br>*optional*|Zeitpunkt (UTC) der letzten Aktualisierung|string (Datum/Uhrzeit)|
|**appInsightsEnabled**  <br>*optional*|Dient zum Aktivieren von Application Insights für einen Dienst.|boolean|
|**autoScaler**  <br>*optional*||[AutoScaler](#autoscaler)|
|**computeResource**  <br>*erforderlich*||[ComputeResource](#computeresource)|
|**containerResourceReservation**  <br>*optional*||[ContainerResourceReservation](#containerresourcereservation)|
|**dataCollection**  <br>*optional*||[ModelDataCollection](#modeldatacollection)|
|**maxConcurrentRequestsPerContainer**  <br>*optional*|Maximale Anzahl von gleichzeitigen Anforderungen.  <br>**Mindestwert**: `1`|integer|
|**numReplicas**  <br>*optional*|Anzahl von Pod-Replikaten, die jeweils ausgeführt werden. Kann nicht angegeben werden, wenn die automatische Skalierung aktiviert ist.  <br>**Mindestwert**: `0`|integer|
|**error**  <br>*optional*||[ErrorResponse](#errorresponse)|


<a name="serviceupdaterequest"></a>
### <a name="serviceupdaterequest"></a>ServiceUpdateRequest
Eine Anforderung zum Aktualisieren eines Diensts.


|NAME|BESCHREIBUNG|Schema|
|---|---|---|
|**appInsightsEnabled**  <br>*optional*|Dient zum Aktivieren von Application Insights für einen Dienst.|boolean|
|**autoScaler**  <br>*optional*||[AutoScaler](#autoscaler)|
|**containerResourceReservation**  <br>*optional*||[ContainerResourceReservation](#containerresourcereservation)|
|**dataCollection**  <br>*optional*||[ModelDataCollection](#modeldatacollection)|
|**imageId**  <br>*optional*|Image zum Erstellen des Diensts|Zeichenfolge|
|**maxConcurrentRequestsPerContainer**  <br>*optional*|Maximale Anzahl von gleichzeitigen Anforderungen.  <br>**Mindestwert**: `1`|integer|
|**numReplicas**  <br>*optional*|Anzahl von Pod-Replikaten, die jeweils ausgeführt werden. Kann nicht angegeben werden, wenn die automatische Skalierung aktiviert ist.  <br>**Mindestwert**: `0`|integer|


<a name="targetruntime"></a>
### <a name="targetruntime"></a>TargetRuntime
Der Typ der Ziellaufzeit.


|NAME|BESCHREIBUNG|Schema|
|---|---|---|
|**properties**  <br>*erforderlich*||<string, string>-Zuordnung|
|**runtimeType**  <br>*erforderlich*|Gibt die Laufzeit an.|enum (SparkPython, Python)|

