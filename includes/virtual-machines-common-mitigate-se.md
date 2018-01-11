
**Letzte Aktualisierung des Dokuments**: 7. Januar, 3:30 Uhr MEZ

Eine vor kurzem öffentlich gemachte [neue Art von CPU-Sicherheitsrisiken](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002), die als „Seitenkanalangriffe mit spekulativer Ausführung“ bezeichnet wird, hatte Fragen von Kunden zur Folge, sich mehr Klarheit wünschen.  

Die Infrastruktur, in der Azure ausgeführt und Kundenworkloads voneinander isoliert werden, ist geschützt.  Dies bedeutet, dass andere Kunden in Azure diese Sicherheitsrisiken nicht für Angriffe auf Ihre Anwendung ausnutzen können.

## <a name="keeping-your-operating-systems-up-to-date"></a>Halten Sie Ihre Betriebssysteme auf dem neuesten Stand

Ein Betriebssystemupdate ist zwar nicht erforderlich, um Ihre in Azure ausgeführten Anwendungen von anderen Kunden in Azure zu isolieren, es gilt jedoch immer als bewährte Methode, die Versionen von Betriebssystemen auf dem neuesten Stand zu halten. 

Die folgenden Angebote enthalten unsere empfohlenen Aktionen zur Aktualisierung Ihres Betriebssystems: 

<table>
<tr>
<th>Angebot</th> <th>Empfohlene Maßnahme </th>
</tr>
<tr>
<td>Azure Cloud Services </td>  <td>Aktivieren Sie automatische Updates, oder stellen Sie sicher, dass das neueste Gastbetriebssystem ausgeführt wird.</td>
</tr>
<tr>
<td>Virtuelle Azure Linux-Computer</td> <td>Installieren Sie bei Verfügbarkeit Updates vom Anbieter Ihres Betriebssystems. </td>
</tr>
<tr>
<td>Virtuelle Azure Windows-Computer </td> <td>Stellen Sie vor dem Installieren von Betriebssystemupdates sicher, dass eine unterstützte Antivirenanwendung ausgeführt wird. Holen Sie Informationen zur Kompatibilität vom Anbieter der Antivirensoftware ein.<p> Installieren Sie das [Sicherheitsrollup von Januar](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002). </p></td>
</tr>
<tr>
<td>Weitere Azure PaaS-Dienste</td> <td>Für Kunden, die diese Dienste verwenden, sind keine Maßnahmen erforderlich. Azure hält Ihre Betriebssystemversionen automatisch auf dem neuesten Stand. </td>
</tr>
</table>

## <a name="additional-guidance-if-you-are-running-untrusted-code"></a>Zusätzliche Anleitungen für den Fall, dass nicht vertrauenswürdiger Code ausgeführt wird 

Weitere Kundenaktionen sind nur erforderlich, wenn nicht vertrauenswürdiger Code ausgeführt wird. Wenn Sie Code zulassen, der nicht vertrauenswürdig ist (z.B. indem Sie einem Ihrer Kunden das Hochladen einer Binärdatei oder eines Codeausschnitts gewähren, die bzw. den Sie dann in der Cloud innerhalb Ihrer Anwendung ausführen), müssen die folgenden zusätzlichen Schritte ausgeführt werden.  


### <a name="windows"></a>Windows 
Wenn Sie Windows verwenden und nicht vertrauenswürdigen Code hosten, müssen Sie auch die Windows-Funktion zur Remotesteuerung der virtuellen Kerneladresse aktivieren, die zusätzlichen Schutz gegen Sicherheitsrisiken durch Seitenkanalangriffe mit spekulativer Ausführung bietet. Diese Funktion ist standardmäßig deaktiviert, da sie im aktivierten Zustand die Leistung beeinträchtigen kann. Führen Sie die Anweisungen in [Windows Server KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution) aus, um Schutzmaßnahmen auf dem Server zu aktivieren. Wenn Sie Azure Cloud Services ausführen, stellen Sie sicher, dass „WA-GUEST-OS-5.15_201801-01“ oder „WA-GUEST-OS-4.50_201801-01“ (verfügbar ab 10. Januar) ausgeführt wird, und aktivieren Sie den Registrierungsschlüssel über eine Startaufgabe.


### <a name="linux"></a>Linux
Wenn Sie Linux verwenden und nicht vertrauenswürdigen Code hosten, müssen Sie auch Linux auf eine neuere Version aktualisieren, die KPTI (Kernel Page-Table Isolation) implementiert. Dadurch werden die Seitentabellen im Kernelspeicher von denen im Benutzerbereich getrennt. Diese Risikominderungen erfordern ein Update des Linux-Betriebssystems und können bei Verfügbarkeit von Ihrem Distributionsanbieter bezogen werden. Der Anbieter Ihres Betriebssystems kann Auskunft darüber geben, ob Schutzmaßnahmen standardmäßig aktiviert oder deaktiviert sind.








## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Schützen von Azure-Kunden vor CPU-Sicherheitsrisiken](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/).