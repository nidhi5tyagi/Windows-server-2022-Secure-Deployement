# Windows-server-2022-Secure-Deployement
Detailed document on How I deployed and configured a secure **Windows Server 2022** environment. The process involved setting up Active Directory, configuring essential server roles, implementing Sysmon for monitoring, and demonstrating role-based access for users Alice and Bob. 

## 1. Environment Setup

### Virtualization Environment
To start, I set up my virtualization environment using **UTM**. I created two virtual machines:
1. **Domain Controller VM**: Installed Windows Server 2022 and configured it as the domain controller.
2. **Server VM**: Dedicated for hosting server roles like IIS, DNS, and DHCP.
3. **Client VM**: Used to test domain access and user configurations.

> Note: Due to resource constraints, I combined the Server and Domain Controller roles into a single VM and used one Client VM for testing.

---

## 2. Windows Server 2022 Installation

I downloaded and installed the evaluation version of **Windows Server 2022**. During installation:
- I chose the **Desktop Experience** option to have a GUI for easier management.
- Configured the server's hostname and assigned a static IP address for easier domain integration.

---

## 3. Active Directory Setup

### Domain Controller Configuration
1. Promoted the Domain Controller VM to a **domain controller**.
2. Created a new **Active Directory forest** with the domain `int.company.local`.
3. Installed and configured the **DNS service** as part of the promotion process.

### Organizational Units (OUs)
- I created separate **OUs** for:
  - Administrative accounts.
  - Standard user accounts (e.g., Bob).
  - Groups and computers for easier policy management.

### Domain Joining
- Joined the Server VM and the Client VM to the newly created domain `int.company.local`.

---

## 4. User Management: Alice and Bob

1. **User Creation**:
   - Created an **Administrator account** for Alice and assigned it to an "Admins" OU.
   - Created a **Standard User account** for Bob and assigned it to a "Users" OU.

2. **Permissions**:
   - Configured granular permissions for Bob to access only `/Users/Bob`.
   - Applied **Group Policy Objects (GPOs)** to manage settings for both Alice and Bob, ensuring:
     - Alice had administrative privileges.
     - Bob had restricted access to specific resources.

---

## 5. Server Roles Configuration

### 5.1 IIS for Alice
- Installed **IIS** on the Server VM and set up a basic web application for Alice's use.
- Applied security best practices:
  - Restricted access to authorized users within the domain.
  - Configured the IIS application pool to run with appropriate permissions.

### 5.2 DNS
- Verified DNS functionality within the domain.
- Configured conditional forwarders to resolve external DNS queries.

### 5.3 DHCP
- Installed and configured **DHCP** on the Server VM for automatic IP address assignment.
- Integrated DHCP with Active Directory to enable **dynamic DNS updates**.

---

## 6. Sysmon Monitoring

1. Installed **Sysmon** on the Server VM using the Sysinternals package.
2. Configured Sysmon with a custom XML configuration file to capture relevant events while filtering noise.
3. Simulated various activities to generate logs:
   - Alice performing administrative actions.
   - Bob accessing his folder.

4. **Log Analysis**:
   - Used **Event Viewer** to analyze Sysmon logs.
   - Identified key events, including process creations, file access, and network connections.
   - Documented suspicious activities for the client-facing report.

---

## 7. Client-Side Testing and Reporting

### Testing User Access
- Logged into the Client VM using **Alice's credentials** to verify administrative access.
- Logged in using **Bob's credentials** to confirm restricted access to his folder.
- Verified GPO policies for each user.

### Sysmon Logs and Security Insights
- Reviewed Sysmon logs to identify:
  - Alice's administrative actions (e.g., service configuration).
  - Bob's folder access attempts.

### Client Report
I created a client-facing report highlighting:
- Key Sysmon events and their relevance to security monitoring.
- Differences in access levels and permissions between Alice and Bob.
- Recommendations for improving system security.

---

## Final Thoughts

This project helped me develop hands-on skills with **Windows Server 2022**, especially in areas like:
- **Active Directory** configuration and user management.
- Deploying and securing server roles like IIS, DNS, and DHCP.
- Leveraging **Sysmon** for security monitoring and analysis.

Following are the screen shots of the project
![image](https://github.com/user-attachments/assets/b86d9d12-b404-4dec-b966-32ed2335acb4)

![image](https://github.com/user-attachments/assets/12622de1-7912-480a-91ca-55c5945c9392)

![image](https://github.com/user-attachments/assets/c4ba9eb1-161c-4287-8ec5-a2f4d4838743)

![image](https://github.com/user-attachments/assets/be56e306-a8fe-40ad-8a67-c9dad69d4192)

![image](https://github.com/user-attachments/assets/b082b1f5-ccd7-4e2b-b772-bdbc5593d447)

![image](https://github.com/user-attachments/assets/a5eeb979-f1fc-4bcd-82c0-755bc2444d5a)

![image](https://github.com/user-attachments/assets/4560a094-9d74-430c-843b-718d9e267a99)







