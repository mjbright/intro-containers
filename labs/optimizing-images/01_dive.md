
## Investigating Images with Dive

```Dive``` is  an open source tool providing a TUI-* which allows to look at the contents of a Container Image.

TUI: Textual User Interface - an interactive interface running in a text console/terminal, e.g. top

### Installing Dive

The Dive Github Repository is at https://github.com/wagoodman/dive

Dive is a single Go static binary which can be downloaded from the releases page at https://github.com/wagoodman/dive

### Download the 0.12.0 Release tar archive

Perform the following steps to install and validate Dive:

```
wget https://github.com/wagoodman/dive/releases/download/v0.12.0/dive_0.12.0_linux_amd64.tar.gz
tar xf dive_0.12.0_linux_amd64.tar.gz dive
sudo mv dive /usr/local/bin/
```

Validate that the 0.12.0 version was correctly installed:
```
dive --version
```

Then remove the unneeded tar file:
```
rm dive_0.12.0_linux_amd64.tar.gz dive
```

### Running Dive

Run Dive on the latest nginx image as follows:
```
dive nginx:latest
```

Prior to launching the TUI, Dive informs us that it is fetching, then analyzing the image:

```
Image Source: docker://nginx:latest
Fetching image... (this can take a while for large images)
Analyzing image...
Building cache...
```

Once the TUI is launched you will see a page looking similar to the below text.

Note that we see, left and right-panes.

The Left-hand panes are:
- Top 'Layers' pane: The ● symbol indicates that this is the current pane, with one line selected
- Middle 'Layer Details' pane: The details here correspond to the selected line above
- Bottom 'Image Details' pane: Overall details about the nginx:latest image

The Right-hand pane is:
- 'Current Layer Contents' pane: Shows the file-system corresponding to the selected image layer

```
                                                          │ Current Layer Contents ├───────────────────────────────
┃ ● Layers ┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ ├── bin → usr/bin
Cmp   Size  Command                                       ├── boot
    **75 MB  FROM blobs**                                    ├── dev
    117 MB  RUN /bin/sh -c set -x     && groupadd --syste ├── etc
    1.6 kB  COPY docker-entrypoint.sh / # buildkit        │   ├── .pwd.lock
    2.1 kB  COPY 10-listen-on-ipv6-by-default.sh /docker- │   ├── adduser.conf
     389 B  COPY 15-local-resolvers.envsh /docker-entrypo │   ├── alternatives
    3.0 kB  COPY 20-envsubst-on-templates.sh /docker-entr │   │   ├── README
                                                          │   │   ├── awk → /usr/bin/mawk
│ Layer Details ├──────────────────────────────────────── │   │   ├── awk.1.gz → /usr/share/man/man1/mawk.1.gz
                                                          │   │   ├── builtins.7.gz → /usr/share/man/man7/bash-buil
Tags:   (unavailable)                                     │   │   ├── nawk → /usr/bin/mawk
Id:     blobs                                             │   │   ├── nawk.1.gz → /usr/share/man/man1/mawk.1.gz
Digest: sha256:c3548211b8264f8bfa47a6727043a64f1791b82ac9 │   │   ├── pager → /bin/more
65a284a7ea187e971a95e2                                    │   │   ├── pager.1.gz → /usr/share/man/man1/more.1.gz
Command:                                                  │   │   ├── rmt → /usr/sbin/rmt-tar
ADD rootfs.tar.xz / # buildkit                            │   │   ├── rmt.8.gz → /usr/share/man/man8/rmt-tar.8.gz
                                                          │   │   ├── which → /usr/bin/which.debianutils
│ Image Details ├──────────────────────────────────────── │   │   ├── which.1.gz → /usr/share/man/man1/which.debian
                                                          │   │   ├── which.de1.gz → /usr/share/man/de/man1/which.d
Image name: nginx:latest                                  │   │   ├── which.es1.gz → /usr/share/man/es/man1/which.d
Total Image size: 192 MB                                  │   │   ├── which.fr1.gz → /usr/share/man/fr/man1/which.d
Potential wasted space: 3.6 MB                            │   │   ├── which.it1.gz → /usr/share/man/it/man1/which.d
Image efficiency score: 99 %                              │   │   ├── which.ja1.gz → /usr/share/man/ja/man1/which.d
                                                          │   │   ├── which.pl1.gz → /usr/share/man/pl/man1/which.d
Count   Total Space  Path                                 │   │   └── which.sl1.gz → /usr/share/man/sl/man1/which.d
```

At the bottom of the page are some basic navigation & control instructions - but we will detail below how to move around within and between Panes.

```
▏^C Quit ▏Tab Switch view ▏^F Filter ▏^L Show layer changes ▏^A Show aggregated changes ▏
```

### Using Dive - navigating the left-hand pane

- Use TAB to toggle between left-hand and right-hand panes.
- Use left/right arrow keys to navigate between left-hand panes
- Use up/down arrow keys to select the line within a pane

```
┃ ● Layers ┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Cmp   Size  Command
     **75 MB  FROM blobs**
    117 MB  RUN /bin/sh -c set -x     && groupadd --system --gid
    1.6 kB  COPY docker-entrypoint.sh / # buildkit
    2.1 kB  COPY 10-listen-on-ipv6-by-default.sh /docker-entrypo
     389 B  COPY 15-local-resolvers.envsh /docker-entrypoint.d #
    3.0 kB  COPY 20-envsubst-on-templates.sh /docker-entrypoint.
    4.6 kB  COPY 30-tune-worker-processes.sh /docker-entrypoint.

│ Layer Details ├───────────────────────────────────────────────

Tags:   (unavailable)
Id:     blobs
Digest: sha256:c3548211b8264f8bfa47a6727043a64f1791b82ac965a284a
7ea187e971a95e2
Command:
ADD rootfs.tar.xz / # buildkit


│ Image Details ├───────────────────────────────────────────────

Image name: nginx:latest
Total Image size: 192 MB
Potential wasted space: 3.6 MB
Image efficiency score: 99 %

Count   Total Space  Path
    2        1.6 MB  /var/cache/debconf/templates.dat

```

```
▏^C Quit ▏Tab Switch view ▏^F Filter ▏^L Show layer changes ▏^A Show aggregated changes ▏
```

### Using Dive - navigating the right-hand pane

- Use TAB to toggle between left-hand and right-hand panes.
- Use up/down arrow keys to select the line within a pane

This pane shows the contents of the currently selected (on left-hand) Image Layer.
It is the union of all previous image layers.

Note: Image layers start from the ```FROM <image>``` line

Any files or folders modified by the currently selected Image Layer are colored as follows:
- yellow: modified (indicates files added in the case of a folder)
- bold: added

```
│ Current Layer Contents ├──────────────────────────────────────
Permission     UID:GID       Size  Filetree
-rwxrwxrwx         0:0        0 B  ├── bin → usr/bin
drwxr-xr-x         0:0        0 B  ├── boot
drwxr-xr-x         0:0        0 B  ├── dev
drwxr-xr-x         0:0     172 kB  ├── etc
-rw-------         0:0        0 B  │   ├── .pwd.lock
-rw-r--r--         0:0     3.0 kB  │   ├── adduser.conf
drwxr-xr-x         0:0      100 B  │   ├── alternatives
-rw-r--r--         0:0      100 B  │   │   ├── README
-rwxrwxrwx         0:0        0 B  │   │   ├── awk → /usr/bin/ma
-rwxrwxrwx         0:0        0 B  │   │   ├── awk.1.gz → /usr/s
-rwxrwxrwx         0:0        0 B  │   │   ├── builtins.7.gz → /
-rwxrwxrwx         0:0        0 B  │   │   ├── nawk → /usr/bin/m
-rwxrwxrwx         0:0        0 B  │   │   ├── nawk.1.gz → /usr/
-rwxrwxrwx         0:0        0 B  │   │   ├── pager → /bin/more
-rwxrwxrwx         0:0        0 B  │   │   ├── pager.1.gz → /usr
-rwxrwxrwx         0:0        0 B  │   │   ├── rmt → /usr/sbin/r
-rwxrwxrwx         0:0        0 B  │   │   ├── rmt.8.gz → /usr/s
-rwxrwxrwx         0:0        0 B  │   │   ├── which → /usr/bin/
-rwxrwxrwx         0:0        0 B  │   │   ├── which.1.gz → /usr
-rwxrwxrwx         0:0        0 B  │   │   ├── which.de1.gz → /u
-rwxrwxrwx         0:0        0 B  │   │   ├── which.es1.gz → /u
-rwxrwxrwx         0:0        0 B  │   │   ├── which.fr1.gz → /u
-rwxrwxrwx         0:0        0 B  │   │   ├── which.it1.gz → /u
-rwxrwxrwx         0:0        0 B  │   │   ├── which.ja1.gz → /u
-rwxrwxrwx         0:0        0 B  │   │   ├── which.pl1.gz → /u
-rwxrwxrwx         0:0        0 B  │   │   └── which.sl1.gz → /u
drwxr-xr-x         0:0      79 kB  │   ├── apt
drwxr-xr-x         0:0     3.3 kB  │   │   ├── apt.conf.d
-rw-r--r--         0:0      399 B  │   │   │   ├── 01autoremove
```

### Explore

1. Navigate within images - Experiment selecting different image layers, then exploring the layers details and layers contents

2. Observe the image details

Observe the details, such as wasted space/efficiency - see the description provided at the project home page at https://github.com/wagoodman/dive?tab=readme-ov-file#basic-features

### Extra: Investigate other images

Investigate some other images of interest
- consider images used, or built in earlier labs
- search for images of interest in the Docker Hub at https://hub.docker.com

