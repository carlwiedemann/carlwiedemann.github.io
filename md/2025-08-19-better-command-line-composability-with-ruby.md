# Better command line composability with Ruby

2025-08-19

The Unix Philosophy of small, composable, text-based utilities gives us a lot of mileage. It's quite an elegant vehicle, to receive our next bespoke requirement as yet another piped command. But if you've done this enough you know what its like when that vehicle's mileage runs short.

Usually it happens after we are piping things together quite handily.

**"I have a space delimited text file..."**

> *Okay.*

<pre><code class="language-bash">
❯ cat FOO.txt
foo bar
foo buzz
bar bar
bar buzz
alpha bravo
charlie delta
</code></pre>

**"...where I want to know first word on each line..."**

> *Great, that's easy, I'll just pipe `cat` to `cut`.*

<pre><code class="language-bash">
❯ cat FOO.txt | cut -d ' ' -f 1
foo
foo
bar
bar
alpha
charlie
</code></pre>

**"...and the word's total count in the file..."**

> *Yep I'll pipe `cut` to `uniq`.*

<pre><code class="language-bash">
❯ cat FOO.txt | cut -d ' ' -f 1 | uniq -c
   2 foo
   2 bar
   1 alpha
   1 charlie
</code></pre>

**"...and it should be JSON formatted hash map, keyed by count mapped to arrays of words having that count, the arrays are sorted in order from greatest word length to least."**

> *Ah well...now I have to write a custom script from scratch.*

## Out of gas

This is when "just one more pipe" falls short. We've all been there, devotedly fussing around with `awk`, `sed`, `grep`, `tr`, skirting the narrow boundary between clever and clandestine. But ultimately we arrive at a place where these tools just aren't enough, and we're puzzling through remembering all the different options and syntax for each. We then turn to writing a script from scratch all on its own. This, of course, is a fine (and perhaps common) way to live. But I think we can do better.

Before it's claim to fame driving argubably the most influential web app framework of the past 20 years, Ruby was (and still is) a great scripting language with consistent syntax and a great standard library.

We begin to open up its composable power by using its executable with literal inlined syntax via the `-e` option.

<pre><code class="language-bash">
❯ echo "foo" | ruby -e 'puts "hello #{STDIN.gets}"'
hello foo
</code></pre>

We soon learn the `-n` option allows executing Ruby code for each line.

<pre><code class="language-bash">
❯ echo "foo\nbar" | ruby -n -e 'puts "hello #{$_}"'
hello foo
hello bar
</code></pre>

But we can really improve the DX here by thinking more broadly and noting the following:

* We are usually interested in thinking about `STDIN` as an array to map -- we don't just want to be inside a block (like with `-n`), but control the entirety of the array
* We are almost universally calling `puts` or `print`
* The env var `RUBYLIB` provides a global directory under which arbitary code is loadable

By embracing these constraints, I've come up with the following patterns:

### Pattern: Define a custom `RUBYLIB`, add your own tools

I set `RUBYLIB` to `~/_lib/ruby`, where I've added some custom code, namely a file `main.rb` that loads things from stdlib and `require`s anything else in `~/_lib/ruby`.

<pre><code class="language-ruby">
# main.rb
require 'json'
Dir.glob(File.dirname(__FILE__) + '/*.rb').each { it == __FILE__ || (require it) }
</code></pre>

### Pattern: Simplify access to `STDIN`

Because I am usually interested in `STDIN`, a sibling file `misc.rb` defines two constants, `II` array of strings from `STDIN` and `I` as the first item of the array (for single-line inputs).

<pre><code class="language-ruby">
# misc.rb
II = STDIN.readlines.map(&:chomp)
I = II.first
</code></pre>

### Pattern: Always assume `puts` will occur

Embracing the Unix Philosophy our tool is aimed at meaningful output. In my `.zshrc` I alias the inclusion of `main.rb` as a loaded library for `ruby -e` under the alias `rep`, meaning "ruby exec & puts"

<pre><code class="language-bash">
# .zshrc
alias re="ruby -r main.rb -e"
function rep {
  re "puts $1"
}
</code></pre>

## In practice

* I can pipe commands to `rep`, then use Ruby as my language instead of various utilites
* I can use `I` and `II` in my Ruby syntax to get `STDIN` as I want
* I can even add other utilities alongside `main.rb` to do other nice tricks (e.g. I monkeypatch `Array` to define `#jn` that runs `self.join("\n")`)

Let's have a look at a few before/after comparative examples.

### Example: Reverse lines from input

Using awk

<pre><code class="language-bash">
❯ echo "foo\nbar\nbaz" | awk '{lines[NR]=$0} END {for(i=NR;i>=1;i--) print lines[i]}'
baz
bar
foo
</code></pre>

Using rep: A very simple idea now has a very simple implementation.

<pre><code class="language-bash">
❯ echo "foo\nbar\nbaz" | rep 'II.reverse.jn'
baz
bar
foo
</code></pre>

### Example: Capitalize all words

Using tr

<pre><code class="language-bash">
❯ echo "foo\nbar\nbaz" | tr a-z A-Z
FOO
BAR
BAZ
</code></pre>

Using rep: A more literal and descriptive command

<pre><code class="language-bash">
❯ echo "foo\nbar\nbaz" | rep 'II.map(&:upcase)'
FOO
BAR
BAZ
</code></pre>

### Example: A custom JSON form

So let's revisit that requirement.

**"I have a space delimited text file where I want to know first word on each line and the word's total count in the file and it should be JSON formatted hash map, keyed by count mapped to arrays of words having that count, the arrays are sorted in order from greatest word length to least.**

(I won't even try to make a "before" version because I don't think it is easily done. Maybe you are better at this than me. 😎)

<pre><code class="language-bash">
❯ cat FOO.txt | rep 'II.reduce(Hash.new { 0 }) { |m, v| m[v.split(" ")[1]] += 1; m}.group_by { it[1] }.transform_values { it.map(&:first).sort_by { -it.length } }.to_json'
{"2":["foo","bar"],"1":["charlie","alpha"]}
</code></pre>

Again, what I love most about this solution is that we don't have to step *outside* of Ruby -- we don't have to juggle `sed` and `awk` and recall the nuances between various linux & macos flavors. We just stay within Ruby, and think in Ruby.

Also, I stay within the confines of the Unix Philosphy -- text based streams with small tools. But this takes us beyond where we could go before.

I hope it inspires your own vehicles of command line exploration.
