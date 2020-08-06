### 一、背景

对于维护过多个 package 的同学来说，都会遇到一个选择：这些 package 是放在一个仓库里维护还是放在多个仓库里单独维护，数量较少的时候，多个仓库维护不会有太大问题，但是当 package 数量逐渐增多时，一些问题逐渐暴露出来：

- package 之间相互依赖，开发人员需要在本地手动执行 npm link，维护版本号的更替；
- issue 难以统一追踪，管理，因为其分散在独立的 Git 项目中；
- 每一个 package 都包含独立的 node_modules，而且大部分都包含 babel,webpack 等开发时依赖，安装耗时冗余并且占用过多空间。

### 二、什么是 lerna

用于管理具有多个包的 JavaScript 项目的工具。
这个介绍可以说很清晰了，引入 lerna 后，上面提到的问题不仅迎刃而解，更为开发人员提供了一种管理多 packages javascript 项目的方式。

- 自动解决 packages 之间的依赖关系。
- 通过 git 检测文件改动，自动发布。
- 根据 git 提交记录，自动生成 CHANGELOG

### 三、常用命令

#### 1、安装 lerna

> 安装在项目更目录中

```bash
yarn add lerna
```

> 安装在全局中

```bash
yarn global add lerna
```

#### 2、使用 lerna 初始化项目

> 固定模式(Fixed mode)默认为固定模式，packages 下的所有包共用一个版本号(version)

```bash
lerna init
```

> 独立模式(Independent mode)，每一个包有一个独立的版本号

```bash
lerna init --independent
```

#### 3、清理环境

> 清理所有的 node_modules

```bash
lerna clean
```

> 执行所有 package 的 clean 操作

```bash
yarn workspaces run clean
```

### 四、Yarn + Lerna (workspace) (Independent) 模式

通过使用使用 **yarn** 的 **workspace** 与 Lerna 结合的模式下，执行 **​yarn install​** 会自动的帮忙解决安装和 link 问题.

```bash
yarn install # 等价于 lerna bootstrap --npm-client yarn --use-workspaces
```

#### 安装\删除依赖

> 给某个 package 安装依赖

```bash
yarn workspace packageB add packageA
# 将packageA作为packageB的依赖进行安装
yarn workspace packageB remove packageA
# 将packageA从packageB的依赖中删除
```

> 给所有的 package 安装依赖

```bash
yarn workspaces add lodash
# 将lodash作用给所有的package依赖进行安装
yarn workspaces remove lodash
# 将lodash从所有的package依赖中删除
```

> 给 root 根目录项目 安装依赖

```bash
​yarn add -W -D typescript
# 一般的公用的开发库都是安装在 root 里
yarn remove -W -D typescript
# 删除安装在 root 里的公用库
```
