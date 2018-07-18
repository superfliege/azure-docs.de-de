---
title: Sichern von Azure Data Lake Analytics für mehrere Benutzer
description: Erfahren Sie, wie Sie mehrere Benutzer zum Ausführen von Aufträgen in Azure Data Lake Analytics konfigurieren.
ms.service: data-lake-analytics
services: data-lake-analytics
author: matt1883
ms.author: mahi
manager: kfile
editor: jasonwhowell
ms.topic: conceptual
ms.date: 05/30/2018
ms.openlocfilehash: 1d92e6d0e619584dedcbc9a66450c25dd1ac8b75
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34702296"
---
# <a name="configure-user-access-to-job-information-to-job-information-in-azure-data-lake-analytics"></a>Konfigurieren des Benutzerzugriffs auf Auftragsinformationen in Azure Data Lake Analytics 

In Azure Data Lake Analytics können Sie mehrere Benutzerkonten oder Dienstprinzipale verwenden, um Aufträge auszuführen. 

Damit diese Benutzer detaillierte Auftragsinformationen sehen können, müssen sie den Inhalt der Auftragsordner lesen können. Die Auftragsordner befinden sich im `/system/`-Verzeichnis. 

Wenn die nötigen Berechtigungen nicht konfiguriert sind, wird möglicherweise ein Fehler ausgegeben: `Graph data not available - You don't have permissions to access the graph data.` 

## <a name="configure-user-access-to-job-information"></a>Konfigurieren des Benutzerzugriffs auf Auftragsinformationen

Sie können den **Assistenten für das Hinzufügen von Benutzern** verwenden, um die ACLs für die Ordner zu konfigurieren. Weitere Informationen finden Sie unter [Hinzufügen eines neuen Benutzers](data-lake-analytics-manage-use-portal.md#add-a-new-user).

Wenn Sie differenziertere Kontrolle benötigen oder die Berechtigungen verknüpfen müssen, sichern Sie die Ordner wie Folgt:

1. Gewähren Sie Berechtigungen zum **Ausführen** (über die Zugriffssteuerungsliste) für den Stammordner:
   - /
   
2. Gewähren Sie **Ausführungs**- und **Lese**berechtigungen (über die Zugriffssteuerungsliste und eine Standard-ACL) für die Ordner, die die Auftragsordner enthalten. Beispielsweise muss für einen bestimmten Auftrag, der am 25. Mai 2018 ausgeführt wurde, auf die folgenden Ordner zugegriffen werden:
   - /system
   - /system/jobservice
   - /system/jobservice/jobs
   - /system/jobservice/jobs/Usql
   - /system/jobservice/jobs/Usql/2018
   - /system/jobservice/jobs/Usql/2018/05
   - /system/jobservice/jobs/Usql/2018/05/25
   - /system/jobservice/jobs/Usql/2018/05/25/11
   - /system/jobservice/jobs/Usql/2018/05/25/11/01
   - /system/jobservice/jobs/Usql/2018/05/25/11/01/b074bd7a-1448-d879-9d75-f562b101bd3d

## <a name="next-steps"></a>Nächste Schritte
[Hinzufügen eines neuen Benutzers](data-lake-analytics-manage-use-portal.md#add-a-new-user)
