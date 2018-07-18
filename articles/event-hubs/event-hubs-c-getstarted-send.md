---
title: Senden von Ereignissen an Azure Event Hubs mithilfe von C | Microsoft-Dokumentation
description: Senden von Ereignissen an Azure Event Hubs mithilfe von C
services: event-hubs
documentationcenter: ''
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: c
ms.devlang: csharp
ms.topic: article
ms.date: 12/4/2017
ms.author: sethm
ms.openlocfilehash: e3267b54fa0c8593e0f9366c009656f36e4094ef
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/06/2018
ms.locfileid: "34807814"
---
# <a name="send-events-to-azure-event-hubs-using-c"></a>Senden von Ereignissen an Azure Event Hubs mithilfe von C

## <a name="introduction"></a>Einführung
Event Hubs ist ein hochgradig skalierbares Erfassungssystem, das Millionen von Ereignissen pro Sekunde verarbeiten kann. Anwendungen erhalten dadurch die Möglichkeit, die von Ihren verbundenen Geräten und Anwendungen erzeugten immensen Datenmengen zu verarbeiten und zu analysieren. Nach der Erfassung in einem Event Hub können Sie Daten über einen beliebigen Echtzeitanalyseanbieter oder einen Speichercluster transformieren und speichern.

Weitere Informationen finden Sie unter [Übersicht über Event Hubs](https://docs.microsoft.com/azure/event-hubs/event-hubs-overview).

In diesem Tutorial wird beschrieben, wie Sie mithilfe einer Konsolenanwendung in C Ereignisse an einen Event Hub senden. Klicken Sie für weitere Informationen zum Empfangen von Ereignissen links im Inhaltsverzeichnis auf die entsprechende Empfangssprache.

Für dieses Tutorial benötigen Sie Folgendes:

* Eine C-Entwicklungsumgebung. In diesem Tutorial wird vom gcc-Stack auf einem virtuellen Azure-Computer unter Linux mit Ubuntu 14.04 ausgegangen.
* [Microsoft Visual Studio](https://www.visualstudio.com/).
* Ein aktives Azure-Konto. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

## <a name="send-messages-to-event-hubs"></a>Senden von Nachrichten an Ereignis-Hubs
In diesem Abschnitt wird das Schreiben einer C-App beschrieben, mit der Ereignisse an den Event Hub gesendet werden. Im Code wird die Proton AMQP-Bibliothek aus dem [Apache Qpid-Projekt](http://qpid.apache.org/) verwendet. Dies entspricht der Verwendung von Service Bus-Warteschlangen und -Themen mit AMQP aus C, wie [in diesem Beispiel](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504) beschrieben. Weitere Informationen finden Sie in der [Qpid Proton-Dokumentation](http://qpid.apache.org/proton/index.html).

1. Befolgen Sie auf der Seite [Qpid AMQP Messenger](https://qpid.apache.org/proton/messenger.html) die Anweisungen zum Installieren von Qpid Proton für Ihre Umgebung.
2. Um die Proton-Bibliothek zu kompilieren, installieren Sie die folgenden Pakete:
   
    ```shell
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```
3. Laden Sie die [Qpid Proton-Bibliothek](http://qpid.apache.org/proton/index.html) herunter, und extrahieren Sie sie, z.B.:
   
    ```shell
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```
4. Erstellen Sie einen Build-Verzeichnis, kompilieren und installieren Sie:
   
    ```shell
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```
5. Erstellen Sie in Ihrem Arbeitsverzeichnis eine Datei namens **sender.c** mit folgendem Code. Denken Sie daran, die Werte durch Ihren SAS-Schlüssel/-Namen, den Event Hub-Namen und den Namespace zu ersetzen. Sie müssen auch eine URL-codierte Version des Schlüssels für das zuvor erstellte **SendRule**-Element eingeben. Die URL-Codierung können Sie [hier](http://www.w3schools.com/tags/ref_urlencode.asp) vornehmen.
   
    ```c
    #include "proton/message.h"
    #include "proton/messenger.h"
   
    #include <getopt.h>
    #include <proton/util.h>
    #include <sys/time.h>
    #include <stddef.h>
    #include <stdio.h>
    #include <string.h>
    #include <unistd.h>
    #include <stdlib.h>
   
    #define check(messenger)                                                     \
      {                                                                          \
        if(pn_messenger_errno(messenger))                                        \
        {                                                                        \
          printf("check\n");                                                     \
          die(__FILE__, __LINE__, pn_error_text(pn_messenger_error(messenger))); \
        }                                                                        \
      }
   
    pn_timestamp_t time_now(void)
    {
      struct timeval now;
      if (gettimeofday(&now, NULL)) pn_fatal("gettimeofday failed\n");
      return ((pn_timestamp_t)now.tv_sec) * 1000 + (now.tv_usec / 1000);
    }  
   
    void die(const char *file, int line, const char *message)
    {
      printf("Dead\n");
      fprintf(stderr, "%s:%i: %s\n", file, line, message);
      exit(1);
    }
   
    int sendMessage(pn_messenger_t * messenger) {
        char * address = (char *) "amqps://{SAS Key Name}:{SAS key}@{namespace name}.servicebus.windows.net/{event hub name}";
        char * msgtext = (char *) "Hello from C!";
   
        pn_message_t * message;
        pn_data_t * body;
        message = pn_message();
   
        pn_message_set_address(message, address);
        pn_message_set_content_type(message, (char*) "application/octect-stream");
        pn_message_set_inferred(message, true);
   
        body = pn_message_body(message);
        pn_data_put_binary(body, pn_bytes(strlen(msgtext), msgtext));
   
        pn_messenger_put(messenger, message);
        check(messenger);
        pn_messenger_send(messenger, 1);
        check(messenger);
   
        pn_message_free(message);
    }
   
    int main(int argc, char** argv) {
        printf("Press Ctrl-C to stop the sender process\n");
   
        pn_messenger_t *messenger = pn_messenger(NULL);
        pn_messenger_set_outgoing_window(messenger, 1);
        pn_messenger_start(messenger);
   
        while(true) {
            sendMessage(messenger);
            printf("Sent message\n");
            sleep(1);
        }
   
        // release messenger resources
        pn_messenger_stop(messenger);
        pn_messenger_free(messenger);
   
        return 0;
    }
    ```
6. Kompilieren Sie die Datei, wobei **gcc**vorausgesetzt wird:
   
    ```
    gcc sender.c -o sender -lqpid-proton
    ```

    > [!NOTE]
    > Dieser Code verwendet ein Ausgabefenster von 1, um eine sofortige Ausgabe der Meldungen zu erzwingen. Es wird empfohlen, dass Ihre Anwendungen Nachrichten stapelweise ausgeben, um den Durchsatz zu erhöhen. Informationen zur Verwendung der Qpid Proton-Bibliothek in dieser und anderen Umgebungen und auf Plattformen, für die Bindungen bereitgestellt werden (derzeit Perl, PHP, Python und Ruby), finden Sie auf der Seite [Qpid AMQP Messenger](https://qpid.apache.org/proton/messenger.html).


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Event Hubs finden Sie unter den folgenden Links:

* [Übersicht über Event Hubs](event-hubs-what-is-event-hubs.md)
* [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)

<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png
