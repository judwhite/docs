From memory:

- OKRs: Objectives and Key Results.
  - OKRs should be directly tied to business value.
  - Key Results measurable.
  - Everyone's OKRs, past and present, are visible to everyone else.
  - Measure not directly tied to employee performance.
- Cascade down: Business Mission -> Org Vision -> Product Vision -> Engineering. Can influence in both directions. Engineering should be aware of the visions and OKRs up to the Business Mission.
- Everyone should understand how they're contributing to business value and the end customer; otherwise they can fall into role-specific traps to achieve job satisfaction.
- Ubiquitous/Common Language between Business and Engineering.
- The changes within a Bounded Context are higher than between them.
  - Greenfield: Use User Journeys.
  - Brownfield: Look at the data flow.
- Team is responsible for end-to-end delivery, from the Customer to deployment. The Team is not all engineers, but they have full autonomy end-to-end.
- A User Journey may be too big for one team. Remember the rate of change rule. A Context team may help coordinate E2E (consistent UX, for example).
- CI/CD, monitoring, alterting absolutely necessary.
- Independent deployments.
- Continous Discovery - user interviews, JTBD (Jobs-to-be-Done).
- On "micro" services: use the Bounded Contexts. The UX is the most difficult part. Think of how a product page on Amazon may be decomposed into separate teams/services (masthead, product details, reviews, recommendations)

More on JTBD:
- https://www.youtube.com/watch?v=f2l75aAJo44 - Bob Moesta - Understanding the Jobs to be Done (8m13s)
- https://www.youtube.com/watch?v=GLDqs099mXE - Bob Moesta - The Causality of Consumer Behavior - (9m54s)
- https://www.youtube.com/watch?v=VNTW_9mFM7k - Sian Townsend - Jobs to be Done: from Doubter to Believer (36m46s)
- Intercom on Jobs-to-be-Done - http://jud.io/papers/cicd/Intercom_on_Jobs-to-be-Done.pdf
- JTBD Cheatsheet - http://jud.io/papers/cicd/JTBD_Cheatsheet.pdf
- https://strategyn.com/jobs-to-be-done/jobs-to-be-done-theory/
- https://strategyn.com/customer-centered-innovation-map/
- https://strategyn.com/jobs-to-be-done/jobs-to-be-done-examples/
- https://jtbd.info/replacing-the-user-story-with-the-job-story-af7cdee10c27
- https://medium.com/@marksweep/jobs-to-be-done-interview-script-d290c42b2b48
- https://jtbd.info/a-script-to-kickstart-your-jobs-to-be-done-interviews-2768164761d7
