# Docker

## Scenario

You want to validate that your Dockerfile builds correctly so you use machinery
to inspect the resulting image and save it in your system under
"docker_machinery".

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
 docker_machinery:
   * os
   * packages
   * repositories
```

## Analysing the results

Our Dockerfile consist of the following lines:

```
FROM opensuse:leap

# Add the machinery repository
RUN zypper ar -G -f http://download.opensuse.org/repositories/systemsmanagement:/machinery/openSUSE_Leap_42.1/ machinery
# Install machinery
RUN zypper -n --gpg-auto-import-keys install machinery
```

Now you are going to see what kind of information is in the description by using
the `show` command. You want to make sure this description is based on a docker
image and not on a regular Linux system so you pass the `--verbose` flag.

```
$ machinery show docker_machinery --verbose
# Inspection details

  Type of inspected container: docker

# Operating system [docker/machinery] (2016-03-03 13:59:22)

  Name: openSUSE Leap
  Version: 42.1
  Architecture: x86_64


# Packages [docker/machinery] (2016-03-03 13:59:22)

  * machinery-1.17.0-8.1.x86_64 (obs://build.opensuse.org/systemsmanagement)

# Repositories [docker/machinery] (2016-03-03 13:59:22)

  * machinery
    URI: http://download.opensuse.org/repositories/systemsmanagement:/machinery/openSUSE_Leap_42.1/
    Alias: machinery
    Enabled: Yes
    Refresh: Yes
    Priority: 99
    Type: rpm-md
```

As you can see machinery is aware that of the type of container it is
inspecting. It also knows that the image is based on openSUSE Leap version 42.1
and most important you can see that the repository and package that you are
looking for are installed.

## Uninstall tutorial

If you don't want to use the tutorial any more you can simply unset the
environment variable or close your current shell.

```
$ unset MACHINERY_DIR
```
