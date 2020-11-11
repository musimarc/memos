# OIDC 
## Description
OpenId Connect (OIDC pour les intimes) est un standard d'identification qui vient se positionner au dessus d'OAuth 2.0. 
Le premier d'entre eux est le principe de délégation de l'authentification des utilisateurs : avec OpenId Connect, cette responsabilité est confiée à un service indépendant et sort complètement du périmètre de l'application. Celle-ci utilise simplement le protocole qui lui permet de s'assurer que l'utilisateur est authentifié, mais quels moyens ont été utilisés pour l'identification ou bien la façon dont sont stockés les comptes ne sont pas de son ressort.
Pour aller plus loin, c'est également le service OIDC qui est en charge d'imposer ou de proposer les niveaux de sécurité, avec par exemple l'utilisation d'un mode deux facteurs, du SMS OTP, un formulaire de login de base, un capteur biométrique, etc.
Comme ce service est indépendant de l'application, il peut de facto être transverse et faciliter le principe d'identification unique dans un système d'information (SSO). En conjugant ce point avec le précédent, charge au service OIDC de procéder à l'intégration de protocoles plus complexes à mettre en oeuvre comme Kerberos.

Conçu sur la base des services web, OpenId Connect a sa place tant sur Internet que dans l'informatique d'entreprise.

## JWT
JSON Web Token, nom de code RFC 7519. Un JWT est un JSON accompagné d'une signature et de la référence à la clef qui permet de vérifier la signature. Le tout est encodé en Base64 et les 3 parties sont séparées par des points. Elles sont assemblées comme suit : la référence à la clef, le JSON et ensuite la signature.
JWT spécifie un ensemble de champs normalisés qu'un jeton doit contenir (domaine de sécurité, date d'émission, date d'expiration, etc.), mais c'est un standard extensible qui permet d'ajouter des champs personnalisés.
la signature permet aux applications de faire confiance au JWT car elle ne peut être forgée que grâce à la clef privée correspondante qui est détenue par le service OIDC et elle permet de s'assurer que le JSON n'a pas été modifié depuis la signature.
JWT permet ainsi d'allier scalabilité et sécurité dans la propagation de l'identité de service en service.

La première tâche qu'une application doit accomplir est la découverte de la configuration du service OIDC. Pour cela, le standard prévoit que le fournisseur doive exposer un service web qui donne l'ensemble de la configuration d'OpenId Connect, .well-known/openid-configuration
l'ensemble des clefs qui peuvent être utilisées pour valider la signature des jetons et cette information est portée par le champ jwks_uri.
L'application collecte également l'URL vers laquelle rediriger les utilisateurs non connectés, authorization_endpoint, et le service web qui délivre les jetons, token_endpoint.
L'accès à ces informations est public.

## Séquence authentification

1-L'application reçoit une requête sans token ou avec un token invalide (expiré), elle redirige l'utilisateur vers l'URL d'authentification du service OpenId Connect.
2-Le service OIDC procède à l'identification de l'utilisateur. Dans la plupart des cas, cela va se résumer à afficher une mire de login, cependant tous les rafinements en la matière sont envisageables : authentifications par deux facteurs (OTP, clef USB de sécurité), biométrie, Kerberos si l'utilisateur est dans un domaine AD, etc.
3-Si l'identification se déroule sans accroc, le service OIDC fournit un code et redirige l'utilisateur vers l'application.
4-L'application présente ce code au service OIDC pour obtenir un jeton au nom de l'utilisateur qui s'est connecté.
5-L'application fournit ce token au client qui peut maintenant le présenter dans les prochains appels.


## Keycloak
Kerberos
Edit this section
Report an issue

Keycloak supports login with a Kerberos ticket through the SPNEGO protocol. SPNEGO (Simple and Protected GSSAPI Negotiation Mechanism) is used to authenticate transparently through the web browser after the user has been authenticated when logging-in his session. For non-web cases or when ticket is not available during login, Keycloak also supports login with Kerberos username/password.

A typical use case for web authentication is the following:

    User logs into his desktop (Such as a Windows machine in Active Directory domain or Linux machine with Kerberos integration enabled).

    User then uses his browser (IE/Firefox/Chrome) to access a web application secured by Keycloak.

    Application redirects to Keycloak login.

    Keycloak renders HTML login screen together with status 401 and HTTP header WWW-Authenticate: Negotiate

    In case that the browser has Kerberos ticket from desktop login, it transfers the desktop sign on information to the Keycloak in header Authorization: Negotiate 'spnego-token' . Otherwise it just displays the login screen.

    Keycloak validates token from the browser and authenticates the user. It provisions user data from LDAP (in case of LDAPFederationProvider with Kerberos authentication support) or let user to update his profile and prefill data (in case of KerberosFederationProvider).

    Keycloak returns back to the application. Communication between Keycloak and application happens through OpenID Connect or SAML messages. The fact that Keycloak was authenticated through Kerberos is hidden from the application. So Keycloak acts as broker to Kerberos/SPNEGO login.

For setup there are 3 main parts:

    Setup and configuration of Kerberos server (KDC)

    Setup and configuration of Keycloak server

    Setup and configuration of client machines

Setup of Kerberos server

This is platform dependent. Exact steps depend on your OS and the Kerberos vendor you’re going to use. Consult Windows Active Directory, MIT Kerberos and your OS documentation for how exactly to setup and configure Kerberos server.

At least you will need to:

    Add some user principals to your Kerberos database. You can also integrate your Kerberos with LDAP, which means that user accounts will be provisioned from LDAP server.

    Add service principal for "HTTP" service. For example if your Keycloak server will be running on www.mydomain.org you may need to add principal HTTP/www.mydomain.org@MYDOMAIN.ORG assuming that MYDOMAIN.ORG will be your Kerberos realm.

    For example on MIT Kerberos you can run a "kadmin" session. If you are on the same machine where is MIT Kerberos, you can simply use the command:

sudo kadmin.local

Then add HTTP principal and export his key to a keytab file with the commands like:

addprinc -randkey HTTP/www.mydomain.org@MYDOMAIN.ORG
ktadd -k /tmp/http.keytab HTTP/www.mydomain.org@MYDOMAIN.ORG

The Keytab file /tmp/http.keytab will need to be accessible on the host where Keycloak server will be running


