
`seccomp` (short for **secure computing mode**)  is a sandboxing facility in the Linux kernel that acts like a firewall for system calls (syscalls). It uses Berkeley Packet Filter (BPF) rules to filter syscalls and control how they are handled. These filters can significantly limit a containers access to the Docker Host's Linux kernel - especially for simple containers/applications.

## You will complete the following steps in this lab:

- Step 1 - Clone the labs GitHub repo
- Step 2 - Test a `seccomp` profile
- Step 3 - Run a container with no `seccomp` profile
- Step 4 - Selectively remove syscalls
- Step 5 - Write a `seccomp` profile
- Step 6 - A few Gotchas
