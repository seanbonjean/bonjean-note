# conda

conda可以提供python虚拟环境，虚拟环境之间是隔离的，因此解决了python版本冲突、pip包冲突等问题

可以安装anaconda来获取conda环境。anaconda是一个打包了conda环境和其他各种python工具的软件  
对于存储空间受限的设备，更推荐安装miniconda或换用python自带的venv

修改 `.condarc` 文件配置conda镜像源

## CLI

### 环境管理

* `conda env list` 列出所有环境
* `conda list -n <env_name>`列出该环境内安装的包
* `conda create -n <env_name> python=3.7` 创建环境
* `conda create --name new_env --clone old_env`克隆一个已有环境
* `conda remove -n <env_name> --all` 删除环境
* `conda activate <env_name>` 激活环境，即进入这个虚拟python环境
* `conda deactivate` 退出当前环境

### 环境内包管理

* `conda list` 列出当前环境内安装的包（一定要在环境内执行，否则会列出所有环境的所有包）
* `conda install <package_name>` 安装包
* `conda install -c conda-forge <package_name>=1.2.3` 安装包，使用conda-forge源，指定版本号
* `conda update <package_name>` 更新包
* `conda update --all` 更新所有包
* `conda remove <package_name>` 卸载包
* `conda search <package_name>` 搜索包

### 环境导入/导出

* `conda env export > environment.yml` 导出当前环境配置到yml文件
* `conda env create -f environment.yml` 导入yml文件创建环境
* `conda env update -f environment.yml` 用yml文件更新当前环境配置
