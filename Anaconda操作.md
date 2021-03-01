## 镜像源
- 查看镜像源：`conda config --show`
- 删除镜像源：`conda config --remove channels https://xxx`
- 添加镜像源：`conda config --add channels https://xxx`
## 版本更新
- 软件升级：
```
conda update conda
conda update anaconda
conda update anaconda-navigator
```
- 升级所有库：`conda update --all`
- 升级指定库：`conda update xxx`
## 虚拟环境
- 新建环境：`conda create -n pytorch python=3.6`

-n后为环境名称，python=3.6表示环境使用python3.6版本
- 查看环境：`conda info --envs`
- 激活环境：`conda activate xxx`