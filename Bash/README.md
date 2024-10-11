# bash
```bash
# sort files list reverse alphabetical in only one column
ls -1 *.csv | sort -r
```

# search a term in .bashrc
```bash
# example: search for keyboard shortcut function
# my alias: grepb keyboard # this gives line number for 'keyboard'
# my alias: catb | sed -n '114p'

egrep -ni "keyboard" ~/.bashrc
sed -n '114{p;q}' ~/.bashrc # p=print q=quit
```

# Grep Commands
```bash
# i=case insensitive, w=exact word (not part), n = show line number
grep -iwn "template_05" util_models.py

```
