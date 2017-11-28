+++
date = "2017-11-06"
title = "About"
+++

Metaparticle is a _standard library_ for cloud native applications on Kubernetes.

The goals of the Metaparticle project are to democratize the development of distributed
systems. Metaparticle achieves this by providing simple, but powerful building blocks,
built on top of containers and Kubernetes.

These building blocks enable novice distributed system developers to easily adopt
proven patterns for distributed system development. At the same time, these patterns
also enable seasoned developers to build applications more quickly and more reliably.

Metaparticle provides access to these primitives via familiar programming language
interfaces. Developers no longer need to master multiple tools and file formats to
harness the power of containers and Kubernetes.

Metaparticle provides idiomatic language interfaces that help you build systems that:

   * Containerize your application
   * Deploy your application to Kubernetes
   * Quickly develop replicated, load-balanced services
   * Handle synchronization like locking and master election between distributed replicas
   * Easily develop cloud-native patterns like sharded-systems

To get a sense for what this looks like, here is the Java code for implementing master
election in Metaparticle:

```java
import io.metaparticle.sync.Election;

public class ElectionMain {
    public static void main(String[] args) throws InterruptedException {
      Election e = new Election("test");
      e.addMasterListener(() -> {
        System.out.println("I am the master.");
        // <-- Do something as master here -->
      });
      e.addMasterLostListener(() -> {
        System.out.println("I lost the master.");
        // <-- Handle losing the master here -->
      });
}
```

And the same functionality in Javascript

```javascript
var mp = require('@metaparticle/sync');

var election = new mp.Election(
    // Name of the election shard
    'test',
    // Event handler, called when a program becomes the leader.
    () => {
        console.log('I am the leader');
    },
    // Event handler, called when a program that was leader is no longer leader.
    () => {
        console.log('I lost the leader');
    });

election.run();
```

To learn more about what Metaparticle can do, 
head over to the [tutorials](/tutorials/) page.

Want to help hack? Find a bug? See something that really bothers you?

Metaparticle is an open source project and a work in progress.

Come on over to [github](https://github.com/metaparticle-io) and get
involved!