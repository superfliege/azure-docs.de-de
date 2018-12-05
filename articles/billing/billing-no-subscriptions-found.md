---
title: Fehlermeldung „Keine Abonnements gefunden“ beim Versuch der Anmeldung beim Azure-Portal oder Azure-Kontocenter | Microsoft-Dokumentation
description: Bietet die Lösung für das Problem, das beim Anmelden bei Azure-Portal oder Azure-Kontocenter die Fehlermeldung „Keine Abonnements gefunden“ angezeigt wird.
services: ''
documentationcenter: ''
author: genlin
manager: adpick
editor: ''
tags: billing
ms.assetid: d1545298-99db-4941-8e97-f24a06bb7cb6
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 05/11/2018
ms.author: cwatson
ms.openlocfilehash: 584342b3dd223c45495db36ad49d83dece858137
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2018
ms.locfileid: "52581781"
---
# <a name="no-subscriptions-found-error-in-azure-portal-or-azure-account-center"></a>Fehlermeldung „Keine Abonnements gefunden“ in Azure-Portal oder Azure-Kontocenter

Wenn Sie versuchen, sich beim [Azure-Portal](https://portal.azure.com/) oder [Azure-Kontocenter](https://account.windowsazure.com/Subscriptions) anzumelden, erhalten Sie möglicherweise die Fehlermeldung „Keine Abonnements gefunden“. Dieser Artikel bietet eine Lösung dieses Problems.

## <a name="symptom"></a>Symptom

Wenn Sie versuchen, sich beim [Azure-Portal](https://portal.azure.com/) oder [Azure-Kontocenter](https://account.windowsazure.com/Subscriptions) anzumelden, erhalten Sie die Fehlermeldung „Keine Abonnements gefunden“.

## <a name="cause"></a>Ursache

Dieses Problem tritt auf, wenn Sie das falsche Verzeichnis ausgewählt haben oder Ihr Konto nicht über ausreichende Berechtigungen verfügt. 

## <a name="solution"></a>Lösung

### <a name="scenario-1-error-message-is-received-in-the-azure-portalhttpsportalazurecom"></a>Szenario 1: Fehlermeldung wird im [Azure-Portal](https://portal.azure.com) empfangen

So beheben Sie dieses Problem:

* Vergewissern Sie sich, dass das richtige Azure-Verzeichnis ausgewählt ist, indem Sie oben rechts auf Ihr Konto klicken.

  ![Auswählen des Verzeichnisses oben rechts im Azure-Portal](./media/billing-no-subscriptions-found/directory-switch.png)
* Wenn das richtige Azure-Verzeichnis ausgewählt ist, die Fehlermeldung jedoch immer noch ausgegeben wird, [weisen Sie Ihrem Konto die Besitzerrolle zu](../role-based-access-control/role-assignments-portal.md).

### <a name="scenario-2-error-message-is-received-in-the-azure-account-centerhttpsaccountwindowsazurecomsubscriptions"></a>Szenario 2: Fehlermeldung wird im [Azure-Kontocenter](https://account.windowsazure.com/Subscriptions) empfangen

Überprüfen Sie, ob das Konto, das Sie verwendet haben, das des Kontoadministrators ist. Gehen Sie folgendermaßen vor, um zu überprüfen, wer der Kontoadministrator ist:

1. Melden Sie sich bei der [Ansicht „Abonnements“ im Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) an.
1. Wählen Sie das zu überprüfende Abonnement aus, und sehen Sie unter **Einstellungen** nach.
1. Wählen Sie **Eigenschaften** aus. Der Kontoadministrator des Abonnements wird im Feld **Kontoadministrator** angezeigt.  

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
