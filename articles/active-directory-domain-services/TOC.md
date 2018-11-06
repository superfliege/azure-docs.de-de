# [Dokumentation zu Azure AD Domain Services](index.yml)

# Übersicht
## [Was ist Azure AD Domain Services?](active-directory-ds-overview.md)
## [Häufig gestellte Fragen](active-directory-ds-faqs.md)
## Ist der Dienst für Sie geeignet?
### [Vergleichen mit Windows Server AD](active-directory-ds-comparison.md)
### [Vergleichen mit Azure AD Join](active-directory-ds-compare-with-azure-ad-join.md)
## [Neuerungen](https://azure.microsoft.com/updates/?product=active-directory-ds)
## [Funktionen](active-directory-ds-features.md)
## [Szenarios](active-directory-ds-scenarios.md)
## [Funktionsweise der Synchronisierung](active-directory-ds-synchronization.md)
## [Kompatible Drittanbietersoftware](active-directory-ds-compatible-software.md)

# Erste Schritte
## [Aufgabe 1: Konfigurieren der Grundeinstellungen](active-directory-ds-getting-started.md)
## [Aufgabe 2: Konfigurieren der Netzwerkeinstellungen](active-directory-ds-getting-started-network.md)
## [Aufgabe 3: Konfigurieren der Administratorgruppe und Aktivieren von Azure AD Domain Services](active-directory-ds-getting-started-admingroup.md)
## [Aufgabe 4: Aktualisieren der DNS-Einstellungen für das virtuelle Netzwerk](active-directory-ds-getting-started-dns.md)
## [Aufgabe 5: Aktivieren der Kennworthashsynchronisierung](active-directory-ds-getting-started-password-sync.md)

# Anleitung
## [Überprüfen der Integrität einer verwalteten Domäne](active-directory-ds-check-health.md)
## [Verwenden von Azure AD Domain Services in Azure CSP-Abonnements](active-directory-ds-csp.md)
## [Enable Azure AD Domain Services using PowerShell (Aktivieren von Azure AD Domain Services mit PowerShell)](active-directory-ds-enable-using-powershell.md)
## [Konfigurieren der bereichsbezogenen Synchronisierung von Azure AD mit einer verwalteten Domäne](active-directory-ds-scoped-synchronization.md)
## [Schützen Ihrer verwalteten Domäne](active-directory-ds-secure-your-domain.md)
## [Erstellen einer Organisationseinheit für eine verwaltete Domäne](active-directory-ds-admin-guide-create-ou.md)
## [Erstellen eines gruppenverwalteten Dienstkontos in einer verwalteten Domäne](active-directory-ds-create-gmsa.md)
## [Verwalten von Gruppenrichtlinien für eine verwaltete Domäne](active-directory-ds-admin-guide-administer-group-policy.md)
## [Konfigurieren von Kennwortrichtlinien für eine verwaltete Domäne](active-directory-ds-password-policy.md)
## [Auswählen eines virtuellen Netzwerks](active-directory-ds-networking.md)
## Bereitstellen von Anwendungen
### [Konfigurieren der Unterstützung für die Profilsynchronisierung für SharePoint Server](active-directory-ds-enable-sharepoint-profile-sync.md)
### [Konfigurieren der eingeschränkten Kerberos-Delegierung](active-directory-ds-enable-kcd.md)
### [Bereitstellen eines Azure AD-Anwendungsproxys](active-directory-ds-deploy-azure-app-proxy.md)
## [Löschen einer verwalteten Domäne](active-directory-ds-disable-aadds.md)

# Beitreten zu einer verwalteten Domäne
## [Virtueller Windows Server-Computer](active-directory-ds-admin-guide-join-windows-vm-portal.md)
## [Virtueller Windows Server-Computer aus einer Vorlage](active-directory-ds-join-windows-vm-template.md)
## [CentOS](active-directory-ds-join-centos-linux-vm.md)
## [CoreOS](active-directory-ds-join-coreos-linux-vm.md)
## [Red Hat Enterprise Linux](active-directory-ds-join-rhel-linux-vm.md)
## [Ubuntu Server](active-directory-ds-join-ubuntu-linux-vm.md)
# Verwalten einer verwalteten Domäne
## [Verwalten einer verwalteten Domäne](active-directory-ds-admin-guide-administer-domain.md)
## [Verwalten des DNS in einer verwalteten Domäne](active-directory-ds-admin-guide-administer-dns.md)
## [Aktualisieren von E-Mail-Benachrichtigungen](active-directory-ds-notifications.md)
# Konfigurieren von sicherem LDAP für eine verwaltete Domäne
## [Aufgabe 1: Erwerben eines Zertifikats für Secure LDAP](active-directory-ds-admin-guide-configure-secure-ldap.md)
## [Aufgabe 2: Exportieren des Zertifikats für Secure LDAP](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md)
## [Aufgabe 3: Aktivieren von Secure LDAP für die verwaltete Domäne über das Azure-Portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
## [Aufgabe 4: Konfigurieren von DNS für den Zugriff auf die verwaltete Domäne über das Internet](active-directory-ds-ldaps-configure-dns.md)
## [Aufgabe 5: Binden an die verwaltete Domäne und Sperren des Secure LDAP-Zugriffs](active-directory-ds-ldaps-bind-lockdown.md)
## [Problembehandlung für Secure LDAP](active-directory-ds-ldaps-troubleshoot.md)

# Problembehandlung
## [Handbuch zur Problembehandlung](active-directory-ds-troubleshooting.md)
## [Problembehandlung für Warnungen](active-directory-ds-troubleshoot-alerts.md)
### [Korrigieren einer fehlerhaften NSG-Konfiguration](active-directory-ds-troubleshoot-nsg.md)
### [Wiederherstellen fehlender Dienstprinzipale](active-directory-ds-troubleshoot-service-principals.md)
### [Fehler im Zusammenhang mit Secure LDAP](active-directory-ds-troubleshoot-ldaps.md)
## [Beheben von Fehlern durch nicht übereinstimmende Mandanten](active-directory-ds-mismatched-tenant-error.md)
## [Angehaltene Domänen](active-directory-ds-suspension.md)


# Verweis
## [Codebeispiele](https://azure.microsoft.com/resources/samples/?service=active-directory)

# Verwandte Themen
## [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md)
## [Azure Active Directory B2C](../active-directory-b2c/active-directory-b2c-overview.md)
## [Multi-Factor Authentication](../active-directory/authentication/multi-factor-authentication.md)

# Ressourcen
## [Azure AD-Feedbackforum](https://feedback.azure.com/forums/169401-azure-active-directory)
## [Azure-Roadmap](https://azure.microsoft.com/roadmap/?category=security-identity)
## [So erreichen Sie uns](active-directory-ds-contact-us.md)
## [Preise](https://azure.microsoft.com/pricing/details/active-directory-ds/)
## [Preisrechner](https://azure.microsoft.com/pricing/calculator/)
## [Dienstupdates](https://azure.microsoft.com/updates/?product=active-directory-ds)
