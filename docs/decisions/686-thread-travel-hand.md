## #686 - lathe_thread_travel builds and passes again

**Decided:** two test-only repairs in `crates/app/tests/lathe_thread_travel.rs`:
1. the `OpParams::Threading` literal sets `hand: ThreadHand::Right`;
2. the short-X fixture rewrites only the X stroke's top (`"max": 320.0`)
   instead of the whole `"min": 0.0, "max": 320.0` pair.

**Why:** #643 added the suite; two later merges broke it without a textual
conflict. #639 added `hand` to `OpParams::Threading`, so the binary stopped
compiling. 53e4aebb moved the built-in Lathe's X minimum to -2.5, so the
fixture's replace matched nothing and the guard test failed on its own
"fixture rewritten" check (hidden until the compile error was fixed). The
bar and insert describe an ordinary right-hand thread, which every other
test literal also uses. Matching only the maximum keeps the over-travel
guard independent of where the X minimum sits.

**Considered and dropped:** a struct-update default on
`OpParams::Threading` so test literals survive new fields - a product-type
change for a test-only miss; parsing the .mch and editing the stroke as
data - more code for the same check.

**Cost to change later:** none; test-only.
