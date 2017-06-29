# Stable Systems Checklist
Below is an opinionated list of attributes and policies that need to be met in order to establish a stable software system.

## Preparation
- [ ] Developers are in control of the Software and they own the code.
- [ ] Only small units of work every time. Fix, deploy, Develop.
- [ ] Only few people in each project no more than 6.
- [ ] Every project has a win condition.
- [ ] Show feasability of the project by working on an initial seed no longer than 3-4 days.
- [ ] Gamble any new technology sensibly.
- [ ] Define scope of project. Make sure is not too broad.
- [ ] Define feature extensions of each project.
- [ ] Make experiments and flag them as analysis pre-planning work to the real project.
- [ ] Be on guard and look for bad APIS.
- [ ] Data should be clean and not garbage.

## Process & People
- [ ] Pick the parts of Agile/XP/SCRUM/Kanban that work for the team and kill the rest.
- [ ] Prefer asynchronous communication.
- [ ] Know how different people on your team likes to work with the code base.

## System Planning
- [ ] The system is built for production.
- [ ] You build your system as a 12 factor app.
    - [ ] Use revision control with many deploys.
    - [ ] Declare dependancies with package managers.
    - [ ] Store configuration in the environment.
    - [ ] Track backend services as resources.
    - [ ] Use seperate build, and run stages.
    - [ ] Execute the app as one or more stateless processes.
    - [ ] Export services via port binding.
    - [ ] Scale out via the process model. Never daemonize or write PID files. Use process managers.
    - [ ] Processes shut down gracefully when they receive a SIGTERM. They have a fast startup and graceful shutdown.
    - [ ] Keep development, staging, and production as similar as possible. Vagrant allow developers to run local environments.
    - [ ] Treat logs as event streams.
    - [ ] Run admin/management tasks as one-off processes. For example django manage.py commands.
- [ ] The system is a set of modules with loose coupling.
- [ ] Modules communicate loosely via a protocol.
- [ ] Design protocols for future extension. Design each module for independence. Design each module so it could be ripped out and placed in another system and still work.
- [ ] Avoid deep dependency hierarchies.
- [ ] Avoid intermediaries parsing and interpreting on data.
- [ ] Have a supervision/restart strategy.
- [ ] Prefer ratcheting methods via idempotence.
- [ ] Uses a unique ID on all messages which means you can always retry said message in case of a timeout and be sure it won’t be rerun by the receiving system, if the receiver keeps a log of what it has already done.
- [ ] UNIX principle: each tool does one thing well.
- [ ] Define the capacity of the system up front.

## Setup
- [ ] First you build an empty project.
- [ ] Add this empty project to continuous integration.
- [ ] Deploy the empty project into staging.
- [ ] Once this works, you start building the application.
- [ ] Preconfigure your systems so you need no external dependencies when deploying.
- [ ] The same artifact is deployed to staging and production. It picks up a context from the environment and this context configures it. 
- [ ] Don’t use advanced technology too early on.
- [ ] Lock dependencies to specific tags/versions. 
- [ ] Make upgrading dependencies a decision on your part.
- [ ] Vendor everything.
- [ ] Make a production deploy take less than 1 minute from button-push-to-operational-on-the-first-instance.
- [ ] Build a default library you include in every application you write.
- [ ] Let every application use the same library.

## Development
* Correctness is more important than fast.
* Elegant is more important than fast.
* Code Quality is more important than fast.
* Fast is not really important.

- [ ] Build your system to collect metrics about itself as it runs.
- [ ] Ship metrics to a central point for further analysis.
- [ ] Unit test, property based test, type systems, static analysis, and profiling.
- [ ] No vendor lock-in.
- [ ] No code formatting disputes.
- [ ] Use load regulation in the border of the system.
- [ ] Use circuit breakers to break cascading dependency failure.

## Picking a database
- [ ] Pick postgresql as default.If you need MongoDB-like functionality you create a jsonb column.
- [ ] Export to elasticsearch from postgres.
- [ ] Use pg_bouncer.
- [ ] Isolate complex transactional interactions to a few parts of the store.
- [ ] Look for idempotent ratcheting methods as an alternative.

## Picking a programming language
- [ ] Avoid the monoculture.
- [ ] Know the weaknesses of a language.
- [ ] The deployment tooling must be in place before use.
- [ ] Use make. Use the same make targets for all projects in the organization.

## Configuration
- [ ] Secure defaults.
- [ ] Persistent data lives outside of the artifact path, on a dedicated disk with dedicated quota.
- [ ] Log rotation.
- [ ] The artifact path is not writable by the application.
- [ ] Use different credentials in production and staging.
- [ ] Deny developers laptops easy access to the production environment.
- [ ] Avoid the temptation of too early etcd/Consul/chubby setups.

## Operations
- [ ] Optimize for sleep.The system must avoid waking people up in the middle of the night at all costs.
- [ ] The system must be able to gracefully degrade.
- [ ] The system runs out of monit, supervise, upstart, systemd, rcNG, SMF, or the like.
- [ ] The application must gracefully stop and start if given the command to do so.
- [ ] Every log file is shipped and indexed outside of the system. Every interesting metric too.
- [ ] The only way to make changes to a production host is to redeploy.
- [ ] Make it easy to roll back and downgrade a deployment.
- [ ] In a production system you must be able to query its state in an ad-hoc fashion.
- [ ] If you enable ad-hoc query and tracing on the system and then disable it again, there must no segfaults, no kernel crashes and no long-term impact.

## Tools
### Debugging
* Dtrace
* Gdb

### Cloud Storage
* [minio](https://minio.io/)

## Security
### Database
- [ ] Use encryption for sensitive data.
- [ ] All backups are stored encrypted as well.
- [ ] Use minimal privilege for the database access user account.
- [ ] Store and distribute secrets using a key store designed for the purpose.
- [ ] Don’t hard code in your applications.
- [ ] Only using SQL prepared statements.

### Development
- [ ] Use vulnerability scanners for every version pushed to production.

### Authentication
- [ ] Ensure all passwords are hashed using appropriate crypto such as bcrypt. Use secure random bytes.
- [ ] Apply password rules that encourage users to have long, random passwords.
- [ ] Use multi-factor authentication for your logins to all your service providers.

### Denial of Service Protection
- [ ] At a minimum, have rate limiters on your slower API paths and authentication related APIs like login and token generation routines.
- [ ] Use CAPTCHA in front end.
- [ ] Enforce sanity limits on the size and structure of user submitted data and requests.
- [ ] Use a global caching proxy service like CloudFlare.

### Web Traffic
- [ ] Use the strict-transport-security header to force HTTPS on all requests.
- [ ] Cookies must be httpOnly and secure and be scoped by path and domain.
- [ ] Use Content Security Policy without allowing unsafe-* backdoors.
- [ ] Use CSP Subresource Integrity for CDN content.
- [ ] Use X-Frame-Option, X-XSS-Protection headers in client responses.
- [ ] Use CSRF tokens in all forms.
- [ ] Use the new SameSite Cookie response header which fixes CSRF once and for all newer browsers.

### APIs
- [ ] No resources are enumerable in your public API.
- [ ] All users are fully authenticated and authorized appropriately when using your API.
- [ ] Use canary checks in APIs to detect illegal or abnormal requests that indicate attacks.

### Validation and Encoding
- [ ] Do client-side input validation.
- [ ] Escape text before showing.

### Cloud Configuration
- [ ] Ensure all services have minimum ports open.
- [ ] Host backend database and services on private VPCs that are not visible on any public network.
- [ ] Isolate logical services in separate VPCs and peer VPCs to provide inter-service communication.
- [ ] Ensure all services only accept data from a minimal set of IP addresses.
- [ ] Restrict outgoing IP and port traffic to minimize APTs and “botification”.
- [ ] No root credentials.
- [ ] Use minimal access privilege for all ops and developer staff.
- [ ] Regularly rotate passwords and access keys according to a schedule.

### Infrastructure
- [ ] Ensure you can do upgrades without downtime. Automated.
- [ ] Create all infrastructure using a tool such as Terraform, and not via the cloud console. Have zero tolerance for any resource created in the cloud by hand.
- [ ] Use centralized logging for all services. You should never need SSH to access or retrieve logs.
- [ ] Don’t SSH into services except for one-off diagnosis. Using SSH regularly, typically means you have not automated an important task.
- [ ]  Don’t keep port 22 open on any AWS service groups on a permanent basis. If you must use SSH, only use public key authentication and not passwords.
- [ ] Create immutable hosts instead of long-lived servers that you patch and upgrade.
- [ ] Protect infrastructure secrets with Centralized secret management tools like Vault or Keywhiz.

### Operation
- [ ] Power off unused services and servers.
- [ ] Have a practiced security incident plan.

### Test
- [ ] Do Penetration Testing.
- [ ] Do fuzz testing.
- [ ] Everything is Auditable.

### Security tools
#### Auditing
* [auditd](https://linux.die.net/man/8/auditd)
### Encryption
* [Keyczar](https://github.com/google/keyczar)

## References
* [12 Factor app](https://12factor.net/)
* [How to build stable systems](https://medium.com/@jlouis666/how-to-build-stable-systems-6fe9dcf32fc4)
* [Continious Code Quality](https://www.sonarsource.com/)
* [Distirbuted Systems Safety Research](http://jepsen.io/)
* [Site Reliability Book](https://landing.google.com/sre/book/index.html)
* [How Complex Systems Fail](http://web.mit.edu/2.75/resources/random/How%20Complex%20Systems%20Fail.pdf)
* [Web operations](https://www.safaribooksonline.com/library/view/web-operations/9781449377465/)
* [Continious Delivery](https://www.safaribooksonline.com/search/?query=Continuous%20Delivery&extended_publisher_data=true&highlight=true&is_academic_institution_account=false&source=user&include_assessments=false&include_case_studies=true&include_courses=true&include_orioles=true&include_playlists=true&formats=book&sort=relevance)
* [Release it](https://www.safaribooksonline.com/library/view/release-it/9781680500264/)
* [Web Developer Security Checklist](https://simplesecurity.sensedeep.com/web-developer-security-checklist-f2e4f43c9c56)
* [Making the Netflix API more Resilient](https://medium.com/netflix-techblog/making-the-netflix-api-more-resilient-a8ec62159c2d)


## License

[![CC0](http://mirrors.creativecommons.org/presskit/buttons/88x31/svg/cc-zero.svg)](http://creativecommons.org/publicdomain/zero/1.0)

To the extent possible under law, Theo Despoudis has waived all copyright and
related or neighboring rights to this work.
