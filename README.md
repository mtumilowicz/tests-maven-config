[![Build Status](https://travis-ci.com/mtumilowicz/tests-maven-config.svg?branch=master)](https://travis-ci.com/mtumilowicz/tests-maven-config)

# tests-maven-config

_Reference_: https://maven.apache.org/surefire/maven-surefire-plugin/  
_Reference_: https://maven.apache.org/surefire/maven-failsafe-plugin/

# manual
* plugins version
    ```
    <maven.test.plugin>2.22.0</maven.test.plugin>
    ```
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
We have 3 tests (that follow naming conventions):
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
so running:
* maven clean test:
    ```
    [INFO] -------------------------------------------------------
    [INFO]  T E S T S
    [INFO] -------------------------------------------------------
    [INFO] Running GroovyTest
    [INFO] Running unit.JUnit4Test
    JUnit4Test
    [INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.116 s - in unit.JUnit4Test
    GroovyTest
    [INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.908 s - in GroovyTest
    [INFO] 
    [INFO] Results:
    [INFO] 
    [INFO] Tests run: 2, Failures: 0, Errors: 0, Skipped: 0
    ```

* maven clean verify
    ```
    [INFO] -------------------------------------------------------
    [INFO]  T E S T S
    [INFO] -------------------------------------------------------
    [INFO] Running GroovyTest
    [INFO] Running unit.JUnit4Test
    JUnit4Test
    [INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.117 s - in unit.JUnit4Test
    GroovyTest
    [INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.803 s - in GroovyTest
    [INFO] 
    [INFO] Results:
    [INFO] 
    [INFO] Tests run: 2, Failures: 0, Errors: 0, Skipped: 0
    [INFO] 
    [INFO] 
    [INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ tests-maven-config ---
    [INFO] Building jar: C:\Users\user\repo\tests-maven-config\target\tests-maven-config-1.0-SNAPSHOT.jar
    [INFO] 
    [INFO] --- maven-failsafe-plugin:2.22.0:integration-test (default) @ tests-maven-config ---
    [INFO] 
    [INFO] -------------------------------------------------------
    [INFO]  T E S T S
    [INFO] -------------------------------------------------------
    [INFO] Running integration.IntegrationIT
    IntegrationIT
    [INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.128 s - in integration.IntegrationIT
    [INFO] 
    [INFO] Results:
    [INFO] 
    [INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
    ```

* failsafe integration-test
    ```
    [INFO] -------------------------------------------------------
    [INFO]  T E S T S
    [INFO] -------------------------------------------------------
    [INFO] Running integration.IntegrationIT
    IntegrationIT
    [INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.076 s - in integration.IntegrationIT
    [INFO] 
    [INFO] Results:
    [INFO] 
    [INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
    ```