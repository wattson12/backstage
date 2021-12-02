## XCMetrics Setup Notes

### Local Server

Followed the instructions [here](https://github.com/spotify/XCMetrics/blob/main/docs/Run%20the%20Backend%20Locally.md)

### XCMetrics Executable 

Followed the instructions [here](https://github.com/spotify/XCMetrics/blob/main/docs/Getting%20Started.md)

### Build Target 

I added this script via tuist:
```
func xcMetricsScript(for name: String) -> String {
    """
    ${SRCROOT}/../XCMetrics/XCMetricsLauncher ${SRCROOT}/../XCMetrics/.build/release/XCMetrics --name XCMetricsTestProject --buildDir ${BUILD_DIR} --serviceURL http://localhost:8080/v1/metrics
    """
}
```

Example of script:
```
buildAction: .buildAction(
    targets: [.project(path: ".", target: "Networking")],
    preActions: [],
    postActions: [
        .init(
            title: "XCMetrics",
            scriptText: xcMetricsScript(for: "Networking"),
            target: .project(path: ".", target: "Networking")
        )
    ],
    runPostActionsOnFailure: true
),
``` 

### Backstage 

I used instructions [here](https://backstage.io/docs/getting-started/running-backstage-locally) to run backstage locally and logged in via tests

### XCMetrics Plugin

I followed the instructions [here](https://github.com/backstage/backstage/tree/master/plugins/xcmetrics) but needed to make some changes:
- [This](https://github.com/yarnpkg/yarn/issues/7807) issue blocked me from adding the package so I needed to run `yarn policies set-version 1.18.0` first (pinning yarn to a lower version)
- I set `target: http://0.0.0.0:8080/v1` instead of `target: http://127.0.0.1:8080/v1` to match the local host environment

### Order

#### Run XCMetrics Backend Locally 

Run this from the `XCMetrics` root directory
```
docker-compose -f docker-compose-local.yml up
```

### Build Backstage

Run these from backstage root directory 

```
yarn install 
yarn tcs
yarn build
```

#### Run Backstage backend 

Run `yarn start` from `packages/backend`

#### Run Backstage frontend

Run `yarn start` from backstage root directory. This should open backstage at http://localhost:3000. Choose guest authentication and then navigate to http://localhost:3000/xcmetrics
