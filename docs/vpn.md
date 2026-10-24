# VPN access

This page describes how to access the ACC's resources through a [_virtual private network_](https://en.wikipedia.org/wiki/Virtual_private_network) (VPN). We use [NetBird](https://netbird.io/) for managing the connections and keys, which relies on [WireGuard](https://www.wireguard.com/) for encrypting the network traffic. [Keycloak](https://www.keycloak.org/) (`sso.acc.unibuc.ro`) is used to provide OIDC-based single sign-on, based on our [FreeIPA](https://www.freeipa.org/) account database. This solution is [open source](https://github.com/netbirdio/netbird), cross-platform (supports Windows, MacOS, Linux, Android and iOS), [secure](https://www.wireguard.com/protocol/) and [highly performant](https://www.wireguard.com/performance/).

We used to rely on Fortinet's proprietary IPsec-based VPN before, but it had too many limitations. The old instructions for configuring the FortiClient VPN are available [here](forti-vpn.md). We cannot guarantee the former VPN solution will still be available for long.

## Setup guide

1. Go to [https://vpn.acc.unibuc.ro](https://vpn.acc.unibuc.ro/) and choose _Login with Keycloak_.

    ![NetBird landing page](images/netbird-vpn-landing-page.png){ width="300" }

2. Fill in your ACC user credentials and sign in.

    ![Keycloak sign-in page](images/keycloak-sign-in-page.png){ width="300" }

3. Follow the instructions shown on that page to download and install the appropriate [NetBird client](https://app.netbird.io/install) for your OS.

    ![NetBird install page](images/netbird-install.png){ width="300" }

4. After starting the NetBird app, choose the _Self-hosted_ option and use `https://vpn.acc.unibuc.ro` as the URL.

    ![NetBird app config](images/netbird-client-config.png){ width="300" }

5. Save the configuration and click on _Connect_ (press the toggle). NetBird will open a new tab in your browser, logging you in against our identity provider.

6. You should now be able to access all of the ACC-UB resources.

Try `ping hpc-ctrl01.acc-ub.local` to confirm that DNS resolution and the overlay network are working as expected.

Let us know if you're having trouble setting up the VPN connection; we can help. The official [NetBird client troubleshooting guide](https://docs.netbird.io/help/troubleshooting-client) is also useful.


