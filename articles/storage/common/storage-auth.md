---
title: Autorisierung des Zugriffs auf Azure Storage | Microsoft-Dokumentation
description: Weitere Informationen über die verschiedenen Methoden zum Autorisieren des Zugriffs auf Azure Storage, z.B. Azure Active Directory, Authentifizierung mit gemeinsam verwendetem Schlüssel oder Shared Access Signatures (SAS).
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 05/18/2018
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 8289f358bbfc2fe6229abf4eefc7bc8e57f05d73
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55464312"
---
# <a name="authorizing-access-to-azure-storage"></a>Autorisierung des Zugriffs auf Azure Storage

Immer dann, wenn Sie auf Daten in Ihrem Speicherkonto zugreifen, sendet der Client eine Anforderung über HTTP/HTTPS an Azure Storage. Jede Anforderung an eine sichere Ressource muss autorisiert sein, damit der Dienst sicherstellt, dass der Client über die erforderlichen Berechtigungen zum Zugriff auf die Daten verfügt. Azure Storage bietet diese Optionen für die Autorisierung des Zugriffs auf sichere Ressourcen:

- **Integration von Azure Active Directory (Azure AD) (Vorschau)** für Blobs und Warteschlangen. Azure AD bietet rollenbasierte Zugriffssteuerung (RBAC, Role-Based Access Control) für eine präzise Steuerung des Zugriffs eines Clients auf Ressourcen in einem Speicherkonto. Weitere Informationen finden Sie unter [Authentifizieren des Zugriffs auf Azure Storage mit Azure Active Directory (Vorschau)](storage-auth-aad.md).
- **Autorisierung mit gemeinsam verwendetem Schlüssel** für Blobs, Dateien, Warteschlangen und Tabellen. Ein Client mit gemeinsam verwendetem Schlüssel übergibt mit jeder Anforderung einen Header, der mit dem Speicherkonto-Zugriffsschlüssel signiert wird. Weitere Informationen finden Sie unter [Authentifizieren mit gemeinsam verwendetem Schlüssel](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-shared-key/).
- **Shared Access Signatures** für Blobs, Dateien, Warteschlangen und Tabellen. Shared Access Signatures (SAS) ermöglichen den begrenzten delegierten Zugriff auf Ressourcen in einem Speicherkonto. Einschränkungen des Zeitintervalls, für das die Signatur gültig ist, oder von Berechtigungen, die sie gewährt, bieten Flexibilität beim Verwalten des Zugriffs. Weitere Informationen finden Sie unter [Verwenden von Shared Access Signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md).
- **Anonymer öffentlicher Lesezugriff** für Container und Blobs. Autorisierung ist nicht erforderlich. Weitere Informationen finden Sie unter [Verwalten des anonymen Lesezugriffs auf Container und Blobs](../blobs/storage-manage-access-to-resources.md).  

Standardmäßig werden alle Ressourcen in Azure Storage gesichert und stehen nur dem Kontobesitzer zur Verfügung. Obwohl Sie jede der oben beschriebenen Autorisierungsstrategien verwenden können, um Clients Zugriff auf Ressourcen in Ihrem Speicherkonto zu gewähren, empfiehlt Microsoft im Sinne optimaler Sicherheit und Benutzerfreundlichkeit nach Möglichkeit die Verwendung von Azure AD. 



