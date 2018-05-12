---
layout : post
comments : true
title : Making systems solid
---

For most of developers, making solid system is very important. The developers, who were hired by a company who has been providing web services, work on the systems already created and on operation. In addition, our clients always want to add, change, and remove features on the systems. We know something will change quickly, so we should prepare the future changes by making our system solid but flexible to change.

<!--break-->

There are several ways to make systems solid. But I think the most important thing is to detect unexpected behaviors. Writing tests to place harnesses of the unexpected behaviors, we can detect them easily.

Regarding flexibility, the main idea is isolation. If you can isolate your systems into several partitions horizontally, accidents happening on the system also should be isolated. So with isolation, you don't need to be scared of changing your systems. Just take a very small part to place new change, and let it be tested by you and few of users.

Solidity by detecting unexpected behaviors, flexibility by isolating systems.
