+++
tags = ["TryHackMe", "Red Team", "Fundamentals"]
title = "Red Team Fundamentals - TryHackMe"
weight = 10
draft = false
images = [ "/walkthroughs/Red Team Fundamentals/logo.png" ]
description = "Learn about the basics of a red engagement, the main components and stakeholders involved, and how red teaming differs from other cyber security engagements."
+++

![Logo](logo.png)

Date written: January 2024      
Date published: January 2024

## Task 1: Introduction 

Room objectives:
- Learn about the basics of red team engagements
- Identify the main components and stakeholders involved in a red team engagement
- Understand the main differences between red teaming and other types of cybersecurity engagements

Room prerequisites:
- Before beginning this room, familiarity with general hacking techniques is required. Although not strictly necessary, completing the Jr. Penetration Tester Learning Path is recommended.

> Click to continue to the next task

> Answer: No answer needed

## Task 2: Vulnerability Assessment and Penetration Tests Limitations 

Vulnerability Assessments:
- The simplest form of security assessment.
- Main goal is to  identify as many vulnerabilities as possible.
- May not discover active attacks/attackers.
- Mostly done with automated tools.
- Little technical knowledge needed.
- Does not exploit any of the vulnerabilities. 

Penetration Tests:
- Scanning for vulnerabilities and then exploiting them.
- Need to understand how vulnerabilities impact the network as a whole. So you need to understand how the network works as a whole.
- Post exploitation steps are also used to gain additional useful knowledge that could allow for additional exploits. 
- Will be able to detect real attack paths an attacker could take. 
- Can be a "loud" test. 

> Would vulnerability assessments prepare us to detect a real attacker on our networks? (Yay/Nay)

> Answer: Nay

> During a penetration test, are you concerned about being detected by the client? (Yay/Nay)

> Answer: Nay

> Highly organised groups of skilled attackers are nowadays referred to as ...

> Answer: Advanced Persistent Threats

## Task 3: Red Team Engagements

Red Team Engagements:
- Starts with clear and defined goals.
- Tests the defensive (blue) team's capabilities at detecting and responding to a real attack(er).
- The key is to remain undetected and not perform any "loud" testing.
- TTP simulation. 
- Can run in several ways. Full engagement, assumed breach, and a table top exercise. 

> The goals of a red team engagement will often be referred to as flags or...

> Answer: crown jewels

> During a red team engagement, common methods used by attackers are emulated against the target. Such methods are usually called TTPs. What does TTP stand for?

> Answer: Tactics, Techniques and Procedures

> The main objective of a red team engagement is to detect as many vulnerabilities in as many hosts as possible (Yay/Nay)

> Answer: Nay


## Task 4: Teams and Functions of an Engagement 

Teams or cells:
- Red cell - the component that makes up the offensive portion of a red team engagement that simulates a given target's strategic and tactical responses.
    - Red Team Lead - Plans and organizes engagements at a high level—delegates, assistant lead, and operators engagement assignments.
    - Red Team Assistant Lead - Assists the team lead in overseeing engagement operations and operators. Can also assist in writing engagement plans and documentation if needed.
    - Red Team Operator - Executes assignments delegated by team leads. Interpret and analyze engagement plans from team leads.
- Blue cell - the opposite side of red. It includes all the components defending a target network. The blue cell is typically comprised of blue team members, defenders, internal staff, and an organization's management.
- White cell - Serves as referee between red cell activities and blue cell responses during an engagement. Controls the engagement environment/network. Monitors adherence to the ROE. Coordinates activities required to achieve engagement goals. Correlates red cell activities with defensive actions. Ensures the engagement is conducted without bias to either side.
- Learn more [here](https://redteam.guide/docs/definitions)



> What cell is responsible for the offensive operations of an engagement?

> Answer: Red cell

> What cell is the trusted agent considered part of?

> Answer: White cell

## Task 5: Engagement Structure 


Reconnaissance:
- Obtain information on the target
- Harvesting emails, OSINT

Weaponization:
- Combine the objective with an exploit. Commonly results in a deliverable payload.
- Exploit with backdoor, malicious office document

Delivery:
- How will the weaponized function be delivered to the target
- Email, web, USB

Exploitation:
- Exploit the target's system to execute code
- MS17-010, Zero-Logon, etc.

Installation:
- Install malware or other tooling 
- Mimikatz, Rubeus, etc.

Command & Control:
- Control the compromised asset from a remote central controller
- Empire, Cobalt Strike, etc.

Actions on Objectives:
- Any end objectives: ransomware, data exfiltration, etc.
- Conti, LockBit2.0, etc.

> If an adversary deployed Mimikatz on a target machine, where would they be placed in the Lockheed Martin cyber kill chain?

> Answer: Installation

> What technique's purpose is to exploit the target's system to execute code?

> Answer: Exploitation

## Task 6: Overview of a Red Team Engagement 


> Click the "View Site" button and follow the example engagement to get the flag 

> Answer: Click through the engagement site to find the flag. 

## Task 7: Conclusion 

> Read the above and continue learning!

> Answer: No answer needed.
