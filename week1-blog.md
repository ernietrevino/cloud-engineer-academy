Week 1 — Cloud Engineer Academy

Week 1: AWS account hardening, IAM fundamentals, and networking basics on macOS
Setting up a production-grade AWS account from scratch, tracing internet routing, and bootstrapping a macOS dev environment.

Week 1 covered AWS account setup and security hardening, cloud service models, HTTP fundamentals using curl, traceroute analysis, and local dev environment setup on macOS. Here's what I built and what I learned.

AWS account setup and IAM hardening
First task was creating an AWS root account and immediately locking it down. The root user has unrestricted access to all AWS services and billing — it should never be used for day-to-day operations. Standard practice is to create an IAM user for all regular work and restrict root access to account-level tasks only.

I created an IAM user with the following policies attached:

AdministratorAccess (AWS managed - job function) AWSBillingConductorFullAccess (AWS managed) Billing (AWS managed - job function) IAMUserChangePassword (AWS managed)
Note: AdministratorAccess already grants full billing permissions — AWSBillingConductorFullAccess and Billing are redundant here. Best practice is least privilege — only attach what's needed.
MFA was enabled on both root and IAM user via Google Authenticator (TOTP virtual device). This ensures that even with compromised credentials, account access requires physical possession of the registered device.

Budget alerts and cost anomaly detection
Set up a $100 monthly cost budget with three alert thresholds:

Alert 1: Actual cost > 85% ($85.00) → email notification Alert 2: Forecasted cost > 100% ($100.00) → email notification Alert 3: Actual cost > 100% ($100.00) → email notification
Important distinction: AWS budget alerts are notifications only — they do not enforce spending limits or terminate resources automatically. To enforce hard limits, you need to configure Budget Actions with IAM or SCP policies attached. Cost Anomaly Detection was also automatically enabled, which uses ML to flag statistically anomalous spend patterns across services.

HTTP fundamentals with curl
Used curl to make GET requests and inspect HTTP response headers directly from the terminal:

curl https://example.com curl https://example.com -o example.html curl -I https://example.com
The -I flag fetches only the response headers. Output from example.com:

HTTP/2 200 date: Sat, 04 Apr 2026 19:57:36 GMT content-type: text/html server: cloudflare last-modified: Tue, 24 Mar 2026 22:06:31 GMT cf-cache-status: HIT
Key status codes to know: 200 (OK), 301 (Moved Permanently), 404 (Not Found). The cf-cache-status: HIT confirms Cloudflare served this from cache rather than origin.

Traceroute — analyzing network path to google.com
Ran traceroute to map the network path from my machine to Google's infrastructure:

traceroute google.com
Results showed 23 hops total. Notable observations:

Hop 1: 10.40.0.1 — local gateway/home router Hop 2: 10.56.6.152 — local ISP infrastructure Hop 5: 68.1.0.242 — Cox Communications (ISP handoff) chnddbbrc01-pos0202.rd.ph.cox.net Hop 10: 216.239.63.232 — Google backbone entry Hop 23: 192.178.155.139 — google.com destination yuiadrs-in-f139.1e100.net
Hops 14-22 returned * * * — Google's internal routers are configured to drop ICMP TTL-exceeded messages, which is standard practice for hiding internal network topology. Total RTT to destination was ~41ms from the Phoenix metro area.

macOS dev environment setup
Bootstrapped a clean macOS development environment in three steps:

xcode-select --install /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)" brew install --cask visual-studio-code
After Homebrew installation, added it to PATH by appending to ~/.zprofile:

echo >> ~/.zprofile echo 'eval "$(/opt/homebrew/bin/brew shellenv zsh)"' >> ~/.zprofile eval "$(/opt/homebrew/bin/brew shellenv zsh)"
Verified with brew --version → Homebrew 5.1.3. VS Code 1.114.0 installed and confirmed reachable via code --version.

Key takeaways
Root account = account administration only. MFA on everything. Budget alerts are not hard limits — configure Budget Actions if you need enforcement. Traceroute is a powerful first-line tool for diagnosing network latency and routing issues. And Homebrew makes macOS dev environment setup significantly faster than manual installs.

Week 2 coming up — will be covering Git workflows, Python fundamentals, and deeper AWS services.

AWS IAM DevOps Networking macOS CloudEngineerAcademy 100DaysOfCloud