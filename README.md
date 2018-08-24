# tests-maven-config

_Reference_: https://maven.apache.org/surefire/maven-surefire-plugin/  
_Reference_: https://maven.apache.org/surefire/maven-failsafe-plugin/

```
<maven.test.plugin>2.22.0</maven.test.plugin>
```

# manual

* unit tests
    * `maven-surefire-plugin`
    * maven phase: `test`
    * `maven-surefire-plugin` phase: `test`
    * run in parallel
    * naming conventions: https://maven.apache.org/surefire/maven-surefire-plugin/examples/inclusion-exclusion.html
    ```
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-plugin</artifactId>
        <version>${maven.test.plugin}</version>
        <configuration>
            <forkCount>4</forkCount>
            <reuseForks>true</reuseForks>
            <argLine>-Xmx1024m -XX:MaxPermSize=256m</argLine>
        </configuration>
    </plugin>    
    ```
    
* integration test
    * `maven-failsafe-plugin`
    * maven phase: `verify`
    * `maven-failsafe-plugin` 
        * phase: `integration-test` (without junit)
        * phase: `verify` (with junit)
    * should not be run in parallel
    * naming conventions: https://maven.apache.org/surefire/maven-failsafe-plugin/examples/inclusion-exclusion.html
    * **remark**: unit tests should be fired in `verify` phase also
    ```
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-failsafe-plugin</artifactId>
        <version>${maven.test.plugin}</version>
        <executions>
            <execution>
                <goals>
                    <goal>integration-test</goal>
                    <goal>verify</goal>
                </goals>
            </execution>
        </executions>
    </plugin>
    ```
    
# project description
We have 3 tests:
    * `GroovyTest`:
        ```
        def "test"() {
            println 'GroovyTest'
            expect:
            true
        }    
        ```
    * IntegrationIT
        ```
        @Test
        public void test() {
            System.out.println("IntegrationIT");
            assertTrue(true);
        }    
        ```
    * JUnit4Test
        ```
        @Test
        public void test() {
            System.out.println("JUnit4Test");
            assertTrue(true);
        }    
        ```