# HookEuicc
解除某些软件对eSIM的要求，也可用于获取eSIM代码

在某些不直接提供eSIM激活码的应用，捕获到激活码后会优先交给 NekokoLPA2 直接安装；
未安装 NekokoLPA2 时，仍会复制到剪切板并弹出分享面板。

## 配合 NekokoLPA2 安装

- 支持 `ee.nekoko.nlpa` 与 `ee.nekoko.nlpa.flavor1` 两个包名
- 通过 `lpa:<激活码>` 深链拉起 NekokoLPA2 的安装流程
- 激活码必须保持原样，不能做 URL 编码：编码后 `$` 会变成 `%24`，NekokoLPA2 将无法识别，并把整条 URI 当成激活码显示
- NekokoLPA2 会弹出一次「Install Now / Install Later」确认；确认后在当前选中的 eUICC 或读卡器上完成下载
- 同一个激活码只触发一次，避免重复弹窗
- 需要把目标应用加入模块作用域；从后台系统进程（如 `com.android.se`）启动会被 Android 10+ 的后台 Activity 限制拦截
- 「自动安装」走 Google Play Services 时（如 Holafly），需把 `com.google.android.gms` 也加入作用域；模块会从 `https://esimsetup.android.com/esim_qrcode_provisioning?carddata=...` 的 Intent 中解析激活码
- 「代码安装 / 扫码安装」只需勾选对应 App

## 其他功能

### OMAPI Bypass
绕过 ARA、ARF 限制。通常用于无ARA的卡进行OMAPI访问

> [!NOTE]
> 需勾选 `com.android.se` 并重启手机 (或执行 `su -c killall com.android.se`)

## 构建

本地构建：

```bash
./gradlew assembleRelease
```

未设置签名环境变量时输出未签名 APK；设置以下变量后自动签名：

- `SIGNING_KEYSTORE_FILE`
- `SIGNING_STORE_PASSWORD`
- `SIGNING_KEY_ALIAS`
- `SIGNING_KEY_PASSWORD`

GitHub Actions（`.github/workflows/build-release.yml`）：

- 推送 `v*` 标签：构建、校验签名并发布 GitHub Release
- 手动 `workflow_dispatch`：只构建并上传 artifact，不发布 Release
- 签名材料存放在仓库 Secrets：`SIGNING_KEYSTORE_BASE64`、`SIGNING_STORE_PASSWORD`、`SIGNING_KEY_ALIAS`、`SIGNING_KEY_PASSWORD`

发布新版本前先把 `app/build.gradle` 里的 `versionCode` / `versionName` 改好，再打 tag。

## 捐赠
如果您喜欢这个项目，可以向以下地址捐赠：

USDT (ERC20): `0x4d62f0c2a5bd9358bcd58352b0a3efd60afcf180`

USDT (TRC20): `TDJvmrzpS496wjyx7rTR4bediAvWvx1XLf`
