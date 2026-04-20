# Forti VPN setup

The ACC VPN is based on [Fortinet](https://www.fortinet.com/) technology. You must install **the latest stable version of [FortiClient](https://www.fortinet.com/support/product-downloads)** for your operating system (you do not need to register; choose the "FortiClient VPN-only" variant).

Once the VPN client is installed, open it and configure a new VPN connection:

- Connection Name: `ACC-UB`
- Remote Gateway: `vpn-acc.unibuc.ro:44398`
- Customize Port: `44398`
- Username and password: the credentials you've been provided by the ACC administrators when your account was created

See also the image below.

![FortiClient VPN configuration](images/forticlient-config.png)
