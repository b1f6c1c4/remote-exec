# remote-exec: Run command on remote server with sshfs and overlayfs

## TL;DR

So if you have a powerful server and you want to run some heavy job
on your local file using the CPU/memory/GPU/etc. on that server,
you can just type:

```bash
cat >.rmt-config <<EOF
RMT_HOST=<user>@<host>
RMT_RSHELL=/usr/bin/zsh
EOF
, <the-command-you-want-to-run>
```

## Configuration

- [ ] `RMT_HOST`: (Required) The remote machine
- [ ] `RMT_RSHELL`: (Optional) Override login shell **on remote machine**
- [ ] `RMT_RDIR`: (Optional) A path **on remote machine** relative to `$HOME` to store your data
