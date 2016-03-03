# Failed migration tutorial

## Scenario

During the process of moving your VMs to AWS you realise that one of your
servers presents some inconsistencies. As a way to ensure a successful migration
you used machinery to create a system description called "before" from the
working VM, one called "after" from the ec2 instance that presents the issues
and one called "working_aws" from one of the machines that was migrated
successfully.

## Install tutorial

By the default machinery will look for system descriptions inside the
`.machinery` directory in your home. We can change this functionality by setting
the environment variable `MACHINERY_DIR` to point to the folder containing our
example descriptions.

```
$ export MACHINERY_DIR=$(pwd)/machinery
```

Before we get started double check that your system descriptions are listed
correctly with the `list` command:

```
$ machinery list
 after:
   * packages
   * unmanaged-files (not extracted)

 before:
   * packages
   * unmanaged-files (not extracted)

 working_aws:
   * packages
   * unmanaged-files (not extracted)
```

You also have the option to pass an `--html` flag which will start a local
server that can be accessed at http://127.0.0.1:7585/

## Finding out what changed

In order to find the differences between the system "before" and "after" it was
migrated you use machinery's `compare` command:

```
$ machinery compare before after
# Packages

Only in 'after':
  * kernel-ec2

  # Unmanaged files

  Only in 'after':
    * /boot/do_purge_kernels (file)
```

Let's also compare our "befor" system agains the "working_aws" to see what
a successful migration looks like.

```
$ machinery compare before working_aws
# Packages

Only in 'before':
  * kernel-xen

Only in 'working_aws':
  * kernel-ec2
```

From these results we can interpret the following:

1. In the unsuccessful migration the package "kernel-xen" was not removed
2. This seems to be related to the still existing file `/boot/do_purge_kernels`

Like with the `list` command we can also pass the `--html` flag to the
`compare` command in order to see the comparison in an HTML view.

## Uninstall tutorial

If you don't want to use the tutorial any more you can simply unset the
environment variable or close your current shell.

```
$ unset MACHINERY_DIR
```
