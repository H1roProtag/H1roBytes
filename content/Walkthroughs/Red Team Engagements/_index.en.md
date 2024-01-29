+++
tags = ["TryHackMe", "Red Team"]
title = "Red Team Engagements - TryHackMe"
weight = 10
draft = true
images = [ "/walkthroughs/Red Team Engagements/logo.png" ]
description = "Learn the steps and procedures of a red team engagement, including planning, frameworks, and documentation."
+++

![Logo](logo.png)

Date written: January 2024      
Date published: January 2024

## Task 1: Introduction 

Room objectives:
- Understand components and functions of a red team engagement.
- Learn how to properly plan an engagement based of needs and resources available and TTPs. 
- Understand how to write engagement documentation in accordance to client objectives.

Room prerequisites:
- None

> Read the above and continue to the next task.
> 
> Answer: No answer needed

## Task 2: Defining Scope and Objectives 

> Read the example client objectives and answer the questions below.
> s
> Answer: No answer needed. 

Below is an example of the client objectives of a mature organization with a strong security posture.

Example 1 - Global Enterprises:

Objectives:

- Identify system misconfigurations and network weaknesses.
    - Focus on exterior systems.
- Determine the effectiveness of endpoint detection and response systems.
- Evaluate overall security posture and response.
    - SIEM and detection measures.
    - Remediation.
    - Segmentation of DMZ and internal servers.
- Use of white cards is permitted depending on downtime and length.
- Evaluate the impact of data exposure and exfiltration.

Scope:

- System downtime is not permitted under any circumstances.
    - Any form of DDoS or DoS is prohibited.
    - Use of any harmful malware is prohibited; this includes ransomware and other variations.
- xfiltration of PII is prohibited. Use arbitrary exfiltration data.
- Attacks against systems within 10.0.4.0/22 are permitted.
- Attacks against systems within 10.0.12.0/22 are prohibited.
- Bean Enterprises will closely monitor interactions with the DMZ and critical/production systems.
    - Any interaction with "*.bethechange.xyz" is prohibited.
    - All interaction with "*.globalenterprises.thm" is permitted.


> Answer: No answer needed

> What CIDR range is permitted to be attacked?
> 
> Answer: 10.0.4.0/22

> Is the use of white cards permitted? (Y/N)
> 
> Answer: Y

Learn a little more about what white cards and are how they are used [here](https://sixdub.medium.com/common-ground-part-1-red-team-history-overview-82803bbdc975), but it is essentially a simulated portion of the test to overcome limitations and allow continued testing. 

> Are you permitted to access "*.bethechange.xyz?" (Y/N)
>
> Answer: N


## Task 3: Rules of Engagement 

Sections of Rules of Engagements or RoE:

- Executive Summary	
  - Overarching summary of all contents and authorization within RoE document
- Purpose	
  - Defines why the RoE document is used
- References	
  - Any references used throughout the RoE document (HIPAA, ISO, etc.)
- Scope	
  - Statement of the agreement to restrictions and guidelines
- Definitions	
  - Definitions of technical terms used throughout the RoE document
- Rules of Engagement and Support Agreement	
  - Defines obligations of both parties and general technical expectations of engagement conduct
- Provisions	
  - Define exceptions and additional information from the Rules of Engagement
- Requirements, Restrictions, and Authority 	
  - Define specific expectations of the red team cell
- Ground Rules	
  - Define limitations of the red team cell's interactions
- Resolution of Issues/Points of Contact	
  - Contains all essential personnel involved in an engagement
- Authorization	
  - Statement of authorization for the engagement
- Approval 	
  - Signatures from both parties approving all subsections of the preceding document
- Appendix	
  - Any further information from preceding subsections


> Download the sample rules of engagement from the task files.
> Once downloaded, read the sample document and answer the questions below.
> 
> Answer: No answer needed

> How many explicit restriction are specified?
> 
> Answer: 3

> What is the first access type mentioned in the document?
> 
> Answer: Phishing

> Is the red team permitted to attack 192.168.1.0/24? (Y/N)
> 
> Answer: N


## Task 4: Campaign Planning 

Type of Plans:

- Engagement Plan
  - An overarching description of technical requirements of the red team.
  - CONOPS, Resource and Personnel Requirements, Timelines
- Operations Plan
  - An expansion of the Engagement Plan. Goes further into specifics of each detail.
  - Operators, Known Information, Responsibilities, etc.
- Mission Plan
  - The exact commands to run and execution time of the engagement.
  - Commands to run, Time Objectives, Responsible Operator, etc.
- Remediation Plan
  - Defines how the engagement will proceed after the campaign is finished.
  - Report, Remediation consultation, etc.


> Read the above and move on to engagement documentation.
> 
> Answer: No answer needed

## Task 5: Engagement Documentation  


> Read the above and move on to the upcoming engagement specific tasks. 
> 
> Answer: No answer needed

## Task 6: Concept of Operations 

> Read the example CONOP (Concept of Operation) and answer the questions below. 
>
> Answer: No answer needed

> Based on customer security posture and maturity, the TTP of the threat group: FIN6, will be employed throughout the engagement.
>
> Answer: No answer needed

> How long will the engagement last?
>
> Answer: 1 month

> How long is the red cell expected to maintain persistence?
>
> Answer: 3 weeks

> What is the primary tool used within the engagement?
>
> Answer: Cobalt Strike

## Task 7 Resource Plan
