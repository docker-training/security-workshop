# Docker Security Workshop: Seccomp Lab
`seccomp` (short for **secure computing mode**)  is a sandboxing facility in the Linux kernel that acts like a firewall for system calls (syscalls). It uses Berkeley Packet Filter (BPF) rules to filter syscalls and control how they are handled. These filters can significantly limit a containers access to the Docker Host's Linux kernel - especially for simple containers/applications.

## You will complete the following steps in this lab:

- [Step 1 - Clone the labs GitHub repo](step1.md)
- [Step 2 - Test a `seccomp` profile](step2.md)
- [Step 3 - Run a container with no `seccomp` profile](step3.md)
- [Step 4 - Selectively remove syscalls](step4.md)
- [Step 5 - Write a `seccomp` profile](step5.md)
- [Step 6 - A few Gotchas](step6.md)
- [Summary](finish.md)
