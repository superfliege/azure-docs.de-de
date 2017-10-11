---
title: Azure AD-Java-Befehlszeile erste Schritte | Microsoft Docs
description: Vorgehensweise zum Erstellen einer Java-Befehlszeile-app, die Benutzer meldet sich auf eine API zugreifen.
services: active-directory
documentationcenter: java
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 51e1a8f9-6ff0-4643-a350-0ba794e26fd1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 01/23/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 91e4a7b2ac454465d5cce4948a4d5f0b542d2b55
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="using-java-command-line-app-to-access-an-api-with-azure-ad"></a>Mithilfe von Java-Befehlszeile App Zugriff auf eine-API bei Azure AD
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure AD erleichtert einfach und unkompliziert zum Auslagern der identitätsverwaltung für Ihre Web-app, bietet einmaliges an- und Abmeldung mit nur wenigen Codezeilen.  In Java-Web-apps erreichen dies Sie mit der Microsoft-Implementierung von der Community gesteuerte ADAL4J.

  Verwenden Sie hier ADAL4J an:

* Melden Sie den Benutzer in der app mithilfe von Azure AD als Identitätsanbieter an.
* Einige Informationen über den Benutzer angezeigt.
* Melden Sie den Benutzer aus einer app.

Zu diesem Zweck müssen Sie Folgendes ausführen:

1. Registrieren einer Anwendung in Azure AD
2. Richten Sie Ihre app aus, um die ADAL4J-Bibliothek verwenden.
3. Verwenden der Bibliothek ADAL4J an- und Abmelde Anforderungen an Azure AD ausgibt.
4. Drucken Sie Daten über den Benutzer aus.

Um zu beginnen, [Herunterladen der app Skeleton](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) oder [Laden Sie das vollständige Beispiel](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\\/archive/complete.zip).  Sie benötigen auch einen Azure AD-Mandanten, in dem Ihre Anwendung registrieren.  Wenn Sie noch, besitzen keine [erfahren Sie, wie eine](active-directory-howto-tenant.md).

## <a name="1--register-an-application-with-azure-ad"></a>1.  Registrieren einer Anwendung in Azure AD
Um Ihre app zur Authentifizierung von Benutzern zu aktivieren, müssen Sie zunächst eine neue Anwendung in Ihrem Mandanten registriert.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie in der oberen Menüleiste auf, auf Ihr Konto und unter der **Directory** wählen Sie die Active Directory-Mandanten, wo Ihre Anwendung registriert werden sollen.
3. Klicken Sie auf **mehr Dienste** im linken Navigationsbereich, und wählen Sie **Azure Active Directory**.
4. Klicken Sie auf **App Registrierungen** , und wählen Sie **hinzufügen**.
5. Folgen Sie den Assistenten, und erstellen Sie ein neues **Webanwendung und/oder WebAPI**.
  * Die **Namen** der Anwendung wird die Anwendung für Endbenutzer beschreiben
  * Die **Anmelde-URL** ist die base-URL Ihrer App.  Das Skelett Standardwert ist `http://localhost:8080/adal4jsample/`.
6. Sobald Sie die Registrierung abgeschlossen haben, wird AAD Ihre app eine eindeutige Anwendung-ID zuweisen  Sie benötigen diesen Wert in den nächsten Abschnitten so kopieren Sie ihn aus der Registerkarte "Anwendung".
7. Aus der **Einstellungen** -> **Eigenschaften** Seite für Ihre Anwendung, aktualisieren Sie die App-ID-URI. Die **App ID-URI** ist ein eindeutiger Bezeichner für Ihre Anwendung.  Die Konvention ist die Verwendung `https://<tenant-domain>/<app-name>`, z. B. `http://localhost:8080/adal4jsample/`.

Einmal im Portal für Ihre app erstellen eine **Schlüssel** aus der **Einstellungen** Seite für Ihre Anwendung, und kopieren Sie ihn nach unten.  Sie benötigen ihn in Kürze.

## <a name="2-set-up-your-app-to-use-adal4j-library-and-prerequisites-using-maven"></a>2. Richten Sie Ihre app ADAL4J-Bibliothek und erforderlichen Komponenten mit Maven verwenden
Konfigurieren Sie hier ADAL4J, um die OpenID Connect-Authentifizierungsprotokoll verwenden.  ADAL4J wird zum an- und Abmelde Anforderungen ausstellen, verwalten die Sitzung des Benutzers und Abrufen von Informationen zum Benutzer, unter anderem verwendet werden.

* Im Stammverzeichnis des Projekts, geöffnet/erstellt `pom.xml` und suchen Sie die `// TODO: provide dependencies for Maven` und Ersetzen Sie durch Folgendes:

```Java
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>public-client-adal4j-sample</artifactId>
    <packaging>jar</packaging>
    <version>0.0.1-SNAPSHOT</version>
    <name>public-client-adal4j-sample</name>
    <url>http://maven.apache.org</url>
    <properties>
        <spring.version>3.0.5.RELEASE</spring.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>adal4j</artifactId>
            <version>1.1.2</version>
        </dependency>
        <dependency>
            <groupId>com.nimbusds</groupId>
            <artifactId>oauth2-oidc-sdk</artifactId>
            <version>4.5</version>
        </dependency>
        <dependency>
            <groupId>org.json</groupId>
            <artifactId>json</artifactId>
            <version>20090211</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.0.1</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.5</version>
        </dependency>
</dependencies>
    <build>
        <finalName>public-client-adal4j-sample</finalName>
        <plugins>
                <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>1.2.1</version>
            <configuration>
                <mainClass>PublicClient</mainClass>
            </configuration>
        </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <executions>
                    <execution>
                        <id>install</id>
                        <phase>install</phase>
                        <goals>
                            <goal>sources</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>2.5</version>
                <configuration>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
        <artifactId>maven-assembly-plugin</artifactId>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>single</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
          </descriptorRefs>
        </configuration>
      </plugin>
      <plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-assembly-plugin</artifactId>
  <configuration>
    <archive>
      <manifest>
        <mainClass>PublicClient</mainClass>
      </manifest>
    </archive>
  </configuration>
</plugin>
        </plugins>
    </build>

</project>


```



## <a name="3-create-the-java-publicclient-file"></a>3. Die Java-PublicClient-Datei erstellen
Wie oben angegeben, werden die Graph-API verwendet. zum Abrufen von Daten über den angemeldeten Benutzer. Für diese Option, um uns einfach werden erstellen wir die sowohl eine Datei zur Darstellung einer **Verzeichnisobjekt** und eine einzelne Datei zur Darstellung der **Benutzer** , damit das Muster OO Java verwendet werden kann.

* Erstellen Sie eine Datei namens `DirectoryObject.java` das Lernprogramm zu Data über alle DirectoryObject speichern (Sie können gerne ihn später für andere Graph-Abfragen verwenden, weisen verwenden können) verwendet wird. Sie können Ausschneiden und dadurch von unten einfügen:

```Java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

import javax.naming.ServiceUnavailableException;

import com.microsoft.aad.adal4j.AuthenticationContext;
import com.microsoft.aad.adal4j.AuthenticationResult;

public class PublicClient {

    private final static String AUTHORITY = "https://login.microsoftonline.com/common/";
    private final static String CLIENT_ID = "2a4da06c-ff07-410d-af8a-542a512f5092";

    public static void main(String args[]) throws Exception {

        try (BufferedReader br = new BufferedReader(new InputStreamReader(
                System.in))) {
            System.out.print("Enter username: ");
            String username = br.readLine();
            System.out.print("Enter password: ");
            String password = br.readLine();

            AuthenticationResult result = getAccessTokenFromUserCredentials(
                    username, password);
            System.out.println("Access Token - " + result.getAccessToken());
            System.out.println("Refresh Token - " + result.getRefreshToken());
            System.out.println("ID Token - " + result.getIdToken());
        }
    }

    private static AuthenticationResult getAccessTokenFromUserCredentials(
            String username, String password) throws Exception {
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(AUTHORITY, false, service);
            Future<AuthenticationResult> future = context.acquireToken(
                    "https://graph.windows.net", CLIENT_ID, username, password,
                    null);
            result = future.get();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;
    }
}


```


## <a name="compile-and-run-the-sample"></a>Kompilieren und Ausführen des Beispiels
Ändern Sie in Ihrem Stammverzeichnis wieder, und führen den folgenden Befehl zum Erstellen des Beispiels einfach zusammen mit `maven`. Dies wird mithilfe der `pom.xml` Datei, die Sie geschrieben, für die Abhängigkeiten haben.

`$ mvn package`

Sie verfügen jetzt über eine `adal4jsample.war` Datei Ihre `/targets` Verzeichnis. Sie können bereitstellen, die im Tomcat-Container und besuchen Sie 

`http://localhost:8080/adal4jsample/`

> [!NOTE]
> Es ist sehr einfach, eine WAR mit den neuesten Tomcat-Servern bereitstellen. Wechseln Sie einfach zu `http://localhost:8080/manager/` und befolgen Sie die Anweisungen zum Hochladen Ihrer '' adal4jsample.war "Datei. Mit den richtigen Endpunkt tritt ein Autodeploy für Sie.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
Gratulation! Sie verfügen jetzt über eine funktionierende Java-Anwendung, die zum Authentifizieren von Benutzern, sichere sicherheitskontodatenbanken aufrufen, Web-APIs, die mithilfe von OAuth 2.0, und erhalten grundlegende Informationen über den Benutzer.  Wenn Sie nicht bereits getan haben, ist jetzt die Zeit zum Auffüllen von Ihrem Mandanten mit einigen Benutzern.

Zu Referenzzwecken im abgeschlossenen Beispiel (ohne die Konfigurationswerte) [dient als eine ZIP hier](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip), oder Sie können von GitHub Klonen:

```git clone --branch complete https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect.git```

