# anaconda使用指南



###导入环境变量

```
source ~/.bashrc


anaconda-navigator
```

### 创建环境

```
conda create -n tensorflow_jw
```

### 创建环境时，可以指定版本

```
conda create -n py3 python=3.5
source activate py3
source deactivate
```

### 进入环境

```
source activate tensorflow_jw
```

### 查看安装的包

```
conda list
```

### 安装包

```
conda install pa	ckage_name
```

### 离开环境

```
source deactivate
```

### 共享环境

```
conda env export > enviroment.yaml
conda env update -f=/path/to/envrioment.yaml 
```

### 列出环境

```
conda env list
conda env remove -n tensorflow_jw    #删除环境
```





## 安装jupyter

`conda install jupyter notebook`

## 运行jupyter

`jupyter notenbook`





## git使用TIPS

1. 将某个文件恢复到指定的版本

   ```
   git checkout $(commit_id)  path/to/file
   ```

   ​