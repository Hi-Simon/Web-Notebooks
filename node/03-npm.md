## npm常用的命令

-   npm 全称为 node package manager
-   只要安装的node就已经安装的npm

查看版本号

```shell
npm -v
```

升级npm

```shell
npm install --global npm
```

```shell
npm init //初始化项目目录,同时生成package.json配置文件

npm install jquery //只下载依赖包
npm install --save jquery //安装依赖包，并在package.json中添加依赖说明

npm install //直接根据文件中的package.json配置文件中的dependence，安装相应的依赖包
```

跳过向导快速初始化项目

```shell
npm init -y
```

删除包，如果有依赖项依然保存

```shell
npm uninstall jquery
```

删除的同时也会把依赖项信息也删除

```shell
npm uninstall --save jquery
```

查看命令帮助

```shell
npm 命令 --help
```

