# SDWAN

The Cisco SD-WAN solution is a software-based, virtual IP fabric overlay network that builds secure, unified connectivity over any transport network (the underlay). The underlay transport network is the physical infrastructure for the WAN, such as public internet, MPLS, Metro Ethernet, and LTE/4G. The underlay network provides a service to the overlay network and is responsible for the delivery of packets across networks.

The virtualized network runs as an overlay on cost-effective hardware, whether they are physical routers, called WAN edge routers running either Cisco IOS XE or the Viptela operating systems, or virtual machines (VMs) in the cloud, such as the Cisco CSR1000v, Cisco WAN Edge Cloud, or Cisco ISRv. Centralized controllers, called vSmart controllers, oversee the control plane of the Cisco SD-WAN fabric, efficiently managing the provisioning, maintenance, and security for the entire Cisco SD-WAN overlay network. The Cisco vBond orchestrator automatically authenticates all other Cisco SD-WAN devices when they join the Cisco SD-WAN overlay network.

## vBond

The orchestration plane is responsible for zero-touch deployment and security validation during deployment.
The orchestration plane has the following characteristics:
1. performs initial authentication
2. orchestrates the connectivity between the management, control, and data plane
3. requires a public IP address or 1:1 Network Address Translation (NAT)
4. facilitates NAT traversal
5. authorizes all control connections (allow list model)
6. distributes a list of Cisco vSmart controllers to all Cisco WAN Edge routers

## vManage (Management Plane)

The management plane provides control and visibility to the entire fabric.
The management plane has the following characteristics:
1. Single pane of glass for Day 0, Day 1, and Day 2 operations
2. Real-time alerting
3. Centralized provisioning
4. Configuration standardization
5. Simplicity of deploying
6. Simplicity of change
7. Supports various programmatic application programming interfaces (APIs)

## vSmart (Control Plane)

Cisco vSmart controllers are a scale-out control plane function of the Cisco SD-WAN fabric.

The main characteristics of the control plane with Cisco vSmart controllers are as follows:
1. Facilitates fabric discovery
2. Disseminates control plane information between the Cisco WAN Edge routers
3. Distributes data plane and application-aware routing policies to the Cisco WAN Edge routers
4. Implements control plane policies, such as service chaining, multitopology, and multihop
5. Dramatically reduces control plane complexity
6. Highly resilient

## WAN Edge (Data Plane)

The Cisco SD-WAN data plane is responsible for tunnel establishment and data forwarding within the fabric.
The main characteristics of the data plane with Cisco WAN Edge routers are as follows:
1. Provides secure data plane with other WAN Edge routers
2. Establishes a secure control plane with Cisco vSmart controllers (OMP)
3. Implements data plane and application-aware routing policies
4. Exports performance statistics
5. Leverages traditional routing protocols such as Open Shortest Path First (OSPF), Border Gateway Protocol (BGP), Enhanced Interior Gateway Routing Protocol (EIGRP), and Virtual Router Redundancy Protocol (VRRP)
6. Supports Zero-Touch Provisioning (ZTP) and Cisco plug-and-play (PnP)
7. Physical and virtual form factor

## vAnalytics

The Cisco vAnalytics platform provides graphical representations of the performance of your entire Cisco SD-WAN overlay network over time and enables you to drill down to the characteristics of a single carrier, tunnel, or application at a particular time.

The main characteristics of Cisco vAnalytics are as follows:
1. A cloud-based analytics engine
2. Data anonymization
3. Optional solution element
4. Opt-in customer model
5. Analyze fabric telemetry
6. Capacity projections
7. Service level agreement (SLA) violation trends
8. Utilization anomaly detection
9. Application quality of experience (QoE)
10. Carrier grading

## Cisco SD-WAN Terminology
The Overlay Management Protocol (OMP) is the central component of the Cisco SD-WAN fabric.

Cisco SD-WAN characteristics are as follows:
1. TCP-based extensible control plane protocol
2. Runs between Cisco WAN Edge routers and Cisco vSmart controllers, and between the Cisco vSmart controllers
3. Inside authenticated TLS/DTLS connections
4. Cisco WAN Edge routers are not required to peer with all Cisco vSmart controllers
5. Advertises control plane context and policies
6. Dramatically lowers control plane complexity and raises overall solution scale

Using the concepts of address families and route attributes, the OMP advertises all pertinent control plane information between the Cisco WAN Edge routers to establish direct IPsec communication between the Cisco WAN Edge routers without reliance on the Internet Key Exchange (IKE) protocol. The OMP allows the exchange of full reachability without relying on traditional routing protocols, such as OSPF, EIGRP, and BGP, over the IPsec tunnels. Furthermore, the OMP allows the propagation of centrally (on Cisco vManage) defined data and application-aware routing policies to the Cisco WAN Edge routers.

### OMP Network Terminology
Every router at the edge of a network has two sides for routing:

1. Transport side for routing to the transport network

2. Service side for routing to the service side of the network

All routers must learn all prefixes to have full-mesh communication among all routers. Traditionally, routers learn these prefixes by using full-mesh interior gateway protocol (IGP) and BGP, or by enabling routing on an overlay tunnel, such as BGP or IGP over MPLS or Generic Routing Encapsulation (GRE).

The Cisco SD-WAN fabric builds on the route reflector model by centralizing routing intelligence. Essentially, all prefixes learned from the service side on a router are advertised to a centralized controller, which then reflects the information to other routers over the control plane of the network. The controllers do not handle any data traffic—they are involved only in control plane communication. Scale challenges associated with full-mesh routing on the transport side of the network are eliminated.

In a Cisco SD-WAN overlay network, VPNs divide the network into different segments. By default, two VPNs are present in the configurations of all Cisco SD-WAN devices, and these VPNs serve specific purposes:

1. VPN 0: This is the transport VPN. It carries control traffic over secure DTLS or TLS connections between Cisco vSmart controllers and Cisco WAN Edge routers, and between Cisco vSmart controllers and Cisco vBond orchestrators. Initially, VPN 0 contains all device interfaces except for the management interface, and all the interfaces are disabled. You must configure WAN transport interfaces in VPN 0 for the control plane to establish itself so that the overlay network can function.

2. VPN 512: This is the management VPN. It carries out-of-band network management traffic among the Cisco SD-WAN devices in the overlay network. By default, VPN 512 is configured and enabled. You can modify this configuration if desired.

To segment user networks and user data traffic locally at each site and to interconnect user sites across the overlay network, you need to create additional VPNs on Cisco WAN Edge routers. (These VPNs are identified by a number that is not 0 or 512.) To enable data traffic flow, you need to associate interfaces with each VPN, assigning an IP address to each interface. These interfaces connect to local-site networks, not to WAN transport clouds. For each of these VPNs, you can set other interface-specific properties, and you can configure features specific for the user segment, such as BGP, OSPF, EIGRP routing, VRRP, quality of service (QoS), traffic shaping, and policing.

Transport locators (TLOCs) are collections of information that make up a transport-side connection.

TLOCs contain the following information:

1. System-IP: IPv4 address (nonrouted identifier; router ID)

2. Color: type of WAN interface on the local Cisco WAN Edge

3. Encapsulation type: IPsec or GRE

4. Private TLOC: IP address on the interface located inside NAT

5. Public TLOC: IP address on the interface located outside of NAT

![OMP](https://raw.githubusercontent.com/deliawolf/SDWAN/main/OMP.png)
The system IP address is a persistent IP address that identifies the Cisco SD-WAN device. The system IP is similar to a router ID on a regular router, which is the address used to identify the router from which the packets originated. The system IP address is used internally as the device's loopback address in the transport VPN, which is VPN 0.
On a Cisco WAN Edge router, you need to use the system IP address as the router ID for BGP or OSPF. If you configure a router ID for either of these protocols and the router ID is different from the system IP address, the router ID takes precedence.

Organization Name is case-sensitive and must be identical on all the devices in your overlay network. The name must match the name in the certificates for all Cisco SD-WAN network devices.

The site ID is the numeric identifier of the site in the Cisco SD-WAN overlay network. The site ID must be the same for all Cisco SD-WAN devices that reside on the same site.

The figure shows a device that has two WAN connections and therefore two TLOCs. The system IP address of the router is 1.1.1.1. The TLOC on the left is uniquely identified by the system IP address 1.1.1.1, the color metro-ethernet, and the encapsulation IPsec, and it maps to the physical WAN interface with the IP address 184.168.0.69. The TLOC on the right is uniquely identified by the system IP address 1.1.1.1, the color biz-internet, and the encapsulation IPsec, and it maps to the WAN IP address 75.1.1.1.

#### Transport Locators
A transport locator (TLOC) identifies the physical interface where a Cisco WAN Edge router connects to the WAN transport network or to a NAT gateway. A TLOC is denoted by a 3-tuple that consists of the system IP address of the OMP speaker, a color, and an encapsulation type. In this tuple, the IP address, the color, and the encapsulation identify a VPN or traffic flow within a VPN. OMP advertises TLOCs using TLOC routes.

It is important to remember that you can establish tunnels between endpoints of different colors as long as there is IP connectivity.


#### TLOCs and Colors
When assigning colors to WAN transport tunnels, there are the following color options:

The specific color that is used is categorized as private or public.

Private colors (mpls, private1-6, metro-ethernet)

All other colors are public (lte, biz-internet, public-internet, gold, and so on)

Private versus public color is highly significant.

Private = No NAT

The color setting applies to the following:

WAN Edge-to-WAN Edge communication

WAN Edge-to-controller communication

You can identify an individual WAN transport tunnel by assigning it a color. The color is one of the TLOC parameters associated with the tunnel (although the CLI on a Cisco vSmart controller allows you to locally configure a color, the color has no meaning because Cisco vSmart controllers have no TLOCs). On a Cisco WAN Edge router, you can configure only one tunnel interface with the color default.

The values are as follows: 3G, biz-internet, blue, bronze, custom1, custom2, custom3, default, gold, green, lte, Metro Ethernet, mpls, private1, private2, private3, private4, private5, private6, public internet, red, and silver.

The private versus public setting is very important and has implications on WAN Edge-to-WAN Edge and WAN Edge-to-controller communication.

Based on the color settings on both sides, Cisco WAN Edge routers know when to use public or private IP addresses when establishing IPsec tunnels:

If private colors are used on both sides, a private IP and port are used to establish a tunnel.

If there is a mix of private and public colors, a public IP and port are used.

When a tunnel is established between two public colors, public addressing as a tunnel source and destination is used.

The private IP address is the IP address assigned to the interface of the Cisco SD-WAN device. This IP address is the pre-NAT address, and despite the name, it can be a publicly routable address or a private address (RFC 1918).

The public IP address is the Post-NAT address detected by the Cisco vBond orchestrator. This address can be either a publicly routable address or a private (RFC 1918) address. In the absence of NAT, the private and public IP addresses of the Cisco SD-WAN device are the same.

By default, Cisco WAN Edge routers attempt to connect to every TLOC over each WAN transport, including TLOCs that belong to other transports marked with different colors. This feature is helpful when you have different internet transports at different locations, for example, the locations that are required to communicate directly with each Cisco WAN Edge router. To prevent this behavior, there is a restrict keyword that can be specified along with the color of the tunnel. Using the restrict keyword prevents attempts to establish Bidirectional Forwarding Detection (BFD) sessions to TLOCs with a different color and is enabled on private transports to prevent forming sessions with public transports.

On Cisco WAN Edge routers, BFD is automatically started between peers and cannot be disabled. It runs between all Cisco WAN Edge routers in the topology encapsulated in the IPsec tunnels and across all transports. BFD operates in echo mode, which means when the BFD packets are sent by a Cisco WAN Edge router, the receiving Cisco WAN Edge router returns the BFD packets without processing them. The purpose of BFD is to detect path liveliness, and it can also perform quality measurements for application-aware routing, such as loss, latency, and jitter. BFD is also used to detect network failures caused when there is either a complete power outage or a drop in voltage in the electrical power supply system.

#### Fabric Operation

OMP runs between Cisco WAN Edge routers and Cisco vSmart controllers and also as a full mesh between Cisco vSmart controllers. When DTLS/TLS control connections are formed, OMP is automatically enabled. OMP peering is established using the system IPs, and only one peering session is established between a WAN Edge device and a Cisco vSmart controller, even if multiple DTLS/TLS connections exist. OMP exchanges route prefixes, next-hop routes, crypto keys, and policy information.

OMP advertises three types of routes from Cisco WAN routers to Cisco vSmart controllers:

1. OMP routes, or vRoutes, are prefixes that are learned from the local site, or service side, of a Cisco WAN Edge router. The prefixes are originated as static or connected routes, or from within the OSPF, BGP, or EIGRP protocol, and redistributed into OMP so they can be carried across the overlay. OMP routes advertise attributes such as transport location (TLOC) information, which is similar to a BGP next-hop IP address for the route, and other attributes such as origin, origin metric, originator, preference, site ID, tag, and VPN. An OMP route is only installed in the forwarding table if the TLOC to which it points is active.

2. TLOC routes advertise TLOCs connected to the WAN transports, along with an extra set of attributes such as TLOC private and public IP addresses, carrier, preference, site ID, tag, weight, and encryption key information.

3. Service routes represent services (firewall, IPS, application optimization, and so on) that are connected to the Cisco WAN Edge local-site network and are available for other sites for use with service insertion. In addition, these routes include originator System IP, TLOC, and VPN-IDs; the VPN labels are sent in this update type to tell the Cisco vSmart controllers which VPNs are serviced at a remote site.

The Cisco SD-WAN builds a secure virtual IP fabric that includes the following elements:

1. Routing and routing advertisements to establish and maintain the flow of traffic throughout the network.

2. Layer 3 segmentation using virtual routing and forwarding (VRF), to isolate various flows of traffic. This process is useful for separating traffic from various customers or various business organizations within an enterprise.

3. Peer-to-peer concepts for setting up and maintaining bidirectional connections between pairs of protocol entities.

4. Authentication and encryptions.

5. Policies for routing and data traffic.

The Cisco SD-WAN fabric identifies transport-side links and automatically encrypts traffic between sites. The associated encryption keys are exchanged over a secure session with the centralized controller. The secure sessions with the controller are set up automatically, using RSA and certificate infrastructure.

This approach has several benefits:

1. The Cisco SD-WAN fabric itself authenticates all devices participating in the network, which is an important step in securing the infrastructure.

2. The fabric automatically exchanges encryption keys associated with the transport links, eliminating the need to configure thousands of pair-wise keys.

3. The fabric ensures that the network is not prone to attacks from the transport side.
