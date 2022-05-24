---
layout: post
bg: "/zero-trust-network/bg.jpg"
title: "Zero Trust Network"
summary: "Zero Trust Defined, Explained, Implementation, and Explored"
tags: ['Computer Security','Seminar']
date: 2022-05-24
toc: true
---

# Problems with traditional network models
Businesses are increasingly depending on cloud platforms to manage a wide range of devices and networks, and bad actors are using the disarray to increase account hacking. It gets increasingly difficult to maintain the network secure as the number of users climbs. Hackers' ways of attacking and exploiting massive systems are becoming more complex, making them more dangerous.

The idea that datacenter systems and traffic can be trusted is erroneous. The traditional network security architecture divides the network into different zones, each separated by layers of firewalls. Each zone has different trusts, resource access rights. This model is very safe from outside attacks. 
<div style="text-align: center">
    <img src="https://i.imgur.com/bjiPvPh.jpg"/>
</div>

But what will happen if the attack is from the inside?

If an attacker acquires network access from a low-security zone, they can utilize the resources of that zone and interact with other servers in the higher-security zone, then move and take over the server in a highly secure zone.

# What is Zero Trust?
The Zero Trust network was intended to address the inherent issues with trusting a network. This is a security model that requires strict verification of all requests, assuming that requests to access a private network's resources come from the public Internet. Whether the request comes from outside or within the private network itself, zero trust says: *"Never Trust, Always Verify"*.

Putting it more simply:
- **Traditional network security**: Trusts device, users, anything inside the network.
- **Zero trust network**:  Never Trust anyone or anything.

<div style="text-align: center">
    <img src="https://www.cloudflare.com/static/a0ed30da3e38b7b72df14089ca2173c2/zero-trust-castle-spot-illustration_after.svg"/>
</div>

Putting it more simply:
- **Traditional network security**: Trusts device, users, anything inside the network.
- **Zero trust network**:  Never trust anyone or anything.

A Zero trust network is built on 5 fundamentals:
- The network is always assumed to be hostile.
- External and internal threats exist on the network at all times.
- Network locality is not sufficient for deciding trust in a network.
- Every device, user, and network flow is authenticated and authorized.
- Policies must be dynamic and calculated from as many sources of data as possible.

According to IBM Technology, three Zero trust principles are:
- Never Trust, Always Verify
- Implement Least Privilege
- Assume Breach

# Why you need to use ZTA
## Because of advantages of itself
Zero Trust is not a single architecture but a set of guiding principles for workflow, system design and operations that can be used to improve the security posture of any classification or sensitivity level.

* Verify explicitly. Always authenticate and authorize based on all available data points, including user identity, location, device health, service or workload, data classification, and anomalies.

* Use least privilege access. Limit user access with Just-In-Time and Just-Enough-Access (JIT/JEA), risk-based adaptive polices and data protection to protect both data and productivity.

* Assume breach. Minimize blast radius for breaches and prevent lateral movement by segmenting access by network, user, devices, and application awareness. Verify all sessions are encrypted end to end. Use analytics to get visibility, drive threat detection, and improve defenses.


## Because of the market require
<div style="text-align: center">
    <img src="https://i.imgur.com/e0fTkfp.png"/>
    <b>
        <a href="https://www.grandviewresearch.com/industry-analysis/zero-trust-security-market-report">U.S zero trust security market size </a>
    </b>
</div>

With the development of infomation technology, the risk of service is increment. So many company are having plan or on moving to zero trust network architecture. 

## Because of the trust of end user/engineer/professional
<div style="text-align: center">
    <img src="https://i.imgur.com/P89goKo.png"/>
    <b>
        <a href="https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWGWha">Zero Trust Network Implementation Plan Percentage by Microsoft Security</a>
    </b>
</div>

<div style="text-align: center">
    <img src="https://i.imgur.com/MUEoorS.png"/>
    <b>
        <a href="https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWGWha">Zero Trust Network Enterprise Implementation Percentage by Microsoft Security</a>
    </b>
</div>
Nowaday Zero Trust Network is a trend at cybersecurity network. Why your company don't plan to implementation it now !!!


# Comparison to alternative technology

## Software-defined Perimeter (SDP) vs. Zero Trust Network

<div style="text-align: center">
    <img src="https://www.nist.gov/sites/default/files/images/2020/10/23/zero%20trust.png"/>
    <b>
         Software-defined Perimeter (SDP) vs. Zero Trust Network
    </b>
</div>

| SDP                                                                                              | Zero Trust                                                                 |
| ------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------- |
| Build a barrier between trusted and untrusted resources                                          | No resources is trusted and all have to identify themselves                |
| Is an network that sit on top of another network to coceal it                                    | Is a network that restrict access an  assume that all actors are malicious |
| Controllers are used for continuous authentication and validation of users to the network assets | All hosts must provide identification and data has to be encrypted         |

## Virtual Priavte NetWork (VPN) vs. Zero Trust NetWork

| VPN                                                                      | Zero Trust                                                                |
| ------------------------------------------------------------------------ | ------------------------------------------------------------------------- |
| Blindly trusts authorized users and gives them access to the network     | Takes a much more holistic approach to security                           |
| Encrypt connections between remote, authorized users and company network | Restrict access from all traffic that try to access the network resources |
| Require investments to deploy appliances and scale                       | Easier to deploy, maintain and scale                                      |
| Have bottleneck issues at VPN gateway causing backhaul and latency       | Do not have this issues                                                   |

# Design
In this section we explain the concept of ZT which was wrote since 2015 in book Zero Trust Network (publis by Ororeilly), one's in NIST Special Publication 800-207 (wrote by engineers of NIST and NCCoE since 2018 and publish by DOI at 8/2020), one's in The UK National Cyber Security Centre (known as UKNCSC).
<!-- and real concept in some big enterprises(Google, Cloudflare, Gitlab) -->

## Making Authorization Decisions [by Gilman and Barth](http://shop.oreilly.com/product/0636920052265.do)
Authorization is the most important process in a Zero trust network. According to the main idea of security Zero trust, for every flow or request that occurs, an authorization decision needs to be made.

These decisions will be made primarily based on a factor called the Trust Score. The thing is calculated based on the data (user data, device data, or otherwise) of the system and the object making the request.

Here we will focus only on the high-level architecture, including the components for making an authorization decision in the Zero trust model.

Authorization Architecture has four main components:
- **Enforcement**: ensures that clients are authenticated (a load balancer, proxy, or even a firewall,...)
- **Policy engine**: compares the request and its context to policy, and informs the enforcer whether the request will be permitted or not.
- **Trust engine**: is used by the policy engine for risk analysis purposes (*Google’s BeyondCorp*)
- **Data stores**: user data, device data, or otherwise, (authenticated) are heavily leveraged by both the policy engine and trust engine

<div style="text-align: center">
    <img src="https://i.imgur.com/LgHMMUK.jpg"/>
</div>
#### Enforcement
Enforcement is the gateway to secure access for corporate resources. This components intercepts the access request and ensures that clients are authenticated.

Then the Enforcement calls the policy engine for authorization. The results returned will determine the next action. Only authorized, trusted requests are allowed to access the resources.

#### Policy engine
Requests forwarded from the Enforcement will be reviewed with policies (*should be stored separately from the engine*) to decide if they are trustworthy for authorization.

Since the policy of a Zero trust System generally expects as much detail as possible and changes reasonably, it is common for organizations to let small groups define their own policies for their services and let security teams review each proposed change.

#### Trust Engine 
Trust Engine will evaluate the riskiness of allowing a particular request/action (which can be represented by **Trust Score**). This result will be used by the Policy engine to make the final authorization decision.

Risk assessment:
- Define a set of ad hoc rules that score an entity’s riskiness.
- Using Machine Learning to calculate Trust Score:  
    - The training data are observations from the behaviors of trusted and untrusted entities. 
    - Model results are compared with human risk assessment and fine-tuned.

#### Data Stores (DS)
Data Stores are the sources of truth for the current and past state of the system.

Zero trust networks two primary types DS:
- Inventor: save the current state of the resources. Eeach entity has an primary key. (Example: User - Primary key is Name)
- Historical: store the data, behavior in the past (use for risk analysis).

#### Implementation
There are a lot of opinions on how a zero-trust network should be implemented. These are the most commonly agreed upon steps:

**1. Identify areas that need to be protected**
In today's threat scenario, working relentlessly to decrease the attack surface is no longer possible. It's impossible to define, reduce, or fight against the assault surface since it's increasing continuously. Instead of focusing on the attack surface's macro level, you determine what you need to protect or keep a eye on. These include:
    - Actors: employees, Service accounts, robotic process automations (RPAs/bots), serverless functions, system administrators, developers.
    - Data: User accounts, digital certificates, credentials, credit card information.
    - Devices: Personal computers, smartphones, tablets, IoT devices (printers, smart security cameras), switches, routers, modems.
    - Application: 
    - Services: DNS, DHCP.

**2. Understand the transaction flows**
Examine and document how different apps interact with one another. Even if you don't have all of the data, your administrators will obtain a clear understanding of where restrictions are required. Understanding how your systems function can help you determine where access controls are needed. To put it another way, know what you're defending before you construct a defense around it.

**3. Establish policies**
You'll need to set Zero Trust rules to whitelist which resources should have access to others after the network is built. The "Kipling Method" or the "‘Who? What? When? Where? Why? How?’" system is a is an effective way to detect if a user or entity meets the requirements for access to your restricted areas.
    - Who should be accessing a resource?
    What application is being used to access a resource inside the protect surface?
    - When is the resource being accessed?
    - Where is the packet destination?
    - Why is this packet trying to access this resource within the protect surface?
    - How is the packet accessing the protect surface via a specific application?
    
**4. Continuous Monitoring**
Documenting as much activity circulating your environment as you can, is at the forefront of what makes Zero Trust effective. Knowledge is power, so this data can provide valuable insights into how to improve the network overtime.


## Eight Core valuable by UKNCSC

<div style="text-align: center">
    <img src="https://i.imgur.com/LK27xtG.png"/>
</div>

This a a set of instruction to help you design and deploy a zero trust architecture.
* Know your architecture including users, devices, and services
<!-- In order to get the benefits from zero trust, you need to know about each component of your architecture. This will allow you to identify where your key resources are, the main risks to your architecture and also avoid any late stage pitfalls integrating legacy services which do not support zero trust. -->
* Know your user, service and device identities
<!-- An identity can represent a user (a human), service (software process) or device. Each identity should be unique and cryptographically identifiable in a zero trust architecture. This is one of the most important factors in deciding whether someone or something should be given access to data or services. -->
* Know the health of your users, devices and services
<!-- User behaviour, and service or device health, are important indicators when looking to establish confidence in the security of your systems, making them important signals for policy engines. Therefore, having the ability to measure user behaviour, device and service health is key in a zero trust architecture. -->
* Use policies to authorise requests
<!-- Each request for data or services should be authorised against a policy. The power of a zero trust architecture comes from the access policies you define. Policies can also help to facilitate risk managed sharing of data or services with guest users or partner organisations. </br>
The policy engine is a key component of the zero trust architecture, it uses multiple signals and provides a flexible and secure access control mechanism that adapts to the resources being requested. -->
* Authenticate everywhere
<!-- Authentication and authorisation decisions should consider multiple signals, such as device location, device health, user identity and status to evaluate the risk associated with the access request. We do this as we assume the network is hostile and want to ensure all connections that access your data or services are authenticated and authorised. -->
* Focus your monitoring on devices and services
<!-- In a zero trust architecture, it is highly likely that your monitoring strategy will change to focus on users, devices and services. Monitoring of these devices, services and users behaviours will help you establish their health. Monitoring should link back to the policies you have set to gain assurance in their configuration. -->
* Don’t trust any network, including your own
<!-- Don't trust any network between the device and the service it's accessing, including the local network. Communications over a network, to access data or services, should use a secure transport protocol to gain assurance that your traffic is protected in transit and less susceptible to threats. </br>
A zero trust architecture changes the way traditional user protections such as malicious website filtering and phishing protection are implemented, these may need to provided by different solutions in your zero trust architecture. -->
* Choose services designed for zero trust
<!-- Services may not support zero trust and thus may require additional resources to integrate and increase support overhead. In these scenarios it may be prudent to consider alternative products and services that have been designed with zero trust in mind. </br>
Using products that utilise standards-based technologies allows for easier integration and interoperability between services and identity providers. -->

More detail: [https://github.com/ukncsc/zero-trust-architecture](https://github.com/ukncsc/zero-trust-architecture)

## Model by NIST 800-207
### Seven tenets

* All data sources and computing services are considered resources
* All communication is secured regardless of network location
* Access to individual enterprise resources is granted on a per-session basis
* Access to resources is determined by dynamic policy—including the observable state of client identity, application/service, and the requesting asset—and may include other behavioral and environmental attributes.
* The enterprise monitors and measures the integrity and security posture of all owned and associated assets
* All resource authentication and authorization are dynamic and strictly enforced before access is allowed.
* The enterprise collects as much information as possible about the current state of assets, network infrastructure and communications and uses it to improve its security posture.


### Six addition assumptions
* The entire enterprise private network is not considered an implicit trust zone (Tenet 2)
* Devices on the network may not be owned or configurable by the enterprise
* No resource is inherently trusted.
* Not all enterprise resources are on enterprise-owned infrastructure
* Remote enterprise subjects and assets cannot fully trust their local network connection. (Tenet 2)
* Assets and workflows moving between enterprise and nonenterprise infrastructure should have a consistent security policy and posture (prepare for migrating system)

### Core ZTN concept
<div style="text-align: center">
    <img src="https://i.imgur.com/e1gi4BF.png"/>
    <b>
        Core Zero Trust Network concept
    </b>
</div>

<!-- TODO: Copy và tóm tắt khái niệm các thành phần vào -->
* Core:
    * PE: Policy Engine
    * PA: Policy Administrator
    * PEP: Policy Enforcement point
* Additional:
    * CDM Systems: Continuous diagnostics and mitigation Systems
    * ICS: Industry compliance system
    * Threat intelligence feed
    * Network and system activity logs
    * Data access policies
    * Enterprise public key infrastructure (PKI)
    * ID management system
    * Security information and event management (SIEM) system

### Trust Algorithm
For an enterprise with a ZTA deployment, the policy engine can be thought of as the brain and the PE’s trust algorithm as its primary thought process. The trust algorithm (TA) is the process used by the policy engine to ultimately grant or deny access to a resource. The policy engine takes input from multiple sources the policy database with observable information about subjects, subject attributes and roles, historical subject behavior patterns,  threat intelligence sources, and other metadata sources. The process can be grouped into broad categories and visualized in Figure below.

<div style="text-align: center">
    <img src="https://i.imgur.com/HZAANE8.png"/>
    <b>
        Trusting Algorithm
    </b>
</div>

* Access request
* Subject database
* Asset database (and observable status)
* Resource requirements
* Threat intelligence

There are different ways to implement a TA but the two type which people usually implement is: 
- **Criteria- versus score-based**
a set of qualified attributes that must be met before access is granted to a resource or an action (e.g., read/write) is allowed. These criteria are configured by the enterprise and should be independently configured for every resourceIf the score is greater than the configured threshold value for the resource, access is granted, or the action is performed. Otherwise, the request is denied, or access privileges are reduced (e.g., read access is granted but not write access for a file).
- **Singular versus contextual**
Treats each request individually and does not take the subject history into consideration when making its evaluation. This can allow faster evaluations, but there is a risk that an attack can go undetected if it stays within a
subject’s allowed role.

### Variations of Zero Trust Architecture Approaches
<div style="text-align: center">
    <img src="https://user-images.githubusercontent.com/68043327/121101807-8eeab600-c7ca-11eb-88c3-b9cb8b5648ea.png"/>
    <b>
        <a href="https://www.nccoe.nist.gov/sites/default/files/legacy-files/zta-project-description-final.pdf">Illustration for variations of ZTA Approaches</a>
    </b>
</div>

- **Enhanced Identity Governance** - Enterprise resource access policies are based on identity and assigned attributes. The primary requirement for resource access is based on the access privileges granted to the given subject. Other factors such as device used, asset status, and environmental factors may alter the final confidence level calculation (and ultimate access authorization) or tailor the result in some way, such as granting only partial access to a given data source based on network location.

  ![image](https://user-images.githubusercontent.com/68043327/121345101-74136100-c8f2-11eb-8caf-a6f6a7c88758.png)
 
- **Logical micro-segmentation** - In this approach, the enterprise places infrastructure devices such as intelligent switches (or routers) or next generation firewalls (NGFWs) or special purpose gateway devices to act as PEPs protecting each resource or small group of related resources. Alternatively (or additionally), the enterprise may choose to implement host-based micro-segmentation using software agents.

  ![image](https://user-images.githubusercontent.com/68043327/121345018-5cd47380-c8f2-11eb-95c4-eec873b29177.png)
 
- **Software Defined Perimeters** - This can be achieved by using an overlay network (i.e., layer 7 but also could be set up lower of the OSI network stack). These approaches are sometimes referred to as software defined perimeter (SDP) approaches and frequently include concepts from Software Defined Networks (SDN).

  ![image](https://user-images.githubusercontent.com/68043327/121345208-92795c80-c8f2-11eb-8269-7dab41088109.png)

### Trust Algorithm Variations
* Criteria- versus score-based
* Singular versus contextual

More detail: [https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-207.pdf](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-207.pdf)

### Six scenarios attacking by NCCOE
Six scenarios of attack network security:
* Employee Access to Corporate Resources
* Employee Access to Internet Resources
* Contractor Access to Corporate and Internet Resources
* Inter-server Communication Within the Enterprise
* Cross-Enterprise Collaboration with Business Partners
* Develop Trust Score/Confidence Level with Corporate Resources

<!-- * - [Use Case 1 - Remote Application Access](https://www.actiac.org/zero-trust-use-case/use-case-1-remote-application-access)
- [Use Case 2 - Digital Worker Access](https://www.actiac.org/zero-trust-use-case/use-case-2-digital-worker-access)
- [Use Case 3 - SOC Improvement](https://www.actiac.org/zero-trust-use-case/use-case-3-soc-improvement)
- [Use Case 4 - Container Isolation / Access](https://www.actiac.org/zero-trust-use-case/use-case-4-container-isolation-access)
- [Use Case 5 - Machine-To-Machine Application Access](https://www.actiac.org/zero-trust-use-case/use-case-5-machine-machine-application-access)
- [Use Case 6 - Secure Operational Technology And Internet Of Things Devices](https://www.actiac.org/zero-trust-use-case/use-case-6-secure-operational-technology-and-internet-things-devices) -->

<div style="text-align: center">
    <img src="https://i.imgur.com/MzZRVrL.png"/>
    <b>
        <a href="https://www.nccoe.nist.gov/sites/default/files/legacy-files/zta-project-description-final.pdf">ZTA High-Level Architecture</a>
    </b>
</div>

More detail: 
[https://www.nccoe.nist.gov/sites/default/files/legacy-files/zta-project-description-final.pdf](https://www.nccoe.nist.gov/sites/default/files/legacy-files/zta-project-description-final.pdf)


### Enterprise 
- The concept of ZTN was implemented in many big enterprise like Google, Cloudflare, Gitlab, Microsoft ... Many other enterprises was implemented zero trust architecture and they're not have more special feature compare to basic ZTN. But if you're interesting, there a few name of companies for your research: 
* HashiCorp: a big company deliver many  security solution in Japan
* Okta: an American identity and access management company, it uses cloud software that helps companies manage and secure user authentication into applications
* ...
  
## Required to transform existing systems to ZTN
- All network flows MUST be authenticated before being processed.
In a zero-trust network, all packets received by the system are immediately suspicious. As such, they must be rigorously inspected before allowing the data contained within them to be processed. Strong authentication is the primary mechanism by which we accomplish this.

Authentication is absolutely required in order to gain confidence in the provenance of network data. It is, perhaps, the single most important component of a zero trust network. Without it, we have nothing and are forced to place trust in the network.

- All network flows SHOULD be encrypted before being transmitted.
A network link cannot be trusted to reliably convey data or signals from one system to another. The physical accessibility of a network link to unsafe actors makes it trivial for that network to be compromised. Moreover, even in a physically secure network, bad actors can digitally infiltrate a system and passively probe the network for valuable data.

By encrypting data on a device before transmitting it on the network, we reduce the attack surface of that communication to the trustworthiness of the device itself, namely application and physical device security.

- Authentication and encryption MUST be performed by the endpoints in the network.
Since zero trust networks recognize the threat that trusting network links pose to the security of a system, it is important that secure communications be established between application-layer endpoints. Adding middleware components that handle these responsibilities (like VPN concentrators or TLSterminating load balancers) can leave upstream network communications exposed to physical and virtual threats.

As a result, a system that claims to be zero trust is required to implement encryption and authentication at every application-layer endpoint on the network.

- All network flows MUST be enumerated so that access can be enforced by the system.
Zero trust networks depend on data that defines the expected characteristics of the network. Therefore, defining every expected network flow is critical to safeguarding the network.

We should be careful to note that enumerating flows does not require onerous change management controls to provide value. A simple process for defining expected flows brings enormous value in terms of network enforcement and change auditing.

Without the list of expected network flows, zero trust systems are unable to highlight unexpected communications which need attention from administrators or should be denied.

- The strongest authentication and encryption suites SHOULD be used within the network.
Zero trust networks assume a hostile network environment, so strong authentication and encryption suites are an important component in the security of a zero trust network.

Which suites offer strong security unfortunately changes so system administrators should always aim for the strongest suites possible, but device and application capabilities might limit the types of suites that are available. In these cases, administrators should be aware that by reducing the strength of these suites, security is being compromised in their network.

- Devices SHOULD be regularly scanned, patched, and rotated.
Scanning can be used to discover and prevent known malicious software from running on the device. Keeping devices fresh is also very important. System administrators should have a plan for regularly installing the latest security patches. Additionally, a regular device rotation will help ensure that devices don’t accrue cruft, which can compromise the security of that system.


## Further reading

The zero-trust security model was first articulated by [John Kindervag](http://www.virtualstarmedia.com/downloads/Forrester_zero_trust_DNA.pdf) in 2010, and by Google in 2011 as a result of the [Operation Aurora](https://en.wikipedia.org/wiki/Operation_Aurora) breach. What follows is a curated list of resources that covers the topic in more depth.

**Recommendations**

- Jericho Forum™ Commandments - [Jericho Forum™ Commandments](https://static.spiceworks.com/attachments/post/0016/4842/commandments_v1.2.pdf) - May 2007
- NCCoE [IMPLEMENTING A ZERO TRUST ARCHITECTURE](https://www.nccoe.nist.gov/sites/default/files/library/project-descriptions/zta-project-description-final.pdf) - Oct 2020
- UK National Cyber Security Centre [Zero trust architecture design principles](https://github.com/ukncsc/zero-trust-architecture/) - 2020
- NIST SP 800-207 [Zero Trust Architecture](https://doi.org/10.6028/NIST.SP.800-207) - August 2020
- Department of Defense (DOD) [Enterprise Identity, Credential, and Access Management (ICAM) Reference Design](https://dodcio.defense.gov/Portals/0/Documents/Cyber/DoD_Enterprise_ICAM_Reference_Design.pdf) Aug 2020
- Department of Defense (DOD) [Zero Trust Reference Architecture](https://dodcio.defense.gov/Portals/0/Documents/Library/(U)ZT_RA_v1.1(U)_Mar21.pdf) - February 2021
- ACT-IAC [Zero Trust Project Briefing](https://www.actiac.org/document/zero-trust-project-briefing) - May 2021
- ACT-IAC [ZERO TRUST REPORT: LESSONS LEARNED FROM VENDOR AND PARTNER RESEARCH](https://www.actiac.org/document/zero-trust-report-lessons-learned-vendor-and-partner-research) - May 2021



**Books**

- [Zero Trust Networks](http://shop.oreilly.com/product/0636920052265.do) by Gilman and Barth
- [Zero Trust Security](https://www.apress.com/us/book/9781484267011) by Garbis and Chapman

**Papers**

- Forrester [Build Security Into Your Network's DNA: The Zero Trust Network Architecture](http://www.virtualstarmedia.com/downloads/Forrester_zero_trust_DNA.pdf)
- Google BeyondCorp 1 [An overview: "A New Approach to Enterprise Security"](https://research.google.com/pubs/pub43231.html)
- Google BeyondCorp 2 [How Google did it: "Design to Deployment at Google"](https://research.google.com/pubs/pub44860.html)
- Google BeyondCorp 3 [Google's front-end infrastructure: "The Access Proxy"](https://research.google.com/pubs/pub45728.html)
- Google BeyondCorp 4 [Migrating to BeyondCorp: Maintaining Productivity While Improving Security](https://research.google.com/pubs/pub46134.html)
- Google BeyondCorp 5 [The human element: "The User Experience"](https://research.google.com/pubs/pub46366.html)
- Google BeyondCorp 6 [Secure your endpoints: "Building a Healthy Fleet"](https://ai.google/research/pubs/pub47356)
- ACT-IAC [Zero Trust Cybersecurity Current Trends 2019](https://www.actiac.org/system/files/ACT-IAC%20Zero%20Trust%20Project%20Report%2004182019.pdf)
- Microsoft [Zero Trust Maturity Model](https://go.microsoft.com/fwlink/p/?LinkID=2109181&clcid=0x409&culture=en)
- Microsoft [Implementing a Zero Trust security model at Microsoft](https://www.microsoft.com/en-us/itshowcase/implementing-a-zero-trust-security-model-at-microsoft#printpdf)
- Palo Alto [Zero Trust Deployment at Palo Alto Networks](https://www.paloaltonetworks.com/apps/pan/public/downloadResource?pagePath=/content/pan/en_US/resources/use-case/zero-trust-deployment-at-palo-alto-networks)
- Okta [Getting Started with Zero Trust](https://www.okta.com/sites/default/files/2021-02/WPR_Getting-Started-With-Zero-Trust.pdf)
- Duo [Zero Trust: Going Beyond the Perimeter](https://duo.com/assets/ebooks/zero-trust-going-beyond-the-perimeter.pdf)

**Posts**
- Blog [Zero Trust Network Note](https://github.com/shreyasudupi/Zero-Trust)
- [Zero Trust vs VPN vs SDP](https://cyolo.io/blog/zero-trust-vs-vpn-vs-sdp-which-one-should-you-choose/)
- [SDP vs VPN vs ZTN and whats the difference](https://www.techtarget.com/searchnetworking/feature/SDP-vs-VPN-vs-zero-trust-networks-Whats-the-difference)
- [Zero Trust 5 step methodology](https://www.embroker.com/blog/cyber-attack-statistics/)
[https://www.paloaltonetworks.com/cyberpedia/zero-trust-5-step-methodology](https://)
- Google [How Google adopted BeyondCorp](https://security.googleblog.com/2019/06/how-google-adopted-beyondcorp.html)
- Wall Street Journal [Google Moves Its Corporate Applications to the Internet](https://blogs.wsj.com/cio/2015/05/11/google-moves-its-corporate-applications-to-the-internet/)
- Gitlab's [Blog series](https://about.gitlab.com/blog/tags.html#zero-trust) and their [reddit AMA](https://www.reddit.com/r/netsec/comments/d71p1d/were_a_100_remote_cloudnative_company_and_were/)
- Microsoft Azure [Implementing zero trust with microsoft azure identity and access management 1 of 6](https://devblogs.microsoft.com/azuregov/implementing-zero-trust-with-microsoft-azure-identity-and-access-management-1-of-6/)
- Microsoft Azure [Protecting cloud workloads for zero trust with azure security 2 of 6](https://devblogs.microsoft.com/azuregov/protecting-cloud-workloads-for-zero-trust-with-azure-security-center-2-of-6/)
- Microsoft Azure [Monitoring cloud security for zero trust with azure sentinel 3 of 6](https://devblogs.microsoft.com/azuregov/monitoring-cloud-security-for-zero-trust-with-azure-sentinel-3-of-6/)
- Microsoft Azure [Enforcing policy for zero trust with azure policy 4 of 6](https://devblogs.microsoft.com/azuregov/enforcing-policy-for-zero-trust-with-azure-policy-4-of-6/)
- Microsoft Azure [Implementing Zero Trust with Microsoft Azure 5 of 6](https://devblogs.microsoft.com/azuregov/insider-threat-monitoring-for-zero-trust-with-microsoft-azure-5-of-6/)
- Microsoft Azure [Implementing Zero Trust with Microsoft Azure 6 of 6](https://devblogs.microsoft.com/azuregov/supply-chain-risk-management-for-zero-trust-with-microsoft-azure-6-of-6/)
- NCCoE [Zero Trust Architecture Technical Exchange Meeting](https://www.nccoe.nist.gov/events/zero-trust-architecture-technical-exchange-meeting)
- [Zero-trust ldap wiki](https://ldapwiki.com/wiki/Zero%20Trust)
- [Adopt a Zero Trust approach for security — Essentials Series — Episode 1](https://techcommunity.microsoft.com/t5/microsoft-mechanics-blog/adopt-a-zero-trust-approach-for-security-essentials-series/ba-p/2348890)

**Videos**

- [USENIX Enigma 2016 - NSA TAO Chief on Disrupting Nation State Hackers](https://youtu.be/bDJb8WOJYdA?list=PLKb9-P1fRHxhSmCy5OaYZ5spcY8v3Pbaf)
- [What, Why, and How of Zero Trust Networking](https://youtu.be/eDVHIfVSdIo?list=PLKb9-P1fRHxhSmCy5OaYZ5spcY8v3Pbaf) by Armon Dadgar, Hashicorp
- [O'Reilly Security 2017 NYC Beyondcorp: Beyond Fortress Security](https://youtu.be/oAvDASLehpY?list=PLKb9-P1fRHxhSmCy5OaYZ5spcY8v3Pbaf) by Neal Muller, Google
- [How Google Protects Its Corporate Security Perimeter without Firewalls](https://youtu.be/d90Ov6QM1jE)
- [LISA17 - Clarifying Zero Trust: The Model, the Philosophy, the Ethos](https://youtu.be/Gi0oedg_UrM)
- [Be Ready for BeyondCorp: enterprise identity, perimeters and your application](https://youtu.be/5UiWAlwok1s?list=PLKb9-P1fRHxhSmCy5OaYZ5spcY8v3Pbaf) by Jason Kent
- [SANS Webcast - Trust No One: Introducing SEC530: Defensible Security Architecture](https://youtu.be/Q6yFqLmlcGo) - 2018
- [Google on BeyondCorp: Empowering Employees with Security for the Cloud Era](https://youtu.be/34VTSI_vgI0)
- [Zero-Trust Networks: The Future Is Here](https://www.youtube.com/watch?v=EF_0dr8WkX8) - SANS Blue Team Summit 2019
- Microsoft [No More Firewalls! How Zero-Trust Networks Are Reshaping Cybersecurity](https://www.youtube.com/watch?v=pyyd_OXHucI) - RSA Conference 2019
- [The Fallacy of the "Zero-Trust Network"](https://www.youtube.com/watch?v=tFrbt9s4Fns) - RSA Conference 2019
- [Zero-Trust Cybersecurity: Trust No One?](https://youtu.be/ooAPzzYkyaE)
- SANS Could Security - Gigamon [Zero Trust What You Need to Know to Secure Your Data and Networks](https://youtu.be/iZ-9lbaFwqI)
- [A Simplified and Practical Approach to Pursuing a Zero Trust Architecture](https://www.youtube.com/watch?v=A32ZwFjXyWU) - RSA Conference 2020
- [Using SABSA to Architect Zero Trust Networks - COSAC Connect #1](https://youtu.be/WXoG9ETfJnk)
- [ACT-IAC Zero Trust Briefing](https://www.youtube.com/watch?v=XIxeXMqT23M) - May 2021
<!--  --><!--  --><!-- 

**Product Vendor Videos**
- Cisco 2020 [How to approach a Zero Trust security model Cisco](https://www.youtube.com/watch?v=6q6c0Ld0qx0)
- Microsoft 2020 [Modern Security w/ End-to-End Zero Trust Strategy](https://youtu.be/8Hx6aSJjpco)
- Palo Alto Networks 2020 [Zero Trust: The Strategic Approach to Stop Data Breaches](https://www.youtube.com/watch?v=MxiuCXNCzFI)
- Palo Alto - John Kindervag 2020 [Implementing Best Practices for Zero Trust](https://www.youtube.com/watch?v=-ld2lfz6ytU)
- Microsoft's approach to Zero Trust 2020 [How Microsoft does Zero Trust](https://www.youtube.com/watch?v=bZCH4nkNP34)
- Okta 2021 [Moving the Zero Trust Maturity Needle -  University of Newcastle](https://www.youtube.com/watch?v=YCScMCRM8Io)
- Microsoft Mechanics Playlist 2021 [Zero Trust Essentials](https://youtube.com/playlist?list=PLXtHYVsvn_b_P09Jqw65XvV0zp6HP2liu)
- Security Architect Podcast [SASE ZeroTrust - Remote Access](https://youtube.com/playlist?list=PL3fwn2_OBVs39WMmTsVfSzobZ2P9JBLwz)
- Security Architect Podcast [SASE Secure Web Gateway - Outbound browsing](https://youtube.com/playlist?list=PL3fwn2_OBVs1FbmVCy7YZRZcBI5eyi0Av)
- VMware 2021 [Trusting Zero-Trust: How VMware IT Reimagined Security and Resiliency](https://www.youtube.com/watch?v=h6zhm9UskSU)


**Materials**
- [Video demo](https://youtu.be/MRH82bqhVek)
- [Slide](https://studenthcmusedu-my.sharepoint.com/:f:/g/personal/19120690_student_hcmus_edu_vn/Eifo2yjRoX5Lkve9IFdtj_UBnjaz0UHDyZRC4EhU4w3BMg?e=7h5AGC)

## Contributor

- [maxminlevel](https://github.com/maxminlevel)
- [pbc781](https://github.com/pbcuong781)
- [Zero-nnkn](https://github.com/Zero-nnkn)
- [binhlecong](https://github.com/binhlecong)
