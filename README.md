Finite, A Simple PHP Finite State Machine
=========================================

Finite is a Simple State Machine, written in PHP. It can manage any Stateful object by defining states and transitions between these states.

[![Build Status](https://travis-ci.org/mytskine/Finite.svg?branch=master)](https://travis-ci.org/mytskine/Finite)
[![Latest Stable Version](https://poser.pugx.org/mytskine/finite/v/stable.png)](https://packagist.org/packages/mytskine/finite)
[![License](https://poser.pugx.org/mytskine/finite/license.png)](https://packagist.org/packages/mytskine/finite)

Features
--------

* Managing State/Transition graph for an object
* Defining and retrieving properties for states
* Event Listenable transitions
* Symfony2 integration
* Custom state graph loaders
* Twig Extension

Documentation
-------------

[Documentation for master (1.1)](http://finite.readthedocs.org/en/master/)

Getting started
---------------

### Installation (via composer)

```
composer require mytskine/finite
```

### Define your Stateful Object

Your stateful object has to implement the `StatefulInterface` Interface.

```php
use Finite\StatefulInterface;

class Document implements StatefulInterface
{
        private $state;

        public function setFiniteState($state)
        {
                $this->state = $state;
        }

        public function getFiniteState()
        {
            return $this->state;
        }
}
```

### Initializing a simple StateMachine

```php
use Finite\StateMachine\StateMachine;
use Finite\State\State;
use Finite\State\StateInterface;

$sm = new StateMachine();

// Define states
$sm->addState(new State('s1', StateInterface::TYPE_INITIAL));
$sm->addState('s2');
$sm->addState('s3');
$sm->addState(new State('s4', StateInterface::TYPE_FINAL));

// Define transitions
$sm->addTransition('t12', 's1', 's2');
$sm->addTransition('t23', 's2', 's3');
$sm->addTransition('t34', 's3', 's4');
$sm->addTransition('t42', 's4', 's2');

// Initialize
$document = getMyStatefulObject();
$sm->setObject($document);
$sm->initialize();

// Retrieve current state
$state = $sm->getCurrentState();

// Can we apply a transition?
$sm->can('t34');

// Apply a transition
$sm->apply('t34');

```

