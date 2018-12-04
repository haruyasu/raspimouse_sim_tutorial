# シミュレータ上のラズパイマウスを動かす方法Part5

## ラズパイマウスを動かすまでの流れ

1. [距離センサの値の読み取り方](how_to_control_raspimouse_on_sim_1.md)
2. [モータの動かし方](how_to_control_raspimouse_on_sim_2.md)
3. [キーボードを用いたラズパイマウスの動かし方](how_to_control_raspimouse_on_sim_3.md)
4. [コントローラを用いたラズパイマウスの動かし方](how_to_control_raspimouse_on_sim_4.md)
5. [距離センサの値を利用したラズパイマウスの動かし方](how_to_control_raspimouse_on_sim_5.md)←今ここ
6. [測域センサ(URG)を用いたSLAMの行い方](how_to_control_raspimouse_sim_6.md)

Part5では左手法という迷路の解析手法を利用して、ラズパイマウスをスタート地点からゴールまで動かしてみようと思います。

## 左手法について

左手法とは、優先的に左側を向き進み、常に左側の壁に沿って動くことでゴールを目指す手法のことです。
マイクロマウス競技などで使用されています。

|次に取る行動|条件1|条件2|条件3|条件4|
|---|---|---|---|---|
|左を向く|左、右、正面が空いている|左、右が空いている|左、正面が空いている|左のみが空いている|
|前進する|正面、右が空いている|正面のみが空いている|なし|なし|
|右を向く|右のみが空いている|なし|なし|なし|
|Uターンする|すべて塞がっている|なし|なし|なし|

## サンプルプログラム

今回はPythonで書きます。
名前は`left_hand.py`とします。


```Python:left_hand.py
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import sys, rospy

```

## コード解説

### lsf_listener関数

rtlightsensorファイルの中を読み、数値だけを返しています。
その値をmotor_vel関数に渡しています。



## 実行結果
