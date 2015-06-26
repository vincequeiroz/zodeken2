# Usage #

1. Put zodeken2.php to root directory of your project.

```
/your-project/
----/config/
----/data/
----/module/
----/public/
----/vendor/
----/composer.json
----[...]
----/zodeken2.php <-- Put it here
```

2. Configure table list for each module in zodeken2.ini file

3. Run php zodeken2.php in terminal. It will read your db configs and ask you for the module name. The 'php' command not found? Please install the php5-cli or php-cli for Linux or add the directory of your php.exe to your system path for Windows.

4. Enter your module name and wait.

5. Models, Mappers and ModelFactory will be created in `module/YourModule/src/YourModule/Model`.

6. Open your Module.php and alter the onBootstrap method.

```
    public function onBootstrap(MvcEvent $e)
    {
        Model\ModelFactory::getInstance()->setServiceLocator($e->getApplication()->getServiceManager());
        [...]
    }
```

7. Use it in your code.

Get a Mapper object:
```
$mapper = \YourModule\Model\ModelFactory::getInstance()->getYourTableNameMapper()
```
Construct a Model:
```
$model = new \YourModule\Model\YourTableName\YourTableNameModel;
$model->setSomeField($someField);
$model->setAnotherField($anotherField);
```
Save the model:
```
$mapper->saveYourTableName($model);
```
Get a model set from database:
```
$mapper->getYourTableNameSetBySomeField($someField);...
```

# Important notes #

  1. Do not make any change to Model and ModelFactory files because they will always be overwritten.
  1. Do not alter existing methods below the construct() in Mapper classes.
  1. Put your custom methods into Mapper classes between `protected $serviceLocator;` and public function `__construct(TableGateway $tableGateway)` like this:
```
    /**
     * @var ServiceLocatorInterface
     */
    protected $serviceLocator;

    public function myCustomMethod($blahblah)
    {
        
    }

    public function myCustomMethodTwo($blahblah)
    {
        
    }

    public function __construct(TableGateway $tableGateway)
    {
        $this->tableGateway = $tableGateway;
    }
```
If you change your tables, Zodeken2 will search for your custom methods and copy them to the new class automatically.