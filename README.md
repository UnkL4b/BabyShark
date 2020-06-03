![babyshark](https://repository-images.githubusercontent.com/268794697/4b8cb480-a584-11ea-9dd2-59cd3993dade)

# INTRO

This is a basic C2 server written in Python and Flask.

This code has based to [GTRS](https://github.com/mthbernardes/GTRS), which uses [Google Translator](https://translate.google.com) as a proxy for sending commands to the infected host.

# INSTALL

```
git clone https://github.com/danilovazb/BabyShark/
cd BabyShark
mkdir database
sqlite3 database/c2.db < schema.sql
```
