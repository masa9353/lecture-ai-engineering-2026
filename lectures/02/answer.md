# 第2回 解答

## 演習

- 使ったモデル・設定（`result.json` の `config` から）：Qwen/Qwen3.5-0.8B
- 自分で学習を実行したか（`executed_training_myself`）：はい
- 学習前：`format_rate` = 0.09523 / `content_rate` = 0.0
- 学習後：`format_rate` =1.0 / `content_rate` = 0.9047
- 退行した質問：なし（`regressed` = `[]`）。ただし「成績はいつ出ますか」は学習後も不正解で、提出期限を誤って回答した。  
- 学習後に残った失敗と、その型（根拠なしの質問に断定した / 根拠ありの質問に確認要求で逃げた / 形式の崩れ）：
  - 「成績はいつ出ますか」：根拠がなく要確認の質問なのに、提出期限を回答として断定した（`needs_review=false`）。  
  - 「オフィスアワーは事前予約が必要ですか」：予約要否は要確認なのに、曜日・時間だけを答えて確認を促さなかった（`needs_review=false`）。  
  - 形式の崩れ：なし（学習後の `format_rate` は 1.0）。  
- 知識を問う質問（セクション 6）の結果（学習前の分類 → 学習後の分類）と気づいたこと：formatエラーは無くなっている。別情報を当てはめるケースあり  
- 学習で変わったこと・変わらなかったこと：
  - 変わったこと：評価時間が長くなり、答え自体もトークンが多い。より詳しく説明しようとしている。  
   学習 loss は 10 step 時点の 1.56 から 40 step 時点の 0.0139 まで低下した。評価では JSON 形式に従ったかは 9.5% から 100%に  
   内容正解率が 0% から 90.5% に上がり、FAQ にある提出期限・提出先・オフィスアワーなどに答えられるようになった。
  - 変わらなかったこと：FAQ に根拠があるかを常に正しく判断する力は身につかなかった。「成績はいつ出ますか」には提出期限を誤って答え、  
  「事前予約が必要ですか」には予約要否を答えられなかった。

``` json
{
  "config": {
    "base_model": "Qwen/Qwen3.5-0.8B",
    "revision": "2fc06364715b967f1860aea9cf38778875588b17",
    "seed": 42,
    "num_train_epochs": 20,
    "warmup_steps": 6,
    "lr": 0.0002,
    "lora_r": 16,
    "lora_alpha": 32,
    "max_seq_len": 512,
    "max_new_tokens": 96,
    "gen": {
      "do_sample": false
    }
  },
  "versions": {
    "transformers": "5.18.0",
    "peft": "0.21.1",
    "trl": "1.14.2",
    "torch": "2.11.0+cu130"
  },
  "method": "LoRA（量子化なし）",
  "before": {
    "label": "before",
    "n": 21,
    "format_rate": 0.09523809523809523,
    "content_rate": 0.0,
    "both_rate": 0.0,
    "seconds": 22.6
  },
  "after": {
    "label": "after",
    "n": 21,
    "format_rate": 1.0,
    "content_rate": 0.9047619047619048,
    "both_rate": 0.9047619047619048,
    "seconds": 46.6
  },
  "regressed": [],
  "executed_training_myself": true
}
```

## 宿題

- 取り組んだレベル：基礎
- 変えた設定やデータ：
`num_train_epochs` を10・20・40で比較
- 結果（比較表や数字）：

変更前
``` json
{
  "config": {
    "base_model": "Qwen/Qwen3.5-0.8B",
    "revision": "2fc06364715b967f1860aea9cf38778875588b17",
    "seed": 42,
    "num_train_epochs": 20,
    "warmup_steps": 6,
    "lr": 0.0002,
    "lora_r": 16,
    "lora_alpha": 32,
    "max_seq_len": 512,
    "max_new_tokens": 96,
    "gen": {
      "do_sample": false
    }
  },
  "versions": {
    "transformers": "5.18.0",
    "peft": "0.21.1",
    "trl": "1.14.2",
    "torch": "2.11.0+cu130"
  },
  "method": "LoRA（量子化なし）",
  "before": {
    "label": "before",
    "n": 21,
    "format_rate": 0.09523809523809523,
    "content_rate": 0.0,
    "both_rate": 0.0,
    "seconds": 22.6
  },
  "after": {
    "label": "after",
    "n": 21,
    "format_rate": 1.0,
    "content_rate": 0.9047619047619048,
    "both_rate": 0.9047619047619048,
    "seconds": 46.6
  },
  "regressed": [],
  "executed_training_myself": true
}
```

変更後 epoch 40
``` json
{
  "config": {
    "base_model": "Qwen/Qwen3.5-0.8B",
    "revision": "2fc06364715b967f1860aea9cf38778875588b17",
    "seed": 42,
    "num_train_epochs": 40,
    "warmup_steps": 6,
    "lr": 0.0002,
    "lora_r": 16,
    "lora_alpha": 32,
    "max_seq_len": 512,
    "max_new_tokens": 96,
    "gen": {
      "do_sample": false
    }
  },
  "versions": {
    "transformers": "5.18.0",
    "peft": "0.21.1",
    "trl": "1.14.2",
    "torch": "2.11.0+cu130"
  },
  "method": "LoRA（量子化なし）",
  "before": {
    "label": "before",
    "n": 21,
    "format_rate": 0.09523809523809523,
    "content_rate": 0.0,
    "both_rate": 0.0,
    "seconds": 29.7
  },
  "after": {
    "label": "after",
    "n": 21,
    "format_rate": 1.0,
    "content_rate": 0.9047619047619048,
    "both_rate": 0.9047619047619048,
    "seconds": 47.4
  },
  "regressed": [],
  "executed_training_myself": true
}
```
変更後 epoch 10
``` json
{
  "config": {
    "base_model": "Qwen/Qwen3.5-0.8B",
    "revision": "2fc06364715b967f1860aea9cf38778875588b17",
    "seed": 42,
    "num_train_epochs": 10,
    "warmup_steps": 6,
    "lr": 0.0002,
    "lora_r": 16,
    "lora_alpha": 32,
    "max_seq_len": 512,
    "max_new_tokens": 96,
    "gen": {
      "do_sample": false
    }
  },
  "versions": {
    "transformers": "5.18.0",
    "peft": "0.21.1",
    "trl": "1.14.2",
    "torch": "2.11.0+cu130"
  },
  "method": "LoRA（量子化なし）",
  "before": {
    "label": "before",
    "n": 21,
    "format_rate": 0.09523809523809523,
    "content_rate": 0.0,
    "both_rate": 0.0,
    "seconds": 37.6
  },
  "after": {
    "label": "after",
    "n": 21,
    "format_rate": 1.0,
    "content_rate": 0.8571428571428571,
    "both_rate": 0.8571428571428571,
    "seconds": 47.4
  },
  "regressed": [],
  "executed_training_myself": true
}
```

当初デフォルトの20から40に変更、`content_rate`でもそれほど差はでなかった。ほぼ収束ししているように見えたので  
10に変更。10・20・40 epoch の学習後を比較すると、`format_rate` はすべて 1.0。`content_rate` は  
10 epochで0.8571（18/21問）、20 epochで0.9048（19/21問）、40 epochで0.9048（19/21問）だった。  
10 epochから20 epochでは1問分改善したが、20 epochから40 epochでは変化がなかった。

- ファインチューニングが向く場面・向かない場面についての考察：
質の良いデータで回答形式、文体を調整したならファインチューニングに向いている。
知識の更新、事実性という所では検索、RAGによる更新を使った方がよい。
