# Lab: Storage Plan and VirtualBox on the D: Drive

## Objectives

- Record my storage baseline and plan the lab's disk budget.
- Create a clean folder structure on D:.
- Point VirtualBox at D: for new VMs.
- Move Metasploitable2 from C: to D:.

## Theory

VirtualBox stores each VM in its own folder (the `.vbox` settings file plus its virtual disks). New VMs go into the **default machine folder**. Virtual disks I create as **dynamically allocated** only use the space they actually need, up to a maximum size I set.

My D: drive has 144 GB free of 273 GB (it was 160 GB a day earlier, so I check the number again before each VM build). The lab's VMs, ISOs and snapshots must fit in that space with a safety margin, so I plan the budget before I build anything.

VirtualBox itself is already installed on C:. I'm not reinstalling it just to move it. Its drivers and per-user settings live on C: regardless, so I keep this as a known exception to my "D: for everything" rule.

## Lab

### Step 1: Record the baseline

Open File Explorer, go to **This PC**, and note the free space on D:.

| Item | Value |
|---|---|
| D: free space | 144 GB of 273 GB |
| Kali VM location | D: |
| Ubuntu VM location | D: |
| Metasploitable2 location | C: (to be moved) |

### Step 2: Check each VM's size

In VirtualBox, open **File → Tools → Virtual Media Manager** (the menu wording varies slightly between versions). For each disk, note its **virtual size** (the maximum) and its **actual size** (what it uses now).

| VM | Virtual size (max) | Actual size now |
|---|---|---|
| Kali | | |
| Ubuntu | | |
| Metasploitable2 | | |

### Step 3: Create the D: folder structure

In File Explorer (or PowerShell), create:

```powershell
New-Item -ItemType Directory -Path "D:\SOC-Lab\downloads","D:\SOC-Lab\repo" -Force
```

If my existing VMs already sit in a folder such as `D:\VirtualBox VMs`, I keep using that same folder for the new VMs instead of creating another one.

ISOs are the one exception: I download them to my normal Downloads folder on C: and delete each ISO once its VM is installed. This is a deliberate, temporary exception to my D: rule. Before each download I check that C: has enough free space (the Windows 11 ISO is about 6 GB).

### Step 4: Set the VirtualBox default machine folder

In VirtualBox: **File → Preferences → General → Default Machine Folder**. Set it to the folder where Kali and Ubuntu already live (for example `D:\VirtualBox VMs`). Click **OK**.

### Step 5: Move Metasploitable2 to D:

1. Shut Metasploitable2 down completely. Do **not** use "Save the machine state". If it was left in a saved state, I start it, shut it down properly, and then continue. (Before I start it, I check in **Settings → Network** that its adapter is **Host-only** or not attached at all, never NAT or Bridged. Module 0 Part B covers network setup in full.)
2. In the VM list, right-click Metasploitable2 and choose **Move…** (in some versions it is under **Machine → Move…**). Pick the machine folder on D: and confirm.
3. Wait for the move to finish. Do not interrupt it.

**If the Move option is missing or greyed out,** use the safe alternative: **File → Export Appliance**, save the `.ova` to `D:\SOC-Lab\downloads`, then **File → Import Appliance** into the D: machine folder. I only delete the old C: copy after the imported VM is confirmed working.

### Step 6: Verify

- In File Explorer, confirm the Metasploitable2 folder (with its `.vbox` file and disk) is now on D:.
- In VirtualBox, open Metasploitable2's **Settings → Storage** and confirm its disk path points to D:.
- Re-check **Settings → Network** one more time and note the adapter configuration.

### Step 7: Disk budget (my estimates for actual use, not maximums)

| Item | Estimated use |
|---|---|
| Wazuh server VM | about 30 GB |
| Windows 11 evaluation VM | about 40 GB |
| Splunk VM | about 20 GB |
| Suricata and Zeek (installed on the existing Ubuntu VM) | about 3 GB extra |
| ISOs | about 12 GB, but temporarily on C: and deleted after install, so **not counted against D:** |
| Snapshots | 15 to 20 GB |
| **Total on D: (without ISOs)** | **about 108 to 113 GB** |

That leaves about 31 to 36 GB free on D:, still above my 20 GB safety margin but with less room than I first thought, so I use these rules:

1. Use dynamically allocated disks everywhere.
2. Keep at least **20 GB free** on D:. If I drop below that, I stop and clean up before continuing.
3. ISOs live temporarily on C: and are deleted after installation (I can redownload them).
4. Keep no more than two or three snapshots per VM, and delete old ones.
5. Build the Wazuh, Windows and Splunk VMs only when their module starts, not before.
6. If space gets tight, I may install Splunk on an existing Ubuntu VM instead of creating a new one.

## Result

*(I fill this in after doing the lab.)*

| Check | Result |
|---|---|
| Folder structure created on D: | |
| Default machine folder set to | |
| Metasploitable2 now stored at | |
| Metasploitable2 network adapter(s) | |
| D: free space after the move | |

## Troubleshooting

| Problem | Likely cause | Fix |
|---|---|---|
| **Move…** is greyed out | VM is running, paused or in a saved state | Shut it down fully, then retry |
| "Access denied" or "file in use" | Another process holds the disk (a second VirtualBox window, antivirus, backup tool) | Close other VirtualBox windows, retry; temporarily pause the offending tool if needed |
| "Not enough space" | D: is short on space | Free space first; the move needs room for the full disk |
| VM won't start after the move | A path in the settings points to the old location | Check **Settings → Storage**, remove the missing disk and re-attach it from D: |
| Disk shows as inaccessible in Virtual Media Manager | The old registration is stale | Release/remove the entry and re-add the disk from its new D: path |

## Takeaways

- A written storage budget tells me early whether the lab fits.
- VM folders live on D:. ISOs sit temporarily on C: and are deleted after install, and VirtualBox's own install and drivers stay on C:. I record both as known exceptions.
- Moving a VM is safest when it is fully powered off, and the original is not deleted until the new copy works.
