## Linux commands

```bash
top — посмотреть процессы, которые едят ресурсы

lsof -i [PORT] -u [USER] -p [PID] — посмотреть процессы, которые используют файл

ssh [username]@[hostname or IP address] — установить соединение по SSH

scp [option] [user_name@source_host:path/to/source/file] [user_name@target_host:target/path] — копировать файлы между девайсами по SSH

cat — вывести файл в терминал

mkdir - создать дерикторию

cp - копировать файлы 

mv - переместить файл

rmdir - удалить дерикторию (только если она пустая)

rm -r — удалить дерикторию полностью

ln [existed_file] [link] — создать ссылку на файл или дерикторию

whoami — увидель себя

useradd — создать пользователя

su — зайти в роль пользователя

man [command] — посмотреть данные о команде

whatis [command] — посмотерть данные о команде быстро

whereis [command] — вывести все, где есть command

which [command] — понять где исполняемый файл команды

wget [URL] — to download staff from the Internet

curl [URL] - to download staff from the Internet

zip [zipfile name] [what to zip]

tr — capitalize every letter

wc — показать информацию о файле (word, line, chat, byte)

grep [pattern] [file] — искать pattern в файле

# Получить доступ до кластера
dp auth configure-kubeconfig --cluster-name <cluster-name> --account-name i.a.romanov@tbank.ru
```
