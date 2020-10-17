---
title: 'github複数アカウントの使い分け'
date: '2020-10-17'
---

① ~/.ssh配下に新ディレクトリ作成
```
mkdir ~/.ssh/sasaking
```
(sasakingは適当に)

② ssh-keygenコマンドで鍵を生成 (githubに登録してるメアド)
```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com" -f ~/.ssh/sasaking/id_rsa
```

③ 確認
```
ls ~/.ssh/sasaking
```
id_rsaとid_rsa.pubがあればOK

④ 公開鍵をコピー
```
pbcopy < ~/.ssh/sasaking/id_rsa.pub
```

⑤ githubに公開鍵を登録  
Settings → SSH and GPG keys → New SSH key  
Titleを適当に入力  
Keyに④を貼り付け  
Add SSH Keyをクリック

⑥ アカウント使い分け
```
vim ~/.ssh/config
```

中身に以下を追加
```
Host github-sub
  HostName github.com
  IdentityFile ~/.ssh/sasaking/id_rsa
  User git
  Port 22
  IdentitiesOnly yes
```

⑦ 接続確認
```
ssh -T github-sub
```
↓これが出たら成功
```
Hi sasaking! You've successfully authenticated, but GitHub does not provide shell access.
```
⑧ リモートリポジトリ確認
```
git remote -v
```

もし違うリポジトリにpushしたいなら、
```
git remote rm origin
git remote add origin git@github-sub:sasaking/next-training.git
git push -u origin master
```