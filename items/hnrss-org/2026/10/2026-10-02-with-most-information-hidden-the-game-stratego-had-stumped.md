---
title: With most information hidden, the game Stratego had stumped AI until now
link: https://arstechnica.com/science/2026/10/ai-finally-beat-the-best-stratego-player-in-history-and-did-it-on-a-budget/
source: hnrss-org
published: 2026-10-02T14:11:24Z
updated: 2026-10-02T14:11:24Z
first_seen: 2026-10-03T00:25:47.283928759Z
authors:
- PaulHoule
summary: 'https://www.nature.com/articles/s41586-026-11036-y https://arxiv.org/abs/2511.07312 Comments URL: https://news.ycombinator.com/item?id=49933740 Points: 159 # Comments: 79'
content: extracted
html: 2026-10-02-with-most-information-hidden-the-game-stratego-had-stumped.html
preview:
  file: 2026-10-02-with-most-information-hidden-the-game-stratego-had-stumped.preview-314b103059cd.webp
  width: 256
  height: 144
  alt: A cartoon version of a Stratego piece, specifically one showing a bomb.
  color: '#b1c7e1'
images:
- source: https://cdn.arstechnica.net/wp-content/uploads/2026/10/GettyImages-1864707238-1152x648.jpg
  original:
    file: 2026-10-02-with-most-information-hidden-the-game-stratego-had-stumped.image-465e54804c81.jpg
    width: 1152
    height: 648
  color: '#fefefe'
---

Deep Blue took down Garry Kasparov at chess in 1997, AlphaGo beat Lee Sedol at Go in 2016, and poker bots have been beating professionals for years. But one classic game called *Stratego* held out. Even DeepMind, with its exceptional budget, couldn’t build a machine that reliably beat the best human players.

Now, a team of researchers from Carnegie Mellon, MIT, New York University, and Stanford University has done it. Their AI, called Ataraxos, beat Pim Niemeijer, arguably the best *Stratego* player of all time, 15 games to one, with four draws. And it took just 16 GPUs and a few thousand dollars to train it.

## Hidden armies

In *Stratego*, each player gets 40 pieces representing military ranks, from a marshal down to a spy, plus bombs and a flag. You win by capturing the opponent’s flag. Your opponent knows *where* your pieces are, but not *what* they are. Identities are revealed only when two pieces collide in battle—the weaker one is removed, and the identity of the winner is revealed. That makes *Stratego* an imperfect-information game, just like poker, which computers cracked years ago. “There’s something super distinctive about *Stratego*, which is that it is a massive amount of hidden information that unfolds over a very long time scale,” said Eugene Vinitsky, a researcher at NYU and co-author of the study.

In some forms of poker, the hidden information is tiny. In Texas Hold’em, “You only have two hidden cards,” said Gabriele Farina, an MIT computer scientist and another co-author. That leaves just 1,326 possible hands, few enough for a machine to weigh them all. “In *Stratego*, there’s 40 pieces on the board that could be in any order,” Farina said. That’s more than a decillion possible setups. Then there’s the game’s length.

“In chess, usually the game lasts 40 moves, but in *Stratego*, a game can easily last 2,000 moves,” Farina said. On top of that, *Stratego* is a game of bluffing. Sometimes you move a weak piece as if it were a marshal, just to scare the opponent off. When players bluff too often, their threats mean nothing; when they never bluff, they become predictable. That balancing act, the team explains, is what stumped earlier AIs like DeepMind’s DeepNash, introduced in 2022.
