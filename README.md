# BCC LSM Scripts (KRSI)

These scripts were written to provide working examples of Kernel Runtime Security Instrumentation (KRSI). Why use KRSI? Because it lets you write custom, modular programs to roll your own mandatory access control. Think of it like using a surgeon's scalpel to carve out the behavior you don't want. It can be a valuable replacement in environments where SELinux causes too large of a performance impact.

You can read my paper about KRSI in [The SANS Reading Room](https://www.sans.org/reading-room/whitepapers/linux/paper/40010). The paper explains these scripts and includes a tutorial in Appendix A. There are still very few examples of KRSI usage in the wild. This will hopefully change when `bpftrace` adds support for it and makes it easier to use.

KRSI has undergone a bit of a naming crisis. It is also called "LSM Probes" and "LSM BPF Hooks" in various projects. All these names refer to the same thing. My scripts use BPF Compiler Collection (BCC) to write programs that leverage KRSI. So I decided to name this repo "BCC LSM Scripts." I know, I'm perpetuating the problem.

## Dependencies

* Linux Kernel: 5.7 or newer
* BPF Compiler Collection (BCC): 0.15.0 or newer

The `lsm=` kernel parameter specifies a comma-delimited string of LSMs to enable at runtime. Add `bpf` to enable KRSI.

## Summary of Scripts

The following table gives a quick summary of each script.

| **Script Name** | **Description** |
| ----------- | ----------- |
| **`mac_fileperms`** | Controls the creation of files with "Set UID" or "Writeable By Others" permission bits. |
| **`mac_killtasks`** | Controls where kill signals are allowed to be sent |
| **`mac_skconnections`** | Controls the destinations to which IPv4 socket connections can be made |
| **`mac_sshlisteners`** | Controls locally listening SSH proxies (look up the -D and -L flags of SSH) |
| **`mac_suidexec`** | Controls the execution of files that have the "Set UID" permission bit set |


## Universal Options

| **Short Flag** | **Long Flag** | **Description** |
| ----------- | ----------- |  ----------- |
| **`-h`** | **`--help`** | Print all options of a script and provide example usage |
| **`-A`** | **`--allow`** | Define an "Allow" policy |
| **`-D`** | **`--deny`** | Define a "Deny" policy |
| **`-u`** | **`--user`** | Apply the policy to a specific user |
| **`-U`** | **`--exclude-user`** | Exclude a specific user from the policy |

## Script-Specific Options

| **Script** | **Short Flag** | **Long Flag** | **Description** |
| ----------- | ----------- | ----------- | ----------- |
| **`mac_killtasks`** | **`-k`** | **`--kernel`** | Apply the policy to kernel signals as well (*dangerous*) |
| **`mac_killtasks`** | **`-e`** | **`--eternal`** | Block all kill signals to the process that loaded this policy |
| **`mac_killtasks`** | **`-p`** | **`--source-pid`** | Apply the policy to a source PID |
| **`mac_killtasks`** | **`-P`** | **`--exclude-source-pid`** | Exclude a source PID from the policy |
| **`mac_killtasks`** | **`-t`** | **`--target-pid`** | Apply the policy to a target PID |
| **`mac_killtasks`** | **`-T`** | **`--exclude-target-pid`** | Exclude a target PID from the policy |
| **`mac_skconnections`** | **`-m`** | **`--mask`** | The subnet mask for an allow/deny policy |
| **`mac_suidexec`** | **`-f`** | **`--file`** | Apply the policy to a file |
| **`mac_suidexec`** | **`-F`** | **`--exclude-file`** | Exclude a file from the policy |

Also, mac_skconnections takes a required positional `[ip]` argument to define which IP or subnet the policy applies to.
