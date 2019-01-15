> Reflection is commonly used by programs which require the ability to examine or modify the runtime behavior of applications running in the Java virtual machine.
>
> 对于运行在Java虚拟机上的程序来说，如果需要检查或修改运行时行为，反射是一种很常见的技术。

> This is a relatively advanced feature and should be used only by developers who have a strong grasp of the fundamentals of the language.
>
> 反射是一种比较高级的编程技术，程序员应该在对Java的基础知识有了牢靠掌握之后，再去使用它。

> With that caveat in mind, reflection is a powerful technique and can enable applications to perform operations which would otherwise be impossible.
>
> 记住这条提醒，反射确实是一种很强大的技术，它使得你的程序能进行某些特定的操作，而这些操作不使用反射是无法完成的。

> Reflection is powerful, but should not be used indiscriminately.
>
> 反射虽然很强大，但不应该不加选择就去使用它。

> If it is possible to perform an operation without using reflection, then it is preferable to avoid using it.
>
> 如果某种操作是不经过反射就可以实现的，那我们就倾向于避免使用反射。

> Because reflection involves types that are dynamically resolved, certain Java virtual machine optimizations can not be performed.
>
> 由于反射涉及到动态解析的类型，所以Java虚拟机中的某些优化无法进行。

> Consequently, reflective operations have slower performance than their non-reflective counterparts, and should be avoided in sections of code which are called frequently in performance-sensitive applications.
>
> 因此，在对性能敏感的程序里，如果一段代码被反复调用，那在这段代码中要尽量避免使用反射。

本文选自：
https://docs.oracle.com/javase/tutorial/reflect/index.html