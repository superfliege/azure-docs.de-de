---
title: Auflisten aller Azure Import/Export-Aufträge | Microsoft Docs
description: Erfahren Sie, wie Sie alle Aufträge des Azure Import/Export-Diensts in einem Abonnement auflisten.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.component: common
ms.openlocfilehash: 69daac71b69969a7ad9acfeb7095053f8138bf53
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2018
ms.locfileid: "39520879"
---
# <a name="enumerating-jobs-in-the-azure-importexport-service"></a>Auflisten von Aufträgen im Azure Import/Export-Dienst
Rufen Sie den Vorgang [List Jobs](/rest/api/storageimportexport/jobs#Jobs_List) auf, um alle Aufträge in einem Abonnement aufzulisten. `List Jobs` gibt eine Liste der Aufträge sowie die folgenden Attribute zurück:

-   Typ des Auftrags (Import oder Export)

-   Aktueller Auftragsstatus

-   Zugeordnetes Speicherkonto des Auftrags

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden der REST-API des Import/Export-Diensts](storage-import-export-using-the-rest-api.md)
