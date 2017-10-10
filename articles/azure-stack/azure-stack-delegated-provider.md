---
title: Delegieren von Angeboten in Azure Stack | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie es anderen Personen ermöglichen, in Ihrem Auftrag Angebote zu erstellen und Benutzer zu registrieren."
services: azure-stack
documentationcenter: 
author: AlfredoPizzirani
manager: byronr
editor: 
ms.assetid: 157f0207-bddc-42e5-8351-197ec23f9d46
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: alfredop
ms.translationtype: HT
ms.sourcegitcommit: 0e862492c9e17d0acb3c57a0d0abd1f77de08b6a
ms.openlocfilehash: 48b75233e1a485e8fb3fbce9776a6ef398c4e99c
ms.contentlocale: de-de
ms.lasthandoff: 09/27/2017

---
# <a name="delegating-offers-in-azure-stack"></a>Delegieren von Angeboten in Azure Stack

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Als Azure Stack-Bediener möchten Sie häufig andere Personen in die Lage versetzen, in Ihrem Auftrag Angebote zu erstellen und Benutzer zu registrieren. Ein Dienstanbieter z.B. muss seinen Handelspartnern ermöglichen können, in seinem Auftrag Kunden zu registrieren und diese zu verwalten. Auch in Unternehmen ist diese Situation nicht ungewöhnlich, wenn eine zentrale IT-Gruppe vorhanden ist, Abteilungen oder Tochterunternehmen aber ohne Eingreifen der IT-Gruppe Benutzer registrieren müssen.

Bei diesen Aufgaben ist die Delegierung eine nützliche Funktion, mit der Sie mehr Benutzer erreichen und verwalten können als bei einer direkten Bearbeitung. Die folgende Abbildung zeigt eine Ebene der Delegierung, Azure Stack unterstützt jedoch mehrere Ebenen. Delegierte Anbieter wiederum können Aufgaben an andere Anbieter delegieren. Bis zu fünf Delegierungsebenen sind möglich.

![](media/azure-stack-delegated-provider/image1.png)

Azure Stack-Bediener können das Erstellen von Angeboten und Benutzern mithilfe der Delegierungsfunktion an andere Benutzer delegieren.

## <a name="roles-and-steps-in-delegation"></a>Rollen und Schritte der Delegierung
Denken Sie daran, dass Rollen an einer Delegierung beteiligt sind:

* Der Azure Stack-Bediener verwaltet die Azure Stack-Infrastruktur, erstellt eine Angebotsvorlage und delegiert die Aufgabe, Benutzern anhand dieser Vorlage Angebote zu unterbreiten, an andere Personen.
* Die delegierten Azure Stack-Bediener werden als **delegierte Anbieter** bezeichnet. Sie können zu anderen Organisationen gehören (beispielsweise zu anderen Azure Active Directory-Benutzern).
* **Benutzer** registrieren sich für die Angebote und verwenden sie, um ihre Workloads zu verwalten, virtuelle Computer zu erstellen, Daten zu speichern usw.

Wie in der folgenden Abbildung gezeigt, müssen zum Einrichten der Delegierung zwei Schritte ausgeführt werden.

1. **Identifizieren Sie die delegierten Anbieter**, indem Sie sie basierend auf einem Plan, der nur den Abonnementdienst enthält, als Abonnent eines Angebots hinzufügen.
   Benutzer, die dieses Angebot abonnieren, erhalten einige der Funktionen des Azure Stack-Bedieners, einschließlich der Möglichkeit, Angebot zu verlängern und Benutzer dafür zu registrieren.
2. **Delegieren Sie ein Angebot an den delegierten Anbieter**. Dieses Angebot fungiert als Vorlage für alle Komponenten, die der delegierte Anbieter offerieren kann. Der delegierte Anbieter ist jetzt in der Lage, das Angebot anzunehmen, ihm einen Namen zuzuweisen (nicht jedoch die darin enthaltenen Dienste und Kontingente zu ändern) und es seinen Kunden zu offerieren.

![](media/azure-stack-delegated-provider/image2.png)

Um als delegierte Anbieter zu fungieren, müssen Benutzer eine Beziehung mit dem Hauptanbieter einrichten, anders gesagt: Sie müssen ein Abonnement einrichten. In diesem Szenario werden die delegierten Anbieter durch das Abonnement als diejenigen Anbieter identifiziert, die das Recht besitzen, im Auftrag des Hauptanbieters Angebote zu präsentieren.

Sobald diese Beziehung eingerichtet ist, kann der Azure Stack-Bediener ein Angebot an den delegierten Anbieter delegieren. Der delegierte Anbieter ist jetzt in der Lage, das Angebot anzunehmen, es umzubenennen (nicht jedoch den Inhalt zu ändern) und es seinen Kunden zu offerieren.

In den folgenden Abschnitten wird beschrieben, wie Sie einen delegierten Anbieter einrichten, ein Angebot delegieren und überprüfen, ob Benutzer sich dafür registrieren können.

## <a name="set-up-roles"></a>Einrichten von Rollen

Um einen delegierten Anbieter einzurichten, benötigen Sie zusätzlich zu Ihrem Azure Stack-Bedienerkonto weitere Azure Active Directory-Konten. Wenn noch nicht über beiden folgenden Konten verfügen, erstellen Sie sie. Die Konten können zu einem beliebigen AAD-Benutzer gehören. Wir bezeichnen sie als delegierten Anbieter (Delegated Provider, DP) und Benutzer.

| **Rolle** | **Organisatorische Rechte** |
| --- | --- |
| Delegierter Anbieter |Benutzer |
| Benutzer |Benutzer |

## <a name="identify-the-delegated-providers"></a>Identifizieren der delegierten Anbieter
1. Melden Sie sich als Azure Stack-Bediener an.
2. Erstellen Sie das Angebot, das es Benutzern ermöglicht, delegierte Anbieter zu werden. Hierzu müssen Sie einen Plan und ein darauf basierendes Angebot erstellen:
   
   a.  [Erstellen Sie einen Plan](azure-stack-create-plan.md).
       Dieser Plan darf nur den Abonnementdienst enthalten. In diesem Artikel verwenden wir einen Plan namens „PlanForDelegation“.
   
   b.  [Erstellen Sie ein Angebot](azure-stack-create-offer.md), das auf diesem Plan basiert. In diesem Artikel verwenden wir ein Angebot namens „OfferToDP“.
   
   c.  Wenn die Erstellung des Angebots abgeschlossen ist, fügen Sie den delegierten Anbieter als Abonnenten zu diesem Angebot hinzu, indem Sie auf **Abonnements** &gt; **Hinzufügen** &gt; **Neues Mandantenabonnement** klicken.
   
   ![](media/azure-stack-delegated-provider/image3.png)

> [!NOTE]
> Wie bei allen Azure Stack-Angeboten haben Sie die Option, das Angebot öffentlich zu machen und Benutzern zu ermöglichen, sich dafür zu registrieren. Sie können auch ein privates Angebot einrichten und es dem Azure Stack-Bediener überlassen, die Registrierung zu verwalten. Delegierte Anbieter sind in der Regel eine kleine Gruppe, und Sie sollten kontrollieren können, wer in diese Gruppe aufgenommen wird. Ein privates Angebot ist also in den meisten Fällen sinnvoll.
> 
> 

## <a name="azure-stack-operator-creates-the-delegated-offer"></a>Erstellen des delegierten Angebots durch den Azure Stack-Bediener

Sie haben jetzt einen delegierten Anbieter eingerichtet. Der nächste Schritt besteht darin, den Plan und das Angebot zu erstellen, den/das Sie delegieren möchten und den/das Ihre Kunden verwenden werden. Sie sollten das Angebot genau so definieren, wie es Ihren Kunden angezeigt werden soll, da der delegierte Anbieter den Plan und die darin enthaltenen Kontingente nicht ändern kann.

1. Melden Sie sich als Azure Stack-Bediener an, und [erstellen Sie einen Plan](azure-stack-create-plan.md) sowie ein darauf basierendes [Angebot](azure-stack-create-offer.md). In diesem Artikel verwenden wir ein Angebot namens „DelegatedOffer“.
   
   > [!NOTE]
   > Dieses Angebot muss nicht öffentlich sein. Sie können es öffentlich machen, aber in den meisten Fällen sollten nur delegierte Anbieter darauf zugreifen dürfen. Sobald Sie, wie in den folgenden Schritten beschrieben, ein privates Angebot delegieren, kann der delegierte Anbieter darauf zugreifen.
   > 
   > 
1. Delegieren Sie das Angebot. Wechseln Sie zu „DelegatedOffer“, und klicken Sie im Bereich „Einstellungen“ auf **Delegierte Anbieter** &gt; **Hinzufügen**.
2. Wählen Sie aus dem Dropdownlistenfeld das Abonnement des delegierten Anbieters, und klicken Sie auf **Delegieren**.

> ![](media/azure-stack-delegated-provider/image4.png)
> 
> 

## <a name="delegated-provider-customizes-the-offer"></a>Anpassen des Angebots durch den delegierten Anbieter

Melden Sie sich als delegierter Anbieter beim Benutzerportal an, und verwenden Sie das delegierte Angebot als Vorlage, um ein neues Angebot zu erstellen.

1. Klicken Sie auf **Neu** &gt; **Mandantenangebote + Pläne** &gt; **Angebot**.

    ![](media/azure-stack-delegated-provider/image5.png)


1. Weisen Sie dem Angebot einen Namen zu. Hier wurde der Name „ResellerOffer“ gewählt. Wählen Sie das delegierte Angebot aus, auf dem dieses Angebot basieren soll, und klicken Sie auf **Erstellen**.
   
   ![](media/azure-stack-delegated-provider/image6.png)

    >[!NOTE] 
    > Beachten Sie den Unterschied zum Erstellen eines Angebots durch einen Azure Stack-Bediener. Delegierte Anbieter stellen ein Angebot nicht aus Basis- und Zusatzplänen zusammen, sie können nur aus Angeboten auswählen, die an sie delegiert wurden, und sie können diese Angebote nicht ändern.

1. Machen Sie das Angebot öffentlich, indem Sie auf **Durchsuchen** &gt; **Angebote** klicken, das Angebot auswählen und auf **Status ändern** klicken.
2. Der delegierte Anbieter macht diese Angebote über die URL zu seinem eigenen Portal verfügbar. Diese Angebote werden nur im Portal mit delegierten Angeboten angezeigt. So suchen und ändern Sie diese URL:
   
    a.  Klicken Sie auf **Durchsuchen**&gt; **Weitere Dienste**&gt; **Abonnements**. Wählen Sie das Abonnement des delegierten Anbieters, in diesem Fall *DPSubscription*. Klicken Sie dann auf **Eigenschaften**.
   
    b.  Kopieren Sie die Portal-URL an einen anderen Ort, beispielsweise in Text-Editor.
   
    ![](media/azure-stack-delegated-provider/dpportaluri.png)  
   
   Sie haben die Erstellung eines delegierten Angebots als delegierter Anbieter abgeschlossen. Melden Sie sich als delegierter Anbieter ab. Schließen Sie die verwendete Registerkarte im Browser.

## <a name="sign-up-for-the-offer"></a>Anmelden für das Angebot
1. Wechseln Sie in einem neuen Browserfenster zu der URL des Portals mit delegierten Angeboten, das Sie im vorherigen Schritt gespeichert haben. Melden Sie sich als Benutzer beim Portal an. Hinweis: Verwenden Sie für diesen Schritt das Portal mit delegierten Angeboten. Andernfalls wird das delegierte Angebot nicht angezeigt.
2. Klicken Sie auf dem Dashboard auf **Abonnement erwerben**. Sie werden feststellen, dass dem Benutzer nur die delegierten Angebote angezeigt werden, die vom delegierten Anbieter erstellt wurden:

> ![](media/azure-stack-delegated-provider/image8.png)
> 
> 

Damit ist der Prozess der Angebotsdelegierung abgeschlossen. Benutzer können sich jetzt für dieses Angebot registrieren, indem sie ein Abonnement dafür beziehen.

## <a name="multiple-tier-delegation"></a>Delegierung auf mehreren Ebenen

Mit der Delegierung auf mehreren Ebenen kann ein delegierter Anbieter das Angebot an andere Entitäten delegieren. So lassen sich z.B. vielschichtigere Handelspartnerkanäle erstellen, in denen der Anbieter, der Azure Stack verwaltet, ein Angebot an einen Großhändler delegiert, der es wiederum an einen Fachhändler delegiert.
Azure Stack unterstützt bis zu fünf Ebenen der Delegierung.

Um mehrere Ebenen der Delegierung zu erstellen, delegiert ein delegierter Anbieter ein Angebot an den nächsten Anbieter. Der Prozess für den delegierten Anbieter ist der gleiche wie für den Azure Stack-Bediener (siehe [Erstellen des delegierten Angebots durch den Azure Stack-Bediener](#cloud-operator-creates-the-delegated-offer)).

## <a name="next-steps"></a>Nächste Schritte
[Bereitstellen einer VM](azure-stack-provision-vm.md)


