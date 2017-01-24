# Metrics

[公式ガイド](http://rubocop.readthedocs.io/en/latest/cops_metrics/)

## Metrics/AbcSize

AbcサイズをチェックするCop。
Abcサイズについては、[こちら](http://hikitest.hatenablog.com/entry/2015/01/14/034346)のブログが参考になると思います。

- default
  - enabled: `false`
- attributes
  - Max
    - AbcSizeの上限値
    - default: `15`

```ruby
# NG (Max: 5の場合)
def index
  a = 1 # 1
  a = 1 # 2
  a = 1 # 3
  a = 1 # 4
  a = 1 # 5
  a = 1 # 6
end
```

```
C: Assignment Branch Condition size for index is too high. [6/5]
def index
^^^
```

## Metrics/BlockLength

ブロック内の行数をチェックするCop。

- default
  - enabled: `false`
- attributes
  - CountComments
    - コメント行を含めるか 
    - default: `false`
  - Max
    - 上限サイズ
    - default: `25`
  - ExcludedMethods
    - 対象外のメソッド
    - default: `[]`

```ruby
# NG (Max: 5の場合)
def index
  1.times do
    _a = 1 # 1
    _a = 1 # 2
    _a = 1 # 3
    _a = 1 # 4
    _a = 1 # 5
    _a = 1 # 6
  end
end
```

```
C: Block has too many lines. [6/5]
  1.times do ...
  ^^^^^^^^^^
```

## Metrics/BlockNesting

ネスト数をチェックするCop。

- default
  - enabled: `false`
- attributes
  - CountBlocks
    - ブロックを含めるか 
    - default: `false`
  - Max
    - 上限サイズ
    - default: `3`

```ruby
# NG (Max: 3の場合)
def index
  if true
    if true
      if true
        if true
          a = 1
        end
      end
    end
  end
end

# OK (CountBlocks: falseの場合)
def index
  1.times do
    1.times do
      1.times do
        1.times do
          a = 1
        end
      end
    end
  end
end
```

```
C: Avoid more than 3 levels of block nesting.
        if true ...
        ^^^^^^^
```

## Metrics/ClassLength

クラスの行数をチェックするCop。

- default
  - enabled: `false`
- attributes
  - CountComments
    - コメント行を含めるか 
    - default: `false`
  - Max
    - 上限サイズ
    - default: `100`

```ruby
# NG (Max: 3の場合)
class ApplicationController < ActionController::Base
  protect_from_forgery with: :exception

  def index
  end
end
```

```
C: Class has too many lines. [4/3]
class ApplicationController < ActionController::Base ...
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```

## Metrics/CyclomaticComplexity

循環的複雑度をチェックするCop。
循環的複雑度については、[こちら](http://qiita.com/tbpgr/items/693f37d763c6ea5f9e9b)の記事が参考になると思います。

- default
  - enabled: `false`
- attributes
  - Max
    - 上限サイズ
    - default: `6`

```ruby
# NG (Max: 6の場合)
def hoge(msg)
  return 1 if msg.empty?   # 1
  return 2 unless msg.nil? # 2
  case msg
  when 'hoge' then 'hoge'  # 3
  when 'hige' then 'hige'  # 4
  when 'hege' then 'hege'  # 5
  else 'other'             # 6
  end
rescue                     # 7
  raise StandardError, 'error'
end
```

```
C: Cyclomatic complexity for hoge is too high. [7/6]
  def hoge(msg)
  ^^^
```

## Metrics/LineLength

1行の文字数をチェックするCop。

- default
  - enabled: `false`
- attributes
  - Max
    - 上限サイズ
    - default: `80`
  - AllowHeredoc
    - ヒアドキュメントを除外するか
    - default: `true`
  - AllowURI
    - URIを除外するか
    - default: `true`
  - URISchemes
    - AllowURIで除外する場合のScheme
    - default: `[http, https]`
  - IgnoreCopDirectives
    - インラインのRubocop設定を除外するか
    - default: `false`
  - IgnoredPatterns
    - 除外パターン
    - default: `（なし）`

```ruby
# NG (Max: 10の場合)
def index
  a=1234567
end

# OK (Max: 20、IgnoreCopDirectives: trueの場合)
def index
  # rubocop:disable Metrics/SomeReallyLongMetricNameThatShouldBeMuchShorterAndNeedsANameChange
  a=1234567
end
```

```
C: Line is too long. [11/10]
  a=1234567
```

## Metrics/MethodLength

メソッドの行数をチェックするCop。

- default
  - enabled: `false`
- attributes
  - Max
    - 上限サイズ
    - default: `10`
  - CountComments
    - コメント行を含めるか
    - default: `false`

```ruby
# NG (Max: 5の場合)
def index
  a = 1 # 1
  a = 1 # 2
  a = 1 # 3
  a = 1 # 4
  a = 1 # 5
  a = 1 # 6
end
```

```
C: Method has too many lines. [6/5]
def index ...
^^^^^^^^^
```

## Metrics/ModuleLength

モジュールの行数をチェックするCop。

- default
  - enabled: `false`
- attributes
  - Max
    - 上限サイズ
    - default: `100`
  - CountComments
    - コメント行を含めるか
    - default: `false`

```ruby
# NG (Max: 5の場合)
module Hoge
  def fuga # 1
    a = 1  # 2
    a = 1  # 3
    a = 1  # 4
    a = 1  # 5
  end      # 6
end
```

```
C: Module has too many lines. [6/5]
module Hoge ...
^^^^^^^^^^^
```

## Metrics/ParameterLists

メソッドの引数の数をチェックするCop。

- default
  - enabled: `false`
- attributes
  - Max
    - 上限サイズ
    - default: `5`
  - CountKeywordArgs
    - キーワード引数を含めるか
    - default: `true`

```ruby
# NG (Max: 5の場合)
def index(a, b, c, d, e, f)
end
```

```
C: Avoid parameter lists longer than 5 parameters.
def index(a, b, c, d, e, f)
         ^^^^^^^^^^^^^^^^^^
```

## Metrics/PerceivedComplexity

メソッドの複数さを計算しチェックするCop。
**※よく分からないです**

```ruby
def index                   # 1
  if cond                       # 1
    case var                    # 2 (0.8 + 4 * 0.2, rounded)
    when 1 then func_one
    when 2 then func_two
    when 3 then func_three
    when 4..10 then func_other
    end
  else                          # 1
    do_something until a && b   # 2
  end                           # ===
end                             # 7 complexity points
```

```
C: Cyclomatic complexity for index is too high. [8/6]
def index
^^^
```