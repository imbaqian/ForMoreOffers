

## T\* copy_backward(T\* first, T\* last, T\* result);

将[first,last) 内容拷贝到[result - (last -first),result )

例如：
<pre>
       first   last          result
       ↓       ↓             ↓  
     0 1 2 3 4 5 6 7 8 9 a b c d e f   
拷贝 *(result-1) = *(last-1)  ~  *(result-(last-first)) = *(first)   
                     result
                     ↓     
     0 1 2 3 4 5 6 7 1 2 3 4 c d e f
最后返回result 也就是拷贝后的first位置
</pre>            

##T\* copy(T\* first, T\* last, T\* result);

将 [first,last) 的内容拷贝到 [result,result + (last - first))
返回 result + (last - first)