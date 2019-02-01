---
title: Auflisten aller Azure Import/Export-Aufträge | Microsoft Docs
description: Erfahren Sie, wie Sie alle Aufträge des Azure Import/Export-Diensts in einem Abonnement auflisten.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 017208c6fca7c4e55a45070f5aa21652e83e7e8d
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55462918"
---
# <a name="enumerating-jobs-in-the-azure-importexport-service"></a>Auflisten von Aufträgen im Azure Import/Export-Dienst
Rufen Sie den Vorgang [List Jobs](/rest/api/storageimportexport/jobs#Jobs_List) auf, um alle Aufträge in einem Abonnement aufzulisten. `List Jobs` gibt eine Liste der Aufträge sowie die folgenden Attribute zurück:

-   Typ des Auftrags (Import oder Export)

-   Aktueller Auftragsstatus

-   Zugeordnetes Speicherkonto des Auftrags

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden der REST-API des Import/Export-Diensts](storage-import-export-using-the-rest-api.md)
