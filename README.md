# CADIP Extension Specification

- **Title:** CADIP
- **Identifier:** <https://rs-python.github.io/cadip-stac-extension/v1.0.0/schema.json>
- **Field Name Prefix:** cadip
- **Scope:** Item, Assets
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Pilot
- **Owner**: @vprivat-ads

This document explains the CADIP Extension to the [SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.
Allows to describe Copernicus CADIP sessions from Acquisition Ground Stations as STAC items.

- Examples:
  - [Item example](examples/item.json): Shows the basic usage of the extension in a STAC Item
- [JSON Schema](json-schema/schema.json)
- [Changelog](./CHANGELOG.md)

## Item Properties

| Field Name                | Type          | Description                                                         |
| ------------------------- | ------------- | ------------------------------------------------------------------- |
| cadip:id                  | string (uuid) | **REQUIRED**. UUID for the Session instance within the CADIP        |
| cadip:acquisition_id      | string        | **REQUIRED**. Acquisition id code as from the acquisition plan      |
| cadip:num_channels        | \[number]     | This is the number of channels for the session (1, 2, 3, 4)         |
| cadip:station_unit_id     | string        | 2 digit X-band station unit ID                                      |
| cadip:antenna_id          | string        | Identification of the antenna used                                  |
| cadip:front_end_id        | string        | FEP identifier. Usually each Station used two acquisition chains    |
| cadip:planned_data_start  | datetime      | Planned Date/Time start of the downlink as extracted from SAPF file |
| cadip:planned_data_stop   | datetime      | Planned Date/Time stop of the downlink as extracted from SAPF file  |
| cadip:retransfer          | boolean       | Flags whether the session corresponds to a retransfer               |
| cadip:antenna_status_ok   | boolean       | Determines the quality of the acquired pass at Antenna level        |
| cadip:front_end_status_ok | boolean       | Determines the quality of the acquired pass at FEP level            |
| cadip:downlink_status_ok  | boolean       | Overall status of downlink                                          |
| cadip:delivery_push_ok    | boolean       | Determines the evaluation of the data dissemination to the DDP      |

### Additional Item Property Information

#### cadip:id

Universally unique identifier (UUID). It is a local identifier for the Session instance within the CADIP, assigned by the service managing it.

#### cadip:acquisition_id

Acquisition id code as from the acquisition plan.
This information can be used by the Processing facility to correlate the status of the acquisition with the Acquisition plan if available to
the Processing facility

#### cadip:num_channels

This is the number of channels for the session. Allowed values are: 1, 2, 3, 4.

#### cadip:station_unit_id

2 digit X-band station unit ID, that can be used to identify different configurations at station level, if needed
(default is 00 if only one configuration is present or the parameter is not needed)

#### cadip:antenna_id

Identification of the antenna used.

#### cadip:front_end_id

Front-End processor (FEP) identifier. Usually each Station used two acquisition chains (one nominal and one in backup).
Each acquisition chain contain a FEP ID. This can be used by the station to trace which demodulator was used for a particular dump.

#### cadip:planned_data_start

Planned Date/Time start of the downlink as extracted from SAPF file.

#### cadip:planned_data_stop

Planned Date/Time stop of the downlink as extracted from SAPF file.

#### cadip:retransfer

Flags whether the session corresponds to a retransfer.

#### cadip:antenna_status_ok

It is set to the value depending on the quality of the acquired pass at Antenna level.

#### cadip:front_end_status_ok

It is set to the value depending on the quality of the acquired pass at Front-End processor by evaluating FER vs. admitted thresholds.

#### cadip:downlink_status_ok

Overall status of downlink:
“false” status shall be used if a problem has been encountered during the downlink as a warning for potential data loss or data corruption
(in any case processing should be initiated since the problem may not effect science data or telemetry).

#### cadip:delivery_push_ok

It is set to the value depending on the evaluation of the data dissemination to the DDP:
“false” status shall be used if a problem has been encountered during the transfer of a DSDB file from the station to the DDP.

## Asset Fields

| Field Name                | Type          | Description                                                                   |
| ------------------------- | ------------- | ----------------------------------------------------------------------------- |
| cadip:id                  | string (uuid) | **REQUIRED**. UUID for the DSDB File instance within the CADIP                |
| cadip:channel             | \[number]     | **REQUIRED**. This is the channel (1, 2, 3, 4)                                |
| cadip:block_number        | \[number]     | **REQUIRED**. DSDB numbering, always starting from 1 in each session          |
| cadip:final_block         | boolean       | **REQUIRED**. Set to true if it corresponds to the final file for the session |
| cadip:retransfer          | boolean       | Flags whether the data file is part of a retransfer                           |

### Additional Asset Field Information

#### cadip:id

Universally unique identifier (UUID). Local identifier for the DSDB File instance within the CADIP, assigned by the service managing it.

#### cadip:channel

This is the channel. Allowed values are: 1, 2, 3, 4.

#### cadip:block_number

This is the DSDB numbering, always starting from 1 in each session.

BlockNumber shall corresponds to the DSDB numbering. It is related to each channel of the session.

#### cadip:final_block

Set to true if it corresponds to the final file for the session.

#### cadip:retransfer

Flags whether the data file is part of a retransfer.

## Contributing

All contributions are subject to the
[STAC Specification Code of Conduct](https://github.com/radiantearth/stac-spec/blob/master/CODE_OF_CONDUCT.md).
For contributions, please follow the
[STAC specification contributing guide](https://github.com/radiantearth/stac-spec/blob/master/CONTRIBUTING.md) Instructions
for running tests are copied here for convenience.

### Running tests

The same checks that run as checks on PR's are part of the repository and can be run locally to verify that changes are valid. 
To run tests locally, you'll need `npm`, which is a standard part of any [node.js installation](https://nodejs.org/en/download/).

First you'll need to install everything with npm once. Just navigate to the root of this repository and on 
your command line run:
```bash
npm install
```

Then to check markdown formatting and test the examples against the JSON schema, you can run:
```bash
npm test
```

This will spit out the same texts that you see online, and you can then go and fix your markdown or examples.

If the tests reveal formatting problems with the examples, you can fix them with:
```bash
npm run format-examples
```

---

![](docs/images/banner_logo.jpg)

This project is funded by the EU and ESA.
