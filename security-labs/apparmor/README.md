# Docker Security Workshop: AppArmor Lab

`AppArmor` (Application Armor) is a Linux Security Module (LSM). It protects the operating system by applying profiles to individual applications. In contrast to managing capabilities with `CAP_DROP` and syscalls with `seccomp`, `AppArmor` allows for much finer-grained control. For example, AppArmor can restrict file operations on specified paths.

In this lab you will learn the basics of `AppArmor` and how to use it with Docker for improved security.

## You will complete the following steps in this lab:

- [Step 1 - `AppArmor` primer](step1.md)
- [Step 2 - The default-docker `AppArmor` profile](step2.md)
- [Step 3 - Running a Container without an `AppArmor` profile](step3.md)
- [Step 4 - `AppArmor` and defense in depth](step4.md)
- [Step 5 - Custom `AppArmor` profile](step5.md)
- [Step 6 - Extra for experts](step6.md)
- [Summary](finish.md)
