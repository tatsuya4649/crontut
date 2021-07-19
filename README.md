# What is cron?


定期的にコマンドを実行するためのデーモンプロセス。

ユーザーの指定した時間に、指定したコマンドやスクリプトを実行してくれる。

# Configure


Cronの設定ファイルは

```

crontab -e


```

で編集可能。書式としては、

```

分　秒　日　月　曜日　コマンド

* * * * * command

```

の時間とコマンドを一行で指定する。

* 分: 0 ~ 59
* 時: 0~23
* 日: 1~31
* 月: 1\~12 or jan\~dec
* 曜日: 0\~7 or sun\~sat

# Who?

コマンド実行時のユーザーは、設定した時のユーザーがそのまま実行ユーザーになる。ファイル操作やソケット操作をする場合は、cronの設定するユーザーに気をつけること。

ex. rootが設定すれば、実行されるコマンドはroot権限で実行される。

# Where? 

cronの実行時のカレントディレクトリは上で説明した実行ユーザーのホームディレクトリ。

# Output

初期設定のままだと、cronの出力はどこにも出力されない。

コマンドごとに出力を変える場合は、crontab -eで

```

*/1 * * * * command > /var/log/command.log 2>&1


```

というように設定することで、commandの標準エラー出力と標準出力が/var/log/command.logに出力される。(標準出力のみがほしい場合は、2>&1を削除。)

ちなみにデフォルトのcronの出力設定は、/etc/rsyslog.d/50-default.confにあり、

```

#cronn.*                         /var/log/cron.log


```

コメントアウトされている。このコメントアウトを外してrsyslogをrestartしてあげると、実行したコマンドの出力が設定したファイルに出力される。

# send Mail

もしも実行されたコマンドの標準出力、標準エラー出力をメール送信したい場合は、crontabの中のMAILTO環境変数にメールアドレスを書き込む。



# Command

```

usage:  crontab [-u user] file
        crontab [ -u user ] [ -i ] { -e | -l | -r }
		(default operation is replace, per 1003.2)
		-e      (edit user's crontab)
		-l      (list user's crontab)
		-r      (delete user's crontab)
		-i      (prompt before deleting user's crontab)


```

# Official document

```

# Edit this file to introduce tasks to be run by cron.
#
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
#
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').
#
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
#
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
#
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
#
# For more information see the manual pages of crontab(5) and cron(8)
"/tmp/crontab.hR34Za/crontab" 23L, 889C            1,1           Top


```
