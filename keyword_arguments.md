# Keyword arguments #

# Splatting #

```ruby
def one_arg(an_array)
  puts an_array.inspect
end

one_arg([1, 2, 3]) 
# [1, 2, 3]

one_arg(*[1, 2, 3]) 
# ArgumentError: wrong number of arguments (3 for 1)
```

# Splatting #

```ruby
def three_args(one_arg, two_arg, three_arg)
  puts [one_arg, two_arg, three_arg].inspect
end

three_args([1, 2, 3])
# ArgumentError: wrong number of arguments (1 for 3) 

one_arg(*[1, 2, 3]) 
# [1, 2, 3]
```

# Splatting #

```ruby
def many_args(*args)
  puts args.inspect
end

many_args([1, 2, 3]) 
# [[1, 2, 3]]

many_args(*[1, 2, 3]) 
# [1, 2, 3]
```

# Slurping #

```ruby
a = 1, 2, 3
a
# [1, 2, 3]

a, b = 1, 2, 3
a
# 1
b
# 2
# 3 is gone...

a, b, c = 1, 2
c
# nil
```

# Slurping #

```ruby
a, *b = 1, 2, 3
a
# ?
b
# ?
```

# Slurping #

```ruby
a, *b = 1, 2, 3
a
# 1
b
# [2, 3]

def one_or_more_args(arg, *the_rest)
  puts arg.inspect
  puts the_rest.inspect
end

one_or_more_args(1)
# 1
# []

one_or_more_args(1, 2, 3)
# 1
# [2, 3]

one_or_more_args(*[1, 2, 3])
# 1
# [2, 3]
```

# Destructuring (arrays) #

```ruby
a, b, c = [1, [2, 3], 4]
b 
# [2, 3]

a, (b, c), d = [1, [2, 3], 4]
a
# 1
b
# 2
c
# 3
d
# 4

a, (b, c), d, *rest = [1, [2, 3], 4, 5, 6, 7]
rest # [5, 6, 7]
```

# Destructuring (blocks) #

```ruby
[[1,2],[3,4],[5,6]].map { |a, b, c| ["#{a}, #{b}"] }
[["1, 2"], ["3, 4"], ["5, 6"]]

[[1, [2, 3], 4], [5, [6, 7], 8]].map { |a,(b, c), d| "#{a} (#{b}, #{c}), #{d}" }
# ["1 (2, 3), 4", "5 (6, 7), 8"]

[[1,2, 'a', 'b', 'c'],[3,4],[5,6]].map { |a, b, *rest| rest }
# [["a", "b", "c"], [], []]
```

# Destructuring (hashes) #

```ruby
{ :a => [1,2,3], b: [4, 5, 6] }
  .find { |key, (num1, num2, num3)| num1 + num2 + num3 == 6 }
# [:a, [1, 2, 3]]

{ ['a', 'b'] => [1,2,3], ['c', 'd'] => [4, 5, 6] }
  .find do |(alpha1, alpha2), (num1, num2, num3)| alpha1 + alpha2 == 'ab' }
# [["a", "b"], [1, 2, 3]]
```

# Destructuring #

* Ruby wants to destructure arrays and hashes
* But it needs hints
* splats provide hints about what to do with remainders

# Keyword arguments #

```ruby
def gimme_options(arg, options = {})
  puts arg.inspect
  puts options.inspect
end

gimme_options(1, { foo: :blah })
# 1
# { foo: :blah }

gimme_options(2, foo: :blah)
# same 

gimme_options(1, nil)
# 1
# nil
```

# Keyword arguments #

```ruby
def gimme_kwargs(arg, **options)
  puts arg.inspect
  puts options.inspect
end

gimme_kwargs(1, foo: :blah)
# 1
# { foo: :blah }

gimme_kwargs(foo: :blah)
# ArgumentError: wrong number of arguments (2 for 1)
```

# Keyword arguments #

```ruby
def gimme_args_and_kwargs(*args, **options)
  puts args.inspect
  puts options.inspect
end

gimme_args_and_kwargs(1, 2, 3, foo: :blah)
# [1, 2, 3]
# {:foo=>:blah}

gimme_args_and_kwargs({baz: :bar}, foo: :blah)
# [{:baz=>:bar}]
# {:foo=>:blah}
```

# Keyword arguments #

```ruby
def gimme_kwargs_with_defaults(foo: :bar, **options)
  puts options.inspect
  puts foo.inspect
end

gimme_kwargs_with_defaults(la: :la)
# {:la=>:la}
# :bar

options = {la: :la}
gimme_kwargs_with_defaults(**options)
# same

gimme_kwargs_with_defaults(options)
# same
```

# Keyword arguments (caveats) #

```ruby
gimme_kwargs_with_defaults(foo: :baz, options)
# SyntaxError: (irb):118: syntax error, unexpected ')', expecting =>

gimme_kwargs_with_defaults({ foo: :blah }, options)
# ArgumentError: wrong number of arguments (1 for 0)

gimme_kwargs_with_defaults(foo: :baz, **options)
# {:la=>:la}
# :baz

options = {'la' => :la }
gimme_kwargs_with_defaults(**options)
# TypeError: wrong argument type String (expected Symbol)
```
