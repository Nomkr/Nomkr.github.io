---
layout: single
title: "双向 DC–DC 超级电容能量管理系统"
permalink: /cn/projects/supercapacitor/
author_profile: true
lang: cn
---

**2025.08–2026.02**

机器人急加速或冲坡时，底盘的瞬时功率需求会超过常规供电能力。为补足这部分功率，我在复刻已有开源方案的基础上做了改进，为学校 RoboMaster 战队制作了一套双向 DC–DC 能量管理系统。负载较低时，系统为超级电容组充电；底盘需要瞬时功率时，电容组放电。

<figure>
  <img src="{{ '/images/works.jpg' | relative_url }}" alt="双向 DC–DC 控制器和串联超级电容组" style="width: 100%; aspect-ratio: 2 / 1; object-fit: cover; object-position: left;">
  <figcaption>图 1　超级电容模组。左为控制器，右为超级电容组。</figcaption>
</figure>

我采用四开关 Buck–Boost 拓扑设计双向变换器，以 STM32F334 为主控，通过 ADC 同步采样和增量式 PID 实现功率、电流与电压的多环控制。硬件采用六层 PCB 与铝基板组成的双板结构，并集成过压和短路保护。针对串联电容充电时电压分配不均的问题，我为 44 F 电容组采用了被动均衡电路，限制单体过压。系统储能约 **1,980 J**，已用于机器人实战，比赛期间无故障运行。

<div class="supercapacitor-robots">
  <figure>
    <img src="{{ '/images/supercapacitor/engineer-robot.png' | relative_url }}" alt="配备六自由度机械臂和夹爪的工程机器人" loading="lazy">
    <figcaption>图 2　工程机器人，配备六自由度夹爪机械臂。</figcaption>
  </figure>
  <figure>
    <img src="{{ '/images/supercapacitor/hero-robot.png' | relative_url }}" alt="采用全舵轮底盘的英雄机器人" loading="lazy">
    <figcaption>图 3　英雄机器人，采用全舵轮底盘。</figcaption>
  </figure>
</div>

比赛期间，我负责英雄机器人（图 3）的全部硬件电路研制和整车走线。起初我以为走线是一项比较简单的工作，实际动手后才体会到整车理线的复杂。在 RoboMaster 高强度对抗中，电路安全和线路连接的可靠性直接关系到机器人能否正常运行。
