## 引言

文档描述了如何为模式中台搭建依赖环境。

模式中台建议部署在```/public/home/bedrock```目录下。

其依赖环境的目录如下， 请保持该目录结构。

```
[bedrock@node1 ~]$ tree -L 3 envs
envs
└── v1.0 # 依赖环境版本号
    ├── conda
    │   └── 4.8.2
    ├── hdf5
    │   └── 1.10.5
    ├── intel
    │   └── 2019.4
    ├── ioapi
    │   └── 3.2
    ├── jasper
    │   └── 1.900.1
    ├── libpng
    │   └── 1.6.37
    ├── ncl
    │   └── 6.5.0
    ├── netcdf
    │   ├── 4.7.0
    │   ├── c
    │   └── f
    └── zlib
        └── 1.2.11
```

## 安装依赖库


#### 编译器
安装目录：```INTEL=/public/home/bedrock/envs/v1.0/intel/2019.4 ```

* 解压 

```
tar -zxvf intel-2019.4.tar.gz $INTEL
```

* 修改路径

```
cd $INTEL/compilers_and_libraries_2019/linux/bin
vim compilervars.sh # 修改 PROD_DIR=$INTEL/compilers_and_libraries_2019/linux
vim ../../../debugger_2019/bin/debuggervars.sh #  INST=$INTEL
```

* 检查证书

```
 $INTEL/compilers_and_libraries_2019.4.243/linux/Licenses
```

#### zlib

```
tar -zxvf zlib-1.2.11.tar.gz
CC=icc ./configure --prefix=/public/home/bedrock/envs/v1.0/zlib
make intall
```

#### hdf5
```
tar -zxvf  hdf5-1.10.5.tar.gz
CC=icc FC=ifort CXX=icpc ./configure --with-zlib=/public/home/bedrock/envs/v1.0/zlib/1.2.11\
--prefix=/public/home/bedrock/envs/v1.0/hdf5/1.10.5
make -j install
```

#### netcdf

* C库

```
tar -zxvf netcdf-c-4.7.0.tar.gz
CC=icc FC=ifort CXX=icpc \
CPPFLAGS="-I/public/home/bedrock/envs/v1.0/hdf5/1.10.5/include -I/public/home/bedrock/envs/v1.0/zlib/1.2.11/include" \
LDFLAGS="-L/public/home/bedrock/envs/v1.0/hdf5/1.10.5/lib  -L/public/home/bedrock/envs/v1.0/zlib/1.2.11/lib" \
./configure --prefix=/public/home/bedrock/envs/v1.0/netcdf/c/4.7.0 --disable-dap

make -j install
```

* F库: 注意先配置C库的环境变量

```
tar -zxvf netcdf-c-4.7.0.tar.gz
export LD_LIBRARY_PATH=/public/home/bedrock/envs/v1.0/netcdf/c/4.7.0/lib:$LD_LIBRARY_PATH
CC=icc FC=ifort \
CPPFLAGS=-I/public/home/bedrock/envs/v1.0/netcdf/c/4.7.0/include \
LDFLAGS=-L/public/home/bedrock/envs/v1.0/netcdf/c/4.7.0/lib \
./configure --prefix=/public/home/bedrock/envs/v1.0/netcdf/f/4.4.5

make -j install
```

最后将C库和F库链接到一起：``` /public/home/bedrock/envs/v1.0/netcdf4.7.0  ```


#### jasper

```
tar -zxvf jasper-1.900.1.tar.gz
CC=icc CXX=icpc ./configure --prefix=/public/home/bedrock/envs/v1.0/jasper/1.900.1
make install
```

#### libpng

注意依赖zlib库

```
tar -zxvf libpng-1.6.37.tar.gz
LDFLAGS="-L/public/home/bedrock/envs/v1.0/zlib/1.2.11/lib" \
CC=icc CXX=icpc ./configure --prefix=/public/home/bedrock/envs/v1.0/libpng/1.6.37
make install
```

#### IOAPI
```
tar -zxvf ioapi-3.2-2020104.tar.gz
cp Makefile.template Makefile
修改makefile: 编译路径和编译器 和netcdf选项、路径；
make all
make install
```

修改makefile内容
```
141 BIN        = Linux2_x86_64ifort                                                                           
142 BASEDIR    = ${PWD}                                                                                          
143 INSTALL    = /public/home/bedrock/envs/v1.0/ioapi/3.2                                                
144 LIBINST    = /public/home/bedrock/envs/v1.0/ioapi/3.2/lib                             
145 BININST    = /public/home/bedrock/envs/v1.0/ioapi/3.2/bin
146 CPLMODE    = nocpl
166 NCFLIBS   = -L/public/home/bedrock/envs/v1.0/netcdf/4.7.0/lib \
                -L/public/home/bedrock/envs/v1.0/hdf5/1.10.5/lib \
                -L/public/home/bedrock/envs/v1.0/zlib/1.2.11/lib \
                -lnetcdff -lnetcdf -lhdf5_hl -lhdf5 -lz
```

将安装包中的部分文件，移动到安装目录（二次开发会用到）

```
cp -r ioapi/*.EXT  ioapi/fixed_src/ ioapi/Makeinclude.Linux2_x86_64ifort ~/envs/v1.0/ioapi/3.2/ioapi/
```

#### ncl
```
tar -zxvf ncl_ncarg-6.5.0-CentOS6.10_64bit_nodap_gnu447.tar.gz
export NCARG_ROOT=$ENVS/ncl/6.5.0
export PATH=$NCARG_ROOT/bin:$PATH
export LD_LIBRARY_PATH=$NCARG_ROOT/lib:$LD_LIBRARY_PATH
```

## 配置环境变量

``` bash
# clean
export LD_LIBRARY_PATH=/lib
export PATH=/bin:/usr/bin:/usr/local/bin

# enviroumment
ENVS=/public/home/bedrock/envs/v1.0

# conda
CONDA=$ENVS/conda/4.8.2
export PATH=$CONDA/bin:$PATH

# Intel & IMPI
INTEL=$ENVS/intel/2019.4
source $INTEL/compilers_and_libraries_2019.4.243/linux/bin/compilervars.sh intel64 # 修改路径

# zlib
ZLIB=$ENVS/zlib/1.2.11
export LD_LIBRARY_PATH=$ZLIB/lib:$LD_LIBRARY_PATH

# hdf5
HDF5=$ENVS/hdf5/1.10.5
export LD_LIBRARY_PATH=$HDF5/lib:$LD_LIBRARY_PATH

# netcdf
NETCDF=$ENVS/netcdf/4.7.0
export PATH=$NETCDF/bin:$PATH
export LD_LIBRARY_PATH=$NETCDF/lib:$LD_LIBRARY_PATH

# jasper
JASPER=$ENVS/jasper/1.900.1
export LD_LIBRARY_PATH=$JASPER/lib:$LD_LIBRARY_PATH

# libpng
PNG=$ENVS/libpng/1.6.37
export LD_LIBRARY_PATH=$PNG/lib:$LD_LIBRARY_PATH

# ioapi
IOAPI=$ENVS/ioapi/3.2
export PATH=$IOAPI/bin:$PATH

#ncl
export NCARG_ROOT=$ENVS/ncl/6.5.0
export PATH=$NCARG_ROOT/bin:$PATH
export LD_LIBRARY_PATH=$NCARG_ROOT/lib:$LD_LIBRARY_PATH

```

