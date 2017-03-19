## AVL树\(Adelson-Velskii and Landis' tree\)

1.AVL = BBST\(Balanced Binary Search Tree\)

2.重平衡\(rebalance\):令刚失衡的搜索树重新恢复为BBST的过程, 不超过O\(logn\)

3.平衡因子\(balanced factor\)  
\(1\)对于二叉树中的任何一个节点v,都可以定义平衡因子\(balanced factor\)  
\(2\)BalFac\(v\) = height\(lChild\(v\)\) - height\(rChild\(v\)\)  
\(3\)AVL树: For all v, asb\(BalFac\(v\)\) &lt;= 1

4.AVL = 适度平衡  
\(1\)height\(AVL\) = O\(logn\)  
\(2\)高度为h的AVL树,至少包含S\(h\) = fib\(h+3\)-1个节点

![](/assets/AVL_fib.png)

### 失衡和复衡

5.失衡+复衡  
\(1\)AVL树刚插入一个节点后失衡节点最多为O\(logn\) \(也就是最深叶子插入, 导致其上面所有父节点都失衡\)  
\(2\)AVL树刚删除一个节点后失衡节点最多为O\(1\) \(因为高度是由最长的节点决定的\)

![](/assets/AVL_insert_remove.png)

### 接口

6.AVL:接口

```
//The only way a preprocessor directive can extend through more than one line is by preceding the newline character at the end of the line by a backslash (\).

#define Balanced \            //理想平衡
( stature((x).lChild) == stature((x).rChild) )

#define BalFac(x) \           //平衡因子
( stature((x).lChild) - stature((x).rChild) )

#define AvlBalanced(x) \    //AVL平衡条件
( (-2<BalFac(x)) && (BalFac(x)<2) )

template <typename T> class AVL: public BST<T>{
  public:                                //BST::search() 等接口,可直接沿用
    BinNodePosi(T) insert(const T &);    //插入*重写*
    bool remove(const T &);                //删除*重写*
}
```

### 插入

7.插入:单旋\(zig/zag\)  
\(1\)同时可有多个失衡节点,最低者g不低于x祖父\(也就是说, 从x失衡开始, 最多失衡到它祖父\)  
\(2\)g经单旋调整后恢复平衡,子树高度复原:更高祖先也必平衡,全树复衡  
\(g=grandfather,f=father\)

8.插入:双旋\(zig & zag\)  
\(1\)同时可有多个失衡节点,最低者g不低于x祖父\(也就是说, 从x失衡开始, 最多失衡到它祖父\)

9.插入:实现

```
/*AVL insert*/

template <typename T> BinNodePosi(T) AVL<T>::insert(const T & e){
  BinNodePosi(T) & x = search(e);
  if(x) return x;

  //new insert
  x = new BinNode<T>(e,_hot);
  _size++;
  BinNodePosi(T) xx = x;

  //从x的父亲出发逐层向上,依次检查各代祖先g
  for( BinNodePosi(T) g = x->parent; g; g=g->parent){
    if(!avlBalanced(*g)){
      FromParentTo(*g) = rotateAt( tallerChild( tallerChild(g) ) ); //这啥?不懂啊
      break;
    } else {
      updateHeight(g);
    }
  }

  return xx;
}
```

### 删除

10.删除:单旋  
\(1\)同时至多一个失衡节点g,首个可能就是x的父亲\_hot  
\(2\)g经单旋调整后复衡,子树高度未必复原;更高祖先仍可能失衡  
\(3\)有失衡传播现象,可能需要做O\(logn\)次调整

11.删除:双旋  
\(1\)同时至多一个失衡节点g,首个可能就是x的父亲\_hot

12.删除:实现

```
/*AVL remove*/

template <typename T> bool AVL<T>::remove(const T & e){
    BinNodePosi(T) & x = search(e);
    if(!x) return false;
    removeAt(x,_hot);
    _size--;

    //从_hot出发逐层向上,依次检查各代祖先g
    for(BinNodePosi(T) g=_hot; g; g=g->parent){
    //注意:起点是被删除节点的父亲,而不是像插入操作那样直接从祖父开始

        if(!AvlBalanced(*g)){   //失衡
            g = FromParentTo(*g) = rotateAt( tallerChild( tallerChild(g) ) );
        }
        updateHeight(g);        //不管怎样都得更新高度

    }//可能需要做O(logn)次调整,无论是否做过调整,全树高度均可能下降

    return true;
}
```

### 3+4重构

13."3+4"重构算法  
\(1\)设g\(x\)为最低的失衡节点,考察组孙三代:g,p,v  
\(2\)按中序遍历次序,将其重命名为 a&lt;b&lt;c  
\(3\)他们总共拥有互不相交的四棵\(可能为空\)子树  
\(4\)按中序遍历次序,重命名为: T0 &lt; T1 &lt; T2 &lt; T3  
\(5\)按中序遍历的次序: T0 a T1 b T2 c T3 直接拼接

14.3+4重构:实现

```
/*3+4 reconstruct*/
//按中序遍历的次序: T0 a T1 b T2 c T3 直接拼接

template <typename T> 
BinNodePosi(T) BST<T>::connect34(
    BinNodePosi(T) a, BinNodePosi(T) b, BinNodePosi(T) c,
    BinNodePosi(T) T0,BinNodePosi(T) T1,BinNodePosi(T) T2,BinNodePosi(T) T3)
{
    a->lChild = T0; if(T0) T0->parent = a;
    a->rChild = T1; if(T1) T1->parent = a; updateHeight(a);
    c->lChild = T2; if(T2) T2->parent = c;
    c->rChild = T3; if(T3) T3->parent = c; updateHeight(c);

    b->lChild = a; a->parent = b;
    b->rChild = c; c->parent = b; updateHeight(b);

    return b;    //该子树新的根节点
}
```

15.ROTATEAT\(\)

```
/*rotateAt*/

template<typename T>
BinNodePosi(T) BST<T>::rotateAt(BinNodePosi(T) v){
    BinNodePosi(T) p=v->parent,g=p->parent; //parent,grandparent
    if(IsLChild(*p)){        //zig
        if(isLChild(*v)){    //zig-zig
            p->parent = g->parent;    //向上联接
            return connect34(v,p,g,v->lChild,v->rChild,p->rChild,g->rChild);
        } else {            //zig-zag
            v->parent = g->parent;    //向上联接
            return connect34(p,v,g,p->lChild,v->lChild,v->rChild,g->rChild);
        }
    } else {
        /*..zag-zig & zag-zag*/
    }
}
```

16.AVL树综合评价  
\(1\)优点:  
-查找,插入,删除,最坏情况复杂度O\(logn\)  
-O\(n\)存储空间  
\(2\)缺点:  
-借助高度或平衡因子,为此需要改造元素结构,或额外封装 \(可用伸展树,无需平衡因子\)  
-实测复杂度与理论值尚有差距  
--插入/删除后的旋转,成本很高  
--删除操作后,最多需旋转O\(logn\)次\(Knuth:平均仅0.21次\)  
--若频繁插入/删除,得不偿失  
-单词动态调整后,全树拓扑结构的变化量可能高达O\(logn\) \(红黑树,可将插入删除变化量控制在常数\)

