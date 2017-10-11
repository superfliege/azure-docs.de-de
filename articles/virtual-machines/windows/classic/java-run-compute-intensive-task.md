---
title: Rechenintensive Java-Anwendung auf einem virtuellen Computer | Microsoft Docs
description: "Informationen Sie zum virtuellen Computer in Azure zu erstellen, der eine rechenintensive-Java-Anwendung ausgeführt wird, die von einer anderen Java-Anwendung überwacht werden können."
services: virtual-machines-windows
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: ae6f2737-94c7-4569-9913-d871450c2827
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 8c51c0bb37e25ad61fe58a85dd641dabe0a1958c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-run-a-compute-intensive-task-in-java-on-a-virtual-machine"></a>Gewusst wie: Ausführen eine rechenintensiven Aufgabe in Java auf einer virtuellen Maschine
> [!IMPORTANT] 
> Azure gibt es zwei verschiedene Bereitstellungsmodelle für das Erstellen und Arbeiten mit Ressourcen: [Ressourcen-Manager und klassischen](../../../resource-manager-deployment-model.md). Dieser Artikel umfasst, mit dem klassischen Bereitstellungsmodell. Microsoft empfiehlt, dass die meisten neue Bereitstellungen, die Ressourcen-Manager-Modell verwenden.

Mit Azure können Sie eine virtuelle Maschine verwenden, behandeln Sie rechenintensive Aufgaben. Beispielsweise kann eine virtuelle Maschine ausführen und Ergebnisse liefern, Clientcomputern oder mobilen Anwendungen. Nach diesen Artikel lesen, müssen Sie einen Überblick über die zum Erstellen eines virtuellen Computers, das eine rechenintensive Java-Anwendung ausgeführt wird, die von einer anderen Java-Anwendung überwacht werden können.

In diesem Lernprogramm wird vorausgesetzt, Sie wissen, wie Java-konsolenanwendungen erstellen, können Importbibliotheken für die Java-Anwendung und ein Java-Archiv (JAR) generieren können. Keine Kenntnisse von Microsoft Azure werden dabei vorausgesetzt.

Sie lernen Folgendes:

* Zum Erstellen eines virtuellen Computers mit einem Java Development Kit (JDK) bereits installiert.
* Wie Sie Remote mit dem virtuellen Computer anmelden.
* Vorgehensweise: erstellen einen Servicebus-Namespace.
* Vorgehensweise: Erstellen von einer Java-Anwendung, die eine rechenintensiven Aufgabe ausführt.
* Erstellen eine Java-Anwendung, die den Fortschritt des Tasks "rechenintensive" überwacht.
* Wie die Java-Anwendungen ausführen.
* So beenden Sie den Java-Anwendungen.

In diesem Lernprogramm wird das Problem des Handlungsreisenden für die rechenintensiven Aufgabe verwendet. Im folgenden ist ein Beispiel für die Java-Anwendung, die die Ausführung der rechenintensive.

![Problem des Handlungsreisenden solver][solver_output]

Im folgenden ist ein Beispiel für die Java-Anwendung, die Überwachung der rechenintensiven Aufgabe.

![Problem des Handlungsreisenden-client][client_output]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a>So erstellen Sie einen virtuellen Computer
1. Melden Sie sich auf die [klassischen Azure-Portal](https://manage.windowsazure.com).
2. Klicken Sie auf **neu**, klicken Sie auf **berechnen**, klicken Sie auf **VM**, und klicken Sie dann auf **aus Katalog**.
3. In der **VM Image wählen** wählen Sie im Dialogfeld **JDK 7, Windows Server 2012**.
   Beachten Sie, dass **JDK 6 Windows Server 2012** ist verfügbar, wenn Sie ältere Anwendungen, die noch nicht verfügen für die Ausführung in JDK 7 bereit sind.
4. Klicken Sie auf **Weiter**.
5. In der **Konfiguration des virtuellen Computers** (Dialogfeld):
   1. Geben Sie einen Namen für den virtuellen Computer an.
   2. Geben Sie die Größe für den virtuellen Computer verwenden.
   3. Geben Sie einen Namen für den Administrator in der **Benutzername** Feld. Denken Sie daran, diesen Namen und das Kennwort, das Sie als Nächstes eingeben, werden Sie sie verwenden, wenn Sie sich Remote an den virtuellen Computer anmelden.
   4. Geben Sie ein Kennwort in die **neues Kennwort** Felds, und geben Sie ihn in die **bestätigen** Feld. Dies ist das Kennwort des Administratorkontos.
   5. Klicken Sie auf **Weiter**.
6. In der nächsten **Konfiguration des virtuellen Computers** (Dialogfeld):
   1. Für **Cloud-Dienst**, verwenden Sie den Standardnamen **Erstellen eines neuen Cloud-Diensts**.
   2. Der Wert für **DNS-Name für Cloud-Diensts** in cloudapp.net eindeutig sein. Ändern Sie diesen Wert ggf. so, dass Azure gibt an, dass er eindeutig ist.
   3. Geben Sie eine Region, Affinitätsgruppe oder virtuelles Netzwerk. Geben Sie für dieses Lernprogramm eine Region ein, z. B. **Westen der USA**.
   4. Für **Speicherkonto**Option **ein automatisch generiertes Speicherkonto verwenden**.
   5. Für **Verfügbarkeitsgruppe**Option **(keine)**.
   6. Klicken Sie auf **Weiter**.
7. In der endgültigen **Konfiguration des virtuellen Computers** (Dialogfeld):
   1. Akzeptieren Sie die Standardeinträge für den Endpunkt an.
   2. Klicken Sie auf **vollständige**.

## <a name="to-remotely-log-in-to-your-virtual-machine"></a>Remote mit dem virtuellen Computer anmelden
1. Melden Sie sich an den [klassischen Azure-Portal](https://manage.windowsazure.com).
2. Klicken Sie auf **VMs**.
3. Klicken Sie auf den Namen des virtuellen Computers an, die Sie sich anmelden möchten.
4. Klicken Sie auf **Verbinden**.
5. Befolgen Sie die Anweisungen nach Bedarf, um eine Verbindung mit dem virtuellen Computer herstellen. Wenn für den Administratornamen und Kennwort aufgefordert werden, verwenden Sie die Werte, die Sie bereitgestellt werden, wenn Sie die virtuelle Maschine erstellt.

Beachten Sie, dass die Azure Service Bus-Funktionalität das Zertifikat "Baltimore CyberTrust Root" als Teil Ihrer JRE installiert werden erfordert **Cacerts** zu speichern. Dieses Zertifikat wird automatisch in der Java Runtime Environment (JRE) verwendet, die für dieses Lernprogramm enthalten. Wenn Sie dieses Zertifikat in Ihre JRE keine **Cacerts** speichern, finden Sie unter [Hinzufügen eines Zertifikats auf den Java CA-Zertifikatspeicher] [ add_ca_cert] Informationen zum Hinzufügen von es (sowie Informationen zum Anzeigen der Zertifikate im Speicher Cacerts).

## <a name="how-to-create-a-service-bus-namespace"></a>Vorgehensweise: erstellen einen Servicebus-namespace
Um zu Service Bus-Warteschlangen in Azure verwenden, müssen Sie zuerst einen Dienstnamespace erstellen. Ein Dienstnamespace stellt ein bereichsdefinitionscontainer zum Adressieren von Service Bus-Ressourcen in Ihrer Anwendung bereit.

So erstellen Sie einen Dienstnamespace:

1. Melden Sie sich an den [klassischen Azure-Portal](https://manage.windowsazure.com).
2. Klicken Sie in der unteren linken Navigationsbereich des klassischen Azure-Portal auf **Servicebus, Zugriffssteuerung & Caching**.
3. Klicken Sie in der oberen linken Bereich des klassischen Azure-Portal auf der **Service Bus** Knoten, und klicken Sie dann auf die **neu** Schaltfläche.  
   ![Bildschirmabbildung von Service Bus-Knoten][svc_bus_node]
4. In der **erstellen Sie einen neuen Dienst-Namespace** Dialogfeld Geben Sie einen **Namespace**, und um sicherzustellen, dass er eindeutig ist, klicken Sie dann auf die **Verfügbarkeit prüfen** Schaltfläche.  
   ![Erstellen Sie einen neuen Namespace-screenshot][create_namespace]
5. Nachdem sichergestellt wird der Name des Namespaces verfügbar ist, wählen Sie das Land bzw. die Region, in dem Ihr Namespace gehostet werden soll, und klicken Sie dann auf, die **Namespace erstellen** Schaltfläche.  
   
   Der Namespace, den Sie erstellt wird dann in der klassischen Azure-Portal angezeigt und akzeptiert einen Moment Zeit, zu aktivieren. Warten Sie, bis der Status der **Active** bevor Sie mit dem nächsten Schritt fortfahren.

## <a name="obtain-the-default-management-credentials-for-the-namespace"></a>Der Standard-Anmeldeinformationen für den Namespace abrufen
Damit, z. B. das Erstellen einer Warteschlange auf dem neuen Namespace Verwaltungsvorgänge müssen Sie die Verwaltungsanmeldeinformationen für den Namespace zu erhalten.

1. Klicken Sie im linken Navigationsbereich auf die **Service Bus** Knoten, um die Liste der verfügbaren Namespaces anzuzeigen.
   ![Screenshot der verfügbaren Namespaces][avail_namespaces]
2. Wählen Sie den Namespace, den Sie gerade erstellt, aus der Liste angezeigt haben.
   ![Bildschirmabbildung von Namespace-Liste][namespace_list]
3. Die Rechte **Eigenschaften** Bereich aufgeführt, die die Eigenschaften für den neuen Namespace.
   ![Bildschirmabbildung von Eigenschaften im Bereich][properties_pane]
4. Die **Standardschlüssel** ausgeblendet ist. Klicken Sie auf die **Ansicht** Schaltfläche, um die Anmeldeinformationen anzuzeigen.
   ![Screenshot der Standard-Schlüssel][default_key]
5. Notieren Sie sich die **Standardausstellers** und **Standardschlüssel** , wie Sie diese Informationen, die folgenden Operationen mit dem Namespace verwenden.

## <a name="how-to-create-a-java-application-that-performs-a-compute-intensive-task"></a>Erstellen eine Java-Anwendung, die eine rechenintensiven Aufgabe ausführt
1. Auf dem Entwicklungscomputer (die nicht unbedingt der virtuellen Maschine werden, die Sie erstellt haben), Herunterladen der [Azure SDK für Java](https://azure.microsoft.com/develop/java/).
2. Erstellen einer Java-Konsolenanwendung mit dem Beispielcode am Ende dieses Abschnitts. In diesem Lernprogramm verwenden wir **TSPSolver.java** mit dem Java-Dateinamen. Ändern der **Ihrer\_Service\_Bus\_Namespace**, **Ihrer\_Service\_Bus\_Besitzer**, und **Ihrer\_Service\_Bus\_Schlüssel** Platzhalter mit Servicebus- **Namespace**, **Standardausstellers** und **Standardschlüssel** bzw. die Werte.
3. Nach dem Schreiben von Code, Exportieren der Anwendung in eine ausführbare Java-Archiv (JAR) und die erforderlichen Bibliotheken in der generierten JAR-Datei verpacken. In diesem Lernprogramm verwenden wir **TSPSolver.jar** als Name des generierten JAR-Datei.

<p/>

    // TSPSolver.java

    import com.microsoft.windowsazure.services.core.Configuration;
    import com.microsoft.windowsazure.services.core.ServiceException;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import java.io.*;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import java.util.ArrayList;
    import java.util.Date;
    import java.util.List;

    public class TSPSolver {

        //  Value specifying how often to provide an update to the console.
        private static long loopCheck = 100000000;  

        private static long nTimes = 0, nLoops=0;

        private static double[][] distances;
        private static String[] cityNames;
        private static int[] bestOrder;
        private static double minDistance;
        private static ServiceBusContract service;

        private static void buildDistances(String fileLocation, int numCities) throws Exception{
            try{
                BufferedReader file = new BufferedReader(new InputStreamReader(new DataInputStream(new FileInputStream(new File(fileLocation)))));
                double[][] cityLocs = new double[numCities][2];
                for (int i = 0; i<numCities; i++){
                    String[] line = file.readLine().split(", ");
                    cityNames[i] = line[0];
                    cityLocs[i][0] = Double.parseDouble(line[1]);
                    cityLocs[i][1] = Double.parseDouble(line[2]);
                }
                for (int i = 0; i<numCities; i++){
                    for (int j = i; j<numCities; j++){
                        distances[i][j] = Math.hypot(Math.abs(cityLocs[i][0] - cityLocs[j][0]), Math.abs(cityLocs[i][1] - cityLocs[j][1]));
                        distances[j][i] = distances[i][j];
                    }
                }
            } catch (Exception e){
                throw e;
            }
        }

        private static void permutation(List<Integer> startCities, double distSoFar, List<Integer> restCities) throws Exception {

            try
            {
                nTimes++;
                if (nTimes == loopCheck)
                {
                    nLoops++;
                    nTimes = 0;
                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.print("Current time is " + dateFormat.format(date) + ". ");
                    System.out.println(  "Completed " + nLoops + " iterations of size of " + loopCheck + ".");
                }

                if ((restCities.size() == 1) && ((minDistance == -1) || (distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-1)] < minDistance))){
                    startCities.add(restCities.get(0));
                    newBestDistance(startCities, distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-2)]);
                    startCities.remove(startCities.size()-1);
                }
                else{
                    for (int i=0; i<restCities.size(); i++){
                        startCities.add(restCities.get(0));
                        restCities.remove(0);
                        permutation(startCities, distSoFar + distances[startCities.get(startCities.size()-1)][startCities.get(startCities.size()-2)],restCities);
                        restCities.add(startCities.get(startCities.size()-1));
                        startCities.remove(startCities.size()-1);
                    }
                }
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        private static void newBestDistance(List<Integer> cities, double distance) throws ServiceException, Exception {
            try
            {
                minDistance = distance;
                String cityList = "Shortest distance is "+minDistance+", with route: ";
                for (int i = 0; i<bestOrder.length; i++){
                    bestOrder[i] = cities.get(i);
                    cityList += cityNames[bestOrder[i]];
                    if (i != bestOrder.length -1)
                        cityList += ", ";
                }
                System.out.println(cityList);
                service.sendQueueMessage("TSPQueue", new BrokeredMessage(cityList));
            }
            catch (ServiceException se)
            {
                throw se;
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        public static void main(String args[]){

            try {

                Configuration config = ServiceBusConfiguration.configureWithWrapAuthentication(
                        "your_service_bus_namespace", "your_service_bus_owner",
                        "your_service_bus_key",
                        ".servicebus.windows.net",
                        "-sb.accesscontrol.windows.net/WRAPv0.9");

                service = ServiceBusService.create(config);

                int numCities = 10;  // Use as the default, if no value is specified at command line.
                if (args.length != 0)
                {
                    if (args[0].toLowerCase().compareTo("createqueue")==0)
                    {
                        // No processing to occur other than creating the queue.
                        QueueInfo queueInfo = new QueueInfo("TSPQueue");

                        service.createQueue(queueInfo);

                        System.out.println("Queue named TSPQueue was created.");

                        System.exit(0);
                    }

                    if (args[0].toLowerCase().compareTo("deletequeue")==0)
                    {
                        // No processing to occur other than deleting the queue.
                        service.deleteQueue("TSPQueue");

                        System.out.println("Queue named TSPQueue was deleted.");

                        System.exit(0);
                    }

                    // Neither creating or deleting a queue.
                    // Assume the value passed in is the number of cities to solve.
                    numCities = Integer.valueOf(args[0]);  
                }

                System.out.println("Running for " + numCities + " cities.");

                List<Integer> startCities = new ArrayList<Integer>();
                List<Integer> restCities = new ArrayList<Integer>();
                startCities.add(0);
                for(int i = 1; i<numCities; i++)
                    restCities.add(i);
                distances = new double[numCities][numCities];
                cityNames = new String[numCities];
                buildDistances("c:\\TSP\\cities.txt", numCities);
                minDistance = -1;
                bestOrder = new int[numCities];
                permutation(startCities, 0, restCities);
                System.out.println("Final solution found!");
                service.sendQueueMessage("TSPQueue", new BrokeredMessage("Complete"));
            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }
        }

    }



## <a name="how-to-create-a-java-application-that-monitors-the-progress-of-the-compute-intensive-task"></a>Erstellen eine Java-Anwendung, die den Fortschritt des Tasks "rechenintensive" überwacht
1. Erstellen Sie auf dem Entwicklungscomputer eine Java-Konsolenanwendung mit dem Beispielcode am Ende dieses Abschnitts. In diesem Lernprogramm verwenden wir **TSPClient.java** mit dem Java-Dateinamen. Wie oben beschrieben zu ändern der **Ihrer\_Service\_Bus\_Namespace**, **Ihrer\_Service\_Bus\_Besitzer**, und **Ihrer\_Service\_Bus\_Schlüssel** Platzhalter mit Servicebus- **Namespace**, **Standardausstellers** und **Standardschlüssel** bzw. die Werte.
2. Exportieren der Anwendung in eine ausführbare JAR-Datei, und Packen Sie die erforderlichen Bibliotheken in der generierten JAR-Datei. In diesem Lernprogramm verwenden wir **TSPClient.jar** als Name des generierten JAR-Datei.

<p/>

    // TSPClient.java

    import java.util.Date;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import com.microsoft.windowsazure.services.core.*;

    public class TSPClient
    {

        public static void main(String[] args)
        {
                try
                {

                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.println("Starting at " + dateFormat.format(date) + ".");

                    String namespace = "your_service_bus_namespace";
                    String issuer = "your_service_bus_owner";
                    String key = "your_service_bus_key";

                    Configuration config;
                    config = ServiceBusConfiguration.configureWithWrapAuthentication(
                            namespace, issuer, key,
                            ".servicebus.windows.net",
                            "-sb.accesscontrol.windows.net/WRAPv0.9");

                    ServiceBusContract service = ServiceBusService.create(config);

                    BrokeredMessage message;

                    int waitMinutes = 3;  // Use as the default, if no value is specified at command line.
                    if (args.length != 0)
                    {
                        waitMinutes = Integer.valueOf(args[0]);  
                    }

                    String waitString;

                    waitString = (waitMinutes == 1) ? "minute." : waitMinutes + " minutes.";

                    // This queue must have previously been created.
                    service.getQueue("TSPQueue");

                    int numRead;

                    String s = null;

                    while (true)
                    {

                        ReceiveQueueMessageResult resultQM = service.receiveQueueMessage("TSPQueue");
                        message = resultQM.getValue();

                        if (null != message && null != message.getMessageId())
                        {

                            // Display the queue message.
                            byte[] b = new byte[200];

                            System.out.print("From queue: ");

                            s = null;
                            numRead = message.getBody().read(b);
                            while (-1 != numRead)
                            {
                                s = new String(b);
                                s = s.trim();
                                System.out.print(s);
                                numRead = message.getBody().read(b);
                            }
                            System.out.println();
                            if (s.compareTo("Complete") == 0)
                            {
                                // No more processing to occur.
                                date = new Date();
                                System.out.println("Finished at " + dateFormat.format(date) + ".");
                                break;
                            }
                        }
                        else
                        {
                            // The queue is empty.
                            System.out.println("Queue is empty. Sleeping for another " + waitString);
                            Thread.sleep(60000 * waitMinutes);
                        }
                    }

            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }

        }

    }

## <a name="how-to-run-the-java-applications"></a>Zum Ausführen von Java-Anwendungen
Führen Sie die Anwendung rechenintensive zuerst an, um die Warteschlange zu erstellen, um das Wegstrecke Saleseman Problem zu beheben, die optimale Route auf aktuellen Servicebus-Warteschlange hinzugefügt wird. Während die Anwendung rechenintensive ausgeführt (oder danach), führen Sie den Client zum Anzeigen der Ergebnisse aus der Servicebus-Warteschlange.

### <a name="to-run-the-compute-intensive-application"></a>Zum Ausführen der Anwendung rechenintensive
1. Melden Sie sich an Ihren virtuellen Computer an.
2. Erstellen Sie einen Ordner, in dem Sie die Anwendung ausgeführt wird. Beispielsweise **c:\TSP**.
3. Kopie **TSPSolver.jar** auf **c:\TSP**,
4. Erstellen Sie eine Datei mit dem Namen **c:\TSP\cities.txt** mit dem folgenden Inhalt.
   
        City_1, 1002.81, -1841.35
        City_2, -953.55, -229.6
        City_3, -1363.11, -1027.72
        City_4, -1884.47, -1616.16
        City_5, 1603.08, -1030.03
        City_6, -1555.58, 218.58
        City_7, 578.8, -12.87
        City_8, 1350.76, 77.79
        City_9, 293.36, -1820.01
        City_10, 1883.14, 1637.28
        City_11, -1271.41, -1670.5
        City_12, 1475.99, 225.35
        City_13, 1250.78, 379.98
        City_14, 1305.77, 569.75
        City_15, 230.77, 231.58
        City_16, -822.63, -544.68
        City_17, -817.54, -81.92
        City_18, 303.99, -1823.43
        City_19, 239.95, 1007.91
        City_20, -1302.92, 150.39
        City_21, -116.11, 1933.01
        City_22, 382.64, 835.09
        City_23, -580.28, 1040.04
        City_24, 205.55, -264.23
        City_25, -238.81, -576.48
        City_26, -1722.9, -909.65
        City_27, 445.22, 1427.28
        City_28, 513.17, 1828.72
        City_29, 1750.68, -1668.1
        City_30, 1705.09, -309.35
        City_31, -167.34, 1003.76
        City_32, -1162.85, -1674.33
        City_33, 1490.32, 821.04
        City_34, 1208.32, 1523.3
        City_35, 18.04, 1857.11
        City_36, 1852.46, 1647.75
        City_37, -167.44, -336.39
        City_38, 115.4, 0.2
        City_39, -66.96, 917.73
        City_40, 915.96, 474.1
        City_41, 140.03, 725.22
        City_42, -1582.68, 1608.88
        City_43, -567.51, 1253.83
        City_44, 1956.36, 830.92
        City_45, -233.38, 909.93
        City_46, -1750.45, 1940.76
        City_47, 405.81, 421.84
        City_48, 363.68, 768.21
        City_49, -120.3, -463.13
        City_50, 588.51, 679.33
5. An der Eingabeaufforderung wechseln Sie zum c:\TSP.
6. Stellen Sie sicher, dass die JRE Ordner "Bin" in der PATH-Umgebungsvariablen angegeben ist.
7. Sie müssen vor dem Ausführen der TSP Solver Permutationen der Servicebus-Warteschlange zu erstellen. Führen Sie den folgenden Befehl zum Erstellen der Servicebus-Warteschlange.
   
        java -jar TSPSolver.jar createqueue
8. Die Warteschlange erstellt wird, können Sie die TSP Solver Permutationen ausführen. Führen Sie z. B. den folgenden Befehl zum Ausführen der Solver für 8 Städte.
   
        java -jar TSPSolver.jar 8
   
   Wenn Sie eine Zahl angeben, wird es für 10 Städten ausgeführt. Wie die Solver aktuelle kürzeste Route gefunden wird, wird er diese an die Warteschlange hinzufügen.

> [!NOTE]
> Je größer die Zahl, die Sie, desto mehr angeben Solver wird ausgeführt. Z. B. Ausführung 14 Städte können einige Minuten dauern und wird ausgeführt für 15 Städte kann möglicherweise mehrere Stunden. Erhöhen mindestens 16 Orte konnte der Common Language Runtime (schließlich Wochen, Monaten und Jahren) in Tagen ergeben. Dies liegt an der rasche Anstieg der Anzahl von Permutationen, die von der Solver ausgewertet wird, als die Anzahl der Orte erhöht.
> 
> 

### <a name="how-to-run-the-monitoring-client-application"></a>So führen Sie die Überwachung Clientanwendung
1. Melden Sie sich an Ihren Computer, auf dem Sie die Clientanwendung ausgeführt wird. Dies ist nicht erforderlich, auf dem gleichen Computer ausgeführt werden die **TSPSolver** -Anwendung einsetzen möchten, obwohl es sein kann.
2. Erstellen Sie einen Ordner, in dem Sie die Anwendung ausgeführt wird. Beispielsweise **c:\TSP**.
3. Kopie **TSPClient.jar** auf **c:\TSP**,
4. Stellen Sie sicher, dass die JRE Ordner "Bin" in der PATH-Umgebungsvariablen angegeben ist.
5. An der Eingabeaufforderung wechseln Sie zum c:\TSP.
6. Führen Sie den folgenden Befehl aus.
   
        java -jar TSPClient.jar
   
    Geben Sie optional die Anzahl der Minuten, in den Energiesparmodus zwischen prüfen die Warteschlange durch ein Befehlszeilenargument übergeben. Für die Überprüfung der Warteschlangenobjekts der Standardzeitraum für Standbymodus ist 3 Minuten, der verwendet wird, wenn kein Befehlszeilenargument, um übergeben wird **TSPClient**. Wenn Sie einen anderen Wert für das Ruheintervall verwenden möchten, z. B. Führen Sie eine Minute, den folgenden Befehl aus.
   
        java -jar TSPClient.jar 1
   
    Der Client wird ausgeführt, bis es eine warteschlangennachricht von "Vollständig" erkennt. Beachten Sie, wenn Sie mehrere Vorkommen von Solver ausführen, ohne den Client ausführen, müssen Sie möglicherweise zum Ausführen der Client mehrere Male, um die Warteschlange vollständig zu leeren. Alternativ können Sie die Warteschlange löschen und erstellen Sie es erneut. Um die Warteschlange zu löschen, führen Sie den folgenden **TSPSolver** (nicht **TSPClient**) Befehl.
   
        java -jar TSPSolver.jar deletequeue
   
    Die Solver wird ausgeführt, bis er abgeschlossen ist, untersuchen alle Routen.

## <a name="how-to-stop-the-java-applications"></a>Beenden Sie den Java-Anwendungen
Für die Solver und -Clientanwendungen können Sie drücken **STRG + C** zu beenden, wenn Sie vor dem normalen Abschluss beenden möchten.

[solver_output]:media/java-run-compute-intensive-task/WA_JavaTSPSolver.png
[client_output]:media/java-run-compute-intensive-task/WA_JavaTSPClient.png
[svc_bus_node]:media/java-run-compute-intensive-task/SvcBusQueues_02_SvcBusNode.jpg
[create_namespace]:media/java-run-compute-intensive-task/SvcBusQueues_03_CreateNewSvcNamespace.jpg
[avail_namespaces]:media/java-run-compute-intensive-task/SvcBusQueues_04_SvcBusNode_AvailNamespaces.jpg
[namespace_list]:media/java-run-compute-intensive-task/SvcBusQueues_05_NamespaceList.jpg
[properties_pane]:media/java-run-compute-intensive-task/SvcBusQueues_06_PropertiesPane.jpg
[default_key]:media/java-run-compute-intensive-task/SvcBusQueues_07_DefaultKey.jpg
[add_ca_cert]: ../../../java-add-certificate-ca-store.md
