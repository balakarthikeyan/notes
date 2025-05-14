What has changed in Symfony 7?

## 1. PHP Version Requirements

Symfony 7 requires a higher PHP version, which allows for more modern language features.

> Symfony 6

```json
// composer.json
{
    "require": {
        "php": ">=8.0.2",
        "symfony/symfony": "^6.0"
    }
}
```

> Symfony 7:

```json
// composer.json
{
    "require": {
        "php": ">=8.2",
        "symfony/symfony": "^7.0"
    }
}
```

## 2. Routing

Symfony 7 fully embraces PHP 8 attributes for routing, replacing the older annotation style.

> Symfony 6:

```php
use Symfony\Component\Routing\Annotation\Route;
 
class ProductController
{
    /**
     * @Route("/product/{id}", name="product_show")
     */
    public function show($id)
    {
        // ...
    }
}
```

> Symfony 7:

```php
use Symfony\Component\Routing\Annotation\Route;
class ProductController
{
    #[Route('/product/{id}', name: 'product_show')]
    public function show($id)
    {
        // ...
    }
}
```

## 3. Security

Symfony 7 simplifies the security configuration by removing the `anonymous` key and using `lazy` directly under the firewall.

> Symfony 6:

```yaml
# config/packages/security.yaml
security:
    firewalls:
        main:
            anonymous: lazy
```

> Symfony 7:

```yaml
# config/packages/security.yaml
security:
    firewalls:
        main:
            lazy: true
```

## 4. Forms

Symfony 7 enforces stricter type hints, including return type declarations for methods like `buildForm`.

> Symfony 6:

```php
class ProductType extends AbstractType
{
    public function buildForm(FormBuilderInterface $builder, array $options)
    {
        // ...
    }
}
```

> Symfony 7:

```php
class ProductType extends AbstractType
{
    public function buildForm(FormBuilderInterface $builder, array $options): void
    {
        // ...
    }
}
```

## 5. Dependency Injection

Symfony 7 simplifies service configuration by removing the `$` prefix for named arguments.

> Symfony 6:

```yaml
# config/services.yaml
services:
    App\Service\MyService:
        arguments:
            $someArgument: '%some_parameter%'
```

> Symfony 7:

```yaml
# config/services.yaml
services:
    App\Service\MyService:
        arguments:
            someArgument: '%some_parameter%'
```

## 6. Console

Symfony 7 introduces the `AsCommand` attribute for console commands, replacing the `configure` method.


> Symfony 6:

```php
class MyCommand extends Command
{
    protected function configure()
    {
        $this->setName('app:my-command');
    }
 
    protected function execute(InputInterface $input, OutputInterface $output)
    {
        // ...
        return Command::SUCCESS;
    }
}
```

> Symfony 7:

```php
use Symfony\Component\Console\Attribute\AsCommand;
 
#[AsCommand(name: 'app:my-command')]
class MyCommand extends Command
{
    protected function execute(InputInterface $input, OutputInterface $output): int
    {
        // ...
        return Command::SUCCESS;
    }
}
```

## 7. Doctrine Integration

Both Symfony 6 and 7 use attributes for Doctrine ORM mapping, but Symfony 7 may enforce stricter type hints.

```php
use Doctrine\ORM\Mapping as ORM;
 
#[ORM\Entity]
class Product
{
    #[ORM\Id]
    #[ORM\GeneratedValue]
    #[ORM\Column(type: 'integer')]
    private $id;
 
    #[ORM\Column(type: 'string', length: 255)]
    private $name;
 
    // ...
}
```

## 8. Testing

Symfony 7 encourages the use of more specific test case classes and may introduce new testing utilities.

```php
use Symfony\Bundle\FrameworkBundle\Test\KernelTestCase;
 
class ProductTest extends KernelTestCase
{
    public function testSomething(): void
    {
        $kernel = self::bootKernel();
        $container = static::getContainer();
 
        // ...
    }
}
```

## 9. Error Handling

Error handling remains similar, but Symfony 7 may introduce new exception classes or error handling features.

```php
use Symfony\Component\HttpKernel\Exception\NotFoundHttpException;
 
public function show($id)
{
    $product = // ... fetch product
    if (!$product) {
        throw new NotFoundHttpException('Product not found');
    }
    // ...
}
```

## 10. Configuration

Configuration files remain largely similar, but Symfony 7 may introduce new options or deprecate old ones.

```yaml
# config/packages/framework.yaml
framework:
    secret: '%env(APP_SECRET)%'
    #csrf_protection: true
    http_method_override: false
```

## 11. Internationalization

The translation system remains similar, but Symfony 7 may introduce new translation features or optimizations.

```php
use Symfony\Contracts\Translation\TranslatorInterface;
 
class ProductController
{
    public function __construct(private TranslatorInterface $translator) {}
 
    public function show($id)
    {
        $message = $this->translator->trans('product.not_found', ['id' => $id]);
        // ...
    }
}
```