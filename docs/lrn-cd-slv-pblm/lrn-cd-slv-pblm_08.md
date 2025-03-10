## 第八章：使用集合和字典组织值

![image](img/common.jpg)

当我们需要存储一系列值时，例如动作人物的高度或文章中的单词时，Python 列表是非常有用的。列表使我们能够轻松保持值的顺序，并根据索引访问某个值。然而，正如我们将在本章中看到的那样，列表并不适合执行一些操作，包括确定一个特定的值是否在集合中以及在一对值之间建立关联。

在本章中，我们将学习 Python 集合和字典，它们是存储值集合的两种替代方案。我们将看到，当我们需要查找特定的值并且不关心它们的顺序时，集合是首选工具；而当我们需要处理一对值时，字典是首选工具。

我们将使用这些新的集合解决三个问题：确定唯一电子邮件地址的数量、在一组单词中找到公共单词，以及确定一对城市和州的特殊组合数量。

### 问题 #18：电子邮件地址

在这个问题中，我们将存储一组电子邮件地址。我们不关心每个电子邮件地址出现的次数，也不关心电子邮件地址的顺序。这些宽松的存储要求意味着我们可以用集合来替代列表——集合是一种 Python 类型，它的速度远远超过列表。我们将学习有关集合的一切。

这是 DMOJ 问题 `ecoo19r2p1`。

#### 挑战

你知道有很多种方式可以写一个人的 Gmail 地址吗？

我们可以在某人的 Gmail 地址中，在 `@` 符号前加上一个加号（`+`）符号和一个字符串，这样他们就能收到我们发送到那个新地址的任何邮件。也就是说，针对 Gmail 地址，所有从 `+` 符号到 `@` 符号之前的字符都会被忽略。例如，我告诉别人我的 Gmail 地址是 *daniel.zingaro@gmail.com*，但这只是写法之一。如果你发送邮件到 *daniel.zingaro+book@gmail.com* 或 *daniel.zingaro+hi.there@gmail.com*，我也会收到。（选择你喜欢的方式，打个招呼！）

在 Gmail 地址中，`@` 符号前的点也会被忽略。例如，如果你发送邮件到 *danielzingaro@gmail.com*（没有点），*daniel..zingaro@gmail.com*（两个点连在一起），*da.nielz.in.gar.o..@gmail.com*（混乱的点），*daniel.zin.garo+blah@gmail.com* 等等，我都会收到。

最后一件事：地址中的大小写差异会被忽略。我希望到此时你没有对我发起一阵攻击，但无论如何，任何发到 *Daniel.Zingaro@gmail.com*、 *DAnIELZIngARO+Flurry@gmAIL.COM* 等的邮件，我都会收到。

在这个问题中，我们被提供了电子邮件地址，并要求我们确定其中有多少是唯一的。这个问题中电子邮件地址的规则与 Gmail 中讨论的规则相同：从 `+` 符号到 `@` 符号前的字符会被忽略，`@` 符号前的点会被忽略，整个地址中的大小写会被忽略。

#### 输入

输入包含 10 个测试用例。每个测试用例包含以下行：

+   一行包含整数 *n*，表示电子邮件地址的数量。*n* 在 1 和 100,000 之间。

+   *n* 行，每行给出一个电子邮件地址。每个电子邮件地址由 `@` 符号前至少一个字符和 `@` 符号本身，以及 `@` 符号后至少一个字符组成。`@` 符号前的字符可以是字母、数字、点和加号。`@` 符号后的字符可以是字母、数字和点。

#### 输出

对于每个测试用例，输出唯一电子邮件地址的数量。

解决测试用例的时间限制是 30 秒。

### 使用列表

你已经学习了本书的七个章节。在每一章中，我提出了一个问题，并教你一些新的 Python 特性，以便你能解决这个问题。因此，你可能会期待我在解决电子邮件地址问题之前，先教你一些新的 Python 知识。

你可能会反对这一点：我们难道已经拥有我们需要的东西了吗？毕竟，我们可以写一个函数，接收一个电子邮件地址并返回清理后的版本，去掉 `+` 部分，去掉 `@` 符号前的点，并且全部小写。我们还可以维护一个清理后的电子邮件地址列表。对于我们看到的每个电子邮件地址，我们可以将其清理并检查它是否已经在清理后的电子邮件地址列表中。如果没有，我们可以将其添加进去；如果已经有了，则什么也不做（因为它已经被计数）。一旦我们处理完所有电子邮件地址，列表的长度将给我们唯一电子邮件地址的数量。

是的。我们可能已经拥有我们需要的东西。让我们试着解决这个问题。

#### 清理电子邮件地址

考虑电子邮件地址 *DAnIELZIngARO+Flurry@gmAIL.COM*。我们将清理这个电子邮件地址，使其变成 *danielzingaro@gmail.com*。去掉 +Flurry，去掉 @ 符号前的点，并且全部转为小写。我们可以将清理后的版本视为真实的电子邮件地址。任何其他表示相同真实电子邮件地址的邮件地址，在清理后也会匹配 *danielzingaro@gmail.com*。

清理邮箱地址是一个小而独立的任务，所以我们来为此写一个函数。这个 `clean` 函数将接受一个表示邮箱地址的字符串，清理它并返回清理后的邮箱地址。我们将执行三个清理步骤：移除从 `+` 符号到 `@` 符号之前的字符、去掉 `@` 符号前的点（`.`），以及转换为小写。这个函数的代码在 Listing 8-1 中。

```py
def clean(address):

    """

    address is a string email address.

    Return cleaned address.

    """

    # Remove from '+' up to but not including '@'

 ❶ plus_index = address.find('+')

    if plus_index != -1:

     ❷ at_index = address.find('@')

        address = address[:plus_index] + address[at_index:]

    # Remove dots before @ symbol

    at_index = address.find('@')

    before_at = ''

    i = 0

    while i < at_index:

     ❸ if address[i] != '.':

            before_at = before_at + address[i]

        i = i + 1

 ❹ cleaned = before_at + address[at_index:]

    # Convert to lowercase

 ❺ cleaned = cleaned.lower()

    return cleaned
```

*Listing 8-1：清理邮箱地址*

第一步是移除从 `+` 符号到 `@` 符号前的字符。`find` 字符串方法在这里很有用。它返回其参数在字符串中最左侧出现的索引，如果找不到该参数，则返回 `-1`：

```py
>>> 'abc+def'.find('+')

3

>>> 'abcdef'.find('+')

-1
```

我使用 `find` 方法来确定最左侧的 `+` 符号的索引 ❶。如果没有 `+` 符号，就不需要做这一步。如果有，则我们再查找 `@` 符号的索引 ❷，并从 `+` 符号到 `@` 符号之间的字符进行删除。

第二步是去掉 `@` 符号前的点（`.`）。为此，我使用一个新字符串 `before_at` 来积累 `@` 符号前的部分地址。每个在 `@` 符号前且不是 `.` 的字符都会被添加到 `before_at` ❸。

`before_at` 字符串不包含 `@` 符号或其后的任何字符。我们不想丢失邮箱地址的这一部分，所以我使用一个新变量 `cleaned` 来表示整个邮箱地址 ❹。

第三步是将整个邮箱地址转换为小写 ❺。完成后，邮箱地址就被清理干净了，因此我们可以返回它。

让我们稍微测试一下。将 `clean` 函数的代码输入到 Python shell 中。下面是该函数清理几个邮箱地址的过程：

```py
>>> clean('daniel.zingaro+book@gmail.com')

'danielzingaro@gmail.com'

>>> clean('da.nielz.in.gar.o..@gmail.com')

'danielzingaro@gmail.com'

>>> clean('DAnIELZIngARO+Flurry@gmAIL.COM')

'danielzingaro@gmail.com'

>>> clean('a.b.c@d.e.f')

'abc@d.e.f'
```

如果邮箱地址已经是干净的，`clean` 函数就直接返回它：

```py
>>> clean('danielzingaro@gmail.com')

'danielzingaro@gmail.com'
```

#### 主程序

我们可以使用 `clean` 函数清理任何邮箱地址。现在的策略是保持一个清理后的邮箱地址列表。只有当某个清理后的邮箱地址还没有被添加到列表中时，我们才会将其加入。这样，我们就能避免添加重复的邮箱地址。

程序的主要部分在 Listing 8-2 中。在这个代码之前，请确保输入我们的 `clean` 函数代码（Listing 8-1），以完整解决问题。

```py
# Main Program

for dataset in range(10):

    n = int(input())

 ❶ addresses = []

    for i in range(n):

        address = input()

        address = clean(address)

     ❷ if not address in addresses:

            addresses.append(address)

 ❸ print(len(addresses))
```

*Listing 8-2：主程序，使用列表*

我们有 10 个测试用例需要处理，因此将程序的其余部分放入一个循环中，循环 10 次。

对于每个测试用例，我们读取邮箱地址的数量，并从一个空的清理邮箱地址列表开始 ❶。

然后我们使用一个内部的 `for` 循环来遍历每个邮箱地址。我们读取每个邮箱地址并清理它。如果这个清理后的邮箱地址之前没有出现过 ❷，我们就把它添加到清理过的邮箱地址列表中。

当内层循环结束时，我们将建立一个所有干净电子邮件地址的列表。这个列表中没有重复项。因此，唯一的电子邮件地址数量就是这个列表的长度，这就是我们输出的内容 ❸。

不错吧？几乎就像我们在第六章学到函数后就能解决这个问题。或者，实际上，在我们学习第五章的列表后就能解决。

差不多，但还差一点。因为如果你提交给裁判，你应该会注意到事情并没有按计划进行。

麻烦的第一个迹象是裁判花了一些时间才显示我们的结果。例如，我刚才在这里等待了一分钟，才看到我的结果。相比之前我们解决的其他问题，那里反馈速度非常快。

麻烦的第二个迹象是，当我们的结果显示出来时，我们没有为这个问题获得满分！我得到了 3.25 分（满分 5 分）。你可能得到更多或更少的分数，但你不应该得到满分 5 分。

我们丢分的原因不是因为程序有错误。我们的程序是正确的。无论测试用例是什么，它都会输出正确的唯一电子邮件地址数量。

那么，如果我们的程序是正确的，问题出在哪里呢？

问题在于我们的程序太慢了。裁判通过在每个测试用例前面加上 `TLE` 来告诉我们这一点。TLE 代表超出时间限制。对于这个问题，裁判为每批 10 个测试用例分配了 30 秒的时间。如果我们的程序超过 30 秒没有完成，裁判会终止程序，剩下的测试用例将无法继续执行。

这可能是你第一次遇到超出时间限制的错误，尽管在完成之前章节的练习时，你可能也遇到过类似的错误。

当你收到这个错误时，首先要检查的是程序是否陷入了无限循环。如果是的话，不管时间限制是多少，它都永远不会结束。裁判会在分配的时间到期时终止程序。

如果没有无限循环，那么可能的罪魁祸首就是我们程序本身的*效率*。当程序员谈论效率时，他们指的是程序运行所需的时间。一个运行得更快（耗时更少）的程序比一个运行得更慢（耗时更多）的程序更高效。为了在时间限制内解决测试用例，我们将使程序更加高效。

### 列表搜索的效率

向 Python 列表添加元素是非常快的。无论列表中只有几个值还是成千上万个值，添加元素所花的时间都差不多。

然而，使用`in`操作符则是另一回事。我们的程序使用`in`操作符来判断一个清洁的电子邮件地址是否已经存在于我们的清洁电子邮件地址列表中。一个测试用例可能包含多达 100,000 个电子邮件地址。在最坏的情况下，我们的程序可能需要使用`in`操作 100,000 次。事实证明，当在一个包含许多值的列表上使用`in`时，它非常慢，这最终会影响我们程序的效率。为了判断一个值是否在列表中，`in`会从头到尾逐个值地搜索列表。它会一直搜索，直到找到它正在寻找的值，或者没有更多的列表值可供检查。`in`需要查看的值越多，它的速度就越慢。

让我们了解一下随着列表长度增加，`in`操作如何变慢。我们将使用一个函数，该函数接受一个列表和一个值，并使用`in`来搜索列表中的该值。它会搜索 50,000 次；如果我们只搜索一次，那会太快，我们根本无法看到发生了什么。

这个函数位于清单 8-3 中。将其代码输入到 Python shell 中。

```py
def search(collection, value):

    """

    search many times for value in collection.

    """

    for i in range(50000):

        found = value in collection
```

*清单 8-3：多次搜索一个集合*

让我们创建一个从 1 到 5,000 的整数列表，并搜索`5000`。通过搜索列表中的最右边的值，我们使得`in`在该列表上花费尽可能多的时间。别担心我们用整数列表而不是电子邮件地址列表来进行探索。效率会类似，而且数字比电子邮件地址要容易生成得多！

开始吧：

```py
>>> search(list(range(1, 5001)), 5000)
```

在我的笔记本上，这大约需要三秒钟来运行。我们不需要精确的计时；我们只是想了解随着列表长度增加，发生了什么情况。

现在让我们创建一个从 1 到 10,000 的整数列表，并搜索`10000`：

```py
>>> search(list(range(1, 10001)), 10000)
```

在我的笔记本上，这大约需要六秒钟。到目前为止的总结是，对于一个长度为`5000`的列表，耗时三秒；将列表长度加倍到`10000`，时间也翻倍，达到了六秒。

一个长度为`20000`的列表？试试看：

```py
>>> search(list(range(1, 20001)), 20000)
```

在我的笔记本上，这大约需要 12 秒。

时间再次翻倍。试试一个长度为`50000`的列表。你需要等一会儿。我刚在我的笔记本上运行了这个：

```py
>>> search(list(range(1, 50001)), 50000)
```

它花了超过 30 秒。记住，我们的`search`函数正在搜索 50,000 次列表。所以，它花了 30 秒来总共搜索一个长度为`50000`的列表 50,000 次。

我们可能会有一个需要进行如此多次搜索的测试用例。例如，假设我们将 100,000 个独特的电子邮件地址一个个添加到我们的列表中。在一半的过程中，我们将有一个包含 50,000 个值的列表；从那时起，剩下的 50,000 次`in`操作将会作用于一个至少包含 50,000 个值的列表。

这仅仅是 10 个测试用例中的一个！我们需要在 30 秒内完成所有 10 个测试用例。如果一个测试用例单独就能花费大约 30 秒，那我们就没有机会了。

查找列表实在是太慢了。Python 列表并不是适合这个工作的类型。我们需要一个更适合的类型。我们需要一个 Python 集合。你简直不敢相信查找集合是多么的快速。

### 集合

一个*集合*是一个 Python 类型，它存储一组值，其中不允许重复的值。我们使用大括号来限定集合。

与列表不同，集合可能不会保持你指定的顺序。这是一个整数集合：

```py
>>> {13, 15, 30, 45, 61}

{45, 13, 15, 61, 30}
```

注意到 Python 打乱了值的顺序。你可能在你的计算机上看到不同的顺序。关键点是你不能依赖值的任何特定顺序。如果顺序对你很重要，集合就不是合适的类型。

如果我们尝试包含一个值的多个出现，只有一个会被保留：

```py
>>> {1, 1, 3, 2, 3, 1, 3, 3, 3}

{1, 2, 3}
```

集合是相等的，如果它们包含完全相同的值，即使我们以不同的顺序书写它们：

```py
>>> {1, 2, 3} == {1, 2, 3}

True

>>> {1, 1, 3, 2, 3, 1, 3, 3, 3} == {1, 2, 3}

True

>>> {1, 2} == {1, 2, 3}

False
```

我们可以创建一个字符串集合，像这样：

```py
>>> {'abc@d.e.f', 'danielzingaro@gmail.com'}

{'abc@d.e.f', 'danielzingaro@gmail.com'}
```

我们不能创建一个包含列表的集合：

```py
>>> {[1, 2], [3, 4]}

Traceback (most recent call last):

  File "<stdin>", line 1, in <module>

TypeError: unhashable type: 'list'
```

集合中的值必须是不可变的，这解释了为什么我们不能将列表放入集合中。这个限制与 Python 如何在集合中查找值有关。当 Python 向集合中添加一个值时，它使用该值本身来确定它应该存储的位置。后来，Python 可以通过查看应该存放值的位置来找到它。如果集合中的值可能会改变，Python 可能会在错误的位置查找，导致找不到该值。

虽然我们不能创建一个包含列表的集合，但创建一个包含集合的列表没有问题：

```py
>>> lst = [{1, 2, 3}, {4, 5, 6}]

>>> lst

[{1, 2, 3}, {4, 5, 6}]

>>> len(lst)

2

>>> lst[0]

{1, 2, 3}
```

你可以使用 `len` 函数来确定集合中值的数量：

```py
>>> len({2, 4, 6, 8})

4
```

你也可以遍历集合中的值：

```py
>>> for value in {2, 4, 6, 8}:

...     print('I found', value)

...

I found 8

I found 2

I found 4

I found 6
```

然而，你不能对集合进行索引或切片。集合中的值没有索引。

要创建一个空集合，你可能会期待使用一个空的大括号`{}`。但在 Python 语法的不一致性中，这样是行不通的：

```py
>>> type({2, 4, 6, 8})

<class 'set'>

>>> {}

{}

>>> type({})

<class 'dict'>
```

使用 `{}` 会给我们错误的类型：一个 `dict`（字典），而不是一个 `set`（集合）。我们将在本章稍后讨论字典。

要创建一个空集合，我们使用 `set()`，像这样：

```py
>>> set()

set()

>>> type(set())

<class 'set'>
```

### 集合方法

集合是可变的，因此我们可以添加和删除值。我们可以通过使用方法来执行这些任务。

你可以通过使用 `dir(set())` 获取集合方法的列表。你也可以使用 `help` 获取某个特定集合方法的帮助，类似于我们用 `help` 学习字符串或列表方法。例如，要了解 `add` 方法，可以输入 `help(set().add)`。

`add` 方法是用来向集合添加一个值的。它类似于列表的 `append` 方法：

```py
>>> s = set()

>>> s

set()

>>> s.add(2)

>>> s

{2}

>>> s.add(4)

>>> s

{2, 4}

>>> s.add(6)

>>> s

{2, 4, 6}

>>> s.add(8)

>>> s

{8, 2, 4, 6}

>>> s.add(8)

>>> s

{8, 2, 4, 6}
```

要删除一个值，我们使用 `remove` 方法：

```py
>>> s.remove(4)

>>> s

{8, 2, 6}

>>> s.remove(8)

>>> s

{2, 6}

>>> s = {2, 6}

>>> s.remove(8)

Traceback (most recent call last):

  File "<stdin>", line 1, in <module>

KeyError: 8
```

**概念检查**

使用 `help` 来了解集合的 `update` 和 `intersection` 方法。

以下代码中 `print` 的输出是什么？

```py
s1 = {1, 3, 5, 7, 9}

s2 = {1, 2, 4, 6, 8, 10}

s3 = {1, 4, 9, 16, 25}

s1.update(s2)

s1.intersection(s3)

print(s1)
```

A. `{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}`

B. `{1, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10}`

C. `{1, 4, 9}`

D. `{1, 4, 9, 16, 25}`

E. `{1}`

答案：A. `update` 方法将集合 `s2` 中存在但 `s1` 中缺少的元素添加到集合 `s1`。在调用 `update` 后，`s1` 就是集合 `{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}`。

现在来看看 `intersection` 的调用。两个集合的交集是一个包含同时存在于两个集合中的值的新集合。在这里，`s1` 和 `s3` 的交集是 `{1, 4, 9}`。然而，`intersection` 方法*不会*修改集合；它会生成一个新集合！因此，它对 `s1` 没有影响。

### 搜索集合的效率

回到解决电子邮件地址的问题。

我们关心清理后的电子邮件地址的顺序吗？不关心！我们只关心一个电子邮件地址是否已经存在于其中。

我们需要在清理后的电子邮件地址中允许重复吗？当然不需要！实际上，我们明确地希望避免存储重复的电子邮件地址。

顺序不重要，且不允许重复。这两点表明集合可能是合适的类型。

我们在尝试使用列表时遇到了困难，因为列表的搜索太慢了。集合将是我们改进的方案，因为我们可以比搜索列表更快地搜索集合。

我们已经在 清单 8-3 中使用了 `search` 函数来搜索一个列表。但这个函数并没有做任何特别需要列表的操作！它使用了 `in` 操作符，而 `in` 同样适用于列表和集合。所以我们可以不做任何修改地将这个函数用来搜索集合。

在 Python shell 中输入来自 清单 8-3 的 `search` 函数。跟着你的电脑操作，感受一下在长列表和大集合中搜索的区别：

```py
   >>> search(list(range(1, 50001)), 50000)

❶ >>> search(set(range(1, 50001)), 50000)
```

在 ❶ 处，我使用 `set` 生成了一个整数集合，而不是列表。

在我的笔记本上，搜索列表大约需要 30 秒。相比之下，搜索集合速度飞快，几乎是瞬间完成。

集合是不可阻挡的。不要在列表上尝试这种操作，但我们来试试，在一个包含 50 万个值的集合中搜索某个值：

```py
>>> search(set(range(1, 500001)), 500000)
```

哗啦！小菜一碟。

Python 以某种方式管理列表，允许我们在任何时候使用任何索引。Python 在值的顺序上没有灵活性：第一个值必须在索引 0，第二个值在索引 1，依此类推。但对于集合，Python 可以以任何方式存储它，因为它没有承诺保持顺序。这种增加的灵活性使得 Python 能够优化集合中的搜索速度。

出于类似的原因，某些操作在大列表上非常慢，但在大集合上非常快。例如，从列表中移除一个值非常慢，因为 Python 必须更新该值右侧所有值的索引。相比之下，从集合中移除一个值非常快：因为没有索引需要更新！

### 解决问题

我们已经有了一个清理电子邮件地址的函数（清单 8-1），并将在基于集合的解决方案中使用它。至于主程序，清单 8-2 基本上解决了大部分问题。我们只需要用集合替换列表。

新的主程序在 Listing 8-4 中。要完整解决此问题，请在此代码之前包含 Listing 8-1。

```py
# Main Program

for dataset in range(10):

    n = int(input())

 ❶ addresses = set()

    for i in range(n):

        address = input()

        address = clean(address)

     ❷ addresses.add(address)

    print(len(addresses))
```

*Listing 8-4：主程序，使用集合*

请注意，我们现在使用的是一组电子邮件地址 ❶，而不是列表。在清理每个电子邮件地址后，我们使用集合的 `add` 方法 ❷ 将其添加到集合中。

在 Listing 8-2 中，我们使用 `in` 操作符检查电子邮件地址是否已经存在于列表中，以避免添加重复项。在我们的基于集合的解决方案中，没有相应的 `in` 检查。它去哪儿了？似乎我们正在将每个电子邮件地址直接添加到集合中，而没有确保它已经存在。

使用集合时，我们可以省略 `in` 检查，因为集合从不包含重复元素。`add` 方法会为我们处理 `in` 检查，确保不会添加重复元素。你可以认为 `add` 已经执行了自己的 `in` 检查。这里没有时间问题，因为查找集合中的元素非常快。

如果你将此解决方案提交给评审员，你应该能够在时间限制内顺利通过所有测试用例。

正如你在这里看到的，选择合适的 Python 类型可以决定一个不令人满意的解决方案和一个令人满意的解决方案之间的差异。在开始编写代码之前，问问自己将频繁执行哪些操作，哪些 Python 类型最适合这些操作。

在继续之前，你可能想尝试解决《章节练习》中的第 1 和第 2 题，见 第 236 页。

### 问题 #19：常见单词

在这个问题中，我们需要将单词与其出现次数关联起来。这超出了集合所能做到的范围，因此我们不会在这里使用集合。相反，我们将学习并使用 Python 字典。

这是 DMOJ 问题 `cco99p2`。

#### 挑战

我们给定了 *m* 个单词。这些单词不一定是唯一的；例如，单词 `brook` 可能会出现多次。我们还给定了一个整数 *k*。

我们的任务是找到 *k* 最常见的单词。如果一个单词 *w* 是第 *k* 最常见的单词，则恰好有 *k* - 1 个不同的单词比 *w* 出现得更频繁。根据数据集的不同，*k* 最常见的单词可能没有单词，可能有一个单词，也可能有多个单词。

让我们确保对 *k* 最常见单词的定义有清晰的理解。如果 *k* = 1，那么我们被要求找出那些没有比它出现次数更多的单词；也就是说，我们要找出最常见的单词。如果 *k* = 2，那么我们被要求找出那些恰好有一个单词比它出现次数更多的单词。如果 *k* = 3，那么我们被要求找出那些恰好有两个不同单词比它出现次数更多的单词，以此类推。

#### 输入

输入包含一行，给出测试用例的数量，接着是测试用例本身的各行。每个测试用例包含以下几行：

+   一行包含整数*m*（测试用例中的单词数）和*k*，中间用空格隔开。*m*在 0 到 1,000 之间；*k*至少为 1。

+   *m*行，每行给出一个单词。每个单词最多包含 20 个字符，并且所有字符都是小写字母。

#### 输出

对于每个测试用例，输出以下行：

+   一行包含以下内容：

    ```py
    p most common word(s):
    ```

    当*p*是`1st`时，如果*k*是 1；`2nd`时，如果*k*是 2；`3rd`时，如果*k*是 3；`4th`时，如果*k*是 4，以此类推。

+   每个*k*最常见单词的一行。如果没有这样的单词，就没有输出行。

+   空行。

解决测试用例的时间限制为 1 秒。

### 探索一个测试用例

让我们从探索一个测试用例开始。这将有助于我们更好地理解问题，并促使我们使用新的 Python 类型。

假设我们对所有单词中最常见的单词感兴趣。这意味着*k*是 1。以下是测试用例：

```py
1

14 1

storm

cut

magma

cut

brook

gully

gully

storm

cliff

cut

blast

brook

cut

gully
```

出现次数最多的单词是`cut`。`cut`出现了四次，其他单词的出现次数没有这么多。因此，正确的输出是：

```py
   1st most common word(s):

   cut

❶
```

注意末尾需要空行 ❶。

现在，如果*k*是 2，我们该怎么做呢？我们可以通过再次扫描单词并计算出现次数来回答这个问题，但还有一种不同的方式来组织单词，这将使我们的任务变得更容易。与其查看单词列表，不如查看每个单词及其出现次数。见表 8-1。

**表 8-1：** 单词及其出现次数

| **单词** | **出现次数** |
| --- | --- |
| cut | 4 |
| gully | 3 |
| storm | 2 |
| brook | 2 |
| magma | 1 |
| cliff | 1 |
| blast | 1 |

我已根据单词的出现次数对它们进行了排序。从第一行看，我们可以确认`cut`是*k* = 1 时的输出单词。从第二行看，我们看到`gully`是*k* = 2 时的输出单词。单词`gully`是唯一一个有一个单词出现次数更多的单词。

现在，对于*k* = 3。此时，有*两个*单词需要输出，`storm`和`brook`，因为它们都有相同的出现次数。每个单词都有恰好两个出现次数更多的单词。这表明有时我们需要输出多个单词。

也有可能我们需要输出零个单词！例如，考虑*k* = 4 时。没有单词恰好有三个单词出现次数更多。从表格中往下看，你可能会想，为什么我们不在*k* = 4 时输出`magma`？我们不输出`magma`，因为`magma`有恰好四个单词（而不是恰好三个单词）出现次数更多。

当*k* = 5 时，我们有三个单词需要输出：`magma`、`cliff`和`blast`。在继续之前，请自行验证对于其他任何*k*的值（如*k* = 6，*k* = 7，*k* = 8，*k* = 9，*k* = 100 等）没有要输出的单词。

表 8-1 简化了我们的任务。接下来，我们将学习如何在 Python 中组织这种信息。

### 字典

*字典* 是一种 Python 类型，用于存储从一组元素（称为 *键*）到另一组元素（称为 *值*）的映射。

我们使用开括号和闭括号来界定字典。这些符号与集合的符号相同，但 Python 可以通过括号内的内容来区分集合和字典。对于集合，我们列出值；对于字典，我们列出 `key:value` 键值对。

这里有一个将一些字符串映射到数字的字典：

```py
>>> {'cut':4, 'gully':3}

{'cut': 4, 'gully': 3}
```

在这个字典中，键是 `'cut'` 和 `'gully'`，值分别是 `4` 和 `3`。键 `'cut'` 映射到值 `4`，键 `'gully'` 映射到值 `3`。

根据我们对集合的了解，你可能会想知道字典是否会按我们输入的顺序保留键值对。例如，你可能会想知道是否会发生以下情况：

```py
>>> {'cut':4, 'gully':3}

{'gully': 3, 'cut': 4}
```

从 Python 3.7 开始，答案是否定的：字典会保留你添加键值对的顺序。在早期版本的 Python 中，字典并不保持顺序，因此你可以按某种顺序添加键值对，但返回时却是另一种顺序。不过，还是建议编写不依赖于 Python 3.7 行为的代码，因为旧版本的 Python 可能会在可预见的未来继续使用。

如果字典包含相同的 `key:value` 键值对，即使顺序不同，它们也是相等的：

```py
>>> {'cut':4, 'gully':3} == {'cut':4, 'gully':3}

True

>>> {'cut':4, 'gully':3} == {'gully': 3, 'cut': 4}

True

>>> {'cut':4, 'gully':3} == {'gully': 3, 'cut': 10}

False

>>> {'cut':4, 'gully':3} == {'cut': 4}

False
```

字典的键必须是唯一的。如果你尝试多次包含相同的键，那么只有与该键相关的一个键值对会被保留：

```py
>>> {'storm': 1, 'storm': 2}

{'storm': 2}
```

相反，重复的值是可以的：

```py
>>> {'storm': 2, 'brook': 2}

{'storm': 2, 'brook': 2}
```

键必须是不可变的值，如数字和字符串。值可以是不可变或可变的。这意味着我们不能使用列表作为键，但可以使用列表作为值：

```py
>>> {['storm', 'brook']: 2}

Traceback (most recent call last):

  File "<stdin>", line 1, in <module>

TypeError: unhashable type: 'list'

>>> {2: ['storm', 'brook']}

{2: ['storm', 'brook']}
```

`len` 函数返回字典中 `key:value` 键值对的数量：

```py
>>> len({'cut':4, 'gully':3})

2

>>> len({2: ['storm', 'brook']})

1
```

要创建一个空字典，我们使用 `{}`。这也是为什么我们只能使用那个二流的 `set()` 语法来创建集合——字典获得了更漂亮的语法：

```py
>>> {}

{}

>>> type({})

<class 'dict'>
```

该类型被称为 `dict`，而不是 `dictionary`。

在 Python 资源和代码中，你会看到“字典”和“dict”交替使用，但在本书中我将坚持使用“字典”。

**概念检查**

以下哪个更适合用字典而不是列表或集合？

A. 人们完成比赛的顺序

B. 一个食谱所需的食材

C. 国家名称及其首都城市

D. 50 个随机整数

答案：C。这是唯一一个包含键值对映射的选项。这里，键可以是国家名，值可以是它们的首都城市。

**概念检查**

以下字典中，忽略键后，值的类型是什么？

```py
{'MLB': {'Bluejays': [1992, 1993],

         'Orioles': [1966, 1970, 1983]},

 'NFL': {'Patriots': ['too many']}}
```

A. 整数

B. 字符串

C. 列表

D. 字典

E. 以上多个选项

答案：D。字典中每个键的值本身就是一个字典。例如，键 `'MLB'` 映射到一个字典；该字典有两个 `key:value` 键值对。

### 索引字典

我们可以使用方括号查找一个键对应的值。这与我们索引列表的方式类似，只不过键作为有效的“索引”：

```py
>>> d = {'cut':4, 'gully':3}

>>> d

{'cut': 4, 'gully': 3}

>>> d['cut']

4

>>> d['gully']

3
```

使用不存在的键是一个错误：

```py
>>> d['storm']

Traceback (most recent call last):

  File "<stdin>", line 1, in <module>

KeyError: 'storm'
```

我们可以通过首先使用`in`来检查一个键是否存在于字典中，从而防止出现错误。当`in`用于字典时，它只检查键，而不检查值。以下是我们如何在尝试查找其值之前检查键是否存在：

```py
>>> if 'cut' in d:

...     print(d['cut'])

...

4

>>> if 'storm' in d:

...     print(d['storm'])

...
```

索引和在字典上使用`in`是非常快速的操作。它们不需要搜索任何类型的列表，无论字典中有多少个键。

有时使用`get`方法而不是索引来查找键的值会更方便。即使键不存在，`get`方法也永远不会产生错误：

```py
>>> print(d.get('cut'))

4

>>> print(d.get('storm'))

None
```

如果键存在，`get`返回其值。否则，它返回`None`，表示该键不存在。

除了查找键的值，我们还可以使用方括号向字典添加键或更改键所对应的值。以下是一些代码，展示了如何做到这一点，从一个空字典开始：

```py
>>> d = {}

>>> d['gully'] = 1

>>> d

{'gully': 1}

>>> d['cut'] = 1

>>> d

{'gully': 1, 'cut': 1}

>>> d['cut'] = 4

>>> d

{'gully': 1, 'cut': 4}

>>> d['gully'] = d['gully'] + 1

>>> d

{'gully': 2, 'cut': 4}

>>> d['gully'] = d['gully'] + 1

>>> d

{'gully': 3, 'cut': 4}
```

**概念检查**

使用`help({}.get)`了解更多关于字典`get`方法的信息。

以下代码的输出是什么？

```py
d = {3: 4}

d[5] = d.get(4, 8)

d[4] = d.get(3, 9)

print(d)
```

A. `{3: 4, 5: 8, 4: 9}`

B. `{3: 4, 5: 8, 4: 4}`

C. `{3: 4, 5: 4, 4: 3}`

D. `get`引起的错误

答案：B。第一次调用`get`返回`8`，因为键`4`在字典中不存在。因此，这一行会添加键`5`，值为`8`。

第二次调用`get`返回`4`：键`3`已经存在于字典中，因此第二个参数`9`被忽略。因此，这一行会添加键`4`，值为`4`。

### 遍历字典

如果我们在字典上使用`for`循环，我们将得到字典的键：

```py
>>> d = {'cut': 4, 'gully': 3, 'storm': 2, 'brook': 2}

>>> for word in d:

...     print('a key is', word)

...

a key is cut

a key is gully

a key is storm

a key is brook
```

我们可能还希望访问与每个键相关联的值，我们可以通过使用每个键作为字典中的索引来做到这一点。下面是一个既访问键又访问其值的循环：

```py
>>> for word in d:

...     print('key', word, 'has value', d[word])

...

key cut has value 4

key gully has value 3

key storm has value 2

key brook has value 2
```

字典有一些方法，可以让我们访问键、值或两者。

`keys`方法给我们提供键，而`values`方法给我们提供值：

```py
>>> d.keys()

dict_keys(['cut', 'gully', 'storm', 'brook'])

>>> d.values()

dict_values([4, 3, 2, 2])
```

这些不是列表，但我们可以将它们传递给`list`以进行转换：

```py
>>> keys = list(d.keys())

>>> keys

['cut', 'gully', 'storm', 'brook']

>>> values = list(d.values())

>>> values

[4, 3, 2, 2]
```

有了作为列表的键，我们可以对这些键进行排序，然后按排序顺序遍历它们：

```py
>>> keys.sort()

>>> keys

['brook', 'cut', 'gully', 'storm']

>>> for word in keys:

...     print('key', word, 'has value', d[word])

...

key brook has value 2

key cut has value 4

key gully has value 3

key storm has value 2
```

我们还可以遍历值：

```py
>>> for num in d.values():

...     print('number', num)

...

number 4

number 3

number 2

number 2
```

遍历键通常比遍历值更受欢迎。因为从键到值很容易。然而，正如我们在下一小节中将看到的，从值回到键就没有那么容易了。

另一个相关的方法是`items`。它让我们可以访问键和值：

```py
>>> pairs = list(d.items())

>>> pairs

[('cut', 4), ('gully', 3), ('storm', 2), ('brook', 2)]
```

这为我们提供了另一种遍历字典`key:value`对的方法：

```py
>>> for pair in pairs:

...     print('key', pair[0], 'has value', pair[1])

...

key cut has value 4

key gully has value 3

key storm has value 2

key brook has value 2
```

仔细查看`pairs`的值：

```py
>>> pairs

[('cut', 4), ('gully', 3), ('storm', 2), ('brook', 2)]
```

这里有些可疑：每个内部值周围是圆括号，而不是方括号。事实证明，这不是一个列表的列表，而是一个元组的列表：

```py
>>> type(pairs[0])

<class 'tuple'>
```

元组与列表相似，它们存储一系列的值。元组和列表之间最重要的区别在于，元组是不可变的。你可以遍历元组、索引它、切片它，但你不能修改它。如果你试图修改元组，会发生错误：

```py
>>> pairs[0][0] = 'river'

Traceback (most recent call last):

  File "<stdin>", line 1, in <module>

TypeError: 'tuple' object does not support item assignment
```

你可以使用圆括号创建自己的元组。对于一个只有一个值的元组，我们需要一个尾随的逗号。对于多个值的元组，则不需要：

```py
>>> (4,)

(4,)

>>> (4, 5)

(4, 5)

>>> (4, 5, 6)

(4, 5, 6)
```

元组有一些方法——但只有少数几个，因为不允许改变元组的操作。如果你感兴趣，我鼓励你了解更多关于元组的知识，但在本书中我们不再使用元组。

### 反转字典

我们已经接近使用字典解决常见单词问题了。计划如下：我们维护一个字典，将单词映射到它们的出现次数。每次处理一个单词时，我们检查该单词是否已经存在于字典中。如果不存在，我们将它加入字典，并赋值为 1。如果存在，则将其值加 1。

这是一个例子，展示了如何添加两个单词，一个我们之前见过，另一个我们没见过：

```py
>>> d = {'storm': 1, 'cut': 1, 'magma': 1}

>>> word = 'cut'  # 'cut' is already in the dictionary

>>> if not word in d:

...     d[word] = 1

... else:

...     d[word] = d[word] + 1

...

>>> d

{'storm': 1, 'cut': 2, 'magma': 1}

>>> word = 'brook'  # 'brook' is not in the dictionary

>>> if not word in d:

...     d[word] = 1

... else:

...     d[word] = d[word] + 1

...

>>> d

{'storm': 1, 'cut': 2, 'magma': 1, 'brook': 1}
```

字典使得从键到值的查找变得容易。例如，给定键 `'brook'`，我们可以轻松查找其值 `1`：

```py
 >>> d['brook']

1
```

参考表格 8-1，这就像是从左列的单词到右列中它们的出现次数。这并不能直接告诉我们出现次数为特定数字的单词，然而。我们真正需要的是能够从右列反向查找，从出现次数到单词。这样我们就可以按出现次数从多到少排序，找到我们需要的单词。

也就是说，我们需要从这种类型的字典开始：

```py
{'storm': 2, 'cut': 4, 'magma': 1, 'brook': 2,

 'gully': 3, 'cliff': 1, 'blast': 1}
```

对于这种类型，*反转字典*：

```py
{2: ['storm', 'brook'], 4: ['cut'], 1: ['magma', 'cliff', 'blast'],

 3: ['gully']}
```

原始字典是从字符串映射到数字。反转后的字典则是从数字映射到字符串。嗯，其实不完全是：反转字典是从数字映射到*字符串的列表*。记住，字典中的每个键只允许出现一次。在反转字典中，我们需要将每个键映射到多个值，因此我们将所有这些值存储在一个列表中。

要反转字典，每个键变成一个值，每个值变成一个键。如果一个键在反转字典中尚不存在，我们就为其值创建一个列表。如果键已经存在于反转字典中，那么我们将其值添加到该键的列表中。

我们现在可以编写一个函数，返回一个字典的反转版本。查看列表 8-5 中的代码。

```py
def invert_dictionary(d):

    """

    d is a dictionary mapping strings to numbers.

    Return the inverted dictionary of d.

    """

    inverted = {}

 ❶ for key in d:

     ❷ num = d[key]

        if not num in inverted:

         ❸ inverted[num] = [key]

        else:

         ❹ inverted[num].append(key)

    return inverted
```

*列表 8-5：反转字典*

我们使用`for`循环遍历字典`d`❶，这会给我们每个键。我们索引`d`以获取该键对应的值❷。然后，我们将这个`key:value`对添加到反转字典中。如果`num`还不是反转字典中的一个键，那么我们就添加它，并将其映射到`d`中的相应键❸。如果`num`已经是反转字典中的一个键，那么它的值已经是一个列表。因此，我们可以使用`append`将`d`中的键作为另一个值添加❹。

将我们的`invert_dictionary`函数代码输入 Python shell 中，试试看：

```py
>>> d = {'a': 1, 'b': 1, 'c': 1}

>>> invert_dictionary(d)

{1: ['a', 'b', 'c']}

>>> d = {'storm': 2, 'cut': 4, 'magma': 1, 'brook': 2,

...      'gully': 3, 'cliff': 1, 'blast': 1}

>>> invert_dictionary(d)

{2: ['storm', 'brook'], 4: ['cut'], 1: ['magma', 'cliff', 'blast'],

 3: ['gully']}
```

现在我们准备通过反转字典来解决常见词问题。

### 解决问题

如果你想更多地练习自顶向下设计，或许可以在继续之前自行尝试解决这个问题。为了节省篇幅，我在这里不会遵循自顶向下设计的步骤，而是直接展示完整的解决方案，然后我们再讨论每个函数及其使用方法。

#### 代码

解决方案见清单 8-6。

```py
   def invert_dictionary(d):

       """

       d is a dictionary mapping strings to numbers.

       Return the inverted dictionary of d.

       """

       inverted = {}

       for key in d:

           num = d[key]

           if not num in inverted:

               inverted[num] = [key]

           else:

               inverted[num].append(key)

       return inverted

❶ def with_suffix(num):

       """

       num is an integer >= 1.

       Return a string of num with its suffix added; e.g. '5th'.

       """

    ❷ s = str(num)

    ❸ if s[-1] == '1' and s[-2:] != '11':

           return s + 'st'

       elif s[-1] == '2' and s[-2:] != '12':

           return s + 'nd'

       elif s[-1] == '3' and s[-2:] != '13':

           return s + 'rd'

       else:

           return s + 'th'

❹ def most_common_words(num_to_words, k):

       """

       num_to_words is a dictionary mapping number of occurrences to

           lists of words.

       k is an integer >= 1.

       Return a list of the kth most-common words in num_to_words.

       """

       nums = list(num_to_words.keys())

       nums.sort(reverse=True)

       total = 0

       i = 0

       done = False

    ❺ while i < len(nums) and not done:

           num = nums[i]

        ❻ if total + len(num_to_words[num]) >= k:

               done = True

           else:

               total = total + len(num_to_words[num])

               i = i + 1

    ❼ if total == k - 1 and i < len(nums):

           return num_to_words[nums[i]]

       else:

           return []

❽ n = int(input())

   for dataset in range(n):

       lst = input().split()

       m = int(lst[0])

       k = int(lst[1])

       word_to_num = {}

       for i in range(m):

           word = input()

           if not word in word_to_num:

               word_to_num[word] = 1

           else:

               word_to_num[word] = word_to_num[word] + 1

    ❾ num_to_words = invert_dictionary(word_to_num)

       ordinal = with_suffix(k)

       words = most_common_words(num_to_words, k)

       print(f'{ordinal} most common word(s):')

       for word in words:

           print(word)

       print()
```

*清单 8-6: 解决常见词问题*

第一个函数是`invert_dictionary`。我们在本章“反转字典”部分已经讨论过它。接下来我们将逐一讲解程序中的其他部分。

#### 添加后缀

`with_suffix`函数❶接受一个数字并返回添加了正确后缀的字符串。我们需要这个函数，是因为有一个恼人的要求：输出*k*时需要带上后缀。例如，如果*k* = 1，那么我们就必须生成这一行作为输出的一部分：

```py
1st most common word(s):
```

如果*k* = 2，我们将需要生成这行作为输出的一部分：

```py
2nd most common word(s):
```

依此类推。我们的`with_suffix`函数确保我们为数字添加正确的后缀。我们首先将数字转换为字符串❷，以便可以轻松访问其各个数字。然后，我们使用一系列测试来判断后缀是`st`、`nd`、`rd`还是`th`。例如，如果最后一位数字是`1`，但最后两位数字不是`11`❸，那么正确的后缀是`st`。这会给我们`1st`、`21st`和`31st`，但不会是`11st`（这是错误的）。

#### 查找第 k 个最常见的单词

`most_common_words`函数❹是实际找到我们需要的单词的函数。它接受一个反转字典（该字典将出现次数映射到单词列表）和一个整数`k`，并返回出现次数第`k`多的单词列表。

为了查看它是如何工作的，让我们看一个反转字典的示例。我已经将它的键按出现次数从多到少排序，因为`most_common_words`就是按照这个顺序遍历键的。以下是字典：

```py
{4: ['cut'],

 3: ['gully'],

 2: ['storm', 'brook'],

 1: ['magma', 'cliff', 'blast']}
```

假设`k`是`3`。因此，恰好必须有两个单词比我们返回的单词更常见。我们需要的单词并没有由第一个字典键提供。那个键只给我们一个单词（`cut`），所以它不能是第三常见的单词。同样，我们需要的单词也没有由第二个字典键提供。那个键给我们另一个单词（`gully`）。到目前为止，我们已经处理了两个单词，但还没有找到第三常见的单词。然而，我们需要的单词*确实*由第三个字典键提供。那个键给我们两个更多的单词；每个单词（`storm`和`brook`）都有恰好两个比它们更常见的单词，因此这些就是当`k`为`3`时的单词。

如果`k`是`4`呢？这次，恰好必须有三个单词比我们返回的单词更常见。候选单词仍然是来自第三个键（`storm`和`brook`），但是只有两个单词比这两个单词出现得更频繁。因此，当`k = 4`时，*没有*单词。

总结来说，我们需要在遍历键时统计我们看到的单词，直到找到可能包含我们需要的单词的键。如果恰好有`k - 1`个单词出现得更频繁，那么我们就有`k`的单词；否则，我们没有要输出的单词。

现在让我们来看看代码本身。我们首先获取字典的键的列表，并按从大到小的顺序对它们进行排序。然后我们以该倒排顺序❺遍历这些键。`done`变量告诉我们是否已经查看过`k`个或更多的单词。一旦我们有了❻，我们就退出循环。

当循环完成时，我们检查是否有任何单词满足`k`。如果恰好有`k - 1`个单词出现得更频繁，并且我们没有超过键的末尾❼，那么我们确实有要返回的单词。否则，没有单词可以返回，所以我们返回空列表。

#### 主程序

现在我们来到了程序的主要部分❽。我们构建了字典`word_to_num`，它将每个单词映射到它的出现次数。接着我们构建了反向字典`num_to_words`❾，它将每个出现次数映射到相应的单词列表。注意这些字典的名称如何传达映射的方向：`word_to_num`从单词到数字，`num_to_words`从数字到单词。

剩下的代码调用了我们其他的辅助函数，并输出了相应的单词。

有了这些，你就准备好提交给评审了。干得好：这是你用字典解决的第一个问题。每当你需要在两种类型的值之间进行映射时，思考一下是否可以使用字典来组织信息。如果可以，很可能你已经走上了高效解决方案的道路！

### 问题 #20：城市与州

这里是另一个我们能够使用字典的问题。当你阅读问题描述时，思考一下我们可以用什么作为键，什么可以作为值。

这是 USACO 2016 年 12 月银奖比赛的题目——城市与州。

#### 挑战

美国被划分为称为*州*的地理区域，每个州包含一个或多个城市。每个州都有一个两字符的缩写。例如，宾夕法尼亚州的缩写是 PA，南卡罗来纳州的缩写是 SC。我们将以全大写字母书写城市名称和州的缩写。

考虑城市对`SCRANTON PA`和`PARKER SC`。这对城市是*特殊的*，因为每个城市的前两个字符代表另一个城市的州的缩写。也就是说，`SCRANTON`的前两个字符给我们提供了 SC（`PARKER`的州），而`PARKER`的前两个字符给我们提供了 PA（`SCRANTON`的州）。

如果一对城市满足这个条件且不在同一州，则该对城市为*特殊的*。

确定提供的输入中有多少个特殊的城市对。

#### 输入

从名为*citystate.in*的文件中读取输入。

输入包含以下几行：

+   一行包含*n*，表示城市的数量。*n*的范围是 1 到 200,000 之间。

+   *n*行，每行一个城市。每行给出一个城市的名称（全大写），一个空格，及其州的缩写（全大写）。每个城市的名称长度在 2 到 10 个字符之间；每个州的缩写恰好为两个字符。相同的城市名称可以出现在多个州中，但在同一州中不会重复出现。该问题中城市或州的名称可以是符合这些要求的任何字符串；它可能不是一个实际存在的美国城市或州的名称。

#### 输出

将输出写入名为*citystate.out*的文件中。

输出特殊城市对的数量。

每个测试用例的时间限制为四秒。

### 探索一个测试用例

也许你认为可以用一个列表来解决这个问题。这是一个不错的想法！如果你有兴趣，在继续之前我建议先尝试这个方法。策略是使用两个嵌套循环来考虑每一对城市，并检查每一对是否特殊。使用这种方法是有可能得到正确解决方案的。

一个正确的解决方案，没错，但它也很慢。城市的列表可能非常庞大——最多可达 200,000 个——任何涉及在列表中查找匹配城市的解决方案注定会太慢。让我们探索一个测试用例，看看字典如何帮助解决这个问题。

这是我们的测试用例：

```py
12

SCRANTON PA

MANISTEE MI

NASHUA NH

PARKER SC

LAFAYETTE CO

WASHOUGAL WA

MIDDLEBOROUGH MA

MADISON MI

MILFORD MA

MIDDLETON MA

COVINGTON LA

LAKEWOOD CO
```

第一个城市是`SCRANTON PA`。要找到与这个城市相关的特殊城市对，我们需要找到其他以`PA`开头且州为`SC`的城市。符合此描述的唯一其他城市是`PARKER SC`。

请注意，对于`SCRANTON PA`，我们关心的是它的名称以`SC`开头，且其州为`PA`。它也可以被称为`SCMERWIN PA`、`SCSHOCK PA`或`SCHRUTE PA`，但它仍然会与`PARKER SC`形成特殊对。

我们将城市名称的前两个字符与城市所在州的简称组合称为*组合*（combo）。例如，`SCRANTON PA`的组合是`SCPA`，而`PARKER SC`的组合是`PASC`。

我们可以不再搜索特殊城市对，而是寻找特殊组合对。让我们试试看。

有两个城市的组合是`MAMI`。它们分别是`MANISTEE MI`和`MADISON MI`，但我们关心的是它们的数量是两个。`MAMI`城市以`MA`开头，且位于`MI`州。为了统计涉及`MAMI`城市的特殊城市对，我们需要知道那些以`MI`开头、且位于`MA`州的城市。也就是说，我们需要知道`MIMA`城市的数量。共有三个`MIMA`城市。它们分别是`MIDDLEBOROUGH MA`，`MILFORD MA`，和`MIDDLETON MA`，但我们关心的是它们的数量是三个。好——所以我们有两个`MAMI`城市和三个`MIMA`城市。因而，这些组合的总特殊城市对为 2 * 3 = 6，因为对于每一个`MAMI`城市，我们都有三种选择的`MIMA`城市。

如果你还不信服，以下是这些组合的六对特殊城市：

+   `MANISTEE MI` 和 `MIDDLEBOROUGH MA`

+   `MANISTEE MI` 和 `MILFORD MA`

+   `MANISTEE MI` 和 `MIDDLETON MA`

+   `MADISON MI` 和 `MIDDLEBOROUGH MA`

+   `MADISON MI` 和 `MILFORD MA`

+   `MADISON MI` 和 `MIDDLETON MA`

如果我们能够将组合——`SCPA`，`PASC`，`MAMI`，`MIMA`等——映射到出现次数，我们就可以遍历这些组合来找出特殊城市对的数量。字典是存储这种映射的完美工具。

这是我们希望为测试用例创建的字典：

```py
{'SCPA': 1, 'MAMI': 2, 'NANH': 1, 'PASC': 1, 'LACO': 2,

 'MIMA': 3, 'COLA': 1}
```

使用这个字典，我们可以计算出特殊城市对的数量。让我们一步步做这个过程。

第一个键是`'SCPA'`；它的值是`1`。为了查找涉及`'SCPA'`的特殊城市对，我们需要查找`'PASC'`的值。该值也是`1`。我们将这两个值相乘，得到 1 * 1 = 1 对涉及这些组合的特殊城市。我们需要对字典中的其他每个键执行相同的操作。

下一个键是`'MAMI'`；它的值是`2`。为了查找涉及`'MAMI'`的特殊城市对，我们需要查找`'MIMA'`的值。该值是`3`。我们将这两个值相乘，得到 2 * 3 = 6 对涉及这些组合的特殊城市。加上之前找到的 1，我们现在一共有 7 对。

下一个键是`'NANH'`；它的值是`1`。为了查找涉及`'NANH'`的特殊城市对，我们需要查找`'NHNA'`的值。但是`'NHNA'`不是字典中的键！因此没有涉及这些组合的特殊城市对。我们仍然总共有 7 对。

请注意接下来的这一点。下一个关键字是 `'PASC'`；它的值是 `1`。要找到涉及 `'PASC'` 的特殊城市配对，我们需要查找 `'SCPA'` 的值。它的值也是 `1`。我们将这两个值相乘，得到 1 * 1 = 1 对涉及这些组合的特殊城市配对。但是等一下：我们在处理 `'SCPA'` 关键字时已经计算过这对配对。如果我们再加 1，那么我们会重复计算这对配对。事实上，通过处理每个关键字，我们将重复计算*每一对*特殊城市配对。别担心：当我们准备输出最终答案时，我们会做调整。我们就先把这个 1 加进去。再加上之前找到的 7 个，我们现在总共有 8 个。

下一个关键字是 `'LACO'`；它的值是 `2`。`'COLA'` 的值是 `1`，因此这两个组合涉及的城市特殊配对数量为 2 * 1 = 2。再加上我们之前找到的 8 个，现在总共有 10 个。

还有两个关键字，`'MIMA'` 和 `'COLA'`。第一个让我们在总数上加上 6，第二个让我们加上 2。加上之前找到的 10，现在总共有 18 个。

记住，我们已经重复计算了每一对特殊城市配对。因此，我们并不拥有 18 对独特的特殊城市配对。我们只有 18 / 2 = 9 对特殊城市配对。我们只需要除以 2 来消除重复计算的结果。

如果你将我们刚才分析的字典与测试用例中的城市进行比较，你会发现字典中缺少了一些内容。那就是城市 `WASHOUGAL WA`！它的组合是 `WAWA`，但在我们的字典中并没有 `'WAWA'` 这个关键字。我们没有考虑到这个城市，需要弄清楚为什么。

`WASHOUGAL WA` 的前两个字符是 `WA`。这意味着 `WASHOUGAL WA` 要成为特殊配对城市的一部分，唯一的方法是找到另一个州为 `WA` 的城市。请注意，`WASHOUGAL WA` 也位于 `WA` 州。然而，问题规定，特殊配对城市的两个城市必须来自不同的州。因此，无法找到涉及 `WASHOUGAL WA` 的特殊配对城市。为了确保我们不会错误地计算伪特殊配对，我们甚至不会将 `WASHOUGAL WA` 包含在字典中。

### 解决问题

我们准备好了！我们可以使用字典来快速且简洁地解决城市和州问题。代码见 清单 8-7。

```py
   input_file = open('citystate.in', 'r')

   output_file = open('citystate.out', 'w')

   n = int(input_file.readline())

❶ combo_to_num = {}

   for i in range(n):

       lst = input_file.readline().split()

    ❷ city = lst[0][:2]

       state = lst[1]

    ❸ if city != state:

           combo = city + state

           if not combo in combo_to_num:

               combo_to_num[combo] = 1

           else:

               combo_to_num[combo] = combo_to_num[combo] + 1

   total = 0

❹ for combo in combo_to_num:

    ❺ other_combo = combo[2:] + combo[:2]

       if other_combo in combo_to_num:

        ❻ total = total + combo_to_num[combo] * combo_to_num[other_combo]

❼ output_file.write(str(total // 2) + '\n')

   input_file.close()

   output_file.close()
```

*清单 8-7：解决城市和州问题*

这是一个 USACO 问题，需要使用文件而不是标准输入和标准输出。

我们将构建的字典叫做 `combo_to_num` ❶。它将四个字符的组合，如 `'SCPA'`，映射到具有该组合的城市数量。

对于输入中的每个城市，我们使用变量来表示城市名称的前两个字符 ❷ 以及它的州。然后，如果这些值不同 ❸，我们将它们组合并将组合添加到字典中。如果该组合尚不存在于字典中，我们将其添加并赋值为 1；如果已存在，则将其值加 1。

字典现在已经建立。我们遍历它的键 ❹。对于每个键，我们构造需要查找的另一组，以找到涉及该键的特殊城市对。例如，如果键是`'SCPA'`，那么我们希望另一组是`'PASC'`。为此，我们取键的最右两个字符，然后跟随左侧两个字符 ❺。如果另一组也在字典中，那么我们将两个键的值相乘并添加到我们的总数 ❻。

现在，我们只需将特殊城市对的总数输出到输出文件中。如前一节所述，我们需要将总数除以 2 ❼，以消除处理字典中每个键时导致的重复计数。

就是这样：用适当的字典部署解决问题的另一个例子。随时提交我们的代码！

### 摘要

在本章中，我们学习了 Python 集合和字典。集合是一组没有顺序且没有重复值的数据。字典是一组`键:值`对。正如我们在本章的问题中看到的那样，有时这些集合比列表更合适。例如，确定值是否在集合中的操作比在列表上进行同样的操作要快得多。如果我们不关心值的顺序或想要消除重复值，我们应该认真考虑使用集合。

同样地，字典使得确定由键映射到的值变得容易。如果我们在维护从键到值的映射，那么我们应该认真考虑使用字典。

在集合和字典混合使用的情况下，你现在有更多的灵活性来存储你的值。然而，这种灵活性意味着你需要做出选择。不要再默认使用列表！使用一种类型还是另一种类型可能是解决问题与否的关键。

我们已经达到了一个重要的里程碑，因为我们现在已经涵盖了本书中我将教给你的大部分 Python 知识。这并不意味着你的 Python 之旅结束了。关于 Python，书中未包含的内容还有很多。不过，这意味着我们已经达到了一个可以用我们的 Python 技能解决各种问题的地步，无论是在竞技编程中还是其他方面。

在书的下一章中，我们将换个角度：从学习新的 Python 特性转向提升我们的问题解决能力。我们将专注于通过搜索所有候选解决方案来解决一类特定的问题。

### 章节练习

这里有一些练习供你尝试。对于每个练习，使用一个集合或字典。有时，使用集合或字典可以帮助你编写运行更快的代码；其他时候，它可以帮助你编写更有组织、更易于阅读的代码。

1.  DMOJ 问题 `crci06p1`，巴德

1.  DMOJ 问题 `dmopc19c5p1`，显眼的神秘清单

1.  DMOJ 问题 `coci15c2p1`，Marko

1.  DMOJ 问题 `ccc06s2`，密文攻击

1.  DMOJ 问题 `dmopc19c3p1`，找众数

1.  DMOJ 问题 `coci14c2p2`，Utrka（尝试通过三种不同方式解决此问题：使用字典、使用集合和使用列表！）

1.  DMOJ 问题 `coci17c2p2`，ZigZag（提示：维护两个字典。第一个字典将每个起始字母映射到其单词列表；第二个字典将每个起始字母映射到其下一个将要输出的单词的索引。这样，我们可以循环遍历每个字母的单词，而无需显式更新出现次数或修改列表。）

### 备注

Email Addresses 最初来自 2019 年安大略省教育计算组织编程竞赛第二轮。Common Words 最初来自 1999 年加拿大计算奥林匹克竞赛。Cities and States 最初来自 2016 年美国计算机奥林匹克银奖竞赛。

如果你想深入了解 Python，我推荐*Python Crash Course*，第二版，由 Eric Matthes 编写（No Starch Press，2019）。当你准备好提升到下一个层次时，你可能会喜欢阅读*Effective Python*，第二版，由 Brett Slatkin 编写（Addison-Wesley Professional，2020），它提供了一些技巧，帮助你编写更好的 Python 代码。
