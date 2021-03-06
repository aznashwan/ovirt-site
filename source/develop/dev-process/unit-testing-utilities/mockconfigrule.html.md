---
title: MockConfigRule
category: unit-testing-utilities
authors: amureini
wiki_category: Unit Testing Utilities
wiki_title: MockConfigRule
wiki_revision_count: 2
wiki_last_updated: 2012-05-16
---

# Mock Config Rule

`MockConfigRule` is JUnit `@Rule` that handles mocking of the Config class. This removes the need of PowerMocking `Config`, and considerably speeds up the test.

## Creating the Rule

Like with any `@Rule`, the only thing you need to do in order to incorporate it in your test class is to declare a public member annotated with `@Rule`:

    @Rule
    public static final MockConfigRule mcr = new MockConfigRule();

## Mocking Different Config Values per Tets

Now that you have the `MockConfigRule` defined, you can call the `mockConfig` method to mock a configuration value. E.g.:

    public void testSomethingRegardingLDAP() {
       mcr.mockConfig(ConfigValues.LDAPSecurityAuthentication, Config.DefaultConfigurationVersion, "SIMPLE");
       // rest of the test the relies on the LDAPSecurityAuthentication configuraion.
    }

Note that if you ommit the version parameter, `Config.DefaultConfigurationVersion` will be used by default:

    public void testSomethingRegardingLDAP() {
       mcr.mockConfig(ConfigValues.LDAPSecurityAuthentication, "SIMPLE");
       // rest of the test the relies on the LDAPSecurityAuthentication configuraion.
    }

## Mocking The Same Config Values for the Entire Test Suite

The above appoarch is comfortable when each test requires a different configuration, but sometimes, you'd like you entire test-suite to use the same configurations. This can be done with a `@Before` annotation, but that would be tedious and repetitive. `MockConfigRule` provides an easier way to do this, in the `@Rule`'s construction tume, using the `mockConfig` static creator, e.g.:

    @Rule
    public static final MockConfigRule mcr = new MockConfigRule(
                        mockConfig(ConfigValues.LDAPSecurityAuthentication, "SIMPLE"),
                        mockConfig(ConfigValues.SearchResultsLimit, 100),
                        mockConfig(ConfigValues.AuthenticationMethod, "LDAP"),
                        mockConfig(ConfigValues.DBEngine, "postgres")
                );

[Category:Unit Testing Utilities](Category:Unit Testing Utilities)
