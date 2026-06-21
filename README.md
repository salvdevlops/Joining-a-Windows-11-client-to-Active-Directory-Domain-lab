# Joining a Windows 11 Client to an Active Directory Domain

## Overview
This lab covers joining a Windows 11 client to an Active Directory domain, verifying the join, and applying two best practices used in real environments: moving the new computer object out of the default `Computers` container and into the proper OU, and adding a description to the computer object.

## Tools Used
- VMware Workstation
- Windows Server (Domain Controller)
- Windows 11 (Domain Client)
- Active Directory Users and Computers (ADUC)

## Environment
- Domain controller running DNS and Active Directory
- Windows 11 client on the same network, using DHCP (no static IP needed — the client just needs to point to the correct DNS server)
- Windows Server 2025

---

## Step 1: Confirm DNS Settings on the Domain Controller

Since the client gets its IP via DHCP, it doesn't need a static IP — but it does need to know which DNS server to query, and that's the domain controller.

On the Windows Server, open a Command Prompt and run `ipconfig`. The IP address listed here is the DNS server address the client will need to use.

<img width="608" height="340" alt="image" src="https://github.com/user-attachments/assets/3dd18337-7bfd-4029-8225-8787981a5264" />

## Step 2: Point the Windows 11 Client at That DNS Server

On the Windows 11 client:
**Settings → Network & Internet → Ethernet → Edit DNS server assignment**

Switch from **Automatic (DHCP)** to **Manual**, then enter the domain controller's IP address as the preferred DNS server.

<img width="493" height="672" alt="image" src="https://github.com/user-attachments/assets/484407f7-1930-4f8e-b811-8922e7ea9a35" />

## Step 3: Join the Computer to the Domain

Open File Explorer → right-click **This PC** → **Properties** → scroll down to **Domain or workgroup** → **Change settings** → **Change** → select **Domain** and enter the domain name.

(This is also where you can rename the computer if you'd like.)

If you don't remember the domain name, check Active Directory Users and Computers on the windows server — it's displayed there.

<img width="726" height="460" alt="image" src="https://github.com/user-attachments/assets/6089b5c2-86be-4f4a-b78f-91b947de26c2" />

When prompted, enter the username and password for the domain admin account created in a previous lab — see [Creating OUs and Groups in AD](https://github.com/salvdevlops/Creating-Ous-and-Groups-in-AD-lab) (the IT admin user set up there).

<img width="438" height="370" alt="image" src="https://github.com/user-attachments/assets/e94b1c2c-cad1-42d0-8d88-fcfeedddfae7" />

## Step 4: Verify the Domain Join

After the computer restarts, log in with a regular domain user account (also created in the previous lab). You should now see the domain name displayed on the sign-in screen, confirming the machine successfully joined the domain.

<img width="573" height="572" alt="image" src="https://github.com/user-attachments/assets/273a33fb-f5ee-4b97-aeaa-0843d963df3b" />

## Step 5: Move the Computer to the Correct OU

This next part is a best practice that's easy to skip but important in real environments.

By default, a newly domain-joined computer lands in the built-in **Computers** container:

<img width="739" height="368" alt="image" src="https://github.com/user-attachments/assets/691674a7-a6ce-4683-a0fa-e21656673386" />

In production environments, that default container generally isn't used for ongoing management. Companies build their own OU structure instead, since it's easier to apply Group Policy, delegate permissions, and reorganize without touching AD's built-in containers.

In ADUC, right-click the computer object → **Move** → select the OU you want it in. In this lab, the computer was moved into the **Computers** sub-OU under the **USA** OU.

<img width="748" height="484" alt="image" src="https://github.com/user-attachments/assets/115ea98e-4b61-4cf0-bbd6-5d7cec935146" />

## Step 6: Add a Description to the Computer Object

As a final best practice, double-click the computer object and add a description — its purpose, location, owner, whatever's useful for whoever looks at it next.

<img width="457" height="514" alt="image" src="https://github.com/user-attachments/assets/d189d850-bba6-411f-9dcc-3ea98fb07a19" />

---

## Key Takeaways
- A domain client doesn't need a static IP — it just needs to be pointed at the right DNS server (the domain controller).
- Domain-joining is straightforward through System Properties, but it requires domain admin credentials.
- The sign-in screen showing the domain name is a quick visual confirmation that the join worked.
- New computer objects always land in the default `Computers` container — moving them to the proper OU and adding a description are small steps that matter a lot in real-world AD administration.
