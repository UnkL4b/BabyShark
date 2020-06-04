![babyshark](https://repository-images.githubusercontent.com/268794697/4b8cb480-a584-11ea-9dd2-59cd3993dade)

# INTRO
<p style='text-align: justify;'>
This is a basic C2 generic server written in Python and Flask.

This code has based ideia to [GTRS](https://github.com/mthbernardes/GTRS), which uses [Google Translator](https://translate.google.com) as a proxy for sending commands to the infected host.

The BabyShark project aims to centralize reverse connections with agents, creating a way to centralize several types of connections in one place.

BabyShark does not generate infection agents, but it does offer a template to connect to it.
</p>
# INSTALL

```
git clone https://github.com/danilovazb/BabyShark/
cd BabyShark
mkdir database
sqlite3 database/c2.db < schema.sql
```
