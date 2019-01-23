---
Titel: Bereitstellen des Studio-Webdiensts in mehreren Regionen titleSuffix: Azure Machine Learning Studio – Beschreibung: Schritte zum Bereitstellen (Kopieren) eines neuen Webdiensts in anderen Regionen. Stellen Sie einfach einen Webdienst in mehreren Regionen bereit, ohne dass mehrere Abonnements oder Arbeitsbereiche erforderlich sind.
Dienste: machine-learning ms.service: machine-learning ms.component: studio ms.topic: Artikel

Autor: ericlicoding ms.author: amlstudiodocs ms.custom: seodec18 ms.date: 19.04.2017
---
# <a name="deploy-an-azure-machine-learning-studio-web-service-to-multiple-regions"></a>Bereitstellen eines Azure Machine Learning Studio-Webdiensts in mehreren Regionen

Die neuen Azure-Webdienste ermöglichen Ihnen das einfache Bereitstellen eines Azure Machine Learning Studio-Webdiensts in mehreren Regionen, ohne dass mehrere Abonnements oder Arbeitsbereiche erforderlich sind. 

Die Preise sind regionsspezifisch. Daher müssen Sie einen Abrechnungsplan für jede Region definieren, in der der Webdienst bereitgestellt wird.

## <a name="to-create-a-plan-in-another-region"></a>So erstellen Sie einen Plan in einer anderen Region
1. Melden Sie sich bei [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/)an.
2. Klicken Sie auf die Menüoption **Plans** .
3. Klicken Sie auf der Übersichtsseite „Plans“ auf **New**.
4. Wählen Sie in der Dropdownliste **Abonnement** das Abonnement aus, in dem sich der neue Plan befinden soll.
5. Wählen Sie in der Dropdownliste **Region** eine Region für den neuen Plan aus. Im Abschnitt **Plan Options** auf der Seite werden die Planoptionen für die ausgewählte Region angezeigt.
6. Wählen Sie in der Dropdownliste **Resource Group** eine Ressourcengruppe für den Plan aus. Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht über Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).
7. Geben Sie in **Plan Name** den Namen des Plans ein.
8. Klicken Sie unter **Plan Options**auf den Abrechnungstarif des neuen Plans.
9. Klicken Sie auf **Create**.

## <a name="deploying-the-web-service-to-another-region"></a>Bereitstellen des Webdiensts in einer anderen Region
1. Klicken Sie auf die Menüoption **Web Services** .
2. Wählen Sie den Webdienst aus, den Sie in einer neuen Region bereitstellen.
3. Klicken Sie auf **Copy**.
4. Geben Sie in **Web Service Name**einen neuen Namen für den Webdienst ein.
5. Geben Sie in **Web service description**eine Beschreibung des Webdiensts ein.
6. Wählen Sie in der Dropdownliste **Abonnement** das Abonnement aus, in dem sich der neue Webdienst befinden soll.
7. Wählen Sie in der Dropdownliste **Resource Group** eine Ressourcengruppe für den Webdienst aus. Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht über Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).
8. Wählen Sie in der Dropdownliste **Region** die Region aus, in der der Webdienst bereitgestellt werden soll.
9. Wählen Sie in der Dropdownliste **Storage account** ein Speicherkonto aus, in dem der Webdienst gespeichert werden soll.
10. Wählen Sie in der Dropdownliste **Price Plan** einen Plan für die Region aus, die Sie in Schritt 8 gewählt haben.
11. Klicken Sie auf **Copy**.

