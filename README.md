# iOS JIT Enabler — 云端构建 JIT 工具

本项目用 GitHub Actions 的 macOS runner 在**云端**编译、并用你的**免费 Apple ID 签名**，产出可安装的 IPA，自动挂到 Releases 公开下载。

> ⚠️ 先读这里，别装完才发现不适用。JIT 能不能开，取决于你的**设备系统版本**和**你手上有什么**，不是取决于这个仓库。

## 1. 先确认你的系统版本

### iOS 26.x（26.0 / 26.5 等，新系统）
- 苹果已**关闭“通用 JIT 通道”**：旧做法（一个带 entitlement 的应用给任意应用开 JIT，如 Jitterbug 老方案）在 iOS 26 上**不再有效**。
- 现在每个应用必须由开发者自己适配苹果新 JIT 方法，所以 JIT **只对明确支持的应用有效**：UTM、Amethyst、MeloNX、maciOS、DolphiniOS（≤26.6）、Geode。
- 当前工具链为 **StikDebug + JitterbugPair / SideStore**，通常需要**一次性电脑配对 + 常挂 VPN（如 StosVPN）+ Wi-Fi**。
- **如果你没有电脑**：iOS 26.x 上目前基本**无法开启 JIT**（自建工具无通用通道，StikDebug 又需电脑配对）。

### iOS 14 – 18（旧系统，如 iPad mini 5）
- 自建 **Jitterbug 类** JIT 工具可行。
- 要求：Jitterbug 与目标模拟器**用同一 Apple ID 团队签名**（免费个人团队即可，限 3 个应用）。

## 2. 你需要做的（免费方案）
在仓库 **Settings → Secrets and variables → Actions** 添加三个 Secrets：

| Secret | 值 | 说明 |
|---|---|---|
| `APPLE_ID` | 你的免费 Apple ID | 用于云端签名 |
| `APPLE_ID_PASSWORD` | App 专用密码 | 到 appleid.apple.com 生成，非登录密码 |
| `APPLE_TEAM_ID` | 团队 ID | 登录 developer.apple.com 可查 |

免费 Apple ID 签名的 IPA：**7 天失效**，需重签/续期；最多 3 个应用。

## 3. 触发构建
1. 添加上述 3 个 Secrets；
2. 推送到 `main` 或手动触发 Actions（workflow_dispatch）；
3. 构建完成后到 **Releases** 下载自动生成并签名好的 IPA。

## 4. 安装到设备
- 下载 Releases 里的 IPA；
- 用签名/侧载工具安装（SideStore、Sideloadly 等，免费 Apple ID 7 天需重签）。

## 5. JIT 本体与来源
- 构建基于开源项目 **osy/Jitterbug**（MIT 协议），这是社区在未越狱设备上开 JIT 的主流工具。
- 本仓库不重复造轮子，职责是：**云端构建 + 免费 Apple ID 签名 + 公开 Releases 分发**。

## 诚实说明 / 已知限制
- 本项目作者（你）**无电脑、无付费开发者账号、设备未越狱**：在此前提下，**iOS 26.x 无法开启 JIT**，这是 Apple 的硬性限制，无法绕过。
- iOS 14–18 旧设备可用本仓库产出的工具，但需同一团队签名目标模拟器。
- CI 首次运行可能需要根据 Jitterbug 上游的工程名/签名配置做一次小调整（本仓库作者环境无法本地跑 Xcode，只能云端验证）。
