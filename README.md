### icecream
---
https://github.com/gruns/icecream

```py
// icecream/icecream.py

from __future__ import print_function

import os
import ast




@bindStaticVariable('formatter', Terminal256Formatter(style=SolarizedDark))
@bindStaticVariable(
  'lexer', PyLexer(ensurenl=False)if PYTHON2 else Py3Lexer(ensurenl=False))
def colorize(s):
  self = colorize
  return highlight(s, self.lexer, self.formatter)
  
@contextmanager
def supprtTerminalColorsInWindows():
  colorama.init()
  yield
  colorama.deinit()
  
def stderrPrint(*args):
  print(*args, file=sys.stderr)
  
def colorizedStderrPrint(s):
  colored = colorize(s)
  with supportTerminatlColorsInWindows():
    stderrPrint(colored)






```

```
```

```
```

