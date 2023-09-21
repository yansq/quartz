By default, all files created inside a container are stored on a writable container layer, which means the data doesn't persist when that container no longer exists, and it can be difficult to get the data out of the container if another process needs it.

Docker provides two options to store files from containers to the host machine: [[volumes]] and [[bind mounts]].

Docker also supports containers storing files in-memory on the host machine. They are tmpfs mount on Linux and pipe on Windows.
