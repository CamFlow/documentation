Description of the types of vertex currently supported. In doubt, refer to [file `include/uapi/linux/provenance_types.h` within the CamFlow development repository](https://github.com/CamFlow/camflow-dev/blob/dev/include/uapi/linux/provenance_types.h).

**string in configuration file/CLI** | **defined in #include<linux/provenance_types.h>** | description | prov type |
|------------------------------------|---------------------------------------------| ------------|-----------|
|**disc_activity**|**ACT_DISC**|disclosed activity|activity|
|**disc_entity**|**ENT_DISC**|disclosed entity|entity|
|**disc_agent**|**AGT_DISC**|disclosed agent|agent|
|**task**|**ACT_TASK**|OS task|activity|
|**inode_unknown**|**ENT_INODE_UNKNOWN**|default value for inode|entity|
|**link**|**ENT_INODE_LINK**|link inode|entity|
|**file**|**ENT_INODE_FILE**|file inode|entity|
|**directory**|**ENT_INODE_DIRECTORY**|directory inode|entity|
|**char**|**ENT_INODE_CHAR**|char device inode|entity|
|**block**|**ENT_INODE_BLOCK**|block device inode|entity|
|**fifo**|**ENT_INODE_FIFO**|fifo (pipe) inode|entity|
|**socket**|**ENT_INODE_SOCKET**|socket inode|entity|
|**mmaped_file**|**ENT_INODE_MMAP**|private mmaped inode|entity|
|**msg**|**ENT_MSG**|IPC message|entity|
|**shm**|**ENT_SHM**|shared memory|entity|
|**address**|**ENT_ADDR**|IP address|entity|
|**sb**|**ENT_SBLCK**|super block|entity|
|**file_name**|**ENT_FILE_NAME**|file name|entity|
|**packet**|**ENT_PACKET**|packet|entity|
|**packet_content**|**ENT_PCKCNT**|packet content|entity|
|**iattr**|**ENT_IATTR**|inode attribute|entity|
|**xattr**|**ENT_XATTR**|inode extended attribute|entity|
|**argv**|**ENT_ARG**|process argument|entity|
|**envp**|**ENT_ENV**|environment variable|entity|
