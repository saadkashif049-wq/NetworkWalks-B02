# 🛡️ Cyber Security Home Lab Setup — Kali Linux on VirtualBox

Personal lab notes from my internship, documenting how I set up an isolated Kali Linux practice lab using VirtualBox **NAT Network** (not plain NAT), configured DNS, and took snapshots.

> **Why NAT Network and not just "NAT"?**
> Plain NAT only lets a VM talk to the internet — VMs can't see each other.
> **NAT Network** creates a private virtual switch (`10.0.0.0/24` in this lab) so all your VMs (Kali, Windows, Android, etc.) can talk to *each other* AND reach the internet. This is essential for a pentesting lab where you attack one VM from another.

---

## Step 1: Create the NAT Network in VirtualBox

Before touching any VM, the virtual network itself has to exist.

1. Open **Oracle VM VirtualBox Manager**
2. Go to the top menu: **File → Tools → Network**
3. Click the **NAT Networks** tab
4. Click **Create** → this generates a network (rename it `NatNetwork` if it isn't already)
5. Set:
   - **IPv4 Prefix:** `10.0.0.0/24`
   - **Enable DHCP:** checked (optional — this lab uses manual/static IPs instead)
6. Click **Apply**

> 📝 Note: the screenshot below shows the *Host-only Networks* tab, which is a **different** network type (used to connect the VM only to your host PC, not other VMs). For this lab, make sure you're clicking into the **NAT Networks** tab specifically, not this one.

![Network Manager tabs](images/03-network-manager-hostonly.png)
*VirtualBox's Network Manager window — Host-only Networks tab is shown here, but for this lab you want the "NAT Networks" tab next to it.*

---

## Step 2: Attach the Kali VM to the NAT Network

1. Right-click your Kali VM in the VirtualBox list → **Settings**
2. Go to **Network** on the left → **Adapter 1** tab
3. Check ✅ **Enable Network Adapter**
4. Set:
   - **Attached to:** `NAT Network`
   - **Name:** `NatNetwork` (the one you created in Step 1)
   - **Promiscuous Mode:** `Deny` is fine for normal use. (Only switch this to **Allow All** later if you're doing packet sniffing/traffic capture exercises between VMs.)
5. Click **OK**

![Kali Adapter 1 network settings](images/01-kali-adapter-settings.png)
*Kali's Adapter 1 attached to "NAT Network", with Promiscuous Mode set to Deny.*

---

## Step 3: Boot Kali and Open Network Settings

1. Start the Kali VM
2. On the Kali desktop, click the **network icon** in the top-right corner of the taskbar
3. A menu pops up — click **Edit Connections...**

![Network icon dropdown menu](images/05-network-icon-menu.png)
*Click the network icon top-right → "Edit Connections..."*

4. This opens the **Network Connections** window, showing your `Wired connection 1`
5. Select it and click the **gear icon** (⚙) at the bottom to edit it

![Network Connections dialog](images/06-network-connections-dialog.png)
*The Wired connection 1 entry — select it, then click the gear/settings icon to edit.*

---

## Step 4: Set the Static IP Address and DNS

1. In the **Editing Wired connection 1** window, go to the **IPv4 Settings** tab
2. Under **Method**, you can leave it as `Automatic (DHCP)` and just add a static address underneath (as shown below), or switch Method to `Manual` — both work as long as the address is set
3. Under **Additional static addresses**, click **Add** and enter:
   - **Address:** `10.0.0.2`
   - **Netmask:** `24`
   - **Gateway:** `10.0.0.1`
4. In the **Additional DNS servers** field, enter: `8.8.8.8`
   - (This is Google's public DNS — used so Kali can resolve domain names and reach the internet through the NAT Network)
   - If you ever get internet connectivity issues, switch this DNS value to `10.0.0.1` instead (VirtualBox's internal NAT Network gateway/DNS)
5. Click **Save**

![IPv4 static settings with DNS 8.8.8.8](images/07-ipv4-static-settings.png)
*Static address 10.0.0.2/24, gateway 10.0.0.1, and DNS set to 8.8.8.8.*

---

## Step 5: Fix Internet Issues (Kali 2026.1+ only)

Newer Kali versions can have a network bug where the connection hangs due to IPv4 "duplicate address detection" (DAD) taking too long. If your internet doesn't work after Step 4:

1. Open a terminal in Kali
2. Run this command:
   ```bash
   sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
   ```
3. Enter your password when prompted
4. Reconnect the network (or reboot the VM) and test again

![Terminal running the nmcli DAD-timeout fix](images/08-nmcli-dad-timeout-fix.png)
*Running the fix command in the terminal.*

---

## Step 6: Verify the IP Configuration

Confirm the static IP actually applied correctly.

1. Open a terminal
2. Run:
   ```bash
   ifconfig
   ```
3. Check the `eth0` section — it should show:
   - `inet 10.0.0.2`
   - `netmask 255.255.255.0`
   - `broadcast 10.0.0.255`

![ifconfig output confirming 10.0.0.2](images/09-ifconfig-verify.png)
*ifconfig confirms eth0 has IP 10.0.0.2 with the correct netmask.*

---

## Step 7: Test Internet Connectivity

Open Firefox inside Kali and browse to confirm DNS + internet access is working end-to-end.

1. Open **Firefox** from the taskbar
2. Search for anything (e.g., "Network Walks") to confirm the page loads

![Firefox loading a Google search successfully](images/04-firefox-internet-test.png)
*If search results load, your DNS (8.8.8.8) and NAT Network internet routing are both working correctly.*

---

## Step 8: Take a Snapshot

Once everything works — IP is set, internet works — **snapshot it immediately**. This is your "clean" restore point. If you break something later (bad exploit, misconfiguration, malware testing gone wrong), you can roll back in seconds instead of reinstalling.

1. Power off the VM (or you can snapshot while running too)
2. In VirtualBox Manager, select the Kali VM
3. Click **Snapshot** tab (or **Machine → Take Snapshot**)
4. Give it a clear name, e.g. `Clean Install - Working Network`
5. Click **OK**

![Snapshot taken in VirtualBox Manager](images/02-snapshot-taken.png)
*Snapshot 1 created, with "Current State (changed)" shown below it — meaning further changes made after this point can always be rolled back to this snapshot.*

---

## ✅ Quick Recap Checklist

- [ ] Created **NAT Network** (`10.0.0.0/24`) in VirtualBox — *not* Host-only
- [ ] Attached Kali's Adapter 1 to that NAT Network
- [ ] Set static IP `10.0.0.2/24`, gateway `10.0.0.1`
- [ ] Set DNS to `8.8.8.8` (fallback: `10.0.0.1`)
- [ ] Ran the `nmcli` DAD-timeout fix if on Kali 2026.1+
- [ ] Verified IP with `ifconfig`
- [ ] Tested internet in Firefox
- [ ] Took a snapshot

---

## 📌 Reference IP Plan (for future VMs in this lab)

| VM              | Static IP    |
|------------------|-------------|
| Kali Linux       | 10.0.0.2    |
| Windows 7        | 10.0.0.7    |
| Android          | 10.0.0.9    |
| Windows 10       | 10.0.0.10   |
| Windows 11       | 10.0.0.11   |
| Server 2016      | 10.0.0.16   |

All on the same **NatNetwork**, `10.0.0.0/24`, so they can all ping/scan/attack each other during exercises.

---

*Lab based on internship training — Networkwalks Academy Cyber Lab Setup Guide.*
