# 取り組み
## 問題点
- N+1問題の解消
## 解決方法
- SQLのJOINを使って解決できないかと試みた。
## 考察
### 計測方法
#### **top**
- プロセスごとのcpu使用率、メモリ使用率をリアルタイムで確認できる
#### **dstat**
- サーバーのリソースの使用具合をリアルタイムで確認できる
#### **journalctl**
### なぜそう思ったか(N+1問題が起こっている根拠)
- systemdで動いているサービスのログを確認
- CPU使用率について
- mysqlについて着目した。（　CPU使用率　187.0、　TIME＋もスクショに載っていないが、大きな値だった。）　　
<img width="390" alt="スクリーン ショット 2023-01-23 に 05 05 16 午前" src="https://user-images.githubusercontent.com/103278404/214067981-0f5624fa-9af9-47dc-bd53-167e963a94a8.png">
- CPU負荷について
- サーバ全体のリソースを監視した
<img width="372" alt="スクリーン ショット 2023-01-23 に 05 07 59 午前" src="https://user-images.githubusercontent.com/103278404/214067485-f4232779-f35f-43b5-b2d6-ab0bb5218867.png">

## 変更したコード
- https://github.com/nakampany/isucon_kadai/pull/2

## 自己評価
- 自分のスキル不足で納得のいく答えは出せなかったが、スキル的に大きな挑戦で成長できたと思う。 
- 期限が1週間で、自分には初めてのことが多すぎて戸惑った。
- 環境構築はできた.
- 修正箇所はある程度見当はついたが、今までｐｈｐのORMで学習してきたので、PDOについて深い理解がなくできなかった。
- の更なる学習が必要だと感じた。
- isuconはいい問題だと思った。

# 使用技術
- AMIとPHPを使用
## AMI
- AMI ID　ami-02e8056293b4a722e
- AMI name　catatsuy_private_isu_arm64_20230114
- 推奨インスタンスタイプ　c6g.large

```
chmod 400 <設定した鍵ファイル>
ssh -i <設定した鍵ファイル> ubuntu@IPアドレス（Elastic IP）
```
## PHP

- 起動する実装をPHPに切り替えるには、以下の操作を行います。

```
$ sudo systemctl stop isu-ruby
$ sudo systemctl disable isu-ruby
$ sudo rm /etc/nginx/sites-enabled/isucon.conf
$ sudo ln -s /etc/nginx/sites-available/isucon-php.conf /etc/nginx/sites-enabled/
$ sudo systemctl reload nginx
$ sudo systemctl start php8.1-fpm
$ sudo systemctl enable php8.1-fpm
```

- エラーの出力
```
$ sudo tail -f /var/log/nginx/error.log
```

## ベンチマーカー
- 『ベンチマーカーとは：並行にアプリケーションが死なない程度に大量のリクエストを特定のWebアプリケーションに送り続けて、そのレスポンスが正しいかどうかを並行に検証し続けるプログラム』
- ベンチマーカー用インスタンスのベンチマーカー実行方法
```
$ sudo su - isucon
$ /home/isucon/private_isu.git/benchmarker/bin/benchmarker -u /home/isucon/private_isu.git/benchmarker/userdata -t http://<target IP>
```
