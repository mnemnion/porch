# Nock -> nck algorithm

This is a public domain, English language exploration of an algorithm to compress Nock into a grammar. This compressed form of Nock is known as nck, pronounced nick. 

This document is thoroughly room temperature. If it proves fruitful, I will chill it.


Nock is formally a binary tree, which is 80% of why I like it. Every cell is a node with exactly two children, and every atom may be considered a leaf. 

n-ary trees are also a product of context-free grammars, given a linear input stream. This means Nock can be compressed into such a grammar, where every cell has exactly two resolutions (left or right), and each possible atom found within the structure is the sole option at a single terminal rule. 

Each of these rules is given a number, and we may as well count from zero. It is probably helpful to have an epsilon rule, that may match any atom, and we will call this rule 0. Rule 1 is the noun we are compressing, rule 2 is the subject and rule 3 is the formula of that noun, and so on, that is, rule 2 for a cell subject, rule 3 for a cell formula, and other rules for atomic terminations. This assumes that both the subject and formula of the Nock noun are cells, which is good for the UR-rule as it keeps the resulting graph extremely compact. 

So, for the degenerate case of a noun that is an atom, we have rule zero, epsilon, and rule 1, the atom, and a bitstream consisting of the bit 0, meaning 'choose the leftmost option of rule 1'. Left will conventionally represent the termination of a rule with one resolution. 

This is slightly larger, rather than compressed. Don't nck atoms. I mean, it's harmless, just, don't.

The noun is therefore represented as series of left-right, 0-1 bits that traverse the graph and end in the correct atoms. This is simple nck, it generates a unique rule for each atom and never uses the Epsilon rule. It will match the Nock noun for which it was created, because it's a straightforward compression of that noun.  

So, presuming we use the same Nock noun, the binary stream needs no terminating clauses. It simply reads out until it hits terminal atoms, then backtracks to unresolved branches and continues filling them in. 

So how far do we get on one bit? Each bit in simple nck will get us back a single noun. These are given back to us depth first, left-to-right, that is, this is the order that the Nock tree is reconstituted in. If we were generating ASCII we would make left brackets when we reach cells, atom strings when we reach atoms, and right brackets upon the final backtrack out of a cell rule. 

So now, instead of a binary tree which only flows down, we have a DAG and a path through it. This is straightforward stuff. 

Let's observe a property: this algorithm makes the rules in the order it finds them, but that order is not used in decompression. The numbers are just names, the order an artifact of their discovery: the names can be permuted without loss of structure. 

This means in principle we can reuse the ruleset from one Nock noun to nck another Nock noun (which I am compulsively spelling nwn). This will work, in the sense that we'll have a superset of rules that will decompress both nouns. It will not work as well as we want, because our atoms are hard coded. This means we'll have large subgraphs that differ only by a few atoms, but poor reuse of that structure, since resolution to a different atom will rebound a considerable distance up the tree. 

It's not even totally clear that we'd end up with anything but a second stack of rules on top of the first. The product of two ncked nouns would be the size of the sum of the two nouns ncked separately. Not what we want. 

Nock has no concept of a variable, at all. nck, however, does: rule 0 resolves to any atom, because it's special. We aren't using it, yet, and it can't appear in the uncompressed Nock: Hoon resolves all variables, it must, because Nock has none. 

Let's zoom out, for a second: we're looking for a way to use Nock that doesn't rely on hinting to produce jet-assisted code. To do this, one step is to put all the jet-assisted formulas at the top of the rule stack, in a way that lets us reuse the rule stack on later nouns. Ideally, the very front are the hardware-level jets, followed by the most important Hoon code, and so on, embodying the entire Hoon kernel in a single compressed ruleset that all nouns are subsequently prepended with during the nckdown stage. 

This requires some changes to our algorithm, and probably to the Hoon toolpath. 

##Nockup

There's no special reason Hoon should have to emit Nock directly under all circumstances. For simplicity, I'll pretend Hoon emits an ASCII Nock file, that is then compressed into rules and a binary stream. We've intentionally avoided talking about rule encoding, but it ain't hard. So all we've used from the ASCII set are the digits, brackets, and two kinds of whitespace, 32 and 10.  This is another reason to consider Operator 10 hateful; any Hoon code will be liberally sprinkled with newlines, though ASCII 9 is banned for good reason. 

We're going to extend this with some markup. Now we're emitting Nockup, which, like Nock, can be compressed into nck. But with a difference!

What we then add is a placeholder character, call it @ for atom, that can match to any atom. Ordinary Hoon never emits this symbol, it is relegated to a special kind of definition that implies the definition of a jet assisted formula. It will still be possible to late-bind a rule in this fashion, but it's bad practice if you're not building a rule header. Hoon doesn't need variables; Nockup is the wrong place to add them, in general. 

When the nck compressor hits an @, it 'throws' a rule. This means it gives the rule a high number, one high enough that it won't interfere with the header you're generating. This is why it's a bad idea to go strewing @s willy-nilly into your code base: throwing is potentially expensive if you're not doing it in early rules.

This high numbered rule has one match: Rule 0, Epsilon.

What you end up with is a deterministic header containing all the formula structures you wanted, with a single rule number that matches the start of each formula. You line them up in order on the left of the rule tree, with the formula structures off to the right, where you don't have to look at them if you don't want to. All the @s are gone, each one has been replaced with a rule that resolves to 0/epsilon, and all of those rules are above the header rules. The header is now frozen; we can load it into memory and use it for every Hoon executable that's in compatible Nockdown.

When you pass another Nock noun through the rules, you eliminate every variable in formulas that are reached. Here's how, minus confusing optimizations. 

We feed the noun in by attempting to go left, which is equivalent to checking if each structure that we've pre-loaded is the same as the first formula of the noun. Failure to match structure backtracks, success records a bitstream. If we find it, there will be rules that resolve to Epsilon, meaning any atom is ok. There will also be rules that resolve to solid atoms, and if that doesn't match, we have a failure to match structure.

This is O(omg), roughly, so we're going to avoid doing it when possible. More Nockup, later. 

If we reach an Epsilon, it's on a throw-rule, which means we're out of the part of the header that we don't want to change. So we add the atom in question as a resolution of the rule. There's some late-binding we have to do here, because until we're done compressing the Nock noun, we don't know how many options there will be. Conceivably it's a lot, but once the Nock noun is fully compressed, it will be a finite number of atoms at the terminal of each formerly Epsilon-bound rule. At that point, we know how many bits are needed to resolve the terminal rule, and we remove the Epsilon from the terminal rule. We then encode the final atom using only the number of bits needed, with the next bit after that a backtrack. 

Here's our last use of Rule 0: it gives us an unambigous end to the bit stream. The left option from Rule 1 is Rule 0, so if we backtrack all the way back to Rule 1 and go left, we exit, since we should never match rule 0 during execution or decompression. This is good because a bitstream can contain padding on the end to fill out the word of the target architecture; if that padding is all zeros, the noun will exit correctly. 

This modifies our earlier formula: Rule 1 is the egress, resolving to Rule 0 or Rule 2. Rule 2 is the noun in question, with all the precompiled header rules on the left and the rest of the Nock on the right. 

That means that every time you are directed left on rule 2, you are in jet land, and you can cheat as much as you want, matching substrings willy-nilly, passing variables around, calculating swiftly and returning to ground. When you go right on Rule 2, you're in Nockville, execute accordingly. 

This is how we avoid O(omg) behavior: we add another character to Nockup. # is the hash operator, and it should be followed by a hash of the formula it's hinting at. This spares us the labor of checking down the left side of the tree when matching formulas that should be jet-assisted: the nck compressor can use the hints to check a table and provide a bitstream that follows the requisite rule path and late-binds the atom 'variables'. 

In fact, a production Nock interpreter doesn't really spend much time looking at rules it won't execute. There's a simple bytestream that encodes a jet assisted rule, and it starts with 10 (right from rule 1 into rule 2, left from rule 2 into Jet-land). That would be enough if we wanted to execute nck directly, but imposing a decompression penalty on executable code at the time of execution is foolhardy. Similarly, if we unroll the nck into pure Nock, we've lost the jet capacity, because there are No Hints In Nock '4K'.

That means we execute Nockdown, a partially-unrolled, interpreter-specific encoding of Nock interspersed with jet streams. I do not care how this is encoded, because it is not Nock, and therefore it is in practice dependent, not on the Nock, but on the Nock + Jet combination. These should be the same, but we now are back in a state where Nock cannot be made indeterminate by changing it. 

There are no variables in Nockdown. There aren't even "variables". There should be no use of Rule 0 at all, because Nockdown doesn't actually have a grammar, it's been unrolled and the left JetLand side of the stack replaced with call-specific binary code that will serve the same purpose. The header rules are kept around, and the hash makes it easy to look up the Nock, unroll it with the bound values, and execute it if necessary. 

Nockdown can be compressed back into nck, or unrolled into Nock, by the same interpreter and compressor that generated it. Both nck and Nock are completely general and hint-free: the rule order of the nck grammar can be used as a guide to what is expected to be jetted, and a header (which should be pure Nock that crashes on 42 right before it gets to the nck encoding) could provide a helpful summary of, say, what Hoon version produced the nck in question.  

Nockdown is a format where left moves on Rule 2 are compressed into a format useful for the Nock interpreter, whereas right moves on Rule 2 are completely unrolled into bog-standard binary Nock. This is beyond the scope of this document and well past the point where it's worth being specific, without either getting some feedback from @cgyarvin or forking off.

So what do we get here? We now have two cold formats and two warm ones: nck and Nock are chilly beasts, and Nockup and Nockdown are exactly as chilly as Hoon and no colder. The nck function mediates: it can accept and emit nck and Nock, as well as the particular flavors of Nockup and Nockdown it's written for. The Hoon compiler emits Nockup, which, as a superset, can be pure Nock. 

What we pass around is nck; it is pure, small, very cold, and contains only suggestions as to how to jet stream it. Different interpreters vary only in speed, not binary compatibility, and can make a good-faith effort to figure out which jets to provide for unfamiliar nck formats. In practice, given Urbit, any publicly available nck 'format' should have as many jet sets as it needs available in the agora. Fully unrolled Nock is a rarity, and difficult to pack down into the 'format' of nck that it came from, but is always available at any time, from within any of the four fundamental forms. 

## Separation of Concerns.

Let's back out a little and see what we've done to ourselves. Fundamentally, this is refactoring. 

First, we have Nock '4K', or Nock 0-9 to presume less. It is hintless, and hence hard to execute efficiently. It is only used in the pure form to expand some noun to a canonical state; because we cannot execute it effiently, we seldom do. 

Second, we have nck. nck is fundamentally compressed Nock, consisting of a grammar followed by a path through the grammar that reconstitutes the original tree. However, not all nck can be unrolled into Nock, since the grammar rules can be made semi-generic and separated from any bitstream that reconstitutes a noun. Perhaps we should call these bare headers nc. nc across any noun at all will produce nck, which can then be unrolled into Nock. 

A nc(k) grammar has information, in the form of the rule order, that is not used in uncompressing Nock from nck. In addition, subgraphs in an nc grammar may contain unresolved atoms that point to Rule 0; in order for a noun to invoke those rules, these atoms must be resolved as part of the process of compression. 

Any Nock noun at all may be compressed into an nc grammer. To do so, we must perform fairly extensive subgraph matching, if we wish to use the rules in the nc grammar and not merely append our own. This may take awhile, but it surely can be done, without any hinting at all, as a one-time price of compilation.

If we wish to make this faster, we can employ Nockup, which we also need to make generic nc rules. Nockup adds only two things to Nock: a pattern that matches any atom, and a hint that a compatible ncknock compressor can use to match the underlying subgraph in constant time. Thus, Nockup does exactly two additional things: populates generic rules, and hints at the use of those generic rules in specific code. ncknock may use those hints or ignore them, but must verify the shape of the subgraph against the grammar rule before consenting to encode it. Hints exclusively help ncknock find the subgraph to be matched faster. 

This leaves Nockdown, which is a binary, execution specific partial unrolling of nck in which everything jetted is expressed in the fastest possible binary format for jet execution and all other Nock is fully unrolled and interpreted. Nockdown is not any one format, though the Martian goal would be to have one canonical Nockdown that corresponds to fast-executing Hoon code. 

This is where the yakety-sax starts playing, because Nock 5K, complete with grungy hints, is a perfectly reasonable Nockdown. The problem is reduced to the status of middleware: cleaning up the Hoon compiler's output to be a binary form of Nockup or something like it, writing ncknock, ncknocking the latest Hoon kernel, and writing another function that unrolls Hoon-encoded nck into Nockdown, which starts with Nock 5k but is no longer constrained by its execution semantics. 

Note that Nockdown never interacts with Nockup at all. Nockup is used for generics and fast subgraph matching, exclusively, and Nockdown is used only and entirely to provide whatever jets are available in the execution environment. Nocking down a nck file produced with different jet expectations may take longer, contain more Nock and less Jet, and run slower as a result. But it will produce the same result in all cases, and if not, we fault the jets, always, because our Nock contains no hints and Cannot Be Undetermined. 

Nockdown is an execution format, not an exchange format or a reference format. Nockdown is produced from nck and cached; the nck is canonical and is all that is ever traded, called, updated or referenced.

The relationship between nck and Nock is that nck <-> Nock is information preserving with respect to correctness of results, but may be destructive to speed if done naively. 


