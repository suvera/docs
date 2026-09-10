# Source: https://mintlify.wiki/suvera/winter-boot/introduction

[Skip to main content](https://mintlify.wiki/suvera/winter-boot/introduction#content-area)

[Winter Boot home page\\ \\ Winter Boot](https://mintlify.wiki/suvera/winter-boot)

Search...

Ctrl K

Search...

Navigation

Getting Started

Introduction to Winter Boot PHP Microservices Framework

[Guide](https://mintlify.wiki/suvera/winter-boot/introduction)

##### Getting Started

- [Introduction](https://mintlify.wiki/suvera/winter-boot/introduction)
- [Quickstart](https://mintlify.wiki/suvera/winter-boot/quickstart)
- [Configuration](https://mintlify.wiki/suvera/winter-boot/configuration)

##### Core Concepts

- [Dependency Injection](https://mintlify.wiki/suvera/winter-boot/core/dependency-injection)
- [AOP](https://mintlify.wiki/suvera/winter-boot/core/aop)
- [App Lifecycle](https://mintlify.wiki/suvera/winter-boot/core/application-lifecycle)

##### Web & REST

- [Controllers](https://mintlify.wiki/suvera/winter-boot/web/rest-controllers)
- [Request Mapping](https://mintlify.wiki/suvera/winter-boot/web/request-mapping)
- [Interceptors](https://mintlify.wiki/suvera/winter-boot/web/interceptors)

##### Data Access

- [Database](https://mintlify.wiki/suvera/winter-boot/data/database)
- [Transactions](https://mintlify.wiki/suvera/winter-boot/data/transactions)
- [Migrations](https://mintlify.wiki/suvera/winter-boot/data/migrations)

##### Async & Scheduling

- [Async Tasks](https://mintlify.wiki/suvera/winter-boot/async/async-tasks)
- [Scheduling](https://mintlify.wiki/suvera/winter-boot/async/scheduling)
- [Daemon Threads](https://mintlify.wiki/suvera/winter-boot/async/daemon-threads)

##### Operations

- [Caching](https://mintlify.wiki/suvera/winter-boot/ops/caching)
- [Locking](https://mintlify.wiki/suvera/winter-boot/ops/locking)
- [Logging](https://mintlify.wiki/suvera/winter-boot/ops/logging)
- [Actuator](https://mintlify.wiki/suvera/winter-boot/ops/actuator)
- [Telemetry](https://mintlify.wiki/suvera/winter-boot/ops/telemetry)

##### Advanced

- [Modules](https://mintlify.wiki/suvera/winter-boot/advanced/modules)
- [JSON & XML](https://mintlify.wiki/suvera/winter-boot/advanced/json-xml)
- [Local Stores](https://mintlify.wiki/suvera/winter-boot/advanced/local-stores)
- [Build & Deploy](https://mintlify.wiki/suvera/winter-boot/advanced/build-deploy)

close

On this page

- [What is Winter Boot?](https://mintlify.wiki/suvera/winter-boot/introduction#what-is-winter-boot)
- [Spring Boot Inspiration](https://mintlify.wiki/suvera/winter-boot/introduction#spring-boot-inspiration)
- [PHP 8 Attributes and Dependency Injection](https://mintlify.wiki/suvera/winter-boot/introduction#php-8-attributes-and-dependency-injection)
- [Aspect-Oriented Programming](https://mintlify.wiki/suvera/winter-boot/introduction#aspect-oriented-programming)
- [Microservices Focus](https://mintlify.wiki/suvera/winter-boot/introduction#microservices-focus)
- [Key Features](https://mintlify.wiki/suvera/winter-boot/introduction#key-features)

> ## Documentation Index
> 
> Fetch the complete documentation index at: [https://mintlify.com/suvera/winter-boot/llms.txt](https://mintlify.com/suvera/winter-boot/llms.txt)
> 
> Use this file to discover all available pages before exploring further.

Winter Boot is a modern PHP framework purpose-built for microservice development, drawing deep inspiration from Spring Boot’s philosophy of convention over configuration. It brings first-class support for PHP 8 attributes (formerly annotations) to deliver a declarative, annotation-driven programming model — covering dependency injection, AOP-based cross-cutting concerns, REST API routing, async task execution, and more — so that teams already familiar with the Java/Spring ecosystem can be productive in PHP immediately.

## 

[​](https://mintlify.wiki/suvera/winter-boot/introduction#what-is-winter-boot)

What is Winter Boot?

Winter Boot turns a plain PHP class into a fully managed application context with a single attribute. The framework scans your namespaces at startup, registers beans, wires dependencies, maps HTTP routes via Swoole, and exposes every cross-cutting concern (caching, transactions, async, scheduling) through attributes rather than boilerplate configuration files. The result is lean, readable service code that focuses on business logic instead of framework plumbing.

Application.php

```
<?php

use dev\winterframework\stereotype\WinterBootApplication;
use dev\winterframework\core\app\WinterWebSwooleApplication;

#[WinterBootApplication(
    configDirectory: [__DIR__ . '/config'],
    scanNamespaces: [['com\\example\\myapp', __DIR__ . '/src']]
)]
class MyApplication
{
    public static function main(): void
    {
        (new WinterWebSwooleApplication())->run(MyApplication::class);
    }
}

MyApplication::main();
```

Winter Boot requires **PHP 8.4 or later**. The Swoole extension (`pecl install swoole`) is required for the built-in HTTP server (`WinterWebSwooleApplication`) and is strongly recommended for async functions (`#[Async]`) and scheduled tasks (`#[Scheduled]`). All other framework features work without Swoole.

## 

[​](https://mintlify.wiki/suvera/winter-boot/introduction#spring-boot-inspiration)

Spring Boot Inspiration

If you have built services with Spring Boot, Winter Boot will feel immediately familiar. Stereotype attributes like `#[Service]`, `#[RestController]`, `#[Autowired]`, `#[Value]`, `#[Configuration]`, and `#[Bean]` map directly to their Spring counterparts. The application context lifecycle, property externalisation via `application.yml`, and the concept of a single annotated entry-point class all follow the same patterns — only the language changes.

## 

[​](https://mintlify.wiki/suvera/winter-boot/introduction#php-8-attributes-and-dependency-injection)

PHP 8 Attributes and Dependency Injection

Winter Boot’s DI container is built entirely on native PHP 8 attributes. There are no XML files, no code-generation steps, and no service locators to call manually. Declare a class with `#[Service]` or `#[Component]`, mark a property with `#[Autowired]`, and the container resolves the dependency graph automatically at startup.

UserServiceImpl.php

```
<?php

use dev\winterframework\stereotype\Service;
use dev\winterframework\stereotype\Autowired;

#[Service]
class UserServiceImpl implements UserService
{
    #[Autowired]
    private UserRepository $repository;

    public function findById(int $id): User
    {
        return $this->repository->findById($id);
    }
}
```

## 

[​](https://mintlify.wiki/suvera/winter-boot/introduction#aspect-oriented-programming)

Aspect-Oriented Programming

Cross-cutting concerns such as caching, transactions, retries, and custom interceptors are expressed as attributes applied to methods and classes. The AOP weaving happens at container startup — no proxy code to write, no separate aspect files to maintain.

## 

[​](https://mintlify.wiki/suvera/winter-boot/introduction#microservices-focus)

Microservices Focus

Winter Boot is optimised for the microservice deployment model: stateless HTTP workers managed by Swoole, an optional in-process key-value store and task queue, built-in Prometheus metrics via the Actuator, and an extensible module system for integrations with Redis, Kafka, Doctrine ORM, Amazon SQS/S3, OpenSearch, and service-discovery solutions such as Consul and Netflix Eureka.

## 

[​](https://mintlify.wiki/suvera/winter-boot/introduction#key-features)

Key Features

[**Quickstart**\\ \\ Install the package, write your first service and REST controller, and have a running HTTP server in minutes.](https://mintlify.wiki/suvera/winter-boot/quickstart)

[**Dependency Injection**\\ \\ Attribute-driven DI with `#[Service]`, `#[Component]`, `#[Autowired]`, `#[Qualifier]`, and `#[Bean]` factories.](https://mintlify.wiki/suvera/winter-boot/core/dependency-injection)

[**REST Controllers**\\ \\ Map HTTP routes with `#[RestController]`, `#[RequestMapping]`, `#[GetMapping]`, `#[PostMapping]`, and more.](https://mintlify.wiki/suvera/winter-boot/web/rest-controllers)

[**AOP & Custom Stereotypes**\\ \\ Implement cross-cutting concerns with custom attributes and aspect-oriented interceptors.](https://mintlify.wiki/suvera/winter-boot/core/aop)

[**Databases & Transactions**\\ \\ Manage datasources, run SQL migrations, and control transactions declaratively with `#[Transactional]`.](https://mintlify.wiki/suvera/winter-boot/data/database)

[**Caching**\\ \\ Add response and method-level caching with a single attribute, backed by any cache provider.](https://mintlify.wiki/suvera/winter-boot/ops/caching)

[**Async & Scheduling**\\ \\ Run background work with `#[Async]` and cron-style scheduling with `#[Scheduled]`.](https://mintlify.wiki/suvera/winter-boot/async/async-tasks)

[**Modules & Extensions**\\ \\ Extend the framework with community modules or build your own by implementing `WinterModule`.](https://mintlify.wiki/suvera/winter-boot/advanced/modules)

[Quickstart: Build and Run Your First Winter Boot Service\\ \\ Next](https://mintlify.wiki/suvera/winter-boot/quickstart)

Ctrl+I

Rate your experience

How was the docs repo generation?

12345

PoorExcellent

## Build docs developers (and LLMs) love

[Get started for free](https://dashboard.mintlify.com/signup?utm_campaign=atlas_referral&utm_medium=suvera-winter-boot&utm_source=atlas_signup) [Talk to us](https://www.mintlify.com/contact/sales?utm_campaign=atlas_referral&utm_medium=suvera-winter-boot&utm_source=atlas_sales)