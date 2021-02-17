### Polaris Forge Ref App Readme

##### Link patterns that are resolvable by this ref app:
- Any issue link from Jira site to which user has access. For example: `https://<YOUR_SITE_NAME>.atlassian.net/browse/ISSUE-KEY-1`
- Any slack message link from workspace where Slack app was installed. For example `https://<YOUR_WORKSPACE>.slack.com/archives/C01736E6Z37/p1613475464011300` 

## How to start

1. Install Forge CLI using this guide: [https://developer.atlassian.com/platform/forge/getting-started](https://developer.atlassian.com/platform/forge/getting-started)
 
2. Install dependencies for project

```bash
npm i 
```

3. Create `.env` file from `.env.default` and define missing variables (ignore `FORGE_APP_ID` for now).

4. Generate manifest from template with command:

```bash
npm run manifest
```

5. Once Forge CLI is installed you need to create new app using command (confirm app details rewrite on promt):

```bash
forge register
```

6. Copy generated `FORGE_APP_ID` from `app` section in `manifest.yml` back to `.env` 

7. Deploy app to development env (`--no-verify ` is required)


```bash
 forge deploy -e development --no-verify 
```

8. Install app to some cloud 

```bash
 forge install -p Jira -s <YOUR_SITE_NAME>.atlassian.net
```
9. Go to Polaris Data Tab and test your URL's

10. (Optional) If you want to test a link using OAuth2 authentication, you need to folow those steps:

First, create a Slack app [Create App](https://api.slack.com/apps?new_app=1)
    - Open the "OAuth & Permissions" tab
    - Set `Redirect URL` to `https://id.atlassian.com/outboundAuth/finish`.
    - In the `Scopes / User Token Scopes` section add the OAuth scopes listed in `manifest.yml`.

Then run the following command to set up the OAuth2 client for your app in Forge:

```bash
npm run externalAuth:set -- --email $EMAIL --api-token $ATLASSIAN_API_TOKEN --forge-app-id $FORGE_APP_ID --forge-env $FORGE_ENV --service-key $EXTERNAL_AUTH_SERVICE_KEY --client-id $SLACK_CLIENT_ID --client-secret $SLACK_CLIENT_SECRET
```
With the following values:

    - `$EMAIL` is your @atlassian.com email address
    - `$ATLASSIAN_API_TOKEN`: [generate an Atlassian API token](https://id.atlassian.com/manage-profile/security/api-tokens)
    - `$FORGE_APP_ID`: copy your Forge app id from `manifest.yml`, after you strip out the `ari:cloud:ecosystem::app/` part. E.g. if the id is `ari:cloud:ecosystem::app/cc531b66-7b42-474c-bbd7-805c73d0asdfasd` then you should set the value as `cc531b66-7b42-474c-bbd7-805c73d0asdfasd`
    - `$EXTERNAL_AUTH_SERVICE_KEY`: slack
    - `$SLACK_CLIENT_ID`: Copy the value from the Slack app configuration, in `Basic information / App Credentials / Client ID`
    - `$SLACK_CLIENT_SECRET`: Copy the value from the Slack app configuration, in `Basic information / App Credentials / Client Secret`

11. All set 🎉

### Addtional docs

1. All our example build on top of `@atlassianintegrations/polaris-forge-object-resolver` library. README available here: [README](https://www.npmjs.com/package/@atlassianintegrations/polaris-forge-object-resolver)
