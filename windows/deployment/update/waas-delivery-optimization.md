---
title: Delivery Optimization for Windows client updates
manager: dougeby
description: This article provides information about Delivery Optimization, a peer-to-peer distribution method in Windows 10.
keywords: oms, operations management suite, wdav, updates, downloads, log analytics
ms.prod: w10
ms.mktglfcycl: deploy
audience: itpro
author: jaimeo
ms.localizationpriority: medium
ms.author: jaimeo
ms.collection:
- M365-modern-desktop
- m365initiative-coredeploy
- highpri
ms.topic: article
ms.custom: seo-marvel-apr2020
---

# Delivery Optimization for Windows client updates

**Applies to**

- Windows 10
- Windows 11

> **Looking for Group Policy objects?** See [Delivery Optimization reference](waas-delivery-optimization-reference.md) or the master spreadsheet available at the [Download Center](https://www.microsoft.com/download/details.aspx?id=102158).

Windows updates, upgrades, and applications can contain packages with large files. Downloading and distributing updates can consume quite a bit of network resources on the devices receiving them. You can use Delivery Optimization to reduce bandwidth consumption by sharing the work of downloading these packages among multiple devices in your deployment. Delivery Optimization is a cloud-managed solution that allows clients to download those packages from alternate sources (such as other peers on the network) in addition to the traditional Internet-based servers. You can use Delivery Optimization with Windows Update, Windows Server Update Services (WSUS), Windows Update for Business, or Microsoft Endpoint Manager (when installation of Express Updates is enabled).  

 Access to the Delivery Optimization cloud services and the Internet, are both requirements for using the peer-to-peer functionality of Delivery Optimization.

For information about setting up Delivery Optimization, including tips for the best settings in different scenarios, see [Set up Delivery Optimization](waas-delivery-optimization-setup.md). For a comprehensive list of all Delivery Optimization settings, see [Delivery Optimization reference](waas-delivery-optimization-reference.md).

>[!NOTE]
>WSUS can also use [BranchCache](waas-branchcache.md) for content sharing and caching. If Delivery Optimization is enabled on devices that use BranchCache, Delivery Optimization will be used instead. 

## New in Windows 10, version 20H2 and Windows 11

- New peer selection options: Currently the available options include: 0 = None, 1 = Subnet mask, and 2 = Local Peer Discovery. The subnet mask option applies to both Download Modes LAN (1) and Group (2). If Group mode is set, Delivery Optimization will connect to locally discovered peers that are also part of the same Group (have the same Group ID)."
- Local Peer Discovery: a new option for **[Restrict Peer Selection By](waas-delivery-optimization-reference.md#select-a-method-to-restrict-peer-selection)** (in Group Policy) or **DORestrictPeerSelectionBy** (in MDM). This option restricts the discovery of local peers using the DNS-SD protocol. When you set Option 2, Delivery Optimization will restrict peer selection to peers that are locally discovered (using DNS-SD). If Group mode is enabled, Delivery Optimization will connect to locally discovered peers that are also part of the same group, for those devices with the same Group ID).

> [!NOTE]
> The Local Peer Discovery (DNS-SD, [RFC 6763](https://datatracker.ietf.org/doc/html/rfc6763)) option can only be set via MDM delivered policies on Windows 11 builds. This feature can be enabled in supported Windows 10 builds by setting the `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\DeliveryOptimization\DORestrictPeerSelectionBy` value to **2**. For more information, see [Delivery Optimization reference](/windows/deployment/update/waas-delivery-optimization-reference.md).

- Starting with Windows 11, the Bypass option of [Download Mode](waas-delivery-optimization-reference.md#download-mode) is no longer used.

## Requirements

The following table lists the minimum Windows 10 version that supports Delivery Optimization:

| Device type | Minimum Windows version 
|------------------|---------------|
| Computers running Windows 10 | Win 10 1511 |
| Computers running Server Core installations of Windows Server | Windows Server 2019 |
| Windows IoT devices | Win 10 1803 |

### Types of download content supported by Delivery Optimization

#### Windows Client

| Windows Client | Minimum Windows version | HTTP Downloader | Peer to Peer | Microsoft Connected Cache (MCC)
|------------------|---------------|----------------|----------|----------------|
| Windows Update (feature updates quality updates, language packs, drivers) | Win 10 1511, Win 11 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Windows 10 Store files | Win 10 1511, Win 11 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Windows 10 Store for Business files | Win 10 1511, Win 11 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Windows Defender definition updates | Win 10 1511, Win 11 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Intune Win32 apps| Win 10 1709, Win 11 | :heavy_check_mark: | :heavy_check_mark:  | :heavy_check_mark: |
| Microsoft 365 Apps and updates | Win 10 1709, Win 11 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Edge Browser Updates | Win 10 1809, Win 11 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Configuration Manager Express updates| Win 10 1709 + Configuration Manager version Win 10 1711, Win 11 | :heavy_check_mark: | :heavy_check_mark:  | :heavy_check_mark: |
| Dynamic updates| Win 10 1903, Win 11 | :heavy_check_mark: | :heavy_check_mark:  | :heavy_check_mark: |
| MDM Agent | Win 11 | :heavy_check_mark: | :heavy_check_mark:  | :heavy_check_mark: |
| Xbox Game Pass (PC) | Win 10 1809, Win 11 | :heavy_check_mark: |  | :heavy_check_mark: |
| Windows Package Manager| Win 10 1809, Win 11 | :heavy_check_mark: |  |  |
| MSIX | Win 10 2004, Win 11 | :heavy_check_mark: |  |  |

#### Windows Server

| Windows Server | Minimum Windows version | HTTP Downloader | Peer to Peer | Microsoft Connected Cache (MCC)
|----------------|--------------------------|----------------|----------|----------------|
| Windows Update | Windows Server 2019 (1809) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Edge Browser Updates | Windows Server 2019 (1809) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |

#### Linux (Public Preview)

| Linux ([Public Preview](https://github.com/microsoft/do-client)) | Linux versions | HTTP Downloader | Peer to Peer | Microsoft Connected Cache (MCC)
|------------------------|----------------|-----------------|--------------|---------------|
| Device Update for IoT Hub | Ubuntu 18.04, 20.04 / Debian 9, 10 | :heavy_check_mark: |  | :heavy_check_mark: |
> [!NOTE]
> Starting with Configuration Manager version 1910, you can use Delivery Optimization for the distribution of all Windows update content for clients running Windows 10 version 1709 or newer, not just express installation files. For more, see [Delivery Optimization starting in version 1910](/mem/configmgr/sum/deploy-use/optimize-windows-10-update-delivery#bkmk_DO-1910).

In Windows client Enterprise, Professional, and Education editions, Delivery Optimization is enabled by default for peer-to-peer sharing on the local network (NAT). Specifically, all of the devices must be behind the same NAT (which includes either Ethernet or WiFi), but you can configure it differently in Group Policy and mobile device management (MDM) solutions such as Microsoft Intune.

For more information, see "Download mode" in [Delivery optimization reference](waas-delivery-optimization-reference.md).

## Set up Delivery Optimization

See [Set up Delivery Optimization](waas-delivery-optimization-setup.md) for suggested values for many common scenarios.

You can use Group Policy or an MDM solution like Intune to configure Delivery Optimization. 

You will find the Delivery Optimization settings in Group Policy under **Configuration\Policies\Administrative Templates\Windows Components\Delivery Optimization**.
In MDM, the same settings are under **.Vendor/MSFT/Policy/Config/DeliveryOptimization/**.

Starting with Microsoft Intune version 1902, you can set many Delivery Optimization policies as a profile, which you can then apply to groups of devices. For more information, see [Delivery Optimization settings in Microsoft Intune](/intune/delivery-optimization-windows))

**Starting with Windows 10, version 1903,** you can use the Azure Active Directory (Azure AD) Tenant ID as a means to define groups. To do this set the value for DOGroupIdSource to its new maximum value of 5.

## Reference

For complete list of every possible Delivery Optimization setting, see [Delivery Optimization reference](waas-delivery-optimization-reference.md).

## How Microsoft uses Delivery Optimization
At Microsoft, to help ensure that ongoing deployments weren't affecting our network and taking away bandwidth for other services, Microsoft IT used a couple of different bandwidth management strategies. Delivery Optimization, peer-to-peer caching enabled through Group Policy, was piloted and then deployed to all managed devices using Group Policy. Based on recommendations from the Delivery Optimization team, we used the "group" configuration to limit sharing of content to only the devices that are members of the same Active Directory domain. The content is cached for 24 hours. More than 76 percent of content came from peer devices versus the Internet.

For more information, check out the [Adopting Windows as a Service at Microsoft](https://www.microsoft.com/itshowcase/Article/Content/851/Adopting-Windows-as-a-service-at-Microsoft) technical case study.

## Frequently asked questions

#### Does Delivery Optimization work with WSUS?
Yes. Devices will obtain the update payloads from the WSUS server, but must also have an internet connection as they communicate with the Delivery Optimization cloud service for coordination.

#### Which ports does Delivery Optimization use?
Delivery Optimization listens on port 7680 for requests from other peers by using TCP/IP. The service will register and open this port on the device, but you might need to set this port to accept inbound traffic through your firewall yourself. If you don't allow inbound traffic over port 7680, you can't use the peer-to-peer functionality of Delivery Optimization. However, devices can still successfully download by using HTTP or HTTPS traffic over port 80 (such as for default Windows Update data).

If you set up Delivery Optimization to create peer groups that include devices across NATs (or any form of internal subnet that uses gateways or firewalls between subnets), it will use Teredo. For this to work, you must allow inbound TCP/IP traffic over port 3544. Look for a "NAT traversal" setting in your firewall to set this up.

Delivery Optimization also communicates with its cloud service by using HTTP/HTTPS over port 80.

#### What are the requirements if I use a proxy?
For Delivery Optimization to successfully use the proxy, you should set up the proxy by using Windows proxy settings or Internet Explorer proxy settings. For details see [Using a proxy with Delivery Optimization](./delivery-optimization-proxy.md). Most content downloaded with Delivery Optimization uses byte range requests. Make sure your proxy allows byte range requests. For more information, see [Proxy requirements for Windows Update](/windows/deployment/update/windows-update-troubleshooting.md).

#### What hostnames should I allow through my firewall to support Delivery Optimization?

For communication between clients and the Delivery Optimization cloud service: **\*.do.dsp.mp.microsoft.com**.

**For Delivery Optimization metadata**:

- *.dl.delivery.mp.microsoft.com
- *.emdl.ws.microsoft.com

**For the payloads (optional)**:

- *.download.windowsupdate.com 
- *.windowsupdate.com

#### Does Delivery Optimization use multicast?
No. It relies on the cloud service for peer discovery, resulting in a list of peers and their IP addresses. Client devices then connect to their peers to obtain download files over TCP/IP.

#### How does Delivery Optimization deal with congestion on the router from peer-to-peer activity on the LAN?
Starting in Windows 10, version 1903, Delivery Optimization uses LEDBAT to relieve such congestion. For more details, see this post on the [Networking Blog](https://techcommunity.microsoft.com/t5/Networking-Blog/Windows-Transport-converges-on-two-Congestion-Providers-Cubic/ba-p/339819).

#### How does Delivery Optimization handle VPNs?
Delivery Optimization attempts to identify VPNs by checking the network adapter type and details and will treat the connection as a VPN if the adapter description contains certain keywords, such as "VPN" or "secure."

If the connection is identified as a VPN, Delivery Optimization will suspend uploads to other peers. However, you can allow uploads over a VPN by using the [Enable Peer Caching while the device connects via VPN](waas-delivery-optimization-reference.md#enable-peer-caching-while-the-device-connects-via-vpn) policy.

If you have defined a boundary group in Configuration Manager for VPN IP ranges, you can set the [DownloadMode](waas-delivery-optimization-reference.md#download-mode) policy to 0 for that boundary group to ensure that there will be no peer-to-peer activity over the VPN. When the device is not connected using a VPN, it can still use peer-to-peer with the default of LAN.

With split tunneling, make sure to allow direct access to these endpoints:

Delivery Optimization service endpoint:
- `https://*.prod.do.dsp.mp.microsoft.com`

Delivery Optimization metadata:
- `http://emdl.ws.microsoft.com`
- `http://*.dl.delivery.mp.microsoft.com`

Windows Update and Microsoft Store backend services and Windows Update and Microsoft Store payloads

- `http://*.windowsupdate.com`
- `https://*.delivery.mp.microsoft.com`
- `https://*.update.microsoft.com`
- `https://tsfe.trafficshaping.dsp.mp.microsoft.com`

For more information about remote work if you're using Configuration Manager, see this post on the [Configuration Manager blog](https://techcommunity.microsoft.com/t5/configuration-manager-blog/managing-patch-tuesday-with-configuration-manager-in-a-remote/ba-p/1269444).


#### How does Delivery Optimization handle networks where a public IP address is used in place of a private IP address?
Starting with Windows 10, version 1903 or later, Delivery Optimization no longer restricts connections between LAN peers to those using private IP addresses. If you use public IP addresses instead of private IP addresses, you can use Delivery Optimization in LAN mode.

> [!NOTE]
> If you use public IP addresses instead of private in LAN mode, the bytes downloaded from or uploaded to LAN peers with public IP addresses might be reported as coming from Internet peers.


## Troubleshooting

This section summarizes common problems and some solutions to try.

### If you don't see any bytes from peers

If you don't see any bytes coming from peers the cause might be one of the following issues:

- Clients aren’t able to reach the Delivery Optimization cloud services.
- The cloud service doesn’t see other peers on the network. 
- Clients aren’t able to connect to peers that are offered back from the cloud service.
- None of the computers on the network are getting updates from peers.

### Clients aren't able to reach the Delivery Optimization cloud services.

Try these steps:

1. Start a download of an app that is larger than 50 MB from the Store (for example "Candy Crush Saga").
2. Run `Get-DeliveryOptimizationStatus` from an elevated PowerShell window and observe the [DownloadMode](waas-delivery-optimization-reference.md#download-mode) setting. For peering to work, DownloadMode should be 1, 2, or 3.
3. If DownloadMode is 99, it could indicate your device is unable to reach the Delivery Optimization cloud services. Ensure that the Delivery Optimization host names are allowed access: most importantly **\*.do.dsp.mp.microsoft.com**.

### The cloud service doesn't see other peers on the network.

Try these steps:

1. Download the same app on two different devices on the same network, waiting 10 – 15 minutes between downloads.
2. Run `Get-DeliveryOptimizationStatus` from an elevated PowerShell window and ensure that **[DownloadMode](waas-delivery-optimization-reference.md#download-mode)** is 1 or 2 on both devices.
3. Run `Get-DeliveryOptimizationPerfSnap` from an elevated PowerShell window on the second device. The **NumberOfPeers** field should be non-zero.
4. If the number of peers is zero and **[DownloadMode](waas-delivery-optimization-reference.md#download-mode)** is 1, ensure that both devices are using the same public IP address to reach the internet (you can easily do this by opening a browser window and do a search for “what is my IP”). In the case where devices are not reporting the same public IP address, configure **[DownloadMode](waas-delivery-optimization-reference.md#download-mode)** to 2 (Group) and use a custom **[GroupID (Guid)](waas-delivery-optimization-reference.md#group-id)**, to fix this.

> [!NOTE]
> Starting in Windows 10, version 2004, `Get-DeliveryOptimizationStatus` has a new option `-PeerInfo` which returns a real-time list of the connected peers.

### Clients aren't able to connect to peers offered by the cloud service

Try a Telnet test between two devices on the network to ensure they can connect using port 7680. Follow these steps:

1. Install Telnet by running `dism /online /Enable-Feature /FeatureName:TelnetClient` from an elevated command prompt.
2. Run the test. For example, if you are on device with IP 192.168.8.12 and you are trying to test the connection to 192.168.9.17 run `telnet 192.168.9.17 7680` (the syntax is *telnet [destination IP] [port]*. You will either see a connection error or a blinking cursor like this /_. The blinking cursor means success.

> [!NOTE]
> You can also use [Test-NetConnection](/powershell/module/nettcpip/test-netconnection) instead of Telnet to run the test.
> **Test-NetConnection -ComputerName 192.168.9.17 -Port 7680**

### None of the computers on the network are getting updates from peers

Check Delivery Optimization settings that could limit participation in peer caching. Check whether the following settings in assigned group policies, local group policies, or MDM policies are too restrictive:

- Minimum RAM (inclusive) allowed to use peer caching
- Minimum disk size allowed to use peer caching
- Enable peer caching while the device connects using VPN.
- Allow uploads when the device is on battery while under the set battery level
