---
title: Uncommon Uses of Python in Commonly Used Libraries
link: https://eugeneyan.com//writing/uncommon-python/
source: eugeneyan-com
published: 2022-07-31T00:00:00Z
updated: 2022-07-31T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
labels:
- engineering
- python
- 🔥
summary: Some off-the-beaten uses of Python learned from reading libraries.
content: extracted
html: 2022-07-31-uncommon-uses-of-python-in-commonly-used-libraries.html
preview:
  file: 2022-07-31-uncommon-uses-of-python-in-commonly-used-libraries.preview-5f8654df5c20.webp
  width: 256
  height: 134
  color: '#b4aa9f'
images:
- source: https://eugeneyan.com/assets/og_image/off-the-beaten-path-alexander-milo.jpg
  original:
    file: 2022-07-31-uncommon-uses-of-python-in-commonly-used-libraries.image-05a3fe43c799.jpg
    width: 1200
    height: 630
  color: '#c8d3db'
- source: https://eugeneyan.com/assets/fastapi-layers.webp
  original:
    file: 2022-07-31-uncommon-uses-of-python-in-commonly-used-libraries.image-39140847aad5.webp
    width: 800
    height: 1085
  color: '#fdfdfe'
- source: https://eugeneyan.com/assets/icon-twitter.svg
  original:
    file: 2022-07-31-uncommon-uses-of-python-in-commonly-used-libraries.image-9f73746a86e9.png
    width: 512
    height: 512
  variants:
  - file: 2022-07-31-uncommon-uses-of-python-in-commonly-used-libraries.image-10d32c0d6dac.webp
    width: 320
    height: 320
  - file: 2022-07-31-uncommon-uses-of-python-in-commonly-used-libraries.image-1a1653690e12.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://eugeneyan.com/assets/icon-linkedin.svg
  original:
    file: 2022-07-31-uncommon-uses-of-python-in-commonly-used-libraries.image-50dfb45d5f9e.png
    width: 505
    height: 505
  variants:
  - file: 2022-07-31-uncommon-uses-of-python-in-commonly-used-libraries.image-b7fdeee63d68.webp
    width: 320
    height: 320
  - file: 2022-07-31-uncommon-uses-of-python-in-commonly-used-libraries.image-003cf9435e7e.webp
    width: 505
    height: 505
  color: '#000000'
- source: https://eugeneyan.com/assets/bluesky.svg
  original:
    file: 2022-07-31-uncommon-uses-of-python-in-commonly-used-libraries.image-cd93481613cb.png
    width: 600
    height: 530
  variants:
  - file: 2022-07-31-uncommon-uses-of-python-in-commonly-used-libraries.image-1790c4125e73.webp
    width: 600
    height: 530
  color: '#1084fd'
- source: https://eugeneyan.com/assets/icon-facebook.svg
  original:
    file: 2022-07-31-uncommon-uses-of-python-in-commonly-used-libraries.image-68df389f82c9.png
    width: 256
    height: 256
  variants:
  - file: 2022-07-31-uncommon-uses-of-python-in-commonly-used-libraries.image-6035d4122baf.webp
    width: 256
    height: 256
  color: '#3b5998'
- source: https://eugeneyan.com/assets/icon-mail.svg
  original:
    file: 2022-07-31-uncommon-uses-of-python-in-commonly-used-libraries.image-bf2110bd7265.png
    width: 512
    height: 512
  variants:
  - file: 2022-07-31-uncommon-uses-of-python-in-commonly-used-libraries.image-c80acd329b85.webp
    width: 320
    height: 320
  - file: 2022-07-31-uncommon-uses-of-python-in-commonly-used-libraries.image-84c4e78a876d.webp
    width: 512
    height: 512
  color: '#000000'
---

To learn how to build more maintainable and usable Python libraries, I’ve been reading some of the most widely used Python packages. Along the way, I learned some things about Python that are off the beaten path. Here are a few things I didn’t know before.

## Using `super()` in base classes

Python’s `super()` lets us inherit base classes (aka super or parent classes) without having to explicitly refer to the base class. It’s usually used in the `__init__` method. While this might be simply a nice-to-have in single inheritance, multiple inheritance is almost impossible without `super()`.

However, one interesting use of `super()` is calling it in the *base* class. I find noticed this in requests’ [BaseAdapter](https://github.com/psf/requests/blob/main/requests/adapters.py#L71).

```python
class BaseAdapter:
    """The Base Transport Adapter"""

    def __init__(self):
        super().__init__()

    def send(self, request, stream=False, timeout=None, verify=True, cert=None, proxies=None):
```

Given that the base class doesn’t inherit from anything, why call `super()` in it?

After a bit of digging, here’s what I learned: Using `super()` in the base class allows for cooperative multiple inheritance. Without it, the `__init__` calls of parent classes—after a non-supered class—are skipped. Here’s an example with a base class (BaseEstimator) and mixin (ServingMixin), both of which will be inherited by our DecisionTree class.

First, we have a BaseEstimator that doesn’t call `super()` in its `__init__` method. It has a basic `__repr__` method to print attributes.

```python
class BaseEstimator:
    def __init__(self, name, **kwargs):
        self.name = name
        
    def __repr__(self):
        return f', '.join(f'{k}: {v}' for k, v in vars(self).items())
```

Next, we inherit BaseEstimator via the DecisionTree subclass. Everything works fine—printing the DecisionTree instance shows the attributes of BaseEstimator and DecisionTree.

```python
class DecisionTree(BaseEstimator):
    def __init__(self, depth, **kwargs):
        super().__init__(**kwargs)
        self.depth = depth

dt = DecisionTree(name='DT', depth=1)
print(dt)
> name: DT, depth: 1
```

Now, let’s also inherit ServingMixin and create an instance of DecisionTree.

```python
class ServingMixin:
    def __init__(self, mode, **kwargs):
        super().__init__(**kwargs)
        self.mode = mode

class DecisionTree(BaseEstimator, ServingMixin):
    def __init__(self, depth, **kwargs):
        super().__init__(**kwargs)
        self.depth = depth

dt = DecisionTree(name='Request Time DT', depth=1, mode='online')
print(dt)
> name: Request Time DT, depth: 1

dt.mode
> AttributeError: 'DecisionTree' object has no attribute 'mode'
```

You’ll notice that ServingMixin isn’t inherited properly: The ServingMixin attribute (mode) doesn’t show when we print our decision tree instance and if we try to access the mode attribute, it doesn’t exist.

This is because, without `super()` on the BaseEstimator, DecisionTree doesn’t call the next parent class in the [method resolution order](https://realpython.com/python-super/#method-resolution-order).

We can fix this by calling `super()` in the BaseEstimator and DecisionTree works as expected.

```python
class BaseEstimator:
    def __init__(self, name, **kwargs):
        self.name = name
        super().__init__(**kwargs)
        
    def __repr__(self):
        return f', '.join(f'{k}: {v}' for k, v in vars(self).items())

class ServingMixin:
    def __init__(self, mode, **kwargs):
        super().__init__(**kwargs)
        self.mode = mode

class DecisionTree(BaseEstimator, ServingMixin):
    def __init__(self, depth, **kwargs):
        super().__init__(**kwargs)
        self.depth = depth

dt = DecisionTree(name='Request Time DT', depth=1, mode='online')
print(dt)
> name: Request Time DT, mode: online, depth: 1

dt.mode
> 'online'
```

And that’s why we might want to call `super()` in a base class.

Further reading:

- [Calling `super()` in parent class](https://stackoverflow.com/a/47660836)
- [Understanding Python super() with **init**() methods](https://stackoverflow.com/a/27134600)
- [Python’s super() considered super!](https://rhettinger.wordpress.com/2011/05/26/super-considered-super/)

## When to use a Mixin

A mixin is a class that provides method implementations for reuse by multiple child classes. It is a limited form of multiple inheritance, and is a parent class that simply provides functionality for subclasses, does not contain state, and is not intended to be instantiated. Scikit-learn uses mixins liberally where they have [ClassifierMixin](https://github.com/scikit-learn/scikit-learn/blob/main/sklearn/base.py#L600), [TransformerMixin](https://github.com/scikit-learn/scikit-learn/blob/main/sklearn/base.py#L801), [OutlierMixin](https://github.com/scikit-learn/scikit-learn/blob/main/sklearn/base.py#L916), etc.

When should we use mixins? They are appropriate when we want to (i) provide a lot of optional features for a class and (ii) when we want to use a particular feature in a lot of different classes. Here’s an example of the former. We start with creating a basic request object in werkzeug.

```python
from werkzeug import BaseRequest

class Request(BaseRequest):
    pass
```

If we want to add accept header support, we would update it as follows.

```python
from werkzeug import BaseRequest, AcceptMixin

class Request(AcceptMixin, BaseRequest):
    pass
```

Need support for user agent, authentication, etc? No problem, just add the mixins.

```python
from werkzeug import BaseRequest, AcceptMixin, UserAgentMixin, AuthenticationMixin

class Request(AcceptMixin, UserAgentMixin, AuthenticationMixin, BaseRequest):
    pass
```

By having these features modularized as mixins—instead of adding them to the base class—we prevent our base class from getting bloated with features that only a few subclasses may use. In addition, these mixins can now be reused by other child classes (that may not inherit from BaseRequest).

Further reading:

- [Python Mixins](https://www.pythontutorial.net/python-oop/python-mixin/)
- [What is a mixin and why is it useful?](https://stackoverflow.com/questions/533631/what-is-a-mixin-and-why-is-it-useful)

## Using relative imports (almost all the time)

Relative imports ensure we search the *current* package (and import from it) before searching the rest of the `PYTHONPATH`. We use it by adding `.` before the package imported. Here’s an example from sklearn’s [`base.py`](https://github.com/scikit-learn/scikit-learn/blob/main/sklearn/base.py#L21).

```python
from .utils.validation import check_X_y
from .utils.validation import check_array
```

What happens if `base.py` doesn’t use relative imports? If we have a package named utils in our script’s directory, during import, Python will search *our* utils package instead of sklearn’s utils package, thus breaking sklearn. The `.` ensures sklearn’s `base.py` searches its own `utils` first.

(That said, is there a reason not to use relative imports? Please comment below!)

Further reading

- [Guido’s Decision on Relative Imports](https://peps.python.org/pep-0328/#guido-s-decision)
- [Python “from \[dot\]package import …” syntax](https://stackoverflow.com/questions/22511792/python-from-dotpackage-import-syntax)

## When to add to `__init__.py`

`__init__.py` marks directories as Python package directories. The common practice is to leave them empty. Nonetheless, many libraries I read had non-empty and sometimes long `__init__.py` files. This led me to dig into why we might add to `__init__.py`.

First, we might add imports to `__init__.py` when we want to refactor code that has grown into multiple modules *without* introducing breaking changes to existing users. Say we have a single module (`models.py`) that contains implementation for `DecisionTree` and `Bandit`. Over time, that single module grows into a models package with modules for `tree` and `bandit`. To ensure a consistent API for existing users, we might add the following to the `__init__.py` in the models package.

```python
from .tree import DecisionTree, RandomForest
from .bandit import Bandit, TSBandit
```

This ensures existing users can continue to import via `from models import DecisionTree` instead of `from models.tree import DecisionTree`. To them, there’s no change in API and existing code doesn’t break.

This brings us to another reason why we might add to `__init__.py`—to provide a simplified API so users don’t have to dig into implementation details. Consider the example package below.

```bash
app
  __init__.py
  model_implementation.py
  data_implementation.py
```

Instead of having users figure out what to import from `model_implementation` and `data_implementation`, we can simplify by adding to app’s `__init__.py` below.

```python
from .model_implementation import SimpleModel
from .data_implementation import SimpleDataLoader
```

This states that SimpleModel and SimpleDataLoader are the only parts of app that users should use, streamlining how they use the app package (i.e., `from app import SimpleModel, SimpleDataLoader`). And if they know what they’re doing and want to import directly from `model_implementation`, that’s doable too.

Libraries that do this include [Pandas](https://github.com/pandas-dev/pandas/blob/main/pandas/__init__.py#L48), where datatypes, readers, and the reshape API are imported in `__init__.py`, and Hugging Face’s [Accelerate](https://github.com/huggingface/accelerate/blob/main/src/accelerate/__init__.py#L7).

Other than what’s mentioned above, we might also want to (i) [initialize a logger](https://stackoverflow.com/questions/15727420/using-logging-in-multiple-modules/15729700#15729700) in the main package’s `__init__.py` for use across multiple modules and (ii) perform [compatibility checks](https://github.com/psf/requests/blob/main/requests/__init__.py#L58).

Further reading

- [ What is `__init__.py` for? ](https://stackoverflow.com/a/18979314)
- [What’s your opinion on what to include in `__init__.py`?](https://www.reddit.com/r/Python/comments/1bbbwk/whats_your_opinion_on_what_to_include_in_init_py/)
- [What do you have in your `__init__.py` files?](https://www.reddit.com/r/learnpython/comments/lgbxry/what_do_you_have_in_your_init_py_files/)

## When to use instance, class, and static methods

A quick recap of the various methods we can implement for a class:

- Instance methods need a class instance and can access the instance through `self`
- Class methods don’t need an instance. Thus, they can’t access the instance (`self`) but have access to the class (`cls`)
- Static methods don’t have access to `self` or `cls`. They work like regular functions but belong to the class namespace.

When should we use class or static methods? Here are some basic guidelines I found.

We use class methods when we want to call it without creating an instance of the class. This is usually when we don’t need instance information but need class information (i.e., its other class or static methods). We might also use class methods as a [constructor](https://realpython.com/instance-class-and-static-methods-demystified/#delicious-pizza-factories-with-classmethod). The benefit of class methods is that we don’t have to hardcode the class, thus allowing subclasses to use the methods too.

We use static methods when we don’t need class or instance arguments, but the method is related to the class and it is convenient for the method to be in the class’s namespace. For example, utility methods specific to the class. By decorating a method as a static method, we improve readability and understanding by telling others that the method doesn’t depend on the class or instance.

Further reading:

- [Python Class Methods](https://www.pythontutorial.net/python-oop/python-class-methods/) and [Python Static Methods](https://www.pythontutorial.net/python-oop/python-static-methods/)
- [What’s an example use case for a Python classmethod?](https://stackoverflow.com/a/5738586)
- [Meaning of @classmethod and @staticmethod for beginners?](https://stackoverflow.com/a/12179752)
- [Why do we use @staticmethod?](https://stackoverflow.com/a/23508293)
- [Python’s Instance, Class, and Static Methods Demystified](https://realpython.com/instance-class-and-static-methods-demystified/)

The common use of `conftest.py` is to [provide fixtures for the entire directory](https://docs.pytest.org/en/6.2.x/fixture.html#conftest-py-sharing-fixtures-across-multiple-files). By defining fixtures in `conftest.py`, they can be used by any test in the package without having to import them. Beyond that, it’s also used to [load external plugins](https://pytest.org/en/latest/how-to/plugins.html#requiring-loading-plugins-in-a-test-module-or-conftest-file) and [define hooks](https://docs.pytest.org/en/6.2.x/reference.html#hooks) such as setup and teardown methods.

However, while browsing sklearn, I came across an [empty `conftest.py`](https://github.com/scikit-learn/scikit-learn/blob/main/conftest.py) which had this interesting comment.

```python
# Even if empty this file is useful so that when running from the root folder
# ./sklearn is added to sys.path by pytest. See
# https://docs.pytest.org/en/latest/explanation/pythonpath.html for more
# details. For example, this allows to build extensions in place and run pytest
# doc/modules/clustering.rst and use sklearn from the local folder rather than
# the one from site-packages.
```

It turns out that sklearn was taking advantage of a lesser-known feature of `conftest.py`: By having it in the root path, it ensures that pytest recognizes the modules without having to specify the `PYTHONPATH`. In the background, pytest modifies the `sys.path` by including all submodules found in the root path.

Further reading:

- [pytest import mechanisms and sys.path/PYTHONPATH](https://docs.pytest.org/en/latest/explanation/pythonpath.html)
- [In pytest, what is the use of conftest.py files?](https://stackoverflow.com/a/34520971)

## Papers that explain a library’s design principles

Other than reading code, we can also learn by reading papers explaining a library. Let’s focus on the design principles of each library.

Scikit-learn’s [design principles](https://arxiv.org/abs/1309.0238) include (i) consistency, where all objects share a consistent interface composed of a limited set of methods, and (ii) composition where objects are implemented via existing building blocks wherever feasible.

As a result, most machine learning models and data transformers have a `fit()` method. In addition, machine learning models have a `predict()` method and data transformers have a `transform()` method. This consistency and simplicity contributes to sklearn’s ease of use. The principle of composition also explains why sklearn is built on multiple inheritance of base classes and mixins.

Another example is fastai which uses a [layered approach](https://www.mdpi.com/2078-2489/11/2/108/htm). It provides a high-level API that provides ready-to-use functionality to train models for various applications. The high-level API is built on a hierarchy of lower-level APIs which provide composable building blocks. This layered approach enables one to quickly build a prototype before customizing by tweaking the middle-layer APIs.

![Image](https://eugeneyan.com/assets/fastapi-layers.webp "Image")

PyTorch also shared its [design principles](https://arxiv.org/abs/1912.01703) such as (i) provide pragmatic performance and (ii) [worse is better](https://dreamsongs.com/RiseOfWorseIsBetter.html). The former states that, to be useful, a library needs to deliver compelling performance but not at the expense of ease of use. Thus, PyTorch is willing to trade off 10% speed, but not 100% speed, for a significantly simpler to use library. The latter states that it’s better to have a simple but slightly incomplete solution than a comprehensive but hard-to-maintain design.

Those are some of the uncommon usages of Python I’ve learned while reading several libraries such as [requests](https://github.com/psf/requests), [flask](https://github.com/pallets/flask), [fastapi](https://github.com/tiangolo/fastapi), [scikit-learn](https://github.com/scikit-learn/scikit-learn), [pytorch](https://github.com/pytorch/pytorch), [fastai](https://github.com/fastai/fastai), [pydantic](https://github.com/samuelcolvin/pydantic), and [django](https://github.com/django/django). I’m sure I only scratched the surface—did I miss anything? Please comment below!

If you found this useful, please cite this write-up as:

> Yan, Ziyou. (Jul 2022). Uncommon Uses of Python in Commonly Used Libraries. eugeneyan.com. https://eugeneyan.com/writing/uncommon-python/.

or

```
@article{yan2022python,
  title   = {Uncommon Uses of Python in Commonly Used Libraries},
  author  = {Yan, Ziyou},
  journal = {eugeneyan.com},
  year    = {2022},
  month   = {Jul},
  url     = {https://eugeneyan.com/writing/uncommon-python/}
}
```

Share on:

![](https://eugeneyan.com/assets/icon-twitter.svg)

![](https://eugeneyan.com/assets/icon-linkedin.svg)

![](https://eugeneyan.com/assets/bluesky.svg)

![](https://eugeneyan.com/assets/icon-facebook.svg)

![](https://eugeneyan.com/assets/icon-mail.svg)

Join **11,800+** readers getting updates on machine learning, RecSys, LLMs, and engineering.
