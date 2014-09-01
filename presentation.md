class: center, middle

# Title

---

# Agenda

1. Introduction
2. Deep-dive
3. ...

---

# 12 Factor App

12 Factor Aps Manifesto: building and releasing at the web-scale

---

Scaling horizontally web apps. Can scale up without any major effort.
Minimize divergence of dev and prod environments. Maximum portability, enabling coninuous deployment.
Scale up easily.
Suitable for deployment on modern cloud platforms.
Understand the practices and when it does make sense to deviate.

---

## Motivation

- Docker/coreos ...
- PaaS

---

## I. Codebase

One codebase tracked in revision control, many deploys

- One app per codebase
- Multiple apps sharing the same code is a violation of twelve-factor

.center[ ![]( http://12factor.net/images/codebase-deploys.png ) ]

---

## II. Dependencies

Explicitly declare and isolate dependencies

- Never rely on the implicit existence of any system tools.
- Supports reproducible builds
- Examples of tools: gem/bundle, pip/virtualenv, autoconf, ...
- Also valid for system tools (like ImageMagick)
- If the app need to shell out to a system tool, it should be vendored into the app

HIGH importance

---

## III. Config

Store configuration in the environment (NOT code)

- Config is anything that may vary between deploys:
  - Resource handles
  - Credentials
  - Canonical hostname for the deploy
- Strict separation of config from code
- Config varies substantially across deploys, code does not
- This does not include internal application config (like Spring)
- Environment variables as config
- Never group config together as "environments"

Note: using version control might be convenient, like chef databags

Medium importance. Valuable with microservices and on scale

---

## IV. Backing services

Treat backing services as attached resources

- The code for a twelve-factor app makes no distinction between local and third party services
- Allows great flexibility
- Loose coupling to the deploy they are attached to
- Resources can be attached and detached to deploys at will, without code changes

.center[ ![](http://12factor.net/images/attached-resources.png) ]

HIGH importance

---

## V. Build, release, run

Strictly separate build, release and run stages

- **Build**: Converts code repo into an executable bundle
- **Release**: Build with deploy's current config, ready for immediate execution
- **Run**: Launches a set of app's processes against a selected release
- Example tool: Capistrano
- Run stage is simple, build might be more complex. Since errors are in the foreground for the dev driving the deploy.

.center[ ![](http://12factor.net/images/release.png) ]

Conceptual importance. The tools you use shape these processes.

---

## VI. Processes

Execute the app as one or more stateless processes

- The app is executed in the execution environment as one or more processes
- Stateless and share-nothing
- Sticky sessions is a violation
- Session data should be stored with a time-expiration, good candidates are Memcached or Redis
- Stateless means more robust, easier to manage, incurs fewer bugs and scales better


HIGH importance

---

## VII. Port binding

Export services via port binding

- The twelve-factor app is completely self-contained.
- Does not rely on runtime injection of a webserver into the execution environment to create a web-facing service.
- The web app exports HTTP as a service by binding to a port, and listening to requests coming in on that port.
- Also there is a routing layer to handle requests routing to a hostname to a port bound web process
- Webserver libraries such as Jetty for JVM or Thin for Ruby.
- This way one app can become the backing service for another app


Importance: Medium

---

## VIII. Concurrency

Scale out via the process model

- In the twelve-factor app, processes are a first class citizen.
- The share-nothing, horizontally partitionable nature of twelve-factor app processes means that adding more concurrency is a simple and reliable operation
- Twelve-factor app processes should never daemonize or write PID files
- Relies on OS process manager (Upstart, Systemd, Foreman, ...) to manage output streams, respond to crashed processes and handle restarts and shutdowns.


---

## IX. Disposability

Maximize robustness with fast startup and graceful shutdown

- The twelve-factor app’s processes are disposable, meaning they can be started or stopped at a moment’s notice.
- Strive to maximize robustness with fast startup and graceful shutdown
- Processes shut down gracefully when they receive a SIGTERM signal from the process manager
- Processes should also be robust against sudden death

Importance: Medium Depending on how often you are releasing new code (hopefully many times per day, if you can), and how much you have to scale your app traffic up and down on demand, you probably won’t have to worry about your startup/shutdown speed, but be sure to understand the implications for your app.

---

## X. Dev/prod parity

Keep development, staging, and production as similar as possible

- Designed for continuous deployment by keeping the gap between development and production small
- Resists the urge to use different backing services between development and production.

---

## XI. Logs

Treat logs as event streams

- Never concerns itself with routing or storage of its output stream
- It should not attempt to write to or manage logfiles 
- Each running process writes its event stream, unbuffered, to stdout
- Use log routers (such as Logplex and Fluent)

Note: writing to stdout seems very purist

---

## XII. Admin processes

Run admin/management tasks as one-off processes

- Run against a release: same code and config as any process run against that release.
- Must ship with application code to avoid synchronization issues.

Importance: HIGH Having console access to a production system is a critical administrative and debugging tool, and every major language/framework provides it. No excuses for sloppiness here

## More

http://12factor.net/
https://news.ycombinator.com/item?id=3267187
http://www.clearlytech.com/2014/01/04/12-factor-apps-plain-english/
http://www.coderanch.com/t/626165/java/java/factor-app-principles-apply-Java

## Summary

An organization should allow good practices around I-V
Devs should consider in their daily basis III, VI, VIII, IV and X

