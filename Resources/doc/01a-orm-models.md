Concrete classes for Doctrine ORM
=================================

This page lists some example implementations of FOSMessageBundle models for the Doctrine
ORM.

Given the examples below with their namespaces and class names, you need to configure
FOSMessageBundle to tell them about these classes.

Add the following to your `app/config/config.yml` file.

```yaml
# app/config/config.yml

fos_message:
    db_driver: orm
    thread_class: AppBundle\Entity\Thread
    message_class: AppBundle\Entity\Message
```

[Continue with the installation][]

Annotations version
=================================

Message class
-------------

```php
<?php
// src/AppBundle/Entity/Message.php

namespace AppBundle\Entity;

use Doctrine\ORM\Mapping as ORM;
use Doctrine\Common\Collections\Collection;
use FOS\MessageBundle\Entity\Message as BaseMessage;

/**
 * @ORM\Entity
 */
class Message extends BaseMessage
{
    /**
     * @ORM\Id
     * @ORM\Column(type="integer")
     * @ORM\GeneratedValue(strategy="AUTO")
     */
    protected $id;

    /**
     * @ORM\ManyToOne(
     *   targetEntity="AppBundle\Entity\Thread",
     *   inversedBy="messages"
     * )
     * @var \FOS\MessageBundle\Model\ThreadInterface
     */
    protected $thread;

    /**
     * @ORM\ManyToOne(targetEntity="AppBundle\Entity\User")
     * @var \FOS\MessageBundle\Model\ParticipantInterface
     */
    protected $sender;

    /**
     * @ORM\OneToMany(
     *   targetEntity="AppBundle\Entity\MessageMetadata",
     *   mappedBy="message",
     *   cascade={"all"}
     * )
     * @var MessageMetadata[]|Collection
     */
    protected $metadata;
}
```

MessageMetadata class
---------------------

```php
<?php
// src/AppBundle/Entity/MessageMetadata.php

namespace AppBundle\Entity;

use Doctrine\ORM\Mapping as ORM;
use FOS\MessageBundle\Entity\MessageMetadata as BaseMessageMetadata;

/**
 * @ORM\Entity
 */
class MessageMetadata extends BaseMessageMetadata
{
    /**
     * @ORM\Id
     * @ORM\Column(type="integer")
     * @ORM\GeneratedValue(strategy="AUTO")
     */
    protected $id;

    /**
     * @ORM\ManyToOne(
     *   targetEntity="AppBundle\Entity\Message",
     *   inversedBy="metadata"
     * )
     * @var \FOS\MessageBundle\Model\MessageInterface
     */
    protected $message;

    /**
     * @ORM\ManyToOne(targetEntity="AppBundle\Entity\User")
     * @var \FOS\MessageBundle\Model\ParticipantInterface
     */
    protected $participant;
}
```

Thread class
------------

```php
<?php
// src/AppBundle/Entity/Thread.php

namespace AppBundle\Entity;

use Doctrine\ORM\Mapping as ORM;
use Doctrine\Common\Collections\Collection;
use FOS\MessageBundle\Entity\Thread as BaseThread;

/**
 * @ORM\Entity
 */
class Thread extends BaseThread
{
    /**
     * @ORM\Id
     * @ORM\Column(type="integer")
     * @ORM\GeneratedValue(strategy="AUTO")
     */
    protected $id;

    /**
     * @ORM\ManyToOne(targetEntity="AppBundle\Entity\User")
     * @var \FOS\MessageBundle\Model\ParticipantInterface
     */
    protected $createdBy;

    /**
     * @ORM\OneToMany(
     *   targetEntity="AppBundle\Entity\Message",
     *   mappedBy="thread"
     * )
     * @var Message[]|Collection
     */
    protected $messages;

    /**
     * @ORM\OneToMany(
     *   targetEntity="AppBundle\Entity\ThreadMetadata",
     *   mappedBy="thread",
     *   cascade={"all"}
     * )
     * @var ThreadMetadata[]|Collection
     */
    protected $metadata;
}
```

ThreadMetadata class
--------------------

```php
<?php
// src/AppBundle/Entity/ThreadMetadata.php

namespace AppBundle\Entity;

use Doctrine\ORM\Mapping as ORM;
use FOS\MessageBundle\Entity\ThreadMetadata as BaseThreadMetadata;

/**
 * @ORM\Entity
 */
class ThreadMetadata extends BaseThreadMetadata
{
    /**
     * @ORM\Id
     * @ORM\Column(type="integer")
     * @ORM\GeneratedValue(strategy="AUTO")
     */
    protected $id;

    /**
     * @ORM\ManyToOne(
     *   targetEntity="AppBundle\Entity\Thread",
     *   inversedBy="metadata"
     * )
     * @var \FOS\MessageBundle\Model\ThreadInterface
     */
    protected $thread;

    /**
     * @ORM\ManyToOne(targetEntity="AppBundle\Entity\User")
     * @var \FOS\MessageBundle\Model\ParticipantInterface
     */
    protected $participant;
}
```
XML Version
=================================

Message class
-------------

```xml
<?xml version="1.0" encoding="utf-8"?>
<doctrine-mapping xmlns="http://doctrine-project.org/schemas/orm/doctrine-mapping" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://doctrine-project.org/schemas/orm/doctrine-mapping http://doctrine-project.org/schemas/orm/doctrine-mapping.xsd">
  <entity repository-class="AppBundle\Repository\MessageRepository" name="AppBundle\Entity\Message">
    <id name="id" type="integer" column="id">
      <generator strategy="AUTO"/>
    </id>
    <many-to-one field="thread" target-entity="AppBundle\Entity\Thread" inversed-by="messages"/>
    <many-to-one field="sender" target-entity="AppBundle\Entity\User" />
    <one-to-many field="metadata" target-entity="AppBundle\Entity\MessageMetadata" mapped-by="message">
        <cascade>
            <all/>
        </cascade>
    </one-to-many>
  </entity>
</doctrine-mapping>
```

```php
<?php

namespace AppBundle\Entity;

use Doctrine\Common\Collections\Collection;
use FOS\MessageBundle\Entity\Message as BaseMessage;

/**
 * Message
 */
class Message extends BaseMessage
{
    /**
     * @var integer
     */
    protected $id;

    /**
     * @var MessageMetadata[]|Collection
     */
    protected $metadata;

    /**
     * @var \FOS\MessageBundle\Model\ThreadInterface
     */
    protected $thread;

    /**
     * @var \FOS\MessageBundle\Model\ParticipantInterface
     * 
     */
    protected $sender;

}

```


MessageMetadata class
---------------------

```xml
<?xml version="1.0" encoding="utf-8"?>
<doctrine-mapping xmlns="http://doctrine-project.org/schemas/orm/doctrine-mapping" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://doctrine-project.org/schemas/orm/doctrine-mapping http://doctrine-project.org/schemas/orm/doctrine-mapping.xsd">
  <entity repository-class="AppBundle\Repository\MessageMetadataRepository" name="AppBundle\Entity\MessageMetadata">
    <id name="id" type="integer" column="id">
      <generator strategy="AUTO"/>
    </id>
    <many-to-one field="message" target-entity="AppBundle\Entity\Message" inversed-by="metadata"/>
    <many-to-one field="participant" target-entity="AppBundle\Entity\User" />
  </entity>
</doctrine-mapping>
```

```php
<?php

namespace AppBundle\Entity;

use FOS\MessageBundle\Entity\MessageMetadata as BaseMessageMetadata;

/**
 * MessageMetadata
 */
class MessageMetadata extends BaseMessageMetadata
{
    /**
     * @var integer
     */
    protected $id;

    /**
     * @var \FOS\MessageBundle\Model\MessageInterface
     */
    protected $message;

    /**
     * @var \FOS\MessageBundle\Model\ParticipantInterface
     */
    protected $participant;

}
```

Thread class
------------

```xml
<?xml version="1.0" encoding="utf-8"?>
<doctrine-mapping xmlns="http://doctrine-project.org/schemas/orm/doctrine-mapping" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://doctrine-project.org/schemas/orm/doctrine-mapping http://doctrine-project.org/schemas/orm/doctrine-mapping.xsd">
  <entity repository-class="AppBundle\Repository\ThreadRepository" name="AppBundle\Entity\Thread">
    <id name="id" type="integer" column="id">
      <generator strategy="AUTO"/>
    </id>    
    <many-to-one field="createdBy" target-entity="AppBundle\Entity\User" />
    <one-to-many field="messages" target-entity="AppBundle\Entity\Message" mapped-by="thread"/>
    <one-to-many field="metadata" target-entity="AppBundle\Entity\ThreadMetadata" mapped-by="thread">
        <cascade>
            <all/>
        </cascade>
    </one-to-many>
  </entity>
</doctrine-mapping>
```

```php
<?php

namespace AppBundle\Entity;

use Doctrine\Common\Collections\Collection;
use FOS\MessageBundle\Entity\Thread as BaseThread;

/**
 * Thread
 */
class Thread extends BaseThread
{
    /**
     * @var integer
     */
    protected $id;

    /**
     * @var Message[]|Collection
     */
    protected $messages;

    /**
     * @var ThreadMetadata[]|Collection
     */
    protected $metadata;

    /**
     * @var \FOS\MessageBundle\Model\ParticipantInterface
     */
    protected $createdBy;

}
```
ThreadMetadata class
--------------------

```xml
<?xml version="1.0" encoding="utf-8"?>
<doctrine-mapping xmlns="http://doctrine-project.org/schemas/orm/doctrine-mapping" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://doctrine-project.org/schemas/orm/doctrine-mapping http://doctrine-project.org/schemas/orm/doctrine-mapping.xsd">
  <entity repository-class="AppBundle\Repository\ThreadMetadataRepository" name="AppBundle\Entity\ThreadMetadata">
    <id name="id" type="integer" column="id">
      <generator strategy="AUTO"/>
    </id>
    <many-to-one field="thread" target-entity="AppBundle\Entity\Thread" inversed-by="metadata"/>
    <many-to-one field="participant" target-entity="AppBundle\Entity\User" />
  </entity>
</doctrine-mapping>

```

```php
<?php

namespace AppBundle\Entity;

use FOS\MessageBundle\Entity\ThreadMetadata as BaseThreadMetadata;

/**
 * ThreadMetadata
 */
class ThreadMetadata extends BaseThreadMetadata
{
    /**
     * @var integer
     */
    protected $id;

    /**
     * @var \FOS\MessageBundle\Model\ThreadInterface
     */
    protected $thread;

    /**
     * @var \FOS\MessageBundle\Model\ParticipantInterface
     */
    protected $participant;

}

```
[Continue with the installation]: 01-installation.md
