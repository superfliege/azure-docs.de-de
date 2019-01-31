---
title: Untersuchen von Daten in Azure Blob Storage mit „pandas“ – Team Data Science-Prozess
description: Erfahren Sie, wie Sie in einem Azure-Blobcontainer gespeicherte Daten mithilfe des Python-Pakets „pandas“ untersuchen.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/09/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 1d76f9a76c85f4f3f052c3b409f5be28b51ebe91
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55459127"
---
# <a name="explore-data-in-azure-blob-storage-with-pandas"></a>Untersuchen von Daten in Azure Blob Storage mit Pandas

In diesem Artikel wird erläutert, wie Sie in einem Azure-Blobcontainer gespeicherte Daten mithilfe des Python-Pakets [pandas](http://pandas.pydata.org/) untersuchen.

Dieser Task ist ein Schritt im [Team Data Science-Prozess](overview.md).

## <a name="prerequisites"></a>Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie Folgendes abgeschlossen haben:

* Sie haben ein Azure-Speicherkonto erstellt. Anweisungen finden Sie unter [Erstellen eines Azure-Speicherkontos](../../storage/common/storage-quickstart-create-account.md).
* Die Daten wurden in einem Azure Blob Storage-Konto gespeichert. Anweisungen finden Sie unter [Verschieben von Daten in und aus Azure Storage](../../storage/common/storage-moving-data.md)

## <a name="load-the-data-into-a-pandas-dataframe"></a>Laden der Daten in ein Pandas-DataFrame
Damit ein Dataset untersucht und bearbeitet werden kann, muss es zuerst aus der Blobquelle in eine lokale Datei heruntergeladen werden, die anschließend in ein Pandas-DataFrame geladen werden kann. Nachfolgend sehen Sie für dieses Verfahren erforderlichen Schritte:

1. Laden Sie die Daten mithilfe des Blob-Diensts und folgenden Python-Beispielcodes aus dem Azure-Blob herunter. Ersetzen Sie die Variablen im folgenden Code durch die für Ihre Umgebung geltenden Werte: 
   
        from azure.storage.blob import BlobService
        import tables
   
        STORAGEACCOUNTNAME= <storage_account_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        LOCALFILENAME= <local_file_name>        
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>
   
        #download from blob
        t1=time.time()
        blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
        blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
        t2=time.time()
        print(("It takes %s seconds to download "+blobname) % (t2 - t1))
2. Lesen Sie die Daten aus der heruntergeladenen Datei in ein Pandas-DataFrame ein.
   
        #LOCALFILE is the file path    
        dataframe_blobdata = pd.read_csv(LOCALFILE)

Sie können nun die Daten durchsuchen und Funktionen mit diesem DataSet generieren.

## <a name="blob-dataexploration"></a>Beispiele für das Untersuchen von Daten mithilfe von Pandas
Hier sind einige Beispiele für Möglichkeiten zum Durchsuchen von Daten mithilfe von Pandas:

1. Untersuchen der **Anzahl von Zeilen und Spalten** 
   
        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. **Überprüfen** der ersten oder letzten **Zeilen** im folgenden Dataset:
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. Überprüfen des **Datentyps** der einzelnen importierten Spalten:
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. Überprüfen der **Basisstatistiken** für die Spalten im DataSet:
   
        dataframe_blobdata.describe()
5. Überprüfen der Anzahl der Einträge für jeden Spaltenwert:
   
        dataframe_blobdata['<column_name>'].value_counts()
6. **Zählen der fehlenden Werte** im Vergleich zur tatsächlichen Anzahl der Einträge in den einzelnen Spalten:
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. Löschen **fehlender Werte** in einer bestimmten Spalte aus den Daten:
   
     dataframe_blobdata_noNA = dataframe_blobdata.dropna()   dataframe_blobdata_noNA.shape
   
   Alternatives Ersetzen der fehlenden Werte mit der "mode"-Funktion:
   
     dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        
8. Erstellen eines **Histogramms** mithilfe der Variablenanzahl von Gruppen zur Darstellung der Verteilung einer Variablen:    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. Überprüfen der **Korrelationen** zwischen Variablen mithilfe eines Punktdiagramms oder einer integrierten „correlation“-Funktion:
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

