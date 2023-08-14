# IBL WP Auth

WP Open ID client forked [from](https://github.com/daggerhart/openid-connect-generic).

### Dependencies

- [IBL edX Base OAuth SSO Backend](https://github.com/ibleducation/ibl-edx-base-oauth-sso-backend)
- [IBL WP Platform](https://github.com/ibleducation/ibl-wp-platform)

### Update

To make sure this plugin is upto date with the original version.

- Clone repo.

```
git clone git@github.com:ibleducation/ibl-wp-auth.git
```

- Add new remote to the original version.

```
git remote add original https://github.com/daggerhart/openid-connect-generic.git
```

- Fetch update from original repo.

```
git fetch original dev
git checkout dev
git pull original dev
```

- Rebase or merge changes to our main branch.

```
git checkout main
git rebase dev
git push origin main -f
```

### Create an Open id client in edX for WP.

- Head over to `https://<edx_lms_domain>/admin/oidc_provider/client/add/`.
- **Name**: `Wordpress`
- **Client Type**: `Confidential`
- **Response types**: `code (Authorization Code Flow)`
- **Redirect URIs**:

```
https://<wordpress_domain>/sso-proxy/
https://<wordpress_domain>/sso-proxy
https://<wordpress_domain>/openid-connect-authorize
```

- **Require Consent?**: `False` (Uncheck)
- **Scopes**: `email profile openid`
- **Post Logout Redirect URIs**:

```
https://<wordpress_domain>/sso/logout
https://<wordpress_domain>/sso/logout/
https://<wordpress_domain>/reestablish-session
https://<wordpress_domain>/reestablish-session/
```

- Click the `Save and continue editing` button. This should generate `Client ID` and `Client SECRET` under `Credentials`, copy them as we shall be using them to configure WP to use edX as the Identity Provider.

### Configue WP to use edX as the idp.

- Head over to `https://<wordpress_domain>/wp/wp-admin/options-general.php?page=openid-connect-generic-settings`.
- **Client ID**: `Client ID` generated above.
- **Client Secret Key**: `Client SECRET` generated above.
- **OpenID Scope**: `email profile openid`
- **Login Endpoint URL**: `https://learn.ibl.edducation/openid/authorize`
- **Userinfo Endpoint URL**: `https://learn.ibl.edducation/openid/userinfo`
- **Token Validation Endpoint URL**: `https://learn.ibl.edducation/openid/token`
- **End Session Endpoint URL**: `https://learn.ibl.edducation/openid/end-session`
- **Identity Key**: `nickname`
- **Nickname Key**: `nickname`
- **Enable Refresh Token**: `True` (Check)
- **Link Existing Users**: `True` (Check)
- **Create user if does not exist**: `True` (Check)
- **Redirect Back to Origin Page**: `True` (Check)
- **Redirect to the login screen when session is expired**: `True` (Check)
- **Alternate Redirect URI**: `True` (Check)
- **Enable Logging**: `True` (Check)
- Click `Save Changes` button.
