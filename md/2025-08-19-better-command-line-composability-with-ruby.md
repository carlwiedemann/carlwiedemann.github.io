# Better command line composability with Ruby

2025-08-19

I like the command line. I don't spend all my time there, but I spend quite a bit of it there before turning to other tools.

What are the nature of command line tools? We come to understand the Unix Philosophy as unavoidable practice. And we can get a lot of mileage out of small portable utilities all piped together. It's quite a nice vehicle, an elegant flow to treat the next bespoke requirement as just another piped command.

But if you've done this enough you know what its like when that vehicle's mileage runs short.

## Just one more pipe

Usually it happens as we are piping things together quite handily.

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

**"...and it should be JSON formatted has a hash map where keys are the counts and values area a array of words with that count, the arrays are sorted in order from greatest word length to least."**

> *Ah, um...ok, well...now I have to write a custom script from scratch.*

## Out of gas

This is when "just one more pipe" falls short. We've all been there, devotedly fussing around with `awk`, `sed`, `grep`, `tr`, skirting the narrow boundary between clever and clandestine. But ultimately we arrive at a place where these tools just aren't enough, and we're puzzling through remembering all the different options and syntax for each. We then turn to writing a script from scratch all on its own. This, of course, is a fine (and perhaps common) way to live. But I think we can do better.

## Ruby off Rails

Before it's claim to fame driving argubably the most influential webapp framework of the past 20 years, Ruby was (and still is) a great scripting language has with consistent syntax and a great standard library.

As a scripting language, it can run standalone `ruby foo.rb` but also with literal syntax via the `-e` option.

<pre><code class="language-bash">
❯ echo "foo" | ruby -e 'puts "hello #{STDIN.gets}"'
</code></pre>

There are other options like `-n` which allows executing Ruby code for each line.

But we can really improve things by realizing the following:

* We are usually interested in thinking about `STDIN` as an array we'll be mapping
* We are almost universally calling `puts` or `print`
* The env var `RUBYLIB` provides a global directory under which arbitary code is loadable

By coordinating these ideas, I've come up with the following pattern that is quite nice.

I set `RUBYLIB` to `~/_lib/ruby`, where I've added some custom code, namely a file `main.rb` that loads things from stdlib and `require`s anything else in `~/_lib/ruby`

<pre><code class="language-ruby">
# main.rb
require 'json'
Dir.glob(File.dirname(__FILE__) + '/*.rb').each { it == __FILE__ || (require it) }
</code></pre>

Because I am usually interested in STDIN, I have a sibling file `misc.rb` that defines two constants, `I`, and `II` as a single line and array of strings from STDIN.

<pre><code class="language-ruby">
# misc.rb
II = STDIN.readlines.map(&:chomp)
I = II.first
</code></pre>

Finally, in `.zshrc` I alias the inclusion of `main.rb` as a loaded library for `ruby -e` under the alias `rep`, meaning "ruby exec & puts"

<pre><code class="language-bash">
# .zshrc
alias re="ruby -r main.rb -e"
function rep {
  re "puts $1"
}
</code></pre>

## Going farther

What does this all mean?

* I can pipe commands to `rep`, then use Ruby as my language instead of various utilites
* I can use `I` and `II` in my Ruby syntax to get STDIN as I want
* I can even add other utilities alongside `main.rb` to do other nice tricks (e.g. I monkeypatch `Array` to define `#jn` that runs `self.join("\n")`)

So let's revisit that requirement.

**"I have a space delimited text file where I want to know first word on each line and the word's total count in the file and it should be JSON formatted has a hash map where keys are the counts and values area a array of words with that count, the arrays are sorted in order from greatest word length to least."**

> Got it.

<pre><code class="language-bash">
❯ cat FOO.txt | rep 'II.reduce(Hash.new { 0 }) { |m, v| m[v.split(" ")[1]] += 1; m}.group_by { it[1] }.transform_values { it.map(&:first).sort_by { -it.length } }.to_json'
{"2":["foo","bar"],"1":["charlie","alpha"]}
</code></pre>

What I love most about this solution is that I don't have to step *outside* of thinking in Ruby -- I don't have to juggle `sed` and `awk` and recall the nuances between various linux & macos flavors. I just stay within Ruby, and think in Ruby.

Also, we stay within the confines of the Unix Philosphy -- text based streams with small tools. But this takes us beyond where we could go before.

I hope it inspires your own vehicles of command line exploration.
