# How to enforce node engine on package.json?

- `package.json`의 `engines` 필드를 채워넣으면 된다.
  ```json
  {
    "engines": {
      "npm": "8.0.0 <9.0.0",
      "node": ">=20.0.0"
    }
  }
  ```

  - `npm`을 설정하는 경우 다음을 추가적으로 설정한다.
    - 이를 설정할 때 `npm` 커맨드를 실행할 때 마다 머신의 npm이 강제되고 있는지 확인한다.
      ```shell
      # .npmrc
      engine-strict=true
      ```



- 스크립트를 실행하기 전에 패키지 버전을 체크한 후 실행할 수 있다.
  ```js
  /* eslint-disable no-console */
  const fs = require('fs');
  const semver = require('semver');
  const childProcess = require('child_process');

  // checks that current node and npm versions satisfies requirements in package.json
  // to run manually:   node check-version.js [verbose]

  const VERBOSE_FORCED = false;    
  const args = process.argv.slice(2);
  const VERBOSE = VERBOSE_FORCED || (args.length > 0 && args[0] === 'verbose');

  const printErrAndExit = (x) => {
    console.error(x);
    console.error('Aborting');
    process.exit(1);
  };

  const checkNpmVersion = (npmVersionRequired) => {
    if (!npmVersionRequired) {
      console.log('No required npm version specified');
      return;
    }
    const npmVersion = `${childProcess.execSync('npm -v')}`.trim();
    if (VERBOSE) console.log(`npm required: '${npmVersionRequired}' - current: '${npmVersion}'`);
    if (!semver.satisfies(npmVersion, npmVersionRequired)) {
      printErrAndExit(`Required npm version '${npmVersionRequired}' not satisfied. Current: '${npmVersion}'.`);
    }
  };

  const checkNodeVersion = (nodeVersionRequired) => {
    if (!nodeVersionRequired) {
      console.log('No required node version specified');
      return;
    }
    const nodeVersion = process.version;
    if (VERBOSE) console.log(`node required: '${nodeVersionRequired}' - current: '${nodeVersion}'`);
    if (!semver.satisfies(nodeVersion, nodeVersionRequired)) {
      printErrAndExit(`Required node version '${nodeVersionRequired}' not satisfied. Current: '${nodeVersion}'.`);
    }
  };

  const json = JSON.parse(fs.readFileSync('./package.json'));
  if (!json.engines) printErrAndExit('no engines entry in package json?');
  checkNodeVersion(json.engines.node);
  checkNpmVersion(json.engines.npm);
  ```