---
title: "Welcome to Metaparticle!"
date: 2017-11-19T20:35:01-08:00
draft: true
---

Ever since I helped create Kuberentes, I've been driven by a mission to democratize distributed systems.
To make them easier to build and more reliable.

Today, I'm excited to introduce the next step in that journey:

Metaparticle
------
*A standard library for cloud-native development*

From `libc` to the standard library in every modern programming language, as well as domain specific
libraries like OpenGL, the history of programming is rich with standard libraries that empower developers.
Initially problems
that are complicated to solve have a myriad of complicated, hand-written, bespoke solutions. But over time,
the common patterns emerge, and these patterns are codified into _standard libraries_ which provide
re-usable, high quality components that everyone can use. The ubiquity of the standard library enables people
to deliver higher quality applications with less effort. Fundamentally it democratizes the technology.

As a classic example of this, consider that in the days of the DOS PC, every application had it's own implementation of 
menus, buttons. This made application development so cumbersome that many applications resorted to simple command-line interactions. 
The introduction of Windows and other GUI toolkits radically changed this environment by providing a standard library for graphical application development. Suddenly any programmer could take advantage of a standard set of graphical interface libraries. This development democratized the development of graphical programs, making it available to a much wider set of developers, and the reliance on the standard library fostered a common interface that enabled users to simply and consistently move from one application to another.

However, despite the power they provide, to this point, a standard library for cloud native development on Kubernetes has not been created. 
Consequently, cloud native development is bespoke, complicated and limited to a small number of expert developers. 
Metaparticle changes this by introducing a set of utilities in familiar programming languages that meet the developer where they are and enables them to
begin to develop cloud-native systems using familiar language features. 

As an example, the Javascript code below packages itself as a container and deploys four replicas of itself, behind a
load balancer to Kubernetes:

```javascript
const http = require('http');
const os = require('os');
const mp = require('@metaparticle/package');

const port = 8080;

const server = http.createServer((request, response) => {
	console.log(request.url);
	response.end(`Hello World: hostname: ${os.hostname()}\n`);
});

mp.containerize(
	{
        	ports: [8080],
        	replicas: 4,
		runner: 'metaparticle',
		repository: 'docker.io/docker-user-goes-here',
		publish: true,
		public: true
	},
	() => {
		server.listen(port, (err) => {
			if (err) {
				return console.log('server startup error: ', err);
			}
			console.log(`server up on ${port}`);
		});
	}
);
```

When you run this program with `npm start` the program packages itself as a container and deploys four replicas of itself into Kubernetes behind a load balancer.

For more details about this, please see the tutorials in [Javascript](/tutorials/javascript/), [Java](/tutorials/java/) and [.NET](/tutorials/dotnet/).

## Metaparticle/Synchronization
Once a developer can easily deploy and replicate their application, an obvious next problem is distributed synchronization between the different replicas.
Metaparticle builds on top of Kubernetes primitives to make distributed synchronization easier as well. It supplies language independent modules for
locking and leader election as easy-to-use abstractions in familiar programming languages.

To give an idea of how these abstractions work, consider the task of implementing master election. Typically this might involve setting up a storage layer like etcd or Zookeeper, integrating a storage library into your code, and determining how to do master election using that library. Each of these steps involve significant learning and effort. You need to figure out how to reliabily deploy Zookeeper, learn about the client libraries available in your language of choice, and then integrate those libraries into your existing application.

In contrast, here’s how you can do this with Metaparticle and Java:


```java
import Election e = new Election(“my-election”);

e.addMasterListener(() -> {
    System.out.println(“I am the master.”);
});
e.addMasterLostListener(() -> {
    Sytem.out.println(“I lost the master.”);
});
new Thread(e).start();
```

And here's how you would do it in Javascript:
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

And how you would do it in .NET-core:
```cs
...
    public static void Main(string[] args) {
      var election = new Election(
        "test",
        () => {
          Console.WriteLine("I am the leader!");
        },
        () => {
          Console.WriteLine("I lost the leader!");
        });
      Console.WriteLine("Waiting for election");
      election.Run();
    }
  }
}
```

In addition to this simple code library, Metaparticle also includes a collection of generic, re-usable containers which integrate with your code using well established container group patterns like the sidecar.

## Moving Metaparticle forward

Metaparticle empowers developers who may never have built a cloud-native application, to build with confidence and ease. For more experienced developers, Metaparticle invites them to harness re-usable containers, code and Kubernetes to build their applications more quickly and consistently.

But Metaparticle is more than just a library to empower developers, it is also intended as a community effort. A place where people can come, establish and build the reference implementations for the canonical patterns that make up modern distributed systems. As excited as we are to see what people build with Metaparticle, we're also excited to see the community develop that will build Metaparticle itself. Today is the first step on that journey.

I invite everyone to head over to the [tutorials](https://metaparticle.io/tutorials/), and try out a few of the tutorials, 
then head on over to our [github page](https://github.com/metaparticle-io/)  and start collaborating with the Metaparticle community.


Thanks!

Brendan Burns  
Co-Founder - Kubernetes  
Distinguished Engineer - Microsoft Azure  



