# [Title, outcome-flavored: "Investigating an Identity Attack in Entra ID"]

## Scenario
A junior intern at Mad Hat Labs was granted temporary Contributor access to deploy a test environment in Azure without fully understanding the organization's governance policies. After the deployment was left unattended over the weekend, I was tasked with investigating the resources, identifying potential governance violations, and documenting evidence using read-only access to the Azure environment.
## Environment

Platform: Azure

Services: Resource Groups (RG), Azure Subscriptions, Azure Resources

Access Level: Read-Only

Tools: Azure Portal, Azure Resource Manager (ARM), Azure Policy


## Investigation

1. I looked through the companies resource groups and found a RG-Subscription that wasn't the normal naming convention. As it looked suspicious I investigated further into it. <img width="1026" height="593" alt="Screenshot 2026-10-18 115057" src="https://github.com/user-attachments/assets/50335f0e-e7ed-43cc-a454-bf0389f5347a" />

2. I investigated the resource and looked at the Tags to see who created it and found that the owner of the Resource Group is the intern.
<img width="1076" height="352" alt="Screenshot 2026-10-18 120135" src="https://github.com/user-attachments/assets/6d07960e-795f-4b94-967e-beb3bb08ed17" />

3. Next I want to know what deployment created the resource so I can establish a timeline and find who specifically inititated the deployment.
 <img width="868" height="297" alt="Screenshot 2026-10-18 123758" src="https://github.com/user-attachments/assets/b25aecc1-ce40-40d7-9f25-fe95eca19d6d" />

Within the deployments I confirmed that it was deployed by the Intern

4. I wanted to figure out why the policy assignment that's configured to enforce naming conventions didn't stop this resource from being created as it exposes a security risk and flaw as it shows anyone is able to create a resource.

5. Navigating to Policy I can see that Naming Convention which enforces naming is in a state of Non-Compliant
  <img width="1223" height="416" alt="Screenshot 2026-10-18 124440" src="https://github.com/user-attachments/assets/b832ddd3-7edb-4bd2-9ac0-71e2e17efe32" />

6.  Next I went into Assignments then into the Naming Convention where I see that the Parameter value is set as "Audit" which will still allows requests through but is logged as violations and doesn't block the deployment.
  <img width="995" height="407" alt="Screenshot 2026-10-18 124945" src="https://github.com/user-attachments/assets/71419283-e796-420d-8add-01170283f308" />


## What broke / what surprised me

I was surprised that the naming convention policy was set to Audit and why this wasn't checked before previously. 

## Findings and recommendations

During the investigation it was determined that there was a single resource group that didn't follow the naming convention that was created. It was found that the Naming Convention Policy was set to Audit. I would consider changing it from Audit to Deny so that the policy is set and that resources can't be created without the proper naming convention. A deployment process review should be established to know who is deploying what and when.
## What I learned
I learned how Azure Policy effects, such as Audit and Deny, work and how they affect resource compliance and deployments.

I gained hands-on experience investigating Azure resource groups, reviewing deployment history, and examining policy assignments to identify potential governance violations.

In future investigations, I would review governance policies and their enforcement settings earlier to better understand which security controls are in place and whether they're being enforced.
