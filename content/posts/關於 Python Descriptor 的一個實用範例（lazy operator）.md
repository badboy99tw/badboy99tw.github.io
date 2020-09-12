Title: 關於 Python Descriptor 的一個實用範例（lazy operator）
Slug: 關於 Python Descriptor 的一個實用範例（lazy operator）
Date: 2015-09-23 23:07
Category: Note
Tags: Python

最近在看 [Mastering Python Design Patterns](http://www.tenlong.com.tw/items/9864340417?item_id=1006942)。看到這個例子很有趣，他延遲了某些運算成本比較高的 member variable 的初始化時間。

```python
class LazyProperty:

    def __init__(self, method):
        self.method = method
        self.method_name = method.__name__
        print('function overriden: {}'.format(self.method))
        print('function\'s name: {}'.format(self.method_name))

    def __get__(self, obj, cls):
        if not obj:
            return None
        value = self.method(obj)
        setattr(obj, self.method_name, value)
        return value

class Test:

    def __init__(self):
        self.x = 'foo'
        self.y = 'bar'
        self._resource = None

    @LazyProperty
    def resource(self):
        print('initializing self._resource which is: {}'.format(self._resource))
        self._resource = tuple(range(5))
        return self._resource

def main():
    t = Test()
    print(t.x)
    print(t.y)
    print(t.resource)
    print(t.resource)

if __name__ == '__main__':
    main()
```

輸出的結果如下

```shell
function overriden: <function Test.resource at 0x10b0fc9d8>
function's name: resource
foo
bar
initializing self._resource which is: None
(0, 1, 2, 3, 4)
(0, 1, 2, 3, 4)
```

可以發現 resource 的 initialization 只被執行過一次！原因是因為 descriptor 只有定義 `__get__`，所以 descriptor 在第一次執行 `__get__` 的時候，就被 `setattr(obj, self.method_name, value)` 這行取代了，讓第二次的取值，變成直接拿值，而不是再次執行 `__get__`。

這招實在太酷炫了，趕快記下來！
