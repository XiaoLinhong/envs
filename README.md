# envs

业务部署依赖环境

## 编译器
安装目录：```INTEL=/public/home/bedrock/envs/v1.0/intel/2019.4 ```
1. 解压 intel-2019.4.tar.gz
```
tar -zxvf intel-2019.4.tar.gz $INTEL
```
2. 修改路径
```
cd $INTEL/compilers_and_libraries_2019/linux/bin
vim compilervars.sh # 修改PROD_DIR=$INTEL/compilers_and_libraries_2019/linux
vim ../../../debugger_2019/bin/debuggervars.sh #  INST="$INTEL"
```
3. 检查证书
```
 $INTEL/compilers_and_libraries_2019.4.243/linux/Licenses
```

4. 配置环境变量
```
# enviroumment

ENVS=/public/home/bedrock/envs/v1.0
# intel
INTEL=$ENVS/intel/2019.4/compilers_and_libraries_2019/linux/bin
source $INTEL/compilervars.sh intel64

```