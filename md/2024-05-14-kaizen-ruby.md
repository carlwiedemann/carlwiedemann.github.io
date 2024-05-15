# Kaizen: Ruby

2024-05-14

Over the past 3 years, I've been using Ruby full time at my day job. At first I wasn't fond of it. But I've come to appreciate it more.

For _certain_ use cases (i.e. scripting), I find it superb. For other use cases (i.e. [large-scale](https://railsatscale.com/) applications), I find it less than superb.

After learning some of the basics, the language continues to surprise me in things I didn't know. I'm documenting some of those here, both as a reminder for myself but also for others who are learning the language.

*This post will be continuously updated.*

### `#itself` ([docs](https://devdocs.io/ruby~3.3/object#method-i-itself))

Calling on any object simply returns...itself 🥁.

<pre><code class="language-ruby">a = 123
a == a.itself
</code></pre>

Why on earth is this useful? Sometimes there are instances where blocks should simply return the sole argument passed to the block:

<pre><code class="language-ruby">a = [1, 1, 1, 2, 2, 2, 2, 3, 3, 3, 3, 3]

# Convert to a hash, keyed by unique array values:
g = a.group_by { |v| v }
g == { 1 => [1, 1, 1], 2 => [2, 2, 2, 2], 3 => [3, 3, 3, 3, 3] }

# Alternative, using numbered parameter shorthand:
g = a.group_by { _1 }
g == { 1 => [1, 1, 1], 2 => [2, 2, 2, 2], 3 => [3, 3, 3, 3, 3] }
</code></pre>

Using the `#to_proc` shorthand (`&`) with `#itself` is perhaps more explicit:

<pre><code class="language-ruby">g = a.group_by(&:itself)
g == { 1 => [1, 1, 1], 2 => [2, 2, 2, 2], 3 => [3, 3, 3, 3, 3] }
</code></pre>

(Sidenote: For this particular example, you'd might be more interested in [`Enumerable#tally`](https://devdocs.io/ruby~3.3/enumerable#method-i-tally))

### Single character strings ([docs](https://devdocs.io/ruby~3.3/syntax/literals_rdoc#label-String+Literals))

Denote a single character string by prefixing a character with `?`:

<pre><code class="language-ruby">?a == 'a'</code></pre>

Should you use this? Hard to say. I suppose it saves some keystrokes, but can be a little tricky, especially if you see something like `foo.split(?|)`

### Splat `*` destructuring assignment ([docs](https://devdocs.io/ruby~3.3/syntax/assignment_rdoc))

Can be used in assignment to match multiple values to an array:

<pre><code class="language-ruby">a, *b = [1, 2, 3, 4]
a == 1
b == [2, 3, 4]

*c, d = [5, 6, 7, 8]
c == [5, 6, 7]
d == 8
</code></pre>

### Splat `*` on `Range` ([stackoverflow](https://stackoverflow.com/a/55888963))

Splatting a range will convert to an array, it is basically a shorthand for `#to_a`:

<pre><code class="language-ruby">a = *1..10

a == [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
</code></pre>

Saves a few keystrokes from `(1..10).to_a`.

### Passing `Range` to `Array#[]` ([docs](https://devdocs.io/ruby~3.3/array#method-i-5B-5D))

Can accept a range ending with `-1` to match to the end:

<pre><code class="language-ruby">a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

a[5..-1] == [6, 7, 8, 9, 10]</code></pre>

Interestingly enough, using a non-inclusive range will exclude the last element (probably not useful):

<pre><code class="language-ruby">a[5...-1] == [6, 7, 8, 9]</code></pre>

See doc link above for more interesing examples using `Enumerator::ArithmeticSequence`.

### Passing `Regexp` to `String#[]` ([docs](https://devdocs.io/ruby~3.3/string#method-i-5B-5D))

Can accept a `Regexp` which will return the first match as a string, `nil` if no match:

<pre><code class="language-ruby">"asdf12345"[/\d/] == "1"
"asdf12345"[/foo/] == nil
</code></pre>

### Different ways to call lambdas/procs ([docs](https://devdocs.io/ruby~3.3/proc#method-i-call))

Lambdas/Procs can be called using `#call`, `#()`, `#[]`, `#yield`:

<pre><code class="language-ruby"># With arguments...
f = ->(name) { "Hi #{name}" }

f.call("John")  == "Hi John"
f.("John")      == "Hi John"
f["John"]       == "Hi John"
f.yield("John") == "Hi John"

# Without arguments...
g = -> { "Goodbye" }

g.call  == "Goodbye"
g.()    == "Goodbye"
g[]     == "Goodbye"
g.yield == "Goodbye"
</code></pre>

What's best? I think I prefer the explicitness of `call`, all the other syntax seem really unnatural to me (especially `#[]`). Also, `#yield` is something that I prefer to reserve for methods that receive blocks.

### The `DATA` global variable ([docs](https://devdocs.io/ruby~3.3/globals_rdoc#label-Embedded+Data))

If the line `__END__` appears in a Ruby file, all subsequent lines will be available earlier in the script as `DATA`:

<pre><code class="language-ruby"># data.rb
DATA.each do |line|
  puts "Line: #{line.chomp}"
end
__END__
xray
yankee
zulu
</code></pre>

Running:

<pre><code class="language-bash">❯ ruby data.rb
Line: xray
Line: yankee
Line: zulu
</code></pre>

What could this be used for? Perhaps coding exercises where that must parse some input and now you don't have to save it in a separate file to use `File.readlines`.

### The `BEGIN` and `END` global variables ([docs](https://devdocs.io/ruby~3.3/syntax/miscellaneous_rdoc#label-BEGIN+and+END))

Can be passed blocks that will happen before and after all other steps in the script. The block syntax must use `{...}` instead of `do...end`.

<pre><code class="language-ruby"># begin_end.rb
puts "Today is #{Time.now.strftime('%F')}"

BEGIN { puts "hai" }
END { puts "bai" }
</code></pre>

Running:

<pre><code class="language-bash">❯ ruby begin_end.rb
hai
Today is 2024-05-14
bai
</code></pre> 

Instantiated variables are global scope, but the `BEGIN` block must appear before they are used. Here is the same script using a variable to store the date string:

<pre><code class="language-ruby"># begin_end.rb
BEGIN {
  now_str = Time.now.strftime('%F')
  puts "hai"
}

END { puts "bai" }

# In order to use the variable, the `BEGIN` block must exist on an earlier line.
puts "Today is #{now_str}"
</code></pre>

The variables are only available in the scope of the existing file, they are not available in other files that would be `require`'d etc.

Is this useful? Maybe for debugging or doing some sort of setup & teardown. (I have learned these have been [influenced by Perl](https://perldoc.perl.org/perlmod#BEGIN%2C-UNITCHECK%2C-CHECK%2C-INIT-and-END))

## Always be <strike>closing</strike> learning

Hopefully you have learned some things as well. Check back on this post for future updates.
