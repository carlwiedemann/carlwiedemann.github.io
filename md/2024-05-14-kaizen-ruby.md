# Kaizen: Ruby

2024-05-14

Over the past 3 years, I've been using Ruby full time at my day job. At first I wasn't fond of it. But I've come to appreciate it more.

For _certain_ use cases (i.e. scripting), I find it superb. For other use cases (i.e. [large-scale](https://railsatscale.com/) applications), I find it less than superb.

After learning some of the basics, the language continues to surprise me in things I didn't know. I'm documenting some of those here, both as a reminder for myself but also for others who are learning the language.

*This post will be continuously updated.*

## Update 2024-05-14

### Single character strings

Denote a single character string by prefixing a character with `?`:

<pre><code class="language-ruby">?a == 'a'</code></pre>

Should you use this? Hard to say. I suppose it saves some keystrokes, but can be a little tricky, especially if you see something like `foo.split(?|)`

## Splat `*`

Can be used in dereferencing to match multiple values to an array:

<pre><code class="language-ruby">a, *b = [1, 2, 3, 4]
a == 1
b == [2, 3, 4]

*c, d = [5, 6, 7, 8]
c == [5, 6, 7]
d == 8
</code></pre>

### `Array#push`

Can accept splatted ranges:

<pre><code class="language-ruby">a = [1, 2, 3, 4, 5]
a.push(*6..10)

a == [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
</code></pre>

### `Array#[]`

Can accept a range ending with `-1` to match to the end:

<pre><code class="language-ruby">a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

a[5..-1] == [6, 7, 8, 9, 10]</code></pre>

Interestingly enough, using a non-inclusive range will exclude the last element (probably not useful):

<pre><code class="language-ruby">a[5...-1] == [6, 7, 8, 9]</code></pre>


### `String#[]`

Can accept a regex which will return the first match as a string, `nil` if no match:

<pre><code class="language-ruby">"asdf12345"[/\d/] == "1"
"asdf12345"[/foo/] == nil
</code></pre>

### Calling lambdas

Lambdas can be called either by using `#call`, or by just using `#()`:

<pre><code class="language-ruby"># With arguments...
f = ->(name) { "Hi #{name}" }

f.call("John") == "Hi John"
f.("John") == "Hi John"

# Without arguments...
g = -> { "Goodbye" }

g.call == "Goodbye"
g.() == "Goodbye"
</code></pre>

What's better? I think I prefer the explicitness of `call`, otherwise the `.` on its own can be hard to see.

### The `DATA` global variable

If the line `__END__` appears in a Ruby file, all subsequent lines will be available earlier in the script as `DATA`:

<pre><code class="language-ruby"># data.rb
DATA.each_with_index do |line|
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

### The `BEGIN` and `END` global variables

Can be passed blocks that will happen before and after all other steps in the script. The block syntax must use `{...}` instead of `do...end`.

<pre><code class="language-ruby"># begin_end.rb
puts "Today is #{Time.now.strftime('%F')}"

BEGIN { puts "hi"}
END { puts "bai"}
</code></pre>

Running:

<pre><code class="language-bash">❯ ruby begin_end.rb
hi
Today is 2024-05-14
bai
</code></pre> 

Instantiated variables are global scope, but the `BEGIN` block must appear before they are used. Here is the same script using a variable to store the date string:

<pre><code class="language-ruby"># begin_end.rb
BEGIN {
  now_str = Time.now.strftime('%F')
  puts "hi"
}

END { puts "bai" }

# In order to use the variable, the `BEGIN` block must exist on an earlier line.
puts "Today is #{now_str}"
</code></pre>

The variables are only available in the scope of the existing file, they are not available in other files that would be `require`'d etc.

Is this useful? Maybe for debugging or doing some sort of setup & teardown.

## Always be <strike>closing</strike> learning

Hopefully you have learned some things as well. Check back on this post for future updates.
