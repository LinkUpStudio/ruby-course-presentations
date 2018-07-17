# HOMEWORK 2

---

###### 04_pig_latin

```ruby
def translate(str)
  vowels = "aeiou".split("")
  str.split(" ").reduce([]) do |words, word|
    pig_word = word.chars
    if !vowels.include?(word[0])
      n = pig_word.find_index {|x|vowels.include?(x)}
      n += 1 unless pig_word[n-1..n].join("") != "qu"
      pig_word.push(pig_word.shift(n))[1..-1]
    end
    words << pig_word.insert(-1, "ay").join("")
  end.join(" ")
end
```
@[2](it could be just array)
@[3](we have map for it, Carl)

+++

```ruby
def vowel_first(s)
  @vowels.include? s
end

def translate(str)
  @vowels = %w[a e i o y]
  str.split.map do |word|
    i = 0
    while !vowel_first word[i,1]
      i += 1
    end
    word[i..-1] + word[0,i] + 'ay'
  end.join(" ")
end
```
@[1-3](this method is not quite good...)
@[9-11]()

+++

This solution had a lot of custom methods with readable names, which is good.

Though the implementation wasn't very good.

```ruby
def translate(string)
  if
    words = string.split(' ')
    words.each_index { |i| words[i] = translate_word(words[i]) }.join(' ')
  else
    translate_word(string)
  end
end

def translate_word(word)
  capitalize = is_capitalized?(word)
  word.downcase!
  if begins_with_vowel?(word)
    word << 'ay'
  elsif begins_with_three_consonants?(word)
    word = word[3..-1] << word[0..2] << 'ay'
  elsif begins_with_two_consonants?(word)
    word = word[2..-1] << word[0..1] << 'ay'
  elsif begins_with_consonant?(word)
    word = word[1..-1] << word[0] << 'ay'
  end

  word[0] = word[0].upcase! if capitalize
  word
end
# ...
```
@[2-7]()
@[13-21]()

+++

```ruby
def translate(str)
  words = str.split(" ")
  words.map! do |word|
    second_methods(word)
  end
  words.join(" ")
end

def softLit?(let)
  "aeiouy".include? let
end

def second_methods(second)
  size = second.length
  if softLit?(second[0])
    second + "ay"
  elsif second[0..1]=="qu"
    second[2..(second.length - 1)] + second[0..1] + "ay"
  elsif !softLit?(second[0]) && second[1..2]=="qu"
    second[3..(second.length - 1)] + second[0..2] + "ay"
  elsif !softLit?(second[0]) && !softLit?(second[1]) && !softLit?(second[2])
    second[3..(second.length - 1)] + second[0..2] + "ay"
  elsif !softLit?(second[0]) && !softLit?(second[1])
    second[2..(second.length - 1)] + second[0] + second[1] + "ay"
  elsif !softLit?(second[0])
    second[1..(second.length - 1)] + second[0] + "ay"
  end
end
```
@[9-11]()
@[13]()
@[14](it was never used)
@[18]()

+++

```ruby
def translate(string)
  arr = string.split
  arr.map! do |x|
  if x =~ /\A[aeiou]/i
  x.concat('ay')
  elsif x =~ /\A(?i:(?![a|e|i|o|u])[a-z]){3}/ || x.start_with?('sch') || x =~ /\A(?i:(?![a|e|i|o|u])[a-z]){1}qu/
  x[3..x.size].concat(x[0..2], 'ay')
  elsif x =~ /\A(?i:(?![a|e|i|o|u])[a-z]){2}/ || x.start_with?('qu')
  x[2..x.size].concat(x[0..1], 'ay')
  elsif x =~ /\A(?i:(?![a|e|i|o|u])[a-z]){1}/
  x[1..x.size].concat(x[0], 'ay')
  end
  end
  s_res = ''
  arr.each { |x| s_res.concat(x, ' ') }
  s_res.rstrip!
end
```
@[3-13]()
@[14-16](we got `join` for it)

+++

```ruby
def translate(words)
  result = ''
  array = words.split(/\s/)
  array.map {|word|
    position = (word  =~ /[a,e,y,i,o,u]/)
    if word[0..position][/..$/] == 'qu'
      position += 1
    end
    if position == 0
      if word[0...2] == 'qu'
        position
      end
      result += word + 'ay' + ' '
    elsif position == 1
      result += word[1...word.length] + word[0...1] + 'ay' + ' '
    elsif position == 2
      result += word[2...word.length] + word[0...2] + 'ay' + ' '
    else position == 3
      result += word[3...word.length] + word[0...3] + 'ay' + ' '
    end
  }
  result.chop
end
```
@[4](using `map` like it is `each`)
@[5](commas in regexp)
@[10-12](it does nothing)
@[14-19](not very DRY)

+++

```ruby
def translate(text) # спаси и сохрани, аминь (Индус стайл)
  text.split(' ').map do |word|
    unless word.empty?
      until (word[0] =~ /[aeioy]/)
        p word
        char = word[0]
        word[0] = ''
        word << char
      end
    word << "ay"
    end
    end.join(' ')
end
```
@[1](useful comments, must have)
@[3](you could use `next if word.empty?` here, though it's not needed at all)
@[4-9]()

+++

Very neat solution @fa[thumbs-up]

Though, it will fail for word _"unity"_, _"number"_,
and some other words with vowel _"u"_ letter inside.

:(

```ruby
def translate(sentence)
  sentence.gsub(/\w+/) do |word|
    word.sub!(
      /
      ^([^aeio]*) # select all consonants at the beginning
      (.*)$       # select rest characters
      /xi, '\2\1ay'
    )
    word.capitalize! unless word.scan(/[A-Z]/).empty?
    word
  end
end
```
@[2]()
@[3-8]()
@[9-10]()

+++

Pretty good logic

```ruby
def translate(word)
  translator = word.split.map do |hub, i|
    letter = hub.index(/[aeiou]/)
    translator = hub + 'ay' if letter.zero?
    letter += 1 if word[letter] == 'u' && word[letter - 1] == 'q'
    cons = hub.slice!(0, letter)
    translator = hub + cons + 'ay'
  end
  translator.join(' ')
end
```
@[2](what's i?)
@[4](use `next` here, or remove this line at all, as it does nothing)
