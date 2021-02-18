# スレッドのコンテンション

via [multithreading - What is thread contention? - Stack Overflow](https://stackoverflow.com/questions/1970345/what-is-thread-contention/7064008)

本質的にスレッドの競合とは、あるスレッドが現在他のスレッドによって保持されてい
るロック/オブジェクトを待っている状態です。したがって、この待機中のスレッドは、
他のスレッドがその特定のオブジェクトのロックを解除するまで、そのオブジェクトを
使用することができません。
