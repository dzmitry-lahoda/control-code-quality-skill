## Units

Because: 
- too many Rust features are long time behind unstable
- lack of support of reasonable(for our domain) TryFrom and Add amid NonZero, 128 and 64 bit numbers 
- Rust prevents extending other crates with other crate traits,
- main [primitive extending crates are out of date](https://github.com/rust-num/num-traits/pull/346),
- no decent and [safe](https://github.com/danlehmann/arbitrary-int/issues/53) [custom ranges](https://github.com/danlehmann/arbitrary-int/issues/37) numbers in Rust,
- no [maintained](https://github.com/iliekturtles/uom/pull/529) and [composable generic composable multibase units of measure](https://yoric.github.io/post/uom.rs/),
- nor can type binding enforce custom ranges (including arbitrary)  
- nor `rust_decimal` has decent enough performance (see its iter loop during multiplication)
- 64 bit number multiplied by 64 bit can produce 128 bit one, like price x size = quote.
- nor cannot automate storage compatibilty of values (pg can do 64 signed bits, but not unsinged)

the best approach is NewType pattern to represent numerics composition,
and that is used here.
As NewTypes consistently replace binding, 
that leads to systematic reduce amount of boilerplate code with increased safety

### Financial values

Seems it is typical to have:

- values which can be only positive(trully positive like balance change or just numerically positive - depend on side of order) or negative(value which when applied only decreases other value) or both(sided size or balance)
- zeroable(for example storage at rest) or not(for example delta at single value receupts) 
- linear(must use only once, like fee taken from account to be applied to system) or not 

For example `balance` maximal count of types can be 2x2x3, but really it would be less.
This is poor man approximation of [refinements types](https://blog.yoshuawuyts.com/a-grand-vision-for-rust/).


@dzmitry-lahoda with all these newtypes we may wanna consider macro 😭

@beborngod
beborngod
4 days ago
Author
created ticket long time ago #2262 but dima closed it as not planned, perhaps we might want to consider reopening it?

@dzmitry-lahoda
dzmitry-lahoda
yesterday
• 
ids - limit to u31/u63 - use deranged i guess as javier did

@dzmitry-lahoda
dzmitry-lahoda
yesterday
use derive_more as needed for shorte impls

@dzmitry-lahoda
dzmitry-lahoda
yesterday
will add macro next time for newid (newtype id) - that seems simple and presented in other langs.

@dzmitry-lahoda
dzmitry-lahoda
yesterday
• 
@cherryman

for newnum types - i have no idea yet.

i created such only when 1. reused in several areas/features and/or composed(arithmetic) a lot with other numbrs. 2. is sent to view/host/state.

i did not new create newnum if 1 and/or 2 are not satisfied enough.

and even when creted, especially for 128 bit values, described what unit it is.

@dzmitry-lahoda
dzmitry-lahoda
yesterday
• 
NonZero newtype. It is special case. Rust lacks a lot of math and public traits for NonZero. So wrapping NonZero is cheaper in all means rather then handling their underdevelopment all time.

newnum created represent different scale of value(different unit in uow) approach. size and balances sure are different, quote of balance scale is different from qty of market size scale different from basis points from per million from risk number from funding index.

i was not big proponent of fee = balance type added in some PR and commented (people were in that comments).

@dzmitry-lahoda
dzmitry-lahoda
yesterday
need to ask @bob-the-agent opinion if current usage satisfies criterias of newnum usefulnes

