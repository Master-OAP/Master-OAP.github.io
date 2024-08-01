---
title: "设计模式"
date: 2024-06-10T02:10:00+08:00
summary: '不止代码'
categories: ['Design-Pattern']
draft: true
images: ['/img/DesignPattern/gangOfFour.gif', '/img/DesignPattern/gangOfFour.png']
---
{{< notice notice-warning >}}
小心过度设计，谨慎使用设计模式！
{{< /notice >}}
# 1. 模板方法
对于拥有固定流程的任务，框架设计者希望能够由使用者来负责其中某些子流程的编写。例如，对于任务`1->2->3->4->5`，假设`2和4`是可被替换的，这时有两种方案：
1. 由使用者负责重写`2和4`，再由其去调用其他步骤，但这样做无疑会给使用者增加不必要的心智负担；
2. 借用多态机制，重写`2和4`，由框架设计者固定流程，而使用者只需要`重写可扩展的子流程`。

代码示例：
1. 框架：
```c++
//程序库开发人员
class Library{
public:
    //稳定 template method
    void Run(){
        Step1();
        if (Step2()) { //支持变化 ==> 虚函数的多态调用
            Step3(); 
        }
        for (int i = 0; i < 4; i++){
            Step4(); //支持变化 ==> 虚函数的多态调用
        }
        Step5();

    }
    virtual ~Library(){ }

protected:
    void Step1() { //稳定
        //.....
    }
    void Step3() { //稳定
        //.....
    }
    void Step5() { //稳定
        //.....
    }
    // 对于变化部分声明为虚函数，由使用者重写
    virtual bool Step2() = 0; //变化
    virtual void Step4() =0;  //变化
};
```
2. 应用
```c++
//应用程序开发人员
class Application : public Library {
protected:
    virtual bool Step2(){
        //... 子类重写实现
    }

    virtual void Step4() {
        //... 子类重写实现
    }
};

int main() {
    Library* pLib=new Application();
    lib->Run();

    delete pLib;
}
```
模板方法很好，能够在不改变算法结构的情况下重新定义算法的某些步骤，但其缺点也不可忽视：
1. 每一套都需要一个抽象基类和众多具体子类，难免导致代码冗余；
2. 从子步骤的方面考虑的确是能复用，但反过来想，固定的流程也是限制了子类的灵活性；
3. 难调试，用了继承和多态就难调，看都难看，妈的😭。
# 2. 单例模式

# 3. 工厂模式

# 3. TODO


# 参考
1. [李建忠-设计模式](https://www.youtube.com/watch?v=6f7ykipOmfE&list=PLE0JTxLz7jTR2e8nAyV9vPIqH5NNxlI3N&ab_channel=PengjieLi)
2. [过度设计](https://developer.aliyun.com/article/1092076)