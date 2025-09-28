
# Step 0: Check Basic Network Interface

## Why
Before diving into pings, DNS lookups, or higher-level troubleshooting, always confirm your device’s network foundation:

- A device needs a valid IP address and a correct default gateway to communicate outside itself.
- If the interface has no IP, wrong subnet, or no default route, then all advanced tests will fail (e.g., ping google.com will not even leave your PC).

Think of it like a car: before checking GPS or traffic, confirm the car actually has fuel + engine running.

---

## When
Perform this check as your first action whenever:

- The system cannot connect to the internet.
- Applications fail with errors like **“network unreachable”** or **“host not found”**.
- You are unsure whether the network card (Ethernet/Wi-Fi) is even active.
- You just connected to a new Wi-Fi or plugged in Ethernet, but not sure if DHCP assigned an address.

---

## Commands
```bash
ip addr      # Shows network interfaces + IPv4/IPv6 addresses
ip route     # Displays routing table + default gateway
```

### Differences & Alternatives
- **ifconfig** → Old (net-tools). Shows basic info. Missing advanced flags (carrier, state, etc).  
- **route -n** → Legacy route viewer. Fast for checking gateways. But `ip route` shows per-interface metrics, proto source.

---

## Practical Example (from this machine)

### Interfaces → `ip addr`
```
1: lo: <LOOPBACK,UP,LOWER_UP> ...
   inet 127.0.0.1/8 scope host lo
   inet6 ::1/128 scope host

2: enp3s0: <NO-CARRIER,BROADCAST,MULTICAST,UP> ...
   link/ether f0:2f:74:23:fe:ca  (Ethernet port, no cable connected)

3: wlp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> ...
   link/ether d8:c0:a6:d8:ca:bb  (Wi-Fi MAC)
   inet 172.20.10.3/28 scope global dynamic  
   inet6 … (various IPv6 addresses)
```

### Detailed Breakdown
- **lo (Loopback)**
  - `127.0.0.1/8` → reserved block. All traffic stays inside your PC.
  - Always UP, cannot be disabled normally.
  - Used for apps talking to themselves (e.g., web servers, databases).

- **enp3s0 (Ethernet)**
  - `NO-CARRIER` → No cable or inactive switch port.
  - “Carrier” = Layer 1 electrical signal from switch.
  - Even though UP, without carrier it’s like a phone off-hook with no line.
  - Flags: BROADCAST, MULTICAST → Interface supports ARP, DHCP, IPv6 multicast.
  - `link/ether f0:2f:74...` → MAC address (unique hardware ID).

- **wlp1s0 (Wi-Fi)**
  - Flags:
    - UP → Software enabled.
    - LOWER_UP → Hardware/physical link active (associated with AP).
    - BROADCAST → Can send broadcast packets (needed for DHCP).
    - MULTICAST → Can join multicast groups (needed for IPv6, streaming).
  - `inet 172.20.10.3/28` → IPv4 config:
    - Host address: 172.20.10.3
    - Subnet mask: `/28` (255.255.255.240)
    - Range: 172.20.10.0 – 172.20.10.15
    - `.0` = network, `.15` = broadcast → usable: `.1 – .14`
    - Router = `.1`, your PC = `.3`
  - `scope global` → Reachable outside host (real LAN IP).
  - `dynamic` → Assigned by DHCP.
  - IPv6 → Often multiple addresses (link-local + global).

This confirms DHCP worked, Wi-Fi is active, and you belong to a valid LAN.

---

### Routes → `ip route`
```
default via 172.20.10.1 dev wlp1s0 proto dhcp src 172.20.10.3 metric 600
172.20.10.0/28 dev wlp1s0 proto kernel scope link src 172.20.10.3 metric 600
```

### Detailed Breakdown
- **Default Route**
  - `default via 172.20.10.1` → Any traffic not matching local subnet is forwarded here (the router).
  - Equivalent of: “Send unknown mail to the post office.”
  - `dev wlp1s0` → Route bound to Wi-Fi card.
  - `proto dhcp` → This route came from DHCP (not manual config).
  - `src 172.20.10.3` → Packets sent with this IP as source.
  - `metric 600` → Route priority. Lower = higher priority.
    - If Ethernet was active, it might have a lower metric (say 100), so system prefers wired.

- **Local Subnet Route**
  - `172.20.10.0/28` → LAN range.
  - `scope link` → Reachable directly on link (no router needed).
  - `proto kernel` → Auto-added by Linux when interface got IP.

---

## Tips & Practical Checks
- **state UP vs DOWN**
  - UP = software enabled
  - DOWN = disabled (like turning off network card)

- **LOWER_UP vs NO-CARRIER**
  - LOWER_UP = physical connection present
  - NO-CARRIER = no link (cable unplugged / Wi-Fi not associated)

- **Verify default gateway**
  - Must be inside your subnet (here 172.20.10.1 in /28 is valid).
  - If wrong, packets can’t escape LAN.

- **Check multiple interfaces**
  - Sometimes Ethernet + Wi-Fi both active → wrong metric causes traffic to exit wrong interface.

- **Subnet mask sanity check**
  - `/28` = 16 addresses (small but fine).
  - If mismatch between PC and router, communication fails.

---

## Step 0 Result
Your Wi-Fi interface (`wlp1s0`) is:

- **UP** (software) + **LOWER_UP** (hardware link active)  
- Assigned a **valid IPv4 lease** (172.20.10.3/28)  
- Default route points to router (172.20.10.1)  
- System knows both:
  - Local subnet → 172.20.10.0/28
  - Where to send outside traffic → router at .1  

This means your device is fully ready at **Layer 3 (Network layer)**.  
Next logical step → **Step 1: Ping local gateway to confirm connectivity.**
