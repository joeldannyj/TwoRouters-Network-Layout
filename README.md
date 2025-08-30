# 🌐 Building a Multi-Network Topology with Cisco 1941 Routers
A beginner-friendly guide to creating a complex network in Cisco Packet Tracer.

---

## 📋 What We're Building
We’re creating **two office sites** connected together. Each site has a router with two separate local networks.


---

## 🛠️ Step 1: Gather Your Equipment
1. **Open Cisco Packet Tracer**
2. **Add 2 Routers:**
   - Choose the `1941` model → Place `Router0` and `Router1`
3. **Add 4 Switches:**
   - Choose the `2960` model → Place `Switch0–Switch3`
4. **Add 9 PCs:**
   - Add `PC0, PC1, PC2, PC3, PC4, PC5, PC6, PC7`

---

## 🔧 Step 2: Add Serial Ports to Routers
1. Turn off both routers (power button on side).
2. Go to **Physical tab** → drag `HWIC-2T` module into empty slot.
3. Do this for **both routers**.
4. Power them back on.

---

## 🔌 Step 3: Connect Everything Together

### Site 1 (Left Side)
- **Computers to Switches**
  - `PC0 → Switch0 (Fa0/1)`
  - `PC1 → Switch0 (Fa0/2)`
  - `PC2 → Switch1 (Fa0/1)`
  - `PC3 → Switch1 (Fa0/2)`
- **Switches to Router**
  - `Switch0 (Fa0/24) → Router0 G0/0`
  - `Switch1 (Fa0/24) → Router0 G0/1`

### Site 2 (Right Side)
- **Computers to Switches**
  - `PC4 → Switch2 (Fa0/1)`
  - `PC5 → Switch2 (Fa0/2)`
  - `PC6 → Switch3 (Fa0/1)`
  - `PC7 → Switch3 (Fa0/2)`
- **Switches to Router**
  - `Switch2 (Fa0/24) → Router1 G0/0`
  - `Switch3 (Fa0/24) → Router1 G0/1`

### Connect the Two Sites
- Use **Serial DCE cable**:
  - `Router0 S0/1/0 → Router1 S0/1/0`  
  *(Clock icon ⏰ must be on Router0 side)*

---

## 📊 Step 4: IP Address Planning

### Site 1 – Router0
| Device        | IP Address      | Subnet Mask     | Default Gateway |
|---------------|----------------|----------------|----------------|
| Router0 G0/0  | 192.168.10.1   | 255.255.255.0  | -              |
| Router0 G0/1  | 192.168.11.1   | 255.255.255.0  | -              |
| Router0 S0/1/0| 10.0.0.1       | 255.255.255.252| -              |
| PC0           | 192.168.10.10  | 255.255.255.0  | 192.168.10.1   |
| PC1           | 192.168.10.11  | 255.255.255.0  | 192.168.10.1   |
| PC2           | 192.168.11.10  | 255.255.255.0  | 192.168.11.1   |
| PC3           | 192.168.11.11  | 255.255.255.0  | 192.168.11.1   |

### Site 2 – Router1
| Device        | IP Address      | Subnet Mask     | Default Gateway |
|---------------|----------------|----------------|----------------|
| Router1 G0/0  | 192.168.20.1   | 255.255.255.0  | -              |
| Router1 G0/1  | 192.168.21.1   | 255.255.255.0  | -              |
| Router1 S0/1/0| 10.0.0.2       | 255.255.255.252| -              |
| PC4           | 192.168.20.10  | 255.255.255.0  | 192.168.20.1   |
| PC5           | 192.168.20.11  | 255.255.255.0  | 192.168.20.1   |
| PC6           | 192.168.21.10  | 255.255.255.0  | 192.168.21.1   |
| PC7           | 192.168.21.11  | 255.255.255.0  | 192.168.21.1   |

---

## 💻 Step 5: Configure the Computers
For each PC:
- Go to **Desktop → IP Configuration**
- Set to **Static**
- Enter the values from the table above.

---

## 🖥️ Step 6: Configure Router0
```bash
enable
configure terminal

! LAN for Switch0
interface g0/0
 ip address 192.168.10.1 255.255.255.0
 no shutdown
exit

! LAN for Switch1
interface g0/1
 ip address 192.168.11.1 255.255.255.0
 no shutdown
exit

! WAN Link (DCE)
interface s0/1/0
 ip address 10.0.0.1 255.255.255.252
 clock rate 64000
 no shutdown
exit

! Static Routes to Site 2
ip route 192.168.20.0 255.255.255.0 10.0.0.2
ip route 192.168.21.0 255.255.255.0 10.0.0.2

exit
write memory
```
## 🖥️ Step 7: Configure Router1
```bash
enable
configure terminal

! LAN for Switch2
interface g0/0
 ip address 192.168.20.1 255.255.255.0
 no shutdown
exit

! LAN for Switch3
interface g0/1
 ip address 192.168.21.1 255.255.255.0
 no shutdown
exit

! WAN Link (DTE)
interface s0/1/0
 ip address 10.0.0.2 255.255.255.252
 no shutdown
exit

! Static Routes to Site 1
ip route 192.168.10.0 255.255.255.0 10.0.0.1
ip route 192.168.11.0 255.255.255.0 10.0.0.1

exit
write memory
```

---

## ✅ Step 8: Test Your Network
### Check Interfaces
``` bash
show ip interface brief
```
- All interfaces should show as up
### Check Routing Tables
``` bash
show ip route
```
### Check Routing Tables
``` bash
show ip start
```
- Should show C (connected) + S (static) routes

### Ping Tests
- From PC0:
  - ping 192.168.10.1 ✅
  - ping 192.168.11.10 ✅
  - ping 192.168.20.10 ✅
  - ping 192.168.21.10 ✅

- From PC7:
  - ping 192.168.21.1 ✅
  - ping 192.168.20.10 ✅
  - ping 192.168.10.10 ✅

---

## Troubleshooting Tips
- Double-check IP addresses on PCs and routers
- Verify cables and port connections
- Ensure HWIC-2T module is installed
- Confirm clock rate (only on Router0 / DCE)
- Use show run to verify configs
- Use Simulation mode to watch packets

## Congrats
### You’ve successfully built a multi-network topology with:
- 2 Routers
- 4 Switches
- 8 PCs
- Full connectivity across sites 🚀
---
