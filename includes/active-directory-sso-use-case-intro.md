Organisationen verwenden mehrere [Software-as-a-Service (SaaS)](https://azure.microsoft.com/overview/what-is-saas/) Anwendungen für Produktivität da Cloud-basierte Technologie und Tools werden immer schneller zur Verfügung. Mit steigender Anzahl von SaaS-apps wird es schwierig, für die Administratoren zum Verwalten von Konten und Zugriffsrechte und für die Benutzer ihre verschiedenen Kennwörter merken zu müssen. Verwalten diese Anwendungen einzeln zusätzliche Aufgaben erstellt und ist weniger sicher.

* Mitarbeiter, die über viele Kennwörter des tendenziell weniger sicheren Methoden verwenden, daran zu denken, Kennwörter aufzuschreiben oder mithilfe der gleichen Kennwörter über viele Konten.
* Wenn ein neuer Mitarbeiter eingeht oder eine verlässt, müssen alle ihre Konten einzeln bereitgestellt oder Deduplizierung bereitgestellt werden.
* Darüber hinaus können Mitarbeiter mit SaaS-apps für ihre Arbeit ohne durchlaufen starten IT, d. h., sie erstellen ihre eigenen Konten auf Systemen, die der IT-Administratoren noch nicht genehmigt und werden nicht überwachen.  

Eine Lösung für alle zu dieser Komplexität ist einmaliges Anmelden für (Unternehmen SSO). Es ist die einfachste Methode zum Verwalten von mehreren apps und Benutzern eine konsistente Umgebung anmelden zur Verfügung stellen. Azure Active Directory (Azure AD) bietet eine zuverlässige SSO-Lösung und verfügt über viele verfügbare vorab integrierte Anwendungen, mit der Lernprogramme für "Administratoren" können Sie schnell eine neue app einrichten und Starten der Bereitstellung von Benutzern.

## <a name="how-does-azure-active-directory-integrate-apps"></a>Wie integrieren Azure Active Directory apps?
Azure AD ermöglicht Ihnen, Ihre apps und bereitgestellten Konten zu integrieren. Dies kann durch einen der beiden Ansätze erfolgen.

* Wenn die app bereits in der app-Katalog integriert ist, gelangen Sie über dieses Portal apps einrichten und Konfigurieren der Einstellungen zum Zulassen von SSO. Für jede app Gallery möglich loslegen zu folgen, die einfache schrittanleitungen in der app-Katalog und in der Azure-Portal einmaliges Anmelden ermöglichen.
* Wenn die app nicht im Katalog vorhanden ist, können Sie die meisten apps weiterhin in Azure AD als eine benutzerdefinierte app einrichten. Dies erfordert etwas besser Expertenwissen zu konfigurieren. Sie können jede Anwendung, die SAML 2.0 als verbundene app unterstützt, oder jede Anwendung, die eine HTML-basierte Anmeldeseite als Kennwort-SSO-Anwendung hinzufügen.

Im Fall, in denen Benutzer ihre eigenen Konten für SaaS-apps, die von verwaltet werden nicht erstellt haben, IT, die [Cloud App Discovery](../articles/active-directory/active-directory-cloudappdiscovery-whatis.md) Tool bietet eine Lösung. Dieses Tool überwacht den Webdatenverkehr, um zu ermitteln, welche apps in der gesamten Organisation und die Anzahl der Personen, die mit den einzelnen von ihnen verwendet werden. IT können diese Informationen verwenden, um apps zu erhalten, die Benutzer bevorzugen, und entscheiden, welche für SSO in Azure AD zu integrieren.  

Wenn Sie eine app in Azure AD zu integrieren, können Sie auferlegen eingerichteten Anwendung zuordnen, zu ihren jeweiligen Azure AD-Identitäten.  

