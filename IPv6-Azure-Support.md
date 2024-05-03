# IPv6 Azure Support
Is there IPv6 support in Azure? **Yes**. Mostly Infrastructure as a Service, but not many Platform as a Service. Curremtly no native IPv6 only networking is supported.

## Pricing

There's **no charge** to use Public IPv6 Addresses or Public IPv6 Prefixes. Associated resources and bandwidth are charged at the same rates as IPv4.

## Is the Azure Portal Dual Stacked?
Yes, the https://portal.azure.com site is dual-stacked, https://portal.azure.us is not. But the Azure Bash cli nor the Azure Powershell cli (ifconfig shows only ipv4 address) seem to support IPv6. Azure Powershell to be confirmed.

## Using Azure as Code:
* Using the infrastructure as code tools are dependant on the platform you are using in the first place. You need to support IPv6 and allow IPv6 connectivity out.
* Then the specific tool can have a dependancy on IPv6 support

### Is the AZ CLI IPv6 capable?
In short, the AZ CLI IPv6 capable
* AZ CLI is written in python and is dependant on python support, but python does support IPv6
* AZ Cli interracts with different URLS and will depend on their enablement - see [Azure CLI endpoints for proxy bypass](https://learn.microsoft.com/en-us/cli/azure/azure-cli-endpoints?tabs=azure-cloud)

### Is the AZ Powershell IPv6 capable?
Documentation needs to be research more as well as testing.
See https://learn.microsoft.com/en-us/powershell/azure/az-powershell-proxy?view=azps-10.1.0

### Is the Azure ARM (Azure Resource Manager) IPv6 capable?
ARM can be deployed via the Azure Portal or Azure CLI or Powershell. See each section above about support.

### Is the Azure BICEP IPv6 capable?

### Is the Azure Terraform IPv6 capable?

Terraform for the Azure provider connects using both IPv4 and IPv6, but little documentation found at this time.

```bash
> terraform refresh
....
> netstat -vatWn
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 10.1.0.4:39426          20.62.130.212:443       TIME_WAIT  
tcp        0      0 10.1.0.4:39440          20.62.130.212:443       TIME_WAIT   
tcp6       0      0 ace:cab:deca::4:36418   2603:1037:1:128::6:443  TIME_WAIT  
tcp6       0      0 ace:cab:deca::4:36438   2603:1037:1:128::6:443  TIME_WAIT  
tcp6       0      0 ace:cab:deca::4:36426   2603:1037:1:128::6:443  TIME_WAIT  
tcp6       0      0 ace:cab:deca::4:51432   2603:1036:3000:138::4:443 TIME_WAIT  
tcp6       0      0 ace:cab:deca::4:39276   2603:1036:3000:128::81:443 TIME_WAIT  
tcp6       0      0 ace:cab:deca::4:53854   2603:1037:1:128::8:443  TIME_WAIT  
``` 

### Is the Azure Cloud Bash Shell IPv6 capable?

No

### Is the Azure Cloud Powershell Shell IPv6 capable?

No

## Infrastructure (IaaS) Support

| Service  | Supported | Reference | Notes | 
| ------------- | ------------- | ------------- | ------------- | 
| Azure Public IP address prefixes | Yes - Used for VMs, Standard Load Balancers, Azure Firewall, VPN Gw* | https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/public-ip-address-prefix |
| VNET (Virtual Networks) | Yes, Dual Stack  | https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/ipv6-overview |
| Subnets | Yes, Dual Stack and must be exactly /64 | https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/ipv6-overview |
| VNET peering | Yes | https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview | 
| Azure Virtual WAN | No | https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/ipv6-overview | Azure Virtual WAN currently supports IPv4 traffic only. |
| Network Security Groups (NSG) | Yes | https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/ipv6-overview | ICMPv6 isn't currently supported in Network Security Groups. |
| User Defined Routes (UDR) | Yes | https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/ipv6-overview |
| DHCPv6 for Linux VMs (some need manual config) | Yes | https://learn.microsoft.com/en-us/azure/load-balancer/load-balancer-ipv6-for-linux?tabs=ubuntu | 
| NAT Gateway | No | https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/configure-public-ip-nat-gateway | Public IPv6 address and public IPv6 prefixes aren't supported on NAT gateways at this time. However, NAT gateways can be deployed on a dual stack virtual network subnet with IPv6 and IPv4 prefixes. | 
| Public IP | Yes | https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/public-ip-addresses | support DNS Name Label | 
| NIC  | Yes, Dual Stack (must have IPv4) | |
| Azure Firewall | No| https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/ipv6-overview | Azure Firewall doesn't currently support IPv6. It can operate in a dual stack VNet using only IPv4, but the firewall subnet must be IPv4-only. |
| Private Endpoints | No | |
| Private Links | No | |
| VPN Gateway | No | |
| ExpressRoute Gateway | Yes, Dual Stack | |
| Standard Internal Load Balancer | Yes | https://learn.microsoft.com/en-us/azure/load-balancer/ipv6-dual-stack-standard-internal-load-balancer-powershell |
| Standard Public Load Balancer | Yes | https://learn.microsoft.com/en-us/azure/load-balancer/virtual-network-ipv4-ipv6-dual-stack-standard-load-balancer-powershell |
| Application Gateway v2 | Yes - Commercial | https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/ipv6-overview | Support dual-stack or can serve single stack. Application Gateway v2 can dual-stack to users while inside backend is IPv4 only in IaaS, PaaS or any reachable IPv4 address |
| Front Door | Yes | https://learn.microsoft.com/en-us/azure/frontdoor/front-door-overview | Support in Commercial and US Gov. Support dual-stack or can serve single stack. Application Gateway v2 can dual-stack to users while inside backend is IPv4 only in IaaS, PaaS or any reachable IPv4 address |
| Bastion | No | | |
| DDOS | Yes | https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/ipv6-overview |
| IPv6 Troubleshooting and Diagnostics are available with load balancer metrics/alerting and Network Watcher features such as packet capture, NSG flow logs, connection troubleshooting and connection monitoring. | Yes | |

Limitations, see https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/ipv6-overview

The current IPv6 for Azure Virtual Network release has the following limitations:
* VPN gateways currently support IPv4 traffic only, but they still can be deployed in a dual-stacked virtual network using Azure PowerShell and Azure CLI commands only.
* Dual-stack configurations that use floating IP can only be used with public load balancers, not internal load balancers.
* Application Gateway v2 doesn't currently support IPv6. It can operate in a dual stack virtual network using only IPv4, but the gateway subnet must be IPv4-only. Application Gateway v1 doesn't support dual stack virtual networks.
* IPv6-only Virtual Machines or Virtual Machines Scale Sets aren't supported, each NIC must include at least one IPv4 IP configuration.
* To add IPv6 to existing IPv4 deployments, you can't add IPv6 ranges to a virtual network that has existing resource navigation links.
* While it's possible to create NSG rules for IPv4 and IPv6 within the same NSG, it isn't currently possible to combine an IPv4 subnet with an IPv6 subnet in the same rule when specifying IP prefixes.
* ICMPv6 isn't currently supported in Network Security Groups.
* Azure Virtual WAN currently supports IPv4 traffic only.
* Azure Firewall doesn't currently support IPv6. It can operate in a dual stack VNet using only IPv4, but the firewall subnet must be IPv4-only.

Note: In my experience, deploying Marketplace solutions may not properly deploy on dual-stacked subnets.

Creating a dual-stacked VM - Note that *INTERNAL* IPv6 NIC address is address-translated to the Public IPv6 address for inbound and outbound Internet access. Remember that NAT-GW is not supported yet, so only one Public IP address is translated. If scalability is required, see Load Balancer as an option.
- Azure Portal - https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/create-vm-dual-stack-ipv6-portal
- Azure CLI - https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/create-vm-dual-stack-ipv6-cli 
- Azure Powershell - https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/create-vm-dual-stack-ipv6-powershell 


## Platform as a Service (PaaS) Support
| Service  | Supported | Reference | Notes |
| ------------- | ------------- | ------------- | ------------- | 
| Azure Active Directory | Yes | See https://learn.microsoft.com/en-us/troubleshoot/azure/active-directory/azure-ad-ipv6-support |
| Azure DNS - Azure DNS support for IPv6 (AAAA) records, no inverse resolution* | | | Forward DNS for IPv6 is supported for Azure public DNS. Reverse DNS isn't supporte | 
| AKS | Yes with limitations | [https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/ipv6-overview](https://learn.microsoft.com/en-us/azure/aks/configure-kubenet-dual-stack?tabs=azure-cli%2Ckubectl) | See page for more details |s

## Software as a Service (SaaS) Support

| Service  | Supported | Reference | Notes |
| ------------- | ------------- | ------------- | ------------- | 
| Microsoft Azure Active Directory | Yes |  https://learn.microsoft.com/en-us/troubleshoot/azure/active-directory/azure-ad-ipv6-support | Ensure to update Conditial Access and outbound traffic policies if needed to continue to allow customers to work |
| Microsoft Exchange Online - M365 | Yes | https://learn.microsoft.com/en-us/microsoft-365/enterprise/ipv6-support?view=o365-worldwide |  |
| Microsoft SharePoint Online (Can be enabled with IPv6 with a support request) | | | 
| Microsoft Teams (Only supports IPv4) | | | 


