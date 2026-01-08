# Network Design
This section gives the detailed network design.

[Assumptions](#assumptions) | [Network Design Diagrams and Justifications](#network-design-diagrams-and-justifications) | [WiFi Design](#wifi-design) | [Address Allocations](#address-allocations) | [Recommended Hardware](#recommended-hardware) | [Plan](./plan.md) | [Cloud Services](./cloud.md) | [Security](./security.md) | [Ethics](./ethics.md) | [Reflection](./reflection.md) | [Return to index](./README.md)

## Assumptions
The following assumptions clarify the design parameters for the Truelec network. 

- **Headquarters location (city):** Bundaberg, Queensland. 
- **Branch locations (cities):** Brisbane (QLD), Perth (WA), Adelaide (SA). 
- **Number of staff at HQ:** 70 staff members. 
- **Number of staff at one branch (designed branch):** 25 staff members (Perth branch). 
- **Redundancy requirement:** WAN routers and key links should have redundancy to maintain availability if a device or link fails.

## Network Design Diagrams and Justifications

The design includes a Headquarters (HQ) network and one Branch Office (Perth) network. 
![Main Network Diagram](./images/main_network_diagram.drawio.png)

### Key design decisions
- **Hierarchical layout at HQ:** The HQ network is designed using a simple hierarchical approach (core/gateway + distribution/access switching) to support growth and easier troubleshooting. 
- **Redundant WAN edge:** Two WAN-capable routers are recommended at HQ to reduce the risk of single points of failure for Internet/WAN connectivity. 
- **Segmentation by function:** Different subnets are allocated for management, servers, staff WiFi, and guest WiFi to reduce broadcast domains and improve security and manageability. 
- **Branch simplicity:** The branch uses a smaller design (edge router/firewall + access switching + APs) because it has fewer users and services. 

## WiFi Design
WiFi is provided for staff mobility and guest Internet access with separation between corporate and guest traffic. 

### SSIDs and settings
- **SSID (Staff):** `Truelec_Corporate`
  - **Security:** WPA3-Enterprise (802.1X/RADIUS). 
  - **Band preference:** 5 GHz preferred for performance; 2.4 GHz enabled only if required for coverage/legacy devices. 
- **SSID (Guest):** `Truelec_Guest` 
  - **Security:** WPA2-PSK (separate passphrase). 
  - **Client isolation:** Enabled to prevent guest-to-guest traffic. 
  - **Access control:** Internet-only (no access to internal LAN/server subnets). 

## Address Allocations
This addressing plan meets the project requirements: only /24 masks are used, and the first octet is the last two digits of a group member student ID (74 or 36). [file:1]
Private address ranges (e.g., 192.168.x.x) are not used anywhere. 

### Subnet plan (/24 only)

| Site / Segment | Subnet (Network /24) | Example gateway | Notes |
| :--- | :--- | :--- | :--- |
| HQ - Wired LAN | 74.10.1.0/24 | 74.10.1.1 | Office users (wired). |
| HQ - Staff WiFi | 74.10.2.0/24 | 74.10.2.1 | Corporate WiFi clients. |
| HQ - Guest WiFi | 74.10.3.0/24 | 74.10.3.1 | Guest WiFi (Internet-only). |
| HQ - Servers | 74.10.10.0/24 | 74.10.10.1 | Internal servers (HR/CRM/accounting). |
| Perth Branch - Wired LAN | 36.20.1.0/24 | 36.20.1.1 | Branch office users (wired). |
| Perth Branch - Staff WiFi | 36.20.2.0/24 | 36.20.2.1 | Branch corporate WiFi clients. |
| Perth Branch - Guest WiFi | 36.20.3.0/24 | 36.20.3.1 | Branch guest WiFi (Internet-only). |

## Recommended Hardware

## Recommended Hardware
The following hardware is a recommended minimum set to implement the design at HQ and the Perth branch, including redundancy at the WAN edge (HQ) and WiFi coverage at both sites. 

| Item | Suggested model (example) | Minimum specs / why | Example AUD price | Spec / product link |
| :--- | :--- | :--- | :--- | :--- |
| WiFi Access Point (HQ + Branch) | Ubiquiti UniFi U6 Pro (U6-PRO) | Wi‑Fi 6 AP, supports modern security (WPA3), PoE powered, suitable for office coverage. | ~$339 AUD | https://www.scorptec.com.au/product/networking/access-points-&-extenders/94778-u6-pro [web:83] |
| Core/Distribution Switch (HQ) | Cisco Catalyst 9200L 48‑Port PoE+ L3 (C9200L-48P-4G-E) | Enterprise switch, PoE+ for APs/phones, Layer 3 capable, suitable for HQ aggregation/distribution. | ~$5,198.95 AUD | https://www.mwave.com.au/product/cisco-catalyst-9200l-48-port-gigabit-poe-l3-manageable-switch-with-4-port-sfp-ac56993 [web:88] |
| Next‑Gen Firewall (HQ) | Fortinet FortiGate 60F (FG‑60F) | NGFW feature set suitable for a small/medium HQ, supports IPsec VPN for branch connectivity. | ~$938.74 AUD | https://thetechgeeks.com/products/fortinet-fortigate-60f-61f [web:92] |
| Branch Router/Firewall (Perth) | Fortinet FortiGate 60F (FG‑60F) | Same model at branch simplifies management and supports site‑to‑site IPsec VPN back to HQ. | ~$938.74 AUD | https://thetechgeeks.com/products/fortinet-fortigate-60f-61f [web:92] |


