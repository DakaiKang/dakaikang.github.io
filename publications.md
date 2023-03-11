---
layout: default
title: "Publications"
permalink: /publications
---

### [PVP: Practical View-Change-Less Protocol](jekyll/update/2023/03/10/PVP.html)

PVP is a high-throughput consensus protocol that provides a stable and low latency in all cases, has a simple and lightweight recovery step incorporated in the normal commitment path, and can deal with unreliable communication. PVP does so by combining a *chained consensus design*, which can replicate requests with a reduced message complexity and that uses a novel *Rapid View Synchronization* protocol to enable robust and low-cost failure recovery, with a high-performance *concurrent consensus architecture* in which independent instances of the chained consensus can operate concurrently to process requests with high throughput and without single-replica bottlenecks.