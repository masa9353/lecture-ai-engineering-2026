# 第3回 解答

## 演習

- 第1回（素のLLM）とエージェントの回答の違い：

 第1回は「一般的に。。。」という事で推定の答えだが
 今回は断定している

- `--trace` で見えたツール呼び出し（ツール名と引数）：

```
[tool call]   search_glossary({'query': 'LoRA'})
[tool call]   search_glossary({'query': 'QLoRA'})
```

- 1つの質問での LLM 呼び出し回数（`requests`）：

２回

```
 details={'text_prompt_tokens': 1196}, input_text_tokens=1196, input_tokens=1196, output_tokens=193, requests=2, tool_calls=2)
```

- 追加したツールと、docstring に書いた説明：

```python

def make_quiz(lecture: int) -> str:
    """指定回の確認問題を求められたとき、その回の用語から1問を出すために使う。

    lectureには回番号を指定する。問題文と正解をJSON文字列で返す。
    """
    terms = [entry for entry in load_glossary() if entry["lecture"] == lecture]
    if not terms:
        return json.dumps(
            {"error": f"第{lecture}回の用語は用語集にありません。"},
            ensure_ascii=False,
        )

    term = random.choice(terms)
    return json.dumps(
        {"question": f"「{term['term']}」の意味は？", "answer": term["definition"]},
        ensure_ascii=False,
    )
```

```
{"question": "「ランク（LoRAのr）」の意味は？", "answer": "LoRAで使う低ランク行 列の大きさを決めるハイパーパラメータ。大きいほど表現力が上がるが、学習するパラメータとメモリも増える。"}
```


## 宿題

- 取り組んだレベル：基礎
- やったこと：
  - agent.pyのINSTRUCTIONSで「必ず search_glossary で用語集を確認する」
  を消してみたが、ツールをまだ呼んでいるように見える(tool_calls=2)。
  「search_glossary」は使用しないと明示したらcallしなかった。
  -  docstring の「query には調べたい用語を1つだけ入れる」を「まとめて入れる」に変えると用語を同じリストの中にいれてくる。問い合わせは1回のみ。


- 結果：

同上


- 気づき・疑問：
Agentへの指示により挙動が変わっていくものの
ある指標についての評価においても定性的な評価が難しいと感じた。
ソフトウェアの品質評価にAgentを使う事を考えているが、
結局必ずある挙動を実行させたい場合は、
ユーザー自身の実行によるコードベースでの実行しかないかと感じた