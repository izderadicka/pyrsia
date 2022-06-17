---
title:  Peer Metrics in the Pyrsia Network
authors:
- name: Mark Seaborn
  title: Pyrsia Contributor
  image_url: https://github.com/mseabornIBM.png
tags: [p2p, peer-metrics]
---

Since the advent of peer-to-peer networks in the 1970s, we have seen them applied in a variety of use cases on the internet. From the first usages in the Usenet network, to the infamous Napster network, astute developers have created peer networks to enable sharing of information and data without the use of central servers. Over the past few years, we have seen a new increase in the use of peer to peer networks and formalization of algorithms to implement these networks. The Pyrsia network is an example of a new application of peer to peer networks. Pyrsia is an open-source project that is designed to build trust that an open-source distribution is free of malicious code via a consensus algorithm, then share that distribution amongst the peers. An important goal of this project is to create a peer network that is reliable and "fast". To do this, the team added a unique feature to the peer network protocol that helps one peer in the network choose the best peer to download software distributions.

Herein I present the mechanism the Pyrsia team has created to help improve the selection process for downloading software distributions from peers. The idea is to improve upon the Kademila peer selection process and make "smarter" choices from Pyrsia peers based on a "peer metric". The mechanism is layered atop the Kademila peer selection process, which in Pyrisa returns a list of peers known to be in possession of an open-source distribution of interest. The peer metric is a real time assessment of the work-load of any given machine in the network, both related and unrelated to any current software transfers within the Pyrsia network.

The following table defines terms used in this blog.

|Term|Definition|
|---|---|
|Requester|In this context a requestor is a Pyrsia client requesting a file to be downloaded or uploaded to/from its peer(s).|
|Peer (Pyrsia Peer)|Any Pyrsia client that is participating in the network.|
|Neighboring Node|Any node that is returned by the Kademila algorithm as having a software distribution desired by a peer.|

As previously stated, a primary goal of the peer-to-peer software distribution sharing function of Pyrsia is that it efficiently uses peers. The efficient use of peers can be broken down into two aspects, efficiencies related to uploading software distributions to peers and efficiencies related to downloading software distributions from peers. This post limits the discussion to downloading software distributions from peers.

The primary rule defined by our selection process is that requesters must not overwhelm peers by requesting too many software distributions too often from any one neighboring node. Though hard resource usage limits are a configuration function for Pyrsia Peers, Pyrsia’s peer selection mechanism should also attempt to balance requests across the network of peers. Additionally, the peer selection mechanism should operate such that file requesters are not starved for bandwidth in the network by unresponsive peers, preventing fast downloads. While the first efficiency could be guaranteed by Pyrsia policy, the second one is more nebulous because Pyrsia cannot guarantee enough stable network peers exist at request times to achieve “fast downloads”.

The terms “fast downloads” and “requesting too much data” must be further defined to quantify what is meant. The means of describing these terms can be defined as a function of the attributes within the environment. There are measurable attributes of both the network and the Pyrsia peers participating in the networks that can help us define metrics to balance requests. These metrics will ultimately determine the quality of peers in the requesters' peer list. The term quality in this context is related to the ability of a peer to satisfy a request for download and will be defined by the function Q(x):

![$Q(x) = \sum_{n=1} ^{pa\ count} pa_n * weight](https://latex.codecogs.com/svg.image?Q(x)&space;=&space;\sum_{n=1}&space;^{pa\&space;count}&space;pa_n&space;*&space;weight)

where `pa` is a peer attribute of the environment and `weight` is a number that determines the expense of the attribute. The quality number will be calculated on demand by peers returned in the list of peers generated by the Kadimela algorithm. The quality function can be used for either upload quality or download quality depending on the need. The metric is currently used during the selection process for downloading software distributions. Finally, we need to define what characteristics are important to drive analytics for decisions about transactions on the network. The following table lists the attribute used by to Pyrsia generate the peer metric in Pyrsia. The peer with the lowest number is considered to be the ideal candidate for the software distribution download.

|Peer Attribute|Definition|
|---|---|
|Peer Network Load|A measurement of the current network bandwidth usage in terms packets in and packet out summed over all network interfaces.|
|CPU Load|The average CPU load over the last minute|
|Disk I/O Load|A measure of the current packets being read and written summed over all current processes on the system|

This system of measuring the quality of peers will evolved over time and as other attributes are defined, they will be integrated into the Pyrsia network.