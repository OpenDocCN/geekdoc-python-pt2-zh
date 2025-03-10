## 第七章：优先队列与堆

![](img/chapterart.png)

*优先队列*是一类根据每个项的给定分数来检索排序项的数据结构。而在第四章中，栈和队列仅依赖于数据插入的顺序，优先队列则使用额外的信息来确定检索顺序——项的优先级。正如我们将看到的，这一新信息使我们能够进一步适应数据，并且在许多有用的应用中，比如优先处理紧急请求，发挥重要作用。

举个例子，假设你所在的社区开了一家新的咖啡店——动态选择咖啡店。兴奋不已的你走进店里，看到有 10 款你从未尝试过的咖啡豆。在投入尝试之前，你花了一个小时仔细研究每个新品牌的相对优缺点，根据它们那份极其简陋的菜单描述，列出了一份排名咖啡清单。你从榜单顶部选择了最有前景的品牌，购买了它，并带回家享受这次体验。

第二天，你又回到动态选择咖啡店，准备尝试你列表中的第二款咖啡，却发现他们在菜单上又新增了两款咖啡。当你问咖啡师为什么做出这个改变时，他们指向店里的招牌，解释说动态选择咖啡店提供的是一个不断扩展的咖啡选择，目标是最终提供超过一千种咖啡。你既兴奋又害怕，每天你都需要优先排序这些新咖啡，并将它们插入到你的列表中，这样你就知道下一次该尝试哪款咖啡。

从优先列表中检索项的任务在计算机程序中经常出现：给定一个项列表及其相关的优先级，如何高效地按优先顺序检索下一个项？通常，我们需要在一个动态的环境中进行检索，因为新的项不断加入。我们可能需要根据优先级选择处理哪个网络数据包，基于常见拼写错误提供拼写检查的最佳建议，或者在最佳优先搜索中选择下一个选项。在现实生活中，我们可能会使用自己的心理优先队列来决定下一个要执行的紧急任务、观看哪部电影，或者在拥挤的急诊室中首先看哪位病人。一旦你开始留意，优先检索随处可见。

在本章中，我们介绍了优先队列，这是一类用于从集合中检索优先项的数据结构，然后讨论了实现这一有用工具的最常见数据结构：堆。堆使得优先队列的核心操作变得非常高效。

## 优先队列

优先级队列存储一组项目，并使用户能够轻松检索具有最高优先级的项目。它们是动态的，允许插入和检索操作交替进行。我们需要能够从我们的优先任务列表中添加和删除项目。如果我们被迫使用固定的数据结构，作者可能会花一整天查看他那静态的列表，并一遍又一遍地执行“去拿早晨的咖啡”这一最高优先级任务。如果在完成任务后无法将其移除，它将一直留在作者的列表顶部。虽然这可能让人享受一天的时光，但不太可能是高效的一天。

在最基本的形式下，优先级队列支持几个主要操作：

+   添加一个项目及其相关的优先级分数。

+   查找具有最高优先级的项目（如果队列为空，则为 null）。

+   移除具有最高优先级的项目（如果队列为空，则为 null）。

我们还可以添加其他有用的功能，允许我们检查优先级队列是否为空，或者返回当前存储的项目数量。

我们根据当前的问题设定项目的优先级。在某些情况下，优先级值可能很明显，或者由算法决定。例如，在处理网络请求时，每个数据包可能带有显式的优先级，或者我们可能选择首先处理最旧的请求。然而，决定优先处理什么并不总是一个简单的任务。当我们优先考虑尝试哪种品牌的咖啡时，我们可能想根据价格、可用性或咖啡因含量来创建优先级——这取决于我们打算如何使用我们的优先级队列。

使用像排序链表或排序数组这样的原始数据结构实现优先级队列是可能的，但并不理想，我们需要根据优先级将新项目添加到列表中。图 7-1 展示了将值 21 添加到排序链表中的示例。

![一个链表，在向列表中间添加第八个元素后，列表的值按降序排列。列表的头部值为 50，列表的尾部值为 9。值 21 插入在值 28 和值 15 之间。](img/f07001.png)

图 7-1：将元素（21）添加到表示优先级队列的排序链表中

排序链表将最高优先级的项目保持在列表的前端，以便于查找。实际上，在这种情况下，查找只需要常数时间，无论优先级队列的长度如何——我们只需查看第一个元素。不幸的是，添加新元素可能是昂贵的。每次我们添加一个新项目时，可能需要遍历整个列表，这会花费与优先级队列长度成正比的时间。

作者使用这种排序列表的方法来整理冰箱，将物品按过期日期从前到后存储。最靠近的物品始终具有最高的优先级，即将要最先过期的物品。取出正确的物品非常简单——只需拿出最前面的那个。这种方案对于存储牛奶或咖啡用的奶油特别有利：没有人愿意在早晨昏昏欲睡时查看过期日期。然而，将新物品放在旧物品后面可能需要时间，并且需要烦人的移动。

类似地，我们可以在 *未排序* 的链表或数组中维护优先队列。新的元素添加非常简单——只需将元素标记到列表的末尾，如图 7-2 所示。

![未排序数组在添加第八个元素之前和之后的状态。值 21 被添加到值 39 后面，成为未排序数组的末尾。](img/f07002.png)

图 7-2：将元素（21）添加到表示优先队列的未排序数组中

不幸的是，我们现在在查找下一个元素时需要付出很高的代价。我们必须扫描整个列表，以确定哪个元素具有最高的优先级。如果我们要删除该元素，还需要将其他元素移动，以填补空缺。这种方法就像作者在冰箱里扫描所有物品，以找到哪个最接近过期。这对于冰箱里只有几盒牛奶的情况或许有效，但想象一下在大型超市里逐一检查每个食物或饮料的成本。

将优先队列实现为排序列表可能比使用未排序列表更有效，反之亦然，这取决于我们打算如何使用优先队列。如果添加操作比查找操作更常见，我们会偏好使用未排序列表。如果查找操作更常见，我们则需要承受保持元素排序的成本。在冰箱的例子中，查找操作要常见得多——我们取出一盒牛奶使用的频率远高于购买新盒牛奶的频率，因此保持牛奶的排序是有益的。问题出现在当添加操作和查找操作都很常见时；优先处理一个操作会导致整体效率低下，因此我们需要一种平衡两者成本的方法。一个巧妙的数据结构——堆，可以帮助我们解决这个问题。

## 最大堆

*最大堆* 是二叉树的一种变体，它在节点和其子节点之间保持一种特殊的有序关系。具体来说，最大堆根据 *最大堆性质* 存储元素，该性质规定树中任何节点的值都大于或等于其子节点的值。为了简便起见，接下来的章节中我们将经常使用更一般的术语 *堆* 和 *堆性质* 来指代最大堆和最大堆性质。

图 7-3 显示了一个按照最大堆性质组织的二叉树的表示。

![堆的二叉树从根节点（99）开始，根节点有两个子节点 67 和 97。对于任何节点，左右子节点的值都小于或等于节点本身的值。](img/f07003.png)

图 7-3：堆作为二叉树的表示

左右子节点之间没有特殊的优先级或顺序，除了它们的优先级低于父节点。为了做个比较，想象一个精英咖啡爱好者的导师计划——咖啡相关知识提升协会。每个成员（节点）同意指导最多两个其他咖啡爱好者（子节点）。唯一的条件是，每个受导师指导的人必须比导师了解的咖啡知识少——否则，导师计划将毫无意义。

计算机科学家 J. W. J. Williams 最初发明堆作为新排序算法堆排序的一部分，我们将在本章后面讨论。然而，他意识到堆作为数据结构在其他任务中也很有用。最大堆的简单结构使得它能够高效地支持优先队列所需的操作：（1）允许用户高效查找最大元素，（2）删除最大元素，以及（3）添加任意元素。

堆通常被可视化为树形结构，但为了提高效率，它们通常使用数组实现。在本章中，我们并行展示这两种表示方法，以帮助读者在它们之间建立心理联系。然而，使用数组进行实现并不是必须的。

在基于数组的实现中，数组中的每个元素对应树中的一个节点，根节点位于索引 1（我们跳过索引 0，以保持与堆的常见约定一致）。子节点的索引相对于其父节点的索引定义，因此位于索引*i*的节点的子节点位于索引 2*i*和 2*i* + 1。例如，索引为 2 的节点有两个子节点，分别位于索引 2 × 2 = 4 和索引 2 × 2 + 1 = 5，如图 7-4 所示。

![堆同时以数组和树的形式表示，箭头指示每个节点在数组中的位置。根节点 98 对应数组中的第一个元素。节点的两个子节点，95 和 50，是数组中的第二个和第三个元素。](img/f07004.png)

图 7-4：堆的位置对应于索引位置。

类似地，我们将一个节点的父节点的索引计算为`Floor(i/2)`。这种索引方案使得算法可以轻松地根据父节点的索引计算子节点的索引，反之亦然。

根节点始终对应最大堆中的最大值。由于我们将根节点存储在数组中的固定位置（索引 = 1），我们可以始终在常数时间内找到这个最大值。这只是一个数组查找。数据本身的布局因此解决了优先队列所需的一个操作。

由于我们将在优先队列中添加和移除任意元素，为了避免不断调整数组大小，我们希望预先分配一个足够大的数组来容纳我们预计要添加的元素数量。记住在第三章中提到的，动态调整数组大小可能非常昂贵，它迫使我们创建一个新的数组并复制数据，这样做会浪费堆的宝贵效率。相反，我们可以最初分配一个较大的数组，跟踪数组中最后一个填充元素的索引，并将该索引称为数组的虚拟末尾。这使得我们只需要通过更新最后一个元素的索引来添加新元素。

```py
Heap {
    Array: array
    Integer: array_size
    Integer: last_index
}
```

当然，这种整体分配的代价是，如果我们的堆没有像预期那样增长，就可能会有一部分未使用的内存。

使用数组来表示基于树的数据结构本身就是一个有趣的步骤。我们可以使用打包数组和数学映射来表示堆，而不是依赖指针，这样可以减少堆的内存占用。通过维护节点索引与其子节点之间的映射关系，我们可以在不使用指针的情况下重建树形数据结构。正如我们下面将看到的那样，基于数组的表示对于堆来说是可行的，因为数据结构始终保持着几乎完整和平衡的树结构。这就产生了一个没有空隙的打包数组。虽然我们也可以使用相同的数组表示其他类型的树，如二叉搜索树，但这些数据结构通常会在整个结构中产生空隙，因此需要非常大的（并且可能大部分是空的）数组来存储深分支的树。

### 向堆中添加元素

当向堆中添加新元素时，我们必须确保结构保持堆的性质。就像你不会让一位装饰性的将军去报告给一位新任的中尉一样，你也不会把一个高优先级的堆节点放在低优先级的节点下方。我们必须将新元素添加到堆的树结构中，以确保所有新增元素下方的节点优先级都小于或等于新节点的优先级。同样，所有新节点上方的节点应当具有大于或等于新节点优先级的优先级。

堆的数组实现的亮点之一是，它在以打包数组存储节点的同时保持了这个特性。在之前的章节中，将节点添加到数组的中间是昂贵的，需要我们将后面的元素向后移动。幸运的是，每次向堆中添加新元素时，我们不需要付出这种线性代价。相反，我们通过首先破坏堆的性质，再沿着树的单一路径交换元素来恢复堆的性质。

换句话说，向堆中添加一个新元素时，我们将它添加到树底层的第一个空位置。如果这个新值大于其父节点的值，我们就将其上浮，直到它小于或等于其父节点，从而恢复堆的性质。堆本身的结构使我们能够高效地完成这个操作。在堆的数组实现中，这对应于将新元素添加到数组的末尾，并将其向前交换。

考虑 图 7-5 中的示例，该图展示了堆的结构，分别以数组和树的形式展示了每个步骤。 图 7-5(a) 显示了新元素添加之前的堆结构。在 图 7-5(b) 中，我们将新元素 85 添加到数组的末尾，相当于将其插入到树的底部。在 图 7-5(c) 中的第一次比较之后，由于 85 大于 50，我们交换了新元素与其父节点的位置。交换操作如 图 7-5(d) 所示。第二次比较发生在 图 7-5(e) 中，此时新节点已处于正确的位置：98 大于 85，因此不需要再与父节点交换。 图 7-5(f) 显示了添加操作完成后的堆结构。

![我们从图 7-4 中的数组开始。节点 85 被添加到数组的末尾，然后上浮到正确的位置。我们首先将 85 与它的父节点 50 进行比较，并交换它们。在下一步中，我们将 85 与它的新父节点 98 进行比较，并保持顺序。](img/f07005.png)

图 7-5：向堆中添加一个元素（85）

实现此添加操作的代码使用了一个单一的`WHILE`循环，逐层向上遍历堆，直到到达根节点或找到一个父节点的值大于或等于新节点的值：

```py
HeapInsert(Heap: heap, Type: value):
  ❶ IF heap.last_index == heap.array_size - 1:
        Increase Heap size.

 ❷ heap.last_index = heap.last_index + 1
    heap.array[heap.last_index] = value

    # Swap the new node up the heap.
  ❸ Integer: current = heap.last_index
    Integer: parent = Floor(current / 2)
  ❹ WHILE parent >= 1 AND (heap.array[parent] < 
                           heap.array[current]):
      ❺ Type: temp = heap.array[parent]
        heap.array[parent] = heap.array[current]
        heap.array[current] = temp
        current = parent
        parent = Floor(current / 2)
```

由于我们使用数组来存储堆，代码首先会检查数组中是否还有空间来添加新元素 ❶。如果没有空间，它会增加堆的大小，可能通过应用第三章中描述的数组扩展技术。接下来，代码将新元素添加到数组的末尾，并更新最后一个元素的位置 ❷。`WHILE` 循环从新添加的元素 ❸ 开始，逐层向上遍历堆，通过将当前值与其父节点的值进行比较 ❹，并在必要时交换 ❺。当我们到达堆的顶部（`parent == 0`）或找到一个大于或等于子节点的父节点时，循环终止。

我们可以将这一过程比作一个奇特设计但高效的包裹分配中心，如图 7-6 所示。员工们将包裹按照堆的属性整齐地排列在地板上：最前排是一个具有最高优先级的包裹，即下一个要发货的包裹。在它后面坐着两个优先级较低的包裹。在这两个包裹后面分别是两个更多的包裹（这一排总共四个），每一对包裹的优先级都小于或等于其前面的包裹。每增加一排，包裹的数量就翻倍，因此，随着你向仓库后部移动，排列越来越宽广。每个包裹最多有两个优先级较低或相等的包裹坐在它的后面，最多有一个优先级较高或相等的包裹在它前面。仓库地板上的矩形标记帮助指示每一排中包裹的可能位置。

![一张图显示了仓库地板上的一排排包裹，第一排一个包裹，第二排两个，第三排四个，第四排八个。第五排是最后一排，但没有完全填满。](img/f07006.png)

图 7-6：一个组织成堆的仓库地板

新的包裹从仓库后部运入。每个送货员都会嘀咕一些关于奇怪排序方案的话，松了一口气，因为至少他们不用将包裹搬到最前面，便将包裹放在最前面可用的位置，然后尽可能快地离开。随后，仓库员工开始行动，将新包裹与前面紧挨着的包裹进行优先级比较（忽略这一排中的其他包裹）。如果他们发现优先级倒置，就交换这两个包裹。否则，包裹就保持原位。这一过程持续进行，直到新包裹占据了正确的层级位置，而其前方的包裹具有更高或相等的优先级。由于包裹又重又分散，员工们通过每排最多进行一次比较和交换来最小化工作量。他们绝不会在一排内重新排列包裹。毕竟，没人愿意不必要地搬运箱子。

直观地，我们可以看到堆的新增操作并不非常昂贵。在最坏的情况下，我们可能需要将新节点交换到树的根部，但这只意味着交换数组中一小部分的值。根据设计，堆是*平衡的*二叉树：我们在插入下一个节点之前，会先填满树的每一层。由于满二叉树中每一层的节点数都是上一层的两倍，因此，新增操作在最坏情况下需要 log2 次交换。这比维护一个排序列表所需的最坏情况的 *N* 次交换要好得多。

### 从堆中移除最高优先级的元素

从优先队列中查找并移除最高优先级元素是一个核心操作，它允许我们按优先级顺序处理项目。也许我们正在存储一组待处理的网络请求，想要处理其中优先级最高的一个。或者我们可能在急诊室工作，想查看最紧急的病人。在这两种情况下，我们都希望从优先队列中移除该元素，以便提取下一个最高优先级的元素。

要移除最高优先级节点，我们必须首先打破，然后恢复堆的性质。请参阅图 7-7 中的示例。我们首先将最高优先级节点与树的最低层最后一个节点交换位置（见图 7-7(b)），有效地将最后一个元素作为新的根节点。然而，这种对新根节点的“史诗般的提升”几乎可以保证无法经得起检验。在数组实现中，这对应于交换数组中的第一个和最后一个元素。这次交换填补了通过移除第一个元素而在数组前部创建的空缺，从而保持了数组的紧凑结构。

接下来，原始的最大值 98（当前是树中的最后一个元素）被删除，如图 7-7(c)所示。我们现在已删除正确的节点，但在此过程中可能已经破坏了堆的性质。我们可能将一个低优先级的包裹移到了仓库的前端。

![节点 98 通过先与最后一个元素（23）交换位置被从堆中移除。新的根节点通过与其两个子节点中较大的一个交换位置，向下冒泡。第一步中，节点 23 与 95 和 50 进行比较，然后与 95 交换位置。](img/f07007.png)

图 7-7：从堆中移除最高优先级元素

为了修复堆的性质，我们从新的（不正确的）根节点 23 开始，沿着树向下走，恢复每一层的堆性质。被错误放置的包裹会逐层向仓库后端移动，与新包裹逐层向前移动的过程相反。诚然，这个遍历过程没有像将紧急包裹移到队列前端那么令人激动，但这是为了堆结构的健康。在每一层，我们将正在移动的包裹的优先级与其两个子节点（即下一层的两个包裹）进行比较（见图 7-7(d)）。如果它的优先级小于其中任何一个子节点的优先级，我们就将新的根节点向后移动，通过与两个子节点中较大的一个交换位置来恢复堆的性质（见图 7-7(e)）。这代表着包裹的高优先级继任者向前移动，取而代之。

向下交换操作会在没有更大的子节点时终止。图 7-7(f)展示了当当前节点处于正确位置时的比较结果。堆的性质已经恢复，所有节点都对其相对位置感到满意。图 7-7(g)展示了删除操作完成后最终的堆。

在固定根节点的位置时，我们沿着树的单一路径向下遍历，只检查并恢复我们进行交换的后代分支的堆属性。无需检查其他分支。因为我们没有做任何事情破坏堆属性，所以其他分支已经保证保持堆属性。这意味着，在最坏的情况下，我们需要进行 log2 次交换。

下面是删除最大元素的代码：

```py
HeapRemoveMax(Heap: heap):
  ❶ IF heap.last_index == 0:
        return null

    # Swap out the root for the last element and shrink heap.  
  ❷ Type: result = heap.array[1]
    heap.array[1] = heap.array[heap.last_index]
    heap.array[heap.last_index] = null
    heap.last_index = heap.last_index - 1

    # Bubble the new root down.
    Integer: i = 1
  ❸ WHILE i <= heap.last_index:
        Integer: swap = i
      ❹ IF 2*i <= heap.last_index AND (heap.array[swap] < 
                                       heap.array[2*i]):
            swap = 2*i
      ❺ IF 2*i+1 <= heap.last_index AND (heap.array[swap] <
                                         heap.array[2*i+1]):
            swap = 2*i+1

      ❻ IF i != swap:
            Type: temp = heap.array[i]
            heap.array[i] = heap.array[swap]
            heap.array[swap] = temp
            i = swap
        ELSE:
            break
    return result
```

这段代码首先检查堆是否为空❶。如果堆为空，则什么也不返回。然后，代码将第一个元素（`index == 1`）与最后一个元素（`index == heap.last_index`）交换，破坏堆的属性，为移除最大元素做准备❷。接着，代码使用`WHILE`循环通过一系列比较遍历堆，修复堆的属性❸。在每次迭代中，它将当前值与两个子节点进行比较，并在必要时与较大的子节点交换❻。我们必须添加额外的检查❹❺，确保代码只将当前值与现有的子节点进行比较。我们不希望它尝试与数组最后一个有效索引之外的元素进行比较。循环在到达堆底部或没有交换的迭代后终止（通过`break`语句）。

### 存储辅助信息

很多时候，我们需要堆为每个条目存储额外的信息。例如，在我们的任务列表中，我们不仅需要存储任务的优先级，还需要存储待办任务的其他信息。如果我们不知道该任务是什么，那么仅仅知道下一个任务的优先级是 99 是没有意义的。我们不如直接手动扫描原始列表。

扩展堆以存储复合数据结构或对象（如`TaskRecord`）很简单：

```py
TaskRecord {
    Float: Priority
    String: TaskName
    String: Instructions
    String: PersonWhoWillYellIfThisIsNotDone
    Boolean: Completed
}
```

我们通过修改之前的代码来处理基于这个复合记录的优先级字段的比较。我们可以通过直接修改代码来实现（例如在`HeapInsert`函数中）：

```py
 WHILE parent >= 1 AND (heap.array[parent].priority < 
                             heap.array[current].priority):
```

然而，这需要我们将堆实现专门化为特定的复合数据结构。更干净的方法是添加一个特定于复合数据结构的辅助函数，例如：

```py
IsLessThan(Type: a, Type: b):
  return a.priority < b.priority
```

我们将在`HeapInsert`函数的代码中使用这个函数，代替数学中的小于符号：

```py
 WHILE parent >= 1 AND IsLessThan(heap.array[parent], 
                                     heap.array[current]):
```

类似地，我们会修改`HeapRemoveMax`函数中的比较操作，使用辅助函数。

```py
 IF 2*i <= heap.last_index AND IsLessThan(heap.array[swap],
                                                 heap.array[2*i]):
            swap = 2*i
        IF 2*i+1 <= heap.last_index AND IsLessThan(heap.array[swap],
                                                   heap.array[2*i+1]):
            swap = 2*i+1
```

这些小的改动使我们能够从复合数据结构构建堆。只要我们能定义一个`IsLessThan`函数来排序元素，我们就可以为它们构建一个高效的优先队列。

## 更新优先级

有些使用场景可能需要另一种动态行为模式：允许算法更新优先队列中元素的优先级。考虑一个书店数据库，根据每本书的请求人数来确定哪些书籍应该补货。系统首先根据初始的书单构建堆，并用它来确定接下来应该订购哪本书。但在一篇热门博客文章指出数据结构在计算思维中的重要性后，书店突然迎来了急剧—虽然完全可以理解—的增长，更多顾客请求有关数据结构的书籍。它的优先队列必须能够处理这种突如其来的流量。

为了满足这个需求，我们使用了与添加和删除相同的方法。当我们改变一项的值时，我们会检查是提高还是降低优先级。如果我们提高了该项的值，我们需要将该项“冒泡”到最大堆中，以恢复堆的性质。类似地，如果我们降低了该项的值，我们让它下沉到最大堆中，直到到达正确的位置。

```py
UpdateValue(Heap: heap, Integer: index, Float: value):
    Type: old_value = heap.array[index]
    heap.array[index] = value

    IF old_value < value:
        Bubble the element up the heap using the
        procedure from inserting new elements 
        (swapping with parent).
    ELSE:
        Drop the element down the heap using the
        procedure from removing the max element 
        (swapping with the larger child).
```

我们甚至可以将让元素“冒泡”或“下沉”的代码提取出来，这样相同的代码可以用于更新、添加以及删除最大值操作。

我们如何找到我们想要更新的元素呢？如前所述，堆并不适合查找特定的元素。如果我们对目标元素只有它的值而没有其他信息，我们可能需要遍历数组的大部分内容来找到它。通常，我们可以通过使用二级数据结构来解决这个问题，例如哈希表（第十章讨论过），将项的键映射到堆中的元素。在本节的示例中，我们假设程序已经有了该项的当前索引。

## 最小堆

到目前为止，我们专注于最大堆，它利用树中任何节点的值都大于（或等于）其子节点的值的性质。*最小堆*是堆的一种版本，它有助于找到值最小的元素。对于最小堆，树的根节点是最小值，从而使我们可以轻松找到得分最低的元素。例如，我们可能不仅仅根据优先级来排序网络数据包，而是根据到达时间来排序，先处理到达时间较早的包，而不是最近到达的包。更重要的是，如果我们的咖啡架空间用完了，我们需要移除最不喜欢的品牌。经过一番令人心痛的内心挣扎，我们决定放弃一架架子的盘子或碗，而不是舍弃我们珍贵的咖啡粉，最终我们决定丢掉得分最低的咖啡。我们查阅了每种咖啡的享受度评分，并选择了得分最低的那一款。

理论上，我们可以通过仅仅将值取反，继续使用最大堆。然而，更简洁的策略是对我们的堆性质进行小小的调整，直接解决问题。*最小堆性质*是树中任何节点的值都小于（或等于）其子节点的值。一个最小堆的例子见图 7-8。当我们插入新元素时，值最小的元素会上浮到树的上层。同样地，我们始终会提取并返回值最小的元素。

![一个最小堆的表示，既以数组的形式也以树的形式呈现，箭头表示每个节点在数组中的位置。根节点 10 对应数组中的第一个元素，节点的两个子节点 23 和 17 分别是数组中的第二和第三个元素。](img/f07008.png)

图 7-8：最小堆的位置对应索引位置。

当然，最小堆中添加和删除元素的算法需要相应地进行修改。对于插入操作，我们需要在`WHILE`循环中改变比较函数，以检查父节点的值是否大于当前节点的值：

```py
MinHeapInsert(MinHeap: heap, Type: value):
    IF heap.last_index == heap.array_size - 1:
 Increase Heap size.

    heap.last_index = heap.last_index + 1
    heap.array[heap.last_index] = value

    # Swap the new node up the heap.
    Integer: current = heap.last_index
    Integer: parent = Floor(current / 2)
  ❶ WHILE parent >= 1 AND (heap.array[parent] > 
                           heap.array[current]):
        Type: temp = heap.array[parent]
        heap.array[parent] = heap.array[current]
        heap.array[current] = temp
        current = parent
        parent = Floor(current / 2)
```

代码的绝大部分与插入最大堆的代码相同，唯一的变化是在比较节点与其父节点时将`<`替换为`>` ❶。

我们在删除操作中的两个比较步骤也做了类似的修改：

```py
MinHeapRemoveMin(Heap: heap):
    IF heap.last_index == 0:
        return null

    # Swap out the root for the last element and shrink heap.  
    Type: result = heap.array[1]
    heap.array[1] = heap.array[heap.last_index]
    heap.array[heap.last_index] = null
    heap.last_index = heap.last_index - 1

    # Bubble the new root down.
    Integer: i = 1
    WHILE i <= heap.last_index:
        Integer: swap = i
      ❶ IF 2*i <= heap.last_index AND (heap.array[swap] > 
                                       heap.array[2*i]):
            swap = 2*i
      ❷ IF 2*i+1 <= heap.last_index AND (heap.array[swap] >
                                         heap.array[2*i+1]):
            swap = 2*i+1

        IF i != swap:
            Type: temp = heap.array[i]
            heap.array[i] = heap.array[swap]
            heap.array[swap] = temp
            i = swap
        ELSE:
            break
    return result
```

在这里，唯一的变化是，在决定是否交换节点时，将`<`替换为`>` ❶ ❷。

## 堆排序

堆是一种强大的数据结构，广泛应用于计算机科学的多种任务，不仅限于实现优先队列和高效返回优先级列表中的下一个元素。通过堆以及数据结构来看待问题的另一个有趣角度是它们所能启用的新算法。J. W. J. Williams 最初在提出一种新算法排序数组的背景下提出了堆：*堆排序*。

顾名思义，堆排序是一种使用堆数据结构对一组元素进行排序的算法。输入是一个无序数组，输出是一个包含相同元素的数组，但按*降序*排序（对于最大堆）。堆排序的核心包含两个阶段：

1.  从所有元素中构建最大堆

1.  从堆中提取所有元素并按降序排列，存储在一个数组中

就这么简单。

这是堆排序的代码：

```py
Heapsort(Array: unsorted):
    Integer: N = length(unsorted)
    Heap: tmp_heap = Heap of size N
    Array: result = array of size N      

    Integer: j = 0
  ❶ WHILE j < N:
        HeapInsert(tmp_heap, unsorted[j])
        j = j + 1

    j = 0
  ❷ WHILE j < N:
        result[j] = HeapRemoveMax(tmp_heap)
        j = j + 1
    return result
```

这段代码由两个`WHILE`循环组成。第一个循环将每个元素插入到一个临时堆中 ❶。第二个循环使用`HeapRemoveMax`函数移除最大元素，并将其添加到数组中的下一个位置 ❷。或者，我们也可以通过使用最小堆和`HeapRemoveMin`来实现堆排序，从而得到一个*升序*排列的结果。

假设我们想将以下数组 `[46, 35, 9, 28, 61, 8, 38, 40]` 按照递减顺序进行排序。我们首先将这些值逐一插入到堆中。图 7-9 显示了插入后的数组最终排列（以及其等效的树形表示）。请记住，我们总是从数组的末尾插入新元素，然后将它们交换到前面，直到恢复堆的性质。在 图 7-9 中，箭头表示新元素通过数组到达其最终位置的路径。树形表示也一并展示，其中阴影部分表示已被修改的节点。

![这张插图展示了堆排序第一阶段中的每一个插入过程，既包括数组中的插入，也包括树中的插入。数字 46、35、9 和 28 都是在没有交换的情况下插入的。当 61 被插入到树的顶部和数组的前面时，它首先与 35 交换位置，然后与 46 交换，成为新的根节点。](img/f07009.png)

图 7-9：堆排序的第一阶段，其中未排序的数组中的元素逐一添加到堆中

我们通过单次插入的最坏情况运行时间来限制构建堆的运行时间。正如我们在本章前面所看到的，最坏情况下，将一个新元素插入到一个包含 *N* 个元素的堆中，其时间复杂度是与 log2 成正比的。因此，要构建一个包含 *N* 个元素的堆，我们将最坏情况的运行时间限定为与 *N*log2 成正比。

现在我们已经构建好了堆，接下来进入第二阶段，按照 图 7-10 所示提取每个元素。

![这张插图展示了堆排序第二阶段中的每一个移除过程，既包括数组中的移除，也包括树中的移除。在第一次移除时，节点 61 被移除，46 被交换到它的位置，40 被交换到 46 原来的位置，而 28 被提升到 40 原来的位置。](img/f07010.png)

图 7-10：堆排序的第二阶段，在该阶段，最大元素会在每次迭代中从堆中移除

我们按照优先级递减的顺序逐一从堆中移除元素。这就产生了递减排序的结果。在每一步中，根节点被提取，堆中的最后一个元素交换到根节点的位置，然后新的根节点下沉到一个位置，恢复堆的性质。图中展示了每次迭代结束时数组（以及对应的树形表示）的状态。图中的箭头和阴影节点展示了过度提升的节点在堆中向下移动的过程。当我们提取元素时，它们会直接添加到存储结果的数组中。一旦堆被清空，我们就丢弃它，它已经完成了它的任务。

与插入操作类似，我们可以将最坏情况下的运行时间限制为与 *N*log2 成正比。每次删除操作最多需要 log2 的时间来恢复堆的性质，我们需要提取所有 *N* 个项目来形成排序列表。因此，堆排序算法的总最坏情况运行时间与 *N*log2 成正比。

## 为什么这很重要

堆是二叉树的一种简单变体，它允许一组不同的计算高效操作。通过将二叉搜索树的性质转换为堆的性质，我们可以改变数据结构的行为，并支持一组不同的操作。

新元素的添加和最大元素的删除都要求我们最多走一条从树顶到底部的路径。由于我们可以在不增加树底部的新层次的情况下，几乎将堆中节点数量翻倍，因此即使是大型堆也能保持高效的操作。以这种方式翻倍节点数量仅增加了插入和删除操作的一次额外迭代！此外，这两种操作都能确保树保持平衡，以便未来的操作仍然高效。

然而，总是存在权衡：通过从二叉搜索树性质转变为堆性质，我们不再能有效地搜索特定值。优化某一组操作通常会妨碍我们优化其他操作。我们需要仔细思考如何使用数据，并相应地设计其结构。
