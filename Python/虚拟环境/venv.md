# venv

python的venv模块是python标准库自带的轻量级虚拟环境工具

## 环境管理

### 创建环境

venv根据文件夹名区分虚拟环境

```bash
mkdir <envname>
python3 -m venv <envname>
```

其中 `-m` 是python命令的参数，指将其（venv）作为模块运行（从__main__.py或模块入口开始执行，而不是像脚本一样从一个文件开头执行下去）

### 激活环境

#### Linux/Mac

```bash
source <envname>/bin/activate
```

#### Windows

```cmd
<envname>\Scripts\activate
```

### 退出环境

```bash
deactivate
```

### 安装包

仍然使用pip安装python包，但最好调用虚拟环境里的python

激活环境前：

```bash
<envname>/bin/python -m pip install <package-name>
```

激活环境后：

```bash
python -m pip install <package-name>
```
