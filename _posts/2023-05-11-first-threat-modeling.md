---
title: what I just learned about threat modeling
date: 2023-05-11 20:10:00 +/-TTTT
categories: [notes]
tags: [threat modeling, architecture, appsec]     # TAG names should always be lowercase
---

### TLDR:

I just led my first threat modeling exercise! I sat in on a few of them in my previous role where skilled security architects drove; however, I'd never quarterbacked one myself. 

### The team:
It was just 3 of us -- the CTO (my old bandmate during college who wrote most of our codebase), our DevSecOps guy (puts the team on his back), and me (actively fighting impostor syndrome trying to convince myself I was qualified to be in the room). 

### The process:

We kicked off the session by discussing what our new API was going to do at a high level. We then identified the business problems we intended on solving for our customers. We followed the vision of the CTO here and were able to align on our goals quickly. 

We proceeded to step through an architectural diagram I'd prepared that outlined what I expected would be our data flow. I made a lot of assumptions but tried to have all the assets and controls laid out in [Threat Dragon](https://github.com/OWASP/threat-dragon) (an awesome open source tool by [OWASP](https://owasp.org/)). Here's an unrelated example if you haven't seen data flow diagrams:

![Threat dragon UI](/assets/img/threat_dragon.png)

I made some design mistakes like assuming that our API response would route to the customer directly through our cloud edge. Instead, it will be passing through a CDN -- which, in hindsight, is another chance to add more controls! Hearing that we wanted to route through the CDN made me recall a support ticket from early in my career where developers reached out to my team trying to shave down the latency incurred by our proxies. Even though we wanted to help, we ended up not making an exception for their team to bypass the normal egress route / other parts of our security stack just to shave off 100ms on their outbound call. I wonder whether whether any future customers of this API I'm about to build will have a similar request that we shave off latency by removing hops on the network route. 

I then explained STRIDE and why it's a useful methodology for enumerating threats. If you are new to this acronym, here's each threat along with the security property it violates:
- **Spoofing**: <i>Authentication</i><br>
- **Tampering**: <i>Integrity</i><br>
- **Repudiation**: <i>Non-repudiation</i><br>
- **Information disclosure**: <i>Confidentiality</i><br>
- **Denial of service**: <i>Availability</i><br>
- **Elevation of privilege**: <i>Authorization</i><br><br>

We then proceeded to step through the data flow, talking through each relevant threat per node of the graph. Each time we identified a threat, we'd do the due diligence of jotting down a note of the mitigation. Threat Dragon was really useful here because of how it lets you visually identify whether you have any unmitigated threats. If you add a new threat such as a bad actor possibly spoofing a customer request, it turns the component red. When you then apply a mitigation, you can add notes like "Using [Oauth 2.0 client credentials grant](https://oauth.net/2/grant-types/client-credentials/)" and watch as the component's color reverts back to the default of black.


After enumerating the mitigations per threat, we started to jot down a list of the threats that remained unmitigated and the controls we felt were crucial to verify. In order to speed up the prioritization, I tried to prepare for the session by reading through our startup's previous security reviews that outlined business-level risks. Since we create technology for the healthcare industry, the output related to threats like information disclosure immediately went to the top of the list. Our DevOps lead also wanted to tackle some low hanging fruit that he realized he could harden that wasn't directly related to the data ingress for our new API but would increase the security posture in our cloud tenant. 

### Lessons learned:
- A small startup may not have the resources to add in every single "nice-to-have" control. Focus on risk-based security. What really matters? 
- It is fun but challenging to predict the architecture of what you are about to develop. Don't waste too much time here since you may make mistakes like I did.
- I could've focused more on threat actors.
  - Next time, I'll try to better understand the incentives of a threat actor in the healthcare domain


### In summary:

We started small and kept it simple for this first exercise, only outputting 2 things:
- A threat dragon diagram showing the flow with all assets and controls
- A prioritized list of mitigations for us to implement or verify

