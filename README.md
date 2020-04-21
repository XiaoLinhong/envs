# 安装

业务部署依赖环境

## 编译器
安装目录：```INTEL=/public/home/bedrock/envs/v1.0/intel/2019.4 ```

* 解压 

```
tar -zxvf intel-2019.4.tar.gz $INTEL
```

* 修改路径

```
cd $INTEL/compilers_and_libraries_2019/linux/bin
vim compilervars.sh # 修改PROD_DIR=$INTEL/compilers_and_libraries_2019/linux
vim ../../../debugger_2019/bin/debuggervars.sh #  INST="$INTEL"
```

* 检查证书

```
 $INTEL/compilers_and_libraries_2019.4.243/linux/Licenses
```

## zlib

```
tar -zxvf zlib-1.2.11.tar.gz
CC=icc ./configure --prefix=/public/home/bedrock/envs/v1.0/zlib
make intall
```

## hdf5
```
tar -zxvf  hdf5-1.10.5.tar.gz
CC=icc FC=ifort CXX=icpc ./configure --with-zlib=/public/home/bedrock/envs/v1.0/zlib/1.2.11\
--prefix=/public/home/bedrock/envs/v1.0/hdf5/1.10.5
make -j install
```

## netcdf

C库

```
tar -zxvf netcdf-c-4.7.0.tar.gz
CC=icc FC=ifort CXX=icpc \
CPPFLAGS="-I/public/home/bedrock/envs/v1.0/hdf5/1.10.5/include -I/public/home/bedrock/envs/v1.0/zlib/1.2.11/include" \
LDFLAGS="-L/public/home/bedrock/envs/v1.0/hdf5/1.10.5/lib  -L/public/home/bedrock/envs/v1.0/zlib/1.2.11/lib" \
./configure --prefix=/public/home/bedrock/envs/v1.0/netcdf/c/4.7.0 --disable-dap

make -j install
```

F库: 注意先配置C库的环境变量

```
tar -zxvf  hdf5-1.10.5.tar.gztar -zxvf netcdf-c-4.7.0.tar.gz

CC=icc FC=ifort \
CPPFLAGS=-I/public/home/bedrock/envs/v1.0/netcdf/c/4.7.0/include \
LDFLAGS=-L/public/home/bedrock/envs/v1.0/netcdf/c/4.7.0/lib \
./configure --prefix=/public/home/bedrock/envs/v1.0/netcdf/f/4.4.5

make -j install
```

## jasper
```
tar -zxvf jasper-1.900.1.tar.gz
CC=icc CXX=icpc ./configure --prefix=/public/home/bedrock/envs/v1.0/jasper/1.900.1
make install
```

## libpng
```
tar -zxvf libpng-1.6.37.tar.gz
CC=icc CXX=icpc ./configure --prefix=/public/home/bedrock/envs/v1.0/libpng/1.6.37
make install
```

## IOAPI
```
tar -zxvf ioapi-3.2-2020104.tar.gz
修改makefile: 编译路径和编译器 和netcdf选项、路径；
make install
```

## ncl
```
tar -zxvf ncl_ncarg-6.5.0-CentOS6.10_64bit_nodap_gnu447.tar.gz
export NCARG_ROOT=$ENVS/ncl/6.5.0
export PATH=$NCARG_ROOT/bin:$PATH
export LD_LIBRARY_PATH=$NCARG_ROOT/lib:$LD_LIBRARY_PATH
```

# 环境变量

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

# hdfr
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

