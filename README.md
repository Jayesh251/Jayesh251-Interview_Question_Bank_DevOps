# DevOps Interview Question Bank (2026)

Target: DevOps / Platform / SRE roles, from startups and Indian product companies
(goal ~20 LPA) up to big tech (Google, Meta, Apple, Amazon, Microsoft).
November scope: Git, Linux (with networking), shell scripting, Docker, Python,
Kubernetes, GitHub Actions, Jenkins, AWS.

Compiled 29 Oct 2026 from candidate interview reports (GeeksforGeeks, Glassdoor
and LeetCode snippets, Taro, Blind, AmbitionBox, Substack/Medium/dev.to
write-ups), Indian and big-tech company loops, public question collections on
GitHub, and official material (Google SRE Workbook, AWS EKS best practices,
GitHub Actions security hardening). All questions are paraphrased.

## Contents

- **Part 1: Questions by topic** - for each topic: core questions, additional
  questions from public collections, and questions reported in real interviews.
- **Part 2: Writing tasks** - Dockerfiles, Kubernetes manifests, Helm, Jenkinsfiles,
  GitHub Actions workflows, shell and Python scripts.
- **Part 3: Debugging questions** - symptom questions and broken-setup labs.
- **Part 4: Company interview loops and big-tech track.**
- **Part 5: Your resume questions, study plan and sources.**

## How to read the tags

Level:
- **[B] Basic** - asked to everyone. Answer in under a minute without hesitation.
- **[M] Mid (2-4 years)** - explain how it works and give an example from your work.
- **[S] Senior (5-6 years)** - scenarios, design and trade-offs.

Source (on reported questions):
- `[Meta]`, `[GfG, Company, year]`, `[Glassdoor/Company]`, `[Taro/Company]`,
  `[LeetCode/Company]` - a candidate reported being asked this.
- `[GfG list]`, `[Guide]` and `(agg)` - prep sites or aggregators that say the
  question is commonly asked but show no named candidate. Lower confidence.
- `[C5]`-style codes refer to the big-tech source list at the end.

## How to study each question

1. Answer out loud without notes (record yourself).
2. Check it against docs, or run it in a terminal or lab.
3. Mark it solid / shaky / don't know. Revisit shaky ones every 3 days.

For any concept, expect "show me": type the command, write the YAML, or fix a
broken setup live. Reading answers is not enough.

## What interviews look like in 2026

- **Startups and Indian product companies (4-6 rounds):** recruiter screen ->
  technical screen -> hands-on (fix a broken cluster/pipeline, write a script or
  Dockerfile, or build a CI/CD pipeline as an assignment) -> design / deep dive ->
  hiring manager -> HR. Tool knowledge (Docker, Kubernetes, Jenkins, AWS) is tested
  heavily, and coding is usually easy (Razorpay 2025: coding in every round, easy).
- **Big tech (Google SRE, Meta PE, Amazon, Apple):** tools are barely tested.
  Expect deep Linux internals, networking from first principles, troubleshooting
  graded on method, practical coding with harder follow-ups ("now the file is
  50 GB"), large-scale system design with real numbers (Google NALSD), and a values
  or leadership-principles round that can decide the result.
- **Everywhere:** Kubernetes pod failures (CrashLoopBackOff, Pending,
  ImagePullBackOff, OOMKilled), disk and resource triage, "what happens when you
  type a URL", Docker basics, AWS VPC and IAM, and an end-to-end walkthrough of a
  pipeline you built are the most repeated questions across all sources.

---

# Part 1: Questions by topic

## 1. Git

### Core questions

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

### Additional questions (public question collections)

- [B] What are the working directory, staging area (index) and repository? Where does `git add` put a change?
- [B] How do you discard uncommitted changes to one file? (`git restore`, `git checkout --`)
- [B] How do you delete a branch locally and on the remote?
- [M] You ran `git reset --hard` and lost three days of commits. Walk through recovery.
- [M] Someone force-pushed to `main` and overwrote other people's commits. How do you recover, and how do you stop it happening again?
- [M] Revert one file to how it looked two commits ago without touching anything else.
- [M] What does `git rebase --onto` do? Give a case where you need it (moving a branch off the wrong base).
- [M] How do you sign commits (GPG or SSH keys), and why do some orgs require signed commits?
- [M] Developers bypass hooks with `--no-verify`. How do you enforce the same checks in a way they cannot skip? (server-side / CI required checks)
- [M] What are Git submodules? How do you clone a repo including its submodules? Submodules vs subtrees.
- [M] What is `.gitattributes` used for? (line endings, LFS tracking, merge drivers, diff settings)
- [M] A merge reverts cleanly but re-merging the same branch later brings nothing in. Why? (revert of a merge, revert-the-revert)
- [M] What merge strategies does Git have? (ort/recursive, ours, octopus) What is `-X ours` vs `-s ours`?
- [M] What happens internally when you run `git branch feature`? (a new ref file pointing at the current commit)
- [S] How is `git status` fast on a repo with 200k files? (index stat cache, fsmonitor, untracked cache)
- [S] What is a pre-receive hook? Design one that rejects pushes containing large files or secrets.
- [S] What are the risks of storing CI credentials in `.git/config` or the credential store on shared runners? Credential store vs cache.
- [S] What is `safe.directory` and why did Git add it? (CVE around repos owned by other users)
- [S] How would you structure Git repos for GitOps: app code and Kubernetes manifests in one repo or separate? Environment branches vs environment folders.
- [S] Git anti-patterns you have seen and how you fixed them (long-lived branches, giant commits, committing generated files, env branches).

#### Hands-on tasks
1. Recover a commit "lost" after `reset --hard` using reflog and create a branch from it.
2. Move the 4th-last commit to the top of the branch with interactive rebase (and explain what changes for already-pushed history).
3. Remove a 200 MB file and an `.env` file from all history with `git filter-repo`, then force-push and tell collaborators what to do.
4. Configure SSH commit signing and show a verified commit.
5. Write a `pre-commit` config that runs a secret scanner (gitleaks) and a linter.

### Reported in real interviews

#### Indian companies, startups and community reports: Git

- [GfG, rtCamp, ~2023] [B] Can Git be used without GitHub? How do the two differ?
- [GfG, rtCamp, ~2023] [B] Where does Git store its data?
- [GfG, Paytm, 2024] [B] Explain Git and how you manage code with branches. What are the advantages of branches?
- [GfG, Paytm, 2024] [B] Walk through creating code and pushing it to GitHub.
- [GfG, HashedIn, 2022] [B] Common Git and GitHub commands, and Git vs GitHub.
- [Apurv Gujjar blog, 2025] [B] Explain the Git workflow from `git init` to a pull request.
- [Taro/Apple India, 2025] [B] Specific GitHub/Git commands, with exact answers expected.
- [Substack MNC, 2026] [M] Do you follow GitFlow or trunk-based development?
- [GfG list] [B] git fetch vs git pull.
- [GfG list] [M] git merge vs git rebase, and when to use each.
- [GfG list] [B] What is git stash? stash apply vs stash pop.
- [GfG list] [M] What does cherry-pick do?
- [GfG list] [M] How do you revert a commit that is already pushed and public?
- [GfG list] [M] revert vs reset, and reset --hard vs clean -fd.
- [GfG list] [M] reflog vs log.
- [GfG list] [M] What is a detached HEAD?
- [GfG list] [M] How do you squash several commits with an interactive rebase?
- [GfG list] [M] How do you resolve a merge conflict?
- [GfG list] [M] What are Git hooks? Set up a pre-commit check.
- [GfG list] [S] How do you use git bisect to find a bad commit?
- [GfG list] [M] Lightweight vs annotated tags. What is SemVer?
- [GfG list] [M] How do you handle large binaries in Git, with and without LFS?
- [GfG list] [M] What are submodules?
- [GfG list] [B] How do you change the last commit message?
- [GfG list] [M] How do you delete a remote branch? How do you manage multiple remotes?
- [GfG list] [S] Explain the Git object model and what HEAD, the index and the working tree are.

#### Big tech (Google, Meta, Apple, Amazon, Microsoft and others): Git

- No company-specific Git questions turned up in any report for these nine companies. Git is not a tested area in these loops. Expect it, at most, inside behavioural or CI/CD design discussion. One design list mentions "design a GitHub-like version-control system" for Apple [C37 (agg)].

## 2. Linux (with networking)

### Core questions

#### Core
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

#### Networking (asked in almost every loop)
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

### Additional questions (public question collections)

#### Core
- [B] Your first five commands after logging into an unknown server, and why.
- [B] Every command fails with "command not found". How do you find the cause? (`PATH`, broken profile, use absolute paths)
- [B] What is `umask`? What do 022, 027 and 077 produce for new files and directories?
- [B] Run a long command so it survives logout. `&` vs `nohup` vs `disown` vs `tmux`/`screen`.
- [B] What are the process states R, S, D, Z, T in `ps`/`top` output?
- [B] Create a system user for a service that cannot log in. (`/sbin/nologin` vs `/bin/false`)
- [B] Find files over 100 MB, or modified in the last 60 minutes.
- [M] Load average is 40 on an 8-core server but CPU usage is only 15%. What is going on? (I/O wait, `D` state processes, NFS hangs)
- [M] `df` and `du` show very different numbers. List every reason. (deleted-open files, files hidden under a mount point, reserved blocks, snapshots)
- [M] "No space left on device" but `df -h` shows free space. (inodes, reserved blocks, quotas)
- [M] A server rebooted and you cannot SSH in after 10 minutes. What could be wrong and how do you get in? (fstab error, emergency mode, sshd failed, firewall, serial console / SSM)
- [M] The root filesystem fails to mount at boot. How do you recover? (rescue mode, `fsck`, fix `/etc/fstab`, `nofail`)
- [M] `umount` says "target is busy". How do you find what is holding it? (`lsof`, `fuser -m`)
- [M] How does the OOM killer choose a victim? How do you protect a critical process? (`oom_score`, `oom_score_adj`)
- [M] What is swappiness? What would you set it to on a database server and why?
- [M] What is the page cache? Why does `free` show almost no free memory on a healthy server? (buff/cache, "available")
- [M] Add a new disk to a running server and put a filesystem on it without reboot. (rescan, `lsblk`, partition, `mkfs`, fstab with UUID)
- [M] You accidentally deleted a script/log that a running process still has open. How do you recover it? (`/proc/<pid>/fd`)
- [M] What is a file descriptor? How do you find a process leaking file descriptors?
- [M] Limit a process to a fixed share of CPU or memory. (`nice`/`renice`, cgroups via `systemd-run`, `cpulimit`)
- [M] What does `LD_PRELOAD` do and when is it used? How do you fix "error while loading shared libraries"? (`ldd`, `ldconfig`, `LD_LIBRARY_PATH`)
- [M] What is `sysctl`? Change a kernel parameter at runtime and make it persistent.
- [M] Is it safe to attach `strace` to a production process? What is the overhead? Alternatives (`perf`, eBPF tools).
- [M] How do you log every command run as root on a server? (auditd, sudo logging, session recording)
- [S] High iowait on a 24-core server with idle CPUs. How do you find which process and which disk? (`iostat -x`, `iotop`, `pidstat -d`, `/proc/pressure`)
- [S] A server becomes unresponsive over SSH because memory is exhausted. How do you prevent it from happening again? (cgroup limits, earlyoom/systemd-oomd, swap, alerting)
- [S] What is Transparent Huge Pages and why do Redis and MongoDB recommend disabling it?
- [S] `vm.dirty_ratio` vs `vm.dirty_background_ratio` - what problem do large writes cause and how do you tune it?
- [S] What is Pressure Stall Information (PSI) and why is it better than load average for spotting resource pressure?
- [S] What is copy-on-write in `fork()` and why does it matter for memory usage of preforking servers?
- [S] Fastest way to delete 100 GB of small files or copy terabytes between servers. (`rsync` options, tar over ssh, parallel copies)
- [S] What are the risks of LVM snapshots on a busy database volume?
- [S] Upgrade the kernel on 500 servers safely. (canary batch, config management, reboot orchestration, rollback via previous kernel)
- [S] What is the golden troubleshooting order you follow in a production Linux outage, and why that order?

#### Networking
- [B] What is ARP? What breaks when ARP entries are wrong?
- [B] What is the default gateway? Show the routing table and the default route. (`ip route`)
- [B] Difference between 127.0.0.1 and `localhost`. Why might `ping localhost` fail?
- [B] Which four values uniquely identify a TCP connection? What are the source and destination ports when a browser talks to a web server?
- [B] What port does `ping` use? (trick: none, ICMP)
- [B] VLAN vs subnet.
- [M] Server A cannot talk to server B. Your step-by-step checks from layer 1 up.
- [M] An HTTP monitor says the site is down, but `telnet host 443` connects. What do you check next?
- [M] What is SSH local vs remote port forwarding? Use a bastion to reach a private database from your laptop. (`ssh -L`, `ProxyJump`)
- [M] "Host key verification failed" after a server rebuild. What does it mean, and should automation disable host key checking?
- [M] How do you capture traffic on one port to see what is really being sent? (`tcpdump -i any port 443 -w`)
- [M] Measure DNS, connect, TLS and time-to-first-byte for one request with `curl`. (`-w` timings)
- [M] Test a TCP port with no `telnet` or `nc` installed. (`/dev/tcp`, `curl telnet://`)
- [M] What is SNI and why can many HTTPS sites share one IP address?
- [M] Check that a private key matches a certificate, and check a certificate's expiry from the command line. (`openssl`)
- [M] What is split-horizon DNS? Authoritative vs recursive resolver vs non-authoritative answer.
- [M] What is a PTR record and where does reverse DNS matter? (mail, logs)
- [M] What is the TCP accept queue (backlog)? How do you detect SYN or accept queue overflow? (`ss -lnt`, `netstat -s`)
- [M] A client gets "connection reset by peer". What sends a RST and why?
- [M] HTTP 413 on file upload behind nginx. Where do you fix it?
- [S] What is conntrack? A busy NAT/Kubernetes node drops packets with "nf_conntrack: table full". Fix it.
- [S] What happens when a process binds to port 0? What are ephemeral ports and how can a busy proxy run out of them?
- [S] How would you tune a Linux server for 100k concurrent connections? (fd limits, `somaxconn`, backlog, ephemeral port range, buffers)
- [S] `SO_REUSEADDR` vs `SO_REUSEPORT`.
- [S] Blocking a whole subnet: iptables rule vs blackhole route vs security group/NACL. Trade-offs.
- [S] iptables vs nftables. Why do modern distributions and Kubernetes care?
- [S] What is a WAF and where would you put one? CORS vs CSP - what does each protect against?

#### Hands-on tasks
1. Find what is filling `/var` and what process holds a deleted 20 GB file; free the space without restarting the service.
2. Configure an SSH bastion with `ProxyJump` and key-only auth; disable root login and passwords.
3. Use `curl -w` to break down request latency into DNS, connect, TLS and server time.
4. Capture a TLS handshake with `tcpdump`, open it in Wireshark and identify the SNI and certificate.
5. Create a systemd service with `MemoryMax=` and `CPUQuota=` and show the limit working.
6. Simulate high iowait with `dd`/`fio` and find it with `iostat`, `pidstat` and `/proc/pressure/io`.

### Reported in real interviews

#### Indian companies, startups and community reports: Linux

- [GfG, rtCamp, ~2023] [B] Find which process or app is using the most memory or storage, and remove it.
- [GfG, rtCamp] [B] How do you kill a running process?
- [GfG, Jivox, 2020] [B] Check free memory. Check disk space.
- [GfG, Jivox, 2020] [B] Hide a file from a particular user. Log in to Linux without a GUI.
- [GfG, Nutanix, 2020] [B] What do df, du, free and vmstat show?
- [GfG, Cohesity, 2024] [B] Which file systems do Windows and Linux use?
- [GfG, Cohesity, 2024] [M] Troubleshoot a hardware issue using Linux commands.
- [GfG, Media.net, ~2023] [M] What is a system call? What happens when a process hits a function call in its text segment?
- [GfG, Media.net] [M] Describe process memory (stack, heap, text, data), and loading vs linking.
- [Taro/Apple India, 2025] [B] Commands to check memory and to partition disks.
- [Glassdoor/Siemens] [M] Questions on find, awk and sed.
- [LeetCode/Google SRE-SE Bangalore] [M] Inodes, networking commands and performance commands.
- [Dataford/PhonePe SRE, 2026] [M] How does a user process talk to the kernel?
- [Dataford/PhonePe SRE] [B] Soft link vs hard link.
- [Apurv Gujjar blog, 2025] [B] Hard vs soft links, inode, `/etc/passwd` vs `/etc/shadow`, SELinux.
- [Apurv Gujjar blog] [B] What are htop, strace, journalctl, ncdu, dig, rsync and nmap used for?
- [Apurv Gujjar blog] [M] Find the 10 largest files. Find which process holds a port. Watch live disk I/O.
- [Apurv Gujjar blog] [M] Check failed login attempts. Find zombie processes.
- [Apurv Gujjar blog] [M] What is a kernel panic?
- [dev.to interview post, 2025] [B] Show the last 5 users who logged in. Check CPU usage.
- [dev.to deadlock SRE list] [M] Walk through Linux boot until the login prompt.
- [dev.to deadlock SRE list] [M] What happens when you type `ls`?
- [dev.to deadlock SRE list] [M] Explain /proc, kernel vs user space, load average and interrupts.
- [dev.to deadlock SRE list] [M] Crash vs panic. Memory leaks. Race conditions.
- [dev.to deadlock SRE list] [M] Which performance tools would you use on a Linux box?
- [GfG list] [M] Explain the permission string `rwxr-xr--`, and SUID/SGID/sticky bit.
- [GfG list] [M] nice/renice, jobs/bg/fg, cron vs at.
- [GfG list] [M] Linux boot process, BIOS vs UEFI, GRUB, systemd/systemctl.
- [GfG list] [M] LVM, swap, RAID 0/1/5. Mount a filesystem permanently.
- [GfG list] [M] What is the OOM killer? Zombie vs orphan processes.
- [k8s-interview-prep repo] [M] What load average counts as high?
- [k8s-interview-prep repo] [S] Linux signals (TERM, KILL, HUP) and how they map to Kubernetes pod shutdown.
- [meetassist, 2025] [B] Change the SSH port from 22 to 322.
- [meetassist, 2025] [B] Change a cron job from 11 p.m. to 10 a.m.

#### Indian companies, startups and community reports: Networking

- [GfG, Inorg Global, ~2024] [B] Explain the OSI model. TCP vs UDP.
- [GfG, Nutanix, 2020] [B] OSI layers 2 and 3, the TCP/IP model, ARP and DHCP.
- [GfG, Nutanix, 2020] [B] Subnetting and IP address types.
- [GfG, Nutanix, 2020] [M] Can more than 2^32 devices be online on IPv4 at once, and how?
- [GfG, Cohesity, 2024] [B] OSI model and DHCP.
- [GfG, Media.net, ~2023] [M] What happens when you type a URL? Cover DNS lookup, IP routing and NAT tables.
- [GfG, Media.net] [M] Walk through the full DNS message exchange.
- [GfG, Media.net] [M] Why classless addressing? Why do private and public IPs exist?
- [GfG, Media.net] [M] Why is UDP unreliable, and where is it used?
- [GfG, MakeMyTrip] [B] GET vs POST, HTTP vs HTTPS, client vs server.
- [GfG, MakeMyTrip] [B] What happens when you open www.google.com?
- [GfG, ZS Associates, 2022] [B] TCP vs UDP. DNS.
- [GfG, ZS Associates, 2022] [B] Which command checks connectivity? How do you tell whether a site is secure?
- [GfG, rtCamp] [B] Sessions vs cookies. Can sessions work without cookies?
- [GfG, rtCamp] [B] Hashing vs encryption. What is a salt?
- [GfG, Harsco, ~2023] [B] HTTP vs HTTPS.
- [Glassdoor/Meesho DevOps, 2021] [M] How does DNS resolution work?
- [LeetCode/eBay Cloud SRE Bangalore] [M] TCP vs UDP vs QUIC.
- [LeetCode/eBay] [M] Explain everything that happens when you load google.com.
- [HN, Jeff Barr thread] [M] Describe everything between clicking a link and the page rendering (round-robin DNS, firewalls, LBs, app servers).
- [Apurv Gujjar blog, 2025] [M] How does an app in a VPC reach the outside world? Which networking protocols do you know?
- [dev.to interview post, 2025] [M] How does an application communicate with the outside world?
- [Glassdoor Bangalore DevOps bank] [S] How would you detect and block bot traffic hitting your servers?
- [k8s-interview-prep repo] [S] Fix TIME_WAIT pile-ups and ephemeral port exhaustion under load.
- [k8s-interview-prep repo] [M] dig vs nslookup. Trace latency with ss, tcpdump and traceroute.
- [GfG list] [B] SSH, SCP vs SFTP, curl, netstat. Check open ports.

#### Big tech (Google, Meta, Apple, Amazon, Microsoft and others): Linux internals

- [Google] What is an inode, and what does it not contain? [C2 (agg)]
- [Google] What does SIGKILL (`kill -9`) do, and what are its downsides? [C2 (agg)]
- [Google] Which signal does `kill` send by default? (older Glassdoor) [C38]
- [Google] What happens when you run `ls *`? [C2 (agg)]
- [Google] How does the shell parse and execute a command like `rm`? [C2 (agg)]
- [Google] What happens when you type `ps` at a shell prompt? (older Glassdoor) [C38]
- [Google] How does `malloc()` work down to the kernel level? [C6 (agg)]
- [Google] How do you delete a file named `-rf`? [C6 (agg)]
- [Google] How do interrupts and signal handlers work, including signal masking? [C10 (agg)]
- [Google] Background vs foreground processes; hard links vs soft links [C10 (agg)]
- [Google] How do you find and clean up zombie processes? [C10 (agg)]
- [Google] Inodes, and SIGTERM vs SIGKILL [C10 (agg)]
- [Google] Rank by latency: CPU register read, disk seek, context switch, main-memory read [C38]
- [Meta] How does `ls` work under the hood (syscalls, directory entries, inodes, permission checks)? [C9 (agg)]
- [Meta] Process vs thread [C7 (agg)]
- [Meta] What is a zombie process, and can you kill it? [C7 (agg), C8 (agg)]
- [Meta] How does `kill` work at the kernel level? [C7 (agg)]
- [Meta] Explain virtual memory, using a process reading a particular memory block as the example [C7 (agg)]
- [Meta] Explain in as much depth as you can what an operating system is and does [C7 (agg)]
- [Meta] Interpret the output of `free -h` [C7 (agg)]
- [Meta] Symlink vs hard link [C8 (agg)]
- [Meta] Onsite Linux round: memory management, processes, file systems, security basics (2025) [C5]
- [Meta] Explain process management, memory allocation and I/O handling internals (Dec 2024 onsite) [C4]
- [Meta] OA topics: fork/exec, PID tree, zombie vs orphan, rwx/umask, /proc, redirection, deleting a file that is still open [C9 (agg)]
- [Meta] OS theory in the second screen: virtual memory and signals [C39]
- [Meta] OA: what does `chmod 755 script.sh` mean? [C7 (agg)]
- [Amazon] Hard vs soft links, inodes, file descriptors [C33 (agg)]
- [Amazon] File permissions and chmod [C33 (agg)]
- [Apple] What is an inode? [C19]
- [Apple] What does load average on Linux mean? [C19]
- [Apple] [S] Questions on Linux filesystems (SRE Manager) [C19]
- [LinkedIn] Linux memory management: why it is needed, virtual memory, swap, swappiness [C28]
- [Google] Why is Linux implemented this way (VM, paging, page cache, page faults, OOM, signals)? Topic list from a candidate prep guide [C40]

#### Big tech (Google, Meta, Apple, Amazon, Microsoft and others): Networking

- [Meta] What happens between typing meta.com (or facebook.com) into a browser and the page rendering: DNS, TCP, TLS, routing, backend, DOM [C8, C9 (agg)]
- [Meta] Follow-up: what are the layers of DNS caching? [C9 (agg)]
- [Meta] Where is DNS configuration stored on a host? [C8 (agg)]
- [Meta] TCP vs UDP [C8 (agg), C9 (agg)]
- [Meta] HTTP vs HTTPS, and why HTTPS is more secure [C41 (agg)]
- [Meta] How does ping work? [C21]
- [Meta Network PE] Explain the difference between BGP and OSPF in depth [C29]
- [Meta Network PE] TCP handshake, and why TCP congestion control is a problem [C29]
- [Meta Network PE] ARP, DHCP, DNS, HTTP fundamentals [C29]
- [Google] Walk through the full lifecycle of loading a URL in a browser [C10 (agg)]
- [Google] Explain QUIC and what it improved [C10 (agg)]
- [Google] OSI model; TCP handshake [C2 (agg)]
- [Google] HTTP methods [C2 (agg)]
- [Google] When would you use nslookup, dig, ping or traceroute? [C2 (agg)]
- [Amazon] OSI layers, TCP vs UDP, how TCP works [C42]
- [Amazon] Diagnose transmission errors with pcaps: zero-window, duplicate ACKs [C42]
- [Amazon] What happens when you type amazon.com; DNS resolution; the 3-way handshake [C33 (agg)]
- [Amazon] End-to-end packet flow to a service behind a load balancer across subnets [C33 (agg)]
- [LinkedIn] TCP/IP, ARP, DNS resolution [C28]
- [Apple] Network-stack round (part of the Network stack + Linux internals round) [C18]

## 3. Shell scripting

### Core questions

#### Questions
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

#### Hands-on tasks (practise writing these from scratch, timed)
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

### Additional questions (public question collections)

- [B] What is the difference between an environment variable and a shell variable? (`export`)
- [B] How do you split a comma-separated string into an array? (`IFS`, `read -ra`)
- [B] How do you redirect stdout and stderr to two different files in one command?
- [B] What does `tee` do? Show output on screen and save it to a file at the same time.
- [M] How do you stop two cron runs of the same script overlapping? (`flock -n`)
- [M] Why use `mktemp` instead of `/tmp/myfile`? (race conditions, symlink attacks)
- [M] How do you parse long options like `--env prod --version 1.2` in Bash? (`getopts` limits, manual `case` loop)
- [M] Validate user input to avoid command injection in a script that builds commands. (whitelists, quoting, never `eval`)
- [M] Heredoc with and without variable expansion. (`<<EOF` vs `<<'EOF'`)
- [M] Use `jq` to extract names of running pods from `kubectl get pods -o json`, and build a JSON payload safely with `--arg`.
- [M] Use `yq` to change `spec.replicas` in a manifest in place.
- [M] Run 100 `curl` checks in parallel with a limit of 10 at a time. (`xargs -P`, GNU parallel)
- [M] What do `2>&-`, `2>/dev/null`, `|&`, `&>/dev/null` and `>/dev/null 2>&1` each do? Why does order matter in `2>&1 >file`?
- [M] Why does a variable set inside `while read ... done < <(cmd)` survive but not inside `cmd | while read`? (subshells in pipelines)
- [M] What does `LC_ALL=C` in front of `sort` or `grep` change, and why is it faster?
- [M] How do you lint a shell script? (`shellcheck`) Give three bugs it catches.
- [S] Your Bash script is called from cron and works by hand but fails from cron. List the reasons. (PATH, env, working dir, shell, permissions, no TTY)
- [S] How do you write a script that is safe to run on 200 hosts at once? (idempotency, locking, timeouts, retries, logging, dry-run)

#### Hands-on tasks
1. Script with strict mode and a `trap` that cleans up a temp directory on EXIT and on Ctrl-C.
2. Script that waits for a TCP port to open with a timeout, using `/dev/tcp`, for use in a container entrypoint.
3. Cron-safe script with `flock` that rotates and gzips logs older than 1 day and deletes those older than 30.
4. Parse `kubectl get pods -A -o json` with `jq` and list pods that have restarted more than 5 times.
5. Script that compares installed package versions on two servers over SSH and prints differences.
6. Script that checks SSL certificate expiry for a list of domains and warns when under 14 days.

### Reported in real interviews

#### Indian companies, startups and community reports: Shell scripting

- [GfG, Amazon DevOps/Support 5y, 2020] [M] Extract specific error codes from a debug log (cat and sed), then turn it into a script.
- [GfG, Amazon, 2020] [M] Script that reports the top 10 space-consuming folders with sizes.
- [GfG, Amazon, 2020] [M] File processing with cat, sed, awk, head, tail, grep -B, sort and uniq.
- [Code360/Shiprocket, 2022] [B] Script to find large files in a directory.
- [Glassdoor/UKG] [B] Shell script to reverse a string.
- [Glassdoor/Delhivery] [M] Bash scripting questions inside a Kubernetes-focused round.
- [LeetCode/Robin.io, 2022] [M] Write awk one-liners for given text problems.
- [Walmart LinkedIn, 2023] [M] Shell scripting questions in the technical round.
- [Code360/PhonePe SRE campus, 2021] [M] Shell scripting in the face-to-face round.
- [Dataford/PhonePe SRE, 2026] [M] Script that monitors a service and restarts it if it dies.
- [Apurv Gujjar blog, 2025] [B] What is a shebang? Make a script executable and run it.
- [Apurv Gujjar blog] [B] Restart a stopped service from a script.
- [Apurv Gujjar blog] [B] Grep errors out of a log from a script.
- [Apurv Gujjar blog] [B] Schedule a script (cron).
- [meetassist, 2025] [M] Script that checks a process every 2 minutes and logs the status.
- [k8s-interview-prep repo] [M] Show an awk one-liner and explain when awk is the right tool.

#### Big tech (Google, Meta, Apple, Amazon, Microsoft and others): Shell scripting

- [Amazon DevOps] Extract error codes from a log with sed, then write a reusable script [C14]
- [Amazon DevOps] Top-10 disk-consuming directories script [C14]
- [Amazon DevOps] Text-processing pipelines with awk/sed/sort/uniq/grep -B [C14]
- [Meta] awk-based column extraction and filtering into an output file [C7 (agg)]
- [Meta] OA: grep, awk, sed, ps, top, netstat, lsof usage [C9 (agg)]
- [Meta] Explain output of ping, `ps aux`, vmstat during troubleshooting [C43]
- [Google] Build ps- or find-like tools [C11]

## 4. Docker

### Core questions

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

### Additional questions (public question collections)

- [B] What does `docker run` do step by step? (pull if missing, create container, set up namespaces/network/mounts, start process)
- [B] `docker exec` vs `docker attach`.
- [B] Restart policies: `no`, `on-failure`, `always`, `unless-stopped`.
- [B] What is the build context and what does `.dockerignore` change? Why is a 2 GB build context slow?
- [B] `docker save`/`load` vs `docker export`/`import`.
- [B] What is a dangling image? What does `docker system prune` remove?
- [M] Shell form vs exec form for CMD/ENTRYPOINT. Which one gets signals?
- [M] How do you limit a container's CPU and memory? What happens when it hits the memory limit?
- [M] What is a container image digest and why deploy by digest rather than tag?
- [M] How do multi-architecture images work? Build one for amd64 and arm64 with `buildx`.
- [M] How do you pass a secret during build without leaving it in a layer? (BuildKit `--mount=type=secret`, not ARG)
- [M] Container exits immediately with no logs. How do you debug? (override entrypoint, `docker run -it --entrypoint sh`, check exit code)
- [M] In Compose, how do you make one service wait for another to be healthy? (`depends_on` with `condition: service_healthy`)
- [M] Does killing the Docker daemon stop running containers? What is the containerd shim and `live-restore`?
- [M] What is a distroless image? Trade-offs vs alpine (musl issues, debugging without a shell).
- [M] What are rootless containers / user namespaces? What breaks when you use them?
- [M] Where are container logs stored and how do you stop them filling the disk? (logging driver, `max-size`, `max-file`)
- [S] Walk through the call chain: docker CLI -> dockerd -> containerd -> shim -> runc. What does each part do?
- [S] What is the OCI? What do the image spec, runtime spec and distribution spec standardise?
- [S] What are gVisor and Kata Containers? When would you need a sandboxed runtime instead of runc?
- [S] Your image has 300 CVEs from the base image. How do you prioritise and reduce them? (base image choice, rebuild cadence, VEX, reachability)
- [S] How do you prevent a single container from crashing or starving the host? (limits, pids-limit, ulimits, read-only rootfs, no privileged, seccomp/AppArmor)
- [S] Should databases run in containers in production? Argue both sides.

#### Hands-on tasks
1. Take a 1.2 GB Python image and get it under 150 MB; show `docker history` before and after.
2. Build and push a multi-arch image with `buildx` and inspect the manifest list.
3. Use a BuildKit secret mount to install from a private package index without the token appearing in `docker history`.
4. Run a container as non-root with a read-only root filesystem, a tmpfs for `/tmp`, dropped capabilities and a memory limit.
5. Write a Compose file for app + Postgres + Redis with healthchecks, named volumes and a `.env` file.
6. Generate an SBOM with Syft, scan it with Grype/Trivy, and sign the image with cosign (keyless).

### Reported in real interviews

#### Indian companies, startups and community reports: Docker

- [GfG, rtCamp] [B] What is a Docker volume? Image vs container?
- [GfG, Inorg Global] [B] Docker architecture and components. What a registry does. Virtualisation vs containerisation.
- [Apurv Gujjar blog, 2025] [B] VM vs Docker. Why Docker? Dockerfile plus build command.
- [Apurv Gujjar blog] [B] What is Docker Compose? Give an example.
- [dev.to interview post, 2025] [M] What is a multi-stage build? Can the second stage be used as the first?
- [Substack MNC, 2026] [M] What are the stages of building an image? ENTRYPOINT vs CMD.
- [Substack MNC] [M] Pass environment variables at build time vs run time.
- [Substack MNC] [M] Scan images for vulnerabilities. Which tools or plugins?
- [Glassdoor/Meesho, 2021] [M] Docker internals.
- [Medium/Cisco Senior SRE] [M] How do you optimise a Docker image?
- [Taro/Flipkart, 2020] [M] How do you ship logs out of a container?
- [Taro/Flipkart, 2020] [M] A container throws an error on mount: how do you mount or debug it?
- [GfG list] [M] Shell form vs exec form. ARG vs ENV. ADD vs COPY.
- [GfG list] [M] Build context and .dockerignore.
- [GfG list] [M] docker save/load vs export/import.
- [GfG list] [M] exec vs attach. How do you debug a container?
- [GfG list] [M] Volumes vs bind mounts vs tmpfs. When is container data lost?
- [GfG list] [M] Restart policies. Limit a container's CPU and memory.
- [GfG list] [S] How does Docker use namespaces and cgroups? How does the build cache work?
- [GfG list] [M] Ensure container 1 starts before container 2 in Compose.
- [GfG list] [S] On a 16 GB, 4-core host, what decides how many containers you can run?
- [GfG list] [M] Best practices for container security in production.

#### Big tech (Google, Meta, Apple, Amazon, Microsoft and others): Docker

- No candidate-reported Docker questions turned up for these companies. Containers show up inside Kubernetes design (Apple) and Linux internals (namespaces and cgroups are implied by Apple's Linux virtualisation focus [C47 (agg)]).

## 5. Python for DevOps

### Core questions

#### How much Python is enough
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

#### Questions
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

#### Hands-on tasks
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

### Additional questions (public question collections)

- [B] How do you read environment variables with a default, and fail clearly if a required one is missing?
- [B] What is the difference between `requirements.txt` with pinned versions and a lock file? Why pin in CI?
- [B] How do you exit a script with a non-zero status so CI/cron sees the failure?
- [M] How do you read a 20 GB log file and compute a checksum without loading it into memory? (chunks, generators)
- [M] How do you make `requests` calls safe in automation? (timeouts always, session reuse, retries via `HTTPAdapter`/`tenacity`, raise_for_status)
- [M] How do you respect an API's rate limit when calling it concurrently? (semaphore, token bucket, backoff on 429)
- [M] How do you test a boto3 script without touching a real AWS account? (`moto`, stubber) And HTTP calls? (`responses`)
- [M] How do you run a command on 50 servers concurrently from Python? (`ThreadPoolExecutor` + paramiko, asyncssh, or Fabric)
- [M] How do you trigger a rolling restart of a Deployment with the Kubernetes Python client? (patch pod template annotation)
- [M] How do you parse and validate config from env vars and YAML with types? (pydantic / pydantic-settings, dataclasses)
- [M] Mutable default arguments - what is the bug and how do you avoid it?
- [M] What is a context manager? Write one that times a block or acquires a lock file.
- [M] How do you structure a script so it works both as a CLI and as an importable module, and so it can be unit-tested?
- [S] When would you write a Kubernetes operator or controller in Python (kopf) vs Go? What does a reconcile loop need to be correct? (idempotency, requeue)
- [S] Your automation script deleted the wrong resources in production. What safety features should it have had? (dry-run default, explicit confirmation, tag filters, blast-radius limits, logging)
- [S] How would you build a Slack/ChatOps bot that runs deployments, and how do you secure it?

#### Hands-on tasks
1. boto3: find unattached EBS volumes older than 30 days across all regions, dry-run by default, `--apply` to delete.
2. boto3: find security groups with 0.0.0.0/0 on port 22 or 3389 across regions and print a report.
3. Kubernetes client: list pods in CrashLoopBackOff across all namespaces with their last termination reason.
4. Fetch a Prometheus `/metrics` endpoint and print the value of one metric with labels.
5. Script that rotates a secret in AWS Secrets Manager and restarts a Deployment that uses it.
6. Write `pytest` tests for task 1 using `moto`.

### Reported in real interviews

#### Indian companies, startups and community reports: Python

- [GfG, Inorg Global, ~2024] [B] Print the first 10 primes. Swap two numbers. Frog-jump problem.
- [GfG, HashedIn, 2022] [B] Python and SQL query writing in the technical round.
- [GfG, Amazon DevOps/Support, 2020] [M] Classify an array as increasing, decreasing or a mix of the two.
- [GfG, MakeMyTrip] [B] Which Python libraries did your project use, and what do specific functions do?
- [Code360/Shiprocket, 2022] [M] Python script to run commands on several servers over SSH.
- [Glassdoor UK, live coding] [M] Parse a log file in Python with strip/split and aggregate counts in a dict.
- [LinkedIn/Atlassian SRE] [M] Python: compute total and prorated amounts for free and premium subscribers.
- [Glassdoor/Persistent Systems] [M] Given a Python script, debug it.
- [Walmart LinkedIn, 2023] [M] Python automation alongside Kubernetes and AWS.
- [Apurv Gujjar blog, 2025] [B] Why Python for DevOps? What is pip?
- [Apurv Gujjar blog] [M] Automate AWS with Python (boto3).
- [Apurv Gujjar blog] [M] Scan logs and extract errors in Python.
- [Apurv Gujjar blog] [M] Bash or Python: which is better for DevOps tasks?
- [meetassist, 2025] [M] Parse and validate a command's JSON output in Python or Bash, handling non-zero exits, partial output and malformed JSON.
- [Medium @sre-devops-interview/IBM, 2025 snippet] [M] Python scripting for operational automation.

#### Big tech (Google, Meta, Apple, Amazon, Microsoft and others): Python

- [Meta] Goat Latin (LC 824), 2025 E4 screen [C5]
- [Meta] Minimum Remove to Make Valid Parentheses (LC 1249), Valid Word Abbreviation (LC 408), Binary Tree Vertical Order Traversal (LC 314). These are listed as Meta PE question-bank items; the specific candidate did not say they got them [C3].
- [Meta] Validate abbreviations and brackets (SRE, Apr 2026) [C23]
- [Meta] Battleship board variant (hard; practise DFS/BFS) [C21]
- [Meta] Palindrome check; count 2D-grid cells matching a condition; binary search on a sorted array; DFS/BFS/stack/queue problems [C7 (agg)]
- [Meta] Detect a cycle in a directed graph [C9 (agg)]
- [Meta] Screen: one LC easy and one LC medium [C44]
- [Google] Room booking: book [start, end) intervals, returning True or False [C2 (agg)]
- [Google] Average of the last n stream values after dropping the top few [C2 (agg)]
- [Google] Add two numbers given as strings [C2 (agg)]
- [Google SWE-SRE] Trapping Rain Water (LC 42), from brute force to optimal [C45 (agg)]
- [Google] Two-part questions: an easy core, then a harder follow-up. BFS/DFS, counting islands-style objects in a matrix, maze solving [C46]
- [Amazon SysDE] Binary tree boundary traversal; add 1 to a number stored as a linked list [C32]
- [Amazon SysDE] Best Time to Buy and Sell Stock II (LC 122) plus variants; search in a rotated sorted array (LC 33) [C32]
- [Amazon SysDE] Sort letters by frequency groups, with singles kept in original order; zero-sum subarray [C32]
- [Amazon SysDE] Balanced parentheses in O(1) space; Two Sum (LC 1) [C32]
- [Microsoft] Find where two singly linked lists merge (LC 160 style) (2026) [C23]
- [Microsoft] OOP concepts plus easy HackerRank problems (senior SRE screen) [C20]
- [Uber] LRU cache (LC 146) (older SRE report) [C26]
- [Atlassian] LC medium where testing your own solution is graded [C27]
- [LinkedIn] FizzBuzz variant [C28]

## 6. Kubernetes

### Core questions

#### Architecture and basics
- [B] What is Kubernetes and what problems does it solve?
- [B] Control plane components: kube-apiserver, etcd, kube-scheduler, kube-controller-manager, cloud-controller-manager. What does each do?
- [B] Worker node components: kubelet, kube-proxy, container runtime.
- [B] Pod vs Deployment vs ReplicaSet.
- [B] What is a namespace? What is not namespaced?
- [B] Service types: ClusterIP, NodePort, LoadBalancer, ExternalName. What is a headless Service?
- [B] ConfigMap vs Secret. Are Secrets encrypted by default?
- [B] Basic `kubectl`: get, describe, logs (with `--previous`), exec, apply, delete, rollout status/undo, port-forward.
- [B] What are labels and selectors?

#### Workloads and configuration
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

#### Networking
- [M] How does a request flow from a domain name to a pod? (DNS -> load balancer -> Ingress controller -> Service -> kube-proxy/iptables or IPVS -> pod)
- [M] What does kube-proxy actually do?
- [M] What is a CNI plugin? Name a few (Calico, Cilium, AWS VPC CNI).
- [M] Ingress vs Ingress controller vs LoadBalancer Service. What is the Gateway API?
- [M] How does service discovery work? What is CoreDNS? Format of a Service DNS name.
- [M] What is a NetworkPolicy? Allow traffic to a database pod only from the backend pods.
- [S] Pods on different nodes cannot talk to each other. How do you debug?

#### Troubleshooting scenarios (most important part)
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

#### Design and security
- [S] How would you do blue-green and canary deployments on Kubernetes? (Argo Rollouts, service switching, Ingress weights)
- [S] What is GitOps? How does Argo CD work? Push vs pull deployment.
- [S] How do you manage secrets properly? (External Secrets Operator + AWS Secrets Manager, Sealed Secrets, SOPS, Vault)
- [S] How do you upgrade a Kubernetes cluster with zero downtime? (EKS: control plane, then node groups, PDBs, version skew)
- [S] Pod security: Pod Security Standards, `runAsNonRoot`, read-only root filesystem, dropping capabilities.
- [S] How do you run a multi-tenant cluster for several teams? (namespaces, RBAC, quotas, LimitRange, NetworkPolicy)
- [S] How do you back up a cluster? (etcd snapshot, Velero)
- [S] How would you reduce Kubernetes cost by 30%? (right-sizing requests, spot nodes, Karpenter, autoscaling, idle namespaces)

### Additional questions (public question collections)

#### Architecture and internals
- [B] What happens when you run `kubectl apply -f deployment.yaml`? Trace it from kubectl to a running container. (API server authn/authz/admission -> etcd -> Deployment controller -> ReplicaSet -> scheduler -> kubelet -> CRI -> CNI)
- [B] What happens when you delete a pod? (graceful termination, preStop, SIGTERM, endpoints removal, SIGKILL after grace period)
- [B] What are static pods? Where do their manifests live and how do you delete one?
- [B] Labels vs annotations.
- [M] What happens to running pods if the kubelet on a node stops? If the whole control plane goes down?
- [M] What is the controller/reconcile loop? Name three controllers and what each watches.
- [M] What are admission controllers? Mutating vs validating webhooks. How can a broken webhook take down deployments?
- [M] What is a CRD and an operator? Give an example of an operator you have used.
- [M] What are finalizers? A namespace or PVC is stuck in `Terminating` - why and how do you fix it safely?
- [M] What is Kustomize? Kustomize vs Helm - when to use each, or both together?
- [M] What are native sidecar containers (init container with `restartPolicy: Always`, GA in 1.33)? What problems did they fix for Jobs and startup/shutdown order?
- [S] How does etcd work? Why does it need an odd number of members and fast disks? What happens when etcd disk latency is high?
- [S] What is API Priority and Fairness? The API server returns HTTP 429 - what do you check?
- [S] Explain the version skew policy between control plane, kubelet and kubectl.
- [S] How do you rotate cluster certificates on a kubeadm cluster before they expire?

#### Workloads, scheduling and scaling
- [B] What are the pod phases, and how do they differ from the `STATUS` column in `kubectl get pods`?
- [M] A CronJob created hundreds of failed Jobs and filled the cluster. Which fields stop this? (`concurrencyPolicy`, history limits, `backoffLimit`, `activeDeadlineSeconds`, `startingDeadlineSeconds`)
- [M] Pod topology spread constraints vs pod anti-affinity. Spread replicas across AZs.
- [M] What are PriorityClasses and preemption? How can a low-priority pause pod be used for overprovisioning?
- [M] What are ResourceQuota and LimitRange? What happens to a pod with no requests in a namespace that has a quota?
- [M] What does CPU throttling look like when a container hits its CPU limit? Why do some teams remove CPU limits but keep memory limits?
- [M] What is KEDA and how does it scale on queue length, including to zero?
- [M] What is in-place pod resize? What does it change compared with recreating the pod?
- [M] How does a StatefulSet give stable identity? What happens to its PVCs when you scale down or delete it?
- [S] Karpenter: how does it choose instances, and what are consolidation, disruption budgets and node expiry? How do you stop it from disrupting a critical pod? (`do-not-disrupt`)
- [S] Mixed Spot and On-Demand nodes: how do you keep critical workloads on On-Demand and handle Spot interruptions gracefully?
- [S] You need to evict all pods from 50 nodes for an AMI upgrade with no user impact. Plan it.

#### Networking
- [M] What is the `ndots:5` problem and how does it add latency to external DNS lookups? How do you fix it?
- [M] `externalTrafficPolicy: Cluster` vs `Local`. What happens to client source IP and to load balancer health checks?
- [M] Gateway API: what are GatewayClass, Gateway and HTTPRoute, and which team owns each? Why is it replacing Ingress?
- [M] Migrate an existing Ingress to Gateway API HTTPRoutes without downtime. (Ingress NGINX retirement, run both, shift DNS)
- [M] Are pods in different namespaces able to talk by default? Write a default-deny policy for a namespace and then allow DNS.
- [M] How does a container find its Service the first time? (CoreDNS vs environment variables)
- [S] CoreDNS pods keep getting OOMKilled and the whole cluster has DNS failures. Immediate fix and long-term fix. (scale, NodeLocal DNSCache, ndots)
- [S] What is a service mesh? Sidecar mesh vs ambient/sidecarless (Istio ambient, Cilium). When is a mesh not worth it?
- [S] What does Cilium do with eBPF? How can it replace kube-proxy?
- [S] On EKS, pods fail to start with "failed to assign an IP address". Why, and what are your options? (VPC CNI IP exhaustion, prefix delegation, custom networking with 100.64/10, IPv6)

#### Storage
- [M] Volume reclaim policies: Retain vs Delete. You deleted a PVC by mistake - can you get the data back?
- [M] A pod with an EBS-backed PVC is stuck in `ContainerCreating` after rescheduling to another AZ. Why? (volume AZ affinity, `WaitForFirstConsumer`)
- [M] emptyDir vs hostPath - use cases, and why hostPath is a security risk.

#### Troubleshooting scenarios
- [M] Pod stuck in `ContainerCreating`. Causes and checks. (volume attach/mount, CNI IP, secret/configmap missing, image pull)
- [M] Pod stuck in `Terminating` for hours. Why, and how do you clear it safely? (finalizers, node gone, `--force --grace-period=0` risks)
- [M] Exit code 137 vs 143 in pod status. Exit 137 without OOMKilled - what else causes it? (liveness failure, eviction)
- [M] Traffic works between pods on the same node but fails across nodes. (CNI, overlay/MTU, security groups, routes)
- [M] Nodes are going `NotReady` with disk pressure from container images and logs. Fix and prevent. (image GC thresholds, log rotation, larger root volume)
- [M] Liveness probe is too aggressive and kills pods during slow startup or GC pauses. How do you tune it?
- [M] A Job never finishes because its sidecar keeps running. Fix it. (native sidecar)
- [S] Someone deleted a production namespace. Recovery steps and prevention. (Velero/GitOps restore, RBAC, admission policy, deletion protection)
- [S] Error rate rises during every rolling deployment even with readiness probes. Why? (no preStop delay, endpoint propagation lag, connection draining, keep-alive)
- [S] Worker nodes flap between Ready and NotReady. Investigation. (kubelet/PLEG, runtime, node resources, network to API server)
- [S] HPA is not scaling even though load is high. All the reasons. (no requests set, metrics-server down, wrong metric, maxReplicas, cooldown, quota)

#### Security and policy
- [M] What is Kyverno or OPA Gatekeeper? Write a policy that blocks images with the `latest` tag or from untrusted registries.
- [M] What is a SecurityContext? `allowPrivilegeEscalation`, `privileged`, `capabilities`, `seccompProfile`.
- [M] Why should the default ServiceAccount token not be auto-mounted? How do you disable it?
- [S] How do you verify image signatures at admission time? (cosign + Kyverno/policy-controller)
- [S] What is runtime security? What does Falco detect?
- [S] How do you keep secrets out of etcd entirely? (Secrets Store CSI driver) And how do you encrypt Secrets at rest in etcd? (KMS provider)
- [S] Soft vs hard multi-tenancy: namespaces vs vcluster vs separate clusters. When is each justified?

#### Hands-on tasks (CKA style)
1. Create a Gateway and an HTTPRoute that sends 90% of traffic to v1 and 10% to v2.
2. Add a native sidecar log shipper to an existing Deployment.
3. Back up etcd with `etcdctl snapshot save` and restore it to a new data directory.
4. Upgrade a kubeadm cluster one minor version (control plane then workers, with drain/uncordon).
5. Write a NetworkPolicy set: default deny, allow DNS, allow frontend -> backend on 8080 only.
6. Create a ServiceAccount, Role and RoleBinding; prove access with `kubectl auth can-i --as`.
7. Install a chart with Helm using a values file, upgrade it, view history, roll back.
8. Taint a node, schedule a pod that tolerates it, then remove the taint.
9. Fix a broken kubelet on a worker node (wrong config path / stopped service) from `journalctl`.
10. Add topology spread constraints so replicas spread across 3 zones.
11. Create a PV/PVC pair by hand with a StorageClass and mount it into a pod.
12. Find the pod using the most memory across the cluster and sort with `kubectl top`.

### Reported in real interviews

#### Indian companies, startups and community reports: Kubernetes

- [GfG, Inorg Global] [B] What is a node?
- [Apurv Gujjar blog, 2025] [B] Why Kubernetes if we have Docker? Pods, Deployments, Services.
- [dev.to interview post, 2025] [B] Service types. What is a headless service?
- [Substack MNC, 2026] [M] Roles of the control plane vs worker nodes.
- [Substack MNC] [M] Which errors have you hit (CrashLoopBackOff, ImagePullBackOff)?
- [Substack MNC] [B] How do you get into a running pod?
- [Substack MNC] [M] How do you authenticate to EKS and manage secrets?
- [Substack MNC] [M] Create and manage clusters with Terraform.
- [LeetCode/NetApp SRE, Dec 2024] [M] StatefulSet vs Deployment, namespaces, taints and tolerations.
- [LeetCode/NetApp] [M] Helm and ingress controllers.
- [LeetCode/NetApp] [M] Spread a deployment's replicas across different nodes.
- [LeetCode/Robin.io, 2022] [M] Kubernetes architecture and pod networking.
- [Walmart LinkedIn, 2023] [S] Persistent volume lifecycle. Back up etcd. Where are the control-plane certificates?
- [Medium/Accenture DevOps] [S] Prerequisites for a cluster upgrade.
- [Medium/IBM, 2025] [M] Kubernetes architecture and the role of each component.
- [HireStepX India guide, 2026] [M] What happens after `kubectl apply`? Networking with Services, Ingress and CNI.
- [Atlassian SRE mock, LinkedIn 2025] [S] OOM storms and HPA flapping: right-size requests/limits, use VPA, QoS.
- [Atlassian SRE mock] [S] p95 latency jumps after enabling mesh/mTLS: isolate the cause.
- [k8s-interview-prep repo] [S] Scheduler placement. Owner references and finalizers.
- [k8s-interview-prep repo] [M] ResourceQuota and LimitRange.
- [k8s-interview-prep repo] [M] Probes (liveness, readiness, startup). Requests vs limits and QoS.
- [k8s-interview-prep repo] [M] Pod deletion lifecycle (preStop, SIGTERM, grace period).
- [k8s-interview-prep repo] [M] PV/PVC/StorageClass, access modes, reclaim policy.
- [k8s-interview-prep repo] [M] HPA, VPA and Cluster Autoscaler. PodDisruptionBudgets.
- [k8s-interview-prep repo] [S] RBAC with no deny rules. What replaced PSP. Admission webhooks.
- [k8s-interview-prep repo] [S] externalTrafficPolicy Cluster vs Local. kube-proxy iptables vs IPVS.
- [GfG list] [B] Static pods. Multi-container patterns (sidecar, ambassador, adapter).
- [GfG list] [M] DaemonSet vs ReplicaSet. Jobs and CronJobs.
- [GfG list] [M] nodeSelector vs node affinity vs taints. Pod anti-affinity.
- [GfG list] [M] ConfigMap vs Secret. Init containers.
- [GfG list] [M] Ingress vs LoadBalancer. NetworkPolicy. CoreDNS.
- [GfG list] [S] CRDs, operators. Upgrade a cluster. Back up and restore a cluster.
- [GfG list] [M] Cordon, drain and uncordon. apply vs create vs replace.
- [GfG list] [M] Isolate resources for multiple teams on one cluster.
- [GfG list] [M] Schedule latency-sensitive services next to batch jobs.
- [LivingDevOps guide] [M] Pin a pod to a specific node. Run a one-time DB migration before app start.
- [LivingDevOps guide] [M] Read-only RBAC so users can view logs.
- [LivingDevOps guide] [M] Allow only labelled pods to reach a backend.

#### Big tech (Google, Meta, Apple, Amazon, Microsoft and others): Kubernetes

- [Apple] What components make up the Kubernetes control plane? [C19]
- [Apple] How do you troubleshoot a pod stuck in Pending? (reported in Apple SRE search results; sourcing is thin) [C19]
- [Apple] [S] Kubernetes default node counts / cluster defaults (SRE Manager) [C19]
- [Apple] System design round focused on Kubernetes [C18, C19]

## 7. GitHub Actions

### Core questions

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

### Additional questions (public question collections)

- [B] Do jobs run in parallel or in sequence by default? How do you change it?
- [B] `workflow_dispatch` vs `repository_dispatch` vs `workflow_call`.
- [B] What does `continue-on-error` do, and how is it different from `if: always()`?
- [M] What is script injection in workflows? Why is `run: echo "${{ github.event.pull_request.title }}"` dangerous, and how do you fix it? (pass via `env`)
- [M] How do you pass secrets into a reusable workflow? `secrets: inherit` vs explicit secrets.
- [M] How do you set step outputs and environment variables now that `set-output` is removed? (`$GITHUB_OUTPUT`, `$GITHUB_ENV`)
- [M] Why do secrets not work in workflows triggered from forks, and how should you handle CI for external contributors?
- [M] What is `workflow_run` and what are its security risks?
- [M] How does the OIDC trust policy restrict which repo, branch or environment can assume an AWS role? What claim mistakes make it insecure? (wildcard `sub`)
- [M] How do you use `hashFiles()` and `restore-keys` well? Why can a cache be poisoned across branches?
- [M] What are path filters and `paths-ignore`? How do they interact with required status checks?
- [M] How do you protect workflow files themselves from being changed in a PR? (CODEOWNERS on `.github/workflows`, rulesets)
- [M] What are artifact attestations / build provenance? How do you generate and verify one? (`actions/attest-build-provenance`, `gh attestation verify`)
- [M] Deploy to Kubernetes/EKS from Actions without storing a kubeconfig. (OIDC -> IAM role -> EKS access entries)
- [S] The tj-actions/changed-files compromise leaked secrets from logs. Walk through what an attacker did and the full list of controls that would have limited it. (SHA pinning, minimal token permissions, harden-runner egress, no secrets in public repo logs)
- [S] What is StepSecurity harden-runner or egress filtering, and why would you restrict outbound network from runners?
- [S] Self-hosted runners on public repos: why is this dangerous? Ephemeral / just-in-time runners vs persistent runners.
- [S] What is SLSA? What level does a typical GitHub Actions build reach and what is needed for level 3?
- [S] How would you manage hundreds of repos' workflows centrally? (reusable workflows in a central repo, required workflows/rulesets, versioning them)
- [S] Merge queue: what problem does it solve and how do you configure CI for `merge_group` events?
- [S] Deployment fails halfway in a matrix of 10 regions. How do you design the workflow for safe partial rollout and rollback?

#### Hands-on tasks
1. Workflow with OIDC to AWS: trust policy restricted to `repo:org/repo:environment:prod`.
2. Fix a workflow that has script injection, `permissions: write-all` and unpinned actions; add Dependabot for actions.
3. Reusable workflow for build-scan-push with inputs, secrets and outputs, called from two repos.
4. Build an image, generate SBOM, sign with cosign keyless, attach provenance attestation.
5. Monorepo workflow with path filters that builds only changed services and still satisfies required checks.

### Reported in real interviews

#### Indian companies, startups and community reports: GitHub Actions

- [Livspace via placementpreparation, ~2025] [M] Build a full CI/CD pipeline using GitHub Actions, Terraform and Kubernetes (technical challenge round).
- [Apurv Gujjar blog, 2025] [B] Jenkins vs GitHub Actions.
- [Guide: interview-prep sites] [M] GitHub-hosted vs self-hosted runners. Why not self-hosted runners on public repos?
- [Guide] [M] Reusable workflows: passing inputs and secrets (`secrets: inherit`) across many microservices.
- [Guide] [M] Environment-scoped secrets for staging vs prod.
- [Guide] [M] Run a job on specific hardware with runner labels.
- [Guide] [S] Scale runners with ARC on Kubernetes and ephemeral runners.

## 8. Jenkins

### Core questions

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

### Additional questions (public question collections)

- [B] Explain job, build, executor, node/agent and workspace.
- [B] How do you temporarily disable a scheduled job? How do you restart a pipeline from a specific stage?
- [M] What are credential scopes (global vs system vs folder)? How do you stop credentials appearing in console logs?
- [M] What is Jenkins Configuration as Code (JCasC)? Why manage Jenkins itself as code?
- [M] What is in-process script approval and the Groovy sandbox? Why do shared libraries sometimes need approval?
- [M] How do you version and test a shared library? (tags/branches with `@Library('lib@v1')`, unit tests with JenkinsPipelineUnit)
- [M] How do you share data between stages that run on different agents? (`stash`/`unstash`, artifacts)
- [M] `parallel` with `failFast` - what happens when one branch fails?
- [M] How does the Kubernetes plugin create pod agents per build? What goes wrong with them? (pod template, image pull time, workspace volume)
- [M] Jenkins controller runs out of disk ("no space left on device"). Cleanup and prevention. (build discarders, workspace cleanup, artifact storage outside JENKINS_HOME)
- [M] An agent shows offline. How do you troubleshoot? (JNLP/inbound connection, Java version, disk, network)
- [M] Pipeline fails only on the `main` branch. How do you investigate? (branch-specific `when`, credentials scoped to folders, different agents)
- [S] Migrate 50 freestyle jobs to declarative pipelines. Plan and order.
- [S] Hundreds of jobs created and deleted every week. How do you manage them? (Job DSL, multibranch / organization folders, JCasC)
- [S] Team X's builds must run before Team Y's. How do you prioritise? (labels, dedicated agents, priority sorter)
- [S] How do you make Jenkins highly available or quickly recoverable? (JENKINS_HOME on persistent volume, JCasC, backups, controller is a single point of failure)
- [S] Agents connect to the controller - how do you stop a compromised agent from attacking the controller? (agent-to-controller security, no builds on controller, isolated agents)
- [S] How do you monitor Jenkins? (queue length, executor usage, build duration, Prometheus plugin, OpenTelemetry plugin)

#### Hands-on tasks
1. Declarative Jenkinsfile with parallel test stages, `stash`, `input` approval with timeout, and `post` cleanup.
2. Minimal shared library with `vars/buildImage.groovy`, loaded by version tag.
3. JCasC YAML that configures security realm, an agent cloud on Kubernetes and a credential from an env var.
4. Groovy script in the script console that deletes old builds for all jobs (and explain why that console is dangerous).

### Reported in real interviews

#### Indian companies, startups and community reports: Jenkins

- [Taro/Flipkart, 2023] [M] Jenkins questions in a Linux, Kubernetes and Jenkins interview.
- [Taro/Adobe, 2021] [B] Jenkins basics.
- [Substack MNC, 2026] [M] Describe your end-to-end CI/CD. Which Jenkins pipeline type do you use?
- [Substack MNC] [M] Declarative vs scripted pipelines. How do you define and trigger them?
- [Substack MNC] [M] What are shared libraries, and how do you design and reuse them?
- [Substack MNC] [M] Which stages enforce code quality?
- [Substack MNC] [B] What is a webhook in CI/CD?
- [Apurv Gujjar blog, 2025] [B] How does Jenkins help CI/CD? How does it differ from GitHub Actions?
- [GfG list] [B] Default port, home directory, initial password path.
- [GfG list] [B] Poll SCM vs webhook. Cron schedule syntax.
- [GfG list] [M] Credentials handling. Agents and master-agent setup.
- [GfG list] [M] Multibranch pipelines. stash/unstash. when, parallel and input steps.
- [GfG list] [S] JCasC, Groovy sandbox, HA, backup and restore, RBAC.
- [GfG list] [M] Blue-green, canary and rolling deploys from Jenkins.

#### Big tech (Google, Meta, Apple, Amazon, Microsoft and others): CI/CD

- [Apple] Design a CI/CD system [C19]
- [Apple] Design a highly available Jenkins pipeline [C19]
- [Google/Microsoft/Apple] Design CI/CD for a large monorepo [C37 (agg)]
- [Meta/Apple] Design automated release management (CI and CD) [C37 (agg)]
- [Google/Meta] Roll out new software to hundreds of thousands of servers with minimal downtime [C37 (agg)]
- [Meta] Automate deploying a new service to thousands of servers [C48 (agg)]

## 9. AWS

### Core questions

#### Core / IAM
- [B] Region vs Availability Zone vs edge location.
- [B] What is IAM? Users, groups, roles, policies.
- [B] IAM role vs IAM user. Why use roles for EC2 and pods?
- [M] Identity-based policy vs resource-based policy. How is an explicit deny evaluated?
- [M] How do you give an EC2 instance access to S3 without keys? (instance profile)
- [M] How do you give access across AWS accounts? (assume role, trust policy)
- [M] What is least privilege and how do you find unused permissions? (IAM Access Analyzer)
- [S] How do pods in EKS get AWS permissions? (IRSA vs EKS Pod Identity)
- [S] Organizations and SCPs - what are they for?

#### Networking (VPC is asked in almost every AWS loop)
- [B] What is a VPC? Public vs private subnet - what actually makes a subnet public?
- [B] Internet Gateway vs NAT Gateway.
- [B] Security group vs Network ACL (stateful vs stateless).
- [M] Design a VPC for a 3-tier app across 2 AZs. Subnets, route tables, NAT, where each tier lives.
- [M] Private EC2 cannot reach the internet. Debug path.
- [M] VPC peering vs Transit Gateway vs PrivateLink.
- [M] What are VPC endpoints? Gateway vs interface endpoints. Why use them for S3?
- [M] Route 53: A vs CNAME vs Alias. Routing policies (simple, weighted, latency, failover, geolocation). (you used Route 53 - expect follow-ups)
- [S] Design multi-region active-passive with automatic failover.

#### Compute and scaling
- [B] EC2 instance types and purchase options: on-demand, reserved, savings plans, spot.
- [B] What is an AMI? EBS vs instance store.
- [M] Auto Scaling Group: launch template, desired/min/max, scaling policies (target tracking, step, scheduled), health checks. (you used this - be ready)
- [M] ALB vs NLB vs Classic LB. Target groups and health checks.
- [M] ALB returns 502/503/504 - what does each usually mean?
- [M] What is IMDSv2 and why enforce it?
- [M] How do you do a zero-downtime deployment on EC2? (rolling ASG instance refresh, blue-green with target groups)
- [S] ECS vs EKS vs Lambda - when to use which.
- [S] EKS: managed node groups vs Fargate vs Karpenter.

#### Storage and databases
- [B] S3 storage classes and lifecycle rules.
- [M] S3 versioning, replication, Object Lock. How do you migrate data between buckets? (you did this: `aws s3 sync`, replication, Batch Operations, permissions on both sides)
- [M] Give temporary access to one S3 object. (pre-signed URL) Bucket policy vs IAM policy vs pre-signed URL.
- [M] Give a user in another account access to one S3 file.
- [M] How do you block public access and enforce encryption on S3?
- [M] RDS Multi-AZ vs read replicas. How does failover work?
- [M] How do you upgrade an RDS engine version with minimal downtime? (snapshot, test on a restored copy, maintenance window, blue/green deployments) (you did this - expect detail)
- [M] RDS backups: automated backups vs snapshots, point-in-time recovery.
- [S] Database CPU spikes while traffic is flat. How do you investigate?

#### Security, monitoring and cost
- [M] Where do you store secrets? Secrets Manager vs Parameter Store.
- [M] KMS: what is envelope encryption?
- [M] CloudWatch metrics, logs, alarms. CloudTrail vs CloudWatch vs AWS Config.
- [M] ACM certificates: DNS vs email validation, renewal, attaching to ALB/CloudFront. (you managed SSL - expect follow-ups)
- [M] What is CloudFront and why put it in front of S3 or an ALB?
- [S] Your AWS bill doubled this month. How do you find out why and fix it? (Cost Explorer, tags, NAT data transfer, idle resources, rightsizing)
- [S] An access key leaked on GitHub. Incident steps.
- [S] Design highly available, secure infrastructure for a web app with a database, and explain each choice.

### Additional questions (public question collections)

#### IAM and accounts
- [B] What is MFA and why enforce it on root and IAM users? What should the root account be used for?
- [M] Walk through IAM policy evaluation across SCP, permission boundary, identity policy, resource policy and session policy.
- [M] What is a permission boundary and when do you use one? (delegating role creation safely)
- [M] A user gets AccessDenied on an S3 bucket. List every place the deny can come from. (identity policy, bucket policy, SCP, KMS key policy, Block Public Access, VPC endpoint policy, object ownership)
- [M] What is IAM Identity Center (SSO) and why is it preferred over IAM users for humans?
- [M] What are IAM condition keys like `aws:SourceIp`, `aws:PrincipalOrgID`, `aws:SourceVpce`? Give a use for each.
- [S] What is the confused deputy problem and how does `ExternalId` / `aws:SourceArn` prevent it?
- [S] Design a multi-account setup for a startup growing to 50 engineers. (Organizations, OUs, Control Tower, separate prod/non-prod/security/log accounts)
- [S] A role was compromised through leaked credentials. Contain it without breaking everything. (revoke active sessions, deny by `aws:TokenIssueTime`, CloudTrail investigation)
- [S] How do you detect and auto-remediate a security group opened to 0.0.0.0/0 on port 22 within a minute? (Config rule or EventBridge + Lambda, GuardDuty)

#### Networking
- [B] How many usable IPs are in an AWS /24 subnet? (AWS reserves 5)
- [M] Private-subnet instances cannot download packages from GitHub. Debug path. (route to NAT, NAT in public subnet, SG/NACL egress, DNS, NACL ephemeral ports)
- [M] NACL allows inbound 443 but responses fail. Why? (stateless, ephemeral ports outbound)
- [M] What are VPC Flow Logs and what can and can't they tell you?
- [M] How do you connect on-premises to AWS? Site-to-Site VPN vs Direct Connect, and how do you make it fail over?
- [M] Overlapping CIDRs between two VPCs that need to talk. What are your options? (PrivateLink, NAT, re-IP)
- [M] Your NAT Gateway bill is huge. Why, and how do you reduce it? (cross-AZ traffic, S3/ECR via gateway/interface endpoints, chatty pulls)
- [S] Route 53 Resolver: how do you resolve on-prem DNS names from a VPC and vice versa? (inbound/outbound endpoints, forwarding rules)
- [S] How do you protect a public app from DDoS and bots on AWS? (Shield, WAF rate rules, CloudFront, ALB)

#### Compute, containers and serverless
- [B] Which purchase option fits each case: short non-interruptible job, steady 3-year workload, interruptible batch, licensing that needs a dedicated host?
- [M] An instance's public IP changed after stop/start. Why, and how do you keep a fixed IP? (Elastic IP, or better: a load balancer/DNS)
- [M] Placement groups: cluster vs spread vs partition. When to use each.
- [M] How do you get a shell on an instance with no SSH key and no open port 22? (SSM Session Manager)
- [M] Lambda cold starts: what causes them and how do you reduce them? (provisioned concurrency, smaller packages, SnapStart)
- [M] Lambda in a VPC cannot reach the internet or an AWS API. Why?
- [M] SQS standard vs FIFO. What is a dead-letter queue and visibility timeout? A message is processed twice - why?
- [M] SNS vs SQS vs EventBridge - when to use each.
- [M] ECS: task role vs task execution role. Fargate vs EC2 launch type.
- [M] EKS access: aws-auth ConfigMap vs EKS access entries. How do you give a new team access to one namespace?
- [S] EKS Pod Identity vs IRSA: how each works, and why Pod Identity is easier across many clusters and accounts.
- [S] EKS Auto Mode vs managed node groups + Karpenter. What do you give up?
- [S] How do you upgrade EKS across two minor versions, including add-ons (VPC CNI, CoreDNS, kube-proxy) and deprecated APIs? Can you roll back?
- [S] How do you handle Spot interruptions on EKS and ASGs? (2-minute notice, node termination handling, capacity-optimized allocation, diversification)

#### Storage and databases
- [M] An unencrypted RDS instance must be encrypted. How? (snapshot -> encrypted copy -> restore, plan downtime)
- [M] EBS volume types gp3 vs io2 vs st1. How do you increase IOPS or size without downtime?
- [M] EFS vs EBS vs S3 - when to use each, including from Kubernetes.
- [M] RDS `max_connections` reached during a traffic spike. Immediate fix and long-term fix. (RDS Proxy, pooling, instance size)
- [M] DynamoDB: partition keys, hot partitions, on-demand vs provisioned. Causes of high latency or throttling.
- [M] ElastiCache Redis vs Memcached. How does caching make an app stateless?
- [S] Design PostgreSQL DR with RPO under 1 minute and RTO under 5 minutes. (Multi-AZ, cross-region replica / Aurora Global Database, tested runbook)
- [S] Migrate an on-prem database to Aurora with minimal downtime. (DMS with CDC, cutover plan, rollback)
- [S] S3 is being read from an unexpected IP even though Block Public Access is on. How do you investigate? (CloudTrail data events, presigned URLs, access points, cross-account policies)

#### Reliability, cost and operations (Well-Architected)
- [B] What are the six pillars of the Well-Architected Framework?
- [M] DR strategies: backup-and-restore, pilot light, warm standby, multi-site active-active. RTO/RPO and cost of each.
- [M] What are AWS Budgets, Cost Anomaly Detection and Compute Optimizer for?
- [M] Savings Plans vs Reserved Instances. Compute Savings Plan vs EC2 Instance Savings Plan.
- [S] A $50k spike in one month came from CloudWatch Logs and S3 API calls. How do you find the source and fix it? (Cost Explorer by usage type, debug logging left on, retention, request patterns)
- [S] Automatically find and retire unused resources across 100 accounts. (tagging policy, Config aggregator, Lambda/Step Functions, owner notification first)
- [S] An Availability Zone has an outage. What in your design keeps you up, and what usually breaks anyway? (single-AZ NAT, single-AZ RDS, uneven ASG capacity, static stability)
- [S] How would you run a GameDay or chaos test on AWS? (AWS FIS, hypothesis, blast radius)
- [S] An S3 bucket became public after a `terraform apply`. Incident steps and prevention. (BPA at account level, SCP, policy checks in CI)

#### Hands-on tasks
1. VPC with public/private subnets in 2 AZs, NAT per AZ, S3 gateway endpoint; prove private instances reach S3 without NAT.
2. IAM role for GitHub Actions OIDC with a trust policy limited to one repo and environment.
3. Enable SSM Session Manager on a private instance and connect with no SSH key.
4. EKS: install Karpenter with a NodePool for Spot + On-Demand, and set up EKS Pod Identity for one workload to read an S3 bucket.
5. Create a Cost Anomaly Detection monitor and a Budget with an alert; tag resources and filter Cost Explorer by tag.
6. Encrypt an existing unencrypted RDS instance via snapshot copy and restore.

### Reported in real interviews

#### Indian companies, startups and community reports: AWS

- [GfG, HashedIn, 2022] [B] What did you use EC2 and S3 for, and how did you set them up and deploy?
- [GfG, HashedIn, 2022] [B] Cloud types. What is multi-tenancy?
- [GfG, Inorg Global] [M] IAM roles vs permissions vs policies. All storage services.
- [GfG, Inorg Global] [M] S3 vs RDS. What is Elastic Transcoder for?
- [GfG, Paytm, 2024] [B] Basics of AWS services.
- [GfG, Jivox, 2020] [B] AWS vs Azure.
- [Apurv Gujjar blog, 2025] [B] IaaS/PaaS/SaaS mapped to AWS services. Regions vs AZs.
- [Apurv Gujjar blog] [B] Host static vs dynamic sites. Which Route 53 records?
- [Apurv Gujjar blog] [M] Inbound and outbound rules. Launch multiple EC2s at once.
- [Apurv Gujjar blog] [M] Do EC2s in the same or different subnets need an IGW to talk?
- [Apurv Gujjar blog] [M] Cross-VPC traffic: peering or Transit Gateway vs IGW or NAT.
- [Apurv Gujjar blog] [M] Is EC2 public by default? Keep a public IP across reboot.
- [Apurv Gujjar blog] [M] How did you build a three-tier architecture? How do you handle millions of users?
- [Apurv Gujjar blog] [B] Which AWS service is serverless?
- [dev.to interview post, 2025] [M] What is VPC peering and how does it work?
- [Medium/Accenture DevOps] [M] NAT Gateway questions.
- [HireStepX India guide, 2026] [M] VPC peering vs PrivateLink. How does IAM evaluate access?
- [Substack MNC, 2026] [M] Create and deploy Lambda functions. Ways to ship Lambda artifacts.
- [Substack MNC] [M] Securely connect to databases, EC2, EKS and ECS.
- [Glassdoor/Dream11, 2020] [M] About 20 AWS MCQs in the online test.
- [Glassdoor/Freshworks] [M] Terraform state file handling, plus basic AWS.
- [GfG list] [M] Security groups vs NACLs. KMS envelope encryption.
- [GfG list] [M] Elastic IP vs public IP. ALB/NLB types. ASG components.
- [GfG list] [M] S3 vs EBS vs EFS. ECS vs EKS. SNS vs SQS.
- [GfG list] [M] Give a private-subnet EC2 outbound internet for patches.
- [GfG list] [S] DR plan for RTO 15 min and RPO 1 min in a single region.
- [GfG list] [S] Leaked IAM keys in a public GitHub repo: response steps.
- [GfG list] [M] Store and rotate secrets (API keys, DB passwords).
- [Atlassian SRE mock, 2025] [S] Multi-AZ that fails worse than expected: validate zonal independence.

#### Big tech (Google, Meta, Apple, Amazon, Microsoft and others): AWS

- [Microsoft] [S] Azure / cloud knowledge probed in depth (Senior SRE) [C20]
- [Microsoft] Design log aggregation across Azure regions [C49 (agg)]
- [Amazon DevOps] AWS appears through the debugging and design rounds. I found no specific AWS-service question in a credible candidate report.

## 10. Beyond this month: Terraform, CI/CD design, SRE, observability, 2025-26 topics

Not part of the November scope, but they come up inside the rounds above. Skim now, study in December.

### New 2025-26 topics

- [M] What are the three signals of OpenTelemetry (traces, metrics, logs)? What does the OTel Collector do, and agent vs gateway deployment?
- [M] What is context propagation, and why do traces break at queues or async boundaries?
- [M] Argo CD: what are an Application and an ApplicationSet? What do auto-sync, self-heal and prune do?
- [M] Argo CD sync waves and hooks (PreSync/PostSync). How do you run a DB migration before the app updates?
- [M] An Argo CD app is permanently OutOfSync even though nothing changed. Why? (fields mutated by controllers/webhooks, `ignoreDifferences`)
- [S] How do you promote a release across dev, staging and prod with GitOps? (image updater, PRs per environment, Kargo)
- [S] How do you manage 100 clusters with GitOps? (ApplicationSets with cluster generator, hub-and-spoke vs per-cluster Argo CD)
- [S] SBOM formats (SPDX vs CycloneDX) and what VEX adds.
- [M] How do you use AI coding assistants for infrastructure work safely? (no secrets in prompts, review generated IAM/Terraform, plan before apply, licensing)
- [S] An AI agent has access to your cloud/CI. What guardrails do you put in place? (least-privilege role, read-only by default, approval gates, audit logs, sandboxed credentials)
- [S] How would you use an LLM to speed up incident response (log summarisation, runbook lookup) without trusting it blindly?
- [S] What is platform engineering / an internal developer platform, and how is it different from a DevOps team? (golden paths, Backstage)

### Terraform (reported)

- [GfG, Inorg Global] [B] .tf vs .tfvars. Structure of a resource block.
- [GfG, Cisco, 2023] [M] Walk through your Terraform code that provisions AWS.
- [Glassdoor/Flipkart, 2025] [M] Terraform commands, plus GCP/GKE.
- [Medium/Accenture] [M] Terraform in CI/CD design.
- [Apurv Gujjar blog] [M] What is infrastructure drift?

### CI/CD, SRE and observability (reported)

- [GfG, Paytm, 2024] [B] What is DevOps? What does a DevOps engineer do? Do you know CI/CD?
- [Taro/IBM, 2023] [B] CI vs CD difference.
- [Taro/GE, 2024] [S] Define SLOs and SLIs for an e-commerce-style portal.
- [Walmart LinkedIn, 2023] [M] What are Prometheus collectors and exporters? Grafana dashboards.
- [Atlassian SRE mock, 2025] [S] Error budget burning mid-month after a release: freeze, roll back or flag?
- [Atlassian SRE mock] [S] Blameless postmortem. What makes a corrective action useful?
- [HireStepX India guide] [M] USE vs RED monitoring methods.
- [Glassdoor/Zoho DevOps] [M] How would you manage a server and automate deployments?
- [GfG, rtCamp] [M] Site slows under high traffic: how do you cut server load? Horizontal vs vertical scaling.
- [GfG, rtCamp] [M] How do you back up your servers?
- [Apurv Gujjar blog] [M] Canary deployments, handling pipeline secrets, artifacts, rollback.

### SRE concepts asked at big tech

- [Atlassian] Name the four golden signals of monitoring (Bengaluru 2022) [C27]
- [Atlassian] Difference between availability and reliability, and how you calculate each [C27]
- [Google] Define SLOs for a service that has none [C6 (agg; cites Glassdoor)]
- [Google] Toil vs engineering work, with an example of converting toil [C6 (agg; cites Glassdoor)]
- [Microsoft] When should ops work be automated, and what separates good-enough from excellent automation? (2026) [C23]

---

# Part 2: Writing tasks

Things you write by hand, in an editor or on a shared doc, during the interview.
Practise each one from a blank file, with a timer, then validate it with the tool named.

## Practice tasks with requirements

Write each of these by hand, from an empty file, without copying from docs,
then validate it (`kubectl apply --dry-run=server`, `hadolint`, `actionlint`,
`shellcheck`, `helm lint`, Jenkins replay). Time yourself: 10-20 minutes each.

### Kubernetes manifests
1. **Deployment:** `web` with 3 replicas of `nginx:1.27`, rolling update `maxSurge: 1`, `maxUnavailable: 0`, requests 100m/128Mi and memory limit 256Mi, readiness probe on `/healthz`, liveness probe with a 20 s initial delay, `runAsNonRoot`, read-only root filesystem, and labels `app`/`tier`.
2. **Service:** a ClusterIP Service for `web` on port 80 -> named targetPort `http`; then a headless Service for the same pods; explain the DNS difference.
3. **Ingress:** host `app.example.com`, TLS from a Secret, `/api` -> `api-svc:8080` and `/` -> `web:80`, with `ingressClassName: nginx`.
4. **Gateway API:** a Gateway with an HTTPS listener and an HTTPRoute that splits 90/10 between `web-v1` and `web-v2`.
5. **ConfigMap and Secret:** a ConfigMap with an app config file mounted at `/etc/app/config.yaml` and a Secret with DB credentials exposed as env vars; add a checksum annotation so changes trigger a rollout.
6. **PVC:** a 10Gi `ReadWriteOnce` claim on a `gp3` StorageClass with `WaitForFirstConsumer`, mounted at `/data` in a pod; also write the StorageClass.
7. **HPA (autoscaling/v2):** scale `web` between 2 and 10 replicas at 70% CPU, with a scale-down stabilisation window of 5 minutes.
8. **NetworkPolicy:** default-deny ingress and egress for namespace `prod`; allow DNS to kube-system; allow `frontend` -> `backend` on TCP 8080; allow `backend` -> `postgres` on 5432.
9. **RBAC:** ServiceAccount `ci-deployer` that can `get/list/patch/update` Deployments and `get/list` pods in namespace `staging` only; verify with `kubectl auth can-i --as=system:serviceaccount:staging:ci-deployer`.
10. **CronJob:** run a DB backup at 02:30 daily, `concurrencyPolicy: Forbid`, keep 3 successful and 1 failed job, `backoffLimit: 2`, `activeDeadlineSeconds: 3600`.
11. **StatefulSet:** 3-replica Redis or Postgres with a headless Service, `volumeClaimTemplates`, `podManagementPolicy: OrderedReady`, and anti-affinity across nodes.
12. **DaemonSet:** a log agent on every node including control-plane nodes (tolerations), with a hostPath mount of `/var/log` read-only.
13. **Job with native sidecar:** a batch Job whose log-forwarder sidecar is an init container with `restartPolicy: Always`, so the Job completes.
14. **PodDisruptionBudget + topology spread:** for a 6-replica Deployment, `minAvailable: 4` and even spread across zones.
15. **ResourceQuota + LimitRange:** for a team namespace, cap CPU/memory/pod count and give defaults to containers without requests.

### Helm
1. Create a chart for the Deployment + Service + Ingress above with values for image tag, replicas, resources and an `ingress.enabled` toggle; use `_helpers.tpl` for labels.
2. Add a `values-prod.yaml` override and a `NOTES.txt`; show `helm template` output for dev and prod.
3. Add a pre-upgrade Helm hook Job that runs DB migrations, with a hook delete policy.
4. Add a chart test (`helm test`) that curls the Service.

### Dockerfiles
1. Multi-stage Dockerfile for a Python FastAPI app: builder stage installs wheels, final stage is slim/distroless, non-root user, `HEALTHCHECK`, exec-form `CMD`, `.dockerignore`.
2. Multi-stage Dockerfile for a Go service producing a static binary in a `scratch` or distroless image under 20 MB.
3. Dockerfile for a Node.js app that caches `npm ci` correctly (copy lock file first) and uses a BuildKit cache mount.
4. Dockerfile that uses a BuildKit secret mount to install from a private PyPI index, with no token in any layer.
5. A `docker-compose.yml` for app + Postgres + Redis with healthchecks, `depends_on` conditions, named volumes, resource limits and an `.env` file.

### GitHub Actions workflows
1. CI for a Python repo: matrix over 3 Python versions and 2 OSes, pip caching, lint + test, upload coverage as an artifact, `permissions: contents: read`, actions pinned to SHAs.
2. Build-and-deploy: on push to `main`, build an image, scan with Trivy (fail on HIGH/CRITICAL), push to ECR via OIDC, then deploy to EKS with Helm in a `production` environment with required reviewers and a `concurrency` group.
3. Reusable workflow (`workflow_call`) with inputs `service`, `environment` and an output `image-digest`; a caller workflow that uses it for two services.
4. Composite action that sets up a toolchain and caches it, used by two workflows.
5. Release workflow triggered by a `v*` tag: build wheels/binaries, create a GitHub release, generate an SBOM and provenance attestation.
6. Scheduled workflow that runs nightly, opens an issue if a check fails, and can also be run with `workflow_dispatch` inputs.
7. Monorepo workflow that detects changed folders and runs a matrix only over changed services.

### Jenkinsfiles
1. Declarative Jenkinsfile: checkout, build and unit test in parallel stages (`failFast true`), build Docker image tagged with the commit SHA, push to ECR using `withCredentials`, `input` approval with a 30-minute timeout before prod, Helm deploy, Slack notify in `post { failure }`, `cleanWs()` in `post { always }`.
2. Jenkinsfile using a Kubernetes pod template agent with two containers (build and kubectl), and `when { branch 'main' }` for the deploy stage.
3. Multibranch-friendly Jenkinsfile: PR builds only test; `main` builds deploy to staging; tags deploy to production.
4. Shared library step `vars/dockerBuildPush.groovy` taking image name and registry, plus a Jenkinsfile that uses it at a pinned version.
5. Pipeline with `options { timeout(...); buildDiscarder(...); disableConcurrentBuilds() }` and a `retry(3)` around a flaky network step.

### Shell scripts
1. `deploy.sh --env <dev|stage|prod> --version <semver>`: strict mode, argument validation with usage text, confirmation prompt for prod, logging with timestamps, non-zero exit on any failure.
2. `log_report.sh access.log`: top 10 IPs, count per status code, 5xx per minute, top 10 slowest URLs (if response time is logged), output as a table.
3. `wait-for.sh host port timeout`: waits for a TCP port using `/dev/tcp`, exits 1 on timeout; usable as a container entrypoint helper.
4. `backup.sh`: dump a Postgres database, compress, upload to S3 with a date prefix, keep the last 7 locally, run under `flock`, and clean up on failure via `trap`.
5. `cert_check.sh domains.txt`: print days to expiry for each domain and exit non-zero if any is under 14 days.
6. `k8s_restarts.sh`: using `kubectl` + `jq`, list pods with more than N restarts across all namespaces, sorted.
7. `user_sync.sh users.csv`: create missing users with a given group and SSH key; idempotent, dry-run flag.

### Python scripts
1. `ec2_janitor.py`: stop running EC2 instances tagged `env=dev` across all regions outside working hours; `--dry-run` default; structured logging; handles pagination and API throttling.
2. `s3_audit.py`: list buckets without default encryption, without Block Public Access, or with public ACLs/policies; output CSV.
3. `url_checker.py urls.txt`: check URLs concurrently (thread pool or asyncio), timeout and retries, print status and latency, exit non-zero on any failure.
4. `config_validate.py config.yaml`: load YAML, validate required keys and types (pydantic or manual), print all errors at once.
5. `pod_report.py`: Kubernetes Python client; list pods not Ready or in CrashLoopBackOff with last termination reason and restart count.
6. `snapshot_cleanup.py`: delete EBS snapshots older than N days that are not used by any AMI; dry-run default; summary at end.
7. `gh_stale_prs.py`: GitHub API; list open PRs older than 7 days across an org's repos, handling pagination and rate-limit headers.
8. `log_parser.py`: stream a multi-GB log with a generator, count errors per service per hour, print the top 5.

## What candidates were actually asked to write

### Indian companies, startups and community reports

#### Dockerfile
- [GfG, Inorg Global, ~2024] Write a Dockerfile for a Node.js application.
- [dev.to interview post, 2025] Write a Dockerfile for a ReactJS application.
- [meetassist, 2025] Write a Dockerfile to package a Python app.
- [meetassist, 2025] Implement a multi-stage build.
- [k8s-interview-prep repo] Write a Dockerfile for a Python service and justify every line.
- [Apurv Gujjar blog, 2025] Write an example Dockerfile and give the build command. Write a docker-compose example.

#### CI/CD pipeline (Jenkinsfile or GitHub Actions)
- [dev.to interview post, 2025] Write a pipeline that builds a Node.js image and pushes it to Docker Hub.
- [Livspace, ~2025] Technical challenge: build a complete CI/CD pipeline with Terraform, Kubernetes and GitHub Actions, including monitoring, logging and security.
- [GfG list, Jenkins] Write a sample declarative Jenkins pipeline.
- [GfG list, Jenkins] Design a pipeline that deploys to Kubernetes.

#### Terraform and IaC
- [dev.to interview post, 2025] Write Terraform for 5 EC2 instances with different names and sizes.
- [Medium/Cisco Senior SRE] Practical round: Terraform for EC2 and S3.
- [GfG, Inorg Global] Write the structure of a Terraform resource. Explain .tf vs .tfvars.

#### Shell
- [GfG, Amazon, 2020] Script to pull specific error codes from a log file.
- [GfG, Amazon, 2020] Script listing the top 10 folders by disk usage.
- [GfG, Amazon, 2020] Live text processing with sed, awk, grep -B, sort and uniq.
- [Code360/Shiprocket, 2022] Script to find large files in a directory.
- [Glassdoor/UKG] Shell script to reverse a string.
- [LeetCode/Robin.io, 2022] awk commands for given problems.
- [Dataford/PhonePe SRE] Script to monitor a service and restart it if it is down.
- [meetassist, 2025] Script that checks a process every 2 minutes and logs the result.
- [Taro/Apple India, 2025] Three scripting questions in a CoderPad online assessment, plus 2 easy programs where exact syntax was checked.

#### Python
- [Code360/Shiprocket, 2022] Python script that runs commands over SSH on multiple servers.
- [Glassdoor UK] Parse a log file with strip/split and aggregate in a dict.
- [LinkedIn/Atlassian SRE] Prorated billing report for free and premium users.
- [meetassist, 2025] Parse and validate a command's JSON output, handling failures and malformed input.
- [GfG, Inorg Global] Print the first 10 primes.
- [Code360/ShareChat, 2022] Log-processing logic using a map and string manipulation.

#### Kubernetes manifests and Helm
- No candidate report found that names a live manifest or Helm chart writing task. Prep guides (ThinkCloudly, Simpliaxis) say "debug a Kubernetes manifest live" is common, but they cite no candidate.

#### Take-home assignment
- [GfG, rtCamp] A DevOps take-home with a flexible deadline, submitted by link before the Zoom round. The task is hosted at careers.rtcamp.com/assignments/devops and is not described in the post.

### Big tech (Google, Meta, Apple, Amazon, Microsoft and others)

**Amazon DevOps**
- [Amazon DevOps] From a log full of debug prints, extract specific error codes, first with cat/sed and then as a shell script (the candidate used a for-loop) [C14].
- [Amazon DevOps] Write a shell script that reports the top 10 directories by disk usage with details (df/du in a loop) [C14].
- [Amazon DevOps] File-processing one-liners with cat, sed, awk, tail, head, grep -B, sort and uniq (debugging round) [C14].
- [Amazon DevOps] Python, any language allowed: given an array, report whether it is increasing, decreasing, increasing-then-decreasing or decreasing-then-increasing [C14].
- [Amazon SysDE] OA: easy log and string manipulation programs (L5, Feb 2025) [C17].
- [Amazon SysDE] A Python parsing question during the SysDE I loop (Dec 2025) [C35].

**Meta PE**
- [Meta] [M] Read two CSV files of dinosaur data, compute speed for bipedal dinosaurs from stride and leg length with a given formula, and print the names from fastest to slowest (the "Dinosaur" question, 2025 E4 screen) [C5, C8].
- [Meta] [M] Onsite: one file-parsing problem, then many follow-ups to optimise and extend it [C5].
- [Meta] Read several files and print the word(s) common to all of them, with the files they appear in [C21].
- [Meta] Parse log files: extract columns, sort and filter, write the output to a file. Some versions give you helper functions you must use [C7 (agg)].
- [Meta] Minesweeper: build a small grid with randomly placed mines, count neighbours, and reveal recursively with DFS [C9 (agg), C21].
- [Meta] Implement a streaming vmstat alert: a monitor that consumes metric lines and alerts over a sliding window (SRE technical screen, Apr 2026) [C23].
- [Meta] Command-line input handling plus text processing after a sliding-window warm-up [C21].

**Google SRE**
- [Google] Write a script or tool similar to ps or find, and parse logs (scripting round, per Blind) [C11].
- [Google] Implement `tail` for a very large file [C2 (agg)].
- [Google] Implement `deleteDirectoryTree(path)` using only list-dir and delete APIs [C2 (agg)].
- [Google] Write a script that finds duplicate files [C2 (agg)].
- [Google] Compute a folder's size from a given filesystem structure [C2 (agg)].
- [Google] Write a ping script that finds slow hosts and prints chosen fields, then extend it across iterations [C10 (agg)].
- [Google] Parse an input file, compute a result, and handle the follow-ups: empty file, billions of rows, malformed lines, visible parse failures, real-time input, and data bigger than RAM [C10 (agg)].
- [Google] [M] Process a file with a "system function" in Python. Iterate lazily over the file object; the interviewer pushed toward a sliding window (2022 onsite) [C1].
- [Google] Scan a 50 GB log for 5xx errors within 512 MB of RAM [C6 (agg)].
- [Google] Concurrent URL fetcher with a rate limit and timeouts [C6 (agg)].

**LinkedIn and unnamed SRE screens**
- [LinkedIn] Parse logs and count messages within a date range [C28].
- [LinkedIn] Recursive API question: walk an employee API and print the reporting hierarchy [C28].
- [Unnamed SRE screen, LinkedIn-style] Parse syslog lines: output a CSV of messages per minute, then a CSV of counts per program (regex-heavy) [C36].
- [LinkedIn] Code review: find the bugs in three given programs (backup script, log parser, one more) [C28].

**Others**
- [Uber] Screen-share coding: parse a data set [C26].
- [Apple] Given a dict, sort it and remove duplicates (coding screen opener) [C19].
- [Microsoft] SQL: count files where "bull" and "bear" appear as standalone space-delimited words (2026) [C23].
- [Atlassian] Build a middleware URL router with hidden extra requirements (Aug 2022) [C27].
- [Atlassian] Build a multi-user rate limiter [C27].

Nobody, at any of these companies, reported being asked to write a Dockerfile, a Kubernetes manifest, or a CI pipeline file live. Generic guides mention such tasks, but I found no candidate report tying them to these companies.

---

# Part 3: Debugging questions

For every scenario, answer in this order: impact and mitigation first, then
gather data (what changed, logs, metrics), form a hypothesis, test it, fix, and
say what you would change so it cannot happen again.

## Scenarios and broken-setup labs

Two kinds of item here. **Symptom questions** ("X is happening - walk me through
it") are asked verbally. **Broken-setup labs** are the Killercoda/CKA style: you
are given a broken system and must fix it live. For each lab, build the broken
state yourself first (break it on purpose in a VM, kind cluster or free-tier
account), then fix it with a timer running. Items already in the main bank (Pod
Pending, CrashLoopBackOff, disk full, slow server, ALB 502, etc.) are not repeated.

### Linux
- [M] `sudo` hangs for 20+ seconds before every command. Why? (hostname not resolvable in `/etc/hosts`, DNS timeout)
- [M] SSH login takes 30 seconds after you type the password. What do you check? (`UseDNS`, GSSAPI, reverse DNS, PAM, `ssh -vvv`)
- [M] A systemd service shows `active (exited)` or keeps flapping with "start request repeated too quickly". Debug it. (`Type=` wrong for a forking/foreground process, `StartLimitBurst`, `journalctl -u -b`)
- [M] A cron job runs fine manually but never runs on schedule. List your checks. (crond running, syntax, `%` escaping, PATH, permissions, `/etc/cron.allow`, logs in `journalctl`/syslog)
- [M] A process is using 100% CPU. Find out what it is doing without stopping it. (`top -H`, `pidstat`, `strace -p`, `perf top`, `py-spy`/`jstack` for runtimes)
- [M] Memory use on a server climbs over days until the OOM killer fires. How do you find the leaking process and prove it? (`ps --sort=-rss` over time, `smem`, `/proc/<pid>/status`, slab via `slabtop`)
- [M] `apt`/`yum` fails with a lock error or GPG key error during automation. Fix safely.
- [M] Clock is drifting and TLS handshakes / AWS API calls fail with signature errors. What do you check? (chrony/NTP, `timedatectl`)
- [S] A server boots into emergency mode. Get it back. (bad fstab entry, missing disk UUID, `nofail`, `systemctl --failed`)
- [S] `ls` in one directory takes minutes. Why? (millions of files, NFS, dentry cache) How do you clean it safely?
- [S] After a kernel update the NIC or a storage driver is missing. How do you boot the old kernel and pin it?
- **Broken-setup labs**
  1. Nginx will not start: port 80 already in use by another process, and the config has a syntax error. Fix both (`ss -ltnp`, `nginx -t`).
  2. A user cannot log in over SSH with their key: home directory and `~/.ssh` permissions are wrong. Fix without weakening sshd config.
  3. A disk is full because a deleted 5 GB log is still held open by a running app. Free the space without restarting it.
  4. A systemd unit points to a missing binary and has the wrong `User=`. Fix it and make it restart on failure.
  5. `/etc/fstab` has a typo for a data disk; reboot into the rescue shell and fix it.
  6. The `PATH` is broken in `/etc/profile.d/` so common commands fail for all users. Find and fix the file.

### Networking
- [M] DNS works with `dig @8.8.8.8` but not with plain `dig`. What is wrong? (`/etc/resolv.conf`, systemd-resolved stub, search domains)
- [M] `curl https://api.example.com` fails with "certificate verify failed" on one server only. Causes. (missing CA bundle, missing intermediate, clock wrong, corporate proxy)
- [M] Large file uploads hang but small requests work across a VPN. What is the likely cause and how do you prove it? (MTU/PMTUD, `ping -M do -s`)
- [M] A service is reachable on `localhost` but not from other machines. Why? (bound to 127.0.0.1, host firewall, SG)
- [M] Requests through a load balancer randomly fail with 502 every few minutes. (upstream keep-alive timeout shorter than LB idle timeout)
- [S] Packet loss shows in `mtr` at a middle hop but not at the destination. Is that a real problem?
- [S] Intermittent DNS resolution failures (5-second delays) inside containers. (conntrack race with parallel A/AAAA queries, `single-request-reopen`, NodeLocal DNS)
- **Broken-setup labs**
  1. An iptables rule order blocks HTTPS while allowing HTTP. Find and fix it without locking yourself out of SSH.
  2. A host has the wrong default gateway. Restore connectivity and make it persistent.
  3. A certificate chain on nginx is missing the intermediate. Prove it with `openssl s_client` and fix it.

### Docker
- [M] `docker build` is suddenly slow and re-runs every step. Why? (cache bust from `COPY . .` too early, changing build args, timestamps, no cache in CI)
- [M] A container cannot resolve other containers by name. Why? (default bridge has no DNS; use a user-defined network)
- [M] Container works locally but crashes in CI/production with "exec format error". (architecture mismatch, arm64 vs amd64)
- [M] A bind-mounted directory gives "permission denied" inside the container. (UID mismatch, SELinux `:z` labels, rootless)
- [M] Docker's disk usage grows to fill the host. Find what is using it and clean up. (`docker system df -v`, logs, dangling images, build cache, volumes)
- [M] `docker pull` fails with "toomanyrequests". What is it and how do you avoid it? (Docker Hub rate limits, auth, pull-through cache/ECR mirror)
- [S] A container cannot reach the internet but the host can. (iptables flushed by another tool, `ip_forward=0`, DNS config, MTU on overlay/VPN)
- **Broken-setup labs**
  1. A Dockerfile uses shell-form ENTRYPOINT so the app ignores SIGTERM and stop takes 10 s. Fix it and prove clean shutdown.
  2. A Compose app starts before its database is ready and crashes. Add healthchecks and `depends_on` conditions.
  3. An image runs as root and writes to `/app`; switch it to a non-root user without breaking the write path.

### Kubernetes
- [M] A Service has no endpoints. Walk through every cause. (selector mismatch, pods not Ready, wrong namespace, port name mismatch)
- [M] `kubectl` returns "Unable to connect to the server" or "x509: certificate signed by unknown authority". Debug. (kubeconfig context, expired certs, API endpoint, VPN)
- [M] A pod cannot pull from a private registry even though the secret exists. (secret in wrong namespace, not in `imagePullSecrets`/ServiceAccount, wrong registry hostname, ECR token expired)
- [M] A ConfigMap was updated but the app still uses old values. Why? (env vars are read at start, subPath mounts do not update, need rollout restart / checksum annotation)
- [M] Ingress returns 404 for one path while others work. (pathType, rewrite annotation, ingressClassName, host mismatch)
- [M] A PVC stays `Pending`. Causes. (no default StorageClass, no matching PV, wrong access mode, `WaitForFirstConsumer`, CSI driver not installed)
- [M] `kubectl drain` hangs forever. Why? (PDB with `minAvailable` equal to replicas, unmanaged pods, emptyDir data flag)
- [M] Pods are `Running` but `0/1 Ready` after a deploy. How do you find out why? (readiness probe path/port, `describe` events, app dependency down)
- [S] DNS lookups from pods fail intermittently only on certain nodes. (CoreDNS placement, kube-proxy on that node, conntrack, node-local DNS)
- [S] After a cluster upgrade, some Helm releases fail with "no matches for kind". (removed API versions, `kubectl convert`, pluto)
- [S] A validating webhook is down and now no pods can be created anywhere. Recover. (`failurePolicy`, delete/patch the webhook config, namespace selectors)
- **Broken-setup labs (CKA/Killercoda style)**
  1. A Deployment's pods are Pending: requests ask for 64 CPUs. Fix requests so they schedule.
  2. A Service selects `app: web` but pods are labelled `app: webapp`, and `targetPort` is wrong. Fix both and prove with `curl` from a test pod.
  3. A worker node is `NotReady` because the kubelet is stopped and its config points to a wrong CA path. Fix it from the node.
  4. kube-scheduler static pod manifest has a typo, so all new pods stay Pending. Find it in `/etc/kubernetes/manifests` and fix it.
  5. A pod is in CrashLoopBackOff because a required env var comes from a Secret key that does not exist. Fix the Secret.
  6. A NetworkPolicy blocks the frontend from reaching the backend. Fix it to allow only that path, and keep DNS working.
  7. An HPA shows `<unknown>` for CPU. Fix it (missing metrics-server or missing CPU requests).
  8. A PVC is Pending because the StorageClass name is misspelled. Fix it without deleting data.
  9. A CronJob never runs because of a bad schedule and `suspend: true`. Fix and trigger one run manually.
  10. A pod cannot be scheduled because every node is tainted. Add the right toleration (do not remove the taint).
  11. An RBAC error: a ServiceAccount gets "forbidden" listing pods. Fix with the minimum Role and RoleBinding.
  12. Liveness probe points to the wrong port and keeps killing a healthy container. Fix it and add a startup probe.

### Jenkins
- [M] A pipeline fails with "No such DSL method" after a plugin upgrade. What happened and how do you recover? (plugin removed/renamed, pin versions, restore backup)
- [M] Builds hang at "Waiting for next available executor" though agents look online. (label mismatch, executors set to 0, cloud template errors)
- [M] `docker: permission denied` inside a Jenkins agent. (user not in docker group, socket permissions, rootless alternatives like Kaniko/BuildKit)
- [M] Git checkout fails with "Host key verification failed" or auth errors only in Jenkins. (credentials ID, known_hosts on agent, SSH vs HTTPS URL)
- [M] Webhook from GitHub does not trigger builds. Debug. (webhook delivery logs, URL/CSRF, firewall, job trigger config)
- [S] The Jenkins controller becomes very slow and the UI times out. (heap/GC, too many builds kept, heavy plugins, builds running on controller, thread dump)
- **Broken-setup labs**
  1. A Jenkinsfile has a wrong `agent` label and a credential ID typo. Fix both from the console log alone.
  2. A shared library is referenced at a branch that was deleted. Pin it to a tag and make the build pass.

### GitHub Actions
- [M] A workflow is not triggering at all on push. Why? (YAML in wrong path, branch/path filters, workflow disabled, `[skip ci]`, events from `GITHUB_TOKEN` do not trigger new workflows)
- [M] A job is skipped and you do not know why. (`if:` expression, a `needs` job failed or was skipped, `always()` usage)
- [M] OIDC to AWS fails with "Not authorized to perform sts:AssumeRoleWithWebIdentity". What do you check? (`id-token: write` permission, `sub`/`aud` claim mismatch in trust policy, provider thumbprint/URL, environment name)
- [M] The cache never hits. Why? (key includes changing values, different OS, cache scoped to branch, cache evicted at size limit)
- [M] A step fails with 403 when pushing a tag or commenting on a PR. (GITHUB_TOKEN permissions, fork PR read-only token, branch protection)
- [M] An artifact uploaded in one job is not found in another. (name mismatch, v4 artifact immutability, job not dependent via `needs`)
- [S] Self-hosted runner jobs are queued forever. (runner offline, labels mismatch, runner group not allowed for repo, ARC scaling config)
- **Broken-setup labs**
  1. A workflow with wrong indentation, a missing `needs`, and an unquoted expression in `if:`. Fix it so it passes `actionlint`.
  2. A deploy workflow fails OIDC because the trust policy expects `ref:refs/heads/main` but the job uses an environment. Fix the trust policy.

### AWS
- [M] An EC2 instance is running but status check "instance reachability" fails. What do you do? (system log, serial console, kernel/fstab issues, stop/start to move host)
- [M] You cannot SSH to an EC2 instance. Full checklist. (SG, NACL, route to IGW, public IP, key pair, user, OS firewall, sshd, instance status)
- [M] A new ASG launches instances that are immediately terminated in a loop. (health check grace period, failing user data, ELB health check path, AMI problem)
- [M] ALB target group shows all targets unhealthy. Debug. (SG from ALB to target, health check path/port/codes, app listening on 0.0.0.0)
- [M] A Lambda function times out when calling DynamoDB/S3. Why? (in VPC without endpoint or NAT, timeout too low, cold start)
- [M] Route 53 record points to an ALB but the domain does not resolve. (wrong hosted zone, NS delegation at registrar, private vs public zone)
- [M] An EKS node group's nodes never join the cluster. (IAM node role not in access entries/aws-auth, subnet tags, security groups, bootstrap/user data)
- [M] A CloudFront distribution serves stale content after deploy. (cache TTL, invalidation, versioned file names)
- [S] Cross-account `AssumeRole` fails with AccessDenied. Which three policies must all allow it? (caller's identity policy, target role trust policy, SCPs in both accounts)
- [S] RDS storage suddenly full and the database is read-only. Immediate and long-term actions. (storage autoscaling, binlog/WAL retention, long transactions)
- **Broken-setup labs**
  1. A private instance cannot reach the internet: the route table sends 0.0.0.0/0 to the IGW instead of the NAT. Fix it.
  2. An S3 bucket policy denies all access because of a bad `Condition`. Fix it as an admin without removing the protection it intended.
  3. An ALB returns 503 because the target group has no registered targets; register them via the ASG and fix the health check path.

## Reported debugging questions

### Indian companies, startups and community reports

#### Kubernetes
- [dev.to interview post, 2025] [M] A pod is in CrashLoopBackOff: troubleshoot it.
- [dev.to interview post, 2025] [M] One ReplicaSet in the cluster is not working: debug it.
- [Glassdoor/Delhivery] [M] You cannot connect to a pod in the cluster: debug the connection.
- [Glassdoor bank] [M] A pod is in CrashLoopBackOff or Pending: how do you debug it?
- [Substack MNC, 2026] [M] The deployment succeeded but the application is failing: what do you check first?
- [meetassist, 2025] [M] A Service is not routing traffic to the right pods.
- [meetassist, 2025] [M] Pods are Pending on AKS even though CPU is free.
- [meetassist, 2025] [M] A container works locally but is OOMKilled in production.
- [GfG list] [M] What do CrashLoopBackOff, ImagePullBackOff, OOMKilled, Pending and Evicted mean, and how do you fix each?
- [LivingDevOps guide] [M] The load balancer returns 503 for a Kubernetes app.
- [LivingDevOps guide] [S] The firewall breaks between master and worker nodes: what happens?
- [LivingDevOps guide] [M] All StatefulSet pods share one volume: fix it.
- [k8s-interview-prep repo] [M] Pods are Running but none are Ready, so the Service is dead.
- [k8s-interview-prep repo] [M] A Deployment rollout is stuck.
- [k8s-interview-prep repo] [M] One Service returns intermittent 503s.
- [k8s-interview-prep repo] [S] Traffic doubled but the HPA did not scale. The HPA shows `<unknown>`.
- [k8s-interview-prep repo] [S] A node goes NotReady and its pods disappear (on-call).
- [k8s-interview-prep repo] [S] A stateful pod cannot mount its volume.
- [k8s-interview-prep repo] [S] kubectl is slow and the API server times out.
- [k8s-interview-prep repo] [S] p99 latency doubles after a deploy while errors stay flat.
- [k8s-interview-prep repo] [S] Intermittent cluster-wide DNS failures.
- [k8s-interview-prep repo] [S] A namespace is stuck in Terminating.
- [k8s-interview-prep repo] [S] A node hits DiskPressure and evicts pods.
- [k8s-interview-prep repo] [S] Certificates expired and kubelets fail TLS.
- [k8s-interview-prep repo] [S] A drain hangs during an upgrade.
- [k8s-interview-prep repo] [S] Memory climbs all day and the pod OOMs nightly: is it a leak?
- [k8s-interview-prep repo] [S] Intermittent packet loss between two specific pods.
- [GfG list, AWS] [S] Services on EKS fail to talk to each other.

#### Linux and server
- [GfG, Amazon DevOps/Support, 2020] [M] Debug a disk-space issue on a Linux host.
- [meetassist, 2025] [M] Disk space is running low: find what is using it.
- [meetassist, 2025] [M] A production server or service is suddenly slow.
- [Medium/VMware SRE snippet] [M] A Linux system is slow: walk through CPU and memory checks.
- [Dataford/PhonePe SRE] [M] Troubleshoot high CPU on a production server.
- [dev.to deadlock SRE list] [M] "Filesystem full" but df shows free space.
- [dev.to deadlock SRE list] [M] Troubleshoot high disk I/O.
- [k8s-interview-prep repo] [M] df and du disagree: find the culprit (deleted-but-open files).
- [k8s-interview-prep repo] [M] Handle a CPU spike end to end.
- [Apurv Gujjar blog] [M] SSH is not working: how do you get into the server?
- [Apurv Gujjar blog] [M] First check for high server load. First check when a website is slow.
- [GfG list] [M] What is the first command on a slow server? Diagnose high CPU and high memory.
- [Glassdoor/Persistent Systems] [M] Debug a provided Python script (client round).
- [GfG, rtCamp] [B] Find and stop the process eating the most resources.

#### Network, API and application
- [GfG, Amazon DevOps/Support, 2020] [S] An API returns bad output: debug from network connectivity down to the backing database.
- [Taro/Flipkart, 2020] [M] An unnamed troubleshooting question was the whole first interview.
- [meetassist, 2025] [M] The application cannot reach its database.
- [meetassist, 2025] [M] An API returns 401 though the token looks valid.
- [meetassist, 2025] [S] A Kafka cluster is not processing data, or a broker is down.
- [meetassist, 2025] [M] A Grafana dashboard renders very slowly. A SQL query is slow.
- [GfG, Nutanix, 2023] [M] Many real networking troubleshooting scenarios (not itemised).
- [GfG, Nutanix, 2020] [B] Use ping and traceroute to troubleshoot connectivity.
- [Atlassian SRE mock, 2025] [S] 3 a.m. page: high latency and intermittent 5xx, with an outdated runbook.
- [Atlassian SRE mock] [S] A risky config change mid-incident: which guardrails?
- [Atlassian SRE mock] [S] RDS failover browns out the app for 90 seconds: fix connection handling and DNS TTL.

#### CI/CD pipeline
- [Substack MNC, 2026] [M] A Jenkins pipeline runs but the build does not trigger: possible causes.
- [Substack MNC] [M] A critical bug is found in production: what is your approach?
- [meetassist, 2025] [M] The pipeline passes in dev but fails in prod.
- [meetassist, 2025] [M] Users see 500s after a new version deploys via CI/CD.
- [GfG list, Jenkins] [M] A new commit does not trigger a build.
- [GfG list, Jenkins] [M] A build fails for lack of available nodes.
- [GfG list, Jenkins] [M] Inconsistent pipeline results across runs.
- [GfG list, Jenkins] [M] A missing dependency in the build environment.
- [GfG list, Jenkins] [M] No failure notifications. A slow job blocked on external dependencies.
- [GfG list, Jenkins] [S] The controller is out of resources: scale it.
- [GfG list, Jenkins] [M] Steps for a broken build.
- [Apurv Gujjar blog] [M] A deployment fails at night: what do you do?

#### AWS
- [GfG list] [M] Intermittent 502s behind an ALB plus ASG.
- [GfG list] [M] An EC2 instance cannot read objects in an S3 bucket.
- [GfG list] [M] An ASG does not scale out though CPU is above the threshold.
- [GfG list] [M] Lambda in a VPC times out talking to RDS.
- [GfG list] [M] AWS costs suddenly spike: investigate.
- [GfG list] [M] Lost EC2 key pair: regain access.
- [Apurv Gujjar blog] [M] High RDS load. Why AWS costs are rising.

### Big tech (Google, Meta, Apple, Amazon, Microsoft and others)

**Meta PE screens and onsite**
- [Meta] [M] "Uncle Bernie's website is down, find out why." The scenario changes as you ask questions, and the root cause is a full disk (2025 E4 screen) [C5].
- [Meta] For a site that keeps going down, what extra information would you collect from the system? [C21]
- [Meta] Given an architecture diagram of a Meta-like service, find out why requests are being processed slowly [C8 (agg)].
- [Meta] An application won't start on a server. How do you troubleshoot it? [C7 (agg)]
- [Meta] Diagnose a web server that is slow or crashing [C7 (agg)].
- [Meta] A daily job fails to allocate 1 GB even though `free -h` shows 1 GB free and swap is off. Why? (Hint: fragmentation) [C7 (agg)]
- [Meta] An app is slow and swapping heavily. How do you fix it? [C7 (agg)]
- [Meta] Debug a memory leak in a Linux service [C9 (agg)].
- [Meta] Troubleshoot a production server outage (SRE technical screen, Apr 2026) [C23].
- [Meta] Troubleshoot a web server outage at midnight (Apr 2026) [C23].
- [Meta] Debug a site, front end plus back end, that goes down. A Blind commenter adds that you should know how grep or a DNS lookup works internally [C30].
- [Meta] Systems round: troubleshoot a server issue, then explain the output of specific Linux commands (Dec 2024 onsite) [C4].
- [Meta Network PE] A machine in one branch can ping 29 hosts but not one host in the UK branch. Why? [C29]
- [Meta Network PE] Walk through troubleshooting a server that is unreachable [C29].

**Google SRE**
- [Google] Two servers, one is unreachable. Find out why (Tel Aviv, Feb 2024) [C13].
- [Google] How do you troubleshoot a three-tier application? (Bengaluru, Nov 2024) [C13]
- [Google] Debug a slow web server using any tool you like [C31].
- [Google] You cannot SSH into a remote machine. What do you do? [C2 (agg), C10 (agg)]
- [Google] A system is running out of PIDs. Detect the cause and stop it [C2 (agg)].
- [Google] Errors started after a recent deploy. Find the root cause, state a short-term mitigation and a long-term fix, and explain why each hypothesis is ruled out [C10 (agg)].
- [Google] Latency doubled while CPU sits at 50%. Find the cause [C10 (agg), C6 (agg)].
- [Google] Given an architecture with symptoms in performance, memory, storage, I/O, load balancing or firewall, isolate the cause [C2 (agg)].
- [Google] Capture and analyse network traffic to debug an issue (tcpdump) [C2 (agg)].
- [Google] Identify packet loss along a network path [C2 (agg)].
- [Google] Tell whether a proxy, including a transparent one, sits in the path [C2 (agg)].
- [Google] Why does a process sit in D state, and how do you debug it? [C6 (agg; cites Glassdoor)]
- [Google] Respond to a "SearchFailure" black-box probe alert. This is the scenario from Google's SRE Workbook, and candidates report similar ones [C6 (agg)].

**Amazon**
- [Amazon DevOps] How do you debug a disk-space problem on Linux? [C14]
- [Amazon DevOps] An API returns bad output. Debug every layer, from network connectivity through to the database it reads from [C14].
- [Amazon SysDE] A website cannot fetch data from its SQL database. What do you check? [C32]
- [Amazon SysDE] The current webpage goes blank. Troubleshoot it interactively with follow-ups [C32].
- [Amazon] Investigate a slow Linux host: CPU, memory, disk and network [C33 (agg)].

**Apple**
- [Apple] You have shell access, but the PID limit is exhausted. Find the root cause using only shell builtins [C19].
- [Apple] [S] On-call scenario: iCloud Photos are not displaying for users. How do you investigate? (SRE Manager) [C19]
- [Apple] A customer reports problems with a website backed by a web server and a DB. Find out whether the issue is slow requests or server errors, then dig into component configuration (Blind commenter describing an Apple-style variant) [C34].

**Microsoft**
- [Microsoft] Live troubleshooting: diagnose and mitigate very high CPU on a public-facing service (2026) [C23].
- [Microsoft] Infrastructure troubleshooting task in the loop, plus troubleshooting in the senior manager round [C20].
- [Microsoft] On-call: judge severity, decide when to escalate, and trace an issue back to a recent change (2026) [C23].

**Uber**
- [Uber] A VM has no SSH access. How do you investigate using Linux tools? [C26]
- [Uber] 1 in 20 requests through the stack fail. Troubleshoot it [C26].

**LinkedIn**
- [LinkedIn] A live Apache server returns 500 and 400 errors. Fix it (onsite troubleshooting round) [C28].

**Netflix**
- [Netflix] A dedicated incident-management round: walk through how you run a major incident [C24].

---

# Part 4: Company interview loops and big-tech track

## Indian product companies, startups and MNC India centres

### Indian product companies and scale-ups
- **Flipkart (DevOps/SRE, Bengaluru)**
  - 2020 report (Taro): troubleshooting-only interview, plus a second interviewer.
  - 2023 report (Taro): about 1 hour, covering Linux, Kubernetes and Jenkins.
  - Glassdoor (contract role): 2 rounds. Round 1 technical, round 2 semi-technical with SRE practice and on-call incident hypotheticals.
  - Late-2025 Glassdoor snippet: Terraform commands, scripting and GCP/GKE. Rated difficult.
- **PhonePe (SRE)**
  - 2021 Glassdoor/Code360 campus reports: one online test (networking, DBMS, Linux) and three interviews on the same day. The face-to-face rounds covered networking and shell scripting.
  - Dataford 2026 guide: Linux/OS is the heaviest area and Kubernetes is a nice-to-have.
- **Meesho (SDE-DevOps / SRE)**
  - 2021 Glassdoor: Docker internals, Kubernetes concepts, an easy sorting problem and DNS resolution.
  - Another report: a deep technical round on Docker, Linux and Kubernetes. The process took about 3 weeks.
- **Myntra (Senior SRE)**: 5-6 on-site rounds: Linux fundamentals, system design, advanced troubleshooting and coding (Glassdoor). A 2018 DevOps report mentions only basic Linux questions.
- **Dream11 (DevOps, 2020)**
  - HackerRank test: 2 programs plus about 20 AWS MCQs.
  - Then 2 technical rounds, an upper-management discussion and HR.
  - Topics: monitoring, Kubernetes, Python, AWS, ELK, Ansible and OS. The candidate was steered toward SRE.
- **ShareChat (DevOps, 0-2 yrs, Jan 2022, Code360)**: Linux, cloud, Kubernetes and Docker. Real-world DevOps problems to solve. A log-processing design question using a map and string handling. Prep topics: networking, OS, Terraform and shell.
- **Shiprocket (DevOps, 0-2 yrs, May 2022, Code360)**: a shell task to find large files, a Python script to run commands over SSH on many servers, then a video round with the DevOps lead on containers, monitoring and Agile.
- **Livspace (DevOps)**: 6 rounds.
  1. Screen with the lead DevOps engineer (AWS, Kubernetes, IaC, monitoring, CI/CD).
  2. Technical challenge: build a full CI/CD pipeline with Terraform, Kubernetes, GitHub Actions, monitoring and security.
  3. Infrastructure design deep dive.
  4. Troubleshooting: failing Kubernetes clusters, incidents, multi-region.
  5. Sysadmin and security.
  6. Leadership.
- **Juspay (SRE/DevOps, Glassdoor)**: Linux, databases, cloud, OS and networking. SDE loops add an MCQ round, a hackathon round and computer-fundamentals-heavy final rounds.
- **Freshworks (SRE/DevOps, Glassdoor)**: a single 60-minute round on DevOps basics and databases: Terraform (state handling), Git, CI/CD, Kubernetes and basic AWS. SRE hires are told automation coding is expected.
- **Zoho (DevOps Project Trainee, GfG, about 2025)**
  1. Aptitude and C/Python MCQ.
  2. 100 CS-fundamentals MCQs.
  3. 5 easy-to-medium coding problems.
  4. Technical (networking, cloud) plus HR.
  - Glassdoor adds Python/DevOps/CN MCQ rounds.
- **Paytm**
  - DevOps intern, on-campus 2024 (GfG): a coding round (3 SQL, 1 Java OOP, 1 array DSA), a technical round (AWS basics, Git branching, Linux, 2 DSA) and a technical plus managerial round.
  - 2022 campus (GfG): DSA-heavy, with DBMS and OOP.
- **Delhivery (DevOps, Glassdoor)**: Kubernetes concepts, debugging a connection to a pod, and bash scripting.
- **Cars24 (Glassdoor)**: DevOps questions appear only inside SDE/TPM loops (CI/CD, nginx, load balancing, deployment to error monitoring).
- **Increff (DevOps campus 2022, GfG)**: leaned on CS fundamentals (networking, SQL, DBMS) more than DSA.

### MNC India centres
- **Walmart Global Tech (DevOps/SRE)**: 5 rounds.
  1. HR screen with tool self-ratings.
  2. Technical: CI/CD, Kubernetes, AWS, Linux and shell.
  3. Late-night call with the US team: Prometheus/Grafana, Python automation, Kubernetes and AWS.
  4. "Managerial" round that was over 60% technical (Docker, Terraform, infrastructure).
  5. HR.
- **Atlassian (SRE)**: 5 rounds of about 60 minutes.
  1. Python coding: a subscription proration report (Jira-themed).
  2. SRE system design from architecture diagrams.
  3. SRE craft: monitoring with Prometheus and Grafana.
  4. Values.
  5. Managerial.
- **Amazon (DevOps/Support Engineer, 5 yrs, Dec 2020, GfG)**
  1. Hiring manager, behavioural (leadership principles).
  2. Linux fundamentals plus live shell scripting.
  3. Python coding plus leadership.
  4. Linux/API debugging.
  5. Bar raiser.
- **Apple (DevOps)**
  - GfG 2023: a 2-hour online assessment (scripting, CI/CD, Git, Terraform, Ansible), a DevOps tools round, a fault-tolerant system design round, behavioural and hiring manager.
  - Taro, India, May 2025: CoderPad MCQs plus 3 scripting questions, then a technical round with 2 easy programs (syntax checked), Linux memory and partition commands, specific GitHub commands, and Kubernetes/Docker Q&A.
- **Nutanix (SRE)**
  - 2020 intern (GfG): 60 MCQs, then a technical round (OSI, ARP, DHCP, Linux networking, virtualization), then technical plus managerial (df, du, free, vmstat, ping, traceroute, subnetting, deadlock).
  - 2023 (GfG): an MCQ test on networking, OS, Linux and cloud, then a personal interview with many real networking scenarios.
- **Cisco**
  - Cloud apps role (GfG, 2023 campus): an online assessment with a BFS problem, a DSA plus OOP round, and a project round that walked through the candidate's Terraform-on-AWS code.
  - Senior SRE (Medium snippet): a practical round on Terraform for EC2 and S3, then a 60%-technical managerial round (Docker image optimisation).
- **Adobe (SRE/DevOps, 2021, Taro)**: DSA (add two numbers stored as linked lists), Jenkins basics, Kafka basics and project discussion.
- **Siemens (DevOps, Bengaluru, Glassdoor)**: Linux shell, find, awk and sed, with more weight on Kubernetes and Docker.
- **GE (SRE/DevOps, Aug 2024, Taro)**: an architecture round, an SRE round, managerial and HR. No coding test.
- **NetApp (SRE, Bangalore, Dec 2024, LeetCode)**: a technical round on Kubernetes workloads and scheduling.
- **eBay (Cloud SRE, Bangalore, LeetCode)**: DSA coding rounds, then a bar raiser with a "type google.com" walkthrough, TCP vs UDP vs QUIC and a Kubernetes deep dive.
- **Google SRE-SE (Bangalore, L3, LeetCode)**: a Linux internals round on inodes, networking and performance commands.
- **Roku (Senior DevOps/SRE, Bengaluru, LeetCode)**: 6 rounds. Distributed systems, AWS/GCP, Kubernetes, system design and incident management.
- **Accenture (DevOps, 16 LPA)**
  - Medium snippet: Kubernetes internals, Terraform and CI/CD design.
  - Taro, Dec 2024: real-time scenarios, AWS and CI/CD.
- **IBM (DevOps, Bengaluru, 2023-2025)**: Kubernetes architecture, Docker, Linux, Python scripting and projects.
- **Capgemini (DevOps, Sep 2025, Taro)**: basic containerisation, Docker, AWS and pipelines.
- **HashedIn by Deloitte (DevOps-1, Dec 2022, GfG)**: an online assessment (2 array/string problems), then 2 technical rounds on AWS EC2/S3, Docker and Git commands, SQL and Python.
- **rtCamp (DevOps, GfG)**: a take-home assignment, then a Zoom technical round.
- **ZS Associates (Cloud Platform Eng, 2022, GfG)**: aptitude, then technical (Linux, TCP/UDP, DNS, cloud), then managerial.

**Pattern:**
- Campus and fresher loops are DSA plus CS fundamentals.
- Loops for 2-5 years of experience are tool deep dives (Kubernetes, AWS, CI/CD), troubleshooting scenarios and one live script.
- Senior and SRE loops add system design, SLO/incident handling and a mostly technical "managerial" round.

## Big tech

**Google SRE (two tracks).**
- **SRE-SE (Systems Engineer) track:** recruiter screen, then a phone screen (practical coding/scripting in a Google Doc, sometimes plus Linux questions). The onsite is about 5 x 45 min: Practical Coding/Scripting, NALSD, Linux/Unix internals, Troubleshooting, and Googleyness & Leadership [C1, C2, C6, C10].
- **SRE-SWE track:** about 3-4 algorithmic coding rounds plus system design. The 5th round can be swapped between Troubleshooting and Linux internals, or replaced by another coding round [C11].
- **Difficulty:** SE-track coding is "practical algorithms". Typical tasks are file and log processing, filesystem utilities, and ps/find-like tools, at roughly LC easy-medium with follow-ups on scale (huge files, streaming, memory limits). The SWE track uses the same bar as SWE (LC medium-hard).
- **Bengaluru Systems Engineer report (Nov 2024):** 5 onsite rounds covering OS, Networking, Scripting, Troubleshooting, and Behavioural/Leadership [C13].

**Meta Production Engineer.**
- Optional recruiter call, then an online MCQ assessment (about 18-30 questions on Linux, OS and networking, mainly for early-career candidates).
- **Two screens:** PE Coding (file parsing, plus sometimes an LC easy/medium) and PE Basics/Systems (Linux internals and a troubleshooting scenario).
- **Onsite:** 4 rounds: Coding (practical file-parsing with many follow-ups, or SWE-style LC medium), Systems/Linux internals, System design (infra-flavoured), and Behavioral. Team matching follows [C3, C4, C5, C7, C8].
- **Coding difficulty:** LC easy-medium. Practical parsing matters more than hard DS&A, and one candidate warns that Meta-tagged LC problems did not predict their questions [C21].
- **Network PE:** a separate variant with networking screens (BGP/OSPF, TCP) and a network design round [C29].

**Amazon DevOps Engineer / Systems Development Engineer (SysDE).**
- **Shape:** OA (for SysDE this can be easy log/string manipulation instead of DS&A [C17]), then 4-5 loop rounds.
- **Typical India DevOps loop (Bengaluru, Feb 2025):** OA, coding, an OS/Networking/Linux round, a Leadership Principles round, then the offer [C15].
- **Older Bangalore DevOps loop:** HM (LPs), Linux fundamentals with live shell scripting, Python coding, Linux debugging, and Bar Raiser [C14].
- **SysDE (US, 2025):** 2 x LC-medium coding plus 1 system design. Every round has 1-2 LP questions with deep follow-ups [C16].
- **Coding difficulty:** LC easy-medium. LPs are asked in essentially every round.

**Apple SRE / DevOps.**
- **Cupertino report:** HR screen, HM call, coding screen, then a virtual loop of about 4 rounds: Kubernetes-focused system design, reliability/availability design, coding, and Network stack + Linux internals [C18].
- **Other reports:** up to 6 remote technical interviews in one day. Coding is LC-medium or Python dict/list manipulation, followed by a short systems quiz (inode, load average, k8s control plane) [C19].
- **Character:** heavily team-dependent, often Kubernetes- and Python-centric.

**Microsoft SRE.**
- **Shape:** technical screen, then a same-day loop of about 4 rounds: system design, infra troubleshooting, coding (DS&A, often easy-medium HackerRank style), and a senior manager round that also includes troubleshooting [C20].
- **One team (EM on Blind):** "2 SRE + 1 SWE" rounds [C22].
- **2026 PracHub items:** live high-CPU troubleshooting, a SQL/text task, a linked-list merge, automation philosophy, and on-call behavioural [C23].
- **Senior:** Azure/cloud is probed in depth.

**Netflix SRE.**
- **Shape:** HM screen, then 4 rounds: system design, incident management, behavioral, and culture (the Netflix culture memo). One report says there was "no live coding" [C24].
- **Other notes:** Netflix is more likely than other FAANGs to run a security-focused design round [C25]. Very little SRE-specific data is public, and the sample is small.

**Uber SRE.**
- **Older reports (2017, the only SRE-specific ones):** phone screen with a short coding task plus web-architecture discussion. The onsite had a whiteboard architecture problem and an in-person Linux/troubleshooting interview [C26].
- **Glassdoor SRE page:** a screen-share coding session on data-set parsing [C26].
- **Current Uber SWE loops in Bengaluru:** DSA, machine coding, system design, and managerial rounds.

**Atlassian SRE.**
- **Shape:** 1 technical screen (coding), then a 5-round onsite: System Design, Coding, Values, Manager, and "SRE Craft". The craft round varies by profile [C27].
- **Coding:** LC medium. The interviewer watches how you test, because Atlassian has no QA team. Prompts are practical builds with hidden requirements, such as a URL router or a rate limiter.
- **Values round:** maps to Atlassian's 5 values.

**LinkedIn SRE.**
- **Shape:** HackerRank OA (MCQ on OS/networks/Linux/DBMS, SQL, 2 easy programs), then technical screens, then an onsite of about 5 parts.
- **Onsite parts:** system design, code review (find bugs in given code), service management/operations, troubleshooting (live broken Apache), and resume/HM [C28].
- **Coding:** easy-medium (a FizzBuzz variant, recursive API traversal, log parsing).

## System design and NALSD (big tech)

- [Google] [M] NALSD: estimate storage, traffic and server count for the design; watch for single points of failure (2022 onsite) [C1]
- [Google] NALSD: scale a service across datacentres; how many servers does the task need? Estimates within 2-3x are acceptable [C46]
- [Google] Design a system that stays reliable when some servers crash or get overloaded (Systems Engineer) [C13]
- [Google] Design a 3-tier architecture, then explain how you'd debug it [C2 (agg)]
- [Google] Design a caching server [C2 (agg)]
- [Google] Design a Netflix-like streaming engine [C2 (agg)]
- [Google] Migrate live users from NoSQL to SQL with no user impact [C2 (agg)]
- [Google] NALSD examples cited by blogs: chat for 10M users, thumbnail service, copying files to remote servers, AdWords [C50 (agg)]
- [Google] DR for a 5 PB cluster with a 4 h RTO (bandwidth napkin maths) [C6 (agg)]
- [Google] Metrics pipeline at 10M events/s [C6 (agg)]
- [Google] Feature-flag system that survives control-plane failure [C6 (agg)]
- [Google — official] SRE Workbook NALSD worked example: an AdWords click-through-rate pipeline (500k QPS, 10k clicks/s, under 5 min freshness), evolved from one machine to distributed (LogJoiner) to multi-datacentre with Paxos. This is study material, not an interview report [C51].
- [Google] [S] How would you improve log capture? (Senior SRE, Warsaw 2024) [C13]
- [Meta] Design a WhatsApp-like chat app; or pick any system and design it; LB, caching and TTL discussion [C7 (agg)]
- [Meta] Infra-flavoured design: log collection pipeline, config distribution service (given as examples) [C9 (agg)]
- [Meta] Design a system for billions of requests per day [C9 (agg)]
- [Meta] Design a practical system for a real-world problem (Dec 2024 onsite) [C4]
- [Meta/Apple] Large-scale Linux kernel update system with A/B rollout and auto-rollback [C37 (agg)]
- [Meta/Google] Large-scale file distribution [C37 (agg)]
- [Google/Microsoft] Distributed configuration management [C37 (agg)]
- [Google/Meta/Microsoft] Monitoring microservice health; distributed tracing [C37 (agg)]
- [Google/Meta] Public API rate limiter [C37 (agg)]
- [Meta Network PE] Design a basic network with a couple of routers and switches [C29]
- [Amazon SysDE] Design a certificate distribution system (2025) [C16]
- [Amazon SDE-II-adjacent] Device locator service [C52]
- [Apple] Reliability and availability design round [C18, C19]
- [Netflix] Design an unsubscribe flow for a website [C24]
- [Netflix] Security-only design round is more common than at other FAANGs [C25]
- [Microsoft] System design task in the loop [C20]
- [Uber] Moving-target software architecture problem on the whiteboard [C26]
- [Atlassian] Design rate limiting for an API (treated as the bar-raiser round) [C27]
- [LinkedIn] The app gets a million read requests. How do you handle it? [C53]
- [LinkedIn] Scale a ride-hailing app to about 10k requests/min [C53]
- [LinkedIn] Scale one of your own resume projects into a production architecture [C54]
- [Citadel, for comparison] Safe config and deploy for hundreds of services (2026) [C23]

## Behavioural questions by company

**Amazon (Leadership Principles, in every round; Bar Raiser)**
- [Amazon] Convincing a client: how did you persuade them? [C14]
- [Amazon] What does operational excellence mean to you? [C14]
- [Amazon] Features are incomplete and the deadline can't be met. What do you do? [C14]
- [Amazon] A conflict of interest with a coworker [C14]
- [Amazon] A time you went well beyond the bar, and how you'd improve it now [C14]
- [Amazon] Bar Raiser: handling ambiguous data [C14]
- [Amazon] Bar Raiser: a failure and what you learned [C14]
- [Amazon] Bar Raiser: a time you showed ownership, and what you'd do differently [C14]
- [Amazon] Bar Raiser: improving teammates' lives [C14]
- [Amazon] Explain your latest automation end to end, and how you'd improve it now [C14]
- [Amazon] Going above and beyond (Ownership / Customer Obsession) [C32]
- [Amazon] Working to a strict timeline without exploring all options [C32]
- [Amazon] Research-heavy work [C32]
- [Amazon] Reacting quickly at work [C32]
- [Amazon] Dive Deep (asked again for a work example after a college answer) [C32]

**Google (Googleyness & Leadership)**
- [Google] What does diversity mean to you? [C2 (agg)]
- [Google] A time you pivoted midway through work [C2 (agg)]
- [Google] A time your actions positively affected your team [C2 (agg)]
- [Google] Working in a diverse team: benefits, conflict, feedback [C2 (agg)]
- [Google] An opinion your team rejected [C10 (agg)]
- [Google] Working with a difficult colleague [C10 (agg)]
- [Google] Staying positive under pressure [C10 (agg)]
- [Google] A failure or mistake and how you corrected it [C10 (agg)]
- [Google] Describe a stressful incident you handled [C6 (agg; cites Glassdoor)]

**Meta (Meta values)**
- [Meta] How do you handle conflict? [C4]
- [Meta] How do you prioritise across projects? [C4]
- [Meta] A disagreement with a colleague [C4]
- [Meta] Learning a new technology fast [C8 (agg)]
- [Meta] Adapting to a big change [C8 (agg)]
- [Meta] Influencing a team decision [C9 (agg)]
- [Meta] Explaining tech to non-technical people [C9 (agg)]
- [Meta] Measurable impact [C9 (agg)]
- [Meta] Working with someone whose style differs from yours; why Meta? [C7 (agg)]

**Microsoft**
- [Microsoft] A problem that stumped you, your peers, and AI tools: how you solved it (2026) [C23]

**Atlassian**
- [Atlassian] For each of Atlassian's 5 values, an example, e.g. "Be the change you seek": an incident where you drove change [C27]
- [Atlassian] A process you identified and changed [C27]

**Netflix**
- [Netflix] Culture round based on the Netflix culture memo, plus a tough HM deep-dive into past projects [C24]

**LinkedIn**
- [LinkedIn] Why SRE? Walk through your resume roles [C54]

**Question count:** 246 question lines across sections 2.x. About 10 of them repeat the same question under a second topic (for example, the Amazon shell tasks appear under both Writing tasks and Shell scripting), so there are roughly 235 distinct questions. 107 lines are marked `(agg)` (aggregator sourcing). The rest come from first-hand candidate reports, Glassdoor or Blind snippets, or PracHub listings tagged with a company. About 10 lines describe a type of round rather than a single question.

## Big-tech preparation track (Jan - Apr 2027)

Realistic targets at ~2.5 years: Google SRE Systems Engineer (L3/L4), Meta
Production Engineer (E4), Amazon DevOps / Systems Development Engineer (L4/L5),
Apple SRE/Infrastructure. Ask the recruiter which track and rounds you will get;
Google's SE-SRE and SWE-SRE loops are very different.

What to add on top of the November topics:

| Area | What "ready" looks like | How |
|---|---|---|
| Coding | Solve easy problems in 15 min, mediums in 30-35 min, in Python, explaining complexity | ~150 problems (arrays, hash maps, strings, two pointers, stacks/queues, heaps, BFS/DFS, intervals). Plus practical tasks: write `tail`, `find`, `du`, log and CSV parsers, then handle "the file is 50 GB" and "it is a stream" |
| Linux internals | Explain fork/exec, signals, virtual memory, page cache, inodes, file descriptors, D-state, what happens in `ls`/`ps`/`malloc` | "How Linux Works" (Ward), Brendan Gregg's USE method and Linux performance pages, `strace`/`ltrace`/`perf` on your own machine |
| Networking | Walk through URL-to-render with DNS caching layers, TCP states, TLS 1.3, HTTP/2 and QUIC, load balancing; debug packet loss | "High Performance Browser Networking" (free online), `tcpdump`/Wireshark labs |
| Troubleshooting | Lead a 45-minute "site is down" scenario: mitigate first, ask for data, state hypotheses in order | Mock with a friend who changes the facts as you go; practise Part 3 out loud |
| NALSD / system design | Estimate QPS, storage, bandwidth and machine count; design for failure and defend the numbers | Google SRE Workbook chapter on NALSD, then 1 design per week out loud |
| SRE concepts | SLI/SLO/error budgets, toil, golden signals, postmortems, on-call | Google SRE Book (free online): chapters on SLOs, toil, monitoring, postmortems |
| Behavioural | 8-10 STAR stories mapped to each company's values (Amazon Leadership Principles, Googleyness, Meta values) | Reuse your MOOSE stories; write one story per principle and rehearse |

---

# Part 5: Your resume, study plan and sources

## Questions about your own resume (prepare these first)

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

## Study plan

There are far more questions here than one month allows. Each week, do the
**Core questions** for that week's topics fully, the **Writing tasks** and
**Debugging** items for those topics, and the **Reported** questions. Use the
**Additional questions** for revision and as a second pass in December.

### November 2026 (core DevOps)

About 2 hours on weekdays, 5-6 hours on weekends. Every day: 30 minutes on your
resume stories until they are smooth.

| Week | Weekdays | Weekend |
|---|---|---|
| 1 (1-8 Nov) | Linux core + networking; Git | Shell writing tasks; Linux and network debugging labs; one mock |
| 2 (9-15 Nov) | Docker; Python must-know list | Dockerfile and Python writing tasks; 10 easy coding problems |
| 3 (16-22 Nov) | Kubernetes (local kind or minikube cluster) | Kubernetes manifests from Part 2; break-and-fix labs from Part 3 |
| 4 (23-30 Nov) | GitHub Actions, Jenkins, AWS (IAM, VPC, EC2/ASG, S3, RDS) | Jenkinsfile and workflow writing tasks; AWS debugging; two full mocks |

### December 2026

Start applying (startups and Indian product companies first). Second pass on
"Additional questions", Section 10 topics (Terraform, observability), the AWS
portfolio project, and 3-4 coding problems a week.

### January - April 2027

Big-tech track (Part 4) alongside interviews.

Labs: `kind`/`minikube`, Killercoda scenarios, AWS free tier (set a billing alarm
first), GitHub Actions in a personal repo, a Jenkins container on your laptop.

## Sources

### Core bank

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

### Public question collections

- https://github.com/bregman-arie/devops-exercises (topics/kubernetes, topics/aws, topics/git, topics/containers, topics/cicd)
- https://raw.githubusercontent.com/bregman-arie/devops-exercises/master/topics/kubernetes/README.md
- https://raw.githubusercontent.com/bregman-arie/devops-exercises/master/topics/aws/README.md
- https://raw.githubusercontent.com/bregman-arie/devops-exercises/master/topics/git/README.md
- https://raw.githubusercontent.com/bregman-arie/devops-exercises/master/topics/containers/README.md
- https://raw.githubusercontent.com/bregman-arie/devops-exercises/master/topics/cicd/README.md
- https://github.com/trimstray/test-your-sysadmin-skills
- https://github.com/chassing/linux-sysadmin-interview-questions
- https://github.com/NotHarshhaa/DevOps-Interview-Questions (containers, cloud, ci-cd, linux-system-admin, automation-scripting, version-control, mock-interviews)
- https://docs.aws.amazon.com/eks/latest/best-practices/introduction.html
- https://docs.aws.amazon.com/eks/latest/best-practices/networking.html
- https://docs.github.com/en/actions/security-for-github-actions/security-guides/security-hardening-for-github-actions
- https://www.interviewbit.com/jenkins-interview-questions/
- https://www.geeksforgeeks.org/devops/docker-interview-questions/
- https://kube-dojo.github.io/k8s/cka/part3-services-networking/module-3.5-gateway-api/ (Gateway API on the CKA)
- https://kodekloud.com/community/t/cka-2025-exam-syllabus-tips-and-feedback-request/474245 (CKA 2025 task reports: Helm, CRDs, sidecars)

### Community and Indian-company reports

- https://www.geeksforgeeks.org/interview-experiences/zoho-interview-experience-for-devops-engineer/
- https://www.geeksforgeeks.org/interview-experiences/rtcamp-interview-experience-for-devops-engineer/
- https://www.geeksforgeeks.org/paytm-interview-experience-for-devops-engineer-on-campus-2022/
- https://www.geeksforgeeks.org/interview-experiences/paytm-interview-experience-for-devops-intern-on-campus-2024/
- https://www.geeksforgeeks.org/hashedin-by-deloitte-interview-experience-for-devops-engineer-1/
- https://www.geeksforgeeks.org/interview-experiences/inorg-global-interview-experience-associate-devops-engineer-fresher/
- https://www.geeksforgeeks.org/amazon-interview-experience-for-support-engineerdevops-5-years-experienced/
- https://www.geeksforgeeks.org/interview-experiences/apple-interview-experience-for-devops-engineer/
- https://www.geeksforgeeks.org/interview-experiences/zs-associates-cloud-interview-experience-for-platform-engineer-on-campus-2023/
- https://www.geeksforgeeks.org/makemytrip-interview-experience-set-15-devops-intern/
- https://www.geeksforgeeks.org/jivox-interview-experience-for-devops-engineer-on-campus/
- https://www.geeksforgeeks.org/increff-interview-experience-for-devops-on-campus-july-2022/ (search snippet)
- https://www.geeksforgeeks.org/interview-experiences/nutanix-interview-experience-for-sre-2023/
- https://www.geeksforgeeks.org/nutanix-virtual-interview-experience-2020-on-campus-for-sre-intern-role/
- https://www.geeksforgeeks.org/interview-experiences/cohesity-interview-experience-for-site-reliability-engineer-role-on-campus-2024/
- https://www.geeksforgeeks.org/interview-experiences/linkedin-internship-interview-experience-for-sre-site-reliability-engineer/
- https://www.geeksforgeeks.org/interview-experiences/media-net-directi-interview-experience-for-sre-role-on-campus/
- https://geeksforgeeks.org/cisco-interview-experience-sde-for-cloud-applications-6-month-internship-fte-on-campus
- https://www.geeksforgeeks.org/interview-experiences/harsco-enviri-interview-experience-for-cloud-engineer-on-campus/ (search snippet)
- https://www.geeksforgeeks.org/devops/devops-interview-questions/
- https://www.geeksforgeeks.org/devops/kubernetes-interview-questions/
- https://www.geeksforgeeks.org/devops/docker-interview-questions/
- https://www.geeksforgeeks.org/devops/jenkins-interview-questions/
- https://www.geeksforgeeks.org/linux-unix/linux-interview-questions/
- https://www.geeksforgeeks.org/git/git-interview-questions-and-answers/
- https://www.geeksforgeeks.org/aws-interview-questions/
- https://www.jointaro.com/interviews/companies/flipkart/experiences/devops-engineer-bengaluru-september-13-2023-no-offer-positive-bc92dd7a/
- https://www.jointaro.com/interviews/companies/flipkart/experiences/sredevops-engineer-bengaluru-november-1-2020-no-offer-negative-31ed98c6/
- https://www.jointaro.com/interviews/companies/general-electric/experiences/sredevops-engineer-bengaluru-karnataka-august-1-2024-no-offer-positive-07a64f2d/
- https://www.jointaro.com/interviews/companies/adobe/experiences/sredevops-engineer-bengaluru-september-1-2021-no-offer-neutral-0f62f91b/
- https://www.jointaro.com/interviews/companies/apple/experiences/devops-engineer-india-may-1-2025-no-offer-negative-1a14bc36/
- https://www.jointaro.com/interviews/companies/accenture/experiences/devops-engineer-bengaluru-karnataka-december-19-2024-accepted-offer-positive-ed142ce8/ (search snippet)
- https://www.jointaro.com/interviews/companies/capgemini/experiences/devops-engineer-india-september-5-2025-accepted-offer-positive-fc18b190/ (search snippet)
- https://www.jointaro.com/interviews/companies/ibm/experiences/devops-engineer-bengaluru-october-19-2023-accepted-offer-positive-1664184e/ (search snippet)
- https://apurv-gujjar.me/my-devops-interview-experience-and-questions-2025
- https://dev.to/iragani_vamsisai_5a349cc/i-recently-attended-a-devops-interview-here-are-the-questions-they-asked-me-3k25
- https://dev.to/deadlock/sre-devops-interview-questions-linux-troubleshooting-4878
- https://arvindverma021.substack.com/p/devops-interview-questions-from-a
- https://www.placementpreparation.io/blog/devops-engineer-interview-experience/
- https://www.linkedin.com/posts/praveen-singampalli_my-walmart-devopssre-interview-experience-activity-7105021503603175424-mEaK
- https://www.linkedin.com/posts/praveen-singampalli_devopssre-atlassian-interview-guide-activity-7169684893818454016-mfPV
- https://www.linkedin.com/posts/ayush-singh-rathore-aa1953156_atlassian-sre-devops-activity-7393882674312097792-7UCX
- https://www.teamblind.com/post/atlassian-sre-coding-interview-questions-i3428xz7
- https://meetassist.io/interview-questions/devops-engineer
- https://github.com/vigneshakaviki/kubernetes-interview-prep (and questions/README.md)
- https://livingdevops.com/devops-interview/12-kubernetes-scenario-based-interview-questions-for-devops/
- https://hirestepx.com/blog/devops-engineer-interview-questions-india-2026
- https://news.ycombinator.com/item?id=2010895
- https://news.ycombinator.com/item?id=17857285 (search snippet)
- Search snippets only (pages returned 403):
  - https://www.naukri.com/code360/interview-experiences/shiprocket/shiprocket-interview-experience-may-2022-exp-0-2-years
  - https://www.naukri.com/code360/interview-experiences/sharechat/interview-experience-jan-2022-exp-0-2-years-2-1900
  - https://www.naukri.com/code360/interview-experiences/phonepe/phone-pe-interview-experience-dec-2021-exp-0-2-years
  - https://leetcode.com/discuss/post/6160124/ (NetApp)
  - https://leetcode.com/discuss/interview-experience/6006265/Google-SRE-SE-or-Bangalore-or-L3-or-Rejected/
  - https://leetcode.com/discuss/post/8159896/ebay-bangalore-cloud-sre-interview-exper-vnhk/
  - https://leetcode.com/discuss/post/2001782/Robin.io-or-SDE2-or-Bangalore-or-March-2022-or-Offer/
  - https://leetcode.com/discuss/post/7414036/ (Roku)
  - https://medium.com/@sre-devops-interview/walmart-devops-sre-interview-experience-questions-02d679b12525
  - https://medium.com/@sre-devops-interview/accenture-devops-engineer-interview-experience-7c2ab4b88e9d
  - https://medium.com/@sre-devops-interview/ibm-devops-engineer-interview-experience-a31e4a04cb23
  - https://medium.com/@sre-devops-interview/vmware-sre-interview-experience-questions-be9016140c31
  - https://medium.com/@sre-devops-interview/cisco-senior-site-reliability-engineer-sre-interview-experience-e6500986a7c1
  - https://medium.com/@techrecruiter/devops-engineer-interview-experience-livspace-b36224fb4ec1
  - https://www.glassdoor.co.in/Interview/Dream11-Devops-Engineer-Interview-Questions-EI_IE493359.0,7_KO8,23.htm
  - https://www.glassdoor.com/Interview/Meesho-Interview-Questions-E1503776.htm?filter.jobTitleExact=Software+Development+Engineer(DevOps)
  - https://www.glassdoor.co.in/Interview/Myntra-Devops-Engineer-Interview-Questions-EI_IE508705.0,6_KO7,22.htm
  - https://www.glassdoor.com/Interview/PhonePe-SRE-Interview-Questions-EI_IE2289138.0,7_KO8,11.htm
  - https://dataford.io/interview-guides/phonepe/site-reliability-engineer
  - https://www.glassdoor.com/Interview/JUSPAY-SRE-DevOps-Engineer-Interview-Questions-EI_IE1102352.0,6_KO7,26.htm
  - https://www.glassdoor.com/Interview/Freshworks-Interview-Questions-E1680273_P6.htm
  - https://www.glassdoor.co.in/Interview/Zoho-Devops-Engineer-Interview-Questions-EI_IE328604.0,4_KO5,20.htm
  - https://www.glassdoor.co.in/Interview/Delhivery-Devops-Engineer-Interview-Questions-EI_IE513409.0,9_KO10,25.htm
  - https://www.glassdoor.co.in/Interview/UKG-Senior-DevOps-Engineer-Interview-Questions-EI_IE7900.0,3_KO4,26.htm
  - https://www.glassdoor.com/Interview/Siemens-Devops-Engineer-Interview-Questions-EI_IE3510.0,7_KO8,23.htm
  - https://www.glassdoor.com/Interview/Persistent-Systems-Devops-Engineer-Interview-Questions-EI_IE150639.0,18_KO19,34.htm
  - https://clear.glassdoor.nl/Interview/Flipkart-Devops-Engineer-Interview-Questions-EI_IE300494.0,8_KO9,24.htm
  - https://www.glassdoor.com/Interview/bangalore-devops-engineer-interview-questions-SRCH_IL.0,9_IM1091_KO10,25.htm
  - https://www.glassdoor.co.uk/Interview/-live-coding-write-a-python-script-to-parse-a-log-file-strip-splitting-on-string-Use-of-dictionary-to-aggregate-data-QTN_5096654.htm
  - https://www.glassdoor.com/Interview/cars24-Interview-Questions-E1316104.htm
- Not reachable: Reddit, Stack Overflow, Server Fault (crawler-blocked). AmbitionBox returned 403. No Fishbowl or Blind DevOps content was found beyond the one Atlassian Blind thread.

### Big-tech reports (codes such as [C5] in Parts 1-4 refer to this list)

- C1  https://bala-krishnan.com/posts/google-sre-onsite/ (Google Systems SRE onsite, 2022, candidate)
- C2  https://www.coditioning.com/blog/17/google-sre-interview-questions (aggregated; "candidate-reported" section)
- C3  https://www.jointaro.com/interviews/companies/meta/experiences/production-engineer-united-states-may-1-2025-no-offer-positive-a274e3d5/
- C4  https://bala-krishnan.com/posts/18-meta/ (Meta PE, Dec 2024, candidate)
- C5  https://reliabilitywhisperer.substack.com/p/2025meta-e4-production-engineer-interview (Meta E4 PE 2025, candidate)
- C6  https://ewry.net/devops-sre-interview-questions/google-sre-interview-questions/ (aggregated; per-question attribution)
- C7  https://www.coditioning.com/blog/8/meta-production-engineer-interview-guide-ii
- C8  https://www.coditioning.com/blog/7/meta-production-engineer-interview-guide
- C9  https://www.linkjob.ai/interview-questions/meta-production-engineer-interview/ (candidate blog, 2026; the author admits using an AI copilot, so lower trust)
- C10 https://www.aced.io/guides/google-site-reliability-engineer-interview
- C11 https://www.teamblind.com/post/google-sre-onsite-troubleshooting-or-os-internals-wgndkhyr (plus Blind search-result snippets on the SE vs SWE track and the scripting round)
- C13 Glassdoor Google Systems Engineer / SRE / Senior SRE pages, via search snippets: https://www.glassdoor.com/Interview/Google-Systems-Engineer-Interview-Questions-EI_IE9079.0,6_KO7,23.htm , https://www.glassdoor.com/Interview/Google-Site-Reliability-Engineer-Interview-Questions-EI_IE9079.0,6_KO7,32.htm , https://www.glassdoor.com/Interview/Google-Senior-Site-Reliability-Engineer-Interview-Questions-EI_IE9079.0,6_KO7,39.htm
- C14 https://www.geeksforgeeks.org/amazon-interview-experience-for-support-engineerdevops-5-years-experienced/ (Amazon DevOps Bangalore; older but detailed)
- C15 https://www.glassdoor.com/Interview/Amazon-Devops-Engineer-Interview-Questions-EI_IE6036.0,6_KO7,22.htm (snippet: Feb 2025 Bengaluru loop)
- C16 https://www.jointaro.com/interviews/companies/amazon/experiences/systems-development-engineer-north-reading-ma-may-4-2025-no-offer-positive-1638e969/
- C17 https://www.teamblind.com/post/amazon-sysde-l5-interview-loop-a7xxjt4y
- C18 https://medium.com/@sre-interview/my-apple-sre-interview-experience-cupertino-ca-e32d79e784b4 (via search snippet; fetch blocked)
- C19 Glassdoor Apple SRE / SRE Engineer / SRE Manager pages, via search snippets: https://www.glassdoor.com.au/Interview/Apple-Site-Reliability-Engineer-Interview-Questions-EI_IE1138.0,5_KO6,31.htm , https://www.glassdoor.sg/Interview/Apple-SRE-Engineer-Interview-Questions-EI_IE1138.0,5_KO6,18.htm , https://www.glassdoor.sg/Interview/Apple-SRE-Manager-Interview-Questions-EI_IE1138.0,5_KO6,17.htm
- C20 Glassdoor Microsoft SRE / Senior SRE, via snippets: https://www.glassdoor.co.in/Interview/Microsoft-Site-Reliability-Engineer-Interview-Questions-EI_IE1651.0,9_KO10,35.htm , https://www.glassdoor.com/Interview/Microsoft-Senior-Site-Reliability-Engineer-Interview-Questions-EI_IE1651.0,9_KO10,42.htm
- C21 Meta PE LeetCode Discuss and Glassdoor snippets: https://leetcode.com/discuss/post/6475453/Meta-Production-Engineer/ , https://leetcode.com/discuss/post/7214157/ , https://www.glassdoor.com/Interview/Meta-Production-Engineer-Interview-Questions-EI_IE40772.0,4_KO5,24.htm (content via search snippets; direct fetch blocked)
- C22 https://www.teamblind.com/post/Site-Reliability-Engineer-SRE-Interview---Microsoft-ufhnnfsR
- C23 https://prachub.com/positions/site-reliability-engineer (pages 1-2), https://prachub.com/companies/meta/positions/site-reliability-engineer , https://prachub.com/companies/microsoft/positions/site-reliability-engineer , https://prachub.com/interview-questions/implement-a-streaming-vmstat-alert
- C24 https://www.glassdoor.com/Interview/Netflix-Site-Reliability-Engineer-Interview-Questions-EI_IE11891.0,7_KO8,33.htm (snippet)
- C25 https://interviewing.io/guides/hiring-process/netflix (snippet)
- C26 https://www.glassdoor.com/Interview/Uber-Site-Reliability-Engineer-Interview-Questions-EI_IE575263.0,4_KO5,30.htm , https://www.glassdoor.com/Interview/Uber-SRE-Interview-Questions-EI_IE575263.0,4_KO5,8.htm , https://www.jointaro.com/interviews/companies/uber/experiences/site-reliability-engineer-san-francisco-ca-may-1-2017-no-offer-neutral-9d1c86c7/ (snippets)
- C27 https://www.glassdoor.co.in/Interview/Atlassian-Site-Reliability-Engineer-Interview-Questions-EI_IE115699.0,9_KO10,35.htm and the .com/.com.au variants (snippets)
- C28 https://www.jointaro.com/interviews/companies/linkedin/experiences/site-reliability-engineer-new-delhi-february-1-2021-no-offer-negative-2fe28886/ ; https://www.glassdoor.com/Interview/LinkedIn-Site-Reliability-Engineer-Interview-Questions-EI_IE34865.0,8_KO9,34.htm (snippet)
- C29 https://www.glassdoor.co.in/Interview/Meta-Network-Production-Engineer-Interview-Questions-EI_IE40772.0,4_KO5,32.htm (snippet)
- C30 https://www.teamblind.com/post/meta-production-engineer-in-2024-knqcxfrt
- C31 https://www.glassdoor.com/Interview/Google-Site-Reliability-Engineer-Interview-Questions-EI_IE9079.0,6_KO7,32_IP4.htm (snippet)
- C32 https://www.geeksforgeeks.org/interview-experiences/amazon-interview-experience-for-system-development-engineer-exp-6-months/
- C33 https://dataford.io/interview-guides/amazon/systems-engineer and Glassdoor Amazon Linux pages (snippets, agg)
- C34 https://www.teamblind.com/post/in-an-sre-interview-how-do-you-answer-this-classic-question-rv2bhkdr
- C35 https://www.glassdoor.com/Interview/Amazon-Systems-Development-Engineer-II-Interview-Questions-EI_IE6036.0,6_KO7,38.htm (snippet)
- C36 https://yumminhuang.github.io/note/sreinterview/ (SRE phone screen; company not named)
- C37 https://reliabilitywhisperer.substack.com/p/production-engineer-system-design (community list, company tags unverified)
- C38 Older Google SRE Glassdoor reports (2010-2014) via search snippets; kept only as classics
- C39 https://www.teamblind.com/post/how-is-the-interview-for-production-engineers-like-at-facebook-qaahvrbt
- C40 https://underpaid.medium.com/i-received-sre-offers-from-facebook-and-google-without-a-university-degree-here-is-how-224f06b49e7d (snippet; fetch blocked)
- C41 https://www.glassdoor.com/Interview/Meta-Production-Engineer-Intern-Interview-Questions-EI_IE40772.0,4_KO5,31.htm and the cited candidate blog (snippet)
- C42 https://www.geeksforgeeks.org/interview-experiences/amazon-interview-experience-for-system-development-engineer-on-campus-dec-2020/ (snippet)
- C43 Meta PE systems-round snippet (Blind / prep pages)
- C44 https://www.teamblind.com/post/e4-production-engineer-meta-onsite-interview-question-jtto1oaa
- C45 https://reliabilitywhisperer.substack.com/p/devopssre-coding-interview-levels
- C46 https://deavid.wordpress.com/2019/12/08/interviewing-for-googles-sre/
- C47 https://dataford.io/interview-guides/apple/site-reliability-engineer (agg)
- C48 https://www.cleverprep.com/companies/meta/production-engineer (editorial; one item only)
- C49 https://dataford.io/interview-guides/microsoft/site-reliability-engineer (agg)
- C50 https://dev.to/aceinterviews/google-sre-nalsd-round-a-real-interview-walkthrough-mh2 and https://igotanoffer.com/blogs/tech/google-site-reliability-engineer-interview (snippets; promotional)
- C51 https://sre.google/workbook/non-abstract-design/ (official Google SRE Workbook)
- C52 https://www.teamblind.com/post/amazon-sde-2-loop-interview-experience-us-regions-jcczglcs (snippet)
- C53 https://www.glassdoor.com/Interview/LinkedIn-Site-Reliability-Engineer-Interview-Questions-EI_IE34865.0,8_KO9,34.htm (snippet)
- C54 https://www.geeksforgeeks.org/linkedin-internship-interview-experience-for-sre-site-reliability-engineer/
