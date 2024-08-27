# SOC Analyst Home Lab
## Introduction
In this home lab, based on Eric Capuano's blog post [*So You Want To be a Soc Analyst?*](https://blog.ecapuano.com/p/so-you-want-to-be-a-soc-analyst-intro?utm_campaign=post&utm_medium=web), we'll be setting up a realistic cybersecurity scenario. 

The lab will involve creating two virtual machines: one running Linux Ubuntu (acting as the attacker) and the other running Windows (serving as the victim). The Linux machine will use [Sliver](https://github.com/BishopFox/sliver/wiki), a Command & Control (C2) framework, to simulate a cyber attack, while the Windows machine will employ [LimaCharlie](https://limacharlie.io/) SIEM as an Endpoint Detection and Response (EDR) solution to detect and respond to the attack.

#

## Setup
To start the lab, we'll set up both the Ubuntu (Linux) and Windows machines. A key step is disabling Microsoft Defender on the Windows 11 machine. On the Ubuntu machine, we'll install Sliver to use as the attacking tool. Meanwhile, LimaCharlie will be configured as a sensor and EDR solution on the Windows machine, with Sysmon logs being imported for monitoring.

#

### Ubuntu (Linux): Setup & Sliver Attack System Installation

![Ubuntu Setup](https://github.com/user-attachments/assets/79269741-b9c0-46b9-b47c-575b860b944f)
![SSH Ubuntu Downloads](https://github.com/user-attachments/assets/e72b3240-e700-4740-ac7b-c4813be6de6f)

#

### Windows Setup: Disabling Defender, Installing Sysmon & LimaCharlie Agent

![Sysmon](https://github.com/user-attachments/assets/526d51e1-0ad8-42c8-9e4a-a17dd63a96bc)
![Regedit](https://github.com/user-attachments/assets/9bcb2acb-54a3-4188-ac27-a8d2ba0403ad)
![LimaCharlie](https://github.com/user-attachments/assets/b212d810-e7f5-40ca-a42f-a8fd7039523b)

#

## Attack & Defense Machine
On the Ubuntu machine, we generate a malware payload named 'CONTENT_CABIN' using Sliver. This payload is then staged on the Windows machine, preparing it for use. To safeguard the lab, we create a snapshot of the Windows machine, allowing us to easily revert any mistakes that might compromise the environment.

#

![Implants](https://github.com/user-attachments/assets/8a48095c-e7fb-401c-b75a-fa29f66640e4)
![Malware Staged](https://github.com/user-attachments/assets/8fb7861c-2078-4494-861f-d5d1af7fcc4f)

#

We can examine our privileges and other details provided by the payload via SSH on the Linux machine. In the LimaCharlie SIEM, we can observe the telemetry data and detect the malware activity from the attacker.

![LimaCharlie Content Cabin](https://github.com/user-attachments/assets/d8cb0975-2503-46ed-a764-22ba46f6a9f8)
![Attack Privileges](https://github.com/user-attachments/assets/bfdea9d5-591c-4d09-b0e7-98f013ec8124)
![LimaCharlie Content Cabin 2](https://github.com/user-attachments/assets/d04cf9de-9aa7-4566-946a-4f946cf751f2)

#

We also utilize LimaCharlie to scan the hash of the generated payload, but it remains undetected due to its custom creation. This underscores the importance of not blindly trusting an object simply because it appears secure.

![VirusTotal](https://github.com/user-attachments/assets/ea10f337-4a62-47e2-a39a-f579f3302fe5)

#

We can now examine how 'CONTENT_CABIN' is being utilized and gather additional details by using multiple methods to review our timeline.

![Timeline](https://github.com/user-attachments/assets/9a92d1cd-4577-4ab6-9c2f-d18457518d58)
![Timeline 2](https://github.com/user-attachments/assets/b224e95a-2feb-45fa-9900-b0fdf21094b2)
![Timeline 3](https://github.com/user-attachments/assets/0d209389-acde-4ba3-b278-aa8ade77d5b5)

#

We will now implement detection and response mechanisms for such events and conduct tests to ensure their effectiveness.

![EDR](https://github.com/user-attachments/assets/37a561ee-8b0f-4357-a565-bccfbf42b830)
![EDR Test](https://github.com/user-attachments/assets/18de7da4-4523-4c2e-b7f1-bb76da3a0a83)
![EDR LSASS](https://github.com/user-attachments/assets/f97b6412-02b5-4cb0-be42-fdbbe99217cf)

#

Instead of focusing solely on detecting threats, we can now practice using LimaCharlie to create a rule that detects and blocks attacks originating from the Sliver server. On the Ubuntu machine, we will simulate aspects of a ransomware attack by attempting to delete volume shadow copies. In LimaCharlie, we can monitor the telemetry and then develop a rule to fully block the attack. Once the rule is implemented in our SIEM, the Ubuntu machine will be unable to execute the same attack again.

![VSS Shadow Copies](https://github.com/user-attachments/assets/23d07831-d024-440e-9903-96e06b561f16)
![Shadow Copies](https://github.com/user-attachments/assets/bb351529-757a-498f-b28e-2fc2770d79fa)
![VSS Deletion Kill It](https://github.com/user-attachments/assets/312474d0-92f2-4979-a551-19c921d5df35)
