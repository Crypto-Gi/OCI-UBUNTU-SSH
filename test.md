# Silver Peak Unity ECOSTM (ECOS) Release Notes

###### Version 9.2.11.1_95160 Revision A: January 2, 2025

 This document provides important information about ECOS 9.2.11.1, including top items for the release, release limitations, new features, issues fixed, upgrade considerations, and known issues.

## Top Items for this Release

###### [Before upgrading to ECOS 9.2.11.1, except if you are upgrading from 8.3.3.0+, you need to disable the “Verify ]
 Image Signature” option under Orchestrator’s Advanced Security Settings. You can enable this option again after the upgrade.



###### [After an appliance is upgraded to ECOS 9.2.11.1 and if image signature verification is enabled, the appliance can ]
 only be upgraded to newer releases that are signed using the Silver Peak certificate.


###### NOTE: FIPS appliances, or appliances with image signature verification enabled, will need to be upgraded to ECOS 8.3.6.1 before upgrading to 9.2.11.1.



###### [There is a risk of tunnels being down due to the incompatibility described in the ECOS IPSec UDP ESN anti-]
 replay compatibility matrix below. If all EdgeConnects are not being upgraded simultaneously to compatible releases, then before starting the upgrade, an admin should disable either the IPSec key rotation or anti-replay settings on all labels. These settings can be re-enabled after all EdgeConnects in the environment are upgraded. For important details, see Upgrade Considerations.

**ECOS Version** **9.4.1.0+** **9.3.2.0+** **9.2.8.0+** **9.1.10.0+** **9.2.2.x-** **9.1.2.x-**
**9.2.7.x** **9.1.9.x**

9.4.1.0+ Yes Yes Yes Yes No No

9.3.2.0+ Yes Yes Yes Yes No No

9.2.8.0+ Yes Yes Yes Yes No No

9.1.10.0+ Yes Yes Yes Yes No No

###### [ECOS 9.2.11.1 requires Silver Peak Unity Orchestrator][TM][ version 9.2.4 or later. Before upgrading any appliances to ]
 this version of ECOS, you must upgrade Orchestrator to at least 9.2.4.

|Col1|ECOS Version|9.4.1.0+|9.3.2.0+|9.2.8.0+|9.1.10.0+|9.2.2.x- 9.2.7.x|9.1.2.x- 9.1.9.x|Col9|
|---|---|---|---|---|---|---|---|---|
||9.4.1.0+|Yes|Yes|Yes|Yes|No|No||
||9.3.2.0+|Yes|Yes|Yes|Yes|No|No||
||9.2.8.0+|Yes|Yes|Yes|Yes|No|No||
||9.1.10.0+|Yes|Yes|Yes|Yes|No|No||


-----

## Top Items for this Release (continued)

###### [To ensure you can roll back the upgrade if needed, note the following prior to upgrading to 9.2.11.1: ]

 • 8.1.7.x customers should upgrade to 8.1.7.22, or a later 8.1.7.x release, before the upgrade.



###### • 8.1.9.x customers should upgrade to 8.1.9.15, or a later 8.1.9.x release, before the upgrade.

 If you need to roll back the upgrade and you did not upgrade to 8.1.9.15 first, you must first do the following on the appliance before the rollback or the procedure will fail:

 1. Unlink the /var/tmp, /var/run, and /var/lock symbolic links 2. If they don’t already exist, create tmp, run, and lock directories in /var

 You can find complete details about these steps in Procedure to Roll Back from 8.3.x.x+.



###### [Orchestrator and Cloud Portal certificate verification is enabled by default on new EdgeConnect appliances ]
 running ECOS 8.3.0.4 and higher. If the appliance is using Orchestrator as a proxy during the initial installation (e.g., MPLS-only sites), it will never connect to Orchestrator, or to Cloud Portal via Orchestrator, until certificate verification can be disabled manually via CLI or after making a direct connection to Cloud Portal.

 [ECOS 9.2.11.1 interoperates fully with most prior versions, though it may interoperate in Reduced Functionality ]
 mode with some older prior versions.


-----

## Before You Begin

###### Carefully review the following items and any linked sections before starting the upgrade.

 [Review all items under ][Release Limitations][,][ Known Issues,][ and ][Upgrade Considerations][ before starting the ]
 upgrade.



###### [If your network includes appliances running ECOS up to 8.1.7.x ][and][ 8.1.9.10 or later, see ][ECOS Compatibility for ]
 FastFail/Underlay Paths for details about incompatibilities between these releases.

 [If you have customized the system limits on an EC-V (Maintenance > Software & System Management > System ]
 Limits), or if the EC-V has 4 GB memory, it is recommended that you test the upgrade in a lab environment on an identical system before upgrading the production EC-V. If the upgrade fails (e.g., the EC-V is in a continuous reboot), lower some of the system limits or increase the memory on the EC-V, then test the upgrade again.


###### NOTE: If the EC-V currently has 4 GB of system memory, increase to 8 GB.


-----

## Release Limitations

###### This section lists feature limitations and other considerations for customers who are planning to use the ECOS and Orchestrator 9.1.x releases. As these limitations are addressed in subsequent builds of this release, this list will be updated in future revisions of the release notes.

#### Feature Limitations
###### The following features have these specified limitations:

 • When using the Radius snooping feature, source and destination must be collocated.

 • When multiple senders and multiple receivers use the same multicast group over SD-WAN fabric, the multicast feature may experience delays or not function properly.

#### Feature Limitations when Segmentation is Enabled
###### The following features are not supported in this release when segmentation is enabled:

 • IPv6

 • Network Address Translation (NAT), available in the 8.3 release, is not supported when segmentation is enabled. To apply NAT rules when segmentation is enabled, use Inter-Segment NAT.

 • Bridge Mode and Server Mode. Inline Router Mode is the only mode supported.

 • VRRP works as expected, but you cannot configure two groups with the same IP address. Overlapping subnets are supported, but the same IP address cannot be configured on two interfaces on the same appliance.

 • Flow redirection is not working when segmentation is enabled.

#### Features Supported in the Default or Same Segment
###### The following features work as expected when the feature is contained to the default segment or the same segment:

 • The IPSLA HTTP monitor is supported only in the default segment.

 • Multicast is supported in the default segment.

 • Management Services

 o HTTP(S), Cloud Portal, Orchestrator are only supported in the default segment.

 o RADIUS/TACACS+ are only supported in the default segment.

 • LAN-side passthrough tunnels (IPSec and GRE) must use the default segment.

 • AWS, Azure, and Check Point work in the default segment. You will need to create inter-segment policies to access these integrations from non-default segments.

 • DHCP server is only supported in the default segment, but IP address pools cannot overlap. If IP address pools do not overlap, DHCP server should work in multiple segments at the same site.


-----

## Release Limitations (continued)

#### Additional Considerations
###### Note the following additional considerations regarding the v9 release:

 • All appliances in your SD-WAN network must be running ECOS 9.0.x.x before the advanced segmentation feature can be enabled.

 • Peer priority and admin distance settings will be applied globally across all segments.

 • If you are using end-to-end Zone Based Firewall, it is recommended that you review the Zone Based Firewall document linked on the Orchestrator/ECOS v9 documentation resources page.

 • Enabling segmentation is a one-way operation. When enabled, it requires deleting the entire Overlay network and policies. Orchestrator will recreate every tunnel and policy for the entire network.

 • In ECOS 9.2.0.0, a behavioral change was introduced that affects how the generic app group name is derived. In prior releases, if the application was not part of any app group, the app group name would be “none” and the software did not attempt to derive the app group name using the generic app (https/http). Post-9.2.0.0, if the application is not part of any app group, the app group name is blank and the software attempts to derive the app group name using the generic app.

 • When creating an HPE Aruba Networking Central Account in Orchestrator from the Aruba Central Site Mapping tab: Because of how Aruba Central processes account information, if you click Test or Save during configuration, you must wait 30 minutes before you click Test or Save again. If you click Test or Save a second time before 30 minutes have passed, you will receive an error that the connection failed even if you successfully connected to Aruba Central. To resolve this issue, wait 30 minutes before clicking Test or Save again. For more information, see Aruba Central Site Mapping on the HPE Aruba Networking EdgeConnect SD-WAN Documentation site.


-----

## Security Issues Fixed

###### The following table contains security-related issues fixed in ECOS 9.2.x.x releases, organized by the software version that first resolved them.

|Issue ID|CVE|CVSS Score|CVSS Vector|Description|First Release to Resolve|
|---|---|---|---|---|---|
|ID: 71389|N/A|7.2|CVSS:3.1 /AV:N/AC:L/PR:H /UI:N/S:U/C:H/I:H /A:H|A validation error created the potential for an unauthenticated user to execute arbitrary code on the host.|9.2.10.0|
|ID: 71380|N/A|7.2|CVSS:3.0/AV:N/A C:L/PR:H/UI:N/S: U/C:H/I:H/A:H|An issue with port forwarding created the potential for an unauthenticated user to obtain a root shell on the appliance.|9.2.10.0|
|ID: 71361|N/A|N/A|N/A|A code sanitization issue created the potential for an unauthenticated user to execute arbitrary code on the appliance.|9.2.10.0|
|ID: 68775|N/A|7.2|CVSS:3.1 /AV:N/AC:L/PR:H /UI:N/S:U/C:H/I:H /A:H|An issue with the tunbug configuration file created a potential command injection vulnerability in the command line interface.|9.2.10.0|
|ID: 68197|N/A|7.2|CVSS:3.0/AV:N/A C:L/PR:H/UI:N/S: U/C:H/I:H/A:H|A validation error created the possibility for a malicious user to modify the prototype of a JavaScript object.|9.2.10.0|
|ID: 68343|CVE-2023-2650|6.5|CVSS:3.1/AV:N/A C:L/PR:N/UI:R/S: U/C:N/I:N/A:H|This release was patched to address CVE- 2023-2650 (OpenSSL vulnerability).|9.2.6.0|
|ID: 67010|CVE-2023-0215|7.5|CVSS:3.1/AV:N/A C:L/PR:N/UI:N/S: U/C:N/I:N/A:H|This release was patched to address CVE- 2023-0215 (OpenSSL vulnerability).|9.2.4.0|
|ID: 67009|CVE-2022-4450|7.5|CVSS:3.1/AV:N/A C:L/PR:N/UI:N/S: U/C:N/I:N/A:H|This release was patched to address CVE- 2022-4450 (OpenSSL vulnerability).|9.2.4.0|
|ID: 67008|CVE-2023-0286|7.4|CVSS:3.1/AV:N/A C:H/PR:N/UI:N/S: U/C:H/I:N/A:H|This release was patched to address CVE- 2023-0286 (OpenSSL vulnerability).|9.2.4.0|
|ID: 67006|CVE-2022-4304|5.9|CVSS:3.1/AV:N/A C:H/PR:N/UI:N/S: U/C:H/I:N/A:N|This release was patched to address CVE- 2022-4304 (OpenSSL vulnerability).|9.2.4.0|
|ID: 66760|N/A|N/A|N/A|A command issue created a potential vulnerability where admin users could execute arbitrary code as the root user.|9.2.4.0|
|ID: 66144|CVE-2023-30506|7.2|CVSS:3.1/AV:N/A C:L/PR:H/UI:N/S: U/C:H/I:H/A:H|A configuration vulnerability allowed the potential upload of malicious files through the web UI.|9.2.4.0|
|ID: 66112|CVE-2023-30503|7.2|CVSS:3.1/AV:N/A C:L/PR:H/UI:N/S: U/C:H/I:H/A:H|This release was patched to address CVE- 2023-3053 (command execution vulnerability).|9.2.4.0|


-----

|Issue ID|CVE|CVSS Score|CVSS Vector|Description|First Release to Resolve|
|---|---|---|---|---|---|
|ID: 65904|CVE-2023-30502|7.2|CVSS:3.1/AV:N/A C:L/PR:H/UI:N/S: U/C:H/I:H/A:H|A validation issue created a potential command injection vulnerability in the command line interface.|9.2.4.0|
|ID: 63835|N/A|N/A|N/A|Default SSH options on the appliance were erroneously presenting as security issues during vulnerability scans.|9.2.4.0|
|ID: 62644|CVE-2023-30509|4.9|CVSS:3.1/AV:N/A C:L/PR:H/UI:N/S: U/C:H/I:N/A:N|A file authentication issue created a potential vulnerability where admin users had read access to sensitive files.|9.2.4.0|
|ID: 62643|CVE-2023-30508|4.9|CVSS:3.1/AV:N/A C:L/PR:H/UI:N/S: U/C:H/I:N/A:N|A command issue created a potential vulnerability where admin users had read access to sensitive files.|9.2.4.0|
|ID: 62609|CVE-2023-30505|7.2|CVSS:3.1/AV:N/A C:L/PR:H/UI:N/S: U/C:H/I:H/A:H|A command issue created a potential vulnerability where admin users could execute shell commands as the root user.|9.2.4.0|
|ID: 59961|CVE-2023-30504|7.2|CVSS:3.1/AV:N/A C:L/PR:H/UI:N/S: U/C:H/I:H/A:H|A network monitoring vulnerability allowed the potential execution of arbitrary shell scripts.|9.2.4.0|
|ID: 72333|CVE-2021-41617|7.0|CVSS:3.1/AV:L/A C:H/PR:L/UI:N/S: U/C:H/I:H/A:H|This release was patched to address CVE- 2021-41617 (OpenSSH vulnerability).|9.2.3.0|
|ID: 66451|N/A|N/A|N/A|A configuration issue incorrectly allowed read/write permissions on SNMPv3.|9.2.3.0|
|ID: 72334|CVE-2022-31676|7.8|CVSS:3.1/AV:L/A C:L/PR:L/UI:N/S:U /C:H/I:H/A:H|This release was patched to address CVE- 2022-31676 (local privilege escalation vulnerability).|9.2.2.0|
|ID: 64781|CVE-2022-44533|7.2|CVSS:3.1/AV:N/A C:L/PR:H/UI:N/S: U/C:H/I:H/A:H|This release was patched to address CVE- 2022-44533 (RCE vulnerability).|9.2.2.0|
|ID: 62877|CVE-2022-43542|7.2|CVSS:3.1/AV:N/A C:L/PR:H/UI:N/S: U/C:H/I:H/A:H|This release was patched to address CVE- 2022-43542 (command injection vulnerability).|9.2.2.0|
|ID: 62707|CVE-2022-37926|5.5|CVSS:3.0/AV:N/A C:L/PR:H/UI:N/S: C/C:L/I:L/A:N|This release was patched to address CVE- 2022-37926 (cross-site scripting vulnerability).|9.2.2.0|
|ID: 62620|CVE-2022-44532|4.9|CVSS:3.1/AV:N/A C:L/PR:H/UI:N/S: U/C:H/I:N/A:N|This release was patched to address CVE- 2022-44532 (arbitrary file read vulnerability).|9.2.2.0|
|ID: 62607|CVE-2022-43541|7.2|CVSS:3.1/AV:N/A C:L/PR:H/UI:N/S: U/C:H/I:H/A:H|This release was patched to address CVE- 2022-43541 (command injection vulnerability).|9.2.2.0|
|ID: 62929|CVE-2023-30507|4.9|CVSS:3.1/AV:N/A C:L/PR:H/UI:N/S: U/C:H/I:N/A:N|This release was patched to address CVE- 2023-30507 (read/write vulnerability).|9.2.1.0|
|ID: 62736|CVE-2023-30510|4.1|CVSS:3.1/AV:N/A C:L/PR:H/UI:N/S: C/C:L/I:N/A:N|This release was patched to address CVE- 2023-30510 (server-side request forgery vulnerability).|9.2.1.0|


-----

|Issue ID|CVE|CVSS Score|CVSS Vector|Description|First Release to Resolve|
|---|---|---|---|---|---|
|ID: 62670|CVE-2022-37923|7.2|CVSS:3.0/AV:N/A C:L/PR:H/UI:N/S: U/C:H/I:H/A:H|This release was patched to address CVE- 2022-37923 (command injection vulnerability).|9.2.1.0|
|ID: 62668|CVE-2022-37922|7.2|CVSS:3.1/AV:N/A C:L/PR:H/UI:N/S: U/C:H/I:H/A:H|This release was patched to address CVE- 2022-37922 (command injection vulnerability).|9.2.1.0|
|ID: 62666|CVE-2022-37921|7.2|CVSS:3.1/AV:N/A C:L/PR:H/UI:N/S: U/C:H/I:H/A:H|This release was patched to address CVE- 2022-37921 (command injection vulnerability).|9.2.1.0|
|ID: 62608|CVE-2023-30501|7.2|CVSS:3.1/AV:N/A C:L/PR:H/UI:N/S: U/C:H/I:H/A:H|This release was patched to address CVE- 2023-30501 (command injection vulnerability).|9.2.2.0|
|ID: 62573|CVE-2022-43518|6.5|CVSS:3.0/AV:N/A C:L/PR:L/UI:N/S:U /C:H/I:N/A:N|This release was patched to address CVE- 2022-43518 (arbitrary file read vulnerability).|9.2.0.0|
|ID: 61781|CVE-2022-37919|7.5|CVSS:3.1/AV:N/A C:L/PR:N/UI:N/S: U/C:N/I:N/A:H|Scans running against EdgeConnect were causing appliances to be intermittently grayed out in Orchestrator.|9.2.0.0|
|ID: 61715|CVE-2022-25236|9.8|CVSS:3.1/AV:N/A C:L/PR:N/UI:N/S: U/C:H/I:H/A:H|The appliance was patched to address CVE-2022-25236 (expat advisory).|9.2.0.0|
|ID: 61264|CVE-2022-0778|7.5|CVSS:3.1/AV:N/A C:L/PR:N/UI:N/S: U/C:N/I:N/A:H|The appliance was patched to address CVE-2022-0778 (OpenSSL advisory).|9.2.0.0|


-----

## Issues Fixed

###### The following known issues have been fixed in ECOS 9.2.11.1.

 ID: 66533 An issue with the way DF flags were stored and propagated in IPv6 flow labels caused performance issues on the appliance.

 NOTE To see a historical list of issues fixed in appliance software releases, see Issues Fixed from Past Releases.


-----

## Upgrade Considerations

###### The following list summarizes considerations that must be addressed when upgrading from any previous version of appliance software to ECOS 9.2.11.1.

 Change in default settings for src-ip-based DNS cache When source IP address-based DNS caching is enabled, the EdgeConnect appliances can experience performance issues during high volumes of DNS traffic. Typical symptoms include unexpected packet loss and bouncing BGP peer session establishment. This has been observed mostly on datacenter appliances where DNS traffic to and from remote branches is concentrated.

 Source-based DNS caching is disabled by default in ECOS 9.2.9.0+, ECOS 9.3.3.0+, and ECOS 9.4.2.0+. For older releases, the source IP address-based DNS caching mechanism can be administratively disabled using the following CLI command. Source IP-based DNS caching is an extension of the DNS caching feature that stores the source IP of the DNS client in the cache. This has limited use in some circumstances. This recommendation disables only the source IP-based DNS cache; the DNS caching feature continues to work even when source IP-based DNS caching is disabled.

 Note: This command is available only in software releases ECOS 9.1.4.4+ and ECOS 9.2.3.0+. Older releases must first upgrade to a fixed version to mitigate this issue.
```
silverpeak > enable
silverpeak # configure terminal
silverpeak (config) # dns cache src-ip disable
silverpeak (config) # exit
silverpeak # write memory
silverpeak #

 Upgrade all appliances to avoid tunnels dropping from IPSec anti-replay incompatibility If all EdgeConnects in the environment are not being upgraded to a compatible release simultaneously, as per the compatibility matrix, then additional upgrade planning is required.

 Three workarounds are available when upgrading. Use whichever of the following options works best for your environment:

 1. Disable key rotation before starting upgrades and re-enable after all appliances are upgraded. 

 Note: Upon disabling key rotation, the Orchestrator generates and distributes the new key to all appliances one final time. Before you begin upgrades, make sure that the new key is distributed and activated.

 a. Before disabling the key rotation, navigate to Support > Reporting > IPSec UDP Status in Orchestrator, and then verify that all appliances in the Active Key column have a status of Yes.

 b. Navigate to Configuration > Overlays & Security > Security > IPSec Key Rotation, and then clear the Enable Key Rotation check box. This triggers a new key generation, distribution, and activation.

 c. Repeat step a to verify that all appliances have the current and active key material.

 d. Proceed with appliance upgrades per your upgrade schedule.

 e. Navigate to Configuration > Overlays & Security > Security > IPSec Key Rotation, and then select the Enable Key Rotation check box.

 2. Disable IPSec anti-replay on all network WAN labels, and then re-enable after all appliances in the network are upgraded.

 Note: Disabling and enabling anti-replay causes the existing tunnel state to bounce once. Plan a maintenance window to make these changes.

```

-----

## Upgrade Considerations (continued)

###### a. In Orchestrator, navigate to Orchestrator > Orchestrator Server > Tools > Tunnel Settings.

 b. Click IPSec.

 c. In the IPSec anti-replay window dropdown menu, select Disable.

 d. Click Save.

 3. Upgrade all appliances in the network at the same time. To mitigate the risk of a tunnel being down due to the incompatibility described in the compatibility matrix, all appliances should be upgraded together to ECOS 9.1.10.0+, ECOS 9.2.8.0+, ECOS 9.3.2.0+, or ECOS 9.4.1.0+, as appropriate.

 IPSec anti-replay window incompatibility Orchestrator 9.1.4 disables IPSec anti-replay window functionality unless all appliances are running ECOS 9.1.2.0 or later or ECOS 9.2.2.0 or later. This avoids a compatibility issue that prevents IPSec tunnels from forming between appliances running older ECOS versions.

 Fragments in Multicast Not Supported ECOS does not support multicast IP fragment re-assembly. Starting from ECOS 9.2.7.0, multicast fragments are dropped. For more information, see ID: 69595 in the list of Issues Fixed for this release.

 C5 Instance Types Now Recommended In previous releases, using M4 instance types was recommended for WAN bandwidth up to 500 Mbps. It is now recommended to use C5. If you are still using M4 instance types, it is strongly recommended to use c5.xlarge instead.

 Change in Enforce Return Tunnel Behavior Starting with ECOS 9.0.4.0, return traffic is placed in the same tunnel from which forward traffic is received only when no better route is found. When a better route (either with lower metric or higher peer priority) is available, return traffic may be placed in a different tunnel.

 Minor Alarm Upon Upgrading from Older ECOS Images An alarm may trigger on Orchestrator version 9.1 or newer under the following conditions:

 • A pair of appliances deployed in EdgeHA configuration are upgraded from ECOS version 9.0 or older to ECOS version 9.1 or newer, OR

 • A pair of appliances deployed in EdgeHA configuration running ECOS version 9.1 or newer are added and the number of EdgeHA subnets is less than 32.

 To clear this alarm, perform the following steps for each EdgeHA pair (or for each EdgeHA pair identified in the alarm, if the alarm contains this information):

 • If the number of EdgeHA subnets is less than 32, modify the EdgeHA configuration so that the number of subnets is 32 or more.

 • If the number of EdgeHA subnets is already 32 or more, open the Deployment page for each appliance, open HA link, click OK, do not make any changes, and then click Save.


-----

## Upgrade Considerations (continued)

###### New Configuration Requirement for IKE-based IPSec Tunnels Under the presence of NAT devices, IKE_ID must be configured correctly for the tunnel to come up; there is no more wildcard lookup for pre-shared keys (PSK).

 Previously, for IKEv1 and IKEv2, PSK lookup was based on (ANY, remote IKE_ID). This caused unreliable PSK lookup when there were two different local IKE_IDs with different PSKs going to the same remote IKE_ID. Now, for IKEv1 and IKEv2, PSK lookup is based on (local IKE_ID, remote IKE_ID), which returns PSK reliably.

 Note: When IKE_IDs are not configured (left blank), EdgeConnect assumes tunnel endpoint IP addresses as IKE_IDs (local IP as local IKE_ID, remote IP as remote IKE_ID).

 When upgrading to ECOS 9.2.11.1:

 • In Orchestrator 9.1.3 or later, all Orchestrated third-party IPSec tunnels and SDWAN fabric IKE IPSec tunnels (with or without NAT devices in between) should come up as expected, as Orchestrator sets IKE_IDs explicitly.

 • All manually created third-party IPSec tunnels and SDWAN fabric IKE tunnels:

 With NAT devices in between With IKE_IDs configured   Tunnel UP

 With NAT devices in between Without IKE_IDs configured   Tunnel DOWN

 Without NAT devices in between With IKE_IDs configured   Tunnel UP

 Without NAT devices in between Without IKE_IDs configured   Tunnel UP

 Change in DHCP Option Values Starting with ECOS 8.3.3.0, DHCP option 43 and option 63 values will be written to dhcpd.conf exactly as entered by the user in the UI. In earlier releases, user input was enclosed in double quotes before being written to dhcpd.conf. For any value that had been entered in the UI enclosed in double quotes, the quotes will be removed upon upgrade. 

 For example, if the user entered 10.1.2.3 in the UI, the value would have been written to dhcpd.conf as “10.1.2.3” in releases prior to 8.3.3.0 and will be written as 10.1.2.3 in 8.3.3.0+. If the value needs to be enclosed in double quotes, users will need to re-enter the value after the upgrade.

 DHCP Option 43 Value Error DHCP option 43 values should be in colon-separated hex format (e.g., fe:02). If you have DHCP option 43 configured, ensure the value is in colon-separated hex format before upgrading. 

 Route Map Creation when Upgrading from 8.1.9.x It is important to understand how route maps are created when upgrading from ECOS 8.1.9.x:

 • When upgrading from 8.1.9.x to 8.3.0.x, existing route redistribution configurations will be migrated to route maps. 

 • Static routes with the options “Advertise to Silver Peak Peers,” “Advertise to OSPF Peers,” and “Advertise to BGP Peers” will be added as prefix-based entries in the respective route maps.

 • BGP Peer Export Polices will be converted and added to route maps, which are created per peer. 

 Note: Export policy options 1-3, 6, and 8-9 will be converted as is. Policy options 4, 5, and 7 are not converted, so care must be taken to either enable the appropriate options before upgrade or verify whether the route map should be updated after the upgrade.

|With NAT devices in between|With IKE_IDs configured|Tunnel UP|
|---|---|---|
|With NAT devices in between|Without IKE_IDs configured|Tunnel DOWN|
|Without NAT devices in between|With IKE_IDs configured|Tunnel UP|
|Without NAT devices in between|Without IKE_IDs configured|Tunnel UP|


-----

## Upgrade Considerations (continued)

###### • OSPF configurations will be converted to route maps based on the configuration that is currently in place on the appliance.

 Advertising New Static Routes: Upgrades from 8.1.9.x and Earlier A new static route configured on an appliance after upgrading to ECOS 8.3.x will not be advertised to local BGP and OSPF routing peers on remote EdgeConnect appliances running 8.1.9.x and earlier

 Recommendations

 1. Upgrade all EdgeConnect appliances to ECOS 8.3.x at the same time. The upgrade process migrates previous configurations by creating system generated route maps.

 - or 
 2. Upgrade all EdgeConnect appliances with BGP/OSPF peering on the LAN interfaces first.

 Workaround in Mixed Environments (8.1.9.x and 8.3.x)

 1. A new EdgeConnect should be deployed on 8.1.9.x to ensure it is added to the SD-WAN fabric in working order. After verifying connectivity, the appliance can be upgraded to 8.3.x. This ensures compatibility in a mixed environment.

 2. If you want to configure new static routes on appliances running 8.3.x, you must do the following: Create identical static routes on 8.1.9.x appliances by enabling “Advertise to BGP Peers” and “Advertise to OSPF Neighbors” as appropriate, and do not enable the “Advertise to Silver Peak Peers” option.

 VLANs Must Be Registered on the Deployment Page When upgrading physical or virtual appliances to any 8.3.x.x release, traffic from VLANs will only be captured if the VLAN is registered on the appliance deployment page.

 “ECOS” Replaces “VXOA” in ‘show version’ Commands The product name and release now reflect “ECOS” instead of “VXOA” for ‘show version’ and ‘show version concise’ in the CLI.

 Upgrade to Latest Release Available When upgrading from a previous release train (for example, 8.2.1.x to 8.3.0.x), you should upgrade to the latest version currently available to prevent upgrade failures and to get the latest security and product updates. 

 Upgrades from ECOS 6.2 ECOS 6.2 can be directly upgraded to ECOS 9.2.11.1. Network downtime is expected during the process. It is recommended to upgrade spokes first, then hubs. 

 Orchestrator Certificate Check – Internet Access and Proxy Considerations In this ECOS release, appliances are configured to reject self-signed and privately signed certificates by default. If Orchestrator does not have a certificate signed by a public CA, the appliance will fail to connect to Orchestrator.

 This feature can be disabled on Orchestrator under Advanced Security Settings. In a typical deployment, the appliance connects to Cloud Portal over the internet by establishing a TLS socket to Orchestrator using Cloud Portal as a proxy. Using this socket, Orchestrator informs the appliance to disable the certificate check.


-----

## Upgrade Considerations (continued)

###### In some environments, the appliance cannot communicate with Cloud Portal (no internet connectivity or connecting through a proxy [1]). In these cases, the steps noted above to disable the validity check will not work, and the appliance will not be able to connect to Orchestrator.

 Removal of Weak Cyphers Support for older, weak ciphers has been removed from SNMP, HTTPS, and ssh. Please use modern tools to manage Silver Peak appliances. Additionally, TLS 1.0 and 1.1 are no longer supported for HTTPS access – only TLS 1.2 is supported. 

 Limited Support for Inline Router Mode on x600 Inline router mode support on x600 platforms is limited. Please contact customer support if you wish to enable inline router mode on x600 model.

 Removal of ip datapath route Command The “ip datapath route” CLI command has been replaced with the “subnet” command.

 No Support for Specific NX-1700 Appliances ECOS 9.2.11.1 does not support NX-1700 appliances with part number 200404-001.

 10 Gbps Default for Auto-sensing Fiber Interfaces Auto-sensing fiber interfaces are configured for 10 Gbps operation by default. If you wish to use fiber interfaces in 1 Gbps mode, please set the interface speed via Configuration > [System & Networking] Interfaces.

 Change Admin Distance if PE-router is Configured Admin distance must be changed if BGP peer-type of PE-router is configured. When upgrading from ECOS 8.1.5.x, change the “Subnet Shared” and “BGP Remote” Admin Distance values to lower than the default value of 20 (for example, 10).

 Enhanced Admin Distance Configuration The 8.1.9.4 release replaced Silver Peak “PE” and “Branch” AD values with EBGP and IBGP AD values respectively. After upgrade: 

 • The EBGP AD will be the previously configured BGP-PE AD 

 • The IBGP AD will be the previously configured BGP-Branch AD. 

 For new installations the following are the default ADs: 

 • Subnet-shared static = 10 

 • Subnet-shared-BGP = 15 

 • Subnet-shared-OSPF = 15 

 • EBGP = 20 

1 Proxies typically have internally signed certificates, which will cause certificate validation of Cloud Portal to fail.


-----

## Upgrade Considerations (continued)

###### • IBGP = 200 

 • OSPF = 110

 If branch or branch-transit BGP peers on LAN interfaces are configured as EBGP peers, ensure that the default ADs or configured ADs in your network will continue to result in intended traffic paths for destination prefixes received from multiple sources (e.g., same prefix received from MPLS PE router, subnet-sharing, and branch side BGP peers). If you notice any change in traffic patterns after the upgrade, you may want to adjust ADs or other BGP parameters to achieve desired traffic behavior.

 Change Duplicate Routes Advertisement Starting in release 8.1.9.4, the software selects the best route based on metric and then determines if the best route needs to be advertised. If there are duplicate routes with the same characteristics, the appliance chooses one of the routes at random. Due to this, you should remove any configured duplicate routes and enable the LAN subnet advertisement flag. 


-----

## Known Issues

###### The following list contains known issues in ECOS 9.2.11.1.

 ID: 75751 Multicast over SD-WAN fabric is not functioning properly when multiple senders and multiple receivers are present in the same multicast group.

 ID: 73621 When receiving a BGP route with MED set to 0, the default metric value is applied incorrectly, resulting in a local BGP route being advertised to the peer despite the SD-WAN fabric being the more favorable route.

 ID: 68602  A latency issue causes tunnels to drop unexpectedly. Debugging code was added to identify the root cause of this issue.

 ID: 67981 When BGP is enabled and a BGP and BFD session is established, then a BFD table row leak occurs and BGP is disabled globally in the segment.

 ID: 67577 The EC-10104 appliance does not currently support bridge mode.

 ID: 66287 Upon upgrade to version 9.2.0.0, an assertion change in the EC-V configuration files invalidates 1-vCPU configurations, causing tunneld to reboot unexpectedly.

 ID: 65980 When the hub appliance is set to “Do Not Re-Advertise Routes,” the spoke does not learn the hub’s locally connected IPv6 route.

 ID: 63472 When there are ECMP routes on the LAN side, IP spoof check drops flows coming on LAN interfaces that have ECMP. A fix will be available in future release.

 As a workaround, if there are ECMP routes on the LAN side, disable IP spoof check in the Firewall Protection Profile. If there are no ECMP routes on the LAN side, there is no issue.

 ID: 63049 When a protection profile is enabled, traceroute via SD-WAN tunnel receives no response from the remote appliance.

 ID: 62037 On the EC-XL-H-10G appliance, an interface configured with 1 Gbps link speed comes up as a 10 Gbps interface.

 ID: 61398 An interface with VRRP configured on it loses its IP address when it is moved from one routing segment to another. To avoid this, VRRP configuration needs to be removed before the segment change and re-added afterwards.

 ID: 60120 IPv6 IPsec UDP tunnels are flapping at random intervals on AWS EC-V instances.

 ID: 59585 The EC-XL-P, or any platform that uses Intel i40 NICs, does not turn off the optical signal when the interface is in the admin down state. Due to this, a device connected on the other side will see the link as up and continue to use it. It is recommended that customers using these ports perform an "admin down" on both sides of the link.

 ID: 54195 When upgrading from 8.1.7.x, users must ensure that peer priority values are not zero. Peer priority values must be set to appropriate positive integer values before performing the upgrade.

 ID: 51187 In an EdgeHA configuration with regional routing, hubs are resending routes learned from the EdgeHA site back to the same site. To work around this issue, do the following on each appliance in the EdgeHA pair:


-----

## Known Issues (continued)

###### 1. On the BGP page (Configuration > Networking > Routing > BGP), enable BGP, configure the ASN and Router ID, and then click Apply. 

 NOTE: It is not necessary to configure BGP peers if you are using OSPF on the LAN side.

 2. On the Routes page (Configuration > Networking > Routing > Routes), select the checkbox for “Filter Routes From SD-WAN Fabric With Matching Local ASN” and “Include BGP Local ASN to routes sent to SD-WAN Fabric,” and then click Apply.

 ID: 24657 For SaaS optimization in router mode, both LAN- and WAN-side routes must be identical. Configuring a different LAN-side route (aka LAN gateway) is not currently supported.

 ID: 23470 PPPoE is only supported in ILRM. PPPoE interfaces cannot be used for flow redirection.

 ID: 16824 Jumbo frames are not supported on Virtual Appliances installed on the KVM hypervisor.

 ID: 16799 Jumbo frames are not supported on Virtual Appliances installed on the XenServer hypervisor. Only one VLAN is supported on Virtual Appliances installed on the XenServer hypervisor. The XenServer hypervisor can be configured with one and only one VLAN; the hypervisor will strip the VLAN tag and send untagged packets to the Virtual Appliance.

 ID: 16106 The EdgeConnect-US, EdgeConnect-XS, NX-700 and NX-1700 appliances do not support jumbo frames.

 ID: 15101 Jumbo frames are not supported on Virtual Appliances installed on Microsoft’s Hyper-V hypervisor.

 ID: 14168 Hot-swapping an SSD (or an SSD failure) may result in TCP connection resets or IP packet drops.

 ID: 13155 If the https server and client negotiate an SSL compression method that is other than “NONE” (which can happen if a compression method is configured on the https server), the connection will not receive SSL-specific optimization (deduplication). If this occurs the error message reported in Current Flows will be “'unsupported SSL compression method'”. To work around this, configure the compression method on the https server as “NONE”.

 ID: 12818 Using VMware Snapshots severely degrades the performance of Virtual Machines. Do not take snapshots of Silver Peak Virtual Appliances.

 ID: 10929 On the VX, VRX, and EC-V Virtual Appliances, configuration of Ethernet MTU, speed and/or duplex settings requires host configuration in addition to Virtual Appliance configuration in the Appliance Manager.

 ID: 9316 Application classification of http on non-standard ports relies on heuristics that are determined after flow creation. Therefore, the application classification is valid for reporting and monitoring but cannot be used for route, QoS, or optimization match lookups because these lookups occur simultaneously with flow creation. Such flows will be annotated “Heuristically Classified” in Monitoring > Current Flows.

 ID: 6508 mgmt1 cannot be in the same subnet as mgmt0. Always use separate subnets for mgmt0 and mgmt1.

 ID: 6370 Bonding interfaces may fail to negotiate correctly with a Cisco switch after an appliance reboot. To avoid this, enable auto-recovery on the Cisco switch to which the appliance is connected.


-----

## Known Issues (continued)

###### ID: 6333 If WCCP custom redirection is used with a 7-bit mask, add a route map entry that directs all WCCP control traffic (protocol UDP, port 2048) from Appliance IP addresses to pass-through unshaped.

 ID: 6332 Auto-optimization is not effective in a network where there is a NAT implementation or a firewall that does TCP sequence offsetting. In such networks, connections will fail to optimize.

 ID: 6330 While configuring tunnels, software checks are not present to disallow the VRRP virtual IP address from being the tunnel endpoint. Configuring the virtual IP address to be the tunnel endpoint can disrupt traffic when a VRRP master switch happens and is not recommended.

 ID: 2899 CIFS directory browsing optimization is not effective with Samba servers.


-----

## Additional Information

###### This section contains additional information about ECOS software, as well as past features and fixes that are included in this release.

#### System Requirements

##### Hardware Compatibilities and Dependencies

###### ECOS supports Unity EdgeConnect US, Unity EdgeConnect XS, Unity EdgeConnect S, Unity EdgeConnect M, Unity EdgeConnect L, Unity EdgeConnect XL, NX-700, NX-1700, NX-2500, NX-2600, NX-2610, NX- 2700, NX-3500, NX-3600, NX-3700, NX-5500, NX-5504, NX-5600, NX-5700, NX-6700, NX-7500, NX-7504, NX-7600, NX-7700, NX-8504, NX- 8600, NX-8700, NX-9610, NX-9700, NX-10700, and NX-11700 Silver Peak Appliance hardware, and Unity EdgeConnect V, VX-500, VX-1000, VX-2000, VX-3000, VX-5000, VX- 6000, VX-7000, VX-8000 and VX-9000 Silver Peak Virtual Appliance software, and VRX-2, VRX-4, VRX-6, and VRX-8 Velocity Replication Acceleration software.

##### Software Compatibilities and Dependencies

###### • In the latest ECOS release, both HTTP and HTTPS connections to the Appliance Manager GUI are supported; HTTPS is the recommended method of connection. The default method supports both.

 • The Silver Peak Appliance’s Command Line Interface (CLI) can be accessed through a remote connection to the device’s management interface using Secure Shell (ssh), or via the serial console port. For security reasons, telnet connections are not supported.

 • The Silver Peak Software upgrade process supports transferring the software image from a server to the appliance via HTTP, HTTPS, FTP, and SCP (Secure Copy), via the Orchestrator (GMS), or transferring the image directly from a host running the Appliance Manager GUI.

 • It is highly recommended that interconnected appliances run the same image version. It is also highly recommended that all appliances run the latest software version, 8.3.0.3. Consult the hardware/software compatibility matrix on the Silver Peak Customer Support Portal at http://www.silver- peak.com/support/portal_login.asp to ensure compatibility of your hardware platform(s) with ECOS 8.3.0.3.

 • Please refer to the Silver Peak Appliance Manager Operator’s Guide for product and feature descriptions, detailed instructions on how to configure and monitor the Silver Peak Appliances, and for detailed instructions on how to install or upgrade appliance software.

 • Silver Peak Systems does not recommend or support WCCP deployments with WCCP running on the Catalyst 6500 or 7600 running Hybrid CATOS.


-----

#### New Features from Past Releases
###### The following table contains new features and enhancements, organized by the software version that first included them.

|Appliance Model|Part Number|
|---|---|
|EC-10104|201857-001|

|Feature|Description|Earliest Release with Feature|
|---|---|---|
|SNMP Support|There is now SNMP support for standard Linux Server MIBs, such as CPU, memory, disk, and other metrics.|9.2.11.0|
|IDS/IPS on EC-10104|IDS/IPS functionality is now supported on the EC-10104 appliance (8 GB/4-core model).|9.2.3.0|
|LTE Over USB|EdgeConnect now supports Aruba USB LTE Modems (500731-001).|9.2.3.0|
|Branch NAT No-Translate Rules|The restriction of overlapping a translated source/destination subnet with a source/destination subnet has been removed.|9.2.3.0|
|Support for New Appliance Model|This release adds support for the following part number: Appliance Model Part Number EC-10104 201857-001|9.2.3.0|
|Enable Logging for WAN-side Stateful Interface Drops|Users can configure Log Settings to include WAN-side stateful drops.|9.2.1.0|
|Proxy ARP Support|EdgeConnect now responds to any ARP received with the appliance's own local MAC address. In combination with Private/Community VLANs on the downstream switch, all traffic flows through the EdgeConnect, forming a Layer-2 hub and spoke topology within an Ethernet segment. Proxy ARP can be turned on and off per interface/label.|9.2.0.0|
|MAC Address Assignment for GCP Appliances|CloudInit now fetches MAC and subnet information from the metadata server and automatically assigns MAC addresses for GCP appliances.|9.2.0.0|
|Link Aggregation Control Protocol (LACP)|LACP provides a negotiation mechanism to control link aggregation. Link aggregation combines data from multiple interfaces into a channel group that provides a single high-speed link. Configuring link aggregation also adds failover redundancy to the interfaces in the group.|9.2.0.0|
|Multicast Group Filtering|Users can now allowlist multicast groups, so that EdgeOS processes only the groups matching the defined list.|9.2.0.0|
|Secure Logging|Orchestrator now allows you to configure the port number and protocol of remote log receivers and upload client certificates for remote log receivers.|9.2.0.0|
|OSPF and BGP Route Map Enhancements|Several enhancements to OSPF and BGP route maps now enable community filtering for OSPF routes, AS override in BGP neighbor configuration, and LE/GE prefix matching.|9.2.0.0|
|Large Scale Nexthop Adjacencies|The number of adjacencies (nexthops) supported over each interface has been increased from 16 to 127.|9.2.0.0|
|Increase in Scaling for OSPF and BGP Neighbor|This feature increases the scale of the number of OSPF neighbors supported on a single appliance, thereby increasing the maximum limit to at least 64 neighbors over two or more interfaces.|9.2.0.0|


-----

|Appliance Model|Part Number|
|---|---|
|EC-XL-H-10G|201915|

|Appliance Model|Part Number|
|---|---|
|EC-L-H|201754|
|EC-XL-H|201756|
|EC-XS|201694|

|Feature|Description|Earliest Release with Feature|
|---|---|---|
|BiDirectional Forwarding Detection (BFD)|BFD is a networking protocol that detects faults between devices. In addition to supporting single- and multi-hop configurations and asynchronous mode, BFD can be configured for up to 20 segments with a maximum of 100 simultaneous sessions across all segments. The EdgeConnect appliance supports BFD for both BGP and OSPF.|9.2.0.0|
|AVC Attributes|There are now additional static attributes under the Address Map parameter that can be used as match criteria. These attributes are secondary parameters to the address map, and are evaluated for a policy match only when the configured address map parameter matches with the flow. This release includes support for MS Instance, MS Category, and Proxy attributes.|9.2.0.0|
|Firewall Protection Profiles|Users can now add firewall protection profiles in the Configuration menu. Protection profiles allow users to define firewall thresholds around specific threats and security objectives of an environment where the firewall will be used, map the profile to a segment or zone of the firewall, and quickly add/edit the profile as a template.|9.2.0.0|
|5K Tunnels for EC-XS|There is now support for up to 5,000 tunnels on EC-XS platforms with 16GB RAM and four cores.|9.2.0.0|
|IPSec Suite B|There is now a more robust set of secure algorithms for IPSec tunnel establishment and data exchange. NOTE: This feature is not fully supported in ECOS 9.2.0.0. Full support will be provided in a future version.|9.2.0.0|
|Intrusion Prevention System (IPS)|In addition to the existing Intrusion Detection System (IDS), which designates traffic for inspection using matching rules, IPS protects traffic by matching a signature and then performing a configured action (alert, block, or allow).|9.2.0.0|
|Radius Snooping|EdgeConnect now provides identity and context-aware micro segmentation based on user and device information collected during radius authentication. Users can write policies based on user-based match criteria for traffic steering, selecting firewall zones, and other policies.|9.2.0.0|
|Support for New Appliance Models|This release adds support for the following part numbers: Appliance Model Part Number EC-XL-H-10G 201915|9.2.0.0|
|Support for New Appliance Models|This release adds support for the following part numbers: Appliance Model Part Number EC-L-H 201754 EC-XL-H 201756 EC-XS 201694|9.1.1.0|


-----

|Appliance Model|Part Number|
|---|---|
|EC-M-H AC|201762|

|Feature|Description|Earliest Release with Feature|
|---|---|---|
|Support for New Appliance Models|This release adds support for the following part numbers: Appliance Model Part Number EC-M-H AC 201762|9.1.0.0|
|Performance Enhancements|This release introduces a software-centric approach to enhancing performance for the EdgeConnect appliances, whether hardware- based, virtual, or cloud-hosted. These enhancements include improvements to total flows/sec capacity, as well as overall improvements to total throughput (packets-per-second), without requiring any upgrades to existing hardware. Performance optimizations in this release are based on new techniques to maximize parallel processing and are focused primarily on the higher-end appliances, where the most processing power is available.|9.1.0.0|
|Intrusion Detection System (IDS)|This release includes an Intrusion Detection System (IDS) that can monitor traffic for potential threats and malicious activity and generate threat events based on preconfigured rules. Packets are copied and inspected against signatures downloaded to Orchestrator from Cloud Portal. Traffic is designated for inspection using matching rules enabled in the zone-based firewall.|9.1.0.0|
|Support for ACL Group Objects|This release includes two new features related to ACLs: Address Groups and Service Groups. An address group is a logical collection of IP hosts or subnets, and a service group is a logical collection of protocols and ports. Both can be referenced in source or destination matching criteria in the zone-based firewall and security policies.|9.1.0.0|
|Zone Based Firewall Overrides for Control Traffic|Two new built-in policies, 65508 and 65509, will exempt Cloud Portal HTTPS and HTTP traffic to ensure that essential management services are not blocked by firewall misconfigurations.|9.1.0.0|
|Support for Non-routing Hub (Stub Hub)|This release adds support for designating a non-routing hub or stub hub by configuring it to not re-advertise spoke-learned routes to other hubs in the region.|9.1.0.0|
|Shaper Max Bandwidth for EdgeHA|This release includes an enhancement that will change the shaper max bandwidth setting when the primary circuit goes down.|9.1.0.0|
|Feature Licensing|In addition to traditional bandwidth and Boost licensing, this release adds support for specific feature licensing on a per appliance basis. Feature licensing allows administrators to enable specific features only on the appliances that require it.|9.1.0.0|
|Improved Failover between Primary and Backup Labels|This release adds support for enabling faster (more aggressive) failover between primary and backup labels. This change can be enabled or disabled via CLI using: system aggressive-path-fail [enable | disable] This setting is disabled by default.|9.0.5.0|


-----

|Feature|Description|Earliest Release with Feature|
|---|---|---|
|Support for Link Aggregation|This release adds support for link aggregation, which allows users to combine two, three, or four interfaces into a channel group that provides a single high-speed link. Configuring link aggregation also adds failover redundancy to the interfaces in the group. You can configure link aggregation under Configuration > System & Networking > Link Aggregation.|9.0.3.0|
|Show BGP Routes via CLI|Routes received from and advertised to a BGP neighbor can be displayed via the CLI.|9.0.3.0|
|Import Management Routes to Routing Table|In this release, any non-default route added to the management table (e.g., 1.1.1.0/24-->10.1.1.1, lan0) with a datapath outgoing interface will be automatically added to routing table. This feature can be enabled and disabled and is enabled by default during an upgrade. Turning this feature on or off will add or delete any pre- configured management routes to the routing table.|9.0.3.0|
|NSSA Support in OSPF|This release allows the configuration of an OSPF area, and its type can be set to standard or NSSA (Not-So-Stubby Area).|9.0.3.0|
|DSCP Marking per Interface|This feature allows the user to configure DSCP marking on tunnel packets, normally determined by QoS policy, to be overridden on a per WAN interface basis.|9.0.3.0|
|Source Interface Configuration for DNS|When configuring DNS, users can now configure the source interface associated with each DNS server IP address. Source interface determines the routing segment in which the DNS server can be used and the IP address to use.|9.0.3.0|
|TCP Application Delay Stats for IPFIX|In this release, application delay is calculated for TCP connections and included in the NetFlow/IPFIX exports.|9.0.3.0|
|Advanced Segmentation Supported in OSPF|This release supports OSPF routing in multiple segments.|9.0.3.0|
|DHCP Support in Advanced Segmentation|This release adds support for advanced segmentation in DHCP relay. A single DHCP relay instance running on the appliance works with a remote, segmentation-aware DHCP server to serve all interfaces that may belong to different routing segments.|9.0.3.0|
|Custom CA Certificate Trust Store|Orchestrator’s trust store can be customized by adding and deleting CA certificates under Configuration > System & Networking > Custom CA Certificate Trust Store.|9.0.3.0|
|Enable or Disable Portal WebSocket Connection|In the Appliance UI, under Administration > Silver Peak Cloud Portal & Orchestrator, administrators now have an option to enable or disable WebSockets as a redundant connection option.|9.0.2.0|
|Configure Encryption and Hash Algorithms|Using the ssh server encryption-algos and ssh server mac-algos CLI commands, administrators can select one or more provided algorithms for encryption and hashing. Selected algorithms can be reset with the no modifier.|9.0.2.0|
|Subnet Sharing Metric Enhancements|Subnet sharing metric values can be configured on a per-peer basis, allowing hubs in an MRSS (multi-region subnet sharing) region to redistribute a given route with different metrics.|9.0.2.0|
|ECMP Support for BGP|This release adds support for ECMP (Equal Cost Multi Path) routing to up to 20 BGP peers.|9.0.2.0|


-----

|Feature|Description|Earliest Release with Feature|
|---|---|---|
|Dynamic Subnet Sharing Hold- down Timer|Subnet sharing automatically throttles updates to a given peer to limit update traffic.|9.0.2.0|
|Route Map Enhancements|This update enables the use of an OSPF tag as a matching criterion in route redistribution to subnet sharing.|9.0.2.0|
|Added Appliance CPU Stats|In this release, appliance CPU usage stats have been added to appliance historical stats.|9.0.2.0|
|Multicast Enhancements|This release adds stability to multicast when advanced segmentation is enabled.|9.0.2.0|
|New HTTP Ping Metrics|Loss and latency metrics for HTTP ping monitor are now supported.|9.0.2.0|
|Enhancements for Cleared and Acknowledged Alarms|The following details are now captured for alarms: Cleared By, Acked By, Acked Time, Comments|9.0.2.0|
|Secure WebSocket Connection to Orchestrator|To establish a secure connection to Orchestrator, appliances will now generate an auth token and encrypt it using the account key.|9.0.0.0|
|Advanced Segmentation (VRF)|This release includes support for Advanced Segmentation (VRF), enabling multiple routing tables on a single appliance. Segments do not share data routes – data packets are only forwarded between interfaces within the same segment. Because routing segments are independent, overlapping IP address spaces can be used by multiple segments. NOTE: It is recommended that you review the release notes for the Orchestrator version 9 release, as well as the Orchestrator/ECOS v9 documentation resources, available here.|9.0.0.0|
|Secure Shell Access|This release includes support for varying levels of security for shell access: open shell access, secure shell access, or disabled shell access. In the case of upgrades, the default mode of operation will continue to be open shell access. For new installations, the default mode of operation will be secure shell access. In this mode, shell access requires a challenge-response from Silver Peak technical support.|9.0.0.0|
|Certificate Verification with Orchestrator and Cloud Portal|Users can now enforce certificate verification with Orchestrator and Cloud Portal under Orchestrator’s Advanced Security Settings. If this feature is enabled, the FQDN for Cloud Portal or Orchestrator must be used instead of global IPs.|9.0.0.0|
|Enhanced Multicast Support|This release provides additional support for multicast over the SD- WAN fabric.|9.0.0.0|
|Improved Software Upgrade Stability|This release increases the stability of appliance software upgrades by adding image signature and binary checksum verifications.|9.0.0.0|
|Security Enhancements|This release patches several low and medium severity security vulnerabilities identified in ECOS software.|9.0.0.0|
|Enhancements to Disabled Subnet Sharing via IP SLA|Disabling subnet sharing via IP SLA rules has been enhanced to ignore updates from appliance peers, allowing for greater control of traffic flow. Additionally, those routes will not be shared to BGP/OSPF peers.|9.0.0.0|
|Custom Tags in YAML Preconfig|YAML preconfig now supports the use of up to eight custom tags to be set on appliances.|9.0.0.0|


-----

|Feature|Description|Earliest Release with Feature|
|---|---|---|
|IPSec Anti-Replay Improvements|IPSec anti-replay window protection has been enhanced to support a window size of up to 64K.|9.0.0.0|
|Multiple Domain Matches in DNS Cache|This release adds support for multiple domain matches in the DNS cache.|9.0.0.0|
|Port Flexibility for Bonding|This feature enables customers to bond only LAN side interfaces. Previously, when enabling bonding, both LAN and WAN side interfaces were bonded (blan0 and bwan0).|9.0.0.0|
|API Support for Route Redistribution Templates|In this release, Route Redistribution Templates are supported in the REST API.|8.3.2.1|
|Secure WebSocket Connection to Orchestrator|To establish a secure connection to Orchestrator, appliances will now generate an auth token and encrypt it using the account key.|8.3.2.0|
|Multiple Ranges for DHCP Server|DHCP Settings now support adding multiple IP address ranges under the DHCP Server options.|8.3.2.0|
|Security Enhancements|This release patches several low and medium severity security vulnerabilities identified in ECOS software.|8.3.2.0|
|Custom Tags in YAML Preconfig|YAML preconfig now supports the use of up to eight custom tags to be set on appliances.|8.3.2.0|
|Security Enhancements|Changes have been made that greatly reduce or eliminate the possibility of a cross-site forgery request (CSRF) on the appliance.|8.3.1.0|
|Custom Bonding Improvements|Added a new custom bonding option that performs load-balancing based on tunnel capacity.|8.3.1.0|
|New Link Bonding Option|Added a new link bonding option that supports user-configurable link prioritization and traffic steering/load balancing policies.|8.3.1.0|
|IPSec Anti-replay Enhancements|IPSec anti-replay window protection has been enhanced to support window size of up to 64K.|8.3.1.0|
|Increased Support for Traceroute|Traceroute is now supported across stateful-SNAT firewall type, across allow-all type with NAT configured, as well as across EdgeHA links.|8.3.1.0|
|Report Improvements|The Top Applications report now excludes Silver Peak control (non- user) traffic.|8.3.1.0|
|Ping IPSLA Enhancements|Ping IPSLA monitor has been enhanced to include loss/latency measurements and thresholds; ping IPSLA can now be directed into a 3rd party IPSec or GRE tunnel.|8.3.1.0|
|Expanded IPv6 DHCP Support|IPv6 DHCP is now supported on WAN interfaces.|8.3.1.0|
|Internet Breakout Improvements|The internet breakout feature has been enhanced, enabling selection of the best quality internet link for local break-out based on user- defined criteria.|8.3.1.0|
|Support for New Appliance Model|This release adds support for the EC-S-P appliance model, part number 201687.|8.3.0.12|
|Power Supply Monitoring on EC-S-P|Power supplies, single or dual, are monitored for A/C input failure and PSU failure in the EC-S-P appliance.|8.3.0.12|


-----

|Appliance|Fiber Card Part Number|
|---|---|
|EC-XL-P|201632|
|EC-XL-P-NM|201633|

|Feature|Description|Earliest Release with Feature|
|---|---|---|
|32 Interfaces Supported on EC- V|EC-V now supports up to 32 interfaces along with auto-mac configuration.|8.3.0.7|
|Increased Support for Inbound Port Forwarding Rules|The limit for inbound port forwarding has been increased to support up to 100 rules.|8.3.0.7|
|Remote IP Logged for TACACS+ and RADIUS|ECOS 8.3.0.0 adds support for logging the source IP in the “Remote- Address” field for requests that originate via TACACS+ and RADIUS.|8.3.0.0|
|Route Filtering|ECOS 8.2.1 adds route filtering to control route redistribution between subnet sharing, OSPF and BGP.|8.2.1.0|
|IPFIX Enhancements|ECOS 8.2.1 enhances IPFIX by adding support for firewall logging and application performance information elements.|8.2.1.0|
|LAN-side VTI|ECOS 8.2.1 adds support for Virtual Tunnel Interfaces on LAN-side interfaces.|8.2.1.0|
|Dead Peer Detection for IPSec Service Chaining|ECOS 8.2.1 adds support for DPD (Dead Peer Detection) for IPSec Service Chaining.|8.2.1.0|
|Branch NAT|ECOS 8.2 adds support for branch-side NAT which connects multiple sites with overlapping IP addresses to a single SD-WAN fabric. “Branch NAT" performs S-NAT or D-NAT for each session with user configured NAT pools and NAT address mapping. Branch NAT is configured via the Orchestrator “Configuration > NAT.”|8.2.0.0|
|Multi-Region Subnet Sharing|ECOS 8.2 enhances subnet sharing by adding support for regions. With regions, hubs can redistribute spoke-learned routes to other spokes in the same region and/or hubs in other regions.|8.2.0.0|
|IPSec Service Chaining IKEv2|IPSec Service Chaining has been enhanced to support IKEv2.|8.2.0.0|
|BGP over IPSec|ECOS 8.2.0 supports BGP over IPSec tunnels with VTIs (Virtual Tunnel Interfaces). VTIs can be configured via the Orchestrator “Configuration > VTI” and then added as interfaces for BGP peers via “Configuration > BGP.”|8.2.0.0|
|Multicast GUI Support|ECOS 8.2.0 adds GUI configuration of the multicast feature via Configuration > Multicast.|8.2.0.0|
|BGP Configuration of Source Address|ECOS 8.2.0 adds the ability to configure the source IP address for a specific BGP peer.|8.2.0.0|
|Support for 25 Gbps Fiber Interface Cards|This release adds support for 25 Gbps fiber interface cards in the EC- XL appliance with the following part numbers: Appliance Fiber Card Part Number EC-XL-P 201632 EC-XL-P-NM 201633|8.1.9.12|
|spsadmin Account Removed|The spsadmin account has been removed.|8.1.9.12|
|Reject Self-signed Certificates|Appliances can now be configured to reject self-signed certificates. This feature addresses CVE-2020-12143 and CVE-2020-12144.|8.1.9.12|


-----

|EC-M-P|201552|
|---|---|
|EC-M-B|201553|
|NX-2700|201554|
|NX-3700|201555|
|NX-5700|201556|
|NX-6700|201557|
|NX-7700|201558|

|Feature|Description|Earliest Release with Feature|
|---|---|---|
|IKE-less Seed Distribution|A new IKE-less seed distribution mechanism is now supported in ECOS. This feature addresses CVE-2020-12142.|8.1.9.12|
|Directory Traversal Fixes|API changes have been made to restrict traversal of other directories, limiting access to sensitive data.|8.1.9.12|
|CSRF Fixes|Changes have been made that greatly reduce or eliminate the possibility of a cross-site forgery request (CSRF) on the appliance.|8.1.9.12|
|Support for additional hardware appliance part numbers.|ECOS 8.1.9 adds support for the following part numbers. EC-M-P 201552 EC-M-B 201553 NX-2700 201554 NX-3700 201555 NX-5700 201556 NX-6700 201557 NX-7700 201558|8.1.9.6|
|DNS Proxy|ECOS 8.1.9 adds DNS proxy support to allow local Internet breakout traffic to resolve DNS with one server while internal traffic is resolved via a separate server. DNS proxies are configured vis the Orchestrator “Configuration > DNS Proxy.” It is required to create a loopback interface for the DNS proxy (see below).|8.1.9.5|
|Support for Multiple DHCP Relay Agents|ECOS 8.1.9 allows the configuration of multiple DHCP relay agents (on an interface or VLAN basis).|8.1.9.5|
|Zone Based Firewall Flow Logging|ECOS 8.1.9 adds flow logging to the zone-based firewall. This is configured via the Orchestrator “Security Policies” tab.|8.1.9.5|
|Loopback Interfaces|ECOS 8.1.9.5 adds support for loopback interfaces, configured via Orchestrator under Configuration > Networking > Loopback Interfaces.|8.1.9.5|
|USB ZTP Configuration|ECOS 8.1.9.5 adds support for USB-based Zero Touch Provisioning (ZTP) by searching for a yaml pre-configuration file on attached USB drives.|8.1.9.5|
|Zscaler Orchestration|ECOS 8.1.9 adds support for orchestration of tunnels to Zscaler ZENs. Please consult the Orchestrator release notes for details on this feature.|8.1.9.4|
|Multi-hop BGP|ECOS 8.1.9 adds support for BGP peers that are not directly connected.|8.1.9.4|
|BGP Next-Hop Self|ECOS 8.1.9 adds configurable BGP next-help-self. BGP next-hop-self can be enabled from Configuration > BGP.|8.1.9.4|
|AS Path Propagate|ECOS 8.1.9 adds the capability to propagate AS path learned at remote sites to local BGP peers. This can be enabled from Configuration > BGP.|8.1.9.4|
|Port Forwarding Enhancement|Port forwarding now supports protocol wildcard specifier “any”.|8.1.9.4|


-----

|EC-L-B|201270|
|---|---|
|EC-XL-B|201271|
|EC-L-B-NM|201272|
|EC-XL-B-NM|201273|
|EC-L-P|201305|
|EC-XL-P|201306|
|EC-L-P-NM|201307|
|EC-XL-P-NM|201308|

|Feature|Description|Earliest Release with Feature|
|---|---|---|
|Admin Distance Enhancements|ECOS 8.1.9 replaces Silver Peak “PE” and “Branch” AD values with EBGP and IBGP AD values respectively. After upgrade: • The EBGP AD will be the previously configured BGP-PE AD • The IBGP AD will be the previously configured BGP-Branch AD. For new installations the following are the default ADs: • Subnet-shared static = 10 • Subnet-shared-BGP = 15 • Subnet-shared-OSPF = 15 • EBGP = 20 • IBGP = 200 • OSPF = 110 Please review “Considerations for Upgrade.”|8.1.9.4|
|BGP Graceful Restart|ECOS 8.1.9 adds support for BGP graceful restart. BPG graceful restart can be enabled from Configuration > BGP.|8.1.9.3|
|Support for additional hardware appliance part numbers.|ECOS 8.1.9 adds support for the following part numbers. EC-L-B 201270 EC-XL-B 201271 EC-L-B-NM 201272 EC-XL-B-NM 201273 EC-L-P 201305 EC-XL-P 201306 EC-L-P-NM 201307 EC-XL-P-NM 201308|8.1.9.1|
|Find Preferred Route and Admin Distance / Peer Priority Metrics|ECOS 8.1.9 provides the ability to find the preferred route for a specific IP address. This is available via Configuration > Routes. ECOS 8.1.9 adds a metric column to Configuration > Routes that can be toggled to display either the admin distance or peer priority metrics.|8.1.9.1|
|“Comments” Field for Deployment|ECOS 8.1.9 adds a “Comments” field to the deployment dialog which retains notes or comments specific to the appliance.|8.1.9.1|
|Support for Multiple LAN-side Interfaces in the Same Subnet|ECOS 8.1.9 adds support for multiple LAN-side interfaces in the same subnet.|8.1.9.1|
|Disabling Support for OSPF Opaque LSAs|ECOS 8.1.9 adds the capability to disable support for opaque OSPF LSAs. Opaque LSAs are enabled by default and can be disabled via the CLI commend: ospf opaque disable.|8.1.9.1|
|ATA Secure Erase|ECOS 8.1.9 adds ATA secure erase that wipes the entire contents of a drive at the hardware level. Extremely security conscious customers may want to exercise this command prior to replacing and disposing of a failed SSD. ATA secure erase is available via the CLI command: system disk <disk ID> remove secure.|8.1.9.1|


-----

|Feature|Description|Earliest Release with Feature|
|---|---|---|
|“Fail Open” in Bridge Mode|ECOS 8.1.9 adds the capability to configure a bridge mode appliance to “fail open” instead of the default fail-to-wire behavior. This is configurable via the CLI command: system bypass mode [fail-to- open | fail-to-close | fail-to-nic | default].|8.1.9.1|
|Protection from Port Scanning|ECOS 8.1.9 adds protection from port scanning devices or software by internally rate limiting the rate at which connections with no data are serviced.|8.1.9.1|
|SSL Optimization Enhancement|SSL optimization now supports ECDHE ECDSA with AES 256 GCM SHA384.|8.1.9.1|
|DNS Application Classification Enhancement|DNS application classification has been enhanced to cached DNS entries across appliance reboots.|8.1.9.1|
|Multicast|ECOS 8.1.9 introduces support for Protocol Independent Multicast - Sparse Mode (PIM-SM). Currently this feature supports MPLS links only and is configurable by the CLI: • pim set rp <rp-ip> • pim interface <name of intf> enable/disable • show pim neighbors • show pim mroute • show igmp groups • show pim interfaces • show pim rp • igmp interface <name of intf> enable/disable Multicast is a Beta feature.|8.1.9.0|
|IP Address ACL Match Enhancements|• ECOS 8.1.9 enhances the ACL match for IP Address by allowing for ranges and wildcards.|8.1.9.0|
|Enhancements to SaaS Optimization|SaaS optimization now supports: • User-defined SaaS applications. This enables SaaS optimization for applications that are not available from the Silver Peak portal. Configuration of SaaS optimization probe interface via labels in addition to physical interfaces.|8.1.9.0|
|Explicit Declaration of Implicit Policies|ECOS 8.1.9 displays built-in policies via “Support > User Documentation > Built-in Policies.”|8.1.9.0|
|Inbound Port Forwarding Enhancement|Inbound port forwarding has been enhanced to allow in-bound WAN packets to pass un-modified (not translated) to the LAN.|8.1.9.0|
|SSL Optimization Enhancement|SSL optimization now supports ECDHE named curve 29.|8.1.9.0|
|MOS Estimation|ECOS 8.1.9 adds MOS (Mean Opinion Score) estimates for Quality of Experience. MOS estimates are visualized via Orchestrator.|8.1.9.0|


-----

|Feature|Description|Earliest Release with Feature|
|---|---|---|
|Zone Based Firewall|ECOS 8.1.8 introduces zone-based firewalling which enables end-to- end network segmentation across an enterprise SD-WAN network. The zone-based firewall provides complete segmentation between zones on the LAN side with built-in anti-spoofing. Zones can be defined based on physical interfaces, logical interfaces, sub-interfaces, VLAN-tags, or layer-7 ACLs. The zone-based firewall is fully automated via the Orchestrator “Security Policies” tab.|8.1.8.0|
|IPSec Service Chaining|ECOS 8.1.8 introduces IPSec service chaining to 3rd parties (e.g. cloud-hosted next generation firewalls). IPSec service chaining can be configured via the Orchestrator “Tunnels” tab.|8.1.8.0|
|IPFIX|ECOS 8.1.8 adds support for flow export via IPFIX. IPFIX is enabled via the Orchestrator “Flow Export” tab.|8.1.8.0|
|IP SLA Enhancements|ECOS 8.1.8 adds an HTTP ping monitor to the IP SLA tracking feature. IP SLA is fully automated via the Orchestrator “IP SLA” tab.|8.1.8.0|
|Web Proxy Support|ECOS 8.1.8.0 now supports application classification when deployed behind a web proxy.|8.1.8.0|
|EC-M-P and EC-M-B|ECOS 8.1.7 introduces support for the Unity EdgeConnect EC-M-P and EC-M-B appliances. The EC-M-P (P for pluggable) is a variant of the EC-M that supports pluggable SR and/or LR optics. EC-M-B (B for bypass) is the new name for the EC-M.|8.1.7.3|
|IPv6 Support for Inline Router Mode|• ECOS 8.1.7 adds IPv6 support to inline router mode deployments.|8.1.7.0|
|BGP Enhancements|ECOS 8.1.7 adds the following enhancements to BGP routing: • Soft-Reset: Soft reset is a manual trigger to request a BGP route update from a BGP peer • Input Metric: Input metric provides the capability to change the BGP learned metric before the route is added to the routing table BGP Communities: Locally learned BGP communities are carried over the Silver Peak fabric to remote Silver Peak peers and advertised to the remote BGP neighbors|8.1.7.0|
|Inbound Port Forwarding|ECOS 8.1.7 adds inbound port forwarding in order to allow reachability to LAN-side branch devices from the WAN. Inbound port forwarding rules can be added via the Orchestrator: Configuration > Inbound Port Forwarding. Inbound port forwarding is limited to one hundred rules.|8.1.7.0|


-----

|Feature|Description|Earliest Release with Feature|
|---|---|---|
|Shaper Enhancements|ECOS 8.1.7 adds the capability to automatically rebalance the shaper’s bandwidth should any of outbound WAN links become unavailable. To enable this feature, select the “Rebalance for Available Interfaces” checkbox on Configuration > Shaper. Additionally, shaper minimum and maximum bandwidths can now be configured both as absolute values and relative percentages.|8.1.7.0|
|TCP MSS Clamping for Internet Breakout|ECOS 8.1.7 introduces TCP MSS clamping in order to prevent fragmentation (and potentially increased latency or dropped packets) for internet breakout traffic on networks with lower MTUs. To enable TCP MSS clamping, set the Maximum TCP MSS on the Configuration > System page.|8.1.7.0|
|Flow Redirection on WAN Interfaces|ECOS 8.1.7 adds support for flow redirection on WAN interfaces.|8.1.7.0|
|TCP Acceleration for IPv6|ECOS 8.1.7 adds support for TCP acceleration of IPv6 traffic.|8.1.7.0|
|Cloud-Init|ECOS 8.1.7 adds Cloud-Init support to EdgeConnect Virtual (EC-V). When EC-V is used as a VNF (Virtual Network Function), Cloud-Init loads startup configuration on first boot. This configuration typically contains management IP address / mask, default route, interface MAC addresses, tenant’s account name, key etc.|8.1.7.0|
|OSPF|ECOS 8.1.7 introduces OSPF routing to allow Silver Peak’s SD-WAN subnet-sharing protocol to share routes with traditional WAN routers. ECOS 8.1 can advertise routes to traditional routers as well as learn routes from traditional routers. The primary use-cases supported are: 1. Advertisement of Silver Peak SD-WAN subnets into an existing data-center router for the purposes of allowing traditional branch routers to gain reachability to Silver Peak SD-WAN branches. 2. Learning branch routes from an existing large branch router that is subtending many subnets. OSPF support is enabled via Configuration > OSPF.|8.1.7.0|
|Edge High Availability|ECOS 8.1.6 introduces Edge High Availability. Edge High Availability is a unique high availability architecture that enables each appliance in an HA pair to terminate only one WAN link while still providing automation via Business Intent Overlays and link resiliency via tunnel bonding.|8.1.6.0|
|IPSec UDP Overlays|ECOS 8.1.6 introduces IPSec UDP overlays. IPSec UDP overlays are more deterministic and reliable than traditional IPSec Overlays. IPSec UDP overlays must be configured via the Orchestrator.|8.1.6.0|
|Mini License|ECOS 8.1.6 introduces the Mini license which supports up to 50Mbps of SD-WAN. Please note that Plus cannot be applied to a Mini license.|8.1.6.0|


-----

|Feature|Description|Earliest Release with Feature|
|---|---|---|
|Application Visibility and Control Enhancements|ECOS 8.1.6 introduces new Application Groups that provide more flexible grouping of applications than was previously possible. Further, over 200 groups are pre-defined and most data-center and SaaS applications are pre-assigned into groups. • Upon upgrade to 8.1.6 existing Application Groups can be migrated to the new Application Groups.|8.1.6.0|
|Configurable BGP Parameters|ECOS 8.1.6 adds configuration of the following BGP peer parameters: • Local Preference • MED (Multi-Exit Discriminator) • AS Prepend Count • Keep Alive Timer Hold Timer|8.1.6.0|
|Configurable Interface for SaaS Probes|ECOS 8.1.6 allows configuration of the physical interface over which SaaS Optimization probes are sent.|8.1.6.0|
|Modified High Efficiency Bonding|ECOS 8.1.5 modifies the behavior of High Efficiency bonding to maintain equal percentage utilization of all links.|8.1.5.3|
|EC-US|ECOS 8.1.5 introduces support for the Unity EdgeConnect US appliance. The Unity EdgeConnect US is an ultra-compact form factor, thin edge appliance that serves to build an SD-WAN fabric using Zero Touch Provisioning. It supports MPLS, 4G/LTE and internet-based hybrid WAN data paths and a control plane that is automated and secured by the Unity Orchestrator software, providing policy-based virtual network segmentation and acceleration of on-premise and SaaS cloud applications. A key feature of EdgeConnect US is silent, fanless operation, making it an ideal platform for small branch and home office environments with typical WAN bandwidth up to 100 Mbps.|8.1.5.3|
|IP SLA|ECOS 8.1.5 introduces IP SLA tracking that allows you to perform actions according to the state of certain monitored objects. The currently available monitors are interface, IP ping, and VRRP state. The current actions are enable/disable tunnel, decrease/increase VRRP priority, and modify subnet metrics. IP SLA can be configured via “Configuration > IP SLA”.|8.1.5.0|
|PPPoE Interfaces|ECOS 8.1.5 introduces support for PPPoE (Point-to-Point Protocol over Ethernet). PPPoE can be configured via “Configuration > PPPoE”.|8.1.5.0|
|New “Interfaces” Configuration Page|“Configuration > Interfaces” has been redesigned for simplicity and clarity.|8.1.5.0|
|Improved Application Classification by Port|Port-based application classification is now dynamically updated from the Silver Peak cloud portal via the published IANA port list.|8.1.5.0|
|Per-Flow Maximum Rate Control|ECOS 8.1.5 introduces a new QoS parameter – per-flow maximum rate control.|8.1.5.0|


-----

|Feature|Description|Earliest Release with Feature|
|---|---|---|
|DNS Application Classification Enhancement|DNS Application Classification now extracts domain information from HTTP get responses.|8.1.4.0|
|Internet Breakout with Stateful Firewall and NAT|ECOS 8.1.4 introduces internet breakout with stateful firewall. Internet breakout is enabled via Configuration > Tunnels.|8.1.4.0|
|Fine Grained Control of Management Traffic|ECOS 8.1.3 adds fine-grained control of appliance management plane traffic.|8.1.3.0|
|Enhanced Application Visibility|ECOS 8.1 greatly enhances application visibility with IP intelligence, domain name classification, and automatic classification of RTP traffic. ECOS 8.1 also adds wild-card matching (e.g. “*Netflix*) in match criteria.|8.1.0.0|
|BGP Routing|ECOS 8.1 introduces BGP routing to allow Silver Peak’s SD-WAN subnet-sharing protocol to share routes with traditional WAN routers. ECOS 8.1 can advertise routes to traditional routers as well as learn routes from traditional routers. The primary use-cases supported are: 1. Advertisement of Silver Peak SD-WAN subnets into an existing data-center router for the purposes of allowing traditional branch routers to gain reachability to Silver Peak SD-WAN branches. 2. Learning branch routes from an existing large branch router that is subtending many subnets. BGP support is enabled via Configuration > BGP.|8.1.0.0|
|Interface Bonding on 10Gbps Ports|ECOS 8.1 introduces interface bonding for 10Gbps interfaces. This feature is only available on the EdgeConnect-XL, NX-10700, and NX- 11700 appliances. When 10Gbps interface bonding is enabled, the bonded pairs are blan0 (tlan0/tlan1) and bwan0 (twan0/twan1). Interface bonding is enabled via Configuration > Deployment.|8.1.0.0|
|IPv6|ECOS 8.1 introduces IPv6 UDP, GRE, and IPSec tunnels.|8.1.0.0|
|SHA-2 Hash for IPSec|ECOS 8.1 introduces SHA-2 hash support for IPSec tunnels. SHA-2 hash support is enabled via Configuration > Tunnels [Advanced Options].|8.1.0.0|
|Extended DHCP Server Options|ECOS 8,1 extends the DHCP Server feature to support all options in RFC 1533.|8.1.0.0|
|SNMPv3 Enhancements|ECOS 8.1 extends SNMPv3 support by adding support for traps and supporting multiple SNMPv3 users, each with their own authentication/privacy settings.|8.1.0.0|
|Custom https Certificates for Appliance Management|ECOS 8.1 supports the addition of custom https certificates for appliance management, Custom certificates can be uploaded via Administration > HTTPS Certificate Upload.|8.1.0.0|
|Interface Flexibility for Flow Redirection|ECOS 8.1 allows any configured physical interface to be utilized for Flow Redirection.|8.1.0.0|


-----

|EC-M|200969|
|---|---|
|NX-2700|201020|
|NX-3700|201021|
|NX-5700|201022|
|NX-6700|201023|
|NX-7700|201024|

|Feature|Description|Earliest Release with Feature|
|---|---|---|
|Return Pass-Through Traffic to L2 Sender|ECOS 8.1 allows pass-through L2 redirected traffic to be sent to the original (forwarding) router instead of the WAN-side next hop. This feature is disabled by default and can be enabled via Configuration > System [Always send pass-through traffic to original router].|8.1.0.0|
|Support for additional hardware appliance part numbers.|ECOS 8.0 adds support for the following part numbers. EC-M 200969 NX-2700 201020 NX-3700 201021 NX-5700 201022 NX-6700 201023 NX-7700 201024|8.0.3.0|
|Business Intent Overlays|ECOS 8.0 introduces Business Intent Overlays. Business Intent Overlays virtualize all underlying transports and segment the WAN allowing for different policies to be applied per application or application group. Business Intent Overlays are described at a high-level and are applied enterprise wide. The components of the Overlays include the access policy, logical topology, link bonding and QoS policies. Business Intent Overlays are deployed and managed by the Unity Orchestrator.|8.0.0.0|
|Deployment Profiles|ECOS 8.0 introduces Deployment Profiles to abstract the personality of EdgeConnect devices. Deployment Profiles assign labels with global (SD-WAN fabric-wide) semantics to the underlying physical transports (for example, “MPLS” and “Internet”). Deployment Profiles can be applied at the time a new EdgeConnect device is Zero Touch Provisioned and ensure consistent configuration of network policies without configuration drift due to manual box-by-box configuration. Deployment Profiles are created and managed by the Unity Orchestrator.|8.0.0.0|
|Packet-Based DPC|ECOS 8.0 introduces Packet-Based Dynamic Path Control also known as “bonded tunnels.” Bonded tunnels form a virtual transport that combines one more underlying physical transports (for example, MPLS and Internet) into one logical “pipe.” Bonded tunnels allow the configuration of a bonding policy that can either emphasize transmission resiliency in the face of transmission errors and losses or favor maximum throughput by load balancing packet data. Bonded tunnels also allow for the specification of blackout or brownout SLA policies that dictate when data are sent over a backup physical transport instead (for example, 4G LTE). Bonded tunnels form the data plane of the Business Intent Overlays and therefore each application or category of applications can specify its own independent bonding policies. Bonded tunnels are deployed and managed by the Unity Orchestrator.|8.0.0.0|


-----

|Feature|Description|Earliest Release with Feature|
|---|---|---|
|DHCP Server/Relay|ECOS 8.0 introduces built-in DHCP server and relay capabilities. The DHCP server or relay function can be configured per interface i.e. any combination of physical port and/or VLAN. DHCP Server/Relay configuration can be applied by the Unity Orchestrator from the Deployment Profiles screen or performed manually using the CLI on an EdgeConnect device.|8.0.0.0|
|Inbound QoS|ECOS 8.0 introduces support for inbound (ingress) QoS. Inbound QoS can be enabled via Configuration > [System & Networking] Shaper.|8.0.0.0|
|IPv6|ECOS 8.0 introduces support for PBR router mode deployments.|8.0.0.0|


-----

#### Issues Fixed from Past Releases
###### The following table contains issues fixed in past releases that are also included in ECOS 9.2.11.1, organized by the software version that first resolved them.

|Issue|First Release to Resolve|
|---|---|
|ID: 75446. A corrupted SSL record caused a decryption error during SSL acceleration.|9.2.11.0|
|ID: 75358. An issue with how parallel datapath CPUs handle message queues going to network memory created a datapath conflict that caused the appliance to reboot unexpectedly.|9.2.11.0|
|ID: 75321. On platforms where the core is shared between datapath and IDS/IPS, a tasklet that receives events from the IDS/IPS was creating extra UDS sockets, causing a decrease in datapath performance.|9.2.11.0|
|ID: 75285. A validation issue with string store data in the DNS revmap API caused tunneld to reboot unexpectedly.|9.2.11.0|
|ID: 75019. A corrupted SSL handshake packet created an “invalid handshake” error that caused tunneld to reboot unexpectedly.|9.2.11.0|
|ID: 74625. A memory leak in ICMP error state processing of Firewall Protection Profiles caused tunneld to reboot unexpectedly.|9.2.11.0|
|ID: 74386. An error in the limit check function for the physical index of an interface prevented the acquisition of realtime stats from the appliance.|9.2.11.0|
|ID: 74310. An issue with lookup logic prevented the USB-LTE modem from connecting to certain ISPs.|9.2.11.0|
|ID: 74248. An internal hairpin flow failed to clear an NM flow index and caused tunneld to reboot unexpectedly.|9.2.11.0|
|ID: 73768. Adding DPI for RSSP apps created an issue in the parsing logic of DNS queries, resulting in reading bytes beyond packet boundary and causing tunneld to reboot unexpectedly.|9.2.11.0|
|ID: 73697. When the EC-XS appliance was onboarded from factory settings and rebooted before activation, new seeds sent to the appliance were not properly stored on disk.|9.2.11.0|
|ID: 73639. On the EC-XL-P appliance, a malformed payload sent over UDP port 53 should have been rejected by the DNS deep packet parser, but instead caused tunneld to reboot unexpectedly.|9.2.11.0|
|ID: 73626. Sanity check of the packet length parameter failed in certain scenarios, causing tunneld to reboot unexpectedly.|9.2.11.0|
|ID: 73466. A truncation issue in the internal timer module that occurred when the appliance’s uptime exceeded approximately 944 days caused a major slowdown in packet processing.|9.2.11.0|
|ID: 73097. An issue in the MGMT interface caused the mgmtd feature to sleep, triggering a restart in the ntpd service and un unexpected system reboot.|9.2.11.0|
|ID: 72796. A configuration issue in the shaper algorithm caused traffic to queue unexpectedly slowly, resulting in high CPU usage and diminished performance.|9.2.11.0|
|ID: 67953. On the EC-M-P appliance, the mrtrd process went into a pending state upon enabling multicast, rending multicast nonfunctional.|9.2.11.0|
|ID: 76024. Multi-hop routes were not checking to see if better nexthops were available, causing an asymmetric routing scenario that had to be resolved by rebooting the appliance.|9.2.10.5|
|ID: 74795. Virtual EdgeConnect running on KVM dropped packets on non-native VLAN interfaces due to improper handling of different MAC addresses assigned to different VLAN interfaces.|9.2.10.4|
|ID: 73030. An issue with DNS cache purging caused internet access to become unavailable when Proxy DNS was enabled.|9.2.10.3|
|ID: 72485. An error in the configuration file for the EC-XS appliance with part number 201571-001 or 201571-002 set the maximum number of routes in the routing table to an incorrect value.|9.2.10.3|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 72882. Virtual EdgeConnect appliance deployed on KVM processed packets that were not destined for a local MAC address.|9.2.10.2|
|ID: 74078. Upon upgrade to 9.2.10.0, an issue in the CPU configuration caused the EC-XLH10G appliance to reboot unexpectedly.|9.2.10.1|
|ID: 73628. An issue in nat map, security map, and ACL policy evaluation caused increased latency on the LAN/WAN-side nexthops.|9.2.10.1|
|ID: 72930. Timestamp values were stored in an incorrect location, causing nexthop to use incorrect definitions and resulting in iBGP routes dropping unexpectedly.|9.2.10.0|
|ID: 72623. The transmit amplitude was set too high by default on ports and was over-driving the CPU NIC ports, creating link stability issues.|9.2.10.0|
|ID: 72608. Some ISPs were unable to connect to the appliance because they were unable to use hardcoded APNs.|9.2.10.0|
|ID: 72454. When duplicating packets, the IP header compression preamble was not accounted for when calculating worst-case packet size.|9.2.10.0|
|ID: 72431. EdgeConnect’s BFD implementation erroneously detected faults when the BFD session did not transition into UP state and when the peer sent an “AdminDown” message.|9.2.10.0|
|ID: 72318. An error in the node callback chain resulted in the incorrect username appearing in audit logs for certain operations.|9.2.10.0|
|ID: 71495. An application classification issue caused inbound flows to be directed to a default security policy instead of another specified policy.|9.2.10.0|
|ID: 71389. A validation error created the potential for an unauthenticated user to execute arbitrary code on the host.|9.2.10.0|
|ID: 71380. An issue with port forwarding created the potential for an unauthenticated user to obtain a root shell on the appliance.|9.2.10.0|
|ID: 71361. A code sanitization issue created the potential for an unauthenticated user to execute arbitrary code on the appliance.|9.2.10.0|
|ID: 71313. An authentication issue with the admin user created the potential for a monitor user to gain admin CLI access through a web UI shell.|9.2.10.0|
|ID: 71083. An issue with the interface event handler caused the management layer of the appliance to become unresponsive when the USB LTE interface was put in admin down or unplugged.|9.2.10.0|
|ID: 70157. The link between the BGP route entries and nexthop entries became corrupted, causing an internal check to fail and tunneld to drop unexpectedly.|9.2.10.0|
|ID: 69866. A configuration issue caused domain-based policy lookup to take longer than expected, stalling the data path and generating lag for users.|9.2.10.0|
|ID: 69177. A race condition in the handling of a hairpin flow on multiple CPUs caused the appliance to reboot unexpectedly.|9.2.10.0|
|ID: 68775. An issue with the tunbug configuration file created a potential command injection vulnerability in the command line interface.|9.2.10.0|
|ID: 68258. Upon upgrade, flow redirection experienced intermittent failure with asymmetric flow registering because new sessions were being created on nodes with a “non-syn” flag.|9.2.10.0|
|ID: 68197. A validation error created the possibility for a malicious user to modify the prototype of a JavaScript object.|9.2.10.0|
|ID: 67215. If a security policy was added via Appliance Manager and previous Orchestrator rules existed, then the appliance repeatedly tried to apply the firewall zone policy.|9.2.10.0|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 72969. An issue with the payload for SSDP multicast packets in VRF caused tunneld to reboot unexpectedly.|9.2.9.3|
|ID: 72533. An issue with integer values in flow statistics resulted in flow count being reported as erroneously high.|9.2.9.2|
|ID: 71333. IPS drop action was incorrectly applied to flow-redirected packets.|9.2.9.0|
|ID: 71298. A buffer leak in the enqueue packet chain caused the appliance to reboot unexpectedly.|9.2.9.0|
|ID: 70892. A data path issue caused the appliance to reboot unexpectedly.|9.2.9.0|
|ID: 70881. An internal IPS state was not properly initialized, causing SSH sessions to drop intermittently.|9.2.9.0|
|ID: 70837. An issue with the location of PID files prevented the PPPoE interface of the appliance from getting an IP address upon reboot.|9.2.9.0|
|ID: 70830. Passive FTP was incorrectly identified in the Flows table.|9.2.9.0|
|ID: 70672. BGP was not redistributing routes to OSPF when the prefix was also advertised via the SD-WAN fabric.|9.2.9.0|
|ID: 70647. Session-Affinity was overriding overlay policy and choosing the incorrect internet passthrough for flows with the same IP pair.|9.2.9.0|
|ID: 70641. Additional LAN interfaces for each EdgeHA WAN interface and HA Sync connection that did not contain user traffic flows were not being excluded by default in NetFlow exports for EdgeHA sites.|9.2.9.0|
|ID: 70640. An issue with how nexthop is recognized produced some incorrect IP addresses in the nexthop table.|9.2.9.0|
|ID: 69544. An issue with string storage capacity caused some traffic to map to the incorrect overlay.|9.2.9.0|
|ID: 69351. An issue with egress handling of default passthrough packets routed to VTI tunnels caused flow redirected packets to drop unexpectedly.|9.2.9.0|
|ID: 68731. If an EdgeHA peer was unreachable, an issue with checking the socket status resulted in unnecessary CPU usage.|9.2.9.0|
|ID: 68640. An issue in IPsec anti-replay handling caused keepalive packet drops, causing tunnels to go down unexpectedly.|9.2.9.0|
|ID: 70580. An issue around RADIUS snooping packet handling caused the appliance to reboot unexpectedly.|9.2.8.0|
|ID: 70555. Cloudinit YAML configuration files were not read properly from the USB drive on bootup.|9.2.8.0|
|ID: 70554. An object handling error prevented the appliance node server from properly logging the GMS websocket, resulting in the node restarting unexpectedly.|9.2.8.0|
|ID: 70121. An encapsulation error after an IPsec rekey event caused IPSLA to time out.|9.2.8.0|
|ID: 69598. Flows not resetting automatically after hardware failover.|9.2.8.0|
|ID: 69531. A validation error in radius snooping caused the appliance to reboot unexpectedly.|9.2.8.0|
|ID: 69163. An uninitialized handshake buffer used to process the SSLv2 handshake caused the appliance to reboot unexpectedly.|9.2.8.0|
|ID: 69127. When duplicate routes were present, static routes were not being advertised to the BGP neighbor.|9.2.8.0|
|ID: 68693. Upon upgrade, an ESN synchronization issue caused tunnels to go down.|9.2.8.0|
|ID: 68654. When a peer priority was changed, the incorrect AS path was advertised to the LAN side of the appliance.|9.2.8.0|
|ID: 68519. Management interface info was not being sent to routerd after routerd restart when OSPF and/or BGP was enabled.|9.2.8.0|
|ID: 68130. The peer list did not strictly guarantee peer ID-to-peer index matches with appliances. This could cause traffic to be routed to an incorrect peer.|9.2.8.0|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 63821. An issue with authentication tokens resulted in appliances with the same security policy configuration displaying different behavior.|9.2.8.0|
|ID: 69595. An error in processing multicast IP fragments caused the appliance to reboot unexpectedly.|9.2.7.0|
|ID: 69088. A logic issue caused unexpected usage on LTE tunnels that were supposed to be idle.|9.2.7.0|
|ID: 68459. When BGP ECMP was enabled, both peers dropped unexpectedly, with only one recovering before reboot.|9.2.7.0|
|ID: 67768. A string in any custom IE exceeding 255 characters in length caused an encoding error.|9.2.7.0|
|ID: 66670. A route policy issue prevented Link Integrity tests on the WAN1 interface from completing successfully.|9.2.7.0|
|ID: 00542. When using a DNS proxy, the appliance changed the TC bit setting from 1 to 0, causing issues with the DNS query.|9.2.7.0|
|ID: 00512. On the EC-10104 appliance, a port initialization issue caused the admin status to appear incorrectly in the web UI.|9.2.7.0|
|ID: 00498. The EC-10104 appliance does not currently support bridge mode.|9.2.7.0|
|ID: 69293. In a rare case, interface measurements were incorrectly discarded, resulting in a suboptimal interface being chosen for internet breakout traffic.|9.2.6.0|
|ID: 69047. The appliance web server was restarted repeatedly after upgrading from 8.1.9.18 to 9.1.6.2, causing unstable connectivity to the Orchestrator.|9.2.6.0|
|ID: 68679. After the appliance was upgraded to 9.1.6.2_92636, the appliance restarted unexpectedly.|9.2.6.0|
|ID: 68655. BGP-learned routes were not being redistributed into OSPF and SD-WAN fabric.|9.2.6.0|
|ID: 68598. After ECMP is enabled for LAN-side BGP, the connection to the web UI rebooted intermittently.|9.2.6.0|
|ID: 68343. This release was patched to address CVE-2023-2650 (OpenSSL vulnerability).|9.2.6.0|
|ID: 68226. After appliances were upgraded to 9.1.4.2_92345 from 8.3.6.1_86378, idle tunnels on hub appliances began sending more data to spoke appliances.|9.2.6.0|
|ID: 68465. HTTP-based IPSLA probes were not sent to the intended Zscaler tunnel on appliances where segmentation was disabled and an overlay was configured to match traffic to the Zscaler VPN service.|9.2.5.0|
|ID: 68369. An issue with NAT allocation caused local internet breakout to improperly route NAT traffic.|9.2.5.0|
|ID: 68364. Flows from wireless access points were disconnected after a tunnel flap, requiring the customer to reset the flows.|9.2.5.0|
|ID: 68019. An anti-replay window de-synchronization issue in IPSec UDP tunnels caused underlay fabric tunnels to drop unexpectedly.|9.2.5.0|
|ID: 67997. A DNS cache issue caused some appliances to lose connectivity to Orchestrator.|9.2.5.0|
|ID: 67995. Under certain conditions, the peer index tracking a particular peer ID was reused with a different peer ID, resulting in the new peer erroneously showing it owned a different bonded tunnel.|9.2.5.0|
|ID: 67841. A concurrency issue between tunneld and VRRP process while accessing shared memory resources caused the appliance to reboot unexpectedly.|9.2.5.0|
|ID: 67484. The Inbound WAN lost count on some appliances was incorrect.|9.2.5.0|
|ID: 67422. An error during the packet sane check process created a packet double-free in the FPOC/REO engine, causing tunneld to reboot unexpectedly.|9.2.5.0|
|ID: 67908. An internal queue configuration error led to a flow leak in an external hairpin with segmentation.|9.2.4.0|
|ID: 67845. The Cluster Peer Down alarm was being raised too often, resulting in asymmetric flow redirection.|9.2.4.0|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 67473. A configuration issue stored incorrect Admin distance values in a helper data structure, resulting in BGP route loop prevention not functioning as expected.|9.2.4.0|
|ID: 67397. A misconfiguration in the cifs_proxy_monitor's select timer resulted in the proxy service rebooting unexpectedly.|9.2.4.0|
|ID: 67361. When segmentation was enabled, an IPv6 configuration error led to tunnel traffic erroneously appearing in application reports for EdgeHA appliances.|9.2.4.0|
|ID: 67296. When trying to establish BGP peering from a newly created VRF segment, the Peer State value appeared empty in the web UI.|9.2.4.0|
|ID: 67027. On the EC-10104 appliance, a routing error erroneously classified standard ports such as lan0 and wan0 as VLAN sub-interfaces, resulting in a configuration error that caused routerd to reboot unexpectedly.|9.2.4.0|
|ID: 67010. This release was patched to address CVE-2023-0215 (OpenSSL vulnerability).|9.2.4.0|
|ID: 67009. This release was patched to address CVE-2022-4450 (OpenSSL vulnerability).|9.2.4.0|
|ID: 67008. This release was patched to address CVE-2023-0286 (OpenSSL vulnerability).|9.2.4.0|
|ID: 67006. This release was patched to address CVE-2022-4304 (OpenSSL vulnerability).|9.2.4.0|
|ID: 66760. A command issue created a potential vulnerability where admin users could execute arbitrary code as the root user.|9.2.4.0|
|ID: 66644. The multicast engine was not recognizing or forwarding SSDP packets.|9.2.4.0|
|ID: 66641. The BGP peer was not establishing to the ISP neighbor on wan0.|9.2.4.0|
|ID: 66590. If no subnets were specified in VRF, then the "force-internal" flag was not set, causing a classification issue where traffic was improperly identified as internal.|9.2.4.0|
|ID: 66545. Subnet API documentation contained some missing fields, resulting in dropped API calls.|9.2.4.0|
|ID: 66433. A stale flow entry in the hash table caused IPsec tunnels to drop unexpectedly.|9.2.4.0|
|ID: 66398. A duplicate or overlay ID was causing a bonded tunnel to enter a misconfigured state.|9.2.4.0|
|ID: 66252. The jsond process stopped unexpectedly, causing the web UI to reboot.|9.2.4.0|
|ID: 66144. A configuration vulnerability allowed the potential upload of malicious files through the web UI.|9.2.4.0|
|ID: 66112. This release was patched to address CVE-2023-3053 (command execution vulnerability).|9.2.4.0|
|ID: 66024. If the appliance rebooted during the period between when active and passive seed-ids were pushed, the active seed-id was lost and tunnels dropped unexpectedly.|9.2.4.0|
|ID: 65904. A validation issue created a potential command injection vulnerability in the command line interface.|9.2.4.0|
|ID: 65888. ECOS interface changes caused the NTPD service to restart, in turn causing NTP to reboot unexpectedly.|9.2.4.0|
|ID: 65842. A validation issue prevented IPv6 addresses from being used in passthrough tunnel configuration.|9.2.4.0|
|ID: 63884. An invalid tunnel flow entry caused packets to be dropped.|9.2.4.0|
|ID: 63835. Default SSH options on the appliance were erroneously presenting as security issues during vulnerability scans.|9.2.4.0|
|ID: 62644. A file authentication issue created a potential vulnerability where admin users had read access to sensitive files.|9.2.4.0|
|ID: 62643. A command issue created a potential vulnerability where admin users had read access to sensitive files.|9.2.4.0|
|ID: 62609. A command issue created a potential vulnerability where admin users could execute shell commands as the root user.|9.2.4.0|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 59961. A network monitoring vulnerability allowed the potential execution of arbitrary shell scripts.|9.2.4.0|
|ID: 00447. Addresses CVE-2022-4304, CVE-2022-4450, CVE-2023-0215, and CVE-2023-0286.|9.2.4.0|
|ID: 72333. This release was patched to address CVE-2021-41617 (OpenSSH vulnerability).|9.2.3.0|
|ID: 66614. An authorization issue caused the EC-XS appliance to reboot unexpectedly.|9.2.3.0|
|ID: 66520. Errors in memory deletion and ruleset functionality caused incorrect IPS action to be performed.|9.2.3.0|
|ID: 66451. A configuration issue incorrectly allowed read/write permissions on SNMPv3.|9.2.3.0|
|ID: 65648. A change in the Linux kernel version used in ECOS 8.3 prevented LACP packets from being bridged properly, causing the neighboring switch to declare some interfaces as down.|9.2.3.0|
|ID: 57761. A packet handling issue in tunneld caused checksum verification errors.|9.2.3.0|
|ID: 72334. This release was patched to address CVE-2022-31676 (local privilege escalation vulnerability).|9.2.2.0|
|ID: 66174. A configuration database validation issue prevented successful upgrade of the appliance.|9.2.2.0|
|ID: 65828. A pre-configuration issue caused an unexpected loss of connectivity between the appliance and the Orchestrator.|9.2.2.0|
|ID: 65736. A subnet sharing issue caused OSPF routes to drop unexpectedly if there were no routes in the default segment.|9.2.2.0|
|ID: 64781. This release was patched to address CVE-2022-44533 (RCE vulnerability).|9.2.2.0|
|ID: 64342. An internal flow timer issue led to SYN packets being dropped unexpectedly.|9.2.2.0|
|ID: 63809. LAN RX and LAN TX bytes were present in some Netflow reports even when they were not configured to be.|9.2.2.0|
|ID: 63774. TCP traffic was moving slowly between the remote server on AWS and the on-premise appliance.|9.2.2.0|
|ID: 63651. An issue with how packet loss is calculated led to “Inbound WAN lost” and “Inbound WAN jitter counter” values that seemed excessive.|9.2.2.0|
|ID: 63598. ACL matching failed when an address group containing 1.1.1.1/32 was used.|9.2.2.0|
|ID: 63575. Packets were being dropped in FPOC for reaching the maximum buffer limit.|9.2.2.0|
|ID: 63574. The Overlay-Interface-Transport report was not generating when the Transport option was selected.|9.2.2.0|
|ID: 63289. Upon upgrade to newer versions, OSPF routes were experiencing unexpected interruptions due to the way the appliance was improperly deleting and then adding routes (instead of upgrading them).|9.2.2.0|
|ID: 63253. Changed passwords in BGP peer connections were not taking effect until the connections were restarted.|9.2.2.0|
|ID: 63243. After a spoke appliance was upgraded to 8.3.6.1, it routed traffic to unexpected hub peers. Rebooting it resolved the issue.|9.2.2.0|
|ID: 63114. TCA alerts remained active after tunnels were deleted.|9.2.2.0|
|ID: 63071. Accessing the precursor table from multiple datapath threads concurrently resulted in IP SLA tunnels going down unexpectedly.|9.2.2.0|
|ID: 63062. A configuration issue caused the appliance to not recognize SaaS Optimization settings if certain RTTs were set.|9.2.2.0|
|ID: 62883. After the appliance was upgraded to an ECOS 8.3.6.x version, fiber interfaces on some hardware models failed to come up.|9.2.2.0|
|ID: 62877. This release was patched to address CVE-2022-43542 (command injection vulnerability).|9.2.2.0|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 62864. Route status indicates Up when peer interface is Down.|9.2.2.0|
|ID: 62759. An assertion failure while processing a multicast packet caused the appliance to restart unexpectedly.|9.2.2.0|
|ID: 62722. The denylist was not functioning as expected for SD-WAN flows.|9.2.2.0|
|ID: 62707. This release was patched to address CVE-2022-37926 (cross-site scripting vulnerability).|9.2.2.0|
|ID: 62658. IKE IPsec tunnel traffic loss was observed during the rekey process.|9.2.2.0|
|ID: 62654. An issue in the IPsec anti-replay setting while changing a tunnel configuration caused the appliance to reboot unexpectedly.|9.2.2.0|
|ID: 62620. This release was patched to address CVE-2022-44532 (arbitrary file read vulnerability).|9.2.2.0|
|ID: 62608. This release was patched to address CVE-2023-30501 (command injection vulnerability).|9.2.2.0|
|ID: 62607. This release was patched to address CVE-2022-43541 (command injection vulnerability).|9.2.2.0|
|ID: 62593. On the Interface Summary tab, inbound firewall log drops appear blank even if there have been drops.|9.2.2.0|
|ID: 62929. This release was patched to address CVE-2023-30507 (read/write vulnerability).|9.2.1.0|
|ID: 62884. Flow-redirected flows were not deleted fast enough, causing the number of redirection entries to grow too large and eventually resulting in flow redirection failure.|9.2.1.0|
|ID: 62783. Adding 0.0.0.0/0 to the list of internal subnets did not account for non-default segments.|9.2.1.0|
|ID: 62736. This release was patched to address CVE-2023-30510 (server-side request forgery vulnerability).|9.2.1.0|
|ID: 62670. This release was patched to address CVE-2022-37923 (command injection vulnerability).|9.2.1.0|
|ID: 62668. This release was patched to address CVE-2022-37922 (command injection vulnerability).|9.2.1.0|
|ID: 62666. This release was patched to address CVE-2022-37921 (command injection vulnerability).|9.2.1.0|
|ID: 62486. The main packet handling process became unresponsive and caused the appliance to reboot unexpectedly.|9.2.1.0|
|ID: 62481. When a label was used for an action interface, VRRP priority was not decreased for all VRRP instances.|9.2.1.0|
|ID: 62472. A control message buffer depletion prevented routes from being distributed properly in subnet sharing.|9.2.1.0|
|ID: 62323. Upon upgrade, a UI issue prevented real-time stats from rendering correctly in the Orchestrator Live View.|9.2.1.0|
|ID: 62304. After upgrading the appliance, pings were not being successfully relayed between HA interfaces.|9.2.1.0|
|ID: 60916. In some cases, the appliance was unexpectedly reverting to previous bandwidth license limits.|9.2.1.0|
|ID: 62573. This release was patched to address CVE-2022-43518 (arbitrary file read vulnerability).|9.2.0.0|
|ID: 62480. An error in sequence number handling caused the packet daemon to become unresponsive, resulting in an unexpected appliance reboot.|9.2.0.0|
|ID: 61905. A race condition during Orchestrator update was causing conflicts in API calls, resulting in SaaS optimization running for sites that did not have it enabled.|9.2.0.0|
|ID: 61876. A UI issue was incorrectly validating invalid distance values in Orchestrator.|9.2.0.0|
|ID: 61781. Scans running against EdgeConnect were causing appliances to be intermittently grayed out in Orchestrator.|9.2.0.0|
|ID: 61715. The appliance was patched to address CVE-2022-25236 (expat advisory).|9.2.0.0|
|ID: 61612. A logic issue in the processing of static routes erroneously created multiple route entries but did not mark them as duplicates, causing a conflict and unexpected reboot of the appliance.|9.2.0.0|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 61264. The appliance was patched to address CVE-2022-0778 (OpenSSL advisory).|9.2.0.0|
|ID: 61142. Some sub-interfaces were using the same interface name, creating issues in public IP discovery.|9.2.0.0|
|ID: 60920. AWS URLs were not successfully being added on the Application Definition page.|9.2.0.0|
|ID: 60894. A validation issue was allowing invalid admin distance values for OSPF routes.|9.2.0.0|
|ID: 60702. Zscaler GRE tunnels were unexpectedly going offline.|9.2.0.0|
|ID: 60192. Yocto mii-tool output was displaying advertising and link partner fields backwards.|9.2.0.0|
|ID: 60125. Some appliances were unexpectedly switching to System Bypass mode and then recovering within a short time.|9.2.0.0|
|ID: 59496. On the flow details page, Configured Tx Action was not being updated in all conditions, and the Subnet field was not being displayed when there was no route available.|9.2.0.0|
|ID: 59166. iperf3 running in UDP mode on LAN was being capped by the WAN inbound shaper.|9.2.0.0|
|ID: 58892. If IP fragments were received with DF bit set, they would be dropped by the far-end appliance across the SD-WAN fabric.|9.2.0.0|
|ID: 67175. An invalid redirect flow caused tunneld to reboot unexpectedly.|9.1.5.0|
|ID: 65931. A static route with unreachable NextHop was still advertised by the appliance.|9.1.5.0|
|ID: 66312. A socket leak in the RADIUS module caused a connectivity error when logging in to the appliance from the web UI.|9.1.5.0|
|ID: 66251. When an IPSEC DPD timeout originated from transient WAN packet loss, third-party IPSEC tunnels would erroneously stay up on the appliance, requiring manual recovery of the tunnel.|9.1.5.0|
|ID: 66678. A validation error caused the HASync peer to go down, forcing tunneld to reboot unexpectedly.|9.1.4.4|
|ID: 66294. A data corruption error led to the appliance rebooting unexpectedly when receiving a flow redirection bulk delete request.|9.1.4.4|
|ID: 65945. During IPsec rekey, active SAs were being deleted erroneously, causing packet drops.|9.1.4.4|
|ID: 62479. The EC-XL-H appliance rebooted unexpectedly while testing flow redirection.|9.1.4.4|
|ID: 66824. An issue with a high rate of concurrent flows caused the current number of flows to be incorrect.|9.1.4.3|
|ID: 66744. The EC-10104 appliance had no IP configured by default for lan0.|9.1.4.3|
|ID: 66634. A proxy manager deadlock condition in the appliance caused the appliance to restart.|9.1.4.3|
|ID: 66613. When the flow redirection feature was enabled, the counter for internal packet drops with the “LAN Rx q full” drop code continued incrementing.|9.1.4.3|
|ID: 66478. If there are multiple hosts on the WAN-side of the router, the appliance may have issues accessing some external URLs.|9.1.4.3|
|ID: 66458. A memory issue created a tunneld error and caused the appliance to reboot unexpectedly. This issue was given a temporary fix in a previous release. This update provides a permanent fix.|9.1.4.3|
|ID: 66444. A counting issue caused packet order correction buffer limit to max out, causing unexpected packet drops.|9.1.4.3|
|ID: 66416. An issue with the curl library in OpenSSL caused the system daemon to restart unexpectedly.|9.1.4.3|
|ID: 66378. An issue with flow deletion caused the appliance to reboot unexpectedly. The root cause is still unknown. In this release, additional debugging code was added to capture additional information when this issue happened.|9.1.4.3|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 66143. A race condition stemming from a flow issue in the inbound shaper caused the appliance to reboot unexpectedly.|9.1.4.3|
|ID: 66333. The Link Integrity test was not properly executing after upgrade.|9.1.4.2|
|ID: 63692. An issue with CIFS acceleration disrupted tunneld function, causing the appliance to reboot unexpectedly.|9.1.4.2|
|ID: 62110. During EC-V deployment, Cloud-init failed to configure Account Name if it contained a comma.|9.1.2.0|
|ID: 62081. After a subnet configuration change, mgmt0 interface became unstable.|9.1.2.0|
|ID: 61789. An internal error in TCP flow management caused the appliance to reboot unexpectedly.|9.1.2.0|
|ID: 61778. The appliance was incorrectly reporting model information on SNMP discovery.|9.1.2.0|
|ID: 61695. A routing issue involving multiple ECMP paths was causing unexpected interruptions to multicast traffic.|9.1.2.0|
|ID: 61254. The appliance was unable to configure an IPV6 address due to an error in subnet validation.|9.1.2.0|
|ID: 60968. Upon upgrade, a provisioning issue prevented Azure waagent from starting as expected.|9.1.2.0|
|ID: 60955. The appliance was not able to renew the license from the Cloud Portal.|9.1.2.0|
|ID: 60935. Excessive OSPF logging was causing performance issues on the appliance.|9.1.2.0|
|ID: 60891. DHCP relay was sometimes not working due to mishandling of interface labels.|9.1.2.0|
|ID: 60875. When the user clicked the Soft Reset button, a BGP route refresh message was not sent to the BGP peer as expected.|9.1.2.0|
|ID: 60694. The DHCP client was incorrectly handling the timeout state.|9.1.2.0|
|ID: 60156. The appliance failed to route some user traffic terminating on a VRRP interface with segmentation enabled due to a default route that was not properly installed.|9.1.2.0|
|ID: 59552. Tunnels were incorrectly reporting latency measurements when the tunnels were in an idle state.|9.1.2.0|
|ID: 59162. An issue in how tunnel bandwidth usage was calculated prevented HT link bonding policy from working as expected.|9.1.2.0|
|ID: 59068. Appliance CLI access was not working from Orchestrator.|9.1.2.0|
|ID: 62544. A configuration change for the tunnel retry timer caused underlay tunnels to go down unexpectedly.|9.1.1.3|
|ID: 62289. An error in hair-pinned packet processing caused the appliance to reboot unexpectedly.|9.1.1.3|
|ID: 62201. When a port-based application definition includes TCP/UDP port 65535, it causes an internal variable overflow that results in perpetual reboot of the appliance.|9.1.1.3|
|ID: 61728. When an “OR” operator was used in a firewall rule’s matching criteria, the rule was not matched and, as a result, packets were incorrectly denied.|9.1.1.3|
|ID: 61240. Certain flow state variables caused an issue where flow reclassification was not triggered as expected.|9.1.1.3|
|ID: 60927. The appliance was throwing the error “appliance could not get license lease” even though it could reach the Cloud Portal.|9.1.1.3|
|ID: 66092. An issue with the DNS snooping cache caused high CPU utilization, causing user traffic slowdown.|9.1.1.2|
|ID: 61193. When an ACL rule contained more than one group name, it incorrectly matched some flows, resulting in the flows being dropped.|9.1.1.2|
|ID: 60892. DHCP relay was sometimes not working due to mishandling of PPP interfaces.|9.1.1.2|
|ID: 60786. LAN to LAN traffic did not work without configuring a static route on the outgoing LAN interface.|9.1.1.2|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 60745. After upgrading ECOS, some packets related to Microsoft Office were being dropped internally.|9.1.1.2|
|ID: 61069. The appliance failed to forward Radius response messages due to a race condition in an internal queue.|9.1.1.1|
|ID: 60638. In some use cases, tunnel's quiescent state was misconfigured, prompting tunnels to send too many keepalive packets and causing excessive LTE data usage.|9.1.1.1|
|ID: 60668. BGP multihop peers were configured from 169.254.0.0/16, resulting in unexpected NLRI prefix import failure.|9.1.1.0|
|ID: 60649. During route redistribution, prefixes of larger prefix length were being incorrectly matched with routes of smaller prefix length.|9.1.1.0|
|ID: 60637. A race condition in an internal queue caused the appliance to stop sending PIMv2 Hello messages.|9.1.1.0|
|ID: 60623. Some vrf-names were not properly updated in DHCP failover settings when tagged interfaces were not part of the default vrf.|9.1.1.0|
|ID: 60529. A WAN routing policy was not marking LAN SNAT flows as internet traffic, resulting in the flows being dropped.|9.1.1.0|
|ID: 60445. When IPsec pre-shared keys were changed, EdgeConnect was causing unexpected reboots due to a race condition in the encryption code.|9.1.1.0|
|ID: 60409. Improperly set ack_by,clear_by parameters caused an unexpected reboot when the appliance raised an alarm.|9.1.1.0|
|ID: 60361. An issue with incorrectly setting an interface index resulted in ARP not getting resolved on HA control link VLAN interfaces.|9.1.1.0|
|ID: 60280. Some SNMP traps were still being received for alarms that were no longer active and had been cleared on the appliance.|9.1.1.0|
|ID: 60236. When specific appliance models were configured for traditional HA, an issue with packet validation for ICMP/traceroute traffic was causing the appliances to reboot unexpectedly.|9.1.1.0|
|ID: 60204. An issue with BGP continuous route updates was triggered by default route received.|9.1.1.0|
|ID: 60166. For certain appliance models, using the Configuration History to compare configuration records was generating an error.|9.1.1.0|
|ID: 60141. EdgeConnect was rebooting due to memory corruption resulting from incorrect handling of redirected flows.|9.1.1.0|
|ID: 60083. A recommendation for setting the HTTP IPSLA "Request Timeout" value smaller than the "Inactive Flow Aging Interval" was missing from the help text.|9.1.1.0|
|ID: 60078. Security policy caused SaaS return traffic for hair-pinned flows to drop unexpectedly.|9.1.1.0|
|ID: 60076. Trying to process an FTP payload caused an unexpected reboot in certain EdgeConnect devices.|9.1.1.0|
|ID: 60071. The reply to external traffic that was allowed to pass using a port forwarding rule was being sent to the incorrect WAN interface and dropped.|9.1.1.0|
|ID: 59945. An unexpectedly high sequence number packet was causing a problem with the FPOC engine, causing the appliance to reboot unexpectedly with no snapshot generated.|9.1.1.0|
|ID: 59944. On appliances with two PPPoE interfaces, the appliance stopped sending IPSLA traffic to the correct interface after one of the interfaces went down.|9.1.1.0|
|ID: 59927. After upgrading to 8.3.0.x, the bonding configuration on the EC-M-H appliance was changing from copper (wan0/wan1, lan0/lan1) to fiber (twan0/twan1, tlan0/tlan1).|9.1.1.0|
|ID: 59910. SHA256 and SHA512 were not supported on the EdgeConnect SSH client.|9.1.1.0|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 59883. Moving an interface from one segment to another segment caused the DHCP failover configuration to become invalid.|9.1.1.0|
|ID: 59806. When using branch NAT, a rule was setting the SNAT IP to the source IP, causing the appliance to reboot unexpectedly.|9.1.1.0|
|ID: 59794. If advanced segmentation was not enabled in CGNAT deployments, IPsec UDP tunnels and some PAT tunnels could enter a down state.|9.1.1.0|
|ID: 59744. Appliance generated spurious Disk SMART alarms.|9.1.1.0|
|ID: 59694. NTP alarms were reporting the NTP server as unreachable from the appliance even though the server was reachable.|9.1.1.0|
|ID: 59657. Traffic was being backhauled due to brownout limits, but appliance jitter, loss, and latency reports indicated no cause for brownout.|9.1.1.0|
|ID: 59629. TACACS+ authorization requests were not binding to the right source IP, causing them to go via mgmt0.|9.1.1.0|
|ID: 59582. A packet that should have been dropped due to a length mismatch was allowed to pass, causing the appliance to reboot unexpectedly.|9.1.1.0|
|ID: 59505. The GCM cipher configuration was blocking SSH to the appliance.|9.1.1.0|
|ID: 59412. When regional routing was enabled on a hub appliance, adding a local static route caused some prefixes received from spoke appliances to stop being advertised.|9.1.1.0|
|ID: 59347. Some appliances were unable to establish multiple IPsec tunnels toward Cisco Umbrella.|9.1.1.0|
|ID: 59147. Child SA ESP packets were sometimes dropped during phase2/IPsec rekeying in IKEv1 and IKEv2.|9.1.1.0|
|ID: 59131. Routes were being shared with BGP peers even though the outbound BGP route map did not permit it.|9.1.1.0|
|ID: 58267. When a tunnel went down and ICMP flows failed over to passthrough, they were not being reclassified even if the tunnel came back within the reclassification window.|9.1.1.0|
|ID: 58181. An issue with shared memory resources on an EC-V caused the appliance to reboot unexpectedly, resulting in VRRP failures.|9.1.1.0|
|ID: 56994. A match string validation error caused an issue syncing ACL/policy maps.|9.1.1.0|
|ID: 55397. Flows were not getting reclassified to a new peer when a better route having the same metric and different peer priority was found.|9.1.1.0|
|ID: 60105. Following the upgrade to 9.1, some locations experienced performance issues.|9.1.0.3|
|ID: 60451. Following the upgrade to 9.1, some locations experienced performance problems due to packet drops on the inbound shaper|9.1.0.2|
|ID: 59848. An issue with select system calls was causing CPU0 to be stuck at 100% on EdgeHA appliances.|9.1.0.2|
|ID: 59794. If advanced segmentation was not enabled in CGNAT deployments, IPSec UDP tunnels and some PAT tunnels were ending up in a down state.|9.1.0.1|
|ID: 59716. The default admin credentials on a new KVM EC-V were not working in some earlier versions of ECOS.|9.1.0.1|
|ID: 59397. OSPF peering was not working on the lan0 interface, and various troubleshooting steps failed to fix the issue.|9.1.0.0|
|ID: 59257. When adding a new security policy rule from the table view, an additional "deny everything" rule was being added. The new rule could be deleted from the matrix view but not from the table view.|9.1.0.0|
|ID: 59254. A BGP neighbor configured with a leftover interface from a previous configuration was causing memory issues that eventually caused the appliance to reboot unexpectedly.|9.1.0.0|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 59235. When using preconfig to deploy new sites with lan0 set as the DHCP server, DHCP settings were being removed from some appliances.|9.1.0.0|
|ID: 59112. File transfer throughput, via FTP or Windows file sharing, was getting throttled when both Advanced Segmentation and DNAT were configured.|9.1.0.0|
|ID: 59037. Following the upgrade to ECOS 9.0.2.6, two separate EC-Vs deployed in Azure were rebooting unexpectedly on a frequent basis.|9.1.0.0|
|ID: 58946. UDP DNS packets were being dropped at the firewall due to a bad checksum after SNAT had been performed.|9.1.0.0|
|ID: 58875. If a native interface was admin down and a VLAN interface was deployed on it, the appliance would fail to send ARP requests on that interface after it was brought up.|9.1.0.0|
|ID: 58183. The global number of buffers queued in FPOC were being exceeded, resulting in tunneld rebooting unexpectedly.|9.1.0.0|
|ID: 57683. The ip mgmt-ip command was allowing any IP as input without validating that the IP address was configured on the appliance.|9.1.0.0|
|ID: 57670. The total length of ACL rule entries was not being validated, resulting in issues when pushing new security policies.|9.1.0.0|
|ID: 57459. An appliance that had mgmt0 disconnected was generating a large amount of alarm emails because DHCP was continuously trying to gen an IP address for it.|9.1.0.0|
|ID: 57296. Cloud Orchestrator showed a top talker IP for a specific appliance with a lot of flows that was not showing up in flow details or TCP dumps.|9.1.0.0|
|ID: 57252. All EC-Vs with 8 GB or 12 GB of RAM and default system limits will continue to support 5000 and 10000 tunnels, respectively, but network memory allocation is reduced by 800 MB.|9.1.0.0|
|ID: 57116. During a file transfer between two sites, live view showed the backup link in red but the circuit had been up and available.|9.1.0.0|
|ID: 56968. Options to enable or disable validation of the Cloud Portal SSL certificate are now available in the Appliance Manager UI.|9.1.0.0|
|ID: 56943. DNS proxy self flows were not matching ACL rules with interface label match criteria.|9.1.0.0|
|ID: 56772. This release includes some code changes intended to block attackers from executing unauthorized shell commands.|9.1.0.0|
|ID: 56453. The monitor account had lost access to traceroute, ping, slogin, and other diagnostic commands.|9.1.0.0|
|ID: 56194. On an appliance with three OSPF peers, one of the peers was expectedly remaining in 2-way state, but the appliance was erroneously generating an alarm for it.|9.1.0.0|
|ID: 62320. The grub boot command line option for resetting to factory defaults failed to clear application definition-related files.|9.0.9.0|
|ID: 60307. With DNS proxy enabled on the EdgeConnect, certain systems did not register on the AD server with their domain name.|9.0.6.0|
|ID: 59132. After bringing up a new site, switches at multiple datacenters stopped receiving SD-WAN routes via OSPF, causing all sites to lose access to critical applications.|9.0.3.2|
|ID: 58947. After making changes to an overlay configuration, appliances were deleting and recreating the overlay and ending up with an incorrect configuration.|9.0.3.2|
|ID: 57799. After changing the IP of the internet interface with NAT turned on, the interface's public IP was not getting discovered.|9.0.3.0|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 57688. With advanced segmentation enabled, a route was not being removed from the routing table after the BGP peer stopped advertising the prefix.|9.0.3.0|
|ID: 57585. Orchestrator was failing to apply the TACACS+ configuration in the Management template on certain appliances.|9.0.3.0|
|ID: 57330. For appliances with uptime greater than one year, the calculation to number of days was being converted incorrectly for display in the appliance WebUI banner.|9.0.3.0|
|ID: 56999. A default route added to the route table via the GUI was not showing up in the CLI.|9.0.3.0|
|ID: 56955. A software issue encountered during the upgrade from ECOS 8.1.9.12 to 8.3.2.1 caused the appliance to become unresponsive.|9.0.3.0|
|ID: 56842. Following the migration from NX appliances, traffic in the Flows table was not displaying correctly.|9.0.3.0|
|ID: 56724. After upgrading to ECOS 8.3.0.x, a tunnel that had a space in its name would not come up.|9.0.3.0|
|ID: 56619. Enabling advanced segmentation and moving lan0 from the default segment caused the appliance to reboot unexpectedly.|9.0.3.0|
|ID: 56156. Routes were flapping on one appliance of an EdgeHA pair due to ARP failures for some neighbors.|9.0.3.0|
|ID: 55853. Sending an appliance bandwidth increase at the same time as a shaper bandwidth increase that exceeded the current maximum bandwidth was resulting in a 50 Gbps max auto bandwidth on a 1 Gbps circuit. In addition, the maximum license bandwidth was not enforced in the inbound shaper.|9.0.3.0|
|ID: 58261. When disabling BGP, a flag that indicated a route had been advertised was not being reset, so routes were not being advertised when BGP was enabled again.|9.0.2.5|
|ID: 57259. Packets chained together by the Shaper had been inbound from passthrough and bonded tunnels and only passthrough was expected, causing the appliance to reboot unexpectedly.|9.0.2.5|
|ID: 57491. An issue when handling multicast join/prune messages caused the appliance to reboot unexpectedly.|9.0.2.2|
|ID: 57123. DNS snooping did not properly handle domain names that might be mapped to distinct IP addresses based on the client IP address.|9.0.2.2|
|ID: 57000. In some cases, during an upgrade, multiple calls were being made to management processes for database query operations. This was creating a race condition, the result being that OSPF route map rules were not getting configured correctly.|9.0.2.2|
|ID: 56967. When a loopback interface was selected as the source interface for management services such as NetFlow/IPFIX, traffic associated with these services was getting routed incorrectly as passthrough.|9.0.2.2|
|ID: 56910. ECOS 9.0.2.0 had a memory leak in the multicast routing process.|9.0.2.1|
|ID: 56881. The default route in the advanced segmentation route table was missing, causing connectivity issues with an appliance in a specific segment.|9.0.2.1|
|ID: 56877. Following a route change from a remote appliance, a tunnel mismatch on an inbound packet caused the appliance to reboot unexpectedly.|9.0.2.1|
|ID: 56791. In a case where two firewalls were sending packets to one another at the same time, the advanced segmentation code was dropping inbound packets.|9.0.2.1|
|ID: 56707. The interfaces page was displaying incorrect segment information when VLANs were in use.|9.0.2.0|
|ID: 56580. A default route learned from an eBGP peer was not always advertised to the SD-WAN fabric.|9.0.2.0|
|ID: 56164. Appliance rebooted due to a mishandling of sequence number wraparound in the packet order correction function.|9.0.2.0|
|ID: 56150. Previous ECOS versions failed to populate RTM and subnet route table with AWS TGW eBGP prefix updates.|9.0.2.0|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 56141. BGP sessions were not coming up with VTI in a non-default segment.|9.0.2.0|
|ID: 55993. An RTCP (RTP Control Protocol) connection was not properly classified when it was established using SIP.|9.0.2.0|
|ID: 55933. Compound applications were not matching correctly when using a hyphen to indicate an IP range.|9.0.2.0|
|ID: 55806. When advanced segmentation was enabled, enabling Boost with IP Header Compression disabled was causing udp flows to fail with invalid checksums.|9.0.2.0|
|ID: 55798. A path characterization packet was mishandled when it was received after the corresponding tunnel had been deleted, causing the appliance to reboot.|9.0.2.0|
|ID: 55739. Hub site tunnels were stuck in "MTU discovery," which caused a network outage.|9.0.2.0|
|ID: 55692. There were memory leaks associated with certain tunnel configuration steps, causing system memory depletion and eventually appliance reboot.|9.0.2.0|
|ID: 55663. ECOS stats from the Orchestrator Rest API were returning interface stats and details but the label but the label wasn't showing up.|9.0.2.0|
|ID: 55586. At times, when MySQL traffic was being sent over the Default BIO, WAN latency was spiking and causing application issues.|9.0.2.0|
|ID: 55500. After adding back an interface that had been deleted more than 100 days earlier, packets were getting held up in a shaper queue, resulting in a LAN Rx buffer shortage.|9.0.2.0|
|ID: 55352. Updated the SSH library to address a number of CVEs.|9.0.2.0|
|ID: 55181. The WebSocket connection to Orchestrator was being closed incorrectly after receiving a delayed close event from the kernel TCP stack.|9.0.2.0|
|ID: 55073. After deployment with ECOS 8.3.1.x, the lan3 and wan3 interfaces on an EC-V were down and required manual intervention to make operational.|9.0.2.0|
|ID: 54959. RADIUS/TACACS+ were not available under Management Services.|9.0.2.0|
|ID: 54593. Appliance rebooted when it hit an ACL configuration error in the configuration database during initialization.|9.0.2.0|
|ID: 55830. If advanced segmentation was enabled, errors were being generated when trying to add more than 10 BGP peers from the routing segmentation page.|9.0.1.1|
|ID: 55728. Following the upgrade to ECOS 9.0.1.0, BGP sessions to AWS Transit Gateway stopped working.|9.0.1.1|
|ID: 55555. When deleting many BGP/OSPF routes learned from a peer subnet update, some routes were left undeleted but marked as pending deletes. This was causing affected appliances to show as DOWN for remote SD- WAN routes.|9.0.1.0|
|ID: 55203. ssh was listening on all interfaces even though it was enabled only on loopback.|9.0.1.0|
|ID: 54980. Internet flows were performing poorly and there was an increase in packet decode errors when Boost was enabled.|9.0.1.0|
|ID: 54956. BGP routes were flapping at multiple sites in different locations at the same time.|9.0.1.0|
|ID: 54928. With advanced segmentation enabled, OSPF peers were not coming up unless BGP was enabled in the default segment.|9.0.1.0|
|ID: 54926. Multicast pimX interface IP addresses were being advertised to OSPF/BGP peers, requiring users to add route-filtering rules to OSPF and BGP outbound maps to prevent advertisement of internal subnets used for multicast.|9.0.1.0|
|ID: 54373. DNS proxy was not supported with advanced segmentation enabled.|9.0.1.0|
|ID: 53538. Under heavy logging demand, some log messages were overwritten resulting in corrupted messages.|9.0.0.0|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 51178. Added more logging around tunnel stats to help in troubleshooting tunnel down/flapping issues.|9.0.0.0|
|ID: 48844. HA traffic (inbound flow) was being sent to LAN0 when the secondary appliance (LAN1) was powered down.|9.0.0.0|
|ID: 66201. The twan0 and twan1 interfaces did not link up as expected on the appliance.|8.3.8.0|
|ID: 59412. When regional routing was enabled on a hub appliance, adding a local static route caused some prefixes received from spoke appliances to stop being advertised.|8.3.4.0|
|ID: 59365. On an EC-V running in Azure, traffic was dropping every time an Azure endpoint triggered an IPSec rekey.|8.3.4.0|
|ID: 59304. An issue with improperly stored packet chain information was causing the appliance to reboot unexpectedly.|8.3.4.0|
|ID: 59076. Log messages were erroneously indicating that a static route had been deleted because the HA next hop.|8.3.4.0|
|ID: 59025. While testing inbound shaping, a "wrong library or version mismatch" error was seen and the appliance rebooted unexpectedly.|8.3.4.0|
|ID: 58965. A specific encapsulation scenario was causing packet validation to fail, resulting in the DNS proxy failing after enabling branch NAT.|8.3.4.0|
|ID: 58572. An issue with protocol classification was causing some custom application definitions to not be applied in flows.|8.3.4.0|
|ID: 58465. In rare cases, eBGP network layer reachability information was was not appearing in the route table as it was erroneously deleted.|8.3.4.0|
|ID: 58384. In some cases, breakout internet traffic was choosing a low bandwidth tunnel when it should have chosen a different tunnel.|8.3.4.0|
|ID: 58243. After an appliance is upgraded to 8.3.0.17, 8.3.4.0, or 9.0.3.0, and if image signature verification is enabled, only newer releases that are signed using SHA-256 can be installed on it.|8.3.4.0|
|ID: 58181. An issue with shared memory resources on an EC-V caused the appliance to reboot unexpectedly, resulting in VRRP failures.|8.3.4.0|
|ID: 58161. In some cases, when trying to enable SaaS Optimization, SaaS routes were not behaving as expected and traffic was getting bounced between the customer firewall and EdgeConnect appliances.|8.3.4.0|
|ID: 58076. An EC-V running with sufficient resources was not meeting 1 Gbps bi-directional performance specifications.|8.3.4.0|
|ID: 58064. Following the upgrade from ECOS 8.1.9.x, DHCP relay stopped working on a PPPoE interface while WAN interfaces continued to work as expected.|8.3.4.0|
|ID: 58062. If a SaaS route is learned from a remote appliance, traffic was always getting backhauled, regardless of AD in local vs. subnet-shared routes.|8.3.4.0|
|ID: 58020. If a SaaS route is learned from a remote appliance, traffic was always getting backhauled, regardless of AD in local vs. subnet-shared routes.|8.3.4.0|
|ID: 57957. DSCP markings were not included in flow details, making it hard to verify if per-interface DSCP marking was working as expected.|8.3.4.0|
|ID: 57945. In this release, LAN-side management traffic is no longer returned using built-in policy 65534. To enable management access to the appliance via a LAN-side data-plane interface (i.e., not mgmt0 or mgmt1), the appliance needs to have the best route matching the return traffic pointing to a LAN-side interface. This route can either be statically configured or learned from BGP or OSPF.|8.3.4.0|
|ID: 57900. Some appliances were logging messages that the RTP sessions table was full, so the size of the RTP sessions table has been increased.|8.3.4.0|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 57899. In the appliance manager UI and the CLI, SFP part number AFCT-739DMZ was showing up as not supported in an appliance where it should have been supported.|8.3.4.0|
|ID: 57867. In a configuration using Azure vWAN for passthrough tunnels and BGP, removing an interface label was causing a node process error and the appliance became unreachable.|8.3.4.0|
|ID: 57842. Appliances were not populating the remote address field (source IP) in TACACS+ authentication requests when a user entered enable mode.|8.3.4.0|
|ID: 57839. In some cases, the appliance was not sending BGP routes to a neighbor firewall when route maps were in use.|8.3.4.0|
|ID: 57825. Flows were not getting reclassified to a new peer when a better route having the same metric and different peer priority was found.|8.3.4.0|
|ID: 57809. One side of an unused backup tunnel was up/active, causing unexpectedly high LTE usage.|8.3.4.0|
|ID: 57587. Threshold crossing alerts were mistakenly being generated for internal nexthop tunnels, causing false alerts for exceeding 100% utilization.|8.3.4.0|
|ID: 57527. After upgrading to 8.3.0.11, an appliance was advertising SDWAN-learned BGP routes to a PE router even though the outbound rout map was set to DENY this.|8.3.4.0|
|ID: 57527. After upgrading to 8.3.0.11, an appliance was advertising SDWAN-learned BGP routes to a PE router even though the outbound rout map was set to DENY this.|8.3.4.0|
|ID: 57508. Following an unexpected reboot, device snapshots/sysdumps were being generated unnecessarily, resulting in connectivity issues to the appliance from Orchestrator and mgmt0.|8.3.4.0|
|ID: 57423. At times, breakout traffic was being directed to the backup link if the bandwidth threshold on the primary link had been exceeded instead of only when the primary link was down.|8.3.4.0|
|ID: 57397. The WebSocket connection to Cloud Portal was blocked by the firewall, resulting in an expired appliance license.|8.3.4.0|
|ID: 57342. If a SaaS route is learned from a remote appliance, traffic was always getting backhauled, regardless of AD in local vs. subnet-shared routes.|8.3.4.0|
|ID: 57327. In some cases, packets with a total header length (standard plus extension) that was too large were causing the appliance to reboot unexpectedly.|8.3.4.0|
|ID: 57299. Some bulk apps were using the default overlay because the application name was being overwritten with the snooped domain name regardless of the confidence value.|8.3.4.0|
|ID: 57291. A packet that should have been dropped due to a length mismatch was allowed to pass, causing the appliance to reboot unexpectedly.|8.3.4.0|
|ID: 57191. A security scan determined that hardened interfaces were responding to UDP requests on port 53.|8.3.4.0|
|ID: 57148. In some cases, an excess of DHCP discovery packets were causing excess logging. This resulted in too much CPU overhead and eventually caused the appliance to reboot unexpectedly.|8.3.4.0|
|ID: 56880. Appliances were incorrectly advertising link local addresses to BGP and OSPF peers.|8.3.4.0|
|ID: 56252. DHCP failover settings were being cleared and not saved after being applied on the appliance or Orchestrator deployment page.|8.3.4.0|
|ID: 55133. In rare cases, an IP subnet could not be advertised to BGP if the same subnet was configured on mgmt0.|8.3.4.0|
|ID: 58143. After upgrading to ECOS 8.3.3.0, 8.3.3.1, or 8.3.3.2, an ECV with 8GB RAM might fail to boot up due to a tunnel ID being larger than 2000. This could be because the appliance had more than 2000 tunnels before the upgrade or tunnels had been deleted and re-added multiple times resulting in large tunnel IDs being used.|8.3.3.3|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 57535. Flows configured via optimization policy should not have undergone TCP acceleration, but the appliance was still trying to redirect those flows.|8.3.3.0|
|ID: 57548. Development-level logging was enabled for OSPF, causing intermittent high CPU utilization.|8.3.3.1|
|ID: 57400. In some cases, IP SLA over an HA link was not working as expected. IP SLA tracking appears OK but there was no outgoing IP SLA traffic. Actions|8.3.3.0|
|ID: 57362. At times, breakout traffic was being directed to the backup link if the bandwidth threshold on the primary link had been exceeded instead of only when the primary link was down.|8.3.3.0|
|ID: 57360. If a Radius or TACACS+ key/value pair contained a space character, the upgrade to ECOS 8.3.0.x or higher was failing from previous ECOS releases.|8.3.3.0|
|ID: 57330. For appliances with uptime greater than one year, the calculation to number of days was being converted incorrectly for display in the appliance WebUI banner.|8.3.3.0|
|ID: 57290. An issue with improperly stored packet chain information was causing the appliance to reboot unexpectedly.|8.3.3.0|
|ID: 57245. In some cases, a timing mismatch related to dead peer detection (DPD) and newly negotiated IKE phase 1 SAs was causing prolonged connectivity issues over Zscaler.|8.3.3.0|
|ID: 57239. A large IP packet was allowed to pass without being fragmented, resulting in an unexpected appliance reboot.|8.3.3.0|
|ID: 57135. The quality of VLC multicast traffic was degraded after enabling inbound shaping.|8.3.3.0|
|ID: 57113. After configuring HTTP IP SLA for each of two Zscaler tunnels, both probes were being sent over the same tunnel after some time.|8.3.3.0|
|ID: 57111. In some cases, running the Link Integrity Test over an underlay tunnel was resulting in "null" in some areas of the output.|8.3.3.0|
|ID: 57060. In a previous ECOS release, Zscaler tunnels were flapping approximately every five minutes.|8.3.3.0|
|ID: 57056. An incomplete IPSLA rule configuration was causing excessive and unnecessary internal messages between IPSLA and some management processes.|8.3.3.0|
|ID: 57050. In some cases, diagnostic packet captures were consuming too much disk space and causing the appliance to reboot unexpectedly.|8.3.3.0|
|ID: 57011. An issue with NATD packets bouncing between the appliance and a third-party firewall without reducing TTL at each device was creating excessive traffic and high CPU at the firewall.|8.3.3.0|
|ID: 56984. When using small excess weight values, outbound shaper traffic was not being distributed exactly as intended.|8.3.3.0|
|ID: 56978. In some cases, all inbound TCP passthough traffic from source port 443 was being permitted when firewall mode was set to Harden.|8.3.3.0|
|ID: 56963. After Zscaler brought down a tunnel due to dead peer detection, EdgeConnect was sending tunnel negotiation packets with an old security association and the tunnel was not coming back up on the old IP.|8.3.3.0|
|ID: 56955. A software issue encountered during the upgrade from ECOS 8.1.9.12 to 8.3.2.1 caused the appliance to become unresponsive.|8.3.3.0|
|ID: 56911. In some cases, when booting with insufficient disk space, an appliance did not initialize properly and it was unreachable by Orchestrator.|8.3.3.0|
|ID: 56757. Following a reboot, EdgeConnect ports set to 100 Mbps/full duplex that were connected to provider equipment at the same speed/duplex were switching to 100 Mbps/half duplex.|8.3.3.0|
|ID: 56753. Adding a duplicate management route with a different metric caused the appliance to reboot unexpectedly.|8.3.3.0|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 56752. On rare occasions, some appliance models became stuck in a reboot loop after adding a WAN VLAN interface and rebooting the appliance.|8.3.3.0|
|ID: 56731. A FIPS-enabled appliance failed to boot following the upgrade from ECOS 8.1.9.17 to ECOS 8.3.0.12.|8.3.3.0|
|ID: 56708. API changes have been made to restrict traversal of other directories, limiting access to potentially sensitive data.|8.3.3.0|
|ID: 56687. In some cases, a BGP route with the best admin distance was not selected as the preferred route, though it should have been.|8.3.3.0|
|ID: 56682. NAT pool IDs were being reused when deleting and adding new pools, which could cause an appliance to reboot unexpectedly.|8.3.3.0|
|ID: 56677. A SaaS route from a hub was not being applied on a spoke, resulting in a routing loop.|8.3.3.0|
|ID: 56675. Internet traffic to a specific IP was traversing the tunnel instead of breaking out locally. In the same subnet for another IP, however, local breakout was working as expected.|8.3.3.0|
|ID: 56661. When a large packet was not fragmented and received on the LAN interface, the packet was not dropped and the appliance rebooted unexpectedly.|8.3.3.0|
|ID: 56618. The appliance did not properly handle retransmitted SYN packet for a hairpinned TCP connection, resulting in an appliance reboot.|8.3.3.0|
|ID: 56575. Enabling source and destination NAT was causing the appliance to lose connectivity and reboot unexpectedly.|8.3.3.0|
|ID: 56549. DHCP option 43 was not getting validated correctly. For additional information, see “DHCP Option 43 Value Error” under Upgrade Considerations.|8.3.3.0|
|ID: 56255. The appliance was unable to read an encrypted private key following an upgrade and ended up in a continuous reboot loop.|8.3.3.0|
|ID: 56220. Responses from EdgeConnect appliances were missing some secure HTTP headers.|8.3.3.0|
|ID: 56017. In certain scenarios on inbound flows, the "Consider Non-Default as Internal" was not working as expected for LAN host to EC local IP flows.|8.3.3.0|
|ID: 57995. Processing a packet with an invalid IPv4 protocol ID caused the appliance to reboot unexpectedly.|8.3.2.4|
|ID: 57727. Some EC-XL appliances became unstable following the upgrade to ECOS 8.3.2.3 due to some recent changes to optimize flow redirection.|8.3.4.0|
|ID: 57129. In a redirect cluster of three appliances, flow redirection was failing intermittently.|8.3.2.3|
|ID: 58004. Minimum shaper accuracy value was 5000 microsec and it did not apply to flow rate shaping. In this release, minimum shaper accuracy value is 1000 microsec (smaller value means more accurate shaper) and this setting applies to flow rate shaping as well.|8.3.2.2|
|ID: 56183. If the inbound WAN queue became full and stateful firewall was enabled on the WAN interface, a packet buffer could get freed twice and result in an unexpected reboot.|8.3.2.1|
|ID: 53087. The upgrade to 8.3.2.0 was failing if IPSec anti-replay window had been disabled.|8.3.2.1|
|ID: 55883. A local route was being advertised to BGP peers even though the "Automatically advertise local WAN subnets" setting was disabled.|8.3.2.0|
|ID: 55690. On an EC-V with LAN traffic going through a VTI to AWS Transit Gateway, packets over 1480 bytes were not being fragmented, exceeding the MTU, which was causing all traffic over 1480 bytes to be black holed.|8.3.2.0|
|ID: 55568. Payload packets exchanged between a vulnerability scanner client and server were being processed like internal control packets, causing the appliance to reboot unexpectedly.|8.3.2.0|
|ID: 55429. PIM output was not consistent in the CLI when multicast was enabled with a layer 3 switch.|8.3.2.0|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 55425. Cloud Portal was unreachable after enabling loopback Orchestration and removing connectivity on the mgmt interface.|8.3.2.0|
|ID: 55386. At times, when MySQL traffic was being sent over the Default BIO, WAN latency was spiking and causing application issues.|8.3.2.0|
|ID: 55254. Following the upgrade to Orchestrator 8.9.11, an IPSec UDP key/see mismatch issue was causing some appliances to reboot continuously.|8.3.2.0|
|ID: 55149. An issue with statistics collection was incorrectly showing a high number of flows and bandwidth consumption for TCP traffic on port 0.|8.3.2.0|
|ID: 54730. The appliance CLI could not be accessed from Orchestrator due to an "unsupported key format" error.|8.3.2.0|
|ID: 54699. If bridge interfaces had VLAN tagging and an IPSLA monitor was configured to install on the label representing such an interface, the monitor failed to install on the correct interface.|8.3.2.0|
|ID: 54685. A missing IP mask was causing an appliance to reboot unexpectedly and would not come back up without a power cycle.|8.3.2.0|
|ID: 54573. When changing the flow redirection interface and keeping the same IP address, redirection packets were getting misrouted to the original interface.|8.3.2.0|
|ID: 53890. On rare occasions, the appliance would try to access an invalid memory location, causing an unexpected reboot.|8.3.2.0|
|ID: 53795. In some cases, memory allocation during flow classification was causing a spike in CPU usage, resulting in WCCP flapping and latency issues.|8.3.2.0|
|ID: 53647. Some IPSLAs were showing zero for status change and last up/down times.|8.3.2.0|
|ID: 53603. An error was generated when deploying a Peer Priority Group that contained the name of the host where it was deployed.|8.3.2.0|
|ID: 55676. In rare cases, after an internal queue became full, a packet buffer could get freed twice and result in an unexpected reboot.|8.3.1.4|
|ID: 55486. Following the upgrade to ECOS 8.3.1.1 and Orchestrator 8.10.11, flow optimization policies stopped working.|8.3.1.2|
|ID: 55367. NAT policy was not being applied correctly to the lan1 interface because another interface had been labelled as "lan1."|8.3.1.2|
|ID: 55318. When the DNS proxy feature was enabled, the appliance would incorrectly send DNS replies back to the client with its loopback IP as the source address instead of the DNS server's IP address, causing DNS resolution to fail on the client.|8.3.1.2|
|ID: 54819. The appliance rebooted due to an error in overlay tunnel configuration.|8.3.1.1|
|ID: 54818. Underlay tunnel deletion caused corruption in overlay tunnel configuration which in turn caused the appliance to reboot.|8.3.1.1|
|ID: 54760. When running the link integrity test in passthrough modes, Cloud Orchestrator was generating a "failed to get wan side interfaces" error.|8.3.1.1|
|ID: 54295. ACL rules were not working as expected if the rule used both an IP range and a wildcard.|8.3.1.0|
|ID: 54149. Orchestrator failed to push optimization template to an appliance without a boost license.|8.3.1.0|
|ID: 54123. The LAN interface was listening on TCP 80 even though session management was configured for HTTPS only.|8.3.1.0|
|ID: 54098. In previous releases, the REST API did not include support for route redistribution templates.|8.3.1.0|
|ID: 53989. Threshold crossing alert descriptions for loss and PoC were using tenths of % instead of %.|8.3.1.0|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 52650. On the deployment page, when outbound bandwidth was summed to a value exceeding the Mini license (50 Mbps), the license was changing to BASE+Plus.|8.3.1.0|
|ID: 52605. Routes configured with no next hop were included for consideration in route lookups. They were meant to be for advertisement only.|8.3.1.0|
|ID: 52062. TACACS key was displayed in plain text in the audit log.|8.3.1.0|
|ID: 51639. This releases fixes CVE-2019-16105, ECOS earlier than 8.1.7.x allows ..%2f directory traversal.|8.3.1.0|
|ID: 56509. The ip_summed field in a 3rd-party IPSec inner packet was incorrectly inherited from an outer tunnel packet, causing spurious kernel error messages.|8.3.0.12|
|ID: 56414. Previous ECOS releases were setting the TLS Server Name Indication (SNI) to IP address, instead of FQDN, in the TLS handshake to Orchestrator.|8.3.0.12|
|ID: 56293. Erroneous SMART temperature disk failure alarms were being raised on the EC-XS appliance.|8.3.0.12|
|ID: 56226. In this release, CPU statistics are logged periodically.|8.3.0.12|
|ID: 56138. Due to a timing issue, only two NATD packets were being sent within the active window, leaving remote peers in the backoff state.|8.3.0.12|
|ID: 56241. Appliances were advertising a local BGP-PE route to BGP peers even though an SD-WAN fabric route was better (lower metric).|8.3.0.11|
|ID: 56180. Due to a timing issue, only two NATD packets were being sent within the active window, leaving remote peers in the backoff state.|8.3.0.11|
|ID: 56178. The Zscaler tunnel on one of two EdgeConnect appliances was flapping every five minutes while the other tunnel remained stable.|8.3.0.11|
|ID: 56172. Following the upgrade to ECOS 8.3.0.10, the appliance was continually rebooting.|8.3.0.11|
|ID: 56121. Following an appliance reboot, connection via the Cloud Portal WebSocket was not successful unless the node service was restarted.|8.3.0.11|
|ID: 56117. In an HA configuration, the appliance was sending traffic to an HA passthrough tunnel instead of the local route.|8.3.0.11|
|ID: 56106. Fixed an issue that was allowing port 8001 to be opened in listen mode.|8.3.0.11|
|ID: 56023. A local route was being advertised to BGP peers even though the "Automatically advertise local WAN subnets" setting was disabled.|8.3.0.11|
|ID: 53835. Following the upgrade to ECOS 8.3.0.3, certain appliances were showing a persistent alarm indicating that disks were rebuilding.|8.3.0.11|
|ID: 52608. After adding a local route to a branch EdgeConnect and sharing to Silver Peak peers, the route was learned by one hub but not another.|8.3.0.11|
|ID: 55871. Prisma tunnels went down following the upgrade to ECOS 8.3.0.6 because the IKE version in the configuration was not being honored.|8.3.0.10|
|ID: 54495. If an automatic route duplicated two or more dynamic routes (BGP or OSPF), one of the dynamic routes was left in the routing table without the duplicate tag, causing the appliance to reboot unexpectedly.|8.3.0.10|
|ID: 55706. Insufficient static memory allocation was causing the appliance to reboot unexpectedly.|8.3.0.9|
|ID: 55685. In rare instances, when an appliance's BGP Peer configuration has Local Interface set to any, the appliance will not establish peer connections.|8.3.0.9|
|ID: 54639. In some cases, 25 Gbps EC-XL-P appliances were unable to recover link because noise on the interface was causing the network interface card to continue to register a signal.|8.3.0.9|
|ID: 55875. Appliances were unable to receive updated application definitions from Orchestrator.|8.3.0.8|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 55577. Traffic loss across IPSec tunnels to Zscaler was being seen after the implementation of IP SLA enhancements.|8.3.0.8|
|ID: 55566. A race condition was preventing an internal packet fragment counter to increment properly, resulting in all packet fragments being dropped and forcing an appliance reboot.|8.3.0.8|
|ID: 55525. Connectivity to Silver Peak Cloud Portal was not working over an EdgeHA link.|8.3.0.8|
|ID: 55421. On some platforms, a management controller that reboots the appliance in the case of a system hang could lose its configuration and fail to operate as intended.|8.3.0.8|
|ID: 55388. BGP sessions were failing after receiving a 0.0.0.0/32 prefix.|8.3.0.8|
|ID: 55385. After an HA interface is set to admin down and then admin up, some tunnels failed to recover and required an appliance reboot to restore them.|8.3.0.8|
|ID: 55353. Some time after upgrading appliances to 8.3.0.6, OSPF abruptly stopped redistributing the default route.|8.3.0.8|
|ID: 55317. A driver on certain appliance platforms was preventing the appliance from rebooting itself after detecting a system hang.|8.3.0.8|
|ID: 54212. When trying to open the DHCP Leases page from Orchestrator or directly from the appliance, the appliance would lose its connection to Orchestrator.|8.3.0.8|
|ID: 55027. Mishandling of a route advertisement packet caused the appliance to reboot unexpectedly.|8.3.0.7|
|ID: 54956. BGP routes were flapping at multiple sites in different locations at the same time.|8.3.0.7|
|ID: 54859. Cloud Portal traffic was matching a manual policy because the destination IP was missing from the built-in policy.|8.3.0.7|
|ID: 54822. The appliance rebooted due to an error in overlay tunnel configuration.|8.3.0.7|
|ID: 54756. LAN routing for some flows was showing Hairpin2, even though the subnet was local.|8.3.0.7|
|ID: 54754. Following an ECOS upgrade, a limit on the number of rules per interface was causing LAN SNAT rules to stop working.|8.3.0.7|
|ID: 54702. Flows matching an allow rule were getting denied after matching the default rule, which denied them.|8.3.0.7|
|ID: 54578. For some BIOs configured for High Throughput, backhaul traffic was only utilizing one of three available links.|8.3.0.7|
|ID: 54113. In some cases, the appliance was unable to obtain an IP address on the PPPoE interface until the admin status of interface was manually set to down and then back up.|8.3.0.7|
|ID: 52563. Advertisement of internal BGP communities was causing issues with setting up BGP sessions. These communities are not being advertised.|8.3.0.7|
|ID: 52104. DHCP Option 150 was not supported properly.|8.3.0.7|
|ID: 51717. The appliance was rebooting unexpectedly when trying to decode certain IPv6 packets from Zscaler.|8.3.0.7|
|ID: 54145. Orchestrator failed to push an optimization template to an appliance without a boost license.|8.3.0.5|
|ID: 54108. A corrupted internal message was causing the BGP/OSPF daemon to crash, resulting in an unexpected appliance reboot.|8.3.0.5|
|ID: 54092. Routes were not getting advertised to BGP PE peers, and an auto- configured failover route was not getting installed after the manual route went down.|8.3.0.5|
|ID: 54049. The EC node process had to be rebooted when Orchestrator-SP allocated a physical EdgeConnect appliance to a tenant.|8.3.0.5|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 54008. An internal deadlock under heavy route update traffic was causing the appliance to reboot unexpectedly.|8.3.0.5|
|ID: 53979. ACL port rules were being processed incorrectly if the rules were specified using both Range and Compound format.|8.3.0.5|
|ID: 53938. Under some conditions, DNAT was not working properly on return traffic, causing LAN NATed connections to fail.|8.3.0.5|
|ID: 53902. At times, an appliance could have duplicate entries on Cloud Portal, and one of the entries would remain in the unapproved state.|8.3.0.5|
|ID: 53861. Tunnels terminating at WAN interfaces, in "Allow All" mode at a pair of appliances in an Edge HA configuration, were going down intermittently.|8.3.0.5|
|ID: 53611. If syslogd did not launch on startup, it did not try to relaunch. This could cause issues when applying template configuration changes.|8.3.0.5|
|ID: 53181. CPU utilization was unexpectedly high on EC-US, EC-XS, and EC-V models after upgrading to 8.3.0.3.|8.3.0.5|
|ID: 52891. Tunnel alarms contained internal tunnel ID instead of user-configured name.|8.3.0.5|
|ID: 52647. The cloudinit script was checking the status of the wrong process before proceeding to a ready state.|8.3.0.4|
|ID: 52733. A built in policy for learned routes was not being honored on VTI interfaces, causing IP SLA traffic to be routed to it instead of being sent as passthrough-unshaped.|8.3.0.3|
|ID: 52632. Cloud-init was not initializing the system.|8.3.0.3|
|ID: 52586. sysdump files were missing a file (ipmitool.txt) that was needed to help troubleshoot hardware and environmental issues.|8.3.0.3|
|ID: 52414. DHCP requests were not getting a response back to the originating interface using DHCP relay.|8.3.0.3|
|ID: 52382. Fixed an issue that was causing an appliance to be unreachable for approximately 40 minutes following a reboot.|8.3.0.3|
|ID: 51881. Routes were being advertised incorrectly and causing a routing loop. Note that this issue had been listed under upgrade considerations in previous release notes: If upgrading from a release prior to 8.2.1.0, and LAN side BGP is enabled, BGP PE routes will be announced for subnet sharing, which may create routing loops in the network.|8.3.0.3|
|ID: 51692. Some route map matching prefixes were not working on different subnet masks.|8.3.0.3|
|ID: 52343. After upgrading to 8.3.0.1, tunnels created using wan1 IP addresses went down and would not come back up.|8.3.0.2|
|ID: 52129. BGP branch and BGP transit route types from a node on an older release (8.1.7.x and 8.1.9.x) is not processed properly.|8.3.0.1|
|ID: 49650. Internet breakout traffic fails when the TCP MSS clamping feature is enabled.|8.3.0.1|
|ID: 51282. Save the init config wizard should now stop it from popping up automatically when login into appliance WebUI.|8.3.0.0|
|ID: 50991. WebUI login/logout is now logged in auditlog.|8.3.0.0|
|ID: 50590. When a route policy directs traffic pass-through and an optimization policy for the same traffic disables TCP acceleration, expired flows are not deleted.|8.3.0.0|
|ID: 51147. When boosted traffic contained a significant number of jumbo packets and jumbo support was not enabled, the appliance was intermittently running out of jumbo buffers.|8.2.1.0|
|ID: 51102. For statically configured hosts, options like domain-name-servers and ntp-servers were not being applied.|8.2.1.0|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 50987. DHCP server option 150 was not supported, causing issues for certain devices that required it.|8.2.1.0|
|ID: 50976. Fixed an issue with the way advertise flags were being updated for specific prefixes, which was causing an issue with subnet sharing.|8.2.1.0|
|ID: 50288. When altering the Boost license, outstanding Boost alarms were not cleared, which incorrectly let to throttling.|8.2.1.0|
|ID: 50217. Added validation for hostname in DHCP server's static IP assignment configuration to avoid potential DHCP server failure.|8.2.1.0|
|ID: 49711. Multiple snapshot files may lead to utilization of entire disk space and result in unexpected behavior.|8.2.1.0|
|ID: 49595. In some cases, when authenticating with a remote server, logins were taking longer than expected, adding latency to existing sessions and causing issues with realtime charts.|8.2.1.0|
|ID: 49246. IPFIX – add specific interface IDs (lan0, lan1, wan0, wan1) for pass- through traffic. IPFIX – add specific interfaces IDs (lan0, lan1) for LAN-side traffic to/from overlays.|8.2.0.0|
|ID: 48185. Route lookup needs to be optimized.|8.2.0.0|
|ID: 53857. Under some conditions, IPSec UDP tunnels went down after a tunnel configuration change|8.1.9.12|
|ID: 53788. VLANs were not working on the EC-XL with 25Gbps fiber cards|8.1.9.12|
|ID: 53677. Appliance software could not be rolled back to 8.1.9.x after being upgraded to 8.3.x.|8.1.9.12|
|ID: 53670. After advertising a route to the LAN side, the appliance would reboot unexpectedly after receiving ping packets.|8.1.9.12|
|ID: 53644. An issue with flow redirection was causing the appliance to reboot unexpectedly|8.1.9.12|
|ID: 53636. Setting speed-duplex to auto/auto for fiber interfaces is now allowed, and the setting will be preserved during an upgrade.|8.1.9.12|
|ID: 53548. GRE tunnels with Zscaler were not working with an interface's secondary IP address.|8.1.9.12|
|ID: 53534. Some changes have been made to help prevent a stack overflow that was causing the appliance to reboot unexpectedly.|8.1.9.12|
|ID: 53180. Threshold crossing alerts were not being cleared after a tunnel had been deleted.|8.1.9.12|
|ID: 53169. Running the running "show ssh server host-keys" command was generating an internal error.|8.1.9.12|
|ID: 52989. Some BGP peers were advertising the appliance's own route back to the appliance.|8.1.9.12|
|ID: 52980. In rare cases, when http/https snooping was enabled, Orchestrator was unable to fetch flows on an appliance, and the appliance indicated that flow data had been corrupted.|8.1.9.12|
|ID: 52805. Named passthrough tunnels were not being assigned a valid remote node ID, which was causing traffic to be routed incorrectly.|8.1.9.12|
|ID: 52803. In certain cases, a TCP MSS mismatch between the sender and receiver was causing issues with some application traffic.|8.1.9.12|
|ID: 52782. Traffic matching the Zscaler BIO was using an incorrect underlay.|8.1.9.12|
|ID: 52703. In some cases, the Orchestrator IP address could not be configured via USB ZTP.|8.1.9.12|
|ID: 52663. When TCP acceleration was enabled, some flows that should have been passthrough had an optimization policy applied.|8.1.9.12|
|ID: 52631. The appliance was unreachable from Orchestrator due to an issue with the node.js service. Rebooting the appliance fixed the issue.|8.1.9.12|
|ID: 52613. For some appliances that were connected to Cloud Orchestrator via internet circuits, the appliances were repeatedly showing as unreachable in Orchestrator.|8.1.9.12|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 52514. In some cases, sshd was causing unexpectedly high CPU usage.|8.1.9.12|
|ID: 52488. Customers can now use the RMA wizard to replace a virtual appliance.|8.1.9.12|
|ID: 51566. In certain cases, some tunnels would only come up after applying a tunnel exception in Orchestrator and then later removing the exception.|8.1.9.12|
|ID: 48581. After modifying the deployment LAN side next hop IP address, the VRRP configuration was getting unexpectedly deleted.|8.1.9.12|
|ID: 52453. In some cases when a partial page was being merged from a flow that had aged out and been deleted, the appliance would reset unexpectedly.|8.1.9.11|
|ID: 52378. Management traffic from certain sites was getting dropped.|8.1.9.11|
|ID: 52362. For some EdgeHA UDP IPSec tunnels, more than one packet in a flow was being treated as the first packet, and NAT was allocating multiple ports.|8.1.9.11|
|ID: 52091. A mismatch in UDP payload size was causing DNS resolution to fail from LAN side appliances.|8.1.9.11|
|ID: 52049. Added some additional logging capabilities to help diagnose issues with certain tunnels going down and showing up in flows as Passthrough (NO ROUTE).|8.1.9.11|
|ID: 51982. Manually adding a management route results in duplicate routing table entries as the system is auto- creating the same route.|8.1.9.11|
|ID: 51977. Self-gen packets were getting dropped at a non-existent interface, causing the appliance to reboot unexpectedly.|8.1.9.11|
|ID: 51964. The appliance reset unexpectedly after adding an interface on the Deployment page.|8.1.9.11|
|ID: 51789. Default routes were being repeatedly added and deleted, causing connection issues.|8.1.9.11|
|ID: 51749. Rapid addition/deletion of the same route was causing the appliance to reboot unexpectedly.|8.1.9.11|
|ID: 51707. Tunnel utilization alarms were being triggered even though the alert threshold had not been crossed.|8.1.9.11|
|ID: 51129. NAT rules were not working for all flows.|8.1.9.11|
|ID: 50763. sysd.EMERG logs were being seen at the NOTICE level.|8.1.9.11|
|ID: 48515. Rapid addition/deletion of the same route was causing the appliance to reboot unexpectedly.|8.1.9.11|
|ID: 47759. VRRP was ignoring configured priority on sub interfaces when authentication was enabled.|8.1.9.11|
|ID: 47454. Users were unable to select bwan0 as the flow-redirection interface on a pair of EC-XL appliances.|8.1.9.11|
|ID: 51825. BGP community information was not being passed between appliances running different ECOS software versions.|8.1.9.10|
|ID: 51738. After upgrading to 8.1.9.6, spokes were using LTE backup links as the primary transmit path.|8.1.9.10|
|ID: 51702. Following a reboot, the PPPoE interface was losing label information. Since IPSLAs use interface labels for specifying an interface, the PPPoE IPSLA stopped working as the label was no longer associated with any interface.|8.1.9.10|
|ID: 51735. Audit logs were displaying the password for PPPoE.|8.1.9.9|
|ID: 51727. Disabled Dead Peer Detection (DPD) to avoid having tunnels torn down due to timeouts. Note that full DPD support is available in ECOS 8.2.1.0 and 8.3.0.0.|8.1.9.9|
|ID: 51695. An issue with WAN packet routing was causing the data path to use an invalid flow WAN tunnel ID, resulting in an unexpected reboot.|8.1.9.9|
|ID: 51648. In some scenarios, the node process was trying to synchronize a change while also updating the RTT calculation, resulting in an unexpected reboot.|8.1.9.9|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 51439. If an HA tunnel went down, the flow was dropped but not resuming when the tunnel came back up because the flow was not getting reclassified.|8.1.9.9|
|ID: 51279. Fixed an issue that was causing flows to be transmitted as "passthrough-unshaped," even though a route to the destination existed and underlay and overlay tunnels were up - active.|8.1.9.9|
|ID: 50731. Fixed an issue that was causing UDP traffic to be sent as passthrough, even though a valid route and peer were available.|8.1.9.9|
|ID: 50690. In some scenarios, SNMPv3 pooling would stop working and could only be restored by disabling and re-enabling it.|8.1.9.9|
|ID: 51654. Following an upgrade to ECOS 8.1.9.7, flow count was unusually high and causing excessive CPU utilization.|8.1.9.8|
|ID: 51475. When multicast was enabled on mgmt0, routes were getting added to the management routing table and causing unexpected reboots.|8.1.9.8|
|ID: 51445. When using the API to configure multiple applications, including compound applications, multiple domain/service names were given for a compound rule, resulting in an unexpected reboot.|8.1.9.8|
|ID: 51324. A memory leak in the IPSLA module was causing unexpected appliance reboots.|8.1.9.8|
|ID: 51313. In rare cases, the appliance would reboot when processing a Citrix- accelerated connection.|8.1.9.8|
|ID: 51300. In some cases, following a tunnel down event, the best route for a subnet was being determined incorrectly.|8.1.9.8|
|ID: 51150. If a subnet metric value was modified by an IPSLA, the value was not being restored to its original setting after the affecting IPSLA was removed.|8.1.9.8|
|ID: 51035. In some instances, the API was being called with invalid data, causing one or more processes to hang and eventually leading to an unexpected reboot.|8.1.9.8|
|ID: 51000. High flow rates were causing abnormally high CPU utilization, resulting in an overall performance degradation.|8.1.9.8|
|ID: 50800. ESP traffic was getting dropped at the WAN interface when firewall state was set to "stateful."|8.1.9.8|
|ID: 50591. When a route policy was directing traffic pass-through and an optimization policy for the same traffic was disabling TCP acceleration, expired flows were not being deleted.|8.1.9.8|
|ID: 49757. Some changes have been made to help prevent a stack overflow that was causing the appliance to reboot unexpectedly.|8.1.9.8|
|ID: 51263. The PPPoE interface was failing to recover when the underlying interface went down and came back up.|8.1.9.7|
|ID: 50964. Insufficient validation when manually adding routes via CSV import were allowing bad routes to be created, and these entries could not be deleted without restoring a previous configuration to the appliance.|8.1.9.7|
|ID: 50879. Changed the way security associations (SAs) behave for 3rd party IPSec tunnels to avoid a high number of repeated negotiation requests or rekeys.|8.1.9.7|
|ID: 50875. Creating a default route entry (0.0.0.0/32) for interfaces assigned a 32- bit IP address and not having a configured default gateway was causing issues in some customer deployments.|8.1.9.7|
|ID: 50803. Local BGP routes were not being advertised to local BGP peers during Admin Distance (AD) change for remote static routes. If two protocols (e.g., subnet sharing and BGP) have the same AD, route metrics will be used to choose the best route among duplicates across route types.|8.1.9.7|
|ID: 50756. In this release, we removed CIFS, CITRIX, and iSCSI rules and changed TCP advanced options for lan-to- wan and wan-to-lan max buffer for default 65535 from 64,000KB to 4,000KB.|8.1.9.7|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 50692. Packet direction was being set incorrectly when received from the WAN side and destination was local. This was causing the return packet direction to be set as WAN to LAN, which meant that reverse DNAT was not being applied.|8.1.9.7|
|ID: 50687. Under some circumstances, BGP/OSPF was causing CPU0 to hit 0% idle intermittently.|8.1.9.7|
|ID: 50652. A race condition between tunnel deletion and pathchar message processing was causing a segment fault, resulting in an unexpected reboot.|8.1.9.7|
|ID: 50592. During upgrade, the appliance rebooted unexpectedly while applying loopback configuration, causing the upgrade to fail.|8.1.9.7|
|ID: 50567. For hairpin flows, the Rx action was being updated before the flow became bidirectional and it was displaying an incorrect status.|8.1.9.7|
|ID: 50523. When a route policy directed traffic pass-through and an optimization policy for the same traffic disabled TCP acceleration, expired flows were not being deleted.|8.1.9.7|
|ID: 50492. Under some rare circumstances, subnet sharing updates were delayed by up to 15 minutes.|8.1.9.7|
|ID: 50490. Fixed an issue where appliance datapath IPs were not accessible from the SDWAN fabric when Zone- based Firewall was enabled.|8.1.9.7|
|ID: 50486. The auto-generated route had been DOWN for a newly added loopback interface.|8.1.9.7|
|ID: 50442. The "ssh listen interface" command was not working as expected, depending on the interface selected.|8.1.9.7|
|ID: 50375. After Orchestrator closed the appliance browser, the active session would remain until the appliance auto logout timer expired, resulting in failure of new logins when sessions should be available.|8.1.9.7|
|ID: 50320. In some cases, Threshold Crossing Alerts were being incorrectly triggered for bonded tunnels.|8.1.9.7|
|ID: 50248. AVC domain match was incorrectly matching substrings of a domain.|8.1.9.7|
|ID: 50214. On rare occasions, an installed IP SLA monitor could not be modified or deleted until after an appliance reboot.|8.1.9.7|
|ID: 50199. When the IP SLA monitor comes back up, the priority of the VRRP interface was not being restored to the configured value.|8.1.9.7|
|ID: 50132. Under certain circumstances, the VRRP module was delayed in responding to management requests, causing the UI to hang briefly.|8.1.9.7|
|ID: 50020. In rare cases, deleting and adding VLAN interfaces was causing the appliance to reboot unexpectedly.|8.1.9.7|
|ID: 49995. DHCP relay flows were being policy dropped and not getting reset for some time, making it appear that the appliance had no route to the DHCP server.|8.1.9.7|
|ID: 49841. Added a fix so that PPPoE interfaces start and restart in a more reliable way to ensure that unknown or non-configured device numbers are getting created.|8.1.9.7|
|ID: 50218. Multicast failed to initialize on tlan0 or twan0 interfaces.|8.1.9.6|
|ID: 50186. Some tunnels “Down Mis-configured” in Edge HA deployment.|8.1.9.6|
|ID: 50026. Incorrect subnet selection after 8.1.7.x to 8.1.9.x upgrade running mis- matched releases.|8.1.9.6|
|ID: 49866. DSCP matching in ACL incorrect.|8.1.9.6|
|ID: 49839. Only the first port entry in a comma separated ACL entry is matched.|8.1.9.6|
|ID: 49814. Node service fails to start when 'web https enable' is enabled.|8.1.9.6|
|ID: 49765. Zone-based firewall fails to forward traffic when using tagged VLAN interfaces with LAN interface labels.|8.1.9.6|
|ID: 49734. CLI: show pim neighbors has no output.|8.1.9.6|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 49688. ECOS should permit /31 addresses.|8.1.9.6|
|ID: 49675. [CVE-2018-15473] OpenSSH vulnerability.|8.1.9.6|
|ID: 49635. EC-V is limited to 16 CPUs. Resolution: EC-V now supports 32 CPUs.|8.1.9.6|
|ID: 49560. BGP- local preference attribute ignored over AS path.|8.1.9.6|
|ID: 49497. BGP route updates are not logged.|8.1.9.6|
|ID: 49375. ECOS should support the option to add ASN and locally configured communities to subnet shared routes.|8.1.9.6|
|ID: 49209. ECOS does not support AWS CloudInit.|8.1.9.6|
|ID: 49186. IP addresses in routing logs are not human readable.|8.1.9.6|
|ID: 49094. LAN side VRRP addresses not responding to ICMP when received via tunnel.|8.1.9.6|
|ID: 48992. Appliance reset when performing FEC at high rates with a large amount of errored packets.|8.1.9.6|
|ID: 46714. EC-V VLAN filtering issues with SRIOV i40evf driver on ENCS platform.|8.1.9.6|
|ID: 46607. EC-V with SR-IOV adds double VLAN tags.|8.1.9.6|
|ID: 50012. When BGP branch route flapped, subnet shared default route was removed from peers.|8.1.9.5|
|ID: 49466. CVE-2019-11477, CVE-2019-5599, CVE-2019-11479 vulnerabilities.|8.1.9.5|
|ID: 49371, Appliance reset in network with 10,000+ tunnels and large differential latency.|8.1.9.5|
|ID: 49339. Some TCP non-accelerated flows did not timeout.|8.1.9.5|
|ID: 49318. Some UDP traffic was incorrectly sent pass through.|8.1.9.5|
|ID: 49297. Bonded tunnel incorrectly set to fail state causing traffic to be routed incorrectly.|8.1.9.5|
|ID: 49252. AVC compound classification is not functioning correctly with DSCP.|8.1.9.5|
|ID: 49250. Unexpected reset when changing virtual interface.|8.1.9.5|
|ID: 49194. IPSec UDP tunnel down after upgrade to 8.1.9.4.|8.1.9.5|
|ID: 49046. Appliance reset when processing IPSec packet with no ESP header.|8.1.9.5|
|ID: 49036. When BGP and OSPF are both disabled the learned protocol information (AS path, communities, route tags) is not persisted and is therefore not available when BGP or OSPF are later enabled.|8.1.9.5|
|ID: 48988. When switching from DHCP server to DHCP relay then back to DHCP server, the process fails.|8.1.9.5|
|ID: 48987. Custom SAAS application NAT failed.|8.1.9.5|
|ID: 48948. Tunnel health retry count defaults to 30 secs after reboot.|8.1.9.5|
|ID: 48889. Subnet sharing failed to redistribute to OSPF with very large routing table.|8.1.9.5|
|ID: 48510. There is no CLI to display the routing table for debug.|8.1.9.5|
|ID: 48464. Improve the precision of the formula used to calculate tunnel metric for sorting overlay tunnels.|8.1.9.5|
|ID: 48463. DHCP server intermittent performance under load.|8.1.9.5|
|ID: 48433. Appliance manager does not correctly validate VLAN IDs.|8.1.9.5|
|ID: 48412. Traffic flow failed to route correctly when a WAN label was changed.|8.1.9.5|
|ID: 48363. DSCP value appears “na” in Flows report.|8.1.9.5|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 48352. Appliance manager route policy page is slow to load with 100’s of entries.|8.1.9.5|
|ID: 48316. EC-V appliance reboot when applying pre-configuration script.|8.1.9.5|
|ID: 48315. PPPoE fails to initialize when configured using a tagged interface.|8.1.9.5|
|ID: 48307. Portal unreachable alarm should not be generated if portal string is empty.|8.1.9.5|
|ID: 48301. Flows should remain “sticky” to peers.|8.1.9.5|
|ID: 48286. BGP stops advertising OSPF-redistributed routes after AS number change, until appliance is rebooted.|8.1.9.5|
|ID: 48275. Traffic routed incorrectly when the set action of an existing ACL is modified (permit/deny).|8.1.9.5|
|ID: 48254. Backup tunnels should not send unnecessary data.|8.1.9.5|
|ID: 48224. The CLI command to add routes should default to BGP and OSPF advertisement disabled.|8.1.9.5|
|ID: 48204. Unexpected reset when applying multiple route updates simultaneously.|8.1.9.5|
|ID: 48164. Every other ICMP packet sent at 30 second intervals gets dropped.|8.1.9.5|
|ID: 48161. IPSec UDP tunnel stuck in NATD complete and down state.|8.1.9.5|
|ID: 48135. Mgmt0 interface flapping up/down after changing to a new, dedicated management VLAN.|8.1.9.5|
|ID: 48054. IPSLA failed to disqualify breakout tunnel.|8.1.9.5|
|ID: 48004. IPsec tunnel is down due to packet loss from 'IPsec rx replay’.|8.1.9.5|
|ID: 46236. When remote authentication fails, the appliance should not fallback to local authentication. Resolution: When remote authentication server is unavailable/unreachable (could be radius/tacacs), fallback to local authentication. When remote authentication server fails to login the user, DO NOT fallback to local authentication, just fail to login user.|8.1.9.5|
|ID: 46120. Appliance reset when processing new application classification definitions.|8.1.9.5|
|ID: 44196. Eliminate hard-coded brownout thresholds in HA mode.|8.1.9.5|
|ID: 49347. Unexpected appliance reset when updating application classification definitions.|8.1.9.4|
|ID: 48933. Unexpected appliance reset when updating application classification.|8.1.9.4|
|ID: 48262. Appliance reset due to invalid length in UDP header.|8.1.9.4|
|ID: 48175. Link Integrity Test matches application definition “Oracle” rather than “Silverpeak_iperf".|8.1.9.4|
|ID: 48169. Upgrade to 8.1.9.3 potentially disables NAT.|8.1.9.4|
|ID: 48165. "Generate Show Tech" button should be removed from GUI.|8.1.9.4|
|ID: 48107. BGP not advertising route to BGP neighbor when same route also learnt from that neighbor.|8.1.9.4|
|ID: 48034. Appliance reboot when SNMP traps fail to get sent due to physical interface mis configuration.|8.1.9.4|
|ID: 48014. Unanticipated network-wide reports of high latency.|8.1.9.4|
|ID: 47926. IPSLA tracking for VRRP failed between two appliances.|8.1.9.4|
|ID: 47908. Unexpected packet loss on appliances with 2,000+ tunnels.|8.1.9.4|
|ID: 47899. Flows and flow details fail to display custom defined application definition.|8.1.9.4|
|ID: 47894. Rarely, BGP communities did not populate on remote routers.|8.1.9.4|
|ID: 47868. OSPF incorrectly learns route on the same interface it is advertising that route.|8.1.9.4|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 47841. Unexpected appliance reset when processing route updates.|8.1.9.4|
|ID: 47820. When modifying flow system limits on an EC-V, tunnel limits will be reset to 50.|8.1.9.4|
|ID: 47817. All available fixed wan interfaces should support ZTP. Resolution: wan1 interfaces on all appliances support ZTP. Additionally, wan2 on the EC-S, twan0 on the EC-M and EC-L, and twan1 on the EC-XL support ZTP.|8.1.9.4|
|ID: 47812. Packets are dropped (ifdown drop) when route map policy is set to passthrough (shaped or unshaped).|8.1.9.4|
|ID: 47790. When mgmt0 is statically configured and appliance is rebooted, the static routes for mgmt0 are removed.|8.1.9.4|
|ID: 47760. Appliance reset due to malformed IPSec packet.|8.1.9.4|
|ID: 47737. Appliance reset when cross-connect tunnels have different IPSec authentication algorithms configured.|8.1.9.4|
|ID: 47735. Global OSPF redistribution tags should overwrite remotely learned OSPF tags.|8.1.9.4|
|ID: 47710. Improve performance of route lookups.|8.1.9.4|
|ID: 47703. Appliance reset when processing http get requests with null hostname for domain name matching.|8.1.9.4|
|ID: 46875. Shaper should process multiple packets in a row.|8.1.9.4|
|ID: 47671. Under heavy load some traffic classes dropped packets early.|8.1.9.4|
|ID: 47623. VRRP caused appliance reset upon initialization.|8.1.9.4|
|ID: 47600. LAN-side packet drops in very high throughput scenarios.|8.1.9.4|
|ID: 47550. All virtual appliances (except ESXi) should follow RFC by using virtual MAC address as master.|8.1.9.4|
|ID: 47533. Orchestrator temporarily lost connectivity to appliance.|8.1.9.3|
|ID: 47520. After upgrading hub to 8.1.9.2 IPSEC tunnels to spoke sites running 8.1.5.x won’t establish.|8.1.9.3|
|ID: 47498. Appliance reset after upgrade to 8.1.9.2.|8.1.9.3|
|ID: 47464. Unexpected reset when processing application classification updates.|8.1.9.3|
|ID: 47462. Underlay tunnels sourced from HA link are down (orphaned DNAT).|8.1.9.3|
|ID: 47391. Tunnel version mismatch alarm description is misleading.|8.1.9.3|
|ID: 47229. Reset due to invalid tunnel encapsulation mode.|8.1.9.3|
|ID: 47182. High latency was experienced at some sites.|8.1.9.3|
|ID: 46872. Performance degradation when using > 100 ACLs.|8.1.9.3|
|ID: 46806. Adding interface labels on Orchestrator resets IP SLA on appliances.|8.1.9.3|
|ID: 46764. Setting management IP via cli is not persistent across reboots.|8.1.9.3|
|ID: 46751. New "Peer/Service" details are not displayed in route policies set actions "Destination" field when "Destination Type" is set as peer.|8.1.9.3|
|ID: 46583. Spurious NTP server unreachable alarm.|8.1.9.3|
|ID: 46458, Unable to modify deployment bandwidth of HA device when peer is offline.|8.1.9.3|
|ID: 46422. Internal hairpinning failed for traceroute traffic.|8.1.9.3|
|ID: 46382. Subnet directed broadcast not supported.|8.1.9.3|
|ID: 46362. IP SLA subnet metric manipulation incorrectly uses the configured subnet metric and not the current subnet metric value.|8.1.9.3|


-----

|Issue|First Release to Resolve|
|---|---|
|ID: 46203. Tunnel state “Down – Brownout” is misleading.|8.1.9.3|
|ID: 46096. Internal hair pinning should honor the incoming overlay.|8.1.9.3|
|ID: 45546. Default route to mgmt0 persisted after interface was down.|8.1.9.3|
|ID: 44900. Add default routes for ZTP on tagged interfaces.|8.1.9.3|
|ID: 44829. EdgeConnect should prefer LAN-side interfaces when connecting to Orchestrator.|8.1.9.3|
|ID: 43572. Increase supported VLANs from 20 to 64 in router mode and 32 in bridge mode.|8.1.9.3|
|ID: 47166. Appliance reset when port scanning protection feature processed redirected flows.|8.1.9.2|
|ID: 46796. Unexpected appliance reset due to null application pointer.|8.1.9.2|
|ID: 46732. Unexpected reset due to memory leak in SSL proxy feature.|8.1.9.2|
|ID: 46506. Unexpected reset when parsing DNS packets.|8.1.9.2|
|ID: 46386. Previously idle IPSec UDP tunnels took > 10 minutes to re-establish after reboot.|8.1.9.2|
|ID: 44899. Inbound 3rd-party IPSec tunnel traffic should be allowed when the WAN interface firewall mode is Stateful.|8.1.9.0|
|ID: 44646. Upgrade to 8.1.8.1 failed to load configuration database.|8.1.9.0|
|ID: 44386. ECOS should permit the user to statically configure a Public IP for tunnel creation.|8.1.9.0|


-----

## Procedure to Roll Back from 8.3.x.x+

###### Due to some of the changes between the 8.3.x.x and later releases and all prior releases, you must carry out the following procedure on the appliance if you need to roll back to a pre-8.3.x.x release. 

 NOTE: If you are rolling back to 8.1.9.15 (i.e., if you were running 8.1.9.15 prior to the upgrade), this procedure should not be necessary.

 1. Open an SSH session to the appliance.

 2. Enter privileged exec mode:

 > enable

 3. Enter configuration mode:

 # configure terminal

 4. Open a new SSH session to the appliance or duplicate the current session.

 5. In the second SSH session, enter privileged exec mode:

 > enable

 6. Break out of the appliance session and access the OS shell:

 # _spsshell

 7. Change to the /var directory:

 # cd /var

 8. Unlink the /var/tmp, /var/run, and /var/lock symbolic links:

 # unlink tmp

 # unlink run

 # unlink lock

 9. If they don’t already exist, create tmp, run, and lock directories in /var:

 # mkdir tmp

 # mkdir run

 # mkdir lock

 10. Return to the first SSH session and boot from the next partition with a DB reset:

 # reboot empty-db next


-----

## ECOS Compatibility for FastFail/Underlay Paths

###### Silver Peak EdgeConnect appliances measure underlay path characteristics to determine which tunnels carry data. These measurements include loss, latency, jitter, and sub-second path failure detection. This protocol changed in the 8.1.9.10 release and will affect certain features when inter-operating with 8.1.7.x and older releases.

#### Compatible Releases

###### ECOS releases up to 8.1.7.x are fully compatible with ECOS 8.1.9.0 through 8.1.9.9. If all appliances are running ECOS 8.1.9.9 and earlier, there will be no issues with FastFail in your mixed-mode network.

#### Incompatible Releases

###### If your network includes appliances running ECOS up to 8.1.7.x and 8.1.9.10 or later, you should note the following incompatibilities and plan to make appropriate configuration changes:

 • 8.1.7.x appliances will not detect sub-second failure of underlay paths. They will still detect failure of underlay paths in 30 seconds or less depending on the tunnel keep-alive timeout.

 • 8.1.7.x appliance overlays will detect brownout - but not sub-second underlay path failures. Underlay path failure detection will be equal to the tunnel keep-alive timeout.

 • 8.1.9.10+ appliances will not detect sub-second failure of underlay paths. They will still detect failure of underlay paths in 30 seconds or less depending on the tunnel keep-alive timeout.

 When running in this mixed mode, it is advised to turn off FastFail on 8.1.7.x appliances if underlays are used in route policies. As always, it is recommended to run the same software release on all appliances in the network.


-----

## Need Help?

###### If you have any questions, contact your Silver Peak sales representative.

 For product and technical support, contact Silver Peak Systems using any of the methods below:

 • 1.877.210.7325 (toll-free in USA)

 • +1.408.935.1850

 • www.silver-peak.com/support

## Revision History

###### Jan 2, 2025 Rev A: Initial document revision.

**Copyright**
Copyright © 2025 Silver Peak Systems, Inc. All rights reserved. Information in this document is subject to change at any time. Use of this documentation is restricted as specified
in the End User License Agreement. No part of this documentation can be reproduced, except as noted in the End User License Agreement, in whole or in part, without the
written consent of Silver Peak Systems, Inc.

**Trademark Notification**
Silver Peak, the Silver Peak logo, and all Silver Peak product names, logos, and brands are trademarks or registered trademarks of Silver Peak Systems, Inc. In the United States
and/or other countries. All other product names, logos, and brands are property of their respective owners.

**Warranties and Disclaimers**
This documentation is provided “as is” without warranty of any kind, either expressed or implied, including, but not limited to, the implied warranties of merchantability, fitness for a
particular purpose, or non-infringement. Silver Peak Systems, Inc. assumes no responsibility for errors or omissions in this documentation or other documents which are referenced
by or linked to this documentation. References to corporations, their services and products, are provided “as is” without warranty of any kind, either expressed or implied. In no
event shall Silver Peak Systems, Inc. be liable for any special, incidental, indirect or consequential damages of any kind, or any damages whatsoever, including, without limitation,
those resulting from loss of use, data or profits, whether or not advised of the possibility of damage, and on any theory of liability, arising out of or in connection with the use of this
documentation. This documentation may include technical or other inaccuracies or typographical errors. Changes are periodically added to the information herein; these changes
will be incorporated in new editions of the documentation. Silver Peak Systems, Inc. may make improvements and/or changes in the product(s) and/or the program(s) described in
this documentation at any time.


-----

