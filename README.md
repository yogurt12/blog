# BLOG

# Kotlin coroutines / thread / virtual thread

## Thread

- 昂贵：线程需要昂贵的上下文切换。
- 有限：可被启动的线程数受底层操作系统的限制。在服务器端应用程序中，这可能会导致严重的瓶颈。
- 难度：调试线程与避免竞争条件是我们在多线程编程中遇到的常见问题。

## Callback - 回调

```kotlin
fun preparePostAsync(callback: (Token) -> Unit) {
    // 发起请求并立即返回
    // 设置稍后调用的回调
}
```

- 回调嵌套：通常被用作回调的函数，经常最终需要自己的回调。这导致了一系列回调嵌套并导致出现难以理解的代码。该模式通常被称为标题圣诞树（大括号代表树的分支）。

## Future、 Promise 及其他

（线程池中常见的）futures 或 promises 背后的想法（这也可能会根据语言/平台而有不同的术语），是当我们发起调用的时候，我们承诺在某些时候它将返回一个名为 Promise 的可被操作的对象。

```kotlin
fun postItem(item: Item) {
    preparePostAsync() 
        .thenCompose { token -> 
            submitPostAsync(token, item)
        }
        .thenAccept { post -> 
            processPost(post)
        }

}

fun preparePostAsync(): Promise<Token> {
    // 发起请求并当稍后的请求完成时返回一个 promise
    return promise 
}
```

