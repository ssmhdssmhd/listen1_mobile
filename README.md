# Listen1 Mobile V0.8.2

<p align="center"><img src="https://raw.githubusercontent.com/ssmhdssmhd/MXLOGO/main/web/web-logo.png" width="160"/></p>

[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg)](LICENSE)
[![Build APK](https://github.com/ssmhdssmhd/listen1_mobile/actions/workflows/build.yml/badge.svg)](https://github.com/ssmhdssmhd/listen1_mobile/actions/workflows/build.yml)

## 简介

一款支持多平台音乐播放和搜索的移动音乐 App。现有版本已支持网易云音乐，QQ 音乐，虾米音乐。还有丰富的歌单管理功能。使用 React Native 开发，基于 MIT 协议开源免费。

**支持 iOS 和 Android 平台**

[![imgur](https://i.imgur.com/zYyaK92.png)]()

## 特性

- 一个 App 播放多平台的音乐
- 搜索多平台音乐
- 浏览，播放多平台歌单
- 收藏音乐到自建歌单
- 夜间模式
- 备份，恢复(支持从`Listen1 chrome extension`导入数据)

## 下载

- Release 页面（推荐）：<https://github.com/ssmhdssmhd/listen1_mobile/releases>
- 每次推送 `master` 后，GitHub Actions 会自动编译签名 APK，并在 Actions 运行页面的 `Artifacts` 中提供下载。

> 本仓库 APK 使用固定签名（`com.listen1.app`），后续版本可直接覆盖升级；与官方 F-Droid 版本的签名不同，二者不能互相覆盖安装，安装前请先卸载旧版。

## 云端构建（GitHub Actions）

仓库已配置 [`.github/workflows/build.yml`](.github/workflows/build.yml)，推送代码到 `master` 或手动触发 `workflow_dispatch` 即可在云端完成编译、签名、产物上传与 Release 发布。

- 运行环境：Ubuntu 22.04 + JDK 8（Gradle）/ JDK 17（Android SDK）+ Node.js 12 + Android SDK 28
- 依赖仓库：`google()` / `mavenCentral()` / 阿里云镜像（替代已下线的 jcenter）
- 签名文件：由仓库 Secrets `KEYSTORE_BASE64` 解码注入，密码来自 `KEYSTORE_PASSWORD` / `KEY_PASSWORD`
- 产物命名：`Listen1.<版本号>.<构建时间 yyyyMMddHHmm>.apk`

首次使用或更换签名时，需要配置以下 Secrets：

| Secret | 说明 |
|--------|------|
| `KEYSTORE_BASE64` | 签名 keystore 的 Base64 内容 |
| `KEYSTORE_PASSWORD` | keystore 密码 |
| `KEY_PASSWORD` | key 密码（与 keyAlias 对应） |

## 安装

### iOS

iOS 只支持编译安装，请拥有开发者证书的开发者连接 iPhone 后，将项目文件中的证书换成自己的证书，然后执行命令安装。

### Android

下载 Release 中的 `Listen1.*.apk` 安装即可。

## 本地编译

开发环境

- Java 8 JDK （更高版本需更新默认 gradle 版本）
- Node.js 12（版本 >12.10.0 可能遇到 metro 正则表达式 bug 导致启动失败，云端构建已固定为 `12.22.12`）
- Android SDK 28（`platforms;android-28` + `build-tools;28.0.3`）

编译步骤

- Clone 或下载本项目代码
- `yarn` 安装依赖
- `yarn run link` 链接 React Native 的依赖库
- `yarn start:ios` 将在 iOS 模拟器上运行项目
- `yarn start:android` 将在安卓真机或模拟器（取决于手机是否连接）运行项目

APK 打包

```bash
cd android
./gradlew assembleRelease
```

本地签名需将 keystore 放到 `android/app/my-release-key.keystore`，并在 `android/gradle.properties` 或环境变量中提供 `MYAPP_UPLOAD_STORE_PASSWORD` 与 `MYAPP_UPLOAD_KEY_PASSWORD`。未提供 keystore 时将跳过 release 签名（产物不可直接分发）。

更详细的打包信息（包括生成 keystore）

https://reactnative.cn/docs/signed-apk-android

## 代码基本结构

- api: 音乐平台相关资源 API
- asset: 图片等资源
- components: 可复用的组件
- views: 业务相关的 screen 组件
- modules: 组件使用的自定义函数库
- redux: redux 需要的 action 和 reducer 函数

## 鸣谢

- [git-point](https://github.com/gitpoint/git-point): github 的 RN 客户端，提供本项目开发环境搭建的结构支持。
- [Binaryify/NeteaseCloudMusicApi](https://github.com/Binaryify/NeteaseCloudMusicApi): 网易 API，参考了网络协议部分代码。
- [yezihaohao/NeteaseCloudMusic](https://github.com/yezihaohao/NeteaseCloudMusic): 网易音乐 RN 端，参考了 RN 导航，播放部分的库代码实现。
- [soimort/you-get](https://github.com/soimort/you-get): 音乐下载命令行，参考了协议和法律相关声明（如下）。

开发过程还有很多开源软件提供了各种问题的解决方案，详见代码注释，篇幅原因不一一列出，感谢开源社区的各位开发者。

## 更新日志

`2026-11-21` · v0.8.2

- 重写 GitHub Actions 云端打包流程：JDK 8 + Node 12 + Android SDK 28，推送即自动编译签名 APK
- 替换已停止服务的 jcenter，改用 `mavenCentral` 与阿里云镜像，修复云端依赖拉取失败
- 新增固定签名配置：keystore 通过仓库 Secrets 注入，APK 自动签名并校验，支持后续版本覆盖升级
- APK 产物按 `Listen1.<版本号>.<构建时间>.apk` 命名，自动上传 Artifacts 并发布 Release
- 版本号升级至 0.8.2（versionCode 2），版本号统一读取 `package.json`
- 同步更新 README：下载方式、云端构建说明、签名说明与更新日志

`2020-10-31`

- 修复网易云歌单只有 10 首歌的 bug （感谢 @eatenid 的提交)
- 修复虾米云音乐歌单只有 30 首的 bug
- 优化过长歌曲名或标题的显示
- 优化下侧播放控制栏的弹窗性能
- 修正了点击暂停按钮时导致闪退的 bug
- 支持 GitHub action 在线打包

`2019-11-27`

- 修复 qq 音乐因 user-agent 无法访问的 bug

`2019-08-09`

- 修复网易云音乐无法访问的 bug

`2019-07-31`

- 首次发布

## 法律相关

This software is distributed under the MIT license

In particular, please be aware that

> THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
> IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
> FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
> AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
> LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
> OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
> SOFTWARE.

Translated to human words:

_In case your use of the software forms the basis of copyright infringement, or you use the software for any other illegal purposes, the authors cannot take any responsibility for you._

We only ship the code here, and how you are going to use it is left to your own discretion.
