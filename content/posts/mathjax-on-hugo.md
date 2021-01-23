---
title: "MathJax を Hugo で読み込む"
date: 2020-04-01T14:33:19+09:00
draft: false
toc: false
images:
tags:
  - hugo
  - mathjax
mathjax: true
---

Hugo on GitHub Pages にお引越ししてみました。  
そのついでに MathJax を導入してみました。MathJax 3 になり、導入方法が変わっていたので公式の [ドキュメント](https://www.mathjax.org/#gettingstarted) を参考にしました。

このサイトは Hugo で生成されているので `layouts/patials/footer.html` に

```html
<script>
    MathJax = {
        tex: {
            inlineMath: [['$', '$']],
            displayMath: [['$$','$$']],
            processEscapes: true,
            processEnvironments: true
        }
    };
</script>
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"></script>
```

を追記すれば OK。このままでは全ページで MathJax がロードされてしまうので、[先人の記事](https://kenbannikki.com/notes/using-mathjax-with-hugo/) にある通り

```html
{{ if .Params.mathjax }}
  ...
{{ end }}
```

で挟んであげたのち、記事の .md ファイルの Front Matter で

```yaml
mathjax: true
```

を指定するだけで記事を絞り込んで MathJax を有効化できます。今回はインライン数式に `$` を指定したので

```html
$ \sin^2 x + \cos^2 x = 1 $
```

と書くと $ \sin^2 x + \cos^2 x = 1 $ が出力され、代わりに `$$` を使うと

```html
$$ \int_{-\infty}^{+\infty}f(x)dx \tag{1} $$
```

$$ \int_{-\infty}^{+\infty}f(x)dx \tag{1} $$

が出力されます。以下、MathJax のレンダリングテスト。

- 二項定理

$$ \left(x+a\right)^n=\sum_{k=0}^{n}{\binom{n}{k}x^ka^{n-k}}  \tag{2} $$

- 許容/禁制遷移と電子双極子の遷移確率 $ P_{mn} $

$$ P_{mn}=\int_{-\infty}^{+\infty}{\varphi_m(x)ex}\varphi_n(x)dx \tag{3} $$
