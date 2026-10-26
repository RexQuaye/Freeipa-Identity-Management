
<a name="top"></a>
<h1>FreeIPA Identity Management</h1>

### 📋 Tickets Covered in This Repository

- 🔐 [**Ticket #5** — Install and Configure FreeIPA Client](#ticket-5)
- 👤 [**Ticket #6** — Add User to FreeIPA](#ticket-6)
- 👥 [**Ticket #7** — Add User to Group Using FreeIPA](#ticket-7)
- 🖥️ [**Ticket #23** — Configure FreeIPA Client on the Stage VM](#ticket-23)
- 🗑️ [**Ticket #44** — List All FreeIPA Users and Delete a User](#ticket-44)


<br />
<br />

---

<br />
<br />

<a name="ticket-5"></a>
<h1>Ticket #5 — Install and Configure FreeIPA Client</h1>

<h2>Languages and Utilities Used</h2>
- <b>Bash</b>
- <b>yum</b>
- <b>ipa-client-install</b>
- <b>firewall-cmd</b>
- <b>hostnamectl</b>
- <b>kinit</b>
- <b>FreeIPA Web UI</b>

<h2>Environments Used</h2>
- <b>CentOS Stream 9</b>
- <b>VMware vSphere (VM console)</b>
- <b>FreeIPA (procore.dev)</b>
<br />
<h2>Project walk-through:</h2>

**Task:** Proceed with the installation and configuration of the IPA client on the new virtual machine (VM).

**Objective:** Gain skills in integrating Linux systems with centralized identity management.

**Requirements:**
- VM: dev-app server
- Package: ipa-client

**Information:** Configure the IPA client on the dev-app VM created earlier.

<br />
<br />
<br />
<p align="center">
<b>Setting the hostname: 'localhost' rejected:</b>
<img width="1283" height="424" alt="Installer rejects the default hostname localhost; hostname set with hostnamectl" src="https://github.com/user-attachments/assets/42257a17-d9e7-4bdc-a935-e8549bacd194" />


</p>

<br />
<br />
<br />

<p align="center">
<b>Hostname rejected for uppercase letters, then corrected to lowercase:</b>
<img width="1007" height="194" alt="Installer requires a lowercase hostname; hostname reset to dev-app-rexq.procore.prod1 and the install re-run" src="https://github.com/user-attachments/assets/d96f5362-3a34-4c5b-a11f-32e159dc9a79" />


</p>

<br />
<br />
<br />

<p align="center">
<b>Opening firewall services for FreeIPA:</b>
<img width="1407" height="264" alt="image" src="https://github.com/user-attachments/assets/782fdf95-b9ac-4513-a363-30418cc96067" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Opening the individual FreeIPA ports:</b>
<img width="1023" height="833" alt="image" src="https://github.com/user-attachments/assets/03bd449c-bc8d-49c3-be48-2b6c4eb8317c" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Enrollment failed: user not found in Kerberos database:</b>
<img width="1460" height="967" alt="image" src="https://github.com/user-attachments/assets/77882f18-ba7d-4081-bbdd-b178c4bcb444" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Running the installer with sudo:</b>
<img width="889" height="540" alt="06_sudo-run-fail" src="https://github.com/user-attachments/assets/647bc5b9-e78c-414a-8876-9746471a6ba5" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Enrollment failed with NTP configured:</b>
<img width="1150" height="681" alt="07_fail-ntp-configured" src="https://github.com/user-attachments/assets/911221e3-56b0-4812-ae36-42e0b9bb7cb1" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Confirming the firewall ports are already open:</b>
<img width="1060" height="637" alt="08_firewall-already-enabled" src="https://github.com/user-attachments/assets/070df03b-8fc6-44d2-a260-cf6d469e8d28" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Verifying the account name in the FreeIPA web UI:</b>
<img width="1917" height="334" alt="09_ipa-ui-account-name" src="https://github.com/user-attachments/assets/600382ff-f46f-4ccb-9cc4-54e4b918b77e" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Enrollment failed: wrong credentials:</b><img width="1097" height="421" alt="10_fail-wrong-credentials" src="https://github.com/user-attachments/assets/41b0508b-a0bd-4217-a2e4-869e4d04df06" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Enrollment successful with the correct IPA credentials:</b>
<img width="916" height="643" alt="11_success-annotated" src="https://github.com/user-attachments/assets/65a83df7-3228-4604-8eec-ff98df1a80fb" />



</p>

<br />

<b><u>Solution:</u></b>  I installed the FreeIPA client on the dev-app VM and enrolled it in the procore.dev realm with ipa-client-install --mkhomedir. The first attempts failed on installer prerequisites: the default hostname (localhost) was rejected, then a hostname with uppercase letters, so I set a lowercase hostname with hostnamectl. I also opened the FreeIPA services and ports in the firewall, and later confirmed they were already enabled. The wiki did not cover the NTP prompt, so I tested enrollment with and without chrony configured. The enrollment kept rolling back with the error that the client account was not found in the Kerberos database. I checked the FreeIPA web UI and found that my IPA login was different from the username I had been entering. Re-running the installer with the correct IPA username and my IPA (not VM) password completed the enrollment successfully.

<br />
<br />

<p align="right"><a href="#top">⬆ Back to top</a></p>

---

<br />
<br />

<a name="ticket-6"></a>
<h1>Ticket #6 — Add User to FreeIPA</h1>

<h2>Languages and Utilities Used</h2>
- <b>Bash</b>
- <b>FreeIPA command line (ipa)</b>
- <b>usermod</b>
- <b>userdel</b>

<h2>Environments Used</h2>
- <b>CentOS Stream 9</b>
- <b>VMware vSphere (VM console)</b>
- <b>FreeIPA (procore.dev)</b>
<br />
<h2>Project walk-through:</h2>

**Task:** Create a FreeIPA user for a newly hired developer, add the user to the webmasters group, and set a temporary password as documented in the ticket.

**Objective:** Strengthen skills in centralized user and group management with FreeIPA.

**Requirements:**
- FreeIPA user created
- Group: webmasters
- Password set and documented

<br />
<br />
<br />
<p align="center">
<b>Initial attempt with a local account, checked against the FreeIPA group:</b>
<img width="1512" height="387" alt="image" src="https://github.com/user-attachments/assets/d7c30e30-0243-460c-8bd2-8de99229fbd8" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Creating the FreeIPA user tstark:</b>
<img width="1036" height="513" alt="image" src="https://github.com/user-attachments/assets/7d390ff0-ff18-44c0-b722-c4ea8aaec1bd" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Adding tstark to the webmasters group and verifying membership:</b>
<img width="883" height="433" alt="image" src="https://github.com/user-attachments/assets/a0c0723a-9c74-4b34-a1df-67ad30adab37" />



</p>

<br />

<b><u>Solution:</u></b>  I created a new FreeIPA account (tstark) for the newly hired developer, Tony Stark, with ipa user-add and set a temporary password per the ticket requirements. My first attempt used a local account with usermod, but the webmasters group is managed in FreeIPA, so the local change had no effect on the directory group. I removed the local account and worked in FreeIPA instead. I then added tstark to the webmasters group with ipa group-add-member and confirmed membership with ipa group-show, where tstark appears under Member users.

<br />
<br />

<p align="right"><a href="#top">⬆ Back to top</a></p>

---


<br />
<br />

<a name="ticket-7"></a>
<h1>Ticket #7 — Add User to Group Using FreeIPA</h1>

<h2>Languages and Utilities Used</h2>
- <b>Bash</b>
- <b>FreeIPA command line (ipa)</b>

<h2>Environments Used</h2>
- <b>CentOS Stream 9</b>
- <b>VMware vSphere (VM console)</b>
- <b>FreeIPA (procore.dev)</b>
<br />
<h2>Project walk-through:</h2>

**Task:** The previously added user was reassigned to support a webpage revamp project. Add this user to the support group.

**Objective:** Strengthen skills in centralized user and group management with FreeIPA.

**Requirements:**
- FreeIPA group: support

<br />
<br />
<br />
<p align="center">
<b>Adding tstark to the support group and confirming the result:</b>
<img width="995" height="431" alt="image" src="https://github.com/user-attachments/assets/895d566f-5c7e-4731-8f0a-ca44a02e833e" />



</p>

<br />

<b><u>Solution:</u></b>  The user created in the previous ticket (tstark) was reassigned to a project revamping the production webpage, so I added the account to the support group with ipa group-add-member. The command reported one member added, and tstark appears in the support group's Member users list in the output. I then ran ipa group-show tstark to confirm the account's own user group exists in FreeIPA.

<br />
<br />

<p align="right"><a href="#top">⬆ Back to top</a></p>

---


<br />
<br />

<a name="ticket-23"></a>
<h1>Ticket #23 — Configure FreeIPA Client on the Stage VM</h1>

<h2>Languages and Utilities Used</h2>
- <b>Bash</b>
- <b>ipa-client-install</b>
- <b>SSH (ssh-keygen, ssh-copy-id)</b>
- <b>Ansible</b>
- <b>GitLab</b>
- <b>vi</b>

<h2>Environments Used</h2>
- <b>CentOS Stream 9</b>
- <b>VMware vSphere (VM console)</b>
- <b>FreeIPA (procore.dev)</b>
<br />
<h2>Project walk-through:</h2>

**Task:** All new servers must be enrolled in our IPA server. Enroll the staging server that was just created.

**Objective:** Gain skills in integrating Linux systems with centralized identity management.

**Requirements:**
- VM: stage-web server
- FreeIPA installed
- Passwordless authentication: dev-ansible <-> stage-web, and stage-web <-> GitLab
- Successful Ansible ping testing

**Information:**
- Create a stage group in the inventory file on the Ansible server
- Add the stage-web server to the stage group
- Add the stage-web server to the Ansible server's local DNS file (/etc/hosts)
- Generate SSH keys on stage-web and configure SSH connections with GitLab

<br />
<br />
<br />
<p align="center">
<b>Enrolling stage-web in FreeIPA with ipa-client-install:</b>
<img width="1006" height="806" alt="image" src="https://github.com/user-attachments/assets/af4a85a4-cfb5-4040-9284-0970e09bf863" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Generating SSH keys on stage-web and copying the key to dev-ansible:</b>
<img width="972" height="828" alt="image" src="https://github.com/user-attachments/assets/f71efd9e-6e5c-4fb8-a6dd-974689d5007a" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Passwordless SSH from dev-ansible to stage-web:</b>
<img width="974" height="739" alt="image" src="https://github.com/user-attachments/assets/d26234db-72be-4b99-90bf-e148d25eb4ed" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Displaying the stage-web public key:</b>
<img width="980" height="292" alt="image" src="https://github.com/user-attachments/assets/d616fc29-97c4-4008-b1ac-84a9c1ccd160" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Opening the SSH keys page in GitLab:</b>
<img width="1266" height="402" alt="image" src="https://github.com/user-attachments/assets/4b7995b1-422c-4396-ab64-0a0a936952de" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Adding the stage-web public key to GitLab:</b>
<img width="1050" height="530" alt="image" src="https://github.com/user-attachments/assets/22583a97-b997-4dd8-9be6-e401cc1a40d8" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Confirming the new key in GitLab:</b>
<img width="1305" height="396" alt="image" src="https://github.com/user-attachments/assets/ef02401e-63c9-4080-a189-21386ad13061" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Adding stage-web to the dev-ansible local hosts file:</b>
<img width="888" height="631" alt="image" src="https://github.com/user-attachments/assets/2d787c3f-428d-434f-ba82-0d1e93f6f5e4" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Creating the stage group in the Ansible inventory:</b><img width="870" height="549" alt="image" src="https://github.com/user-attachments/assets/f5faadad-c561-4916-ad9e-1f1c4f123944" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Successful Ansible ping test:</b>
<img width="962" height="243" alt="image" src="https://github.com/user-attachments/assets/a2dfe4ca-55a0-489d-a552-e3386a9ce347" />



</p>

<br />

<b><u>Solution:</u></b>  I enrolled the stage-web server in FreeIPA with ipa-client-install --mkhomedir, supplying the procore.dev domain and my IPA credentials, and the installation completed successfully. For passwordless access, I generated SSH keys on stage-web and copied the public key to dev-ansible with ssh-copy-id, then cleared the old host key entries and copied the dev-ansible key to stage-web so the connection works in both directions. I added the stage-web public key to my GitLab account under SSH keys to configure the GitLab connection. On dev-ansible, I added stage-web to the local /etc/hosts file and created a stage group containing the server in the Ansible inventory. An Ansible ping against that group returned SUCCESS and pong. Ansible printed a warning about characters in the group name, but it did not prevent the test from passing.

<br />
<br />

<p align="right"><a href="#top">⬆ Back to top</a></p>

---

<br />
<br />



<a name="ticket-44"></a>
<h1>Ticket #44 — List All FreeIPA Users and Delete a User</h1>

<h2>Languages and Utilities Used</h2>
- <b>Bash</b>
- <b>FreeIPA command line (ipa)</b>
- <b>tee</b>

<h2>Environments Used</h2>
- <b>CentOS Stream 9</b>
- <b>VMware vSphere (VM console)</b>
- <b>FreeIPA (procore.dev)</b>
<br />
<h2>Project walk-through:</h2>

**Task:** The administration requested a detailed list of all users registered on the FreeIPA server. Generate the list using IPA commands and redirect the output to a file.

**Objective:** Master centralized authentication and streamline user and group management using FreeIPA.

**Requirements:**
- VM: dev-app
- Delete the FreeIPA user created in Ticket #6 (via an IPA command)
- Path to the redirected output file (FreeIPA users)

**Additional information:**
- The HR department terminated the user created in Ticket #6
- Delete that user with an IPA command and capture the result
- Provide the file containing the list of users

<br />
<br />
<br />
<p align="center">
<b>Generating the list of all FreeIPA users and saving it to a file:</b>
<img width="879" height="257" alt="image" src="https://github.com/user-attachments/assets/65de4a11-9619-4c7d-a73e-ef111b55a421" />



</p>

<br />
<br />
<br />

<p align="center">
<b>Deleting the Ticket #6 user and confirming the result:</b>
<img width="698" height="207" alt="image" src="https://github.com/user-attachments/assets/e5e14142-c1fc-43f9-b8ba-d57fc7bd95fe" />


</p>

<br />

<b><u>Solution:</u></b>  I generated the list of all users registered on the FreeIPA server with ipa user-find --all --raw and piped the output through tee, which displayed it and saved it to a file in my home directory. The command returned 359 entries. Because the HR department terminated the user created in Ticket #6, I then deleted that account (tstark) with ipa user-del. To confirm the result, I ran ipa user-show on the same account, and FreeIPA returned a "user not found" error, showing the account no longer exists.

<br />
<br />

<p align="right"><a href="#top">⬆ Back to top</a></p>

---
