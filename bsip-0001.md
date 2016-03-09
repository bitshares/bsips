    BSIP: 0001
    Title: BSIP Purpose and Guidelines
    Authors: Fabian Schuh <Fabian@BitShares.eu>
    Status: Draft
    Type: Informational
    Created: 2015-12-16

# What is a BSIP?

BSIP stands for BitShares Improvement Proposal but can also seen as an
improvement *protocol*. A BSIP is a design document providing information to the
BitShares community, or describing a new feature for BitShares or its processes
or environment. The BSIP should provide a concise technical specification of the
feature or the idea and a rationale for it. It may not only describe technical
improvements but also document *best-practises* and recommendations.

We intend BSIPs to be the primary mechanisms for proposing new features, for
collecting community input on an issue, and for documenting the design decisions
that have gone into BitShares. The BSIP author is responsible for building
consensus within the community and documenting dissenting opinions.

Because the BSIPs are maintained as text files in a versioned repository, their
revision history is the historical record of the feature proposal.

# BSIP Types

There are two kinds of BSIPs:

* An **Informational** BSIP describes a BitShares design issue, or provides general
  guidelines or information to the BitShares community, but does not propose a
  new feature, protocol change or any other modification. Informational BSIPs do
  not necessarily represent a BitShares community consensus or recommendation,
  so users and implementors are free to ignore Informational BSIPs or follow
  their advice. Examples would be *best-practises* or *recommendations*.
* A **Protocol** Upgrade BSIP describes any change that affects most or all
  BitShares implementations, such as a change to the protocol, a change in block
  or transaction validity rules, or any change or addition that affects the
  interoperability of applications using BitShares.

# Contributing

People wishing to submit BSIPs first should propose their idea as github
issue first. After discussion you will be assigned a number for the bsip
and can send a pull request for your *draft*. Once consensus among
discussion participants is reached, the status can be switched to
*accepted*. From this time on, major changes of the document will not be
permitted.

If the proposal requires a protocol upgrade, the proposal is considered
*implemented* only if shareholders have approved a corresponding worker or
hard fork proposal. Informational BSIPs can only reach the *accepted*
state since their implementation is not enforced by the blockchain.

We are fairly liberal with listing BSIP drafts here since the
final decision of its actual implementation is made solely by BitShares
shareholders via approval voting.

It is highly recommended that a single BSIP contain a single key
proposal or new idea. Small enhancements or patches often don't need a
BSIP and can be injected into the BitShares development work flow with a
patch submission to the BitShares issue tracker. The more focused the
BSIP, the more successful it tends to be. The BSIP editor reserves the
right to reject BSIP proposals if they appear too unfocused or too
broad. If in doubt, split your BSIP into several well-focused ones.

Vetting an idea publicly before going as far as writing a BSIP is meant to save
the potential author time. Many ideas have been brought forward for changing
BitShares that have been rejected for various reasons. Asking the BitShares
community first if an idea is original helps prevent too much time being spent
on something that is guaranteed to be rejected based on prior discussions
(searching the internet does not always do the trick). It also helps to make
sure the idea is applicable to the entire community and not just the author.
Just because an idea sounds good to the author does not mean it will work for
most people in most areas where BitShares is used.

Following a discussion, the proposal should be sent to the BitShares developers
and the BSIP editors with the draft BSIP. This draft must be written in BSIP
style as described below, else it will be sent back without further regard until
proper formatting rules are followed.

If the BSIP editor approves, he will assign the BSIP a number, label it, give it
status "Draft", and add it to the git repository. The BSIP editor will not
unreasonably deny a BSIP. Reasons for denying BSIP status include duplication
of effort, being technically unsound, not providing proper motivation or
addressing backwards compatibility, or not in keeping with the BitShares
philosophy.

The BSIP author may update the Draft as necessary in the git repository. Updates
to drafts may also be submitted by the author as pull requests.

For a BSIP to be accepted it must meet certain minimum criteria. It must be a
clear and complete description of the proposed enhancement. The enhancement must
represent a net improvement. The proposed implementation, if applicable, must be
solid and must not complicate the protocol unduly.

Once a BSIP has been published, the reference implementation must be
completed.  When the reference implementation is complete and accepted
by the shareholders via approval voting, the status will be changed to
"Accepted". A BSIP can also be "Rejected" by shareholders.

Furthermore, a BSIP can be assigned status "Deferred". The BSIP author or editor
can assign the BSIP this status when no progress is being made on the BSIP. Once
a BSIP is deferred, the BSIP editor can re-assign it to draft status.

BSIPs can also be superseded by a different BSIP, rendering the original
obsolete. This is intended for Informational BSIPs, where version 2 of an API
can replace version 1.

# What belongs in a BSIP?

Each BSIP *should* have the following parts:

* Preamble -- RFC 822 style headers containing meta-data about the BSIP,
  including the BSIP number, a short descriptive title (limited to a maximum of
  44 characters), the names, and optionally the contact info for each author,
  etc.

* Abstract -- a short (~200 word) description of the technical issue being
  addressed.

* Copyright/public domain -- Each BSIP must either be explicitly labelled as
  placed in the public domain (see this BSIP as an example) or licensed under
  the Open Publication License.

* Motivation -- The motivation is critical for BSIPs that want to change the
  BitShares protocol. It should clearly explain why the existing protocol
  specification is inadequate to address the problem that the BSIP solves. BSIP
  submissions without sufficient motivation may be rejected outright.

* Rationale -- The rationale fleshes out the specification by describing what
  motivated the design and why particular design decisions were made. It should
  describe alternate designs that were considered and related work, e.g. how the
  feature is supported in other languages. The rationale should provide evidence
  of consensus within the community and discuss important objections or concerns
  raised during discussion.

* Specification -- The technical specification should describe the syntax and
  semantics of any new feature. The specification should be detailed enough to
  allow competing, interoperable implementations for any of the current
  BitShares platforms.

* Discussion -- The BSIP shall include a discussion on positive and negative
  effects on the BitShares ecosystem shall it be accepted by shareholders. This
  section is supposed to be the most important section for shareholders to grasp
  the full impact of the BSIP and help shareholders to make a decision.

* Summary for Shareholders -- Most BSIPs will probably be of technical nature.
  However, many shareholders are not as technical as the author of a particular
  BSIP. This non-technical paragraph serves as a place which can be used to
  to interact with shareholders and help them form their opinion. It is not
  meant to be a marketing driven paragraph to convince shareholders to vote
  **for** or **against** a proposal, though.

# BSIP Formats and Templates

BSIPs should be written in mediawiki or markdown format. Image files should be
included in a subdirectory for that BSIP. A template including the header
preamble is provided in [this repository](BSIPs-Template.md).

# BSIP Editors

The current BSIP editors are:

 * Fabian Schuh <Fabian@BitShares.eu>
 * Sigve Kvalsvik <sigvekvalsvik@gmail.com>
 * *cass* <Cass@BitShares.org>

The editors don't pass judgement on BSIPs. We merely do the administrative &
editorial part.

Many BSIPs are written and maintained by developers with write access to the
BitShares codebase. The BSIP editors monitor BSIP changes, and correct any
structure, grammar, spelling, or markup mistakes we see.

For each new BSIP that comes in an editor does the following:

* Read the BSIP to check if it is ready: sound and complete. The ideas must make
  technical sense, even if they don't seem likely to be accepted.
* The title should accurately describe the content.
* Edit the BSIP for language (spelling, grammar, sentence structure, etc.),
  markup (for reST BSIPs), code style (examples should match BSIP 8 & 7).

Once the BSIP is ready for the repository it should be submitted as a "pull
request" to the [https://github.com/BitShares/bsips BitShares/BSIPs] repository
on GitHub where it may get further feedback.

The BSIP editor will:

* Assign a BSIP number (almost always just the next available number, but
  sometimes it's a special/joke number, like 666 or 3141) in the pull request
  comments.
* Merge the pull request when the author is ready (allowing some time for
  further peer review).
* List the BSIP in [[README.mediawiki]]
* Send email back to the BSIP author with next steps (post to BitShares mailing
  list).

# History

This document was derived heavily from Python's PEP-0001 and Bitcoin BIP-0001.
In many places text was simply copied and modified. Although the
PEP-0001/BIP-0001 text was written by Barry Warsaw, Jeremy Hylton, and David
Goodger, they are not responsible for its use in the BitShares Improvement
Process, and should not be bothered with technical questions specific to
BitShares or the BSIP process. Please direct all comments to the BSIP editors
or the BitShares development mailing list.
