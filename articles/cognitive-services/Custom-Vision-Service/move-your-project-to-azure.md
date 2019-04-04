---
title: Verschieben eines Projekts aus der eingeschränkten Testversion in Azure
titlesuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie ein Projekt aus der eingeschränkten Testversion in Azure verschoben wird.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: anroth
ms.openlocfilehash: a9f49af54f391b159f8b3d626fffc36635f5e51f
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2019
ms.locfileid: "56821302"
---
# <a name="how-to-move-your-limited-trial-project-to-azure-using-the-customvisionai-site"></a>Verschieben eines Projekts aus der eingeschränkten Testversion in Azure mithilfe der Website „CustomVision.ai“


Da ab sofort Custom Vision Service in der [Azure-Vorschau](https://azure.microsoft.com/services/preview/) verfügbar wird, endet der Support für Projekte aus der eingeschränkten Testversion außerhalb von Azure. In diesem Dokument erfahren Sie, wie Sie mithilfe der [Custom Vision-Website](https://customvision.ai) ein Projekt aus der eingeschränkten Testversion zur Verknüpfung mit einer Azure-Ressource verschieben. 

> [!NOTE]
> Wenn Sie Ihre Custom Vision-Projekte in eine Azure-Ressource verschieben, erben diese die der Azure-Ressource zugrunde liegenden [Berechtigungen]( https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal). Wenn andere Benutzer in Ihrer Organisation Besitzer der Azure-Ressource sind, in der sich Ihr Projekt befindet, haben diese Benutzer Zugriff auf Ihr Projekt auf der [Custom Vision-Website](https://customvision.ai). Entsprechend werden Ihre Projekte beim Löschen von Ressourcen gelöscht.  


Eine Einführung zu den Konzepten der Abonnements und Ressourcen bei Azure finden Sie im [Handbuch für Entwickler in Azure](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#manage-your-subscriptions).


## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen ein gültiges Azure-Abonnement in Verbindung mit dem entsprechenden Microsoft-Konto oder AAD-Konto (Azure Active Directory), das Sie für die Anmeldung bei der [Custom Vision-Website](https://customvision.ai) verwenden. 

Wenn Sie noch nicht über ein Azure-Konto verfügen, [erstellen Sie ein Konto](https://azure.microsoft.com/free/) kostenlos.


## <a name="create-custom-vision-resources-in-the-azure-portal"></a>Erstellen von Custom Vision-Ressourcen im Azure-Portal
Für die Verwendung des Custom Vision Service mit Azure müssen Sie im [Azure-Portal](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=microsoft_azure_cognitiveservices_customvision#create/Microsoft.CognitiveServicesCustomVision) Custom Vision-Trainings- und -Vorhersageressourcen erstellen. 

 Wenn Sie Ihr Projekt mithilfe dieser Plattform der [Custom Vision-Website](https://customvision.ai) verschieben möchten, müssen Sie Ihre Ressourcen in der Region „USA, Süden-Mitte“ erstellen, da alle Projekte aus der eingeschränkten Testversion in der Region „USA, Süden-Mitte“ gehostet werden. 

Dabei können Sie einer Ressource mehrere Projekte zuordnen. Ausführlichere Informationen hierzu finden Sie unter [Preise und Einschränkungen](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/limits-and-quotas). Wenn Sie Custom Vision Service weiterhin kostenlos nutzen möchten, können Sie im Azure-Portal die Dienstebene F0 auswählen. 


## <a name="move-your-limited-trial-project-to-an-azure-resource"></a>Verschieben Ihres Projekts aus der eingeschränkten Testversion in eine Azure-Ressource

1.  Navigieren Sie im Webbrowser zur [Custom Vision-Website](https://customvision.ai), und wählen Sie __Sign in__ (Anmelden) aus. Öffnen Sie das Projekt, das Sie in ein Azure-Konto migrieren möchten. 
2.  Öffnen Sie die Seite „Settings“ (Einstellungen) für Ihr Projekt, indem Sie auf das Zahnradsymbol in der oberen rechten Ecke des Bildschirms klicken. 

    ![Das Zahnradsymbol in der oberen rechten Ecke der Projektseite steht für Projekteinstellungen.](./media/move-your-project-to-azure/settings-icon.png)


3. Klicken Sie auf __Move to Azure__ (In Azure verschieben).

    ![Die Schaltfläche „Move to Azure“ (In Azure verschieben) befindet sich unten links auf der Seite „Project Settings“ (Projekteinstellungen).](./media/move-your-project-to-azure/move-to-azure.jpg)


4. Wählen Sie in der Dropdownliste der Schaltfläche __Move to Azure__ (In Azure verschieben) die Azure-Ressource aus, in die Sie Ihr Projekt verschieben möchten. Klicken Sie auf __Move__ (Verschieben). 

5. Wenn die Azure-Ressource, die Sie zuvor für Custom Vision Service erstellt haben, nicht angezeigt wird, befindet sie sich möglicherweise in einem anderen Verzeichnis. Wenn Sie Ihr Projekt in eine Ressource in einem anderen Verzeichnis verschieben möchten, befolgen Sie die Anweisungen weiter unten. 

    ![Fenster „Projektmigration“](./media/move-your-project-to-azure/Project_Migration_Window.jpg)


## <a name="move-project-to-another-azure-directory"></a>Verschieben eines Projekts in ein anderes Azure-Verzeichnis 

> [!NOTE]
> Sowohl im Azure-Portal als auch auf der Website „CustomVision.ai“ können Sie im Dropdownmenü des Benutzers in der oberen rechten Ecke des Bildschirms auswählen.   


1. Geben Sie an, in welchem Verzeichnis sich Ihre Azure-Ressource befindet. Das Verzeichnis wird unter Ihrem Benutzernamen oben rechts in der Menüleiste des Azure-Portals angezeigt. 

    ![Das Verzeichnis wird unter Ihrem Benutzernamen oben rechts in der Menüleiste des Azure-Portals angezeigt. .](./media/move-your-project-to-azure/identify_directory.jpg)

2. Suchen Sie nach der Ressourcen-ID Ihrer Custom Vision-Trainingsressource. Sie finden sie im Azure-Portal, indem Sie Ihre Custom Vision-Trainingsressource öffnen und im Abschnitt „Resource Management“ (Ressourcenverwaltung) die Option „Properties“ (Eigenschaften) auswählen. Dort wird Ihre Ressourcen-ID angezeigt. 

    ![Sie finden Ihre Ressourcen-ID im Azure-Portal, indem Sie Ihre Custom Vision-Trainingsressource öffnen und im Abschnitt „Resource Management“ (Ressourcenverwaltung) die Option „Properties“ (Eigenschaften) auswählen.](./media/move-your-project-to-azure/resource_ID_azure_portal.jpg)


3. Alternativ finden Sie die Ressourcen-ID Ihrer Custom Vision-Ressource direkt auf der [Einstellungsseite]( https://www.customvision.ai/projects#/settings) der Custom Vision-Website. Sie müssen zu dem Verzeichnis wechseln, in dem sich Ihre Azure-Ressource befindet.

    ![Ihre Ressourcen-ID wird für die einzelnen Ressourcen auf der Einstellungsseite der Custom Vision-Website angezeigt.](./media/move-your-project-to-azure/resource_ID_CVS_portal.jpg)

4. Nachdem Sie Ihre Ressourcen-ID gefunden haben, kehren Sie zum Custom Vision-Projekt zurück, das Sie aus einer eingeschränkten Testversion in eine Azure-Ressource verschieben möchten. Hierzu müssen Sie möglicherweise wieder zu Ihrem ursprünglichen Verzeichnis wechseln. Befolgen Sie die Anweisungen weiter [oben](#move-your-limited-trial-project-to-an-azure-resource), um die Einstellungsseite Ihres Projekts zu öffnen, und wählen Sie __Move to Azure__ (In Azure verschieben) aus. 


5. Aktivieren Sie im Fenster „Move to Azure“ (In Azure verschieben) das Kontrollkästchen „Move to a different Azure directory?“ (In ein anderes Azure-Verzeichnis verschieben?). Wählen Sie das Verzeichnis aus, in das Sie Ihr Projekt verschieben möchten, und geben Sie die Ressourcen-ID der Ressource ein, in die Sie Ihr Projekt verschieben. Klicken Sie auf __Move__ (Verschieben). 



5. Beachten Sie, dass sich Ihr Projekt nun in einem anderen Verzeichnis befindet. Sie finden Ihr Projekt, indem Sie zu dem Verzeichnis im Custom Vision-Webportal wechseln, in dem sich Ihr Projekt befindet. Sowohl im Azure-Portal als auch auf der [Custom Vision-Website](https://customvision.ai) können Sie im Dropdownmenü des Kontos in der oberen rechten Ecke des Bildschirms auswählen. 

## <a name="next-steps"></a>Nächste Schritte

Ihr Projekt wurde in eine Azure-Ressource verschoben. Sie müssen Ihre Trainings- und Vorhersageschlüssel in allen Anwendungen, die Sie geschrieben haben, aktualisieren.
