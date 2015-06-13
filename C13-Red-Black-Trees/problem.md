### Problems 1 : Persistent dynamic sets
***






### `Answer`
a. 插入的时候，需要改变根节点到这个叶子节点路径上的所有节点. BST删除有3种情况，如果删除的是根节点，那么需要改变根节点到这个叶子节点路径上的所有节点; 如果删除的节点只有一个子节点，那么需要改变根节点到这个删除节点路径之间的所有节点; 如果删除的节点有两个子节点，那么也是需要修改被影响的路径.只有一条路径被影响，因为某个节点和其后继肯定是同一条路径的.

b. 先定义两个操作. MAKE-NEW-NODE(k) 创建一个键值为k的新节点,左儿子和右儿子默认为NIL,并返回这个节点;COPY-NODE(x) 创建一个和x一模一样的节点并返回.
r是根节点
![image](./repo/p/1.png)

c. O(h)的时间和空间

d. 很好理解，因为根节点变了，所以子节点要指向新的节点，必然也要创建新的子节点.

e. 首先要认识到最重要的两点. 1. 根据d可知节点不能有parent属性，因此我们在RB-INSERT的时候需要用stack去保存从根到叶子节点，然后将这个stack作为参数传到RB-INSERT-FIXUP或者RB-DELETE. 2. 旋转或重新着色不会改变超过O(lgn)的节点. <br /><br />
INSERT : 首先和BST一样，最后插入了红节点. 有3种case,第一种节点z上升两层，修改了z的父亲节点(INSERT的时候已经创建)，爷爷节点(INSERT的时候已经创建)和叔父节点(调用COPY-NODE创建并修改颜色),所以这种情况只创建了一个新节点和修改3次颜色. 而case2和case3执行后是不会再次循环的，最多旋转两次. case2和case3只对6个节点进行操作，而且这6个节点都已经创建出来，只是简单的修改指针和颜色.  <br /><br />
DELETE : 4个case,只有case2会进入循环，最多会执行3次rotation结束. case1会创建新节点D;case2也会创建新节点D,然后节点x往上移动一层;case3会创建新节点C和D;case4会创建新节点D和E(注意，不一定会创建，如果一个case是从其他来的有些节点就已经创建好了).




### Problems 2 : Join operation on red-black trees
***
a. Given a red-black tree T, we store its black-height as the field bh[T]. Argue that this field can be maintained by RB-INSERT and RB-DELETE without requiring extra storage in the nodes of the tree and without increasing the asymptotic running times. Show that while descending through T, we can determine the black-height of each node we visit in O(1) time per node visited.





### `Answer`
a. 在insert时，如果迭代回到根节点并修改了颜色，那么黑高度就＋1;在delete时，如果迭代回到根节点，那么黑高度就－1;当T沿高度下降时，每遇到一个黑节点就将黑高度－1，自然是O(1)的.

b. 从T1往下迭代，有右节点就走右节点;碰到黑节点黑高度就－1，一直到黑高度为bh[T2].

c. 构造子树![](http://latex.codecogs.com/gif.latex? T_x)以x为根，左儿子是![](http://latex.codecogs.com/gif.latex? T_y)右儿子是![](http://latex.codecogs.com/gif.latex? T_2),将x挂到y的父节点下面,并将x设为RED(保持性质5).

d. RED.当y的父节点是红色时需要调整，根INSERT-FIXUP的case1类似，是O(lgn).

e. 情况反一下而已～

f. 根据前面的分析，是log(n)的.


### Problems 3 : AVL trees
***
An **AVL tree** is a binary search tree that is **height balanced**: for each node x, the heights of the left and right subtrees of x differ by at most 1. To implement an AVL tree, we maintain an extra field in each node: h[x] is the height of node x. As for any other binary search tree T, we assume that root[T] points to the root node.





### `Answer`
a. 对于斐波那契数列有F(0) = 1, F(1) = 1, F(2) = 2,...,F(n) = F(n-1)+F(n-2). <br \>
设T(n)为高度h的AVL树的最少节点数. 我们尝试证明 ![](http://latex.codecogs.com/gif.latex?T\(n\)\\ge F\(n\)) .<br \> 
一开始，有![](http://latex.codecogs.com/gif.latex? T\(1\)\\ge F\(1\)) 和![](http://latex.codecogs.com/gif.latex? T\(2\)\\ge F\(2\)) <br />
![](http://latex.codecogs.com/gif.latex? T\(n\)\\ge T\(n-1\) + T\(n-2\) + 1 \\\\  ~\\hspace{15 mm} \\ge F\(n-1\) + F\(n-2\) + 1 \\\\ ~\\hspace{15 mm}
 \> F\(n\) 
) 
<br />
并且有![](http://latex.codecogs.com/gif.latex? 2^n \\le F\(n\) \\le 1.6^n),
因此![](http://latex.codecogs.com/gif.latex? T\(n\) = O\(  \\lg\(n\)   \) ).

b. 	thanks [mit](http://courses.csail.mit.edu/6.046/spring04/handouts/ps5-sol.pdf) for this picture. 只画出了右边大于左边的情况.
![image](./repo/p/2.png)
![image](./repo/p/3.png)

c. 
<br \>
![image](./repo/p/4.png)

d. 因为AVL树的高度是lg(h),所以AVL-INSERT需要P(lgn)的时间.因为BALANCE操作会将不平衡的子树的高度减掉1，所以不会影响到其他地方.


### Problems 4 : Treaps
***
a. Show that given a set of nodes x1, x2, ..., xn, with associated keys and priorities (all distinct), there is a unique treap associated with these nodes.









d. 比较明显就不证明了.

e. 每一次旋转, C+D都会增加1;画张图出来会很明显的.

这题剩下的[答案](http://www2.myoops.org/twocw/mit/NR/rdonlyres/Electrical-Engineering-and-Computer-Science/6-046JFall-2005/760259F8-457D-4895-AFDC-8CE9C73D18A5/0/ps4sol.pdf)

***
Follow [@louis1992](https://github.com/gzc) on github to help finish this task.