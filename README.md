## ⚙️ Intended Behavior (How It Should Work)

When the application is running correctly on a supported host, here is exactly what it does:

* **Elevates Privileges Safely:** On launch, it prompts for Administrator access. This is strictly required so that the Python backend can interact with the Windows Registry (`HKLM`) to manage hardware identity virtualization.
* **Deploys a Guardian Watchdog:** It automatically spawns a silent, background companion process. If the main interface is closed, crashes, or gets killed, this watchdog instantly rolls back your Windows `MachineGuid` to its original, true value so your system isn't left modified.
* **Coordinates Interface & Binary:** The user interface toggles system flags. Clicking "Identity Cycle" or "HWID Mask" generates a brand-new, valid cryptographic UUIDv4 on the fly and updates your registry footprint instantly.
* **Purges Tracking Caches:** The cache cleanup button searches your local application directories (like `%LOCALAPPDATA%\Roblox`) and force-deletes log files, telemetry dumps, and cached session keys while safely bypassing locks on files currently in use by active game loops.

---

## ⚠️ Potential Breakages (How It Might Not Work)

Like any system-level privacy utility, certain edge cases, configurations, or environmental conflicts can cause the app to fail or misbehave. Here is what to look out for:

* **Permission Denied Errors (No Admin Rights):** If a user bypasses or denies the User Account Control (UAC) prompt, or if local group policies restrict registry writes, clicking the identity rotation switches will fail or show a "Permission Denied" error because `HKLM` cannot be modified by standard users.
* **The "Black and White / Broken UI" Bug:** If the system is running an environment without the modern Edge WebView2 Runtime installed (like bare-bones Windows Server builds, custom stripped ISOs, or deeply outdated Windows 10 versions), the UI fallback breaks, which is why on my tests on vm, it gave me half a million cdn and definition errors. The ancient legacy engine (`mshtml`) cannot read the CSS variables or modern JavaScript syntax, causing a completely unstyled, non-functional application frame.
* **Ghost Watchdog Processes:** If a hard system crash occurs or if the background watchdog process is aggressively terminated by third-party optimization software before it can execute its cleanup thread, your original hardware ID backup might not restore automatically on exit, so do not turn on the hwid spoofer for now.
* **CDN Isolation Failures:** The current visualization component (Chart.js) is linked externally. If a user has a strict firewall, a misconfigured local proxy, or is completely offline during launch, the analytics chart script will fail to load, blocking subsequent interface actions from initializing properly.
