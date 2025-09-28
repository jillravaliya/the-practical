# 🛠️ Network Troubleshooting — From Scratch to Deep  

Networking is the **circulatory system** of IT — if it breaks, everything breaks.  
Servers, cloud apps, even your Wi-Fi at home rely on packets moving correctly from point A to point B.  

But here’s the challenge:  
👉 When something goes wrong, where do you even start?  
Is it the cable? The Wi-Fi? DNS? A blocked port?  

This repo gives you a **structured 6-step framework** that I personally practiced on my own Linux system.  
It’s not just theory — each step includes **commands, screenshots, and real output analysis** so you can learn how to debug like a professional.  

---

## 🔹 Why a Framework?  

Without structure, troubleshooting = chaos.  
You’ll waste hours rebooting routers, guessing random fixes, or reinstalling apps.  

Instead, this framework helps you:  
- Start simple → verify device + IP.  
- Go deeper → routing, DNS, ports, connections.  
- Narrow down exactly where the failure is happening.  

It works because it’s **layered** (just like the OSI/TCP/IP models):  
- **Physical** → Is the device connected?  
- **Network** → Can it reach other hosts?  
- **Transport** → Are ports open?  
- **Application** → Is DNS resolving?  

---

## 🔹 The 6 Core Steps  

1. [Step 0: Check Interfaces & Routes](./step-0-check-interfaces/README.md)  
   → Do I even have an IP address? Do I know my gateway?  

2. [Step 1: Basic Connectivity](./step-1-basic-connectivity/README.md)  
   → Can I reach external hosts via ping?  

3. [Step 2: Trace the Path](./step-2-traceroute/README.md)  
   → Where along the path is the failure happening?  

4. [Step 3: DNS Resolution](./step-3-dns-resolution/README.md)  
   → Can hostnames resolve to IP addresses correctly?  

5. [Step 4: Port Connectivity](./step-4-port-connectivity/README.md)  
   → Is the service actually listening on its port?  

6. [Step 5: Active Connections](./step-5-active-connections/README.md)  
   → Which services are running locally? Any port conflicts?  

7. [Step 6: Network Manager & Wi-Fi Info](./step-6-network-manager/README.md)  
   → Is my device properly connected (SSID, signal strength, active connections)?  

---

## 🔹 Who is this for?  

- **Students** → learning Linux & networking basics.  
- **DevOps/Cloud aspirants** → need hands-on troubleshooting skills.  
- **Anyone stuck with "Why is my Wi-Fi not working?"**  

---

👉 With this repo, you don’t just memorize commands.  
You learn to **think like a troubleshooter** — systematic, step by step, until the root cause is found.  
