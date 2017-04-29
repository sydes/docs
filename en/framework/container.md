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

## Additional constructor parameters

To use additional constructor parameters, you can set them with the `createArgs` parameter or with the `setCreateArgs` method

``` php
<?php

namespace Foo\Bar;

class Baz
{
    protected $param;
    public function __construct($param)
    {
        $this->param = $param;
    }
    public function hello()
    {
        echo "Hello {$this->param}\n";
    }
}

class Bla
{
    protected $param;
    public function __construct(Baz $baz, $param)
    {
        $this->param = $param;
    }
    public function hello()
    {
        echo "Hello {$this->param}\n";
    }
}

class Yadda
{
    protected $scalar;
    public function __construct(Baz $baz, $scalar, Bla $bla)
    {
        $this->scalar = $scalar;
    }
    public function hello()
    {
        echo "Hello {$this->scalar}\n";
        $this->bla->Hello();
    }
}

$container = new Wart(array(), array(
    'createArgs' => array(
        'Foo\Bar\Baz'   => ['world'],
        'Foo\Bar\Bla'   => ['you'],
        'Foo\Bar\Yadda' => function (array $createArgs, $className, \Wart $cnt) {
            // $createArgs contains the constructor parameters Wart has already figured out (i.e. all with class type hints)
            // Must return an array with all constructor args
            if ($buildArgs && $buildArgs[0] instanceof \Foo\Bar\Baz) use ($container) { # Wart found
                return [$buildArgs[0], "!", $cnt['Foo\Bar\Baz']];
            }
            throw new \Exception("Oh no!");
        }
    )
));
$container->register('Foo\Bar\Baz');   # does not return anything and does NOT create an object instance just yet
$container['Foo\Bar\Baz']->hello();    # prints "Hello world\n"
$container['Foo\Bar\Bla']->hello();    # prints "Hello you\n"
$container['Foo\Bar\Yadda']->hello();  # prints "Hello !\nHello you\n"
```

You can also set (replace!) the constructor parameters later on with `setCreateArgs`

``` php
<?php

$container = new Wart;
$container->setCreateArgs(array(
    'Foo\Bar\Baz'   => ['world']
));
```

## Limitations

### Constructor parameter signature order

Mind that Container supports only auto determines constructor args which are type hinted classes.

The following Container tries to resolve and inject:
``` php
<?php

class Foo
{
    public function __construct(Bar $bar) {}
}
```

The following Container cannot does _not_ inject, unless you use the `setCreateArgs` method or the `createArgs` argument (see above).
``` php
<?php

class Foo
{
    public function __construct($bar) {}
}
```

Now if the constructor parameter signature order is "adverse", Container cannot resolve it either. The following will _not_ (automatically) work:

``` php
<?php

class Bla
{
    public function __construct($param, Baz $baz) # << Baz $baz is on second position, Wart gives up on first as it's not typehinted to a class
    {
    }
}
```

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
