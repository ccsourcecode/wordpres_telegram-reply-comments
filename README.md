# wp-comments-tgbot
A Telegram Bot for WordPress comments! Simple and fast reply to your comments!
![](pics/1.png)

![](pics/2.jpg)

# Warning
By using this plugin, you agree to open Basic Auth on your WordPress.

Please setup a complex passphrase for your WordPress

# 使用方式
基本可以如下歸納

# 安裝插件
創建 bot、獲取自己的 user id
 插件中添加相應的設置
編輯配置，並運行 bot 後端
可以了。

# 注意事項
服務器在國內
WordPress 的服務器上，想辦法開個代理（目前只支持 http 代理），然後插件設置裡加入配置信息；

Telegram 的服務器上，也想辦法開個代理，然後設置這樣兩條環境變量

export http_proxy=http://127.0.0.1:1087;export https_proxy=http://127.0.0.1:1087;
或者乾脆上 proxychains 試試。

proxychains 貌似只能代理 dynamically linked 的程序，golang 的二進制是 statically linked 的哎
Basic Auth
Basic Auth 就是在 HTTP 請求頭裡加上 base64 編碼之後的認證信息。優點是簡單方便，易於實現，缺點很多。

但是有一點是肯定的，在 www 這個層面上，不走 TLS 的登錄，都是耍流氓。

-c 指定配置文件路徑，默認使用當前目錄下的 config.json

-f 強制運行，哪怕是 http 的……

-c file set configuration file (default "config.json")
-f force to run even on http sites.
-h this help
-v show version and exit

# Requirements
1. WordPress native comment system.

2. Network connection to Telegram.

# General guide
## 1. Install plugin
ssh to your blog
```shell script
cd /path/to/root/wp-content/plugins
git clone https://github.com/BennyThink/wp-comments-tgbot
``` 

Then navigate to your Plugin and enable WordPress Comments Telegram Bot.
## 2. Create bot and find your user id
Talk to [@BotFather](https://t.me/BotFather), create your own bot and copy bot token.

Talk to [@get_id_bot](https://t.me/get_id_bot), get your own user id.

## 3. Edit configuration on WordPress
Fill in your token, user id and proxy if needed.
![Alt text](pics/plugin.png)

## 3. Download binary on GitHub Release
[GitHub Release](https://github.com/BennyThink/wp-comments-tgbot/releases)

## 4. Create config
Create your own `config.json`
```json
{
  "username": "admin",
  "password": "admin",
  "url": "http://localhost/",
  "token": "907J6Tw",
  "uid": "23231321",
  "admin": 1,
  "tail": "本评论由Telegram Bot回复～❤️"
}
```
Explanations:
* username & password: username and password for WordPress
* url: site url, must include `/` as suffix.
* token: Telegram bot token
* uid: your Telegram user id
* admin: your user id in WordPress, typically it should be 1
* tail: suffix message to your reply.

## 5. Run
Supply `config.json` as `-c /path/tp/config.json`. By default it will search on working directory.
```shell script
chmod u+x /path/to/bot
/path/to/bot -c /path/to/config.json
```
### cli arguments
```text
  -c file  set configuration file (default "config.json")
  -f      force to run even on http sites.
  -h      this help
  -v      show version and exit
```
# Q&A
## 1. No email notifications while replying by telegram bot?
Potentially bug about your theme/plugin. Try to find the code of sending email.

You may find some hook like this:
``php
add_action('comment_post', 'comment_mail_notify');	
``
Change `comment_post` to `wp_insert_comment` will solve this issue.

[Reference commit](https://github.com/BennyThink/WordPressGit/commit/c64a3a5e70e10239ba9217debf16a075e2a13874)

# Credits
* [Basic Auth](https://github.com/WP-API/Basic-Auth)

# License
GPLv2
