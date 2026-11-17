# 云影TV · 蜂蜜影视二开版

基于开源项目 [FongMi/TV](https://github.com/FongMi/TV)（俗称「蜂蜜影视 / 影视TV」）二次开发的 Android 影音客户端，支持 Android TV、手机、平板。

上游提供的是「播放器外壳 + 可配置数据接口」的完整工程：应用本身不含片源，片源/直播数据全部来自你在设置里填写的**自有或已获授权**的接口配置。

## 本二开版改了什么

| 项目 | 上游 | 本仓库（二开） |
| --- | --- | --- |
| 应用名 | 影视 / TV | 云影TV（简体、繁体、英文资源同步改） |
| applicationId | `com.fongmi.android.tv` | `com.yunying.android.tv`（可与官方版共存安装） |
| 工程名 | TV | YunYingTV |
| 自更新源 | `FongMi/Release` | `ojkky/TV`（发布在自己的 Release） |
| 构建签名 | 必须配置 `local.properties` 才能跑 | 缺省时回退 debug 签名，clone 即可构建 |
| 文档 | 仅上游使用指南 | 新增《二开指南.md》 |

**Java 类包名（`com.fongmi.android.tv`）保持不动**：只改 applicationId 即可独立安装，避免移动上千个文件的包路径带来的编译风险。若确实要彻底改名，见《二开指南.md》第 2 节的做法 B。

## 构建

环境要求：JDK 21、Android SDK（compileSdk 见 `gradle/libs.versions.toml`）、Gradle 由 wrapper 提供。

```bash
./gradlew :app:assembleLeanbackDebug   # TV 版
./gradlew :app:assembleMobileDebug     # 手机版
./gradlew :app:assembleLeanbackRelease :app:assembleMobileRelease   # 正式包
```

产物：`app/build/outputs/apk/leanback/debug/`、`app/build/outputs/apk/mobile/debug/`。

### 签名配置

根目录新建 `local.properties`（模板见 `local.properties.sample`）：

```properties
storeFile=/absolute/path/to/your.jks
keyAlias=your_alias
storePassword=your_store_password
keyPassword=your_key_password
```

未配置签名时 release 会回退 debug 签名，仅供自测，不要用于分发。

## 接口配置

安装后进入「设置 → 点播 / 直播」，填入自己的接口地址（本地文件、局域网、直链均可），如 `file:///sdcard/my_vod.json` 或你自己的 HTTPS 配置地址。接口格式见上游文档与 [CatVodSpider](https://github.com/FongMi/CatVodSpider)。

> 合规提示：本工程不含任何第三方影视资源，请只接入你拥有版权或已获授权的内容。分发前请自行确认片源与直播源的授权，并遵守当地法规。

## 目录速览

- `app/` 主工程（`src/leanback` = 电视版，`src/mobile` = 手机版，`src/main` = 公共逻辑）
- `catvod/` 爬虫/解析核心（CatVod 兼容）
- `chaquo` `nodejs` `quickjs` `runtime` `thunder` `tvbus` `hook` 播放与运行时模块
- `buildSrc/` 打包与 ABI 自定义 Gradle 逻辑
- `website/` 网页遥控与文档站点

详细改造步骤、图标替换、内置默认接口、升级源与上架注意事项见 **[二开指南.md](二开指南.md)**。

## 许可证与致谢

上游采用 GPL-3.0（见 `LICENSE.md`），本二开仓库同样遵循 GPL-3.0：分发时必须保留许可证、版权声明与源码可获取性，并注明来源于 FongMi/TV。感谢上游作者与社区。
