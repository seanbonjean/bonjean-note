# uv

uv是新一代python包管理与虚拟环境管理工具，得益于rust的特性，uv能够加速python的包安装、依赖解析和虚拟环境创建

注：下面所述 ***包、依赖、模块*** 都是python模块在不同语境下的表述

## 兼容性

uv创建的虚拟环境与venv完全兼容：目录结构一致、激活方式一样

可以在 `pip` 前添加 `uv` 来使用uv，例如： `uv pip install -r requirements.txt`

## 核心文件

* `pyproject.toml`：记录依赖需求，类似于`requirements.txt`，其中注明的可能是版本范围
* `uv.lock`：当前`.venv`的精确版本快照，同时还记录了pypi源等信息

## 安装

`pip install uv` 或 `pipx install uv`

## CLI

注意：用uv ***进行项目管理*** 和仅仅只是用uv代替venv ***管理虚拟环境*** 所使用的命令不同，请严格区分

### 公用命令

* `uv run main.py`：使用当前项目的`.venv`环境运行`main.py`，无需激活环境

### 项目管理

* `uv init`：初始化项目（创建`pyproject.toml`文件管理项目依赖、创建一个`.python-version`记录了项目使用的python版本、初始化`.git`目录和`.gitignore`文件、创建一个`main.py`模板和`README.md`模板）
* `uv sync`：同步依赖（根据`pyproject.toml`文件中记录的依赖安装虚拟环境）
* `uv add 模块名`：安装模块；如无环境，将自动创建虚拟环境（相对的，仅管理虚拟环境时，`uv pip install`不会将依赖添加到`pyproject.toml`）
* `uv remove 模块名`：卸载模块

### 虚拟环境

* `uv venv 环境名`：创建虚拟环境；直接运行`uv venv`默认创建`.venv`环境到当前路径，适合为单个项目创建虚拟环境（相对的，用作项目管理时，`uv add`命令会自动创建虚拟环境，因此不需要运行`uv venv`）
* `uv pip install 模块名`：安装模块，其他命令同`pip`
