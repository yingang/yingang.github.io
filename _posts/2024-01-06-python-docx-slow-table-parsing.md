---
layout: post
title:  "python-docx 读取大表格速度很慢的问题"
date:   2024-01-06 16:36:00 +0800
categories: python docx
---

尝试写个小 Python 脚本来提取 Word 需求文档里的内容，结果发现读取文档里的表格性能有点差，特别是在读取行数比较多（几百）的表格时，速度非常慢。

使用了比较标准的方式来读取表格，类似于：

~~~python
from docx import Document
doc = Document(filepath)
for tbl in doc.tables:
  for row in tbl.rows:
    value = row.cells[0].text
    ...
~~~

结果对于一个大概 800 多行的表格，需要花费十几分钟才能解析完，从表格一开始大概半秒一行到后来一秒多一行，完全不能理解为啥会这么慢！

然后先写了个更简单的脚本，仅进行表格内容的遍历，可以基本确定性能瓶颈不在表格读取之外的那些处理逻辑上。再然后尝试在网上做了些检索，也没找到合适的答案。

再后来去翻看了 python-docx 的官方文档，想着也许有什么我不知道的接口可能更适合我的使用场景，然后在[这里](https://python-docx.readthedocs.io/en/latest/_modules/docx/table.html)就看到比较奇怪的东西了：

~~~python
class _Row(Parented):
    """Table row."""

    @property
    def cells(self) -> Tuple[_Cell]:
        """Sequence of |_Cell| instances corresponding to cells in this row."""
        return tuple(self.table.row_cells(self._index))
~~~

行的 `cells` 方法实际是调用了表格的 `row_cells` 方法，而表格的 `row_cells` 方法又是这么定义的：

~~~python
class Table(Parented):
    """Proxy class for a WordprocessingML ``<w:tbl>`` element."""

    def row_cells(self, row_idx: int) -> List[_Cell]:
        """Sequence of cells in the row at `row_idx` in this table."""
        column_count = self._column_count
        start = row_idx * column_count
        end = start + column_count
        return self._cells[start:end]

    @property
    def _cells(self) -> List[_Cell]:
        """A sequence of |_Cell| objects, one for each cell of the layout grid.

        If the table contains a span, one or more |_Cell| object references are
        repeated.
        """
        col_count = self._column_count
        cells = []
        for tc in self._tbl.iter_tcs():
            for grid_span_idx in range(tc.grid_span):
                if tc.vMerge == ST_Merge.CONTINUE:
                    cells.append(cells[-col_count])
                elif grid_span_idx > 0:
                    cells.append(cells[-1])
                else:
                    cells.append(_Cell(tc, self))
        return cells
~~~

也就是说，`row_cells` 引用的 `_cells` 看上去每次被调用时都在重新构建一个新的列表？？？

不确定是否完全理解这些代码及其目的，尝试在自己的代码直接拿到 `_cells`，然后基于行列坐标自己去访问想要读取的 `cell`，结果对于上面说的那个 800 多行的表格，不到一秒就访问完了！差不多三个数量级的性能提升？

示例代码如下，供参考：

~~~python
from docx import Document
doc = Document(filepath)
for tbl in doc.tables:
  cells = tbl._cells
  row_count = len(tbl.rows)
  col_count = tbl._column_count
  for row in range(0, row_count):
    value = cells[row * col_count].text
    ...
~~~

<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>