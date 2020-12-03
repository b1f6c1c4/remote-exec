# remote-exec: Run command on remote server with sshfs

## TL;DR

So if you have a powerful server and you want to run some heavy job
on your local file using the CPU/memory/GPU/etc. on that server,
you can just type:

```bash
mkdir magic
cat >magic/.rmt-config <<EOF
RMT_HOST=<user>@<host>
RMT_RENV='PATH=/home/<user>/.local/bin/:/home/<user>/.local/sbin/:/usr/local/bin:/usr/bin'
RMT_RSHELL=/usr/bin/zsh
EOF

. , magic   # 1) Create a temp dir on remote machine
            # 2) Map it using sshfs into `magic` on local machine
            # 3) Locally `cd` into the mapped `magic`

, <cmd>     # Run a simple command on remote machine

            # Run a complex command on remote machine
, [NAME=VALUE]... <command> <args>...

,           # Launch a remote shell

. ,         # 1) Stop mapping `magic`
            # 2) Keep the temp dir on remote machine for future use
            # 3) Locally `cd` into the parent of `magic`
```

## Configuration

- `RMT_HOST`: (Required) The remote machine
- `RMT_SSH`: (Optional) Override command line for ssh, default `ssh -Y -t`
- `RMT_SSHFS`: (Optional) Override command line for sshfs, default `sshfs`
- `RMT_RENV`: (Optional) Will be added to `/usr/bin/env` **on remote machine**
- `RMT_RSHELL`: (Optional) Override login shell **on remote machine**, can be automatically detected
- `RMT_RDIR`: (Optional) A path **on remote machine** relative to `$HOME` to store your data, default `sshfs`

## Limitation

Quotation and connection is a mess.
If your command contains `$`, escape carefully.
There's absolutely no gurantee that any kind of escape will or will not work.
If you want to run any command containing `&&`, `||`, or `|`, type `,` and use the remote shell.

## License

MIT
