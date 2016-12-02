# 課題 (マルチプルテーブルを読む) レポート

## 課題内容

OpenFlow1.3 版スイッチの動作を説明しよう。

スイッチ動作の各ステップについて、trema dump_flows の出力 (マルチプルテーブルの内容) を混じえながら動作を説明すること。

## 方針

OpenFlow1.3 版スイッチを起動して、これに対して以下の操作を実行した。起動直後と各操作の後に dump_flows コマンドを実行して、フローテーブルの中身を確認した。

* 操作 1 : ホスト 1 からホスト 2 へパケットを送信。
* 操作 2 : ホスト 2 からホスト 1 へパケットを送信。
* 操作 3 : 再びホスト 1 からホスト 2 へパケットを送信。

## 実行結果と解説

実行結果を以下に掲載する。なお、見やすいように適宜改行やコメントを付している。

```
# まず起動直後のフローテーブルを確認

ensyuu2@ensyuu2-VirtualBox:~/learning_switch$ ./bin/trema dump_flows lsw
cookie=0x0, duration=12.520s, table=0, n_packets=0, n_bytes=0, priority=2,dl_dst=01:00:00:00:00:00/ff:00:00:00:00:00 actions=drop
cookie=0x0, duration=12.482s, table=0, n_packets=0, n_bytes=0, priority=2,dl_dst=33:33:00:00:00:00/ff:ff:00:00:00:00 actions=drop
cookie=0x0, duration=12.482s, table=0, n_packets=0, n_bytes=0, priority=1 actions=goto_table:1
cookie=0x0, duration=12.482s, table=1, n_packets=0, n_bytes=0, priority=3,dl_dst=ff:ff:ff:ff:ff:ff actions=FLOOD
cookie=0x0, duration=12.482s, table=1, n_packets=0, n_bytes=0, priority=1 actions=CONTROLLER:65535

# 操作 1

ensyuu2@ensyuu2-VirtualBox:~/learning_switch$ ./bin/trema send_packets --source host1 --dest host2
ensyuu2@ensyuu2-VirtualBox:~/learning_switch$ ./bin/trema dump_flows lsw
cookie=0x0, duration=25.202s, table=0, n_packets=0, n_bytes=0, priority=2,dl_dst=01:00:00:00:00:00/ff:00:00:00:00:00 actions=drop
cookie=0x0, duration=25.164s, table=0, n_packets=0, n_bytes=0, priority=2,dl_dst=33:33:00:00:00:00/ff:ff:00:00:00:00 actions=drop
cookie=0x0, duration=25.164s, table=0, n_packets=1, n_bytes=42, priority=1 actions=goto_table:1
cookie=0x0, duration=25.164s, table=1, n_packets=0, n_bytes=0, priority=3,dl_dst=ff:ff:ff:ff:ff:ff actions=FLOOD
cookie=0x0, duration=25.164s, table=1, n_packets=1, n_bytes=42, priority=1 actions=CONTROLLER:65535

# 操作 2

ensyuu2@ensyuu2-VirtualBox:~/learning_switch$ ./bin/trema send_packets --source host2 --dest host1
ensyuu2@ensyuu2-VirtualBox:~/learning_switch$ ./bin/trema dump_flows lsw
cookie=0x0, duration=66.643s, table=0, n_packets=0, n_bytes=0, priority=2,dl_dst=01:00:00:00:00:00/ff:00:00:00:00:00 actions=drop
cookie=0x0, duration=66.605s, table=0, n_packets=0, n_bytes=0, priority=2,dl_dst=33:33:00:00:00:00/ff:ff:00:00:00:00 actions=drop
cookie=0x0, duration=66.605s, table=0, n_packets=2, n_bytes=84, priority=1 actions=goto_table:1
cookie=0x0, duration=66.605s, table=1, n_packets=0, n_bytes=0, priority=3,dl_dst=ff:ff:ff:ff:ff:ff actions=FLOOD
cookie=0x0, duration=14.809s, table=1, n_packets=0, n_bytes=0, idle_timeout=180, priority=2,in_port=2,dl_src=19:1b:8d:39:15:d1,dl_dst=fd:0d:7a:9f:c2:36 actions=output:1
cookie=0x0, duration=66.605s, table=1, n_packets=2, n_bytes=84, priority=1 actions=CONTROLLER:65535

# 操作 3

ensyuu2@ensyuu2-VirtualBox:~/learning_switch$ ./bin/trema send_packets --source host1 --dest host2
ensyuu2@ensyuu2-VirtualBox:~/learning_switch$ ./bin/trema dump_flows lsw
cookie=0x0, duration=79.756s, table=0, n_packets=0, n_bytes=0, priority=2,dl_dst=01:00:00:00:00:00/ff:00:00:00:00:00 actions=drop
cookie=0x0, duration=79.718s, table=0, n_packets=0, n_bytes=0, priority=2,dl_dst=33:33:00:00:00:00/ff:ff:00:00:00:00 actions=drop
cookie=0x0, duration=79.718s, table=0, n_packets=3, n_bytes=126, priority=1 actions=goto_table:1
cookie=0x0, duration=79.718s, table=1, n_packets=0, n_bytes=0, priority=3,dl_dst=ff:ff:ff:ff:ff:ff actions=FLOOD
cookie=0x0, duration=27.922s, table=1, n_packets=0, n_bytes=0, idle_timeout=180, priority=2,in_port=2,dl_src=19:1b:8d:39:15:d1,dl_dst=fd:0d:7a:9f:c2:36 actions=output:1
cookie=0x0, duration=5.649s, table=1, n_packets=0, n_bytes=0, idle_timeout=180, priority=2,in_port=1,dl_src=fd:0d:7a:9f:c2:36,dl_dst=19:1b:8d:39:15:d1 actions=output:2
cookie=0x0, duration=79.718s, table=1, n_packets=3, n_bytes=126, priority=1 actions=CONTROLLER:65535

# 最後にパケットが各ホストに届いていることを確認

ensyuu2@ensyuu2-VirtualBox:~/learning_switch$ ./bin/trema show_stats host1
Packets sent:
  192.168.0.1 -> 192.168.0.2 = 2 packets
Packets received:
  192.168.0.2 -> 192.168.0.1 = 1 packet
ensyuu2@ensyuu2-VirtualBox:~/learning_switch$ ./bin/trema show_stats host2
Packets sent:
  192.168.0.2 -> 192.168.0.1 = 1 packet
Packets received:
  192.168.0.1 -> 192.168.0.2 = 2 packets

```
### 起動直後と操作 1

実行結果を見ると、スイッチ起動直後は二つのフローテーブル (テーブル 0 、テーブル 1) がある。

まず、テーブル 0 の内容は以下の通りである。

| 優先度 | 送信元 MAC アドレス / ポート | 宛先 MAC アドレス / ポート | アクション |
|:------:|:-------------------------------:|:-----------------------------:|:-----------:|
|    2     | - / -                                     | 01 から始まるアドレス / -    | ドロップ     |
|    2     | - / -                                     | 33:33 から始まるアドレス / - | ドロップ     |
|    1     | - / -                                     | - / -                                  | テーブル 1 へ |

ここから、テーブル 0 は特定の MAC アドレス宛のパケットをドロップするフィルタリングテーブルであることが推測される。また、テーブル 0 で定めたフィルタに該当しなかったパケットがテーブル 1 へ転送されることが、3 番目のエントリからわかる。 

次にテーブル 1 の内容は以下の通りである。

| 優先度 | 送信元 MAC アドレス / ポート | 宛先 MAC アドレス / ポート | アクション |
|:------:|:-------------------------------:|:-----------------------------:|:-----------:|
|    3     | - / -                                     | ff:ff:ff:ff:ff:ff / -                 | フラッディング |
|    1     | - / -                                     | - / -                                  | コントローラへ転送 |

続いて、操作 1 後のフローテーブルを見ると、新たに加わったエントリはない。一方で、フローテーブルの n_packets の内容から、パケットはスイッチによって以下の手順で処理されたことが推測される。

1. テーブル 0 ではいずれのフィルタにも該当せず、テーブル 1 へ転送される。
2. テーブル 1 の 2 番目のエントリにより、コントローラへ Packet In される。 

この後、コントローラの FDB には、ホスト 2 と接続するポートのエントリがないため、コントローラはパケットをフラッディングする。同時に、ホスト 1 とスイッチのポート 1 の対応が FDB に登録される。

### 操作 2

続いて、操作 2 後のフローテーブルを見ると、新たに以下のエントリがテーブル 1 へ追加されていることが分かる。

| 優先度 | 送信元 MAC アドレス / ポート | 宛先 MAC アドレス / ポート | アクション |
|:------:|:-------------------------------:|:-----------------------------:|:-----------:|
|    2     | 19:1b:8d:39:15:d1 (ホスト 2) / 2 | fd:0d:7a:9f:c2:36 (ホスト 1) / - | ポート 1 へ送信 |

ホスト 2 からスイッチに流入したパケットは、操作 1 の時と同様にコントローラに Packet In される。コントローラの FDB にはホスト 1 とスイッチのポート 1 の対応を表すエントリがあるので、コントローラはポート 1 へ Packet Out し、同時に上記のフローエントリがテーブル 1 へ追加されるものと考えられる。またこの時、ホスト 2 とスイッチのポート 2 の対応が FDB に登録される。

### 操作 3

続いて、操作 3 後のフローテーブルを見ると、新たに以下のエントリがテーブル 1 へ追加されていることがわかる。

| 優先度 | 送信元 MAC アドレス / ポート | 宛先 MAC アドレス / ポート | アクション |
|:------:|:-------------------------------:|:-----------------------------:|:-----------:|
|    2     | fd:0d:7a:9f:c2:36 (ホスト 1) / 1 | 19:1b:8d:39:15:d1 (ホスト 2) / - | ポート 2 へ送信 |

ホスト 1 からスイッチに流入したパケットは、操作 2 の時と同様にコントローラに Packet In される。コントローラの FDB にはホスト 2 とスイッチのポート 2 の対応を表すエントリがあるので、コントローラはポート 2 へ Packet Out し、同時に上記のフローエントリがテーブル 1 へ追加されるものと考えられる。