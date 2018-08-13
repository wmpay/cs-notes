# Architecting for Scale

# Part 1: Availability

## Chapter 1: What is Availability

- Reliability: ability of system to perform operations without mistakes
- Availability: ability of system to be operational when needed
- Causes of poor availability
	- resource exhaustion
	- unplanned load based changes
	- more moving parts (programs)
	- external dependancies
	- technical debt

## Chapter 2: Five focuses to improve availability

- assume external dependancies will fail
- things to focus on wrt availability:
	- build with failure in mind
		- error catching
		- retries
		- "circuit breaker design"
	- always think about scaling
		- ability to scale dbs
		- ability to add servers
		- use CDNs for static content
			- static content can usually be factored out of dynamic content
	- mitigate risk
		- application should be designed to work even when resources fail
		- maybe redirect to a related page instead of just a 404
		- give incentives to keep using application, like 10% off purchase
	- monitor availability
		- server health
		- configuration change
		- application performance
		- sythetic testing
			- examine how application is functioning from user's perspective
		- alerting
		- look for trends, and once you have identified them, look for outliers
	- respond to availability issues in a predictable and defined way
		- know how to respond to issues
			- running diagnostics
			- restart a daemon
			- rebooting server last resort
			- owners of service should be first to be alerted, then adjacent teams
			- these procedures should be prepared ahead of time

## Chapter 3: measuring availability

- site_availability_percentage = (total_seconds_in_period - seconds_system_down) / total_seconds_in_period
- "Nines" 2 nines - 99% available, 6 nines - 99.9999% available
- 3 Nines is considered acceptable
- Planned maintenance hurts availability

## Chapter 4: improving your availability

- Measure and track current availability
- Service tiers - labels for services that indicate how critical the service is
- Risk matrix - gain visibility into technical debt and associated risk in application
- Automate manual processes to remove risk - never perform a manual operation on a production system
- Using "repeatable tasks" (idempotent?)
	- test the task
	- tweak the task
	- have the task reviewed
	- put task in version control
	- apply task to related resources
	- all related resorces act consistently
		- if you make one off changes to resources like servers, the will "drift" and start to behave differently
	- implement repeatable tasks/ auditable tasks
- lock down production environments, noone should have access except through automated processes and procedures
- automate deploys, allow rollbacks to good states
- configuration management - change configuration in automated manner
	- puppet, chef
	- applies to all infrastructure components: switches, routers, network components, monitoring applications, systems
- automated change management process
	- document proposed change
	- review the change
	- test the change
	- deploy the change
	- examine results

# Part II: Risk Management

## Chapter 5: What is Risk Management
- risk management - where is there risk, which risks can be removed, and mitigating risks to reduce likelyhood and severity
- balance the cost of removing risk with the cost of the risk occuring
- risk matrix brainstorming
	- wisdom of developers
	- high-support areas
	- threat vectors or vulnerabilities
	- areas where the system is incomplete
	- poor performance areas
	- traffic spikes
	- concerns from business owners, support personel, users
	- technical debt
- in summary:
	- identify risk
	- remove worst offenders
	- mitigate risk
	- review regularly

## Chapter 6: Likelihood vs Serverity
- severity - cost if the risk happens
- likelihood - chance that it happens
- high severity/high likelihood risks are the worst

## Chapter 7: Risk Matrix
- every row is a risk
- columns:
	- risk ID
	- system
	- owner
	- risk description
	- date identified
	- likelihood
		- low/med/high
	- severity
		- low/med/high
	- mitigation plan
	- status
		- active/mitigated/inprogress/resolved
	- eta
	- monitoring
	- triggered plan
		- usually management level
	- comment
	- tracking ID
	- history
- one matrix per team is good for a large company, but different for each company

## Chapter 8: Risk Mitigation
- risk mitigation - knowing what to do when a problem occurs so you can reduce the impact
- recovery plan
	- actions to stop problem
	- actions to implement a workaround
	- messages to inform costumers
	- escalation processes to use
- disaster recovery plan - like a recovery plan but high severity, low impact, high visibility

## Chapter 9: Game Days
- game day - test failures and see how operators/engineers respond
- use staging/test environments
- make sure staging/test enviroment mimics production as closely as possible
- usually not possible because production environments are scaled to more servers, with more data, etc. not financially viable
- possible to test on production when users not using application

## Chapter 10: Building systems with reduced risk
- redundancy
	- design application so that it can safely run on multiple independeny hardware components simultaneously
	- run tasks independently
	- run tasks asynchronously
	- localize state
	- utilize idempotent interfaces
	- running servers on a hypervisor is not really independent
- security
- simplicity
- self repair
	- load balancer that reroutes automatically
	- hot standby database that can be switched to
	- service that retries a request
	- queueing system that keeps track of pending work, can reschedule work if it fails
	- background system that introduces errors to check if system responds reliably (Netflix Chaos Monkey)
	- service that requests something from multiple independent services and checks if there is a majority response, and shuts down the wrong services
- operational processes
	- use documented repeatable processes
	- don't fat finger and rm -rf your server

# Part III: Services and Microservices

## Chapter 11: Why use services?
- each service has a clear owner
- service oriented architecture make it easy to split an application into domains
- service based application benefits:
	- scaling decisions are more granular
	- team assignment and focus
	- complexity localization
		- other developers don't need to understand your team's system
	- testing is easier
- services provide a contract:
	- capabilites of the service
		- what it does
		- how to call it and what each call means
	- responsiveness of the service (SLA)
		- how often can it be used
		- how fast will it respond
		- is it dependable

## Chapter 12: Using Microservices
- a service is a standalone component:
	- maintains own code base
	- manages own data
	- provides capabilites to others
	- consumes capabilities from others
	- single owner
- microservice and service are pretty much interchangable
	- companies split applications up into services of different sizes: few large services or many small services
	- no right or wrong way, but technologies like docker are making it easier to create many small services
- dividing into services (guidelines)
	- business requirements
	- team ownership
	- seperable data
	- shared capabilites/data
- when splitting things into services you are decreasing complexity of the components, but increasing the complexity of the whole application
- downsides
	- hard to see big picture
	- more failure opportunites
	- harder to change services
	- more dependancies

## Chapter 13: Dealing with service failures
- cascading failure - one services failure causes multiple due to dependancies
- responding to the failure
	- predictable
		- error message
	- understandable
		- agreed upon format (contract)
	- reasonable
		- indicate what actually happened
	- graceful degradation - service reduces amount of work it can do by as little as possible
	- graceful backoff - service fails but provides alternative
	- fail as early as possible
		- increased resource conservation
		- increased responsiveness
		- reduced error complexity

# Part IV: Scaling Applications

## Chapter 14: Two mistakes high
- Anecdote: keep your plane high enough that it can recover from two independant mistakes
- number_of_nodes_needed = ciel(number_of_requests/requests_per_node)
- If you only account for minumum number of nodes, and a node fails, you will overload your servers
- Conclusion: have more nodes than the minumum - take node failure into consideration when deciding number of nodes
- Add more nodes to calculation since they will need to be taken offline for upgrades
- Same goes for data centers: for example if you have half of your nodes in one datacenter and it goes offline
- nodes_per_data_center = ciel(minimum_number_of_servers/(number_of_data_centers-1))
- more data centers you have, the less nodes you need to provide data center redundancy
- hidden shared failure types - example all your servers are in the same rack
- failure loop - system fails in a way that makes it hard or impossible to fix the problem without making it worse
- fly two mistakes high in this context means don't just account for surface failures, also account for hidden failures
- if it touches production, it is production

## Chapter 15: Service Ownership
- STOSA - single team owned service architecture
	- application w/ service based architecture
	- multiple development teams that are responsible for building and maintaining the app
	- all services are assigned to a team
	- no service assigned to more than one team
	- teams may own more than one service
	- teams are responsible for all aspects of service, dev, test, deploy, monitor, incident resolution
	- strong boundaries between services, well documented APIs
	- maintain SLAs
- STOSA is 3-8 engineers
- To be a service owner:
	- API design
	- service deployment
	- manage data
	- deployment windows - when it is safe to deploy
	- production changes
	- environments
	- service SLAs
	- monitoring
	- oncall/incident response
	- reporting
- owned by infrastructure team:
	- hardware
	- tooling
	- databases

## Chapter 16: Service tiers
- service tier - a label associated with a service
	- tier 1 - most critical
		- login system
		- credit card processor
		- permission service
		- order accepting service
	- tier 2 - important
		- search service
		- order fulfillment service
	- tier 3 - minor
		- customer icon service
		- recommendations service
	- tier 4 - insignificant
		- sales report generator service
		- marketing email sending service

## Chapter 17: Using Service Tiers
- expectations
	- SLAs help manage this
- responsiveness
	- how quickly do you need to respond to a problem
	- how severe the problem is
- dependencies
	- if your service is a higher tier than one of it's dependencies, it is critical

## Chapter 18: Service level agreements
- agreements between service owners and consumers
- external vs internal SLAs
	- internal SLAs help to diagnose problems, e.g. a service is not responding as fast as it's SLA says it will
- performance measurements for SLAs
	- call latency
	- traffic volume
		- how many requests over a period of time
	- uptime
	- error rates
- top percentile SLA
	- percentage of data points above/below a specific value
	- e.g. TP90 is less than 20ms - 90% of all requests will take less than 20%
- latency groups
	- call latency TP90 < 25ms when traffic volume < 250k req/sec
- how many SLAs
	- keep number as low as possible
	- cover critical areas
	- negotiate with consumers
	- only specifiy SLAs you can monitor and alert on

## Chapter 19: Continous Improvement
- stateless services
	- manage no data and no state of their own
- localize data - services and data stores manage only the data they need to manage to perform their jobs
- localization benefits:
	- reduced size of individual datasets
	- localized access
		- reduce amount of unneeded data in your queries
	- optimized access methods
		 - optimize data store for each data set
- data partitioning (federation)
	- A-D costumers in one db, E-K in another
	- increased complexity
	- choosing partitioning key extremely important
	- repartitioning to balance traffic
	- best partition key is one that results in consistently sized partitions as much as possible

# Part V: Cloud Services

## Chapter 20: Change and the Cloud
- bla bla cloud is more popular now
- easier for smaller companies to build applications with third party services

## Chapter 21: Distributing the Cloud
- cloud distribution follows same principles as data center distribution
- AWS Region - large area of cloud resources representing a specific geographical area
	- composed of a single or multiple availability zones (AZs)
	- AZs describe and document network topological diversity of cloud resources
	- can assume different AZs = different data centers
	- region name: us-east-1, AZ name: us-east-1a
- run an applications in the same region, AWS provides dedicated links within the region
- run seperate instances of the app in different locations
- added benefit that you can support different gov't regulations
- a single AZ can be contained in multiple data centers
- deploying to different AZs does not guatantee redundancy

# Chapter 22: Managed Infrastructure
- three basic types of cloud based services
	- unmanaged/raw resource
	- managed resource server based
	- managed resource non server based
- network access control list (ACL) - firewall layer
- unmanaged/raw resource - ec2 - you have control over the OS
- managed resource server based - RDS - they expose the database, not the OS
- managed resource non server based - S3

# Chapter 23: Cloud Resource Allocation
- allocated capacity resources
	- EC2
	- allocated in discrete units
	- resource remain idle if you don't use them
	- proper capacity planning is important
- usage based resources
	- S3
	- charged for the amount of resource you consume
	- no allocation step involved

# Chapter 24: Scalable Computing Options
- cloud based servers
- compute slices
	- deploy to platform as a service (PaaS)
	- could run computations on multiple servers
- dynamic containers
- microcompute
	- AWS lambda
	- small piece of code ready to execute

# Chapter 25: AWS Lambda
- lambda can scale to almost any rational size
- use cases
	- image transformation
	- streaming data validation
	- real time metric data processing
- API Gateway - API creation service designed to work with AWS Lambda
- Amazon Kinesis real time streaming data designed to handle large data streams
- Good for scaling small scripts, bad at large scale application deployment