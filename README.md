Incident-Response [A simple incident response playbook](https://docs.google.com/document/d/1EQ5MzN95POLaRIMulYg3PIH3UGHtDNcGdkFvgOXyEXQ/edit#heading=h.14falem1ttrj)
<details>
  <summary>Brute Force attempt</summary>
  
  Incident Response: Brute Force Attack on Azure Windows VM
Overview
Incident Summary
On August 12, 2024, an alert was triggered for a potential brute force attack on the Azure-hosted Windows virtual machine (windows-vm). The alert, generated by Microsoft Sentinel, indicated a successful brute force attempt from an external IP address (203.135.22.130) located in Lahore, Pakistan.

<p align="center">
<img src="https://github.com/user-attachments/assets/a74ca45d-0f7e-4550-9b30-eaf0f1656ee0" height="80%" width="80%" alt="NSGALLOW"/>

Upon investigation, it was determined that the alert was a false positive, attributed to a service account-related misconfiguration. Despite the initial appearance of a successful breach, further analysis revealed no unauthorized changes or suspicious activity on the system. See final report for findings and reasoning
<p align="center">
<img src="https://github.com/user-attachments/assets/3582efe4-aa88-4e36-9b6d-79799373b74e" height="80%" width="80%" alt="NSGALLOW"/>

Investigation and Analysis
Preparation:

Custom detection and logging rules were configured in Microsoft Sentinel, ensuring that all brute force attempts were captured and sent to the Log Analytics Workspace for analysis.

Detection:

The SIEM alert indicated a successful brute force attempt, which prompted immediate investigation. Initial indicators included a high volume of failed login attempts (over 600) followed by a single successful logon from an IP address outside the expected geographic region.
<p align="center">
<img src="https://github.com/user-attachments/assets/dd37d745-0a30-4da4-9874-dadaf5c233b0" height="80%" width="80%" alt="NSGALLOW"/>
  
Analysis:

The alert's geolocation data showed that the source IP originated from Lahore, Pakistan, a location not associated with any authorized access. The account in question was NT AUTHORITY\ANONYMOUS LOGON, commonly involved in such alerts.
Despite the alert indicating a "success," repeated login attempts continued from the same IP, suggesting that the attacker's access was not fully established.
Further research revealed that this scenario is a known issue, often related to service accounts and resulting in false positives for brute force detection. (I forgot to take more screenshots, I was having so much fun. I'll make sure not to forget in the next 2 incidents, I apologize.)

Containment:

Immediate actions were taken to prevent further attempts, including adjusting the Network Security Group (NSG) rules to restrict inbound traffic solely to known, trusted IP addresses.
A detailed review of the NSG settings was conducted to ensure no similar traffic could bypass the firewall in the future. (Usually this first goes through the Change Management Process where you would first file an RFC, or a Request For Change)

<p align="center">
<img src="https://github.com/user-attachments/assets/d15bb91a-a544-4a1d-87b6-f26bdd1bbae0" height="80%" width="80%" alt="NSGALLOW"/>

Conclusion:

The incident was ultimately classified as a false positive. However, the investigation highlighted the importance of robust firewall and NSG configurations to prevent such occurrences.


Final Report:

Date: August 12, 2024
Time: 12:03 PM
Incident: Brute Force Alert on windows-vm
Outcome: False Positive

Details:
The alert was triggered by a service account anomaly, resulting in an erroneous indication of a successful brute force attack. Continuous monitoring and repeated failed attempts from the same IP address suggested no real breach occurred. The false positive was confirmed through both system logs and external research.

Remediation:
The NSG rules were hardened to block all unauthorized traffic, limiting access to a specific, trusted IP range. These measures will help prevent similar false positives in the future and ensure that only legitimate traffic can reach the Azure VMs.

</details>

<details>
  <summary>Possible Malware Outbreak</summary>
  
 Incident Summary:
On March 15, 2023, at 03:14 PM, the security team received an alert indicating potential malware activity on the Azure-hosted Windows virtual machine (windows-vm). The alert, which originated from Microsoft Defender for Cloud, suggested that the virtual machine had been compromised.

Upon further investigation, it was determined that the alert was a false positive, triggered by the user conducting tests with EICAR files, a standard tool used for testing antivirus response. The incident was quickly resolved after corroborating with the user and their manager.

Investigation and Analysis
Initial Assignment:

The incident was promptly assigned for investigation. An initial overview of the incident was taken to understand its scope and potential impact

<p align="center">
<img src="https://github.com/user-attachments/assets/db6d3788-6a01-458c-98fa-05ff950e7789" height="80%" width="80%" alt="NSGALLOW"/>

Related Alerts:

During the investigation, it was noted that the windows-vm had been involved in multiple brute force attack attempts. However, no successful compromises were reported.
Analytics Rule Examination:

The next step involved examining the query that generated the alert. This was crucial in understanding why the alert was triggered and ensuring that no actual threats were missed.

<p align="center">
<img src="https://github.com/user-attachments/assets/d1b65eb8-e7e4-4278-8795-13e7f52cb20c"height="80%" width="80%" alt="NSGALLOW"/>

Log Query and Analysis:

Logs from the compromised entity were queried to check for any signs of malware installation or execution. The logs confirmed that Microsoft Defender had successfully blocked the malware and quarantined the EICAR test files, preventing any potential harm 

<p align="center">
<img src="https://github.com/user-attachments/assets/ef5ddc52-0f68-4f76-a19b-c135de982756"height="80%" width="80%" alt="NSGALLOW"/>

  
Conclusion:

The incident was confirmed as a false positive. The alert was triggered due to legitimate EICAR file tests conducted by the user. After confirming this with the user and their manager, the alert was closed.

<p align="center">
<img src="https://github.com/user-attachments/assets/5ff3b63b-e7b2-4d9a-80a2-c8a5a960a778"height="80%" width="80%" alt="NSGALLOW"/>

  
Final Report:


Date: March 15, 2023
Time: 03:14 PM

Incident: Possible Malware Outbreak on windows-vm

Outcome: False Positive

Details:
The alert for a potential malware outbreak on windows-vm was generated by Microsoft Defender in response to EICAR test files used by the user. These files are harmless and are specifically designed to test the effectiveness of antivirus software. After thorough investigation and corroboration with the involved user and their manager, the incident was classified as a false positive and the alert was closed.

Remediation:
No further action was required, as the situation was already effectively handled by Microsoft Defender. The incident was documented, and no system changes were necessary.

</details>

<details>
  <summary>Possible Privilege Escalation</summary>
  
 Incident Summary
On February 16, 2023, an alert was triggered by the Security Information and Event Management (SIEM) system indicating a potential privilege escalation within the Azure environment. The alert flagged suspicious activity involving the user account josh.madakorgmail.onmicrosoft.com, which had accessed critical credentials in the Azure Key Vault multiple times and was associated with several other high-risk actions, including password resets and the assignment of global administrator roles.

Upon further investigation, it was determined that the actions were legitimate and performed by the account owner as part of their normal duties. The incident was corroborated with the account owner and their manager, leading to the closure of the alert.

Investigation and Analysis
Initial Assignment:

The incident was promptly assigned for investigation. An initial overview of the alert was taken, focusing on the entities involved and the potential scope of the incident

<p align="center">
<img src="https://github.com/user-attachments/assets/2ff5b1c2-624d-497b-a2ae-b2568dfe94df"80%" width="80%" alt="NSGALLOW"/>

<p align="center">
<img src="https://github.com/user-attachments/assets/b08ba064-74c5-4c91-893e-55c5f41883b7"80%" width="80%" alt="NSGALLOW"/>

Related Alerts:

During the investigation, it was discovered that the user had been involved in multiple other security alerts, including excessive password resets and the assignment of global administrator privileges. This raised concerns about possible lateral movement within the network

<p align="center">
<img src="https://github.com/user-attachments/assets/3f4167ab-a145-424d-ae02-4051c81b345d"height="80%" width="80%" alt="NSGALLOW"/>


Direct Communication:

The security team reached out to the account owner to verify the legitimacy of the activities. The account owner confirmed that the actions were part of their regular duties. This was further corroborated by their manager, who confirmed the legitimacy of the activities (see screenshot "Close Ticket").
Conclusion:

The incident was classified as a false positive due to inaccurate data. The actions taken by the user were legitimate, and the alert was closed following confirmation from both the user and their manager.

<p align="center">
<img src="https://github.com/user-attachments/assets/396ff738-4f6d-4ae6-a873-c7ef61d9df57"height="80%" width="80%" alt="NSGALLOW"/>



Final Report
Date: February 16, 2023
Incident: Possible Privilege Escalation in Azure Key Vault
Outcome: False Positive - Inaccurate Data

Details:
The SIEM alert indicated potential privilege escalation by the user account josh.madakorgmail.onmicrosoft.com. The investigation revealed that the user had accessed critical credentials multiple times and was involved in several high-risk activities, including password resets and the assignment of global administrator roles. After direct communication with the user and corroboration with their manager, it was confirmed that these activities were legitimate and part of the user's normal duties. The alert was closed as a false positive.

Remediation:
No further action was required, as the activities were authorized. The incident was documented, and the alert was closed to prevent unnecessary escalation.



</details>
