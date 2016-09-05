#使用stringstream对象简化类型转换

<pre>
#include <sstream>
#include <string>
string itoa(int i){ //整型转字符串
    stringstream s;
    s << i;
    return s.str();
}

int atoi(string str){//字符串转整型
    stringstream s;
    int n=0;
    s << str;
    s >> n;
    return n;
    
}
</pre>