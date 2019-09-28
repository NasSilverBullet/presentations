# Gopherdojo7 LT大会発表資料

## テーマ

### t.Errorf を考えよう

## トークフロー

### 1. Goではなんでテススティングフレームワーク(アサーションテストを含む)を使わないんだっけ？

#### [Why does Go not have assertions? ](https://golang.org/doc/faq#assertions)

- アサーションテストは便利
- でも楽しようとしてるだけじゃね？
- エラーレポートが適当になったら意味ないぜ
- 適切なエラーハンドリングをしようぜ
- これが当たり前ではないのはわかってる

=> 適切なエラーハンドリングをするため

### 2.ん？「適切なエラーハンドリング」って何?

#### [Where is my favorite helper function for testing?](https://golang.org/doc/faq#testing_framework)

> Proper error handling means letting other tests run after one has failed, so that the person debugging the failure gets a complete picture of what is wrong.

> 適切なエラーハンドリングとは、あるテストが失敗したとしても他のテストを実行できるようにすること。つまりデバッグをする人が何が問題なのかを完全にに把握できるようにすること。(意訳)

> It is more useful for a test to report that isPrime gives the wrong answer for 2, 3, 5, and 7 (or for 2, 4, 8, and 16) than to report that isPrime gives the wrong answer for 2 and therefore no more tests were run.

> isPrime って関数が 「2 を渡したときに失敗する」とレポートしてテストが終わるより、 「2, 3, 5, 7 を渡したときに失敗する」とレポートした方がわかる。(意訳)

=> どこで何が問題になってるのか明瞭にする必要がある

### 3. じゃあ具体的にはどんなエラーレポートがいいの？

#### [Where is my favorite helper function for testing?](https://golang.org/doc/faq#testing_framework)

> The standard Go library is full of illustrative examples, such as in [the formatting tests for the fmt package](https://golang.org/src/fmt/fmt_test.go).

> 標準パッケージに実例が山ほどある。[fmt](https://golang.org/src/fmt/fmt_test.go)とかね。(意訳)

### 4. 見てみよう

`v $GOPATH/src/github.com/golang/go/src/fmt/fmt_test.go`

https://golang.org/src/fmt/fmt_test.go

`t.Errorf("f(%v) = %v want %v", arg, got, want)`

`t.Errorf("f(%v) => %v, want %v", arg, got, want)`

`t.Errorf("got %v expected %v", got, expect`)

ここらへんが多い様子

### 4. 見てみよう2

`$ v $GOPAHT/src/github.com/golang/go/src/reflect/all_test.go`

`t.Errorf("hoges (%#v) do NOT-ok (but should)", elem)`

$ v $GOPAHT/src/github.com/golang/go/src/os/os_test.go

`t.Errorf("a shoud not be hoge")`

かなり柔軟性がある!

### 5. 見てみよう3

`v $GOPATH/src/github.com/docker/cli/opts/hosts_test.go`

`t.Errorf("tcp %v address expected error %v return, got %s and addr %v", invalidAddr, expectedError, err, addr)`

簡略化せずに文章で書いたり

`v $GOPATH/src/github.com/gohugoio/hugo/hugolib/site_test.go`

`t.Errorf("GroupByParam didn't return an expected error")`

エラーを期待するテストもある!

### 6. まとめ

- Go は適切なエラーハンドリングをするためにテスティングフレームワークを使わない

- 適切なエラーハンドリングとは、デバッグをする人が何が問題なのかを完全にに把握できること

- 標準パッケージでもOSSでも、書き方はかなり異なる

- 伝えたい情報を伝えることさえできればよい?
