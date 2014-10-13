name: 12factor
class: center, middle

# 12 Factor App

### Building and releasing at the web-scale

.hidden[
With powerful tools and modern cloud platforms one can think on how to properly build web apps that suits these environments. The 12  Factor App is a set of best practices on how apps built for PaaS should be architected. At its core, apps need not to care where they are.
]

---

## Motivation

- Heroku PaaS with Cedar stack as way of building portable apps
- Easy to deploy and scalable, for any language
- IaaS => Infra, PaaS => Apps
- Docker, CoreOS, Dokku, Flynn, Deis, ...
- 12 Factor: Best practices on how PaaS apps should architected
- PaaS-friendly apps need to not care where they are

---

## Introduction

- **Declarative** formats for setup automation
- **Maximum portability** between execution environments
- **Deployment** on modern **cloud platforms**
- **Minimize divergence** between prod/dev
- Enables **continuous deployment**
- **Scale up** easily

---

## I. Codebase

One codebase tracked in revision control, many deploys

- One app per codebase
- Multiple apps sharing the same code is a violation

.center[ ![]( http://12factor.net/images/codebase-deploys.png ) ]

---

## II. Dependencies

Explicitly declare and isolate dependencies

- Never rely on the implicit existence of any system tools
- Supports reproducible builds
- Examples of tools: gem/bundle, pip/virtualenv, autoconf, ...

HIGH importance

---

## III. Config

Store configuration in the environment (NOT code)

- Config is anything that may vary between deploys:
  - Resource handles
  - Credentials
  - Canonical hostname for the deploy
- Strict separation of config from code
- Does not include internal application config (like Spring)

---

## IV. Backing services

Treat backing services as attached resources

- No distinction between local and third party services
- Allows great flexibility
- Loose coupling to the attached deploy
- Resources can be attached and detached to deploys at will, no code changes

.center.fixsize[ ![](http://12factor.net/images/attached-resources.png) ]

HIGH importance

---

## V. Build, release, run

Strictly separate build, release and run stages

- **Build** : Converts code repo into an executable bundle
- **Release** : Build with deploy's current config, ready for immediate execution
- **Run** : Launches a set of app's processes against a selected release
- Example tool: Capistrano

.hidden[
- Run stage -> simple
- Build -> more complex: errors are visible for the devs
]

.center[ ![](http://12factor.net/images/release.png) ]

Conceptual importance. The tools you use shape these processes.

---

## VI. Processes

Execute the app as one or more stateless processes

- The app is executed in the execution environment
- Stateless and share-nothing
- Session data should be stored with a time-expiration, e.g. *Memcached* or *Redis*
- Stateless means:
 - More robust
 - Easier to manage
 - Incurs fewer bugs
 - Scales better

.hidden[
- Easy to tear down and move to other server
]

HIGH importance

---

## VII. Port binding

Export services via port binding

- Exports HTTP as a service by binding to a port
- Routing layer to handle requests routing to a hostname
- Uses Webserver libraries such as Jetty for JVM or Thin for Ruby
- One app can become the backing service for another app

Importance: Medium

---

## VIII. Concurrency

Scale out via the process model

- Processes are a first class citizen
- Never daemonize or write PID files
- Relies on OS process manager (upstart, systemd, launchd, foreman, ...) to:
 - Manage output streams
 - Respond to crashed processes
 - Handle restarts and shutdowns

---

## IX. Disposability

Maximize robustness with fast startup and graceful shutdown

- .hidden[ App’s processes are disposable: ] Can be started or stopped at a moment’s notice
- Maximize robustness with fast startup and graceful shutdown
- Gracefully shut down when receiving a SIGTERM signal
- Processes should be robust against sudden death

.hidden[
- Crash-only software
]

.hidden[ Importance: Medium Depending on how often you are releasing new code (hopefully many times per day, if you can), and how much you have to scale your app traffic up and down on demand, you probably won’t have to worry about your startup/shutdown speed, but be sure to understand the implications for your app. ]

---

## X. Dev/prod parity

Keep development, staging, and production as similar as possible

- Designed for continuous deployment
- Keep the gap between development and production small:
 - Time gap
 - Personnel gap
 - Tools gap
- Resists to use different backing services between dev and prod

---

## XI. Logs

Treat logs as event streams

- Event stream is written to STDOUT
- Use log routers (such as Logplex and Fluent)

.hidden[
Note: writing to stdout seems very purist
]

---

## XII. Admin processes

Run admin/management tasks as one-off processes

- Run against a release: same code and config as any process run against that release
- Must ship with application code to avoid synchronization issues
- e.g. Database migration


.hidden[ Importance: HIGH Having console access to a production system is a critical administrative and debugging tool, and every major language/framework provides it. No excuses for sloppiness here ]

---

## More

- http://12factor.net/
- https://news.ycombinator.com/item?id=3267187
- http://www.clearlytech.com/2014/01/04/12-factor-apps-plain-english/
- http://www.coderanch.com/t/626165/java/java/factor-app-principles-apply-Java
- https://blog.appfog.com/docker-and-the-future-of-the-paas-layer/

---

class: center, middle

## Questions

