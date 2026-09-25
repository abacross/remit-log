# The Abacross Remit log

This is Abacross's public [Remit](https://github.com/abacross/remit) log: an append-only record of every warrant under which Abacross's own agents may act in AWS, and of the reconciliation results that check what they did.
Remit's broker issues no AWS session for a warrant until the warrant is proven to be in this log (Remit SPEC section 9.3), so a warrant that was never logged was never used.

It is served as static files in the [tlog-tiles](https://c2sp.org/tlog-tiles) layout at `https://abacross.github.io/remit-log/`: `checkpoint` is the log's current size and root, signed by the log and cosigned by its witness, and `tile/` holds the Merkle tree and the entries.

## Checking it

`policy.txt` names the log's key, the witness keys to require, and how many.
With the [Remit command](https://github.com/abacross/remit), a proof that a chain of warrants is in this log checks offline:

```
remit log verify --policy policy.txt --chain <chain> --proof <proof>
```

Reconciliation results appear as signed commitments only: their contents name Abacross's AWS account, which it does not publish.

## What the witness means today

The log's only witness so far is run by Abacross, which protects against nothing Abacross itself might do.
A witness's value is that it is someone else: it cosigns only a checkpoint consistent with every one it has seen, so a log cannot show one history to one reader and another to another without every witness a reader trusts going along.
Anyone can run one with `remit witness serve` and ask to be added; a reader's own policy decides which witnesses count.

The witness signs with ML-DSA-44, the post-quantum form [tlog-cosignature](https://c2sp.org/tlog-cosignature) recommends for new witnesses.
