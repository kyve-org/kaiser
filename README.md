<p align="center">
  <a href="https://kyve.network">
    <img src="https://user-images.githubusercontent.com/62398724/137493477-63868209-a19b-4efa-9413-f06d41197d6d.png" style="border-radius: 50%" height="96">
  </a>
  <h3 align="center"><code>@kyve-org/kysor</code></h3>
  <p align="center">🚀 The Cosmovisor of KYVE</p>
</p>

## Why use KYSOR

KYVE has a broad ecosystem of projects archiving their data with KYVE. To standardize different data from different projects KYVE created special runtimes for standard type like `@kyve/evm` for all EVM based chains. This has great benefits like standardized data but also has downsides for protocol node runners in terms of user experience.

Without KYSOR for every pool the node runner has to get the binaries manually. If you want to run on another pool which has a different runtime you again have to manually obtain the binaries. Furthermore, if a pool ever upgrades to a newer protocol node version, you have the same procedure like before. Even worse, you might miss the update and receive a timeout slash for being offline

**Running nodes with KYSOR has the following benefits:**

- Only use **one** program to run on **every** pool
- Not installing and compiling protocol binaries **manually** for every pool
- Getting the new upgrade binaries during a pool upgrade **automatically** and therefore **don't risk timeout slashes**
- Make running protocol nodes **standardized** and **easier**

## Installation

Currently, there are no binaries for the KYSOR, therefore it has to be installed and build manually by cloning the repository:

```bash
git clone https://github.com/kyve-org/kysor.git
cd kysor
```

## Setup

General directory structure and important files:

- `src` contains all the source code of KYSOR
- `kysor.conf.ts` is the main config file. It is explained in more detail here
- `secrets` is the directory where your secrets are stored like `arweave.json` and `mnemonic.txt`
- `pools` will be autogenerated once you start KYSOR and will contain all binaries structured by pool and version
- `db` will be autogenerated once you start KYSOR and will contain all caching data for the protocol nodes
- `logs` will be autogenerated once you start KYSOR and will contain all log data for the protocol nodes

In order to setup KYSOR the following secrets need to be added under the `secrets` directory:

- `arweave.json` this is the arweave keyfile you need to provide in order to run protocol nodes. This one keyfile will be used for **all** of your protocol nodes
- `mnemonic.txt` this is the file which contains your mnemonic of your validator account. This one mnemonic will be used for **all** of your protocol nodes

After adding the required secrets you can check the `kysor.conf.ts` file. When you initially open it, it should look like this:

```ts
import { IConfig } from "./src/faces";

const config: IConfig = {
  // target of the host machine, can be either "linux" or "macos"
  // important for downloading the correct binaries
  hostTarget: "linux",

  // whether KYSOR should auto download new binaries
  // if set to false, you have to insert the binaries manually
  autoDownload: true,

  // whether KYSOR should verify the checksums of downloaded binaries
  // if autoDownload is false this option can be ignored
  verifyChecksums: true,

  // settings for protocol node
  // notice that mnemonic and keyfile is missing, those need to be files under the secrets directory
  protocolNode: {
    poolId: 0,
    network: "korellia",
    initialStake: 100,
    space: 1000000000,
    verbose: true,
  },
};

export default config;
```

After entering your config like the `poolId` and your `initialStake` and other settings that are required to run a protocol node on a pool you can start KYSOR by running

```
yarn start
```

When you see logs like this everything should work fine

```ts
2022-05-19 13:54:00.299  INFO  Starting KYSOR ...
2022-05-19 13:54:00.299  INFO  Validating files ...
2022-05-19 13:54:00.300  INFO  Found kysor.conf.ts
2022-05-19 13:54:00.300  INFO  Found arweave.json
2022-05-19 13:54:00.300  INFO  Found mnemonic.txt
2022-05-19 13:54:00.300  INFO  Attempting to fetch pool state.
2022-05-19 13:54:00.543  INFO  Fetched pool state
2022-05-19 13:54:00.543  INFO  Binary with version 1.0.5 not found locally
2022-05-19 13:54:00.544  INFO  Found downloadable binary on pool
2022-05-19 13:54:00.545  INFO  Downloading https://github.com/kyve-org/evm/releases/download/v1.0.5/kyve-linux.zip?checksum=8662ddeb3a3e0eaedbbb5a665a4fa0eed12448349218ef0d98afed25cfacd60c ...
2022-05-19 13:54:01.910  INFO  Extracting binary to bin ...
2022-05-19 13:54:02.664  INFO  Comparing binary checksums ...

2022-05-19 13:54:02.666  INFO  Provided checksum  = 8662ddeb3a3e0eaedbbb5a665a4fa0eed12448349218ef0d98afed25cfacd60c
2022-05-19 13:54:02.666  INFO  Local checksum     = 8662ddeb3a3e0eaedbbb5a665a4fa0eed12448349218ef0d98afed25cfacd60c

2022-05-19 13:54:02.666  INFO  Checksums are equal. Continuing ...
2022-05-19 13:54:02.666  INFO  Starting child process ...


2022-05-19 13:54:06.858  INFO  Starting node ...

2022-05-19 13:54:06.861  INFO  Name              = impressed-emerald-spoonbill
2022-05-19 13:54:06.953  INFO  Address           = kyve1eka2hngntu5r2yeuyz5pd45a0fadarp3zue8gd
2022-05-19 13:54:06.954  INFO  Pool Id           = 0
2022-05-19 13:54:06.954  INFO  @kyve/core        = v1.0.8
2022-05-19 13:54:06.954  INFO  @kyve/evm         = v1.0.5

2022-05-19 13:54:06.955  DEBUG Attempting to fetch pool state.
2022-05-19 13:54:07.245  INFO  Running node on runtime @kyve/evm.
2022-05-19 13:54:07.248  INFO  Pool version requirements met
2022-05-19 13:54:07.249  INFO  Fetched pool state
2022-05-19 13:54:07.349  INFO  Node is already staked. Skipping ...
2022-05-19 13:54:07.350  INFO  Running node with a stake of 100.0000 $KYVE
```
