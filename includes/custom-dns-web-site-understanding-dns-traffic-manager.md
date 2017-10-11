Domain Name System (DNS) wird verwendet, um Punkte im Internet zu suchen. Z. B. Wenn Sie eine Adresse in Ihrem Browser eingeben, oder klicken Sie auf einen Link auf einer Webseite, verwendet er DNS, um die Domäne in eine IP-Adresse zu übersetzen. Die IP-Adresse ist, wie eine Anschrift an, aber es ist nicht sehr menschlichen freundliche. Beispielsweise ist es viel einfacher, einen DNS-Namen wie merken **"contoso.com"** als eine IP-Adresse wie z. B. 192.168.1.88 oder 2001:0:4137:1f67:24a2:3888:9cce:fea3 zu merken ist.

Der DNS-System basiert auf *Datensätze*. Datensätze zuordnen einen bestimmten *Namen*, wie z. B. **"contoso.com"**, mit einer IP-Adresse oder einen anderen DNS-Namen. Wenn eine Anwendung, z. B. ein Webbrowser einen Namen im DNS nachgeschlagen werden, sucht den Datensatz, und verwendet, was sie als Adresse zeigt. Wenn der Wert, dem er verweist eine IP-Adresse ist, wird der Browser diesen Wert verwenden. Wenn an einen anderen DNS-Namen verwiesen wird, muss die Anwendung, führen Sie die Lösung erneut. Schließlich wird die gesamte namensausauflösung in eine IP-Adresse beendet.

Wenn Sie eine Azure-Website erstellen, wird ein DNS-Name der Website automatisch zugewiesen. Dieser Name hat das Format  **&lt;Yoursitename&gt;. azurewebsites.net**. Wenn Sie Ihre Website als Azure Traffic Manager-Endpunkt hinzufügen, wird Ihre Website klicken Sie dann über die  **&lt;Yourtrafficmanagerprofile&gt;. trafficmanager.net** Domäne.

> [!NOTE]
> Wenn Ihre Website als Traffic Manager-Endpunkt konfiguriert ist, verwenden Sie die **. trafficmanager.net** Adresse bei der DNS-Einträge zu erstellen.
> 
> CNAME-Einträge können mit Traffic Manager Sie nur
> 
> 

Es gibt auch mehrere Arten von-Datensätze; jeder mit ihren eigenen Funktionen und Einschränkungen, jedoch für Websites, die so konfiguriert, dass als Traffic Manager-Endpunkten, wir nur erledigen zu einem; *CNAME* Datensätze.

### <a name="cname-or-alias-record"></a>CNAME oder Alias-Datensatz
Ein CNAME-Eintrag ordnet eine *bestimmte* DNS-Server ein, z. B. **an "Mail.contoso.com"** oder **www.contoso.com**, zum anderen (kanonischen) Domänennamens. Im Fall von Azure-Websites mit Traffic Manager, der kanonische Domänenname ist die  **&lt;"MyApp" >. trafficmanager.net** Domänennamen Ihres Traffic Manager-Profils. Nach der Erstellung, erstellt der CNAME-Eintrag einen Alias für die  **&lt;"MyApp" >. trafficmanager.net** Domänennamen. Der CNAME-Eintrag in die IP-Adresse aufgelöst wird Ihre  **&lt;"MyApp" >. trafficmanager.net** Domänenname automatisch, wenn die IP-Adresse der Website geändert wird, haben Sie keine Maßnahmen ergreifen.

Nach der Datenverkehr am Traffic Manager geht, leitet dann den Datenverkehr zu Ihrer Website, mit der Lastenausgleichsmethode, es konfiguriert ist, weiter. Dies ist für Besucher Ihrer Website vollständig transparent. Es werden nur der Name der benutzerdefinierten Domäne in ihrem Browser angezeigt.

> [!NOTE]
> Einige domänenregistrierungsstellen vorschreiben Unterdomänen zuordnen, wenn Sie einen CNAME-Eintrag wie **www.contoso.com**, und nicht-root-Namen, z. B. **"contoso.com"**. Weitere Informationen zu den CNAME-Einträge, finden Sie in der Dokumentation von Ihrer Registrierungsstelle <a href="http://en.wikipedia.org/wiki/CNAME_record">Wikipedia-Eintrag für den CNAME-Eintrag</a>, oder die <a href="http://tools.ietf.org/html/rfc1035">Domänennamen IETF - Implementierung und Spezifikation</a> Dokument.
> 
> 

