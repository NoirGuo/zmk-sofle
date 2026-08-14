# Sofle

- [中文](README.md)
- [English](README_EN.md)

## DYA Studio / Zephyr 4.1

本分支是无接收器、无 OLED 的纯键盘版本。左手为 central，右手为 peripheral；固件基于 `cormoran/zmk#main+dya` 和 `cormoran/zephyr#v4.1.0+zmk-fixes+nrf-half-duplex-uart`。

已启用：

- DYA Studio USB RPC（USB 连接左手）
- Runtime Macro
- Runtime Combo
- BLE 管理、左右电量历史和 Settings RPC
- 原有静态 Macro 与静态 Combo
- Sofle 旋钮、RGB 和按键背光

未启用接收器、OLED、轨迹球或鼠标输入模块。

### 连接 DYA Studio

1. 刷入同一次 Actions 构建生成的左、右手固件。
2. USB 连接左手并打开 [DYA Studio](https://studio.dya.cormoran.works/)。
3. 切到第 3 层，按最左侧的 `&studio_unlock` 后连接。

### Runtime Macro

第 4 层左上角预留为 `&rmacro 0`，对应 DYA Studio 中的 Runtime Macro Slot 0。尚未配置 Slot 0 时，该键不会输出内容。keymap 中的静态 `screenshot` Macro 仍可正常使用。

### Runtime Combo

Runtime Combo 可在 DYA Studio 中创建和修改，不会覆盖 keymap 中已有的静态 `softoff` Combo。首次刷写时没有额外的默认 Runtime Combo。

### 构建产物

- `eyelash_sofle_left_dya.uf2`：左手 central，包含 DYA Studio、Runtime Macro 和 Runtime Combo
- `eyelash_sofle_right...uf2`：右手 peripheral
- `settings_reset...uf2`：清除蓝牙配对及运行时设置

## 更新列表

- 2024/12/21
  1. 增加zmk-studio支持（只需要刷新左手即可使用）。
- 2024/10/24
  1. 修改供电模式，功耗降低。
  2. 修正RGB供电自动关闭的功能。
- 2025/3/30 增加睡眠进入时间1小时  增加防抖时间 优化睡眠后功耗 
- 2025/8/22
  1. 更新了soft off。当您同时按下 Q、S 和 Z 键并按住 2 秒钟时，键盘将进入深度睡眠状态，无法通过按键唤醒。携带外出时可以使用此功能。激活方式为按一次复位开关。
  2. 这个月，我还更新了矮轴版本sofle和corne的外壳。框架和底板加厚了，复位开关的开口也进行了调整，可以轻松按下复位开关。目前，我们仍在构思如何设计带有倾斜支架的外壳。如果您仔细检查过 PCB，您会注意到有用于扩展 IO 的预留接口。不知道有没有人能够使用它们，我会尝试一下！
  3. 右侧键盘屏幕上的GIF动画被移除，这将显著降低右侧键盘的功耗。

> 如果您的键盘于2025年8月22之前更新，请更新最新的固件。
>

## 联系我

如需3D打印的模型文件或者键盘有任何异常和故障，请联系380465425@qq.com

## Sofle键位图

![Sofle键位图](keymap-drawer/eyelash_sofle.svg)
