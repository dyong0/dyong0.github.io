---
layout : post
comments : true
title : Making systems solid
---

I really liked to write a post about this topic, since I started to my career few years ago. For most of developers, making solid system is very important. Most of developers, who got hired by a company who has been providing web services, work on the systems already created and on operation. In addition, our clients always want to add, change, and remove features on the systems. We know something will change quickly, so we should prepare the future changes by making our system solid but flexible to change.

<!--break-->

There are several ways to make systems solid. But I think the most important thing is detecting unexpected behaviors. As we write unit tests to place harnesses, we can detect unexpected behaviors on systems as well by throwing exceptions as much as we want and reporting the exceptions to a monitoring system.

Regarding flexibility, the main idea is isolation. If you can isolate your systems into several partitions horizontally, accidents happening on the system also should be isolated. So with isolation, you don't need to get scard of changing your systems. Just take a very small part to place new change, and test it by yourself as well as users.

In conclusion, solidity and flexibility will give you good experience as a engineer but also as a service provider. Solidty by detecting unexpected behaviors, flexibility by isolating systems.
