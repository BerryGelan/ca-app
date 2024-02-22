# ca-app

FastAPI app

![App Architecture](./img/architecture.jpg?raw=true "App Architecture")

## Components

### Metadata

| Volumes     | Description                                                                                  |
| :---------- | :------------------------------------------------------------------------------------------- |
| `v-src-cam` | shared docker volume **/ca-src** between CAsrc and CAmon. Contains **/in** and **/out**      |
| `v-cam-cas` | shared docker volume **/ca-cas** between CAmon and CAsrv. Contains **studyDate** directories |
| `v-cae-cad` | shared docker volume **/ca-cad** between CAetl and CAdb and CAsrv. Contains **/desc**        |

| Networks    | Components      |
| :---------- | :-------------- |
| `n-src-cam` | CAsrc and CAmon |
| `n-cam-cas` | CAmon and CAsrv |
| `n-cas-cae` | CAsrv and CAetl |
| `n-cae-cad` | CAetl and CAdb  |
| `n-cas-cau` | CAsrv and CAui  |

| Users   | Component | Volume Access               | Actions                                                                                          |
| :------ | :-------- | :-------------------------- | :----------------------------------------------------------------------------------------------- |
| `u-src` | CAsrc     | `v-src-cam`                 | Can access, change permissions, ownership and move data files from **/in** to **/out**           |
| `u-cam` | CAmon     | `v-src-cam` and `v-cam-cas` | Can access data files in **v-src-cam/out** and move to **v-cam-cas**                             |
| `u-cas` | CAsrv     | `v-cam-cas` and `v-cae-cad` | Can access data file descriptions **/desc**                                                      |
| `u-cae` | CAetl     | `v-cae-cad`                 | Can do actions in CAdb **data-file-etl** database and create data file descriptions in **/desc** |
| `u-cad` | CAdb      | `v-cae-cad`                 | Can read CAdb **data-file-etl** database and notify data file etl pre-processing is done         |

### CAsrc

Uses: cron, data-manager.sh

v-src-cam, n-src-cam, u-src

-   creates data files and saves to v-src-cam/in
-   changes data files permissions, ownership for u-cam
-   moves data files to v-src-cam/out

(repeats process every 5\* minutes - configurable)
