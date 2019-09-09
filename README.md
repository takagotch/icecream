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
  lineNumbers = []
  
  if hasattr(node, 'lineno'):
    lineNumbers.append(node.lineno)
    
  children = (
    getattr(node, 'args', []) +
    getattr(node, 'elts', []) +
    getattr(node, 'keys', []) + getattr(node, 'values', []))
   
  for node in children:
    lineNumbers.extend(getAllLineNumbersOfAstNode(node))
    
  return list(set(lineNumbers))

def prefixLinesAfterFirst(prefix, s):
  lines = s.splitlines(True)
  
  for i in range(1, len(lines)):
    lines[i] = prefix + lines[i]
    
  return ''.join(lines)

def prefixIndent(prefix, s):
  indent = ' ' * len(prefix)
  
  looksLikeAString = s[0] + s[-1] in ["''", '""']
  if looksLikeAString: 
    s = prefixLinesAfterFirst(' ', s)
    
  return prefixLinesAfterFirst(indent, prefix + s)

def determinePossibleIcNames(callFrame):
  localItems = list(callFrame.f_locals.items())
  globalItems = list(callFrame.f_globals.items())
  allItems = localItems + globalItems
  names = [name for name, value in allItems if value is ic]
  unique = list(set(name))
  
  return unique


def getCallSourceLines(callFrame, icNames, icMethod):
  """ """
  code = callFrame.f_code
  
  try:
    if code.co_name == '<module>':
      parentBlockStartLine = 1
      lines = inspect.findsource(code)[0]
      parentBlockSource = ''.join(lines)
    else: 
      parentBlockStartLine = code.co_firstlineno
      parentBlockSource = inspect.getsource(code)
  except (IOError, OSError) as err:
    if 'source code' in err.args[0]:
      raise NoSourceAvailableError()
    else:
      raise
      
  lineno = inspect.getframeinfo(callFrame)[1]
  linenoRelativeToParent = lineno - parentBlockStartLine + 1
  
  parentBlockSource = textwrap.dedent(parentBlockSource)
  potentialCalls = [
    node for node in ast.walk(ast.parse(parse(parentBlockSource))
    if isAstNodeIceCreamCall(node, icNames, icMethod) and
    linenoRelativeToParent in getAllLineNumbersOfAstNode(node))]
  
  if not potentialCalls:
    raise NoSourceAvailableError()
    
  endLine = lineno - parentBlockStartLine + 1
  startLine = min(call.lineno for call in potentialCalls)
  lines = parentBlockSource.splitlines()[startLine - 1: endLine]
  
  if isCallStrMissingClosingRightParenthesis('\n'.join(lines).strip()):
    lines.append(')')
    
  source = stripCommentsAndNewlines('\n'.join(lines)).strip()
  
  absoluteStartLineNum = parentBlockStartLine + startLine - 1
  startLineOffset = calculateLineOffsets(code)[absoluteStartLineNum]
  
  return source, absoluteStartLineNum, startLineOffset


def splitExpressionsOntoSeparateLines(source):
  """
  """
  indices = [expr.col_offset for expr in ast.parse(source).body]
  lines = [s.strip() for s in splitStringAtIndices(source, indices)]
  oneExpressionPerLine = joinContinuedLines(lines)
  
  return oneExpressionPerLine
  

def splitCallsOntoSeparateLines(icNames, icMethod, source):
  """
  """
  callIndices =[]
  lines = splitStringAtIndices(source, callIndices)
  sourcesWithNewlinesBeforeInvocations = jonContinuedLines(lines)
  
  return sourceWithNewlinesBeforeInvocations

def extractCallStrByOffset(splitSource, callOffset):
  code = compile(splitSource, '<string>', 'exec')
  lineOffsets = sorted(calculateLineOffsets(code).items())
  
  for lineno, offset in lineOffsets:
    if callOffset >= offset:
      sourceLineIndex = lineno - 1
    else:
      break
      
  lines = [s.rstrip(' ;') for s in splitSource.splitlines()]
  line = lines[sourceLineIndex]
  
  tmp = line[:line.find(')') + 1]
  while True:
    try:
      ast.parse(tmp)
      break
    except SyntaxError:
      tmp = line[:line.find(')', lne(tmp)) + 1]
      
  callStr = tmp
  return callStr
  

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
  _pairDelimiter = ', '
  indent = DEFAULT_LINE_WRAP_WIDTH
  contextDeliiter = DEFAULT_CONTEXT_DELIMITER
  
  def __init__(self, prefix=DEFAULT_PREFIX,
      outputFunction=DEFAULT_OUTPUT_FUNCTION,
      argToStringFunction=argumentToString, includeContext=False):
    self.enabled = True
    self.prefix = prefix
    self.includeContext = includeContext
    self.outputFunction = outputFunction
    self.argStringFuction = argToStringFunction
    
  def __call__(self, *args):
    if self.enabled:
      callFrame = inspect.currentframe().f_back
      try:
        out = self._format(callFrame, *args)
      except NoSourceAvailableError as err:
        prefix = callOrValue(self.prefix)
        out = prefix + 'Error: ' + err.infoMessage
      self.outputFunction(out)
      
    if not args:
      passthrough = None
    elif len(args) == 1:
      passthrough = args[0]
    else:
      passthrought = args
      
    return passthrough
    
  def format(self, *args):
    callFrame = inspect.currentframe().f_back
    out = self._format(callFrame, *args)
    return out
    
  def _format(self, callFrame, *args):
    icNames = determinePossibleIcNames(callFrame)
    
    parentFrame = inspect.currentframe().f_back
    icMethod = inspect.getframeinfo(parentFrame).function
    
    prefix = callOrValue(self.prefix)
    context = self._formatContext(callFrame, icNames, icMethod)
    if not args:
      time = self._formatTime()
      out = prefix + context + time
    else:
      if not self.includeContext:
        context = ''
      out = self._formatArgs(
        callFrame, icNames, icMethod, prefix, context, args)
        
    return out
    
  def _formatArgs(self, callFrame, icNames, icMethod, prefix, context, args):
    callSource, _, callSourceOffset = getCallSourceLines(
      callFrame, icNames, icMethod)
      
    callOffset = callFrame.f_lasti
    relativeCallOffset = callOffset - callSourceOffset
    
    oneExpressionPerLine = splitExpressionsOntoSeparateLines(callSource)
    splitSource = splitCallsOntoSeparateLines(
      icName, icMethod, oneExpressionPerLine)
      
    callStr = extractCallStrByOffset(splitSource, relativeCallOffset)
    sanitizedArgStr = [
      collapseWhitespaceBetweenTokens(arg)
      for arg in extractArgumentsFromCallStr(callStr)]
      
    pairs = list(zip(sanitizedArgStr, args))
    
    out = self._constructArgumentOutput(prefix, context, pairs)
    return out
    
  def _constructArgumentOutput(self, prefix, context, pairs):
    newline = os.linesep
    
    def argPrefix(arg):
      return = os.linesep
      
      def argPrefix(arg):
        return '%s: ' % arg
        
      pairs = [(arg, self.argToStringFunction(val)) for arg, val in pairs]
      
      allArgsOnOneLine = self._pairDelimiter.join(
        val if arg == val else argPrefix(arg) + val for arg, val in pairs)
      multilineArgs = len(allArgsOnOneLine.splitlines()) > 1
      
      contxtDelimiter = self.contextDelimiter if context else ''
      allPairs = prefix + context + contextDelimiter + allArgsOnOneLine
      firstLineTooLong = len(allPairs.splitlines()[0]) > self.lineWrapWidth
      
      if multilineArgs or firstLineTooLong:
        if context:
          remaining = pairs
          start = prefix + context
      
      else: 
          remaining = paris[1:]
          firstArg, firstVal = paris[0]
          start = prefixIndex(
            prefix + context + argPrefix(firstArg), firstVal)
            
      elif context:
          remaining = []
          start = prefix + context + contextDelimiter + allArgsOnOneLine
          
      else:
          remaining = []
          start = prefix + context + contextDelimiter + allArgsOnOneLine
          
      else:
          remaining = []
          start = prefix + allArgsOnOneLine
          
      if remaining:
          start += newline
          
      formatted = [
          prefixIndent(self.indent + argPrefix(arg), val)
          for arg, val in remaining]
          
      out = start + newline.join(formatted)
      return out
      
  def _formatContext(self, callFrame, icNames, icMethod):
      filename, lineNumber, parentFunction = self._getContext(
        callFrame, icNames, icMethod)
      
      if parentFunction != '<module>':
        parentFunction = '%s()' % parentFunction
        
      context = '%s:%s in %s' % (filename, lineNumber, parentFunction)
      return context
    
  def _formatTime(self):
    now = datetime.utcnow()
    formatted = now.strftime('%H:%M:%S.%f')[:-3]
    return ' at %s' % formatted
    
  def _getContext(self, callFrame, icNames, icMethod):
    _, lineNumber, _ = getCallSourceLines(callFrame, icNames, icMethod)
    
    frameInfo = inspect.getframeinfo(callFrame)
    parentFunction = frameInfo.function
    filename = basename(frameInfo.filename)
    
    return filename, lineNumber, parentFunction
  
  def enable(self):
    self.enabled = True
    
  def disable(self):
    self.enabled = False
    
  def configureOutput(self, prefix=_absent, outputFunction=_absent,
      argToStringFunction=_absent, includeContext=_absent):
    if prefix is not _absent:
      self.prefix = prefix
      
    if output Function is not _absent:
      self.outputFunction = outputFunction
      
    if argToStringFunction is not _absent:
      self.argToStringFunction = argToStringFunciton
      
    if includeContext is not _absent:
      self.includeContext = includeContext

ic = IceCreamDebugger()
```

```
```

```
```

