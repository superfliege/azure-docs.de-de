---
title: Includedatei
description: Includedatei
services: service-bus-messaging
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 06/29/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 7ed298fc8f13685c4872c4c54ba1e447debea79f
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2018
ms.locfileid: "43702631"
---
Vergewissern Sie sich, dass Sie bereits wie [hier][namespace-how-to] gezeigt einen Service Bus-Namespace erstellt haben.

1. Melden Sie sich beim [Azure-Portal][azure-portal] an.
2. Klicken Sie im linken Navigationsbereich des Portals auf **Service Bus**. (Sollte **Service Bus** nicht angezeigt werden, klicken Sie auf **Alle Dienste**.)
3. Klicken Sie auf den Namespace, in dem die Warteschlange erstellt werden soll. In diesem Fall: **sbnstest1**.
   
    ![Erstellen einer Warteschlange][createqueue1]
4. Klicken Sie im Namespacefenster auf **Warteschlangen** und dann im Fenster **Warteschlangen** auf **+ Warteschlange**.
   
    ![„Warteschlangen“ wählen][createqueue2]
5. Geben Sie den **Warteschlangennamen** ein, und lassen Sie die anderen Werte unverändert.
   
    ![„Neu“ wählen][createqueue3]
6. Klicken Sie unten im Fenster auf **Erstellen**.

[createqueue1]: ./media/service-bus-create-queue-portal/create-queue1.png
[createqueue2]: ./media/service-bus-create-queue-portal/create-queue2.png
[createqueue3]: ./media/service-bus-create-queue-portal/create-queue3.png

[namespace-how-to]: ../articles/service-bus-messaging/service-bus-create-namespace-portal.md
[azure-portal]: https://portal.azure.com
