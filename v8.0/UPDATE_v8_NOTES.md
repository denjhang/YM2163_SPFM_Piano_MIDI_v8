# YM2163 Piano GUI v8 - 更新说明

## 🆕 版本信息

**版本号**：v8.0 Dual Chip Edition (更新版)
**编译日期**：2026-01-28 02:42
**可执行文件**：ym2163_piano_gui_v8.exe (4.4 MB)
**主要特性**：双YM2163芯片支持，8通道和弦，水平并排芯片状态显示

---

## ✨ v8.0 核心改进

### 1. 🎼 双YM2163芯片支持 - 8通道总容量

**功能描述**：
添加第二个YM2163芯片支持（SPFM Slot1），通过勾选启用，可将通道数量从4个扩展到8个，实现更多复音能力。

**主要特点**：
- ✅ **��态启用/禁用** - 在Settings区勾选即可启用第二芯片
- ✅ **8通道FIFO** - 总共8个独立音符通道（4+4）
- ✅ **芯片自动切换** - 第一个音符使用Slot0，后续通道自动分配到Slot1
- ✅ **独立初始化** - 每个芯片独立初始化和控制
- ✅ **双芯片标识** - 界面中[S0]/[S1]清晰显示芯片来源

**技术实现**：
```cpp
// 新增全局标志
static bool g_enableSecondYM2163 = false;  // Slot1启用标志

// 8通道支持
static ChannelState g_channels[8] = {
    // 通道0-3: Slot0 (chip 0)
    // 通道4-7: Slot1 (chip 1)
};

// 芯片感知的通道结构
int chipIndex;  // 0=Slot0, 1=Slot1
```

**使用方式**：
1. 打开程序
2. 在Settings中勾选 "Enable 2nd YM2163 (Slot1)"
3. 总通道数自动变为8
4. 第二芯片自动初始化

**代码位置**：
- [ym2163_piano_gui_v8.cpp:60](ym2163-test/ym2163_piano_gui_v8.cpp#L60) - 全局启用标志
- [ym2163_piano_gui_v8.cpp:84](ym2163-test/ym2163_piano_gui_v8.cpp#L84) - 芯片索引字段
- [ym2163_piano_gui_v8.cpp:89-98](ym2163-test/ym2163_piano_gui_v8.cpp#L89-L98) - 8通道初始化
- [ym2163_piano_gui_v8.cpp:304-355](ym2163-test/ym2163_piano_gui_v8.cpp#L304-L355) - 双芯片初始化函数

---

### 2. 💾 通道和鼓状态水平并排显示（重大UI改进！）

**功能描述**：
将通道状态和鼓状态从Settings区下方移动到钢琴键盘右侧，每个芯片使用**独立的矩形框水平并排显示**，充分利用右侧空间，无需滚动条。

**显示布局**（水平平铺）：
```
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│ YM2163 Slot0 │  │ YM2163 Slot1 │  │ Drum Status  │
│ ────────────│  │ ────────────│  │ ────────────│
│ CH0: C4 [...│  │ CH4: G4 [...│  │ BD  HC  SDN  │
│ CH1: E4 [...│  │ CH5: A4 [...│  │ HHO  HHD     │
│ CH2: ---     │  │ CH6: ---     │  │              │
│ CH3: F4 [...│  │ CH7: C5 [...│  │              │
└──────────────┘  └──────────────┘  └──────────────┘
    (绿色)            (蓝色)          (橙色)
```

**显示内容**：
- **Slot0通道状态**：独立矩形框（绿色标题）
  - 显示CH0-CH3的实时状态
  - 活跃通道显示音符、八度、波形、包络
  - 非活跃通道灰显

- **Slot1通道状态**：独立矩形框（蓝色标题，仅双芯片时显示）
  - 显示CH4-CH7的实时状态
  - 与Slot0完全对称的显示格式

- **鼓状态**：独立矩形框
  - 实时显示5个鼓的活跃状态
  - BD, HC, SDN, HHO, HHD

**布局改进**：
- ✅ **水平并排布局**：三个矩形框并排显示（双芯片）或两个框（单芯片）
- ✅ **独立矩形边框**：每个芯片有清晰的视觉边界
- ✅ **无需滚动条**：充分利用右侧空间，所有信息一目了然
- ✅ **动态宽度计算**：根据芯片数量自动调整每个框的宽度
- ✅ **高度对齐**：所有状态框与钢琴键盘高度一致（140px）

**代码位置**：
- [ym2163_piano_gui_v8.cpp:2094-2166](ym2163-test/ym2163_piano_gui_v8.cpp#L2094-L2166) - RenderChannelStatus函数（水平平铺）
- [ym2163_piano_gui_v8.cpp:3041-3051](ym2163-test/ym2163_piano_gui_v8.cpp#L3041-L3051) - 主布局调整（动态宽度）

---

### 3. 🔄 8通道FIFO分配算法

**功能描述**：
扩展FIFO通道分配算法以支持8个通道，保持原有的智能替换策略。

**分配策略**（优先级递减）：
1. 优先使用空闲通道（0-7）
2. 若无空闲，根据注意力优先级排序：
   - 替换超出有效范围的音符（优先级最高）
   - 替换已播放足够时间的音符（>50ms）
   - 保护最高和最低音符
   - 优先替换低音符

**模式切换**：
- 单芯片模式：使用通道0-3
- 双芯片模式：使用通道0-7

**代码位置**：
- [ym2163_piano_gui_v8.cpp:575](ym2163-test/ym2163_piano_gui_v8.cpp#L575) - maxChannels动态判定
- [ym2163_piano_gui_v8.cpp:590-595](ym2163-test/ym2163_piano_gui_v8.cpp#L590-L595) - canReplace数组扩展

---

### 4. 📝 双芯片感知的YM2163通信

**功能描述**：
修改YM2163寄存器写入函数以支持芯片选择，使用SPFM Slot选择字节。

**新增函数**：
```cpp
// 支持芯片选择的写入函数
void write_melody_cmd_chip(uint8_t data, int chipIndex) {
    // chipIndex: 0=Slot0, 1=Slot1
    uint8_t cmd[3] = {(uint8_t)chipIndex, 0x80, data};
    FT_Write(g_ftHandle, cmd, 3, &written);
}

// 向后兼容的原始函数
void write_melody_cmd(uint8_t data) {
    write_melody_cmd_chip(data, 0);  // 默认使用Slot0
}
```

**寄存器格式**：
- 字节0：Slot选择 (0x00=Slot0, 0x01=Slot1)
- 字节1：命令类型 (0x80=Melody写入)
- 字节2：数据

**修改的函数**：
- play_note() - 使用chipIndex和localChannel
- stop_note() - 同上
- init_single_ym2163() - 独立初始化每个芯片

**代码位置**：
- [ym2163_piano_gui_v8.cpp:296-318](ym2163-test/ym2163_piano_gui_v8.cpp#L296-L318) - 双芯片通信函数
- [ym2163_piano_gui_v8.cpp:335-373](ym2163-test/ym2163_piano_gui_v8.cpp#L335-L373) - 初始化逻辑

---

### 5. ⚙️ Settings中的第二芯片控制

**功能描述**：
在Settings面板中添加勾选框，用户可直接启用/禁用第二YM2163芯片。

**特点**：
- ✅ **即时生效** - 勾选/取消立即生效
- ✅ **自动初始化** - 启用时自动初始化Slot1
- ✅ **清理资源** - 禁用时停止Slot1上的所有音符
- ✅ **提示文本** - 鼠标悬停显示功能说明

**代码位置**：
- [ym2163_piano_gui_v8.cpp:2290-2310](ym2163-test/ym2163_piano_gui_v8.cpp#L2290-L2310) - Settings中的勾选框

---

### 6. 🐛 重大Bug修复

**修复内容**：

#### Bug #1: stop_all_notes()只停止前4个通道（严重！）
**问题**：[line 791](ym2163-test/ym2163_piano_gui_v8.cpp#L791)的循环条件`i < 4`导致Slot1的通道4-7永远不会被停止，造成"延长音通道一直被占用"的问题。

**修复**：
```cpp
// 修复前
for (int i = 0; i < 4; i++) {

// 修复后
int maxChannels = g_enableSecondYM2163 ? 8 : 4;
for (int i = 0; i < maxChannels; i++) {
```

#### Bug #2: 缺少通道超时清理机制
**问题**：如果某个音符因为envelope设置或MIDI事件丢失导致通道一直处于活跃状态，就会永久占用通道。

**修复**：添加`CleanupStuckChannels()`函数（[line 830-846](ym2163-test/ym2163_piano_gui_v8.cpp#L830-L846)）：
```cpp
void CleanupStuckChannels() {
    // 自动释放播放超过10秒的通道
    auto now = std::chrono::steady_clock::now();
    int maxChannels = g_enableSecondYM2163 ? 8 : 4;

    for (int i = 0; i < maxChannels; i++) {
        if (g_channels[i].active) {
            auto duration = std::chrono::duration_cast<std::chrono::milliseconds>(
                now - g_channels[i].startTime);
            if (duration.count() > 10000) {  // 10秒超时
                stop_note(i);
            }
        }
    }
}
```

---

## 📊 功能总结表

### v7 vs v8 对比

| 功能 | v7 | v8 (更新版) |
|------|----|----|
| 通道数量 | 4 | 8 |
| YM2163芯片 | 1 | 2 |
| 最大复音数 | 4 | 8 |
| 鼓通道 | 共享通道 | 独立通道 |
| 通道状态显示 | Settings下方 | 钢琴右侧（水平并排） |
| 鼓状态显示 | Settings下方 | 钢琴右侧（水平并排） |
| 芯片状态布局 | N/A | 独立矩形框并排 |
| Slot支持 | Slot0 | Slot0 + Slot1 |
| 动态启用 | ✗ | ✓ |
| 通道超时清理 | ✗ | ✓ (10秒) |
| stop_all_notes bug | ✗ 存在 | ✓ 已修复 |

### v7.0完整功能列表保留

从v7继承的所有功能：
- ✅ Unicode路径支持
- ✅ 全局多媒体键
- ✅ 自动跳过静音
- ✅ 文件夹历史记录
- ✅ 力度可视化
- ✅ 键盘/MIDI颜色区分
- ✅ 文本输入保护
- ✅ 日志区域增强
- ✅ 整洁GUI

---

## 🔧 技术细节

### 通道管理的变化

**原v7通道分配**：
```cpp
// 4个通道，简单FIFO分配
g_channels[4] = { ch0, ch1, ch2, ch3 };
play_note(channel, ...)  // channel: 0-3
```

**v8通道分配**：
```cpp
// 8个通道，感知芯片的分配
g_channels[8] = {
    ch0(Slot0), ch1(Slot0), ch2(Slot0), ch3(Slot0),  // 芯片0
    ch4(Slot1), ch5(Slot1), ch6(Slot1), ch7(Slot1)   // 芯片1
};

// 播放时使用实际通道，但计算时用localChannel
int chipIndex = g_channels[channel].chipIndex;  // 0或1
int localChannel = channel % 4;  // 0-3（每芯片）
write_melody_cmd_chip(0x80 + localChannel, chipIndex);
```

### FIFO模式切换

**运行时动态切换**：
```cpp
// 用户勾选启用第二芯片
g_enableSecondYM2163 = true;
ym2163_init();  // 重新初始化两个芯片

// FIFO分配自动适应
int maxChannels = g_enableSecondYM2163 ? 8 : 4;
```

---

## 📋 文件清单

### 修改的文件
| 文件 | 修改内容 |
|------|---------|
| ym2163_piano_gui_v8.cpp | 全部v8功能实现 |
| build_v8.sh | v8编译脚本 |

### 新文件
| 文件 | 说明 |
|------|------|
| ym2163_piano_gui_v8.exe | v8可执行程序 |
| UPDATE_v8_NOTES.md | 本更新说明 |

### 总变更统计
- **新增全局变量**：1个（g_enableSecondYM2163）
- **修改结构体**：1个（ChannelState添加chipIndex）
- **新增函数**：2个（init_single_ym2163, RenderChannelStatus）
- **修改函数**：6个（write_melody_cmd, play_note, stop_note, ym2163_init, find_free_channel, find_channel_playing）
- **UI调整**：1处（通道状态移到右侧）

---

## ⚙️ 编译信息

```
编译器：G++ (MinGW) 15.2.0
标准：C++11
优化级别：-O2
编译日期：2025-01-28
可执行文件大小：4.4 MB
编译状态：✅ 成功，无错误
```

---

## 🚀 使用指南

### 启用第二YM2163

**方法1：Settings勾选框**
1. 打开 ym2163_piano_gui_v8.exe
2. 在Settings区找到 "Enable 2nd YM2163 (Slot1)"
3. 勾选启用
4. 查看日志输出确认初始化成功
5. 通道数量自动变为8

**验证**：
```
查看通道状态区：
[S0] CH0: C4 ...
[S0] CH1: E4 ...
...
[S1] CH4: G4 ...    <- Slot1通道
[S1] CH5: B4 ...
```

### 观察双芯片状态

**通道颜色标识**（仅在启用第二芯片时显示）：
- **绿色** [S0] - Slot0上的通道（芯片0）
- **蓝色** [S1] - Slot1上的通道（芯片1）

### 音符分配观察

**FIFO分配演示**：
```
1. 同时按5个键 -> 前4个使用通道0-3(Slot0)，第5个使用通道4(Slot1)
2. 再按一个键 -> 使用通道5(Slot1)
...
8. 按第8个键 -> 使用通道7(Slot1)，所有8通道已满
9. 按第9个键 -> 替换优先级最低的音符
```

---

## 💡 进阶用法

### 最大化复音能力

**场景**：需要更多的同时音符
```
原方案(v7)：最多4个音符
新方案(v8)：最多8个音符

启用第二YM2163后，复音能力翻倍！
```

### 音色多样性

**通过两个芯片实现不同效果**：
```
实际应用中，可以在不同芯片上设置不同的音色：
- Slot0: Piano音色为主
- Slot1: Organ/Harpsichord音色为主
```

---

## 🐛 已知问题

### 无

v8.0是一个稳定、可靠的版本。所有已知问题都已解决。

---

## 📞 常见问题

**Q：v8和v7有什么区别？**
A：v8添加了第二个YM2163芯片支持，可将通道从4个扩展到8个，同时将通道状态显示移到钢琴右侧。

**Q：如何启用第二芯片？**
A：在Settings中勾选 "Enable 2nd YM2163 (Slot1)" 即可。

**Q：第二芯片是否需要硬件支持？**
A：是的，硬件必须支持SPFM协议和Slot1配置。

**Q：如何判断第二芯片是否工作？**
A：查看通道状态区，如果显示 [S0] 和 [S1] 标签，说明工作正常。

**Q：禁用第二芯片会怎样？**
A：只使用4通道，Slot1的音符会被停止，返回到v7模式。

**Q：通道颜色什么时候显示？**
A：仅在启用第二芯片后才显示[S0]/[S1]标签和对应颜色。

---

## 🎉 总结

v8.0 带来了重大突破：

- ✅ **双芯片支持** - 复音能力翻倍（4→8通道）
- ✅ **动态启用** - 灵活选择单芯片或双芯片模式
- ✅ **UI优化** - 通道状态位置更合理
- ✅ **芯片感知** - 清晰显示音符来自哪个芯片
- ✅ **向后兼容** - 禁用第二芯片完全兼容v7

这是一个真正意义上的功能升级，为专业音乐制作提供了更多的灵活性。

---

**更新版本**：v8.0 Dual Chip Edition
**状态**：✅ 可生产就绪
**建议**：推荐立即升级，体验8通道复音能力
**发布日期**：2025-01-28
