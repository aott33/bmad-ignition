# Ignition System Architectures (8.1)

Reference for choosing and designing Ignition Gateway deployment architectures. All patterns below apply to Ignition 8.1. Gateway Network is the backbone for multi-Gateway communication in all multi-server patterns.

---

## Architecture Decision Guide

| Need | Architecture |
|---|---|
| Single site, simple deployment | Basic |
| Add HA to any architecture | + Redundancy overlay |
| Multiple sites, centralized data | Hub-and-Spoke |
| High concurrent client load | Scale-Out (front/back-end split) |
| Many Gateways to manage centrally | Enterprise (EAM) |
| Network edge / IoT / offline-capable | Edge |
| Reduce on-premise IT overhead | Cloud-Based |
| Security isolation (OT/IT) | Security Architecture patterns |

Architectures compose: a large enterprise deployment typically combines Hub-and-Spoke + Redundancy + Scale-Out + EAM.

---

## Basic Architecture

**Use when:** Single site, straightforward operational requirements, limited client load.

- One Ignition Gateway: PLC comms, tag execution, history, client connections
- Can use dual-NIC server to bridge OT network and corporate IT network
- Clients from different networks can access same resources with role-based restrictions
- Starting point — scale out or add Hub-and-Spoke when growth demands it

**Limitation:** Single point of failure; not suitable for highly distributed or high-client-count environments.

---

## Redundancy Architecture

**Use when:** Mission-critical systems cannot tolerate downtime.

- Two Ignition installations: **Primary** + **Backup** Gateway pair
- Automatic failover in ~20 seconds — clients reconnect automatically
- Historical logging, tag execution, and scripts continue uninterrupted on failover
- Backup-specific license is discounted (not full platform license)
- **Can be overlaid on any other architecture type** (each Gateway in a Hub-and-Spoke can have its own backup)
- Preferred over cloud-provider HA — Ignition failover (~20s) is faster than cloud instance restart (several minutes)

**Caveats:**
- Single internet connection remains a single point of failure in cloud deployments
- Redundancy does NOT eliminate the need for proper architecture — it adds resilience to an existing design

---

## Scale-Out Architecture

**Use when:** High concurrent client load, or need to isolate I/O workload from client workload.

Splits responsibilities across Gateway types connected via **Gateway Network**:

| Gateway Type | Responsibilities |
|---|---|
| **Back-End Gateway** | PLC comms, OPC-UA, tag execution, history recording, Remote Tag Providers |
| **Front-End Gateway** | Perspective client connections, Reporting — no direct PLC connections |

- Load balancers distribute clients across multiple front-end Gateways
- Add a back-end Gateway when adding new PLCs/devices
- Add a front-end Gateway when adding concurrent clients
- Fault isolation: a failing front-end doesn't affect data collection; a failing back-end doesn't take down other back-ends
- Multiple databases can be clustered for additional data tier scaling

**Tag sharing:** Back-end Gateways expose tags via **Remote Tag Providers** consumed by front-ends.

---

## Hub-and-Spoke Architecture

**Use when:** Multiple remote sites each need local autonomy plus centralized data aggregation.

| Component | Role |
|---|---|
| **Hub Gateway** | Aggregates data from all spokes, central reporting, enterprise dashboards |
| **Spoke Gateway** | Local PLC connections, local history, local Perspective clients for fallback |

Key capabilities:
- **Store-and-Forward**: Spokes buffer data locally during hub connectivity loss; replay when connection restores — no data gaps
- **Local fallback**: Spoke operators retain full visibility/control even if hub is unreachable
- **Remote alarming**: Spoke alarms route through hub via Alarming Notification module
- **Split historian**: Tags can log locally at spoke AND centrally at hub for redundant history

**Tag addressing:** Hub accesses spoke tags via Remote Tag Provider: `[SpokeAlias]path/to/tag`

---

## Edge Architectures

**Use when:** Deploying at network periphery — field devices, IoT, or machines requiring standalone capability.

Two distinct editions (can be combined):

### Panel Edition
- Standalone Ignition at machine/panel level
- Local PLC connections, local Perspective clients
- Acts as "Spoke" in Hub-and-Spoke: publishes data to central Hub via Edge Sync Services
- Local client fallback when Hub connectivity drops
- Data storage: up to 35 days / 10 million data points of Tag History

### IIoT Edition
- MQTT publisher next to PLCs — Sparkplug B protocol
- Integrates into broader IoT infrastructure (MQTT Broker, cloud)
- Multi-device connectivity
- Alarm Journal and Audit Log limited to 7 days

**Edge EAM**: Allows central Enterprise Gateway to manage multiple Edge devices.

---

## Enterprise Architecture

**Use when:** Managing many Ignition Gateways centrally — project synchronization, monitoring, alerts.

- **Controller Gateway**: Designated master server, uses Enterprise Administration Module (EAM)
- **Agent Gateways**: Secondary servers reporting to Controller
- Capabilities:
  - Push standardized project updates to all Agent Gateways automatically
  - Monitor all agents from one console, generate alarms on agent failure
  - Agent events recorded in SQL for reporting
  - Facilitate recovery from hardware failures
- Typically combined with Hub-and-Spoke or Scale-Out for data architecture

---

## Cloud-Based Architecture

**Use when:** Reducing on-premise IT infrastructure burden; smaller organizations without dedicated OT IT.

- Ignition Gateway and database hosted in cloud (AWS EC2, Azure)
- Edge devices placed locally near PLCs forward data to cloud via MQTT or OPC-UA
- Internet dependency: design for connectivity loss scenarios
- **Use Ignition Redundancy** (not cloud provider HA) — ~20s failover vs several minutes for cloud instance restart
- Follow Ignition Security Hardening Guide — internet-exposed Gateway requires hardening

**Network considerations:**
- PLCs connect via secure VPN, cellular, or dedicated WAN
- Optional on-site backup database for buffering during outages
- Single internet link = single point of failure; architect accordingly

---

## Security Architecture Patterns

**Framework alignment:** ISA 62443, NIST CSF, Zero Trust — apply appropriate framework for your regulatory environment.

### Stateless vs Stateful Service Separation

| Service Type | Gateways | Characteristics |
|---|---|---|
| **Stateless (user-facing)** | Front-End Gateways hosting Perspective, Reporting | Load-balanced, horizontally scalable, can be identically configured |
| **Stateful (machine-facing)** | I/O Gateways managing PLCs, historians, databases | Segmented from internet/user access, scale by adding distinct Gateways |

### Network Zone Model (IEC 62443)

```
Level 4-5: Enterprise Network (ERP, corporate IT)
           ↕ Firewall / DMZ
Level 3.5: DMZ — Ignition Gateway, historian
           ↕ Firewall
Level 2:   Control Network — SCADA clients, HMI stations
           ↕ Firewall
Level 1:   Field Network — PLCs, controllers
Level 0:   Safety Network — SIS (isolated, no SCADA connection)
```

- Ignition Gateway sits in DMZ (Level 3.5)
- OPC-UA connections go DOWN to Level 1/2
- Client/web connections go UP to Level 4-5
- **Never** create direct connections between OT and business network without DMZ routing
- Dual-NIC servers can bridge networks with controlled protocols

### SIS Boundary
- Safety Instrumented Systems are on an isolated network
- SCADA does NOT control SIS — read-only status monitoring at most
- Any design touching SIS scope: certified safety engineer review + MOC documentation required

---

## Scan Classes

Scan classes define OPC polling rates for tag groups. Key design decisions:

| Class | Typical Rate | Use for |
|---|---|---|
| Fast | 100–500ms | Process control values, operator interaction |
| Default | 1s | Standard process monitoring |
| Slow | 10–60s | Equipment status, low-change values |
| Expression | Event-driven | Calculated tags, derived values |

**Design rules:**
- Don't assign everything to Default — excessive polling degrades OPC server performance
- High-frequency scan classes require more Gateway CPU and OPC server resources
- Match scan rate to process dynamics, not operator preference
- History logging rate is independent of scan class rate

---

## Gateway Network Configuration

The backbone for all multi-Gateway architectures:

- Defined in Gateway web UI: Config → Gateway Network
- Each Gateway connection requires: remote Gateway address, TLS certificates, connection type (incoming/outgoing)
- Used for: Remote Tag Providers, distributed history, EAM agent connections, Store-and-Forward
- Secure by default: mutual TLS between Gateways
