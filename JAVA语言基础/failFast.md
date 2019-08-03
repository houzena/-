failFast机制

在使用迭代器遍历时会发生这种情况，for(:)作为一种语法糖，也是迭代器实现的。

具体是迭代器在调用.next()时，会检查modCount和expectModCount是否一致，如果在外面调用的修改操作，modCount就会加1,从而和expectModCount不一样，这时就会抛出**并发修改异常。**

注意如果是使用的迭代器提供的接口修改，则modCount和expectModCount都会加1，不会抛出异常。

