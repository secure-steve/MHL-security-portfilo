# OPERATION DEAD DEPLOY 

## Scenario
The scenario: an intern with temporary Contributor access deployed a "test environment" over a weekend, cut every corner, and left. You come in Monday as the on-call engineer with Reader access and have to reconstruct what happened and why governance did not stop it.

## Environment
Live multi-user Azure training tenant, Reader access.

## Investigation
#### The core. Numbered steps IN YOUR OWN WORDS: what you looked at, what you found, what you concluded at each step. 6 to 12 screenshots of meaningful moments (portal views, query results, before/after).  
Step 01. Identify anything that could indicate it is the interns "test environment." A resource must belong to a resource group, so the first place I checked was resource groups. I found the offender. A resource group that doesn't match the standard naming convention. Microsoft's official recommended naming convention recommends all resource groups start with "rg" prefix.  
screenshot01-01  

Step 02. Inside the resource group is a single resource. I opened the resource and inspected it's tags. Tags should tell the story of the resource, who created it and why. To the interns partial credit, they did add some tags. I was able to identify the intern tag and owner tag that the intern was instructed to use when deploying test resources.  
screenshot01-02  

Step 03. To get an exact timeline for my investigation, I wanted to check the deployments in this resource group. This is where I can find more details about the resource deployment, including the timestamp. Correlate this deployment timestamp with the alert, and I found the beginning of the incident.
screenshot01-03

Step 04. Why was the intern able to do this? We use policies to enforce the naming convention. I had to verify the policy. Upon checking the Naming Convention policy, I found the effect parameter was set to audit. The audit effect will allow a deployment to happen, but log it as a violation. This policy needs to have the effect set to deny, this will block the resource deployment from ever happening. This is the finding that matters for the investigation when I get asked "How do we prevent this from happening again in the future?"  
screenshot01-04

## What broke / what surprised me
#### The most credible section in the document. Dead ends, wrong guesses, the thing that took an hour. Employers know real work is messy. This section separates you from certificate collectors.


## Findings and recommendations
#### What you determined, plus 2 or 3 recommendations as if you were reporting to the resource owner.


## What I learned
#### 3 to 5 bullets. At least one technical, one "what I'd do differently."
