---
layout: post
title: "Uneinigkeit bei SSH und ZSH"
date: 2024-12-07 00:38:00 +0100
tags: [zsh, ssh, shorts]
lang: de
author: d33pjs
---

### Tab-Completion unter zsh für SSH liefert falsche Hostnamen? Hier ist die Lösung!

**tl;dr:** Überprüfe deine `~/.ssh/known_hosts`.

Da ich selbst lange nach einer Lösung für ein merkwürdiges Problem mit der Tab-Completion gesucht habe, möchte ich meine Erkenntnisse mit euch teilen – vielleicht erspare ich dem einen oder anderen ein paar graue Haare.

---

#### Das Problem

Meinem Heimnetz besteht fast nur aus Linux-Maschinen, die ich ausschließlich per SSH verwalte. SSH ist daher ein essenzieller Bestandteil meines Arbeitsalltags. Doch seit Monaten hatte ich ein nerviges Problem mit der Tab-Completion unter zsh (in Kombination mit oh-my-zsh):

Wenn ich z. B. `ssh langerhostna<tab>` tippte, wurde `ssh langerhostname.` vervollständigt. Das ist jedoch kein korrekter Hostname in meinem Setup, und folglich scheiterte der SSH-Befehl mit der Fehlermeldung:

```
Could not resolve hostname langerhostname.: Name or service not known
```

Frustrierend.

Noch ärgerlicher war, dass ein doppeltes Tab (`<tab><tab>`) lange Zeit kein alternatives Ergebnis lieferte. Doch irgendwann änderte sich das Verhalten – vermutlich durch ein Update. Plötzlich zeigte mir zsh bei `ssh langerhostna<tab><tab>` zwei Varianten zur Auswahl:

- `langerhostname.dmz`
- `langerhostname.int`

Das brachte mich endgültig ins Grübeln. In meiner `~/.ssh/config` waren zwar korrekte Aliase wie `langerhostname` und `langerhostname.int` definiert, aber definitiv nichts mit `.dmz`.

---

#### Die Lösung

Der neue Hinweis mit `.dmz` gab mir endlich eine Richtung, um das Problem zu debuggen. Während ich von `grep -ril dmz ./.*` noch auf Ergebnisse wartete, bekam ich von ChatGPT den entscheidenden Tipp:

**Überprüfe die Datei `~/.ssh/known_hosts`.**

Und tatsächlich! Dort fanden sich alte Einträge von Testservern, die mit `.dmz` endeten. Hostnamen aus known_hosts werden also offenbar von der Zsh-Completion mit einbezogen.

Ich habe die entsprechenden Zeilen aus der `known_hosts` entfernt, und siehe da – die Tab-Completion funktioniert jetzt wieder wie erwartet.

---

#### Fazit

Wenn die Tab-Completion unter Zsh für SSH nicht korrekt funktioniert oder unerwartete Ergebnisse liefert, lohnt sich ein Blick in die Datei `~/.ssh/known_hosts`. Alte oder fehlerhafte Einträge können hier die Ursache sein. Laut ChatGPT könnte aber natürlich auch die globale SSH Config `/etc/ssh/ssh_config` oder `/etc/ssh/ssh_known_hosts` oder `.zshrc` oder sogar die Hostnamen Auflösung für Probleme sorgen: je nach Setup mal mit `getent hosts langerhostname` schauen.

Ich hoffe, diese Erkenntnis erspart euch die monatelange Suche, die ich hinter mir habe. 😊

Happy Hacking! 🚀

