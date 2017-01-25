# Examples

### simple class definition

    class Car {

    }

### class definition with properties


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

### class definition with methods


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
