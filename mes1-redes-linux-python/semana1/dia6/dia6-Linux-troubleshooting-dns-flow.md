# Day 6 – Linux Networking Troubleshooting & DNS Flow

## Objectives
- Consolidate a professional troubleshooting workflow for networking issues
- Understand DNS as a multi-layer system with multiple caching points
- Practice diagnostic commands focusing on connectivity, routing, DNS, and application behavior
- Strengthen reasoning by isolating problems by OSI layers

---

## Summary
Day 6 focused on **systematic network troubleshooting**, emphasizing DNS resolution, ICMP connectivity, routing validation, and application-layer diagnostics.

Rather than executing commands mechanically, the goal was to **think like an engineer**: isolate layers, validate assumptions, and determine whether an issue is local, network-related, DNS-related, or server-side.

DNS received special attention because it is one of the most common real-world failure points and spans multiple layers, from application-level resolvers to network transport.

---

## Task 1 – DNS Caching and Resolution Flow

DNS is generally classified as an **application-layer protocol**, but in practice it spans multiple layers due to caching and transport mechanisms.

When a DNS-related issue occurs, cached data may exist in several locations:

1. **Browser cache** – Some browsers store DNS records locally
2. **Operating system resolver cache** – Managed by the system resolver libraries
3. **Local configuration files** – Such as `/etc/hosts` and resolver configuration under `/etc`
4. **Router / ISP DNS cache** – Intermediate recursive resolvers
5. **Authoritative DNS servers**

A common issue arises when a domain changes its IP address but cached records remain valid until the TTL expires. This can cause clients to resolve outdated IP addresses.

### Possible Solutions
- Manually flush or override DNS resolution locally
- Modify local resolver configuration to query a different DNS server
- Wait until the TTL expires and caches refresh automatically (can take up to 24 hours)

This explains why DNS issues can persist even when the authoritative configuration is already correct.

---

## Task 2 – Network State Verification

### Interface Validation
Using `ip addr`, the active network interface can be confirmed along with its assigned IP address and subnet mask.  
A `/24` subnet mask is common in home and lab environments.

### Routing Table
Using `ip route`, the default gateway is identified. The gateway IP must belong to the same subnet as the active interface.

Additional routes may appear that are used for internal network communication without traversing the router.

### DNS Resolver Behavior
In this environment, DNS resolution is handled by **NetworkManager**, not systemd-resolved.  
The resolver configuration is inspected directly via the local DNS configuration file. No persistent local DNS cache is present.

### Services and Ports
Using `ss -tulnp`, no listening services are observed. This is expected behavior for a security-focused OS such as Kali Linux, which minimizes exposed services by default.

---

## Task 3 – Troubleshooting a Website That Does Not Load

A structured troubleshooting workflow was defined and applied.

### Step 1 – Verify Local Network State
```bash
ip addr
Ensure at least one interface is in the UP state and has a valid IP address.

Step 2 – Verify Connectivity
bash
Copy code
ping <destination>
ICMP is used to verify basic IP connectivity.
A failure here does not always imply a full outage, but success confirms reachability at the network layer.

Step 3 – Verify Routing
bash
Copy code
ip route
Confirm the presence of a default gateway and valid routing paths.

Step 4 – Verify DNS Resolution
bash
Copy code
dig domain.com
If DNS resolution fails:

Clear caches if applicable

Re-query

Test alternative DNS servers

Step 5 – Application Layer Testing
bash
Copy code
curl domain.com
At this stage:

HTTP status codes are analyzed

Headers and redirections are inspected

Server behavior is validated

If connectivity and DNS are correct but the resource fails to load, the issue is likely:

An internal server error

A moved resource (HTTP 301/302)

An application-level problem outside the client’s control

Core Troubleshooting Flow (Playbook)
text
Copy code
1. Verify network interface state
2. Verify IP connectivity (ICMP)
3. Verify routing and gateway
4. Verify DNS resolution
5. Clear caches and re-test DNS if needed
6. Test application-layer behavior
7. Analyze HTTP responses
8. Conclude whether the issue is local, network, DNS, or server-side
This flow mirrors real-world L2/L3 and DevOps troubleshooting practices.

Key Takeaways
DNS is a frequent failure point due to distributed caching

ICMP verifies connectivity, not application availability

HTTP responses vary by method and protocol

Structured reasoning prevents wasted effort and false assumptions

Layer-by-layer isolation is the foundation of professional troubleshooting

Extended Notes (Notion)
Additional theory, mental models, and personal annotations are documented in Notion:
👉 https://www.notion.so/D-a-6-Diagn-stico-Troubleshooting-2e3e10fabb288054a70dc0691fb3ba23