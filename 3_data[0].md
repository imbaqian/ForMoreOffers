#<center>c语言数组扩展</center>

<pre>
struct MyData {
    int nLen;
    char data[0];
};
</pre>

 在结构中，data是一个数组名；但该数组没有元素；该数组的真实地址紧随结构体MyData之后，而这个地址就是结构体后面数据的地址（如果给这个结构体分配的内容大于这个结构体实际大小，后面多余的部分就是这个data的内容）；  
 这种声明方法可以巧妙的实现C语言里的数组扩展。  
 实际用时采取这样：  
 <pre>struct MyData \*p = (struct MyData \*)malloc(sizeof(struct MyData )+strlen(str));</pre>
 这样就可以通过p->data 来操作这个str。
 
 <pre>
示例：
#include <iostream>
using namespace std;
struct MyData {
    int nLen;
    char data[0];
};

int main(){
    int nLen = 10;
    char str[10] = "123456789";
    cout << "Size of MyData: " << sizeof(MyData) << endl;
    MyData *myData = (MyData*)malloc(sizeof(MyData) + 10);
    memcpy(myData->data,  str, 10);//将str中10个字节的数据拷贝到myData->data
    cout << "myData's Data is: " << myData->data << endl;
    free(myData);
    return 0;
}
 </pre>
 
 输出：
Size of MyData: 4
myData's Data is: 123456789         
由于数组没有元素，该数组在该结构体中分配占用空间，所以sizeof(struct Mydata) = 4。
malloc申请的是14个字节的连续空间，它返回一个指针指向这14个字节，强制转换成struct INFO的时候，前面4个字节被认为是Mydata结构，后面的部分拷贝了“123456789”的内容