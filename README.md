# Jayesh251-Interview_Question_Bank_DevOps

# DevOps Interview Question Bank (2026)

Target: DevOps / Platform engineer roles at product companies, up to ~20 LPA.
Scope this month: Git, Linux (with networking), shell scripting, Docker, Python,
Kubernetes, GitHub Actions, Jenkins, AWS. Terraform, monitoring and system design
come next.

## How to use this file

Every question has a level tag:

- **[B] Basic** - asked to everyone, including freshers. Answer in under a minute,
  without hesitation. Failing one of these usually ends the interview.
- **[M] Mid (2-4 years)** - the core of your interviews. Explain *how it works*,
  not just *what it is*, and give an example from your own work.
- **[S] Senior (5-6 years)** - scenarios, design and trade-offs. Product companies
  ask a few of these even at 2-3 years to see your ceiling.

For each question:
1. Say the answer out loud first, without notes (record it on your phone).
2. Then check the docs or run it in a terminal/lab.
3. Mark it: solid / shaky / don't know. Revisit shaky ones every 3 days.

A rule for 2026 interviews: for any concept, be ready for "show me" - type the
command, write the YAML, or debug a broken setup live. Reading answers is not
enough; you need to have run things.

---

## What interviews look like in 2026

From reported interviews (sources at the end) and the current market:

- **Typical product-company loop (4-6 rounds):** recruiter screen -> technical
  screen (fundamentals, rapid questions) -> hands-on round (fix a broken
  pipeline/cluster, write a script, or build a small CI/CD pipeline) -> deep dive
  / design round (design infra for a service, multi-region, failure handling) ->
  hiring manager (ownership, incidents, behavioural) -> HR.
- **Scenario questions dominate.** "Pod is Pending - walk me through it", "site
  is slowing down progressively", "deployment raised error rates - what now and
  what do you change afterwards". Definitions alone are not enough.
- **Coding is common but easy.** Razorpay's senior DevOps loop (Oct 2025) had
  coding in every round, reported as easy-level, plus advanced networking. Expect
  a Python or Bash script, sometimes an easy array/string/hash-map problem.
- **Networking is the hidden filter.** DNS, TCP, TLS, subnets, load balancers,
  "request flow from domain to pod" come up in almost every reported loop.
- **Newer topics now expected:** GitOps (Argo CD), OIDC instead of long-lived
  cloud keys in CI, supply-chain security (image scanning, SBOM, signing),
  OpenTelemetry, cost optimisation, and how you use AI tools safely in your work.
- **Resume deep dive.** Every line on your resume will be questioned. Your MOOSE
  and Inspiron work is part of this bank (last section).

---

## 1. Git

- [B] Difference between `git fetch` and `git pull`.
- [B] Difference between `git merge` and `git rebase`. When would you not rebase?
- [B] What is a merge conflict and how do you resolve one?
- [B] `git reset --soft` vs `--mixed` vs `--hard`.
- [B] `git revert` vs `git reset`. Which do you use on a shared branch and why?
- [B] What does `git stash` do? How do you apply a specific stash?
- [B] What is `.gitignore`? A file is already tracked - why does adding it to `.gitignore` not work, and how do you fix it?
- [B] What is the difference between a branch and a tag? Lightweight vs annotated tags.
- [M] Explain `git cherry-pick` and a real case where you used it.
- [M] How do you undo a commit that has already been pushed?
- [M] What is a detached HEAD? How do you get into it and out of it safely?
- [M] What is `git reflog` and how does it save you after a bad reset?
- [M] Squash the last 4 commits into one. Two ways.
- [M] Explain branching strategies: GitFlow, GitHub Flow, trunk-based development. Which suits CI/CD and why?
- [M] How do you enforce code review and passing CI before merge? (branch protection, required checks, CODEOWNERS)
- [M] What is a fast-forward merge? What does `--no-ff` change?
- [M] How do Git hooks work? Client-side vs server-side. Give one useful pre-commit hook.
- [M] Someone committed an AWS secret to the repo and pushed. What exactly do you do? (rotate the key first, then history rewrite with `git filter-repo`/BFG, then prevention)
- [M] What is `git bisect` and how would you use it to find the commit that broke a build?
- [M] Monorepo vs polyrepo - trade-offs for CI.
- [S] How do you version and release software? Semantic versioning, release branches, tags triggering pipelines.
- [S] How would you design a branching and release process for 5 teams deploying daily?
- [S] What is inside `.git`? Explain objects (blob, tree, commit), refs and how a commit is stored.
- [S] Repo is 5 GB and clones are slow. Options? (shallow clone, partial clone, LFS, history cleanup)

## 2. Linux (with networking)

### Core
- [B] What happens when you boot a Linux machine? (BIOS/UEFI -> bootloader -> kernel -> init/systemd -> targets)
- [B] Explain file permissions `rwx`, numeric mode (755, 644), and `chmod`/`chown`.
- [B] What are SUID, SGID and the sticky bit? Where is the sticky bit used?
- [B] Hard link vs soft link. What happens to each when the original file is deleted?
- [B] What is an inode? What can make a disk "full" while `df -h` shows free space?
- [B] How do you check CPU, memory, disk and running processes? (`top`/`htop`, `free -h`, `df -h`, `du`, `ps aux`)
- [B] Difference between a process and a thread.
- [B] `kill` vs `kill -9`. What are SIGTERM, SIGKILL, SIGHUP?
- [B] How do you find which process is using port 8080? (`ss -tulpn`, `lsof -i :8080`)
- [B] How do you schedule a job? Explain cron syntax: run a script every day at 2:30 am.
- [B] How do you view the last 100 lines of a log and follow it live?
- [B] What is `/etc/passwd`, `/etc/shadow`, `/etc/fstab`, `/etc/hosts`?
- [M] What is a zombie process? An orphan process? How do you clean up zombies?
- [M] What is load average? A 4-core machine shows load 8 - is that bad?
- [M] Explain systemd: write a unit file for a Python app that restarts on failure. `systemctl` vs `journalctl` usage.
- [M] Disk is 100% full on a production server. Step by step. (find large files/dirs, deleted-but-open files via `lsof +L1`, log rotation)
- [M] A file was deleted but disk space was not freed. Why? How do you fix it without reboot?
- [M] Server is slow. Walk through your checks. (USE method: CPU, memory, swap, disk I/O with `iostat`/`iotop`, network, then app)
- [M] What is swap? When is high swap usage a problem?
- [M] What is the OOM killer? How do you find what it killed? (`dmesg`, `journalctl -k`)
- [M] Explain `/proc` and give 3 useful files in it.
- [M] How does SSH key-based authentication work? What is `known_hosts`? How do you harden SSH?
- [M] What is `ulimit`? A service fails with "too many open files" - fix it permanently.
- [M] Explain LVM. How do you extend a filesystem without downtime?
- [M] What is `strace` and when would you use it?
- [S] What exactly happens when you type `ls -l` in a shell? (fork, exec, syscalls, wait)
- [S] Namespaces and cgroups - what are they and how do containers use them?
- [S] A process is in `D` state and cannot be killed. Why?
- [S] How would you troubleshoot intermittent high latency on a server where CPU and memory look normal?

### Networking (asked in almost every loop)
- [B] OSI model and TCP/IP model. Where do HTTP, TCP, IP, Ethernet sit?
- [B] TCP vs UDP. Give examples of each.
- [B] Explain the TCP 3-way handshake.
- [B] Common ports: 22, 25, 53, 80, 443, 3306, 5432, 6443, 2379.
- [B] What is DNS? Explain A, AAAA, CNAME, MX, TXT, NS records.
- [B] What is a subnet? How many usable IPs in a /24, /26, /28?
- [B] Public vs private IP ranges. What is NAT?
- [B] `ping`, `traceroute`, `dig`/`nslookup`, `curl -v`, `telnet`/`nc` - what each tells you.
- [M] What happens when you type `https://google.com` in the browser and press Enter? (DNS resolution chain, TCP, TLS handshake, HTTP, response)
- [M] How does a TLS handshake work? What is a certificate chain? What breaks when an intermediate certificate is missing?
- [M] HTTP status codes: 301 vs 302, 401 vs 403, 500 vs 502 vs 503 vs 504. What usually causes a 502 behind a load balancer?
- [M] L4 vs L7 load balancing. Reverse proxy vs forward proxy.
- [M] What is DNS TTL? You changed a record but some users still hit the old IP - why?
- [M] HTTP/1.1 vs HTTP/2 vs HTTP/3. What is keep-alive?
- [M] What is MTU? What symptoms do MTU problems cause?
- [M] iptables basics: allow port 443 only from one subnet.
- [M] "Connection refused" vs "connection timed out" - what does each tell you?
- [S] A service can reach a database by IP but not by hostname, only from some pods. How do you debug?
- [S] Explain TIME_WAIT. A server has thousands of TIME_WAIT connections - is that a problem?
- [S] Design: users in India and US hit the same app. How do you route them to the nearest region and fail over?

## 3. Shell scripting

### Questions
- [B] What is the shebang line? `#!/bin/bash` vs `#!/usr/bin/env bash`.
- [B] How do you pass arguments to a script? `$0`, `$1`, `$#`, `$@`, `$*`, `$?`, `$$`.
- [B] Difference between `"$@"` and `"$*"`.
- [B] Single quotes vs double quotes.
- [B] How do you check if a file exists, a directory exists, a variable is empty?
- [B] `>` vs `>>` vs `2>&1` vs `&>`. What does `/dev/null` do?
- [B] Write a for loop and a while loop that reads a file line by line.
- [M] What does `set -euo pipefail` do and why do you use it?
- [M] How do you handle errors and cleanup in a script? (`trap` on EXIT/ERR)
- [M] `grep`, `awk`, `sed`, `cut`, `sort`, `uniq`, `xargs`, `find` - one practical use of each.
- [M] Difference between `$(command)` and backticks; `[ ]` vs `[[ ]]`.
- [M] How do you make a script idempotent?
- [M] How do you debug a shell script? (`bash -x`, `set -x`)
- [M] How do you run a command on 50 servers? (loop over SSH, parallel, or better: Ansible)
- [S] When do you stop using Bash and switch to Python?

### Hands-on tasks (practise writing these from scratch, timed)
1. Print the top 10 IP addresses from an nginx access log by request count.
2. Count HTTP 5xx responses per minute from an access log.
3. Find and delete files older than 7 days in `/var/log/app`, but only `*.log`.
4. Check disk usage on `/` and send an alert (echo/curl to a webhook) if above 80%.
5. Check whether a list of URLs return HTTP 200; print failures.
6. Back up a directory to a timestamped `.tar.gz` and keep only the last 5 backups.
7. Monitor a process; restart it if it is not running, and log the restart.
8. Read a CSV of usernames and create users only if they do not exist.
9. Replace a value in a config file across all `*.conf` files in a directory tree.
10. Script that takes an environment name as argument (dev/stage/prod), validates it, and fails with usage help otherwise.

## 4. Docker

- [B] What is a container? Container vs virtual machine.
- [B] Image vs container. What is a registry?
- [B] Explain the main Dockerfile instructions: FROM, RUN, COPY, ADD, WORKDIR, ENV, ARG, EXPOSE, CMD, ENTRYPOINT, USER.
- [B] CMD vs ENTRYPOINT. How do they combine?
- [B] COPY vs ADD.
- [B] ARG vs ENV.
- [B] Common commands: list running/all containers, logs, exec into a container, inspect, remove unused images.
- [B] What is a Docker volume? Volume vs bind mount.
- [B] How do you expose a container port? `-p 8080:80` meaning.
- [M] What are image layers? How does build cache work, and how do you order a Dockerfile to use it well?
- [M] What is a multi-stage build? Write one for a Python or Go app.
- [M] How do you reduce image size? (slim/distroless base, multi-stage, `.dockerignore`, combine RUN, no build tools in final image)
- [M] Why should a container not run as root? How do you change that?
- [M] Docker networking: bridge, host, none, overlay. How do two containers talk to each other by name?
- [M] What is Docker Compose and when do you use it?
- [M] A container keeps restarting. How do you debug? (`docker logs`, `docker inspect` exit code, OOMKilled flag, run entrypoint manually)
- [M] What do exit codes 0, 1, 137, 139, 143 mean?
- [M] How do you pass secrets to a container? Why are ENV variables in an image a bad idea?
- [M] What is the PID 1 problem in containers? Why does `docker stop` sometimes take 10 seconds? (signals, `exec` form, `tini`)
- [M] How do health checks work in Docker?
- [M] How do you scan images for vulnerabilities? (Trivy, Grype, ECR scanning)
- [M] Explain image tagging strategy. Why is `latest` bad for production?
- [S] How does Docker actually isolate processes? (namespaces, cgroups, union filesystem/overlayfs)
- [S] Docker vs containerd vs CRI-O. Why did Kubernetes remove dockershim?
- [S] What is BuildKit? How do you speed up image builds in CI? (cache mounts, registry cache, layer ordering)
- [S] How do you sign images and verify them before deployment? (cosign, SBOM)
- [S] Build a reproducible image: what makes builds non-reproducible?

## 5. Python for DevOps

### How much Python is enough
You need **scripting and automation Python**, not software-engineer Python. You
are done when you can, in 20-30 minutes without help, write a script that reads
a file or API, processes data, handles errors, and calls AWS or a REST API.

**Must know (this month):**
- Data types: str, list, dict, set, tuple; slicing; list/dict comprehensions.
- Control flow, functions, `*args`/`**kwargs`, default arguments.
- String handling: split, join, strip, f-strings, `re` basics.
- Files: `with open(...)`, reading line by line, `json`, `csv`, `yaml` (PyYAML).
- Errors: try/except/finally, raising exceptions, custom exceptions.
- Modules: `os`, `sys`, `pathlib`, `shutil`, `subprocess`, `argparse`, `logging`, `datetime`, `collections` (Counter, defaultdict).
- HTTP: `requests` (GET/POST, headers, timeouts, status codes, retries).
- AWS: `boto3` - list EC2 instances, stop instances by tag, list S3 objects, upload a file, paginators.
- Virtual environments, `pip`, `requirements.txt`, `pyproject.toml` basics (you already know packaging deeply - use that).
- Classes at a basic level: `__init__`, methods, when to use a class.

**Good to know (next month):** `concurrent.futures` (threads for parallel API
calls), decorators, generators, type hints, `pytest` basics, Kubernetes Python
client, writing a small Flask/FastAPI health endpoint.

**Not needed:** deep OOP design patterns, metaclasses, data science libraries,
advanced algorithms.

**Coding rounds:** practise ~30 easy LeetCode-style problems in Python:
two-sum (hash map), reverse a string, valid anagram, valid parentheses (stack),
first non-repeating character, merge two sorted lists, find duplicates, group
anagrams, top-k frequent elements, longest substring without repeating chars.

### Questions
- [B] List vs tuple vs set vs dict. Which are mutable?
- [B] What is a virtual environment and why use it?
- [B] How do you read a JSON file and access a nested key safely?
- [B] How do you run a shell command from Python and capture output? (`subprocess.run` with `check=True`, `capture_output=True`, `text=True`)
- [B] Difference between `is` and `==`.
- [B] What is `if __name__ == "__main__":` for?
- [M] Why is `subprocess` with `shell=True` risky?
- [M] How do you handle retries with backoff when calling an API?
- [M] Explain the GIL. Threads vs processes vs asyncio - which for many API calls?
- [M] What is a decorator? Write a simple timing or retry decorator.
- [M] What is a generator and why use one for a 10 GB log file?
- [M] How do you manage secrets in a Python script? (env vars, AWS Secrets Manager, never hardcode)
- [M] boto3: client vs resource. How does pagination work?
- [M] How do you structure logging in a script (levels, format, file vs stdout)?
- [S] How would you package and distribute an internal CLI tool to 50 engineers? (your strong area: pyproject, wheels, entry points, private index)

### Hands-on tasks
1. Parse an nginx log; print top 5 IPs and count of each status code.
2. Read a YAML config, validate required keys, exit with clear errors.
3. Check a list of URLs concurrently; report status code and response time.
4. boto3: list all EC2 instances with tag `env=dev` that are running, and stop them (dry-run flag).
5. boto3: find S3 buckets without encryption or with public access.
6. Delete EBS snapshots older than 30 days (with a dry-run mode).
7. Call the GitHub API: list open PRs in a repo older than 7 days.
8. Write a CLI with `argparse`: `deploy.py --env prod --version 1.2.3` with validation.
9. Compare two JSON/YAML config files and print the differences.
10. Health-check script: check disk, memory and a process, output JSON.

## 6. Kubernetes

### Architecture and basics
- [B] What is Kubernetes and what problems does it solve?
- [B] Control plane components: kube-apiserver, etcd, kube-scheduler, kube-controller-manager, cloud-controller-manager. What does each do?
- [B] Worker node components: kubelet, kube-proxy, container runtime.
- [B] Pod vs Deployment vs ReplicaSet.
- [B] What is a namespace? What is not namespaced?
- [B] Service types: ClusterIP, NodePort, LoadBalancer, ExternalName. What is a headless Service?
- [B] ConfigMap vs Secret. Are Secrets encrypted by default?
- [B] Basic `kubectl`: get, describe, logs (with `--previous`), exec, apply, delete, rollout status/undo, port-forward.
- [B] What are labels and selectors?

### Workloads and configuration
- [M] Deployment vs StatefulSet vs DaemonSet vs Job vs CronJob - one real use of each.
- [M] Explain rolling update strategy: `maxSurge` and `maxUnavailable`. How do you roll back?
- [M] How do you restart all pods in a Deployment without changing the image? (`kubectl rollout restart`)
- [M] Liveness vs readiness vs startup probes. Which runs first? What happens when each fails?
- [M] Resource requests vs limits. What happens when a container exceeds its memory limit? Its CPU limit?
- [M] QoS classes: Guaranteed, Burstable, BestEffort. Which pods are evicted first?
- [M] Explain PV, PVC and StorageClass. Static vs dynamic provisioning. Access modes.
- [M] How do you expand a PVC that is full? (you did this at Inspiron - be ready with details)
- [M] Init containers and sidecar containers - use cases.
- [M] HPA: how does it work, what does it need (metrics-server), and why might it not scale?
- [M] HPA vs VPA vs Cluster Autoscaler vs Karpenter.
- [M] Taints and tolerations vs node affinity vs nodeSelector. Cordon vs drain vs taint.
- [M] How do you move workloads off an unhealthy node? (cordon, drain, PodDisruptionBudget)
- [M] What is a PodDisruptionBudget and why can it block a node drain?
- [M] What is Helm? Chart, release, values. How do you pass values at install time? How do you roll back a release?
- [M] RBAC: Role vs ClusterRole, RoleBinding vs ClusterRoleBinding, ServiceAccount. Give a pod read-only access to ConfigMaps in its namespace.

### Networking
- [M] How does a request flow from a domain name to a pod? (DNS -> load balancer -> Ingress controller -> Service -> kube-proxy/iptables or IPVS -> pod)
- [M] What does kube-proxy actually do?
- [M] What is a CNI plugin? Name a few (Calico, Cilium, AWS VPC CNI).
- [M] Ingress vs Ingress controller vs LoadBalancer Service. What is the Gateway API?
- [M] How does service discovery work? What is CoreDNS? Format of a Service DNS name.
- [M] What is a NetworkPolicy? Allow traffic to a database pod only from the backend pods.
- [S] Pods on different nodes cannot talk to each other. How do you debug?

### Troubleshooting scenarios (most important part)
- [M] Pod stuck in `Pending`. All possible causes and how you check each. (insufficient resources, taints, affinity, PVC not bound, quota)
- [M] `CrashLoopBackOff` - causes and your debugging steps.
- [M] `ImagePullBackOff` / `ErrImagePull` - causes.
- [M] Pod is `OOMKilled`. What do you do short-term and long-term?
- [M] Pod is Running but the app is not reachable through the Service. Debug path. (selector/labels, endpoints, targetPort, readiness, NetworkPolicy)
- [M] Deployment rollout is stuck. How do you find out why?
- [M] Node is `NotReady`. What do you check? (kubelet, disk/memory pressure, network, runtime)
- [M] A pod is evicted. Why does that happen?
- [S] Pods restart randomly only during peak traffic. Investigation plan.
- [S] The API server is slow / kubectl hangs. What could be wrong? (etcd, too many objects, webhooks)
- [S] A bad deployment caused 5xx errors in production. Your incident steps, and what you change afterwards so it cannot happen again.
- [S] Hands-on: given a broken Deployment (wrong selector, huge requests, wrong port, failing probe) - fix it live.

### Design and security
- [S] How would you do blue-green and canary deployments on Kubernetes? (Argo Rollouts, service switching, Ingress weights)
- [S] What is GitOps? How does Argo CD work? Push vs pull deployment.
- [S] How do you manage secrets properly? (External Secrets Operator + AWS Secrets Manager, Sealed Secrets, SOPS, Vault)
- [S] How do you upgrade a Kubernetes cluster with zero downtime? (EKS: control plane, then node groups, PDBs, version skew)
- [S] Pod security: Pod Security Standards, `runAsNonRoot`, read-only root filesystem, dropping capabilities.
- [S] How do you run a multi-tenant cluster for several teams? (namespaces, RBAC, quotas, LimitRange, NetworkPolicy)
- [S] How do you back up a cluster? (etcd snapshot, Velero)
- [S] How would you reduce Kubernetes cost by 30%? (right-sizing requests, spot nodes, Karpenter, autoscaling, idle namespaces)

## 7. GitHub Actions

This is your strongest topic - you run real multi-platform pipelines. Aim to
answer every [M] question with a MOOSE example.

- [B] What are workflows, events, jobs, steps, actions and runners?
- [B] Common triggers: push, pull_request, schedule, workflow_dispatch, release, tags.
- [B] GitHub-hosted vs self-hosted runners.
- [B] How do you use secrets in a workflow? Where are they stored?
- [M] How do jobs share data? (artifacts vs cache vs job outputs)
- [M] How does a matrix build work? `fail-fast`, `include`/`exclude`, `max-parallel`. (cibuildwheel across OS and Python versions)
- [M] How does `actions/cache` work? What is a good cache key?
- [M] `needs`, `if` conditions, and running a job only on tags or on main.
- [M] `pull_request` vs `pull_request_target` - why is the latter dangerous?
- [M] What is `GITHUB_TOKEN` and how do you restrict its permissions?
- [M] How do you deploy to AWS from GitHub Actions without storing access keys? (OIDC + IAM role with trust policy on repo/branch)
- [M] Reusable workflows vs composite actions.
- [M] Environments and required reviewers for production deployment.
- [M] `concurrency` groups - how do you stop two deployments running at once?
- [M] How do you debug a failing workflow? (step debug logging, re-run with debug, `act` locally, tmate)
- [M] How do you publish a Python package to PyPI from Actions? (trusted publishing - you can speak from experience)
- [S] Why should you pin third-party actions to a commit SHA? (supply-chain attacks such as the tj-actions compromise in 2025)
- [S] Design a CI/CD pipeline for a microservice: build, test, scan, push image, deploy to staging, approval, deploy to prod, rollback.
- [S] CI takes 40 minutes. How do you bring it under 10? (caching, parallel jobs, test splitting, path filters, larger runners)
- [S] How would you run self-hosted runners at scale on Kubernetes? (Actions Runner Controller), and what are the security risks?
- [S] Monorepo: run only the pipelines for services that changed.

## 8. Jenkins

- [B] What is Jenkins? Freestyle job vs Pipeline.
- [B] Declarative vs scripted pipeline.
- [B] Structure of a Jenkinsfile: agent, stages, steps, post, environment.
- [B] How do you trigger a job? (webhook, poll SCM, cron, upstream job, manual)
- [B] How do you point a job to a branch other than master/main?
- [M] Master (controller) and agent architecture. Why should builds not run on the controller?
- [M] How do you store and use credentials in Jenkins? (Credentials plugin, `withCredentials`)
- [M] What is a multibranch pipeline?
- [M] What are shared libraries and why use them?
- [M] Parallel stages - how and why.
- [M] How do you add a manual approval stage before production? (`input`)
- [M] What does `post { always / success / failure }` do? Use it for notifications and cleanup.
- [M] How do you run builds inside Docker or on Kubernetes agents?
- [M] How do you back up Jenkins and upgrade it safely?
- [M] A build works locally but fails on Jenkins. How do you debug?
- [S] Jenkins is slow and builds queue for a long time. What do you check and change?
- [S] Jenkins vs GitHub Actions vs GitLab CI - when would you choose each? Would you migrate from Jenkins, and how?
- [S] How do you secure Jenkins? (RBAC, no anonymous access, plugin hygiene, agents isolation, secrets)
- [S] Write (on paper) a Jenkinsfile: checkout, build Docker image, run tests, push to ECR, deploy to EKS with Helm, notify on failure.

## 9. AWS

### Core / IAM
- [B] Region vs Availability Zone vs edge location.
- [B] What is IAM? Users, groups, roles, policies.
- [B] IAM role vs IAM user. Why use roles for EC2 and pods?
- [M] Identity-based policy vs resource-based policy. How is an explicit deny evaluated?
- [M] How do you give an EC2 instance access to S3 without keys? (instance profile)
- [M] How do you give access across AWS accounts? (assume role, trust policy)
- [M] What is least privilege and how do you find unused permissions? (IAM Access Analyzer)
- [S] How do pods in EKS get AWS permissions? (IRSA vs EKS Pod Identity)
- [S] Organizations and SCPs - what are they for?

### Networking (VPC is asked in almost every AWS loop)
- [B] What is a VPC? Public vs private subnet - what actually makes a subnet public?
- [B] Internet Gateway vs NAT Gateway.
- [B] Security group vs Network ACL (stateful vs stateless).
- [M] Design a VPC for a 3-tier app across 2 AZs. Subnets, route tables, NAT, where each tier lives.
- [M] Private EC2 cannot reach the internet. Debug path.
- [M] VPC peering vs Transit Gateway vs PrivateLink.
- [M] What are VPC endpoints? Gateway vs interface endpoints. Why use them for S3?
- [M] Route 53: A vs CNAME vs Alias. Routing policies (simple, weighted, latency, failover, geolocation). (you used Route 53 - expect follow-ups)
- [S] Design multi-region active-passive with automatic failover.

### Compute and scaling
- [B] EC2 instance types and purchase options: on-demand, reserved, savings plans, spot.
- [B] What is an AMI? EBS vs instance store.
- [M] Auto Scaling Group: launch template, desired/min/max, scaling policies (target tracking, step, scheduled), health checks. (you used this - be ready)
- [M] ALB vs NLB vs Classic LB. Target groups and health checks.
- [M] ALB returns 502/503/504 - what does each usually mean?
- [M] What is IMDSv2 and why enforce it?
- [M] How do you do a zero-downtime deployment on EC2? (rolling ASG instance refresh, blue-green with target groups)
- [S] ECS vs EKS vs Lambda - when to use which.
- [S] EKS: managed node groups vs Fargate vs Karpenter.

### Storage and databases
- [B] S3 storage classes and lifecycle rules.
- [M] S3 versioning, replication, Object Lock. How do you migrate data between buckets? (you did this: `aws s3 sync`, replication, Batch Operations, permissions on both sides)
- [M] Give temporary access to one S3 object. (pre-signed URL) Bucket policy vs IAM policy vs pre-signed URL.
- [M] Give a user in another account access to one S3 file.
- [M] How do you block public access and enforce encryption on S3?
- [M] RDS Multi-AZ vs read replicas. How does failover work?
- [M] How do you upgrade an RDS engine version with minimal downtime? (snapshot, test on a restored copy, maintenance window, blue/green deployments) (you did this - expect detail)
- [M] RDS backups: automated backups vs snapshots, point-in-time recovery.
- [S] Database CPU spikes while traffic is flat. How do you investigate?

### Security, monitoring and cost
- [M] Where do you store secrets? Secrets Manager vs Parameter Store.
- [M] KMS: what is envelope encryption?
- [M] CloudWatch metrics, logs, alarms. CloudTrail vs CloudWatch vs AWS Config.
- [M] ACM certificates: DNS vs email validation, renewal, attaching to ALB/CloudFront. (you managed SSL - expect follow-ups)
- [M] What is CloudFront and why put it in front of S3 or an ALB?
- [S] Your AWS bill doubled this month. How do you find out why and fix it? (Cost Explorer, tags, NAT data transfer, idle resources, rightsizing)
- [S] An access key leaked on GitHub. Incident steps.
- [S] Design highly available, secure infrastructure for a web app with a database, and explain each choice.

---

## 10. Questions about your own resume (prepare these first)

Write a 2-minute answer for each, using STAR (situation, task, action, result).

1. Tell me about yourself. (90 seconds: Cognizant -> self-funded switch -> Inspiron -> NCBS, what you own now, what you want next)
2. Walk me through the pymoose release pipeline end to end.
3. Windows wheels shipped without DLLs - how did you find it, what was the root cause, how did you fix it, how do you prevent it now?
4. macOS wheels had unresolved CBLAS symbols - how did you diagnose it? (`nm`, `otool -L`, what "flat namespace" means)
5. Why did you move from CMake to Meson? What was hard?
6. How did you make jardesigner a single pip install? What was the 65 MB to 12.5 MB fix?
7. What is a hermetic build and why does conda-forge need one?
8. Kubernetes pods crashing at Inspiron - exactly what did you see, which commands, what did you change?
9. RDS upgrade at Inspiron - which versions, how did you plan downtime and rollback?
10. EC2 Auto Scaling for the internal app - what metric and thresholds?
11. Jenkinsfile you wrote - stages, where credentials lived, how you deployed.
12. Terraform on Azure - what resources, where was state stored?
13. Biggest mistake or outage you caused and what you learned.
14. A disagreement with a lead about a technical decision and how it ended. (the jardesigner packaging layout debate is a good one)
15. Why are you leaving? (funded project ends in December; contract was renewed once with a raise)
16. Why only 6 months at Inspiron? (better role, more ownership, open-source engineering)
17. How do you use AI tools in your work, and how do you check their output?
18. Where do you see yourself in 3 years?

---

## Study plan: 1 Nov - 30 Nov 2026

About 2 hours on weekdays, 5-6 hours on weekends. Every day: 30 minutes on
your resume stories (section 10) until they are smooth.

| Week | Weekdays | Weekend |
|---|---|---|
| 1 (1-8 Nov) | Linux core + networking; Git | Shell scripting tasks 1-10; mock round on Linux/networking |
| 2 (9-15 Nov) | Docker; Python must-know list | Python tasks 1-10; 10 easy coding problems |
| 3 (16-22 Nov) | Kubernetes basics, workloads, networking (use kind or minikube locally) | Kubernetes troubleshooting: break and fix your own cluster |
| 4 (23-30 Nov) | GitHub Actions, Jenkins, AWS (IAM, VPC, EC2/ASG, S3, RDS) | Full mock interviews (2), revise all "shaky" questions |

Labs (free or cheap): `kind`/`minikube` for Kubernetes, Killercoda scenarios,
AWS free tier (set a billing alarm first), GitHub Actions in a personal repo.

---

## Sources

- [Razorpay Senior DevOps Engineer interview, India, Oct 2025 (Taro)](https://www.jointaro.com/interviews/companies/razorpay/experiences/senior-devops-engineer-india-october-23-2025-accepted-offer-neutral-e87a2a07)
- [Razorpay DevOps interview guide (Dataford)](https://dataford.io/interview-guides/razorpay/devops-engineer)
- [DevOps interview experiences: Livspace, Microsoft (PlacementPreparation)](https://www.placementpreparation.io/blog/devops-engineer-interview-experience/)
- [DevOps interview experience: 32 practical questions (Substack)](https://arvindverma021.substack.com/p/todays-devops-interview-experience)
- [PwC DevOps interview questions 2026 (Substack)](https://arvindverma021.substack.com/p/pwc-devops-engineer-interview-questions)
- [EY DevOps interview experience (Medium)](https://medium.com/@sre-devops-interview/ey-devops-engineer-interview-experience-questions-8d8a7606082d)
- [AmbitionBox DevOps Engineer interview questions](https://www.ambitionbox.com/profiles/devops-engineer/interview-questions)
- [Glassdoor DevOps Engineer interviews](https://www.glassdoor.com/Interview/devops-engineer-interview-questions-SRCH_KO0,15_SDRD.htm)
- [Glassdoor Senior DevOps Engineer interviews](https://static.glassdoor.com.ar/Interview/senior-devops-engineer-interview-questions-SRCH_KO0,22_IP185.htm)
- [DevOps engineer interview questions India 2026 (HireStepX)](https://hirestepx.com/blog/devops-engineer-interview-questions-india-2026)
- [KodeKloud DevOps interview questions](https://kodekloud.com/blog/devops-interview-questions/)
- [DevOps-Interview-Questions, 2,200+ questions with answers (GitHub)](https://github.com/NotHarshhaa/DevOps-Interview-Questions)
