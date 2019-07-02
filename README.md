# OpenSNS-BeterMetElkaar-SSOLaunchSamenBeter-PHP

SNS Launch protocol code example in PHP.

This repository provides PHP code examples on how to implement the [SNS Launch protocol](https://github.com/GidsOpenStandaarden/OpenSNS-BeterMetElkaar-SSOLaunchSamenBeter-Protocol).

# composer.json

```json
{
    "name": "othillo/sns-launch-demo-php",
    "type": "project",
    "require": {
        "lcobucci/jwt": "^3.2",
        "broadway/uuid-generator": "^0.4.0",
        "ramsey/uuid": "^3.8"
    },
    "require-dev": {
        "phpunit/phpunit": "^8.0"
    },
    "license": "MIT"
}
```

# Generate a JSON Web Token

```php
<?php

require_once __DIR__ . '/../vendor/autoload.php';

use Broadway\UuidGenerator\Rfc4122\Version4Generator;
use Lcobucci\JWT\Builder;
use Lcobucci\JWT\Signer\Rsa\Sha256;

$signer = new Sha256();

$privateKeyPem = "-----BEGIN RSA PRIVATE KEY-----\n[INSERT PRIVATE KEY]\n-----END RSA PRIVATE KEY-----";

$token = (new Builder())
    ->setIssuer('issuer.nl')
    ->setAudience('audience.nl')
    ->setId((new Version4Generator())->generate(), true)
    ->setIssuedAt(time())
    ->setExpiration(time() + 5*60)

    ->set('sub', 'urn:sns:user:issuer.nl:123456')
    ->set('resource_id', 'dagstructuur')
    ->set('first_name', 'Klaas')
    ->set('middle_name', 'de')
    ->set('last_name', 'Vries')
    ->set('email', 'klaas@devries.nl')

    ->sign($signer, $privateKeyPem)
    ->getToken();

echo $token;
