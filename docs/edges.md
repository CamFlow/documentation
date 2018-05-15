Description of the types of edge currently supported. In doubt, refer to the following [file](https://github.com/CamFlow/camflow-dev/blob/dev/include/uapi/linux/provenance_types.h).

**string in configuration file/CLI** | **defined in #include<linux/provenance_types.h>** | description | prov type |
|------------------------------------|---------------------------------------------| ------------|-----------|
**unknown**|**RL_UNKNOWN**| used as default value should normally not appear |undefined
**perm_read**|**RL_PERM_READ**| read permission check| used
**perm_write**|**RL_PERM_WRITE**| write permission check| used
**perm_exec**|**RL_PERM_EXEC**| exec permission check| used
**search**|**RL_SEARCH**| search permission check (i.e. exec on directory)| used
**write**|**RL_WRITE**| write information to an object | generatedBy
**read**|**RL_READ**| read information from an object| used
**create**|**RL_CREATE**| create a new object| generatedBy
**change**|**RL_CHANGE**| used for setuid| informedBy
**mmap_write**|**RL_MMAP_WRITE**| mmap with write privilege| generatedBy
**mmap_read**|**RL_MMAP_READ**| mmap with read privilege| used
**mmap_exec**|**RL_MMAP_EXEC**| mmap with exec privilege| used
**mmap**|**RL_MMAP**| mmap with exec privilege (inode to memory)| derivedFrom
**sh_write**|**RL_SH_WRITE**| conservatively assume write to shared memory (see [section 3](https://scholar.harvard.edu/files/tfjmp/files/socc-2017.pdf))| generatedBy
**sh_read**|**RL_SH_READ**| conservatively assume write to shared memory (see [section 3](https://scholar.harvard.edu/files/tfjmp/files/socc-2017.pdf))| used
**bind**|**RL_BIND**| socket bind| generatedBy
**connect**|**RL_CONNECT**| socket connect| generatedBy
**listen**|**RL_LISTEN**| socket listen| generatedBy
**accept**|**RL_ACCEPT**| socket accept| used
**accept_socket**|**RL_ACCEPT_SOCKET**| socket accept | derived from
**open**|**RL_OPEN**| file open| used
**link**|**RL_LINK**| file link| generatedBy
**read_link**|**RL_READLINK**| read link| used
**setattr**|**RL_SETATTR**| file set attribute (process to inode)| generatedBy
**setattr_inode**|**RL_SETATTR_INODE**| file set attribute (inode to attribute)| derivedFrom
**getattr**|**RL_GETATTR**| file get attribute| used
**setxattr**|**RL_SETXATTR**| file set extended attribute (process to inode) | generatedBy
**setxattr_inode**|**RL_SETXATTR_INODE**| file set extended attribute (inode to attribute) | derivedFrom
**getxattr**|**RL_GETXATTR**| file get extended attribute (process to inode) | used
**getxattr_inode**|**RL_GETXATTR_INODE**| file get extended attribute (inode to attribute) | derivedFrom
**removexattr**|**RL_RMVXATTR**| file remove extended attribute (process to inode) | generatedBy
**removexattr_inode**|**RL_RMVXATTR_INODE**| file remove extended attribute (inode to attribute) | derivedFrom
**listxattr**|**RL_LSTXATTR**| list xattr | used
**version_entity**|**RL_VERSION**| entity state version relation | derivedFrom
**version_activity**|**RL_VERSION_PROCESS**| process state version relation| informedBy
**named**|**RL_NAMED**| file name | derivedFrom
**named_process**|**RL_NAMED_PROCESS**| process name | used
**exec**|**RL_EXEC**| process exec (inode to process)| used
**exec_process**|**RL_EXEC_PROCESS**| old "process" to "new" process| informedBy
**clone**|**RL_CLONE**| process clone| informedBy
**send**|**RL_SND**| socket send (process to inode)| generatedBy
**send_unix**|**RL_SND_UNIX**| unix socket send (process to inode)| generatedBy
**send_packet**|**RL_SND_PACKET**| socket send (inode to packet)| derivedFrom
**receive**|**RL_RCV**| socket receive (inode to process)| used
**receive_unix**|**RL_RCV_UNIX**| unix socket receive(inode to process)| used
**receive_packet**|**RL_RCV_PACKET**| socket receive (packet to inode)| derivedFrom
**terminate**|**RL_TERMINATE_PROCESS**| process terminated (structure freed in kernel) | informedBy
**closed**|**RL_CLOSED**| inode was freed| derivedFrom
**arg**|**RL_ARG**| argument to process | used
**env**|**RL_ENV**| environment variable to process| used
**log**|**RL_LOG**| attach log to process | used
