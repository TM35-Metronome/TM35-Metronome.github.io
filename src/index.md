

# Metronome
##### *A set of tools for modifying and randomizing Pokémon games*
<nav>
    <a href="https://github.com/TM35-Metronome/metronome/releases">download</a> |
    <a href="https://github.com/TM35-Metronome/metronome">source</a>
</nav>

Metronome is built as a competitor to [PPRE](http://tinyurl.com/r6fuhcb),
the [Universal Pokemon Randomizer](http://tinyurl.com/yyu9msno), and
[other tools](http://tinyurl.com/vahcn2n) used to modify Pokémon games.

Unlike most of these tools, Metronome is built from the ground up to be
extendible, meaning that anyone can build their own tool on top of
Metronome. So far, a [Randomizer](/randomizer.html) has been built
using Metronome.

Metronome is a set of programs called `tm35-load` and `tm35-apply`.
`tm35-load` is a command-line tool that loads a Pokémon rom and
outputs a lot of data about this rom:

<pre><code>> <span class="sh-comment"># Full output is omitted with `head`</span>
> tm35-load emerald.gba | head
.version=Emerald
.game_title=POKEMON EMER
.gamecode=BPEE
.starters[0]=277
.starters[1]=280
.starters[2]=283
.trainers[0].class=0
.trainers[0].encounter_music=0
.trainers[0].trainer_picture=0
.trainers[0].items[0]=0
</code></pre>

The format is really simple. Each line is a sequence of field
and array accesses followed by a value. `.trainers[0].encounter_music=0`
should be read as:

```
field 'trainers'
index '0'
field 'encounter_music'
is '0'
```

`tm35-apply` is the counterpart to `tm35-load`. It takes the same
format that `tm35-load` outputs, and applies that information to a
Pokémon rom.

## Examples

<pre><code>> <span class="sh-comment"># Set Treeckos first type to be Fire.</span>
> <span class="sh-comment"># This will make Treecko a Fire/Grass type.</span>
> echo <span class="sh-string">'.pokemons[277].types[0]=Fire'</span> | tm35-apply emerald.gba
</code></pre>

![randomizer](/images/treecko-fire-grass.png)

<pre><code>> <span class="sh-comment"># Set the max level of all wild Pokémons to 100</span>
> tm35-load emerald.gba | sed <span class="sh-string">'s/max_level=.*$/\max_level=100/'</span> |
      tm35-apply emerald.gba
</code></pre>

![randomizer](/images/wild-max-100.png)

<pre><code>> <span class="sh-comment"># 150 power, 25% accuracy, and 5 PP for all moves. Very fun!</span>
> tm35-load plat.nds | sed -e <span class="sh-string">'s/\.power=.*$/.power=150/'</span> \
      -e <span class="sh-string">'s/\.accuracy=.*$/.accuracy=25/'</span> \
      -e <span class="sh-string">'s/\.pp=.*$/.pp=5/'</span> | tm35-apply plat.nds
</code></pre>

![randomizer](/images/150pw-25ac-5pp.png)

