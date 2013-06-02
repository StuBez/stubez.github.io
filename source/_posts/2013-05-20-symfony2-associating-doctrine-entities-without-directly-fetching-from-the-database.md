---
layout: post
title: Symfony2- associating Doctrine entities without (directly) fetching from the database
categories: programming
---

Several times I've want to associate an entity with a known primary key, such as a status, with an entity that I've already fetched from the database and need to update, without having to make another manual request before I can update the parent entity.

<!-- more -->

The Doctrine `EntityManager` has a handy method called `getReference` to do exactly this:

{% codeblock lang:php %}
<?php
// Get the EntityManager
$em = $this->getDoctrine()->getManager();

// Fetch the Parent entity from the database
$em->getRepository('AcmeDemoBundle:Parent')->find(10);

// Get a reference to an existing entity
$entity = $em->getReference('AcmeDemoBundle:Entity', 1);

// Associate it with the previously fetched object, as normal
$parentEntity->setEntity($entity);

// Flush the changes
$em->flush();
{% endcodeblock %}

It appears that Doctrine needs to fetch the referenced object from the database before the update can happen but it at least makes the code shorter and ensures the `SELECT` and `UPDATE` happen in the same database connection.