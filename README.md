# Getdone performance audit

## Observations

Front end applications (Customer, Pro, and Invictus) have intermittent latency during usage.

Invictus: when navigating orders, changing views, and creating new jobs & orders

Pro: when uploading photos

Customer: when navigating a flow, viewing orders and jobs, and interacting with change order modals

### Technical investigation

- Tightly coupled monolithic code base: code base is HUGE for an app less than 2 years old. In my over a decade experience across 50+ code bases I've never seen so many 1000+ line files in such a young code base. It appears the decision to pursue a gRPC api over traditional RESTful architecture has led to some deeply deeply coupled front and backend components with no straightforward way to separate them without major breaking changes. Code refactoring is the process used by engineers to creating maintainable, testable, and measurable improvements over time. Because of the current code bases deeply and tightly coupled nature, refactoring without major chaos to critical systems would be very costly (both in terms of time and developer productivity). Auditing is also an important process for engineering leaders to perform regularly to ensure engineering teams adhere to best coding practices ie writing maintainable, compose-able, and scalable code. Given the codes current condition, this audit also incurred these same set backs (time and productivity). I was only able to scratch the surface during this time-boxed due diligence.

- Polling architecture: while utilizing the browser web developer tools to further investigate, it appears certain api calls are being made at consistent intervals. Due to the nature of the gRPC api architecture determining the exact nature of these requests is complex. Previous experience would suggest a polling architecture is being utilized to provide alerts for certain app events (ie change order, customer reminders, etc). This approach is problematic as these api calls increase exponentially with each active user, increasing backend overhead with each additional request.

## Assertions

- Reduce technical debt progressively

- Stop api calls that run at regular intervals

- Reduce api calls with frontend caching techniques

- Optimize naive sql queries to more efficiently utilize permanent data stores

- Add additional data layers to increase data efficiency (no sql data stores, in memory caching for frequently requested data)

## Next Steps

- Tightly coupled monolithic code base: recommend we utilize the Austin team in order to progressively work to decouple the tightly coupled backend architecture from the frontend by peeling off small pieces of functionality over time into maintainable greenfield applications. Simultaneously keeping the Allion engineers (familiar with the current monolith) to progress on features within the current architecture. We can then progressively move more resources from old to the new over time to prevent loss of app feature productivity for business objectives.

- Polling architecture: recommend re-architecting approach to real time events with a push based event system rather than a polling system to decrease exponential overhead of polling architecture.
