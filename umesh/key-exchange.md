Simple Password-Based Encrypted Key Exchange Protocols
------------------------------------------------------------

**Abstract.**
Password-based encrypted key exchange are protocols that are designed to provide pair
of users communicating over an unreliable channel with a secure session key even when the
secret key or password shared between two users is drawn from a small set of values. In
this paper, we present two simple password-based encrypted key exchange protocols based
on that of Bellovin and Merritt. While one protocol is more suitable to scenarios in which
the password is shared across several servers, the other enjoys better security properties.
Both protocols are as efficient, if not better, as any of the existing encrypted key exchange
protocols in the literature, and yet they only require a single random oracle instance. The
proof of security for both protocols is in the random oracle model and based on hardness
of the computational Diffie-Hellman problem. However, some of the techniques that we use
are quite different from the usual ones and make use of new variants of the Diffie-Hellman
problem, which are of independent interest. We also provide concrete relations between the
new variants and the standard Diffie-Hellman problem.

[http://www.di.ens.fr/~mabdalla/papers/AbPo05a-letter.pdf](http://www.di.ens.fr/~mabdalla/papers/AbPo05a-letter.pdf)


Security Analysis of KEA Authenticated Key Exchange Protocol
---------------------------------------------------------------------

**Abstract.** KEA is a Diffie-Hellman based key-exchange protocol devel-
oped by NSA which provides mutual authentication for the parties. It
became publicly available in 1998 and since then it was neither attacked
nor proved to be secure. We analyze the security of KEA and find that
the original protocol is susceptible to a class of attacks. On the positive
side, we present a simple modification of the protocol which makes KEA
secure. We prove that the modified protocol, called KEA+, satisfies the
strongest security requirements for authenticated key-exchange and that
it retains some security even if a secret key of a party is leaked. Our secu-
rity proof is in the random oracle model and uses the Gap Diffie-Hellman
assumption. Finally, we show how to add a key confirmation feature to
KEA+ (we call the version with key confirmation KEA+C) and discuss
the security properties of KEA+C.

[http://eprint.iacr.org/2005/265.pdf](http://eprint.iacr.org/2005/265.pdf)







