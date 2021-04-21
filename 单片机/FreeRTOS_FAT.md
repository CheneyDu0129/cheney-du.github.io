

# FreeRTOS+FAT组件

## Feature

| Fully thread aware                        |
| ----------------------------------------- |
| Scalable                                  |
| Optional long file name support           |
| Optional directory name hashing for speed |
| FAT12, FAT16 and FAT32 support            |
| Task specific working directories         |
| Task specific errno support               |
| Additional comprehensive error reporting  |
| Standard and comprehensive API            |
| Supported technology                      |

## Source Code Organization

```C
FreeRTOS-Plus-FAT    [Contains the source files that implement the FAT FS]
  |
  +-include          [Contains the header files for the FAT FS]
  |
  +-portable
      |
      +-common       [Contains source and header files used by all ports, inc. a RAM disk driver]
      |
      +-Platform_1   [Contains source file specific to the chip/compiler identified by the directory’s name]
      |
      +-Platform_2   [Contains source file specific to the chip/compiler identified by the directory’s name]

The FreeRTOS+FAT Directory Structure
```
## Standard API errno Values

| **Value** | **Constant**                   | **Description**                               |
| --------- | ------------------------------ | --------------------------------------------- |
| 0         | pdFREERTOS_ERRNO_NONE          | No such file or directory                     |
| 2         | pdFREERTOS_ERRNO_ENOENT        | No such file or directory                     |
| 5         | pdFREERTOS_ERRNO_EIO           | I/O error                                     |
| 6         | pdFREERTOS_ERRNO_ENXIO         | No such device or address                     |
| 9         | pdFREERTOS_ERRNO_EBADF         | Bad file number                               |
| 11        | pdFREERTOS_ERRNO_EAGAIN        | No more processes                             |
| 11        | pdFREERTOS_ERRNO_EWOULDBLOCK   | Operation would block                         |
| 12        | pdFREERTOS_ERRNO_ENOMEM        | Not enough core                               |
| 13        | pdFREERTOS_ERRNO_EACCES        | Permission denied                             |
| 14        | pdFREERTOS_ERRNO_EFAULT        | Bad address                                   |
| 16        | pdFREERTOS_ERRNO_EBUSY         | Mount device busy                             |
| 17        | pdFREERTOS_ERRNO_EEXIST        | File exists                                   |
| 18        | pdFREERTOS_ERRNO_EXDEV         | Cross-device link                             |
| 19        | pdFREERTOS_ERRNO_ENODEV        | No such device                                |
| 20        | pdFREERTOS_ERRNO_ENOTDIR       | Not a directory                               |
| 21        | pdFREERTOS_ERRNO_EISDIR        | Is a directory                                |
| 22        | pdFREERTOS_ERRNO_EINVAL        | Invalid argument                              |
| 28        | pdFREERTOS_ERRNO_ENOSPC        | No space left on device                       |
| 29        | pdFREERTOS_ERRNO_ESPIPE        | Illegal seek                                  |
| 30        | pdFREERTOS_ERRNO_EROFS         | Read only file system                         |
| 42        | pdFREERTOS_ERRNO_EUNATCH       | Protocol driver not attached                  |
| 50        | pdFREERTOS_ERRNO_EBADE         | Invalid exchange                              |
| 79        | pdFREERTOS_ERRNO_EFTYPE        | Inappropriate file type or format             |
| 89        | pdFREERTOS_ERRNO_ENMFILE       | No more files                                 |
| 90        | pdFREERTOS_ERRNO_ENOTEMPTY     | Directory not empty                           |
| 91        | pdFREERTOS_ERRNO_ENAMETOOLONG  | File or path name too long                    |
| 95        | pdFREERTOS_ERRNO_EOPNOTSUPP    | Operation not supported on transport endpoint |
| 105       | pdFREERTOS_ERRNO_ENOBUFS       | No buffer space available                     |
| 109       | pdFREERTOS_ERRNO_ENOPROTOOPT   | Protocol not available                        |
| 112       | pdFREERTOS_ERRNO_EADDRINUSE    | Address already in use                        |
| 116       | pdFREERTOS_ERRNO_ETIMEDOUT     | Connection timed out                          |
| 119       | pdFREERTOS_ERRNO_EINPROGRESS   | Connection already in progress                |
| 120       | pdFREERTOS_ERRNO_EALREADY      | Socket already connected                      |
| 125       | pdFREERTOS_ERRNO_EADDRNOTAVAIL | Address not available                         |
| 127       | pdFREERTOS_ERRNO_EISCONN       | Socket is already connected                   |
| 128       | pdFREERTOS_ERRNO_ENOTCONN      | Socket is not connected                       |
| 135       | pdFREERTOS_ERRNO_ENOMEDIUM     | No medium inserted                            |
| 138       | pdFREERTOS_ERRNO_EILSEQ        | An invalid UTF-16 sequence was encountered    |
| 140       | pdFREERTOS_ERRNO_ECANCELED     | Operation cancelled                           |

## 常用API

### FAT库自带的API

#### 分区函数

`FF_Error_t FF_Partition( FF_Disk_t *pxDisk, FF_PartitionParameters *pxFormatParameters );`

| **Parameters:**       | Description：                                                |
| --------------------- | ------------------------------------------------------------ |
| pxDisk                | The FF_Disk_t structure that describes the media being partitioned. |
| *FF_FormatParameters* | A pointer to a structure that describes how the media will be partitioned. |

**Returns:** If the media is successfully partitioned then FF_ERR_NONE is returned. If the media could not be partitioned then an error code is returned. FF_GetErrMessage() converts error codes into error descriptions.

```c
typedef enum _FF_SizeType
{
/* xSizes within the FF_PartitionParameters structure are specified as a
quotum (the sum of all xSizes is free, all disk space will be allocated). */
eSizeIsQuota,

/* xSizes within the FF_PartitionParameters structure are specified as a
percentage of the total disk space (the sum of all xSizes must be <= 100%) */
eSizeIsPercent,

/* xSizes within the FF_PartitionParameters structure are specified as a
number of sectors (the sum of all xSizes must be < ulSectorCount). */
FF_Size_Sectors,
} eSizeType_t;

typedef struct _FF_PartitionParameters
{
/* The total number of sectors on the media, including hidden/reserved
sectors. */
uint32_t ulSectorCount;

/* The number of sectors to keep free. */
uint32_t ulHiddenSectors;

/* The number of sectors to keep between partitions. */
uint32_t ulInterSpace;

/* The size of each partition – how the sizes are specified depends on the
value of eSizeType. */
BaseType_t xSizes[ FF_MAX_PARTITIONS ];

/* The number of primary partitions to create. 要创建的主分区数量*/
BaseType_t xPrimaryCount;

/* How the values within the xSizes array are specified. */
eSizeType_t eSizeType;
} FF_PartitionParameters;

The FF_PartitionParameters and associated types
```

#### 挂载设备函数

`FF_Error_t FF_Mount( FF_Disk_t *pxDisk, BaseType_t xPartitionNumber );`

| **Parameters:**    | Description：                                                |
| ------------------ | ------------------------------------------------------------ |
| pxDisk             | The FF_Disk_t structure that describes the media being partitioned. |
| *xPartitionNumber* | The number of the partition on the media to mount. Partition numbers start from 0. |

**Returns:**

If the partition was successfully mounted then FF_ERR_NONE is returned. If the partition could not be mounted then an error code is returned. FF_GetErrMessage() converts error codes into error descriptions.

## 自定义接口Porting

#### 创建一个存储介质驱动



## 移植要点

+ 实现`#define portINLINE __inline`
+ 实现FF_PRINTF

## Configuration Option

必须提供==FreeRTOSFATConfig.h==文件

有如下配置可供选择

 #### ffconfigBYTE_ORDER

该定义必须设置为pdFREERTOS_LITTLE_ENDIAN 或pdFREERTOS_BIG_ENDIAN，取决你的FreeRTOS系统配置。

  #### ffconfigHAS_CWD

设为1表示可以维护当前访问文件系统的每个任务的当前工作目录，允许使用**相对路径**

设为0表示只能用**绝对路径**

  #### ffconfigCWD_THREAD_LOCAL_INDEX

Set to an index within FreeRTOS’s thread local storage array that is free for use by FreeRTOS+FAT. FreeRTOS+FAT will use two consecutive indexes from this that set by ffconfigCWD_THREAD_LOCAL_INDEX. The number of thread local storage pointers provided by FreeRTOS is set by configNUM_THREAD_LOCAL_STORAGE_POINTERS in FreeRTOSConfig.h.

  #### ffconfigLFN_SUPPORT

是否支持长文件名，1表示支持

If long file name support is excluded then only 8.3 file names can be used. Long file names will be recognised, but ignored.

Users should familiarise themselves with any patent issues that may potentially exist around the use of long file names in FAT file systems before enabling long file name support.

  #### ffconfigINCLUDE_SHORT_NAME

当ffconfigLFN_SUPPORT为1时有效

是否包含在列出目录时显示文件的简称，1表示支持

Set to 0 to only include a file’s long name.

  #### ffconfigSHORTNAME_CASE

是否支持大小写的识别，1表示支持(建议支持以获得最大兼容性)

  #### ffconfigUNICODE_UTF16_SUPPORT

在ffconfigLFN_SUPPORT 设为1时有效

设置1表示文件名和目录名用UTF-16 (wide-characters)格式

设置0表示文件名和目录名用8-bit ASCII || UTF-8格式（具体格式取决于ffconfigUNICODE_UTF8_SUPPORT设置）

  #### ffconfigUNICODE_UTF8_SUPPORT

在ffconfigLFN_SUPPORT 设为1时有效

设置为1表示用UTF-8格式

设置为0表示用8-bit ASCII || UTF-16格式(具体格式取决于ffconfig_UTF_16_SUPPORT 设置)

  #### ffconfigFAT12_SUPPORT

设置为1表示支持FAT12

FAT16和FAT32总是支持

  #### ffconfigOPTIMISE_UNALIGNED_ACCESS

如果写入和读取数据时，大小超过512字节，效率下降

当改配置设置为1，设置为1时，每个文件句柄将分配一个512字节的字符缓冲区，以方便“未对齐访问”

  #### ffconfigCACHE_WRITE_THROUGH

磁盘的输入输出**缓存区**仅在以下时候刷新：

+ 当需要新的缓冲区并且没有其他缓冲区可用时。
+ 当以READ模式为刚刚更改的扇区打开缓冲区时。
+ 在创建、移动或关闭一个文件或者目录时。

通常，这足够快且有效。 如果将ffconfigCACHE_WRITE_THROUGH设置为1，则每次释放缓冲区时也会刷新缓冲区，效率较低，但更安全。


  #### ffconfigWRITE_BOTH_FATS

在大多数情况下，FAT表在磁盘上具有两个相同的副本，从而在发生读取错误的情况下可以使用第二个副本。

设置1表示使用所有的FATS，这样效率会低但是更安全。

设置0表示只使用一个FAT，第二个FAT将不会被使用。

  #### ffconfigWRITE_FREE_COUNT

关系到**启动速度**，设置为1启动速度快，但是效率低。

设置为1以具有空闲集群的数量，并且每当其中一个值更改时，第一个空闲集群将被写入FS信息扇区。

设置为0不会将这些值存储在FS信息扇区中，从而使启动速度较慢，但更改速度更快。

  #### ffconfigTIME_SUPPORT

设置1表示可以维护文件和目录的创建、修改和最后访问的时间戳。

  If time support is used, the following function must be supplied:

  ```
  time_t FreeRTOS_time( time_t *pxTime );
  ```

  FreeRTOS_time has the same semantics as the standard time() function.

  #### ffconfigREMOVABLE_MEDIA

是否是可移动介质

设置为1时，如果存储介质被取走，则所有文件句柄都将“无效”。 如果设置为0，则文件句柄将不会无效。 在这种情况下，用户将必须在每次访问之前确认媒体仍然存在。

  #### ffconfigMOUNT_FIND_FREE

设置为1可以确定磁盘的可用空间以及挂载磁盘时磁盘的第一个可用集群。    

设置为0可以在首次需要这两个值时找到它们。 确定这些值可能需要一些时间。

  #### ffconfigFSINFO_TRUSTED

决定是否信任‘ulLastFreeCluster’和ulFreeClusterCount字段的内容。

  #### ffconfigPATH_CACHE

是否使用路径缓存(占用空间换取效率上的提升)

  #### ffconfigPATH_CACHE_DEPTH

定义最大数量的路径缓存

  #### ffconfigHASH_CACHE

是否使用HASH值

当遇到大型目录或文件名称相似的情况时，可以提高效率

  #### ffconfigHASH_FUNCTION

当ffconfigHASH_CACHE为1时有效

设置为CRC8或CRC16分别使用8位或16位HASH值。

  #### ffconfigMKDIR_RECURSIVE

设置为1可向ff_mkdir（）添加一个参数，该参数允许一次创建整个目录树，而不必一次在树中创建一个目录。

 例如`mkdir( "/etc/settings/network"，pdTRUE);`

设置为0以使用常规的mkdir（）语义（不带附加参数）。

  #### ffconfigBLKDEV_USES_SEM

设定调用fnReadBlocks 和fnWriteBlocks 两个函数时是否加锁  

  #### ffconfigMALLOC

定义内存分配函数`#define ffconfigMALLOC(size) pvPortMalloc(size)`

  #### ffconfigFREE

定义内存释放函数 `#define ffconfigFREE( ptr ) vPortFree( ptr )`

  #### ffconfig64_NUM_SUPPORT

设置为1用**64位数据类型**计算空闲大小和文件大小，否则用**32位数据类型**

  #### ffconfigMAX_PARTITIONS

定义可以识别的最大分区数（以及逻辑分区）

  #### ffconfigMAX_FILE_SYS

定义总共可以合并多少个驱动器。 **至少应设置为2**。

  #### ffconfigDRIVER_BUSY_SLEEP_MS

如果底层驱动程序返回错误“ FF_ERR_DRIVER_BUSY”，则库将暂停时间（ms），在ffconfigDRIVER_BUSY_SLEEP_MS中定义，然后重试。

  #### ffconfigFPRINTF_SUPPORT

选择构建是否包含`ff_fprintf()`函数

不包含`ff_fprintf`函数可以大大减少代码大小

  #### ffconfigFPRINTF_BUFFER_LENGTH

设置`ff_fprintf()`分配的缓存区大小

  #### ffconfigINLINE_MEMORY_ACCESS

一些内部存储器访问功能是否使用`inline`语法

  #### ffconfigFAT_CHECK

正式的决定FAT种类的唯一条件是簇的总量：

+ if( ulNumberOfClusters < 4085 ) : Volume is FAT12
+ if( ulNumberOfClusters < 65525 ) : Volume is FAT16
+  if( ulNumberOfClusters >= 65525 ) : Volume is FAT32

并非每个格式化的设备都遵循以上规则

设置为1可以执行另外的检查并且检查磁盘上的簇以决定FAT种类。

设置为0只检查磁盘上的簇以决定FAT种类。

  #### ffconfigMAX_FILENAME

设置文件名的最大长度，包括路径。 请注意，此定义的值与FAT库的最大堆栈使用量直接相关。 在某些API中，将在堆栈中声明大小为'ffconfigMAX_FILENAME'的字符缓冲区。


