# Examples

### simple class definition

```php
<?php

class Car {

}

```

### class definition with properties

```php
<?php

class Car {

    /*
    * @var string licensePlate
    */
    public $licensePlate;

    /*
    * @var int $numberOfWheels
    */
    private $numberOfWheels;

    /*
    * @var string $color
    */

    protected $color = 'blue';

}

```
### class definition with methods

```php
<?php

 class Car {

    public $wheels;

    /*
    * Starts the car 
    */
    public function startEngine()
    {
        //
    }

}

```
