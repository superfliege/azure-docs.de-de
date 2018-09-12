# [Azure Active Directory-Dokumentation](index.md)

# Übersicht
## [Was ist Azure Active Directory?](fundamentals/active-directory-whatis.md)
## [Informationen zur Azure-Identitätsverwaltung](fundamentals/identity-fundamentals.md)
## [Grundlegendes zu Azure-Identitätslösungen](fundamentals/understand-azure-identity-solutions.md)
## [Auswählen einer Hybrididentitätslösung](choose-hybrid-identity-solution.md)
## [Zuordnen von Azure-Abonnements](fundamentals/active-directory-how-subscriptions-associated-directory.md)
## [Überlegungen zu Residenz und Daten](fundamentals/active-directory-data-storage-eu.md)
## [Häufig gestellte Fragen](fundamentals/active-directory-faq.md)
## [Neuerungen](fundamentals/whats-new.md)


# Erste Schritte
## [Erste Schritte mit Azure AD](fundamentals/get-started-azure-ad.md)
## [Registrieren für Azure AD Premium](fundamentals/active-directory-get-started-premium.md)
## [Hinzufügen eines benutzerdefinierten Domänennamens](fundamentals/add-custom-domain.md)
## [Konfigurieren des Unternehmensbrandings](fundamentals/customize-branding.md)
## [Hinzufügen von Benutzern zu Azure AD](fundamentals/add-users-azure-active-directory.md)
## [Zuweisen von Lizenzen zu Benutzern](fundamentals/license-users-groups.md)
## [Konfigurieren der Self-Service-Kennwortzurücksetzung](authentication/quickstart-sspr.md)
## [Hinzufügen der Datenschutzinformationen Ihrer Organisation in Azure AD](active-directory-properties-area.md)


# Anleitung
## Planen und Entwerfen
### [Informationen zur Azure AD-Architektur](fundamentals/active-directory-architecture.md)
### [Zuordnen von Benutzeransprüchen in Azure Active Directory](active-directory-claims-mapping.md)
### [Bereitstellen einer Hybrididentitätslösung](active-directory-hybrid-identity-design-considerations-overview.md)
#### Bestimmen der Anforderungen
##### [Identität](active-directory-hybrid-identity-design-considerations-business-needs.md)
##### [Verzeichnissynchronisierung](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)
##### [Mehrstufige Authentifizierung](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)
##### [Strategie zum Identitätslebenszyklus](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)
#### [Plan für Datensicherheit](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)
##### [Datenschutz](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)
##### [Content Management](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)
##### [Zugriffssteuerung](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)
##### [Reaktion auf Vorfälle](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)
#### Planen des Identitätslebenszyklus
##### [Aufgaben](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)
##### [Strategie für die Einführung](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)
#### [Nächste Schritte](active-directory-hybrid-identity-design-considerations-nextsteps.md)
#### [Vergleich von Tools](active-directory-hybrid-identity-design-considerations-tools-comparison.md)

## Verwalten von Benutzern
### [Hinzufügen neuer Benutzer zu Azure AD](fundamentals/add-users-azure-active-directory.md)
### [Verwalten von Benutzerprofilen](fundamentals/active-directory-users-profile-azure-portal.md)
### [Zurücksetzen von Benutzerkennwörtern](fundamentals/active-directory-users-reset-password-azure-portal.md)
### [Freigeben von Konten](active-directory-sharing-accounts.md)
### [Zuweisen von Benutzern zu Administratorrollen](fundamentals/active-directory-users-assign-role-azure-portal.md)
### [Hinzufügen von Gastbenutzern aus einem anderen Verzeichnis (B2B)](b2b/what-is-b2b.md)
#### [Administratoren, die B2B-Benutzer hinzufügen](b2b/add-users-administrator.md)
#### [Information-Worker, die B2B-Benutzer hinzufügen](b2b/add-users-information-worker.md)
#### [API und Anpassung](b2b/customize-invitation-api.md)
#### [Verbund mit Google](b2b/google-federation.md)
#### [Code- und Azure PowerShell-Beispiele](b2b/code-samples.md)
#### [Beispiel für Self-Service-Registrierungsportal](b2b/self-service-portal.md)
#### [Einladungs-E-Mail](b2b/invitation-email-elements.md)
#### [Einlösen der Einladung](b2b/redemption-experience.md)
#### [Hinzufügen von B2B-Benutzern ohne Einladung](b2b/add-user-without-invite.md)
#### [Zulassen oder Blockieren von Einladungen](b2b/allow-deny-list.md)
#### [Bedingter Zugriff für B2B](b2b/conditional-access.md)
#### [B2B-Freigaberichtlinien](b2b/delegate-invitations.md)
#### [Hinzufügen eines B2B-Benutzers zu einer Rolle](b2b/add-guest-to-role.md)
#### [Dynamische Gruppen und B2B-Benutzer](b2b/use-dynamic-groups.md)
#### [Verlassen einer Organisation](b2b/leave-the-organization.md)
#### [Überwachung und Berichte](b2b/auditing-and-reporting.md)
#### [B2B für Hybridorganisationen](b2b/hybrid-organizations.md)
##### [Gewähren des Zugriffs auf lokale Apps für B2B-Benutzer](b2b/hybrid-cloud-to-on-premises.md)
##### [Gewähren des Zugriffs auf Cloud-Apps für lokale Benutzer](b2b/hybrid-on-premises-to-cloud.md)
#### [B2B und externe Office 365-Freigaben](b2b/o365-external-user.md)
#### [B2B-Lizenzierung](b2b/licensing-guidance.md)
#### [Aktuelle Einschränkungen](b2b/current-limitations.md)
#### [HÄUFIG GESTELLTE FRAGEN](b2b/faq.md)
#### [Problembehandlung für B2B](b2b/troubleshoot.md)
#### [Grundlegendes zu B2B-Benutzern](b2b/user-properties.md)
#### [B2B-Benutzertoken](b2b/user-token.md)
#### [B2B für integrierte Azure AD-Apps](b2b/configure-saas-apps.md)
#### [Zuordnen von B2B-Benutzeransprüchen](b2b/claims-mapping.md)
#### [Gegenüberstellung von B2B-Zusammenarbeit und B2C](b2b/compare-with-b2c.md)
#### [Anfordern von Unterstützung für B2B](b2b/get-support.md)

## [Verwalten von Gruppen und Mitgliedern](fundamentals/active-directory-manage-groups.md)
### [Gruppen verwalten](fundamentals/active-directory-groups-create-azure-portal.md)
### [Verwalten von Gruppeneinstellungen](fundamentals/active-directory-groups-settings-azure-portal.md)
## [Verwalten von Berichten](reports-monitoring/overview-reports.md)
### [Anmeldungsaktivität](reports-monitoring/concept-sign-ins.md)
### [Überwachungsaktivität](reports-monitoring/concept-audit-logs.md)
### [Gefährdete Benutzer](reports-monitoring/concept-user-at-risk.md)
### [Riskante Anmeldungen](reports-monitoring/concept-risky-sign-ins.md)
### [Risikoereignisse](reports-monitoring/concept-risk-events.md)
### [Überwachen von Protokollen mithilfe von Azure Monitor](reports-monitoring/overview-activity-logs-in-azure-monitor.md)
### [HÄUFIG GESTELLTE FRAGEN](reports-monitoring/reports-faq.md)

### Aufgaben
#### [Konfigurieren benannter Orte](active-directory-named-locations.md)
#### [Suchen nach Aktivitätsberichten](reports-monitoring/howto-find-activity-reports.md)
#### [Verwenden des Azure AD-Power BI-Inhaltspakets](reports-monitoring/howto-power-bi-content-pack.md)
#### [Bereinigen von Benutzern mit Risikomarkierung](reports-monitoring/howto-remediate-users-flagged-for-risk.md)
#### [Weiterleiten von Aktivitätsprotokollen an eine Azure Event Hub-Instanz](reports-monitoring/quickstart-azure-monitor-stream-logs-to-event-hub.md)
#### [Archivieren von Aktivitätsprotokollen in einem Azure-Speicherkonto](reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md)
#### [Integrieren von Aktivitätsprotokollen mit Splunk unter Verwendung von Azure Monitor](reports-monitoring/tutorial-integrate-activity-logs-with-splunk.md)
#### [Integrieren von Aktivitätsprotokollen mit SumoLogic unter Verwendung von Azure Monitor](reports-monitoring/howto-integrate-activity-logs-with-sumologic.md)

### Verweis
#### [Vermerkdauer](reports-monitoring/reference-reports-data-retention.md)
#### [Wartezeiten](reports-monitoring/reference-reports-latencies.md)
#### [Referenz zu Überwachungsaktivitäten](reports-monitoring/reference-audit-activities.md)
#### [Fehlercodes für Anmeldeaktivitäten](reports-monitoring/reference-sign-ins-error-codes.md)
#### [Interpretieren des Überwachungsprotokollschemas in Azure Monitor](reports-monitoring/reference-azure-monitor-audit-log-schema.md)
#### [Interpretieren des Anmeldeprotokollschemas in Azure Monitor](reports-monitoring/reference-azure-monitor-sign-ins-log-schema.md)

### Problembehandlung
#### [Fehlende Daten in Azure AD-Aktivitätsprotokollen](reports-monitoring/troubleshoot-missing-audit-data.md)
#### [Fehlende Daten in den Downloads](reports-monitoring/troubleshoot-missing-data-download.md)
#### [Azure AD-Aktivitätsprotokolle: Fehler in Inhaltspaketen](reports-monitoring/troubleshoot-content-pack.md)
#### [Fehler in der Azure AD Reporting-API](reports-monitoring/troubleshoot-graph-api.md)

### [Programmgesteuerter Zugriff](reports-monitoring/concept-reporting-api.md)
#### [Voraussetzungen](reports-monitoring/howto-configure-prerequisites-for-reporting-api.md)
#### [Verwenden von Zertifikaten](reports-monitoring/tutorial-access-api-with-certificates.md)

## [Verwalten von Kennwörtern](authentication/concept-sspr-howitworks.md)

## Verwalten von Apps
### [Übersicht](manage-apps/what-is-application-management.md)
### [Erste Schritte](manage-apps/plan-an-application-integration.md)
### [Tutorials für die Integration von SaaS-Apps](saas-apps/tutorial-list.md)

### [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von SaaS-Apps](manage-apps/user-provisioning.md) 
#### [Tutorials für die App-Integration](saas-apps/tutorial-list.md) 
#### [Automatisieren der Bereitstellung in SCIM-fähigen Apps](manage-apps/use-scim-to-provision-users-and-groups.md) 
#### [Anpassen von Attributzuordnungen](manage-apps/customize-application-attributes.md) 
#### [Schreiben von Ausdrücken für Attributzuordnungen in Azure Active Directory](manage-apps/functions-for-customizing-application-data.md) 
#### [Verwenden von Bereichsfiltern](manage-apps/define-conditional-rules-for-provisioning-user-accounts.md) 
#### [Berichterstellung zur automatischen Benutzerbereitstellung](manage-apps/check-status-user-account-provisioning.md) 
#### [Problembehandlung bei der Benutzerbereitstellung](active-directory-application-provisioning-content-map.md) 

### [Remotezugriff auf Apps mit dem Anwendungsproxy](manage-apps/application-proxy.md)
#### Erste Schritte
##### [Aktivieren der App-Proxy](manage-apps/application-proxy-enable.md)
##### [Veröffentlichen von Apps](manage-apps/application-proxy-publish-azure-portal.md)
##### [Benutzerdefinierte Domänen](manage-apps/application-proxy-configure-custom-domain.md)
#### [Einmaliges Anmelden](manage-apps/application-proxy-single-sign-on.md)
##### [SSO mit KCD](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)
##### [SSO mit Headern](manage-apps/application-proxy-configure-single-sign-on-with-ping-access.md)
##### [SSO mit Kennworttresoren](manage-apps/application-proxy-configure-single-sign-on-password-vaulting.md)
#### Konzepte
##### [Connectors](manage-apps/application-proxy-connectors.md)
##### [Sicherheit](manage-apps/application-proxy-security.md)
##### [Netzwerke](manage-apps/application-proxy-network-topology.md)


##### [Upgrade von TMG oder UAG](manage-apps/application-proxy-migration.md)

#### Erweiterte Konfigurationen
##### [Veröffentlichen in getrennten Netzwerken](manage-apps/application-proxy-connector-groups.md)
##### [Proxyserver](manage-apps/application-proxy-configure-connectors-with-proxy-servers.md)
##### [Ansprüche unterstützende Apps](manage-apps/application-proxy-configure-for-claims-aware-applications.md)
##### [Native Client-Apps](manage-apps/application-proxy-configure-native-client-application.md)
##### [Installation im Hintergrund](manage-apps/application-proxy-register-connector-powershell.md)
##### [Benutzerdefinierte Startseite](manage-apps/application-proxy-configure-custom-home-page.md)
##### [Übersetzen von Inline-Links](manage-apps/application-proxy-configure-hard-coded-link-translation.md)
##### [Platzhalter](manage-apps/application-proxy-wildcard.md)
##### [Entfernen von personenbezogenen Daten](manage-apps/application-proxy-remove-personal-data.md)


#### Veröffentlichen von exemplarische Vorgehensweisen
##### [Remotedesktop](manage-apps/application-proxy-integrate-with-remote-desktop-services.md)
##### [SharePoint](manage-apps/application-proxy-integrate-with-sharepoint-server.md)
##### [Microsoft Teams](manage-apps/application-proxy-integrate-with-teams.md)
##### [Tableau](manage-apps/application-proxy-integrate-with-tableau.md)
##### [Qlik](manage-apps/application-proxy-qlik.md)
#### [PowerShell](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#application_proxy_application_management) 

#### [Problembehandlung](manage-apps/application-proxy-troubleshoot.md)

### Verwalten von Unternehmens-Apps
#### [Hinzufügen einer Anwendung](manage-apps/add-application-portal.md)
#### [Anzeigen von Mandanten-Apps](manage-apps/view-applications-portal.md)
#### [Konfigurieren von einmaligem Anmelden](manage-apps/configure-single-sign-on-portal.md)
#### [Zuweisen von Benutzern](manage-apps/assign-user-or-group-access-portal.md)
#### [Anpassen des Brandings](manage-apps/change-name-or-logo-portal.md)
#### [Deaktivieren von Benutzeranmeldungen](manage-apps/disable-user-sign-in-portal.md)
#### [Entfernen von Benutzern](manage-apps/remove-user-or-group-access-portal.md)

#### [Verwalten der Bereitstellung von Benutzerkonten](manage-apps/configure-automatic-user-provisioning-portal.md)

#### [Erweiterte Zertifikatsignatur für SAML-Apps](manage-apps/certificate-signing-options.md)
#### [Ausblenden einer Anwendung auf der Benutzeroberfläche](manage-apps/hide-application-from-user-portal.md)
### [Configure Sign-In Auto-Acceleration using HRD Policy (Konfigurieren der automatischen Anmeldebeschleunigung per HRD Policy)](manage-apps/configure-authentication-for-federated-users-portal.md)
### [Migrieren von AD FS-Apps zu Azure AD](manage-apps/migrate-adfs-apps-to-azure.md) 
### [Verwalten des Zugriffs auf Apps](manage-apps/what-is-access-management.md)
#### [SSO-Zugriff](manage-apps/what-is-single-sign-on.md)
#### [Zertifikate für SSO](manage-apps/manage-certificates-for-federated-single-sign-on.md)
#### [Mandanteneinschränkungen](manage-apps/tenant-restrictions.md)
#### [Verwenden von SCIM zum Bereitstellen von Benutzern](manage-apps/use-scim-to-provision-users-and-groups.md)

### [Grundlegendes zur Zustimmung für Azure AD-Anwendungen](application-consent-experience.md)

### Problembehandlung



#### Anpassung des Zugriffsbereichs
##### [App wird nicht angezeigt](manage-apps/access-panel-troubleshoot-application-not-appearing.md)
##### [App wird unerwartet angezeigt](manage-apps/access-panel-troubleshoot-unexpected-application.md)
##### [Anmeldung nicht möglich](manage-apps/access-panel-troubleshoot-web-sign-in-problem.md)
##### [Fehler beim Installieren der Browsererweiterung](manage-apps/access-panel-extension-problem-installing.md)
##### [Verwenden des Self-Service-App-Zugriffs](manage-apps/access-panel-manage-self-service-access.md)
##### [Fehler beim Verwenden des Self-Service-App-Zugriffs](manage-apps/access-panel-troubleshoot-self-service-access.md)

#### Hinzufügen einer App
##### [Auswählen des App-Typs](manage-apps/choose-application-type.md)
##### [Allgemeine Probleme: Katalog-Apps](manage-apps/adding-gallery-app-common-problems.md)
##### [Allgemeine Probleme: Nicht-Katalog-Apps](manage-apps/adding-non-gallery-app-common-problems.md)

#### Anwendungsproxy
##### [Problem beim Anzeigen der App-Seite](manage-apps/application-proxy-page-appearance-broken-problem.md)
##### [Laden der Anwendung dauert zu lange.](manage-apps/application-proxy-page-load-speed-problem.md)
##### [Links auf der Anwendungsseite funktionieren nicht.](manage-apps/application-proxy-page-links-broken-problem.md)
##### [Für die App zu öffnende Ports](manage-apps/application-proxy-connectivity-ports-how-to.md)
##### [Kein funktionierender Connector in einer Connectorguppe für meine App](manage-apps/application-proxy-connectivity-no-working-connector.md)
##### [Konfigurieren im Verwaltungsportal](manage-apps/application-proxy-config-how-to.md)
##### [Konfigurieren des einmaligen Anmeldens für meine App](manage-apps/application-proxy-config-sso-how-to.md)
##### [Problem beim Erstellen einer App im Verwaltungsportal](manage-apps/application-proxy-config-problem.md)
##### [Konfigurieren der eingeschränkten Kerberos-Delegierung](manage-apps/application-proxy-back-end-kerberos-constrained-delegation-how-to.md)
##### [Konfigurieren mit PingAccess](manage-apps/application-proxy-back-end-ping-access-how-to.md)
##### [Fehler „Zugriff auf diese Unternehmensanwendung nicht möglich“](manage-apps/application-proxy-sign-in-bad-gateway-timeout-error.md)
##### [Problem beim Installieren des Anwendungsproxy-Agent-Connectors](manage-apps/application-proxy-connector-installation-problem.md)


#### Anwendungsregistrierung
##### [Eingeben von Feldern für das Anwendungsobjekt](application-dev-registration-config-specific-application-property-how-to.md)
##### [Ändern der Standardwerte für die Tokengültigkeitsdauer](application-dev-registration-config-change-token-lifetime-how-to.md)

#### Authentifizierung
##### [Konfigurieren von Endpunkten](application-dev-registration-config-how-to.md)

#### Bedingter Zugriff
##### [Der Kunde hat die Voraussetzungen der Geräteregistrierung nicht erfüllt.](active-directory-conditional-access.md)
##### [Der Mandant wird aufgrund einer falschen Einstellung der Richtlinien für bedingten Zugriff blockiert.](active-directory-conditional-access-device-remediation.md)
##### [Wie und wann treten Regeln außerhalb des Unternehmensnetzwerks in Kraft?](https://aka.ms/calocation)
##### [Wie erhöht man die Anzahl von Geräten, bei denen ein Benutzer sich in Azure AD registrieren darf?](active-directory-azureadjoin-setup.md)
##### [Wie wird der bedingte Zugriff für Exchange Online eingerichtet?](https://aka.ms/csforexchange)
##### [Wie wird der bedingte Zugriff für Windows 7-Geräte eingerichtet?](active-directory-conditional-access.md#device-based-conditional-access)
##### [Welche Anwendungen werden mit bedingtem Zugriff unterstützt?](active-directory-conditional-access-supported-apps.md)

#### Suchen einer API
##### [Suchen einer API](application-dev-api-find-an-api-how-to.md)

#### Verwalten des Zugriffs
##### [Zuweisen von Benutzern und Gruppen zu einer App](manage-apps/methods-for-assigning-users-and-groups.md)
##### [Entfernen des Benutzerzugriffs auf eine App](manage-apps/methods-for-removing-user-access.md)
##### [Konfigurieren der Self-Service-App-Zuweisung](manage-apps/manage-self-service-access.md)
##### [Unerwarteter Benutzer zugewiesen](manage-apps/ways-users-get-assigned-to-applications.md)
##### [Unerwartete App in der Anwendungsliste](manage-apps/application-types.md)

#### Mehrinstanzenfähige Apps
##### [Konfigurieren einer neuen App](application-dev-setup-multi-tenant-app.md)
##### [Hinzufügen zum App-Katalog](application-dev-registration-config-multi-tenant-application-add-to-gallery-how-to.md)

#### Berechtigungen
##### [Auswählen von Berechtigungen für eine API](application-dev-perms-for-given-api.md)
##### [Gewähren von Berechtigungen für meine App](application-dev-registration-config-grant-permissions-how-to.md)
##### [Delegierte Berechtigungen und Anwendungsberechtigungen](application-dev-delegated-and-app-perms.md)
##### [Anwendungszustimmung](application-dev-consent-framework.md)

#### Bereitstellung
##### [Dauer](manage-apps/application-provisioning-when-will-provisioning-finish-specific-user.md)
##### [Dauert Stunden: Katalog-App](manage-apps/application-provisioning-when-will-provisioning-finish.md)
##### [Konfigurieren der Benutzerbereitstellung: Katalog-App](manage-apps/application-provisioning-config-how-to.md)
##### [Problem beim Konfigurieren der Benutzerbereitstellung: Katalog-App](manage-apps/application-provisioning-config-problem.md)
##### [Problem beim Speichern von Administratoranmeldeinformationen während des Konfigurierens der Benutzerbereitstellung in einer Katalog-App](manage-apps/application-provisioning-config-problem-storage-limit.md)
##### [Benutzer werden nicht bereitgestellt: Katalog-App](manage-apps/application-provisioning-config-problem-no-users-provisioned.md)
##### [Falsche Benutzer bereitgestellt : Katalog-App](manage-apps/application-provisioning-config-problem-wrong-users-provisioned.md)

#### Einmaliges Anmelden
##### [Auswählen einer Methode](manage-apps/single-sign-on-modes.md)
##### [Konfigurieren](application-dev-registration-config-sso-how-to.md)
##### [Konfigurieren eines Verbunds: Katalog-Apps](manage-apps/configure-federated-single-sign-on-gallery-applications.md)
##### [Allgemeine Probleme beim Konfigurieren eines Verbunds: Katalog-Apps](manage-apps/configure-federated-single-sign-on-gallery-applications-problems.md)
##### [Konfigurieren eines Verbunds: Nicht-Katalog-Apps](manage-apps/configure-federated-single-sign-on-non-gallery-applications.md)
##### [Allgemeine Probleme beim Konfigurieren eines Verbunds: Nicht-Katalog-Apps](manage-apps/configure-federated-single-sign-on-non-gallery-applications-problems.md)
##### [Konfigurieren von Kennwörtern: Katalog-Apps](manage-apps/configure-password-single-sign-on-gallery-applications.md)
##### [Allgemeine Probleme beim Konfigurieren von Kennwörtern: Katalog-Apps](manage-apps/configure-password-single-sign-on-gallery-applications-problems.md)
##### [Konfigurieren von Kennwörtern: Nicht-Katalog-Apps](manage-apps/configure-password-single-sign-on-non-gallery-applications.md)
##### [Allgemeine Probleme beim Konfigurieren von Kennwörtern: Nicht-Katalog-Apps](manage-apps/configure-password-single-sign-on-non-gallery-applications-problems.md)

#### Probleme bei der Benutzeranmeldung
##### [Unerwartete Zustimmungsaufforderung](manage-apps/application-sign-in-unexpected-user-consent-prompt.md)
##### [Benutzerzustimmungsfehler](manage-apps/application-sign-in-unexpected-user-consent-error.md)
##### [Probleme beim Anmelden über das benutzerdefinierte Portal](manage-apps/application-sign-in-other-problem-deeplink.md)
##### [Probleme beim Anmelden über den Zugriffsbereich](manage-apps/application-sign-in-other-problem-access-panel.md)
##### [Fehler auf der Anwendungsanmeldeseite](manage-apps/application-sign-in-problem-application-error.md)
##### [Problem beim einmaligen Anmelden per Kennwort: Nicht-Katalog-App](manage-apps/application-sign-in-problem-password-sso-non-gallery.md)
##### [Problem beim einmaligen Anmelden per Kennwort: Katalog-App](manage-apps/application-sign-in-problem-password-sso-gallery.md)
##### [Problem bei der Anmeldung bei einer Microsoft-App](manage-apps/application-sign-in-problem-first-party-microsoft.md)
##### [Problem bei der einmaligen Verbundanmeldung: Nicht-Katalog-App](manage-apps/application-sign-in-problem-federated-sso-non-gallery.md)
##### [Problem bei der einmaligen Verbundanmeldung: Katalog-App](manage-apps/application-sign-in-problem-federated-sso-gallery.md)
##### [Problem mit einer benutzerdefinierten App](manage-apps/application-sign-in-problem-custom-dev.md)
##### [Problem mit einer lokalen App: Anwendungsproxy](manage-apps/application-sign-in-problem-on-premises-application-proxy.md)

### [Entwickeln von Apps](active-directory-applications-guiding-developers-for-lob-applications.md)
### [Dokumentbibliothek](active-directory-apps-index.md)

## Verwalten Ihres Verzeichnisses
### [Azure AD Connect](./connect/active-directory-aadconnect.md)
### Benutzerdefinierte Domänennamen
#### [Schnellstart](fundamentals/add-custom-domain.md)
### [Verwalten Ihres Verzeichnisses](fundamentals/active-directory-administer.md)
### [Enterprise State Roaming](active-directory-windows-enterprise-state-roaming-overview.md)
#### [Aktivieren](active-directory-windows-enterprise-state-roaming-enable.md)
#### [Gruppenrichtlinieneinstellungen](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
#### [Windows 10-Einstellungen](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
#### [Häufig gestellte Fragen](active-directory-windows-enterprise-state-roaming-faqs.md)
#### [Problembehandlung](active-directory-windows-enterprise-state-roaming-troubleshooting.md)


### [Integrieren lokaler Identitäten mit Azure AD Connect](./connect/active-directory-aadconnect.md)

### [Konfigurieren der Tokengültigkeitsdauer](active-directory-configurable-token-lifetimes.md)

## Zugriffsüberprüfungen
### [Übersicht über Zugriffsüberprüfungen](active-directory-azure-ad-controls-access-reviews-overview.md)
### [Abschließen einer Zugriffsüberprüfung](active-directory-azure-ad-controls-complete-access-review.md)
### [Erstellen einer Zugriffsüberprüfung](active-directory-azure-ad-controls-create-access-review.md)
### [Durchführen einer Zugriffsüberprüfung](active-directory-azure-ad-controls-perform-access-review.md)
### [Überprüfen des Zugriffs](active-directory-azure-ad-controls-how-to-review-your-access.md)
### [Gastzugriff mit Zugriffsüberprüfungen](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md)
### [Verwaltung des Benutzerzugriffs mit Überprüfungen](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md)
### [Verwalten von Programmen und Steuerelementen](active-directory-azure-ad-controls-manage-programs-controls.md)
### [Abrufen der Ergebnisse der Zugriffsüberprüfung](active-directory-azure-ad-controls-retrieve-access-review.md)

## [Nutzungsbedingungen](active-directory-tou.md)

## Schützen Ihrer Identitäten

### Azure AD Identity Protection
#### [Übersicht](identity-protection/overview.md)
#### [Aktivieren](identity-protection/enable.md)
#### [Erkennen von Sicherheitsrisiken](identity-protection/vulnerabilities.md)
#### [Risikoereignisse](active-directory-identity-protection-risk-events.md)
#### [Benachrichtigungen](identity-protection/notifications.md)
#### [Anmeldevorgang](identity-protection/flows.md)
#### [Simulieren von Risikoereignissen](identity-protection/playbook.md)
#### [Entsperren von Benutzern](identity-protection/howto-unblock-user.md)
#### [Häufig gestellte Fragen](identity-protection/faqs.md)
#### [Glossar](identity-protection/glossary.md)
#### [Microsoft Graph](identity-protection/graph-get-started.md)
### [Privileged Identity Management](privileged-identity-management/pim-configure.md?toc=%2fazure%2factive-directory%2ftoc.json)

## [Bereitstellen von AD FS in Azure](active-directory-aadconnect-azure-adfs.md)
### [Hochverfügbarkeit](active-directory-adfs-in-azure-with-azure-traffic-manager.md)
### [Ändern des Signaturhashalgorithmus](active-directory-federation-sha256-guidance.md)

## [Problembehandlung](fundamentals/active-directory-troubleshooting-support-howto.md)

## Bereitstellen eines Proof of Concept (PoC) für Azure AD
### [PoC-Playbook: Einführen](active-directory-playbook-intro.md)
### [PoC-Playbook: Bestandteile](active-directory-playbook-ingredients.md)
### [PoC-Playbook: Implementierung](active-directory-playbook-implementation.md)
### [PoC-Playbook: Bausteine](active-directory-playbook-building-blocks.md)

# Verweis
## [Codebeispiele](https://azure.microsoft.com/resources/samples/?service=active-directory)
## [Azure PowerShell-Cmdlets](/powershell/azure/overview)
## [Java-API-Referenz](/java/api)
## [.NET API](/active-directory/adal/microsoft.identitymodel.clients.activedirectory)

# Verwandte Themen
## [Multi-Factor Authentication](/azure/multi-factor-authentication/)
## [Azure AD Connect](./connect/active-directory-aadconnect.md)
## [Azure AD Connect Health](./connect-health/active-directory-aadconnect-health.md)
## [Azure AD für Entwickler](./develop/active-directory-how-to-integrate.md)
## [Azure AD Privileged Identity Management](./privileged-identity-management/pim-configure.md)

# angeben
## [Azure AD-Bereitstellungspläne](./fundamentals/active-directory-deployment-plans.md)
## [Azure-Feedbackforum](https://feedback.azure.com/forums/169401-azure-active-directory)
## [Azure-Roadmap](https://azure.microsoft.com/roadmap/?category=security-identity)
## [MSDN-Forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)
## [Preise](https://azure.microsoft.com/pricing/details/active-directory/)
## [Preisrechner](https://azure.microsoft.com/pricing/calculator/)
## [Dienstupdates](https://azure.microsoft.com/updates/?product=active-directory)
## [Stapelüberlauf](http://stackoverflow.com/questions/tagged/azure-active-directory)
## [Videos](https://azure.microsoft.com/documentation/videos/index/?services=active-directory)
