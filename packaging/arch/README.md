# Arch Linux 源码包

源码版 PKGBUILD，从 GitHub Release 的 tarball 构建。

## 本地构建并安装

```bash
cd packaging/arch
# 生成正确的 sha256sums 后再正式发布
updpkgsums
# 构建并安装
makepkg -si
```

只构建不安装：`makepkg`，产物为 `dbx-<ver>-<rel>-x86_64.pkg.tar.zst`。

## 升级版本

1. 改 `pkgver`（必要时把 `pkgrel` 重置为 `1`）。
2. `updpkgsums` 重新生成 `sha256sums`。
3. `makepkg --printsrcinfo > .SRCINFO`（提交到 AUR 时需要）。

## 备注

- 编译耗时较长（duckdb / arrow / aws-lc-sys 都是大件），首次构建预计 10–20 分钟。
- 不需要 ODBC 的话，可以在 `crates/dbx-core/Cargo.toml` 把 `odbc-api` 改成可选 feature，并从 `depends` 移除 `unixodbc`。
- 当前 `tauri.conf.json` 启用了内置 updater；走 pacman 安装的用户应该用 `pacman -Syu` 升级，建议未来给 AUR 构建关闭 `createUpdaterArtifacts`。
