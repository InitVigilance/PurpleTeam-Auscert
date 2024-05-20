### Value creation

Communicate differences between Penetration Test and Purple Team Engagement. Purple team engagements do not replace regular penetration tests and it is important for stakeholders to be aware of the differences.

| **Engagement** | **Penetration Test** | **Purple Team** |
| --- | --- | --- |
| **Approach** | Utilises either a white box or black box methodology. In a white box test, all relevant information is disclosed to the tester, whereas a black box test only provides details about the target scope. | Employs a collaborative approach where Tactics, Techniques, and Procedures (TTPs) are predetermined by both the Purple Team and Blue Team. |
| **Visibility** | IT personnel and developers are informed about the test. The results are communicated to these teams upon completion. | The Blue Team or Security Operations Centre (SOC) are aware of the assessment. Results are shared with both the Blue Team and Red Team during and after the assessment. |
| **Goal** | The primary objective is to uncover as many vulnerabilities as possible within the defined scope and time frame. | Focuses on evaluating and enhancing detection and response capabilities in a controlled environment, facilitating measurable improvements over time. |
| **Scope** | Determined by the organisation requesting the test; can range from a single external IP address to a broader scope. | Based on predefined TTPs, typically tailored to align with the organisation’s business model. |
| **Duration** | Varies according to the scope’s size. | Depends on the number of TTPs selected and any additional specific objectives. |
| **Output** | Produces a comprehensive report detailing the vulnerabilities found within the scope, but does not assess detection and response capabilities. | Provides an extensive report on all predefined TTPs, indicating whether they were detected, blocked, or investigated. The repeatable nature of Purple Team exercises enables the organisation to track and measure improvements over time. |
| **Main Benefits** | Effective for assessing the security of individual applications or networks. | Offers precise metrics for measuring improvements in detection and response capabilities, and is the best tool for evaluating the effectiveness of SOC and Blue Teams. |

### Azure Purple Teaming Cheatsheet

#### [ ] Threat Assessment

- [ ] **Determine Crown Jewels**: Identify critical assets and data ("crown jewels") of the organisation, assess their risk levels, and set Purple Team objectives to compromise the highest-risk items.
- [ ] **Technology Landscape**: Document and understand the technology stack, including Azure services, applications, and infrastructure components used within the organisation.

#### [ ] Planning and Prioritisation

- [ ] **Risk Prioritisation**: Evaluate the risks identified during the Threat Assessment phase and prioritise them based on their likelihood and potential impact on the organisation.
- [ ] **Resource Allocation**: Allocate resources (time, personnel, tools) based on the prioritised risks to ensure efficient mitigation efforts.
- [ ] **Timeline Development**: Create a timeline for executing Purple Team exercises, allocating sufficient time for each phase while considering dependencies and constraints.
- [ ] **Scenario Development**: Develop realistic attack scenarios tailored to the organisation's environment and the identified risks, ensuring they cover a range of potential threat vectors.
- [ ] **Objective Setting**: Define clear objectives for the Purple Team exercise, aligning them with the identified risks and ensuring they address both offensive and defensive capabilities.
- [ ] **Stakeholder Engagement**: Engage key stakeholders, including business leaders, to ensure alignment on objectives, resource allocation, and expectations for the exercise.
- [ ] **Documentation Review**: Review and update documentation, including incident response plans, playbooks, and SOPs, to reflect the latest insights from the Threat Assessment phase and incorporate lessons learned from previous exercises.
- [ ] **Communication Plan**: Develop a communication plan outlining how information will be shared among team members, stakeholders, and leadership throughout the Purple Team exercise, ensuring timely updates and coordination.
- [ ] **Contingency Planning**: Identify potential risks and challenges that may arise during the exercise and develop contingency plans to mitigate them, ensuring the smooth execution of the exercise.

#### [ ] Recon for Initial Access

- [ ] **Email Structure Discovery**: Use websites like `Hunter.io` to discover the email structure of the organisation (e.g., first.last@company.com).
- [ ] **Email List Compilation**: Gather a list of potential email addresses by scraping LinkedIn profiles. Tools such as `GatherContacts` can automate this process.
- [ ] **O365 Domain Validation**: Verify if the organisation's domain is using Office365. Tools to use: `o365spray`
- [ ] **Email List Validation**: Utilise tools like `o365spray` to validate the compiled email list. If operating internally, leverage existing user lists.

#### [ ] Initial Access

- [ ] **Phishing Campaigns**: Conduct phishing simulations targeting the validated email list to gain initial access. Tools: `Gophish`, `Evilginx`
- [ ] **Credential Stuffing and Brute Force**: Perform credential stuffing or password spray attacks against Azure AD or Office 365 login portals. Tools: `omnispray`, `MFAsweep`
- [ ] **Exploitation of Vulnerabilities**: Identify and exploit vulnerabilities in exposed Azure services or applications. Tools: `dnsdumpster` to identify subdomains and `burpsuite` to explore services for further vulnerability enumeration.

### [ ] Discovery Phase

*<u>For internal teams with early stages of maturity on Azure security, Purple teaming should begin at this stage. Unless there is good baseline preventative polices to prevent Initial access, detections are difficult to improve. More value would be obtained to run discovery for misconfigurations and working on improvements, repeating the process until time spent on attack emulation is considered of value.</u>*

- [ ] **Misconfiguration Discovery**: Identify gaps in Azure configurations using tools like `PurpleKnight`, `Maester`.
- [ ] **Resource Enumeration**: Enumerate Azure resources and their permissions using tools like `BloodHound`, `AzureHound`, `ROADtools`

#### [ ] Persistence

- [ ] **Establish Backdoors**: Set up persistent access through backdoors such as Azure AD accounts, service principals, or adding credentials/MFA to existing accounts.
- [ ] **Leveraging Azure Services**: Use Azure Automation, Logic Apps, Azure Lighthouse or Function Apps to maintain persistence within the environment.
- [ ] **Cloud Service Modifications**: Modify cloud service configurations (e.g., adding VM extensions or modifying storage account settings) to ensure continuous access.

#### [ ] Privilege Escalation

- [ ] **Azure AD Role Elevation**: Attempt to escalate privileges by exploiting misconfigurations in Azure AD roles and permissions.
- [ ] **Exploitation of Managed Identities**: Use managed identities (assigned to Azure resources) to access other resources with higher privileges.
- [ ] **Credential Harvesting**: Extract credentials from compromised VMs or applications to gain higher-level access.

#### [ ] Lateral Movement

- [ ] **Internal Spearphishing**: Use AITM phishing using tools like `Evilginx` to target privileged users from a compromised user's mailbox
- [ ] **Service Principal Abuse**: Exploit users and service principals with excessive permissions to move laterally within the environment. Example blog to follow: https://posts.specterops.io/azure-privilege-escalation-via-service-principal-abuse-210ae2be2a5
- [ ] **Networking Exploits**: Use virtual network (VNet) peering and VPN configurations to move between different segments of the network.

#### [ ] Detection and Validation

- [ ] **Log Analysis**: Continuously monitor and analyse logs from Azure Monitor, Security Centre, and Sentinel for suspicious activities.
- [ ] **Threat Hunting**: Perform proactive threat hunting activities using Azure Sentinel's hunting queries and workbooks.
- [ ] **Alert Tuning**: Adjust and fine-tune security alerts to reduce false positives and ensure critical events are accurately detected. If using Sentinel for SIEM, use websites like `kqlsearch` to fine tune detections against known TTPs.
- [ ] **Detection Engineering**: Improve detections by aggregating suspicious activities which, if happening in sequence or within a known threshold time, indicates potential threat actor activity

#### [ ] Improvement

- [ ] **Security Control Enhancements**: Implement and enhance security controls based on the findings from Purple Team exercises. Prioritise preventative policy configurations to stop an attack over detective controls. Detection logic should be built on robust preventative configurations.
- [ ] **Continuous Learning**: Encourage continuous learning and training for both Red and Blue Teams on the latest threat vectors and defence mechanisms. Keep aware of new and emerging TTPs
- [ ] **Feedback Loop**: Establish a feedback loop between Red and Blue Teams to share insights and improve detection and response strategies continuously.

###

### Purple Team Report Template

| **Section** | **Details** |
| --- | --- |
| **Executive Summary** |     |
| **Scope** | Provide an overview of the scope of the Purple Team engagement. |
| **Scenarios Exercised** | List and describe the scenarios that were exercised during the engagement. |
| **Assessment Results** | Include a visual representation of the assessed attack path, highlight attack entry points and map the prevention, and detection and response capabilities discovered against the tactics, techniques, and procedures exercised. For example, this may include a detection and response capability heat map overlay to the MITRE ATT&CK tactics and techniques. |
| **Recommendations** | Provide high-level recommendations based on the engagement results. |
| **Technical Summary** |     |
| **Detailed Scope – Purple Team Plan** | Detail the specific scope of the Purple Team Plan, including targeted systems and objectives. |
| **TTPs Assessed** | List the tactics, techniques, and procedures (TTPs) that were evaluated during the engagement. |
| **Assessment Results and Recommendations** | Present the detailed results of the assessment, including: |
|     | - **Prevention Capability**: Describe the current prevention capabilities assessed. |
|     | - **Detection Capability**: Detail the detection capabilities and any gaps identified. |
|     | - **Response Capability**: Outline the response capabilities and recommend improvements. |
| **Appendices** |     |
|     | Include any additional information, data, or documents that support the report findings. |
| **Supplemental Data** |     |
|     | Provide supplementary data that offers further insights or details, such as log files, screenshots, or additional analysis. Label screenshots for the reader and provide references to previous sections for which the screenshots/logs are relevant |

### Appendix 1: Repos for tools mentioned above

1. [GitHub - 0xZDH/o365spray: Username enumeration and password spraying tool aimed at Microsoft O365.](https://github.com/0xZDH/o365spray)
  
2. [GitHub - 0xZDH/Omnispray: Modular Enumeration and Password Spraying Framework](https://github.com/0xZDH/Omnispray)
  
3. [GitHub - dafthack/MFASweep: A tool for checking if MFA is enabled on multiple Microsoft Services](https://github.com/dafthack/MFASweep)
  
4. [https://getgophish.com/](https://getgophish.com/)
  
5. [GitHub - kgretzky/evilginx2: Standalone man-in-the-middle attack framework used for phishing login credentials along with session cookies, allowing for the bypass of 2-factor authentication](https://github.com/kgretzky/evilginx2)
  
6. [GitHub - BloodHoundAD/BloodHound: Six Degrees of Domain Admin](https://github.com/BloodHoundAD/BloodHound)
  
7. [GitHub - BloodHoundAD/AzureHound: Azure Data Exporter for BloodHound](https://github.com/BloodHoundAD/AzureHound)
  
8. [GitHub - dirkjanm/ROADtools: A collection of Azure AD/Entra tools for offensive and defensive security purposes](https://github.com/dirkjanm/ROADtools)
  
9. [Active Directory Security Assessment | Purple Knight](https://www.semperis.com/purple-knight/)
  
10. [https://maester.dev/](https://maester.dev/)
  
11. [https://www.kqlsearch.com/
