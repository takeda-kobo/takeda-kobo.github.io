こんにちは、竹田電脳工房です。

---

繰り返しですが、一連の作業をあなたに作成していただいた手順書を更新してみました。
これで問題なさそうでしょうか。
ちなみに Windows 環境なので `~` はユーザホームのディレクトリ C:\Users\(username)\ と同値ですよね。
---

🟦 1. アカウントごとに SSH キーを作成する（2 本作る）
✔ fujigy 用 SSH キー
```
ssh-keygen -t ed25519 -C "fujigy" -f ~/.ssh/id_ed25519_fujigy
```
✔ takeda-kobo 用 SSH キー
```
ssh-keygen -t ed25519 -C "takeda-kobo" -f ~/.ssh/id_ed25519_takeda-kobo
```
これで以下の 4 ファイルができます：
```
~/.ssh/id_ed25519_fujigy
~/.ssh/id_ed25519_fujigy.pub
~/.ssh/id_ed25519_takeda-kobo
~/.ssh/id_ed25519_takeda-kobo.pub
```

🟦 2. GitHub に公開鍵を登録する（アカウントごと）
✔ fujigy アカウントで GitHub にログイン
Settings → SSH and GPG keys → New SSH key
id_ed25519_fujigy.pub の中身を貼る

✔ takeda-kobo アカウントで GitHub にログイン
id_ed25519_takeda-kobo.pub を貼る

🟦 3. SSH 設定ファイルで「どのリポジトリにどの鍵を使うか」指定する
~/.ssh/config を作成（または編集）：
```
# fujigy 用
Host github.com-fujigy
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_fujigy

# takeda-kobo 用
Host github.com-takeda-kobo
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_takeda-kobo
```

🟦 4.リポジトリの remote URL を SSH 用に変更する
✔ fujigy.github.io の場合
```
cd C:\Users\jg3tx\home\repo\fujigy\fujigy.github.io
git remote set-url origin git@github.com-fujigy:fujigy/fujigy.github.io.git
```
✔ takeda-kobo.github.io の場合
```
cd C:\Users\jg3tx\home\repo\fujigy\takeda-kobo.github.io
git remote set-url origin git@github.com-takeda-kobo:takeda-kobo/takeda-kobo.github.io.git
```

🟦 5.動作確認（push が自動で正しいアカウントになる）
VSCode で：
- fujigy 側で commit → push
    → fujigy の SSH キーが使われる
- takeda-kobo 側で commit → push
    → takeda-kobo の SSH キーが使われる
