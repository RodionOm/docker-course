# Docker on Windows: setup notes

1. Check virtualization: Task Manager → Performance → CPU → Virtualization: Enabled
2. `wsl --install` → restart
3. Limit WSL memory: `%UserProfile%\.wslconfig`
   [wsl2]
   memory=3GB
   processors=4
   swap=2GB
4. Problem: "WSL2 is unable to start since virtualization is not enabled"
   Cause: hypervisor not launched at boot
   Fix: `bcdedit /set hypervisorlaunchtype auto` → restart
5. Install Docker Desktop (WSL 2 backend)
6. Verify: `docker version` (Client + Server), `docker run hello-world`