# `,`: Run commands on remote server with sshfs

## Requirements

- `,`: Run commands on remote server with files stored remotely
    - Local: `ssh`, `sshfs`, `/usr/bin/printf`, `sha1sum`, `realpath`
    - Remote: `sshd`, `/usr/bin/env`
- `,,`: Run commands on remote server with (existing) files stored locally
    - Local: `ssh`, `sshd`, `/usr/bin/printf`, `sha1sum`, `realpath`
    - Remote: `ssh`, `sshd`, `/usr/bin/env`, `sshfs`

## Usage - `,`

1. On your local machine, create an empty folder called `magic`:

    ```bash
    mkdir magic
    ```

1. Create a config file in the folder:

    ```bash
    echo 'RMT_HOST=<user>@<host>' > magic/.rmt-config
    ```

1. Make the `magic` folder magic by:

    ```bash
    . , magic
    ```

1. Now you can operate on the folder from both side in one single shell:

    ```bash
    vim Makefile   # Edit files using local machine
    , make -j64    # Run heavy jobs using remote machine
    ```
    Note: Files are stored remotely but is visible locally thanks to `sshfs`.

1. You can of course launch a remote shell like ssh:

    ```bash
    ,
    ```

1. To stop the magic:

    ```bash
    . ,
    ```
    **Note: Files are still stored on the remote machine.**

1. To get the files back, simply type `. , magic` again.

## Usage - `,,`

1. Inside a non-empty folder, create a config file in the folder:

    ```bash
    echo 'RMT_HOST=<user>@<host>' > magic/.rmt-config
    ```

1. Now you can operate on the folder from both side in one single shell:

    ```bash
    ,, make -j64    # Run heavy jobs using remote machine
    ```
    Note: Files are stored remotely but is visible locally thanks to `sshfs`.

1. You can of course launch a remote shell like ssh:

    ```bash
    ,,
    ```

1. To stop:

    ```bash
    ,, ,
    ```
    **Note: Files are still stored on the local machine.**

## Configuration: `.rmt-config`

- `RMT_HOST`: (Required) The remote machine.
- `RMT_LOCALHOST`: (Required for `,,`) The local machine.
- `RMT_SSH`: (Optional) To override the command line for ssh. Default is `ssh -Y -t`.
- `RMT_SSHFS`: (Optional) To override the command line for sshfs, Default is `sshfs`.
- `RMT_RENV`: (Optional) Will be added to the command line of `/usr/bin/env` during `ssh` call.
- `RMT_RSHELL`: (Optional) To override the login shell on remote machine. Default is to be automatically detected.
- `RMT_RDIR`: (Optional) To specify where to store the files on remote machine. A path relative to `$HOME`. Default is `.rmt/`.

## Limitation

Quotation and escape is a mess.
If your command contains `$`, escape carefully.
There's absolutely no gurantee that any kind of escape will or will not work.
If you want to run any command containing `&&`, `||`, or `|`, use the remote shell by typing `,` alone.

## License

MIT
