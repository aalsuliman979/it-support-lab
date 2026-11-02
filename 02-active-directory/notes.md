Stage 2: Active Directory
What I wanted to do

Turn DC01 from a plain Windows Server into a domain controller, then build a small company structure inside it: departments, users, groups, and shared folders with permissions.

What I did
1. Installed the AD DS role

From Server Manager I went to Manage > Add Roles and Features and picked Active Directory Domain Services. The install went fine. Installing the role only adds the tools though, the server is not a domain controller yet.

2. Promoted the server to a domain controller

I used "Promote this server to a domain controller" and chose Add a new forest with the root domain company.local. I left the defaults on the other screens and set a DSRM password (kept it somewhere safe, not in this repo). The prerequisites check showed a few yellow warnings but no errors, so I went ahead with the install. The server restarted by itself.

After the restart I logged in with the domain account and checked it worked using echo %USERDOMAIN% and echo %USERDNSDOMAIN%.

3. Created the departments (OUs)

In Active Directory Users and Computers I created three OUs: IT, HR and Sales. I kept "Protect container from accidental deletion" turned on.

4. Created users

I made one user in each OU, using the format first.last for the logon name. For each one I set a temporary password and left "User must change password at next logon" ticked, since that's how it would work in a real company.

5. Created security groups

In each OU I created a security group (Global scope): IT-Team, HR-Team and Sales-Team. Then I added each user to the group of their department through the group's Properties > Members tab. (I missed Sales-Team the first time, see the problems section below.)

The reason is that permissions are easier to manage on groups. If a new person joins HR later, I only add them to HR-Team and they get everything the team has, instead of setting permissions user by user.

6. Shared folders with group permissions

I created a Shares folder on C: with one subfolder per department: IT, HR and Sales. For each one:

Shared the folder and left the share permission as Everyone / Full Control, so the real control happens at the NTFS level.
Disabled inheritance and converted the inherited permissions, then removed the Users group so regular users can't get in by default.
Gave only that department's group (IT-Team, HR-Team or Sales-Team) Modify access.

The result should be that each department can only open its own folder. I'll test this from a client PC once it's joined to the domain.

Problems I ran into

Problem 1: After the promotion, the VM froze on first boot. The mouse didn't move and nothing responded for more than 5 minutes.

What I did: Waited, then tried Machine > ACPI Shutdown from VirtualBox. When that didn't respond I used Machine > Reset. After it came back I gave it a few minutes to finish starting its services before opening anything, and it worked normally.

What I learned: The first boot after promoting a domain controller is slow because AD and DNS services are all starting up. Closing heavy apps on my main PC also helps, since the VM shares the same RAM.

Problem 2: When setting permissions on the Sales folder, Windows said it couldn't find "Sales-Team", and Find Now didn't list it either.

Cause: I hadn't created that group yet. I had only made the other two.

Fix: Created Sales-Team in the Sales OU, added the Sales user to it, then went back and added it to the folder permissions.

Screenshots

Files are in the screenshots folder:

01-ad-ds-installed.png: AD DS role installed
03-dc-server-manager.png: Server Manager after promotion
04-domain-check.png: domain name check in Command Prompt
05-organizational-units.png: the three OUs
06-users-it.png, 07-users-hr.png, 08-users-sales.png: users in each OU
09-group-members.png, 10-groups.png, 11-groups.png: the groups and their members
12-hr-folder-permissions.png and 13-hr-folder-permissions.png: HR folder security tab, and the HR-Team permissions
14-it-folder-permissions.png: IT folder permissions
15-sales-folder-permissions.png: Sales folder permissions
Next

Join a client PC to the domain and test that each user can only open their own department's folder.