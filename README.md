
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






