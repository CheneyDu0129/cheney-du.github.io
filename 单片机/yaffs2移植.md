## 硬件环境 

+ MCU： STM32H743
+ FLASH： W25N01GV

### Flash芯片简介

+ 128M Byte
+ 1024 Block 
  + 64 Page
    + 2048 Byte
    + 64 Byte

![image-20201103110736172](D:\gitRepo_my\cheney-du.github.io\嵌入式\image-20201103110736172.png)



![image-20201103181146589](D:\gitRepo_my\cheney-du.github.io\嵌入式\image-20201103181146589.png)

![image-20201103181201290](D:\gitRepo_my\cheney-du.github.io\嵌入式\image-20201103181201290.png)



![image-20201103181130507](D:\gitRepo_my\cheney-du.github.io\嵌入式\image-20201103181130507.png)





## 移植yaffs2

命令锁存使能 Command Latch Enable, CLE

地址锁存使能 Address Latch Enable，ALE

#### 需要知道的Flash特性

+ 如何标记坏块

chunkId = block_id*chunks_per_block+chunk_offset_in_block



写Chunk

+ 写入流程
+ 

+ 写入数据区
+ 写入Space area