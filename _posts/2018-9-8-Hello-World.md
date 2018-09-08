---
layout: post
title: Just testing, nothing interesting
---

### This is a test of fonts...


## some python code:

```python
class MAGGraphExtractor:
    def __init__(self, filename,
                 drop_empty_ref=True,
                 drop_empty_abs=True,
                 drop_non_en=True,
                 publishers_only=False,
                 drop_all_earlier_than=1980,
                 min_n_citations=20,
                 fuse_links=True
                 ):

        self.records, self.id_links = [], []
        self.ids = set()
        self.n_lines = 0
        with open(filename, 'r') as fin:
            for line in fin:
                self.n_lines += 1
                c_dict = json.loads(line)

                if (drop_non_en and
                        'lang' not in c_dict.keys()):
                    continue

                self.records += [c_dict]
                self.ids.add(c_dict['id'])
                self.id_links += [c_dict['references']]

        if fuse_links:
            self.__fuse_links()

    def __fuse_links(self):
        for i, rec in enumerate(self.records):
            fused_ref_links = []
            for ref in rec['references']:
                if ref in self.ids:
                    fused_ref_links += [ref]
            self.records[i]['references'] = fused_ref_links
```

Now table:

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

> Blockquotes are very handy in email to emulate reply text.
> This line is part of the same quote.

For more instructions head over to the [Jekyll Now repository](https://github.com/barryclark/jekyll-now) on GitHub.

Here is a formula test:
$$\forall x \in R$$

example (2):
$\sum_{i=1}^m y^{(i)}$


And here comes the tasks list:

- [x] @mentions, #refs, [links](), **formatting**, and <del>tags</del> supported
- [x] list syntax required (any unordered or ordered list supported)
- [x] this is a complete item
- [ ] this is an incomplete item

