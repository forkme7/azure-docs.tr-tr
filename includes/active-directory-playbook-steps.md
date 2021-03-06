## <a name="playbook-steps"></a>Playbook adımları
1. [Giriş](../articles/active-directory/active-directory-playbook-intro.md)
2. [Malzemeleri](../articles/active-directory/active-directory-playbook-ingredients.md)
    * [Tema](../articles/active-directory/active-directory-playbook-ingredients.md)
    * [Ortamı](../articles/active-directory/active-directory-playbook-ingredients.md#theme)
    * [Hedef Kullanıcılar](../articles/active-directory/active-directory-playbook-ingredients.md#environment)
3. [Uygulama](../articles/active-directory/active-directory-playbook-implementation.md)
   * [Foundation - AD'den Azure AD'ye eşitleniyor](../articles/active-directory/active-directory-playbook-implementation.md#foundation---syncing-ad-to-azure-ad)
     * [Şirket içi kimliğinizi buluta genişletme](../articles/active-directory/active-directory-playbook-implementation.md#extending-your-on-premises-identity-to-the-cloud)  
     * [Grupları kullanarak Azure AD lisans atama](../articles/active-directory/active-directory-playbook-implementation.md#assigning-azure-ad-licenses-using-groups)
   * [Tema - çok sayıda uygulamalar, bir kimlik](../articles/active-directory/active-directory-playbook-implementation.md#theme---lots-of-apps-one-identity)
     * [SaaS uygulamaları - Federasyon SSO tümleştirme](../articles/active-directory/active-directory-playbook-implementation.md#integrate-saas-applications---federated-sso)
     * [SSO ve kimlik yaşam döngüsü olayları](../articles/active-directory/active-directory-playbook-implementation.md#sso-and-identity-lifecycle-events)
     * [SaaS uygulamaları - parola SSO tümleştirme](../articles/active-directory/active-directory-playbook-implementation.md#integrate-saas-applications---password-sso)
     * [Paylaşılan hesaplarına güvenli erişim](../articles/active-directory/active-directory-playbook-implementation.md#secure-access-to-shared-accounts)
     * [Şirket içi uygulamalara güvenli uzaktan erişim](../articles/active-directory/active-directory-playbook-implementation.md#secure-remote-access-to-on-premises-applications)
     * [LDAP kimlikleri Azure ad eşitleme](../articles/active-directory/active-directory-playbook-implementation.md#synchronize-ldap-identities-to-azure-ad)
   * [Tema - artış güvenlik](../articles/active-directory/active-directory-playbook-implementation.md#theme---increase-your-security)
     * [Güvenli yönetici hesabına erişimi](../articles/active-directory/active-directory-playbook-implementation.md#secure-administrator-account-access)
     * [Uygulamalara güvenli erişim](../articles/active-directory/active-directory-playbook-implementation.md#secure-access-to-applications)
     * [Zamanında (JIT) yönetiminde sadece etkinleştir](../articles/active-directory/active-directory-playbook-implementation.md#enable-just-in-time-jit-administration)
     * [Risk üzerinde temel kimlikleri koru](../articles/active-directory/active-directory-playbook-implementation.md#protect-identities-based-on-risk)
     * [Sertifika tabanlı kimlik doğrulaması kullanarak parolaları kimlik doğrulaması](../articles/active-directory/active-directory-playbook-implementation.md#authenticate-without-passwords-using-certificate-based-authentication)
   * [Tema - Self Servis ölçekli](../articles/active-directory/active-directory-playbook-implementation.md#theme---scale-with-self-service)
     * [Self Servis parola sıfırlama](../articles/active-directory/active-directory-playbook-implementation.md#self-service-password-reset)
     * [Self Servis uygulamalara erişim](../articles/active-directory/active-directory-playbook-implementation.md#self-service-access-to-applications)
4. [Yapı taşları](../articles/active-directory/active-directory-playbook-building-blocks.md)
   * [Aktör Kataloğu](../articles/active-directory/active-directory-playbook-building-blocks.md)
   * [Tüm yapı taşlarını ortak önkoşulları](../articles/active-directory/active-directory-playbook-building-blocks.md#common-prerequisites-for-all-building-blocks)
   * [Dizin eşitleme - parola karması eşitlemesi (PHS) - yeni yükleme](../articles/active-directory/active-directory-playbook-building-blocks.md#directory-synchronization---password-hash-sync-phs---new-installation)
   * [Markalama](../articles/active-directory/active-directory-playbook-building-blocks.md#branding)
   * [Lisans grup tabanlı](../articles/active-directory/active-directory-playbook-building-blocks.md#group-based-licensing)
   * [SaaS Federasyon SSO yapılandırma](../articles/active-directory/active-directory-playbook-building-blocks.md#saas-federated-sso-configuration)
   * [SaaS parola SSO yapılandırma](../articles/active-directory/active-directory-playbook-building-blocks.md#saas-password-sso-configuration)
   * [SaaS paylaşılan hesaplarını yapılandırma](../articles/active-directory/active-directory-playbook-building-blocks.md#saas-shared-accounts-configuration)
   * [Uygulama Proxy Yapılandırması](../articles/active-directory/active-directory-playbook-building-blocks.md#app-proxy-configuration)
   * [Genel LDAP Bağlayıcısı yapılandırması](../articles/active-directory/active-directory-playbook-building-blocks.md#generic-ldap-connector-configuration)
   * [Grupları - temsilci sahipliği](../articles/active-directory/active-directory-playbook-building-blocks.md#groups---delegated-ownership)
   * [SaaS ve kimlik yaşam döngüsü](../articles/active-directory/active-directory-playbook-building-blocks.md#saas-and-identity-lifecycle)
   * [Self Servis uygulama yönetimine erişimi](../articles/active-directory/active-directory-playbook-building-blocks.md#self-service-access-to-application-management)
   * [Self Servis parola sıfırlama](../articles/active-directory/active-directory-playbook-building-blocks.md#self-service-password-reset)
   * [Azure multi-Factor Authentication telefon çağrısı ile](../articles/active-directory/active-directory-playbook-building-blocks.md#azure-multi-factor-authentication-with-phone-calls)
   * [SaaS uygulamaları için MFA koşullu erişim](../articles/active-directory/active-directory-playbook-building-blocks.md#mfa-conditional-access-for-saas-applications)
   * [Privileged Identity Management (PIM)](../articles/active-directory/active-directory-playbook-building-blocks.md#privileged-identity-management-pim)
   * [Risk olaylarını keşfetme](../articles/active-directory/active-directory-playbook-building-blocks.md#discovering-risk-events)
   * [Oturum açma riski ilkelerini dağıtma](../articles/active-directory/active-directory-playbook-building-blocks.md#deploying-sign-in-risk-policies)
   * [Sertifika tabanlı kimlik doğrulamasını yapılandırma](../articles/active-directory/active-directory-playbook-building-blocks.md#configuring-certificate-based-authentication)
