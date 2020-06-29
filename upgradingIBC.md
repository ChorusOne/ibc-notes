### Table of Contents


### 1: Introduction 
This file documents the issues around IBC upgrades at an ecosystem level with multiple chains connected. The concerns were raised in the IBC core co-ordination call.

### 2: Principles for upgrades
This section lists several essential ideas, which will combine in later chapters to detail the various issues with ecosystem-level IBC upgrades.

#### 2.1: The key need of end users
Let's consider this situation described in Figure 1.

[[../Upgrades_figure_one.png]]

Conceive Robinson, who transferred funds from blue chain to red chain using a particular channel on top of a connection/client. Robinson relies on the channel retaining its identity and properties throughout any upgrades to red and blue chains. There are different violations, detailed in this document, that could lead to Robinson’s funds being stuck/frozen on red if upgrades don’t take place correctly.

The basic principle is that: *IBC upgrades, at all levels of the stack, require execution in a way that channels are unimpaired*. Users can expect a particular channel to be immortal since they could have transferred funds over the channel and be unavailable to make any further changes to the status quo. The end-user to design IBC for is Robinson Crusoe - they moved funds over a channel someday, and then are offline on a Pacific island for 40 years. When they return online, those funds should be around in the same state, and transferable back over the channel, provided the two chains live.

#### 2.2: Users will perceive the ecosystem not a pairwise set of chains
The second key idea is that we're collectively building an ecosystem of a hub and multiple zones connected to the Hub. The Hub's value proposition is that it removes the need for all zones to create pairwise connections with each other, thus saving on scarce computational resources for each of them. A user can conceivably have transferred funds from the purple zone to the green zone via the Hub. Their transfer is exposed to 2 channels - one between the purple zone and Hub, and another between Hub and green zone. This orchestration across two channels will probably happen without the user consciously opting into it. An ecosystem of end zones and one Hub will have complicated paths with multiple channels implicated. IBC upgrade paths should exist for all chains such that the entire ecosystem of channels remains functional. In particular, we need to be aware of upgrade patterns that break one channel between two chains, leading to a ripple effect on the entire ecosystem.

#### 2.3: Sequencing upgrades for an IBC corridor
Let's consider this situation described in Figure 1. The Red chain wants to make an upgrade to its consensus process or headers. Post that upgrade, old light clients for Red won't be able to validate the new headers emitted by Red. In other words, it's a breaking change. In this scenario, the light clients must upgrade to follow the new headers before the chain upgrade. If this strict sequencing is not maintained, the old light clients will get stuck at a particular block height and will be unable to make any progress. Any funds that depend on such a stuck light client will also be, correspondingly, stuck. Once stuck, the only way to recover from the situation is to hard fork whichever chains are running the light client through a manual governance process. A messy political battle can ensue between two communities. *Sequencing is vital for upgrades Light clients update first, chains upgrade second for breaking changes.*

#### 2.4: Upgrade constraint in Stargate

