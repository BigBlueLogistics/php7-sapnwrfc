
# SAP NW RFC extension for PHP 7

This extension is intended to provide a means for PHP 7 developers to interface with the SAP NetWeaver SDK.

## Install SAP NetWeaver RFC SDK (required always!)

To ***build or install*** you need always the *SAP NetWeaver RFC SDK* from the *SAP Service Marketplace* first!
You can [follow the instructions here](docs/installing_nwrfcsdk.md).

## Building unix/windows

Please [see the build instructions](docs/build.md)

## Installing

Add `extension=sapnwrfc.so` to your `php.ini` (for windows `extension=php_sapnwrfc.dll`) 

You can verify that the extension is loaded by running `php -m | grep sapnwrfc`.

## Usage

This simple example shows how to call a (fictional) RFC function and get its return value:

```php
use SAPNWRFC\Connection as SapConnection;
use SAPNWRFC\Exception as SapException;

$config = [
    'ashost' => 'my.sap.system.local',
    'sysnr'  => '00',
    'client' => '123',
    'user' => 'DEMO',
    'passwd' => 'XXXX',
    'trace'  => SapConnection::TRACE_LEVEL_OFF,
];

try {
    $c = new SapConnection($config);

    $f = $c->getFunction('Z_TEST_FUNCTION');
    $result = $f->invoke([
        'CHAR1' => 'A',
        'TABL' => [
            ['INT4' => 1, 'CHAR4' => 'NOPE'],
        ]
    ]);

    var_dump($result);
} catch(SapException $ex) {
    echo 'Exception: ' . $ex->getMessage() . PHP_EOL;
    
    /*
     * You could also catch \SAPNWRFC\ConnectionException and \SAPNWRFC\FunctionCallException
     * separately if you want to.
     */
}
```

See [documentation](docs/readme.md) for details.

## Contributing

Contribution to the project (be it by reporting/fixing bugs, writing documentaton, helping with testing) is very welcome.
Just open up an issue or a PR.

## License

This software is licensed under the MIT license. See [LICENSE](LICENSE) for details.

## Legal notice

SAP and other SAP products and services mentioned herein are trademarks or registered trademarks of SAP SE (or an SAP affiliate company) in Germany and other countries.
