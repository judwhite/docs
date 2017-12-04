Ch 1 - High Performing Teams Continously Deliver Business Value

- Aligned Autonomy: A shared understanding of the company's strategic context and key objectives.
- Autonomous Teams:
  - Develop rich understanding of user needs through continuous discovery.
  - Aligned around a business capability.
  - Have the autonomy to:
    - Make business and product decisions (involves more than engineering, more below).
    - Employ technical practices that enable continuous delivery.
    - Organize technical and team boundaries around business outcomes.
  - For work too large for one team, use Composite User Journies
- Teams of specialists lead to high handover costs; work tends to be done in large batches.
- Lack of ownership leads to blame culture.
- You cannot "buy" agility.
- Continuous Delivery: Robust montoring and metrics needed. No time for manual regression. Deep engineering investment.
- Continuous Delivery is not enough. Dependencies between teams will be bottlenecks.
- Find things which change together for business reasons.
- Understanding the value of work leads to emotional investment in success.
- Vertically sliced product teams may not be enough. If there's a dependency to delivery value, not autonomous.
- Knowledge workers desire: Mastery, Autonomy, Purpose (Dan Pink).

Ch 2 - Communicating the Business Context

- Disseminate key business knowledge throughout your organization.
- OKRs - Objectives + Key Results
  - Anyone can see the current and previous goals of anyone else; a new grad could see the OKRs of the CEO
  - Spotify: Prioritized list of business initiatives with link s to further information including teams contributing to them.
  - Cascading Objectives
    - Leadership convincingly and repeatedly share their vision while empowering teams to self organize and make it reality.
  - Objectives are ambitious. Should feel somewhat uncomfortable.
  - Key Results are measurable. Google uses a scale of 0-1.0.
  - Not synonymous with employee evaluation. OKRs are about the company's goals and how each employee contributes.
  - Make sure the business isn't a black box.
  - Example:
    - Objective: Develop products faster than our competitors.
    - Key Results:
      - Each team deploys to PROD 3x/week.
      - Cycle time less than 2 days for 90% of work items.
      - All engineers and testers watch at least 1 user research session per month.
- In lieu of a clear sense of purpose:
  - Engineers will focus on new technologies.
  - Product Managers will copy other products.
  - Line Managers will focus on 100% utilization.
  - ... what they should all be doing instead is maximizing business value.
- Mission: The impact the company wishes to make on the world.
  - Large organization: Each BU may have its own Mission.
- Vision: Where it'll be in 2-5 years. Vision driven by Mission.
- Business Strategy: High level plan for achieving the mission.
  - Can include exploration in new markets, cost cutting, gaining market share from competitors.
  - "SWOT" - Strengths, Weaknesses, Opportunities, Threats.
  - Situational Awareness. [Wardley Maps / Value Chain Maps](https://www.cio.co.uk/it-strategy/introduction-wardley-value-chain-mapping-3604565/).
- Product Strategy: Specific product and service offering needed to achieve the Business Vision.
  - What user needs do our products not solve well?
  - What does success look like and how do we measure it?
  - What are the biggest unknowns and risks in the business strategy that need to be explored?
- Organization Design
  - Organization Design is not just organization chart/structure.
  - Structure at different levels, ranging from org-wide to team makeup. Covered in more detail later.
- Technical Strategy: Deliverables that implement hte product strategy.
  - Not necessarily the latest IT trends.
  - Show off success. Other teams will want to copy what worked for you.
  - Devs need to understand the Product Strategy and business goals.
    - If so, they'll be compelled to apply effort to greatest business value.
    - If not, and just handed a backlog, they'll seek exciting technical challenges as an alternative means to gaining fulfillment from their work.
- Market Research = Finding opportunities / size of market
- Design Research = Exploiting opportunities, finding a usable solution, building  auseful product
- Business Model Canvas: Teach employees how to focus on the most important parts of the business.
- Product Strategy Canvas: Business outcome, not a list of features.
  - Move from our current state to ideal without a fixed rigid plan.

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
- "Jobs-to-be-Done methods are based on a simple but powerful concept: your customers are not buying your product, they are hiring it to
get a job done."
- https://www.youtube.com/watch?v=f2l75aAJo44 - Bob Moesta - Understanding the Jobs to be Done (8m13s)
- https://www.youtube.com/watch?v=GLDqs099mXE - Bob Moesta - The Causality of Consumer Behavior - (9m54s)
- https://www.youtube.com/watch?v=VNTW_9mFM7k - Sian Townsend - Jobs to be Done: from Doubter to Believer (36m46s)
- https://www.youtube.com/watch?v=G3ujSdu8mYQ - Tony Ulwick (Strategyn) - Turn Jobs-to-be-Done Theory Into Practice (57m28s)
  - Key Takeaways:
    - Satisfaction and Importance scores
    - Interview people who made the switch, find out their decision making process. People who haven't will make it up.
- Intercom on Jobs-to-be-Done - http://jud.io/papers/jtbd/Intercom_on_Jobs-to-be-Done.pdf
- JTBD Cheatsheet - http://jud.io/papers/jtbd/JTBD_Cheatsheet.pdf
- JTBD Examples - http://jud.io/papers/jtbd/thrv-White-Paper-with-JTBD-Examples.pdf
- https://strategyn.com/jobs-to-be-done/jobs-to-be-done-theory/
- https://strategyn.com/customer-centered-innovation-map/
- https://strategyn.com/jobs-to-be-done/jobs-to-be-done-examples/
- https://jtbd.info/replacing-the-user-story-with-the-job-story-af7cdee10c27
- https://medium.com/@marksweep/jobs-to-be-done-interview-script-d290c42b2b48
- https://jtbd.info/a-script-to-kickstart-your-jobs-to-be-done-interviews-2768164761d7
- JTBD/Analytics Cards: http://digitaljobstobedone.com/shop-usa/
