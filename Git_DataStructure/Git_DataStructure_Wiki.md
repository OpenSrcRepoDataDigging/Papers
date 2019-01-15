# Git

## Data Structures - From Wikipedia

Git primitives are not inherently a source-code management(SCM) system. Linus Torvalds explains:

Git的初衷不是一个固有的源代码管理系统，Linus解释说：

*In many ways you can just see git as a file system - it's content-addressable, and it has a notion of versioning,but i really really designed it coming at the problem from the viewpoint of a file system person(hey, kernels is what i do), and I actually have absolutely zero interest in creating a traditional SCM system*.

From this initial design approach, Git has developed the full set of features expected of a tradition SCM, with features mostly being created as needed, than refined and extended over time.

从这个初始的设计路线中可以看出，Git开发了传统SCM应有的所有特性，这些特性主要包括按需创建，之后随着时间改进和扩展。

Git has two data structures: a mutable *index*(also called *stage* or *cache*) that caches information about the working directory and the next revision to be committed; and an immutable, append-only *object database*.

Git有两个数据结构：一个可变的“索引”(*index*)，也被称作“阶段”(*stage*)或“缓存”(*cache*)，这其中缓存了有关工作目录和下一个commit的修订信息；一个不可变的，只能附加上去的对象数据库(*object database*)

The index serves as connection point between the object database and the working tree.

Index充当对象数据库和工作树之间的连接。

The object database contains four types of objects:

Object database中包括四种对象：

- A *blob*(abbr. binary large object) is the content of a file. Blobs have no proper file name, time stamps, or other meta-data.(A blob's name internally is a hash of its content.)

  blob是一个文件的内容。blob没有特定的文件名，或者时间戳，或其他元数据。(一个blob的名字在git内部其实是其内容的哈希)

  *注：meta-data，元数据，意义是：一个描述其他数据，并提供替他数据的信息的一组数据。*

- A *tree* object is the equivalent of a directory. It contains a list of file names, each with some type bits and a reference to a blob or tree object that is that file, symbolic link, or directory's contents. These objects are a snapshot of the source tree.(In whole, this comprises a Merkle tree, meaning that only a single hash for the root tree is sufficient and actually used in commits to precisely pinpoint to the exact state of whole tree structures of any number of sub-directories and files)

  一个tree对象等同于一个目录。它包括一个文件名列表，其中每个都有一些类型标志位和一个引用，引用一个作为文件、符号链接或是目录内容的blob或其他tree对象。这些对象是source tree的快照。(总的来说，它包含一个Merkle树，这意味着只有一个根数的哈希就足够了，并且事实上它主要用来在提交中，精确地指出任意数量的子目录和文件组成的树结构的确切状态。)

  *注：snapshot，快照，意义是：它本质是上下文信息的副本，“快”字强调了它是在瞬间获取的，也就是不会出现在复制期间对数据改变而引起的“滚动快门”伪像。*

- A *commit* object links tree objects together into a history. It contains the name of a tree object(of the top-level source directory), a time stamp, a log message, and the names of zero or more parent commit objects.

  一个commit对象将tree对象链接起来组成一个历史记录。它包括一个tree对象的名字(最顶层的源目录)，一个时间戳，一个记录信息，0个或多个父commit对象的名字。

- A *tag* object is a container that contains a reference to another object and can hold added meta-data related to another object. More commonly, it is used to store a digital signature of a commit object corresponding to a particular release of the data being tracked by Git.

  一个tag对象是一个容器，其中装了另一个对象的引用，和另一对象相关的附加元数据。最常见的使用情况是，它用来存储与Git跟踪记录的特定版本的数据对应的commit对象的数字签名。

Each object is identified by a SHA-1 hash of its contents. Git computes the hash and uses this value for the object's name. The object is put into a directory matching the first two characters of its hash. The rest of the hash is used as the file name for that object.

每个对象都被其内容的SHA-1哈希值标识。Git计算这个哈希值，并将这个值作为对象的名字。这个对象被放到一个名字为其名字前两个字符的目录中，哈希值剩下的部分作为其文件名。

Git stores each revision of a file as a unique blob. The relationships between the blobs can be found through examining the tree and commit objects. Newly added objects are stored in their entirely using zlib compression. This can consume a large amount of disk space quickly, so objects can be combined into *packs*, which use delta compression to save space, storing blobs as their changes relative to other blobs.



Git将每个版本存储为唯一的blob。可以通过检查tree对象和commit对象来找到blob之间的关系。新添加的对象将被存储在完全使用的zlib压缩中。这可以快速节省大量的磁盘空间，所以这些对象可以组合成包，使用增量压缩来节省空间，将blob存储为相对于其他blob的更改。