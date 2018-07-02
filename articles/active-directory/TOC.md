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
### [Freigeben von Konten](active-directory-sharing-accounts.md)
### [Zuweisen von Benutzern zu Administratorrollen](fundamentals/active-directory-users-assign-role-azure-portal.md)
### [Wiederherstellen eines gelöschten Benutzers](fundamentals/active-directory-users-restore.md)
### [Hinzufügen von Gastbenutzern aus einem anderen Verzeichnis (B2B)](b2b/what-is-b2b.md)
#### [Administratoren, die B2B-Benutzer hinzufügen](b2b/add-users-administrator.md)
#### [Information-Worker, die B2B-Benutzer hinzufügen](b2b/add-users-information-worker.md)
#### [API und Anpassung](b2b/customize-invitation-api.md)
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
### Verwalten von Gruppen
#### [Azure-Portal](fundamentals/active-directory-groups-create-azure-portal.md)
#### [Azure AD PowerShell für Graph (v2)](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
#### [Azure AD PowerShell MSOnline](active-directory-accessmanagement-groups-settings-cmdlets.md)
### [Verwalten von Gruppenmitgliedern](fundamentals/active-directory-groups-members-azure-portal.md)
### [Verwalten von Gruppenbesitzern](fundamentals/active-directory-accessmanagement-managing-group-owners.md)
### [Verwalten der Gruppenmitgliedschaft](fundamentals/active-directory-groups-membership-azure-portal.md)
### [Zuweisen von Lizenzen mithilfe von Gruppen](fundamentals/active-directory-licensing-whatis-azure-portal.md)
#### [Zuweisen von Lizenzen zu einer Gruppe](active-directory-licensing-group-assignment-azure-portal.md)
#### [Bestimmen und Beheben von Lizenzproblemen in einer Gruppe](active-directory-licensing-group-problem-resolution-azure-portal.md)
#### [Migrieren einzelner lizenzierter Benutzer zur gruppenbasierten Lizenzierung](active-directory-licensing-group-migration-azure-portal.md)
#### [Migrieren von Benutzern zwischen Produktlizenzen](active-directory-licensing-group-product-migration.md)
#### [Zusätzliche Szenarien für die Gruppenbasierte Lizenzierung](active-directory-licensing-group-advanced.md)
#### [Azure PowerShell-Beispiele für die gruppenbasierte Lizenzierung](active-directory-licensing-ps-examples.md)
#### [Referenz zu Produkten und Serviceplänen in Azure AD](active-directory-licensing-product-and-service-plan-reference.md)
### [Einrichten des Office 365-Gruppenablaufs](active-directory-groups-lifecycle-azure-portal.md)
### [Erzwingen einer Benennungsrichtlinie für Gruppen](groups-naming-policy.md)
### [Anzeigen aller Gruppen](fundamentals/active-directory-groups-view-azure-portal.md)
### [Hinzufügen des Gruppenzugriffs auf SaaS-Apps](active-directory-accessmanagement-group-saasapps.md)
### [Wiederherstellen einer gelöschten Office 365-Gruppe](fundamentals/active-directory-groups-restore-azure-portal.md)
### [Verwalten von Gruppeneinstellungen](fundamentals/active-directory-groups-settings-azure-portal.md) 
### Erstellen erweiterter Regeln
#### [Azure-Portal](active-directory-groups-dynamic-membership-azure-portal.md)
### [Einrichten von Self-Service-Gruppen](active-directory-accessmanagement-self-service-group-management.md)
### [Problembehandlung](active-directory-accessmanagement-troubleshooting.md)

## [Verwalten von Berichten](active-directory-reporting-azure-portal.md)
### [Anmeldungsaktivität](active-directory-reporting-activity-sign-ins.md)
### [Überwachungsaktivität](active-directory-reporting-activity-audit-logs.md)
### [Gefährdete Benutzer](active-directory-reporting-security-user-at-risk.md)
### [Riskante Anmeldungen](active-directory-reporting-security-risky-sign-ins.md)
### [Risikoereignisse](active-directory-reporting-risk-events.md)
### [HÄUFIG GESTELLTE FRAGEN](active-directory-reporting-faq.md)
### Aufgaben
#### [Konfigurieren benannter Orte](active-directory-named-locations.md)
#### [Suchen nach Aktivitätsberichten](active-directory-reporting-migration.md)
#### [Verwenden des Azure Active Directory-Power BI-Inhaltspakets](active-directory-reporting-power-bi-content-pack-how-to.md)
#### [Bereinigen von Benutzern mit Risikomarkierung](active-directory-report-security-user-at-risk-remediation.md)
### Verweis
#### [Vermerkdauer](active-directory-reporting-retention.md)
#### [Wartezeiten](active-directory-reporting-latencies-azure-portal.md)
#### [Referenz zu Überwachungsaktivitäten](active-directory-reporting-activity-audit-reference.md)
#### [Fehlercodes für Anmeldeaktivitäten](active-directory-reporting-activity-sign-ins-errors.md)
#### [Multi-Factor Authentication](active-directory-reporting-activity-sign-ins-mfa.md)



### Problembehandlung
#### [Fehlende Überwachungsdaten](active-directory-reporting-troubleshoot-missing-audit-data.md)
#### [Fehlende Daten in den Downloads](active-directory-reporting-troubleshoot-missing-data-download.md)
#### [Azure Active Directory-Aktivitätsprotokolle: Fehler in Inhaltspaketen](active-directory-reporting-troubleshoot-content-pack.md)
### [Programmgesteuerter Zugriff](active-directory-reporting-api-getting-started-azure-portal.md)
#### [Voraussetzungen](active-directory-reporting-api-prerequisites-azure-portal.md)
#### [Überwachungsbeispiele](active-directory-reporting-api-audit-samples.md)
#### [Anmeldebeispiele](active-directory-reporting-api-sign-in-activity-samples.md)
#### [Verwenden von Zertifikaten](active-directory-reporting-api-with-certificates.md)

## Verwalten von Kennwörtern
### [Übersicht über Kennwörter](authentication/active-directory-passwords-overview.md)
### Benutzerdokumente
#### [Zurücksetzen oder Ändern des Kennworts](active-directory-passwords-update-your-own-password.md)
#### [Bewährte Methoden für Kennwörter](active-directory-secure-passwords.md)
#### [Registrieren für die Self-Service-Kennwortzurücksetzung](active-directory-passwords-reset-register.md)
### [SSPR – So funktioniert‘s](authentication/concept-sspr-howitworks.md)
### [SSPR-Bereitstellungshandbuch](authentication/howto-sspr-deployment.md)
### [SSPR and Windows 10 (SSPR und Windows 10)](authentication/tutorial-sspr-windows.md)
### [SSPR-Richtlinien ](authentication/concept-sspr-policy.md)
### [SSPR-Anpassung](authentication/concept-sspr-customization.md)
### [SSPR-Datenanforderungen](authentication/howto-sspr-authenticationdata.md)
### [SSPR-Berichterstellung](authentication/howto-sspr-reporting.md)
### [Smart Lockout](authentication/howto-password-smart-lockout.md)
### [Entfernen unsicherer Kennwörter](authentication/concept-password-ban-bad.md)
### [Konfigurieren der Liste gesperrter Kennwörter](authentication/howto-password-ban-bad.md)
### [Lokale Integration](authentication/concept-password-ban-bad-on-premises.md)
### [Bereitstellen des Kennwortschutzes für Azure AD](authentication/howto-password-ban-bad-on-premises.md)
### [Konfigurieren des Kennwortschutzes für Azure AD](authentication/howto-password-ban-bad-on-premises-operations.md)
### [Überwachen des Kennwortschutzes für Azure AD](authentication/howto-password-ban-bad-on-premises-troubleshoot.md)
### IT-Administratoren: Zurücksetzen von Kennwörtern
#### [Azure-Portal](fundamentals/active-directory-users-reset-password-azure-portal.md)
### [Lizenzieren von SSPR](authentication/concept-sspr-licensing.md)
### [Kennwortrückschreiben](authentication/howto-sspr-writeback.md)
### [Problembehandlung](authentication/active-directory-passwords-troubleshoot.md)
### [HÄUFIG GESTELLTE FRAGEN](authentication/active-directory-passwords-faq.md)


## Verwalten von Geräten
### [Einführung](device-management-introduction.md)
### [Verwenden des Azure-Portals](device-management-azure-portal.md)
### [Planen der Azure AD-Einbindung](active-directory-azureadjoin-deployment-aadjoindirect.md)
### [Häufig gestellte Fragen](device-management-faq.md)
### Aufgaben
#### [Einrichten von bei Azure AD registrierten Windows 10-Geräten](device-management-azuread-registered-devices-windows10-setup.md)
#### [Einrichten von in Azure AD eingebundenen Geräten](device-management-azuread-joined-devices-setup.md)
#### [Einrichten von in Azure AD eingebundenen Hybridgeräten](device-management-hybrid-azuread-joined-devices-setup.md)
#### [Lokale Bereitstellung](active-directory-device-registration-on-premises-setup.md)
#### [Azure AD-Einbindung während des Eindrucks beim ersten Ausführen von Windows 10](device-management-azuread-joined-devices-frx.md)
### Problembehandlung
#### [In Azure AD eingebundene Windows 10- und Windows Server 2016-Hybridgeräte](device-management-troubleshoot-hybrid-join-windows-current.md)
#### [In Azure AD eingebundene ältere Windows-Hybridgeräte](device-management-troubleshoot-hybrid-join-windows-legacy.md)

## Verwalten von Apps
### [Übersicht](manage-apps/what-is-application-management.md)
### [Erste Schritte](manage-apps/plan-an-application-integration.md)
### [Tutorials für die Integration von SaaS-Apps](saas-apps/tutorial-list.md)

### [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von SaaS-Apps](active-directory-saas-app-provisioning.md) 
#### [Tutorials für die App-Integration](saas-apps/tutorial-list.md) 
#### [Automatisieren der Bereitstellung in SCIM-fähigen Apps](manage-apps/use-scim-to-provision-users-and-groups.md) 
#### [Anpassen von Attributzuordnungen](active-directory-saas-customizing-attribute-mappings.md) 
#### [Schreiben von Ausdrücken für Attributzuordnungen in Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md) 
#### [Verwenden von Bereichsfiltern](active-directory-saas-scoping-filters.md) 
#### [Berichterstellung zur automatischen Benutzerbereitstellung](active-directory-saas-provisioning-reporting.md) 
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
##### [Platzhalter](active-directory-application-proxy-wildcard.md)
##### [Entfernen von personenbezogenen Daten](manage-apps/application-proxy-remove-personal-data.md)


#### Veröffentlichen von exemplarische Vorgehensweisen
##### [Remotedesktop](manage-apps/application-proxy-integrate-with-remote-desktop-services.md)
##### [SharePoint](manage-apps/application-proxy-integrate-with-sharepoint-server.md)
##### [Microsoft Teams](manage-apps/application-proxy-integrate-with-teams.md)
##### [Tableau](manage-apps/application-proxy-integrate-with-tableau.md)
##### [Qlik](active-directory-application-proxy-qlik.md)
#### [PowerShell](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#application_proxy_application_management) 

#### [Problembehandlung](manage-apps/application-proxy-troubleshoot.md)

### Verwalten von Unternehmens-Apps
#### [Zuweisen von Benutzern](manage-apps/assign-user-or-group-access-portal.md)
#### [Anpassen des Brandings](manage-apps/change-name-or-logo-portal.md)
#### [Deaktivieren von Benutzeranmeldungen](manage-apps/disable-user-sign-in-portal.md)
#### [Entfernen von Benutzern](manage-apps/remove-user-or-group-access-portal.md)
#### [Anzeigen aller meiner Apps](manage-apps/view-applications-portal.md)
#### [Verwalten der Bereitstellung von Benutzerkonten](manage-apps/configure-automatic-user-provisioning-portal.md)
#### [Verwalten des einmaligen Anmeldens für Unternehmens-Apps](manage-apps/configure-single-sign-on-portal.md)
#### [Erweiterte Zertifikatsignatur für SAML-Apps](manage-apps/certificate-signing-options.md)
#### [Ausblenden einer Anwendung auf der Benutzeroberfläche](manage-apps/hide-application-from-user-portal.md)
### [Configure Sign-In Auto-Acceleration using HRD Policy (Konfigurieren der automatischen Anmeldebeschleunigung per HRD Policy)](manage-apps/configure-authentication-for-federated-users-portal.md)
### [Migrieren von AD FS-Apps zu Azure AD](manage-apps/migrate-adfs-apps-to-azure.md) 
### [Verwalten des Zugriffs auf Apps](manage-apps/what-is-access-management.md)
#### [SSO-Zugriff](manage-apps/what-is-single-sign-on.md)
#### [Zertifikate für SSO](manage-apps/manage-certificates-for-federated-single-sign-on.md)
#### [Mandanteneinschränkungen](manage-apps/tenant-restrictions.md)
#### [Verwenden von SCIM zum Bereitstellen von Benutzern](manage-apps/use-scim-to-provision-users-and-groups.md)


### Problembehandlung



#### Anpassung des Zugriffsbereichs
##### [App wird nicht angezeigt](application-access-panel-unexpected-application-not-appearing.md)
##### [App wird unerwartet angezeigt](application-access-panel-unexpected-application-appears.md)
##### [Anmeldung nicht möglich](application-access-panel-web-sign-in-problem.md)
##### [Fehler beim Installieren der Browsererweiterung](application-access-panel-extension-problem-installing.md)
##### [Verwenden des Self-Service-App-Zugriffs](application-access-panel-self-service-applications-how-to.md)
##### [Fehler beim Verwenden des Self-Service-App-Zugriffs](application-access-panel-self-service-applications-problem.md)

#### Hinzufügen einer App
##### [Auswählen des App-Typs](application-config-add-app-problem-how-to-choose-application-type.md)
##### [Allgemeine Probleme: Katalog-Apps](application-config-add-app-problem-problem-adding-gallery-app.md)
##### [Allgemeine Probleme: Nicht-Katalog-Apps](application-config-add-app-problem-problem-adding-non-gallery-app.md)

#### Anwendungsproxy
##### [Problem beim Anzeigen der App-Seite](application-proxy-page-appearance-broken-problem.md)
##### [Laden der Anwendung dauert zu lange.](application-proxy-page-load-speed-problem.md)
##### [Links auf der Anwendungsseite funktionieren nicht.](application-proxy-page-links-broken-problem.md)
##### [Für die App zu öffnende Ports](application-proxy-connectivity-ports-how-to.md)
##### [Kein funktionierender Connector in einer Connectorguppe für meine App](application-proxy-connectivity-no-working-connector.md)
##### [Konfigurieren im Verwaltungsportal](application-proxy-config-how-to.md)
##### [Konfigurieren des einmaligen Anmeldens für meine App](application-proxy-config-sso-how-to.md)
##### [Problem beim Erstellen einer App im Verwaltungsportal](application-proxy-config-problem.md)
##### [Konfigurieren der eingeschränkten Kerberos-Delegierung](application-proxy-back-end-kerberos-constrained-delegation-how-to.md)
##### [Konfigurieren mit PingAccess](application-proxy-back-end-ping-access-how-to.md)
##### [Fehler „Zugriff auf diese Unternehmensanwendung nicht möglich“](application-proxy-sign-in-bad-gateway-timeout-error.md)
##### [Problem beim Installieren des Anwendungsproxy-Agent-Connectors](application-proxy-connector-installation-problem.md)


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
##### [Zuweisen von Benutzern und Gruppen zu einer App](application-access-assignment-how-to-add-assignment.md)
##### [Entfernen des Benutzerzugriffs auf eine App](application-access-assignment-how-to-remove-assignment.md)
##### [Konfigurieren der Self-Service-App-Zuweisung](application-access-self-service-how-to.md)
##### [Unerwarteter Benutzer zugewiesen](application-access-unexpected-user-assignment.md)
##### [Unerwartete App in der Anwendungsliste](application-access-unexpected-application.md)

#### Mehrinstanzenfähige Apps
##### [Konfigurieren einer neuen App](application-dev-setup-multi-tenant-app.md)
##### [Hinzufügen zum App-Katalog](application-dev-registration-config-multi-tenant-application-add-to-gallery-how-to.md)

#### Berechtigungen
##### [Auswählen von Berechtigungen für eine API](application-dev-perms-for-given-api.md)
##### [Gewähren von Berechtigungen für meine App](application-dev-registration-config-grant-permissions-how-to.md)
##### [Delegierte Berechtigungen und Anwendungsberechtigungen](application-dev-delegated-and-app-perms.md)
##### [Anwendungszustimmung](application-dev-consent-framework.md)

#### Bereitstellung
##### [Dauer](application-provisioning-when-will-provisioning-finish-specific-user.md)
##### [Dauert Stunden: Katalog-App](application-provisioning-when-will-provisioning-finish.md)
##### [Konfigurieren der Benutzerbereitstellung: Katalog-App](application-provisioning-config-how-to.md)
##### [Problem beim Konfigurieren der Benutzerbereitstellung: Katalog-App](application-provisioning-config-problem.md)
##### [Problem beim Speichern von Administratoranmeldeinformationen während des Konfigurierens der Benutzerbereitstellung in einer Katalog-App](application-provisioning-config-problem-storage-limit.md)
##### [Benutzer werden nicht bereitgestellt: Katalog-App](application-provisioning-config-problem-no-users-provisioned.md)
##### [Falsche Benutzer bereitgestellt : Katalog-App](application-provisioning-config-problem-wrong-users-provisioned.md)

#### Einmaliges Anmelden
##### [Auswählen einer Methode](application-config-sso-how-to-choose-sign-on-method.md)
##### [Konfigurieren](application-dev-registration-config-sso-how-to.md)
##### [Konfigurieren eines Verbunds: Katalog-Apps](application-config-sso-how-to-configure-federated-sso-gallery.md)
##### [Allgemeine Probleme beim Konfigurieren eines Verbunds: Katalog-Apps](application-config-sso-problem-configure-federated-sso-gallery.md)
##### [Konfigurieren eines Verbunds: Nicht-Katalog-Apps](application-config-sso-how-to-configure-federated-sso-non-gallery.md)
##### [Allgemeine Probleme beim Konfigurieren eines Verbunds: Nicht-Katalog-Apps](application-config-sso-problem-configure-federated-sso-non-gallery.md)
##### [Konfigurieren von Kennwörtern: Katalog-Apps](application-config-sso-how-to-configure-password-sso-gallery.md)
##### [Allgemeine Probleme beim Konfigurieren von Kennwörtern: Katalog-Apps](application-config-sso-problem-configure-password-sso-gallery.md)
##### [Konfigurieren von Kennwörtern: Nicht-Katalog-Apps](application-config-sso-how-to-configure-password-sso-non-gallery.md)
##### [Allgemeine Probleme beim Konfigurieren von Kennwörtern: Nicht-Katalog-Apps](application-config-sso-problem-configure-password-sso-non-gallery.md)

#### Probleme bei der Benutzeranmeldung
##### [Unerwartete Zustimmungsaufforderung](application-sign-in-unexpected-user-consent-prompt.md)
##### [Benutzerzustimmungsfehler](application-sign-in-unexpected-user-consent-error.md)
##### [Probleme beim Anmelden über das benutzerdefinierte Portal](application-sign-in-other-problem-deeplink.md)
##### [Probleme beim Anmelden über den Zugriffsbereich](application-sign-in-other-problem-access-panel.md)
##### [Fehler auf der Anwendungsanmeldeseite](application-sign-in-problem-application-error.md)
##### [Problem beim einmaligen Anmelden per Kennwort: Nicht-Katalog-App](application-sign-in-problem-password-sso-non-gallery.md)
##### [Problem beim einmaligen Anmelden per Kennwort: Katalog-App](application-sign-in-problem-password-sso-gallery.md)
##### [Problem bei der Anmeldung bei einer Microsoft-App](application-sign-in-problem-first-party-microsoft.md)
##### [Problem bei der einmaligen Verbundanmeldung: Nicht-Katalog-App](application-sign-in-problem-federated-sso-non-gallery.md)
##### [Problem bei der einmaligen Verbundanmeldung: Katalog-App](application-sign-in-problem-federated-sso-gallery.md)
##### [Problem mit einer benutzerdefinierten App](application-sign-in-problem-custom-dev.md)
##### [Problem mit einer lokalen App: Anwendungsproxy](application-sign-in-problem-on-premises-application-proxy.md)

### [Entwickeln von Apps](active-directory-applications-guiding-developers-for-lob-applications.md)
### [Dokumentbibliothek](active-directory-apps-index.md)

## Verwalten Ihres Verzeichnisses
### [Azure AD Connect](./connect/active-directory-aadconnect.md)
### Benutzerdefinierte Domänennamen
#### [Schnellstart](fundamentals/add-custom-domain.md)
#### [Hinzufügen benutzerdefinierter Domänennamen](active-directory-domains-manage-azure-portal.md)
### [Verwalten Ihres Verzeichnisses](fundamentals/active-directory-administer.md)
### [Löschen eines Verzeichnisses](directory-delete-howto.md)
### [Mehrere Verzeichnisse](active-directory-licensing-directory-independence.md)
### [Self-Service-Registrierung](active-directory-self-service-signup.md)
### [Übernehmen eines nicht verwalteten Verzeichnisses](domains-admin-takeover.md)
### [Enterprise State Roaming](active-directory-windows-enterprise-state-roaming-overview.md)
#### [Aktivieren](active-directory-windows-enterprise-state-roaming-enable.md)
#### [Gruppenrichtlinieneinstellungen](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
#### [Windows 10-Einstellungen](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
#### [Häufig gestellte Fragen](active-directory-windows-enterprise-state-roaming-faqs.md)
#### [Problembehandlung](active-directory-windows-enterprise-state-roaming-troubleshooting.md)


### [Integrieren lokaler Identitäten mit Azure AD Connect](./connect/active-directory-aadconnect.md)

## Delegieren des Zugriffs auf Ressourcen
### [Administratorrollen](active-directory-assign-admin-roles-azure-portal.md)
#### [Zuweisen der Administratorrolle zu einem Benutzer](fundamentals/active-directory-users-assign-role-azure-portal.md)
#### [Vergleichen der Berechtigungen für Mitglieder und Gastbenutzer](fundamentals/users-default-permissions.md)
### [Härten der Administratorrollensicherheit](admin-roles-best-practices.md)  
#### [Erstellen von Administratorkonten mit Notfallzugriff](active-directory-admin-manage-emergency-access-accounts.md)
### [Verwaltungseinheiten](active-directory-administrative-units-management.md)
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

## Schützen Ihrer Identitäten
### [Bedingter Zugriff](active-directory-conditional-access-azure-portal.md)
#### [Erste Schritte](active-directory-conditional-access-azure-portal-get-started.md)
#### Schnellstarts
##### [Konfigurieren der Cloud-App-spezifischen MFA](active-directory-conditional-access-app-based-mfa.md)
##### [Festlegen, dass die Nutzungsbedingungen akzeptiert werden müssen](active-directory-conditional-access-tou.md)
#### Tutorials
##### [Migrieren der klassischen MFA-Richtlinie](active-directory-conditional-access-migration-mfa.md)
#### Konzepte
##### [Baselineschutz](active-directory-conditional-access-baseline-protection.md)
##### [Bedingungen](active-directory-conditional-access-conditions.md)
##### [Standortbedingungen](active-directory-conditional-access-locations.md)
##### [Kontrollen](active-directory-conditional-access-controls.md)
##### [Was-wäre-wenn-Tool](active-directory-conditional-access-whatif.md)
##### [Grundlegendes zu Geräterichtlinien für Office 365-Dienste](active-directory-conditional-access-device-policies.md)
#### Anleitungen
##### [bewährten Methoden](active-directory-conditional-access-best-practices.md)
##### [Konfigurieren von Richtlinien für bedingten Zugriff für Zugriffsversuche von nicht vertrauenswürdigen Netzwerken](active-directory-conditional-access-untrusted-networks.md)
##### [Einrichten des gerätebasierten bedingten Zugriffs](active-directory-conditional-access-policy-connected-applications.md)
##### [Einrichten des App-basierten bedingten Zugriffs](active-directory-conditional-access-mam.md)
##### [Angeben von Nutzungsbedingungen für Benutzer und Apps](active-directory-tou.md)
##### [Migrieren von klassischen Richtlinien](active-directory-conditional-access-migration.md)
##### [Einrichten der VPN-Konnektivität](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy)
##### [Einrichten von SharePoint und Exchange Online](active-directory-conditional-access-no-modern-authentication.md)
##### [Problembehandlung](active-directory-conditional-access-device-remediation.md)
#### [Technische Referenz](active-directory-conditional-access-technical-reference.md)
#### [Häufig gestellte Fragen](active-directory-conditional-faqs.md)

### Zertifikatbasierte Authentifizierung
#### [Android](active-directory-certificate-based-authentication-android.md)
#### [iOS](active-directory-certificate-based-authentication-ios.md)
#### [Azure AD Identity Protection](active-directory-certificate-based-authentication-get-started.md)

### [Azure AD Identity Protection](active-directory-identityprotection.md)
#### [Aktivieren](active-directory-identityprotection-enable.md)
#### [Erkennen von Sicherheitsrisiken](active-directory-identityprotection-vulnerabilities.md)
#### [Risikoereignisse](active-directory-identity-protection-risk-events.md)
#### [Benachrichtigungen](active-directory-identityprotection-notifications.md)
#### [Anmeldevorgang](active-directory-identityprotection-flows.md)
#### [Simulieren von Risikoereignissen](active-directory-identityprotection-playbook.md)
#### [Entsperren von Benutzern](active-directory-identityprotection-unblock-howto.md)
#### [Häufig gestellte Fragen](active-directory-identity-protection-faqs.md)
#### [Glossar](active-directory-identityprotection-glossary.md)
#### [Microsoft Graph](active-directory-identityprotection-graph-getting-started.md)
### [Privileged Identity Management](active-directory-privileged-identity-management-configure.md)

## Integrieren anderer Dienste in Azure AD 
### [Integrieren von LinkedIn in Azure AD](linkedin-integration.md)

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
## [Dienst- und andere Einschränkungen](active-directory-service-limits-restrictions.md)

# Verwandte Themen
## [Multi-Factor Authentication](/azure/multi-factor-authentication/)
## [Azure AD Connect](./connect/active-directory-aadconnect.md)
## [Azure AD Connect Health](./connect-health/active-directory-aadconnect-health.md)
## [Azure AD für Entwickler](./develop/active-directory-how-to-integrate.md)
## [Azure AD Privileged Identity Management](./privileged-identity-management/active-directory-securing-privileged-access.md)

# angeben
## [Azure-Feedbackforum](https://feedback.azure.com/forums/169401-azure-active-directory)
## [Azure-Roadmap](https://azure.microsoft.com/roadmap/?category=security-identity)
## [MSDN-Forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)
## [Preise](https://azure.microsoft.com/pricing/details/active-directory/)
## [Preisrechner](https://azure.microsoft.com/pricing/calculator/)
## [Dienstupdates](https://azure.microsoft.com/updates/?product=active-directory)
## [Stapelüberlauf](http://stackoverflow.com/questions/tagged/azure-active-directory)
## [Videos](https://azure.microsoft.com/documentation/videos/index/?services=active-directory)
