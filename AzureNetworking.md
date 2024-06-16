In Azure DNS, you can create address records manually within relevant zones. The records most frequently used will be:

**Host records:** A/AAAA (IPv4/IPv6)
**Alias records:** CNAME

**Considerations**
- The name of the zone must be unique within the resource group, and the zone must not exist already.
- The same zone name can be reused in a different resource group or a different Azure subscription.
- Where multiple zones share the same name, each instance is assigned different name server addresses.
- Root/Parent domain is registered at the registrar and pointed to Azure NS.
- Child domains are registered in AzureDNS directly.

**Delegate DNS domain**
Delegating a domain to Azure DNS, you must use the name server names provided by Azure DNS. You should always use all four name server names, regardless of the name of your domain.
The parent and child zones can be in the same or different resource group. Notice that the record set name in the parent zone matches the child zone name, in this case partners.

**3 Types in Private DNS services**
- Azure DNS Private Zones
- Azure-provided name resolution
- Name resolution that uses your own DNS server

**Limitations of Internal DNS**
- Can't resolve across different VNets.
- Registers resource names, not guest OS names.
- Does not allow manual record creation.

**Azure Private DNS Zones**
Private DNS zones in Azure are available to internal resources only. They are global in scope, so you can access them from any region, any subscription, any VNet, and any tenant.
Configure a specific DNS name for a zone.
For scenarios which require more flexibility than Internal DNS allows, you can create your own private DNS zones. These zones enable you to:
- Create records manually when necessary.
- Resolve names and IP addresses across different zones.
- Resolve names and IP addresses across different VNets.

**Two ways to link VNets to a private zone:**
- **Registration:** Each VNet can link to one private DNS zone for registration. However, up to 100 VNets can link to the same private DNS zone for registration.
- **Resolution:** There may be many other private DNS zones for different namespaces. You can link a VNet to each of those zones for name resolution. Each VNet can link to up to 1000 private DNS Zones for name resolution.

If the DNS server is outside Azure, it doesn't have access to Azure DNS on 168.63.129.16. In this scenario, setup a DNS resolver inside your VNet, forward queries for to it, and then have it forward queries to 168.63.129.16 (Azure DNS). Essentially, you're using forwarding because 168.63.129.16 is not routable, and therefore not accessible to external clients.
