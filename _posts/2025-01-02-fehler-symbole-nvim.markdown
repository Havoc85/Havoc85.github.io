---
layout: post
title: "Fehlerhafte Symboldarstellung in Nvim / NeoVim"
date: 2025-01-02 17:15:00 +0100
categories: font nvim
author: d33pjs
---

# Fehlerhafte Symbole

**tl;dr:** Aktualisiert eure Schriftart!

Wenn bei euch Symbole wie im folgenden Beispiel, insbesondere in nvim, nicht korrekt dargestellt werden:

![nvim_font_error](/assets/nvim_font_error.png)

bin ich bei der Recherche auf diesen Reddit-Eintrag gestoßen:  
[Reddit - Nvim: not able to see some icons](https://www.reddit.com/r/neovim/comments/12uovl4/nvim_not_able_to_see_some_icons/?rdt=40765)

Kurz gesagt: Eure Schriftart muss die Symbole natürlich darstellen können und "neue" UTF-8-Zeichen können dazu führen, dass ihr eure Schriftart aktualisieren müsst.

Ehrlich gesagt hätte ich nicht gedacht, dass das "x" in irgendeiner Form "neu" sein könnte.

Meine genutzte MesloNG Font hatte ich vor längerer Zeit aus dem Powerlevel10K-Repo heruntergeladen:
[GitHub - romkatv, powerlevel10k-media Repo](https://github.com/romkatv/powerlevel10k-media)  
Diese konnte die verwendeten Symbole jedoch nicht mehr darstellen.

Der Vollständigkeit halber: Meine Überschrift ist nicht ganz korrekt. Das "x"-Symbol im nvim-Tab hätte natürlich überall in der Konsole nicht dargestellt werden können. Mir ist es jedoch nur in nvim aufgefallen, weshalb ich meine Recherche dort begonnen habe.

Eine neuere, gepatchte Version habe ich in einem anderen Repo gefunden:  
[GitHub - ryanoasis, nerd-fonts - patched - meslo - M-DZ Repo](https://github.com/ryanoasis/nerd-fonts/tree/master/patched-fonts/Meslo/M-DZ)

Happy Hacking! 🚀
