It’s a good idea to write down the testing strategy you used to create a test suite: the partitions, their subdomains, and which subdomains each test case was chosen to cover. 

```java
public class MaxTest {
    /*
     * Testing Strategy
     * 
     * partition:
     *     a < b
     *     a > b
     *     a = b
     */

    // covers a < b
    @Test
    public void testALessThanB() {
        assertEquals(2, Math.max(1, 2));
    }
    


}
```