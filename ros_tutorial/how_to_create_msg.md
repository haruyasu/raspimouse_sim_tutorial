# ROSチュートリアル

## ROSチュートリアルの流れ

1. [ROSパッケージの作り方](how_to_create_pkg.md)
2. [トピックの書き方](how_to_write_topic.md)
3. [独自のメッセージファイルの作り方](how_to_create_msg.md)　←今ここ
4. [サービスを書き方](how_to_write_service.md)
5. [独自のサービスファイルの作り方](how_to_create_srv.md)

## はじめに

この章では、独自のメッセージファイルを作成し、ROSで使用する方法を解説します。

独自のメッセージファイルをROSで使用するためには、
`CMakeLists.txt`と`package.xml`というファイルを編集します。

この2つのファイルは、ROSチュートリアルの1章でパッケージを作成したときに自動で出来たファイルです。　`package.xml`はパッケージの設定が書いてあり、`CMakeLists.txt`はビルドする際の設定が書いてあります。

[ROSチュートリアルの2](how_to_use_topic.md)ではUNIX時間を配信しましたが、この章では現在日時を送るものに改良したいと思います。

まず、現在日時を送るためのメッセージファイルから作成します。

## メッセージファイルの作り方

メッセージファイルは`msg`ディレクトリに置きます。
無い場合は`msg`ディレクトリを作りましょう。

```text
roscd ros_tutorial/
mkdir msg 
```

日付と時間を送るためのメッセージを`Date.msg`とします。

```text
vim msg/Date.msg
```

`Date.msg`の中は以下のように書きます。

```text
int32 date
float64 time
```
1行目はint型32ビットの`date`で、2行目はFloat型の`time`という名前にしています。


<br>

#### ここからが**重要**です。

#### まず`package.xml`を編集します。

```text
roscd ros_tutorial/
vim package.xml
```

35行目の`<build_depend>message_generation</build_depend>`と、39行目の`<run_depend>message_runtime</run_depend>`のコメントアウトを外します。

```package.xml
 35      <build_depend>message_generation</build_depend> 
```

#### 次に`CMakeLists.txt`を編集します。

```text
vim CMakeLists.txt
```

10行目の`find_package`に`message_generation`を追加します。

```CMAkeLists.txt
10 find_package(catkin REQUIRED COMPONENTS
```

51行目の`add_message_files`のコメントアウトを外し、`Date.msg`を追加する。

```CMAkeLists.txt
51 add_message_files(                                                          
```

71行目の`generate_massages`のコメントアウトを外します。

```CMAkeLists.txt
71 generate_messages(
```

108行目の`catkin_package`の`CATKIN_DEPENDS`のコメントアウトを外し、`message_runtime`を追加します。

```CMAkeLists.txt
105 catkin_package(
```

`catkin_ws`に移動し`catkin_make`を行います。

```text
cd ~/catkin_ws
catkin_make
```

これで、ビルドが通れば正常に`Date.msg`が作成されたはずです。

確認してみましょう。
以下のように打ち込み、

```text
rosmsg show ros_tutorial/Date
```

以下のように表示されたら完了です。

```text
int32 date

```

## プログラムを改良

先程作成したメッセージファイルを使用して、プログラムを改良していきます。
まず`ros_tutorial/scripts`に移動します。

```text
roscd ros_tutorial
```

### パブリッシャ

`time_pub2.py`という名前にします。

```
vim scripts/time_pub2.py
```

```Python:time_pub2.py
#!/usr/bin/env python                                                           
        pub.publish(d)
```

```
chmod +x time_pub2.py
```

### コード解説

```Python:time_pub2.py
#!/usr/bin/env python                                                           
```
新たに、先程作成した`Date`と`datetime`をインポートしています。
`datetime`は現在日時を取得するために必要なモジュールです。


```Python:time_pub2.py
def talker():
```

`talker`という名前の関数を定義しています。

```Python:time_pub2.py
```

`l = []`はリストの初期化を行っています。

`d = Date()`はメッセージの`Date`を`d`という名前でインスタンス化しています。

```Python:time_pub2.py
```

シャットドウンされるまで10Hzでwhile内のコードを実行します。

```Python:time_pub2.py
```

`Date.msg`の`date`と`time`を初期化しています。

```Python:time_pub2.py

現在日時を取得し、`l`に渡しています。

`date`に日付、`time`に時間を渡しています。

        pub.publish(d)
```

そのため`-`と`:`を削除し、`int`と`float`型にしてパブリッシュしています。


### サブスクライバ

サブスクライバはほとんど変わらないため、`time_sub.py`をコピーし、`time_sub2.py`という名前で保存します。

```Python:time_sub2.py
#!/usr/bin/env python
import rospy
```

```
chmod +x scripts/time_sub2.py
```

### コード解説

変更点だけ解説していきます。

```Python:time_sub2.py
from ros_tutorial.msg import Date
```

`Date`をインポートしています。

```Python:time_sub2.py
    print("date : %d , time : %f" % (data.date,data.time) )
```

`date`はint型のため`%d`、`time`はfloat型のため`%f`で表示しています。

```Python:time_sub2.py
    sub = rospy.Subscriber('UnixTime', Date , callback)
```

メッセージの型を`Date`に変更しています。

## 実行方法

それぞれ別のターミナルで実行しましょう。

```text
roscore
```

```text
rosrun ros_tutorial time_pub2.py
```

```text
rosrun ros_tutorial time_sub2.py
```

## 実行結果

```text
ubuntu@ubuntu:~/catkin_ws/src/tutorial_pkg$ rosrun ros_tutorial time_sub2.py 
```

2018年11月08日の22時49分49.95257秒を指しています。