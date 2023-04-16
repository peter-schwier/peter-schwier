---
title: Run your own Firefox Accounts Server — Mozilla Services
pageTitle: Run your own Firefox Accounts Server — Mozilla Services
length: 3685
author: 
timestamp: 2023-04-16T14:47:41-05:00
markdownload-tags: []
markdownload-source: https://mozilla-services.readthedocs.io/en/latest/howtos/run-fxa.html
markdownload-hostname: mozilla-services.readthedocs.io
---

# Run your own Firefox Accounts Server — Mozilla Services

## Excerpt
> The Firefox Accounts server is deployed on our systems using RPM packaging,
and we don’t provide any other packaging or publish official builds yet.

---
The Firefox Accounts server is deployed on our systems using RPM packaging, and we don’t provide any other packaging or publish official builds yet.

Note

This guide is preliminary and vastly incomplete. If you have any questions or find any bugs, please don’t hesitate to drop by the IRC channel or mailing list and let us know.

The Firefox Accounts server is hosted in **git** and requires **nodejs**. Make sure your system has these, or install them:

-   git: [http://git-scm.com/downloads](http://git-scm.com/downloads)
-   nodejs: [http://nodejs.org/download](http://nodejs.org/download)

A self-hosted Firefox Accounts server requires two components: an auth-server that manages the accounts database, and a content-server that hosts a web-based user interface.

Clone the fxa repository and follow the README to deploy your own auth-server and content-server instances:

-   [https://github.com/mozilla/fxa/](https://github.com/mozilla/fxa/)

Now direct Firefox to use your servers rather than the default, Mozilla-hosted ones. The procedure varies a little between desktop and mobile Firefox, and may not work on older versions of the browser.

For desktop Firefox version 52 or later:

-   Enter “[about:config](about:config)” in the URL bar
-   Right click anywhere on the “[about:config](about:config)” page and choose _New > String_
-   Enter “identity.fxaccounts.autoconfig.uri” for the name, and your content-server URL for the value.
-   Restart Firefox for the change to take effect.

Note that this must be set prior to loading the sign-up or sign-in page in order to take effect, and its effects are reset on sign-out.

For Firefox for iOS version 9.0 or later:

-   Go to Settings.
-   Tap on the Version number 5 times.
-   Tap on “Advance Account Settings”
-   Enter your content-server URL
-   Toggle “Use Custom Account Service” to on.

For Firefox Preview for Android (“Fenix”):

-   There is not yet support for using a non-Mozilla-hosted account server.
-   The work is being tracked in a github issue at [https://github.com/mozilla-mobile/fenix/issues/3762](https://github.com/mozilla-mobile/fenix/issues/3762).

For Firefox for Android (“Fennec”) version 44 or later:

-   Enter “[about:config](about:config)” in the URL bar,
    
-   Search for items containing “fxaccounts”, and edit them to use your self-hosted URLs:
    
    > -   use your auth-server URL to replace “api.accounts.firefox.com” in the following settings:
    >     -   identity.fxaccounts.auth.uri
    > -   use your content-server URL to replace “accounts.firefox.com” in the following settings:
    >     -   identity.fxaccounts.remote.webchannel.uri
    >     -   webchannel.allowObject.urlWhitelist
    > -   optionally, use your oauth- and profile-server URLs to replace “{oauth,profile}.accounts.firefox.com” in
    >     -   identity.fxaccounts.remote.profile.uri
    >     -   identity.fxaccounts.remote.oauth.uri
    

**Important**: _after_ creating the Android account, changes to “identity.fxaccounts” prefs will be _ignored_. (If you need to change the prefs, delete the Android account using the _Settings > Sync > Disconnect…_ menu item, update the pref(s), and sign in again.) Non-default Firefox Account URLs are displayed in the _Settings > Sync_ panel in Firefox for Android, so you should be able to verify your URL there.

Since the Mozilla-hosted sync servers will not trust assertions issued by third-party accounts servers, you will also need to [run your own sync-1.5 server](https://mozilla-services.readthedocs.io/en/latest/howtos/run-sync-1.5.html#howto-run-sync15).

Please note that the fxa-content-server repository includes graphics and other assets that make use of Mozilla trademarks. If you are doing anything other than running unmodified copies of the software for personal use, please review the Mozilla Trademark Policy and Mozilla Branding Guidelines:

> -   [https://www.mozilla.org/en-US/foundation/trademarks/policy/](https://www.mozilla.org/en-US/foundation/trademarks/policy/)
> -   [http://www.mozilla.org/en-US/styleguide/identity/mozilla/branding/](http://www.mozilla.org/en-US/styleguide/identity/mozilla/branding/)

You can ask for help:

-   on IRC (irc.mozilla.org) in the #fxa channel
-   in our Mailing List: [https://mail.mozilla.org/listinfo/dev-fxacct](https://mail.mozilla.org/listinfo/dev-fxacct)

> Saved from https://mozilla-services.readthedocs.io/en/latest/howtos/run-fxa.html on 2023-04-16T14:47:41-05:00