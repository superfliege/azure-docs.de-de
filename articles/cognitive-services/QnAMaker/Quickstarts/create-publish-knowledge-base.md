---
title: 'Schnellstart: Erstellen einer Wissensdatenbank – QnA Maker'
titleSuffix: Azure Cognitive Services
description: Sie können eine QnA Maker-Wissensdatenbank (KB) aus Ihren eigenen Inhalten erstellen, z.B. aus FAQs oder Produkthandbüchern. Die QnA Maker-Wissensdatenbank in diesem Beispiel wird aus einer einfachen FAQ-Webseite erstellt, um Fragen zur BitLocker-Schlüsselwiederherstellung zu beantworten.
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: diberry
ms.openlocfilehash: f7af86687a8a61fb7aed028d2868752faaa8045a
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47030253"
---
# <a name="create-train-and-publish-your-knowledge-base"></a>Erstellen, Trainieren und Veröffentlichen der Wissensdatenbank

Sie können eine QnA Maker-Wissensdatenbank (KB) aus Ihren eigenen Inhalten erstellen, z.B. aus FAQs oder Produkthandbüchern. Die QnA Maker-Wissensdatenbank in diesem Beispiel wird aus einer einfachen FAQ-Webseite erstellt, um Fragen zur BitLocker-Schlüsselwiederherstellung zu beantworten.

## <a name="prerequisite"></a>Voraussetzung

> [!div class="checklist"]
> * Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-a-qna-maker-knowledge-base"></a>Erstellen einer QnA Maker-Wissensdatenbank

1. Melden Sie sich mit Ihren Azure-Anmeldeinformationen bei QnAMaker.ai an.

2. Wählen Sie auf der QnA Maker-Website **Wissensdatenbank erstellen** aus.

   ![Erstellen einer Wissensdatenbank](../media/qna-maker-create-kb.png)

3. Wählen Sie auf der Seite **Erstellen** in Schritt 1 die Option **QnA-Dienst erstellen** aus. Sie werden an das [Azure-Portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) weitergeleitet, um einen QnA Maker-Dienst in Ihrem Abonnement einzurichten. Wenn für das Azure-Portal ein Timeout auftritt, wählen Sie **Noch mal versuchen** für die Website aus. Nachdem Sie eine Verbindung hergestellt haben, wird Ihr Azure-Dashboard angezeigt.

4. Nachdem Sie erfolgreich einen neuen QnA Maker-Dienst in Azure erstellt haben, kehren Sie zu qnamaker.ai/create zurück. Wählen Sie den QnA-Dienst aus den Dropdownlisten in Schritt 2 aus. Wenn Sie einen neuen QnA-Dienst erstellt haben, achten Sie darauf, dass Sie die Seite aktualisieren.

   ![Auswählen einer Wissensdatenbank des QnA-Diensts](../media/qnamaker-quickstart-kb/qnaservice-selection.png)

5. Geben Sie in Schritt 3 Ihrer Wissensdatenbank den Namen **My Sample QnA KB**.

6. Wählen Sie zum Hinzufügen von Inhalten zu Ihrer Wissensdatenbank drei Typen von Datenquellen aus. Fügen Sie in Schritt 4 unter **KB mit Daten auffüllen** die URL der [FAQ zur BitLocker-Wiederherstellung](https://docs.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-overview-and-requirements-faq) in das Feld **URL** ein.

   ![Auswählen einer Wissensdatenbank des QnA-Diensts](../media/qnamaker-quickstart-kb/add-datasources.png)

7. Wählen Sie in Schritt 5 **Wissensdatenbank erstellen** aus.

8. Während die Wissensdatenbank erstellt wird, wird ein Popupfenster angezeigt. Der Extraktionsvorgang nimmt einige Minuten in Anspruch, um die HTML-Seite zu lesen und Fragen und Antworten zu identifizieren.

9. Nachdem die Wissensdatenbank erfolgreich erstellt wurde, wird die Seite **Wissensdatenbank** geöffnet. Sie können den Inhalt der Wissensdatenbank auf dieser Seite bearbeiten.

10. Wählen Sie oben rechts **QnA-Paar hinzufügen** aus, um eine neue Zeile im Abschnitt **Editorial** der Wissensdatenbank hinzuzufügen. Geben Sie unter **Frage** die Begrüßung **Hi** ein. Geben Sie unter **Antwort** die Antwort **„Hallo“ ein. Befragen Sie mich zu Bitlocker.**

   ![Hinzufügen eines Fragen-und-Antworten-Paars](../media/qnamaker-quickstart-kb/add-qna-pair.png)

11. Wählen Sie in der oberen rechten Ecke **Speichern und trainieren** aus, um Ihre Änderungen zu speichern und das QnA Maker-Modell zu trainieren. Änderungen werden nur beibehalten, wenn sie gespeichert werden.

   ![Speichern und trainieren](../media/qnamaker-quickstart-kb/add-qna-pair2.png)

12. Wählen Sie in der oberen rechten Ecke **Testen** aus, um zu testen, ob die von Ihnen vorgenommenen Änderungen wirksam sind. Geben Sie **Hi** in das Feld ein, und drücken Sie die EINGABETASTE. Daraufhin sollte die Antwort, die Sie als Reaktion erstellt haben, angezeigt werden.

13. Wählen Sie **Untersuchen** aus, um die Antwort ausführlicher zu untersuchen. Das Testfenster wird verwendet, um die Änderungen an der Wissensdatenbank zu testen, bevor sie veröffentlicht werden.

   ![Testbereich](../media/qnamaker-quickstart-kb/inspect-panel.png)

14. Wählen Sie erneut **Testen** aus, um das Popupfenster **Testen** zu schließen.

15. Wählen Sie im Menü neben **Bearbeiten** die Option **Veröffentlichen** aus. Wählen Sie dann zum Bestätigen **Veröffentlichen** auf der Seite aus.

16. Die QnA Maker-Dienst wurde nun erfolgreich veröffentlicht. Sie können den Endpunkt in Ihrer Anwendung oder im Botcode verwenden.

   ![Veröffentlichen](../media/qnamaker-quickstart-kb/publish-sucess.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Wissensdatenbank](../How-To/create-knowledge-base.md)
