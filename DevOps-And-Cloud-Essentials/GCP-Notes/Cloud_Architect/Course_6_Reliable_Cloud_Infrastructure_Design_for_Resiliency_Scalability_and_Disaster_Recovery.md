# Design for Resiliency, Scalability, Disaster Recovery & Security

- **Resiliency, Scalability, Disaster Recovery**: Implement technologies and processes that assure business continuity in the event of a disaster.
- **Security**: Implement policies that minimize security risks, such as auditing, separation of duties and least privilege.
- **Capacity Planning & Cost Optimization**: Identify ways to optimize resources and minimize cost.
- **Deployment, Monitoring and Alerting, and Incident Response**

## Content

* [Detailled Content](#detailled-content)
* [Design for Resiliency, Scalability, and Disaster Recovery](#design-for-resiliency-scalability-and-disaster-recovery)
   * [Overview](#overview)
   * [Failure Due to Loss](#failure-due-to-loss)
   * [Failure Due to Overload](#failure-due-to-overload)
   * [Coping with Failure](#coping-with-failure)
   * [Business Continuity &amp; Disaster Recovery](#business-continuity--disaster-recovery)
   * [Scalable &amp; Resilient Design](#scalable--resilient-design)
   * [Application: Out of Service!](#application-out-of-service)
   * [Design Challenge #4: Redesign for Time](#design-challenge-4-redesign-for-time)
* [Design for Security](#design-for-security)
   * [Overview](#overview-1)
   * [Cloud Security](#cloud-security)
   * [Network Access Control &amp; Firewalls](#network-access-control--firewalls)
   * [Protections Against Denial of Service](#protections-against-denial-of-service)
   * [Resource Sharing &amp; Isolation](#resource-sharing--isolation)
   * [Data Encryption &amp; Key Management](#data-encryption--key-management)
   * [Design for Security: Identity Access &amp; Auditing](#design-for-security-identity-access--auditing)
   * [Application: Photo Service - Intentional Attack](#application-photo-service---intentional-attack)
   * [Design Challenge #5: Defense in Depth](#design-challenge-5-defense-in-depth)
* [Capacity Planning &amp; Cost Optimization](#capacity-planning--cost-optimization)
   * [Overview](#overview-2)
   * [Capacity Planning](#capacity-planning)
   * [Pricing](#pricing)
   * [Application: Photo Service - Cost &amp; Capacity](#application-photo-service---cost--capacity)
   * [Design Challenge #6: Dimensioning](#design-challenge-6-dimensioning)
* [Deployment, Monitoring and Alerting, and Incident Response](#deployment-monitoring-and-alerting-and-incident-response)
   * [Learning Objectives](#learning-objectives)
   * [Overview](#overview-3)
   * [Deployment](#deployment)
   * [Monitoring &amp; Alerting](#monitoring--alerting)
   * [Incident Response](#incident-response)
   * [Application: Stabilization &amp; Operation](#application-stabilization--operation)
   * [Design Challenge #7: Monitoring &amp; Alerting](#design-challenge-7-monitoring--alerting)
   * [Lab: Deployment Manager - Full Production](#lab-deployment-manager---full-production)
* [Resources/Articles](#resourcesarticles)

## Detailled Content

* [Content](#content)
* [Detailled Content](#detailled-content)
* [Design for Resiliency, Scalability, and Disaster Recovery](#design-for-resiliency-scalability-and-disaster-recovery)
   * [Overview](#overview)
   * [Failure Due to Loss](#failure-due-to-loss)
      * [Failure is mandatory](#failure-is-mandatory)
      * [Single Point of failure](#single-point-of-failure)
      * [Design to avoid Single Point of failure: N 2 (a spare spare)](#design-to-avoid-single-point-of-failure-n2-a-spare-spare)
      * [Correlated failures](#correlated-failures)
      * [Design to avoid correlated failures](#design-to-avoid-correlated-failures)
   * [Failure Due to Overload](#failure-due-to-overload)
      * [Failover design for reliability](#failover-design-for-reliability)
      * [Cascading failures](#cascading-failures)
      * [Design to avoid Cascading failures](#design-to-avoid-cascading-failures)
      * [Queries-of-Death overload failure](#queries-of-death-overload-failure)
      * [Positive feedback cycle overload failure](#positive-feedback-cycle-overload-failure)
      * [Detect overload early: Early warning systems (<em>canaries</em>)](#detect-overload-early-early-warning-systems-canaries)
   * [Coping with Failure](#coping-with-failure)
      * [Forest fires or Controlled burns](#forest-fires-or-controlled-burns)
      * [Prepare the team](#prepare-the-team)
      * [Incorporate failure into SLOs](#incorporate-failure-into-slos)
      * [Monthly meetings to build processes](#monthly-meetings-to-build-processes)
      * [Strategies for dealing with failure](#strategies-for-dealing-with-failure)
   * [Business Continuity &amp; Disaster Recovery](#business-continuity--disaster-recovery)
      * [Cloud DNS: 100\x availability](#cloud-dns-100-availability)
      * [Data Integrity](#data-integrity)
      * [Reliable recovery with Lazy Deletion](#reliable-recovery-with-lazy-deletion)
      * [backup, archive, RESTORE!](#backup-archive-restore)
      * [Tiered backup for resiliency](#tiered-backup-for-resiliency)
         * [Cloud Storage features for backup and DR](#cloud-storage-features-for-backup-and-dr)
      * [Prepare the team for disasters](#prepare-the-team-for-disasters)
   * [Scalable &amp; Resilient Design](#scalable--resilient-design)
      * [Design pattern: General design for scalable &amp; resilient apps](#design-pattern-general-design-for-scalable--resilient-apps)
      * [Microservices design for scalable &amp; resilient streaming](#microservices-design-for-scalable--resilient-streaming)
      * [12-factor system &amp; application design in GCP](#12-factor-system--application-design-in-gcp)
      * [Processes for simple, iterative, aligned development](#processes-for-simple-iterative-aligned-development)
   * [Application: Out of Service!](#application-out-of-service)
      * [Business problem](#business-problem)
      * [Systematic logical troubleshooting](#systematic-logical-troubleshooting)
      * [Collaboration &amp; communication: Report, Document, build policy](#collaboration--communication-report-document-build-policy)
      * [Break down business logic on the photo service](#break-down-business-logic-on-the-photo-service)
      * [What about our Service Level Objectives (SLOs) and Indicators (SLIs)](#what-about-our-service-level-objectives-slos-and-indicators-slis)
   * [Design Challenge #4: Redesign for Time](#design-challenge-4-redesign-for-time)
      * [Design Challenge: Log aggregation delayed troubleshooting](#design-challenge-log-aggregation-delayed-troubleshooting)
      * [Troubleshooting &amp; solution](#troubleshooting--solution)
* [Design for Security](#design-for-security)
   * [Overview](#overview-1)
   * [Cloud Security](#cloud-security)
      * [Google's strategy for cloud security: "Pervasive efense in depth"](#googles-strategy-for-cloud-security-pervasive-efense-in-depth)
      * [Cloud Networking Security: Defense in Depth](#cloud-networking-security-defense-in-depth)
   * [Network Access Control &amp; Firewalls](#network-access-control--firewalls)
      * [Firewall configuration: 1st line of defense for access](#firewall-configuration-1st-line-of-defense-for-access)
      * [Design for securely accessing VMs](#design-for-securely-accessing-vms)
      * [API access control with Cloud Endpoints](#api-access-control-with-cloud-endpoints)
   * [Protections Against Denial of Service](#protections-against-denial-of-service)
      * [Edge protections agaisnt DDoS](#edge-protections-agaisnt-ddos)
   * [Resource Sharing &amp; Isolation... a compromise](#resource-sharing--isolation-a-compromise)
      * [VPC isolation through public IPs](#vpc-isolation-through-public-ips)
      * [IP address isolation using VPN tunneling](#ip-address-isolation-using-vpn-tunneling)
      * [Cross-project VPC network peering](#cross-project-vpc-network-peering)
      * [Cross-organization VPC network peering](#cross-organization-vpc-network-peering)
      * [Shared VPC](#shared-vpc)
      * [Isolation through multiple network interface](#isolation-through-multiple-network-interface)
      * [Access GCP services over internal IP](#access-gcp-services-over-internal-ip)
   * [Data Encryption &amp; Key Management](#data-encryption--key-management)
      * [Server-side encryption](#server-side-encryption)
      * [Customer manager encryption keys (CMEK)](#customer-manager-encryption-keys-cmek)
      * [Customer Supplied encryption keys (CSEK)](#customer-supplied-encryption-keys-csek)
      * [Persistent Disk Encrytion with CSEK](#persistent-disk-encrytion-with-csek)
      * [Moore control over encryption](#moore-control-over-encryption)
   * [Design for Security: Identity Access &amp; Auditing](#design-for-security-identity-access--auditing)
      * [Identity Access Management](#identity-access-management)
      * [Service Accounts](#service-accounts)
      * [GCP security auditing with Forseti-security (Open Source)](#gcp-security-auditing-with-forseti-security-open-source)
      * [Cloud Audit Logging](#cloud-audit-logging)
      * [External audits &amp; GCP Standards Compliance](#external-audits--gcp-standards-compliance)
   * [Application: Photo Service - Intentional Attack](#application-photo-service---intentional-attack)
      * [Business problem](#business-problem-1)
      * [Break down business logic on the photo service](#break-down-business-logic-on-the-photo-service-1)
      * [Security checklist](#security-checklist)
   * [Design Challenge #5: Defense in Depth](#design-challenge-5-defense-in-depth)
* [Capacity Planning &amp; Cost Optimization](#capacity-planning--cost-optimization)
   * [Overview](#overview-2)
   * [Capacity Planning](#capacity-planning)
      * [1. Forecast](#1-forecast)
         * [Forecast estimation](#forecast-estimation)
         * [Instance overhead estimation](#instance-overhead-estimation)
         * [Persistent disks estimation](#persistent-disks-estimation)
         * [Network capacity estimation](#network-capacity-estimation)
         * [Workload estimation](#workload-estimation)
      * [Allocate](#allocate)
      * [Approve](#approve)
      * [Deploy](#deploy)
   * [Pricing](#pricing)
      * [Optimize VMs cost](#optimize-vms-cost)
      * [Optimize Disks cost](#optimize-disks-cost)
      * [Optimize Network cost](#optimize-network-cost)
   * [Application: Photo Service - Cost &amp; Capacity](#application-photo-service---cost--capacity)
      * [Business problem](#business-problem-2)
   * [Design Challenge #6: Dimensioning](#design-challenge-6-dimensioning)
      * [Growth status for BigTable](#growth-status-for-bigtable)
* [Deployment, Monitoring and Alerting, and Incident Response](#deployment-monitoring-and-alerting-and-incident-response)
   * [Learning Objectives](#learning-objectives)
   * [Deployment](#deployment)
   * [Monitoring &amp; Alerting](#monitoring--alerting)
      * [SRE pyramid: Monitoring is measuring](#sre-pyramid-monitoring-is-measuring)
      * [Push-based and Pull-based metrics](#push-based-and-pull-based-metrics)
      * [Black box monitoring (affecting user experience)](#black-box-monitoring-affecting-user-experience)
      * [White box monitoring (monitoring services)](#white-box-monitoring-monitoring-services)
      * [Carefully output of monitoring systems: alerts,](#carefully-output-of-monitoring-systems-alerts)
      * [12-factor administration &amp; operation in GCP: Stack driver](#12-factor-administration--operation-in-gcp-stack-driver)
      * [Some features of Stack driver](#some-features-of-stack-driver)
   * [Incident Response](#incident-response)
      * [Structured incident response](#structured-incident-response)
      * [List of SRE processes](#list-of-sre-processes)
      * [12-factor guidelines on administration and management tasks](#12-factor-guidelines-on-administration-and-management-tasks)
      * [Build a playbook based on alerts](#build-a-playbook-based-on-alerts)
         * [Create "easy buttons" for quick fix](#create-easy-buttons-for-quick-fix)
         * [Balance interrupt-driven work and Incident Response](#balance-interrupt-driven-work-and-incident-response)
   * [Application: Stabilization &amp; Operation](#application-stabilization--operation)
   * [Design Challenge #7: Monitoring &amp; Alerting](#design-challenge-7-monitoring--alerting)
      * [Business Challenge](#business-challenge)
      * [What monitoring and alerting to set up for the logs](#what-monitoring-and-alerting-to-set-up-for-the-logs)
      * [Google's reference architectures online](#googles-reference-architectures-online)
   * [Lab: Deployment Manager - Full Production](#lab-deployment-manager---full-production)
* [Resources/Articles](#resourcesarticles)



## Design for Resiliency, Scalability, and Disaster Recovery

Implement technologies and processes that assure business continuity in the event of a disaster.

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/pWlcV/design-for-resiliency-scalability-and-disaster-recovery-overview)

### Overview

This module deals with resiliency. The ability of a system to stay available and to bounce back from problems. But what is a resilient or available design? Is resiliency something you can add? Is it a feature you can turn on? Not really. Resiliency is the quality of a design that accounts for and handles failure. One principle of design is that sometimes a quality you want in the system isn't something you can really do anything about. To get the quality you want you have to look 180 degrees in the opposite direction. In this case, to get availability you have to look at and deal with the potential causes and sources of failure.

This module is designed for:

- **resiliency**,
- **scalability** and,
- **disaster recovery**.

<img src="../Images/Resiliency_Definition.png"
     alt="Resiliency_Definition.png"
     style="float: left; margin-right: 10px;" />

we're gonna be covering failure; in general, failure due to a loss, failure due to overload. How do we cope with failure? Here, go through to a psychiatrist on that one. What about business continuity? How do we continue if there is a failure and disaster recovery? If there's something major, how do we recover completely from this? Then we will finally talk about scalable and resilient design, which is supposedly supposed to offset all of these bad things from happening or at least dealing with them at least. So then we're going to have an out-of-service issue with our photo service, and then we're going to have to redesign our logging system. 

### Failure Due to Loss

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/1RhEP/design-for-resiliency-scalability-and-disaster-recovery-failure-due-to-loss)

#### Failure is mandatory

For a period of time in the 1960s, a research team was trying to develop a perfect conductor. The theory was that if they could grow a perfect crystal and metal wire, there would be no signal loss and, therefore, no errors in communication. And they succeeded in creating the perfect wire. However, when they tested it, sometimes there were still errors. Do you know why? It's because we live in a quantum mechanical universe, and the location of electrons moving down a conductor is probabilistic. So, occasionally, an electron will appear outside of the wire and get lost. Errors are going to happen. Loss is going to happen.

> So the challenge isn't to avoid it, but to accept it and deal with it.

In this lesson, you'll learn about designing systems to handle failure caused by loss of resources.

<img src="../Images/Resiliency_Failure_Is_Mandatory.png"
     alt="Resiliency_Failure_Is_Mandatory.png"
     style="float: left; margin-right: 10px;" />

#### Single Point of failure

<img src="../Images/Resiliency_Failure_Single_Point_of_Failure.png"
     alt="Resiliency_Failure_Single_Point_of_Failure.png"
     style="float: left; margin-right: 10px;" />

#### Design to avoid Single Point of failure: N+2 (a spare spare)

<img src="../Images/Resiliency_Failure_Design_for_Single_Point_of_Failure.png"
     alt="Resiliency_Failure_Design_for_Single_Point_of_Failure.png"
     style="float: left; margin-right: 10px;" />

#### Correlated failures

<img src="../Images/Resiliency_Failure_Correlated_Failures.png"
     alt="Resiliency_Failure_Correlated_Failures.png"
     style="float: left; margin-right: 10px;" />

#### Design to avoid correlated failures

<img src="../Images/Resiliency_Failure_Design_to_Avoid_Correlated_Failures.png"
     alt="Resiliency_Failure_Design_to_Avoid_Correlated_Failures.png"
     style="float: left; margin-right: 10px;" />


### Failure Due to Overload

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/LbKLf/design-for-resiliency-scalability-and-disaster-recovery-failure-due-to-overload)

When a system is overloaded at some point the system crosses over into nonlinear behavior. It can crash, thrash, stop responding or break adjacent resources at the service depends on. There's a very specific relationship between failure due to loss and failure due to overload. Imagine for example that a resource is lost and you queue up the work for that resource. All the work that comes in gets backlogged. When the resource is restored, there's this tremendous backlog of work to get done before any new work can be handled and if there's enough of it, nothing new ever gets done. So then the system goes from being totally unavailable due to resource loss, to totally unavailable due to overload. In this lesson you'll learn about the common causes of overload failure and how to plan for them and deal with them. When it comes to overload, prevention is really the best solution so design is critical.

<img src="../Images/Resiliency_Failure_Due_to_Overload.png"
     alt="Resiliency_Failure_Due_to_Overload.png"
     style="float: left; margin-right: 10px;" />

#### Failover design for reliability

<img src="../Images/Resiliency_Failure_Due_to_Overload_Failover_Design_for_Reliability.png"
     alt="Resiliency_Failure_Due_to_Overload_Failover_Design_for_Reliability.png"
     style="float: left; margin-right: 10px;" />

#### Cascading failures

<img src="../Images/Resiliency_Failure_Due_to_Overload_Cascading_Failures.png"
     alt="Resiliency_Failure_Due_to_Overload_Cascading_Failures.png"
     style="float: left; margin-right: 10px;" />

#### Design to avoid Cascading failures

<img src="../Images/Resiliency_Failure_Due_to_Overload_Design_to_Avoid_Cascading_Failures.png"
     alt="Resiliency_Failure_Due_to_Overload_Design_to_Avoid_Cascading_Failures.png"
     style="float: left; margin-right: 10px;" />

Example:

<img src="../Images/Resiliency_Failure_Due_to_Overload_Design_to_Avoid_Cascading_Failures_Example.png"
     alt="Resiliency_Failure_Due_to_Overload_Design_to_Avoid_Cascading_Failures_Example.png"
     style="float: left; margin-right: 10px;" />

<img src="../Images/Resiliency_Failure_Due_to_Overload_Design_to_Avoid_Cascading_Failures_Mitigate_Incast_Failure.png"
     alt="Resiliency_Failure_Due_to_Overload_Design_to_Avoid_Cascading_Failures_Mitigate_Incast_Failure.png"
     style="float: left; margin-right: 10px;" />

#### Queries-of-Death overload failure

<img src="../Images/Resiliency_Failure_Due_to_Overload_QueriesOfDeath_Failures.png"
     alt="Resiliency_Failure_Due_to_Overload_QueriesOfDeath_Failures.png"
     style="float: left; margin-right: 10px;" />

#### Positive feedback cycle overload failure

<img src="../Images/Resiliency_Failure_Due_to_Overload_Positive_Feedback_Overload_Failures.png"
     alt="Resiliency_Failure_Due_to_Overload_Positive_Feedback_Overload_Failures.png"
     style="float: left; margin-right: 10px;" />

#### Detect overload early: Early warning systems (_canaries_)

<img src="../Images/Resiliency_Failure_Due_to_Overload_Detect_Early_Canaries.png"
     alt="Resiliency_Failure_Due_to_Overload_Detect_Early_Canaries.png"
     style="float: left; margin-right: 10px;" />

### Coping with Failure

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/jm9LV/design-for-resiliency-scalability-and-disaster-recovery-coping-with-failure)


The time to prepare for an emergency is before it happens. Moreover, the way to prepare for failure is to embrace it, except that sooner or later, it's inevitable. By making the processes and behaviors you want part of the normal routine operations, you avoid the surprise.

For example, if you know that a zone outage is possible, consider establishing rotating outages as part of the routine operations. Here's another idea, if you know that the users expect 99.95 percent availability, and your service is operating at 99.99 percent availability, consider using that 0.04 percent gap to exercise your resiliency and recovery designs. Finally, don't underestimate the importance of meetings. Circumstances are going to change, if you surround the technical processes with the right human processes, the team will catch the issues before they become emergencies.

<img src="../Images/Resiliency_Coping_with_Failure.png"
     alt="Resiliency_Coping_with_Failure.png"
     style="float: left; margin-right: 10px;" />


#### Forest fires or Controlled burns

<img src="../Images/Resiliency_Coping_with_Failure_Forest_Fire_or_Controlled_Burn.png"
     alt="Resiliency_Coping_with_Failure_Forest_Fire_or_Controlled_Burn.png"
     style="float: left; margin-right: 10px;" />

#### Prepare the team

<img src="../Images/Resiliency_Coping_with_Failure_Prepare_the_Team.png"
     alt="Resiliency_Coping_with_Failure_Prepare_the_Team.png"
     style="float: left; margin-right: 10px;" />

#### Incorporate failure into SLOs

<img src="../Images/Resiliency_Coping_with_Failure_Include_in_SLOs.png"
     alt="Resiliency_Coping_with_Failure_Include_in_SLOs.png"
     style="float: left; margin-right: 10px;" />

#### Monthly meetings to build processes

<img src="../Images/Resiliency_Coping_with_Failure_Meeting_Monthly_Meetings.png"
     alt="Resiliency_Coping_with_Failure_Meeting_Monthly_Meetings.png"
     style="float: left; margin-right: 10px;" />

#### Strategies for dealing with failure

<img src="../Images/Resiliency_Coping_with_Failure_Strategies.png"
     alt="Resiliency_Coping_with_Failure_Strategies.png"
     style="float: left; margin-right: 10px;" />


### Business Continuity & Disaster Recovery

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/FYikQ/design-for-resiliency-scalability-and-disaster-recovery-business-continuity-and)

The overall strategy for business continuity can be summed up in this motto;

> No surprises.

Whatever's happening or could happen, you want to find out about it early and you want to give yourself plenty of recovery options in the design of your system. Now that's a balance.

How much resource and energy do you want to spend on insurance? You might have great recovery systems built into your design, but how do you know they're working, and how do you know that something hasn't changed and those systems haven't quietly stopped working? You need to understand what level of testing and exercise of the recovery systems gives you confidence. That will help you decide what to include in the design elements and also help shape the human processes that operate and maintain the system. 

<img src="../Images/Resiliency_Business_Continuity_Disaster_Recovery.png"
     alt="Resiliency_Business_Continuity_Disaster_Recovery.png"
     style="float: left; margin-right: 10px;" />

#### Cloud DNS: 100% availability

<img src="../Images/Resiliency_Business_Continuity_Disaster_Recovery_CloudDNS.png"
     alt="Resiliency_Business_Continuity_Disaster_Recovery_CloudDNS.png"
     style="float: left; margin-right: 10px;" />

#### Data Integrity

<img src="../Images/Resiliency_Business_Continuity_Disaster_Recovery_Data_Integrity.png"
     alt="Resiliency_Business_Continuity_Disaster_Recovery_Data_Integrity.png"
     style="float: left; margin-right: 10px;" />

#### Reliable recovery with Lazy Deletion

<img src="../Images/Resiliency_Business_Continuity_Disaster_Recovery_Lazy_or_Soft_Deletion.png"
     alt="Resiliency_Business_Continuity_Disaster_Recovery_Lazy_or_Soft_Deletion.png"
     style="float: left; margin-right: 10px;" />

#### backup, archive, RESTORE!

<img src="../Images/Resiliency_Business_Continuity_Disaster_Recovery_Backup_Archive_Restore.png"
     alt="Resiliency_Business_Continuity_Disaster_Recovery_Backup_Archive_Restore.png"
     style="float: left; margin-right: 10px;" />

#### Tiered backup for resiliency

<img src="../Images/Resiliency_Business_Continuity_Disaster_Tiered_Backup_Services.png"
     alt="Resiliency_Business_Continuity_Disaster_Tiered_Backup_Services.png"
     style="float: left; margin-right: 10px;" />

##### Cloud Storage features for backup and DR

<img src="../Images/Resiliency_Business_Continuity_Disaster_Tiered_Backup_Services_Cloud_Storage.png"
     alt="Resiliency_Business_Continuity_Disaster_Tiered_Backup_Services_Cloud_Storage.png"
     style="float: left; margin-right: 10px;" />

#### Prepare the team for disasters

<img src="../Images/Resiliency_Business_Continuity_Disaster_Practice_Document_Prepare_the_Team.png"
     alt="Resiliency_Business_Continuity_Disaster_Practice_Document_Prepare_the_Team.png"
     style="float: left; margin-right: 10px;" />


### Scalable & Resilient Design

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/E9vLK/design-for-resiliency-scalability-and-disaster-recovery-scalable-and-resilient) 

Vertical scaling makes components bigger, but it leaves them as a single point of failure. For example, swapping out a single VM for VM with larger capacity, still means that VM can fail and potentially crash your service. Horizontal scaling make services bigger through multiplicity. It leaves a unit capacity the same, but increases the pool of units. For example, instead of growing a larger VM, you could move to a design with a pool consisting of multiple smaller VMs. That not only makes it scalable, but resilient, because if one VM is lost, the others can pick up the workload until replacement is added. There are several steps you can take to make your design resilient, and they're covered in this lesson

1. **Health checks to monitor instances**

<img src="../Images/Resiliency_Design_Health_Checks.png"
     alt="Resiliency_Design_Health_Checks.png"
     style="float: left; margin-right: 10px;" />

2. **Automatically replace instances**

<img src="../Images/Resiliency_Design_Auto_Replace_Instances.png"
     alt="Resiliency_Design_Auto_Replace_Instances.png"
     style="float: left; margin-right: 10px;" />

3. Resilient Storage: Cloud Storage, Cloud SQL

<img src="../Images/Resiliency_Design_Resilient_Storage.png"
     alt="Resiliency_Design_Resilient_Storage.png"
     style="float: left; margin-right: 10px;" />

4. Resilient Network

<img src="../Images/Resiliency_Design_Resilient_Network.png"
     alt="Resiliency_Design_Resilient_Network.png"
     style="float: left; margin-right: 10px;" />

#### Design pattern: General design for scalable & resilient apps

<img src="../Images/Resiliency_Design_Design_Pattern_General_Design_for_Scalable_Resilient_Apps.png"
     alt="Resiliency_Design_Design_Pattern_General_Design_for_Scalable_Resilient_Apps.png"
     style="float: left; margin-right: 10px;" />


* Handles **loss of instance**

<img src="../Images/General_Design_for_Scalable_Resilient_Apps_Loss_of_Instance.png"
     alt="General_Design_for_Scalable_Resilient_Apps_Loss_of_Instance.png"
     style="float: left; margin-right: 10px;" />

* Handles **loss of zone**

<img src="../Images/General_Design_for_Scalable_Resilient_Apps_Loss_of_Zone.png"
     alt="General_Design_for_Scalable_Resilient_Apps_Loss_of_Zone.png"
     style="float: left; margin-right: 10px;" />

* Handles **loss of database**

<img src="../Images/General_Design_for_Scalable_Resilient_Apps_Loss_of_Database.png"
     alt="General_Design_for_Scalable_Resilient_Apps_Loss_of_Database.png"
     style="float: left; margin-right: 10px;" />

* Handles **full disaster recovery**

<img src="../Images/General_Design_for_Scalable_Resilient_Apps_Full_Disaster_Recovery.png"
     alt="General_Design_for_Scalable_Resilient_Apps_Full_Disaster_Recovery.png"
     style="float: left; margin-right: 10px;" />

#### Microservices design for scalable & resilient streaming

<img src="../Images/Resiliency_Design_Design_Pattern_Microservices_Design.png"
     alt="Resiliency_Design_Design_Pattern_Microservices_Design.png"
     style="float: left; margin-right: 10px;" />

#### 12-factor system & application design in GCP

<img src="../Images/Resiliency_Design_Design_Pattern_12_Factor_Design.png"
     alt="Resiliency_Design_Design_Pattern_12_Factor_Design.png"
     style="float: left; margin-right: 10px;" />

#### Processes for simple, iterative, aligned development

<img src="../Images/Resiliency_Design_Processes.png"
     alt="Resiliency_Design_Processes.png"
     style="float: left; margin-right: 10px;" />

<img src="../Images/Resiliency_Design_Processes_Iterate.png"
     alt="Resiliency_Design_Processes_Iterate.png"
     style="float: left; margin-right: 10px;" />

This module covered several related design goals, including:

* reliability,
* scalability,
* and disaster recovery.

The first subject was availability and reliability. A key concept is that planning for failure and dealing with failure in your design leads to improved reliability. Failure can occur due to the loss of a resource or it can occur due to overload. You must be careful when making adjustments to a system, that you don't accidentally create the potential for an overload failure when you're trying to improve resiliency to a loss failure. The second subject was disaster recovery. You learned that planning for disaster and preparing for recovery is key. The third was scalable and resilient design. That brings together many of the design principles you've seen in the previous modules, and shows how they all fit together into a general resilient solution.


### Application: Out of Service!

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/SeWZH/out-of-service) 

#### Business problem

It's a **Design problem**, we lost an entire zone!!!

<img src="../Images/Application_Photo_Service_Crashed.png"
     alt="Application_Photo_Service_Crashed.png"
     style="float: left; margin-right: 10px;" />

The popular and growing photo services suddenly crashed:

* How do you handle a major outage?
* What could the cause be? What changes can you make the design so this problem doesn't take down the entire service again in the future?

<img src="../Images/Application_Photo_Service_Crashed_Problem.png"
     alt="Application_Photo_Service_Crashed_Problem.png"
     style="float: left; margin-right: 10px;" />


**Have a plan for dealing with a major outage**

<img src="../Images/Application_Photo_Service_Crashed_Solution_Have_Process_in_Place.png"
     alt="Application_Photo_Service_Crashed_Solution_Have_Process_in_Place.png"
     style="float: left; margin-right: 10px;" />


#### Systematic logical troubleshooting

**Service loss due to zone outage**

<img src="../Images/Application_Photo_Service_Crashed_Troubleshooting.png"
     alt="Application_Photo_Service_Crashed_Troubleshooting.png"
     style="float: left; margin-right: 10px;" />


#### Collaboration & communication: Report, Document, build policy

So you want to **move these servers to multiple zones.**

<img src="../Images/Application_Photo_Service_Crashed_Solution_Multiple_Zones.png"
     alt="Application_Photo_Service_Crashed_Solution_Multiple_Zones.png"
     style="float: left; margin-right: 10px;" />

<img src="../Images/dummy.png"
     alt="dummy.png"
     style="float: left; margin-right: 10px;" />
<img src="../Images/dummy.png"
     alt="dummy.png"
     style="float: left; margin-right: 10px;" />
<img src="../Images/dummy.png"
     alt="dummy.png"
     style="float: left; margin-right: 10px;" />


#### Break down business logic on the photo service

#### What about our Service Level Objectives (SLOs) and Indicators (SLIs)

SLOs didn't change again. We just added more redundancy, making the service span multiple zones.


<img src="../Images/Application_Photo_Service_Crashed_SLOs.png"
     alt="Application_Photo_Service_Crashed_SLOs.png"
     style="float: left; margin-right: 10px;" />


### Design Challenge #4: Redesign for Time



<img src="../Images/Application_Photo_Service_Crashed_Again_SinglePointOfFAilure.png"
     alt="Application_Photo_Service_Crashed_Again_SinglePointOfFAilure.png"
     style="float: left; margin-right: 10px;" />


**Need to scale frontend server**

<img src="../Images/Application_Photo_Service_Crashed_Again_Solution_Scale_Upload_Servers_Across_Zones.png"
     alt="Application_Photo_Service_Crashed_Again_Solution_Scale_Upload_Servers_Across_Zones.png"
     style="float: left; margin-right: 10px;" />

**Prevent overload: MAke load testing real**

<img src="../Images/Application_Photo_Service_Crashed_Again_Prevent_Overload_with_Real_Testing.png"
     alt="Application_Photo_Service_Crashed_Again_Prevent_Overload_with_Real_Testing.png"
     style="float: left; margin-right: 10px;" />

**Scaling requires breaking state out of the upload server**

<img src="../Images/Application_Photo_Service_Crashed_Again_Scale_Upload_Server_Stateless.png"
     alt="Application_Photo_Service_Crashed_Again_Scale_Upload_Server_Stateless.png"
     style="float: left; margin-right: 10px;" />


What does that look like?

**Need to make Frontend servers stateless**

<img src="../Images/Application_Photo_Service_Crashed_Again_Scale_Upload_Server_Stateless_How.png"
     alt="Application_Photo_Service_Crashed_Again_Scale_Upload_Server_Stateless_How.png"
     style="float: left; margin-right: 10px;" />

**Update on SLOs and SLIs**

You see what it is that you can measure and then quantify it.

<img src="../Images/Application_Photo_Service_Crashed_Again_New_SLOs.png"
     alt="Application_Photo_Service_Crashed_Again_New_SLOs.png"
     style="float: left; margin-right: 10px;" />


#### Design Challenge: Log aggregation delayed troubleshooting

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/9EsdO/design-challenge-4-redesign-for-time)

When the photo service crashed, it became evident that troubleshooting was taking too long. The reason for the delay was tracked to log aggregation. The aggregated logs needed for troubleshooting were delayed. Worse, the more problems that are occurring in the system, the more log entries are generated and the longer it takes for the aggregated logs to become available for troubleshooting. Can you redesign the log system to eliminate the bottlenecks? Watch the lesson that describes the problem, then come up with your own solution. When you're ready, continue the lesson to see a sample solution.

<img src="../Images/Design_Challenge_Redesign_for_Time.png"
     alt="Design_Challenge_Redesign_for_Time.png"
     style="float: left; margin-right: 10px;" />

<img src="../Images/Design_Challenge_Redesign_for_Time_Problem.png"
     alt="Design_Challenge_Redesign_for_Time_Problem.png"
     style="float: left; margin-right: 10px;" />

If you remember the **12-factor design**, it says to **treat log events as streams** so that should be a clue. The business issue is servant's service resiliency. It's just taking too long to troubleshoot service issues, batch processing of the logs, just simply does not support live service troubleshooting. It's causing delays, we can't meet our service level objectives, we can't identify and respond to incidents in times. So here's the design challenge. Replace the cron batch processing with stream processing and here's our hint. Try to consider a microservices design.


<img src="../Images/Design_Challenge_Redesign_for_Time_Challenge.png"
     alt="Design_Challenge_Redesign_for_Time_Challenge.png"
     style="float: left; margin-right: 10px;" />

#### Troubleshooting & solution

<img src="../Images/Design_Challenge_Redesign_for_Time_Troubleshooting.png"
     alt="Design_Challenge_Redesign_for_Time_Troubleshooting.png"
     style="float: left; margin-right: 10px;" />

<img src="../Images/Design_Challenge_Redesign_for_Time_Solution.png"
     alt="Design_Challenge_Redesign_for_Time_Solution.png"
     style="float: left; margin-right: 10px;" />

Instead of having a cronjob, we have not converted it to Stream Processing with Cloud Pub/Sub.





## Design for Security

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/bNzDv/design-for-security-overview)

Implement policies that minimize security risks, such as auditing, separation of duties and least privilege.

### Overview

There are **3 kinds of security services** built into the Google Cloud platform: 
1. services that are **transparent and automatic**, such as encryption of data that occurs automatically when data's transported and when it's at rest.
2. services that have **defaults but that offer methods for customizations**, such as using your own encryption keys rather than those provided.
3. services that can be used as part of your security design, but only contribute to security if you choose to use them in your design.

<img src="../Images/Security_Overview.png"
     alt="Security_Overview.png"
     style="float: left; margin-right: 10px;" />

### Cloud Security

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/wMJ6H/design-for-security-cloud-security)

When you migrate an application to the cloud or develop an application in the cloud, there are security benefits that the application inherits simply because of the host environment. Google already has to protect its applications, and many of the benefits of that security effort are built into the infrastructure itself. You need to know about them so you don't accidentally spend effort duplicating them. You also need to know where those services end and your security design begins so that you don't accidentally leave gaps in your security strategy.

#### Google's strategy for cloud security: "Pervasive efense in depth"

<img src="../Images/Security_Google_Strategy.png"
     alt="Security_Google_Strategy.png"
     style="float: left; margin-right: 10px;" />

#### Cloud Networking Security: Defense in Depth

"So the least that we can actually expose to the Internet, all the better"

<img src="../Images/Security_Google_Defense_in_Depth.png"
     alt="Security_Google_Defense_in_Depth.png"
     style="float: left; margin-right: 10px;" />

### Network Access Control & Firewalls

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/tx88k/design-for-security-network-access-control-and-firewalls)

The first level of security where your design can have a significant impact is on network level access control. For example, if you remove the external IP from an instance in a bastion hosts design, you'll eliminate one target that could be attacked. Locking down network access to only what's required is one way to reduce the potential attack surface. 

#### Firewall configuration: 1st line of defense for access

<img src="../Images/Security_Firewalls.png"
     alt="Security_Firewalls.png"
     style="float: left; margin-right: 10px;" />

#### Design for securely accessing VMs

<img src="../Images/Security_Secure_VMs.png"
     alt="Security_Secure_VMs.png"
     style="float: left; margin-right: 10px;" />

#### API access control with Cloud Endpoints

<img src="../Images/Security_API_Control_with_Cloud_Endpoint.png"
     alt="Security_API_Control_with_Cloud_Endpoint.png"
     style="float: left; margin-right: 10px;" />

### Protections Against Denial of Service

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/9I9iK/design-for-security-protections-against-denial-of-service)

Part of the protections against denial of service attacks are built into the cloud infrastructure. The network, for example, uses software defined networking or SDN. Since there are no physical routers and no physical load balancers, there are no actual hardware interfaces that could be overloaded. There are also services that adapt to demand in intelligent ways, and you can use these in your design to afford further protection against overload attacks

<img src="../Images/Security_DDOS.png"
     alt="Security_DDOS.png"
     style="float: left; margin-right: 10px;" />

#### Edge protections agaisnt DDoS

<img src="../Images/Security_Protection_vs_DDOS.png"
     alt="Security_Protection_vs_DDOS.png"
     style="float: left; margin-right: 10px;" />

<img src="../Images/Security_Protection_vs_DDOS_with_Network.png"
     alt="Security_Protection_vs_DDOS_with_Network.png"
     style="float: left; margin-right: 10px;" />

<img src="../Images/Security_Infrastructure_Protection_vs_DDOS.png"
     alt="Security_Infrastructure_Protection_vs_DDOS.png"
     style="float: left; margin-right: 10px;" />

### Resource Sharing & Isolation... a compromise

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/vUZeS/design-for-security-resource-sharing-and-isolation)

Google Cloud Platform provides a rich array of network topology features, that provide different blends of separation isolation, and communication, and sharing. The least secure design is where everything is in a single failure domain, and all the parts communicate and depend directly on one another. There are many ways of separating those parts and providing more private, or tolerant communication channels between them, creating multiple failure domains and therefore better isolation. In submarine design, the parts of the submarine are divided into compartments that can be sealed off from one another. This helps with reliability, because one compartment can flood and be sealed off from the rest. But it also helps with security, because if an attacker gains entry to one compartment, it can be sealed off to limit the damage.

<img src="../Images/Security_Resource_Sharing_Def.png"
     alt="Security_Resource_Sharing_Def.png"
     style="float: left; margin-right: 10px;" />

#### VPC isolation through public IPs

<img src="../Images/Security_Resource_Sharing_VPC_Isolations_Through_Public_IPs.png"
     alt="Security_Resource_Sharing_VPC_Isolations_Through_Public_IPs.png"
     style="float: left; margin-right: 10px;" />

#### IP address isolation using VPN tunneling

<img src="../Images/Security_Resource_Sharing_IP_Isolations_Through_VPN.png"
     alt="Security_Resource_Sharing_IP_Isolations_Through_VPN.png"
     style="float: left; margin-right: 10px;" />

#### Cross-project VPC network peering

<img src="../Images/Security_Resource_Cross_Project.png"
     alt="Security_Resource_Cross_Project.png"
     style="float: left; margin-right: 10px;" />

#### Cross-organization VPC network peering

<img src="../Images/Security_Resource_Cross_Organization_Sharing.png"
     alt="Security_Resource_Cross_Organization_Sharing.png"
     style="float: left; margin-right: 10px;" />

#### Shared VPC

<img src="../Images/Security_Resource_VPC_Sharing.png"
     alt="Security_Resource_VPC_Sharing.png"
     style="float: left; margin-right: 10px;" />

#### Isolation through multiple network interface

<img src="../Images/Security_Resource_Multi_NIC.png"
     alt="Security_Resource_Multi_NIC.png"
     style="float: left; margin-right: 10px;" />

#### Access GCP services over internal IP

<img src="../Images/Security_Resource_GCP_Over_Internal_IP.png"
     alt="Security_Resource_GCP_Over_Internal_IP.png"
     style="float: left; margin-right: 10px;" />


### Data Encryption & Key Management

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/m1739/design-for-security-data-encryption-and-key-management)

You probably already know that Google automatically encrypts data in motion and data at rest, but that description generalizes some of the details that will help you make design decisions. For example, you can use Google's built-in key management, or you can provide your own keys. Also for particularly sensitive data, you can add your own encryption methods in addition to those provided


<img src="../Images/Security_Encryption_All_Data_in_Motion_At_Rest_Encrypted.png"
     alt="Security_Encryption_All_Data_in_Motion_At_Rest_Encrypted.png"
     style="float: left; margin-right: 10px;" />

#### Server-side encryption

<img src="../Images/Security_Encryption.png"
     alt="Security_Encryption.png"
     style="float: left; margin-right: 10px;" />

#### Customer manager encryption keys (CMEK)

<img src="../Images/Security_Custom_Encryption_Keys.png"
     alt="Security_Custom_Encryption_Keys.png"
     style="float: left; margin-right: 10px;" />

#### Customer Supplied encryption keys (CSEK)

<img src="../Images/Security_Custom_Supplied_Encryption_Keys.png"
     alt="Security_Custom_Supplied_Encryption_Keys.png"
     style="float: left; margin-right: 10px;" />

#### Persistent Disk Encrytion with CSEK

<img src="../Images/Security_Persistent_Disk_CSEK.png"
     alt="Security_Persistent_Disk_CSEK.png"
     style="float: left; margin-right: 10px;" />

#### Moore control over encryption

<img src="../Images/Security_Encryption_More_Control.png"
     alt="Security_Encryption_More_Control.png"
     style="float: left; margin-right: 10px;" />

### Design for Security: Identity Access & Auditing

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/k0FDO/design-for-security-identity-access-and-auditing)

Authorization, access to resources is controlled by Google's Identity and Access Management or IAM system. You're already using this service to control authorized access. By using the auditing tools available, you can also check for unwanted actions like attempts at unauthorized access. That can tell you where the attackers interest is focused. So you can add security measures in those areas. 

#### Identity Access Management

<img src="../Images/Security_IAM.png"
     alt="Security_IAM.png"
     style="float: left; margin-right: 10px;" />

#### Service Accounts

<img src="../Images/Security_Service_Account.png"
     alt="Security_Service_Account.png"
     style="float: left; margin-right: 10px;" />

#### GCP security auditing with Forseti-security (Open Source)

<img src="../Images/Security_GCP_Security_Auditing_Wth_Forseti.png"
     alt="Security_GCP_Security_Auditing_Wth_Forseti.png"
     style="float: left; margin-right: 10px;" />

#### Cloud Audit Logging

<img src="../Images/Security_Cloud_Audit_Logging.png"
     alt="Security_Cloud_Audit_Logging.png"
     style="float: left; margin-right: 10px;" />

#### External audits & GCP Standards Compliance


<img src="../Images/Security_Standard_Compliance.png"
     alt="Security_Standard_Compliance.png"
     style="float: left; margin-right: 10px;" />

This module covered security from several perspectives, including identity and access management, data encryption and key management, resource sharing and isolation for compartmentalization, protections against denial of service attacks, network access control, and the automatic protections built into the platform and services. You learn that there are some security that's inherited from the environment. Some is configured with default options that you might want to change, and some security features are optional and can be included in your design, if it makes sense with your security strategy.

### Application: Photo Service - Intentional Attack

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/dwTwf/photo-service-intentional-attack)

There's evidence that hackers are trying to compromise the private information of system users, and maybe try to bring down the service.

That brings up 2 important issues:
1. How does the system keep users data private?
2. How does the system protect against a denial-of-service attack?

Identify the protections already in place that are provided by the platform by default, then consider additional design changes that could provide additional protections.

<img src="../Images/Application_Photo_Service_Intentional_Attack.png"
     alt="Application_Photo_Service_Intentional_Attack.png"
     style="float: left; margin-right: 10px;" />

#### Business problem

<img src="../Images/Application_Photo_Service_Intentional_Attack_Business_Problem.png"
     alt="Application_Photo_Service_Intentional_Attack_Business_Problem.png"
     style="float: left; margin-right: 10px;" />

<img src="../Images/Application_Photo_Service_Intentional_Attack_Business_Problem_Architecture.png"
     alt="Application_Photo_Service_Intentional_Attack_Business_Problem_Architecture.png"
     style="float: left; margin-right: 10px;" />



#### Break down business logic on the photo service

**Lock down the frontend**

<img src="../Images/Application_Photo_Service_Intentional_Attack_Business_Problem_Solution.png"
     alt="Application_Photo_Service_Intentional_Attack_Business_Problem_Solution.png"
     style="float: left; margin-right: 10px;" />

<img src="../Images/Application_Photo_Service_Intentional_Attack_Business_Process_If_DDoS.png"
     alt="Application_Photo_Service_Intentional_Attack_Business_Process_If_DDoS.png"
     style="float: left; margin-right: 10px;" />

<img src="../Images/Application_Photo_Service_Intentional_Attack_Business_Actions_vs_DDoS.png"
     alt="Application_Photo_Service_Intentional_Attack_Business_Actions_vs_DDoS.png"
     style="float: left; margin-right: 10px;" />

1. use Cloud CDN to cache our thumbnails accross the world
2. use Cloud DNS 
3. Implement auto-scaling (instance group)

**Can we protect the backend?**

<img src="../Images/Application_Photo_Service_Intentional_Attack_Backend.png"
     alt="Application_Photo_Service_Intentional_Attack_Backend.png"
     style="float: left; margin-right: 10px;" />

<img src="../Images/Application_Photo_Service_Intentional_Attack_Backend_Lockdown_VPC.png"
     alt="Application_Photo_Service_Intentional_Attack_Backend_Lockdown_VPC.png"
     style="float: left; margin-right: 10px;" />

<img src="../Images/Application_Photo_Service_Intentional_Attack_Backend_Lockdown_Private_Network.png"
     alt="Application_Photo_Service_Intentional_Attack_Backend_Lockdown_Private_Network.png"
     style="float: left; margin-right: 10px;" />

#### Security checklist

<img src="../Images/Application_Photo_Service_Intentional_Attack_Security_Checklist.png"
     alt="Application_Photo_Service_Intentional_Attack_Security_Checklist.png"
     style="float: left; margin-right: 10px;" />


### Design Challenge #5: Defense in Depth

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/vVI8A/design-challenge-5-defense-in-depth)

The security measures you're considering implementing in the photo service will make the system much more secure. But wait a moment, the user information and event information may be contained in the logs. If the log system isn't secure, none of the rest of the system is secure. Watch the lesson that describes the problem, then come up with your own solution. When you're ready, continue the lesson to see a sample solution, and remember that the sample solution is not the best possible solution. It's just an example.


<img src="../Images/Design_Challenge_Security_Log_Files.png"
     alt="Design_Challenge_Security_Log_Files.png"
     style="float: left; margin-right: 10px;" />

**Problem**

<img src="../Images/Design_Challenge_Security_Log_Files_Problem.png"
     alt="Design_Challenge_Security_Log_Files_Problem.png"
     style="float: left; margin-right: 10px;" />

**Possible solution**

<img src="../Images/Design_Challenge_Security_Log_Files_Solution.png"
     alt="Design_Challenge_Security_Log_Files_Solution.png"
     style="float: left; margin-right: 10px;" />

## Capacity Planning & Cost Optimization

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/SsEgy/capacity-planning-and-cost-optimization-overview)

Identify ways to optimize resources and minimize cost.

### Overview

Both forecasting for future demand on a system, and planning the resources for a system, depend on non-abstract large-scale design sometimes called dimensioning.

When you optimize for one factor by changing a resource, there may be other consequences. For example, if you change the VM size to optimize CPU capacity, it's possible that network throughput memory and disk capacity could change as a consequence. So, you really need to think through all the dimensions that are affected by your design and perform the calculations to ensure there's sufficient capacity for your purposes.

A common mistake is to optimize away resiliency. Remember that overcapacity is sometimes included by design to handle bursty periods, growth, or intentional attacks. Failing to recognize the purpose of excess capacity, and then reducing it to save money, can create opportunities for cascade failures.

<img src="../Images/Dimensioning_Capacity_Planning_Pricing.png"
     alt="Dimensioning_Capacity_Planning_Pricing.png"
     style="float: left; margin-right: 10px;" />

### Capacity Planning

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/0h7MF/capacity-planning-and-cost-optimization-capacity-planning)

Capacity planning is **an ongoing cyclical process**.

There are various common measures such as:

* VM instance capacity,
* disk performance,
* network throughput,
* and workload estimations.

Ultimately, you need to be able to answer the question, **is there sufficient resource with reasonable certainty?** One of the key design principles in this course is to allow other factors to influence your design first, and then come back and dimension the design later. You might have to change some of the design for capacity or for better pricing, but at least, you'll make these adjustments knowing what benefits you're trading for reduced cost or better capacity management.

**Capacity planning cycle:**

<img src="../Images/Dimensioning_Cycle.png"
     alt="Dimensioning_Cycle.png"
     style="float: left; margin-right: 10px;" />

#### 1. Forecast

<img src="../Images/Dimensioning_Cycle_Forecast.png"
     alt="Dimensioning_Cycle_Forecast.png"
     style="float: left; margin-right: 10px;" />

##### Forecast estimation

<img src="../Images/Dimensioning_Cycle_Forecast_Estimation.png"
     alt="Dimensioning_Cycle_Forecast_Estimation.png"
     style="float: left; margin-right: 10px;" />


##### Instance overhead estimation

How to NOT overestimate:

<img src="../Images/Dimensioning_Cycle_Forecast_Instance_Overhead_Estimation.png"
     alt="Dimensioning_Cycle_Forecast_Instance_Overhead_Estimation.png"
     style="float: left; margin-right: 10px;" />

##### Persistent disks estimation

<img src="../Images/Dimensioning_Cycle_Forecast_Presistence_Disks_Estimation.png"
     alt="Dimensioning_Cycle_Forecast_Presistence_Disks_Estimation.png"
     style="float: left; margin-right: 10px;" />


##### Network capacity estimation

<img src="../Images/Dimensioning_Cycle_Forecast_Network_Estimation.png"
     alt="Dimensioning_Cycle_Forecast_Network_Estimation.png"
     style="float: left; margin-right: 10px;" />

##### Workload estimation

<img src="../Images/Dimensioning_Cycle_Forecast_Workload_Estimation.png"
     alt="Dimensioning_Cycle_Forecast_Workload_Estimation.png"
     style="float: left; margin-right: 10px;" />

[**Perfkit Benchmarker**](https://github.com/GoogleCloudPlatform/PerfKitBenchmarker) (Open source tool by Google)



<img src="../Images/Dimensioning_Cycle_Forecast_Workload_Estimation_PerfkitBEnchmarker.png"
     alt="Dimensioning_Cycle_Forecast_Workload_Estimation_PerfkitBEnchmarker.png"
     style="float: left; margin-right: 10px;" />

#### Allocate

<img src="../Images/Dimensioning_Cycle_Allocate.png"
     alt="Dimensioning_Cycle_Allocate.png"
     style="float: left; margin-right: 10px;" />

Example with rough estimation:

<img src="../Images/Dimensioning_Cycle_Allocate_Example.png"
     alt="Dimensioning_Cycle_Allocate_Example.png"
     style="float: left; margin-right: 10px;" />

Opportunity for optimization before allocating more resources?

<img src="../Images/Dimensioning_Cycle_Allocate_Opportunity_for_Optimization.png"
     alt="Dimensioning_Cycle_Allocate_Opportunity_for_Optimization.png"
     style="float: left; margin-right: 10px;" />

#### Approve

<img src="../Images/Dimensioning_Cycle_Approve.png"
     alt="Dimensioning_Cycle_Approve.png"
     style="float: left; margin-right: 10px;" />

#### Deploy

First: Test, test, test...

<img src="../Images/Dimensioning_Cycle_Deploy.png"
     alt="Dimensioning_Cycle_Deploy.png"
     style="float: left; margin-right: 10px;" />

<img src="../Images/Dimensioning_Balanced_Approach_to_Dimensioning.png"
     alt="Dimensioning_Balanced_Approach_to_Dimensioning.png"
     style="float: left; margin-right: 10px;" />



### Pricing

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/cwllj/capacity-planning-and-cost-optimization-pricing)

Pricing is commonly used in:

* **cost optimization**,
* **reducing cost**, 
* and also for **budgeting**.

One feature of Google Cloud Platform is that bulk use discounting is built in an automatic for many services. In this lesson, you'll learn about how design choices can influence price.

For example, you may have distributed an element of your solution over multiple regions to improve reliability. However, that distribution design might result in additional network charges for egress traffic. Is the cost of the reliability worthy additional network charges? Pricing estimation, and pricing that follows capacity planning can help you decide.

#### Optimize VMs cost

<img src="../Images/Pricing_Optimize_VM_Cost.png"
     alt="Pricing_Optimize_VM_Cost.png"
     style="float: left; margin-right: 10px;" />

#### Optimize Disks cost

<img src="../Images/Pricing_Optimize_Disks_Cost.png"
     alt="Pricing_Optimize_Disks_Cost.png"
     style="float: left; margin-right: 10px;" />

#### Optimize Network cost

<img src="../Images/Pricing_Optimize_Network_Cost.png"
     alt="Pricing_Optimize_Network_Cost.png"
     style="float: left; margin-right: 10px;" />

VM to VM in the same zone:

<img src="../Images/Pricing_Optimize_Network_Cost_Same_Zone_VM_to_VM.png"
     alt="Pricing_Optimize_Network_Cost_Same_Zone_VM_to_VM.png"
     style="float: left; margin-right: 10px;" />

In this module, you learned about capacity planning, including the planning cycle, and you learned about pricing. The two of them together, capacity and pricing, provide another perspective on design options. You can modify the design for cost optimization or to limit resource usage. One important point is to apply dimensioning to your design after you've considered other functional aspects of the design.


### Application: Photo Service - Cost & Capacity

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/E42DE/photo-service-cost-and-capacity)

Capacity planning for the coming year's completed.

As a final step, you'll look at the VM options and perform non-abstract **cost optimization analysis**.

Given the growing capacity requirements, what makes sense financially to choose for the most cost-effective?:

* a **bigger capacity VM**, 
* or is **sticking with the current size VM**

Can we offer the same service with less money?

<img src="../Images/Application_Photo_Servicecost_Optimization.png"
     alt="Application_Photo_Servicecost_Optimization.png"
     style="float: left; margin-right: 10px;" />

#### Business problem

<img src="../Images/Application_Photo_Service_Budget.png"
     alt="Application_Photo_Service_Budget.png"
     style="float: left; margin-right: 10px;" />

Reviewing the current architecture:

<img src="../Images/Application_Photo_Service_Review_Archtecture.png"
     alt="Application_Photo_Service_Review_Archtecture.png"
     style="float: left; margin-right: 10px;" />

<img src="../Images/Application_Photo_Service_Cost_Optimization_Problem.png"
     alt="Application_Photo_Service_Cost_Optimization_Problem.png"
     style="float: left; margin-right: 10px;" />

We want to make a recommendation: **Should we move to higher cores CPUs?**.7

We first need to **check cost effectiveness**.

<img src="../Images/Application_Photo_Service_Check_Cost_Effectiveness.png"
     alt="Application_Photo_Service_Check_Cost_Effectiveness.png"
     style="float: left; margin-right: 10px;" />


### Design Challenge #6: Dimensioning

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/MyV6T/design-challenge-6-dimensioning)

The photo application service design is now set to auto scale and grow for the projected doubling of demand in the coming year.

**However, that means the log information will also double**. The current storage service is Bigtable.

* Will the additional demand both data and traffic put stresses on Bigtable?
* Will the system need an additional Bigtable node to handle the demand in the coming year?

Watch the lesson that describes the problem then come up with your own solution, and when you're ready, continue the lesson to see a sample solution.

Current layout of our log service:

<img src="../Images/Design_Challenge_Capacity_Planning_Growth_Logs.png"
     alt="Design_Challenge_Capacity_Planning_Growth_Logs.png"
     style="float: left; margin-right: 10px;" />

#### Growth status for BigTable

<img src="../Images/Design_Challenge_Capacity_Planning_Growth_Logs_BigTable.png"
     alt="Design_Challenge_Capacity_Planning_Growth_Logs_BigTable.png"
     style="float: left; margin-right: 10px;" />

What can handle a BigTable node, in nb. of queries (qps), in throughput (MB/s)?

Our current use of BigTable:


<img src="../Images/Design_Challenge_Capacity_Planning_BigTable_Current_Use.png"
     alt="Design_Challenge_Capacity_Planning_BigTable_Current_Use.png"
     style="float: left; margin-right: 10px;" />

- the size of the log payload for each of our workloads (web, app, data): ~552 B
- estimation of log entries per day: ~300 millions entries per day

Our system handles **~154.2 GB/day**, i.e. **~55TB/year**.


**What our service would look like if the usage double inthe coming year?**

<img src="../Images/Design_Challenge_Capacity_Planning_BigTable_Challenge.png"
     alt="Design_Challenge_Capacity_Planning_BigTable_Challenge.png"
     style="float: left; margin-right: 10px;" />

<img src="../Images/Design_Challenge_Capacity_Planning_BigTable_Estimate_Growing_Capacity.png"
     alt="Design_Challenge_Capacity_Planning_BigTable_Estimate_Growing_Capacity.png"
     style="float: left; margin-right: 10px;" />

a BigTable node can handle up to 10 000 qps and 10MB/s of throughput. So doubling the usage of our app can be handled by a single BigTable node, but we need to consider our **storage capacity** reaching 110TB by the end of the 2nd year.

Doing the math, 22 BigTable servers using SSD drives won't be sufficient for the double growth forecasted for the coming year.

<img src="../Images/Design_Challenge_Capacity_Planning_BigTable_Estimate_Growing_Pricing.png"
     alt="Design_Challenge_Capacity_Planning_BigTable_Estimate_Growing_Pricing.png"
     style="float: left; margin-right: 10px;" />


## Deployment, Monitoring and Alerting, and Incident Response

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/OFVf0/deployment-monitoring-and-alerting-and-incident-response-overview)

This module discusses **deploying, operating and maintaining your design**.

One of the things that's been visited repeatedly during this course, is that for a system to stabilize after implementation, it needs to be surrounded by properly prepared and designed behaviors.

What people do while operating and maintaining the system matters. The SLOs and SLIs you've been evolving through the design process, provide an objective method to manage the solution, to keep it running and on track. However, these same measures and the discipline of iteratively reviewing them, will also help determine when the circumstances have changed, when the assumptions of the original design are no longer true or accurate, and it's time to revisit the design and evolve the system.

> This module focuses on the behavioral part of your design.

<img src="../Images/Design_Behavior_Design_Operations.png"
     alt="Design_Behavior_Design_Operations.png"
     style="float: left; margin-right: 10px;" />




### Learning Objectives

* Implement processes that minimize downtime, such as monitoring and alarming, unit and integration testing, production resilience testing, and incident post-mortem analysis.
* Launch a cloud service from a collection of templates.
* Configure basic black box monitoring of an application.
* Create an uptime check to recognize a loss of service.
* Establish an alerting policy to trigger incident response procedures.
* Create and configure a dashboard with dynamically update charts.
* Test the monitoring and alerting regimen by applying a load to the service.
* Test the monitoring and alerting regimen by simulating a service outage.

### Deployment

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/5imlo/deployment-monitoring-and-alerting-and-incident-response-deployment)

In this lesson, you'll learn some tips about how to deploy your solution. The advice seems like common sense. Make:

* a checklist,
* automate processes,
* use an infrastructure orchestration framework.

But don't underestimate the importance of these activities. They're at the core of deploying a stable solution.

1. Plan your checklist of dependencies for deployment

<img src="../Images/Design_Behavior_Deployment_Plan.png"
     alt="Design_Behavior_Deployment_Plan.png"
     style="float: left; margin-right: 10px;" />

2. Launch automation with resilience in mind

<img src="../Images/Design_Behavior_Deployment_Implement_Automation.png"
     alt="Design_Behavior_Deployment_Implement_Automation.png"
     style="float: left; margin-right: 10px;" />

Tool of choice: **Deployment Manager**

* configuration
* Resources
* Templates

<img src="../Images/Design_Behavior_Deployment_Implement_Automation_Tool.png"
     alt="Design_Behavior_Deployment_Implement_Automation_Tool.png"
     style="float: left; margin-right: 10px;" />

### Monitoring & Alerting

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/WBkXI/deployment-monitoring-and-alerting-and-incident-response-monitoring-and-alerting)


This lesson covers:

* **monitoring and alerting** its concepts.
* It then **illustrates the application of these concepts with a stack driver service** including the kinds of monitoring that can be configured.
* **How to set up an alert and notification?**
* and **how to create a dashboard with charts to help visualize the running system?**

#### SRE pyramid: Monitoring is measuring

(https://landing.google.com/sre/books/)

<img src="../Images/Design_Behavior_Monitoring_Is_Measuring.png"
     alt="Design_Behavior_Monitoring_Is_Measuring.png"
     style="float: left; margin-right: 10px;" />

#### Push-based and Pull-based metrics

<img src="../Images/Design_Behavior_Monitoring_Push_Based_Pull_Based_Metrics.png"
     alt="Design_Behavior_Monitoring_Push_Based_Pull_Based_Metrics.png"
     style="float: left; margin-right: 10px;" />

#### Black box monitoring (affecting user experience)

<img src="../Images/Design_Behavior_Monitoring_Blackbox.png"
     alt="Design_Behavior_Monitoring_Blackbox.png"
     style="float: left; margin-right: 10px;" />

#### White box monitoring (monitoring services)

<img src="../Images/Design_Behavior_Monitoring_Whitebox.png"
     alt="Design_Behavior_Monitoring_Whitebox.png"
     style="float: left; margin-right: 10px;" />

#### Carefully output of monitoring systems: alerts, 

* **Alerts**: a human must take action immediately
* **Tickets**: a human must take action, but the situation isn't yet urgent
* **Logging**: diagnostic information only

<img src="../Images/Design_Behavior_Monitoring_Output_Carefuly.png"
     alt="Design_Behavior_Monitoring_Output_Carefuly.png"
     style="float: left; margin-right: 10px;" />

#### 12-factor administration & operation in GCP: Stack driver

<img src="../Images/Design_Behavior_Monitoring_on_GCP.png"
     alt="Design_Behavior_Monitoring_on_GCP.png"
     style="float: left; margin-right: 10px;" />

<img src="../Images/Design_Behavior_Stackdriver_Unified_Tool.png"
     alt="Design_Behavior_Stackdriver_Unified_Tool.png"
     style="float: left; margin-right: 10px;" />

<img src="../Images/Design_Behavior_Stackdriver_Built_for_AWS.png"
     alt="Design_Behavior_Stackdriver_Built_for_AWS.png"
     style="float: left; margin-right: 10px;" />

> Specify a specific Stackdriver account to make use of its services!

#### Some features of Stack driver

**Uptime (health) check details**

<img src="../Images/Design_Behavior_Stackdriver_Uptime_Check.png"
     alt="Design_Behavior_Stackdriver_Uptime_Check.png"
     style="float: left; margin-right: 10px;" />

**Create alerts** (conditions, notifications, documentation)

<img src="../Images/Design_Behavior_Stackdriver_Create_Alerts.png"
     alt="Design_Behavior_Stackdriver_Create_Alerts.png"
     style="float: left; margin-right: 10px;" />

**Dashboards**

<img src="../Images/Design_Behavior_Stackdriver_Create_Dashboards.png"
     alt="Design_Behavior_Stackdriver_Create_Dashboards.png"
     style="float: left; margin-right: 10px;" />

<img src="../Images/Design_Behavior_Stackdriver_Create_Dashboards_Dynamic.png"
     alt="Design_Behavior_Stackdriver_Create_Dashboards_Dynamic.png"
     style="float: left; margin-right: 10px;" />

**Logging Agents** can be installed to capture all types of logs from other tiers too.

<img src="../Images/Design_Behavior_Stackdriver_Install_Log_Agents_Also_for_Tiers_Products.png"
     alt="Design_Behavior_Stackdriver_Install_Log_Agents_Also_for_Tiers_Products.png"
     style="float: left; margin-right: 10px;" />


### Incident Response

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/7BevS/deployment-monitoring-and-alerting-and-incident-response-incident-response)

Incident response is **the human behavior that results in system stability when things don't go as planned**.

The Site Reliability Engineering or SRE model is introduced (see [references section](#resourcesarticles)). You've actually been learning best practices throughout this course that relate directly to the layers of the SRE model. By developing your design with reliability in mind you've established processes for operating, maintaining, and recovering system in the event that things start to go sideways. In this lesson you'll review the items that were discussed in detail earlier in the class to prepare for successful incident response. Now, we'll discuss a few final steps such as developing playbooks to implement the response strategy.


<img src="../Images/Incident_Response_User_Trust.png"
     alt="Incident_Response_User_Trust.png"
     style="float: left; margin-right: 10px;" />

#### Structured incident response

<img src="../Images/Incident_Response_Structure.png"
     alt="Incident_Response_Structure.png"
     style="float: left; margin-right: 10px;" />
     
It includes:

* Monitoring dashboards
* Alterting regimen
* Plans & Tools for responding to issues

<img src="../Images/Incident_Response_Structure_Details.png"
     alt="Incident_Response_Structure_Details.png"
     style="float: left; margin-right: 10px;" />

<img src="../Images/Incident_Response_Structure_SRE_Pyramid.png"
     alt="Incident_Response_Structure_SRE_Pyramid.png"
     style="float: left; margin-right: 10px;" />

#### List of SRE processes

<img src="../Images/List_Processes.png"
     alt="List_Processes.png"
     style="float: left; margin-right: 10px;" />


#### 12-factor guidelines on administration and management tasks 

<img src="../Images/Incident_Response_12_Factor_Admin_Mgmt_Guidelines.png"
     alt="Incident_Response_12_Factor_Admin_Mgmt_Guidelines.png"
     style="float: left; margin-right: 10px;" />

#### Build a playbook based on alerts

<img src="../Images/Incident_Response_Alerts_and_Processes.png"
     alt="Incident_Response_Alerts_and_Processes.png"
     style="float: left; margin-right: 10px;" />

##### Create "easy buttons" for quick fix

<img src="../Images/Incident_Response_Use_Microservices_and_APIs.png"
     alt="Incident_Response_Use_Microservices_and_APIs.png"
     style="float: left; margin-right: 10px;" />

##### Balance interrupt-driven work and Incident Response

Controlled burns vs Fire fighters again.

<img src="../Images/Incident_Response_Balance_Project_Driven_Incident_Response.png"
     alt="Incident_Response_Balance_Project_Driven_Incident_Response.png"
     style="float: left; margin-right: 10px;" />

This module covered deploying, operating, and maintaining your design. Much of the groundwork needed for successful deployment monitoring and incident response was established earlier in the course in the context of the design process. This module points out how to integrate all those elements together to promote the behaviors that will lead to a stable service.

### Application: Stabilization & Operation

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/0c9fQ/stabilization-and-operation)

The photo service has evolved into a sophisticated scalable, reliable, secure system.

The goal is to **stabilize the system** and **make it maintainable** and **operable**.

* What elements of the service should be monitored?
* What kinds of alerts and notifications would you set up?

<img src="../Images/Application_Photo_Service_Stabilization_Operation.png"
     alt="Application_Photo_Service_Stabilization_Operation.png"
     style="float: left; margin-right: 10px;" />

<img src="../Images/Application_Photo_Service_Current_Architecture.png"
     alt="Application_Photo_Service_Current_Architecture.png"
     style="float: left; margin-right: 10px;" />
     
<img src="../Images/Application_Photo_Service_What_to_Monitor.png"
     alt="Application_Photo_Service_What_to_Monitor.png"
     style="float: left; margin-right: 10px;" />


### Design Challenge #7: Monitoring & Alerting

[video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/9SKko/design-challenge-7-monitoring-and-alerting)


Monitoring & Alerting are **what you add to the log system to stabilize a solution**.


<img src="../Images/Application_Photo_Service_Logging_Current_Architecture.png"
     alt="Application_Photo_Service_Logging_Current_Architecture.png"
     style="float: left; margin-right: 10px;" />

#### Business Challenge

What you think might be important to monitor, and under what conditions alerts and notifications should be sent?

<img src="../Images/Application_Photo_Service_Logging_Business_Challenge.png"
     alt="Application_Photo_Service_Logging_Business_Challenge.png"
     style="float: left; margin-right: 10px;" />

So in this case, make a list of three monitoring and alerting items that you would put into place to support stabilizing this particular solution. 

#### What monitoring and alerting to set up for the logs
     
<img src="../Images/Application_Photo_Service_Logging_Solution_Logs_Watchdogs.png"
     alt="Application_Photo_Service_Logging_Solution_Logs_Watchdogs.png"
     style="float: left; margin-right: 10px;" />

- monitor that latest data in BigTable isn't older than few minutes > **white box**
- monitor the queue mechanisms in Pub/Sub for all 3 feeds (web, app, data) > **black box**
- monitor network latency, network uptime for all 3 feeds > **black box**

#### Google's reference architectures online

<img src="../Images/Application_Photo_Service_Logging_Solution_Logs_Google_Tutorials.png"
     alt="Application_Photo_Service_Logging_Solution_Logs_Google_Tutorials.png"
     style="float: left; margin-right: 10px;" />


### Lab: Deployment Manager - Full Production

- [video](https://www.coursera.org/learn/cloud-infrastructure-design-process/lecture/UERcn/deployment-manager-full-production)
- [lab notes](../Labs/Lab_Deployment_Manager_Full_Production.md)

In this final lab, you'll clone a public repository of deployment manager templates. The public repo is a library of templates that are provided for a variety of purposes.

They provide a flexible base of templates that you can build on to create your own deployment solutions. There are several tutorials available in the online documentation that use the templates in the repo. This lab is based on one of the advanced tutorials. It employs many of the **best practices** and **design principles** you've learned in this class. It creates a scalable, resilient full production service **around a simple logbook application**.

The lab goes beyond the tutorial, by adding monitoring and testing. You'll use stac driver to configure monitoring, alert notifications and to set up graphical dashboards.

- You'll use [Apache Bench](https://httpd.apache.org/docs/2.4/programs/ab.html) to generate load traffic to test the system and trigger auto scaling. 
- You'll also simulate a service outage to test notifications and resiliency features.

During the previous labs in this course you learned a lot about the basic use of deployment manager. In this final lab, you'll clone a public repo of example Deployment Manager templates that you can use as reference for developing advanced deployments. The previous labs all used YAML templates and Jinja2 templates. This final lab uses Python templates. You'll deploy a full production application that implements many of the principles that were discussed and applied during the class.

<img src="../Images/Lab_Full_Production.png"
     alt="Lab_Full_Production.png"
     style="float: left; margin-right: 10px;" />

<img src="../Images/Lab_Full_Production_Architecture.png"
     alt="Lab_Full_Production_Architecture.png"
     style="float: left; margin-right: 10px;" />



## Resources/Articles

- [**Perfkit Benchmarker**](https://github.com/GoogleCloudPlatform/PerfKitBenchmarker): PerfKit Benchmarker is an open effort to define a canonical set of benchmarks to measure and compare cloud offerings.
- **Price calculator**: [cloud.google.com/products/calculator/](https://cloud.google.com/products/calculator/)
- Google's **Site Reliability Engineering**: https://landing.google.com/sre/books/
- Google's **The Site Reliability Workbook**: https://landing.google.com/sre/books/
- [**GCP podcast about Pokémon GO**](https://www.gcppodcast.com/post/episode-57-pokemon-go-with-edward-wu/) with Edward Wu, Director of Software Engineering at Niantic
- GCP solutions: https://cloud.google.com/solutions
- **GCP tutorials & solutions**: https://cloud.google.com/docs/tutorials
- [Apache Bench](https://httpd.apache.org/docs/2.4/programs/ab.html) to generate load traffic to test the system and trigger auto scaling. 
-  Step-by-step tutorials: https://cloud.google.com/deployment-manager/docs/tutorials