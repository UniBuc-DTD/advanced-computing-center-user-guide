# FortiGate IPsec VPN instructions

This page describes the VPN solution based on [Fortinet](https://www.fortinet.com/) technology. We've since deployed [a new VPN solution](vpn.md) based on the open source [NetBird](https://netbird.io/) platform and [WireGuard](https://www.wireguard.com/).

## Agentless VPN

For quick, browser-based connections, go to [`vpn-acc.unibuc.ro`](https://vpn-acc.unibuc.ro) and log in with your ACC username and password.

You can use the browser-based interface to access all ACC resources, including connecting to the HPC nodes using SSH.

## IPsec VPN

You must install **the latest stable version of [FortiClient](https://www.fortinet.com/support/product-downloads)** for your operating system (you do not need to register; choose the "FortiClient VPN-only" variant). Unfortunately, as mentioned above, FortiClient **doesn't** support IPsec-based VPN connections on Linux.

Once the VPN client is installed, open it and configure a new VPN connection:

- VPN type: `IPsec VPN`
- Connection Name: `ACC-UB`
- Remote Gateway: `vpn-acc.unibuc.ro`
- Authentication method: `Pre-shared key`
- Pre-shared key value: please request this from the ACC-UB administrators
- Username and password: the credentials you've been provided by the ACC administrators when your account was created
- Advanced settings (**required** to match the server's settings):
    - VPN settings:
        - IKE: Version 2
        - Address assignment: Mode Config
    - Phase 1:
        - IKE Proposal: AES256GCM + PRFSHA384 or AES256 + SHA256
        - DH Group: 20 or 21
    - Phase 2:
        - IKE Proposal: AES256GCM + NONE or AES256 + SHA256
        - Enable Perfect Forward Secrecy (PFS)
        - DH Group: 21

Furthermore, the only way to enable [EAP-TTLS](https://en.wikipedia.org/wiki/Extensible_Authentication_Protocol#EAP_Tunneled_Transport_Layer_Security_(EAP-TTLS)) support (which we require for authentication) is [to edit the client's internal XML configuration](https://community.fortinet.com/fortigate-3/technical-tip-how-to-enable-eap-ttls-for-ipsec-ikev2-tunnels-in-vpn-only-unlicensed-forticlient-213133). You have to go to the settings page, backup the configuration (save it to an XML file), edit the XML file to add `<eap_method>2</eap_method>` under the `<connection><ike_settings>` node, then re-import it using the restore button.

See also the images below.

![FortiClient IPsec VPN configuration](images/forticlient-ipsec-vpn-config-start.png)
![FortiClient IPsec Phase 1 & Phase 2 settings configuration](images/forticlient-ipsec-vpn-config-phases.png)
![FortiClient IPsec XML config eap-method setting](images/forticlient-ipsec-vpn-config-eap-method.png)
