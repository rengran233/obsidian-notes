`java.util.Random` is used to generate pseudorandom numbers.
```Java
import java.util.Random;
// use a integer seed to create a specific random generator
Random rand = new Random(seed);
// use a integer bound to restrict generation range in [0, bound)
rand.nextInt(bound);
```