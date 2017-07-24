2017-07-24
==========
+ Implemented a clone of bash's "operate-and-get-next" (Ctrl-o)
+ Automatically creates an empty history file when it doesn't exist yet
  (eases the installation)
+ Added some fancy colors to error messages

2015-06-30
==========
+ Fixed #3: Completion is broken inside a Django shell

2015-06-21
==========
+ Changed `history.recall()` to `history.reload()` to avoid confusion
+ Refactored the way we mingle with the `__builtins__` object (closes #1)
