# pystar
Python module for reading and writing STAR formatted text files.

Tries to be as fast as possible while being fully compliant.

STAR files are loaded and returned in standard built-in Python collections.  Conversions are as follows:

```
{ 'block1': {             # data_block1
    'key1': 'value1',     # _key1 value1
    'key2': 1,            # _key2 1
    'frame': {            # frame_frame
      'key3': 'value2',   # _key3 value2
      'key4': 3,          # _key4 3
      ('a', 'b', 'c'): [  # loop_
                          # _a
                          # _b
                          # _c
        [ 1, 2, 3],       # 1 2 3
        [ 3, 4, 5]],      # 3 4 5
    }
    ('d', 'e'): [         # loop_
                          # _d
                          # _e
      [ 1, 'name is' ],   # 1 'name is'
      [ 3, 'name' ]],     # 3 name
    'mlstring':           # _mlstring
      '''                 # ;
      this is a multiline # this is a multiline 
      string with         # string with
      whitespaces         # whitespaces
      '''
  }
  ...
}
```

STAR values are converted to `float` if possible, otherwise they remain `str`.
  note: multiline STAR strings, and single or double quoted strings are handled correctly preserving whitespace

A STAR document becomes a Python ordered dictionary where:
  1) keys are block names ( `data_name` -> `name` )
  2) values are a Python ordered dictionary for each block

A STAR block becomes a Python ordered dictionary that contains:
  1) key: [str, float]
  1) A string or float
  2) A STAR frame
  3) A STAR loop

A STAR frame becomes a Python dictionary that contains:
  1) key, value pairs
  2) STAR loops

A STAR loop becomes a numpy record array with the column names determined from the STAR loop fields
  and the values converted to a string or float as appropriate.
