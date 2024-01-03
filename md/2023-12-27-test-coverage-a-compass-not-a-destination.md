# Test Coverage: A Compass, not a Destination

2023-12-27

When assessing software quality, well-tested software is presumed to be better than poorly-tested (or untested) software. Poorly-tested (or untested) software has little to no enforcement of its boundaries. Poorly-tested (or untested) software cannot be known to be correct when changes are made to that software.

What determines if software is well-tested? For individual files, a common quantitative approach is to measure test "coverage," defined as a file's ratio of tested lines to total lines. What defines a "tested line" is whether the line is executed during a test execution. Files with greater coverage numbers are presumed to be higher quality, since greater coverage number implies the code in the file is well-tested.

Many tools and proprietors exist that perform coverage calculations along with visualizations & dashboards to make the metric visible, and track it over time. Subsequently, these numbers can be used for goals & targets of software codebases, as a metric of the health of a software codebase.

But does the coverage number tell the whole story about the software and its tests? What does coverage miss? And are there alternative ways of achieving well-tested software?

## An example

Consider the following simple Ruby file and it's corresponding test. (A constant in file is removed for the purposes of discussion).

File: `thermometer.rb`

<pre><code class="language-ruby">
module Thermometer
  # WATER_BOILING_POINT would be defined here...

  def self.is_water_boiling?(temperature)
    temperature >= WATER_BOILING_POINT
  end
end
</code></pre>

Test: `thermometer_spec.rb`


<pre><code class="language-ruby">
require './test_helper'

require "./thermometer"

describe Thermometer do
  describe '#is_water_boiling?' do
    it "returns true when temperature is above boiling point" do
      assert(Thermometer.is_water_boiling?(500.0))
    end
  end
end
</code></pre>

Suppose that `Thermometer::WATER_BOILING_POINT` has a legitimate value, and we run this test using the Ruby [SimpleCov](https://github.com/simplecov-ruby/simplecov) library (a popular tool).

The `thermometer_spec.rb` test will pass and also have 100% test coverage.

Console output from test running:

<pre><code>
# Running:

.

Finished in 0.000394s, 2538.0709 runs/s, 2538.0709 assertions/s.

1 runs, 1 assertions, 0 failures, 0 errors, 0 skips
Coverage report generated for RSpec to /Users/carl.wiedemann/_scratch/simplecov_demo/coverage. 4 / 4 LOC (100.0%) covered.
</code></pre>

Despite the simplicity of this example, I now ask the reader the following: **Is this well-tested software?**

Let's describe what the test tells us:

* ✅ The test tells us that passing a float value of 500.0 to the `#is_water_boiling?` method returns `true`.

Let's also describe what the test does **_not_** tell us:

* ❌ The test does not tell us under what conditions the `#is_water_boiling?` returns something other than `true`.
* ❌ The test does not tell us what behavior we should expect if `#is_water_boiling?` is passed an argument of a different type.
* ❌ Most importantly, the test does not actually describe the context of the usage of the file. What kind of thermometer is this? A Fahrenheit thermometer? Or a Celsius thermometer? At what temperature does water boil?

We see that there's quite a bit missing from this test.

Normally, tests can tell us about how code is used, and all the possible outcomes. But this test does **not** do those things **despite having 100% coverage**.

## Improving the test beyond coverage

Let's modify the test such that it answers the questions mentioned above:

* ❓The test should tell us about when water doesn't boil, based on the (yet-to-be-revealed) `Thermometer::WATER_BOILING_POINT` value.
* ❓The test should describe what happens when we have logical exceptions, e.g. different types
* ❓The test should describe what _kind of thermometer we have actually built_.

Making these changes, we see the following:

<pre><code class="language-ruby">
require './test_helper'

require "./thermometer"

describe Thermometer do
  describe '#is_water_boiling?' do
    it "returns true when temperature is at boiling point" do
      assert(Thermometer.is_water_boiling?(373.1339))
    end

    it "returns true when temperature is above boiling point" do
      # Float::MAX exceeds Planck temperature, ~10**32
      assert(Thermometer.is_water_boiling?(rand(373.1339..Float::MAX)))
    end

    it "returns false when temperature is below boiling point" do
      # @todo Consider enforcing absolute min with a `raise`?
      refute(Thermometer.is_water_boiling?(rand(-273.15...373.1339)))
    end

    it "handles integer arguments" do
      assert(Thermometer.is_water_boiling?(400))
      refute(Thermometer.is_water_boiling?(200))
    end

    it "raises when passed a different type" do
      expect { Thermometer.is_water_boiling?("2") }.must_raise(ArgumentError)
      expect { Thermometer.is_water_boiling?(Class) }.must_raise(TypeError)
    end

  end
end
</code></pre>

...and we now see what we had a [Kelvin](https://en.wikipedia.org/wiki/Kelvin) thermometer all along...


<pre><code class="language-ruby">
module Thermometer
  WATER_BOILING_POINT = 373.1339

  # ...
end
</code></pre>

...while also noting that coverage has stayed at 100%.

<pre><code>
# Running:

.....

Finished in 0.000443s, 11286.6818 runs/s, 15801.3545 assertions/s.

5 runs, 7 assertions, 0 failures, 0 errors, 0 skips
Coverage report generated for RSpec to /Users/carl.wiedemann/_scratch/simplecov_demo/coverage. 4 / 4 LOC (100.0%) covered.
</code></pre>

There are maybe some quibbles to be made about this second version. But one would be hard-pressed to affirm that the first version was superior. And yet coverage deems these tests as identical &mdash; moreover, with 100% as model citizens &mdash; whilst a competent human programmer would deem them as anything but.

## Summary: Coverage != Quality

We began this example with a very nice coverage number, at 100%. But we had a pretty weak test. With our first test, we could have changed `Thermometer::WATER_BOILING_POINT` to be [100](https://en.wikipedia.org/wiki/Fahrenheit) or [212](https://en.wikipedia.org/wiki/Celsius) (or even [80](https://en.wikipedia.org/wiki/R%C3%A9aumur_scale)) and the test would have still passed. To paraphrase Dijkstra: _Testing only shows the presence of bugs, never the absence of bugs._

One could imagine other lines of code that would also come through SimpleCov as being "100% tested" despite not actually being so. Things like ternaries and set inclusion come to mind.

## So is coverage useless?

Perhaps not entirely. What _lesser_ coverage can tell us is this: _Have we missed something?_ Is there a leftover method we should have removed? Did we forget to call something that we somehow missed?

Additionally, _lesser_ coverage can have some benefit for addition in an existing codebase with a dubious contribution history. _Where are places we should start to look to backfill?_ It can begin to point us in directions.

But thereafter it is insufficient to tell us anything else. We must use our critical design facilities to improve those missed files, or those dubious codebases. Coverage alone is insufficient to determine anything about the quality of tests and subsequently insufficient to determine anything about the quality of software undergoing tests. It is an aside to the canon.

When introducing coverage metrics to an existing codebase, we can only be _inductively_ certain about the file that has, say, 95% coverage vs a file that has 55% coverage. For it is quite possible that the former has many more mistakes and change risk than the latter.

Put bluntly, **greater coverage cannot tell us anything** aside from that "tested" lines are merely executed at least once during testing. For this is the unfortunate definition of "coverage" that we have come to accept.

Lastly, let us recognize that these benefits are of pure _internal_ utility &mdash; the benefits stated above are tooling for the engineers themselves who write the software within the codebase. To follow the convention we see in code repositories brandishing a "badge" declaring the code coverage number is a purely aesthetic and superficial act. A discerning eye knows better than to infer anything else.

## What instead?

It is my belief that following [test-driven development (TDD)](http://butunclebob.com/ArticleS.UncleBob.TheThreeRulesOfTdd) is a much more reliable way to ensure higher quality of tests above whether if lines are simply executed. In TDD we must observe our boundaries reveal themselves to be mended through our work as a design method, rather than an ex-post formality. Testing is a necessary means, not an optional end.

Let me also remark this: That aforementioned benefit of coverage &mdash; that it tells us what we missed &mdash; becomes entirely moot under TDD. By definition, untested functionality _cannot physically be written_.

Will we still make mistakes with TDD? Yes. But the prerequisite nature of test boundaries ensures fewer of them should result from inadequate tests.

## Code is for humans

The Agile Manifesto states "we have come to value Individuals and interactions over processes and tools". Code is ultimately written for our eyes, not the machine interpreter.

Human judgement cannot overly rely on heuristic. If we are to decide where to place our trust, we must not overly lean on pure quantification & metric. To over-quantify is to risk missing the bigger picture.

Let coverage be an accessory &mdash; a compass &mdash; but not the destination in which we should aspire to thrive.
