# Stage 2: Active Directory

## What I wanted to do
Turn DC01 from a plain Windows Server into a domain controller, then build a small company structure inside it: departments, users, and groups.

## What I did

### 1. Installed the AD DS role
From Server Manager I went to Manage > Add Roles and Features and picked Active Directory Domain Services. The install went fine. Installing the role only adds the tools though, the server is not a domain controller yet.

### 2. Promoted the server to a domain controller
I used "Promote this server to a domain controller" and chose **Add a new forest** with the root domain `company.local`. I left the defaults on the other screens and set a DSRM password (kept it somewhere safe, not in this repo). The prerequisites check showed a few yellow warnings but no errors, so I went ahead with the install. The server restarted by itself.

After the restart I logged in with the domain account and checked it worked using `echo %USERDOMAIN%` and `echo %USERDNSDOMAIN%`.

### 3. Created the departments (OUs)
In Active Directory Users and Computers I created three OUs: **IT**, **HR** and **Sales**. I kept "Protect container from accidental deletion" turned on.

### 4. Created users
I made one user in each OU, using the format `first.last` for the logon name. For each one I set a temporary password and left "User must change password at next logon" ticked, since that's how it would work in a real company.

## Problem I ran into
**Problem:** After the promotion, the VM froze on first boot. The mouse didn't move and nothing responded for more than 5 minutes.

**What I did:** Waited, then tried Machine > ACPI Shutdown from VirtualBox. When that didn't respond I used Machine > Reset. After it came back I gave it a few minutes to finish starting its services before opening anything, and it worked normally.

**What I learned:** The first boot after promoting a domain controller is slow because AD and DNS services are all starting up. Closing heavy apps on my main PC also helps, since the VM shares the same RAM.

## Screenshots
Files are in the `screenshots` folder:
- `01-ad-ds-installed.png`: AD DS role installed
- `03-dc-server-manager.png`: Server Manager after promotion
- `04-domain-check.png`: domain name check in Command Prompt
- `05-organizational-units.png`: the three OUs
- `06-users-it.png`, `07-users-hr.png`, `08-users-sales.png`: users in each OU
- `09-group-members.png`, `10-groups.png`, `11-groups.png`: the groups and their members

### 5. Created security groups
In each OU I created a security group (Global scope): **IT-Team**, **HR-Team** and **Sales-Team**. Then I added each user to the group of their department through the group's Properties > Members tab.

The reason is that permissions are easier to manage on groups. If a new person joins HR later, I only add them to HR-Team and they get everything the team has, instead of setting permissions user by user.

## Next
Create shared folders for each department and give permissions to the groups, then join a client PC to the domain and test access.