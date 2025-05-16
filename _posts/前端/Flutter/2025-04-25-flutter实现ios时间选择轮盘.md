---
layout: post
title: Flutter实现IOS时间选择轮盘
date: 2025-04-25 00:33 +0800
---
```dart
import 'package:flutter/material.dart';

/// 倒计时数字样式
TextStyle countdownStyle = const TextStyle(
  fontFamily: 'Microsoft Yahei',
  fontSize: 80,
  fontWeight: FontWeight.w900,
  letterSpacing: 6,
);

/// 倒计时设置组件
///
/// 用于设置倒计时的时分秒，通过轮盘选择器实现
class CountdownSet extends StatefulWidget {
  final int allSeconds; // 总秒数
  final Function setSeconds; // 设置秒数的回调函数

  const CountdownSet(this.allSeconds, this.setSeconds, {super.key});

  @override
  State<CountdownSet> createState() => _CountdownSetState();
}

class _CountdownSetState extends State<CountdownSet> {
  late int hour; // 小时
  late int minute; // 分钟
  late int second; // 秒

  @override
  void initState() {
    super.initState();
    // 初始化时分秒
    hour = widget.allSeconds ~/ 3600; // 计算小时数
    minute = (widget.allSeconds % 3600) ~/ 60; // 计算分钟数
    second = widget.allSeconds % 60; // 计算秒数
  }

  /// 更新总秒数
  ///
  /// 将当前的时分秒转换为总秒数，并通过回调函数传递给父组件
  void _updateTotalSeconds() {
    int totalSeconds = hour * 3600 + minute * 60 + second;
    widget.setSeconds(totalSeconds);
  }

  /// 构建轮盘选择器
  ///
  /// [items] 可选数字列表
  /// [value] 当前选中的值
  /// [onChanged] 值改变时的回调函数
  Widget _buildScrollWheel(
    List<int> items,
    int value,
    Function(int) onChanged,
  ) {
    return SizedBox(
      width: 130,
      height: 120,
      child: ListWheelScrollView(
        itemExtent: 100, // 每个选项的高度
        diameterRatio: 1.5, // 轮盘的直径比例
        useMagnifier: true, // 使用放大镜效果
        magnification: 1, // 放大倍数
        controller: FixedExtentScrollController(
          initialItem: items.indexOf(value), // 设置初始选中项
        ),
        onSelectedItemChanged: (index) {
          onChanged(items[index]); // 值改变时调用回调函数
          _updateTotalSeconds(); // 更新总秒数
        },
        children: items.map((item) {
          return Center(
            child: Text(
              item.toString().padLeft(2, '0'), // 补零显示两位数
              style: countdownStyle,
            ),
          );
        }).toList(),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Row(
      mainAxisAlignment: MainAxisAlignment.center,
      crossAxisAlignment: CrossAxisAlignment.center,
      children: [
        // 小时选择器
        _buildScrollWheel(
          List.generate(24, (index) => index), // 0-23小时
          hour,
          (value) => setState(() => hour = value),
        ),
        Text(':', style: countdownStyle),
        // 分钟选择器
        _buildScrollWheel(
          List.generate(60, (index) => index), // 0-59分钟
          minute,
          (value) => setState(() => minute = value),
        ),
        Text(':', style: countdownStyle),
        // 秒数选择器
        _buildScrollWheel(
          List.generate(60, (index) => index), // 0-59秒
          second,
          (value) => setState(() => second = value),
        ),
      ],
    );
  }
}
```