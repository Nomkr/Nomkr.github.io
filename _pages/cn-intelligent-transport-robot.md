---
layout: single
title: "中国机器人大赛 · 智能搬运机器人"
permalink: /cn/projects/intelligent-transport-robot/
author_profile: true
lang: cn
---

**2026 · 福州大学 · 车型智能搬运赛全国一等奖**

我与队友共同开发了一台面向模拟运动会场景的自主搬运机器人。它需要把随机排列、形状各异的物块按颜色送入指定区域，将同色但形状不同的物块叠放起来，还要将冠、亚、季军奖杯放到不同高度的领奖台上。两项任务都要求机器人自主行驶到目标附近，准确对准后再完成取放。

<figure style="width: 95%; margin-inline: auto;">
  <video controls playsinline preload="metadata" aria-label="智能搬运机器人比赛实录" style="display: block; width: 100%; height: auto;">
    <source src="{{ '/assets/video/transport-robot-competition.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>比赛实录 · <a href="{{ '/assets/video/transport-robot-competition.mp4' | relative_url }}">打开 MP4 视频</a></figcaption>
</figure>

## 机器人如何完成搬运

我们用两块 STM32 单片机分别负责任务调度和底盘运动控制。麦克纳姆轮底盘根据 OPS 定位反馈沿直线或圆弧行驶。拾取奖杯时，树莓派通过 OpenCV 识别奖杯上的“冠”“亚”“季”字样。机器人沿圆弧行驶的同时，转盘内的舵机完成奖杯分拣，整个过程无需停车。

搬运物块时，树莓派通过 OpenCV 识别颜色，并根据边缘角度区分圆锥和长方体。主控记录每件物料在转盘上的位置，吸盘机械臂负责拾取长方体。机器人根据 OPS 定位到达投放点后，利用视觉对准目标圆心，微调车身位置。转盘选出对应物料后，关闭气泵阀门释放长方体，完成堆叠。

## 我的硬件工作

我担任队长，负责硬件设计与制作，主要工作是设计和焊接两块 PCB：STM32F429IGT6 核心板，以及集成输入保护、三路降压和整车外设接口的拓展板。

<figure>
  <img src="{{ '/images/transport-robot/integration-board.jpg' | relative_url }}" alt="装有核心板的电源与外设集成板" loading="lazy" style="width: 49%; height: auto;">
  <img src="{{ '/images/transport-robot/core-board.jpg' | relative_url }}" alt="STM32F429IGT6 核心板" loading="lazy" style="width: 49%; height: auto;">
  <figcaption style="width: 100%;">左：装有核心板的集成板。右：独立的 STM32F429 核心板。</figcaption>
</figure>

### STM32F429 核心板

我把核心板设计为一块便于批量生产的 60 mm × 45 mm 四层 PCB。板上使用 CAT6219 将 5 V 转为 3.3 V，为 STM32F429IGT6 和逻辑器件供电，并配有晶振电路、AT24C02 EEPROM 和 32 MB SPI Flash。USB-C 和 SWD 接口用于单板供电和程序调试。核心板通过板对板（BTB）连接器与集成板拼装。

### 电源与外设集成板

我把整车配电、输入保护和外设接口集中在一块 100 mm × 80 mm 的四层 PCB 上。24 V 输入直接为底盘供电。一颗 LM2678 和两颗 TPS5430 提供三路降压输出：气泵和舵机控制板使用 12 V，主控等逻辑设备使用 5 V，OPS 定位模块单独使用一路带静电保护的 5 V 电源。

电源入口设有 TVS 瞬态保护、防反接和缓启动电路。底盘、核心板和 OPS 各有独立的供电开关，调试时可以单独上电。集成板为树莓派、舵机控制板、扫码模块和补光灯预留了接口。MAX3232 负责 OPS 的 RS-232 接口与 STM32 串口之间的电平转换，TB6612 双 H 桥控制气泵启停和电磁阀通断。

<figure style="max-width: 28rem;">
  <img src="{{ '/images/intelligent-transport-robot.jpg' | relative_url }}" alt="智能搬运机器人实物，可见转盘、吸盘机构和底盘" loading="lazy">
  <figcaption>智能搬运机器人整机。</figcaption>
</figure>
