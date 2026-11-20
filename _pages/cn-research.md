---
layout: single
title: "可穿戴手部运动感知：小鱼际滚法的三维轨迹重建"
permalink: /cn/research/
author_profile: true
lang: cn
---

## 背景与问题

中医方向学习者练习小鱼际滚法时，手部每一次往返都有自己的节奏、摆幅和折返位置。我想研究的是，操作者戴好手套、完成一次短时视觉标定后，能否仅靠手套上的传感器重建拳面的三维运动轨迹，并分辨出什么时候滚得更慢、摆得更大，再与标准对比，实现可量化的学习效果。

<figure style="width: 74.8%; max-width: 23.76rem; margin-inline: 0;">
  <img src="{{ '/images/shoutao.jpg' | relative_url }}" alt="智能推拿手套实物">
  <figcaption>用于采集小鱼际滚法动作的手套原型。</figcaption>
</figure>

## 我做了什么

本项目由我独立完成。我用 STM32 实现了两枚 IMU 和 48 通道压力阵列的数据采集，开发了用于接收、显示和记录数据的 Unity 上位机。用双目相机跟踪手部的 ArUco 标记，重建标记中心的三维参考轨迹。在此基础上，我比较了多种时序模型，设计了个人标定与重戴实验，检验首段标定能否改善后续记录的轨迹估计。

<figure style="width: 74.8%; max-width: 29.92rem; margin-inline: 0;">
  <img src="{{ '/images/unity.jpg' | relative_url }}" alt="Unity 采集界面中的双 IMU 读数与压力热力图" style="object-view-box: inset(23.5% 19.3% 14.9% 31.6%);">
  <figcaption>Unity 上位机实时显示双 IMU 读数与接触压力分布。</figcaption>
</figure>

## 系统与评估方法

<figure>
  <img src="{{ '/images/tuina/system-overview.png' | relative_url }}" alt="训练和部署流程：双目视觉提供训练参考，部署时由 IMU 数据估计手部三维轨迹。">
</figure>

双目参考轨迹用于模型训练、首次个人标定和离线评分。个人标定时，用首条记录中的正常滚动数据调整时序卷积网络（TCN）的输出层，其余参数保持固定。此后，模型仅凭双 IMU 估计轨迹，后续记录的双目数据只用于评分。48 通道压力阵列用于采集和分析接触分布。

我用三维均方根误差（RMSE）衡量预测轨迹与视觉参考之间的偏差。评分时，对每条记录只做一次整体平移。计算保留了动作原有的方向、尺度和时序。

## 实测结果

我（操作者 A）重新戴上手套，用本次佩戴的首条记录 R04 做个人标定。在随后两条记录 R05 和 R06 中，仅用双 IMU 重建拳面轨迹，**正常滚动的相对三维轨迹 RMSE 为 2.11 mm，慢滚为 1.87 mm**。我的旧记录参与过基础模型训练，这两条测试记录均为新采集。每个随机种子先对两条测试记录的误差取平均，再报告三个种子的中位数。

在同次佩戴的另外两条记录中，我分别改变了滚动幅度和速度。两次从小幅切换到大幅时，预测轨迹的摆动量都随之增大。两次从慢滚切换到正常滚动时，预测的往返周期也都随之缩短。

#### A 正常滚动时的 XYZ 时间曲线

<figure>
  <img src="{{ '/images/tuina/r05-xyz.png' | relative_url }}" alt="操作者 A 重戴后，R05 正常滚动阶段的 X、Y、Z 视觉参考与预测时间曲线">
</figure>

灰线是双目视觉参考，蓝线是双 IMU 预测。X 方向上，预测能跟随每次往返的峰谷，摆幅也接近视觉参考。

#### 同一条记录的三维重建轨迹

<figure style="display: block; aspect-ratio: 960 / 780; overflow: hidden;">
  <img style="display: block; width: 100%; height: auto; margin: 0;" src="{{ '/images/tuina/r05-3d.png' | relative_url }}" alt="R05 正常滚动，个人标定后的三维预测轨迹与双目参考">
</figure>

蓝线沿着灰色参考轨迹往返，在两端折返处仍可见局部偏差。这两幅图展示的是同一条记录 R05 的正常滚动段，随机种子固定为 100。按 5–30 秒整段仅做一次平移评分，图中这一单条示例的相对三维轨迹 RMSE 为 **1.89 mm**。

## 局限与下一步

项目已完成系统搭建和离线开发验证。换一位操作者后，即使做了个人标定，模型仍可能低估折返范围。跨操作者的轨迹形状和相机深度方向的变化，目前还难以稳定重建。
