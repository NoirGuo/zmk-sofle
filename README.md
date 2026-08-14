# Sofle ZMK 0.4 — DYA Studio

[English](README_EN.md)

这是 Eyelash Sofle 无线分体键盘的 ZMK 0.4 / DYA Studio 固件。该版本直接使用左手作为 central，不需要独立接收器；保留左右手之间的蓝牙分体通信、左手编码器和右手五向摇杆，并移除 OLED/nice_view 显示支持。

## 硬件形态

- 左手 central：负责 USB、主机蓝牙、DYA Studio 和分体事件
- 右手 peripheral：通过蓝牙连接左手
- 左手 EC11 编码器
- 右手五向摇杆
- RGB Underglow 与按键背光
- 无独立接收器、无 OLED/nice_view

## 技术栈

- ZMK：`cormoran/zmk#main+dya`（ZMK 0.4 DYA 开发线）
- Zephyr：`cormoran/zephyr#v4.1.0+zmk-fixes+nrf-half-duplex-uart`
- DYA Studio USB RPC
- Runtime Macro、Runtime Combo、Runtime Input Processor
- BLE Management、Battery History、Settings RPC

## 完整功能

### 键盘与连接

- USB 有线输入、蓝牙输入和 5 个蓝牙配置槽
- USB/BLE 输出切换与左右无线分体
- Home Row Mod、NKRO 兼容配置
- 蓝牙清除、系统重启、Bootloader 快捷键
- 低功耗休眠与 Soft Off

### DYA Studio

- USB 连接左手后在线改键
- BLE 管理、左右手电量历史和 Settings RPC
- Runtime Macro 与 Runtime Combo 在线创建、修改和保存
- Runtime Input Processor 支持
- Studio 锁定保护，通过 `&studio_unlock` 主动解锁

### Macro 与 Combo

- 静态 `screenshot` Macro：发送 macOS `Command + Shift + S`
- Runtime Macro Slot 0：Layer 3 左上角预留为 `&rmacro 0`
- 静态 `softoff` Combo：同时按下 Q、S、Z 进入深度关机
- Runtime Combo 可在 DYA Studio 中添加，不覆盖静态 Combo
- 静态与运行时 Macro/Combo 可以共存

### 左手编码器

- Base 层：音量增减
- 其他层：音量增减
- 行为保留在各层的 `sensor-bindings` 中

### 右手五向摇杆

- Base：鼠标上、下、左、右移动；中键为鼠标左键
- Layer 1：方向键上、下、左、右；中键为小键盘 Enter
- Layer 2：五个方向分别提供 MB1、MB3、MB4、MB5、MB2
- 启用 ZMK Pointing、移动/滚动加速度和 Runtime Input Processor

### 灯光与电源

- WS2812 RGB Underglow
- RGB 开关、亮度和效果切换
- PWM 按键背光
- 空闲自动休眠与 RGB 自动关闭
- Soft Off 长按保护

## 默认层级

| 层 | 用途 |
| --- | --- |
| Layer 0 | 主键盘、Home Row Mod、鼠标摇杆、音量编码器 |
| Layer 1 | F 区、导航、RGB、鼠标按键与方向控制 |
| Layer 2 | 蓝牙、USB/BLE、重启、Bootloader、Studio Unlock、鼠标扩展键 |
| Layer 3 | Runtime Macro Slot 0 和 DYA 自定义预留层 |
| Layer 4 | DYA 自定义预留层 |

Layer 2 左下角为 `&mo 3`；按住后，Layer 3 左上角的 `&rmacro 0` 可执行 Runtime Macro Slot 0。

## 连接 DYA Studio

1. 给左右手刷入同一次 Actions 构建生成的固件。
2. USB 连接左手并打开 [DYA Studio](https://studio.dya.cormoran.works/)。
3. 进入 Layer 2，按最左侧 `Studio Unlock` 键。
4. 在 DYA Studio 中选择左手串口。
5. 修改后执行 Write/Save；运行时设置保存在左手 central。

## 固件文件

| 文件 | 刷写位置 |
| --- | --- |
| `eyelash_sofle_left_dya.uf2` | 左手 central |
| `eyelash_sofle_right.uf2` | 右手 peripheral |
| `settings_reset.uf2` | 清除配对和运行时设置 |

## 推荐刷写顺序

首次安装或跨版本升级：

1. 左右手分别刷一次 `settings_reset`。
2. 给右手刷入右手固件。
3. 给左手刷入左手 DYA 固件。
4. 重启两侧，等待分体重新连接并与电脑重新配对。

不要混用 `main`、旧 ZMK 分支和 `0.4-dya` 的左右手固件。

## Runtime Macro

1. USB 连接左手，解锁并进入 DYA Studio。
2. 在 Runtime Macro 页面编辑 Slot 0，然后 Write/Save。
3. 按住 Layer 2 左下角进入 Layer 3。
4. 按 Layer 3 左上角执行 `&rmacro 0`。

未配置 Slot 0 时，该键不会输出内容。

## Runtime Combo

可在 DYA Studio 中设置按键位置、输出行为、适用层和触发时间。固件默认不创建额外 Runtime Combo，因此首次刷写不会改变现有组合键。

## 构建

1. 切换到 `0.4-dya` 分支。
2. 打开 Actions → Build ZMK firmware。
3. 运行工作流或向该分支提交改动。
4. 确认左手、右手和 settings-reset 全部成功。
5. 从 Artifacts 下载 `firmware`。

## 故障恢复

如果左右手无法连接、DYA 保存异常或蓝牙配对混乱：

1. 关闭两侧电源。
2. 左右手分别刷入 `settings_reset`。
3. 重新刷入同一次构建的右手和左手固件。
4. 删除电脑旧蓝牙配对，重启两侧并重新配对。

清除设置会删除蓝牙配对、Runtime Macro、Runtime Combo 和 DYA 运行时设置。

## 注意事项

- DYA Studio 必须连接左手 central，不能连接右手 peripheral。
- `main+dya` 属于 DYA 开发分支，升级前建议保留已验证固件。
- 左右手必须使用同一技术栈和同一次构建的固件。
- 此分支没有接收器和显示固件，不要刷入其他 Sofle 接收器/OLED 文件。

## 键位图

![Sofle 键位图](keymap-drawer/eyelash_sofle.svg)

## 参考

- [DYA Studio](https://studio.dya.cormoran.works/)
- [cormoran/zmk](https://github.com/cormoran/zmk)
- [Runtime Macro](https://github.com/cormoran/zmk-feature-runtime-macro)
- [Runtime Combo](https://github.com/cormoran/zmk-feature-runtime-combo)

## 联系方式

如需 3D 打印模型，或键盘出现硬件和固件问题，请联系：`380465425@qq.com`
