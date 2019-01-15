## Merkle Tree

*Reference： https://www.cnblogs.com/fengzhiwu/p/5524324.html* (非常好的文章！)

Merkle Tree，通常也被成为Hash Tree。顾名思义，就是存储hash的一棵树。Merkle树的叶子是数据块的hash值。非叶子节点是其对应子节点串联字符串的hash。

![merkle](https://github.com/MirageLyu/DailyNote/blob/master/2019_Jan/2019.1.15/images/merkle1.png)

### Hash

- 多用于数据完整性校验
- 如果从一个稳定的服务器进行下载，采用单一的Hash是可取的。但如果数据源不稳定，一旦数据损坏，就需要重新下载，这种下载的效率是很低的。

### Hash List

在点对点网络中作数据传输的时候，会同时从多个及其上下载数据，而且很多机器可认为是不稳定或者是不可信的。为了校验数据的完整性，更好地方法是把大的文件分割成小的数据块。这样的好处是，如果小块数据在传输过程中损坏了，那么只要重新下载这一块数据即可，不用重新下载整个文件。

怎么确定小的数据块没有损坏？只需要为每个数据块做Hash。BT下载的时候，在下载到真正数据之前，我们会先下载一个Hash列表。那么如何确定这个Hash List本身的正确性呢？就是把每个小块数据的Hash值拼到一起，然后对这个长字符串再做一次Hash运算，这样就得到Hash列表的根Hash(称为Top Hash或Root Hash)。下载数据的时候，首先从可信的数据源得到正确的根Hash(例如，网页的可视字符串)，就可以用它来校验Hash列表了，然后通过校验后的Hash列表校验数据块。如下图。

![merkle](https://github.com/MirageLyu/DailyNote/blob/master/2019_Jan/2019.1.15/images/merkle2.png)

### Merkle Tree

Merkle Tree可以看做是Hash List的泛化(Hash List可以看做是一种特殊的Merkle Tree，即树高为2的多叉Merkle Tree)。

在最底层，和哈希列表一样，我们把数据分成小的数据块，有相应的哈希和它对应。但是往上走，并不是直接去运算根哈希，而是把相邻的两个哈希合并成一个字符串，然后运算这个字符串的哈希，这样每两个哈希就结婚生子，得到了一个“子哈希”。如果最底层的哈希总数是单数，那到最后必然出现一个单身哈希。这种情况就直接对它进行哈希运算，所以也能得到它的子哈希。于是往上推，依然是一样的方式，可以得到数目更少的新一级哈希，最终必然形成一棵树，到了树根的文职，这一代就只剩下一个根哈希了，我们把它叫做Merkle Root。



Merkle Tree与Hash List主要的区别在于，可以直接下载并立即验证Merkle Tree的一个分支。因为可以将文件切分为小的数据块，这一样如果有一块数据损坏，仅仅重新下载这个数据块就行了。如果文件非常大，那么Merkle Tree和Hash List都很大，但是Merkle Tree可以一次下载一个分支，然后可以立即验证这个分支，如果分支验证通过，就可以下载数据了。而Hash List只有下载整个Hash List才能验证。

### MT的特点

- MT是一种树，大多是二叉树，也可以是多叉树。具有树的所有特点；
- MT的叶子节点的value是数据集合的单元数据或者单元数据Hash；
- 非叶子节点的value是根据它下面所有的节点值，然后按照Hash算法计算而得出的。

### MT的操作(见原文)

![merkle](https://github.com/MirageLyu/DailyNote/blob/master/2019_Jan/2019.1.15/images/merkle3.png)

### **MT的应用-4 IPFS**



