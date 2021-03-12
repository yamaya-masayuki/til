# Swiftでctags

とりあえず、ctagsをカスタムする方法。

`~/.ctags.d/swift.ctags`

```text
--langdef=Swift 
--langmap=Swift:+.swift 
--regex-swift=/(var|let)[ \t]+([^:=]+).*$/\2/,variable/ 
--regex-swift=/func[ \t]+([^\(\)]+)\([^\(\)]*\)/\1/,function/ 
--regex-swift=/class[ \t]+([^:\{]+).*$/\1/,class/ 
--regex-swift=/protocol[ \t]+([^:\{]+).*$/\1/,protocol/
```
