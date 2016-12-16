# anonymous-function
匿名函数

匿名函数的使用

1.递归

当函数调用自身，或调用另外一个函数，在这个函数的调用树种的某一个地方又调用了自己时，递归就发生了。

2.使用this进行，递归递归中函数引用丢失问题

var ele={

chirp:function(n){

return n>1?ele.chirp(n-1)+"-chirp":"chirp";


}

};

var sub={chirp:ele.chirp};

ele={};

try{

assert(sub.chirp(3)=="chirp-chirp-chirp","Is this going to work?");

}
catch(e){

assert(false,"where is ele.chirp go?");

}

重新给ele对象定义一个空对象后，匿名函数仍然存在，而且可以通过sub.chirp属性进行引用,但是ele.chirp属性却已经不复存在。而该函数是通过原有的ele.chirp

属性引用进行递归调用自身的，所以函数在调用时会出现引用丢失的问题。

解决方案 var ele={

chirp:function(n){

return n>1?this.chirp(n-1)+"-chirp":"chirp";

}

}

解决原理：当一个函数作为方法被调用时，函数上下文指的是调用该方法的那个对象。

调用ele.chirp()时，this对象引用的是ele;而调用sub.chirp()时，this对象引用的则是sub。

3.使用内联命名函数进行递归

var ele={

chirp:function  signal(n){

return n>1?signal(n-1)+"-chirp":"chirp";

}

};

assert(ele.chirp(3)=="chirp-chirp-chirp","works as we would expect it to");

var sub={chirp:ele.chirp};

ele={};

assert(sub.chirp(3)=="chirp-chirp-chirp","the method correctly calls itself.");//按照预期执行

尽管可以给内联函数进行命名，但是这些名称只能在自身函数内部才是可见的。

例如： 


     var ele=function myEle(){

      assert(ele===myEle,"this function is named two things at once!")

      }
      
      ele();
      
      assert(typeof myEle=="undefined","BUT myEle isn't defined outside of the function.")
      
4.使用callee进行递归(callee属性在即将到来的javascript上会被去除，而且ECMAScript5 标准在strict模式下也禁止使用该属性了)

    
