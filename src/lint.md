# Lint

[公式ガイド](http://rubocop.readthedocs.io/en/latest/cops_lint/)

## Lint/AmbiguousOperator

"()"なしのメソッド呼び出しで、引数の曖昧な演算子をチェックするCop。

- default
  - enabled: `false`

```ruby
# NG
hoge *arr
```

```
W: Ambiguous splat operator. Parenthesize the method arguments if it's surely a splat operator, or add a whitespace to the right of the * if it should be a multiplication.
hoge *arr
     ^
```

## Lint/AmbiguousRegexpLiteral

"()"なしのメソッド呼び出しで、引数の曖昧な正規表現リテラルをチェックするCop。

- default
  - enabled: `false`

```ruby
# NG
hoge /regexp/
```

```
W: Ambiguous regexp literal. Parenthesize the method arguments if it's surely a regexp literal, or add a whitespace to the right of the / if it should be a division.
hoge /regexp/
     ^
```

## Lint/AssignmentInCondition

`if/while/until`内で変数代入をしているかをチェックするCop。

- default
  - enabled: `false`
- attributes
  - AllowSafeAssignment
    - "()"で囲まれた代入を除外するか
    - default: `true`

```ruby
# NG
if a = 'hoge'
  hoge
end

# OK (AllowSafeAssignment: trueの場合)
if (a = 'hoge')
  hoge
end
```

```
W: Assignment in condition - you probably meant to use ==.
if a = 'hoge'
     ^
```

## Lint/BlockAlignment

do-endの位置をチェックするCop。

- default
  - enabled: `false`
- attributes
  - EnforcedStyleAlignWith
    - 強制するスタイル
      - `start_of_block`: `end`の位置がブロックの開始位置と同じ
      - `start_of_line`: `end`の位置がブロック行の開始位置と同じ
      - `either`: 上記のどちらか
    - default: `either`
  - SupportedStylesAlignWith
    - 許可するスタイル
    - default: `[either, start_of_block, start_of_line]`

```ruby
# OK (EnforcedStyleAlignWith: start_of_blockの場合)
def index
  hoge.foo
      .bar do
         fuga
      end
end

# OK (EnforcedStyleAlignWith: start_of_lineの場合)
def index
  hoge.foo
      .bar do
         fuga
  end
end

# NG (EnforcedStyleAlignWith: start_of_lineの場合)
def index
  hoge.foo
      .bar do
         fuga
      end
end
```

```
W: end at 10, 6 is not aligned with hoge.foo at 7, 2.
      end
      ^^^
```

## Lint/CircularArgumentReference

引数のデフォルト値が循環参照になっているをチェックするCop。

- default
  - enabled: `false`

```ruby
# NG
def index(hoge = hoge)
  hoge.to_s
end

# OK
def index(hoge = self.hoge)
  hoge.to_s
end
```

```
W: Circular argument reference - hoge.
def index(hoge = hoge)
                 ^^^^
```

## Lint/ConditionPosition

`if/while/until`の式の位置をチェックするCop。

- default
  - enabled: `false`

```ruby
# NG
def index
  if
    true
    hoge
  end
end
```

```
W: Place the condition on the same line as if.
    true
    ^^^^
```

## Lint/Debugger

byebugなどが残っていないかをチェックするCop。

- default
  - enabled: `true`

```ruby
# NG
def index
  byebug
  hoge
end
```

```
W: Remove debugger entry point byebug.
  byebug
  ^^^^^^
```

## Lint/DefEndAlignment

def-endの位置をチェックするCop。

- default
  - enabled: `true`
- attributes
  - EnforcedStyleAlignWith
    - 強制するスタイル
      - `start_of_line`: `end`の位置がブロック行の開始位置と同じ
      - `def`: `end`の位置が`def`の開始位置と同じ
    - default: `start_of_line`
  - SupportedStylesAlignWith
    - 許可するスタイル
    - default: `[start_of_line, def]`
  - AutoCorrect
    - 
    - default: `false`

```ruby
# OK (EnforcedStyleAlignWith: start_of_lineの場合)
private def index
end

# OK (EnforcedStyleAlignWith: defの場合)
private def index
        end

# NG (EnforcedStyleAlignWith: defの場合)
private def index
end
```

```
W: end at 7, 0 is not aligned with def at 6, 8.
end
^^^
```

