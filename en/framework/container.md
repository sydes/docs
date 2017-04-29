# Container

Extends the [Pimple](https://github.com/silexphp/Pimple) dependency injection container
and provides auto class resolving, instantiation and injection.

Implements PSR-11 container interface.

## Example

``` php
<?php
namespace Foo;

class Bar
{
    public function get()
    {
        return 'Bar';
    }
}

class Baz
{
    protected $bar;

    public function __construct(Bar $bar)
    {
        $this->bar = $bar;
    }

    public function get()
    {
        return $this->baz->get() . ' & Baz';
    }
}

app('Foo\Bar')->get(); // returns "Bar"
app('Foo\Baz')->get(); // returns "Bar & Baz"
```

## Auto resolving class instances

Auto resolving enables the _magic_ of instantiating a class by merely naming them as a container key:

``` php
<?php

app('Foo\Baz')->get();
```

Auto resolving can be disabled by either setting the container key manually before fetching it or by constructor parameter

``` php
<?php

// auto resolving only works on non existing keys!

app()->set('Foo\Bar', 'bla');
echo app('Foo\Bar'); // still "bla"
```



## Limitations

### Circular dependencies

Container features a simplistic circular dependency recognition. However it's limited in it's capabilities, especially in the context of constructor parameter order, see above

The following will be recognized and a `RuntimeException` is thrown:

``` php
<?php

class Foo {
    public function __construct(Bar $bar) {}
}

class Bar {
    public function __construct(Baz $baz) {}
}

class Baz {
    public function __construct(Foo $foo) {}
}
```
