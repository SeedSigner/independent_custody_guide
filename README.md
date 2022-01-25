# SeedSigner Independent Custody Guide
### (aka "The SeedSigner Manifesto")
### (aka "The Gospel of Bitcoin Independent Custody According to SeedSigner")

(**Initial Note on Titling:** I first chose the subtitle "The SeedSigner Manifesto" as a way to start off this guide with a little bit of levity. But as I think more and more about Bitcoin custody and how it has evolved over time, the term "manifesto" seems increasingly appropriate to use. Manifestos usually challenge conventional ways of thinking about things and can be threatening to status quo beliefs.

As I view the longer arc of Bitcoin custody, I think of it in three epochs. The first is the epoch characterized by Bitcoin Core and paper wallets, Core being of course the earliest way to store bitcoin, and paper wallets being representative of early attempts at facilitating that storage with improved security assurances.

I consider the second epoch to be the hardware wallet era; that is, the era of proprietary, USB-connected, secure element equipped, separate hardware devices that represented a big advance in bitcoin custodial security assurances, but with some significant compromises, some of which involve varying degrees of reliance on third parties. The second epoch was facilitated by the hierarchical deterministic wallet and the BIP39 seed phrase standards.

In my view, the third custody epoch is being ushered in by the PSBT (partially signed bitcoin transaction) standard, the implementation and refinement of multi-signature wallet standards, and the ongoing re-thinking of how we can increase the separation between devices/software that interact with the Bitcoin protocol, and devices/software that interact with private keys. In my view, stateless signing devices like SeedSigner that leverage transparently airgapped communication (read QR-exchange protocol rather than NFT), with a focus on facilitating user-accessible multi-signature wallet use, will emerge as emblematic of this third epoch of Bitcoin custody. The Do-It-Yourself (DIY) aspect of SeedSigner, leveraging general purpose hardware and FOSS code, will also serve to shift control away from purpose-built, proprietary hardware, moving that power back into the hands of users.

This "manifesto", then seeks to advance the idea that with the right mix of design inputs, users can have acces to simple, user-friendly bitcoin self-custody, with reasonably solid security assurances, using inexpensive, discreetly-acquired hardware, & FOSS code. Now the sub-sub-title, with the gospel reference, that's still just there for comedic value, I think...)

## Introduction

Most Bitcoiners understand the importance of self-custody to the larger Bitcoin ecosystem -- as a digital bearer asset, having personal custody is the whole point (not your keys...not your coins). And holding one's own private keys is not merely some kind of Bitcoiner virtue signaling mechanism, but serves several important functions within the larger Bitcoin ecosystem. Practicing self-custody:

- Allows users to send bitcoin to any recipient without the need for permission **from anyone**
- Protectss bitcoin owners from loss of funds due to custodial service thefts or rugpulls
- Is foundational to privacy-conscious Bitcoin usage
- Protects Bitcoin as a financial asset from rehypothication & other forms of value dilution
- Helps Bitcoin users understand the network's technical fundamentals (when done properly)

Understandably, the first reason above is the primary concern of most people wanting to hold their own keys --  where your money goes, your mind follows. But for many of those earnestly wanting to get to the self-custody finish line, things can get in the way, life happens. A lot of it can have to do with uncertainty and analysis-paralysis. What hardware do I need? Which software is the best? Where should I store backups? What security trade-offs make sense for me?

I am assembling this guide as a living document to (hopefully) help people get to that self-custody finish line, using the custody framework that has evolved over time to make the most sense to me. When putting a self-custody system together, there are a myriad number of variables to consider -- I've tried to minimize the decision tree's branches to make the process less overwhelming for people who might be prone to obsessing over the details. I'll also attempt to justify these choices wherever I can. This guide is primarily intended to be worked through with some kind of self-custody "coach" or consultant, who can provide further context and answer questions along the way. With some supplemental materials, it however may also be useful to those pursuing a more self-guided custody journey.

I should also note that at different points, especially the earlier parts, this guide may read more like an essay and less like a technical guide. Though I initially set out to write a more step-oriented document, the question of "why" kept coming up. And the plain truth is that if people don't understand the "why" behind their actions, they're not able to have a high degree of confidence and conviction about what they're doing. And confidence is a huge part of executing a Bitcoin savings plan, so I'm going to allow for a good ammount of digression, tangents and detours, so just prepare yourself.

But back to nuts and bolts; at this point it is useful to list some assumptions this guide makes about the intended audience:

### This Guide's Assumptions About Users:
- You are seeking to secure what is, or you expect to become, a significant amount of value
- You are willing to spend a reasonable amount of time & energy to create a self-custody system
- You are willing to spend at least ~$100 on the necessary hardware & supplies
- You have access to at least two remote, secure locations for key storage
- You are comfortable with SeedSigner's overall security model (more on this below)

It also makes sense to list what you'll need early on so you can have a sense of what you're getting into:

### Necessary Items:
- Computer running Sparrow Bitcoin Wallet (https://sparrowwallet.com)
- SeedSigner Bitcoin signing device (https://seedsigner.com)
- Printer with paper
- Some sort of metal backup solution (strongly recommended but optional)

## First Off, Why Sparrow?

A few notes on what Sparrow is and why I selected it for this guide. Sparrow is a software program that runs on each of the Big-3 computer OS platforms (Windows, Mac, Linux) that can be described as a "wallet coordinator". What this designation means is that while you can absolutely use it for single-signature wallets, a big part of Sparrow's value proposition is that you can use it to take multiple Bitcoin private keys and "coordinate" them into a multi-signature wallet. Sparrow is also intended to be run on a computer with an active intenet connection, and manages your wallet's interaction with the Bitcoin protocol.

"Specter Desktop" (https://specter.solutions) is another quality software program that can be used to create and manage multi-signature Bitcoin wallets, with functionality similar to Sparrow (side note: Specter also created an an airgapped Bitcoin wallet that was a big part of the inspiration for SeedSigner; more into at https://github.com/cryptoadvance/specter-diy). Specter Desktop generally requires users to have/operate a full Bitcoin node, which could be a standalone / purpose-built computer, or could be an instance of Bitcoin core (https://github.com/bitcoin/bitcoin/releases) running on the same system where Specter desktop is installed. Though running a full node is unarguably a more secure, more private way to access the Bitcoin protocol and something I think that most Bitcoiners should do (or aspire to do), I didn't want it to be a deal-breaking requirement for this guide. As it is with privacy, digging into Bitcoin is a process and people should know that it takes time and you don't have to do everything at once. Undertaking self-custody in the proper way is a big enough task in and of itself.

"BlueWallet" (https://bluewallet.io) is another quality software program that can be used to create and manage multi-signature Bitcoin wallets, and that can be used on both iOS and Android devices (there is a Mac version too!). BlueWallet does not require the use of a full node, but being a mobile-based coordinator is what makes it so compelling given that in many parts of the world, mobile devices are the primary way people interact with Bitcoin as well as the broader internet. BlueWallet is a high-quality, featureful application for creating and using multi-signature Bitcoin wallets (and Lightning wallets too!) but in my opinion BlueWallet's achilles heel, at least for the purposes of this guide, is that it does not support Bitcoin testnet. More on Testnet to come.

The multi-signature custody setup this guide intends to help users create can also be created and operated with both Specter desktop or BlueWallet, but again with the goal of minimizing the number of branches on the decision tree, I have elected to focus on Sparrow.

## And Why Multi-signature Wallets?

Multi-signature wallets have gotten a bad reputation. Several years ago when the concept started to gain traction in the Bitcoin ecosystem, the hype and value proposition was real but it it took a fair amount of time for proper tools to be developed. Like most tools, the earliest versions necessarily targeted a more-technical audience, and by the time those tools emerged the larger hype around multi-signature wallets had died down, resulting in them being perceived as a more niche self-custody option.

One of the primary goals of this guide is to permanently shatter and discard the notion that multi-signature wallets are too complex for most, if not all, Bitcoiners -- full stop. If a given multi-signature wallet seems too complex and is practically too difficult for most Bitcoiners to use, that is the wallet's fault and it's developers need to proverbially "return to the drawing board".

The most approachable way that I've found for people to think about multi-signature wallets is to think about your Bitcoin storage like a corporate board (sometimes referred to in technical-speak as a "quorum"). Your funds are the board's treasury, and they can only be accessed or moved with the approval of a certain number of board members (sometimes referred to as "co-signers"). Usually the number of signers required to access funds is a majority of the total signers (think 3-of-5) but the chartering documents that are laid out when the board was created (sometimes referred to as the "multisig policy") dictate how many votes are needed to access funds. (A board that only required one member to be able to approve the movement of funds wouldn't make a lot of sense, and in most scenarious a board that required every member to approve transactions similarly doesn't make sense)

Like most analogies, this one breaks down as you get deeper into the technical details, but it still serves as a good starting point for most. But now with a general idea of how multi-signature wallets work, what are the advantages & disadvantages?

### Multi-signature wallet advantages:
- Introduces fault-tolerance to Bitcoin storage (no more single-point-of-failure)
- Facilitates the distibution of custody among multiple, disparate parties
- Allows for physical, geographic distribution of the keys that make up a given wallet
- Allows for the use of muliple different hardware/software security models among cosigners

### Multi-signature wallet disadvantages:
- Increased techncial complexity that could result in loss of funds
- Increased volume of information that needs to be stored/maintained
- Users making multiple mistakes could still result in loss of funds

The first multi-signature storage advantage of fault tolerance is actually multi-faceted. With a single-signature wallet, the go-to strategy to mitigate the risk of **losing** a key is to keep a second copy of the secret that secures your Bitcoin. This second copy of the secret is often kept in a separate location with an emphasis on physical security, as the other copy is often protected by some kind of technological access safeguard (read secure element, PIN, etc.). But the mere existence of this second copy ironically increases the risk of the private key being not lost, but rather disclosed, to a potentially malicious third party (think of a rogue bank employee snooping in safe deposit boxes, or perhaps a more targeted malicious attack on a "remote-but-secure" location). Multisignature wallets allow for a scenario where the loss **or** the disclosure of a secret does not necessarily result in catastrophe.

It is my view that the fault tolerance afforded by a multi-signature storage strategy outweighs the necessary added complexity & information management requirement, especially when attempting to safeguard a substantial amount of value. Others are welcome to have differing opinions and takes on these trade-offs, but if you are convinced of the appropriateness of multi-signature storage for your circumstances, I welcome you to read on.

## What's the story with this SeedSigner thing anyway?

When I initially set out to write this guide, I didn't intend to tell the broader story of how SeedSigner came about, but as I thought more about the device's unique characteristics, the story behind its creation helps provide context for the various design decisions that were made as SeedSigner came into being. So here goes:

As I was trying to improve my Bitcoin storage security posture, I found myself in the camp of believers that the benefits of multi-signature storage outweigh the costs/risks. But I am frugal, or put more bluntly, a cheapskate. The prospect of spending $500 or more on hardware wallets was unappealing to me. This position is actually a mistake because depending on the amount of value you are attempting to secure, it absolutely makes sense spend a reasonable proportion of that value to add safeguards to protect against the value's theft/loss. But my inner-cheapskate was convinced that there should be a way to set up and operate multi-signature storage without the absolute need for several devices.

Enter Specter-DIY, as referenced above. While I'd been researching Specter desktop and other emerging multi-signature storage tools, I came across the Specter-DIY, a DIY (do-it-yourself) Bitcoin wallet made from easily acquireable electronic components. One of the most interesting aspects of the Specter-DIY for me was it's use of animated QR code sets to communicate proposed transaction information from the multi-signature wallet coordinator software (in this case Specter Desktop) to the Specter-DIY, where the wallet's private key was available. An updated proposed transaction was then passed back to the multi-signature coordinator software from the Specter-DIY (more on this mechanismm, which in technical jargon is referred to as a "partially signed Bitcoin transaction", to come), again using QR codes.

This proceedure made so much sense to me given my background in digital forenics (with a little bit of information security training sprinkled in). How the private key (which is the absolute safeguard of any Bitcoin storage scheme) could be stored and used on a device separate from the multi-signature coordinator, with only a very narrow QR-exchange communication protocol used to facilitate communication between them (instead of a USB connection), was such an elegant and relatively simple solution. In the digital forensics realm, great care and attention is given to when and how evidentiary devices are permitted to connect to evidence-collection devices; this great care arises from the broader criminal forensic discipline's reliance on Locard's "exchange principle". The exchange principle dictates that when there is physical contact between two given items, there will be an exchange of microscopic material between them. Loosely applied to the discipline of digital forensics, when you connect two electronic devices, "things can happen" that can involve either the transfer of data, or even the creation of new data as a result of the connection. Digital forensic practitioners seek to avoid such, or at least be able to document and explain it, wherever possible.

My interest in Specter-DIY led me to begin casually interacting with members of the Specter team, as well as security researcher Michael Flaxman, author of the excellent "10x Bitcoin Security Guide" (https://btcguide.github.io/). Having previously tinkered with 3D printing and computer-aided design, I created designs for 3D-printable enclosures the DIY. While interacting with Flaxman, he shared an idea for a simple device that integrated a Raspberry Pi Zero single board computer with a display-plus-controls module that would allow users to input seed words they had randomly selected from the BIP39 list and use the device to calculate a seed phrase's final word, that acts as a kind of checksum against the words that precede it. (This mechanism is intended to alert users to errors they might make when entering a seed phrase into a given device.) The beauty part of Flaxman's idea was that the proposed device used a specific version of the Raspberry Pi Zero, the version 1.3 that did not include the physical hardware necessary for th Pi to connect to other devices via WiFi or Bluetooth).

When conducting a digital forensic exam on a mobile electronic device, the practice of radio isolation (using "Faraday" cages a la Michael Faraday) is often implemented to prevent devices subject to examination from connecting to wireless networks (via WiFi or cellular connection) or other device's (via Bluetooth). This isolation prevents incoming data from causing changes on the device (the worst of which being a "death from above" remote wipe signal) and allows for a more controlled examination/acquisition of a mobile electronic device, with a more static data set. I should also note that for security reasons, the digital forensic lab in which I work utilized an internal, offline network that was not architected to connect to the internet. Forensic evidentiary data could be shared by machines in different parts of the lab, but data travel beyond the lab's internal network was not possible. There were several reasons for this segregation, but it basically boiled down to not wanting any of our private data to be able to get out, nor wanting any undesirable data from the open internet to be able to get in.

Flaxman's idea of using a computer with no means of wireless communication to calculate final seed words enforced the same kind of network isolation we had used in the forensic lab and ensured that the private data comprised by the seed phrase would remain private, in a kind of "can't be evil" way. I ordered the necessary components and set to work translating my rudimentary coding skills into a proof-of-concept. Without too much trouble I cobbled together a device that would allow a user to enter 23 BIP39 seed words, and use a Python Bitcoin library called Embit (h/t to Stepan Snigerev of Specter) to generate an appropriate checksum word for a proper seed. After coding a rudimentary dice-to-seed module that would convert 99 dice rolls into a 24-word seed phrase, I started to look for more use cases for my new "toy". I realized that with the addition of an inexpensive, Raspberry Pi-compatible camera, I might be able to replicate the core air-gapped transaction signing ability of the Specter-DIY.

With some more stackoverflow-intensive research and coding, I was able to replicate the DIY's QR-exchange transaction-signing process, and at this point the full concept of SeedSigner had been born. An issue still remained however relative to private keys: would you really want this little device to remember your private keys? The absense of any kind of secure enclave technology in the Raspberry Pi platform that would be able to provide users with some assurance their private key was being stored securely, the resounding answer was a "no", you would not want this device storing your private keys.

The solution to this problem was once again found in my experiences in the forensic lab. To acquire data from hard drives in a forensically defensible manner, examiners will commonly use linux-based "live" operating systems. These forensic live-OS tools allow you to power on an evidentiary computer, connect an evidentiary-collection hard drive, and essentially use the computer to acquire the data from its internal storage location(s). The defining characteristic of these forensic live operating systems is that they operate entirely in the target computer's memory (and then conduct a read-only acquisition of attached storage media) such that after you power the target system off, no persistent, residual data from the acquisition is left behind. If my little handheld signing device was simply not engineered to "remember" private keys (only utilising the keys as a python variable in memory), the issue of securely storing a key on the signing device could be avoided altogether. And not storing keys meant that the device could in theory be safely used with mutiple keys, obviating the need for a dedicated hardware device for each private key within a multi-signature wallet!

The bill-of-materials cost for my physically-disconnected, wireless-incapable, amnesiac device came to approximately $35, a satifying result for my inner cheapskate. But as I shared my idea for a signing device with others Bitcoiners, privacy-related advantages associated with the tool also emerged. Because a SeedSigner is not built from any components that are recognizably identified as bitcoin-specific by most people, they can be acquired without signaling to a merchant or anyone else an intention to build a Bitcoin signing device, or otherwise an intention to interact with the Bitcoin network. For those with strong privacy concerns, this is a desirable feature, and for those living in part of the world where Bitcoin use is discouraged or outright banned (thus making it difficult or impossible to securely and reliably source hardware wallets) SeedSigner can provide a more secure way to save with Bitcoin.

Reading the story behind how SeedSigner came into existence highlights many of the device's advantages, but to condense them more explicitly:

- Operation-in-isolation (no USB/WiFi/Bluetooth) dramatically reduces attack vectors
- Low build cost makes the device accessible to more people in more parts of the world
- Statelessness makes using SeedSigner with multiple seeds and/or multiple wallets feasible
- Underlying fully-FOSS softare architecture makes independent build-yourself-from-source possible
- Use of non-Bitcoin-specific hardware can enhance user privacy

## Alright, But There's Got to Be a Catch, Right?

No technology, especially security-related technologies, are **ALL** positive, there have to be trade-offs or vulnerabilities, right? Absolutely yes. Let me list some of the criticisms/vulnerabilities I am aware of and then I'll discuss them one by one:

- direct access to seeds increases their discloure opportunity
- a full linux installation has a large attack surface
- airgapped communication is security theatre that results in a worse user experience
- unpaid developers don't have time time/expertise/care to properly "do security"
- lack of software validity assurances leaves open the possibility of malicious code installation
- an evil maid could use malicious code to exfiltrate private keys
- the Broadcom BCM2835 chip used in Pi Zero is closed source and potentially compromised

These are the potential weaknesses/vulnerabilities that have been communicated directly to me, or that I have gleaned from observing comments in public forums; if additional criticisms come to light, I will add them to this document.

**Direct access to seeds increases their disclosure opportunity:** This is one that is reasonable and that I agree with. Though some people that appreciate the SeedSigner model have begun using it for their more day-to-day storage needs, the larger model was designed with cold storage in mind, meaning that I anticipate Bitcoin savers using the SeedSigner model would need to make outgoing transactions 1-2 times a year, if that. Deposits to a multi-signature wallet, including to newly-generated receive addresses, can of course be made without accessing private keys. So this means that your seed phrases stay in some kind of remote, physically secure, non-visibly secure, tamper-evident storage spot (more on this later), and when you do access your seed phrases, obvious care should be taken to access them in areas that they will not be seen by unwanted persons, or in areas that are subject to visual surveillance, etc.

**A full linux installation has a large attack surface:** This is another reasonable criticism. Bitcoin hardware security devices written using languages that run on "bare-metal" unarguably have fewer attack vectors or opportunities to introduce code with malicious intent. For SeedSigner, once the operating system image has been written to the MicroSD card that is installed in the signer, I believe that operating the device in isolation from the internet and other devices provides sufficient assurances that malicious code intended to exploit vulnerabilities in the Raspberry Pi OS can't be inadvertently inserted by a SeedSigner user.

**Airgapped communication is security theatre that results in a worse user experience:** I wholeheartedly disagree with the first part of this. Once the MicroSD card is installed in SeedSigner, the only way that the device can communicate with the outside world is via the attached camera, and the display screen. The QR exchange protocol is an effective means of using those two components to facilitate two-way communication with an extremely limited scope. It is technically possible for private key information to be communicated via QR codes displayed on the devices screen, but for an attacker to take advantage of such a situation, both the SeedSigner installation would need to be compromised, and the computer being used with SeedSigner would need to be compromised in some way so as to recognize, capture, and externally communicate the secret. As for the user experience, QR exchange is unarguably less convenient than other methods of communication, but I consider it to be a more than acceptable trade-off given the importance of maintaining the security of private keys.

**Unpaid developers don't have time time/expertise/care to properly "do security":** Many of Bitcoin core's most sophisticated contributors have done their work while not being employed by a company sponsoring their work on the protocol. This kind of blanket statement is sloppy, bordering on malicious, criticism. As a fully FOSS (Free and Open Source Software) project, the SeedSigner code is available to audit and interested parties can suggest additions or modifications as they deem necessary. Security vulnerabilities are further discovered in code written by "professionals" all of the time.

**Lack of software validity assurances leave open the possibility of malicious code installation:** I'd classify this criticism as generally valid but mitigate-able. The meat of this criticism is that because the Pi Zero has no security mechanism to validate installed software as coming from a "trusted source", users might inadvertently download and install a rogue version of the SeedSigner software from a malicious source. Such malicious software could attempt to accumulate a user's private keys, replace addresses in partially signed transactions, as well as other mischevious schemes. Just as self-custody calls upon you to take ultimate responsibility for your Bitcoin stack, you are the SeedSigner software validation mechanism and are responsible for ensuring that authentic software is installed on the device. I have published a PGP public key and sign a message containing a SHA256 hash of each prepared release; steps on how to properly download, verify and write a SeedSigner release image to MicroSD will be subsequently discussed in this document.

**An evil maid could use malicious code to exfiltrate private keys:** The "evil maid" attack typically describes a malevalent third party who has access to a given individual's private space, and happens to inadvertently & secretly come across sensitive or valuable items or information that they steal or otherwise exploit. In the context of SeedSigner, this attack might be renamed the "evil adversary that can create sophisticated malware and secretly accesses your signing device on multiple occasions, installing undetectable rogue software, and subsequently secretly returning to abscond with your private key(s)... attack". I don't want to write this type of attack off as far-fetched, but reasonable countermeasures are to store your SeedSigner in a relatively secure place, use tamper-evident packaging if you feel it to be necessary given your home environment, and if for some specific reason you become concerned that your SeedSigner software may have been tampered with, simply zero out your MicroSD and re-write the software release image to the memory card.

**The Broadcom BCM2835 chip used in Pi Zero is closed source and potentially compromised:** This is perhaps the most vague criticism of SeedSigner I have come across. I haven't located any information to indicate a specific backdoor or other vulnerability relating to the Broadcom chip used on the Raspberry Pi Zero that would create a security vulnerability relating to SeedSigner. Honestly, if the broader Broadcom ecosystem has an undisclosed zero-day vulnerability, the entire Bitcoin network is likely in trouble given the popularity of the standard Raspberry Pi as a full node hardware platform.

## So What Does This SeedSigner Thing Actually Do?

Generally speaking, SeedSigner helps Bitcoiners accomplish three important independent custody tasks:

- Creating secure private keys in a trust-minimized way
- Generating extended public keys used during initial wallet configuration
- Securely signing transactions via animated QR code sets

Each one of these tasks actually deserves some elaboration, so here we go...

## Creating Secure Private Keys in a Trust-Minimized Way

What makes a Bitcoin private key secure? Simply put, ensuring that the private key is generated from information that is un-predictable, un-reproduceable, and un-guessable. These characteristics pretty well nail down what comprises the mathematical concept of entropy, i.e. randomness. Though there have been advances in the ability of software to generate unpredictable data, disagreements persist on the theoretical ability of truly random data to arise from organized, logical code created by human beings. (This may go without saying, but it's not a best practice to trust a private key generated by a bitcoin storage device that does not incorporate any kind of user input into the process.)

It turns out that the simplest, easiest, and perhaps best way to capture entropic data is via the randomness inherent in the movements of the physical world that surrounds us. The best example of this phenomenon I have come across are fingerprints. Human fingerprints, which are created with almost innumerable inputs that include in utero blood pressure, blood oxygen levels, maternal nutrition, hormone levels, amniotic fluid density & composition, fetal position & movement, and even the sum of maternal movement during pregnancy. All of these variables shape the tiny ridges we all have on our finger tips. Given the amount of inputs, it would be virtually impossible to recreate these variables twice, which is why fingerprints are thought to be absolutely unique to a given individual, with no two individuals ever having had identical prints.

Rather than waiting the 9-months it takes to form human fingerpints, Bitcoiners need a more quick-and-dirty way of capturing some physically generated entropy for use in private key generation. With the creation of the BIP39 seed phrase standard, the most common way for Bitcoiners wanting to privately generate a private key was to randomly select 11 or 23 "seed" words (often literally out of a hat) and then use some kind of tool to calculate the aforementioned final "checksum" word. SeedSigner of course supports this method.

Another way of creating a private key from physical inputs is to capture entropy from the results of dice-rolls and convert those results into a private key, and a seed phrase for documentation. SeedSigner supports this method of generating a private key, and does so in a way that is verifiable using other tools, like those produced by Ian Coleman (https://iancoleman.io/bip39/) and Coinkite (https://coldcard.com/docs/verifying-dice-roll-math).

The last way of creating a private key with SeedSigner is to use the entropy from a digital photograph taken using the device's on-board camera. The entropy captured from this process is a "more than meets the eye" feature because not only the pixels in the photograph are used to determine the private key, but also the data from each preview image frame that is rendered on the screen after the feature is activated, as well as the Pi Zero's unique serial number, and the number of milliseconds the device has been powered on. These entropy from these variables is aggregated to capture the entropy that goes a private key generated by this approach.

## Generating Extended Public Keys Used During Initial Wallet Configuration

This is beginning to get into the nuts and bolts of multi-signature wallets, but understanding at a high level what's going on when you set up a multi-signature wallet is helpful as users piece the puzzle together.


A note on testnet -- you can't be skillful and confident about something you don't practice...


Communication I can't see makes me nervous...

What is the big deal with entropy?

Cover build options

What's the deal with Seed QR codes?

More on PSBTs and generally how they work.

Thankfully SeedSigner as well as the aforementioned multisignature-wallet coordinators have obscured a lot of the technical complexity

PSBT's are kind of like proposals or motions if you extend the boardroom analogy.

Confidence comes from practice and practice takes time

### Security Considerations:

Why Testnet and testing?

Note on this being a medium-depth guide.

Acknowledgements: 10x Security Bitcoin Guide

Some of the info in here may seem weird to include in a guide, but I think giving people the whole story helps them wrap their idea around seedsigner and get more comfortable with the security model it employs.

manifesto is an appropriate framing, because the concept of the SeedSigner model is challenges the current, prevalent thinging on bitcoin cold storage

addendum opportunity: using seedsigner as a way to keep your hardware wallets honest

note on the signer vs wallet designation

note about difficulty of accessing signers being a feature not a bug
