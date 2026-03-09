# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

LiNEA40は、ZMKファームウェアベースの左右分割型40%カスタムキーボードのファームウェア設定リポジトリ。右側にPMW3610トラックボール、左側にEC11ロータリーエンコーダーを搭載。MCUはSeeeduino XIAO BLE (nRF52840)。

## Build

### GitHub Actions（メイン）
`main`へのpush/PRで自動ビルド。`build.yaml`がビルドマトリクスを定義し、zmkfirmware公式のreusable workflow (`build-user-config.yml@v0.2.1`) を使用。成果物（.uf2）はActions Artifactsからダウンロード。

### ローカルビルド（Docker）
```bash
# container_name変数にDockerコンテナ名を指定
make build container_name=<container>   # 左右両方ビルド
make clean container_name=<container>   # ビルドディレクトリ削除
```
ローカルビルドはzmk本体が`../zmk/`に、追加モジュールが`../zmk-modules/`に配置されている前提のDocker環境を使用。

## Architecture

### キーマトリクス
- 11列×4行、左右分割（左6列 + 右5列、col-offset=6）
- ダイオード方向: col2row
- 左側はNFCピン(GPIO0 9,10)をGPIOとして使用（col4, col5）

### レイヤー構成（`config/LiNEA40.keymap`）
| # | 名前 | 用途 |
|---|------|------|
| 0 | default | QWERTY基本レイヤー |
| 1 | SIMBOL_LAYER | 記号入力（BSPで起動） |
| 2 | NOT_USE | 予備記号/数字（TABで起動） |
| 3 | CURSOR | 数字行+カーソル移動+スクリーンショット（Q/ESC/LANG2で起動） |
| 4 | FUNCTION | F1-F12+マウスクリック+音量（SPACEで起動） |
| 5 | SCROLL | トラックボールスクロールモード+テンキー（トラックボール自動） |
| 6 | WIRELESS | Bluetooth接続管理+bootloader（RBKTで起動） |

### 外部モジュール（`config/west.yml`）
- `zmk` v0.2.1 — ZMK本体
- `zmk-pmw3610-driver` (inorichi) — トラックボールドライバー
- `zmk-rgbled-widget` (caksoylar) v0.3-branch — バッテリー残量LED表示

### ファイル構成
- `config/LiNEA40.keymap` — キーマップ定義（レイヤー、コンボ、ビヘイビア、トラックボール設定）
- `config/boards/shields/LiNEA40/LiNEA40.dtsi` — 共通ハードウェア定義（物理レイアウト、マトリクス変換、エンコーダー）
- `config/boards/shields/LiNEA40/LiNEA40_left.overlay` — 左側固有（col-gpios、エンコーダー有効化）
- `config/boards/shields/LiNEA40/LiNEA40_right.overlay` — 右側固有（col-gpios、PMW3610トラックボールSPI設定）
- `config/boards/shields/LiNEA40/LiNEA40_right.conf` — 右側設定（トラックボールCPI、ポーリングレート等）
- `config/boards/shields/LiNEA40/LiNEA40_left.conf` — 左側設定（エンコーダー、バッテリー）

### 右側固有: トラックボール
- SPI接続 (PMW3610)、CPI=800、スナイプCPI=400
- automouse-layer=1（MOUSE）、scroll-layers=5（SCROLL）
- ZMK Studioサポート有効（右側がcentral）

### 右側がセントラル
`Kconfig.defconfig`で右側が`ZMK_SPLIT_ROLE_CENTRAL`に設定されている。ZMK Studio RPCも右側のみ有効。
