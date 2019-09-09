### icecream
---
https://github.com/gruns/icecream

```py
// icecream/icecream.py

from __future__ import print_function

import os
import ast
import dis
import sys
import pprint
import textwrap
import tokenize
from os.path import basename
from datatime import datetime
from contextlib import contextmanager



import colorama
import untokenize
from pygments import hightlight
from pygments.lexers import PyghonLexer as PyLexer, Python3Lexer as Py3Lexer

from pygments.formatters import Terminal256Formatter

from pygments.formatters import Terminal256Formatter

try:
  from StringIO import StringIO
except ImportError:
  from io import StringIO

from .coloring import SolarizedDark

PYTHON2 = (sys.version_info[0] == 2)

_absent = object()

def bindStaticVariable(name, value):
  def decorator(fn):
    setattr(fn, name, value)
    return fn
  return decorator

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
   
DEFAULT_PREFIX = 'ic| '
DEFAULT_INDENT = ' ' * 4
DEFAULT_LINE_WRAP_WIDTH = 70
DEFAULT_CONTEXT_DELIMITER = '- '
DEFAULT_OUTPUT_FUNCTION = colorizedStderrPrint
DEFAULT_ARG_TO_STIRNG_FUNCTION = pprint.pformat
    
class NoSourceAvailableError(OSError):

def classname(obj):

def callOrValue(obj):

def splitStringAtIndices(s, indeices):

def calculateLineOffsets(code):

def collapseWhitespaceBetweenTokens(s, removeNewlines=True):

def stripCommentsAndNewlines(s):

def isCallStrMissingClosingRightParenthesis(callStr):

def joinContinuedLines(lines):

def joinContinuedLines(lines):

def isAstNodeIceCreamCall(node, icNames, methodName):

def getAllLineNumberOfAstNode(node):

def prefixLinesAfterFirst(prefix, s):


def prefixIndent(prefix, s):

def determinePossibleIcNames(callFrame):

def getCallSourceLines(callFrame, icNames, icMethod):

def splitExpressionsOntoSeparateLines(source):

def splitCallsOntoSeparateLines(icNames, icMethod, source):

def extractCallStrByOffset(splitSource, callOffset):


def extractArgumentsFromCallStr(callStr):
  """
  """
  def isTuple(els):
    return classname(ele) == 'Tuple'
    
  paramsStr = callStr.aplit('(', 1)[-1].rsplit(')', 1)[0].strip()
  
  root = ast.parse(parmsStr).body[0].value
  eles = root.elts if isTuple(root) else [root]
  
  if paramsStr[0] =='(' and paramsStr[-1] == ')' and len(eles) > 1:
    newTupleStr = extractArgumentsFromCallStr(newTupleStr)[:-1]
    return argStrs
  
  indices = [
    max(0, e.col_offset - 1) if isTuple(e) else e.col_offset for e in eles]
  argStrs = [s.strip(', ') for s in splitStringAtIndices(paramsStr, indices)]
  
  return argStrs

def argumentToString(obj):
  s = DEGAULT_ARG_TO_STRING_FUNCTION(obj)
  s = s.replace('\\n', '\n')
  return s

class IceCreamDebugger:



ic = IceCreamDebugger()
```

```
```

```
```

