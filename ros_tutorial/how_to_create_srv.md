# ROSチュートリアル

## ROSチュートリアルの流れ

1. [ROSパッケージの作り方](how_to_create_pkg.md)
2. [トピックの書き方](how_to_write_topic.md)
3. [独自のメッセージファイルの作り方](how_to_create_msg.md)
4. [サービスを書き方](how_to_write_service.md)
5. [独自のサービスファイルの作り方](how_to_create_srv.md)　←今ここ

## はじめに

この章では、独自のサービスファイルを作成し、ROSで使用する方法を解説します。

[ROSチュートリアルの3](how_to_write_msg.md)で作成した日付と時間を配信するトピックを改良して、日付と時間を返すサービスを作成してみたいと思います。

## サービスファイルの書き方

独自のサービスファイルは、`srv`ディレクトリに置きます。
無い場合は`srv`ディレクトリを作りましょう。

```text
roscd ros_tutorial/
mkdir srv
```

要求に対し日時と時間を返すサービスを作成してみましょう。

```text
vim srv/DateTrigger.srv
```

`DateTrigger.srv`の中はこのように書きます。

```DateTrigger.srv
---
bool success
int32 date
float64 time
```

これは入力は何もなく、出力は`bool`の`success`と`int32`の`date`、`float64`の`time`を定義しています。

つまりサービスに対して、要求があったら`success`と`date`、`time`を返すということです。

<br>

次に`CMakeLists.txt`を編集します。

```
roscd ros_tutorial/
vim CMakeLists.txt
```

57行目の`add_service_files`のコメントアウトを外し、`DateTrigger.srv`を追加します。

```CMAkeLists.txt
 56 ## Generate services in the 'srv' folder

`catkin_ws`に移動し、`catkin_make`を行います。

```text
cd ~/catkin_ws/
catkin_make
```

これで準備完了です。

<br>
`DateTrigger.srv`が使用できるか確認していきます。

```text
rossrv show ros_tutorial/DateTrigger
```

以下のように表示されたら、正しく作成できています。

```text
---
bool success
```

## プログラムを改良

`time_pub2.py`をコピーして`date_server.py`という名前で保存します。

プログラムを以下のように改良します。

```Python:date_server.py
#!/usr/bin/env python                                                           
    except:

```

```
chmod +x scripts/date_server.py
```

### コード解説

```Python:date_server.py
from ros_tutorial.srv import DateTrigger, DateTriggerResponse
```

今回作成したサービスファイルをインポートします。

```Python:date_server.py
def callback_srv(data):
```

`callback_srv`という関数名で定義してます。

```Python:date_server.py
d = DateTriggereResponse()
```

`DateTriggereResponse`を`d`という名前でインスタンス化しています。

```Python:date_server.py
    try:
```

処理自体は同じですが、最後に`return d`で`DateTriggerResponse`を返しています。

```Python:date_server.py
    except:
```

失敗の場合は`False`を返します。

## 実行方法

```
roscore
```

```
rosrun ros_tutorial date_server.py
```

```text
rosservice call /date_call
```
## 実行結果

```text
success: True
date: 20181114
```

2018年11月14日の12時33分4.60767秒を指しています。