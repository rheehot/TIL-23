DATE:2020.07.05.Sun

Kotlin coroutines on Android A coroutine is a concurrency design pattern that you can use on Android to simplify code that executes asynchronously Coroutines were added to Kotlin in version 1.3 and are based on established concepts from other languages

Features

Lightweight: You can run many coroutines on a single thread due to support for suspension, which doesn’t block the thread where the coroutine is running. Suspending saves memory over blocking while supporting many concurrent operations

Fewer memory leaks: Use structured concurrency to run operations within a scope

Built-in cancellation support: Cancellation is propagated automatically through the running coroutine hierarchy

Jetpack integration: Many Jetpack libraries include extensions that provide full coroutines support. Some libraries also provide their own coroutine scope that you can use for structured concurrency

How to use steps

Coroutines are launched with launch coroutine builder in a context of some CoroutineScope meaning that the lifetime of the new coroutine is limited only by the lifetime of the whole application.
