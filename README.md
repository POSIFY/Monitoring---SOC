# Monitoring---SOC
Designing and Implementing a Security Monitoring &amp; Detection System for a Small Business

### Scenario
Acme Logistics Nigeria Ltd has fifty employees and the following in it's infrastructure:
- Windows workstations
- Windows Server
- Linux server
- Firewall
- Employee laptops
- Web server
- Active Directory
- Internet connection

The company currently has no dedicated SOC and Management wants to know the possibilities of detecting intrusions, attacks and unauthorized actions on their network.
The goal is to build the initial security monitoring capability.

In order to achieve this I will deploy my lab with the following:
1. Wazuh
2. Windows endpoint
3. Linux endpoint
4. Sysmon
5. Firewall logs
6. Authentication logs
7. Network traffic

Then create detection for:
1. Multiple failed logins
2. Successful login after multiple failures
3. New administrator account
4. Privilege escalation
5. Suspicious PowerShell
6. Malware execution
7. Port scanning
8. Unauthorized software
9. File modification
10. Suspicious outbound connections

## Procedures
### Configuring Wazuh on Linux
1.  Download the wazuh-certs-tool.sh script and the config.yml configuration file. This creates certificates that encrypt communications between the Wazuh central components.
<img width="974" height="241" alt="image" src="https://github.com/user-attachments/assets/ceac7c0c-49c2-4c82-a4a6-0b66130f5c61" />
<img width="875" height="156" alt="image" src="https://github.com/user-attachments/assets/bda64d18-d50f-4362-8136-2a804a01c2e4" />

2. Editing the IP for indexer, dashboard and server. Note: This is a single node deployment and if it were a clustered deployment, the same configuration should be repeated across endpoints in the cluster.
<img width="997" height="595" alt="image" src="https://github.com/user-attachments/assets/9c51bad1-1118-4aa9-976f-2f2a1d04486f" />

3. Generate the certificates to enable indexer, dashboard and server to securely communicate.
<img width="948" height="216" alt="image" src="https://github.com/user-attachments/assets/9df32546-5a18-429b-ab33-0024bf1cc3bc" />

4. Updating package repository and Install package dependencies for Wazuh.
<img width="890" height="337" alt="image" src="https://github.com/user-attachments/assets/4cd544ae-4ba6-4545-8ad2-94e1a1136769" />

5. Install repository dependencies:
<img width="912" height="150" alt="image" src="https://github.com/user-attachments/assets/3d4daa4e-c159-4f44-b5b9-0db8671118bf" />

6. Add the Wazuh GPG key
```
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | \
sudo gpg --no-default-keyring \
--keyring gnupg-ring:/usr/share/keyrings/wazuh.gpg \
--import
```
<img width="860" height="151" alt="image" src="https://github.com/user-attachments/assets/ca9943c2-fb75-42d1-8598-2fbab088bc5c" />

6b. The command sudo chmod 644 /usr/share/keyrings/wazuh.gpg changes the file permissions of the Wazuh GPG security key so that the owner can read and write to it, while everyone else can only read it.
```
sudo chmod 644 /usr/share/keyrings/wazuh.gpg
```

7. Add the Wazuh 4.11 repository
```
echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.11/apt/ stable main" | \
sudo tee /etc/apt/sources.list.d/wazuh.list
```
<img width="913" height="90" alt="image" src="https://github.com/user-attachments/assets/ac65226f-242d-4f6f-9751-d5f48615d631" />
