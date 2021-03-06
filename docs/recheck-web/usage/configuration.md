# Configuration

Additionally to the [***recheck*** configuration](../../recheck/usage/configuration.md), you can configure some additional properties within ***recheck-web***.

## RecheckWebOptions

The [`RecheckWebOptions`](https://github.com/retest/recheck-web/blob/master/src/main/java/de/retest/web/RecheckWebOptions.java) are an extended form of the plain `RecheckOptions`. Similarly, they are created using the builder pattern and should be a drop-in replacement for the `RecheckOptionsBuilder`.

```java
RecheckWebOptions options = RecheckWebOptions.builder()
    // Do your configuration here
    .build()
```

!!! bug
    The `RecheckOptionsBuilder` contains currently a bug, where the ***recheck-web*** specific options must be set before the common ***recheck*** options.

### Usage

Same as the `RecheckOptions` the `RecheckWebOptions` can be passed to the [`RecheckImpl`](https://github.com/retest/recheck/blob/master/src/main/java/de/retest/recheck/RecheckImpl.java).

```java
RecheckWebOptions opts = RecheckWebOptions.builder()
    // Do your configuration here
    .build();
Recheck re = new RecheckImpl( opts );
```

!!! note
    The `RecheckWebOptions` should always be used instead of the `RecheckOptions` when using ***recheck-web***.

### Options

Below is a list of the available options you may configure with corresponding methods on `RecheckWebOptionsBuilder`. Please refer to the detailed sections below.

| Option                | Default                                                                                                                                                | Description                                                                             |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------- |
| `checkNamingStrategy` | [`CounterCheckNamingStrategy`](https://github.com/retest/recheck-web/blob/master/src/main/java/de/retest/web/selenium/CounterCheckNamingStrategy.java) | Defines the naming strategy used by the `AutocheckingDriver` for the naming the checks. |
| `screenshotProvider`  | [`ViewportOnlyScreenshot`](https://github.com/retest/recheck-web/blob/master/src/main/java/de/retest/web/screenshot/ViewportOnlyScreenshot.java)       | Defines the screenshot strategy that is used by snapshotting the `WebDriver`.           |

### Example

```java
RecheckWebOptions.builder()
        .checkNamingStrategy( new CustomAutocheckingCheckNamingStrategy() )
        .screenshotProvider( new ViewPortOnlyScreenshot() )
        .namingStrategy( new ClassAndMethodBasedShortNamingStrategy() )
        .projectLayout( new GradleProjectLayout() )
        .suiteName( "my-custom-suite-name" )
        .enableReportUpload()
        .addIgnore( "MyCustomIgnore.filter" )
        .build();
```

### Naming Checks Automatically

When using the `AutocheckingDriver`, every action performed will generate a check with a provided `Recheck` instance. Since those checks require a name, it will be uniquely generated by the provided `AutocheckingCheckNamingStrategy`.

### Configuring Screenshots

The creation of a check triggers a screenshot capturing of the `WebDriver` or `WebElement` supplied. This screenshot is only used for documentation purposes and not the comparison (e.g. pixel differences). Since screenshot are quite slow and large, they may be changed or disabled by using a provided implementation of `ScreenshotProvider`.

There is a global system property available to configure these screenshots. However, they are overwritten by the local option defined. This allows you to e.g. globally disable screenshots during test execution for performance reasons, but enable them locally during Golden Master creation.

## Properties

Additionally to [***recheck*** properties](../../recheck/usage/configuration.md), ***recheck-web*** provides some more properties to configure.

```properties
# Configure the strategy of how screenshots are taken.
# fullPage | viewportOnly | viewportOnlyMinimal | none
de.retest.recheck.web.screenshot.provider=viewportOnlyMinimal
```
