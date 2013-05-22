Ruby Styleguide
===============

* Use `UTF-8` as the source file encoding.
* Use two **spaces** per indentation level. No hard tabs.
* Keep lines fewer than **80** characters.
* Never leave trailing whitespace
* Use spaces around operators, after commas, colons and semicolons, around `{` and before `}`.
  
    ````Ruby
    sum = 1 + 2
    a, b = 1, 2
    ````

    The only exception, regarding operators, is the exponent operator:
    
    ````Ruby
    # bad
    e = M * c ** 2
    
    # good
    e = M * c**2
    ````

* No spaces after `(`, `[` or before `]`, `)`.

    ````Ruby
    some(arg).other
    [1, 2, 3].length
    ````

* Indent `when` as deep as `case`.

    ````Ruby
    case when song.name == "Misty"
      puts "Not again!"
    when song.duration > 120
      puts "Too Long!"
    else
      song.play
    end
    
    kind = case year
           when 1850..1889 then "Blues"
           when 1890..1909 then "Ragtime"
           else "Jazz"
           end
    ````
