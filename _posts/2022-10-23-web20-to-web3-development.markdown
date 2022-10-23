---
layout: post
title:  "It's morphin' time (or the jump from web 2.0 into web3 development)"
date:   2022-10-23 10:00:00 -0300
categories: 'development'
---

There's been tons of articles explaining, or at least trying to encompass, what web3 actually is and how it relates to its theoretical predecessor, the so called web 2.0, a term coined in early 2000s and lately popularized at its peak around 2010 to describe how social media came into play in all this. Although this intro might be unnecessary, I feel a quick showcase of a diagram, that by now most likely 90% of the internet saw in the NFT craze from mid-to-late 2021, is necessary:

![Read vs Read/Write vs Read/Write/Own](https://www.gettingsmart.com/wp-content/uploads/2022/08/4ba40746-3ea0-42f3-8561-14cc1856e59e_D0PzD9bppJkDwgCzWr4D6R_02NhLazwsWVpN9BUL_K0L0qBTrhen0lsbe_CObfxkhZ3mHiGFNaB1MIX61dGxtb2BrH3kZNPVu4SjGeOuR0mGnFPOeMyRJnxTedvaBXauSbHtc30eFvSIqGnocae0t8s.png)
Credits to [GettingSmart.com](https://www.gettingsmart.com/2022/08/17/three-ways-web3-will-change-education-for-good/)

It is fair to start this blogpost by clarifying that it isn't my intention to talk about the ethical implications or the actual practicality of the phenomenon, but rather take a look into what the development process of a decentralized app looks like based on my very own, heavily biased, experience.

---

At the moment I began to work with a web3 related project, there was a considerable amount of information around but it sill was somewhat of an emerging trend. This initial project I took part in required to look into Ethereum-based NFTs: the client commended us to work on a web-based NFT project initially leveraging on OpenSea's SDK to be able to provide an MVP-like experience and test the waters. Needless to say, the moment I began working on it, almost the entire team was fully unaware of what a wallet address even was, so from the get go we had a lot of homework to do.

For an outsider to be able to get into blockchains is somewhat of an over-complicated hurdle, as the saturation of possibilities was something that dizzied us in the beginning (see Bitcoin, Solana, Ethereum, Ethereum side-chains like Polygon, etcetera). Our first right choice tackling this was to focus solely on the inner workings of Ethereum exclusively: figuring out **wallets**, **smart contracts** and their capabilities, and how **marketplaces like OpenSea** played a part was key.

After figuring out how wallet addresses, private keys, seed phrases; and smart contracts, proxies, ERC standards, we moved on to understanding the actual process of developing smart contracts, and the limitations of **Solidity** as well. This part included multiple failed attempts of deploying smart contracts to testnets and manually going through the entire token creation flow we had created, a process we knew we will most likely not use but would help us figure out the ins and outs of OpenSea as the most difficult part yet would be understanding *what role custom smart contracts played in the ecosystem of marketplaces*. Surprising no one, we weren't able to figure this out before our launch day, thus we ended up using OpenSea's shared storefront smart contract. For context: all smart contracts deployed that create (a.k.a. mint) NFTs outside of OpenSea's shared storefront smart contract are picked up automatically by the platform, and the wallet address that deployed the contract is considered the "creator", making that address the only one in control to update the collection's information (e.g.: description, name, picture, cover picture). **All smart contracts that mint NFTs are considered "collections" by most marketplaces**, similar to what factories are in development.

**Note:** ERC standards are a particularly tricky subject, especially with themes such as fungibility of tokens. Investing many hours of research into this particular part will be absolutely worth it, trust me.

Long story short, the initial product launched, we relied heavily on OpenSea, but we could've easily relied on the actual blockchain to prevent further centralization, it was only a matter of having more time to do research and execution. There were some misses that in hindsight are obvious -for example, we used cronjobs to keep track of some transactions rather than simply going with a websocket implementation-, but we didn't have the space to conduct proper proofs of concept because of different constraints. Nonetheless, even with this somewhat misguided approach, within the first hour of launching the product the first sale for an exclusive piece of digital art was sold, and the team was ecstatic.

As with every advancement on the web front, web3 has adepts and detractors, and I personally believe it is only natural to explore emerging proposals in order to be able to form an opinion and contribute to the public discourse. I don't believe I have an ulterior motive by writing this article other thant simply logging my experience with this particular part of web history and maybe be of assistance to anyone that might be going through a similar experience in the future. The end.